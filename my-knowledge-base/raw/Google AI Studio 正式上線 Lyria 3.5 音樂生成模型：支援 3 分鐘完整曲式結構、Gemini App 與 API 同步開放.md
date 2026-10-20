---
title: "Google AI Studio 正式上線 Lyria 3.5 音樂生成模型：支援 3 分鐘完整曲式結構、Gemini App 與 API 同步開放"
source: "https://www.aiposthub.com/google-lyria-3-5-music-generation-guide/"
author:
  - "[[Philo]]"
published: 2026-09-05
created: 2026-09-08
description: "Google AI Studio 宣布 Lyria 3.5 音樂生成模型正式開放，支援 44.1 kHz 高音質立體聲、最長 3 分鐘完整曲式結構與多模態圖像啟發。本文深度解析技術特色、官方實測、API 串接代碼與競品比較。"
tags:
  - "clippings"
status: processed
---
過去兩年，AI 生成音樂的賽道雖然熱鬧，但多數工具仍停留在 30 秒至 1 分鐘的副歌片段循環，或因人聲金屬感過重、結構鬆散而難以直接進入專業製作管線。開發者若想將音樂生成功能整合至遊戲或影片編輯應用中，也往往面臨缺乏成熟商業級 API 與版權合規機制的困境。

2026 年 9 月 4 日，Google AI Studio 官方正式宣布，Google DeepMind 旗艦音樂生成模型 **Lyria 3.5** 全面開放存取。此次更新不僅登陸 Google AI Studio 網頁測試平台，更同步整合進 Gemini API（Interactions API）以及消費端 Gemini App，讓一般創作者與工程師能以同等高規格體驗新一代 AI 生成音樂。

官方指出，Lyria 3.5 是目前 Google 聲音表現最佳的音樂生成模型，具備更具表現力的人聲歌唱、更細緻的配器層次與 44.1 kHz 立體聲輸出，並支援生成具備主歌、副歌、橋段的最長 3 分鐘完整歌曲。

我的判斷是中性偏多。Lyria 3.5 補齊了 Google 生態系在音訊生成上的關鍵拼圖，特別是在 API 穩定度、多模態圖像導引與 SynthID 版權防護上建立起企業級門檻；但對於需要進行分軌混音（Stems）或多輪局部精細微調（Inpainting）的專業音樂製作人來說，目前「單輪提示詞生成」的架構仍有進化空間。

## 什麼是 Lyria 3.5？從實驗室走到生產級音訊引擎

Lyria 是 Google DeepMind 專為音樂創作設計的深度生成架構。回顧 Google 的音訊發展脈絡：從早期的 AudioLM、MusicLM，到 2023 年底首度亮相、與 YouTube「Dream Track」合作的 Lyria 初代，Google 一直試圖解決 AI 生成音訊中的「連貫性（Coherence）」與「聽覺擬真度（Acoustic Fidelity）」問題。

在先前的版本中，音樂生成往往只能維持數十秒的節奏動態，一旦時間拉長，旋律容易走調、結構脫節或出現雜音漂移。而 **Lyria 3.5** 則是 DeepMind 針對長音訊建模進行深度優化後的重大升級版本。

其核心技術具備以下特質：

- **44.1 kHz CD 級立體聲採樣率** ：擺脫過往音訊模型常出現的單聲道低頻混濁或高頻失真問題，呈現開闊的音場定位。
- **多模態架構原生支援** ：除了接受文字提示詞（Text Prompts）描述曲風、樂器、BPM 與情緒，還支援上傳最多 10 張圖片，由視覺畫面的色彩、場景與光影激發編曲風格。
- **內建時間對齊歌詞合成** ：模型能依照指定的章節結構與音節節拍，精準將歌詞咬字嵌入旋律中，大幅降低傳統 AI 歌聲常見的吞字或發音模糊現象。

## 4 大關鍵突破：結構、音質、語系與多模態掌控

Lyria 3.5 相比先前的音樂模型，帶來了四個維度的關鍵提升：

### 1\. 完整曲式結構（Structural Coherence）

傳統 AI 音樂最常被詬病的就是「從頭到尾同一個節奏循環」。Lyria 3.5 支援使用者在提示詞中使用結構標籤引導歌曲發展：

- `[Intro]` ：前奏營造氛圍。
- `[Verse]` ：主歌鋪陳敘事，人聲情緒較為收斂。
- `[Chorus]` ：副歌情緒爆發，配器與和聲層次全面加強。
- `[Bridge]` ：橋段變換調性或拍速，提供聽覺轉折。
- `[Outro]` ：尾奏淡出或高潮收尾。

這種結構感知能力，讓 Lyria 3.5 能夠產出長達 2 至 3 分鐘、起承轉合完整的真實流行曲式。

### 2\. 情感豐富的人聲與發音咬字

Lyria 3.5 大幅改善了歌聲合成中的金屬機械音，在氣息轉換、滑音、顫音以及動態強弱控制上更接近真人演唱。模型目前支援 8 種主要語言的歌詞歌唱合成，包括：英語、日語、法語、德語、西班牙語、北印度語、韓語與葡萄牙語。

### 3\. 多模態圖像導引（Image-to-Music）

創作者可以直接上傳電影分鏡腳本、遊戲概念圖或攝影照片，讓模型分析畫面氛圍後，自動構思出符合視覺調性的配樂。例如上傳一張「雨夜霓虹街道」的照片，模型會自動偏向 Synthwave 或 Lo-fi Jazz 配器，減少手動撰寫複雜提示詞的試錯成本。

### 4\. 嚴格的安全防護與 SynthID 水印

為了解決版權與虛假聲音爭議，Lyria 3.5 在生成音訊時，會全面嵌入 Google DeepMind 的 **SynthID 頻域音訊浮水印** 。這項浮水印人耳無法察覺，也不影響音質，但即便經過壓縮、加速、雜訊干擾或二次混音，依然能被檢測演算法辨識為 AI 生成。此外，模型內建安全護欄，會自動攔截「複製特定特定知名歌手嗓音」的請求，並過濾受版權保護的既有歌詞。

## 官方示範影片：2 分 36 秒完整曲目實測

Google AI Studio 在公告中同步釋出了一支長達 2 分 36 秒的完整生成示範曲目。整首歌曲展示了 Lyria 3.5 在真實曲式推進、人聲合音切換與多樂器動態混音上的成果：

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/09/google-lyria-3-5-demo-official.mp4" width="1280" height="720" controls=""></video>

Google AI Studio 官方釋出的 Lyria 3.5 示範影片，展示 2 分 36 秒完整結構的曲目表現與細膩歌聲。 影片來源： Google AI Studio 官方公告 。

從官方示範曲目中可以觀察到幾個亮點：

- **前奏至主歌的動態遞進** ：樂曲開頭以簡潔配器切入，隨後主歌人聲進入時背景音量自動避讓（Ducking），展現出良好的自動混音層次。
- **副歌爆發力與和聲厚度** ：進入副歌時，人聲疊加了立體聲雙軌和聲與高音合唱，低頻鼓組與貝斯下潛扎實，沒有破音或動態擠壓感。
- **自然的人聲呼吸感** ：句與句之間的換氣間隙自然，高音延伸處具有真實歌手的共鳴感，而非單純的頻率拉高。

## 三大存取管道：創作者與開發者該怎麼用？

Google 這次的策略非常明確——讓從輕度消費者到專業企業開發者，都能在對應的產品線中存取 Lyria 3.5：

| 存取管道 | 適合對象 | 主要功能特點 | 計費方式 |
| --- | --- | --- | --- |
| **Gemini App** | 一般使用者、內容創作者 | 支援手機與網頁端直接輸入提示詞或上傳照片生成歌曲，自動產出單曲封面圖，可直接分享試聽連結或下載音訊。 | 整合於 Gemini 免費 / Gemini Advanced 方案 |
| **Google AI Studio** | 開發者、原型設計師 | 網頁端互動介面（Playground），支援自由調整參數、測試歌詞標籤排版、對比不同提示詞效果，無需撰寫程式碼。 | 透過 Google 帳號存取，享開發者測試額度 |
| **Gemini API (Interactions API)** | 軟體工程師、企業架構師 | 支援透過 Python / JavaScript SDK 直接調用 `lyria-3.5` 模型，便於整合進自動化影片生成工作流、遊戲動態配樂系統。 | 依生成次數或長度計費（依 Cloud 控制台費率） |

## 開發者實戰：如何透過 Interactions API 調用 Lyria 3.5

在 API 架構層面，Google 將 Lyria 3.5 納入了最新版 Google GenAI SDK 的 **Interactions API** 規範中（而非舊版的 `generate_content` ）。Interactions API 採用了伺服器端狀態管理與步驟化（Steps）架構，為未來的多輪音訊編輯預留了擴充介面。

以下為使用 Python 呼叫 Lyria 3.5 生成音樂的標準範例：

```python
import os
from google import genai

# 1. 初始化 GenAI 客戶端
client = genai.Client(api_key=os.environ.get("GEMINI_API_KEY"))

# 2. 定義具備曲式標籤與音樂風格的提示詞
prompt = """
Genre: Modern Neo-Soul Pop
BPM: 92, Key: E-flat Major
Mood: Uplifting, warm, reflective
Instruments: Fender Rhodes, warm bassline, crisp acoustic drums, subtle brass section

[Intro]
(Smooth electric piano chords with soft drum brushes)

[Verse 1]
Walking through the city as the morning turns to light
Leaving all the shadows behind in the night
Every step a rhythm, every breath a song
Finding where the pieces of the dream belong

[Chorus]
Hold on to the daylight, let the music take the floor
We don't have to wander in the dark no more
Rise up, feel the harmony grow
This is the moment that we already know

[Outro]
(Electric piano solo fading out gently with background vocal echoes)
"""

# 3. 呼叫 Interactions API 建立生成任務
interaction = client.interactions.create(
    model="lyria-3.5",
    input=prompt
)

# 4. 取得生成音訊與歌曲結構中繼資料
print(f"Interaction ID: {interaction.id}")
# 生成完成後，音訊資料可自步驟中取得
for step in interaction.steps:
    for part in step.content:
        if hasattr(part, "inline_data") and part.inline_data:
            with open("output_track.mp3", "wb") as f:
                f.write(part.inline_data.data)
            print("音訊檔案已成功儲存為 output_track.mp3")
```

提示詞撰寫技巧：在提示詞中明確指定 **音樂類型（Genre）** 、 **節奏（BPM）** 、 **調性（Key）** 與 **主要樂器（Instruments）** ，並搭配方括號 `[Verse]` 、 `[Chorus]` 結構標籤，能顯著提高 Lyria 3.5 對曲式推進的掌控精準度。

## 橫向競品對比：Lyria 3.5 vs. Suno vs. Udio

在 AI 音樂領域，目前的領先者無疑是 Suno 與 Udio。Google 帶著 Lyria 3.5 全面入場，三者在定位與功能上有何差異？

| 比較維度 | Google Lyria 3.5 | Suno (v3.5 / v4) | Udio (v1.5 / v2) |
| --- | --- | --- | --- |
| **最高音訊規格** | 44.1 kHz 立體聲 | 44.1 kHz 立體聲 | 48 kHz 立體聲 |
| **單次生成時長** | 最長可達 3 分鐘 | 2 分鐘（可延伸至 4 分鐘） | 2 分鐘（可延伸至 15 分鐘） |
| **多模態圖像啟發** | 原生支援（最多 10 張照片） | 僅支援文字提示 | 僅支援文字提示 |
| **人聲表現特點** | 自然、發音咬字清晰、支援 8 種語系 | 流行感強、爆發力佳、節奏抓耳 | 音色細膩、真實度高、具備複雜爵士/搖滾聲線 |
| **開發者 API** | 原生 Gemini Interactions API | 需透過第三方或企業專案對接 | 官方封閉測試中，非全量開放 |
| **版權防護與溯源** | 內建 SynthID 頻域浮水印，自動阻擋人聲模仿 | 條款規範，內建防複製過濾 | 條款規範，浮水印防護機制 |
| **分軌混音與精修** | 目前僅支援單輪整軌輸出 | 支援 Stems 分軌分離（付費版） | 支援 Inpainting 局部重新生成 |

從比較中可以看出：

- **Suno 與 Udio 的優勢** ：在於長期針對「音樂創作者社群」打磨的編輯功能，例如分軌匯出（Stems）、片段替換（Inpainting）與社群曲庫互動。
- **Google Lyria 3.5 的優勢** ：在於 **生態整合與多模態能力** 。Lyria 3.5 原生嵌入 Google 龐大的開發者體系（Gemini API、Google AI Studio、Vertex AI），且具備強大的圖像輸入能力與官方合規保證（SynthID），對於需要批量產出配樂的影音工作者、遊戲設計師或企業應用而言，整合門檻遠低於獨立工具。

## 局限性與實際落地考量

儘管 Lyria 3.5 展現了令人印象深刻的音質突破，但在實際商用或創作導入時，仍需注意以下幾點：

1. **單輪生成架構（Single-turn Generation）** ：目前 API 與介面主要以「提示詞 -> 產生完整音訊」的單次推論為主，尚不支援像專業數位音訊工作站（DAW）那樣針對特定第 32 秒至 40 秒的鼓點進行「局部重繪（Inpainting）」或單獨抽換人聲軌道。
2. **嚴格的命名過濾機制** ：為了避免侵害藝人權益，若在提示詞中輸入具體歌手或樂團姓名（例如 "Sing like Taylor Swift"），請求會被安全過濾器直接拒絕。使用者必須轉化為風格描繪詞（例如 "Bright modern pop female vocals with acoustic guitar"）。
3. **長音訊生成的延遲時間** ：生成一首 2 至 3 分鐘的 44.1 kHz 高音質音訊需要進行龐大的擴散模型運算，API 回傳通常需要數十秒的排隊與運算時間，不適合需要亞秒級即時互動的語音對話場景。

## 總結與展望：AI 影音生產閉環的最後一塊拼圖

隨著 Lyria 3.5 的全面釋出，Google 在多模態生成領域的佈局已形成完整閉環：

- 文字與邏輯推理： **Gemini 3.5 / 3.7 / 3.8 Flash & Pro**
- 圖像生成： **Imagen 3**
- 影片生成： **Veo**
- 音樂與歌聲： **Lyria 3.5**

對於一般創作者而言，未來在 Gemini App 中製作一段短影音時，將不再需要外部尋找無版權音樂，只要一張照片或一段故事，就能同步產出高質感的量身配樂；而對於全球開發者而言，透過標準的 Gemini API 就能調用商業級音樂生成能力，勢必將推動新一波創意內容工具與智慧配樂應用的爆發。

## 官方來源與參考資源

- **Google AI Studio 官方公告** ： [Google AI Studio on X (Sep 4, 2026)](https://x.com/GoogleAIStudio/status/2095905336393605624?ref=aiposthub.com)
- **Google AI Studio 體驗入口** ： [ai.studio](https://ai.studio/?ref=aiposthub.com)
- **Google Developer 官方文檔** ： [Gemini API Interactions & Lyria Documentation](https://ai.google.dev/?ref=aiposthub.com)
- **Google DeepMind 技術研究** ： [DeepMind Lyria & SynthID for Audio](https://deepmind.google/technologies/lyria/?ref=aiposthub.com)