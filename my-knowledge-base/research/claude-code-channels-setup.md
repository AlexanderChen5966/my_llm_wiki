---
title: Claude Code Channels：Discord / Telegram 遠端觸發設定
date: 2026-05-28
tags: [ai/claude, ai/workflow]
source: https://www.darrelltw.com/claude-code-channels-discord-telegram/
type: article
status: processed
asset_value: 27
---

## 核心論點

Claude Code Channels 讓你從手機 Discord / Telegram 傳訊息觸發正在跑的 Claude Code session，不需坐在電腦前；Channels 是 MCP server，用 `--channels` 啟動，不同於 Remote Control（遠端桌面）。

## Channels vs Remote Control

| 項目 | Remote Control | Channels |
|------|---------------|---------|
| 本質 | 遠端操作同 session | 外部訊息推入 session |
| 介面 | claude.ai/code、Claude App | Discord Bot、Telegram Bot |
| 接收外部事件 | ❌ | ✅ |
| 多人使用 | ❌ | ✅（allowlist 控制）|
| 啟動 | `claude --remote-control` | `claude --channels plugin:...` |

## 設定步驟（以 Telegram 為例）

1. 建立 Telegram Bot（BotFather）
2. 取得 bot token，設定 allowlist
3. Claude Code 啟動加 `--channels plugin:telegram` 參數
4. 手機傳訊息 → Claude 執行 → 結果推回手機

## Permission Relay

敏感操作時，Claude 推送 Allow / Deny 按鈕到手機，讓你遠端核准，不需回到電腦。

## 相關連結

- [[claude-code-advanced-workflow]]
- [[claude-code-plugins-guide]]
