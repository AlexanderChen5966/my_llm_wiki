---
title: "Godot 遊戲開發教學：結合 Claude Code 從零開始做出能玩的遊戲"
source: "https://aiqkangber.com/blog/godot-game-development"
author:
  - "[[Q kangber]]"
published: 2026-07-30
created: 2026-09-24
description: "Godot 遊戲開發教學：用 Claude Code 從零做出能玩的遊戲，引擎怎麼選、怎麼接上 Godot、提示詞怎麼下，附實際卡住的地方與解法。"
tags:
  - "clippings"
status: "inbox"
---
## 前言

沒寫過程式、又想做出一款自己的遊戲，現在用 **Claude Code** 加一套免費遊戲引擎就辦得到。這篇 **Godot 遊戲開發教學** 從引擎怎麼選、怎麼接上 **Godot** 、提示詞怎麼下一路講起，我自己就是用這套做法做出一款能連線對打的 3D 戰棋卡牌遊戲，過程中真的卡住的地方也一起整理在這篇。

這種不寫程式碼、用講的把東西做出來的做法，現在叫 [Vibe Coding](https://aiqkangber.com/blog/vibe-coding) ，做遊戲只是它其中一條路。

![Claude Code 開發遊戲教學封面圖：用 AI 搭配 Godot 引擎製作 2D 3D 遊戲](https://aiqkangber.com/blog-images/nNXh08QW-cover.webp)

Claude Code 開發遊戲教學：不會寫程式也能做出能玩的遊戲

## Claude Code 和遊戲引擎，各自負責什麼

動手之前要分清楚兩個角色。 **遊戲引擎** （Godot、Unity 這些）是遊戲真正跑起來的地方，畫面、角色、關卡都長在裡面； **Claude Code** 是坐在旁邊的助手，照你講的話去改引擎專案裡的檔案。分工搞清楚，後面每一步才知道自己在做什麼。

### Claude Code 是動手改檔案的助手，不是遊戲設計師

你把玩法用一句話講給它聽，它照著去建場景檔、寫控制角色的程式、把按鍵跟動作接起來。它不會自己決定「這款遊戲要怎麼設計才好玩」，那是你的工作。你講得越具體，它做出來的東西越接近你腦中的畫面；你只丟一句「幫我做個遊戲」，它就只能亂猜一個方向，做出來的多半不是你要的。

📖 **延伸閱讀** ： [Claude Code 怎麼接 GitHub？從裝 Git 到推上第一個專案](https://aiqkangber.com/blog/claude-code-github)

### 遊戲引擎才是畫面真正跑起來的地方

Claude Code 改的是專案資料夾裡的檔案，改完你要回到引擎按下執行，才看得到結果。這個來回會貫穿整個開發過程：你描述需求、它改檔案、你按執行試玩、再把試玩發現的問題講給它聽。它看不到遊戲跑起來的畫面，試玩這件事只有你能做。

![Claude Code 與遊戲引擎分工架構圖：提示詞、改專案檔案、執行試玩、回報問題的循環](https://aiqkangber.com/blog-images/VYy02t2R-arch-roles.webp)

你講需求、Claude Code 改檔案、你按執行試玩，整個開發就是這個循環

## Unity 還是 Godot？新手第一款遊戲引擎怎麼選

這是開工前最多人問的問題。兩套引擎我都碰過，結論放前面：想用 AI 幫你做遊戲、又是第一次碰遊戲開發，選 Godot；已經會 Unity 的人留在 Unity，不用特地搬家。差異列成表：

| 比較項目 | Godot | Unity |
| --- | --- | --- |
| 費用 | 完全免費、開源 | Personal 免費版限年營收 20 萬美元以下，超過要買 Pro |
| 專案檔格式 | 純文字，AI 直接讀得懂 | 大量二進位與代號參照，AI 難以直接閱讀 |
| 硬體需求 | 安裝檔不到 100MB，舊筆電跑得動 | 編輯器吃資源，舊電腦開起來吃力 |
| 教學資源與外掛生態 | 成長中，量比 Unity 少 | 累積十幾年，商業遊戲成熟度高 |

### Godot 的專案檔是純文字，Claude Code 打開就讀得懂

這是我選 Godot 示範的最大原因。棋子怎麼走、卡牌費用與效果，我都先整理成一份份純文字的規則清單，遊戲邏輯再讓 Claude Code 寫成程式。這些清單打開來跟讀一篇文章沒兩樣，看得懂規則寫在哪個系統上，要改哪裡就直接改。檔案格式對 AI 友善，是 Godot 在 AI 遊戲開發這件事上最實在的優勢，跟引擎本身誰強誰弱無關。

### 已經會 Unity 的人，留在熟悉的引擎就好

Unity 在 3D 畫面表現、正式商業遊戲的成熟度、外掛跟教學資源的量，都比 Godot 豐富。如果你本來就熟 Unity 的操作，為了 AI 搬去 Godot 反而把累積的經驗歸零，不划算。花時間把 Claude Code 接上 Unity 的環境設定好，一樣能讓它幫你寫 C# 腳本，只是要花的設定工夫比 Godot 多。

📖 **延伸閱讀** ： [2026 AI 寫程式 10 款 AI Coding 工具比較，新手、工程師、創業者怎麼選？](https://aiqkangber.com/blog/ai-coding-tools)

## 怎麼接上 Godot：下載安裝到讓 Claude Code 直接動手

這一步沒有想像中複雜，因為 Godot 專案本身就是給文字編輯器讀的格式，不需要另外裝任何橋接工具或外掛。整個接法就是下面三步。

<svg viewBox="0 0 1024 768" style="width:100%;height:auto;display:block" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="接上 Godot 的三步驟：下載安裝 Godot、在專案資料夾開啟 Claude Code、回編輯器按執行試玩" font-family="'Microsoft JhengHei','PingFang TC',sans-serif"><defs><linearGradient id="godot-frame" x1="0" y1="0" x2="1" y2="1"><stop offset="0"></stop><stop offset="1"></stop></linearGradient><g id="ic-dl"><path d="M40 14 v40 m-22 -22 l22 22 22 -22"></path><path d="M14 60 h52 v6 a4 4 0 0 1 -4 4 h-44 a4 4 0 0 1 -4 -4 z"></path></g><g id="ic-folder-ai"><path d="M12 24 h20 l6 8 h34 a4 4 0 0 1 4 4 v26 a4 4 0 0 1 -4 4 h-56 a4 4 0 0 1 -4 -4 v-34 a4 4 0 0 1 4 -4z"></path><path d="M40 40 l4 9 9 4 -9 4 -4 9 -4 -9 -9 -4 9 -4z" stroke="none"></path></g><g id="ic-play"><rect x="12" y="18" width="56" height="40" rx="6"></rect><path d="M32 64 h16 M40 58 v6"></path><path d="M34 30 l16 8 -16 8z"></path></g></defs><rect width="1024" height="768" rx="28" fill="url(#godot-frame)"></rect><rect x="22" y="22" width="980" height="724" rx="20" fill="none" stroke="currentColor"></rect><rect x="192" y="52" width="640" height="92" rx="46" fill="none" stroke="currentColor"></rect><text x="512" y="112" text-anchor="middle" font-size="46" font-weight="800" letter-spacing="2"><tspan fill="currentColor">接上 Godot</tspan> <tspan fill="currentColor">　只有這三步</tspan> </text><line x1="362" y1="210" x2="362" y2="560" stroke-width="3" stroke-dasharray="3 12" stroke-linecap="round" stroke="currentColor" stroke-opacity="0.2"></line><line x1="662" y1="210" x2="662" y2="560" stroke-width="3" stroke-dasharray="3 12" stroke-linecap="round" stroke="currentColor" stroke-opacity="0.2"></line><text x="212" y="252" text-anchor="middle" font-size="34" font-weight="800" fill="currentColor">1</text> <use href="#ic-dl" x="172" y="290"></use><text x="212" y="512" text-anchor="middle" font-size="30" font-weight="700" fill="currentColor">下載安裝 Godot</text> <text x="212" y="552" text-anchor="middle" font-size="22" fill="currentColor">免費、不用註冊</text> <text x="512" y="252" text-anchor="middle" font-size="34" font-weight="800" fill="currentColor">2</text> <use href="#ic-folder-ai" x="472" y="290"></use><text x="512" y="512" text-anchor="middle" font-size="30" font-weight="700" fill="currentColor">資料夾裡開 Claude Code</text> <text x="512" y="552" text-anchor="middle" font-size="22" fill="currentColor">切到專案資料夾再打 claude</text> <text x="812" y="252" text-anchor="middle" font-size="34" font-weight="800" fill="currentColor">3</text> <use href="#ic-play" x="772" y="290"></use><text x="812" y="512" text-anchor="middle" font-size="30" font-weight="700" fill="currentColor">回編輯器按執行</text> <text x="812" y="552" text-anchor="middle" font-size="22" fill="currentColor">畫面對不對你自己看</text> <rect x="252" y="600" width="520" height="64" rx="32" fill="none" stroke-width="3" stroke="currentColor"></rect><circle cx="303" cy="632" r="15" fill="none" stroke-width="3" stroke="currentColor"></circle><path d="M303 624 v10 M298 638 h10" stroke-width="3" stroke-linecap="round" fill="none" stroke="currentColor"></path><text x="345" y="642" font-size="26" font-weight="700" fill="currentColor">不用裝外掛，也不用設定任何連線</text> <text x="512" y="712" text-anchor="middle" font-size="22" font-weight="800" letter-spacing="3" fill="currentColor">Q kangber</text></svg>

接上 Godot 的完整流程：裝好引擎、在專案資料夾開 Claude Code、改完回編輯器按執行

### 先下載安裝 Godot

到 [Godot 官方下載頁](https://godotengine.org/download) 抓對應作業系統的版本，解壓縮或安裝完就能開，不用註冊帳號、不用綁信用卡。開一個新專案，資料夾位置記下來，等一下要讓 Claude Code 開同一個資料夾。

### Claude Code 直接打開專案資料夾就能用

不用裝外掛、不用設定連線，你要做的只有一件事：讓 **Claude Code** 在「Godot 專案的那個資料夾」裡啟動。Windows 可以在專案資料夾的空白處按右鍵，選在終端機開啟；或是先開終端機，打 `cd` 空一格，再把整個 Godot 專案資料夾直接拖進終端機視窗，路徑會自動貼上去，按 Enter 就切過去了。位置對了之後打 `claude` ，它就在這個專案裡待命。

第一句話先跟它講清楚這個資料夾是什麼、之後要在這裡做什麼，它會自己把場景檔、程式檔、規則清單掃過一遍。路徑不用自己打，把 Godot 專案資料夾從檔案總管直接拖進輸入框，就會變成引號裡那一串：

💬 提示詞範例

\> 這是我的 Godot 專案資料夾「D:\\我的遊戲專案」，剛開好的空專案，我要在這裡做一款 3D 戰棋卡牌遊戲。先確認你讀得到這個資料夾、看得出 Godot 版本跟現在的專案設定，再跟我說要做出第一個能動的畫面你打算分成哪幾步。這一輪還不要改任何檔案。

剛開的空專案裡幾乎沒東西，它讀得到的是 Godot 的專案設定；等你做出畫面之後，資料夾裡的場景檔、程式檔、規則清單也全部是純文字，跟打開一份文件沒兩樣，它讀得懂、也改得動。最後那句「還不要改任何檔案」很重要：第一輪你要的是它把步驟列出來給你看，方向對了再放它動手，免得一上來就自作主張生一堆東西，你連它做了什麼都還沒搞懂。

確認它認得這個專案之後，你講需求它就直接改資料夾裡的檔案。它不會幫你按執行鍵，畫面到底對不對，還是要你自己回 Godot 編輯器按執行看。

### 我自己用一個指令別名，快速開 Godot 編輯器測試

因為 Claude Code 改完都要切回 Godot 編輯器按執行看結果，這個來回次數很多，我在自己電腦的指令設定裡加了一個捷徑：打這一個指令，幾秒內就直接開好 Godot 編輯器、指到這個專案，不用自己點資料夾、開軟體、選專案這一長串步驟。

這串設定不用自己會寫，把需求講給 Claude Code 聽，它會幫你改好設定檔：

💬 提示詞範例

\> 幫我在終端機的設定檔加一個捷徑，名字叫 gogodot。打這個指令就用 D:\\godot 底下的 Godot 執行檔開啟編輯器，直接載入 D:\\我的遊戲專案 這個資料夾，而且編輯器要獨立開一個視窗，不要佔住我的終端機。改完請告訴我怎麼讓設定生效。

講清楚「Godot 執行檔放在哪」「要載入哪個專案」「不要佔住終端機」這三件事，它一次就會設對；只丟一句「幫我加個快速開 Godot 的捷徑」，它得反問你路徑，或是自己猜一個錯的。名字取什麼都行，取成 `gogodot` 這種一看就知道在幹嘛的最好記。這種小捷徑省下來的時間，一天測十幾次就很有感，值得為常開的專案設定一個。

📖 **延伸閱讀** ： [Claude Code 30 個必學指令與使用情境](https://aiqkangber.com/blog/claude-code-commands)

## 提示詞怎麼寫，AI 才做得出你要的遊戲邏輯

引擎接好之後，真正決定你要改幾次才會對的，是提示詞的品質。一則能一次做對的提示詞，通常都有下面三個部分。

<svg viewBox="0 0 1024 768" style="width:100%;height:auto;display:block" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="好提示詞的三個部分：交代專案與規則放在哪、判斷條件一條條列出、限制這輪範圍並要求回報改了哪些檔案" font-family="'Microsoft JhengHei','PingFang TC',sans-serif"><defs><linearGradient id="prompt-frame" x1="0" y1="0" x2="1" y2="1"><stop offset="0"></stop><stop offset="1"></stop></linearGradient><g id="ic-rules"><rect x="18" y="14" width="44" height="52" rx="5"></rect><path d="M28 30 h24 M28 42 h24 M28 54 h14"></path></g><g id="ic-conds"><path d="M16 24 l6 6 10 -12 M16 44 l6 6 10 -12 M16 64 l6 6 10 -12"></path><path d="M44 26 h22 M44 46 h22 M44 66 h22"></path></g><g id="ic-scope"><rect x="10" y="18" width="44" height="40" rx="6"></rect><path d="M10 32 h44"></path><path d="M20 45 h16"></path><path d="M50 55 l6 7 14 -18"></path></g></defs><rect width="1024" height="768" rx="28" fill="url(#prompt-frame)"></rect><rect x="22" y="22" width="980" height="724" rx="20" fill="none" stroke="currentColor"></rect><rect x="182" y="52" width="660" height="92" rx="46" fill="none" stroke="currentColor"></rect><text x="512" y="112" text-anchor="middle" font-size="46" font-weight="800" letter-spacing="2"><tspan fill="currentColor">一則好提示詞</tspan> <tspan fill="currentColor">　該有的三個部分</tspan> </text><line x1="362" y1="210" x2="362" y2="560" stroke-width="3" stroke-dasharray="3 12" stroke-linecap="round" stroke="currentColor" stroke-opacity="0.2"></line><line x1="662" y1="210" x2="662" y2="560" stroke-width="3" stroke-dasharray="3 12" stroke-linecap="round" stroke="currentColor" stroke-opacity="0.2"></line><use href="#ic-rules" x="172" y="290"></use><text x="212" y="512" text-anchor="middle" font-size="30" font-weight="700" fill="currentColor">交代專案與規則</text> <text x="212" y="552" text-anchor="middle" font-size="22" fill="currentColor">它才不會自己編一套</text> <use href="#ic-conds" x="472" y="290"></use><text x="512" y="512" text-anchor="middle" font-size="30" font-weight="700" fill="currentColor">條件一條條列出</text> <text x="512" y="552" text-anchor="middle" font-size="22" fill="currentColor">漏寫的判斷它就不會做</text> <use href="#ic-scope" x="772" y="290"></use><text x="812" y="512" text-anchor="middle" font-size="30" font-weight="700" fill="currentColor">限制範圍並回報</text> <text x="812" y="552" text-anchor="middle" font-size="22" fill="currentColor">這輪只動一段、改完講清楚</text> <rect x="242" y="600" width="540" height="64" rx="32" fill="none" stroke-width="3" stroke="currentColor"></rect><circle cx="293" cy="632" r="15" fill="none" stroke-width="3" stroke="currentColor"></circle><path d="M293 624 v10 M288 638 h10" stroke-width="3" stroke-linecap="round" fill="none" stroke="currentColor"></path><text x="335" y="642" font-size="26" font-weight="700" fill="currentColor">多花三十秒寫清楚，少改三、四輪</text> <text x="512" y="712" text-anchor="middle" font-size="22" font-weight="800" letter-spacing="3" fill="currentColor">Q kangber</text></svg>

好提示詞的三個部分：先交代專案與規則，再把判斷條件列清楚，最後限制範圍並要它回報

### 先把規則一次講清楚，不要邊做邊補

動手之前，我先把棋子走法、卡牌效果整理成一份份規則清單，不是直接叫 Claude Code 憑空生一套規則。理由很實際：規則散在程式碼裡，改一個數值要請 AI 翻好幾個檔案，改完你也不容易確認有沒有漏掉哪裡。規則集中寫清楚之後，調平衡是改一個數字，AI 負責的是照規則把行為做出來，兩件事分開。你能講清楚的規則就寫成資料，講不清楚的才丟給 AI 猜。

比如一張會讓目標持續掉血的技能卡，我在規則裡先寫清楚它要花多少行動點打出、每回合扣多少血、持續幾回合、對已經中了效果的目標又會怎樣。這些先寫清楚，提示詞裡就不用再重講一次這張卡怎麼運作，Claude Code 讀規則就知道要寫什麼。

### 提示詞要多具體，這是兩種寫法的差別

提示詞含糊，Claude Code 只能亂猜一個方向。同樣是要它做「棋子移動」，這是隨手打的版本：

❌ 含糊的寫法

\> 幫我做棋子可以移動。

同一個需求，把專案背景、判斷條件、這一輪的範圍都交代清楚，變成這樣：

✅ 具體的寫法

\> 你現在改的是一個 Godot 3D 戰棋專案，棋子的走法規則寫在專案裡的規則清單，先讀那份清單再動手。這一輪要做的是棋子移動：

1\. 玩家點到自己的棋子時，把這顆棋子這一步走得到的格子標示出來。

2\. 能不能走要照規則清單判斷，要看棋子面向的方向，背後的格子不能走，路徑上被其他棋子擋住也不能走。

3\. 點到被標示的格子才移動，點其他地方就取消選取。

先只改移動這一段，其他系統不要動。改完告訴我你改了哪些檔案，我自己回編輯器試玩。

差別在三個地方：先講清楚 **這是什麼專案、規則放在哪** ，AI 就不會自己編一套走法；再把 **判斷條件一條條列出來** ，面向、擋路、取消選取都寫明，它才知道哪些判斷不能漏；最後 **限制這一輪只准動移動這一段，並且要它回報改了哪些檔案** ，你才知道要去看哪裡、出事也好退回。只丟「幫我做移動」，它做出來的多半是任何方向都能走、也不會被擋住的版本，你還得再回頭補一次需求。多花三十秒把條件寫清楚，省下的是來回三、四輪的修改。

📖 **延伸閱讀** ： [AI 提示詞怎麼寫？2026 該淘汰的舊指令與新寫法](https://aiqkangber.com/blog/prompt-engineering)

### 東西做錯了，回報的方式決定它抓不抓得到重點

我請它確認遊戲裡幾個機制到底有沒有做出來，它只去查某個資料欄位有沒有被程式讀到，查不到就回我「這幾個功能沒做」。實際上那段邏輯是直接寫在判斷式裡，功能一直都在，是我自己核對才發現它判斷錯了。要它確認某個機制在不在，得明講「去查實際行為」，不能讓它只看資料欄位是不是被用到，不然它推論出來的「沒做」常常只是「查錯地方」。回報問題也是同樣道理，別只丟一句「這裡壞了」，把「你做了什麼、預期看到什麼、實際看到什麼」講清楚，它抓對地方的機率會高很多。

## 實際案例：這套做法最後做出什麼

照上面這套流程，我用 Claude Code 搭配 Godot 做出一款 3D 戰棋加卡牌的手遊雛形，動工當天就有能對戰的版本，後來補上連線，兩個人可以隔著網路對打。

這款遊戲現在有瀏覽器版，不用安裝就能直接開來玩： [夜影傭兵團線上試玩](https://qkangber.itch.io/nightshade-mercenaries) 。首次會下載約 72MB，建議用電腦開。

### 快的是實作，慢的是想清楚

這不是「一句話生出一款遊戲」。棋子怎麼走、什麼算贏，是我自己先想清楚、寫進規則清單的，Claude Code 省下的是把這些想法翻成程式碼的時間，不是想清楚要做什麼的時間。這兩件事常被混在一起講，也是很多人照著做卻做不出東西的原因。

![Claude Code 搭配 Godot 做出的 3D 戰棋卡牌對戰畫面：立體棋盤、雙方血量、行動點與手牌](https://aiqkangber.com/blog-images/WJrMK1c-screenshot-battle.webp)

對戰畫面：血量、行動點、回合這些看得見的數字，都是規則清單裡的設定被實作出來的樣子

#### AI 做的東西正式上線，歡迎洽詢

[看 AI 網站開發服務](https://aiqkangber.com/services/web-development)

## AI 幫得上跟幫不上的地方

同樣一句話丟出去，有些事 Claude Code 一次就做對，有些事它再改幾輪也給不了你要的。分清楚這條線在哪，才不會把時間花在它本來就做不到的事情上。

### 扣血、費用、勝負條件這些規則，交給 AI 最省力

棋子被攻擊扣多少血、卡片費用怎麼算、什麼時候算贏，這些都是規則清單裡寫清楚的內容，跟前面那張持續掉血的技能卡同一個道理，Claude Code 讀規則就能寫出對應邏輯。分清楚眼前這件事是「規則」還是「美感」，就知道該交給 AI 還是自己動手。

一張攻擊卡我在規則裡寫的是「花 2 點行動點、攻擊力加 200、當回合只能打左前方、回合結束自己受 50 傷害、限定某一種棋子才能用」。這五個條件寫成清單交出去，畫面上的卡片說明、點數消耗、限制提示就全部長出來了，我沒有另外解釋任何一句。

![Godot 戰棋卡牌遊戲的卡片詳情畫面：費用、持續回合、效果敘述與可使用棋子限制](https://aiqkangber.com/blog-images/FqKvRmyT-screenshot-card.webp)

卡片上的費用、持續回合、效果敘述、使用限制，全部照規則清單長出來，規則寫得多細畫面就有多細

### 角色圖、音效這些素材要另外準備，Claude Code 不會生

遊戲裡的卡面美術是另外做好、再讓 Claude Code 接進專案的，不是它自己生出來的。Claude Code 是寫程式跟改檔案的工具，不會憑空畫一張圖或做一段配樂，這步常被忽略，以為講一句話連美術都會有，結果卡在素材不知道從哪來。

缺什麼就用對應的工具補，這幾樣都不用你會畫畫、會作曲：

- **卡面、角色圖、介面圖示** ：用 ChatGPT、Gemini 這類生圖工具做。同一款遊戲的圖要用同一組敘述去生，風格才不會每張都不一樣，生完再挑掉不能用的。
- **背景音樂、音效** ：用 Suno 這類 AI 音樂工具，把曲風跟情境講出來（例如緊張的回合制戰鬥背景樂、勝利結算的短音效），生幾首挑一首接進遊戲。
- **介面版面跟配色** ：Claude Code 自己排出來的介面通常很陽春，Claude Design 那種協同設計的做法可以讓 AI 先給你版型跟配色方案，你挑一個再往下做。我是在網頁專案用它，遊戲介面沒辦法整套照搬，但「AI 先出版型、你來拍板」這個用法一樣成立。

素材先準備好放進專案資料夾，再叫 Claude Code 接進去，順序反過來就會一直等一個它生不出來的東西。

![Godot 戰棋卡牌遊戲的卡牌圖鑑畫面：每張技能卡都有各自的卡面美術](https://aiqkangber.com/blog-images/RkQ5nY5j-screenshot-codex.webp)

圖鑑裡每張卡的美術都是另外準備好才接進遊戲的，Claude Code 負責的是讓它們照規則正確運作

📖 **延伸閱讀** ： [Claude Code 做出來的網頁充滿 AI 味？Claude Design 協同設計讓網站 UI/UX 質感飛躍](https://aiqkangber.com/blog/claude-design)

## Claude Code 和 Godot 一起用，真正卡住我的兩個地方

實際卡住我的不是「AI 寫不出程式」，是這兩個發生在它跟引擎之間的落差，先知道能少繞路。

### 它說改好了，畫面上其實沒有變

我請它把遊戲裡一個畫面的背景換成準備好的圖，它回報做好了，圖也確實進了專案資料夾。過了大半天我才發現畫面上還是舊的樣子：圖是進來了，但沒有任何一個畫面真的去用它。素材跟程式不一樣，程式接錯會當場噴錯誤，圖沒被用到不會有任何訊號，它就安安靜靜躺在資料夾裡。後來我加了一道自動檢查，資料夾裡每張圖的檔名都必須在畫面檔或程式裡出現過，沒出現就算沒接好、直接報錯。AI 說的「完成」是它動完了手，不是你要的畫面出現了，這類改動一定要自己開起來看過。

### 它寫出引擎不吃的寫法，還不會當場報錯

有一次腳本跑起來一直不會結束，查下去才發現它把別的程式語言的一種迴圈寫法套進 Godot 用的語言，那個寫法在這裡根本不存在，程式卻不會當場報錯，只是永遠不照預期停下來。AI 是照它熟悉的語言習慣在寫，不會主動確認你這套引擎吃不吃。兩個做法可以擋掉大半：把引擎名稱跟版本寫進專案的說明檔，讓它每一輪對話都看得到；行為怪怪的時候，先請它檢查是不是用了這個版本沒有的寫法，再去查你自己的邏輯。

## 常見問題 FAQ

### Q1：Claude Code 做遊戲能幫到哪些部分？角色圖和音效它也能生嗎？

不能。Claude Code 是動手改檔案的助手，不是遊戲設計師，也不會生素材，角色圖、音效、配樂都得你另外準備。它真正省力的地方是規則類的實作：扣血怎麼算、費用怎麼扣、勝負條件怎麼判定，這些邏輯只要講清楚，它做得又快又準。所以搭 Godot 特別順，Godot 的專案檔是純文字，Claude Code 打開就讀得懂。要注意它有時會說改好了但畫面其實沒變，得自己開編輯器驗一次。

### Q2：用 Claude Code 做遊戲要花多少錢？

Claude Code 要訂閱 Claude Pro（每月 20 美元）才用得順，Godot 本身完全免費、沒有授權費，整個入門成本就是一份訂閱費。想先試水溫，用免費額度跑幾輪對話，也做得出一個簡單的移動畫面。

### Q3：不用 Godot，直接用 Unity 或 Unreal 可以嗎？

可以，但 Unity 的專案檔大多是二進位格式，Claude Code 沒辦法直接讀懂，要另外花設定工夫才能操作編輯器，步驟比 Godot 多。本來就熟 Unity 的人值得花這個設定時間；第一次碰遊戲開發的人，Godot 下載安裝完就能直接用，會讓你少卡在環境上。

### Q4：用 AI 做出來的遊戲，可以上架或商業使用嗎？

可以，程式碼在你自己的專案資料夾裡生成，權利歸你。上架前自己多玩幾輪，牽涉付費、排行榜這類使用者資料或金流的功能，AI 寫的邏輯會動不代表沒有漏洞，找懂的人看過一眼再上線比較穩妥。

### Q5：要學到什麼程度才能開始？

會打字描述你想要的畫面就能開始。決定你做得順不順的是形容需求的能力，不是程式基礎。先寫一條最簡單的規則（像是一顆棋子怎麼走）整理成清單，讓 AI 幫你實作出來，卡住的地方回頭查、問 AI，邊做邊學比先把理論讀完快得多。

## 總結

用 **Claude Code** 做遊戲，門檻不在會不會寫程式，在能不能把規則講清楚。下載安裝好 **Godot** 、把 Claude Code 開在同一個專案資料夾、把提示詞講到具體到位、規則先整理清楚再動手，這幾件事抓住，動工前我也沒想到自己做得出一款能連線對打的遊戲。挑 Godot 當起點，是因為它免費、專案檔是純文字、對 AI 友善，力氣可以花在整理規則上，不用耗在環境設定。

想持續收到這類 AI 工具的第一手實測，歡迎訂閱 [我的電子報](https://aiqkangber.com/newsletter) ，每週 AI 新聞精選加上短評，一封信看完。