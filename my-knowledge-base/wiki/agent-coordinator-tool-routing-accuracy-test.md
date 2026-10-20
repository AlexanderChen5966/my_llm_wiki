---
title: Coordinator 實作：Tool Description 撰寫與 Routing Accuracy／Task Completion 雙指標
date: 2026-09-03
tags: [ai/agent, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10405573（Data Machi 30 天學習系列 Day 19）
type: article
status: processed
asset_value: 64
---

## T（核心論點）

Coordinator 能否正確選擇工具，很大一部分取決於 **Tool Description 是否清楚說明「什麼時候該用／什麼時候不適合用」**，且 Tool 責任最好不要彼此重疊（避免出現 `search_everything()` 這種萬用工具）。Tool Result 應保存成結構化 JSON（含 tool、query、result、source、queried_at）而非只留自然語言，以利多輪對話判斷能否沿用、Verification 比對與錯誤定位。驗收 Coordinator 不能只看 Demo 能不能回答，要建立固定 **Routing Test**，並把 **Routing Accuracy**（Tool 與執行順序是否正確，複合問題須整條路徑都對才算通過）與 **Task Completion**（使用者任務是否真的完成）分開追蹤——兩者不必然一致，Routing 正確不代表任務完成，答案正確也不代表 Routing 過程穩定。

## B（適用場景）

**適用：** 撰寫清楚區分「何時用／何時不用」的 Tool Description；設計 Tool Result 的結構化資料格式；建立 Routing Test 量化 Coordinator 表現；把 Routing 失敗分類（選錯 Tool、漏掉 Tool、多叫 Tool、順序錯誤等）以定位需要修改的環節。

**不適用 / 注意：** 本篇第一版只管理兩個 Tool（Google Sheets + PDF RAG），刻意不追求大量 Tool 或 Multi-Agent；何時該拆成 Multi-Agent 需另外評估（Tool 數量極多、角色需要不同能力與權限、明確角色交接流程三種情境才值得）。

## R（操作步驟）

1. **Tool Description 明確標示適用與不適用情境**：例如 `query_google_sheets` 說明「適合結構化資料、精確數字、統計；不適合政策與文件定義問題」，`search_documents` 反過來說明。
2. **Tool 責任不要重疊**：拆成 `query_structured_data()`／`search_documents()`／`get_project_status()` 三個獨立 Tool，而非一個 `search_everything()`，讓 Coordinator 保有選擇資料來源的能力，也讓最終答案來源可追溯。
3. **第一版先測三種基本路由**：純數據問題（→ Sheets）、純文件問題（→ Document）、跨來源問題（→ Sheets → Document）。
4. **Tool Result 保存結構化 JSON**：包含 `tool`、`query`（含條件）、`result`、`source`、`queried_at`，模型最後整理成自然語言時，系統內部仍保留原始資料。
5. **保留 Coordinator 執行軌跡**：Request ID、執行節點、Tool、Query Parameters、Tool Result Status、Duration、Error Type，第一版可先用 Structured Log 而非視覺化 Trace 工具。
6. **Tool 失敗時明確標示是哪一層失敗**：保留 `google_sheets: SUCCESS` / `document_search: FAILED, error=INDEX_UNAVAILABLE` 這類分層狀態，延續 Day 15 的 Partial Success 概念。
7. **建立 Routing Test**：每題定義 Expected Path 與 Actual Path，計算 `Routing Accuracy = 完全符合預期路徑題數 / 全部題數`；複合問題必須整條路徑都對才算通過。
8. **分開追蹤 Routing Correct 與 Task Completed**：Routing 正確但 Tool 執行失敗 = Task Completion 失敗或部分成功；答案剛好正確但路徑繞了一圈 = Routing 不理想。
9. **把 Routing 失敗分類**：選錯 Tool、漏掉 Tool、多叫 Tool、順序錯誤、該追問卻分流、不必要重查、錯用舊結果，各自指向不同修改方向（Description、Clarification、任務拆解）。

## C（關鍵行動）

- [ ] 每個 Tool Description 都明確寫出「什麼時候該用／什麼時候不適合用」，而不是模糊的一句話說明
- [ ] Tool Result 一律保存結構化 JSON（tool/query/result/source/queried_at），不要只留模型整理後的自然語言
- [ ] 建立固定 Routing Test，計算 Routing Accuracy 與 Task Completion 兩個獨立指標
- [ ] 把 Routing 失敗依類型分類（選錯/漏掉/多叫/順序錯誤等），定位真正該修改的環節
- [ ] 第一版只管理少數 Tool（2-3 個），先確認穩定分流再逐步加入更多來源，不急著拆成 Multi-Agent

## 原始摘要

「Data Machi 30 天學習系列」Day 19：實作將 Google Sheets Tool 與 PDF RAG 收進同一個 Coordinator，重點在 Tool Description 需清楚寫明使用時機、Tool Result 應保存結構化資料，並建立 Routing Test 量化 Routing Accuracy 與 Task Completion 兩個獨立指標。原則：Tool 能解決的問題先做成 Tool，單一 Coordinator 能管理的問題先不要拆成 Multi-Agent。下一篇（Day 20）處理一個問題要查三個來源時的平行/循序安排。

## 相關連結

- [[agent-router-vs-coordinator-state-tracking]] — **系列上一篇（Day 18）**：本篇是該篇 Router/Coordinator 概念的具體實作
- [[agent-multi-source-query-parallel-sequential-scheduling]] — **系列下一篇（Day 20）**：把 Coordinator 放進更複雜的多來源任務，處理 Parallel/Sequential 安排
- [[langgraph-coordinator-refactor-seven-nodes]] — 既有條目：同為 Coordinator 主題但聚焦 LangGraph 七節點重構實作，本篇聚焦 Tool Description 撰寫與 Routing Accuracy/Task Completion 雙指標量測方法，角度互補
