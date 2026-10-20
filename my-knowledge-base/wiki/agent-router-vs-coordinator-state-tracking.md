---
title: Router 與 Coordinator：分流不代表完成任務，Coordinator 需持續追蹤狀態
date: 2026-09-03
tags: [ai/agent, type/concept]
source: https://ithelp.ithome.com.tw/articles/10405571（Data Machi 30 天學習系列 Day 18）
type: article
status: processed
asset_value: 64
---

## T（核心論點）

Router 像總機，只回答「這題該去哪」，適合單一意圖、成本低、行為可預測；Coordinator 像專案經理，需要持續追蹤「使用者目標是否已被滿足」直到 COMPLETED，適合多步驟跨來源任務。兩者關鍵差異是**是否維持任務狀態（State）**：Coordinator 必須保留 Current Task/Selected Period/Top Market/Tool History/Pending 等結構化資訊，才能理解「那相關專案呢？」這類指代省略句。Coordinator 還需判斷使用者訊息屬於全新問題／延續追問／格式調整／重新查核等類型，決定是否 Reuse 既有結果或 Re-query——**Router 的判斷是建議，不是最終命令**，Tool 回傳 No Result 時 Coordinator 應能根據 Observation 重新評估並改查其他來源。

## B（適用場景）

**適用：** 判斷系統該用單純 Router 還是需要 Coordinator；設計 Coordinator 的 State 欄位（Current Question、Tool History、Missing Information、Task Status 等）；判斷使用者訊息是否需要重新查詢還是可直接沿用舊結果；決定 Router 判斷與 Coordinator 決策之間的關係。

**不適用 / 注意：** 本篇是概念層的角色分工說明，尚未涉及 Coordinator 的具體實作與 Tool Description 撰寫（見 [[agent-coordinator-tool-routing-accuracy-test]]）；本篇 Reuse vs Re-query 的訊息分類表是簡化版，完整判準（TTL、Freshness）見既有條目 [[agent-memory-three-layers-reuse-vs-requery]]。

## R（操作步驟）

1. **Router 只回答分流問題**：判斷 `data_query`／`document_query`／`project_query` 之一，交給對應 Tool，工作即結束；適合一題對應一個 Tool 的情境。
2. **Coordinator 持續管理整段任務**：理解任務 → 查看已有資訊 → 判斷還缺什麼 → 選 Tool → 更新狀態 → 任務完成了嗎 → 否則繼續、是則整合回答。
3. **Context 保存結構化任務資訊，而非完整對話紀錄**：Current Task、Selected Period、Top Market、Top Category、Used Tools、Pending 等關鍵欄位，比塞入二十輪完整對話更容易管理。
4. **使用者訊息分類表**：全新問題→新查詢；延續追問（那去年呢）→沿用問題但改條件重查；格式調整（整理成表格）→純 Reuse 不重查；要求重新確認→強制 `force_refresh=true`；條件修改（那香港呢）→保留任務但更換市場後重查。
5. **Router 判斷是 Domain Hint，不是絕對命令**：Router 判斷 `project_query` 但 Tool 回傳 No Result 時，Coordinator 應能重新評估（可能是縮寫問題），改查 Document Tool 確認正式名稱再重查。
6. **Coordinator 最小 State 欄位**：Current Question、Conversation Context、Intent/Domain Hint、Tool History、Tool Results、Query Conditions、Sources、Retrieved At、Missing Information、Task Status。
7. **知道什麼時候不做事**：使用者只要求換格式或翻譯時應直接 Reuse，避免因重新查詢導致同一段對話前後數字不一致造成使用者困惑。

## C（關鍵行動）

- [ ] 只有單一意圖、一題對應一個 Tool 的場景，先用 Router 即可，不必急著上 Coordinator
- [ ] Coordinator 的 Context 設計成結構化欄位（Current Task/Tool History/Missing 等），不要塞入完整對話紀錄
- [ ] 建立訊息分類判準（全新問題/延續追問/格式調整/重新查核），決定 Reuse 或 Re-query
- [ ] 把 Router 判斷當作 Domain Hint 而非最終命令，Coordinator 應能依 Observation 修正方向
- [ ] 使用者只要求換格式或翻譯時直接 Reuse 既有結果，避免同一段對話因重查導致數字前後不一致

## 原始摘要

「Data Machi 30 天學習系列」Day 18：說明 Router（分類分流，適合單一意圖）與 Coordinator（管理整段任務直到目標完成，適合多步驟跨來源）的差異。Coordinator 須維持任務狀態以理解指代，並依訊息類型判斷 Reuse 或 Re-query。Router 判斷只是 Domain Hint，Tool 回傳 No Result 時 Coordinator 應能重新評估。下一篇（Day 19）實作把 Google Sheets Tool 與 PDF RAG 收進同一 Coordinator。

## 相關連結

- [[agent-react-reason-act-observe-stop-conditions]] — **系列上一篇（Day 17）**：本篇的 Coordinator 決策循環延續該篇 ReAct 的 Reason-Act-Observe 概念
- [[agent-coordinator-tool-routing-accuracy-test]] — **系列下一篇（Day 19）**：把本篇 Router/Coordinator 概念落實成具體 Tool Description 與 Routing Test 實作
- [[langgraph-coordinator-refactor-seven-nodes]] — 既有條目：把 Coordinator 拆成七個 LangGraph Node 的實作面案例，可與本篇的角色分工概念互補，建議互相 wikilink
- [[agent-memory-three-layers-reuse-vs-requery]] — 既有條目：完整的 Reuse vs Re-query 判準與 TTL/Freshness 概念，補強本篇簡化版訊息分類表
