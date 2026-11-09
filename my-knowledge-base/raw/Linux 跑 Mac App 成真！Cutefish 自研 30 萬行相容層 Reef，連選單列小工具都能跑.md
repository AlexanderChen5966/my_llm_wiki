---
title: "Linux 跑 Mac App 成真！Cutefish 自研 30 萬行相容層 Reef，連選單列小工具都能跑"
source: "https://www.kocpc.com.tw/archives/700494"
author:
  - "[[達小編]]"
published: 2026-10-24
created: 2026-11-05
description: "Linux 桌面環境 Cutefish 宣布能跑原封不動的 Mac App，連選單列小工具都支援。關鍵是自研相容層 Reef，超過 30 萬行程式碼、70 多個框架，堪稱「Mac 版 Wine」，官方實測 TextMate、Hex Fiend、Itsycal 皆可運作。"
tags:
  - "clippings"
status: "processed"
---
以「長得像 macOS」出名的 Linux 桌面環境 Cutefish，10 月 23 日在官方部落格丟出一顆震撼彈：Cutefish 現在能跑 Mac 應用程式。不只一般視窗軟體，連住在選單列時鐘旁邊的小工具（menu bar apps）都跑得起來。Mac App 在 Cutefish 上開成一般視窗、選單進 Cutefish 的選單列、圖示出現在 Dock，跟桌面上任何其他應用沒兩樣。官網還特別聲明：示範截圖裡每一個 App 都是 Mac App，他們沒有修改、沒有重新編譯任何一隻，就是 Mac 用戶平常下載的同一個檔案。這項能力背後是 Cutefish 自研的相容層 Reef，官方對它的定位很直白：把 Reef 理解成「Mac 版的 Wine」。  
![Linux 跑 Mac App 成真！Cutefish 自研 30 萬行相容層 Reef，連選單列小工具都能跑 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/10/20261024031837_0_bcdfd0.jpg "Linux 跑 Mac App 成真！Cutefish 自研 30 萬行相容層 Reef，連選單列小工具都能跑 - 電腦王阿達")

### 為什麼難：Mac App 自己什麼都不會

Mac App 單獨存在時做的事非常少。要開一個視窗、顯示選單、畫一行字、算出今天幾號，它通通呼叫 macOS：視窗與控制項找 AppKit、文字日期檔案找 Foundation、繪圖找 Core Graphics、新介面找 SwiftUI，還有幾十個其他框架。這些東西在 Linux 上一個都不存在。

要跑 Mac App，就得把整套東西自己做出來，而且行為要跟 macOS 一模一樣，差一點點，App 就會視窗排版錯誤、畫面空白或直接崩潰。Cutefish 的答案叫 Reef：超過 30 萬行程式碼、橫跨 70 多個框架，光 AppKit 與 Foundation 就各自超過 7 萬行。底下還墊著 Mac 程式的載入器（Mac 用的執行檔格式是 Mach-O，跟 Linux 不同）、一套 Objective-C 執行環境、Swift 支援與底層系統函式庫。就算做到這個程度，覆蓋的還是 macOS 的一小部分，那套系統有數以萬計的函式等著被呼叫。

### 怎麼磨出來的：一隻 App 一隻 App 來

沒人事先知道某個 App 會用到 macOS 的哪些部分，所以沒辦法一次寫完。Cutefish 的做法是逐隻擊破：跑一個真的 Mac App，看它死在哪，缺哪個函式、視窗哪裡畫錯、哪顆按鈕沒反應；研究 Mac 在那個情況下的行為，把它實作成通用功能，不做單一 App 的專用補丁，加上測試，再跑一次。每解決一個 App 都讓下一個更容易分析，因為大家共用的東西佔大部分。不過還剩多少沒做，官方坦言真的不知道，下一個 App 隨時可能伸手去抓一個沒人用過的介面。

官方列了幾個踩過的坑，每個都很有畫面：

- 下拉選單跟旁邊的標籤應該共用文字基準線，他們量基準線的邊跟 macOS 不一樣，設定視窗裡的下拉選單就整整偏了幾點
- macOS 會自動幫檔名結尾是「Template」的圖片染上色調，他們漏了這項染色，某隻 App 的視窗按鈕畫出來變成粗黑圈圈
- 視窗還在準備繪製時要求重繪，不該觸發另一次重繪；他們的程式會，一隻閒置的視窗一秒重畫一百多下
- App 會檢查自己跑在哪個 macOS 版本上並改變行為，所以每個版本差異都是抉擇點
- 比較新的 App 用 SwiftUI，內部實作蘋果沒公開，他們只能看文件做出來，再跟真 Mac 上的輸出逐像素比對

### 跑起來是什麼樣子

Mac App 在 Cutefish 上走的是同一套桌面基底：圖形工具箱畫它們的視窗、文字與控制項，合成器給它們跟其他視窗一樣的陰影、外框與動畫，shell 把它們的選單放進 Cutefish 選單列、圖示放進 Dock、條目放進啟動器。

選單列 App 是最大的考驗。在 Mac 上，Itsycal 會在選單列畫一顆帶日期的小圖示，點下去掉出月曆。Cutefish 為此設計了狀態列項目支援：App 自己畫項目、寬度隨它要，彈出視窗就開在正下方。結果 Itsycal 跑起來完全正常，狀態列日期、月曆彈窗、右鍵選單、設定視窗全部到位。  
![Linux 跑 Mac App 成真！Cutefish 自研 30 萬行相容層 Reef，連選單列小工具都能跑 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/10/20261024032106_0_9a7148.jpg "Linux 跑 Mac App 成真！Cutefish 自研 30 萬行相容層 Reef，連選單列小工具都能跑 - 電腦王阿達")

其他實測：老牌文字編輯器 TextMate、十六進位編輯器 Hex Fiend 都能開檔編輯，官方還幽了一默，截圖裡 Hex Fiend 正在檢查一個 Linux 程式。待辦事項軟體 Things 3 跑得起來，部分介面由他們自家的 SwiftUI 繪製，還沒全部完成。  
![Linux 跑 Mac App 成真！Cutefish 自研 30 萬行相容層 Reef，連選單列小工具都能跑 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/10/20261024032209_0_6bef3c.jpg "Linux 跑 Mac App 成真！Cutefish 自研 30 萬行相容層 Reef，連選單列小工具都能跑 - 電腦王阿達")

  

### 程式碼從哪裡來：合法邊界畫得很細

這套東西最敏感的就是「有沒有動到蘋果的東西」。官方說明 Reef 全部是自家程式碼，沒有使用 Darling 或 Wine 的程式碼，不拆解也不複製蘋果關閉原始碼的 macOS 軟體，更沒有用蘋果的開發套件。需要知道行為怎麼運作時，他們讀蘋果的官方文件、觀察 App 要什麼、再跟同一隻 App 在真 Mac 上的表現對照（也就是所謂的「 [無塵室設計](https://www.kocpc.com.tw/archives/671475) 」）。

有拿來用的，是蘋果自己開源的平台元件，依原授權收進 Reef：Core Foundation 來自 swift-corelibs-foundation（Apache 2.0）、App 背景排程用的 Grand Central Dispatch 來自 swift-corelibs-libdispatch（Apache 2.0）、Swift 標準庫與執行環境來自 swiftlang/swift（Apache 2.0）、蘋果的 LZFSE 壓縮格式來自 lzfse 專案（BSD 3-Clause）。至於 objc4、dyld、xnu、Libc 這些蘋果開源的底層專案，只讀來理解 Mac 的行為，不複製裡面的程式碼。Reef 的 Objective-C 執行環境與載入器都是自己寫的，另外還收了 LLVM 的 libc++、libunwind、SQLite、curl、Tcl 等開放原始碼元件。

### 跟 Darling、Wine 比，差在哪

在 Linux 跑 macOS 軟體，最有名的專案是 [Darling](https://github.com/darlinghq/darling) ；Wine 則幫 Windows 軟體做了幾十年同樣的事。Wine 的名言是「我不是模擬器」，它重實作 Windows 的程式介面，Reef 對 Mac 走一樣的路線，官方自己下的註解：把 Reef 想成「Mac 應用的 Wine」。  
![Linux 跑 Mac App 成真！Cutefish 自研 30 萬行相容層 Reef，連選單列小工具都能跑 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/10/20261024032643_0_f3915e.jpg "Linux 跑 Mac App 成真！Cutefish 自研 30 萬行相容層 Reef，連選單列小工具都能跑 - 電腦王阿達")

不過 Darling 的目標是跑一般的 macOS 軟體，在命令列工具與底層系統上領先 Reef 很多，而且任何 Linux 桌面都能用；Wine 的 Windows 軟體覆蓋率更是巨大。Reef 只做圖形介面的 Mac App、只活在 Cutefish 上、圍繞自家的工具箱與合成器打造，命令列工具不列入目標 [\[1\]](https://cutefish.org/blog/running-mac-apps/) 。官方對兩個社群的成果都表達敬意。蘋果生態的相容話題近來很熱，我們也曾報導 [讓 Intel 老 Mac 跑 arm64 應用的「反向 Rosetta」工具 Maclator](https://www.kocpc.com.tw/archives/671959) ，方向相反但一樣是在生態夾縫裡找路。

<iframe sandbox="allow-scripts" title="反向 Rosetta 來了：Maclator 讓 Intel 老 Mac 與黑蘋果運行 arm64 App — 電腦王阿達" src="https://www.kocpc.com.tw/archives/671959/embed#?secret=wEtiSrU3k8#?secret=TeHbneGAMX" width="500" height="241" frameborder="0"></iframe>

### Cutefish 的下一步

先講一個社群最在意的：Reef 暫時不開源。官方的理由是這筆投資很大，而且是 Cutefish 的差異化功能，成長期需要被保護；未來開放是有可能的，例如社區贊助能支撐這項工作之後。

功能面的限制也列得很誠實：Things 3 還沒跑完、SwiftUI 支援要補更多，讓現代 Mac App 都能動；再下一個大目標是 web 引擎，因為很多 App 依賴它。整個方案只支援 Cutefish 桌面，而 Cutefish 的前身就是 2021 年在開源圈走紅的 CutefishOS，當年以 KDE 框架、Qt 打造出神似 macOS 的桌面環境，被翻成 40 多種語言，在國外 Linux 社群相當受歡迎。如今官方把定位改成「美麗的 agentic Linux 發行版」，還留著 [社群版下載站](https://cutefish-ubuntu.github.io/) 。  

### 小結

Reef 的野心，是在 Linux 上把「Mac 的軟體資產」整包搬進來：不用說服開發者移植，直接跑他們 release 出來的原始檔案。30 萬行相容層換來 Itsycal 的月曆能點、TextMate 能改稿、Hex Fiend 能看十六進位，工程量不小，也不是現在比較多人玩的「黑蘋果」。接下來的觀察點很明確：SwiftUI 補完度、web 引擎進度，以及社群贊助能不能把這項閉源技術推向開放。總之未來想在 Linux 用生態更完整的 Mac 軟體，似乎不是那麼遙不可及了。

**資料來源： [Cutefish 官方部落格](https://cutefish.org/blog/running-mac-apps/)**