---
title: "Claude Code 新推 /goals 指令：分離執行與評估，避免 AI 代理偷懶說謊"
source: "https://www.blocktempo.com/anthropic-claude-code-goals-evaluator-model-separates-agent-task-completion/"
author:
  - "[[Joe]]"
published: 2026-05-15
created: 2026-06-01
description: "Anthropic 為 Claude Code 推出 /goals 指令，將任務執行與完成判斷分拆給兩個獨立模型，因為讓同一個 AI 評判自己的功課，在架構上就是有缺陷的設計。（前情提要：Claude Code 宣布每週 Token 使用上限增加 50%！為期兩個月 Anthropic 搶佔開發者生態）（背景補充：Claude Code 自動執行功能 Routines 上線：排程、API、GitHub 事件三種觸發全支援） 你可能有碰到過這種狀況：AI 完成了程式碼設計，它回應你任務完成。但幾天後你才發現，幾個模組根本沒被編譯。這不是模型能力不足，而是模型自己決定它「已經做"
tags:
  - "clippings"
status: processed
---
[![Claude Code 新推 /goals 指令：分離執行與評估，避免 AI 代理偷懶說謊](https://image.blocktempo.com/2026/05/goals-1140x815.png "goals | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://image.blocktempo.com/2026/05/goals.png)

Anthropic 為 Claude Code 推出 /goals 指令，將任務執行與完成判斷分拆給兩個獨立模型，因為讓同一個 AI 評判自己的功課，在架構上就是有缺陷的設計。  
（前情提要： [Claude Code 宣布每週 Token 使用上限增加 50%！為期兩個月 Anthropic 搶佔開發者生態](https://www.blocktempo.com/anthropic-claude-code-weekly-limit-50-percent-boost-developer-ecosystem/) ）  
（背景補充： [Claude Code 自動執行功能 Routines 上線：排程、API、GitHub 事件三種觸發全支援](https://www.blocktempo.com/anthropic-claude-code-routines-schedule-api-github-webhook-trigger-preview/) ）

你可能有碰到過這種狀況：AI 完成了程式碼設計，它回應你任務完成。但幾天後你才發現，幾個模組根本沒被編譯。這不是模型能力不足，而是模型自己決定它「已經做完了」，但實際上它沒有。

為了改善這種狀況，Anthropic 本週為 Claude Code 新推出 /goals 指令。邏輯很直接：執行任務的模型，和判斷任務是否完成的模型，必須是兩個不同的角色。同一個模型不能同時扮演這兩者，因為它永遠是評判自己功課最糟糕的評審。

![](https://x.com/i/status/2054351031279186040)

### AI 代理為何會「提早收工」

AI 編碼代理的工作是一個迴圈：讀取檔案、執行命令、修改程式碼，然後判斷任務是否完成。問題就在最後這一步。

執行過程中積累的上下文：已完成的步驟、嘗試過的方法、走過的錯誤…讓模型對自身進度產生偏差。它傾向把「我做了很多」等同於「我做完了」。這個問題在企業環境中代價昂貴：程式碼遷移或測試修復若在終態前停止，往往要幾天後才被發現。

目前產業也有一些方案。OpenAI 讓代理模型自行決定何時停止，允許開發者自行接入外部評估器。Google ADK 支援透過 LoopAgent 實現獨立評估，LangGraph 也支援類似模式，但這些方案有共同點：批評節點（critic node）和終止邏輯需要開發者自己設計，平台不提供預設。

### 一個指令、兩個模型

/goals 的核心設計是把「執行」和「評估」正式拆成兩個角色。開發者輸入目標條件，例如：

/goal test/auth 目錄下的所有測試都通過，並且 lint 檢查結果為乾淨

每當代理嘗試結束工作，評估模型就接手核查。評估模型預設使用 Claude Haiku（Anthropic 旗下較輕量的模型）。選擇小模型的原因很簡單：評估者只需要做一個二元判斷，條件滿足或條件未滿足，不需要大模型的推理能力。

當條件未滿足，代理繼續執行；當條件滿足，評估模型將結果記錄到對話記錄並清除目標。整個流程在 Claude Code 內部完成，不需要額外的第三方可觀察性平台或自訂日誌系統。

Anthropic 檔案指出，有效的目標條件通常需要三個要素：可衡量的終態（測試結果、建置退出碼、特定檔案數量）；明確的驗證方式（例如「npm test 退出碼為 0」）；以及過程中不得變動的限制（例如「不得修改其他測試檔案」）。

[![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png "動區官網tg banner-1116 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://t.me/blocktemponews/)

## 📍相關報導📍

[Claude Code 桌面版大更新：多工並行、拖拉布局、三種顯示模式+新快捷鍵，為開發者而生](https://www.blocktempo.com/anthropic-claude-code-desktop-app-redesign-parallel-agents-multi-session/)

[Anthropic 訂閱 Claude Code 封殺龍蝦 OpenClaw！往後第三方工具僅能付費額度](https://www.blocktempo.com/anthropic-bans-openclaw-claude-subscription-oauth-extra-usage-api-key/)

[鞭打 Claude code 加速的 badclaude 開源專案，被 Anthropic 寄侵權通知信了](https://www.blocktempo.com/badclaude-github-anthropic-cease-desist-letter-claude-code-open-source/)

[Anthropic AI 經濟指數萬字報告：自動化交易工作流頻率翻倍，Claude 正從工具變生活助理](https://www.blocktempo.com/anthropic-economic-index-learning-curves-user-tenure-success-rates-claude/)

[頂級 AI 模型走向分化：ChatGPT to C，Claude to B](https://www.blocktempo.com/top-ai-models-divergence-gpt-to-c-claude-to-b/)

凡是以長週期代理迴圈為核心產品的公司，正系統性地離開大型實驗室模型、轉向自訓專屬模型。本文源自 Baseten 共同創辦人 Charlie O’Neill [文章](https://x.com/oneill_c/status/2054604986269802579) ，由動區編譯、整理。  
（前情提要： [Cursor 用 Kimi K2.5 訓練模型被抓包！刪推、官方表態急轉彎](https://www.blocktempo.com/cursor-composer-kimi-k2-5-license-disclosure-failure-moonshot-fireworks/) ）  
（背景補充： [Cursor 估值翻倍衝 500 億美元，迎接新一輪 20 億鎂融資](https://www.blocktempo.com/cursor-2-billion-50-billion-valuation-kimi-composer-anthropic-rival/) ）

過去五年，我每天做的事就是拿一個通用 LLM，教它做特定的事。最初是教 GPT-2 做模組化加法；現在則是教兆參數以上的模型執行有時需要數小時才能完成的任務。

### 2024 年：「Wrapper」元年

2024 年是「包裝層」之年。Cursor 是最典型的例子：它包裝大型實驗室的模型，超越 GitHub Copilot，成為 AI 輔助編程的首選。

2025 年，Cursor 推出了 Composer。底層基礎是開源的 Kimi，但魔法來自一個完全在內部進行後訓練（post-training）的模型。他們這麼做不是為了省 API 費用，而是因為他們想清楚了一件市場尚未完全定價的事：

> **在 Cursor 裡做好程式碼的 reward signal，只存在於 Cursor 內部，別無他處。**

### 整個 App 層的共同戰略

Cursor 只是最顯眼的例子，這件事已成為整個 App 層越來越一致的策略：貼近用戶，理解模型在何時真正有效，然後訓練出更好的模型與產品，讓飛輪轉起來。

這個模式一再出現，已不可能只是巧合。 **任何產品是長流程 agentic loop 的公司，都在離開大型實驗室，轉向用自己互動數據訓練出來的模型。**

Decagon、Abridge、OpenEvidence、Hippocratic、Intercom、Chroma、Pinterest、Cognition、Lovable、Notion、Harvey、Gamma、World Labs 等前沿公司，全都在開源權重之上訓練自己的模型。在 Baseten，我們協助這一波公司訓練核心 agent，讓他們從前沿 API 轉移到專業化模型。

### Sutton 的「苦澀教訓」救不了大型實驗室

常見的反駁是：「一般化終將勝過專業化，也就是說預訓練規模會贏，等下一個基礎模型就好。」

這個論點在你用算力對抗 **固定目標** 時成立。但大多數目標並不固定。

「在這個用戶的 repo 裡做好程式碼補全」或「為這位醫生的病患群撰寫好的臨床記錄」，這些都是移動的目標。 **正確性是透過產品迭代才被發現的。**

對靜態語料庫做再多的 next-token prediction，也產生不了它。只有針對結果做 RL（被接受 vs. 被拒絕的補全，在真實工作流程中成功 vs. 失敗的 agent 軌跡）才能產生它。而那些結果只存在於產品運行的地方。

這是 **專業化勝過一般化** 的軸線，也是前沿仍有剩餘空間的軸線。

### 實證案例已無可否認

過去一年，在相同能力預算下，專業化開源模型已能穩定匹配甚至超越前沿模型在 in-distribution agentic 任務上的表現；任務的流程越長、越依賴工具使用，這個差距就越大。

方向與前沿一致，但機制不同。我們正在獲得接近一個數量級更好的單位經濟效益。

### 為什麼大型實驗室無法跟進

原因是 **組織結構** 。

前沿實驗室的組織方式是「用一個模型服務多個客戶」。專業化要求的是相反：「為細分客戶建構多個模型，與推論堆疊和客戶的數據迴路共同設計。」

讓實驗室擅長預訓練的特質（集中式訓練運行、單一模型服務經濟、研究室型組織架構），與讓專業化業務成功所需的特質，存在積極的張力。Fine-tuning API 只是附帶業務，因為它 **必須** 是附帶業務。

佐證： **OpenAI 剛剛棄用了他們的 fine-tuning API。**

把專業化視為一級業務，意味著承認預訓練規模不是現實世界價值的關鍵限制，而這正是他們整個資本結構所立基的論題。他們可以雇用領域專家，但無濟於事，因為讓 OpenEvidence 或 Abridge 優秀的 98%，不是醫學知識，而是他們建立在產品中的 **反饋迴路** 。

### 飛輪只在產品迴路內轉

這一波公司都已想清楚： **在軟體成本崩潰之後，唯一能存活的護城河，是擁有一個用任何人都看不到的信號訓練出來的模型。**

- 每個用戶 session 都在產生訓練數據
- 每次訓練運行都能產出更好的模型
- 更好的模型吸引更多用戶與更多數據

飛輪在產品迴路內轉動，而大型實驗室，無論規模多大，都在這個迴路的外面。

每個產品都有一個極其細緻的「什麼是好」的標準。公司現在關心的是 **模型 UX** ——這是在訓練時決定的，無法透過 prompt 消除（例如，工具調用或搜尋深度的程度、工具調用的並行性等）。

### 結語

App 層公司現在問的問題已不再是「我們如何使用 AI？」

而是： **「我們如何抵抗商品化，為客戶提供更好的結果？」**

答案是：基於你每天服務對象的獨特理解，訓練出專業化模型。大型實驗室做不到，但你可以。

[![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png "動區官網tg banner-1116 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://t.me/blocktemponews/)

## 📍相關報導📍

[馬斯克 SpaceX 取得 Cursor 收購選擇權：$600 億買下 AI 開發神器或 $100 億分手費](https://www.blocktempo.com/spacex-cursor-acquisition-option-60-billion-xai-musk-ai-coding/)

[Notion 推出開發者平台：自訂程式碼、跨資料庫同步、整合 Claude Code,Codex,Cursor](https://www.blocktempo.com/notion-developer-platform-launch-ai-agents-workers-mcp-custom-code/)

[Y Combinator 創業指南解讀：AI Agent 在未來有哪些發展趨勢？](https://www.blocktempo.com/what-ai-agents-and-their-application-directions-are-worth-paying-attention-to/)

[加州大學研究「AI 腦霧」現象：14% 上班族被Agent、自動化搞瘋，離職意願高四成](https://www.blocktempo.com/ai-brain-fry-workplace-study-bcg-harvard-mental-fatigue-quit-intent/)

[完全指南：AI+NFT能碰撞出怎樣的火花？演算法整理、生成方式教學..](https://www.blocktempo.com/what-kind-of-combination-can-ai-and-nft-produce/)