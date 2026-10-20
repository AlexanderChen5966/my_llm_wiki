---
title: Enterprise AI Blueprint：六階段演進與八問收斂框架（Data Machi 系列完結）
date: 2026-09-11
tags: [ai/agent, ai/workflow, type/concept]
source: https://ithelp.ithome.com.tw/articles/10409206（《Data Machi 30 天》Day 30，系列完結篇）
type: article
status: processed
asset_value: 75
---

**系列：《Data Machi 30 天學習系列：從 Chat 到 Product》Day 30（系列完結篇，共 31 篇）。**
前一天的部署實作見 [[data-machi-deploy-render-vercel-cors-secrets]]；
Day 25 Workflow 拆解見 [[langgraph-coordinator-refactor-seven-nodes]]；
Day 26 可靠性機制見 [[agent-reliability-timeout-retry-fallback-verification]]；
Day 27 前端狀態設計見 [[agent-ux-status-transparency-maturity]]；
Day 28 權限治理見 [[enterprise-ai-permission-three-layers-governance]]。
本篇是整個系列的收斂與總結，不重述前面各篇細節，只整理成可攜帶到其他場景的骨架。

## T（核心論點）

> **企業 AI 的價值不在於模型回答得多像人，而在於能不能把資料、工具、判斷與流程串成一段可靠的工作。**

30 天的名詞清單（RAG、Tool Use、LangGraph、Retry、CORS、Service Account……）看似分散，
拆開來看其實只是持續解決同一個問題：**如何把一個只會回答問題的模型，逐步改造成能可靠完成
企業知識工作的產品。** 六個階段不是新技術取代舊技術，而是疊加——走到 Product 階段後，
最底層仍然有 Chat，文件問題仍然需要 RAG，數字仍然交給 Tool。

🔑 **成熟的企業 AI 架構不是找到一個包辦全部的 Framework，而是知道每一種能力該放在哪個位置**；
**架構不是越進階越好，而是剛好能承受現在問題的複雜度最好。**

## B（適用場景）

**適用：**
- 需要一套可攜帶到不同企業 AI 場景（客服知識助理、Data Analysis Agent、會議工作流）的通用骨架，
  而非只複製某一個 Demo 專案
- 正式驗收企業 AI 產品前，想知道除了「答案對不對」還要檢查哪些面向
- 想評估「現在真的需要再加一個 Agent 嗎」，避免技術清單式的過度設計

**不適用 / 注意：**
- ⚠️ 八問框架不是要求每個 Prototype 一次做到全部；第一版只需要 Read-only RAG，就未必需要
  Human Approval
- 本篇是總結收斂層，具體技術細節（Timeout/Retry 設計、Node 拆分、權限三層）分別在 Day 25–28
  已展開，本篇不重複

## R（六階段模型）

| 階段 | 主要解決的問題 |
| --- | --- |
| Chat | 模型能不能理解問題並生成內容？ |
| RAG | 模型怎麼取得企業內部文件與知識？ |
| Tool Use | 怎麼取得精確、結構化或即時資料？ |
| Agent | 有多個能力時，下一步應該做什麼？ |
| Workflow | 怎麼控制、驗證、觀察與恢復 Agent？ |
| Product | 怎麼讓真實使用者安全、可靠地長期使用？ |

### 1. 驗收標準從「回答對不對」升級為八面向檢查

| 面向 | 要確認什麼 |
| --- | --- |
| Function | Chat、RAG、Data Tool、跨來源與多輪追問是否正常 |
| Routing | 問題是否使用正確 Tool 與順序 |
| Evidence | 回答是否有足夠來源與證據 |
| Reliability | Timeout、Retry、Fallback 與 Partial Success 是否正確 |
| Security | Secret、Permission、Data Scope 是否符合規則 |
| Workflow | State、Node、Edge 與 Failure Path 是否按預期 |
| UX | 使用者是否知道目前進度、錯誤與完成狀態 |
| Deployment | Frontend、Backend、CORS、Environment Variable 是否正常 |

一個回答的數字看起來正確，但 Agent 其實沒查 Source of Truth 只是剛好猜對——**不應算成功**；
建立 Task 內容完全正確但跳過 Human Approval——**同樣算失敗**。

### 2. 正式驗收要故意測 Failure Path，不能只測 Happy Path

故意把測試環境 API Key 改錯、取消 Google Sheet 的 Service Account 共享權限、餵無法解析的文件、
讓 Backend URL 指向錯誤位置——目的不是把產品弄壞，而是**確認真實世界把產品弄壞時，系統已經
知道會怎麼反應**。

### 3. Enterprise AI Blueprint 八問框架

| 問題 | 要釐清的事情 |
| --- | --- |
| 1. Job | 使用者真正想完成的工作是什麼？ |
| 2. Workflow | 現在怎麼完成？哪一步最耗時或最容易錯？ |
| 3. Knowledge | 需要哪些文件、結構化資料、即時系統與外部來源？ |
| 4. Intelligence | 哪些工作真的需要 LLM 理解、摘要或判斷？ |
| 5. Deterministic Logic | 哪些計算、驗證與流程必須由程式保證？ |
| 6. Risk | 哪些資料或 Action 需要權限、Approval 或 Audit？ |
| 7. Evaluation | 如何知道答案、Routing 與 Task Completion 都正確？ |
| 8. Recovery | Tool 或模型失敗時，如何 Retry、Fallback、Resume 或轉人工？ |

用途不是再造一套複雜方法論，而是幫忙確認一個企業 AI Use Case 從需求到正式使用是否還有缺口。
新增 Agent 前先問：它到底解決哪一格？若現有 Coordinator 已能完成 Job，新 Agent 沒有改善
Permission、Context Isolation、Evaluation 或 Workflow，就只是增加複雜度。

一句話模板：**「我要為＿＿提供一套企業 AI，使用＿＿取得可驗證資訊，依照＿＿完成工作；
遇到＿＿時追問或停止，涉及＿＿時必須取得人工批准。」**

### 4. 把 30 天的測試變成 Regression Test

按能力分類保存測試案例（`knowledge_cases.csv`／`cross_source_cases.csv`／
`agent_boundary_cases.csv`／`workflow_path_cases.csv`／`production_checklist.md`），
**不要每次改版就重新發明一批測試題**——固定測試才能看見換模型後 Routing Accuracy 提高，
但原本會拒答的 RAG 問題突然開始亂猜的 Trade-off。

不同修改對應不同 Evaluation：改 Chunking/Embedding/Retrieval → 重跑 Knowledge Cases；
新增 Data Source → 重跑 Cross-source Cases；改 Tool Description/Routing Prompt →
重跑 Agent Boundary Test；改 LangGraph State/Node/Edge/Retry/Approval → 重跑 Workflow Test；
改 Environment Variable/Permission/Render/Vercel → 重跑 Production Smoke Test。

### 5. 交接驗收標準：第二位管理者能否獨立接手

留下三類資訊（System Architecture／Asset-Credential Inventory／Deployment-Recovery Guide），
但真正判準不是「文件寫了嗎」，而是讓**第二位管理者在無提示下獨立完成**：找到 GitHub/Render/
Vercel/Cloud Project、依文件重新部署、找到 Environment Variable 與 Secret 存放位置、
完成一次 Credential Rotation、移除原開發者權限後系統仍正常運作。

### 6. 版本可回復是可靠性的一部分

GitHub 是正式程式碼來源，每次重要修改要有清楚 Commit；出問題用 Fix/Revert Commit 回到穩定版，
不要在線上同時改五個設定賭一次成功——**先定位是哪一層失敗，再修改那一層**。

## C（關鍵行動）

- [ ] 把手上 AI 專案的驗收清單，從「回答對不對」擴充成八面向表格（Function/Routing/Evidence/
      Reliability/Security/Workflow/UX/Deployment）
- [ ] 用八問框架重新檢視是否真的需要新增下一個 Agent，而非直接動手拆
- [ ] 把 Demo 階段建立的測試題目整理成按能力分類的 Regression Test，而非跑完就丟
- [ ] 找第二位管理者實測交接九件事，補齊過不了的那一項
- [ ] 下一步從 Business Problem 出發，而非 Technology Checklist（不要為了拆 Multi-Agent 而拆）

## 原始摘要

Data Machi 系列 Day 30（完結篇）。總結全系列六階段演進（Chat → RAG → Tool Use → Agent →
Workflow → Product），核心主張是企業 AI 的價值不在模型回答像不像人，而在能否把資料、工具、
判斷與流程串成可靠工作流。提出 Enterprise AI Blueprint 八問框架（Job/Workflow/Knowledge/
Intelligence/Deterministic Logic/Risk/Evaluation/Recovery）作為新增 Agent 或 Tool 前的判準；
主張正式驗收要同時測 Function/Routing/Evidence/Reliability/Security/Workflow/UX/Deployment
八面向，並要故意測 Failure Path 而非只測 Happy Path；建議把 30 天測試案例轉為可重跑的
Regression Test；交接驗收標準是第二位管理者能否在無提示下獨立完成部署與 Rotation；
最後強調下一步不是繼續加 Agent，而是回到 Business Problem 找真正痛點。

## 相關連結

- [[langgraph-coordinator-refactor-seven-nodes]] — 同系列 Day 25：本篇八問中的 Workflow／
  Deterministic Logic 對應它拆出的七個 Node 與 Edge 規則
- [[agent-reliability-timeout-retry-fallback-verification]] — 同系列 Day 26：本篇 Reliability
  驗收面向的完整機制展開
- [[agent-ux-status-transparency-maturity]] — 同系列 Day 27：本篇 UX 驗收面向的五級成熟度
- [[enterprise-ai-permission-three-layers-governance]] — 同系列 Day 28：本篇 Security/Risk
  驗收面向的三層權限框架
- [[data-machi-deploy-render-vercel-cors-secrets]] — 同系列 Day 29：本篇 Deployment 驗收面向
  的實作篇，**系列完結**——Day 25–30 六篇構成完整的 Workflow→Product 路徑
- [[enterprise-ai-five-stage-evolution]] — 另一套企業 AI 五階段演進模型，可對照本篇六階段模型
- [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] — 企業 AI 四層架構，與本篇八問框架
  可交叉檢核
- [[testing-your-ai-workflow-config-ci]] — Regression Test／CI 落地的另一種實作參考
