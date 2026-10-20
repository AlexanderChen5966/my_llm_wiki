---
title: VoiceStudio：開源全本機 ElevenLabs 替代方案（聲音複製/影片配音/有聲書）
date: 2026-09-14
tags: [ai/mcp, ai/agent]
source: https://github.com/debpalash/VoiceStudio; https://tools.wingzero.tw/articles/4066
type: note
status: processed
asset_value: 46
---

VoiceStudio 是開源、本機優先的桌面語音工具，整合 16 種 TTS Engine、11 種 ASR Engine，涵蓋聲音複製（Voice Cloning，3–15 秒樣本零樣本複製）、依文字描述設計聲音（Voice Design）、影片配音（Video Dubbing）、聽寫、轉錄與有聲書製作，語言目錄達 646 種（實際涵蓋依所選 Engine 而異）。技術棧為 Tauri + React + FastAPI + SQLite，提供 OpenAI-compatible Audio API（`/v1/audio/speech`、`/v1/audio/transcriptions`）與 MCP Server（`generate_speech`、`clone_voice`、`transcribe`），可讓 Claude Desktop、Cursor 等 Coding Agent 直接呼叫語音能力。授權分層複雜：應用本身 AGPL-3.0，但預設 OmniVoice 權重與 Audio Tokenizer 另受 CC-BY-NC／Meta Llama 社群條款拘束，商用前需逐一核對各 Engine 授權。目前為 Active Beta，官方建議用 latest release 而非 main branch；wingzero 中文報導為 README 內容之摘要轉譯，未提供額外實測或安裝踩坑資料，故與 README 合併為單一條目。若需深入查安裝矩陣、架構圖或各 Engine 授權細節，回原始 GitHub README。

## 相關連結
- [[voicebox-local-voice-tool]] — 同屬本機開源語音工具＋MCP 整合，但範疇較窄：Voicebox 專注 Coding Agent 的語音輸出入迴路（聽寫+TTS 朗讀）與 Voice Persona；VoiceStudio 範疇較廣，涵蓋聲音複製、影片配音、有聲書與 646 語言目錄，兩者為互補而非重複條目
