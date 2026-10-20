---
title: Continue.dev 完整評測 2026：開源免費 AI 程式碼助手
date: 2026-05-20
tags: [ai/agent, dev/architecture]
source: https://weavai.app/blog/2026/04/25/continue-dev-完整評測-2026
type: article
status: processed
asset_value: 35
---

Continue.dev 是 MIT 開源授權的 AI 程式碼助手，可安裝到 VS Code 和 JetBrains，支援 100+ AI 模型（包含本地 Ollama）。2026 年新增 Agent 模式，GitHub 超過 25,000 顆星。

## 核心差異化特點

1. **模型自由切換**：Claude Opus/Sonnet、GPT-4o、Gemini、Llama、DeepSeek——任何模型只要有 API Key 就能用
2. **VS Code + JetBrains 雙平台**（Cursor/Windsurf 不支援 JetBrains）
3. **本地 LLM 支援**：搭配 Ollama/LM Studio，程式碼完全不上雲——企業隱私需求的解法
4. **完全免費**：MIT 授權，無訂閱費

## Context Providers（最獨特功能）

- `@codebase`：讓 AI 讀取整個專案架構
- `@docs`：參考特定文件網站（如 React 官方文件）
- `@github`：查看 Issues 和 PR 討論
- `@terminal`：看終端機輸出
- `@web`：即時搜尋網路

## 適合用 Continue.dev 的情境

- 需要在 JetBrains IDE（IntelliJ、PyCharm）工作
- 企業環境需要本地 LLM（程式碼不外傳）
- 預算有限（想用開源替代 Cursor $20/月）
- 需要靈活切換多個 AI 模型

## 不適合的情境

- 需要最頂尖的 AI 輔助開發體驗（Cursor 在整合度上仍有優勢）
- 不想自己設定 API Key 的用戶

## 補充資料（來源：AI 織夢部落格 2026-04-25 完整評測）

### 定價實際成本估算

| 使用強度 | 搭配模型 | 月費估算 |
|---------|---------|---------|
| 輕度（1-2 小時/天）| Claude Haiku 或 DeepSeek | $2-5 美元 |
| 中度（4-6 小時/天）| Claude Sonnet | $10-20 美元 |
| 重度（全天開發）| Claude Opus | $50 美元以上 |
| 本地 LLM（Ollama + Llama 3.3）| 不需 API | 完全免費 |

### 與主流競品的關鍵差異

Continue.dev 是市場上**唯一同時滿足「開源、本地 LLM、JetBrains 支援」**三個條件的工具：
- Cline AI：開源且支援本地模型，但**僅支援 VS Code**
- Cursor / Windsurf：功能強大但閉源收費，**不支援本地模型**
- GitHub Copilot：VS Code 無縫整合，但**無本地 LLM 選項**

### Agent 模式（2026 年最大升級）

2026 年新增 Agent 模式，可自主執行多步驟任務：分析需求 → 規劃步驟 → 修改檔案 → 執行終端指令 → 驗證結果，整個過程幾乎不需人工介入，能力與 Cline AI Agent 模式相當。

### 安裝設定快速指引

1. VS Code 擴充功能安裝 "Continue"
2. 首次啟動選擇 AI 模型（填入 API Key）
3. 設定 `~/.continue/config.json`：Tab 補全用小模型，Chat 和 Agent 用強模型
4. 在 `contextProviders` 陣列啟用 `@codebase` + `@docs`

## 相關連結
- [[dual-tool-strategy]] — 多工具組合策略（Continue.dev 可加入工具鏈）
- [[agentic-ai-learning-roadmap]] — Track A CLI Power User 路徑中的工具比較
