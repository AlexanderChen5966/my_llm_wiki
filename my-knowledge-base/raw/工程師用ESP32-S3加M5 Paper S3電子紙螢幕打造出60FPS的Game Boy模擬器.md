---
title: "工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器"
source: "https://www.koc.com.tw/archives/647925"
author:
  - "[[達小編]]"
published: 2026-07-03
created: 2026-07-16
description: "E-Ink 螢幕之所以慢，是因為它需要對每個像素施加特定的電壓波形，讓黑白粒子移動到正確位置。傳統的更新方式是整頁刷新，一次更新所有像素，耗時數百毫秒。"
tags:
  - "clippings"
status: processed
---
一般人想到 E-Ink 電子紙螢幕，直覺反應是「翻頁很慢」、「只適合看書」。但硬體工程師 Wenting Zhang 用 M5Stack 的 Paper S3 開發板，做出了一台能在 E-Ink 螢幕上以 60FPS 順暢運行的 Game Boy 模擬器，顛覆了許多人對電子紙技術的認知。  

## 工程師用電子紙螢幕打造出60FPS的Game Boy模擬器

### 硬體平台：M5Stack Paper S3

Wenting Zhang 使用的硬體是 M5Stack 推出的 Paper S3，這是一款基於 ESP32-S3 微控制器的 E-Ink 開發板。規格包括 4.7 吋 960×540 解析度的觸控電子紙螢幕、8MB PSRAM、16MB 外部快閃記憶體，內建陀螺儀、蜂鳴器和實體按鍵。該產品目前已停產（End of Life）。  
![工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/07/20260703160900_0_c790fe.jpg "工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達")

Paper S3 的 E-Ink 螢幕採用 raw row column driver interface（也稱為 parallel interface），這與一般 LCD 的並列介面不同，但關鍵在於讓開發者能夠繞過傳統的波形更新方式，直接控制每個像素的驅動，這是實現高更新率的基本要求。  
![工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/07/20260703161053_0_7fdb76.jpg "工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達")

### 核心挑戰：讓 E-Ink 跑 60FPS

E-Ink 螢幕之所以慢，是因為它需要對每個像素施加特定的電壓波形，讓黑白粒子移動到正確位置。傳統的更新方式是整頁刷新，一次更新所有像素，耗時數百毫秒。

Wenting 的解法是：對每個像素獨立管理狀態，採用 pipeline 更新方式。每個像素儲存 4 位元的狀態，與傳入的影像緩衝區比對後，決定要對螢幕送出什麼指令。LCDIF 和 DMA 負責實際將像素資料送入螢幕，讓核心可以同時計算下一批像素。整個系統跑在 ESP32-S3 的第二核心上，每次迭代設定為約 17 毫秒，實現 60Hz 的更新率。雙重緩衝也確保了無撕裂的畫面輸出。  
![工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/07/20260703161425_0_136103.jpg "工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達")

不過 Game Boy 的原生解析度只有 160×144 像素，遠低於 Paper S3 的 960×540。Wenting 的做法是用 3 倍像素完美放大搭配抖色（dithering）來呈現四種灰階，只需要處理 160×3×144 像素的數據量，這讓高更新率在運算資源有限的 ESP32-S3 上成為可能。  
![工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/07/20260703161331_0_b29aee.jpg "工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達")

Wenting Zhang 並非第一次挑戰 Game Boy 相關的硬體專案。早在幾年前，他就曾在 FPGA 上完整實作過 Game Boy 的硬體重製（FPGA re-implementation），對 Game Boy 的架構有深入理解。這次他將 FPGA 時代累積的 E-Ink 高更新率驅動技術，搬到了更實惠、更普及的 ESP32 平台上。

### 模擬核心：從三款開源模擬器中挑選最佳方案

Wenting 過去曾在 FPGA 上實作過 Game Boy 模擬器，但考慮到微控制器上的 Game Boy 模擬已有大量成熟的開源方案，這次他選擇使用現有模擬器。他測試了三款：PeanutGB、WanaCGB 和 Crankboy。  
![工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/07/20260703161530_0_66b36f.jpg "工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達")

PeanutGB 是最受歡迎的微控制器 Game Boy 模擬器，WanaCGB 是它的分支（修改為 16 位元指令讀取以減少記憶體訪問），而 Crankboy 則是 Playdate 掌機的 Game Boy 模擬器前端，在記憶體映射路徑上做了大量最佳化。

經過多款遊戲的性能測試後，Wenting 選用性能最佳的 Crankboy 作為模擬核心。搭配動態跳幀，多款遊戲可以達到 100% 的原始 Game Boy 運行速度，實際幀率約 30 到 60 FPS。不過他也坦承，目前要支援 Game Boy Color 還不太現實，因為 GBC 允許 CPU 以雙倍速度運行，ESP32-S3 的性能還有一段差距。

### 聲音的妥協：從 PDM 到偽複音蜂鳴器

聲音是這個專案中最有趣的技術妥協。Paper S3 只有一個 PWM 蜂鳴器，只能產生單一音調。Game Boy 則有四個音效通道（兩個方波通道、一個取樣通道、一個噪音通道）。  
![工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/07/20260703161638_0_f4526d.jpg "工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達")

Wenting 最初嘗試用 I2S 周邊的 PDM 輸出作為 DAC，再接 RC 濾波器來播放 PCM 音訊，但聲音太小，實用性不足。他最終決定回歸蜂鳴器的本質，用一個巧妙的手法：將四個通道全部近似為方波（取樣通道和噪音通道也被強制近似），然後在四個方波通道之間快速切換，創造出偽複音效果。

他自己對結果的評價是：「同時好聽又難聽。」好聽的部分是夠大聲、夠清楚，遊戲音樂仍然可以辨識；難聽的部分是 Game Boy 從來不該聽起來像這樣。所有聲音處理都被放在第二核心上，確保不佔用模擬器的 CPU 資源。

### 操作與額外功能

Paper S3 配備觸控螢幕，Wenting 直接在螢幕上繪製了一個虛擬搖桿，讓玩家可以直接觸控操作。他也實驗性地加入了藍牙 LE 搖桿支援，雖然目前相容性有限，但作為概念驗證已經足夠。  
![工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/07/20260703161827_0_7622d2.jpg "工程師用ESP32-S3加M5 Paper S3電子紙螢幕打造出60FPS的Game Boy模擬器 - 電腦王阿達")

此外，他還加入了即時存檔與讀檔功能，可以隨時儲存 Game Boy 的完整狀態。下次拿起 Paper S3 時，可以直接讀檔回到遊戲進度，不需要重新開始。這對於掌機遊戲體驗來說是非常實用的功能。這款韌體已上架 M5 Burner，有興趣的人可以直接刷入 Paper S3 使用。有興趣的朋友可以看一下這部完整影片：

![](https://www.youtube.com/watch?v=oPbOK90aJEo)

### E-Ink 的可能性遠不止電子書

Wenting Zhang 這個專案最令人驚豔的地方，不在於「有人在 E-Ink 上跑 Game Boy 模擬器」，而在於他證明了 E-Ink 螢幕在適當的驅動方式下，可以達到傳統 LCD 等級的更新率。這對 E-Ink 產業的應用想像有深遠意義，從數位看板、資訊儀表板到互動式裝置，都有機會受益於這種高速驅動技術。

事實上，Wenting 過去就發表過關於如何在 E-Ink 上實現 60Hz 低延遲更新的技術影片，還曾將 E-Ink 螢幕改裝成電腦外接顯示器。這次的 Game Boy 模擬器只是這個技術思路的最新應用成果。

![](https://www.youtube.com/watch?v=nHbA2-_qzH4)

比較可惜的是 Paper S3 已經停產，不過 M5Stack 最近推出了新的 PaperColor（搭載 Spectra 6 彩色 E-Ink 螢幕），或許 Wenting 的下一個專案就是彩色 E-Ink Game Boy。