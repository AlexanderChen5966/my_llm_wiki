---
title: OpenAI DevDay 2026：ChatGPT 平台更新規格摘要（Plugins 2.0／Meetings／Slack・Teams／Codex Cloud）
date: 2026-10-08
tags: [ai/agent, ai/mcp, ai/workflow]
source: https://www.aiposthub.com/chatgpt-plugins-extensions-sites-mcp-events/; https://www.aiposthub.com/chatgpt-meetings-plugin-guide/; https://www.aiposthub.com/chatgpt-slack-teams-team-tasks-guide/; https://www.aiposthub.com/codex-cloud-cli-code-review-devday-2026/（皆引 https://openai.com/index/devday-2026-recap/）
type: note
status: processed
asset_value: 27
---

DevDay 2026 對 ChatGPT 平台的四組更新，只留規格事實（方案、狀態、限制），供拿 ChatGPT 與 Claude 產品能力對照時速查。⚠️ **來源集中**：四篇都是 Philo／aiposthub 於 2026-09-30 同日發布，唯一一手來源是 DevDay 2026 官方回顧，彼此互相引用、皆未實測——**實質上是一個來源**，不能當成多方佐證。依 [[ideas/openai-agents-api-public-beta]] 的先例留在 ideas/，等 OpenAI 官方文件或非 aiposthub 來源出現再考慮升 wiki。作者另附導入建議，見原文。

## Plugins 2.0

- **Extensions**：側邊欄固定入口＋互動面板＋自訂檔案檢視器；官方回顧列「全方案」，但實際 rollout、地區與管理員政策可能限制（官方轉述）
- **Plugin Creator**：描述功能、接工具、調介面後送審；OpenAI 同時改善送審回饋與目錄推薦排序（官方轉述）
- **Sites 可安裝支援的 Plugins**（Business／Enterprise／Healthcare／Edu），使用者以自己的資料與權限操作；另有 Shareable profiles 展示 Sites 與 Plugins。Sites 本體見 [[openai-codex-sites]]，本項是其增量
- 🔴 **MCP Events**：事件觸發的外掛自動化（新工單、文件更新、狀態改變）。發布時官方稱 **"proposed"——仍是提案規格，不是穩定標準**
- 未公布價格與分潤；與 [[agent-plugins-open-standard]]（Agent Plugins 1.0）的關係**原文沒寫**

## Meetings Plugin

- macOS 桌面版 **beta**，開放 Pro 與 Business；Enterprise 只有小型 alpha；Windows 未公布（官方轉述）
- **不需要 bot 入會**：在本機擷取麥克風＋系統音訊；單次上限 **4 小時**
- 音訊在筆記完成後刪除，離線或處理未完成時可能暫時保留；🔴 **逐字稿、摘要、行動項目與會議頁面不隨音訊刪除**（作者依官方推論）
- 官方要求依當地法律取得必要同意
- 對照：[[meetily-ai-meeting-assistant]]（本機自架）；「刪音訊 ≠ 刪衍生資料」與 [[chatgpt-claude-privacy-protection-9-checkpoints]] 的「三件互不涵蓋的操作」同型

## @ChatGPT in Slack／Microsoft Teams＋Team Tasks

- 開放 Business／Enterprise；可在頻道、thread、DM 提及 @ChatGPT，只能用管理員核准的工具（官方轉述）
- 授權分兩層：管理員層決定哪些 App／Plugin 可進工作區，個人層決定代表誰存取資料（作者依官方整理）
- **Team Tasks**：可排程或事件觸發，可用 Pages、Slides、Plugins、試算表（官方轉述）
- 跨廠商對照 [[claude-tag-slack]]；排程＋事件觸發對照 [[gemini-spark-personal-agent]]

## Codex Cloud／CLI／Code Review

- **Codex Cloud**：可重用、經核准的雲端開發環境，可從手機或其他裝置遠端查看與操作（官方轉述）
- 開放 Plus、Pro、Business、Healthcare、Education、Enterprise，功能與用量依方案、地區、管理設定而異（官方回顧轉述）
- 🔴 新版 CLI 與 Code Review 的具體新功能**原文沒列**，只有定位描述
- 相關：[[codex-desktop-2026-onboarding-guide]]、[[codex-best-practices-guide]]、[[claude-code-review-harness-principles]]

## 同批另兩篇的去向

- OpenAI Marketplace（commitment 抵用）→ [[ideas/claude-marketplace-enterprise-spend-commitment-drawdown]]；AWS Bedrock Managed Agents → [[ideas/openai-agents-api-public-beta]]
- Private Intelligence（ZDR 等三層）→ [[ideas/openai-private-intelligence-zdr]]

## 相關連結

- [[ideas/openai-agents-api-public-beta]] — 同為 DevDay 2026、同作者來源
- [[ideas/openai-private-intelligence-zdr]] — 同批 Private Intelligence 三層區分
- [[chatgpt-chat-work-codex-comparison]] — ChatGPT Chat／Work／Codex 三者定位
- [[codex-plugins-12-guide]] — Codex 端的 Plugins
- [[ai-skills-mcp-plugins-agents-comparison]] — Skills／MCP／Plugins／Agents 的層次區分
