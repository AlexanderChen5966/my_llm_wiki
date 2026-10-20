---
title: Lyria 3.5 上 Gemini App 的實質是分發而非技術突破（含 Interactions API 用法）
date: 2026-09-05
tags: [ai/workflow, type/how-to]
source: https://www.aiposthub.com/google-lyria-3-5-music-generation-guide/（主來源）；https://www.kocpc.com.tw/archives/667874（補充，時間軸與 API 細節）；官方一手來源為 Google AI Studio 的 X 公告（2026-09-04）與 blog.google
type: article
status: processed
asset_value: 18
---

> 本條目合併兩篇同日（2026-09-05）報導：**主來源 aiposthub（技術與 API 角度）
> ＋ 補充來源 kocpc（時間軸與分發角度）**。

## T（核心論點）

🔴 **時間軸修正（務必先看）**：**Lyria 3.5 其實在 2026-07-29 就已於 Google Flow Music 首發**，
44.1 kHz、完整曲式、SynthID、單回合生成這些能力在 Flow Music 階段就都有了。
**9/4 只是擴散入口**（Gemini App、Gemini API、AI Studio、Google Vids 四個管道），
技術升級幅度有限。主來源把 9/4 寫成「全面開放」，讀起來像首次發布，容易造成錯誤印象。

在此修正下，核心論點是：

> **這次發布的實質是分發策略，而不是技術突破。**
> 變化在於從 Flow Music 的音樂創作者社群，推到 Gemini 這個數億等級的入口——
> 把「生成一首歌」的距離從「打開另一個 App」縮短成「打一行 prompt」。

模型規格：44.1 kHz 立體聲、最長 3 分鐘含主歌／副歌／橋段的完整曲式、
支援 8 種語言歌詞演唱（英／日／法／德／西／北印度／韓／葡）、可上傳最多 10 張圖片做 Image-to-Music。

## B（適用場景）

**適用：**
- 影片背景音樂、品牌 jingle、個人化鈴聲（官方明列的三個目標場景）
- 需要原生官方 API 與版權合規機制（SynthID）的企業整合、批量配樂
- 想用一張概念圖／分鏡直接導出配樂風格（多模態圖像導引）

**不適用 / 注意：**
- 🔴 **單輪生成架構**：不支援對第 32–40 秒的鼓點做 Inpainting 局部重繪，也不能單獨抽換人聲軌（無 Stems 分軌）
- 🔴 **提示詞寫入具體歌手／樂團名（如 "Sing like Taylor Swift"）會被安全過濾器直接拒絕**，
  須改寫成風格描繪詞（如 "Bright modern pop female vocals with acoustic guitar"）；版權歌詞生成同樣被封鎖
- ⚠️ 2–3 分鐘高音質生成需**數十秒排隊運算**，不適合亞秒級即時互動場景
- 競品對照：Udio 音質 48 kHz 略高、可延伸 15 分鐘且支援 Inpainting；Suno 支援 Stems 分軌（付費版）、可延伸 4 分鐘

## R（操作步驟）

### 1. 三條存取管道對應不同對象

| 管道 | 對象 | 特點 |
| --- | --- | --- |
| **Gemini App** | 一般創作者 | 手機／網頁輸入 prompt 或上傳照片，自動產出封面圖，可分享試聽連結 |
| **Google AI Studio** | 開發者原型 | Playground 互動介面，免寫程式測試提示詞 |
| **Gemini API（Interactions API）** | 工程整合 | Python/JS SDK，依生成次數／長度計費 |

### 2. API 走新版 Interactions API（不是舊的 `generate_content`）

```python
from google import genai
client = genai.Client(api_key=os.environ.get("GEMINI_API_KEY"))

interaction = client.interactions.create(
    model="lyria-3.5",
    input=prompt
)

# 音訊從 steps 取出
for step in interaction.steps:
    for part in step.content:
        if hasattr(part, "inline_data") and part.inline_data:
            with open("output_track.mp3", "wb") as f:
                f.write(part.inline_data.data)
```

採**伺服器端狀態管理與步驟化（Steps）架構**，為未來多輪音訊編輯預留介面。

### 3. 兩個模型 ID 與輸出格式（來自 kocpc，主來源未提）

| 模型 ID | 輸出 |
| --- | --- |
| `lyria-3-clip-preview` | 固定產出 **30 秒**短片 |
| `lyria-3.5` | 數分鐘完整曲目 |

**預設 MP3，`lyria-3.5` 額外支援 WAV。**

### 4. Prompt 撰寫要訣

- 明確指定 **Genre / BPM / Key / Instruments**
- 用方括號結構標籤引導曲式推進：`[Intro]` `[Verse]` `[Chorus]` `[Bridge]` `[Outro]`
- 🔑 **可用 `timestamps` 指定樂器在何時進入**（kocpc 獨有資訊）
- 🔑 **prompt 用哪種語言，生成歌詞就是那種語言**，聲線風格與發音會跟著調整
- 隨 prompt 最多提供十張圖片

### 5. Gemini App 端的三組降門檻 UI

起始模板（背景音樂／品牌 jingle／個人化鈴聲）／短片段 vs 完整曲目長度選擇／
風格控制與人聲・純器樂切換。目的都是降低 prompt 門檻。

### 6. SynthID 版權防護

**頻域音訊浮水印**，人耳無法察覺、不影響音質，且**經壓縮、加速、雜訊干擾或二次混音後仍可被檢測辨識**。

## C（關鍵行動）

- [ ] 若要整合進工作流，先用 `lyria-3-clip-preview` 的 30 秒輸出做低成本試錯，再切 `lyria-3.5`
- [ ] 需要分軌或局部重繪的用途，直接排除 Lyria，改評估 Suno（Stems）或 Udio（Inpainting）
- [ ] 把「指定歌手名」的 prompt 全部改寫成風格描繪詞，避免安全過濾器直接拒絕
- [ ] 需要 WAV 時記得明確指定，預設是 MP3

## 原始摘要

**主來源（aiposthub）**：2026/9/4 Google 宣布 DeepMind 音樂模型 Lyria 3.5 全面開放，同時登陸
Google AI Studio、Gemini API（Interactions API）與 Gemini App。規格為 44.1 kHz 立體聲、最長 3 分鐘
完整曲式、8 種語言歌詞演唱、可上傳最多 10 張圖片做 Image-to-Music。作者判斷「中性偏多」：在 API
穩定度、多模態圖像導引與 SynthID 版權防護上建立企業級門檻，但仍是單輪生成架構，缺 Stems 與
Inpainting。附 Python Interactions API 完整範例與 vs Suno／Udio 七維度對照表，以及三項落地限制。

**補充來源（kocpc）**：關鍵時間軸——Lyria 3.5 首發是 7/29 的 Google Flow Music，9/4 才擴散到四個
入口；技術升級幅度有限，真正變化在分發。另補雙模型 ID、WAV 支援、timestamps 指定樂器進入時間、
prompt 語言決定歌詞語言、Gemini App 三組降門檻 UI，以及音樂人背書（Wyclef Jean 用 Music AI
Sandbox、Yung Spielburg 用 Lyria 為《Dear Upstairs Neighbors》配樂）。

## 相關連結
- [[gemini-omni-video-prompts]] — 影片生成的 prompt 結構化思考；與本篇同屬 Google 多模態生成閉環（文字 Gemini 3.5/3.7/3.8＋圖像 Imagen 3＋影片 Veo＋音樂 Lyria 3.5）
- [[gemini-spark-personal-agent]] — Google 把能力推向大眾入口的同一套分發策略
