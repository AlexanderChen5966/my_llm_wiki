---
title: "開源專案 UniXNU 讓小米平板 6S Pro Android 平板成功原生運行 iPadOS 17"
source: "https://www.kocpc.com.tw/archives/700251"
author:
  - "[[達小編]]"
published: 2026-10-23
created: 2026-11-03
description: "這個專案的正式名稱叫 UniXNU，由開發者 sharky_dev 與 bluse50 主導，原始貼文 10 月 19 日發出，說得很直接：「我們團隊在搭載 Snapdragon 8 Gen 2 晶片的小米平板 6S Pro 上，成功讓 iPadOS 17"
tags:
  - "clippings"
status: "inbox"
---
由於 AI 的加持，現在各種系統的移植與模擬變的比以前簡單很多，如之前介紹過的 [**AnyPS5**](https://www.kocpc.com.tw/archives/tag/anyps5) 、 **[HackiOS](https://www.kocpc.com.tw/archives/tag/hackios)** 等之類都是，最近在網路上又出現一個全新專案「UniXNU」，可以將 Android 版安裝 iPadOS 系統。目前不管是滑動、開啟設定、拉下通知中心，操作都跟真的 iPad 沒兩樣。不到 48 小時，同一個開發團隊再發一支短片，這次宣告 GPU 驅動也通了，畫面由高通 Adreno 740 硬體直接合成。  

這個專案的正式名稱叫 UniXNU，由開發者 sharky\_dev 與 bluse50 主導，原始貼文 10 月 19 日發出，說得很直接：「我們團隊在搭載 Snapdragon 8 Gen 2 晶片的小米平板 6S Pro 上，成功讓 iPadOS 17 原生執行。」光是「原生」兩個字就足以讓整個圈子炸開，因為另一個打著類似功能的專案 HackiOS，至今連一行程式碼都沒釋出。

![](https://x.com/i/status/2112118162057785396)

### 先把話說清楚：這不是模擬器，也不是換皮

玩機圈講「安卓跑 iOS」，十有八九是那些套殼 App 或改主題的偽作。UniXNU 走的是完全不同的路徑，官方文件的分層模型寫得明白：Android 的 ABL 開機載入 Android boot image，裡頭放的是 BootShim 加 UEFI 韌體卷與裝置樹，UEFI 交棒給 UniXNU 這個開機管理員，再由 DarwinBoot 載入 XNU 核心，最後跑的是從 IPSW 裡原封不動抽出來的 iPadOS 使用者層。  
![開源專案 UniXNU 讓小米平板 6S Pro Android 平板成功原生運行 iPadOS 17 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/10/20261023204629_0_d21118.jpg "開源專案 UniXNU 讓小米平板 6S Pro Android 平板成功原生運行 iPadOS 17 - 電腦王阿達")

整個技術棧分六層，每一層都有獨立的 Git 倉庫、建置產物和交接協定，官方形容自己是「類似 OpenCore 的統一引導程式」。PC 黑蘋果世界靠 OpenCore 讓一般 x86 電腦載入 macOS 的 XNU 核心，UniXNU 想做的事就是把高通 ARM 裝置的硬體差異抽象掉，換成可以重複使用的基礎設施。

不過有一個容易被忽略的細節也要講，該團隊演示的系統版本是 iPadOS 17.7.11，來源為 2017 年 12.9 吋 iPad Pro 第二代（代號 iPad7,1）的原版 IPSW，一個位元組都沒改，並不是最新的 iPadOS 26。

### 最硬的一關：把 16 KiB 頁塞進 4 KiB 的硬體

Apple 自己的 ARM64 核心在多數 SoC 上用 16 KiB 的記憶體頁，而 Snapdragon 平台用的是 4 KiB。這不是改個常數就能解決的事，UniXNU 為此改了 XNU 核心本身。 [官方文件](https://unixnu.com/zh/xnu/four-k-page-tables) 列出的修補範圍包括啟動頁表容量、執行時期頁大小選擇、使用者位址空間映射、程式碼簽章雜湊頁大小，甚至連配置器的對齊都要另外解碼處理。

![開源專案 UniXNU 讓小米平板 6S Pro Android 平板成功原生運行 iPadOS 17 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/10/20261023205740_0_bbb74a.jpg "開源專案 UniXNU 讓小米平板 6S Pro Android 平板成功原生運行 iPadOS 17 - 電腦王阿達")

最棘手的是程式碼簽章。Apple 的 CodeDirectory 是以 16 KiB 為單位做雜湊，4 KiB 核心卻要按 4 KiB 驗簽。UniXNU 的做法是先用原始的 16 KiB 區塊驗過，再從中衍生出每個 4 KiB 頁的摘要，存進核心的唯讀記憶體當快取給簽章驗證函式查表。整個過程沒有虛擬化，也沒有用 Shadow Page Tables。

### GPU 才是門檻，而它只花了兩天

開進桌面是容易的，畫面才是難關。iPadOS 的介面合成與應用程式繪圖全走 Metal，沒有 GPU 驅動的話系統會退回去用 CPU 運算，速度慢到不能用，應用程式也沒有繪圖介面可用。10 月 19 日首支影片發布時，官方自己的說法是「因為還缺 GPU 驅動，系統極度卡頓」，並預告三天內搞定。10 月 20 日深夜，第二支影片釋出，標題就寫「小米平板 6S Pro iPadOS 完成 GPU 驅動」，描述提到這是「第一個帶有 Metal GPU 驅動的已開源第三方 ARM 裝置 iOS 執行專案」。官方 README 也同步更新，宣布 render server 已經能透過 Adreno 740 合成系統介面，列入清單的行程各自拿到自己的 Metal 裝置。

<iframe src="https://player.bilibili.com/player.html?isOutside=true&amp;aid=117473965768925&amp;bvid=BV13NnK6dEQz&amp;cid=42905830299&amp;p=1" width="100%" height="600" frameborder="no" allowfullscreen="allowfullscreen"><span data-mce-type="bookmark" style="display: inline-block; width: 0px; overflow: hidden; line-height: 0;" class="mce_SELRES_start"></span></iframe>

### 開源開得很快，但硬體支援還很空

三個倉庫全部公開：主倉庫 UniXNU 目前 475 星、44 個 fork，裝置倉庫 UniXNU-sheng 12 星，UEFI 韌體倉庫 uefi-sheng 6 星，全部在 10 月 19 日建立，10 月 20 日還在持續提交。官方狀態表寫得很保守，沒有任何一項被誇大：觸控、顯示、UFS 儲存、GPU 標示可用，Wi-Fi、藍牙、相機、USB、音訊、感測器、電池驅動、休眠喚醒全部標示不可用。這台平板跑起 iPadOS 之後 **連不上網路** 。pbxscience 的整理也點出，目前只支援這一款裝置，其他主機板都需要各自的移植，韌體階段還會隨機卡住需要重試。

想自己試的人也有門檻。官方文件列出的建置環境要在 macOS 上跑，需要 Xcode、Python 3.11 以上、LLVM 與 lld、mtools、dtc 與 fastboot，而且目標 IPSW、trust cache 套件、平台種子這些輸入都放在倉庫之外，得自己準備。

### UniXNU – GitHub

### 和 HackiOS 的差別，一句話就能講清楚

同一時間還有另一個打著類似旗號的專案在操作。OOSexGang 的 [HackiOS](https://www.kocpc.com.tw/archives/672302) 宣稱讓 iOS 16.5 跑在 Poco X5 Pro 與小米 Mi 11 Lite 5G 上，社群追蹤他們的 Telegram 頻道可以看到音效、Wi-Fi、相機逐一「修復」的過程，但整個計畫到目前為止沒有公開任何程式碼、沒有可安裝的版本，也沒有第三方實機驗證。

<iframe sandbox="allow-scripts" title="iOS 被破解？Hackios 成功讓 Android 手機運行 iOS 作業系統 — 電腦王阿達" src="https://www.kocpc.com.tw/archives/672302/embed#?secret=2SPrZy5Q2A#?secret=eeirEnnIoQ" width="500" height="623" frameborder="0"></iframe>

另有 [iOS-Sweet 專案](https://github.com/dipa-studio/ios-sweet) 把 XNU 移植到紅米 Note 10 Pro 海外版並公開原始碼，這一點比 HackiOS 誠實得多，但它的畫面全部靠 CPU 軟體渲染，幀率只有 20 到 30 幀，Wi-Fi、藍牙、蜂巢網路、相機、喇叭、感測器全都不能用。UniXNU 的差別就在 GPU 那一段，把軟體渲染換成了硬體加速的 Metal 路徑。

### 這件事的分量

Apple 轉進自研晶片之後，多數人認為黑蘋果這條路已經走到盡頭，因為 Apple 的 GPU 與 Metal 堆疊是關閉的。UniXNU 的做法說明另一種可能：把 Metal 呼叫轉譯到 Adreno 使用的開源 Mesa 與 Turnip 生態系，至少在一台裝置上可行。GPU 通了之後，無線、音訊這些周邊會不會跟上，才決定這台平板是停在開機畫面，還是能拿來日常使用。

<iframe src="https://player.bilibili.com/player.html?isOutside=true&amp;aid=117485911212102&amp;bvid=BV1TbnW6PEc6&amp;cid=42966191356&amp;p=1" width="100%" height="600" frameborder="no" allowfullscreen="allowfullscreen"><span data-mce-type="bookmark" style="display: inline-block; width: 0px; overflow: hidden; line-height: 0;" class="mce_SELRES_start"></span></iframe>

另一層價值在開源本身。官方接連在 10 月 20 日提交了十幾個 commit，從 SMMU 的 TLB 失效處理、BO 記憶體池、shader 特化到 render server 的 GPU 授權，幾乎是一天的量。而海內外的技術評論給了相當一致的評價，問題不在技術可行性，而在於「普通使用者不要拿去嘗試」：解鎖 Bootloader 會清空資料，也會失去官方保固，有能力的朋友可以自己測試看看。