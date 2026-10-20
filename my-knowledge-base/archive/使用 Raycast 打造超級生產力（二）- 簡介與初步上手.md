---
title: "使用 Raycast 打造超級生產力（二）- 簡介與初步上手"
source: "https://vocus.cc/article/661fee7cfd89780001c5ad1e"
author:
  - "[[FLC]]"
published: 2024-05-19
created: 2026-05-28
description: "Raycast 是一個提供命令統一與快捷鍵最佳化的工具，讓開發者可以快速地存取並使用各種工具，進而提升生產力。本文介紹 Raycast 的發展、功能簡介、系統要求與適用對象，以及初步上手和使用建議。"
tags:
  - "clippings"
---
> 這篇文章是 Raycast 的介紹與上手體驗，如果想要了解 Raycast 這個生產力工具嘗試解決的生產力問題，可以參考前一篇文章 [使用 Raycast 打造超級生產力（一）- 為什麼我們需要 Raycast？](https://vocus.cc/article/661943e1fd897800016eb0b9) ，裡面提到了現代工作流程裡，使用了大量的生產力工具為什麼反而降低了我們的工作效率。

## 關於 Raycast

### Raycast 的發展

Raycast 在 2020 年 10 月發布公開測試版，根據開發團隊的 [介紹](https://www.raycast.com/blog/hello-world) ，他們發現公司越來越仰賴各種軟體工具來提高工作效率，卻對使用者造成更多學習成本與使用負擔：為了修復 Bug、更新同事工作進度以及查找資訊，不停地在瀏覽器分頁或是各種服務間切換，破壞了整個開發流程的體驗。

為什麼會看到廣告

受到命令列介面（Command-Line Interface，CLI）的啟發，他們打造了 Raycast，透過 ****將工具的使用轉換為指令（Command）**** ，搭配快捷鍵最佳化效率，讓開發者可以快速地存取並使用各種工具，進而提升生產力，改善開發體驗。雖然 Raycast 最初的目標用戶是軟體開發者，但 Raycast 大部分功能都不要求有程式能力才能使用，一般的工作者同樣可以使用它作為提升生產力的工具。

> 「將工具的使用轉換為指令」可能有點抽象，意思是透過簡單的介面輸入幾個關鍵字（指令），就可以進行原本複雜的操作，後面實際上手 Raycast 就可以明白這句話的意思。

除了統一命令與快捷鍵的最佳化設計，與各種第三方服務進行整合也非常重要。在 Raycast 發布公開測試版的同時，就已經串接了 Github 與 Jira 等等服務，讓使用者可以整合進自己的工作流程；Raycast 團隊也提供 [****Extension API****](https://developers.raycast.com/) ，讓開發者可以針對特定工具或服務開發擴充功能，打造個人化的工作流程並整合至 Raycast。 2021年 10 月，Raycast 團隊宣布，使用者可以開始透過 Raycast 提供的 ****擴充商店**** （ [****Extension Store****](https://www.raycast.com/store) ）安裝社群夥伴所開發的各種擴充功能，進一步讓 Raycast 成為一個擁有強大擴充性的生產力工具平台。

2023 年 5 月，也就是 GhatGPT 發布約 6 個月後，Raycast 宣布推出包含 AI 功能的 [****Raycast Pro****](https://www.raycast.com/pro) 付費訂閱服務。這個付費訂閱新增了雲端同步、自訂主題等功能，並引入 ****Raycast AI**** - 這個功能整合了 AI 模型，讓付費使用者可以在 Raycast 上輕易的打造個人的 AI 生產力流程。

### Raycast 功能簡介

Raycast 目前可以實現的功能：

- ****搜尋應用程式、資料夾或檔案並開啟**** ：類似 Mac 內建的 Spotlight；
- ****快速存取 Mac 上常用功能或設定**** ：例如查看行事曆、鎖定螢幕、休眠、移除外接硬碟、系統設定等等；
- ****剪貼簿紀錄（Clipboard History）**** ：Raycast 提供剪貼簿的歷史紀錄列表功能，可以檢視過去曾經複製過的內容，並對複製內容進行更多細節操作，例如只選取部分複製內容來進行貼上等等；
- ****視窗管理（Window Management）**** ：Mac 原生的視窗管理功能並不強大，如果需要能夠方便的管理視窗版面，例如視窗分割與視窗排列，往往需要另外下載工具，而 Raycast 整合了這項功能讓使用者可以方便地進行視窗管理；
- ****快速連結（Quicklinks）**** ：Quicklinks 允許使用者快速地開啟特定網頁、在應用程式裡開啟連結、甚至快速存取其它 Raycast 擴充的指令，並搭配參數使其功能更強大；
- ****文字片段（Snippets）**** ：這個功能可以用來儲存頻繁使用或是內容繁雜的文字片段，並透過關鍵字來快速輸入這些儲存的文字片段；
- 其他還有 ****計算機（Calculator）**** 、 ****翻譯（Translate）**** 、 ****切換視窗（Switch Window）**** 、 ****搜尋選單項目（Search Menu Items）**** 等等方便的小功能；
- ****第三方擴充（Extensions）功能**** ：使用者可以透過 Raycast 的擴充商店下載各種由社群開發者打造的第三方擴充，整合並建立符合自己工作內容的生產力流程；
- ****Raycast AI**** 、還有一些較為進階或是需要付費的功能，例如 ****Scripts Commands、Raycast Teams**** 等等，這部分由於需要另外付費或是要求使用者具備程式能力，因此不會有太多著墨，有興趣的讀者可以自行研究，或是之後的文章有機會再另外進行分享。

### Raycast 系統要求與適用對象

在這篇文章撰寫的當下（2024 年 5 月），只要擁有一台版本為 [****macOS 12（Monterey）以上****](https://support.apple.com/zh-tw/109033) ****的 Mac**** ，就可以使用 Raycast。

> Raycast 目前並沒有支援 Windows 與 Linux，開發團隊 [表示](https://) 他們希望先專注在 Mac 版本的開發上。

我認為 Raycast 的適用對象是那些 ****使用大量生產力工具的電腦工作者**** 。無論是開發者、專案經理、設計師、影音或文字工作者等，只要使用超過一定數量的生產力工具，且苦於工具彼此間的來回切換，一旦上手 Raycast，就能夠帶來生產力的顯著提升。如果是已經有建立起自己一套高效率的電腦工作流程的使用者（例如使用更早推出，也同樣受歡迎，與 Raycast 概念類似的 [Alfred](https://) ），轉換到 Raycast 對生產力的提升可能有限，但還是推薦嘗試看看。

---

對 Raycast 的發展以及功能有初步了解後，接下來，我們來實際體驗 Raycast，了解如何使用 Raycast 提升我們的生產力。

## Raycast 初步上手

> 目前 Raycast 只有提供英文介面，官方文件也都是英文，為了讓接下來介紹 Raycast 詳細功能的內容易於理解，我會在第一次提到某個功能名詞時自行翻譯為中文，並括號註明介面上或文件內的英文，方便與介面或文件對照。在文章內容裡，如果提到功能，會盡可能使用中文（除了 Quicklinks 或 Snippets 這一類英文描述更精確的功能會直接以英文代稱），而如果是描述 Raycast 的介面的文字，會以英文指稱。

### 下載並安裝

安裝 Raycast 非常簡單，前往 Raycast 官網（ [https://www.raycast.com](https://www.raycast.com/) ），點擊首頁上的下載連結，下載後安裝即可。也可以直接透過 [Homebrew](https://brew.sh/) （Mac 上的套件管理工具，需另外安裝），在終端機使用以下指令進行安裝：

```plain
brew install --cask raycast
```

### 啟動 Raycast

![zoomable](https://images.vocus.cc/9a9ace95-572b-4974-b6c0-c6f1d93beadb.jpg)

Raycast 歡迎畫面

第一次打開 Raycast，會出現歡迎畫面，可以跟隨 Raycast 的引導進行設定（Start Setup）。建議在設定 Raycast 啟動快捷鍵的部分，設定自己喜好的快捷鍵來啟動 Raycast，預設為

```
⌥
```
```
Space
```
，可以設定為
```
⌘
```
```
Space
```
，除了更為方便之外，也能夠 [取代 Mac 預設的 Spotlight 行為](https://manual.raycast.com/hotkey) 。

所有預設設定都可以在之後透過 Raycast 的設定頁面（ ****Settings**** ）進行修改，要開啟設定頁面：

- 只要在 Raycast 視窗開啟的時候，按下
	```
	⌘
	```
	```
	,
	```
- 或是點選桌面右上角狀態選單的 Raycast 圖示，並選擇【 ****Settings...****】也可以開啟。

![桌面右上角的 Raycast 圖示按下右鍵後也可以選擇設定（Settings）](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Ff7c9f61e-19ee-48e2-95c2-5f512605982b.png&width=740&sign=v5xqv4sbhhBEQeFrYtYurlCTFlSSW-F1yrnhmA1hP0U) ![zoomable](https://images.vocus.cc/f7c9f61e-19ee-48e2-95c2-5f512605982b.png)

桌面右上角的 Raycast 圖示按下右鍵後也可以選擇設定（Settings）

關於設定頁面的細節，接下來有提到相關內容會再介紹。

> 為了簡潔一致，提到鍵盤快捷鍵的時候，會使用符號或縮寫來代表 Mac 上常用的按鍵：  
> ```
> ⌘
> ```
> ：command，CMD  
> ```
> ⌃
> ```
> ：control，CTRL  
> ```
> ⌥
> ```
> ：option，OPT  
> ```
> ⇧
> ```
> ：shift  
> ```
> ↩︎
> ```
> ：enter（return）  
> ```
> ⌫
> ```
> ：backspace  
> ```
> ⇪
> ```
> ：caps Lock，CAPS  
> ```
> ⇥
> ```
> : tab  
> ```
> ␣
> ```
> : space

### 介面操作概覽

當我們每次使用設定的快捷鍵開啟 Raycast，會看到如下的 ****主要搜尋**** （ ****Root Search / Main Search**** ）視窗，用來搜尋並選擇想要執行的指令，這個主要搜尋視窗由三個部分組成，由上至下為依序為 ****搜尋列（Search Bar）**** 、 ****內容列表**** 、 ****操作面板（Action Panel）**** ：

![Raycast 主要搜尋視窗](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F89c5c6c2-4228-462a-9874-59f9fe55f05e.png&width=740&sign=J0DgCdj9tBEv_GlSpJo3zZrOdohTV_l96An7yF24A-U) ![zoomable](https://images.vocus.cc/89c5c6c2-4228-462a-9874-59f9fe55f05e.png)

Raycast 主要搜尋視窗

- ****搜尋列（Search Bar）**** ：搜尋列讓使用者可以輸入關鍵字，快速搜尋程式、擴充與指令，每當開啟 Raycast，游標就會聚焦於此讓使用者可以進行搜尋，搜尋過程中，下方的內容列表也會隨著關鍵字變化。另外，特定指令在搜尋過程中，也會在搜尋列出現 ****參數**** （ ****Arguments**** ）欄位，讓使用者可以輸入內容進行進階的指令操作。
- ****內容列表**** ：還沒在搜尋列輸入任何內容之前，內容列表預設會顯示 ****最愛**** （ ****Favorites**** ）、 ****建議**** （ ****Suggestions**** ）與所有其它 ****指令**** （ ****Commands**** ）。 ****Favorites**** 區塊會顯示使用者加入最愛的指令。 ****Suggestions**** 區塊內則會顯示 Raycast 推薦使用者使用的指令，通常會是最近使用或是頻繁呼叫的指令。一旦使用者開始搜尋，內容列表就會顯示 ****搜尋結果**** （ ****Results**** ）。無論是預設內容或是搜尋結果，使用者可以隨時使用方向鍵上下移動選擇指令。
- ****操作面板（Action Panel）**** ：Action Panel 位於 Raycast 視窗的下方，會根據內容列表所選取的指令，顯示對應的 ****主要操作**** （ ****Primary Action**** ），按下
	```
	↩︎
	```
	就可以執行 Primary Action。以上圖為例，Primary Action 就是 【Open Application】。在 Primary Action 的旁邊，則會顯示【Actions】，提示可以使用
	```
	⌘
	```
	```
	K
	```
	快捷鍵來搜尋並執行更多其它操作。

### 體驗 Raycast 的操作

讓我們先以一個內建的 ****Search Google**** 的 Quicklink 指令為例，來實際體驗在 Raycast 執行功能的過程。

> Quicklink 指令會在之後的文章有更詳細的介紹。

Raycast 功能的執行是透過指令來達成的。我們用快捷鍵啟動 Raycast，接著輸入「goo」來搜尋 Search Google 指令，如下圖所示，可以看到內容列表顯示了搜尋結果，同時第一個選項就是我們想要的 Search Google 指令。Raycast 使用模糊搜尋（fuzzy search），這意味著我們不需要完整輸入指令的關鍵字，Raycast 就可以替我們找到各種相關的指令。Raycast 會根據指令使用頻率、最近選擇指令或其它條件來排序搜尋結果，所以如果是第一次用 「goo」 這樣的關鍵字，第一個選項未必是 Search Google，可以輸入較為完整的關鍵字（例如：「search goo」）讓 Search Google 排序更前面。或是使用方向鍵往下選擇 Search Google，這樣在下一次輸入「goo」的時候，Search Google 的選項就會往前排序，甚至會是第一個選項。

> 我在這裡有設定 Search Google 的 Quicklink 別名（Alias）為「goo」，因此用「goo」關鍵字搜尋會更容易將 Search Google 排序在前面。關於 Alias，會在之後文章介紹。

![輸入「goo」，可以看到 Search Google 的指令出現](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fb543fe8d-cea0-468d-90e8-13b532b23fff.png&width=740&sign=VCltTJS8DXbmpE5YGt3YZVAARSvkCKAxcBTNoHTgJVM) ![zoomable](https://images.vocus.cc/b543fe8d-cea0-468d-90e8-13b532b23fff.png)

輸入「goo」，可以看到 Search Google 的指令出現

Search Google 指令同時提供了一個參數欄位【Query】，所以當使用者選中 Search Google 指令時，就會出現參數欄位的輸入框（上圖中顯示 Query 文字的灰底框），按下

```
⇥
```
，游標就會移動到輸入框。在這個例子裡，使用者可以在輸入框輸入想要 Google 搜尋的關鍵字，然後按下
```
↩︎
```
，就會執行主要操作【Open in Arc】，打開 Arc 瀏覽器並且開啟 Google 搜尋的頁面，同時進行關鍵字的查詢，並顯示結果。也就是說，只要按下
```
↩︎
```
，就可以看到想要的搜尋結果分頁出現在眼前。

下面展示完整的 Search Google 指令搜尋「Raycast」關鍵字的過程：

![不需要滑鼠操作或打開瀏覽器分頁，只要幾個快捷鍵與輸入查詢的關鍵字，就能夠快速進行 Google 搜尋](https://images.vocus.cc/b4554dbf-6d9b-4d73-be43-baed9c078cc9.gif) ![zoomable](https://images.vocus.cc/b4554dbf-6d9b-4d73-be43-baed9c078cc9.gif)

不需要滑鼠操作或打開瀏覽器分頁，只要幾個快捷鍵與輸入查詢的關鍵字，就能夠快速進行 Google 搜尋

我設定 Search Google 預設會開啟 Arc 瀏覽器顯示搜尋結果，如果想要暫時改用另外的瀏覽器開啟，該怎麼做？

每個指令除了主要操作外，記得前面提到可以檢視更多操作的列表嗎？在 Search Google 指令選取的狀態下，按下

```
⌘
```
```
K
```
，就會如下圖出現其它可操作行為的列表，其中的【Open With...】操作，就可以讓我們使用其它瀏覽器開啟搜尋結果。透過這個列表，可以搜尋與執行其它操作，也可以檢視並學習對應快捷鍵來讓使用指令更有效率。

![指令被選中時，按下 ⌘ K 就會跳出與指令相關的其它操作列表](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Ff6315d99-b442-4169-83bf-8a983bfdecc9.png&width=740&sign=iiHAHifouClkf3Ww8AnuJwhh4nM7j2gkR8tNCMhkS0Q) ![zoomable](https://images.vocus.cc/f6315d99-b442-4169-83bf-8a983bfdecc9.png)

指令被選中時，按下 ⌘ K 就會跳出與指令相關的其它操作列表

下面展示執行 Search Google 指令，但使用 Safari 瀏覽器來開啟搜尋結果：

![使用 Google Search 指令，但選擇用 Safari 瀏覽器開啟搜尋結果](https://images.vocus.cc/321ed4eb-82b4-41c8-86cf-ea4ebf26b3d2.gif) ![zoomable](https://images.vocus.cc/321ed4eb-82b4-41c8-86cf-ea4ebf26b3d2.gif)

使用 Google Search 指令，但選擇用 Safari 瀏覽器開啟搜尋結果

類似 Google Search 這樣的指令，輸入指令並執行後，通常就會跳到其它的程式介面。

另外有一些指令，在選擇指令後，需要更進一步操作（通常是進一步搜尋），這時候 Raycast 視窗就會顯示進一步功能的視窗。以 ****Search Files**** 指令為例，如下圖所示，當我們在主搜索視窗搜尋列輸入「search files」並按下

```
↩︎
```
後，就會顯示如下圖的內容，這是 Search Files 指令的進一步操作：直接透過 Raycast 搜尋檔案或資料夾。

![Search Files 功能的操作畫面](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Feada891b-8abc-4d96-804e-57e8dd57addd.png&width=740&sign=MmyT1Md05FiJISHFKPYjndFw916biQ_nFNFpyea76gE) ![zoomable](https://images.vocus.cc/eada891b-8abc-4d96-804e-57e8dd57addd.png)

Search Files 功能的操作畫面

下面展示了如何使用 Search Files 指令搜尋檔案並開啟：

![使用 Search Files 指令來搜尋檔案並開啟](https://images.vocus.cc/005db896-957e-41af-b6cf-9ac1214b4272.gif) ![zoomable](https://images.vocus.cc/005db896-957e-41af-b6cf-9ac1214b4272.gif)

使用 Search Files 指令來搜尋檔案並開啟

---

## Raycast 體驗感受

到了這邊，如果已經嘗試使用 Raycast 並熟悉操作以後，應該已經可以感受到 Raycast 是如何透過簡單一致的方式，減少不必要的操作，降低「情境切換」的成本，來提高工作效率，也就是前面提到的「將工具的使用轉換為指令」。

Google 搜尋與搜尋檔案這兩個功能，都從原本的打開對應程式（瀏覽器與 Finder），移動滑鼠，點擊搜尋框，輸入關鍵字，被簡化為輸入 Search Google 與 Search Files 指令，打上搜尋關鍵字，就可以查看對應結果。這樣的過程展現了 Raycast 如何改善之前提到的生產力降低問題：

- 原本較複雜的行為被簡化為指令，統一在 Raycast 執行，節省時間，體驗一致。
- 指令的各種操作透過鍵盤即可達成，並且有清楚對應快捷鍵，幾乎不需要使用滑鼠，降低鍵盤滑鼠之間來回切換的成本。
- 過程都在 Raycast 的介面，不需要適應切換不同的工具畫面，減少認知負擔，降低專注力耗損。

這些改善，除了因為減少不必要的操作而節省了時間，也減少「情境切換」的認知負擔，同時因為少了不必要的操作，避免了專注力的消耗，就更能夠專心在核心的工作項目上。就我自己的個人感受，Raycast 帶給我最直觀的體驗是 ****降低了切換生產力工具的煩躁感**** ，光是少了這種煩躁感，工作起來就舒服許多，至少體感地覺得自己的工作效率直線上升。

---

## Raycast 上手建議

如果你在過去並不習慣使用類似 Raycast 這樣的啟動器工具，那麼可能會需要一些時間去適應它。你的肌肉記憶可能還是習慣過去的操作模式：每當要進行 Google 搜尋，手就會習慣性地移動到滑鼠，移動到瀏覽器圖示點擊，然後打開 Google 搜尋頁。在建立新習慣之前，舊習慣可能會形成阻礙，甚至短時間內降低生產力。

我自己其實在讓 Raycast 變成自己工作流程一部分的一年多前，就已經下載了 Raycast，但由於沒有使用過這一類啟動器工具的習慣，下載後體驗了半小時，我就回到了以往的工作方式。重新再次注意到 Raycast，其實是因為買了新的 MacBook，在設定新筆電的同時，發現以前曾經安裝過 Raycast，於是花上了半天時間，仔細研究，並試著將它整合進自己的工作流程裡，然後在往後幾週內，建立起使用 Raycast 的習慣。

新工具會有一些學習成本，但如果上手後確實能夠改善工作效率，就十分值得投資，而需要的就是 ****耐住性子去體驗與嘗試，並建立起新的工作習慣**** 。建議可以摸索 Raycast 的各種功能，試著根據自己平常的工作流程，思考是否可以透過 Raycast 進一步最佳化改善，並儘量在工作上應用，相信過了一段時間，就可以打造出屬於自己的高效率工作流程。

---

## 接下來，更多 Raycast 的功能與應用 ⋯⋯

我們體驗了 Google Search 與 Search Files 指令，而 Raycast 還有許多其它強大的應用。下面是一些 Raycast 資源與社群，裡面有許多 Raycast 相關的使用介紹與分享，值得參考：

接著在下一篇文章，我會以自己實際使用的角度，介紹更多 Raycast 常用的核心功能與應用，並分享第三方擴充使用範例。