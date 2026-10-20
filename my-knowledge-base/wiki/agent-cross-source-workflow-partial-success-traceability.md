---
title: 跨來源查詢工作流：資料依賴拆解、Partial Success 與可溯源 Metadata
date: 2026-09-03
tags: [ai/agent, ai/workflow, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10405380（Data Machi 30 天學習系列 Day 15）
type: article
status: processed
asset_value: 64
---

## T（核心論點）

跨來源查詢的第一步不是把所有 Tool 都叫一遍，而是先拆解問題、判斷子問題之間有沒有**資料依賴（Data Dependency）**：沒有依賴可以 Parallel 執行降低總等待時間，有依賴（後一步輸入來自前一步輸出）就必須 Sequential，且第二步必須使用第一步**精確計算結果**而非讓模型自行猜測。第一版刻意採用 Deterministic Workflow（程式寫死步驟）而非全自動 Agent，方便逐層除錯；每個 Tool Result 都要保留 Source／Retrieved At／Query／Raw Result 四類 Metadata，單一來源失敗時採 **Partial Success**（保留已成功結果、明確指出缺什麼），而非整體回覆「發生錯誤」。

## B（適用場景）

**適用：** 設計第一個多工具跨來源查詢流程（如 Google Sheets + PDF RAG）；決定資料查詢步驟該平行還是循序；設計 Tool Result 的可溯源 Metadata 結構；處理單一來源失敗但其他來源已成功的情境。

**不適用 / 注意：** 本篇刻意使用固定 Workflow，尚未引入 Agent 動態決策（見 [[agent-tool-vs-workflow-vs-agent-boundary]]）；不涉及平行/循序的自動判斷機制（後續 Day 20 才處理，見 [[agent-multi-source-query-parallel-sequential-scheduling]]）。

## R（操作步驟）

1. **先拆題，再判斷資料依賴**：把使用者問題拆成子問題並標出適合的資料來源；若後一子問題需要前一子問題的結果（如「這個類別」需要先知道 Top Category），即為 Sequential；彼此獨立則可 Parallel。
2. **第一版用 Deterministic Workflow**：明確寫死 `query_google_sheets() → search_documents(top_category) → get_project_status(top_category) → combine_results()`，同樣輸入按固定路徑執行，方便逐層排查是哪個 Tool 算錯或查錯。
3. **保留四類 Metadata**：每個 Tool Result 附上 Source（資料來源系統）、Retrieved At（查詢時間）、Query/Conditions（查詢條件）、Raw Result（原始結果），確保「Delivery Issue 有 382 件」這類錯誤可以追回是 Tool 算錯還是模型整合時算錯。
4. **Partial Success 原則**：某 Tool 失敗（如 Trello Timeout）時保留已成功結果，明確告知使用者缺少哪部分，而不是回覆「系統發生錯誤」。
5. **用最少工具完成任務**：不要因為接了 5 個工具就每次全部呼叫；好的跨來源 Workflow 追求「使用足夠完成任務的最少工具」。
6. **準備三類測試問題**：單一來源、兩個獨立來源（測平行）、有資料依賴（測 Output→Input 傳遞是否精確，而非模型自行猜測條件）。

## C（關鍵行動）

- [ ] 建立資料依賴表：子問題／Source of Truth／必要輸入／是否依賴前一步／預期輸出／失敗處理
- [ ] 每個 Tool Result 附上 Source／Retrieved At／Query／Raw Result 四類 Metadata
- [ ] 第一版先用固定 Deterministic Workflow，確認每個 Tool 可靠、資料能正確傳遞，再逐步把決策交給 Agent
- [ ] 單一來源失敗時採 Partial Success，保留已成功結果並明確指出缺失部分
- [ ] 第二步查詢一定要用第一步的精確計算結果，不能讓 LLM 自行猜測條件繼續往下查

## 原始摘要

「Data Machi 30 天學習系列」Day 15：實作第一個跨來源（Google Sheets + PDF RAG）查詢流程，強調先拆解問題判斷資料依賴，再決定 Sequential 或 Parallel 執行；第一版刻意用固定 Workflow 而非 Agent 方便除錯；每個 Tool Result 應保留四類 Metadata 以利溯源；單一來源失敗時採 Partial Success 而非整體失敗。這是 Data Machi Day15→30 系列鏈的起點，下一篇（Day 16）將討論 Tool Use 何時真正需要進化成 Agent。

## 相關連結

- [[agent-tool-vs-workflow-vs-agent-boundary]] — **系列下一篇（Day 16）**：承接本篇固定 Workflow 的限制，討論 Tool Use 何時真正需要進化成 Agent
- [[agent-memory-three-layers-reuse-vs-requery]] — 系列 Day 21：本篇 Partial Success 與 Metadata 溯源的概念，延伸到多輪對話的 Reuse vs Re-query 判準
