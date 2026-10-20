---
title: "讓 Claude Code 管理我的人生 3 個月後，最有效的其實不是自動化"
source: "https://codelove.tw/@tony/post/31y683#google_vignette"
author:
published:
created: 2026-06-15
description: '[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------\**本文重點整理*- 用 Claude Code × GitHub 做出「自己的使用說明書」，並完整交給 AI ...'
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月14日](https://codelove.tw/@tony/post/31y683 "2026-06-14 08:15:10")

## 讓 Claude Code 管理我的人生 3 個月後，最有效的其實不是自動化

## 前言

\* *本文重點整理*

- 用 Claude Code × GitHub 做出「自己的使用說明書」，並完整交給 AI 的運用，持續了 3 個月
- 也用 Skills 和 MCP 做了自動化。但真正最有效的，不是自動化，而是「強迫自己把自己說清楚」
- 只要一個備忘錄 App 就能開始。就算不是工程師也沒問題

> 讀者對象
> 
> - 覺得跟 AI 諮詢時總是有點淺的人
> - 一再立目標又忘記的人
> - 想實際試試看 Claude Code 的 Skills 和 MCP 的人

突然一問，當你很認真地向 ChatGPT 或 Claude 請教——「職涯該怎麼辦」「這個決定，客觀來看你覺得如何？」——的時候，有沒有每次都先這樣鋪陳？

> 「先說明一下我的情況，我 20 多歲，職業是〇〇……」

我以前完全就是這樣。每次都得從頭講一次「我」這個前提，而且對方還不記得昨天的對話。 **世界上最博學的諮詢對象，卻每次都像第一次見面一樣對待你** 。

今年春天，我決定解決這個「初次見面問題」。簡單說，就是把「我這個人」寫成一份“使用說明書”，讓 AI 可以整份接手。

……這樣寫起來很帥，但我開始的動機其實更不純，就是覺得「如果連人生的事都能交給 AI 自動處理，應該會很輕鬆吧」這種偷懶心態。結果先講結論：這個盤算只對了一半。真正有用的，不是自動化。下面我就來寫這件事。

## 起因：腦中的「自己」，其實幾乎沒有被言語化

這件事的開端，來自一篇文章和一支影片。

hand\_dot 的〈用 GitHub 管理人生〉，以及 Kyohei 的影片。「用 GitHub 的 Issue 管理人生的任務和目標」這個點子，真的直接把我敲醒。實際做下去之後，也確實沉下去了。

不過老實說，這種想法其實已經有很多人做過了。

「GitHub × 人生管理 × AI」早就是一個大類別了。那我為什麼現在還要寫？因為我認真運作了 3 個月之後，終於明白了： **「做出來」和「持續運轉」之間，有一道很深的鴻溝** 。而跨越那道鴻溝的關鍵，不是工具，而是設計思維。

再往深一點看，在開始運用之前，我先發現了一件事：自己的價值觀、目標、經驗，雖然以為「都在腦中」，其實根本沒有真正被說成文字。「你最重視的是什麼？」——能立刻回答的人有多少？就算向 AI 諮詢，答案還是模糊不清，並不是 AI 的問題，而是輸入的「自己」本身就很曖昧。

既然如此，就只能好好寫下來。

## 做的事：用 5 個軸把自己寫出來

一開始最先撞上的牆，就是「要寫什麼、怎麼分類」。這裡如果隨便做，我曾經整個大翻車。最初我憑感覺建了大概 10 個資料夾，結果 3 天後就變成「我到底把東西寫在哪裡了」的狀態了（很丟臉但是真的）。

經過反覆試錯，最後我找到的是用「時態」來分資訊的整理方式。

軸內容時態 `00_self` 自己這個人（價值觀、性格、健康、金錢、經歷）靜態 `01_direction` 前進方向（人生目標、年度目標、里程碑）未來 `02_work` 進行中的工作進行中 `03_journal` 日常紀錄與回顧過去〜現在 `04_knowledge` 從外部吸收的知識（書籍、思想）累積

重點是，不要把「不會變的東西（self）」、「想前往的方向（direction）」、「會流動的東西（journal）」混在一起。

為什麼要用時態來分？因為在讓 AI 幫你諮詢時，該讀的是靜態的自己（self）與方向（direction）；在讓它做回顧時，該讀的是時間序列（journal）。也就是說，依照目的不同，參照的資料來源也會變。圖像化來看，大概就是這樣的概念。

如果是混在一起的雜亂筆記本，AI 也不知道該重點看什麼。資料夾設計，其實就是給 AI 的「請看這裡」地圖。

這裡不一定要用 Git。用 Notion 的頁面、或分資料夾的文字檔都可以。重點是要有「分類的軸」。我只是剛好是工程師，所以才選 Git。

運作規則我寫在一個叫 `CLAUDE.md` 的單一檔案裡，讓 AI 每次都讀。像是「金錢或健康這類敏感資訊不要擅自修改」「新的目標要先從年度目標開始做」——人類很難一直守約，但只要把規則明文化，AI 反而會非常忠實地照做。

裡面包含財務、健康、人際關係等內容，所以這個 repository 我設成私有。原始靈感的作者們把 repository 公開，真的很厲害，但我實在沒那個膽子。所以這篇文章只分享架構和機制。

## 讓人生擁有「命令」與「記憶」（技術部分）

接下來是我用 Claude Code 做的自動化部分。如果你對技術沒興趣，可以直接跳到下一章。

### 自製 Skills ＝ 人生的「斜線命令」

我用 Claude Code 的 Skills 功能，把常用操作命令化了。

- `/life-advice` — 讀入價值觀、目標、現況等所有 MD 檔，依照我的脈絡來給建議
- `/new-task` — 以年度目標為起點，開出新的 Issue
- `/monthly-review` — 回顧當月紀錄，整理結果
- `/daily-sync` — 把每天零散的紀錄，每月 1 次反映到各分類的中繼資訊中

多虧了 `/monthly-review` ，人類最不擅長的行為之一——「把立下的目標在月底回顧一次」——總算能半自動運轉了。

### 用 MCP 連接外部服務

我透過 MCP（Model Context Protocol）把行事曆和任務管理工具連到 Claude。「看一下下週行程，對照這個月的目標，幫我提議空檔該怎麼利用」——像這樣的諮詢，可以不用在各個 App 之間切來切去，而是一個地方就完成。

### 用記憶功能，讓 AI「記住自己」

最關鍵的是記憶功能。AI 會把我在對話中表現出的偏好和回饋，自動寫進檔案並永久保存。下一次對話時，它也會讀取這些內容，所以越用，越能提升「這個人是誰」的解析度。開頭提到的「初次見面問題」，到這裡就消失了一部分。

## 最有用的，其實不是「自動化」

好，這裡就是本文最想講的地方。

Skills、MCP、記憶功能，確實都很方便。但用了 3 個月回頭看，對人生最有影響的，卻不是這些自動化。本質上的領悟有 3 個。

### 1\. 最大的效果是「言語化的強迫力」

比起自動化，最有效的是：為了寫下來，我不得不把自己變成文字。原本以為「自由最重要」，但真要寫的時候卻卡住了：「自由，是從什麼的自由？」這個「卡住」本身，就是自我理解。為了交給 AI 而寫的過程中，我才發現最不了解自己的人，其實就是自己。

原本想著要偷懶，結果最後真正發揮作用的，反而是最原始的「書寫」。老實說，這讓我有點意外。

### 2\. 目標由人類掌握，AI 負責實行

我越來越強烈地覺得，因為方便就把「人生該怎麼活」也丟給 AI，這是不對的。決定前進方向的是自己，把執行速度拉高的是 AI。讀地圖可以交給 AI，但要去哪裡還是得自己決定。要是把這兩件事混在一起，就會變成表面上是在最佳化自己的人生，實際上卻不知不覺被最佳化成「某個人的平均值」。

### 3\. 「看不見的規則」，其實也藏在自己身上

我本來就很喜歡解讀社會上那些看不見的規則，然後用策略去攻略它。這次實作之後，我才意識到：最看不出規則的遊戲，其實是「自己」。只有把自己的價值觀、習慣、判斷基準這些「規格」寫出來，才算真正能把自己當成一個可攻略的對象。

## 今天就能試的 4 個步驟

如果你心想「聽起來不錯，但門檻好像很高」，那我先幫你分階段了。Lv1 今天 5 分鐘就能開始。

1. **Lv1：先寫下來（任何人都可以）** 在備忘錄 App 裡列出「價值觀」「今年想做的事」「最近的煩惱」，然後直接複製貼上給 AI 請教。只這樣就會提升解析度。
2. **Lv2：分開來放** 把檔案大致分成「自己・目標・記錄」。這樣就能指定 AI「希望你看哪一部分來回答」。
3. **Lv3：放上 Git（工程師）** 做成 repository，留下變更歷史。能用 `diff` 比較「半年前的自己」和現在，雖然低調，但很有用。
4. **Lv4：自動化（進階）** 用 Claude Code Skills 或 MCP，把諮詢、回顧、紀錄命令化。

重點是，就算只做到 Lv1，也已經很有效。最可惜的，是一開始就衝 Lv4 然後挫折。

## 結語

今年春天我開始做的，是「寫出自己的使用說明書」這件事。它看起來很平凡，卻確實很有效。原本只是想偷懶，最後卻被最原始的「言語化」拯救了，這就是結局。

再次感謝給我靈感的 hand\_dot 和 Kyohei。若我能在前人的點子上，稍微加上一點自己的設計思維與 3 個月的實際運作經驗，那就太好了。

最後留下一個問題，作為結尾。

> 你的生命 `main` 分支，現在正朝哪裡前進呢？

先從在備忘錄 App 寫下 3 行開始吧。如果你也試了，歡迎跟我說你是用什麼軸來分類的。

---

原文出處： [https://qiita.com/ktdatascience/items/3c8949b62ce1dfe7a024](https://qiita.com/ktdatascience/items/3c8949b62ce1dfe7a024)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/31y683) [40瀏覽](https://codelove.tw/@tony/post/31y683)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/31y683)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [我如何使用 Claude Code Skills 實現股票篩選自動化 \[Python × yfinance × Vibe Coding\]](https://codelove.tw/@tony/post/q5nlWx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝14 ❤️1

445

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

1

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1781493263&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F31y683&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuNTQiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE0OS4wLjc4MjcuNTQiXSxbIkNocm9taXVtIiwiMTQ5LjAuNzgyNy41NCJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1781493263367&amp;bpp=1&amp;bdt=85&amp;idt=39&amp;shv=r20260611&amp;mjsv=m202606090101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1781493177%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1781493177%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1781493177%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2097935953850&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099009%2C95390667&amp;oid=2&amp;pvsid=7848404580204400&amp;tmod=1085849897&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=52" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260611/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>