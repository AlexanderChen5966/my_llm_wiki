---
title: "Loop Engineering 崛起：當 AI Agent 能自己工作，Google 工程師點人類三大職責"
source: "https://techorange.com/2026/06/10/loop-engineering-ai-agent/"
author:
  - "[[廖紹伶]]"
published: 2026-06-10
created: 2026-06-24
description: "AI Coding 的玩法，正從 Prompt Engineering（提示工程）擴大至「Loop Engineering（迴圈工程）」。OpenClaw 創辦人 Peter Steinberger 近日於社群平台 X 上發文：「每月提醒：你不該再對 Coding Agent 下提示了。你該設計的是會自動去提示 Agent 的 loop（迴圈系統）。」這則貼文截至發稿前獲得超過 790 萬次瀏覽。"
tags:
  - "clippings"
status: processed
---
AI Coding 的玩法，正從 Prompt Engineering（提示工程）擴大至「Loop Engineering（迴圈工程）」。OpenClaw 創辦人 Peter Steinberger 近日於社群平台 X 上發文：「每月提醒：你不該再對 Coding Agent 下提示了。你該設計的是會自動去提示 Agent 的 loop（迴圈系統）。」這則貼文截至發稿前獲得超過 790 萬次瀏覽。

同樣地，Anthropic Claude Code 負責人 Boris Cherny 日前表示：「我已經不再直接 Prompt Claude。現在是我建立的 Loop 在替我 Prompt Claude，並自行判斷接下來該做什麼。我的工作，就是設計這些 Loop。」

這些討論讓 Loop Engineering 一詞迅速竄紅。隨後，Google 工程師（Google Cloud AI 部門總監） Addy Osmani 在個人部落格發表長文，替這個概念提出一套完整的理解框架，並進一步點出他認為人類工程師扮演的角色。

## 從 Prompt Engineering 到 Loop Engineering

在 Prompt Engineering 工作模式下，開發者與 AI 的互動大多是一來一往：工程師提出需求，AI 生成程式碼，工程師閱讀結果後再給出下一個指令。整個過程，人類始終是流程控制者。但 Loop Engineering 的目標，則是把這個角色交給系統。

Osmani 將 Loop Engineering 定義為：「把負責下指令的人從自己，換成一套你設計好的系統」。換句話說，開發者不再持續告訴 AI 下一步該做什麼，而是建立一個能自行運作的循環流程。這個循環會自動發現工作、指派任務、驗證結果、記錄狀態，再決定下一步行動。

事實上，這並非全新發明的概念。Anthropic 在 2024 年發表的《Building Effective Agents》技術文章中，就已經提出「Evaluator-Optimizer」與「Orchestrator-Workers」等架構。前者由一個模型產生答案，另一個模型負責批判與修正；後者則由主代理分派工作給多個子代理執行。這些模式本質上都屬於 Agent 迴圈系統。

那麼，此時提出 Loop Engineering 有何不同？關鍵在於工具成熟度。Osmani 指出，一年前若想建立類似系統，工程師往往需要自行撰寫大量 Bash Script、工作流程管理工具與排程系統，如今這些能力已經直接內建進產品。

舉例來說，無論是 Claude Code 或 OpenAI Codex，都開始加入自動化排程（Automations）、子代理（Sub-Agents）、工作區隔離（Worktrees）、長期記憶（Memory）以及 MCP Connector 等功能。因此，Loop Engineering 更像是一種新的工作方法，而非單純的新技術。

## 一個 Loop 需要哪些要素？

Osmani 將 Loop Engineering 拆解成六個核心元素。

第一是 **自動化機制（Automation）** ，負責定期發現問題並啟動任務。第二是 **工作區隔離（Worktree）** ，用來隔離多個 Agent 的工作空間，避免互相覆蓋程式碼。第三是 **技能（Skills）** ，將團隊知識與開發規範寫成可重複使用的文件。第四是 **外掛與連接器（Connector）** ，讓 Agent 能夠連接 GitHub、Slack、資料庫或專案管理工具。第五是子代理（ **Sub-Agent）** ，也就是多代理協作模式。最後則是記憶（ **State Memory）** ，用來記錄任務進度與歷史狀態，例如一個 markdown 檔或 Linear 看板。

其中值得注意的，或許是 Sub-Agent。Osmani 認為，撰寫程式碼的 Agent 不應該同時擔任審查者。「寫程式的模型給自己的作業打分數時實在太仁慈了。」因此更好的做法是讓不同 Agent 扮演不同角色：一個負責探索方案，一個負責實作，另一個專門驗證結果。這也是許多 AI 公司近年大力發展多代理架構的原因。

## 當 AI 開始管理 AI，人類工程師還剩下什麼工作？

然而，Loop Engineering 的興起，也帶來新的問題。如果 Agent 已經能夠自行發現問題、執行任務、驗證結果，甚至指揮其他 Agent，人類工程師的價值還剩下什麼？

但 Osmani 強調，迴圈改變的是工作，不是把人類從工作中刪除，而且有三個問題會隨著迴圈越順暢而越棘手。

第一是 **驗證** 責任仍然在人類身上。因為即使 Loop 能夠長時間自主運作，它仍然可能持續犯錯。「你的工作不是產生程式碼，而是確認程式碼真的可以運作。」

第二是 **理解** （Comprehension）。Osmani 提出一個值得警惕的概念：Comprehension Debt（理解債），也就是當 Agent 產生愈來愈多程式碼，而開發者閱讀越來越少程式碼時，團隊對系統的理解程度將逐漸下降。短期內專案開發速度可能提升，但長期來看，沒有人真正理解系統如何運作。

第三是 **認知投降** （Cognitive Surrender）。Osmani 表示，當 Loop 運作越來越順暢，人們很容易停止思考，長期下來，人類可能逐漸失去形成獨立判斷的能力。他指出：「設計迴圈（Loop）本身並不是答案。當你帶著判斷力去設計它時，它能成為解方；但當你用它來逃避思考時，它反而會成為加速問題惡化的催化劑。同樣的行動，卻可能帶來完全相反的結果。」這也是他為什麼認為 Loop Engineering 比 Prompt Engineering 難的原因。

## Loop Engineering 不一定適合所有任務

不過，儘管社群討論熱烈，但實務上是否值得建立 Loop，仍高度取決於任務性質。科技電子報《AlphaSignal》指出，目前大部分開發者其實還不需要建立 Agent Loop。《AlphaSignal》認為，只有在四個條件同時成立時，Loop 才有可能帶來正面效益。

這四個條件是：任務具有高度重複性、驗證機制已自動化、團隊擁有足夠 Token 預算，以及 Agent 具備完整工具存取權限。如果缺少其中任何一項，Loop Engineering 可能只是在增加成本，而非提升效率。

《AlphaSignal》認為目前最適合導入 Loop Engineering 的情境仍集中在 CI 故障排查、依賴套件更新、Lint 修正與 Issue 轉 PR 等高度標準化工作。至於架構重構、產品設計或需要大量判斷的工作，仍然更適合由人類主導。

Osmani 也提醒，「別忘了，直接與 Agent 對話、親自下指令，同樣是一種有效的方法。」他表示，關鍵不在於完全依賴哪一種模式，而是在兩者之間找到適當的平衡。他認為，如同 Cherny 所言，這或許預示著未來工作的演變方向：真正改變的不是工作變得更簡單，而是創造價值的槓桿點已經移動了。

他最後指出，「建立你的 Loop，但請以一個打算繼續當工程師的人來建立它，而不是只想按下『開始執行』按鈕的人。」

> 【推薦閱讀】
> 
> ◆ [RSI 是新的 AGI：矽谷追逐的最終魔王關，為何讓 Anthropic 呼籲全球按下暫停鍵？](https://techorange.com/2026/06/05/ai-rsi-agi-recursive-self-improvement/)
> 
> ◆ [Anthropic 推出 Fable 5 與 Mythos 5，最重要的升級其實不是模型](https://techorange.com/2026/06/10/anthropic-fable-5-mythos-5-ai/)
> 
> ◆ [軟體股反彈不代表危機解除，Snowflake CEO：AI Agent 時代 SaaS 公司最危險的是太早樂觀](https://techorange.com/2026/06/09/snowflake-ceo-says-better-be-paranoid-than-confident-about-saaspocalypse/)

＊本文開放合作夥伴轉載，資料來源： [Addy Osmani](https://addyosmani.com/blog/loop-engineering/) 、 [Peter Steinberger](https://x.com/steipete/status/2063697162748260627) 、 [AlphaSignal](https://alphasignalai.substack.com/p/most-developers-do-not-need-agent?utm_source=chatgpt.com) ，首圖來源： [Unsplash](https://unsplash.com/photos/a-multicolored-abstract-image-of-a-curved-object-OjusIokzQw8)

![](https://sslcode.adgeek.com.tw/public/images/popad_close_button.png)