---
title: "ChatGPT Work、Codex 現在能同步其他 AI 代理的工作！支援 Claude Code、Cowork 與 Cursor"
source: "https://www.kocpc.com.tw/archives/664961"
author:
  - "[[Rocky]]"
published: 2026-08-12
created: 2026-08-12
description: "現在很多人用 AI 代理都會用多套軟體互相搭配，像我除了 Codex 之外，還有 Claude，每一款都有自己的設定檔、skills、串接的外部服務等，過去這類匯入功能主要比較像一次性的資料搬移，匯入完成之後，如果原本 AI 代理又有新的工作內容，通常還需要再次處理匯入，非常麻煩。而稍早 OpenAI 就推出一項新功能來改善這流程，在 X 官方帳號中宣布在 ChatGPT 桌面版 App 中新增「同步功能」，讓用戶可以輕鬆將其他 AI 代理的工作，與 ChatGPT Work 與 Codex 保持同步。"
tags:
  - "clippings"
status: processed
---
現在很多人用 AI 代理都會用多套軟體互相搭配，像我除了 Codex 之外，還有 Claude，每一款都有自己的設定檔、skills、串接的外部服務等，過去這類匯入功能主要比較像一次性的資料搬移，匯入完成之後，如果原本 AI 代理又有新的工作內容，通常還需要再次處理匯入，非常麻煩。而稍早 OpenAI 就推出一項新功能來改善這流程，在 X 官方帳號中宣布在 ChatGPT 桌面版 App 中新增「同步功能」，讓用戶可以輕鬆將其他 AI 代理的工作，與 ChatGPT Work 與 Codex 保持同步。

## ChatGPT 與 Codex 現在能跟其他 AI 代理保持同步，不用再只靠一次性匯入

OpenAI Developers 稍早在官方帳號寫到：「可以讓其他 agent 的工作內容跟 ChatGPT Work 與 Codex 保持同步；可以匯入專案、聊天、skills 與 plugins，回頭檢視匯入紀錄，並且在設定裡選擇開啟自動更新；已在 ChatGPT 桌面版推出」

![](https://x.com/i/status/2087242829076791392)

這也代表說，打開「保持匯入同步」後，ChatGPT 就會自動同步已匯入工作後續的變更。

根據官方文件，ChatGPT 能匯入的項目包括：

- instructions / 指示檔 → 轉成 AGENTS.md
- settings.json 設定 → 轉成 Codex 的 config.toml
- 技能
- 外掛程式
- 既有專案資料夾
- Claude Code 的 Project Memories
- 最近 30 天的聊天紀錄
- MCP Server 設定
- Hooks
- Slash commands
- Subagents

而支援的 AI 代理現階段只有 Claude Code、Claude Cowork 和 Cursor。

不過 OpenAI 目前沒有詳細說明哪些資料類型支援自動同步，因此設定、Skills、Plugins、MCP 等內容是否也會隨來源端變更而自動更新，目前還不能確定。

我也有先測試聊天記錄，原本以為是即時同步，但看起來不是，我在 Claude 新的聊天沒有立即同步到 ChatGPT，而且等了一個小時也還沒同步。

這功能目前已經在 ChatGPT 桌面版的 macOS 與 Windows 版本推出，如果你沒看到，可以檢查 ChatGPT 是否有更新到最新版本。

相關設定也很好找到，打開 ChatGPT 的設定後，左側就會看到匯入選項，進入即可找到新的「保持匯入同步」功能，將其啟用即可。下方則是 ChatGPT 自動偵測你電腦裡有哪些其他支援匯入的 AI 應用程式：  
![ChatGPT Work、Codex 現在能同步其他 AI 代理的工作！支援 Claude Code、Cowork 與 Cursor - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260812104426_0_3b9996.jpg "ChatGPT Work、Codex 現在能同步其他 AI 代理的工作！支援 Claude Code、Cowork 與 Cursor - 電腦王阿達")

點擊匯入後，會顯示可以匯入的項目，預設全部都打勾，你也能手動取消，沒問題按匯入至 ChatGPT：  
![ChatGPT Work、Codex 現在能同步其他 AI 代理的工作！支援 Claude Code、Cowork 與 Cursor - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260812104426_0_1bdf5c.jpg "ChatGPT Work、Codex 現在能同步其他 AI 代理的工作！支援 Claude Code、Cowork 與 Cursor - 電腦王阿達")