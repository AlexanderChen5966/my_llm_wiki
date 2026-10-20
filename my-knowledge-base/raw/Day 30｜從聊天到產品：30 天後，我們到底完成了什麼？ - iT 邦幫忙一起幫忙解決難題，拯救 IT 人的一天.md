---
title: "Day 30｜從聊天到產品：30 天後，我們到底完成了什麼？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10409206"
author:
  - "[[iThome]]"
published:
created: 2026-09-11
description: "30 天走到最後，如果只回頭看名詞，這個系列似乎學了很多東西：RAG、Tool Use、LangChain、ReAct、Coordinator、Memory、LangGraph、Retry、CORS、..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 31 篇

## Day 30｜從聊天到產品：30 天後，我們到底完成了什麼？

- 分享至

30 天走到最後，如果只回頭看名詞，這個系列似乎學了很多東西：RAG、Tool Use、LangChain、ReAct、Coordinator、Memory、LangGraph、Retry、CORS、Service Account。這些名詞看起來分散在不同技術領域，但如果把它們全部拿掉，其實這 30 天只是在持續解決同一個問題： **如何把一個只會回答問題的模型，逐步改造成能可靠完成企業知識工作的產品。**

這也是 Data Machi 整個系列最重要的主線。我們不是從一開始就決定「我要做一個 Agent」，再把所有熱門框架塞進系統，而是從最基本的工作問題出發。模型不知道企業文件，所以加入 RAG；模型不適合自己猜精確數字，所以加入 Tool；資料來源變多之後，需要有人決定下一步，所以出現 Coordinator；流程開始有 Retry、Verification 與 Human-in-the-loop，又需要 LangGraph 把原本藏在 Agent Loop 裡的控制邏輯整理出來。最後，當系統真的準備交給其他人使用，問題自然延伸到可靠性、使用者體驗、權限、安全、部署與交接。

因此走到 Day 30，我們真正完成的不是一個「功能很多的聊天機器人」，而是一套從工作問題一路走到產品的企業 AI 設計方法。

## 從 Chat 開始，但不要停在 Chat

最早的版本非常簡單：使用者輸入問題，LLM 根據模型既有知識產生回答。這就是大部分人第一次接觸生成式 AI 時最熟悉的 Chat 體驗，它很適合摘要、改寫、發想與一般知識問答，但只要問題碰到企業內部資訊，限制就會很快出現。

模型不知道公司最新政策，不知道剛更新的 Google Sheets，也不知道某個 Project 現在做到哪裡。如果要求它直接回答，它只能依靠 Prompt 裡已有的 Context，或者根據訓練時學到的知識推測。

所以我們開始往下一層走。

加入 **RAG（Retrieval-Augmented Generation）** 後，模型回答之前可以先搜尋企業文件。這讓 AI 從「只靠自己知道的內容回答」，變成「先找資料，再根據資料回答」，同時開始保存 Filename、Page、Section 等來源資訊，讓回答可以被追溯。

但文件搜尋仍然不能解決所有問題。當使用者問「今年有多少需求？哪個市場最高？成長率是多少？」這類需要即時與精確計算的問題時，我們又加入 **Tool Use** ，把 Google Sheets、Database、API 或 Python 變成模型可以使用的外部能力。LLM 負責理解問題，精確資料則由真正的 Source of Truth 與程式取得。

當 Tool 數量開始增加，新的問題又出現了：到底該使用 Sheets、Document 還是 Project Tool？如果一個問題同時需要三個來源，順序又該怎麼安排？這時我們加入 **Coordinator 與 Agent** ，讓系統開始根據使用者目標與前一步結果，決定下一步應該做什麼。

但是 Agent 越自由，流程也越容易變成黑箱。因此後來又加入 **Workflow 與 LangGraph** ，用 State 保存目前資訊、Node 拆分責任、Edge 控制下一步，讓 Verification、Retry、Clarification 與 Human Approval 不再只是寫在 Prompt 裡的提醒，而成為系統真正可以控制的執行路徑。

最後，我們又處理 Timeout、Fallback、Agent UX、Secret、Permission、Render、Vercel 與 CORS。這時關心的已經不只是「AI 能不能回答」，而是「其他人能不能長期、可靠又安全地使用」。

這就是從 Chat 走到 Product 的完整過程。

## 六個階段，其實是在解決六種不同問題

回頭看這 30 天，可以把整個架構濃縮成六個階段：

| 階段 | 主要解決的問題 |
| --- | --- |
| Chat | 模型能不能理解問題並生成內容？ |
| RAG | 模型怎麼取得企業內部文件與知識？ |
| Tool Use | 怎麼取得精確、結構化或即時資料？ |
| Agent | 有多個能力時，下一步應該做什麼？ |
| Workflow | 怎麼控制、驗證、觀察與恢復 Agent？ |
| Product | 怎麼讓真實使用者安全、可靠地長期使用？ |

這六層並不是新的技術不斷取代舊技術。走到 Product 之後，最底層仍然有 Chat，文件問題仍然需要 RAG，數字仍然交給 Tool，Coordinator 仍然負責部分動態判斷。

成熟的企業 AI 架構，通常不是找到一個可以包辦全部事情的 Framework，而是 **知道每一種能力應該放在哪個位置。**

這也是為什麼前面一直強調：不是所有問題都需要 Agent，不是所有 Agent 都需要 LangGraph，也不是 Tool 越多、Agent 越多就越成熟。架構應該隨著工作問題與風險逐步成長。

## 從「能不能回答」走向「任務到底有沒有完成」

一開始測試 AI 時，我們最容易問的是：「它回答得對不對？」

但經過這 30 天之後，驗收的問題已經比這複雜很多。

例如一個回答中的數字看起來正確，但 Agent 其實沒有查 Source of Truth，而是剛好猜對，這不應該算成功。如果結果正確，但 Router 呼叫了五個完全不必要的 Tool，成本與延遲都很高，也代表 Workflow 還沒有真正穩定。

同樣地，如果系統正確找出數據，卻把「相關性」寫成「原因」，Verification 仍然應該判斷這個答案不能直接交付。如果建立 Task 的內容完全正確，但跳過了 Human Approval，對企業 Workflow 來說同樣是失敗。

因此正式驗收應該開始同時看功能、流程、可靠性、安全與使用者體驗。

| 面向 | 需要確認什麼 |
| --- | --- |
| Function | Chat、RAG、Data Tool、跨來源與多輪追問是否正常 |
| Routing | 問題是否使用正確 Tool 與順序 |
| Evidence | 回答是否有足夠來源與證據 |
| Reliability | Timeout、Retry、Fallback 與 Partial Success 是否正確 |
| Security | Secret、Permission、Data Scope 是否符合規則 |
| Workflow | State、Node、Edge 與 Failure Path 是否按照預期 |
| UX | 使用者是否知道目前進度、錯誤與完成狀態 |
| Deployment | Frontend、Backend、CORS、Environment Variable 是否正常 |

這也代表 AI Product 的 Evaluation 不能只依賴一個 Model Benchmark。真正會影響企業使用者信任的，往往是「我叫它查最新資料，它到底有沒有真的查？」「資料不足時，它會不會亂猜？」「一部分來源失敗時，已完成的結果還在不在？」

## 正式驗收不要只測 Happy Path

Demo 最容易展示的是所有事情都正常的情境：Gemini 正常、Google Sheets 正常、PDF 可以解析、Token 有效，最後 Agent 很順利地產生答案。

但正式產品真正需要測試的，是事情不正常的時候。

例如可以故意把測試環境的 Gemini API Key 改錯，確認系統收到 Authentication Error 後能結束 Loading，並提供明確錯誤訊息；暫時取消 Google Sheet 對 Service Account 的共享權限，確認系統回報 Permission Denied，而不是假裝沒有資料；提供無法解析的文件，檢查 RAG 是否能識別失敗；或者讓 Backend URL 指向錯誤位置，確認 Frontend 能結束等待，而不是永遠停在 Spinner。

這類 Failure Test 的目的不是故意把產品弄壞，而是確認：

**當真實世界把產品弄壞時，我們已經知道系統會怎麼反應。**

這正是 Day 26 Reliability 最重要的觀念。正式產品不可能保證所有外部服務永遠成功，但可以保證每一類已知失敗都有可預期的處理方式。

## 把驗收結果真正留下來

驗收也不應該只是開發者自己測過幾題，心裡覺得「應該沒問題」。

可以使用 GitHub Issue、Google Sheets、Notion、Linear 或其他團隊工具留下正式測試紀錄。最基本的欄位只需要包括測試項目、預期結果、實際結果、測試日期與狀態。

| ID | 類型 | 測試情境 | 預期結果 | 實際結果 | 通過 |
| --- | --- | --- | --- | --- | --- |
| PROD-01 | Function | 查詢測試 Sheet | 回傳正確數值 |  |  |
| PROD-02 | RAG | 查詢政策定義 | 找到正確來源 |  |  |
| PROD-03 | Failure | 使用失效 Token | 顯示權限錯誤 |  |  |
| PROD-04 | Workflow | 建立高風險 Action | 停在 Approval |  |  |
| PROD-05 | UX | Tool Timeout | Loading 結束並顯示下一步 |  |  |

這份紀錄之後不只是上線 Checklist，也會成為未來每一次修改後重新驗證系統的基礎。

## 交接：產品不能只存在原開發者的腦袋裡

除了功能驗收，企業產品還有另一件常被忽略的事情： **如果原本建立系統的人不在，產品還能不能繼續運作？**

Data Machi 目前可能牽涉 GitHub Repository、Render Backend、Vercel Frontend、Google Cloud Project、Service Account、Gemini API、Trello、Confluence 與其他外部服務。如果這些資產全部只有一位開發者知道在哪裡、怎麼登入、怎麼部署，那整套系統本身就是一個治理風險。

至少應該留下三類資訊：第一類是 System Architecture，讓下一個維護者知道 Frontend、Backend、Tool 與資料來源怎麼連接；第二類是 Asset / Credential Inventory，記錄有哪些 Project、Environment、Credential 與 Owner，但不保存明文 Secret；第三類是 Deployment / Recovery Guide，說明正式環境如何重新部署、如何更換 Credential，以及出問題時如何回到穩定版本。

不過真正判斷交接完成與否，不應該只看「文件寫了嗎」，而是看另一位管理者能不能真的接手。

可以請第二位管理者在沒有原開發者口頭提示的情況下，找到 GitHub Organization、Render Workspace、Vercel Team 與 Google Cloud Project，依文件重新部署 Frontend 與 Backend，找到 Environment Variable 的名稱與 Secret 存放位置，建立一組測試 Credential，再完成一次 Credential Rotation。

最後甚至可以在測試環境中模擬移除原開發者權限，確認系統仍然可以正常運作。

如果其中任何一步只有原本建立系統的人能做到，代表交接仍然沒有完成。

## 版本可以回復，也是產品可靠性的一部分

正式產品一定會持續修改。尤其現在 AI Coding Agent 可以快速幫忙修改 Prompt、Frontend、Backend、Workflow 與文件，開發速度可能比以前快很多。

但修改速度變快，不代表版本管理可以變得比較隨意，反而更需要清楚的 Commit 與 Recovery Path。

因為 GitHub 是正式程式碼來源，所以每一個重要修改最好都有清楚的 Commit。如果某次 Production Deployment 出問題，可以建立 Fix Commit 或 Revert Commit 回到已知穩定版本，而不是在線上不停修改幾個設定，希望某一次剛好成功。

如果問題發生在部署平台，也應該先看 Log，確認到底是 Build、Environment Variable、Permission 還是 External API 的問題，不要一次改五個地方再重新 Deploy。

這和前面整個系列的 Debugging 原則其實完全一致： **先定位是哪一層失敗，再修改那一層。**

Vibe Coding 讓開發變快，但真正讓快速開發可以長期維持的，是 Version Control、Testing 與 Rollback。

## 30 天後，下一步不是繼續增加 Agent

完成這 30 天後，很容易產生一種衝動：既然已經做了 Coordinator，是不是下一步應該做 Multi-Agent？既然用了 LangGraph，是不是要再加入更多 Node？既然可以查 Sheets，是不是把所有公司系統都接進來？

但如果回到系列一開始 Day 02 的 FUDAT，下一步真正應該問的仍然是： **目前工作流程裡，哪一個問題最值得被改善？**

哪一個步驟最常讓使用者人工切換系統？哪一種資料最難找到？哪一種分析最容易因為定義不同而反覆確認？哪一種工作明明規則固定，卻每天還需要人工重做？

只有當新的 Tool、Workflow 或 Agent 真正解決這些工作痛點時，它才值得加入。

如果 Multi-Agent 只是讓 Architecture Diagram 看起來更厲害，但沒有改善 Task Completion、Accuracy、Latency 或 User Experience，就沒有必要急著拆。

因此 Data Machi 的下一步依然應該從 Business Problem 出發，而不是從 Technology Checklist 出發。

## 30 天後，你真正完成的是什麼？

走到這裡，可以重新回答文章標題的問題：30 天後，我們到底完成了什麼？

我們並不是完成了一個「可以和 AI 聊天的網站」。

而是開始建立一個企業 AI 知識 Workflow 的完整骨架。它知道企業文件應該去哪裡找，結構化數字應該交給哪個 Tool 計算；當問題需要多個來源時，知道哪些可以 Parallel、哪些必須 Sequential；它開始能理解多輪追問，知道哪些 Memory 可以沿用、哪些最新資料必須重新取得。

當證據不足時，它知道應該 Clarify、Re-query、Verification，或明確告訴使用者現在沒有足夠資料；當流程開始變複雜，又能用 State、Node、Edge 與 Failure Path 把 Agent 自主性限制在明確邊界中。

服務 Timeout 時，它知道什麼情況可以 Retry、什麼時候應該 Fallback；前端不再只有 Spinner，而能把真實 Workflow State 轉換成使用者可以理解的進度。Secret 留在 Backend，Service Account 遵守 Least Privilege，正式資產也不再只綁在某一位開發者帳號。

最後，系統可以真正部署出去，讓另一個人從 Browser 使用。

這些能力放在一起，才逐漸構成「Product」。

## 這套架構並不只屬於 Data Machi

更重要的是，這個骨架不應該只能用來複製 Data Machi。

如果把場景換成客服知識助理，仍然要思考哪些問題需要 RAG、哪些資料需要即時 CRM Tool；如果換成 Data Analysis Agent，仍然要思考精確數字、Business Context、Verification 與 Source；如果換成會議工作流，還是會遇到 Tool、Workflow、Human Approval 與 Action Item Tracking。

場景可能完全不同，但核心問題其實高度相似：

**資料從哪裡來？哪些資訊可以相信？誰負責做計算？什麼時候需要 Agent 判斷？哪些規則不能讓模型自由決定？失敗時怎麼恢復？最後怎麼知道工作真的完成？**

因此這個系列真正想留下的不是 Data Machi Source Code，而是一套可以帶到其他企業 AI 場景的思考方式。

## 階段實作六｜完成你的 Enterprise AI Blueprint

前面每五天左右，我們都保存了一份階段成果。到了最後一天，可以把這些成果真正放在一起。

前面的內容可能包括問題定義、Knowledge / RAG 驗證案例、跨來源流程、Agent Decision Boundary、State / Node / Edge Workflow，以及 Reliability、Permission 與 Deployment 的設計。

現在不需要再增加另一個新 Framework，而是把它們整理成一份 **Enterprise AI Blueprint** 。

這份 Blueprint 可以先回答八個問題：

| 問題 | 要釐清的事情 |
| --- | --- |
| **1\. Job** | 使用者真正想完成的工作是什麼？ |
| **2\. Workflow** | 現在怎麼完成？哪一步最耗時或最容易錯？ |
| **3\. Knowledge** | 需要哪些文件、結構化資料、即時系統與外部來源？ |
| **4\. Intelligence** | 哪些工作真的需要 LLM 理解、摘要或判斷？ |
| **5\. Deterministic Logic** | 哪些計算、驗證與流程必須由程式保證？ |
| **6\. Risk** | 哪些資料或 Action 需要權限、Approval 或 Audit？ |
| **7\. Evaluation** | 如何知道答案、Routing 與 Task Completion 都正確？ |
| **8\. Recovery** | Tool 或模型失敗時，如何 Retry、Fallback、Resume 或轉人工？ |

這八個問題的目的，不是再增加一套複雜方法論，而是幫助我們確認一個企業 AI Use Case 從需求到正式使用，是否還有重要缺口。

例如想新增一個新的 Agent 時，可以回來問：它到底解決哪一格？如果現有 Coordinator 已經能完成 Job，新的 Agent 沒有改善 Permission、Context Isolation、Evaluation 或 Workflow，就可能只是增加複雜度。

相反地，如果目前跨市場分析每次都需要人工從三個系統取得資料，而新的 Tool 能直接改善 Workflow，那它的價值就非常清楚。

最後，可以試著用一句話定義自己的第一版：

**我要為＿＿提供一套企業 AI，使用＿＿取得可驗證資訊，依照＿＿完成工作；遇到＿＿時追問或停止，涉及＿＿時必須取得人工批准。**

這句話甚至比「我要做一個 Multi-Agent 平台」更重要，因為它真正描述了使用者、資料、工作與風險。

**階段成果：** 最終需要保存的不是 Data Machi 的複製品，而是屬於自己的 **Enterprise AI Blueprint** ，其中包含 Problem、Knowledge、Tools、Agent Boundary、Workflow、Risk、Evaluation 與 MVP Scope。這份 Blueprint 應該能讓下一個 Prototype、Vibe Coding 或正式 Engineering 工作有明確的起點。

## 把 30 天的測試變成 Regression Test

最後還有一項非常值得保存的資產：前面這 30 天建立的測試案例。

Day 10 已經建立了一批 RAG 問題，用來確認系統能不能找到正確文件與證據；Day 15 開始測跨來源流程，檢查 Tool 之間的 Data Dependency；Day 20 定義 Agent Decision Boundary，確認 Coordinator 什麼時候應該繼續、停止、追問或使用其他 Tool；Day 25 又進一步驗證 Graph 的 State、Node、Edge 與 Failure Path；如果實際完成部署，Day 29 還有 Production Smoke Test。

這些測試不應該在文章實作結束後就被丟掉。

它們可以變成 **Regression Test（回歸測試）** ，也就是每次修改系統時，都重新用相同案例檢查功能有沒有退步。

可以依照能力保存：

```markdown
/evaluation
  ├─ knowledge_cases.csv
  ├─ cross_source_cases.csv
  ├─ agent_boundary_cases.csv
  ├─ workflow_path_cases.csv
  └─ production_checklist.md
```

如果不想一開始建立完整 Evaluation Pipeline，也可以全部放在一張 Google Sheet。最重要的是固定保留測試題目與預期行為。

| ID | 類型 | 測試情境 | 預期結果 | 實際結果 | 通過 |
| --- | --- | --- | --- | --- | --- |
| KNOW-01 | Knowledge | 固定文件問題 | 命中正確來源並根據證據回答 |  |  |
| SOURCE-01 | Cross-source | 跨來源問題 | 使用正確來源與順序 |  |  |
| AGENT-01 | Agent Boundary | 資訊不足問題 | 正確 Clarify / Stop |  |  |
| FLOW-01 | Workflow | Tool Failure | 通過正確 Failure Path |  |  |
| PROD-01 | Production | 正式環境完整流程 | 可以正常完成指定任務 |  |  |

真正重要的是： **不要每一次改版就重新發明一批測試題。**

固定測試才能比較修改前後的差異。如果更換模型後 Routing Accuracy 變高，但原本會拒答的 RAG 問題突然開始亂猜，固定測試才會讓這個 Trade-off 被看見。

## 什麼時候應該重新執行測試？

不同修改可以對應不同 Evaluation。

如果修改 Chunking、Embedding 或 Retrieval Strategy，就重新執行 Knowledge / RAG Cases；新增 Data Source 或改變 Tool Execution Order，就重跑 Cross-source Cases；調整 Tool Description、Routing Prompt 或 Stop Conditions，則重跑 Agent Boundary Test。

如果修改 LangGraph State、Node、Edge、Retry 或 Approval Path，就需要重新驗證 Workflow Test；Environment Variable、Permission、Render 或 Vercel 設定改變後，則重新跑 Production Smoke Test。

如果更換核心模型、重新設計 Coordinator，或進行比較大的 Architecture Refactor，最好把所有關鍵案例重新執行一次。

這樣 Evaluation 就不再只是某一天為了 Demo 準備的測試，而會逐漸形成整套企業 AI 的品質紀錄。

## 最後，確認這套架構真的已經收斂

完成 30 天後，可以用最後一份 Checklist 重新檢查自己的 Enterprise AI Blueprint：

- \[ \] 已定義明確使用者、工作成果與目前 Workflow
- \[ \] 已選出第一版真正要解決的核心問題
- \[ \] 已列出文件、結構化資料、即時系統與 Source of Truth
- \[ \] 已知道哪些問題應該使用 RAG、Tool、Workflow 或 Agent
- \[ \] 已定義跨來源問題的 Sequential、Parallel 與 Data Dependency
- \[ \] 已定義 Agent 可使用的 Tool、最大步數與 Stop Condition
- \[ \] 已定義何時 Answer、Clarify、Re-query、Partial Answer 或停止
- \[ \] 已將 Workflow 整理成 State、Node、Edge 與 Failure Path
- \[ \] 已標記哪些動態資料不能直接沿用舊 Memory
- \[ \] 已定義 Timeout、Retry、Fallback 與 Partial Success
- \[ \] 已確認 Secret、Permission、Data Scope 與 Human Approval
- \[ \] 已建立固定 Evaluation Cases 與驗收標準
- \[ \] 已把原本很大的 AI 構想縮成可以真正驗證的 MVP

這份清單並不代表每一個 Prototype 都必須一次做到全部。如果第一版只需要 Read-only RAG，也許根本不需要 Human Approval；如果只是固定 ETL Workflow，也未必需要 Agent。真正重要的是知道哪些能力是現在必要的，哪些可以等問題真的出現後再加入。

這也回到這個系列一路反覆出現的原則：

**架構不是越進階越好，而是剛好能承受現在問題的複雜度最好。**

## 從「AI 能做什麼」回到「工作應該怎麼被重新設計」

30 天之前，我們可能會從模型能力開始想：

「AI 能不能看 PDF？」

「AI 能不能查 Google Sheets？」

「AI 能不能自己選 Tool？」

「AI 能不能做 Agent？」

走到最後，問題應該慢慢變成：

「這項工作真正的瓶頸在哪裡？」

「哪一段應該交給 AI？」

「哪一段應該交給程式？」

「哪一段一定要由人決定？」

「最後怎麼知道這項工作真的完成？」

這其實才是企業 AI 真正開始變得有價值的地方。

因為企業最終需要的從來不是一個「看起來很聰明的模型」，而是一段更好的工作方式。

---

> **今天的重點：**  
> 這 30 天的主線不是複製 Data Machi，也不是學完一份 AI Framework 清單，而是理解一個企業 AI 為什麼會從 **Chat → RAG → Tool Use → Agent → Workflow → Product** 一步一步演進。每一次增加新的技術，都是因為前一層遇到了新的工作問題。最後真正值得留下來的，是一份能清楚描述 **Problem、Knowledge、Tools、Agent Boundary、Workflow、Risk、Evaluation 與 MVP Scope** 的 Enterprise AI Blueprint，以及一套可以持續重跑的 Regression Test。

如果這 30 天只能留下一個觀念，我希望是： **企業 AI 的價值不在於模型回答得多像人，而在於能不能把資料、工具、判斷與流程串成一段可靠的工作。**

Data Machi 的 30 天在這裡告一段落。但真正的下一步，不是再加一個 Agent，而是帶著這份 Blueprint 回到自己的工作場景，找出那一段最值得被重新設計的知識工作。

謝謝你閱讀到這裡，期待你可以打出屬於自己的數據夥伴！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10408189)

[

Day 29｜實作：把 Data Machi 部署到 Render 與 Vercel

](https://ithelp.ithome.com.tw/articles/10408189)

系列文

[Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI](https://ithelp.ithome.com.tw/users/20169646/ironman/9207) 共 31 篇

目錄

1. 27
	[Day 26｜AI 一定會失敗：逾時（Timeout）、重試（Retry）、備援（Fallback）怎麼設計？](https://ithelp.ithome.com.tw/articles/10408173)
2. 28
	[Day 27｜代理（Agent）在工作時，使用者為什麼不能只看到轉圈圈](https://ithelp.ithome.com.tw/articles/10408179)
3. 29
	[Day 28｜API 金鑰（API Key）只是開始：企業 AI 的權限、安全與治理該怎麼做](https://ithelp.ithome.com.tw/articles/10408186)
4. 30
	[Day 29｜實作：把 Data Machi 部署到 Render 與 Vercel](https://ithelp.ithome.com.tw/articles/10408189)
5. 31
	[Day 30｜從聊天到產品：30 天後，我們到底完成了什麼？](https://ithelp.ithome.com.tw/articles/10409206)
[完整目錄](https://ithelp.ithome.com.tw/users/20169646/ironman/9207)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/a3e9a897-de84-4879-9068-553e4db5f75f_small.jpg)

[Policy priorities and initiatives for securing Industrial Automation and Control Systems (IACS) in the Netherlands](https://itplus.ithome.com.tw/vod-page/161?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/bfb909cb-c713-44ce-bf7b-ef2976001bea_small.jpg)

[大規模網路掃描：以在全球上找出有漏洞的 Data Distribution Service (DDS) 為例](https://itplus.ithome.com.tw/vod-page/339?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/10/5a8abfe5-156d-4f08-a876-62c23d668038_small.png)

[Flowmon - 結合MITRE 與 Netflow 達成CP值最高的資安事件應變手段](https://itplus.ithome.com.tw/vod-page/489?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/e0bb98b2-5844-42a6-bdb1-313b41f156ab_small.png)

[運動x程式x工程師](https://itplus.ithome.com.tw/vod-page/1063?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

23 分