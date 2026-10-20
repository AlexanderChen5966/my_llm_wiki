---
title: "使用 Raycast 打造超級生產力（三）- 功能與擴充介紹"
source: "https://vocus.cc/article/66408839fd89780001f4c9b1"
author:
  - "[[FLC]]"
published: 2024-06-02
created: 2026-05-28
description: "本篇文章會介紹 Raycast 常用的的核心功能與第三方擴充使用範例。"
tags:
  - "clippings"
---
> 本篇文章會介紹 Raycast 常用的的核心功能與第三方擴充使用範例，如果想要對 Raycast 的背景有所了解，並快速上手體驗，可以參考前一篇文章 [使用 Raycast 打造超級生產力（二）- 簡介與初步上手](https://vocus.cc/article/661fee7cfd89780001c5ad1e) 。

---

## 擴充（Extension）是 Raycast 的核心

我們很快就會進入 Raycast 核心功能的介紹， 不過在那之前，讓我們先了解 Raycast 裡擴充的概念與設定。



<iframe frameborder="0" src="https://4cd0a222d99246e8b1e70618725b02c5.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="728" height="90" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>Raycast 的功能可以視為由呼叫不同指令（Command）來達成，而擴充（Extension）則可以看作是類似功能指令的組合。所以 Raycast 預設的功能就可以視為是由一組組預先安裝好的擴充，裡面提供不同的指令來達成。以 Raycast 預設的功能為例： ****System**** ，裡面就提供了各種進行系統操作的指令，例如 Empty Trash、Restart、Show Desktop 等等。

![zoomable](https://images.vocus.cc/51d1828d-2909-4363-94ab-2beec0c9ef26.png)

輸入「system」，就會看到一系列於系統功能相關的指令。

要檢視有哪些擴充，可以開啟 Raycast 的設定頁（別忘了在 Raycast 視窗開啟的時候按下

```
⌘
```
```
,
```
就可以打開設定頁），並選擇【Extensions】頁籤。

![Raycast Extensions 設定頁。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fba3272df-01db-413b-945a-8006526d0c9d.png&width=740&sign=AvIactLPv5xNJy0YeMjv3qhaLeTEWFTrYqBReprytns) ![zoomable](https://images.vocus.cc/ba3272df-01db-413b-945a-8006526d0c9d.png)

Raycast Extensions 設定頁。

Raycast 將指令操作大致分成 ****指令（Commands）**** 、 ****腳本（Scripts）**** 、 ****應用程式（Apps）**** 與 ****快速連結（Quicklinks）**** 。類似的指令集中在一起被分類為 ****擴充（Extension）**** 或 ****集合（Group）類型（Type**** ）。在設定頁面上，Type 為 Extension 的選項，展開來就是一系列相關的 Command，而 Scripts、Apps、Quicklinks 等也被整理成 Type 為 Group 的選項，方便進行設定。

不同的 Extension，會有不同的設定，當選擇指令選項，其它設定就會出現在右側面板中。

![System 選項展開後有許多與系統設定相關的指令。右側面板會顯示對應指令的資訊與設定。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F4c75ff72-501d-4e1a-b487-ac0448b09813.png&width=740&sign=HdsmQY-FQpJtG2V7C0EwO_Y3o_htxgWN8yjuEQg3Ifk) ![zoomable](https://images.vocus.cc/4c75ff72-501d-4e1a-b487-ac0448b09813.png)

System 選項展開後有許多與系統設定相關的指令。右側面板會顯示對應指令的資訊與設定。

每個指令選項都還可以設定 ****Alias**** （ ****別名**** ）與 ****Hotkey**** （ ****快捷鍵**** ），也可以各自決定是否開啟（ ****Enabled**** ）。

![Raycast 的指令可以設定 Alias 與 Hotkey](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F5cc47023-a091-43b5-89b8-1c1519a632c3.png&width=740&sign=5FHb7DaLCjriyiKGNnoUVZRJoNRGS1Q2EXQA7zpc2ZM) ![zoomable](https://images.vocus.cc/5cc47023-a091-43b5-89b8-1c1519a632c3.png)

Raycast 的指令可以設定 Alias 與 Hotkey

- ****Alias（別名）**** ：Alias 讓使用者替常用指令設定別名，輸入別名就可以讓對應的指令更容易排在前面順位。除了更方便搜尋指令，別名更適合的場景，是使用在有參數或進一步搜尋的指令：輸入完指令對應的別名，按下空白鍵就可以直接移動到參數輸入框或是進一步搜尋的頁面，例如前面的 Search Google 與 Search File 指令。以 Search Google 指令為例，我設定了 Alias 為「goo」之後，我們就可以在主搜尋視窗輸入對應的「goo」，此時 Google Search 指令就會在第一順位，接著按下空白鍵，就會直接跳到搜尋輸入框。
![Search Google 與 Search Files 指令分別設定了「goo」與「fd」別名，使用別名呼叫指令，並搭配 Space 按鍵， Search Google 可以快速移動到搜尋輸入框，Search Files 可以前往檔案搜尋視窗。](https://img.vocus.cc/aDAPx2tFV1SFWzNTwAkpnTzGNiAB3b2dIH6EyKxnL_4/f:gif/plain/https://images.vocus.cc/f4768f47-7fda-4104-9f65-52c1e627e15a.gif) ![zoomable](https://img.vocus.cc/aDAPx2tFV1SFWzNTwAkpnTzGNiAB3b2dIH6EyKxnL_4/f:gif/plain/https://images.vocus.cc/f4768f47-7fda-4104-9f65-52c1e627e15a.gif)

Search Google 與 Search Files 指令分別設定了「goo」與「fd」別名，使用別名呼叫指令，並搭配 Space 按鍵， Search Google 可以快速移動到搜尋輸入框，Search Files 可以前往檔案搜尋視窗。

- ****Hotkey（快捷鍵）**** ：我們可以為指令設定對應的全域快捷鍵，這意味著就算不開啟 Raycast 視窗，也可以隨時按下設定的快捷鍵，執行對應的指令。適當的設定 Hotkey，減少搜尋指令的動作，可以提高更多效率。

擴充作為 Raycast 的核心，只有內建的功能對無法滿足所有人的需求，因此第三方的擴充非常重要。後面會有第三方擴充的範例，會再介紹如何搜尋與安裝第三方的擴充。

---

## Raycast 常用功能

接下來，讓我們來看看有哪些實用的功能。

> 部份功能需要系統權限，Raycast 會提示使用所需要的權限內容，依照指示進行設定即可。

### 應用程式（Applications）

應用程式的指令很單純，可以直接從 Raycast 搜尋想要開啟應用程式，選擇後並按下

```
↩︎
```
就可以直接開啟。按下
```
⌘
```
```
K
```
，查看其他操作，可以看到許多相關的應用程式操作，例如顯示應用程式資訊【Show Info in Finder】、關閉應用程式【Quit Application】、重啟應用程式【Restart Application】，甚至移除應用程式【Uninstall Application】。

![輸入 safari，就會出現 Safari 應用程式指令，按下 ↩︎ 可以開啟 Safari，⌘ + K 可以顯示更多操作。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F22a48e47-1308-434d-92f5-fbe0db1f0f15.png&width=740&sign=54aaUkYJ8tewEwCLjGMd8fiPRKCvIKlgJapSANKUMec) ![zoomable](https://images.vocus.cc/22a48e47-1308-434d-92f5-fbe0db1f0f15.png)

輸入 safari，就會出現 Safari 應用程式指令，按下 ↩︎ 可以開啟 Safari，⌘ + K 可以顯示更多操作。

透過更多操作裡的【Add to Favorites】就可以將應用程式加入最愛，使用者可以將自己常用的程式加入最愛，就會出現在 Raycast 主視窗的 ****Favorites**** 區塊，可以更快檢視自己常用的應用程式並選擇執行。

![Safari、Arc、WezTerm 等被加入最愛的應用程式指令，顯示在 Favorites 區塊。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F1b04695a-6b3e-434d-8dbc-cb27f097cbb8.png&width=740&sign=3avMgWOab2NSp_pRcxloRfIkg9imN8P8-t2Wr-7dzkU) ![zoomable](https://images.vocus.cc/1b04695a-6b3e-434d-8dbc-cb27f097cbb8.png)

Safari、Arc、WezTerm 等被加入最愛的應用程式指令，顯示在 Favorites 區塊。

> 所有指令都可以加入最愛，不過太多指令加入最愛可能會喪失加入最愛的意義。另外，有些指令也更適合使用別名或是快捷鍵，讀者可以自行斟酌。

### 搜尋檔案（Search Files）

在前面體驗 Raycast 的操作環節，我們已經嘗試使用 Search Files 指令，對其功能有所了解。這裡提供一些操作細節建議：

- 前面有提到，像 Search Files 這類有進一步搜尋的指令，很適合設定別名，建議設定習慣的別名（例如設定「fd」為別名）來進一步提高使用效率。
- 搜尋檔案除了搜尋並按下
	```
	↩︎
	```
	開啟，也有許多其他實用的操作，例如快速檢視（Quick Look）、複製（Copy Files）、複製檔案路徑（Copy Path）、移動到垃圾桶（Move to Trash）等等，這些操作可以減少使用者開啟 Finder，且降低使用滑鼠對檔案進行操作的頻率，進而提高效率。
- 除了透過檔案名稱直接搜尋之外，也可以 ****搭配檔案路徑進行搜尋**** 。例如：如果想要查詢 Downloads 資料夾底下的檔案（假設想要搜尋 Downloads 底下的 raycast.jpg 圖檔），可以輸入「~/downloads/」或「downloads/」，這時候搜尋視窗會只顯示 Downloads 下的檔案，在搜尋框接續輸入想要查詢的檔案名稱（在「/」之後輸入 「raycast」，此時搜尋框會是「~/Downloads/raycast」），Raycast 就會搜尋 Downloads 資料夾並尋找名稱為「raycast」的檔案。
- Search Files 指令也可以搜尋資料夾，然後在 Finder 開啟並檢視。除此之外，當選擇資料夾時，可以執行 ****進入資料夾【Enter Folder】**** 的操作，這時候搜尋視窗會只顯示資料夾裡的檔案，搜尋框會顯示資料夾路徑，可以透過上下移動來搜尋資料夾內的檔案，與前面提到的搭配路徑進行搜尋概念類似，都是限縮範圍的搜尋。
- 與進入資料夾（Enter Folder）操作相反的上層資料夾【Enclosing Folder】操作，則是可以移動到目前搜尋選中檔案（或資料夾）的上層資料夾，搜尋框也會顯示對應的上層資料夾路徑。
![可以透過【Enter Folder】的操作，或是輸入資料夾路徑，來限縮搜尋範圍在特定資料夾內。](https://img.vocus.cc/p7U-n204EKfjTVID_D-b7v_J6SFV81Z37qam_P_ElTI/f:gif/plain/https://images.vocus.cc/47a1e7cf-9eba-4d35-9c74-9225a82d403c.gif) ![zoomable](https://img.vocus.cc/p7U-n204EKfjTVID_D-b7v_J6SFV81Z37qam_P_ElTI/f:gif/plain/https://images.vocus.cc/47a1e7cf-9eba-4d35-9c74-9225a82d403c.gif)

可以透過【Enter Folder】的操作，或是輸入資料夾路徑，來限縮搜尋範圍在特定資料夾內。

### 剪貼簿紀錄（Clipboard History）

複製貼上是工作流程中時常進行的操作，一般情況下，我們可能只需要單純的複製某個內容，然後到另一個工具服務內貼上。不過想像一下一些比較複雜的情境：

- 想貼上一個較早之前複製的內容；
- 需要同時從不同地方複製多個文字內容，然後同時貼到同一個地方；
- 複製了一段文字，但想先對它做一些修改（只需要這段文字的部分內容），然後貼到多個不同的地方。
- 你想對複製的內容進行有效率的其它操作，例如在網頁上複製了一張圖片，並希望可以立刻用圖片軟體檢視修改，不需要另外下載後再開啟。

這時候一個能對剪貼簿進行檢視與管理的工具就能夠有效的提高複製貼上的效率。

Raycast 可以紀錄你過去複製的各種內容，例如純文字、網址、圖片、檔案等等，並對這些複製的內容進行管理。有許多軟體工具也提供了類似的功能，例如 [Paste](https://pasteapp.io/) ，但 Raycst 的剪貼簿功能完整，足以取代這些還需要另外下載安裝的工具。

在 Raycast 搜尋 「clipboard」，Clipboard History 指令就會出現，按下

```
↩︎
```
就會看到剪貼簿歷史紀錄，並且可以進行搜尋。

![Raycast 剪貼簿歷史紀錄（Clipboard History）搜尋功能](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F9e09e68d-c12e-47bd-83ba-df915db7dce6.png&width=740&sign=LwZOxPlVgeHh7qOKIOkei7BReeKzFb-soRJPMcN1o54) ![zoomable](https://images.vocus.cc/9e09e68d-c12e-47bd-83ba-df915db7dce6.png)

Raycast 剪貼簿歷史紀錄（Clipboard History）搜尋功能

> Clipboard History 是很常用的指令，建議可以設定別名或快捷鍵，快速呼叫出來，我自己就設定了
> ```
> ⌘
> ```
> ```
> ⇧
> ```
> ```
> v
> ```
> 的快捷鍵，可以在任何地方呼叫 Clipboard History。

在剪貼簿紀錄的視窗，按下

```
↩︎
```
就可以將選擇的複製內容貼到目前啟動的程式視窗。例如原本在 Google Docs 編輯，然後呼叫出剪貼簿內容，選擇要貼上的內容，並按下
```
↩︎
```
，複製內容就會貼到 Google Docs。視窗的右側是複製內容的預覽與詳細資訊。

```
⌘
```
```
K
```
，針對複製內容就可以進行更多的操作。例如刪除複製內容【Delete Entry】、分享複製內容【Share...】、釘選複製內容【Pin Entry】等等。使用【Pin Entry】，複製的內容就會被釘選在搜尋視窗最上方的區塊。另外根據複製內容不同，也會有一些不同的操作。如果複製內容是文字，就可以有像是編輯複製內容【Edit Content】；複製內容是網址的話，可以使用瀏覽器開啟【Open in Browser】；而如果複製內容是檔案，就可以使用快速查看【Quick Look】來快速檢視內容。

### 視窗管理（Window Management）

Mac 本身的視窗排列管理功能並不強大，往往需要第三方的工具來達成，例如像是 [Magnet](https://magnet.crowdcafe.com/) 、 [Rectangle](https://rectangleapp.com/) 等等。Raycast 提供了一系列的指令可以來對當前的視窗進行排列管理。

![輸入「window」，就會出現一系列視窗管理的指令](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F81bcc595-89b6-4531-9c84-7a4d3745e38b.png&width=740&sign=iAhIu-BT73eA3XPnlSFmxNds7LOtXCxO-N5iFYSWNT0) ![zoomable](https://images.vocus.cc/81bcc595-89b6-4531-9c84-7a4d3745e38b.png)

輸入「window」，就會出現一系列視窗管理的指令

Raycast 提供了將近 60 組視窗管理的指令，基本上涵蓋了各種想像得到的視窗排列功能。強烈建議為常用的視窗管理指令設定快捷鍵來進一步提高效率，如果不知道如何設定快捷鍵組合，Raycast 參考其它的視窗管理工具（例如前面提到的 Magnet 與 Rectangle）提供了常用快捷鍵預設值，只要的擴充設定頁面選擇，Raycast 就會為常用的視窗管理指令設定對應的快捷鍵預設值。

![在擴充設定可以為視窗管理指令選擇預設的快捷鍵組合，上圖是選擇了 Magnet 的快捷鍵預設值](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F1e5cfe90-e0c2-4d28-9446-2d37aa691d2f.png&width=740&sign=BeuClEh2bHvOZTZhcxu1eWPHQLkZpfE4ppG3F5iwJy8) ![zoomable](https://images.vocus.cc/1e5cfe90-e0c2-4d28-9446-2d37aa691d2f.png)

在擴充設定可以為視窗管理指令選擇預設的快捷鍵組合，上圖是選擇了 Magnet 的快捷鍵預設值

下面展示使用快捷鍵進行視窗排列：

![vocus｜新世代的創作平台](https://img.vocus.cc/7wwDQ0Gmixlxb1Q-Qg5obQdxcxVpMetazYjEHotzi2w/f:gif/plain/https://images.vocus.cc/9fd7b6da-747f-4cbb-ab8d-b05ea4d222bc.gif) ![zoomable](https://img.vocus.cc/7wwDQ0Gmixlxb1Q-Qg5obQdxcxVpMetazYjEHotzi2w/f:gif/plain/https://images.vocus.cc/9fd7b6da-747f-4cbb-ab8d-b05ea4d222bc.gif)

### 計算機（Calculator）

Raycast 的計算機功能非常特別，並不需要輸入「 calculator」 指令來開啟（如果你輸入 「calculator」，會搜尋到 Mac 內建的計算機軟體），直接在主要搜尋框輸入要計算的內容，Raycast 就會輸出結果。計算結果顯示後，按下

```
↩︎
```
就可以複製結果到剪貼簿。

![輸入「100^5」，Raycast 的計算機功能就會直接顯示結果「10,000,000,000」](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F4835a066-79e6-4a58-a5ac-b701eb3ef0ed.png&width=740&sign=3xHUNZMVN-KnNFuhCjslZDEFYDAhy2jUI00hgNcMJHQ) ![zoomable](https://images.vocus.cc/4835a066-79e6-4a58-a5ac-b701eb3ef0ed.png)

輸入「100^5」，Raycast 的計算機功能就會直接顯示結果「10,000,000,000」

計算機的功能不僅限於一般數學計算，還可以進行下列計算：

- 單位或匯率換算：「10 ft in m」 或 「1 USD in NTD」
- 百分比計算：「30% of 1000」
- 時區時間：「time in Japan」
- 日期時間計算：「days until 31 Mar」或 「monday in 3 weeks」

### 螢幕截圖（Screenshots）

Raycast 本身不具備螢幕截圖的功能，不過它提供了 Search Screenshots 的指令，讓使用者可以方便管理截圖。

使用 Search Screenshots 指令前，建議先到擴充設定裡，設定要作為搜尋截圖的資料夾。在 Search Screenshots 指令設定裡點選【Add Directories】，就可以加入搜尋截圖的資料夾。

![Search Screenshots 指令設定，選擇【Add Directories】來加入搜尋截圖的資料夾](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F28bb722d-c063-41e5-90e5-219dac3a65f0.png&width=740&sign=xdr3UuiYE1IBrSEeY6lDMO4lBISj5-ipaIyE46Kn_jU) ![zoomable](https://images.vocus.cc/28bb722d-c063-41e5-90e5-219dac3a65f0.png)

Search Screenshots 指令設定，選擇【Add Directories】來加入搜尋截圖的資料夾

在主搜索視窗，輸入「search screenshots」，按下

```
↩︎
```
，Raycast 就會展示目前位於設定資料夾裡的截圖（以及螢幕錄影），並且可以依據檔名進行搜尋。選擇截圖，按下
```
↩︎
```
，就可以將截圖複製到剪貼簿；按下
```
⌘
```
```
↩︎
```
，就可以將截圖貼到當前的應用程式。Raycast 還提供了其它操作，例如擷取截圖裡的文字等功能，讀者有興趣可以自行研究。

![Raycast 截圖搜尋功能](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F47f60458-1ce3-45f8-a1b6-f46d9f20e823.png&width=740&sign=4xA1GNhZzsH1uz-ppoxzt-OcBZ95W3bEF1UzBTuZiX4) ![zoomable](https://images.vocus.cc/47f60458-1ce3-45f8-a1b6-f46d9f20e823.png)

Raycast 截圖搜尋功能

### Snippets

Snippet 是「片段」的意思。例如我們會用 Text Snippet（文字片段）這樣的詞彙來指稱一段有特別意義或可重複使用的文字。

Raycast 的 Snippets 功能允許使用者設定常用的文字片段，並在需要的時候快速使用這個文字片段，減少重複輸入相同內容。例如我們很常會需要輸入自己的 Email 或手機號碼，使用 Snippet 的話就可以儲存 Email 與手機號碼，並使用關鍵字來在需要的地方插入這些內容。

我們以設定 Email 的 Snippet 為例。首先在 Raycast 搜尋框輸入「create snippet」，按下

```
↩︎
```
進入 Snippet 設定。可以看到有【Name】、【Snippet】 與【Keyword】這三個可以輸入的欄位。【Name】 是設定的 Snippet 的識別名稱，我們就先取名為「Email」。【Snippet】 是設定的文字片段內容，就是我們的 Email，我以「myemail@gmail.com」為例。【Keyword】則是使用者可以設定的關鍵字，並透過輸入關鍵字，快速地「展開」我們所設定的文字片段內容。我們將【Keywrod】設定為「!email」。按下
```
⌘
```
```
↩︎
```
，就會儲存設定的 Snippet。

![使用 Create Snippet 指令設定 Email Snippet](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Ffb043715-ba56-4da9-87fa-e7985f5fd4c9.png&width=740&sign=D4y-ORQGN5miDiwTH8FLOhIuVozPZXO6aO32Bj2kfTU) ![zoomable](https://images.vocus.cc/fb043715-ba56-4da9-87fa-e7985f5fd4c9.png)

使用 Create Snippet 指令設定 Email Snippet

接下來，你就可以在任意需要輸入文字的地方，輸入「!email」關鍵字，接著「!email」這段文字就會立刻被替換成設定的 Email 文字片段，也就是「myemail@gmail.com」。

除了 Email 與手機號碼，地址、常用符號、Emoji 等等常用的文字片段也都可以設定為 Snippet。設定的 Snippet，可以透過 Search Snippets 指令進行搜尋與管理。你可以將自己設定的 Snippet 匯出成 JSON 檔案，並分享給其他人匯入 Snippet（ [Import Snippet](https://manual.raycast.com/snippets/how-to-import-snippets) ）。Raycast 也提供了一些方便的 Snippet 在網站上（ [Snippet Explorer by Raycast](https://snippets.ray.so/) ），使用者可以選擇並匯入。

![輸入關鍵字「!email」，Raycast 就會替換成對應的文字片段「myemail@gmail.com」](https://img.vocus.cc/F-2jmk5xnZGk-jU5ktHIz0ji9f1aYY30xr4W2qZMACM/f:gif/plain/https://images.vocus.cc/dbd1f7e9-0be4-4142-b73e-57b93ac7ee37.gif) ![zoomable](https://img.vocus.cc/F-2jmk5xnZGk-jU5ktHIz0ji9f1aYY30xr4W2qZMACM/f:gif/plain/https://images.vocus.cc/dbd1f7e9-0be4-4142-b73e-57b93ac7ee37.gif)

輸入關鍵字「!email」，Raycast 就會替換成對應的文字片段「myemail@gmail.com」

> 本篇文章使用的快捷鍵符號都是使用 Raycast 提供的 Snippet 來進行輸入的：例如 ⌘ 或是 ↩︎ 符號。

![Search Snippets 指令可以搜尋並使用儲存的 Snippet，也可以對 Snippet 有其它操作，例如將選擇的 Snippet 匯出（Export Snippet）](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F5c3dc79d-06c0-492c-bcab-0944de95f710.png&width=740&sign=XNLUeLfOJZqdZro0W8j85FtPr_gJyNiZx2yohO2-OC0) ![zoomable](https://images.vocus.cc/5c3dc79d-06c0-492c-bcab-0944de95f710.png)

Search Snippets 指令可以搜尋並使用儲存的 Snippet，也可以對 Snippet 有其它操作，例如將選擇的 Snippet 匯出（Export Snippet）

你可能會注意到在建立 Snippet 的時候，Raycast 有提示你可以使用 ****Dynamic Placeholders**** ，這是個強大的工具，可以讓 Snippet 使用起來更有彈性。我們來仔細介紹一下。

> Dynamic Placeholders 也可以使用在 Raycast 裡 AI 相關指令，由於 AI 指令不在這篇文章討論範圍，讀者如果有興趣可以自行研究。

### Dynamic Placeholders

Placeholder 指的是 ****預先保留並可被替換的資訊**** ，它的外顯通常是一段符號或文字，並能夠被替換成真正的內容。就像是我們去看戲劇表演時，最前排的位子往往會有所謂的「保留位」或「貴賓席」的標示，這裡的「保留位」或「貴賓席」，就是一種 Placeholder，到了表演開始前，真正能夠使用這些位子的人就會入座，移除掉「保留位」或「貴賓席」標示。

Raycast 的文件（ [Dynamic Placeholders](https://manual.raycast.com/dynamic-placeholders) ）詳細介紹了能夠使用的 Placeholder 與其使用方式。要在 Snippet 裡使用 Dynamic Placeholders 非常簡單，只要在設定 Snippet的時候，在【Snippet】欄位使用

```
{}
```
符號並在
```
{}
```
符號內輸入對應的 Placeholder 名稱，Raycast 就能夠取得相對應的資訊，並在使用 Snippet 進行對應的行為。

![在設定 Snippet 的【Snippet】欄位，只要輸入「 { 」，Raycast 就會跳出可選擇使用的 Placeholder](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fb072d897-9579-47de-90b7-00dedaf35c9f.png&width=740&sign=MbCSHnO0-toiaoawHq0WHjU8QnodXPGiCgvOks4AoI8) ![zoomable](https://images.vocus.cc/b072d897-9579-47de-90b7-00dedaf35c9f.png)

在設定 Snippet 的【Snippet】欄位，只要輸入「 { 」，Raycast 就會跳出可選擇使用的 Placeholder

接下來以 ****Clipboard Text**** 、 ****Argument**** 、 ****Cursor Position**** 這三種 Dynamic Placeholder 為例說明。

假設你所在公司目前正在擴編積極招募員工，而你是公司的招募主管，時常需要主動聯繫潛在職缺候選人，透過 Linkedin 或是電子郵件發送面試邀請。這種面試邀請內容大同小異，但往往需要根據職缺調整部分內容，複製貼上後仍需要花點心力修改。這種有固定模版的內容顯然特別適合使用 Snippet，讓我們來試試看要如何使用。

首先是面試邀請的內容，顯然像是職位名稱、候選人的專長與經歷等等內容非常適合作為 Placeholder。下面是一個面試邀請的範本：

> 【候選人名字】您好，  
> 我是保羅亞崔迪，厄拉科斯公爵，弗瑞曼人救世主。在瀏覽了【候選人資訊來源】後，我對您在【相關技能、經驗或成就】的豐富經歷感到非常印象深刻。我們正在尋找具有您專業背景的人才來擔任我們弗瑞曼人的【職位名稱】職位，我相信您的專長能極大地強化我們的戰力。  
> 厄拉科斯是一個盛產香料的星球。我們重視忠誠與團隊合作，並致力於讓弗瑞曼人打造厄拉科斯成為天堂。  
> 我們非常希望能有機會與您進一步交流，探討您如何能加入並共同推動聖戰，討伐我們的共同敵人並將他們送往天堂。請問您是否有空閒的時間？我們可以安排一次線上會談或您來我們星球參觀，以便更詳細地討論這個職位和您的職涯發展。  
> 感謝您考慮這個機會。我期待著您的回覆！  
> \---  
> 保羅亞崔迪  
> 厄拉科斯公爵，弗瑞曼人救世主

在上面的面試邀請範本中，【】裡的內容都需要根據實際情況而調整，也就適合使用 Placeholder。

- ****Clipboard Text******
	```
	{clipboard}
	```
	**：這個 Placeholder 可以取得當前系統剪貼簿複製的文字內容。我們不希望候選人名字打錯，所以透過複製的方式會是較好的選擇。
- ****Argument******
	```
	{argument name="xxx"}
	```
	**：Argument 允許使用者在使用 snippet 的時候，跳出彈窗輸入 Argument 對應的內容，【職位名稱】與【候選人資訊來源】可以透過 Argument Placeholder 來在使用 Snippet 時進行填寫，並在 Snippet 展開後填入到對應的欄位。
- ****Cursor Position******
	```
	{cursor}
	```
	**：當使用者使用有設定 Cursor Position 的 Snippet 時，Snippet 被替換成對應文字之後，游標就會移動到 Cursor Position 插入的位置，使用者就可以接續編輯。我們把【相關技能、經驗或成就】這裡設定成 Cursor Position，這樣一來，在使用 Snippet 的內容展開後，游標就會跳到這個位置，讓我們可以接續編輯。

下面是原本面試邀請【】欄位替換成 Placeholder 的 Snippet：

> {clipboard}您好，  
> 我是保羅亞崔迪，厄拉科斯公爵，弗瑞曼人救世主。在瀏覽了{argument name="Candidate Info Source"}後，我對您在{cursor}的豐富經歷感到非常印象深刻。我們正在尋找具有您專業背景的人才來擔任我們弗瑞曼人的{argument name="Position"}職位，我相信您的專長能極大地強化我們的戰力。  
> ⋯⋯

![面試邀請的 Snippet 設定。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F9f21fbdd-8b56-415e-baea-523aeca3aa17.png&width=740&sign=HXnmnPRK98KnVaC8I-QXsLPAXuNme_4FVyY3_Rzok6s) ![zoomable](https://images.vocus.cc/9f21fbdd-8b56-415e-baea-523aeca3aa17.png)

面試邀請的 Snippet 設定。

下面展示面試邀請的 Snippet（使用關鍵字「!interview」來執行）：

![剪貼簿已經複製「史帝加」這個名稱，所以 {clipboard} 這個 Placeholder 會被取代。讀者可以注意這些 Placeholder 是如何被使用的。](https://img.vocus.cc/bUuHziLjUUsAnVGibNhYOAa2V8kb87CBlEL6q4ZpGWU/f:gif/plain/https://images.vocus.cc/5a5b9afb-f1ca-4eb9-a649-929c686b7018.gif) ![zoomable](https://img.vocus.cc/bUuHziLjUUsAnVGibNhYOAa2V8kb87CBlEL6q4ZpGWU/f:gif/plain/https://images.vocus.cc/5a5b9afb-f1ca-4eb9-a649-929c686b7018.gif)

剪貼簿已經複製「史帝加」這個名稱，所以 {clipboard} 這個 Placeholder 會被取代。讀者可以注意這些 Placeholder 是如何被使用的。

### Quicklinks

任何你希望可以快速存取的項目都可以是 Raycast 裡的 Quicklink。在上一篇文章裡，我們有體驗了 Search Google 這個預設的 Quicklink 指令。接下來，我們來嘗試自己建立 Quicklink 指令來體驗其強大之處。

如果要建立 Quicklink，我們可以透過輸入「create quicklink」的指令來進入 Quicklink 設定頁面。

![Raycast Quicklink 設定頁面](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F4141c3e9-0cd1-45ce-af8d-27b0f380035f.png&width=740&sign=xO9usV5fqsYM4K-yTOILAZRgqvI9tBQ18LN-Dhh1r_w) ![zoomable](https://images.vocus.cc/4141c3e9-0cd1-45ce-af8d-27b0f380035f.png)

Raycast Quicklink 設定頁面

設定頁面裡的【Name】是之後用來呼叫的指令名稱，【Link】欄位是想要快速存取的項目，可以是網址、資料夾或是檔案，而【Open With】是要用來開啟這個項目的程式。

如果想要建立一個能夠搜尋 YouTube 影片的 Quicklink，該怎麼做？

首先我們需要正確的搜尋網址。在 Youtube 的網頁進行搜尋，比如搜尋「沙丘」，搜尋結果的網址看起來會像這樣：

> https://www.youtube.com/results?search\_query=沙丘

```
search_query
```
後面所接的文字（也就是
```
沙丘
```
），就是想要搜尋的內容。事實上你可以任意修改這邊文字的內容，然後貼到瀏覽器分頁的網址上，網頁就會出現對應的 YouTube 搜尋結果。我們稱這為 ****搜尋參數**** （ ****Query Parameter**** ），由 ****鍵**** （ ****Key**** ，這裡是
```
search_query
```
）與 ****值**** （ ****Value**** ，這裡是
```
沙丘
```
）。

大部分的具有搜尋功能的網站都有這種搜尋參數的設計，透過搜尋參數，我們就可以建立更彈性的 Quicklink 指令。在 Quicklink 的設定介面【Link】欄位上，搜尋參數可以使用

```
{Query}
```
的形式來代表網址搜尋參數的值。
```
{}
```
內的文字不一定要使用「Query」，它只是個 Placeholder，文字內容會在使用指令時作為輸入框提示文字。

搜尋 YouTube 的【Link】如下：

> https://www.youtube.com/results?search\_query={Query}

下面是搜尋 YouTube 影片的 Quicklink 設定：

![我們使用 {Query} 來表示要用來搜尋影片的關鍵字。執行指令後，會透過 Arc 這個瀏覽器開啟網頁並搜尋。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F6ff060f7-3d37-4981-93a1-bf29176ecdb4.png&width=740&sign=lx5qUYCDXhqulZkBwDoz-V9hPhDLe22PCUn8oFxJMNU) ![zoomable](https://images.vocus.cc/6ff060f7-3d37-4981-93a1-bf29176ecdb4.png)

我們使用 {Query} 來表示要用來搜尋影片的關鍵字。執行指令後，會透過 Arc 這個瀏覽器開啟網頁並搜尋。

使用指令的方式就如同之前提到的 Search Google 指令，由於我們替指令取名為 「Search YouTube」，輸入名稱就可以叫出指令，當然也可以設定別名。這邊設定「yt」為別名，並使用「yt」呼叫出指令，接著就可以在輸入框（提示文字為「Query」）輸入想要搜尋的關鍵字進行影片搜尋。

![在輸入框內輸入的文字會取代原本 Quikclink 設定【Link】的 {Query} 字串](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F9e31b233-4170-4f4a-b92d-54774f1a434b.png&width=740&sign=aZohb2TdXeqH18Ft1Jcyixd3DIx6zQpit_EbO5yMCCQ) ![zoomable](https://images.vocus.cc/9e31b233-4170-4f4a-b92d-54774f1a434b.png)

在輸入框內輸入的文字會取代原本 Quikclink 設定【Link】的 {Query} 字串

現在，嘗試輸入影片關鍵字後按下

```
↩︎
```
，應該就會看到瀏覽器跳出 YouTube 頁面並顯示搜尋結果。

> 試試看如何透過 Quicklink 建立搜尋 Google 地圖的指令？

Quicklink 的【Link】欄位並非只能輸入網址，也可以是檔案或資料夾路徑。我們可以設定【Link】為「~/Downloads」，【Open With】為「Finder」，這樣一來，就可以快速打開 Downloads 資料夾。

![設定 Downloads 資料夾 Quicklink。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fbce0fdec-d03b-47f2-b5d7-e107f2acc289.png&width=740&sign=j3zhG-IRJwBDB0XHn2mXW-KKGpUiC8IttKM0898-Okg) ![zoomable](https://images.vocus.cc/bce0fdec-d03b-47f2-b5d7-e107f2acc289.png)

設定 Downloads 資料夾 Quicklink。

![輸入「downloads」，就會出現 Downloads 的 Quicklink 指令，按下 ↩︎ 就可以開啟 Downloads 資料夾。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F3b82e63c-9fd9-4771-9b02-938adc48df29.png&width=740&sign=32Ne4VbnE_Jss242VvZmh_Gfylh-R97S14AZPbPFPn0) ![zoomable](https://images.vocus.cc/3b82e63c-9fd9-4771-9b02-938adc48df29.png)

輸入「downloads」，就會出現 Downloads 的 Quicklink 指令，按下 ↩︎ 就可以開啟 Downloads 資料夾。

當【Link】的內容是網址時，【Open With】未必只能使用瀏覽器開啟，有些應用程式也可以接受開啟網頁內容。例如假設你有一個固定工作時會播放的 Spotify 播放清單，可以找到它的清單連結（它可能長得像這樣： [https://open.spotify.com/playlist/6qtR2ndjmYNdVUBISJHpB5?si=9891aa109c4e4962](https://open.spotify.com/playlist/6qtR2ndjmYNdVUBISJHpB5?si=9891aa109c4e4962) ），在 Quicklink 裡設定【Link】，【Open With】設定為 Spotify 應用程式，就可以透過 Raycast 快速在 Spotify 裡開啟這個清單。

雖然我們可以透過 Quicklink 來使用應用程式來打開連結，執行生產力工具的功能，不過如果想要更深度整合生產力工具的功能，更好的方式會是使用第三方擴充。

---

## Raycast 第三方擴充使用介紹

Raycast 內建功能上手之後，已經能夠顯著提升我們的生產力。內建功能主要針對常見的工作流程，但使用者很可能會使用各種不同的生產力工具，這些生產力工具有各式各樣五花八門的功能。

如果能夠將工具服務的功能直接整合進 Raycast 使用，就不需要打開服務的頁面，可以運用鍵盤指令，直接執行常用功能，像是詢問 GhatGPT 問題、在 Twitter 發佈推文、用 Jira 更改工作任務狀態、在 GitHub 上發個 PR 等等。而有些功能即使可以透過 Quicklink 達成，整合進 Raycast 會更方便，例如在 Raycast 裡搜尋 YouTube 影片，並直接在 Raycast 查看搜尋結果。

這些進一步專屬特定工具的指令，就可以透過第三方擴充來達成。

### Raycast 擴充商店（Extension Store）

我們可以從 Raycast 的官方網頁進入擴充商店： [Raycast Store](https://www.raycast.com/store) 。

![Raycast 擴充商店首頁示意圖](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F8cd04a47-aa05-42b3-8fc1-33733d15e117.png&width=740&sign=JZ04FAXTUQ-k1RdZvoE2gLrMkxdrU9QeUJac15H5QT4) ![zoomable](https://images.vocus.cc/8cd04a47-aa05-42b3-8fc1-33733d15e117.png)

Raycast 擴充商店首頁示意圖

擴充商店可以搜尋與瀏覽擴充，點擊有興趣的擴充，就可以前往擴充的詳細介紹頁面。

![Raycast 擴充商店展示了琳瑯滿目的擴充。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F4e1d838b-ba61-4276-ac17-1488a4077f89.png&width=740&sign=5-5nQoVnBMkNvkgo0xhWBiYYeco1BH03Jcx4uCvzAdA) ![zoomable](https://images.vocus.cc/4e1d838b-ba61-4276-ac17-1488a4077f89.png)

Raycast 擴充商店展示了琳瑯滿目的擴充。

每個擴充的頁面也會介紹擴充的用途與指令，以及相關資訊（例如開發者、下載次數、擴充版本編號、注意事項等等），按下【Install Extension】按鈕，就會引導到 Raycast 的視窗進行安裝。

![擴充詳細頁面展示了擴充的示意圖、指令與其它相關資訊。上圖為 Spotify Player 擴充，使用者可以用來與 Spotify 整合，在 Raycast 裡進行 Spotify 相關操作。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fa8d48db4-84ef-434e-a1f1-753c444a287f.png&width=740&sign=bOrzedaHnlSxL2O6Cw8r-88609f-W0C9bugqhiir00A) ![zoomable](https://images.vocus.cc/a8d48db4-84ef-434e-a1f1-753c444a287f.png)

擴充詳細頁面展示了擴充的示意圖、指令與其它相關資訊。上圖為 Spotify Player 擴充，使用者可以用來與 Spotify 整合，在 Raycast 裡進行 Spotify 相關操作。

我們也可以在 Raycast 的視窗中輸入「store」指令開啟擴充搜尋的功能，可以直接在 Raycast 裡進行擴充的搜尋與瀏覽，也可以直接進行安裝。

![在 Raycast 裡輸入「store」指令就可以進入擴充搜尋頁面。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F92a6034a-74cb-4c54-9649-decf9f679e0f.png&width=740&sign=0q7LRO1dHPr5V_T-nEnboLvL_N26qi6Nj7uoEEpOSvU) ![zoomable](https://images.vocus.cc/92a6034a-74cb-4c54-9649-decf9f679e0f.png)

在 Raycast 裡輸入「store」指令就可以進入擴充搜尋頁面。

接下來，我們以大家在工作上通常會操作 Spotify 的情境出發，以 Spotify Player 這個擴充為例，介紹這些第三方擴充的使用。

### 第三方擴充上手體驗

首先我們來安裝 [Spotify Player](https://www.raycast.com/mattisssa/spotify-player) 這個擴充。在 Raycast 裡使用 Store 指令搜尋「Spotify Player」，檢視內容並安裝。

![Spotify Player 擴充的頁面，按下 ↩︎ 就可以安裝擴充。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F46346344-61ec-4a8f-80ac-77b42eac5835.png&width=740&sign=08sP_TfjMBehJpR97cfARWEO2gZZVHqWvFKDTaZJgr4) ![zoomable](https://images.vocus.cc/46346344-61ec-4a8f-80ac-77b42eac5835.png)

Spotify Player 擴充的頁面，按下 ↩︎ 就可以安裝擴充。

安裝完成後，我們就可以來看看 Spotify Player 提供了哪些指令。到 Raycast 主視窗搜尋「Spotify Player」，就會列出一系列的 Spotify Player 指令。

![Spotify Player 擴充提供的指令。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fb76bbbf4-dc88-49ac-8ad7-89cd93acb72f.png&width=740&sign=DhL4LATaHuj08jhXIjGULHCwyZGgQ8QVMOWX0hAMHJQ) ![zoomable](https://images.vocus.cc/b76bbbf4-dc88-49ac-8ad7-89cd93acb72f.png)

Spotify Player 擴充提供的指令。

安裝的擴充預設不一定會開啟所有指令，使用者可以開啟設定裡的 Extensions 頁面，開啟想要使用的擴充指令。當然也可以關閉不想使用的指令。

![使用者可以開啟或關閉擴充提供的指令，以符合自己的需求。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fb0e2ecde-aecd-42c0-b739-3df612e0763d.png&width=740&sign=QRJTXf753DrWIOROM8VXmITYK8q0Ck1TIj0P5Q2sYtc) ![zoomable](https://images.vocus.cc/b0e2ecde-aecd-42c0-b739-3df612e0763d.png)

使用者可以開啟或關閉擴充提供的指令，以符合自己的需求。

任意選擇其中一個指令，Raycast 就會引導使用者登入 Spotify，並授權 Spotify Player 擴充存取 Spotify 的權限。

![Spotify Player 會引導使用者登入 Spotify 以取得權限來控制 Spotify。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F222ad75a-29c2-4656-abae-0a29c4ebdbef.png&width=740&sign=xIC59fVTgWsbL4gDUfbUTvoUUJ-fn6mlW7PM0IxBeMs) ![zoomable](https://images.vocus.cc/222ad75a-29c2-4656-abae-0a29c4ebdbef.png)

Spotify Player 會引導使用者登入 Spotify 以取得權限來控制 Spotify。

> 如果安裝的 Raycast 擴充的功能是需要對應服務的使用者權限的話，Raycast 都會引導進行相關的登入與設定。Raycast 的擴充統一管理在 Raycast 的程式碼倉庫（GitHub Repository： [raycast/extensions](https://github.com/raycast/extensions) ），這些擴充的程式碼都是公開並且經過審核，確保不會有隱私疑慮。即使如此，建議使用者在安裝擴充時，仍應該仔細檢視擴充可能要求了哪些權限，權限資訊是否只有在本地端儲存與使用，並評估是否要給予這些權限，以及考量萬一個資外洩的的後果是否能夠承擔。儘量做好風險評估後，再使用這些擴充。在使用 Spotify Player 這個擴充時，登入是透過 Spotify OAuth 登入，擴充不會取得我的登入帳密，只會透過 Spotiy 的授權拿取我的資訊。我的資訊包括 Spotify 訂閱資訊、播放清單、播放歷史紀錄等等。我認為取得這些資訊沒有太大的隱私疑慮，所以可以放心使用。

Spotify Player 取得權限後，就可以試試看它所提供的指令了。例如 Search 指令：

![Spotify Player 提供的 Search 指令。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F70d9ed38-b010-4b87-b27b-ba89f6559491.png&width=740&sign=f7zqmspHAuAsYG1srz_ldnSsdbJ_93zfakHqYxwC-j0) ![zoomable](https://images.vocus.cc/70d9ed38-b010-4b87-b27b-ba89f6559491.png)

Spotify Player 提供的 Search 指令。

我們可以搜尋 Spotify 上的音樂，選擇搜尋結果，按下

```
↩︎
```
就可以直接播放。也可以按下
```
⌘
```
```
K
```
查看更多操作。

![Spotify Player 的 Search 指令功能頁面。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F977b7615-1232-4333-ae03-4ac89e00b25a.png&width=740&sign=EUvytvTrxSxsW5dcipoxdEGH6P3qoW6NuT-L25Zlhbk) ![zoomable](https://images.vocus.cc/977b7615-1232-4333-ae03-4ac89e00b25a.png)

Spotify Player 的 Search 指令功能頁面。

也可以用 Your Library 指令搜尋儲存的歌曲、專輯或播放清單。

![Spotify Player 的 Your Library 指令功能頁面。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fba4ab42b-2948-421a-81e6-65969571ebe6.png&width=740&sign=G99Nkh37RDc3RplS1HjlE1xK9qtzyx1whZN1YuDA0Dw) ![zoomable](https://images.vocus.cc/ba4ab42b-2948-421a-81e6-65969571ebe6.png)

Spotify Player 的 Your Library 指令功能頁面。

從 Spotify Player 的例子可以看到，第三方的擴充安裝與使用都很容易，操作邏輯也都與核心功能的操作邏輯類似，只需要自行摸索並嘗試，就都可以上手。

下面是一些擴充的例子：

軟體工程師離不開的 [GitHub](https://www.raycast.com/raycast/github) ，可以搜尋 Repository、PR、Issue 等等 。

![GitHub 擴充的指令。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F0c4fff8e-2a25-4975-bc10-7e71de1102fa.png&width=740&sign=a0DD3g8MytPcgTHLuouzJ7bNNtAdcywWCrKZ01Wr93A) ![zoomable](https://images.vocus.cc/0c4fff8e-2a25-4975-bc10-7e71de1102fa.png)

GitHub 擴充的指令。

專案管理工具 [Jira](https://) ，可以方便的搜尋任務（Issue），並可以直接在 Raycast 中修改任務的資訊。

![Jira 擴充的指令。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F574b3b1e-df33-4332-9ec0-3921536e62c5.png&width=740&sign=nJ6xpmDnV-9UG_t7xGAn5Ea4Up87jR3zIviN4cFQW4o) ![zoomable](https://images.vocus.cc/574b3b1e-df33-4332-9ec0-3921536e62c5.png)

Jira 擴充的指令。

![可以在 Jira 內打開 Issue 進行常用的操作。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Ffcfb1dfd-8d39-4957-b4e9-3164cc67457b.png&width=740&sign=D2t_9TNjRFAt5q9RUnADN3LU-9d4Wakkyq6qxGuI1dc) ![zoomable](https://images.vocus.cc/fcfb1dfd-8d39-4957-b4e9-3164cc67457b.png)

可以在 Jira 內打開 Issue 進行常用的操作。

[Google Search](https://www.raycast.com/mblode/google-search) 擴充，與 Quicklink 設定的指令不同，在搜尋介面進行搜尋，可以自動建議搜尋內容。

![Google Search 擴充的搜尋介面。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fb2a9b9c6-4751-4794-8b75-86832f9306a7.png&width=740&sign=IR_uKXONuosMtK_4wXj2hP5v0Rz13Ib_pgEi-83lAdM) ![zoomable](https://images.vocus.cc/b2a9b9c6-4751-4794-8b75-86832f9306a7.png)

Google Search 擴充的搜尋介面。

[GIF Search](https://www.raycast.com/josephschmitt/gif-search) ，可以方便快速的搜尋各種有趣的 GIF，然後方便的複製使用。

![GIF Search 的搜尋功能。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F82294da4-a54e-4a7f-b374-f357a204c100.png&width=740&sign=hxzVCTkPCwjQu50Xcz4hhvaDD1Mk8t7IKnp0yotDFUc) ![zoomable](https://images.vocus.cc/82294da4-a54e-4a7f-b374-f357a204c100.png)

GIF Search 的搜尋功能。

當然也有搭配 [ChatGPT](https://www.raycast.com/abielzulio/chatgpt) 的擴充：

![ChatGPT 擴充提供的 Ask Question 功能，可以直接在 Raycast 視窗詢問問題。](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Ff84286d6-d31b-432e-8ce9-d71f5abac937.png&width=740&sign=p5yUNp7W5mqMp88CQgli9QYTt0DAysLOrWwctTYZyRs) ![zoomable](https://images.vocus.cc/f84286d6-d31b-432e-8ce9-d71f5abac937.png)

ChatGPT 擴充提供的 Ask Question 功能，可以直接在 Raycast 視窗詢問問題。

其它還有一些不是與生產力工具結合，但是有趣或實用的小功能：像是查看天氣 [Weather](https://www.raycast.com/tonka3000/weather) 、快速檢視系統狀態的 [System Monitor](https://www.raycast.com/hossammourad/raycast-system-monitor) 、開啟螢幕保護程式 [Screen Saver](https://www.raycast.com/koinzhang/screen-saver) 、可以任意選擇畫面上顏色的 [Color Picker](https://www.raycast.com/thomas/color-picker) 等等。

還有許多形形色色的擴充，由於每個人的工作流程都不盡相同，使用的生產力工具也不一樣，讀者可以自行探索，將適合的擴充整合自己的工作流程。

### 擴充讓 Raycast 成為真正的生產力平台

如果其它生產力工具不能夠與啟動器有更深度的整合，「情境切換」的認知負擔能夠減少的程度就會受限於啟動器既有的核心功能。以前面操作 Spotify 為例，Raycast 的核心功能只能夠讓使用者快速呼叫 Spotify 應用程式，或是使用 Quicklink 建立搜尋音樂或打開音樂清單的連結，但如果你希望更進一步，直接在 Raycast 顯示音樂搜尋結果，或是在 Raycast 直接播放你的音樂清單，就無法做到。

而開發者貢獻的第三方擴充，它們補上了更深度的整合，讓其它的生產力工具看起來就像是 Raycast 的一部分，我們可以更少移動滑鼠，不需要頻繁切換到其它畫面然後花時間適應，操作過程也更加一致。在 Spotify Player 擴充搜尋音樂的例子裡，從搜尋音樂到進行播放，都不需要手動開啟 Spotify，不需要切換到 Spotify 的畫面；操作都是透過鍵盤指令完成，不需要使用滑鼠；操作的介面與方式也與原本使用 Raycast 的指令一致。

第三方擴充的設計可以說是 Raycast 這一類啟動器工具的最後一哩路。在之前的文章中曾經提到，一個好用的啟動器必須要是能夠客製化來滿足自己的工作流程。Raycast 的擴充商店就像是 iOS 的 App Store，讓 Raycast 不再只是一個工具，而是一個生產力平台 。常見生產力工具的擴充幾乎都可以找到：ChatGPT、GitHub、Jira、Slack、Teams、Safari、Visual Studiio Code、Figma、Notion 等等族繁不及備載，我們可以任意組合這些擴充，打造屬於自己的高效率工作流程。

---

## 最後，關於生產力⋯⋯

第一次接觸到 Raycast 的時候，可能會對眾多的擴充感到躍躍欲試，不過 ****提高生產力應該是手段，而非目的**** 。避免「 [生產力上癮](https://www.bbc.com/worklife/article/20200807-when-productivity-becomes-an-addiction) 」，不要過度安裝擴充或執著於將所有工作流程連結到 Raycast，以免像是使用了一堆生產力工具一樣，最後造成混亂與效率低落。

****提高生產力是為了讓專注力能夠放在重要的工作項目上**** 。查看專案進度的效率提高，不代表我們應該更頻繁的查看專案進度。不要落入「 [生產力陷阱](https://www.bbc.com/worklife/article/20210805-how-to-escape-the-productivity-trap) 」，節省的時間與專注力應該用在更有意義的事情上，而非完成更多瑣碎繁雜的工作內容。

另外，雖然上手 Raycast 後確實可以在電腦工作上帶來效率的提升，但是生產力工具的使用也只佔了工作時間的一部分。當團隊因為某些原因，專案管理效率低落，以致於在一整天的工作時間內，仍然必須查看數十次專案與任務內容、或是在 Email 與通訊軟體之間來回切換上百次，就只為了搞清楚現在專案是怎麼回事的時候，減少電腦操作其實幫助有限。Raycast 也許可以提高查看專案進度的效率，但當不是核心的工作項目已經過於繁雜瑣碎時，這時候該做的事情是 ****從源頭減少無意義的工作項目**** ，甚至重新思考自己職位與工作的意義。

這系列標題「超級生產力」可能有些誇張，使用 Raycast 並不會讓工作速度上升十倍，但它確實針對我在第一篇文章（ [使用 Raycast 打造超級生產力（一）- 為什麼我們需要 Raycast？](https://vocus.cc/article/661943e1fd897800016eb0b9) ）所提到的生產力問題，提出了解決方案： ****減少不必要的操作，讓我們可以專注在重要的工作項目上**** 。這也是我認為生產力的核心精神： ****好的生產力並不只是在短時間內提高工作產出，而是可以將時間與專注力投注在真正有意義的事情上**** 。

希望各位都能夠打造高效率，並且有意義的工作流程，建立起真正的超級生產力。