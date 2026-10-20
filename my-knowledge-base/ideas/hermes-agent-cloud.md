---
title: Hermes Agent 正式上雲：Nous Portal 一鍵部署，60 秒啟動雲端 AI 代理
date: 2026-07-17
tags: [ai/agent]
source: https://www.koc.com.tw/archives/648580
type: idea
status: processed
---

產品發布新聞。Nous Research 宣布開源 AI 代理框架 Hermes Agent 現可透過 **Nous Portal** 雲端平台一鍵部署：選模型、選伺服器規格兩步驟，約 60 秒啟動一個全天候運作、具持久記憶的 AI 代理，無需自架基礎設施。以新聞為主、缺可重複操作框架，暫存 ideas。

## 值得留意的參考點
- **Nous Portal（2026-04 推出的統一訂閱平台）**：整合 200+ AI 模型（Claude 系列 Fable 5/Opus 4.7/Sonnet 4.6/Haiku 4.5、GPT-5.5/5.4 Mini、Gemini 3 Pro/Flash、DeepSeek/Llama/Mistral/Qwen），一個 OAuth 登入 + 一份帳單
- **工具閘道（Tool Gateway）**：五種常用代理工具整合在單一訂閱下——網頁搜尋爬取（Firecrawl）/ 圖片生成（FLUX 2、Ideogram V3 等九種）/ TTS 語音合成 / 瀏覽器自動化（Browser Use）/ 程式碼執行沙箱。解決過去要分別註冊 OpenRouter/Anthropic/OpenAI/Firecrawl/FAL/Browser Use 各管 API Key 的「API Key 地獄」
- **雲端 vs 自架**：開源專案本身仍免費可自架（用任何模型供應商）；雲端版付費從每月 $10 儲值起（另有模型訂閱費，有免費方案限特定模型）。自架適合已有伺服器/需資料隱私/用 Portal 未收錄模型的進階使用者；雲端適合嚐鮮、快速上手、不想維運的個人

## 相關連結
- [[hermes-agent]] — Hermes Agent 主條目（三層長效記憶、開源框架）
- [[hermes-agent-architecture-deep-dive]] — 完整架構深度解析
- [[ideas/hermes-agent-desktop]] — Hermes Agent Desktop GUI 版
- [[synthesis-agent-framework-philosophy]] — Agent 框架穩定性哲學（含 Hermes）
