---
title: "用 mattpocock 的 Skills for Real Engineers 組出自己的 AI 工作流"
source: "https://muki.tw/skills-for-real-engineers-introduce/"
author:
  - "[[MUKI space*]]"
published: 2026-05-18
created: 2026-08-14
description: "最近這幾天看到 GitHub 瘋長的 repo，是來自 ma..."
tags:
  - "clippings"
status: "processed"
---
用 mattpocock 的 Skills for Real Engineers 組出自己的 AI 工作流.md

最近這幾天看到 GitHub 瘋長的 repo，是來自 mattpocock 的 [Skills For Real Enginerrs](https://github.com/mattpocock/skills)

![](https://camo.githubusercontent.com/f1dfca7a2d5f6ac1363e06d2d9ddf9504c1afc2b88f8e53483004ebd432de6e0/68747470733a2f2f7265732e636c6f7564696e6172792e636f6d2f746f74616c2d747970657363726970742f696d6167652f75706c6f61642f76313737373338323237372f736b696c6c2d7265706f2d6c696768745f32782e706e67)

剛好看到 threads 也有人在分享這套 skills，所以就來研究一下這套 skills 的內容，以及他可以如何跟 [superpowers](https://github.com/obra/superpowers) 合併成一套工作流，話說我發現他的 star 有 89k，好猛 XXD

---

## 人生總是缺一套工作流

大家在用 AI 寫程式時，可能常常碰到這些狀況：

- 跟 AI 聊了一大堆需求，產生的程式碼卻跟自己想的完全不一樣
- 來回改了幾輪後，發現專案充滿重複的類別、方法、或是微妙變形的 util 函式
- 一開始還算乾淨的架構，沒多久就變成一坨誰也不想碰的爛泥

所以應運而生的，產生了非常多知名的流程框架，包含 GSD、BMAD、Spec‑Kit... 等等，這些所謂的「大流程框架」，都是在解決同一個痛點：幫我們把整個開發流程接管下來

但正因為他們太大了，所以實際用起來反而有點綁手綁腳，而且出問題時，我們只會知道流程怪怪、卡卡的，卻很難精準知道是哪一步驟出事？就算知道了，也很難只改那一小塊 (因為他整個流程是一條龍幫你弄好的)

### 小而專注的技能

Matt Pocock 走的是完全不一樣的路線，他做的不是大框架，而是一個個小而專注的技能，好處是讓我們可以組裝出適合自己的 AI 開發流程

他的設計核心可用三個形容詞概括：

- Small：每個 skill 都是單檔可讀，幾分鐘內就能看完
- Composable：我們可以只用其中一兩個，也可以串成一整條流水線
- Hackable：不喜歡哪個步驟？打開檔案改成你自己的版本就好

對我來說，這套 skills 比較像是 AI 開發版的 Unix，就是一堆小工具各自做好一件事，再讓我們用自己的方式把它們拼裝起來

---

AD

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="722" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-6816776861246904&amp;output=html&amp;h=280&amp;slotname=2619535264&amp;adk=536108718&amp;adf=3680071312&amp;pi=t.ma~as.2619535264&amp;abgtt=7&amp;w=722&amp;fwrn=4&amp;fwrnh=0&amp;lmt=1786686876&amp;rafmt=1&amp;armr=3&amp;format=722x280&amp;url=https%3A%2F%2Fmuki.tw%2Fskills-for-real-engineers-introduce%2F&amp;host=ca-host-pub-2644536267352236&amp;fwr=0&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786686248404&amp;bpp=2&amp;bdt=1093&amp;idt=107&amp;shv=r20260812&amp;mjsv=m202608100101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dabed5c4f6c137a51%3AT%3D1786685192%3ART%3D1786686874%3AS%3DALNI_MYfJJNV1SVrdt4l6IPLqwFH3KHkcg&amp;gpic=UID%3D000014df72091a95%3AT%3D1786685192%3ART%3D1786686874%3AS%3DALNI_MYFojP7AHvvhSEstSBqIy9HMPHDYQ&amp;eo_id_str=ID%3D61820914debd3376%3AT%3D1786685192%3ART%3D1786686874%3AS%3DAA-AfjbBDTvN3lpfNfXahxBDXTZo&amp;prev_fmts=0x0%2C220x600%2C1634x908&amp;nras=2&amp;correlator=8323315462508&amp;frm=20&amp;pv=1&amp;rplot=4&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=322&amp;ady=2322&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95396666%2C95399117%2C122880127%2C42533294&amp;oid=2&amp;pvsid=5980758172481250&amp;tmod=1284125063&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fmuki.tw%2Fjournal%2F&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=M%7C%7CeEbr%7Cp&amp;abl=XS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C241x714_r&amp;bz=0&amp;pgls=CAEaBTcuMC4x~CAEQBBoHMS4xODIuMA..&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 這套 Skills 解決了四個問題

乍看這個 repo 裡有一長串 skills 名單，會有一種：「我真的要學完這一坨嗎？」的壓力。但把 Readme 看完後，我覺得可以用四個經典問題把它們重新歸納收斂：

1. **理解不對齊**  
	你以為 AI 聽懂了需求，結果產出的東西完全跑偏，人類世界叫「產品／工程」，AI 時代只是把工程師換成 agent 而已
2. **沒有共同語言**  
	實務上，很多領域概念在不同層面會被拆成不同的東西，  
	例如我們腦中想的「使用者」、資料庫 schema 裡的 user table、程式碼裡的 User 或 Member class，彼此可能是不同的東西。這有點繞口令，但總之，名字相同的不一定代表同一個概念，名字不同的也可能其實在指同一件事  
	此時，當我們沒有一份共同的詞彙表時，AI 就只能自己猜，最後很容易寫出和你想像迥異的設計
3. **缺少快速回饋**  
	用 AI 產生程式碼，最容易讓人類發懶的症狀，就是 AI 會一口氣丟給你幾百行程式碼，真的沒有人想看 ><  
	同時，假設沒有良好的測試與驗證節奏，我們根本不會知道哪裡有問題，只能用肉眼看，但又懶得看，然後就 Let it go....，然後就爆炸.....
4. **混亂的熵增現象**  
	有了 AI 之後，寫出一大坨可動但難維護的程式碼，變得前所未有地容易。我們產出變快了，但設計品質並不會自動跟上

而 matt 的整套 skills ，就是圍繞這四個問題在設計的

---

## Skills 安裝推薦

你可以選擇一鍵安裝 Skills，但一鍵安裝帶來的熵增也高，你沒辦法很好地領略每個 Skills 代表的意義，就很難有效的直擊痛點，或是呼叫對的 Skills。

但當然，一鍵安裝不是不行，而是建議大家在安裝之前，還是要了解每個 skills 代表的意義，以及評估好他能幫助到你什麼？他可以優化你現有工作流的哪一階段？

然後以下幾個，可能是作者 matt、也可能是我，融合推薦的建議必裝 Skills，歡迎參考：

### 1\. 用 grill-with-docs 跟 AI 建立共同語言

`/grill-with-docs` 是這套裡面最值得安裝的技能之一，可以把它想像成高強度對齊＋文件生成器，這個 skill 功能跟 `/grill-me` 相同，但比他更強大，所以擇一安裝即可

這個 skills 的功能是：我們丟一個需求進去，它不會急著幫你寫程式，而是開始瘋狂問問題，問到沒有問題為止。然後它會一邊問，一邊幫你產出幾個關鍵文件：

- `CONTEXT.md` ：一份專案專用的 **術語／共享語言表**  
	這裡會把你在對話中用到的概念，整理成清楚、短而精準的名詞。像 Readme 裡舉的例子：
	- Before：  
		「當課程裡某一個章節的某一堂課被變成『真正的』課程（在檔案系統有實體位置）時會出問題」
		- After：  
		「materialization cascade 出問題」
	從此之後，AI 看到「materialization cascade」這幾個字，就知道你說的是前面那整段描述
- ADR (Architectures Decision Records)：架構決策紀錄  
	記錄那些「難以回頭、需要上下文解釋、而且是多個方案中做過取捨」的架構決策，包括最後選了什麼方案？為什麼這樣選？捨棄了哪些替代做法？讓未來的我們在改需求時，能看得懂當初的思考邏輯

這個 skill 就是在處理前面提到的兩個問題： **理解不對齊 + 沒有共同語言**

此外，它是我們後面整個工作流的的基礎，其他 skills 都會吃 `CONTEXT.md` 和 ADR 當上下文，所以推薦必裝

### 2\. 用 tdd 幫 AI 強制踩煞車

`/tdd` 則是把經典的 test‑driven development 流程，包裝成一個可以丟給 AI 執行的 skill

流程大致是：

1. 從 issue 或需求出發，先整理要達成的行為
2. 寫一個會失敗的測試
3. 寫最小的實作，讓測試通過
4. 重構、清理

這件事聽起來很老派，但在 AI 開發裡意義特別不同：它不只是「先寫測試」，而是 **強迫 AI 把工作拆成一個個的小步驟，每一步都要有明確的驗證信號**

如同之前所說的，matt 設計的 skills 都可以獨立運作，所以如果你希望 AI 能寫好 TDD，就可以在寫 tdd 時單獨使用這個 skill 協助你

### 3\. diagnose 幫助你正確的 debug

遇到 bug 的時候， `/diagnose` 提供的是一個有節奏的 debug 迴圈，而不是直接請 AI 看看為什麼壞掉，這種比較虛無飄渺的問題

它會帶你跑一套固定的步驟：重現、收集線索、提出假設、加上觀測／log、驗證、最後做回歸測試。也就是把比較成熟的除錯習慣包起來，讓 AI 在這個框架下思考，而不是一次就給你一長串的可能是 X、也可能是 Y、也有可能是 Z... 等等我再想想... 之類的 XXD

### 4\. improve-codebase-architecture 幫你的 codebase 體檢

`/improve-codebase-architecture` 比較像是定期健檢，作者建議每完成一個里程碑、或覺得架構有點散時，就讓它掃一次整個專案，他會幫助你做以下事情：

- 用你在 `CONTEXT.md` 中定義的共享語言來理解你的模組邊界
- 掃過程式碼，找出可以重新設計的模組
- 給出幾個具體的重構建議，讓你選擇要不要進一步展開

這個 skill 解決的就是第四個混亂的熵增問題，當 AI 寫爛(?)程式碼時，可以用這個 skills 把它拉回來重新對齊 XD

---

## 其他的 Skills 介紹

除了上面幾個推薦的 Skills 外，以下是在日常工作流也推薦使用的：

- `to-prd` ：把你和 AI 的對話整理成一份 PRD，直接丟進 issue tracker，解決了聊了很多，但沒有正式文件的問題
- `to-issues` ：從 PRD 自動拆成多個 issue，順帶幫你打上像 `feature` 、 `bug` 、 `ready-for-agent` 這樣的標籤
- `grill-me` ： `grill-with-docs` 的非程式版，適合策略討論、產品構想這種比較抽象的主題
- `caveman` ：讓 AI 用極度壓縮的語句說話，保留技術準確度，但至少省掉廢話和 token
- `handoff` ：幫你把現在這個對話收斂成一份交接文件，丟給下一個 agent 或下一輪 session 接著做

---

## 該怎麼跟 Superpowers 整合

前面一直在講 mattpocock 這包 Skills，其實還有一個很有趣的問題，如果我已經在用像 [superpowers](https://github.com/obra/superpowers) 這種大型的 workflow，那要如何一起用，而不是互相打架？

先簡單對齊他們的定位：

- **Superpowers** 比較像是一條龍，從「需求 → 設計 → 拆任務 → TDD 實作 → review → merge」完全滿足，強調的是 **一整套一致的流程**
- **Skills for Real Engineers** 比較像是一個軟體工具箱，強調的是 **關鍵環節的品質與軟體工程** ，像是 grill-with-docs、tdd、diagnose、improve-codebase-architecture.... 這些

如果兩個加在一起，我會這樣進行：

### 1\. 在 Superpowers 前面，加一層 grill-with-docs 做對標

Superpowers 的 `brainstorming` 本來就會幫我們把需求問清楚，並整理成設計文件，但它的重點是「把 spec 挖出來」->「切成可執行的 plans」，跟 `grill-with-docs` 這種瘋狂問問題的能力，還是有點不同

所以先讓 **`grill-with-docs`** 當前置關卡，確保大家的意見與目標一致：

- 有新需求時，先在專案裡跑一次 `grill-with-docs` ，用它瘋狂問問題的能力，幫我們達到以下兩件事：
	- 長出 `CONTEXT.md` 的共享術語表
		- 把關鍵設計選擇記成 ADR
- 再交給 Superpowers 進行 `brainstorming` 、 `writing-plans`

這樣一來，Superpowers 在幫規劃與拆任務時，就有一份穩定的專案語言和決策背景可以參考，不用每次都從 session 對話了解起

### 2\. 讓 Superpowers 接手大流程，在實作細節時用 matt 的 TDD 觀念加強

Superpowers 自帶的 `test-driven-development` 比較像是一個強制大家都先寫測試的關卡，搭配它自己的 `writing-plans` 、 `executing-plans` 、 `subagent-driven-development` ，可以自動跑很長一段時間

這層我會讓它照跑，不用硬插手，但我會在這之間加一點實作的細節給他，例如當 Superpowers 跑到實作階段時，我會刻意要求他：

- 測試盡量用專案 `CONTEXT.md` 裡的用語命名
- 優先寫像規格的 integration-style 測試
- 以一測一實作的 vertical slice 心態調整 plan 的顆粒度，防止次寫一整片假規格的測試，超無用....

我們甚至可以在呼叫 skill 指令的同時補上一句：「在實際寫測試與實作時，遵守 mattpocock/skills 的 `tdd` 準則」，讓 Superpowers 的 TDD 階段更偏向行為導向，而不是只追求有測試就好

### 3\. 迭代前後給程式做個體檢

Superpowers 很擅長把事情一路做完，但對長期演化的 codebase 的整體健康度，比較少關心與插手，所以這塊剛好可以讓 Matt 的 Skills 協助幫忙

- **遇到棘手的 bug 時，用 `diagnose` 接手**  
	平常開發我會繼續用 Superpowers，但如果真的碰到解不開的 bug，就暫時離開這一條流程，然後使用 `/diagnose` (reproduce → minimize → hypothesize → instrument → fix → regression-test)，專注把問題處理好，再回到原流程
- **每個迭代結束前，跑一次 `improve-codebase-architecture`**  
	當 Superpowers 在 branch 裡完成一整串任務、準備收尾前，可以用 `/improve-codebase-architecture` 掃一遍程式碼，看看有沒有可以優化或重構的部分

### 4\. 理解 SKills 的差別，掌握一個心態

從上述的整理可以知道，Superpowers 掌控的是一個大框架的流程，matt 的 skills 則是穿插在這個框架中的各個 plugins，可獨立運作，也可以整合到每個流程中

有了這樣的思維，我們就不會被任何一個大框架給控制住，我們可以維持開發者對自己設計與架構的主導權，同時又讓 agent 穩定的在這個框架下執行，而不會走偏

---

## 小結

這份 Skills - Skills For Real Engineers 讓我理解到，AI 的加速，除了加速寫 Code 之外，可能還會加速我們做錯事情；但同時透過 AI，也可以幫我們把對的習慣變得更容易堅持

而 matt pocock 他並沒有選擇把人類工程師從整個流程中摘除，而是把所謂「好的工程習慣」拆成一個獨立的、小的 skill，讓 AI 幫我們守住這個底線

如果你也覺得現在各種 AI 開發框架都太重，或是有那種寫得很快，但總覺得哪裡不對的違和感？那這個 repo 很值得抽時間打開來讀一讀，也推薦把幾個核心 skill 裝到你自己的工作流中，慢慢長成一套只屬於你的 AI 開發流程

muki@journal ~/AI 應用 $ 0