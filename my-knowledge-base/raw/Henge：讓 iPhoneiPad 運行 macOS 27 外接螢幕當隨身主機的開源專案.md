---
title: "Henge：讓 iPhone/iPad 運行 macOS 27 外接螢幕當隨身主機的開源專案"
source: "https://www.kocpc.com.tw/archives/702552"
author:
  - "[[達小編]]"
published: 2026-11-08
created: 2026-11-10
description: "Henge 的技術底盤來自作者的前作 Husk，Husk 今年 9 月上線，讓 iPhone 直接執行 Android 遊戲與應用，靠自建轉換層運作，沒有完整開機 Android。Henge 的建置文件明列它直接編入 Husk 的 StikJIT（iOS 上的 JIT 即時編譯工具）、網路堆疊 libslirp 與"
tags:
  - "clippings"
status: "inbox"
---
很多人可能都想過 Apple 的行動處理器效能不錯，要跑 macOS 其實夠用，但就是看蘋果願不願意罷了（後來就有了 Macbook Neo 就是用手機處理器）。近日一個 11 月 2 日才出現的 GitHub 專案「Henge」把「iPhone 跑 macOS」蘋果不願意給你的功能做成成可安裝的軟體。開源模擬器 Henge 由 11 月 6 日發布 0.1 版，直接在 iPhone 與 iPad 上安裝蘋果的 macOS，不需要 Mac、不需要越獄；11 月 7 日跟進的 0.2 版更把最新的 macOS 27（Golden Gate）也塞進了手機。開發者 Leviidev 不是第一次做這種事，他上個月剛用同一套底層技術讓 iPhone 跑起 Android。  

### 從 Husk 到 Henge：同一個人的兩塊拼圖

Henge 的技術底盤來自作者的前作 Husk，Husk 今年 9 月上線，讓 iPhone 直接執行 Android 遊戲與應用，靠自建轉換層運作，沒有完整開機 Android。Henge 的建置文件明列它直接編入 Husk 的 StikJIT（iOS 上的 JIT 即時編譯工具）、網路堆疊 libslirp 與 glib。同一套「在 iOS 沙盒內跑外來作業系統」的工程，上個月翻譯 Android 應用，這個月模擬整個 macOS。

<iframe sandbox="allow-scripts" title="iPhone 跑 Android 遊戲成真！Husk 2.0 不靠模擬器，翻譯層直接執行 Android 程式碼 — 電腦王阿達" src="https://www.kocpc.com.tw/archives/672139/embed#?secret=0T4seNFOOL#?secret=HhDMIfJBoe" width="500" height="636" frameborder="0"></iframe>

### 不用 QEMU，自寫 ARM64 JIT 模擬器

技術路線方面，Henge 沒有使用常見的 QEMU，也沒有呼叫蘋果官方的 Virtualization.framework，核心是自訂的 ARM64 JIT 模擬器，搭配 Metal 圖形加速。因為 iPhone 與 Mac 同為 ARM64 架構，模擬器要在 iOS 的限制下重現 macOS 的指令集行為，省下了跨架構翻譯這個最重的負擔，這是它能在 iPhone 17 Pro 上做到約 15 秒進入 macOS 鎖定畫面的原因。安裝時 Henge 直接下載蘋果公開發布的系統還原映像檔（restore image）取得 macOS Sequoia，專案本身不附任何蘋果系統檔案。

![Henge：把 macOS 塞進 iPhone 的開源專案](https://i.ytimg.com/vi/EOiJuZ2Y9Lc/hqdefault.jpg)

### 0.2 版：macOS 27 上線、接螢幕當主機

11 月 7 日發布的 0.2 版把可行性又推高一層。macOS Golden Gate 27.2 確認可在 iPhone 17 Pro 安裝並開機，12 GB 的 iPhone 實際測得可分配 10 GB 記憶體給虛擬 Mac，記憶體選擇範圍為 1 至 16 GB [\[1\]](https://github.com/Leviidev/Henge/releases) 。新版本也支援透過 USB-C 把虛擬 Mac 輸出到外接顯示器（iOS 27 與 iOS 17 至 26 可用），並開放大眾最在意的一環：免費的個人 Apple 開發者團隊就能簽名安裝，不再需要付費憑證。效能面新增 60 Hz 與 120 Hz ProMotion 更新率選項、iPad P3 螢幕色彩校準，並修正記憶體查詢機制的瓶頸。

![](https://x.com/i/status/2119187250827137190)

### 43 GB、JIT 權限與一堆限制

大家先別看到這邊就躍躍欲試了，目前使用門檻仍然不低。系統需求為 iOS 或 iPadOS 17 以上、A14 或 M1 之後的晶片、約 43 GB 剩餘空間（17 GB 下載檔裝完可刪、26 GB 給 macOS），且 **一定要開啟 JIT 權限** ，途徑包括 StikDebug、TrollStore、越獄，或 iOS 26 以上 Henge 內建的 StikJIT 助手。已知限制包括：macOS 內登入 Apple 帳號會因驗證錯誤失敗、每次啟動只能開一台虛擬 Mac、模擬運行比實際的 Mac 慢，且實測機型至今只有 iPhone 17 Pro。  
![Henge：讓 iPhone/iPad 運行 macOS 27 外接螢幕當隨身主機的開源專案 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261108184043_0_be155b.jpg "Henge：讓 iPhone/iPad 運行 macOS 27 外接螢幕當隨身主機的開源專案 - 電腦王阿達")

### GPL 授權與 App Store 絕緣

授權結構決定了 Henge 的流通方式，專案採 GPL-2.0，因編入 QEMU 生態系的 GPL 元件，二進位檔構成 GPL 合併作品，文件直接寫明它無法上架 App Store（蘋果肯定也不會讓它上架）。使用者必須用 SideStore、AltStore 側載，或透過 TrollStore 安裝。對照組是今年 9 月 InfoQ 報導的 vphone-cli：該專案把完整 iOS 27 虛擬化搬上 Apple Silicon Mac，方向與 Henge 相反，兩者都取用蘋果公開發布的還原映像檔，也都游離於蘋果授權條款的邊緣。

### GitHub – Leviidev/Henge

其實 A14 之後的蘋果手機晶片效能足以應付 macOS 日常操作，限制集中在軟體授權與 JIT 權限（沒這權限會爆慢，正常上架的 QEMU 版光開機就要半小時）。如果你可以搞定開發者簽名、USB-C 外接螢幕、macOS 27 這三塊拼圖，那麼 iPhone 接上螢幕就是一台迷你 Mac 的玩法，正式從演示影片走進可以自行安裝的現實。