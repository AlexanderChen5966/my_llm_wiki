---
title: Meetily：開源自架 AI 會議助理（逐字稿→摘要→待辦）
date: 2026-07-03
tags: [ai/workflow, pkm/automation, type/concept]
source: https://tools.wingzero.tw/article/sn/3967
type: article
status: processed
asset_value: 52
---

Meetily 是開源、可自架的 AI 會議助理，會議進行時自動錄音、語音轉文字、產生摘要並擷取待辦事項（Action Items）。定位不是視訊會議軟體，而是**會議後處理與知識整理**平台。強調隱私：Rust 打造、100% 本地處理、Parakeet/Whisper 即時轉錄（宣稱 4x 快）、講者分離、Ollama 摘要，無需雲端。

## T（核心論點）

會議後整理逐字稿、寫紀錄、追待辦耗時，多數 AI 會議助理採訂閱制且要上傳錄音到第三方雲端。Meetily 用開源自架把會議資料留在自己環境，將錄音轉成結構化資訊（重點/決策/待辦/追蹤），讓團隊不必重聽完整錄音。

## B（適用場景）

- 團隊例會、專案會議、客戶訪談、線上課程、產品需求討論、技術分享
- 重視資料隱私、需符合內部安全/合規的企業、政府單位
- 需搭配現有會議工具使用（本身不做視訊）

## R（主要內容）

**功能**：AI 語音轉文字、自動會議摘要、Action Items 擷取、會議重點整理、錄音檔管理、多場歷史保存、AI 模型整合、現代化 Web 介面、開源可客製。

**最具價值能力**：不只逐字稿，還整理本次重點/已完成決策/待辦事項/後續追蹤。

**隱私技術棧**（英文 README）：Rust、Parakeet/Whisper 即時轉錄、speaker diarization、Ollama 本地摘要，100% local、no cloud。

**部署與限制**：Docker 自架到自己伺服器；本身不提供視訊會議，需搭配現有工具；摘要品質受所選語音辨識模型與 LLM 影響。

## C（對自身的啟發）

- [ ] 會議逐字稿→結構化摘要，可作為 raw/transcripts 素材的上游產生器（接 knowledge-organizer 入庫）
- [ ] 100% 本地 + Ollama 摘要，符合隱私敏感素材的處理需求（對照 Stirling PDF 自架理念）
- [ ] Action Items 擷取結構，可對照本知識庫 daily-brief 的待辦追蹤

## 相關連結
- [[loom-ai-async-screen-recording]] — 非同步的另一半：AI 摘要驅動的螢幕錄影協作，適用於「不必開會」的場景
- [[markitdown-document-conversion]] — 多格式轉 Markdown（會議紀錄入庫）
- [[ollama-local-llm-setup]] — Ollama 本地 LLM
- [[stirling-pdf-toolkit]] — 自架隱私工具理念
- [[ideas/openai-devday-2026-chatgpt-platform-updates]] — 雲端對照組（2026-10-08）：ChatGPT Meetings Plugin 免 bot 入會（macOS beta、Pro／Business、單場 4 小時），音訊在筆記完成後刪除，但**逐字稿與摘要等衍生資料不隨之刪除**；與本條「100% 本地」形成雲端 vs 自架對照
