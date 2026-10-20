---
title: Agent UX 五級成熟度：狀態要來自 Workflow 真實狀態，不是模型自己生成
date: 2026-09-08
tags: [ai/agent, ai/workflow, type/concept]
source: https://ithelp.ithome.com.tw/articles/10408179（《Data Machi 30 天》Day 27）
type: article
status: processed
asset_value: 60
---

**系列：《Data Machi 30 天學習系列：從 Chat 到 Product》Day 27**。
前一天的後端可靠性機制見 [[agent-reliability-timeout-retry-fallback-verification]]，
Workflow 拆解見 [[langgraph-coordinator-refactor-seven-nodes]]，
下一天的權限治理見 [[enterprise-ai-permission-three-layers-governance]]。

## T（核心論點）

> **Agent UX 要解決的不是把等待動畫做漂亮，而是降低等待期間的不確定感。**
> 而降低不確定感的關鍵在於：**進度提示必須來自 Workflow 的真實狀態，不是讓模型自己生成
> 「請稍等，我正在查詢」。**

Chatbot 只等一次模型呼叫；Agent Workflow 是 Router → 多個 Tool → Verification →（Retry/Fallback）
組成的十幾秒以上流程。全程只顯示 Loading，使用者會懷疑系統卡住並重送請求，反而製造更多 Request。

由此衍生兩組最容易被混掉的區分：
- **「內容正在產生」≠「任務已完成」**（Streaming 開始出字時 `status` 仍可能不是 `completed`）
- **「系統在等」≠「系統在等你」**（`running` vs `waiting_for_user`）

## B（適用場景）

**適用：**
- 任何自建 Agent／Workflow 產品的前端狀態設計，尤其單次任務超過十秒
- 已導入 Human-in-the-loop，但畫面上分不出「還在處理」與「在等你批准」
- 想評估自家 Agent 產品目前的 UX 落在哪一級（本篇附五級成熟度尺）

**不適用 / 注意：**
- ⚠️ **透明 ≠ 攤開 Chain-of-Thought**：模型完整內部推理、隱藏 Prompt、API Key 與 Tool 參數都不該顯示
- ⚠️ 不要硬算百分比（Verification 後回頭補查會讓 80% 倒退成 60%）
- 🔴 前端有「繼續任務」按鈕不代表系統真的能繼續——Level 5 完全取決於後端是否真的保存 State

## R（操作步驟）

### 1. 把技術 Node 翻成工作語言（而非直接顯示 Node 名）

| Workflow 狀態 | 使用者看到的提示 |
| --- | --- |
| `planning`（route_request） | 正在理解你的問題 |
| `retrieving_data`（query_google_sheets） | 正在查詢數據 |
| `searching_documents`（search_documents） | 正在搜尋相關文件 |
| `checking_project` | 正在確認專案狀態 |
| `verifying`（verify_results） | 正在確認資料是否完整 |
| `generating`（generate_answer） | 正在整理回答 |
| `completed` / `failed` | 任務完成 / 任務未完成 |

好處是 UX 狀態不必和 Agent 邏輯分開維護；壞處若讓 LLM 自由生成 Progress Message，
**同一個狀態今天叫「正在分析」明天變「正在思考下一步」**。

### 2. 分辨什麼該顯示、什麼不該

**適合**：正在搜尋 PDF、正在重新連線資料來源、已完成 2/3 個來源。
**不該**：模型完整內部推理、隱藏 Prompt、API Key 與 Tool 內部參數。

### 3. 用 Milestone-based Progress 取代百分比

「已完成數據查詢／已找到相關文件／正在確認專案狀態／接著會整理結果」
比一個虛假的 73% 更可信，且不會因為動態補查而倒退。

### 4. Streaming 要與完成狀態脫鉤

維持 `planning / retrieving / verifying / generating / completed / partial / failed` 七個狀態；
即使畫面已有部分文字，只要 `status != completed` 就仍標示「正在完成剩餘步驟」。

🔑 更關鍵的是 **不要在 Verification 前就串流出看似確定的因果結論**——先說「主要是 Paid Traffic
增加造成的」，驗證後才發現證據不足，即使後面修正，使用者已經看過錯誤結論。要串流最終文字，
也應在主要 Verification 通過之後才開始。

### 5. Error UX 要讓使用者判斷「是誰的問題、現在能做什麼」

| Backend Error | 使用者看到的訊息 |
| --- | --- |
| 403 Permission Denied | 目前沒有 Google Sheets 讀取權限，請確認共享設定 |
| Timeout | 資料來源暫時沒有回應，你可以重新嘗試 |
| No Result | 目前沒有找到符合條件的資料，可以調整查詢條件 |
| Missing Input | 還缺少時間範圍，請選擇本月或最近 90 天 |
| Rate Limit | 服務目前繁忙，系統稍後可以重新嘗試 |

🔑 **「找不到資料」與「無法存取來源」意義完全不同**：前者是條件真的沒資料，後者是根本沒成功看到來源。
兩者都寫成「查詢失敗」，使用者就無法判斷下一步。

### 6. Partial Success 要留在 UI，不要整頁消失

Sheets 成功、PDF 成功、Project API Timeout 時，照常顯示前兩段，並在專案位置標示
「目前無法取得最新專案進度」，或直接列成工作項目狀態表。遠勝 `Something went wrong`。

### 7. Retry 期間要更新狀態，但不必暴露細節

「資料來源暫時沒有回應，正在重新嘗試」→ 用完 Retry Limit 才轉「已停止重新嘗試」。
使用者不需要知道「Retry 2/3、Backoff 4 秒」，除非這是工程工具。

### 8. Human-in-the-loop 需要獨立狀態

`waiting_for_user` 畫面必須說明：即將執行什麼操作、影響哪個系統、用哪些資料，並提供 Approve/Reject。
例：「已完成 Email 草稿。確認後將寄送給 Project Team。」
🔑 **Intent、Draft、Approval、Execution、Completion 都該是不同狀態**——
「已建立 Task」與「已產生草稿尚未建立」對使用者完全不同。

### 9. 五級成熟度（可當自評尺）

| 級別 | 使用者看到的體驗 |
| --- | --- |
| **L1** | 從頭到尾只有 Loading（Prototype 常見） |
| **L2** | 有進度，但全是 Node／API／Tool 技術術語（`Executing node: document_retriever`） |
| **L3** | 狀態翻成工作語言，看得懂正在理解／搜尋／驗證／整理 |
| **L4** | Error 可行動＋Partial Success 與 Completion 明確 |
| **L5** | 長任務可離開頁面後追蹤與恢復 |

Level 5 需要 Task 有獨立 `task_id`，後端保存 Status、Current Step、Created/Updated At、
Result、Error、Checkpoint，並靠 Day 25 的 Checkpointer／Interrupt-Resume／Durable Execution
真的能 Resume 而非整條重跑。

### 10. 心智模型的轉變

從 **「送出問題 → Loading → Answer」** 變成
**「送出任務 → 顯示階段 → 多步驟執行 → 必要時詢問或等批准 → 完成或部分完成」**。
差別不只是多了 Progress Bar，而是產品從「聊天」轉向「委派工作」（Workflow UX）。

## C（關鍵行動）

- [ ] 用五級成熟度自評手上的 Agent 產品，找出停在哪一級與缺什麼
- [ ] 檢查進度文案是模型生成的還是 Workflow 狀態驅動的；若是前者改成後者
- [ ] 為 Human-in-the-loop 加一個獨立的 `waiting_for_user` 狀態
- [ ] 把「找不到資料」與「無法存取來源」拆成兩種訊息
- [ ] 若目標是 L5，先確認後端真的有 Checkpoint 可 Resume 再做前端按鈕

## 原始摘要

Data Machi 系列 Day 27。主張 Agent UX 的目標是降低等待期間的不確定感，做法是讓進度提示直接對應
LangGraph Node 的真實狀態並翻成工作語言，而非讓模型自行生成或攤開 Chain-of-Thought。提出兩組關鍵
區分（內容正在產生 ≠ 任務已完成、系統在等 ≠ 系統在等你），並反對硬算百分比、主張 Milestone-based
Progress。另涵蓋 Streaming 的過早結論風險、可行動的 Error UX、Partial Success 的 UI 呈現、
Retry 期間的狀態更新，最後給出 Agent UX 五級成熟度模型與長任務 task_id 的設計。

## 相關連結
- [[ai-native-interface-patterns]] — 🔑 **互補關係**：該篇談「該呈現什麼元件」（20 種 AI-native 元件模式），本篇談「狀態語意如何定義與分級」；本篇的五級成熟度可當該篇的評估尺
- [[agent-reliability-timeout-retry-fallback-verification]] — 同系列 Day 26：本篇的 Error UX 是它 Error Classification 的前端對應
- [[langgraph-coordinator-refactor-seven-nodes]] — 同系列 Day 25：本篇的可讀狀態直接來自它拆出的 Node
- [[enterprise-ai-permission-three-layers-governance]] — 同系列 Day 28
- [[agent-error-recovery-taxonomy]] — Error UX 的分類基礎（八型錯誤）
- [[claude-agent-sdk-human-in-the-loop]] — `waiting_for_user` 狀態的 SDK 實作面
- [[human-in-the-loop-langchain]] — 暫停與恢復的 Middleware 實作
