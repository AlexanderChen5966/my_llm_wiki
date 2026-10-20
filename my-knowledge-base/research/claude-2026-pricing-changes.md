---
title: Claude 2026 計費調整：互動式與程式化用量分池
date: 2026-05-28
tags: [ai/claude, ai/workflow]
source: https://www.bnext.com.tw/article/90109/claude-ai
type: article
status: processed
asset_value: 18
---

## 核心論點

Anthropic 2026/6/15 起將 Claude 用量拆成兩個獨立額度池：互動式（claude.ai、Claude Code 對話）與程式化（Agent SDK、`claude -p`、GitHub Actions），防止第三方工具（Conductor、OpenClaw）耗盡訂閱額度。

## 變化重點

| 項目 | 舊制 | 新制（6/15 起）|
|------|------|--------------|
| 額度池 | 互動式 + 程式化共用 | 各自獨立池 |
| Agent SDK 用盡後 | 影響 claude.ai 使用 | 暫停至下期或按 API 費率計費 |
| 啟用「額外用量」| — | 用盡後以完整 API 費率計費 |

## 最新模型（2026-05）

- Opus 4.7：最強，編碼 + 企業代理
- Sonnet 4.6：性價比最高，日常主力
- Haiku 4.5：最快，輕量任務

## 相關連結

- [[llm-cost-optimization-strategies]]
- [[claude-pilot-suite]]
