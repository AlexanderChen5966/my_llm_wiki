---
title: "Claude Cookbook 是什麼？Anthropic 官方 Agent 工程範例庫與必看資源"
source: "https://www.aiposthub.com/claude-cookbook-agent-engineering-guide/"
author:
  - "[[Philo]]"
published: 2026-07-27
created: 2026-07-28
description: "Claude Cookbook 不只是 Prompt 教學，而是 Anthropic 官方 Agent 工程範例庫。本文整理多 Agent、記憶、工具、驗證與部署等必看資源。"
tags:
  - "clippings"
status: processed
---
如果你正在做 AI Agent，卻還停留在「該怎麼寫 Prompt」的階段，Anthropic 的 [Claude Cookbook](https://platform.claude.com/cookbook/?ref=aiposthub.com) 很值得收藏。

它不是一本從第一頁讀到最後一頁的教科書，而是一套可以依問題查找的實作範例庫。裡面不只提供 Prompt，還有可執行程式碼、Agent 協作模式、記憶與上下文管理、工具呼叫、品質驗證、部署方式，以及實際的產品案例。

我的結論很直接：如果你是開發者、AI 產品經理，或正在規劃 Agent 工作流，Claude Cookbook 值得放進常用書籤。但最有效的使用方式不是全部看完，而是先找出目前產品卡住的工程問題，再挑一篇範例實際跑過。

## Claude Cookbook 重點整理

| 項目 | 說明 |
| --- | --- |
| 資源定位 | Anthropic 官方 Claude 實作指南與程式碼範例庫 |
| 主要內容 | Agent Patterns、Tools、Evals、Skills、Integrations、Multimodal 與 Responses |
| 適合對象 | AI 開發者、AI 產品經理、技術型創作者 |
| 程式語言 | 以 Python 範例為主，架構概念可套用至其他語言 |
| 使用成本 | 網站與 GitHub 可免費瀏覽；執行 Claude API（讓應用程式透過程式呼叫 Claude 的介面）範例可能產生用量費用 |
| 建議用法 | 依產品問題挑選案例，先跑最小範例，再加入自己的資料、工具與驗收條件 |

Claude Cookbook 最適合當成問題導向的 Agent 工程索引，而不是需要依序讀完的線上課程。先確認目前卡在記憶、工具、協作、驗證或部署，再進入對應範例，會比漫無目的收藏更有效率。

## Claude Cookbook 是什麼？

Claude Cookbook 是 Anthropic 提供的官方實作資源，首頁把內容定位為「有效使用 Claude 的實用指南與範例」。你可以直接在網站搜尋，也能依 Agent Patterns、Tools、Evals、Skills、Integrations、Multimodal 與 Responses 等分類篩選。

這裡的 Cookbook 可以理解成「工程食譜」：每篇先設定一個具體問題，再提供架構說明、程式碼與執行方式。讀者不必先把所有 Claude 文件讀完，也能從一個可運作的案例開始拆解。

Claude API 是讓應用程式透過程式呼叫 Claude 模型的介面；SDK 則是 Anthropic 為 Python、TypeScript 等程式語言提供的開發工具包。Cookbook 裡的多數內容會直接使用這些工具，因此它比一般 Prompt 合集更偏向產品實作。

官方也在 [GitHub 公開 Claude Cookbooks](https://github.com/anthropics/claude-cookbooks?ref=aiposthub.com) ，主要範例以 Python 為主，並採用 MIT License。想研究完整檔案、複製 Notebook，或追蹤更新，都可以從 GitHub 版本開始。

## 為什麼它不只是 Prompt 教學？

一個能回答問題的模型，和一個能穩定完成工作的 Agent，中間還隔著不少工程細節。

AI Agent 是能根據目標決定下一步、呼叫工具並持續處理任務的 AI 系統。模型負責判斷，但真正影響產品穩定度的，通常是模型外圍的執行環境，也就是常說的 Agent harness。它包含工具權限、上下文、狀態、錯誤處理與驗收機制。

Claude Cookbook 最近的內容重心，正好涵蓋這些不容易只靠 Prompt 解決的問題：

- 多個 Agent 要如何分工、傳遞訊息與結束任務？
- 長任務超出上下文長度時，哪些資料該留下？
- 工具很多時，如何降低模型選錯工具與 token 浪費？
- Agent 說「完成了」之後，誰負責驗證？
- 原型可以運作後，要怎麼部署成持續服務？

這也是它真正有價值的地方。它不是告訴你「問 Claude 什麼」，而是示範「如何替 Claude 建立可靠的工作環境」。

## Claude Cookbook 有哪些內容值得先看？

截至 2026 年 7 月 27 日，Claude Cookbook 已累積不少跨領域案例。以下不是完整清單，而是我認為對 Agent 產品最有參考價值的六類內容。

### 1\. 多 Agent 協作：不是拆越多 Agent 越好

[Async multi-agent orchestration](https://platform.claude.com/cookbook/patterns-agents-async-multi-agent-orchestration?ref=aiposthub.com) 展示兩種常見做法：固定數量的 Agent 團隊，以及任務進行中動態建立的 subagents。

Subagent 是由主要 Agent 指派特定子任務的獨立執行者。固定團隊適合角色與流程明確的工作，例如研究員、資料分析員與審稿員；動態 subagents 則適合事前不知道會拆出多少工作的任務。

真正值得看的不是「同時開很多模型」，而是 Agent 之間如何傳遞訊息、回報狀態與結束生命週期。多 Agent 會增加成本、延遲與除錯難度，如果一個 Agent 配合清楚工具就能完成，就沒有必要為了架構看起來先進而強行拆分。

![Claude Cookbook 多 Agent 協作架構圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/size/w2400/2026/07/02-multi-agent-architecture.png)

Coordinator 拆分任務，多個 Search workers 分別查詢，再將結果交回整合。圖片來源：Anthropic Claude Cookbooks，MIT License。

### 2\. 記憶與上下文：長任務不能只靠塞更多文字

[Context engineering: memory, compaction, and tool clearing](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools?ref=aiposthub.com) 把長任務常見的資訊管理問題拆成三種機制。

Compaction 是把過長的對話歷史壓縮成摘要；tool-result clearing 是清掉之後能重新取得的工具結果；memory 則是把重要資訊保存到可跨 session 使用的檔案。Session 可以理解成一次持續中的 Agent 工作階段。

這三種做法處理的問題不同。壓縮能延長單次任務，但摘要本身會有資訊損失；清除工具結果能減少雜訊，但需要時必須重新查詢；記憶適合保存偏好與專案狀態，卻也需要決定什麼值得寫入、何時更新。

對產品經理來說，這篇很適合拿來重新檢查「記憶功能」需求。使用者說希望 AI 記得所有事情，不代表系統真的該無限制保存。產品仍要定義資料範圍、有效期限、可刪除性與隱私規則。

### 3\. 工具呼叫：工具越多，選擇與成本問題越明顯

Agent 要查資料、操作系統或執行交易，都需要透過工具完成。當工具從 5 個增加到幾百個，模型不只要選對工具，還得處理大量工具定義與回傳內容。

[Programmatic tool calling](https://platform.claude.com/cookbook/tool-use-programmatic-tool-calling-ptc?ref=aiposthub.com) 示範讓 Claude 在 code execution 環境內用程式呼叫工具，減少每次工具執行都必須回到模型判斷的往返。這種方法適合大量查詢、篩選與彙整，但不適合每一步都需要模型重新推理的流程。

Cookbook 也提供 tool search 相關範例，讓系統依任務需要動態找出可能使用的工具，而不是一次把所有工具定義塞進上下文。

產品設計上的重點是權限。能被程式重複呼叫的查詢工具，和會寄信、付款、刪除資料的工具，不該使用完全相同的開放方式。工具效率提升之前，必須先把可逆操作、重要操作與需要人工確認的操作分開。

### 4\. 驗證迴圈：Agent 說完成，不代表真的完成

[Outcomes: agents that verify their own work](https://platform.claude.com/cookbook/managed-agents-cma-verify-with-outcome-grader?ref=aiposthub.com) 示範一個獨立 grader 如何按照 rubric 檢查產出。Rubric 是把「什麼叫做合格」寫成可逐項驗證的評分標準；grader 則是只負責檢查結果的模型。

範例中的寫作 Agent 會先產出研究摘要，grader 再重新打開來源、核對引用與檢查涵蓋範圍。如果沒有通過，系統就把具體缺口交回原 Agent 修訂。

這比單純要求模型「請自行檢查」更可靠，因為檢查者有獨立的上下文與明確標準。不過，獨立 grader 也不是萬靈丹。若 rubric 寫得模糊，模型只會更有系統地通過一個模糊標準。

我會優先把這個模式用在能明確定義完成條件的任務，例如資料欄位完整性、引用可追溯性、測試是否通過、文件是否缺章節。對純美感或策略判斷，仍需要人類負責最後決策。

### 5\. 從原型到服務：部署、權限與維運才是後半場

Claude Cookbook 也收錄 Hosting your agent、SRE incident response agent、Slack data analyst bot 等案例。這些內容把 Agent 從 Notebook 原型拉到實際環境，開始處理容器、憑證、webhook、檔案掛載與人工審核。

Webhook 是系統在特定事件發生時，主動通知另一個服務的機制。例如 Agent 完成一個階段後，可以用 webhook 通知後端，再由使用者決定是否繼續下一步。

這類案例對產品規劃很重要，因為 Demo 成功只證明主要流程可行，還沒有回答權限怎麼管、失敗怎麼重試、操作如何稽核，以及成本失控時要在哪裡停止。真正能上線的 Agent，往往不是模型最聰明的版本，而是錯誤邊界最清楚的版本。

![Claude Console 中的 SRE Agent 執行紀錄](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/07/03-sre-agent-console.png)

Agent 依序讀取工具、日誌與 runbook，並保留每一步的事件紀錄。圖片來源：Anthropic Claude Cookbooks，MIT License。

### 6\. 前端設計：連「AI 味」也能拆成可操作限制

[Prompting for frontend aesthetics](https://platform.claude.com/cookbook/coding-prompting-for-frontend-aesthetics?ref=aiposthub.com) 比較接近 Prompt 指南，但仍有實作參考價值。它把常見的制式生成介面，拆成字體、配色、動效、背景與版型等可控制面向。

重點不是複製一大段「變好看」的 Prompt，而是學會把抽象評語改成具體限制。例如不要只說「設計要有質感」，而是指定資訊層級、主色比例、字級對比、互動狀態與需要避開的模板。

![未加入前端美學指引的 Claude 生成介面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/07/04-frontend-baseline.png)

沒有具體美學限制時，生成介面容易落入常見的白底、紫色與制式 SaaS 版型。圖片來源：Anthropic Claude Cookbooks，MIT License。

![加入前端美學指引後的 Claude 生成介面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/07/05-frontend-improved.png)

加入字體、色彩、背景與版型限制後，範例呈現更明確的視覺個性。圖片來源：Anthropic Claude Cookbooks，MIT License。

這套思路也適用於其他工作：若 Agent 經常交出看似完成、實際上不符合需求的結果，問題可能不在模型能力，而在驗收標準仍停留在形容詞。

## 哪些人適合使用 Claude Cookbook？

Claude Cookbook 最適合三類讀者。

第一類是正在串接 Claude API 的開發者。你可以直接參考程式碼、工具結構與錯誤處理方式，省下從空白專案猜測架構的時間。

第二類是 AI 產品經理。即使不負責寫正式程式，也能透過範例了解工程限制，把「加入記憶」「支援多 Agent」「自動驗證」這類模糊需求，拆成可以討論的產品規格。

第三類是會做技術原型的創作者或營運人員。若已經能看懂基礎 Python，Cookbook 能幫助你把一次性的 Claude 對話，改造成可重複執行的工作流。

反過來說，如果只是想找 Claude 網頁版的日常 Prompt，這個資源會顯得太工程化。多數範例需要 API key、Python 環境與基本程式能力，不是免安裝的 no-code 模板庫。

## 怎麼使用 Claude Cookbook 才不會收藏後就沒再打開？

最實際的方式，是從一個正在發生的問題開始。

1. 先寫出問題：例如「長任務會忘記前面的決策」「工具太多，模型常選錯」「產出看似完整但引用不可靠」。
2. 用首頁分類或搜尋找案例：不要從最熱門的範例開始，而是找最接近目前障礙的模式。
3. 先跑官方最小範例：確認環境、套件與 API 權限能運作，再替換成自己的資料與工具。
4. 把範例轉成驗收條件：記錄成功條件、失敗處理、成本上限、人工確認點與資料保存規則。
5. 回到最新文件核對：Cookbook 是學習範例，不是 API 規格本身；beta 功能、模型名稱與 SDK 寫法都可能更新。

這套做法比直接複製整個 Notebook 更慢一點，卻更容易維護。你真正要帶回產品的不是某一段程式碼，而是它解決問題的架構方式。

## 使用前要注意的限制

Claude Cookbook 是官方資源，但「官方範例」不等於「可直接上正式環境」。

部分內容使用 Claude Managed Agents beta 或特定版本的 SDK。Beta 代表功能仍在測試階段，介面與行為可能在正式推出前調整。正式產品還要另外評估安全、權限、個資、日誌、成本、延遲與異常復原。

另外，Cookbook 會為了教學而簡化情境。範例能證明一種模式怎麼運作，卻不一定涵蓋高流量、惡意輸入、工具誤用與跨系統資料一致性。最安全的做法，是把它當成原型起點，再依自己的風險等級補上測試與審核。

## FAQ

### Claude Cookbook 是免費的嗎？

Claude Cookbook 網站與官方 GitHub 程式碼可以免費瀏覽，GitHub 專案採 MIT License。不過，實際執行使用 Claude API 的範例時，仍可能產生 API 費用，部分功能也需要對應的 beta 存取權。

### 不會寫程式也看得懂嗎？

產品概念與架構說明仍有參考價值，但要完整執行範例，通常需要基礎 Python、API 與命令列能力。不會寫程式的讀者可以先看每篇的問題設定、流程圖與結論，再請工程師一起評估。

### Claude Cookbook 和官方文件有什麼不同？

官方文件主要說明 API 能做什麼、參數怎麼使用；Cookbook 則把多個功能組合成具體案例。前者是規格來源，後者比較像可拆解的實作範本，兩者應搭配使用。

### Claude Cookbook 只適合 Claude 使用者嗎？

程式碼通常以 Claude API 為核心，但多 Agent、記憶、工具權限與驗證迴圈等架構概念，也能套用到其他模型。需要調整的是 SDK、模型行為與平台提供的工具。

### 最推薦先看哪一篇？

如果還沒有明確問題，可以先看 Context engineering，理解 memory、compaction 與 tool-result clearing 的差別。若正在做多 Agent，就看 Async multi-agent orchestration；若最在意輸出品質，則從 Outcomes 或 Evaluator optimizer 開始。

## 結語：把它當成 Agent 工程模式庫，不是 Prompt 收藏庫

Claude Cookbook 最值得收藏的原因，不是它提供更多 Prompt，而是它把 Agent 產品常見的工程問題拆成可執行案例。

多 Agent、長期記憶、上下文壓縮、工具搜尋、獨立驗證與部署，看起來是不同功能，實際上都在回答同一件事：如何讓模型不只會推理，還能在一套可管理、可驗證的環境裡完成工作。

如果目前正準備做 Agent MVP，我會建議先從一個最明確的痛點挑範例，跑通後只保留必要的機制。先完成單一流程的可靠閉環，再逐步加入記憶、多 Agent 與自動驗證，會比一開始就堆滿所有功能更快落地，也更容易維護。

資源入口： [Claude Cookbook](https://platform.claude.com/cookbook/?ref=aiposthub.com)