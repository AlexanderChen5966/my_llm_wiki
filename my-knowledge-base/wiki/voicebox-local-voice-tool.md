---
title: Voicebox：整合聲音複製、全域聽寫與 MCP Agent 語音輸出的本機開源工具
date: 2026-07-20
tags: [ai/mcp, ai/workflow]
source: https://tools.wingzero.tw/article/sn/3989
type: reference
status: processed
asset_value: 57
---

Voicebox：本機優先（local-first）開源 AI 語音工具，把聲音複製、文字轉語音、語音聽寫、長篇內容製作與 AI Agent 語音輸出整合在同一套桌面 App。官方定位為 ElevenLabs（TTS/聲音複製）+ WisprFlow（語音輸入）的免費開源替代——同時處理完整的語音輸入與輸出流程。Tauri + Rust（非 Electron），跨 macOS/Windows/Linux。

## T（核心論點）

Voicebox 的差異化不在「又一個 TTS 介面」，而在於它把語音互動迴路的兩端（輸入聽寫 + 輸出合成）串成完整的 Voice I/O 工作環境，並透過內建本機 LLM 清理逐字稿、依 Voice Persona 套用角色個性。對知識庫最相關的是它提供 **MCP Server**：支援 MCP 的 Agent（Claude Code/Cursor/Cline）可用 `voicebox.speak` 工具，把原本只顯示在畫面的文字回應直接用指定聲音朗讀出來——重點不是另建語音 Agent，而是替既有 Agent 補上可自選甚至自製的聲音。

## B（適用場景）

**適用：** 想在本機複製聲音/產生 AI 配音的創作者；製作 Podcast/角色對話/長篇有聲內容者；想用全域快捷鍵取代鍵盤輸入的聽寫使用者；重視錄音/逐字稿/聲音資料隱私者；想讓 Claude Code/Cursor 等 MCP Agent 開口回應的開發者；想用 API 整合本機語音能力的工程師。
**不適用 / 注意：** 需自行下載模型、準備儲存空間，生成速度與可用功能取決於硬體（不是以雲端便利取勝，而是資料自主 + 模型選擇 + 完整工作流）。

## R（重點整理）

- **語音輸出（TTS）**：7 套引擎可依硬體/語言/品質/速度切換——Qwen3-TTS、Qwen CustomVoice、LuxTTS、Chatterbox Multilingual、Chatterbox Turbo、HumeAI TADA、Kokoro。其中 Qwen3-TTS/LuxTTS/Chatterbox/TADA 可聲音複製；Qwen CustomVoice/Kokoro 提供預設聲音。
- **聲音複製**：短音訊零樣本複製 + 50+ 預設聲音；匯入參考音訊建 Voice Profile。整體最多 23 種語言（實際依所選引擎而異）。
- **內容製作**：調音高 + 混響/延遲/合唱/壓縮/濾波；`[laugh]`/`[sigh]`/`[gasp]` 標記控制語氣；自然語言描述說話方式控情緒；長文自動切分 + Crossfade 銜接；Stories Editor 多軌時間軸做對話/Podcast/敘事。
- **語音輸入（聽寫）**：全域快捷鍵 Push-to-talk 或切換式錄音 → Whisper 本機轉文字 → 本機 LLM 處理標點/語助詞/自我修正 → 自動貼入焦點欄位；保存錄音與逐字稿可搜尋/重新轉錄/轉為新聲音樣本。
- **Voice Persona**：每個聲音可附自由格式 Persona（個性/語氣），由本機 LLM 執行 Compose/Rewrite/Respond，同段資訊依角色重組再朗讀；MCP Agent 也能用同模式。
- **技術/整合**：提供 REST API + WebSocket API + MCP Server；支援 Apple Silicon MLX/Metal、NVIDIA CUDA、AMD ROCm、Intel Arc、CPU、Docker。全流程可在本機執行，模型/錄音/逐字稿/LLM 處理不必上雲。

## C（關鍵行動）

- [ ] 用 Claude Code/Cursor 的開發者可掛 Voicebox MCP，讓 Agent 用 `voicebox.speak` 語音回應
- [ ] 重視隱私的聽寫/配音需求可用本機 Voicebox 取代 ElevenLabs/WisprFlow
- [ ] 做長篇有聲內容用 Stories Editor 多軌時間軸

## 原始摘要

DeTools 介紹：Voicebox 本機開源 AI 語音工具，整合 7 套 TTS 引擎 + 零樣本聲音複製 + 全域聽寫（Whisper + 本機 LLM 修整）+ Voice Persona + MCP Agent 語音輸出（voicebox.speak）。Tauri/Rust，跨平台，多硬體加速支援。核心相關點為 MCP 整合故歸 ai/mcp。

## 補充資料：LuxTTS —— 走「效率路線」的開源 voice cloning（2026-08-18 併入）

來源：https://tools.wingzero.tw/articles/4029 ／專案 github.com/ysharma3501/LuxTTS

與本條目的多引擎整合路線不同，LuxTTS 是**單一模型但把效率推到極限**的做法：

- 以 **ZipVoice 架構**為基礎，透過蒸餾與改良 Sampling **把推論縮減到約 4 steps**，搭配自訂 **48kHz vocoder**（而非常見的 24kHz）
- 官方公布 GPU **超過 150x realtime**（理論上一秒生成超過 150 秒語音）
- 🔑 **VRAM 可控制在 1GB 以內**，不一定需要高階 NVIDIA GPU；除 CUDA 外支援 **CPU 與 Apple Mac 的 MPS**，**CPU 仍可達 faster-than-realtime**
- Voice cloning **官方建議至少 3 秒參考音訊**（範例預設取約 5 秒做 prompt encoding）

🔑 **它與多數 voice cloning 專案的路線分歧**：不追求大型模型與高運算量，而是在**聲音品質、速度與模型效率之間取平衡**——
這正好對應本庫關注的本機優先取向（見 [[ollama-local-llm-setup]] 的「什麼工作值得留在本機」四篩選）。

適合的情境：需要**大量產生同一個角色聲音**（先錄幾秒，之後用文字批次生成旁白）；
若在做 AI 影片自動化，可把腳本生成結果直接交給它，再送進字幕／對嘴／剪輯流程。

## 相關連結

- [[meetily-ai-meeting-assistant]] — 另一款本機優先開源語音工具（會議錄音→轉錄→摘要），同屬隱私優先本地語音處理
- [[ai-skills-mcp-plugins-agents-comparison]] — MCP 作為 Agent 外接能力的協議定位，Voicebox 即以 MCP 補聲音能力給既有 Agent
- [[voicestudio-local-voice-cloning-tts-asr]]（ideas）— 同屬本機開源語音工具＋MCP 整合，但範疇較廣：涵蓋聲音複製、影片配音、有聲書與 646 語言目錄；Voicebox 範疇較窄，專注 Coding Agent 的語音輸出入迴路與 Voice Persona，兩者互補非重複
