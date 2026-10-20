---
title: 把 Coordinator 拆成七個 Node：Graph 控制邊界，Agent 在邊界內判斷
date: 2026-09-08
tags: [ai/agent, ai/workflow, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10407967（《Data Machi 30 天》Day 25）
type: article
status: processed
asset_value: 60
---

**系列：《Data Machi 30 天學習系列：從 Chat 到 Product》Day 25**（本批 Day 25–28 四篇的起點）。
前一天的 LangGraph 基礎概念（State／Node／Edge、多 Node ≠ Multi-Agent）見 [[langchain-langgraph-basics]]；
後續 Day 26 可靠性機制見 [[agent-reliability-timeout-retry-fallback-verification]]，
Day 27 前端狀態呈現見 [[agent-ux-status-transparency-maturity]]，
Day 28 權限治理見 [[enterprise-ai-permission-three-layers-governance]]。

## T（核心論點）

> **Graph 控制邊界，Agent 在邊界內做判斷。**

「這題該查 Sheets 還是文件」「文件搜尋結果不足時 Query 怎麼改」可以繼續交給 LLM；但
「Verification 沒通過不得進 Action」「Retry 不超過兩次」「高風險 Action 必經 Approval」
**必須由 Edge 與 State counter 保證，不能寫在 Prompt 裡**。差別在於：寫在 Prompt 是「請模型記得」，
寫進 Graph 是「模型根本沒有其他路可走」。

同時給出一個反向判準：**不要為了導入 LangGraph 而拆 Multi-Agent**。Router 只是一次 LLM
classification、Verification 有些只是 Rule、Approval 的決策者是人——這些都不是 Agent。

## B（適用場景）

**適用：**
- 手上已有一個責任過度集中的 Coordinator／單一 Agent Loop，想拆解卻不知從哪切
- 需要回答「這一題到底是哪一步做錯了」這個除錯問題
- 要導入 Human-in-the-loop，且暫停後必須還能繼續（Checkpointer／Resume）

**不適用 / 注意：**
- ⚠️ 簡單 RAG 硬拆七個 Node 是過度設計；「架構剛好能承受問題的複雜度」才是最好
- 🔴 **可控 ≠ 寫死**：LangGraph 的目標不是把 Agent 變成傳統 RPA，被固定的只有邊界規則
- 本篇是 Workflow 規格層，不是 LangGraph API 教學（作者也建議先產出「工程師看得懂的 Workflow Specification」再寫程式）

## R（操作步驟）

### 1. 先把 Coordinator 的責任拆成七個 Node

| Node | 責任 |
| --- | --- |
| Router | 只判斷 `required_capabilities`，判斷完就結束 |
| Data Tool | 查結構化資料 |
| Document Tool | 搜尋文件與 RAG |
| Verification | 確認結果是否足以回答 |
| Clarification | 向使用者補問必要條件 |
| Approval | 高風險 Action 前取得人工確認 |
| Answer | 用已驗證結果產生最終回答 |

關鍵紀律：**Router 不得順手查資料或產生答案；Tool 只回傳結構化結果＋來源＋timestamp，
「夠不夠回答」由下一個 Node 決定**。這樣「送錯來源」只測 Router、「數字算錯」只查 Data Tool。

### 2. 先定 State，再畫 Edge

13 個第一版 State 欄位：`current_question`、`conversation_context`、`required_capabilities`、
`query_conditions`、`tool_results`、`sources`、`verification_status`、`missing_information`、
`retry_count`、`pending_action`、`approval_status`、`errors`、`final_answer`。

State 的目的不是無限保存資料，而是讓 Node **不必靠一大段 Prompt 猜測前面發生了什麼**。

### 3. Verification 的六種結果各對應一條 Edge

`passed`→Answer／`missing_evidence`→補查另一來源／`missing_input`→Clarification／
`stale_data`→重查 Source of Truth／`tool_failed`→依 Error Policy／`conflict`→處理來源衝突。

### 4. 錯誤策略寫成表，Retry Limit 由程式控制

Timeout→有限 Retry；Rate Limit→等待或 Fallback；Permission Denied→停止並說明；
Missing Input→Clarification；No Result→改寫 Query 或回報；Verification Failed→回到相關 Tool；
超過 Retry Limit→Stop／Partial Answer。

### 5. 複合問題要區分資料依賴，而不只是列出 Tool

- 「今年總工單數＋工單的正式定義」→ 兩問題獨立，可 **Parallel**
- 「哪類工單最多＋**這一類**的定義」→「這一類」依賴前一步結果，必須 **Sequential**

依賴關係要寫進 Edge，不能靠模型每次自己記得順序。

### 6. 高風險 Action 前插 Approval，並讓權限分階段開放

`準備 Action → Human Approval →（Approve）Execute ／（Reject）Cancel`。
權限五階段演進：**Read → Draft → Approve-to-Execute → Limited Automation → Broader Automation**。
第一版 Email Tool 只產 Draft，等 Audit、Rollback、Access Control 成熟才逐步開放自動化。

### 7. 暫停之後還要能繼續：三機制＋Freshness Check

需要 **Checkpointer（檢查點）／Interrupt-Resume（中斷與恢復）／Durable Execution（持久執行）**。
🔑 而且不能是 `Resume → Execute`，而應是 **`Resume → Freshness Check → Verification → Execute`**——
早上 9 點查到的數據，主管下午 3 點才批准時可能已失效。

### 8. 測試看「走了哪條 Edge」，不只看答案

五條固定路徑：單一來源／跨來源／資訊不足／Tool 失敗／高風險 Action。
建表記錄「預期 Path vs 實際 Path」：**即使最終答案碰巧正確，只要跳過 Verification 或多繞
Tool，仍算架構測試沒有通過。**

## C（關鍵行動）

- [ ] 把自己 Agent 專案中「寫在 Prompt 裡的規則」逐條分類：哪些屬於邊界（該搬進 Edge／程式），哪些屬於判斷（可留給模型）
- [ ] 對任何有 Approval 的流程，補上 `Resume → Freshness Check` 這一步
- [ ] 測試表改成「預期路徑 vs 實際路徑」，而非只有「答案對不對」
- [ ] 檢查是否誤把 Tool 數量當成 Agent 數量（六個 Tool 拆六個 Agent 只是多出 Context 傳遞與除錯成本）

## 原始摘要

Data Machi 系列 Day 25 的實作篇：把責任過度集中的 Coordinator 拆成 Router／Data Tool／
Document Tool／Verification／Clarification／Approval／Answer 七個 Node，先定義 13 個 State
欄位作為 Node 間唯一傳遞介面，再把資料依賴、錯誤策略、Retry Limit 與 Approval 寫成 Edge。
核心主張是「Graph 控制邊界，Agent 在邊界內做判斷」，並明確反對為導入 LangGraph 而拆 Multi-Agent。
另提出權限五階段演進、Checkpointer／Interrupt-Resume／Durable Execution 三機制，
以及 Resume 前的 Freshness Check。

## 相關連結
- [[langchain-langgraph-basics]] — LangGraph 基礎（StateGraph、State/Node/Edge 定義、多 Node ≠ Multi-Agent 判準）；本篇是它的七 Node 實作版，不重述基礎
- [[agent-reliability-timeout-retry-fallback-verification]] — 同系列 Day 26：本篇的錯誤策略表在該篇展開成 Timeout／Retry／Fallback／Verification 四機制
- [[agent-ux-status-transparency-maturity]] — 同系列 Day 27：本篇拆出的 Node 正好成為前端可讀狀態的來源
- [[enterprise-ai-permission-three-layers-governance]] — 同系列 Day 28：本篇的 Approval Node 是其中的 Permission Boundary
- [[supervisor-multi-agent-langchain]] — Coordinator／Supervisor 模式；🔑 **本篇提供對立論點**：不要為了導入 Graph 而拆 Multi-Agent
- [[human-in-the-loop-langchain]] — Middleware 版 HITL；本篇補上 Checkpointer／Interrupt-Resume／Durable Execution 與 Freshness Check
- [[agent-tool-authorization-gatekeeper]] — Tool 授權把關；對應本篇的權限五階段演進
- [[graph-engineering-node-edge-state]] — Node/Edge/State 編排框架的另一種取徑
- [[subagent-four-boundaries]] — 何時才真的該拆出獨立 Agent
