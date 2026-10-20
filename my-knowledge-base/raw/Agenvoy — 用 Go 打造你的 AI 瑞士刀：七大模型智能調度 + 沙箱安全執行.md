---
title: "Agenvoy — 用 Go 打造你的 AI 瑞士刀：七大模型智能調度 + 沙箱安全執行"
source: "https://repoinside.com/pardnchiu/agenvoy"
author:
  - "[[RepoInside]]"
published: 2026-03-20
created: 2026-06-22
description: "Agenvoy 是一個用 Go 打造的開源 AI Agent 平台，整合七大 AI 模型智能路由、25+ 內建工具、沙箱安全執行與 Discord Bot 模式。只需一行指令，Planner LLM 自動選出最佳模型，搭配 JSON 擴充架構讓你零程式碼新增 API 工具。"
tags:
  - "clippings"
status: processed
---
## 專案簡介

你是否曾經為了不同的 AI 任務，在 ChatGPT、Claude、Gemini 之間來回切換？Agenvoy 是一個以 Go 語言打造的開源 Agentic AI 平台，它讓你把 GitHub Copilot、Claude、OpenAI、Gemini、Nvidia NIM 甚至本地 Ollama 模型全部整合在一條指令裡。只要輸入你的需求，內建的 Planner LLM 會自動判斷哪個模型最適合處理這個任務，完全不需要手動選擇。

更厲害的是，Agenvoy 不只是聊天機器人——它配備了 25 種以上的內建工具，可以幫你讀寫檔案、搜尋網頁、抓取新聞、排程任務，甚至可以啟動一個 Discord Bot 來管理你的伺服器。所有外部指令都在作業系統原生沙箱中執行，確保安全隔離，讓你放心讓 AI 替你做事。

這個專案受到 [OpenClaw](https://openclaw.ai/) 啟發，由台灣開發者邱敬幃（Pardn Chiu）主導開發，採用 Apache-2.0 開源授權。

## 操作畫面

![Agenvoy Cover](https://agenvoy.com/doc/cover.png)

## 這個專案要解決哪些問題？

- **多模型切換麻煩** ：開發者需要在不同 AI 服務之間手動切換，Agenvoy 讓七個 AI 後端統一在一個介面下，由 Planner 自動路由到最適合的模型
- **AI 工具執行不安全** ：讓 AI 直接執行系統指令有資安風險，Agenvoy 透過 OS 原生沙箱（Linux bubblewrap / macOS sandbox-exec）隔離所有外部指令
- **缺乏跨 Session 記憶** ：一般 AI 對話每次都從頭開始，Agenvoy 在每輪結束時自動產生結構化摘要，下次對話注入歷史記憶
- **API 擴充需要改程式碼** ：傳統做法要寫 Go 程式才能加新的 API 工具，Agenvoy 只需丟一個 JSON 檔案就能擴充新的 REST API 工具
- **Skill 執行缺乏標準化** ：不同任務的 Prompt 工程散落各處，Agenvoy 用 Markdown 宣告式 SKILL.md 統一定義任務流程與工具權限
- **API 金鑰管理不安全** ：直接放在 `.env` 檔案容易外洩，Agenvoy 將金鑰儲存在 OS 原生 Keychain（macOS Keychain / Linux Secret Service）
- **Token 使用量不透明** ：多輪工具呼叫後不知道消耗了多少 Token，Agenvoy 跨所有迭代累計追蹤並即時顯示

## 適合哪些使用者？

- **全端開發者** ：需要一個統一的 AI CLI 工具來輔助日常開發工作，包括程式碼分析、文件產生、網路搜尋等
- **DevOps / SRE 工程師** ：需要安全的自動化排程與指令執行，Agenvoy 的沙箱與排程器能安全地自動化重複任務
- **AI 應用開發者** ：想研究多模型調度、Agentic 架構、工具呼叫迴圈等實作方式的工程師
- **Discord 社群管理者** ：需要一個能執行複雜任務的 Discord Bot，包括排程通知、資料查詢、自動回覆
- **開源愛好者 / 獨立開發者** ：想學習 Go 語言在 AI Agent 領域的實戰架構，從中汲取設計靈感
- **有多個 AI 訂閱的使用者** ：已經有 Copilot、OpenAI、Claude 等多個帳號，想用一個工具統一管理所有模型

工商服務時間

RepoInside 為 [開源報報](https://www.patreon.com/githubcurator) 的加值服務。開源報報每天透過 Patreon 為付費會員整理一份使用繁體中文的開源專案摘要列表電子報。

[詳細加值服務介紹](https://repoinside.com/)

## 專案資訊

- GitHub 網址： [https://github.com/pardnchiu/agenvoy](https://github.com/pardnchiu/agenvoy)
- 使用授權：Apache License 2.0
- 作者：邱敬幃 Pardn Chiu
- 官方網站： [https://agenvoy.com/](https://agenvoy.com/)
- 目前狀態：Alpha