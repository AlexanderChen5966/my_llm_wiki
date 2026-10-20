---
title: Codex Plugin for Claude Code：跨供應商 AI Agent 協作
date: 2026-06-02
tags: [ai/agent, ai/claude]
source: https://github.com/openai/codex-plugin-cc
type: note
status: processed
asset_value: 50
---

## T（核心論點）

OpenAI 官方推出的 Claude Code plugin，讓使用者在 Claude Code 內直接呼叫 Codex 進行程式碼審查、對抗性審查或任務委派。這是 AI 工具競爭走向互操作性的里程碑：OpenAI 主動為競爭對手的平台開發 plugin，實現跨供應商的 agent 協作（Claude 執行 + Codex 審查）。

## B（適用場景）

**適用：**
- 在 Claude Code 工作流中需要第二意見的程式碼審查
- 對抗性審查：質疑設計決策、壓力測試假設（auth / data loss / race conditions）
- 將特定任務委派給 Codex 執行（如用更小的模型做快速修復）
- 已有 ChatGPT 訂閱（含 Free）或 OpenAI API key

**不適用 / 注意：**
- 使用量計入 Codex 使用限額
- Review gate 會造成 Claude/Codex 長迴圈，可能快速消耗額度
- 需要 Node.js 18.18+

## R（操作步驟）

### 安裝

1. `/plugin marketplace add openai/codex-plugin-cc`
2. `/plugin install codex@openai-codex`
3. `/reload-plugins`
4. `/codex:setup` — 檢查 Codex 是否就緒

### 六個核心命令

| 命令 | 功能 | 模式 |
|------|------|------|
| `/codex:review` | 標準程式碼審查 | 唯讀，支援 `--base main` |
| `/codex:adversarial-review` | 可操控的對抗性審查 | 唯讀，可加 focus text |
| `/codex:rescue` | 委派任務給 Codex | 可修改，支援 `--model`、`--resume` |
| `/codex:status` | 查看執行中/已完成的 job | 診斷 |
| `/codex:result` | 取得完成 job 的輸出 + session ID | 結果取回 |
| `/codex:cancel` | 取消背景 job | 控制 |

### 典型工作流

1. **Ship 前審查**：`/codex:review --background` → `/codex:status` → `/codex:result`
2. **委派問題**：`/codex:rescue investigate why the build is failing in CI`
3. **對抗性壓測**：`/codex:adversarial-review --base main challenge the caching design`
4. **接續到 Codex**：`codex resume <session-id>` 在 Codex 中繼續工作

### 可選：Review Gate

- `/codex:setup --enable-review-gate` — Claude 每次回應後自動觸發 Codex 審查
- 注意：會造成長迴圈，僅在主動監控時啟用

## C（關鍵行動）

- [ ] 安裝 codex-plugin-cc，在下次 PR 前用 `/codex:review` 取得第二意見
- [ ] 嘗試 `/codex:adversarial-review` 壓測一個設計決策
- [ ] 評估 `/codex:rescue` 在 bug 調查中的效用

## 原始摘要

OpenAI 官方推出的 Claude Code plugin，讓使用者可在 Claude Code 內直接呼叫 Codex 進行程式碼審查（/codex:review）、對抗性審查（/codex:adversarial-review）或將任務委派給 Codex 執行（/codex:rescue），實現跨供應商的 AI agent 協作。

## 相關連結

- [[claude-code-plugins-guide]] — Plugin 六元件系統與 Marketplace 生態
- [[multi-agent-cli-bridge]] — CCB 多 agent CLI 工作區
- [[parallel-code-multi-agent-gui]] — Parallel Code GUI 平行開發
- [[codex-advanced-tips-11]] — Codex 11 個進階技巧
- [[dual-tool-strategy]] — 雙工具策略的理論基礎
