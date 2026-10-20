---
title: Continue.dev 完整評測 2026：開源免費 AI 程式碼助手
date: 2026-06-01
tags: [ai/agent, dev/architecture]
source: https://weavai.app/blog/2026/04/25/continue-dev-%E5%AE%8C%E6%95%B4%E8%A9%95%E6%B8%AC-2026%EF%BC%9A%E9%96%8B%E6%BA%90%E5%85%8D%E8%B2%BB-ai-%E7%A8%8B%E5%BC%8F%E7%A2%BC%E5%8A%A9%E6%89%8B%E6%B7%B1%E5%BA%A6%E8%A9%95%E6%B8%AC/
type: note
status: processed
asset_value: 45
---

Continue.dev 是 MIT 授權的開源 AI 程式碼助手，2026 年版新增 Agent 模式，從補全工具進化為多步驟開發代理。最大差異化：同時支援 VS Code + JetBrains + Neovim，且是市場上唯一同時滿足「開源、本地 LLM、JetBrains 支援」三個條件的工具。

## T（核心論點）

Continue.dev 的定位是「用更少的錢換取更大的彈性與完全的隱私控制」。

**六大核心功能：**
1. **100+ AI 模型支援**：Claude Opus 4.6/Sonnet 4.6、GPT-4o、Gemini 2.0 Pro、Ollama 本地模型，config.json 填 API Key 即切換
2. **雙平台 IDE 整合**：VS Code + JetBrains（IntelliJ/PyCharm/WebStorm）完整支援；Neovim 也支援（Cursor/Windsurf 做不到）
3. **Context Providers**：@codebase、@docs、@github、@terminal、@web，讓 AI 真正理解整個專案
4. **Agent 模式（2026 新增）**：分析需求 → 規劃步驟 → 修改檔案 → 執行終端指令 → 驗證結果，幾乎不需人工介入
5. **本地 LLM 支援**：Ollama/LM Studio/vLLM，所有程式碼不出內部伺服器，企業隱私保障
6. **開源生態**：MIT 授權，社群開發 Context Providers、Slash Commands、Prompt 範本

**定價比較：**
- 輕度使用：$2-5/月（Claude Haiku 或 DeepSeek）
- 中度使用：$10-20/月（Claude Sonnet）
- 本地 LLM（Ollama）：完全免費

**競品比較：**
- vs Cursor（$20/月）：Continue.dev 開源、支援本地 LLM 和 JetBrains，但初始設定複雜
- vs Cline AI（開源）：Cline Agent 模式更強，但僅支援 VS Code
- vs GitHub Copilot（$10-19/月）：Continue.dev 更彈性，但 Copilot 開箱即用

## B（適用場景）

**適用：**
- 企業安全開發者：需要 100% 程式碼留存內部
- JetBrains 重度用戶（IntelliJ/PyCharm/WebStorm）：Continue.dev 幾乎是唯一頂級選擇
- 預算有限獨立開發者：搭配 Ollama 幾乎零成本
- AI 模型實驗者：最好的「AI 模型實驗室」

**不適用 / 注意：**
- 習慣開箱即用的開發者：初始設定需 15-30 分鐘
- 重度雲端 API 使用者：費用可能超過 Cursor Pro $20 固定月費
- 需要企業 SSO/審計日誌的大型團隊：需額外評估 Continue Hub 付費方案或自托管

## R（操作步驟）

1. VS Code 擴充功能搜尋「Continue」並安裝
2. 首次啟動選擇 AI 模型（填入 Anthropic API Key 或設定 Ollama）
3. 在 `~/.continue/config.json` 設定 Tab 補全模型（建議用輕量模型如 DeepSeek Coder V2 Lite）
4. 在 `config.json` 的 `contextProviders` 加入 @codebase 和 @docs
5. 按 Cmd+L 開啟 Chat 面板，開始使用

## C（關鍵行動）

- [ ] 評估開發環境是否有 JetBrains IDE 需求（若有，Continue.dev 幾乎是唯一選擇）
- [ ] 如需降低 API 成本，嘗試 Ollama + Llama 3.3 本地模型
- [ ] 比較 Agent 模式與 Claude Code 的使用體驗差異

## 原始摘要

深度評測開源 AI 程式碼助手 Continue.dev（MIT 授權），支援 VS Code/JetBrains/Neovim，可自由連接 100+ 個 AI 模型（含本地 Ollama）。2026 年版新增 Agent 模式，從補全工具進化為多步驟開發代理，並與 Cursor AI、GitHub Copilot、Cline AI 進行詳細比較。

## 相關連結

- [[research/codex-vs-claude-code]] — Codex vs Claude Code 切換觀察
- [[dual-tool-strategy]] — 雙工具策略（Gemini CLI + Claude Code）
- [[gemini-cli-subagent]] — Gemini CLI 接入 Claude Code
