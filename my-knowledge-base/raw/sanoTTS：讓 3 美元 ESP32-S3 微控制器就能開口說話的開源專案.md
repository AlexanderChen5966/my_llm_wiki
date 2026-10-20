---
title: "sanoTTS：讓 3 美元 ESP32-S3 微控制器就能開口說話的開源專案"
source: "https://www.kocpc.com.tw/archives/668158"
author:
  - "[[達小編]]"
published: 2026-09-08
created: 2026-09-09
description: "sanoTTS 的技術路線是知識蒸餾。團隊以 Piper（一個開源的 VITS 架構 TTS 系統）作為教師模型，透過條件變分自編碼器（CVAE）目標函數，將大型模型的語音合成能力壓縮到極小的學生模型中。訓練過程包含持續時間、潛在介面、波形、對抗和聯合蒸餾等多項損失函數。"
tags:
  - "clippings"
status: processed
---
近日尼泊爾開發者 Ashish Thapa 創辦的 Ampixa Labs 開源了 sanoTTS，這是一個參數量從 294K 到 2.27M 的微型神經語音合成家族，能在一顆約 NT$100 的 ESP32-S3 微控制器上即時執行，也能透過 WebAssembly 直接在瀏覽器裡跑，全程不需要雲端伺服器、GPU 或 NPU。「sano」在尼泊爾語中是「小」的意思。這個命名精準描述了專案的核心哲學：把神經網路 TTS 壓縮到前所未有的體積，讓任何裝置都能獨立發聲。  
![sanoTTS：讓 3 美元 ESP32-S3 微控制器就能開口說話的開源專案 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/09/20260908174432_0_730193.jpg "sanoTTS：讓 3 美元 ESP32-S3 微控制器就能開口說話的開源專案 - 電腦王阿達")

### 技術架構：從 Piper 教師模型蒸餾而來

sanoTTS 的技術路線是知識蒸餾。團隊以 Piper（一個開源的 VITS 架構 TTS 系統）作為教師模型，透過條件變分自編碼器（CVAE）目標函數，將大型模型的語音合成能力壓縮到極小的學生模型中。訓練過程包含持續時間、潛在介面、波形、對抗和聯合蒸餾等多項損失函數。

部署時的完整語音合成管線，從音素辨識（espeak-ng，已編譯為 WebAssembly，僅 275KB）到聲學模型再到波形重建，在 ESP32-S3 上生成 4.54 秒語音只需 1.02 秒，即 0.22 倍即時速度，比播放速度還快。同一套 C 語言核心在沒有浮點運算單元的 ESP32-C3 上也能以 5.72 倍即時速度離線執行。  
![sanoTTS：讓 3 美元 ESP32-S3 微控制器就能開口說話的開源專案 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/09/20260908175249_0_66c1ed.jpg "sanoTTS：讓 3 美元 ESP32-S3 微控制器就能開口說話的開源專案 - 電腦王阿達")

論文作者表示，這是目前已知最小的、能在通用微控制器上即時執行的完整音素到波形神經 TTS 圖，且不依賴神經加速器。

### 跑分數據：15M 參數以下的自然度冠軍

在一份涵蓋多個小型 TTS 系統的基準測試中，sanoTTS 的 amy 聲音（1.46M 參數）在 SCOREQ 和 UTMOS 兩項自然度預測指標上均拿下 sub-15M 等級最佳成績：

- **sanoTTS amy（1.46M 參數）** ：SCOREQ 4.13、UTMOS 4.10
- **TinyTTS（1.62M 參數）** ：SCOREQ 3.94、UTMOS 3.65
- **Inflect Nano（4.63M 參數）** ：SCOREQ 3.81、UTMOS 3.65
- **Kitten TTS nano（15M 參數）** ：SCOREQ 3.02、UTMOS 3.58
- **Kokoro（82M 參數）** ：SCOREQ 4.89、UTMOS 4.52

sanoTTS 用比 TinyTTS 更少的參數拿到了更好的自然度分數。Kokoro 雖然全面領先，但它的參數量是 sanoTTS 最大聲音的 36 倍、最小聲音的 279 倍。在極小體積的限制下，壓縮到這個品質水準是相當突出的工程成果。

團隊同時發現，影響語音品質的關鍵瓶頸在解碼器而非輸出表示。同一個 amy 聲音，將解碼器從 1.09M 參數加倍到 1.84M 參數，SCOREQ 從 3.70 提升到 4.16。

### 從日本到墨西哥：社群自發擴充語言

sanoTTS 原生支援英語、尼泊爾語、印地語、越南語、印尼語和中文共 6 種語言、11 種聲音。但社群的擴充速度比官方更快。

日本開發者 @nnn112358 將 sanoTTS 移植為日語版本 sanoTTS-jp，並整合進 StackChan，一個搭載在 ESP32 上的萌系機器人臉。他在 X 上展示了即時語音合成效果，並指出 sanoTTS-jp 支援串流處理，「文章再長也不會增加延遲，甚至可以做出無限說話的 StackChan」。 另一位日本開發者 [@ayousanz](https://x.com/ayousanz/status/2097194896776114444) 製作了 8MB 和 2MB 兩種日語模型規格，正在提交合併請求。

![](https://x.com/i/status/2097082230556467251)

在拉丁美洲，@MXZKARTA 自行從 Piper 的墨西哥西班牙語教師模型蒸餾出約 1.5M 參數的墨西哥西語聲音，並處理了 Oaxaca、Tlaxcala 等墨西哥地名以及當地數字和貨幣格式的 G2P 正規化。

![](https://x.com/i/status/2097149280402264455)

### 安裝與使用方式

sanoTTS 提供多種使用管道：

- **Python** ：pip install sanotts，透過 CLI 或程式庫呼叫，聲音模型首次使用時自動從 Hugging Face 下載
- **網頁** ：npm install sanotts-web，或直接複製靜態檔案部署，全程客戶端執行
- **Arduino / PlatformIO** ：在 ESP32-S3 上直接編譯執行，透過 GPIO 輸出到 LM386 放大器和揚聲器
- **瀏覽器體驗** ：ampixa.github.io/sanoTTS 提供即時 Demo，無需安裝任何東西

每個聲音模型的權重從 337KB（heart-nano，int8 量化）到數 MB 不等，純 numpy 推理，不需要 PyTorch 或 ONNX Runtime。

### sanoTTS – GitHub

### 限制與適用場景

必須誠實說明的是，sanoTTS 的音質存在明顯代價。論文中的嵌入式版本（567K 參數）在 SCOREQ 上只有 2.54，遠低於教師模型的 4.68。中文語音目前仍處於初步階段，@realfxw 的評測也證實了這一點。

sanoTTS 適合的場景是那些對音質要求不苛刻、但對體積和離線能力有剛需的應用：工業設備的語音提示、低功耗玩具、物聯網裝置的本地播報、以及需要在瀏覽器中提供基本語音回饋的網頁應用。在這些場景中，337KB 權重和零雲端依賴的組合是其他方案無法提供的。