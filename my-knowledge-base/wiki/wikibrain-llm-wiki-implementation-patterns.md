---
title: WikiBrain 實作案例：程式碼守門與工具設計
date: 2026-09-17
tags: [ai/agent, dev/architecture, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10409772, https://ithelp.ithome.com.tw/articles/10409915
type: article
status: processed
asset_value: 64
---

WikiBrain（wikibrain.app）把 Karpathy 的 raw/wiki/schema 三層 LLM Wiki 準則做成獨立 MCP 服務型產品，本條目取自其系列 Day03、Day04，聚焦**操作層的具體實作**：程式碼與規則的分界、raw/ 唯讀雙重防呆、wiki/ 工具清單刻意不給 delete、schema/ 的使用者可改機制，以及七十行工具呼叫迴圈、重試退避策略、逐步記帳機制。

## T（核心論點）

**分界原則：會弄壞資料的寫成程式碼，影響品質的寫成規則。** raw/ 唯讀是程式碼鐵則（因為救不回來），其餘規範（如 wiki/arguments/ 一個可反駁主張一頁）用 Markdown 規則叫 agent 遵守即可，路徑檢查完全不認得這些子分類。工具執行層則用一個不依賴任何 agent 框架的七十行工具呼叫迴圈，把「呼叫模型、執行工具、記 token」這三件事做好即可，複雜的多步驟規劃、子 agent、記憶管理才需要框架。

## B（適用場景）

**適用：**
- 自建 LLM Wiki / agentic 知識庫服務，需要決定「哪些規則該用程式碼強制、哪些該用 Markdown 約束」
- 需要對使用者自付 API key 的功能做透明記帳
- 工具呼叫迴圈的任務單純（呼叫模型→執行工具→記錄），不需要多 agent 協作或複雜狀態機

**不適用 / 注意：**
- 需要多步驟規劃、子 agent、複雜記憶管理的場景，直接寫迴圈就不夠用，作者也明確承認「答案會不一樣」
- 守門邏輯若寫在路由層而非資料存取層，多一個入口（如網頁介面）就可能漏擋——作者曾因此出過刪除漏擋的 bug

## R（操作步驟）

1. **raw/ 唯讀要擋兩次**：擋改寫與擋刪除都寫在 `notes.ts` 資料存取層（而非路由層），因為 Web/MCP/REST 三條路最後都呼叫同一組函式，守門只需一次，新入口自動被擋住。
2. **wiki/ 工具清單刻意精簡到六個**：`get_instructions`、`search_notes`、`read_note`、`create_note`、`update_note`、`list_folder`——沒有 `delete_note`，設計上頁面只能封存、不能真的消失；寫入時版本快照、連結、標籤在同一交易內一起寫（`writeDerived`），避免「內容存了但連結沒更新」的中間狀態。
3. **schema/ 純讀取回傳，程式完全不解析內容**：`getInstructions` 用 `SELECT ... WHERE path LIKE 'schema/%'` 撈出規則原樣塞給 agent；多一種知識庫類型（如研究者專用規則）只需多寫一份 Markdown，不需改程式，因為路徑檢查只認 `raw/`、`wiki/`、`schema/` 三個開頭。
4. **待編纂狀態用連結反查計算**：`raw/` 來源只要沒有任何 `wiki/` 頁面連回它，就是 `NOT EXISTS` 查出來的「待編纂」，不是靠欄位標記；`get_instructions` 首次調用會把待編纂清單與規則一起回傳。
5. **Lint 切成確定性檢查與語意檢查兩段**：孤兒頁、斷連結、log 格式、待編纂清單是 SQL 查得出來的事實，先跑完再把結果餵給 agent 做語意判斷（矛盾、過時主張），讓模型的注意力留給只有它做得到的那一半。
6. **工具呼叫迴圈只有七十行**：`for` 迴圈呼叫模型 → 若無 `toolCalls` 則結束 → 否則逐一 `exec` 並把結果塞回 messages；Anthropic 與 OpenAI 相容兩套 `callModel` 共用同一骨架，伺服器端 agent 與 MCP 端 agent 用**同名同介面**的六個工具，差別只在傳輸方式（前者直接呼叫資料層函式，後者走 HTTP）。
7. **重試依 HTTP 狀態碼判斷，不依錯誤訊息字串**：429/5xx/逾時退避重試（2s/5s/12s，最多三次），401/400 直接放棄；要關掉 SDK 自帶重試，避免退避策略疊加。
8. **使用者自付 key 的逐步記帳**：每步 token 用量累加成金額，用 OpenRouter 公開價目當三家供應商參考價，設定頁與進度面板即時顯示花費，讓使用者對「按下去會花多少錢」有信任感。

## C（關鍵行動）

- [ ] 盤點自己專案裡「弄壞了救不回來」的操作，優先寫成程式碼鐵則而非規則文件
- [ ] 檢查守門邏輯是否寫在所有客戶端都會經過的資料存取層，而非個別路由
- [ ] 若功能涉及使用者自付 API key，設計逐步記帳與即時花費顯示機制

## 原始摘要

Day03：WikiBrain 系列，講「怕 LLM 弄壞的部分寫成程式碼，其他寫成 Markdown 規則」的具體實作，涵蓋 raw/ 唯讀雙重防呆、wiki/ 工具清單設計、schema/ 使用者可改機制、Ingest/Query/Lint 三操作的實際 prompt 片段與 SQL。

Day04：接續 Day03，講怎麼讓 LLM 真正執行 schema/ 裡的規則，核心是一個七十行、不用任何 agent 框架的工具呼叫迴圈，並說明伺服器端/MCP 端同介面不同傳輸、重試退避策略、使用者自付 key 的逐步記帳機制。

## 相關連結

- [[llm-wiki-over-rag-claude-code]] — Karpathy LLM Wiki 概念/方法論層條目（三層架構、Ingest/Query/Lint 已涵蓋），本條目補其缺乏的操作層實作細節
- [[wikibrain-compounding-knowledge-base-demo]] — 同系列 Day05，接續本條目：本篇談「怎麼把六條準則寫成程式碼」，該篇談「程式碼跑起來之後複利長什麼樣子」（實測兩篇來源、完整成本數據）
- [[mcp-server-stateless-streamable-http-oauth]] — 同系列 Day06（＋Day10 選型）：無狀態 Streamable HTTP、中介層順序、401 的 OAuth 探測標頭（2026-09-23 新增）
- [[tool-description-and-error-message-as-prompt]] — 同系列 Day07（＋Day08 衝突回應）：工具描述寫平常怎麼做、錯誤訊息寫碰壁怎麼辦（2026-09-23 新增）
- [[human-agent-shared-data-layer-optimistic-locking]] — 同系列 Day08（＋Day11 草稿與復原）：樂觀鎖、工作區隔離、保護使用者的輸入（2026-09-23 新增）
- [[wikilink-as-database-edges-and-graph]] — 同系列 Day09（＋Day11 圖譜）：wikilink 寫入時解析成邊、懸空補洞、ReDoS 防護（2026-09-23 新增）
- [[raw-immutable-derived-regenerable-data-boundary]] — 同一原則的非工程師版：用「誰產生、能否重來」分三類保護等級，raw 以規則＋OS 唯讀＋備份三層防護（2026-09-23 新增）
