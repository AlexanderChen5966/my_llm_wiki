---
title: 團隊交付格式與三工具搭配 — 2026-09-09
date: 2026-09-09
tags: [pkm/workflow, ai/workflow, pkm/obsidian]
type: note
status: processed
---

# 團隊交付格式與三工具搭配 — 2026-09-09

> 三個提問的整理：①與 team 合作時不用 Markdown 溝通有哪些方法 ②Obsidian／Notion／Gemini Notebook（原 NotebookLM）
> 能否搭配、各自該當什麼角色 ③Notion 是先前用的筆記軟體、與 Obsidian 內容有落差時該怎麼調整。
> 全部結論來自庫內既有條目，各段附條目名可回查。

---

## 一、不用 Markdown 跟 team 溝通

### 核心判準：Markdown 不是拋棄，是退到「來源」位置

出自 [[claude-code-markdown-single-source-multi-format]]（65 分）：

> 內容只寫一次，寫在 Markdown 裡，其他格式全部從它生。**一次寫、三處用，改來源，三種格式重生一次就同步。**

反面就是教材/文件最常見的災難——「三份檔案、三個版本」：Word 改了、簡報忘了改、網頁又是舊的。
所以**對外交付格式的選擇，不影響 Markdown 作為單一真相來源的地位**，兩件事不衝突。

### 為什麼 HTML 是首選交付格式

出自 [[html-as-ai-output-interface]]（72 分，本題全庫最高分條目）：

> **`.md` 只能「說」；`.html` 可以「指」。**

🔴 該條目第一件事就是排除誤解：**這不是「調教技巧」**。對模型來說 `.md` 和 `.html` 都只是文字，
理解能力沒有差別——**好處全部發生在「讀的人」那一端**。

| | Markdown | HTML |
|---|---|---|
| **層次** | 字級無法分層，七十幾行同樣大小同樣顏色 | 重點放大、次要縮小，掃一眼抓到主線 |
| **標記** | 只有粗體 | 顏色／底色框讓「這是關鍵決定」「這裡有雷」一眼區分 |
| **結構** | 表格陽春 | 表格、卡片、分區塊；比較三方案一張表完勝三段文字 |

具體對照：同一份交接檔，HTML 版最上面一排 **7 / 2 / 4**，讓人
「連讀都不用讀就知道做完七項、卡住兩項、還有四項沒動」。

> 🔑 **差別不在資訊量，在你要花多少力氣才能把它讀出來。而且這份東西你不會只看一次——每次回來都省一遍，才是它划算的地方。**

### 完整方法清單

| 交付對象 | 方法 | 出處 |
|---|---|---|
| 同事讀的報告／交接／方案比較 | **請 Claude 直接生 HTML**——連轉都不用，自帶 CSS、能鍵盤翻頁 | [[html-as-ai-output-interface]] |
| 要印的紙本講義 | **pandoc → Word**（包成 skill，一句話轉，自動修表格框線） | [[claude-code-markdown-single-source-multi-format]] |
| 上課／會議投影 | **python-pptx → PPTX**（讀 md 章節結構，逐頁組出標題/條列/重點框） | 同上 |
| 標準投影片需求 | ⚠️ **Marp 這類現成框架更省事**——只有需要完全客製風格才手刻 | 同上（該條目明列的「不適用」） |
| 簡報風格精確控制 | **YAML 點菜單**：色碼取代顏色名、分別指定標題與內文字體、加禁止清單防 AI 過度設計 | [[yaml-structured-ai-presentation]] |
| 團隊持續存取 | **發佈層**：原始檔放案件資料夾，**Git 是正本，發佈只是複製** | [[html-as-ai-output-interface]] 補充資料 |

### 同內容多套皮膚

四份 HTML 的版面結構完全一樣，差別只在最上面那組 CSS 變數（`--bg` / `--ink` / `--accent`）：
親切教學風、蘋果簡約風、深色質感風、三步驟流程風。
🔑 **內容一字沒改，四種風格一鍵切換**——手工做要重排四遍。維護時只動 CSS 變數層，不要重排版面結構。

### 實務上真的會踩到的坑

全部出自 [[html-as-ai-output-interface]] 的補充資料（作者三週累積 273 頁的實戰紀錄）：

- 🔴 **「知道 URL 的人都能看」不算存取控制**——沒有 IP 限制的 URL 只要知道連結就能打開，
  **即使有 `noindex` 和期限也一樣**。含客戶名稱或金額的頁面不能就這樣發出去：
  發佈前先看內容，必要時改內部限定，或把專有名詞改成代稱後另出一版。
- ⚠️ **本機看得懂不代表分享出去別人看得懂**：用沒有 charset 的 `text/html` 發佈時，
  **沒有 `meta charset` 的 HTML 會變成亂碼**。作者曾因此把要給編輯看的頁面全毀了（繁中內容同樣適用）。
- ⚠️ **合格標準不是 PC，而是手機**——能在電車上讀到才算完成。
  實測 202/237 頁（85%）有表格，🔑 **問題根本不是欄數，而是日期的 `nowrap` 和儲存格裡的長文**
  （1198 個超過 40 字的儲存格，最長 545 字）。解法是在發佈端把溢出表格轉成「標題＋數值」卡片。
- 🔑 **要對齊版面時先要一張 wireframe**，不要用文字描述——「左邊放清單」你腦中的寬度和它腦中的
  可能差很多，**你講一百字，也不保證兩邊想的是同一張圖**。
- 🔑 **把 HTML 外觀也做成 skill**：「光把容器整理好，如果裡面的 HTML 風格不一致，
  每次打開都像被丟到不同的 app。」反直覺發現——被否決版本的問題不在「沒用藍色」，
  **而在「藍色太分散」**（卡片、引用、編號徽章各放一點顏色，整體就廉價又雜亂）。
- 🔑 **不要再判斷「這個值不值得發佈」**：發佈端沒有 LLM 運作（只有 S3 + CloudFront），
  publish 一次的 token 不到「生成一頁 HTML」的十分之一。**與其在聊天裡討論要不要發佈，成本反而更高。**

### 簡報另有一層分工

出自 [[ai-presentation-four-layers-human-value]]（58 分）——**骨／皮／肉／靈魂**四層：

| 層次 | 可否交給 AI |
|---|---|
| 骨（邏輯結構） | AI 可代勞初稿，但你要先有清晰藍圖才給得出精準指令 |
| 皮（版面呈現） | AI 擅長，可大幅放手 |
| 肉（講者內容） | 靠人——AI 沒有真實世界經驗，無法決定什麼資訊最重要 |
| 靈魂（風格與感染力） | 只能靠人 |

> ⚠️ 全部外包的後果：講者在台上「只剩一個空殼，嘴裡念的全是機器的邏輯，聽眾會充滿距離感」。

---

## 二、Obsidian ＋ Notion ＋ NotebookLM 能否搭配

**能，但前提是先確定誰是正本。**

### 🔴 先更正一個事實：NotebookLM 已改名 Gemini Notebook

出自 [[note-tools-five-way-comparison-2026]]（50 分）。本庫既有 7＋篇條目沿用舊名
（[[notebooklm-gemini-claude-research-workflow]]、[[notebooklm-py-cli-tool]]、[[notebooklm-skill-research-pipeline]]、
[[google-workspace-notebooklm-gemini-admin-workflow]]、[[gemini-study-notebooks]] 等），
**檢索與對外書寫時兩個名稱都要納入考慮**。

### 先分類，再比功能

同篇條目的第一層分流比功能表更關鍵：

| 類別 | 工具 | 用途 |
|---|---|---|
| **知識庫型** | Notion / **Obsidian** / Heptabase | 長期擴張、跨主題累積，範圍自己決定 |
| **專案工作型** | **Gemini Notebook** / Kuse AI | 圍繞一組明確資料完成研究或產出 |

> 判斷方式：問「該把筆記系統長期建在哪裡」→ 答案在前三者；問「只是想快速消化一批資料」→ 後兩者。
> 🔑 **選錯類別，再怎麼調設定都不對。**

操作心態也不同——**用 Gemini Notebook 時先決定「這次要研究哪些資料」再開始問；
用知識庫工具時則從「我要記下什麼」開始。**

出自 [[notebooklm-gemini-claude-research-workflow]]（64 分，券商分析師的三 AI 接力實例）：

> 多 AI 協作的價值不在單一模型多強，而在**用統一的中介格式（Markdown）＋ 共用儲存層讓工具接力**。
> 每個 AI 負責它最擅長的一段，前一段的輸出成為後一段的輸入。

### 本知識庫的實際規模（2026-09-09 實測）

| 項目 | 數量 |
|---|---|
| wiki 主層條目 | **443 篇** |
| ├ `synthesis/`（精粹） | 13 篇 |
| └ `concepts/`（概念框架） | 6 篇 |
| wiki 總量 | **167 萬字元** |
| `raw/` | 1019 篇 `.md`（939 已處理／80 待處理）＋ 24 個非 md 素材 |
| `research/` ／ `ideas/` ／ `outputs/` ／ `archive/` | 57 ／ 94 ／ 25 ／ 52 |
| `wiki/INDEX.md` | **751 行、16 萬字元** |

### 這組數字直接命中既有門檻

[[obsidian-knowledge-base-planning]]（70 分）自己訂的規模建議表：

| wiki 條目數 | 建議策略 |
|---|---|
| < 30 篇 | 直接讀取 |
| 30–80 篇 | 用 `knowledge-synthesizer` 壓縮同主題群 |
| **> 80 篇** | **考慮接 NotebookLM 或 Claude Projects** |

🔑 現況 443 篇，**已超過該門檻約 5 倍**。所以問題不是「能不能搭配」，而是規模上早就該搭了。

### 角色分配（三者不可混）

| 工具 | 角色 | 為什麼是它 |
|---|---|---|
| **Obsidian** | 🔑 **正本／單一真相來源** | Claude Code 直接讀寫本地檔案、**不需要 MCP**（MCP 為選配）；純文字好版控；wikilink 圖譜是 443 篇的導航基礎 |
| **NotebookLM**（Gemini Notebook） | **大語料問答層** | 167 萬字元遠超單次 context。「基於真實來源、可驗證引用、業界最低幻覺」，適合對整個 wiki 提問而不必自己挑檔 |
| **Notion** | **對外協作層** | team 不必裝 Obsidian、不必碰 md。護城河是**「AI 嵌入既有工作流的位置」**，不是模型能力 |

接力方向必須單向：**Obsidian（寫）→ NotebookLM（問）／Notion（給人看）**。
三者都吃 Markdown，這是能接起來的唯一原因——與券商案例「從第一步就統一輸出 Markdown」同一個道理。

### 三個必須先知道的限制

1. **Notion 在本庫的評分只有 9 分**（[[notion-ai-workspace-analysis]]），全庫偏低。
   對已經有 Obsidian ＋ Claude Code 的人，它能補的只剩「對外」這一格，**不要期待它承擔整理**。
   若 team 協作需求不強，這一角可以先不加。
2. **NotebookLM 自動化有帳號風險**：[[notebooklm-skill-research-pipeline]] 明載——依賴 Google 帳號
   session、需定期更新，且 ⚠️ **Google 服務條款禁止自動化存取，有潛在封號風險**。
   手動上傳無此問題；要做自動化管線才需衡量。
3. **反面觀點也在庫裡**：[[karpathy-no-obsidian-setup]]（65 分）主張三個資料夾加一份 `CLAUDE.md`
   就能跑通，Obsidian 只在需要視覺化圖譜與全文搜尋時才必要。
   🔑 **工具越加越多不等於越好——每加一個都要說得出它獨佔哪一格。**

### 🔑 Notion 該當「下游」，不是「子集」

若把 Notion 設為「子資料庫」（持有一部分獨家知識），會產生第二個寫入點，後果有三：

1. **查詢要跑兩個地方**，而 Claude Code 讀不到 Notion（需另接 MCP，不像 Obsidian 是檔案層直讀）
2. 🔴 **鎖死風險**：Notion 資料庫做得越複雜換平台成本越高——**關聯欄位在 CSV 匯出後會變成純文字網址，
   無法靠重新匯入恢復關聯**（[[note-tools-five-way-comparison-2026]] 列的三個「帶不走」之一）
3. 本庫 pipeline 的判斷全靠 frontmatter `status` 與 wikilink，**Notion 兩者都沒有等價物**

| | 子集 | **下游（建議）** |
|---|---|---|
| 內容來源 | Notion 獨有一部分 | 全部源自 Obsidian |
| 遺失風險 | 高——沒有別處有 | 零——隨時可重生 |
| 查詢 | 要找兩個地方 | 只查 Obsidian |
| 關聯欄位鎖死 | 會中招 | 不痛（正本在別處） |

該條目引原文的混用建議也是這個順序：
**Gemini Notebook 消化大量既有資料 → Obsidian 釐清思路與長期保存 → 最終成果整理進 Notion 與團隊協作。**

### Gemini Notebook 是「消化」不是「累積」

⚠️ 它被歸為**專案工作型**，遷移風險那節寫得很直白：
**資訊組織方式沒有 Obsidian／Notion 自由，較難用來累積跨主題的長期知識系統。**

⚠️ 它**沒有回寫能力**（報告只能匯出到 Google 文件／試算表）——
🔑 **它是唯讀終點，不是中繼站**。整合結果必須人工回流 Obsidian，
否則會累積一堆散在各 notebook、Claude Code 讀不到的分析。

### 修正後的架構

```
                    ┌──────────────────┐
   一批待消化資料 →  │ Gemini Notebook  │  消化／問答／附引用
                    └────────┬─────────┘
                             │ 整合結果人工回流（必須）
                             ▼
                    ┌──────────────────┐
                    │    Obsidian      │  🔑 唯一正本
                    │   443 篇 wiki     │  Claude Code 檔案層直讀
                    └────────┬─────────┘
                             │ 單向投影，只送「該給人看的成果」
                             ▼
                    ┌──────────────────┐
                    │      Notion      │  團隊協作終點
                    └──────────────────┘
```

> **Obsidian 是正本，另外兩個都是它的衛星——一個在上游消化、一個在下游展示，都不持有獨家內容。**

實務前提：**Gemini Notebook 餵 `wiki/` 不要餵 `raw/`**（raw/ 有 1019 篇未提煉素材，
噪音會稀釋它「低幻覺、可追溯引用」的優勢）。

---

## 三、Notion 既有舊內容與 Obsidian 有落差，怎麼處理

前提：Notion 是**先前使用**的筆記軟體，內容與現行 Obsidian 有落差。

🔑 **不要當成「遷移專案」，當成「一批新的 raw 素材」**——既有的 `raw/ → wiki/` 流程已內建
去重比對、asset_value 門檻與 archive 規則，正好是處理落差需要的三件事，重造一套只會多出風險。

### 🔴 最危險的一步在最前面：匯出會靜默漏資料

[[note-tools-five-way-comparison-2026]] 明載：**Notion 資料庫預設只下載第一個檢視的前 50 筆**。

匯出前務必：
- 逐個資料庫**檢查筆數**，不要信預設檢視
- 用 **Markdown & CSV** 格式匯出（不要只 CSV），勾選「包含子頁面」
- 匯完**核對筆數**：Notion 端幾筆 vs 匯出檔幾個，對不上就重來

⚠️ 這步做錯後面全白做，而且**不會有任何錯誤訊息**。

### 落地流程

1. **先止血**——若偶爾還在寫 Notion，先定切換日，之後只寫 Obsidian。雙寫期間做遷移，做完又會落差。
2. **進 `raw/notion-export/`，補 frontmatter**（匯出檔沒有 frontmatter，pipeline 的 inbox 判斷全靠它）：
   ```yaml
   ---
   title: <原標題>
   date: <Notion 的建立日，不是今天>
   tags: []
   source: Notion 匯出（<原資料庫名>）
   type: note
   status: inbox
   ---
   ```
   🔑 `date` 必須用**原始建立日**——這是後面判斷「誰比較新」的唯一依據，用今天等於把資訊丟掉。
3. **走既有流程**：1–9 篇用 `knowledge-organizer`，≥10 篇用 `raw-pipeline`。
   classifier 的 `existing_overlap` 欄位會拿每篇比對現有 443 篇條目自動找出重疊——
   與 2026-09-09 那批 20 篇抓出「Day 24 與 [[langchain-langgraph-basics]] 高度重疊」是同一個機制。

### 落差的四種情況，處置不同

| 情況 | 處置 |
|---|---|
| Notion 有、Obsidian 沒有 | 正常建條目（走 asset_value 門檻） |
| 兩邊都有，Notion 較舊且無增量 | 標記已處理，不建條目 |
| 兩邊都有，但 **Notion 有 Obsidian 沒寫進去的細節** | 補進既有條目的「補充資料」 |
| 🔴 **兩邊結論不同** | **不要合併**——這是觀點演變，要記錄「當時為什麼那樣想、後來為什麼改」 |

⚠️ 第三種比想像中常見：舊筆記常留著**當時的決策脈絡**（為什麼選 A 不選 B、踩過什麼坑），
而後來重寫的條目往往只留結論不留過程——那些脈絡是新條目不會有的東西。
**別預設「舊的＝沒價值」直接跳過**——這是本庫既有操作判準「去重前先比對日期與內容差異，
不要直接標記已處理」的**反方向應用**（該判準原本針對「同主題新文常帶修正」，此處相反：舊文常帶脈絡）。

第四種最有價值：兩邊結論打架代表想法變過，**這個變化本身值得記一筆，比兩份筆記都有價值**。

### 兩個保護措施

- **先抽樣再全量**：先挑 10–20 篇跑一輪，確認 frontmatter 補法與重疊判斷正確再處理其餘。
  對應 `rules/decision-framework.md` 第一條：**可逆性優先**、**讀比寫便宜**。
- **過時內容進 `archive/` 不刪**（CLAUDE.md 鐵則）。Notion 原始匯出檔留在 `raw/`，
  處理完標 `.processed`，天然就是封存。

### Notion 帳號本身的兩條路

1. **完全停用**——內容都在 Obsidian 後，Notion 清空或放著當歷史快照
2. **轉成純下游**——不再持有獨家內容，只放要給 team 看的成果

建議**先做 1，等真的有協作需求再回頭做 2**。理由：它在 [[notion-ai-workspace-analysis]] 只有 9 分，
且目前沒有明確 team 協作場景——遷完先讓架構單純下來。

---

## 四、順帶發現的現實訊號

`wiki/INDEX.md` 已達 **751 行、16 萬字元**（約 4 萬 token 級別）。
它原本的角色是「查詢入口」，但現在**自己也快變成需要被索引的對象**。

這比「該不該接 NotebookLM」更緊迫，而且兩者解的不是同一件事：

- 接 NotebookLM → 解決「對 443 篇提問」
- **INDEX 膨脹 → 只能靠 `knowledge-synthesizer` 壓縮主題群**

目前 443 篇只精粹出 **13 篇** synthesis（比例偏低）。
[[obsidian-knowledge-base-planning]] 提到觸發壓縮的信號是每月 `wiki-health-check`
會提示哪些主題累積 3 篇以上、適合執行 `knowledge-synthesizer`。

---

## 待辦

- [ ] `raw/` 尚有未處理的〈AI agent 的工作成果，用 markdown 看不會很痛苦嗎？→ 我做了「HTML分享君」〉，
      是第一題的專篇，下批優先處理（目前 `raw/` 待處理 80 篇）
- [ ] 評估是否對 443 篇 wiki 執行一輪 `knowledge-synthesizer`，緩解 INDEX.md 膨脹
- [ ] **Notion 匯出**：逐個資料庫核對筆數（防「前 50 筆」靜默截斷），先抽樣 10–20 篇試跑 pipeline
- [ ] 匯出後補 frontmatter 的腳本（`date` 從 Notion 匯出檔名或內文抓原始建立日）
- [ ] 決定 Notion 帳號歸宿：完全停用（建議先行）或轉純下游

## 相關連結

- [[html-as-ai-output-interface]] — 第一題主要依據（md 只能說、html 可以指）
- [[claude-code-markdown-single-source-multi-format]] — 單一來源多格式的核心判準
- [[ai-presentation-four-layers-human-value]] — 簡報的骨皮肉魂分工
- [[yaml-structured-ai-presentation]] — 用 YAML 精確控制簡報視覺
- [[notebooklm-gemini-claude-research-workflow]] — 第二題主要依據（三 AI 用 Markdown 接力）
- [[obsidian-knowledge-base-planning]] — 規模門檻表與 Vault 分層設計
- [[notion-ai-workspace-analysis]] — Notion 定位分析（9 分）
- [[notebooklm-skill-research-pipeline]] — NotebookLM 自動化與其帳號風險
- [[karpathy-no-obsidian-setup]] — 反面觀點：不用 Obsidian 也能跑通
- [[note-tools-five-way-comparison-2026]] — 🔑 第二、三題主要依據：知識庫型 vs 專案工作型分流、
  三個「帶不走」的遷移風險、Notion 匯出「前 50 筆」限制、NotebookLM 改名
- [[claude-code-obsidian-setup]] — 本庫實際採用的 Obsidian + Claude Code 架構
