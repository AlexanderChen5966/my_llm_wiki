---
title: wikilink 存成資料庫的邊：寫入時解析、懸空補洞、ReDoS 防護與圖譜呈現
date: 2026-09-23
tags: [dev/architecture, pkm/obsidian, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10413279; https://ithelp.ithome.com.tw/articles/10414163
type: article
status: processed
asset_value: 60
---

WikiBrain 系列 Day 09（主體）＋ Day 11 圖譜那一半，作者為 iT 邦幫忙使用者 20184129（WikiBrain 作者）。WikiBrain 要回答「有哪些頁連到這一頁」和「整張圖長什麼樣」，中間需要把 Markdown 裡的 wikilink 轉成可查詢的結構。本知識庫本身就靠 wikilink、懸空連結與 INDEX 運作，這條有直接參考價值。

> 📌 本條目所有 wikilink 語法範例都放在 code block 裡，避免 Obsidian 把範例當成本庫的真連結而產生假斷鏈。

## T（核心論點）

**連結在寫入那一刻就解析成資料庫的邊，查詢端就不必再懂任何寫法。** 圖譜與反向連結從「讀取時用 regex 掃全部筆記」的 O(N×M) 降為對 `links` 表的 O(E) 索引查詢（E 是邊數）。難點都集中在寫入那一刻：懸空連結要反向補洞、同一目標有五種寫法、解析用的正規表示式要防災難性回溯。

## B（適用場景）

**適用：**
- 自建知識庫、筆記或文件系統，需要反向連結、圖譜、孤兒頁／斷鏈檢查
- 任何「使用者自由輸入的參照字串」要對應到實體的系統（引用鍵、標籤、@提及）
- 任何在使用者輸入上跑正規表示式的地方（ReDoS 一節可獨立重用）

**不適用 / 注意：**
- 幾十頁的小庫讀取時解析也感覺不出差別；幾百頁以上才開始卡
- 前端若也要判斷連結有無對應頁，會產生重複實作，要明確標註以哪一端為準

## R（操作步驟）

1. **`links(from_note_id, target, to_note_id)` 表**（實際還有 `workspace_id`）。`to_note_id` 由後來的 migration 加上，存解析出的目標頁；圖譜是一句 `SELECT`，反向連結是 `WHERE to_note_id = ?`，兩者都有索引。
2. **`to_note_id` 允許為 NULL＝懸空連結**：指向尚不存在的頁，這是 Obsidian 的慣例——寫東西時提到一個概念就先連過去，那一頁之後再寫。
3. **新頁建立時反向補洞**，不必等下次重掃：
   ```ts
   const candidates = [...new Set([n.path, noMd, noMd.replace(/^[^/]+\//, ''), n.basename, n.title])];
   await c.query(
     `UPDATE links SET to_note_id = $1
       WHERE workspace_id = $2 AND to_note_id IS NULL AND from_note_id <> $1 AND target = ANY($3::text[])`,
     [noteId, ws, candidates],
   );
   ```
   `candidates` 是同一頁的五種寫法：完整路徑、去 `.md`、再去層級前綴、檔名、標題。**改標題也要補洞**，因為標題本身就是合法的連結寫法。
4. **同一目標的多種寫法**，例如：
   ```markdown
   [[llm-wiki-pattern]]
   [[wiki/concepts/llm-wiki-pattern]]
   [[wiki/concepts/llm-wiki-pattern.md]]
   [[LLM Wiki 模式]]
   ```
   解析優先序：完整路徑 → 路徑加 `.md` → 三層前綴（`raw/`／`wiki/`／`schema/`）加 `.md` → 檔名 → 標題。**規則只寫在後端一支 SQL 裡**；前端 `web/src/lib/links.ts` 有一份刻意接受的重複實作，註解寫明以後端為準、改的時候兩邊一起改。
5. **ReDoS 防護**：原本的樣式大致是
   ```
   \[\[(.+?)\]\]
   ```
   有人送進八萬個連續左雙方括號的內容，回溯跑了 14 秒、CPU 全滿；單執行緒 Node 等於一個人就能讓全站停擺。修正後：
   ```ts
   /\[\[([^\[\]|#\n]+)(?:[#|][^\[\]\n]*)?\]\]/g
   ```
   排除 `[`、`]`、`|`、`#` 與換行，任何一個開頭最多掃到下一個方括號就停；後段可選群組順便處理「頁面#段落|顯示文字」的寫法，只取真正的目標。另加**單頁 1MB 上限**，建立與更新都擋。**任何跑在使用者輸入上的 regex，都要假設有人會餵最壞的輸入**——這種問題不會在功能測試裡出現。
6. **學術引用也當成邊**：`[@mensh2017ten]` 與 pandoc 多重引用 `[@a; @b, p. 12]` 解析成指向引用鍵的邊，走「檔名」規則。好處：引用過的來源自動離開待編纂、來源頁反向連結列出引用者、圖譜看得到引用關係、健檢抓得到指向不存在來源的引用。

### 呈現端：連續狀態的視覺化不交給框架渲染週期（Day 11）

- **canvas、事件監聽、動畫迴圈只建一次**，模擬狀態放在 ref，資料變動（時間軸播放、篩選）只同步差異。第一版每次資料變就重建模擬，節點位置歸零，畫面「像壞掉的燈管」一直閃。
- **新節點位置優先序**：先用上次的位置，沒有就放在鄰居旁邊（±30），都沒有才在中心附近隨機；還在的節點連速度都保留。整張圖是「生長」而不是「重擲」。
- **看起來像常數的東西其實在動**：時間軸上界用每次 render 的 `Date.now()` 算，播放時每幀都在變，進度原地踏步；修法是載入時固定上界。同型陷阱：`new Date()` 放進依賴陣列、`Math.random()` 當 key。這類 bug 不會報錯，只會讓畫面怪怪的。
- **大圖怎麼看**：超過 60 節點只標連結數前 15%＋目前頁＋滑鼠指到的節點；縮放超過 1.6 倍全標；半徑取平方根並設上限；**焦點模式**只看 1–2 層鄰居（實際上最有用，因為多數時候想問的是「這一頁跟什麼有關」）。篩選狀態存 localStorage，並顯示「顯示數／總數」，否則下次打開會以為圖譜少了東西。
- **效能**：超過 150 節點斥力只算近距離，捨棄 Barnes-Hut 四叉樹——個人知識庫幾百頁就算多，時間花在焦點模式更值得。

## C（關鍵行動）

- [ ] 本庫健檢時，把「同一目標多種寫法」列入斷鏈檢查（對照 WikiBrain 的五種 candidates）
- [ ] 盤點自己專案中跑在使用者輸入上的 regex，把 `.+?` 類樣式改成明確排除的字元類別，並加輸入長度上限
- [ ] Flutter 專案中有持續動畫／模擬的元件，確認狀態不隨 build 重建

## 原始摘要

Day 09：wikilink 從讀取時解析改為寫入時解析並存成 `links` 表的邊；懸空連結反向補洞、五種寫法與解析優先序、ReDoS 修正與 1MB 上限、學術引用也是邊。Day 11 圖譜部分：持久模擬、新節點位置優先序、`Date.now()` 陷阱、大圖標籤策略與焦點模式。Day 11 編輯器那一半見 [[human-agent-shared-data-layer-optimistic-locking]]。

## 相關連結

- [[wikibrain-llm-wiki-implementation-patterns]] — 同系列 Day03+04；「待編纂＝沒有 wiki 頁連回的 raw 來源」正是靠本條目的 `links` 表反查
- [[wikibrain-compounding-knowledge-base-demo]] — 同系列 Day05；該篇 C 層提到的「短路徑 wikilink 解析不到就不會進連結表」，本條目的五種寫法與優先序就是解法
- [[human-agent-shared-data-layer-optimistic-locking]] — 同系列 Day08：`links` 是五張表之一
- [[mcp-server-stateless-streamable-http-oauth]]、[[tool-description-and-error-message-as-prompt]] — 同系列 Day06、Day07
- [[graphify-codebase-knowledge-graph]] — 另一種知識圖譜的建構方式（程式碼庫）
- [[synthesis-karpathy-llm-wiki-method]] — 反向連結與 INDEX 維護的方法論精粹
- [[karpathy-knowledge-base-method]] — Karpathy 知識庫方法
