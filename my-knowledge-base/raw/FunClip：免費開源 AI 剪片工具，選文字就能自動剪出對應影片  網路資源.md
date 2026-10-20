---
title: "FunClip：免費開源 AI 剪片工具，選文字就能自動剪出對應影片 | 網路資源"
source: "https://tools.wingzero.tw/articles/4068"
author:
  - "[[赤紅翼零]]"
published: 2026-09-16
created: 2026-09-17
description: "FunClip 是開源 AI 影片剪輯工具，透過 FunASR 將長影片轉成帶時間軸的文字，直接選取 Transcript 就能剪出片段，並支援 Speaker Recognition、LLM 智慧剪輯與自動字幕。"
tags:
  - "clippings"
status: processed
---
網站連結 [https://github.com/modelscope/FunClip](https://github.com/modelscope/FunClip)

FunClip 是 ModelScope 團隊開源的 AI 影片剪輯工具，主要利用 FunASR 語音辨識模型分析影片內容，將語音轉換成帶有時間資訊的文字。使用者不需要一直拖動影片時間軸尋找片段，只要從辨識結果中選擇想保留的文字，就能直接剪出對應的影片。

這種「文字就是時間軸」的工作方式，特別適合 Podcast、人物訪談、線上課程、會議錄影、直播回放與 Talking Head 類型影片。對一小時甚至數小時的長影片來說，可以先讓 AI 完成 Transcript，再從文字中找內容，通常比逐段播放影片有效率得多。

FunClip 目前比較值得注意的功能包括：

- **AI 語音辨識** ：透過 FunASR Paraformer 等模型把影片內容轉成文字。
- **文字直接剪影片** ：選擇辨識結果中的句子，即可取得對應影片片段。
- **多段剪輯** ：可以一次選取不同位置的內容重新組合。
- **自動產生字幕** ：能輸出完整影片與裁剪片段的 SRT 字幕。
- **說話者辨識** ：透過 CAM++ 辨識不同 Speaker，可直接挑某位說話者的內容剪輯。
- **Hotword 自訂** ：人名、品牌、專有名詞可以設定成 Hotword，提高辨識效果。
- **LLM 智慧剪輯** ：可利用大型語言模型分析 Transcript，再協助挑選值得保留的片段。
- **本機部署** ：提供 Gradio UI，也能架在自己的 Server 從瀏覽器操作。

其中 LLM Clipping 是 FunClip 相當值得注意的方向。傳統文字剪輯仍然需要使用者自己閱讀 Transcript、挑選句子；加入 LLM 後，可以進一步讓 AI 理解整段內容，再根據 Prompt 尋找符合條件的段落。

例如一段一小時的訪談，可以要求 AI 尋找「最適合做成短影音的重點內容」，再利用辨識結果中的 Timestamp 自動定位與裁切。如此就能形成：

`長影片 → AI 語音辨識 → Transcript → LLM 分析 → 找出片段 → 自動剪輯 → 字幕`

這種 Workflow 對長影片轉 Shorts、Reels 或精華影片尤其實用。

FunClip 也提供不同語音模型選擇。除了主要的 Paraformer，現在可以使用 Fun-ASR-Nano，支援中文、英文、日文、多種中文方言與區域口音；SenseVoice 則加入多語言 ASR、情緒與 Audio Event Detection。近期版本還加入第三方 MOSS-Transcribe-Diarize 路徑，針對長音訊提供語音辨識、Timestamp 與匿名 Speaker Label。

如果影片是多人 Podcast 或訪談，Speaker 功能會更加實用。例如系統辨識出 `spkS01` 、 `spkS02` 後，可以按照說話者選擇內容，不需要人工尋找「主持人在哪裡說話、來賓從哪裡開始回答」。需要注意的是，這類 Speaker Label 是同一段錄音中的匿名角色標籤，並不是辨識真實人物身分。

安裝方面目前仍偏向開源 AI 專案，需要準備 Python 環境與相應 PyTorch／torchaudio，再安裝 Requirements；模型權重會在啟動時另外下載。完成設定後，可以啟動 Local Gradio Service，直接透過瀏覽器操作。

截至 2026 年 9 月，FunClip 最新正式版本為 v2.2.1，專案採 MIT License。近期版本也持續改善長影片語音辨識、Speaker Label 與字幕處理，因此它目前已不只是早期單純的「FunASR + 文字剪影片」Demo，而是一套逐漸完整的開源 AI 影片內容剪輯工具。

整體而言，FunClip 最吸引人的特色就是 **不用一直看影片找時間點，而是直接看文字決定要剪什麼** 。如果平常需要處理 Podcast、訪談、課程、直播或大量 Talking Head 影片，結合 ASR、Speaker Recognition、LLM 與自動字幕後，可以大幅減少整理長影片內容所需要的人工操作。