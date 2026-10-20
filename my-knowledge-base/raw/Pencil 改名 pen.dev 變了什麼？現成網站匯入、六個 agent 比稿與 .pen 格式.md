---
title: "Pencil 改名 pen.dev 變了什麼？現成網站匯入、六個 agent 比稿與 .pen 格式"
source: "https://rar.design/posts/pen-dev-website-import-agentic-canvas"
author:
  - "[[設計師 Riven]]"
published: 2026-09-23
created: 2026-09-24
description: "pen.dev 就是原本的 Pencil，9 月 17 日推出的新版主打把任何網頁匯入成可編輯圖層，改完再推回程式碼。桌面版可用 ChatGPT 或 Claude 訂閱登入，最多六個 agent 平行出稿，設計檔是能放進 Git 的 JSON。軟體目前免費，但還沒有即時共編與 prototype。"
tags:
  - "clippings"
status: "inbox"
---
網頁抓進畫布變成可編輯圖層、ChatGPT 與 Claude 訂閱直接登入、設計檔放進 repo 給 agent 讀：新版的功能、限制與適合誰

![設計師 Riven](https://s.teachifycdn.com/image/width=128,quality=80,format=webp/lecturer/avatar/fd0b78a9-03c9-4ad3-ae0a-a8965aa676ea/d822e5e9-519b-4f2e-b049-807272104edd.png)

[設計師 Riven](https://rar.design/@riven)

2026年9月23日 下午 3:05

AI 設計

AI TOOLS · 2026

![pen.dev 桌面版介面：左側 Agent 面板與模型選單，畫布上是一張咖啡烘焙 App 儀表板](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/8f82edb1-29b7-4553-8408-2cfc5b80a945/fdbaba38-b01f-48a6-8653-bb3a23b8d75a.png)

pen.dev 桌面版介面：左側 Agent 面板與模型選單，畫布上是一張咖啡烘焙 App 儀表板

▲ pen.dev 桌面版：左側是 Agent 面板，輸入框下方是閃電圖示的平行 agent 數量與模型選單（圖中選的是 Claude Opus 5），畫布上是一張 390 × 847 的咖啡烘焙 App 儀表板。

QUICK ANSWER

pen.dev 是改名後的 Pencil，目前免費。9 月 17 日新版主打三件事：網頁匯入成可編輯圖層；用 ChatGPT 或 Claude 訂閱登入，最多六個 agent 平行出稿；設計檔.pen 是 JSON，Claude Code 能用 MCP 讀寫。還沒有即時共編與 prototype。

設計工具打開的第一個畫面，一直都是一張白紙。就算手上的案子是改一個已經上線的網站，流程也差不多：截圖、貼進來、照著重畫一遍，才輪得到真正的設計。

pen.dev 9 月 17 日推出的新版，想拿掉的就是重畫這一步。在 app 裡打開一個網址，整個頁面會變成可以直接改的圖層，改完再交給 agent 推回程式碼。它就是我 [二月寫過的 Pencil](https://rar.design/posts/pencil-dev-ai-design-canvas) ，網址從 pencil.dev 換成 pen.dev，這半年補上了網站匯入、多模型和一整套畫布功能。

這篇把官方文件、示範影片和創辦人這半年的更新對過一輪，回答一個問題：pen.dev 到底猛在哪、還差在哪。會講到網站匯入、模型接法、.pen 格式、跟 Figma 的分工，以及它現在還做不到的事。

我在設計圈快十年，教設計工具的學員累積上萬。看新工具，我最在意的是它能不能接進設計師本來的工作，demo 炫不炫排在後面。

CONTENTS

01

[從 Pencil 到 pen.dev：這半年多了什麼](#chapter-1)

02

[不從白紙開始：把現成網頁抓進畫布](#chapter-2)

03

[用手上的訂閱，讓六個 agent 同場比稿](#chapter-3)

04

[.pen：寫給 agent 讀的設計檔](#chapter-4)

05

[跟 Figma 怎麼分工](#chapter-5)

06

[現在還做不到的事](#chapter-6)

07

[重點整理](#chapter-7)

FAQ

[常見問題](#faq)

CHAPTER 01 · WHAT CHANGED

## 從 Pencil 到 pen.dev：這半年多了什麼

先把名字釐清。pen.dev 就是 Pencil，開發商一樣是 High Agency，舊網址 pencil.dev 現在會直接轉到新網域。去年 9 月底它以 IDE 裡的 MCP 畫布亮相，今年 1 月底正式上線，2 月破 10 萬用戶，同時推出讓多個 agent 平行設計的 [SWARM Mode](https://rar.design/posts/pencil-swarm-mode-ai-design-update) 。

轉折在 7 月。創辦人 Tom Krcha 宣布 pen.dev 是 pencil.dev 的進化版，把主要的模型和 agent 全部接上，還說不少情況下 token 用量減了一半。早期的 Pencil 主要搭配 Claude Code 使用，當時幾篇評測都提醒要另外準備 Claude Code 訂閱；現在你手上有哪家的訂閱，登入就能用。

9 月 17 日晚上（台灣時間），他發布了新版，這次的定位是 agentic canvas：任何網站、web app 都能匯入畫布，迭代完推回程式碼。在那之前，官方頻道已經陸續放出網站匯入、Chrome 擴充、shader 特效和畫布效能的示範片，這次是把它們收成一個完整的版本。

TIMELINE · 2025—2026

2025.09

首次亮相

IDE 裡的 MCP 設計畫布，開放分批邀請

2026.01

正式上線

桌面版與 Cursor 擴充

2026.02

破 10 萬用戶

SWARM Mode 多 agent 平行設計

2026.07

改名 pen.dev

接上各家模型，訂閱帳號直接登入

2026.09

新版上線

網站匯入、Chrome 擴充、畫布效能改善

CHAPTER 02 · START FROM A PAGE

## 不從白紙開始：把現成網頁抓進畫布

新版最值得試的就是網站匯入。桌面版上方有個地球圖示，點開是內建的瀏覽器，輸入網址就能載入頁面，連本機開發中的 localhost 也行。要整頁就直接按 Import；只要某個區塊，就先切到選取元素模式點它，再匯入。匯入前可以調縮放和裝置尺寸，決定要抓桌面版還是手機版的樣子。

不想在 app 裡開網址，也可以裝官方的 Chrome 擴充 pen.dev web import，在平常用的瀏覽器裡複製網頁，回到畫布貼上，就是一層層可以編輯的圖層。只需要一張參考圖的話，同一個瀏覽器面板也能直接截圖，整頁或可見範圍都行。

WORKFLOW · 新版主打的三段流程

STEP 01

匯入

網頁或單一元素變成圖層

→

STEP 02

迭代

手動調整，或請 agent 改

→

STEP 03

推回程式碼

匯出 HTML，或讓 agent 改進專案

官方這支示範片拍的就是網站匯入，建議先看再往下讀。

![](https://www.youtube.com/watch?v=maZsalS3vao)

有三件事要先知道。匯入的圖層不會帶著網頁的 JavaScript 互動，選單和動畫要自己重接；字型和特效要對照原頁檢查一次；登入後才看得到的頁面，匯入前先確認畫面上沒有私人資料。

官方介紹這個功能時，說法是匯入靈感，或你自己擁有的線上網站。拿自家產品、客戶委託的網站來改版，是它最自然的用途；別人的網站抓進來當參考可以，改一改當成自己的作品交出去，侵權風險要自己扛。

網頁轉圖層，Figma 靠外掛也做得到。pen.dev 的差別在它接得起前後兩段：匯入的圖層就放在 agent 讀得懂的檔案裡，改完能直接交回程式碼。改版案的前一兩天，常常耗在把線上畫面重建成設計檔，這段時間現在可以直接拿來討論怎麼改。接案設計師提案時，也能拿客戶現在的網站，直接在上面畫改版方向。

“

設計師手上的案子多半是改版，設計工具卻一直從一張白紙開始。

CHAPTER 03 · ANY MODEL

## 用手上的訂閱，讓六個 agent 同場比稿

模型的部分，官方文件列出的連線選項有 15 種。OpenAI 可以用 ChatGPT Plus／Pro 帳號登入，Anthropic 可以用 Claude Pro／Max 登入，Google Gemini、xAI、DeepSeek、Kimi 等走 API key，也能接 GitHub Copilot、Cursor 或自訂端點。你已經付錢的那份 AI 訂閱，現在也能拿來畫介面。

6

同時出稿的 agent 上限

15

模型連線選項

$0

目前的軟體費用

輸入框下方的閃電圖示是平行 agent，一次開一到六個，分工有兩種。Split Work 讓它們各做任務的不同部分，例如一個做首頁、一個做定價頁；Side by Side 讓它們做同一題，每個 agent 可以選不同模型。另一個叫 Let it cook 的選項，會在第一版之後接著做出二到六個變體，可以指定保留風格只換版面，或換一整套視覺風格。

Side by Side 有個很實用的用法：拿它當模型評測台。同一個 brief 分給 Claude、GPT、Gemini 各一個 agent，三張稿並排在畫布上，哪個模型最抓得到你要的調性，看一眼就知道。Tom Krcha 7 月那波更新，示範的正是四家模型同時在畫布上設計。代價是用量：每多一個 agent、多一輪變體，就多一份模型請求，比稿很過癮，額度也燒得快。

agent 也吃 SKILL.md。在輸入框打斜線可以叫出內建的設計技能，也能把自己寫的 SKILL.md 資料夾加進選單。已經在用 [Claude Skills](https://rar.design/posts/claude-skills-for-designers) 整理品牌規範的人，同一份規則可以直接搬過來。

CHAPTER 04 · DESIGN AS FILE

## .pen：寫給 agent 讀的設計檔

.pen 是一份 JSON，描述一棵物件樹，結構跟 HTML 很像：每個物件有自己的 id 和類型，子物件用類似 flexbox 的規則排版，標記成 reusable 的物件就是元件。格式規格完整公開在官方文件，檔案可以跟程式碼放在同一個 repo，用 Git 管版本、看差異、開分支。

讓 agent 動這份檔案有三條路。第一條是桌面版內建的 agent。第二條是 [MCP](https://rar.design/posts/mcp-model-context-protocol-guide) ，在設定的 MCP 分頁勾選之後，Claude Code、Codex、Gemini CLI、Claude Desktop 這些外部工具就能直接讀寫畫布，它們的連線清單裡顯示的還是舊名字 pencil。第三條是命令列工具，一行指令就能生成、修改設計或匯出圖檔，全程不開視窗。

$ npm install -g @pen.dev/cli

$ pen --out design.pen --prompt "Create a login page with email and password fields"

官方文件的命令列範例：安裝之後，一行指令產生一份登入頁設計（需要 Node.js 22.19 以上）。

![pen.dev 官方示意圖：Claude Code 終端機顯示 pen mcp 已連線，右側屬性面板有 HTML + Tailwind 匯出與 Import Web via Browser 按鈕](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/787c1aa5-cbbf-4e06-bb81-5b235281b963/2377bcbd-a38d-497e-850f-467ace750ad6.png)

pen.dev 官方示意圖：Claude Code 終端機顯示 pen mcp 已連線，右側屬性面板有 HTML + Tailwind 匯出與 Import Web via Browser 按鈕

▲ 官方示意：左邊是 Claude Code 的終端機，底部顯示 pen mcp 已連線；右側屬性面板的 Code 區選的是 HTML + Tailwind，可以複製或匯出 HTML，下方就是網站匯入的入口 Import Web via Browser。

設計和程式碼之間可以雙向走。選取圖層，在屬性面板的 Code 區選 HTML + Tailwind 或 HTML + CSS，直接複製或匯出；要 React、Next.js 或 shadcn/ui 元件，就請 agent 照你專案的框架產生。專案裡已經寫好的元件，也可以請 agent 反向畫回畫布，CSS 變數和 pen.dev 的 Variables 也能互相同步。

CHAPTER 05 · COMPARED TO

## 跟 Figma 怎麼分工

先看畫布本身。元件、Variables、Slots、跨檔案元件庫、類似 Auto Layout 的 flex 排版、鋼筆工具、mesh 漸層、即時 shader 特效都有了，Figma 和 Sketch 的快捷鍵照用，.fig 檔可以整份匯入。做一般的網頁與 App 畫面，工具本身已經夠用。

![pen.dev 官方示意圖：1440 × 900 畫框套用 thermal-eclipse 效果，右側為屬性面板](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/6f71856d-cccb-445a-901d-cd0a8d2df294/ca4a7876-dae1-4034-9ab3-137a8304aaa2.png)

pen.dev 官方示意圖：1440 × 900 畫框套用 thermal-eclipse 效果，右側為屬性面板

▲ 官方示意：檔名叫 Shader 的 1440 × 900 桌面畫框，Fill 欄套用的是 thermal-eclipse 效果，右側一樣有 HTML 匯出與圖層匯出。

真正的差別在路線。Figma 這一年也在做 AI 設計，3 月開放 [use\_figma](https://rar.design/posts/figma-mcp-write-to-canvas-setup-guide) ，讓 Claude Code 這類外部 agent 寫進 Figma 畫布，後來又推出內建的 Figma Agent，方向都是把 agent 請進 Figma。pen.dev 反過來，把設計檔做成 agent 本來就讀得懂的格式，放進 agent 已經在工作的 repo。

Tom Krcha 在 Peter Yang 的訪談裡，把.pen 形容成「agentic PDF」：一份跟平台無關的設計描述，想轉成哪種程式碼都可以。設計師 Roger Wong 看完那集的評語也值得記著：設計系統成熟的團隊，真正的依據是程式碼裡的元件，JSON 只是它的一份描述。

協作的想法差得更遠。Figma 的核心是多人同時在一個檔案裡工作；pen.dev 的協作靠 Git 和分享連結。Tom Krcha 受訪的一集捷克語 podcast，節目標題下的是「多人協作已死，新的時代屬於會用 AI 的設計工程師」。看得出他押的使用者，是一個人帶著一群 agent、把設計一路做到上線的人。

COMPARISON · 兩條路線對照

項目

Figma

pen.dev

設計的起點

空白畫布、元件庫

也能從現成網頁、.fig 檔匯入

AI 怎麼進來

Figma Agent、use\_figma 寫入畫布

內建 agent、外部 MCP、命令列

檔案存在哪

Figma 雲端

本機.pen（JSON），可進 Git

團隊協作

即時共編、留言、Dev Mode

Git 與分享連結

原型

內建 prototype

文件未提供，靠簡報模式或匯出 HTML

價格

免費方案＋付費座位

目前免費，模型費用自付

所以分工很清楚。設計師主導、需要跟 PM 和客戶在同一個檔案裡來回、需要 prototype 驗證流程的團隊，Figma 還是主場。一個人包辦設計和前端、手上多是網站改版、已經在用 Claude Code 或 Codex 的人，pen.dev 現在就值得排進工作流。兩邊也能並用，.fig 檔匯得進來，Figma 可以繼續當團隊討論的那一份。

CHAPTER 06 · LIMITS

## 現在還做不到的事

把限制攤開來看，導入時比較不會踩雷。

團隊協作

官方文件裡找不到即時共編與留言的章節。協作靠 Git 和分享連結，拿到連結的人可以在瀏覽器打開最新版本。

原型

文件沒有 prototype 功能的說明。要展示流程，目前靠 Slides 簡報模式，或直接匯出 HTML 給人點。

Figma 匯入

複製貼上 Figma 圖層時，圖片元素貼不過來，要整份.fig 匯入才會帶圖，或之後手動補。

格式與元件庫

官方明寫保留.pen 格式破壞性變更的權利；檔案一旦轉成元件庫（.lib.pen）就不能復原，正式專案先開副本試。

費用

軟體目前免費，官方說未來可能推出付費方案，收費前會先說明。AI 用量另外算在你的訂閱或 API 帳單上。

CHAPTER 07 · TAKEAWAYS

## 重點整理

TAKEAWAYS · 一分鐘懶人包

→

pen.dev 就是 Pencil，7 月改名，9 月 17 日推出新版，軟體目前免費。

→

網站匯入是這次最值得試的功能：網頁或單一元素直接變成可編輯圖層，但不帶 JavaScript 互動。

→

桌面版可用 ChatGPT 或 Claude 訂閱登入，最多六個 agent 平行出稿，也能拿來比較哪個模型最懂你的調性。

→

.pen 是公開規格的 JSON，能進 Git，Claude Code、Codex 透過 MCP 就能讀寫。

→

缺的是團隊那一塊：即時共編、留言、prototype 都還沒有。

→

最適合一個人包辦設計和前端、手上多是改版案的人；設計團隊先拿一個案子試水溫。

回到最開始的問題：pen.dev 猛不猛？它猛在把設計的起點從白紙換成現成的產品，終點直接落在程式碼，中間用哪家模型由你決定。還差的是團隊協作，那是 Figma 花了快十年累積的東西。趁它還免費，最划算的試法很簡單：把你自己的網站匯入一次，看看改版的第一天能省下多少時間。

RELATED READS · 延伸閱讀[Figma Agent 是什麼？直接在 canvas 上動手的設計 AI 完整解析與用法](https://rar.design/posts/figma-agent-complete-guide)

[

Figma 把 agent 請進畫布的那條路

→](https://rar.design/posts/figma-agent-complete-guide)[

Claude Code /design 還是 Claude Design？終端機直接生 UI 畫板的新指令完整解析

同樣在終端機裡生畫板，Anthropic 的做法

→](https://rar.design/posts/claude-code-design-command-guide)[

Claude Design 怎麼用？從一句話做出互動網頁的完整教學

不開畫布，一句話做出互動網頁

→](https://rar.design/posts/claude-design-web-guide)[

Google Stitch 教學：用 AI 生成 UI 設計的免費工具，完整功能介紹與操作指南

另一個免費的 AI 出稿工具，瀏覽器就能用

→](https://rar.design/posts/google-stitch-ai-ui-design-tutorial)[

Claude Code 怎麼做出沒有 AI 感的網站？三步驟把 AI 調教成懂品味的設計師

讓六個 agent 比稿之前，先把品味寫成規則

→](https://rar.design/posts/claude-code-design-taste-workflow)![AI 覺醒設計應用攻略](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/63e96ac9-97ce-4dfc-be50-154f731f1cc9/e5ca45f0-206a-478f-af1a-96960be47d6a.jpg)

AI 覺醒設計應用攻略

[

SUBSCRIPTION

AI 覺醒設計應用攻略

AI 工具每週都在更新，我幫你過濾雜訊，只留對設計師真正有用的應用。

了解訂閱 →](https://rar.design/courses/aiwaken)

## 常見問題

### Q：pen.dev 跟 Pencil 是同一個工具嗎？

A：同一個。開發商都是 High Agency，舊網址 pencil.dev 會轉到 pen.dev，設計檔一樣是.pen。改名之後接上了更多模型，也加入網站匯入與 Chrome 擴充，現有用戶直接更新 app 就好。

### Q：pen.dev 要錢嗎？

A：軟體本身目前免費，官方說未來可能推出付費功能，收費前會先公布條款，模型選單裡也已經出現標示 pen.dev Pro 的模型。AI 用量另外算，走你自己的 ChatGPT、Claude 訂閱或 API key，平行 agent 開越多，用量越多。

### Q：不會寫程式能用嗎？

A：可以。桌面版內建 agent，用 ChatGPT Plus／Pro 或 Claude Pro／Max 帳號登入就能開始畫。終端機和 MCP 留給想接進程式工作流的人，不碰也能用。

### Q：可以把別人的網站匯入來改嗎？

A：技術上可以，但官方的說法是匯入靈感，或你自己擁有的網站。當參考沒問題，拿別人的版面、圖片、文案改一改當成作品交出去，就有侵權風險。登入後才看得到的頁面，匯入前也要先確認沒有私人資料。

### Q：pen.dev 能取代 Figma 嗎？

A：看團隊。它沒有即時共編、留言與 prototype，設計師主導、需要跟 PM 和客戶來回討論的團隊，Figma 還是主場。一個人包辦設計和前端、手上多是網站改版的人，現在就能直接用。.fig 檔可以整份匯入，兩邊並用也沒問題。

### Q：支援哪些平台？

A：桌面版有 macOS（Apple Silicon 與 Intel）、Windows x64 與 Linux，Windows ARM 版標示即將推出。另外有 Cursor、VS Code、Antigravity、Windsurf 的擴充，需要 Node.js 22.19 以上的命令列工具，以及網頁匯入用的 Chrome 擴充。