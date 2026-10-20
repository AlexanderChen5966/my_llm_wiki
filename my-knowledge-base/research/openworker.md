---
title: OpenWorker：Andrew Ng 開源的本機優先 AI 桌面協作者
date: 2026-07-24
tags: [ai/agent]
source: https://github.com/andrewyng/openworker
type: reference
status: processed
---

## 定位

Andrew Ng 團隊開源、**本機優先（local-first）**的 AI 桌面協作者（open beta）。強調交付「完成的成品（finished work）」而非只是聊天——一份完成的文件、帶數字的 Slack 回覆、更新好的行事曆、分好類的收件匣。

## 關鍵特性

- **不綁模型（BYOK）**：自帶 API key，支援 OpenAI / Anthropic / Google Gemini / DeepSeek / Qwen / Mistral / Grok 等，或用 Ollama 完全本地跑。
- **25+ 連接器 + MCP**：GitHub / Slack / Jira / Notion / Linear / HubSpot / Outlook / Gmail / Google Calendar，加上本機終端機與檔案；任何 MCP 工具都能接入，可逐工具控制權限。
- **排程自動化**：晨間簡報、週報、常態監看頻道，執行結果落在 app 內含完整 transcript。
- **執行前徵求批准**：寫入、傳送、shell 指令都是 approval-gated；無人值守時把「請求」停在 inbox 而非自行動作。
- **從 Slack 工作**：頻道內 `@OpenWorker` 會在桌面開 session，結果以 thread 回覆。

## 技術架構

原生 shell + GUI（React + Tauri）→ 本機 agent server（Python）→ 你的檔案/工具/模型。**引擎建構於 [aisuite](https://github.com/andrewyng/aisuite)** 之上（統一的 chat-completions API + agents 層 + 工具/MCP 支援）；想自建 agent harness 可從 aisuite 起手。macOS（Apple Silicon，已簽章）/ Windows。隱私：agent loop、對話、connector token、model key 全在本機 secret store，唯一雲端環節是 OAuth handshake 中介服務。

## 為什麼歸 research

開源工具介紹，reference 性質，歸 research 追蹤生態發展。

## 相關連結

- [[ai-agent-core-loop]] — agent 核心迴圈的最小實作（OpenWorker 是框架化的完整版）
- [[librechat-platform]] — 另一個開源多模型 AI 平台（對照 BYOK 定位）
- [[ai-coworker-vs-ai-agent]] — AI coworker vs AI agent 的定位差異
- [[synthesis-agent-framework-philosophy]] — 選 agent 框架/harness 的哲學（aisuite 屬底層 harness 選項）
