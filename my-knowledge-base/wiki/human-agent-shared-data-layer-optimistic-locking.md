---
title: 人與 agent 並存的資料層：樂觀鎖、工作區隔離與保護使用者的輸入
date: 2026-09-23
tags: [ai/agent, dev/architecture, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10412617; https://ithelp.ithome.com.tw/articles/10414163
type: article
status: processed
asset_value: 60
---

WikiBrain 系列 Day 08（主體）＋ Day 11 編輯器的草稿與復原兩點，作者為 iT 邦幫忙使用者 20184129（WikiBrain 作者）。情境：使用者在網頁上改一頁改到一半離開，Claude 同時在背景編纂論文、判斷要補進同一頁並先存了一次——使用者回來按儲存，系統要決定放行還是擋下。多寫入者的並發保護是通用模式，Spring Boot 後端同樣適用。

## T（核心論點）

**人類和多個 agent 同時編輯同一頁時，悲觀鎖會讓雙方互相卡死**（agent 在背景編纂，使用者連點開都不行；使用者開著編輯器去吃午餐，agent 全部停住），所以改用樂觀鎖：**用單一條件式 UPDATE 原子比對 version，回傳零列就是衝突。** 關鍵不在拒絕，而在**拒絕時把目前的完整內容一起交給對方**——agent 能自己合併重送，網頁端也不會丟掉使用者打的字。

## B（適用場景）

**適用：**
- 人與 agent（或多個 agent）共寫同一份資料：知識庫、文件、設定、工單
- 衝突實際上不常發生、但發生時代價高（整頁已寫完才被拒）的場景
- 多租戶系統需要一條簡單可審查的資料隔離紀律

**不適用 / 注意：**
- 衝突頻繁、重做成本極高的場景（例如長交易的庫存扣減），可能仍需要悲觀鎖或佇列
- 樂觀鎖的代價是拒絕真的會發生，而且通常發生在對方已經寫好一整頁之後——所以衝突回應的設計比鎖本身更重要

## R（操作步驟）

1. **樂觀鎖規則只有一句：修改時說出你以為的版本號**（`update_note({ path, content, if_version: 7 })`）。比對和寫入是同一句 SQL：
   ```sql
   UPDATE notes SET title = $3, content_md = $4, version = version + 1, updated_at = now()
    WHERE workspace_id = $1 AND path = $2 AND deleted_at IS NULL AND version = $5
   RETURNING id, version
   ```
   版本不合就更新到零列，`RETURNING` 回空，上層據此丟衝突。
2. **不可先 SELECT 再 UPDATE**：中間有空窗，兩個 agent 可以同時通過檢查。條件直接寫在 `WHERE` 裡，資料庫替你把比對和寫入綁成一個動作。
3. **ConflictError 附 `{version, content, updated_at}`**：更新失敗後再查一次該頁，把目前狀態包進錯誤。錯誤訊息的寫法（附下一步指示、不只回 409）見 [[tool-description-and-error-message-as-prompt]]，這裡不重寫。
4. **工作區隔離紀律：路由層一行筆記 SQL 都不准寫。** 以使用者給的路徑為入口的查詢全部集中在 `src/notes.ts`，每個函式第一個參數都是 workspace id、SQL 都帶 `WHERE workspace_id = $1`；已取得 note id 後的衍生查詢（版本、連結、標籤）靠 id 本身已綁定工作區。分界線不在「哪個檔案」，在「哪一種查詢」。這條寫進貢獻指南，也是安全審查的第一站——延伸了 [[wikibrain-llm-wiki-implementation-patterns]] R 第 1 點「守門寫在資料層」。
5. **每次寫入存完整快照，不存 diff**：`note_versions` 保留 90 天（免費方案 7 天），最新版永遠保留。不存 diff 是因為讀取時每次都要重算，而且鏈中間壞一環後面全毀；文字便宜，壓縮後更便宜，選簡單。
6. **先量再優化，不做的事要記下「何時該做」**：搜尋先用 `ILIKE '%kw%'` 全表掃描，實測 1,000 頁約 35ms、10,000 頁×5KB 約 350ms、加 `LIMIT 10` 且關鍵字常見約 0.6ms。門檻「單一工作區超過 5,000 頁才上 pg_trgm」寫進規劃文件並做成營運監控提醒，否則它只會變成一筆被遺忘的技術債。
7. **預設行為要保護使用者的勞動**：網頁端第一版偵測到衝突後按「載入最新版」會把編輯器換成伺服器版本——使用者打了 20 分鐘的字全部消失。改成「保留我的修改並載入最新版本號」：內容原封不動，只更新版本號，使用者自己決定怎麼合併再存。系統可以隨時重讀，人打的字重打不回來。

### 延伸：同一原則在編輯器端（Day 11）

- **(a) 未存草稿交給 localStorage**：key 為 `wb-draft:${note.path}`，打字時 500ms 防抖寫入；以路徑為鍵，同工作區開三個分頁編三頁不會互蓋；儲存或取消時清除；重開時若草稿與伺服器內容不同，跳出「有未儲存的草稿」提示還原或捨棄。約 20 行，擋掉的是使用者最生氣的那種遺失：系統沒壞、字也確實打了，但它就不見了。
- **(b) 復原＝用舊版內容建立新版本**，不是把版本號改回去。歷史只往前長，「誰在什麼時候改回舊版」也留在紀錄裡。

## C（關鍵行動）

- [ ] Spring Boot 專案中檢查有併發寫入的表：是否用 `@Version` 或條件式 UPDATE，而非先查再寫
- [ ] 衝突例外的回應 body 附上目前狀態，讓 client 能直接合併
- [ ] 盤點「刻意先不做」的優化，補上觸發門檻與監控提醒
- [ ] 檢查前端「放棄修改／重新載入」類按鈕的預設行為是否會丟掉使用者輸入

## 原始摘要

Day 08：PostgreSQL 五張表（workspaces／notes／note_versions／links／tags，外加 mcp_tokens）；工作區隔離紀律、完整快照、ILIKE 門檻、樂觀鎖與衝突回應、網頁端不丟字、建立也會衝突。Day 11：編輯器與圖譜都在處理「狀態該歸誰管」；本條目只取編輯器端的草稿與復原，LCS diff 細節與其他 UI 項目不收；圖譜那一半見 [[wikilink-as-database-edges-and-graph]]。

## 相關連結

- [[wikibrain-llm-wiki-implementation-patterns]] — 同系列 Day03+04；本條目的工作區隔離紀律延伸其 R 第 1 點「守門寫在資料層」
- [[wikibrain-compounding-knowledge-base-demo]] — 同系列 Day05
- [[tool-description-and-error-message-as-prompt]] — 同系列 Day07：衝突錯誤訊息怎麼寫
- [[wikilink-as-database-edges-and-graph]] — 同系列 Day09+11：`links` 是五張表之一
- [[mcp-server-stateless-streamable-http-oauth]] — 同系列 Day06：MCP 與伺服器端 agent 共用這一層資料存取
- [[hand-coding-agent-file-tools]] — **不同層級的同一招**：那篇的 write_file 是檔案系統層的 read-before-write＋樂觀鎖，本條目是資料庫層的條件式 UPDATE
- [[mcp-stateless-migration-state-ownership]] — state 的責任歸屬：協定不替你保管的狀態，要明確由誰負責
