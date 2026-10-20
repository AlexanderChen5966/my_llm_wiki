---
title: "Anthropic 自曝過度約束：Claude 5 世代，管得越少越好用"
source: "https://www.blocktempo.com/anthropic-removed-80-percent-claude-code-system-prompt-context-engineering/"
author:
  - "[[Joe]]"
published: 2026-07-26
created: 2026-07-27
description: "Claude Code 團隊成員 Thariq Shihipar 撰文坦承，你辛苦練成的提示詞技巧，多數是舊世代模型才需要的強硬規矩。面對 Claude Opus 5 與 Claude Fable 5，官方把系統提示砍掉逾 8 成，內部編碼評測沒有出現可測量的下滑。（前情提要：Anthropic 發布 Claude Opus 5！效能逼近 Fable 5 價格卻砍半，成最新預設模型）（背景補充：Claude Code 新推 /goals 指令：分離執行與評估，避免 AI 代理偷懶說謊） Claude Code 團隊成員 Thariq Shihipar 於本週在官方部落格發文"
tags:
  - "clippings"
status: processed
---
[![](https://image.blocktempo.com/2026/07/cover_20260726_021024-1140x815.webp "cover_20260726_021024 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://image.blocktempo.com/2026/07/cover_20260726_021024.webp)

Claude Code 團隊成員 Thariq Shihipar 撰文坦承，你辛苦練成的提示詞技巧，多數是舊世代模型才需要的強硬規矩。面對 Claude Opus 5 與 Claude Fable 5，官方把系統提示砍掉逾 8 成，內部編碼評測沒有出現可測量的下滑。  
（前情提要： [Anthropic 發布 Claude Opus 5！效能逼近 Fable 5 價格卻砍半，成最新預設模型](https://www.blocktempo.com/anthropic-announces-claude-opus-5-release/) ）  
（背景補充： [Claude Code 新推 /goals 指令：分離執行與評估，避免 AI 代理偷懶說謊](https://www.blocktempo.com/anthropic-claude-code-goals-evaluator-model-separates-agent-task-completion/) ）

Claude Code 團隊成員 Thariq Shihipar 於本週在官方部落格發文 [指出](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models) ，他們大幅改寫了 Claude Code 的系統提示，對象是 Claude Opus 5 與 Claude Fable 5 這兩款新世代模型。他指出過去一條一條累積下來的提示詞經驗，很大一部分正在這群新模型面前已失效。

這次改動幅度不小：系統提示裡超過 8 成的內容被直接刪掉，而內部的編碼評測沒有出現可測量的下滑。這代表的是：寫得越多、管得越細，不代表 Claude 表現得越好；對新一代模型來說，經常是管得越少，反而跑得越順。

### 過度約束是怎麼發生的

Shihipar 在文章裡拆解了一個具體現場：同一次請求裡，系統提示、CLAUDE.md 與 Skills 三方的指令疊在一起，經常互相打架。例如一邊寫著「視情況保留必要的說明」，另一邊卻寫著「禁止寫程式碼註解」。Claude 得先耗力氣判斷哪一條才算數，才能真正動手。規則不是在幫模型省力，而是先製造了一層要拆解的矛盾。

這些規則不是憑空冒出來的。Claude Code 剛推出時，團隊怕的是最壞情況，例如模型誤刪檔案，所以系統提示裡塞滿強硬規定，等於替判斷力還不夠的舊模型架設護欄。舊版曾經明白寫著：預設不寫註解、禁止多段落的說明字串，除非用戶主動要求，否則不要另外生出規劃或分析用的檔案。

對當時的模型而言，這是必要的取捨。

但同一道護欄擺到新模型面前，情況就變了。新版系統提示已經把那一長串規定換成一句判斷：「Write code that reads like the surrounding code」，白話說就是，註解密度、命名與慣用寫法全部跟著周圍的程式碼走。

### 那些老招式，現在都成了迷思

第一條最反直覺：過去團隊相信，教 Claude 用工具最好的方法是給範例，把使用情境一條條列出來。現在他們發現，範例反而把模型的探索空間鎖死在範例劃出的框框裡。更好的做法是把工具設計得更會說話：例如 Todo 工具只要把狀態列成 pending、in\_progress、completed 這幾個列舉值，模型光看這組狀態就知道怎麼用，不必再靠舉例硬教。

第二條是「全部前置」。過去系統提示習慣把程式碼審查、驗證流程的細節一次寫完，用不用得到都先塞進去，結果工作還沒開始，上下文視窗就先被占掉一大塊。現在驗證與程式碼審查被拆成可按需呼叫的 skill，部分工具說明也改成延遲載入：模型真正要用時，才透過 ToolSearch 把完整定義找出來。

這套漸進揭露的邏輯同樣適用於你自己的 CLAUDE.md：別想著把所有知識塞進一份大檔案，拆成一整棵按時機載入的檔案樹才是正解。

第三條最貼近日常操作：CLAUDE.md 的手動記憶正在被自動記憶取代。過去要靠用戶自己按下熱鍵，才把心得寫進 CLAUDE.md；現在 Claude 會自行判斷哪些內容值得留下並存成記憶。管得越細，不再等於記得越牢。

### 四層分工才是新解法

Shihipar 給的解法不是刪光所有規則，而是重新分配四層脈絡各自該管什麼。

System Prompt 綁的是產品邏輯，一般開發者幾乎碰不到，自己搭建代理框架的人才該把力氣花在這裡。

CLAUDE.md 保持輕量，篇幅留給程式庫裡真正的陷阱與眉角，不必寫「Claude 看一眼檔案結構就懂」的廢話

Skills 當成輕量指南，除非是極重要的領域，否則別寫得太約束，長的 skill 更要拆成多個檔案。

References 這一層則優先用程式碼形式帶進來。一份 HTML 設計稿，通常比一段文字描述或一張截圖更能讓模型抓到你要的樣子。規格也可以更具體：一份詳細的測試套件，或另一個程式庫裡等著被移植的既有函式，都能直接當規格用；再進一步，還能寫成一份評分準則（rubrics），讓 Claude 動態拉出驗證用的子代理，逐項檢查成果是不是符合你要的那種品味。

官方也同步推出 claude doctor 指令，在 Claude Code 裡輸入 /doctor，就能檢查 skills 與 CLAUDE.md 是不是寫得太肥、太約束。開發者社群這陣子聊的正是同一種煩惱：裝了一堆 Skills、Plugins 與 MCP Server，又寫了一份長到看不完的 CLAUDE.md，工作都還沒開始，脈絡視窗就先被吃掉一半。

你手上那套「規則寫越細，AI 才會越聽話」的舊經驗，擺在新一代模型面前不是加分，而是佔位置的死重。真正該問的問題，不是還能再加哪一條規則，而是哪一條規則，已經可以刪掉了。

[![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png "動區官網tg banner-1116 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://t.me/blocktemponews/)

### 📍相關報導📍

**[Claude Sonnet 5 上線：Anthropic 喊多項表現逼近 Opus，但 API 費用便宜 6 成](https://www.blocktempo.com/anthropic-launches-claude-sonnet-5-near-opus-performance-cheaper-ipo/)**

**[Anthropic 發表 Claude Fable 5 與 Claude Mythos 5 模型：針對網路安全設下最嚴護欄](https://www.blocktempo.com/anthropic-launches-claude-fable-5-and-mythos-5/)**

**[Anthropic 不再顯示 Claude 思考過程摘要，防止 Fable 5 被蒸餾](https://www.blocktempo.com/anthropic-hides-claude-thinking-summaries-anti-distillation-fable-reasoning-defense/)**

### 最新文章

---

[![](https://image.blocktempo.com/2026/07/cover_20260725_150037-1140x815.webp "cover_20260725_150037 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://image.blocktempo.com/2026/07/cover_20260725_150037.webp)

北韓當局逮捕一群前國家網民與 IT 專家，他們入侵北韓中央銀行和外貿銀行的內部網路，將國庫資金轉換為加密貨幣後透過中國經紀人洗錢。  
（前情提要： [北韓駭客 2025 年盜竊創紀錄：竊取 20.2 億美元加密貨幣，洗錢週期約 45 天](https://www.blocktempo.com/2025-hack-chainalysis/) ）  
（背景補充： [資安圖解｜美國司法部「起訴中國公民」OTC助北韓駭客洗錢事件: 哪些交易所被盜了？](https://www.blocktempo.com/look-deep-into-case-about-chinese-nationals-accused-of-laundering-millions-in-stolen-crypto-for-north-korea/) ）

北韓當局近期逮捕了一群前國家網路兵與 IT 專家，指控他們入侵北韓中央銀行與外貿銀行的內部網路，將國庫資金轉為加密貨幣後，透過中國經紀人進行洗錢。根據首爾專責北韓新聞的 Daily NK 於 7 月 23 日 [報導](https://www.dailynk.com/20260723-1/) ，援引平壤匿名訊息來源指出，該群體竊取國家資金後，先將韓圓兌換成加密貨幣，再透過駐在中國的經紀人完成洗錢流程。

### 竊取自家銀行的「逆向洗錢」

被入侵的兩家銀行分別是北韓的中央銀行（北韓人民銀行）和外貿銀行（北韓外貿銀行），顯示國家資金在被竊取後經歷了「法幣 → 加密貨幣 → 洗錢」的完整鏈路。

這次逮捕標記了北韓駭客首次被指控竊取自家政府金融機構的案例，屬於少見的「內部反噬」事件。

### 北韓的加密貨幣產業鏈

我們知道北韓駭客組織惡名昭彰，根據近期的資料，北韓在 2025 年共竊取了 [創紀錄的 20.2 億美元加密貨幣](https://www.blocktempo.com/2025-hack-chainalysis/) ，年增率高達 51%。

這些資金被用來支撐北韓的大規模殺傷性武器計畫、匯率操縱以及國際制裁規避。北韓駭客透過入侵加密貨幣交易所、橋接鏈和 DeFi 協議來竊取資產，竊取過程通常包括：假設身份、注入流動性、和快速洗錢…諷刺的是這次目標變成自家銀行。

[![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png "動區官網tg banner-1116 | 動區動趨-最具影響力的區塊鏈新聞媒體 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://t.me/blocktemponews/)

### 📍相關報導📍

**[北韓駭客 2025 年盜竊創紀錄：竊取 20.2 億美元加密貨幣，洗錢週期約 45 天](https://www.blocktempo.com/2025-hack-chainalysis/)**

**[資安圖解｜美國司法部「起訴中國公民」OTC助北韓駭客洗錢事件: 哪些交易所被盜了？](https://www.blocktempo.com/look-deep-into-case-about-chinese-nationals-accused-of-laundering-millions-in-stolen-crypto-for-north-korea/)**

**[聯合國報告：北韓近 2 年駭客攻擊盜得「3 億美元」，KuCoin 交易所恐為受害者](https://www.blocktempo.com/un-report-north-korea-hack-kucoin-benefit-300-million/)**

**[北韓駭客長這樣！FBI全球通緝「懸賞金千萬鎂」涉全球比特幣勒索資助金正恩](https://www.blocktempo.com/north-korean-hacker-group-implicated-in-global-cyberattacks-agents-offered-a-bounty-of-300-million/)**

**[美國司法部指控：北韓情報組織三名駭客，共涉竊「1億美元以上」加密貨幣](https://www.blocktempo.com/us-doj-sue-three-north-korean-rgb-members-hackers-streal-1-3-billion-crypto/)**