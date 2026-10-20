---
title: Google Antigravity：從 AI IDE 到 Agent 開發平台
date: 2026-05-28
tags: [ai/agent, dev/architecture]
source: https://www.darrelltw.com/google-antigravity-ide/
source_2: https://medium.com/@simon3458/google-io-2026-antigravity-cli-cb19b42706a6
type: article
status: processed
asset_value: 36
---

## T（核心論點）

Google Antigravity 在 Google I/O 2026（5/19）從單一 AI Coding IDE 進化為完整 Agent 開發平台：Antigravity Harness 統一了 Gemini API Managed Agents、AI Studio、桌面版三條產品線，同時把 Gemini CLI 併入並以 Go 重寫為 Antigravity CLI（`agy`）。

## B（適用場景）

**適用：** 正在用 Gemini CLI 的開發者需要在 6/18 前遷移；想使用 Google 生態 Agent 開發平台（類似 Codex App 但對 Google 工具鏈）。

**不適用 / 注意：** Antigravity CLI 是 Gemini CLI 的替代品而非升級——指令語法全面更換（`gemini` → `agy`），設定路徑也改變。

## R（三大產品架構）

| 產品 | 定位 | 類比 |
|------|------|------|
| **Antigravity**（桌面 app）| 多 session 任務管理介面 | 類似 Codex App |
| **Antigravity IDE**| 原 Antigravity，重命名 | 類似 Cursor/Windsurf |
| **Antigravity CLI**（`agy`）| 取代 Gemini CLI，Go 重寫 | 類似 Gemini CLI |

共用底層：**Antigravity Harness**（agent runtime）。

## Gemini CLI → Antigravity CLI 遷移指南

**遷移期限**：2026-05-19 至 2026-06-18（30 天）

**指令更換**：
```bash
# 舊
gemini "explain this code"

# 新
agy "explain this code"
```

**設定搬家**（官方工具自動完成）：
- `~/.gemini/` → Antigravity 設定目錄
- 自訂指令、MCP server、hooks、skills 全部可遷移

**安裝 Antigravity CLI**：
```bash
# macOS (30 秒安裝)
brew install google/antigravity/agy
```

## C（關鍵行動）

- [ ] 若使用 Gemini CLI，在 2026-06-18 前完成遷移（CI 腳本中的 `gemini` 要換成 `agy`）
- [ ] 確認 `~/.gemini/` 的 MCP server 設定是否正確搬到新路徑
- [ ] 評估 Antigravity App 是否適合取代現有 Codex App 工作流

## 補充資料：Claude → Antigravity 2.0 MCP 移植防坑

來源：BASHCAT（hackmd.io，2026-05-25）

**核心問題**：Claude Desktop 的 MCP 設定直接移植到 Antigravity 2.0 會靜默失敗（Silent Failure）——遠端 HTTP MCP 連線無聲失效，`npx` 啟動的工具找不到命令。

| 問題 | 原因 | 解法 |
|------|------|------|
| 遠端 HTTP MCP 失效 | 執行 context 的認證/代理設定不同 | 改用 stdio 模式或重新設定認證 |
| `npx` 找不到命令 | Antigravity 的 PATH 未繼承 Node 路徑 | 改用絕對路徑 `~/.nvm/versions/node/.../bin/npx` |

原文附 Python 腳本可自動轉換 Claude Desktop MCP 設定為 Antigravity 格式。移植後務必逐一測試。

## 相關連結

- [[dual-tool-strategy]] — Gemini CLI + Claude Code 雙工具策略（CLI 工具已替換為 agy）
- [[gemini-cli-subagent]] — Gemini CLI 接入 Claude Code 具體設定（指令需更新為 agy）
- [[google-adk-2-graph-workflow]] — Google ADK 2.0（Antigravity Harness 底層）
