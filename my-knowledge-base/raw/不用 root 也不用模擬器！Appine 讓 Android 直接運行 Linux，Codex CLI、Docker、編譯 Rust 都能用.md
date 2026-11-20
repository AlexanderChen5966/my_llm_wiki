---
title: "不用 root 也不用模擬器！Appine 讓 Android 直接運行 Linux，Codex CLI、Docker、編譯 Rust 都能用"
source: "https://www.kocpc.com.tw/archives/703986"
author:
  - "[[達小編]]"
published: 2026-11-19
created: 2026-11-19
description: "開發者 Matthew Lee 開源 Appine，一個不需要 root、不需要 PRoot、也不是 QEMU 虛擬機的 Android Linux 終端機，16MB 的安裝包已上架 Google Play。用 ARM64 指令翻譯器加上使用者空間核心執行未修改的 Linux 二進位檔，可跑 Codex CLI、Docker、Node.js 與 Rust 編譯。官方留下 208 輪開發紀錄與 LTP 測試數據，但效能數字全在虛擬機上量得，設計文件也寫著真機驗證尚未完成，社群無第三方實測。"
tags:
  - "clippings"
status: "inbox"
---
在 Android 手機上跑一個完整的 Linux 環境，過去只有幾種做法：Termux 要自己湊 F-Droid 或 GitHub 的安裝來源，iSH 是在 iOS 上用 x86 模擬器，UserLAnd 之類的工具則是靠 PRoot 模擬器做使用者空間的系統呼叫攔截。現在多了一個選項，開發者 Matthew Lee 在 11 月 17 日開源了 Appine，這是一個不需要 root、不需要 PRoot、也不是 QEMU 虛擬機的 Android Linux 終端機，Google Play 商店上架的版本在 18 日更新。  

作者在 X 上表示這是他做過最瘋狂的專案，並且強調可以「直接在 Android 本地使用 Linux 環境執行 Codex CLI、跑 Nodejs、編譯 Rust，或是執行 Docker」。Appine 採 AGPL-3.0 授權，Rust 撰寫，這個專案的技術路線與常見的 Android Linux 方案都不一樣。要理解它的差別，得先看它怎麼讓未修改的 Linux 執行檔在 Android 行程裡跑起來。

![](https://x.com/i/status/2122872595565699143)

### 它不是虛擬機：用指令翻譯器代替 CPU

Appine 執行的是未經修改的 ARM64 Linux 二進位檔。README 描述的運作機制是，它的 CPU 引擎把 ARM64 指令翻譯成 ARM64 程式碼，翻譯不了的指令交給直譯器處理，Linux 的系統呼叫則由一個使用者空間核心（user-space kernel）承接，終端機則採用 Ghostty 的 libghostty-vt。專案自己的設計文件把定位寫得很清楚，這是一個單一行程的 Android App，跑未修改的 Alpine Linux aarch64 使用者空間程式，作者直接拿 iSH 類比，稱它是 iSH 想法的 Android 對應版本。

### 它到底能跑什麼：從 busybox 到 Docker 容器

專案把工作負載分成四類並公開承諾範圍。日常命令列工具全部可用，包含 busybox ash、apk、coreutils、vim、git、curl 與 OpenSSH 用戶端；運算密集型如 python3、用 gcc/make 編譯中型 C 專案要能跑完；困難的執行環境如靜態連結的 Go 程式、Node.js 與 JVM 要求行為正確但速度不保證；容器、init 系統、核心模組與 X11 圖形介面則不在承諾範圍內。  
![不用 root 也不用模擬器！Appine 讓 Android 直接運行 Linux，Codex CLI、Docker、編譯 Rust 都能用 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261119004646_0_a9243a.jpg "不用 root 也不用模擬器！Appine 讓 Android 直接運行 Linux，Codex CLI、Docker、編譯 Rust 都能用 - 電腦王阿達")  

設計文件的註記提到，不承諾的那一類裡，podman 與 docker（透過命名空間模擬）以及 OpenRC（作為 PID 1）實際上是能運作的，systemd、核心模組與 GUI 則確實超出範圍。strace 與 gdb 同樣可用。

Appine 的設計文件提到 podman 與 docker 是透過命名空間模擬實作，Android 端的儀表測試也會安裝 tmux 與 screen 做瀏覽器橋接測試。而 Termux 這類 App 在 Google Play 上的版本正是因為 exec 限制與政策問題而長期受限，Termux 維護團隊在 2024 年 6 月的公告裡明說，PlayStore 上的版本功能等同於 v0.108、官方發行來源仍然只有 F-Droid 與 GitHub，且該版本隨時可能被下架。

### 效能實測：官方數字與它的測量條件

Appine 的效能數字全都有紀錄，這在同類專案裡不常見。專案給的目標是 CPU 密集型工作落在原生的 3 到 5 倍以內，設計文件寫明優先考量的是互動延遲，\`ls\` 與 tab 補全不能有遲滯感。

各項工作負載的實測倍數如下，數字都是相對原生 Linux 的耗時倍數，越小越快：

- **Gradle 熱編譯** ：9 倍（記憶體存取檢查與翻譯表那一輪量到 9.4 倍，後續最佳化後的數字落在 9 倍）
- **Maven 熱編譯** ：4.0 倍，JIT 工作負載那一輪量到 3.6 倍
- **gcc 編譯 lvm.c** ：3.2 倍（早期測試曾達 6.1 倍，後續因為測試環境修正而下降）
- **python 執行迴圈** ：4.6 倍
- **go build 冷編譯** ：2.8 倍，另一輪量到 2.6 倍，單執行緒 5.6 倍但多執行緒擴展不佳
- **node 的 JIT 產生程式碼** ：1.9 倍
- **git status** ：1.3 到 1.4 倍
- **檔案操作如 find、du** ：約 1 倍

這份數字有個必須說清楚的測量條件。專案文件的量測條件章節寫明，測試主機是 colima 在 Apple Silicon 上跑的 ARM64 Linux 虛擬機（4 核，部分測試 8 核），原生對照組是同一台虛擬機上的 Alpine 3.24 容器，而且採用熱快取，每個工作負載在同一個 Appine session 內跑兩次算第二次。另一組數字跑在 GitHub 的 ubuntu-24.04-arm runner 上，那是 Azure Cobalt 100 的 2 vCPU 機器，官方說它的核心「比較接近手機的大核與中核」。

### 對 AI Agent 的意義：把 libappine.so 當成 Linux Runtime

Appine 對 AI Agent 的意義可能比對一般使用者的意義更大。README 寫著 Appine 執行的是未修改的 ARM64 Linux 二進位檔，手機上因此可以直接跑完整的開發工具鏈，而不是雲端沙箱。程式碼倉庫的 Cargo workspace 結構也說明這點：核心分成 \`core/engine\`（CPU 引擎）、\`core/kernel\`（使用者空間核心）與 \`core/hal\` 三個 crate，\`hosts/android\` 與 \`hosts/cli\` 是兩種宿主，後者就是命令列版的執行器 appine-run。

Android 端的建置說明寫得更直接，終端機模擬器 libghostty-vt 被連接進 \`libappine.so\`，Rust 側則透過 JNI 介面與 \`Native\` 類別對接，兩個 APK 都會建出 arm64-v8a 版本的 \`libappine.so\`。 作者在 X 上也說，可以把 \`libappine.so\` 嵌進自己的 App，當作 Agent 的 Linux Runtime 使用。

這個方向對想在手機上跑本地 Agent 的開發者有實際價值，Codex CLI 加上 Docker 意味著 Agent 能在一台 Android 裝置上完成完整的開發與測試流程，不必依賴雲端容器，也不用把手機變成 Termux 的重度使用者。不過 Appine 目前的定位仍是開發者工具，遊戲與一般消費者 App 沒有可比性，效能數字也尚未在真實手機上驗證過。

### Appine – GitHub / Play Store

### 結語

Appine 把三個一直綁在一起的條件解開了：不需要 root、不需要模擬器、可以走 Google Play 正式審核管道。技術上用指令翻譯器加上使用者空間核心取代虛擬機，208 輪開發紀錄與 LTP 系統呼叫測試都留有可查的數字，這在個人開發者的專案裡相當少見。  
![不用 root 也不用模擬器！Appine 讓 Android 直接運行 Linux，Codex CLI、Docker、編譯 Rust 都能用 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261119005126_0_168fe2.jpg "不用 root 也不用模擬器！Appine 讓 Android 直接運行 Linux，Codex CLI、Docker、編譯 Rust 都能用 - 電腦王阿達")

要下手試之前有兩件事得先知道。效能數字全部來自 Apple Silicon 上的 ARM64 Linux 虛擬機與 Azure 的 2 vCPU runner，沒有一組是真實手機的量測，真機上的實際表現需要自己驗。設計文件自己也寫著 M9 的真機驗證尚未完成，社群則完全沒有第三方實測回饋。目前 GitHub 126 個 stars、10 個 forks，Play 商店下載量顯示 0+（這些數字為 11 月 18 日查詢時的狀態），這是一個剛上架、還在早期階段但技術底子做得相當認真的專案。