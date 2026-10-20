---
title: "Google Lyria 3.5 音樂生成模型上線 Gemini App"
source: "https://www.kocpc.com.tw/archives/667874"
author:
  - "[[達小編]]"
published: 2026-09-05
created: 2026-09-08
description: "Lyria 3.5 於 7 月 29 日首次在 Google Flow Music 推出，當時 Google 強調它在音樂性、歌詞品質和聲線表現上的改進。9 月 4 日這波更新把它帶進 Gemini App 和 Gemini API，覆蓋範圍擴大到網頁端、行動端、Google AI Studio 和 Google"
tags:
  - "clippings"
status: processed
---
Google 官方帳號於 9 月 4 日在 X 上宣布，旗下最強的音樂生成模型 Lyria 3.5 正式上線 Gemini App，所有全球使用者都能直接在手機或網頁端生成完整歌曲。這個模型在 7 月底先在 Google Flow Music 亮相，一個月後才推到 Gemini 這個更大規模的入口。對一般使用者來說，差別在於不必再跳到 Flow Music，直接在 Gemini 對話裡就能生成音樂。  
![Google Lyria 3.5 音樂生成模型上線 Gemini App - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/09/20260905180119_0_6314c2.jpg "Google Lyria 3.5 音樂生成模型上線 Gemini App - 電腦王阿達")

### 從 Flow Music 到 Gemini：Lyria 3.5 的完整路徑

Lyria 3.5 於 7 月 29 日首次在 Google Flow Music 推出，當時 Google 強調它在音樂性、歌詞品質和聲線表現上的改進。9 月 4 日這波更新把它帶進 Gemini App 和 Gemini API，覆蓋範圍擴大到網頁端、行動端、Google AI Studio 和 Google Vids，這也是 Lyria 3.5 首次離開 Flow Music 這個相對小眾的入口。

![](https://www.youtube.com/watch?v=WLRDC_aGCr8)

模型本身的能力方面：Lyria 3.5 能生成 44.1kHz 立體聲音頻，產出包含主歌、副歌、橋段在內的完整歌曲結構，而非只是重複一小段旋律。Lyria 3.5 在旋律結構複雜度、prompt 遵循度、結構感知和發音準確度上都有提升，聲線表現得更真實、更具情感層次。DeepMind 模型頁面顯示 Lyria 支援最長三分钟的歌曲生成，使用者可以自訂歌詞主題、聲線風格和音響偏好，也能上傳圖片讓模型據此生成音樂。

### Gemini App 內的新操作方式

這次上線 Gemini App 帶了三組新功能。第一是模板：Google 提供一系列起始模板，涵蓋背景音樂、品牌 jingle、個人化鈴聲等場景，讓使用者不用從空白 prompt 開始。第二是曲目長度選擇：使用者可以選短片段或較長的完整曲目。第三是風格控制：可以直接選擇或描述音樂類型，並在有人聲和純器樂之間切換。  
![Google Lyria 3.5 音樂生成模型上線 Gemini App - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/09/20260905175609_0_cd3293.jpg "Google Lyria 3.5 音樂生成模型上線 Gemini App - 電腦王阿達")

Google 官方部落格明確列出目標使用場景：影片背景音樂、品牌 jingle、個人化鈴聲。Lyria 3.5 在 Gemini 內的操作方式與之前 Flow Music 一致，但多了這些導引式工具，對不熟悉 prompt 寫法的入門者特別有用。

API 端的規格更細緻。Gemini API 文件列出兩個模型 ID：\`lyria-3-clip-preview\` 固定產出 30 秒短片，\`lyria-3.5\` 則生成數分鐘的完整曲目。開發者最多可以隨 prompt 提供十張圖片，用 section tags（Verse、Chorus、Bridge）寫自訂歌詞，也能用 timestamps 指定樂器在何時進入。預設輸出 MP3，Lyria 3.5 額外支援 WAV 格式。文件也說明，prompt 使用哪種語言，生成的歌詞就會是那種語言，聲線風格和發音會跟著調整。

![](https://x.com/i/status/2095905262229995736)

### SynthID 與版權護欄

所有 Lyria 生成的音頻都附帶 SynthID 水印，Google 將其描述為「人類耳朵無法察覺」的標記，讓聆聽者和平台能辨認音樂是否由 AI 生成或編輯。版權護欄方面，API 文件明確列出：所有 prompt 會經過安全過濾器，指向特定歌手的聲線請求和版權歌詞的生成都會被封鎖。

在音樂人參與方面，Lyria 系列在開發過程中有製作人和音樂家的參與，並列出了幾位曾使用 Google 音樂工具的音樂人：製作人 Wyclef Jean 曾使用 Music AI Sandbox，Yung Spielburg 則用 Lyria 為動畫短片《Dear Upstairs Neighbors》配樂。

Lyria 3.5 現在同時在 Gemini App、Google Flow Music、Google AI Studio 和 Google Vids 四個入口可取得。對一般使用者，Gemini App 是最直接的入口；對開發者，API 提供了更精細的控制；對音樂創作者，Flow Music 仍然是專門的工作流程。Google 這次把入口拉寬，等於把 AI 音樂生成從「工具」推進到「對話」的層級。

### 結語

Lyria 3.5 上線 Gemini 這件事，技術層面的升級幅度有限：44.1kHz、完整歌曲結構、SynthID 水印、單回合生成，這些在 Flow Music 階段就都有了。變化主要在分發：從 Flow Music 的音樂創作者社群，推到 Gemini 這個數億等級的使用者入口。模板、短長曲選擇、風格選項這些看似不起眼的功能，目的是降低 prompt 門檻，讓「在 Gemini 裡生成一首生日歌」直接生成歌曲變成一個自然動作。

Google 這時候把 Lyria 3.5 推到最大入口，既是在搶佔使用者習慣，也是在版權規則還模糊的時候，先用 SynthID 和護欄卡住自己的位置。對已經習慣在 Gemini 裡問問題、整理文件、寫程式的使用者來說，生成一首歌的距離，從「打開另一個 App」縮短到「打一行 prompt」。

[資料來源](https://blog.google/innovation-and-ai/products/gemini-app/better-tracks-lyria-gemini/)