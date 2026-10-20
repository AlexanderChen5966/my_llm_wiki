---
title: CLI 入門：AI Agent 為什麼離不開命令列
date: 2026-06-15
tags: [ai/agent, pkm/workflow]
source: https://launchdock.app/articles/cli-guide/
type: article
status: processed
asset_value: 45
---

## 核心洞察

AI Agent 與 CLI 天生契合：LLM 輸出是文字，CLI 的輸入也是文字。GUI 讓 AI 無法「點滑鼠」，但 CLI 可以讓 AI 直接生成並執行每一步。

## CLI 五大優勢（對 AI Agent 的角度）

1. **速度與效率**：一行指令 vs GUI 十幾個動作
2. **可重複性**：存成 Script，永遠可重複
3. **可自動化**：AI 生成 CLI 指令 = AI 可以做事
4. **可組合（管道魔法）**：`cmd1 | cmd2 | cmd3` 無限組合
5. **遠端操作**：`ssh` + CLI = 操作任何遠端主機

## AI Agent 使用 CLI 的流程

```
自然語言需求
    ↓
LLM 理解需求
    ↓
Agent 決定工具（grep/sed/awk/jq/curl…）
    ↓
生成 CLI 指令
    ↓
執行取得結果
    ↓
LLM 分析決定下一步
    ↓ （重複）
```

## 常用工具分類（AI Agent 視角）

| 類型 | 工具 | Agent 使用場景 |
|------|------|---------------|
| 搜尋 | grep / find | 搜尋 Skill 檔案、找 log 錯誤 |
| JSON | jq | 解析 API 回傳 |
| HTTP | curl | 呼叫外部 API |
| 文字 | sed / awk | 取代、欄位處理 |
| 版控 | git | 提交、分支管理 |

## 相關

- [[api-cli-skills-agent-architecture]] — CLI 在三層架構中的角色（含 Agent-Friendly CLI 設計原則）
- [[ai-agent-architecture-patterns]] — AI Agent 架構（CLI 如何整合）
