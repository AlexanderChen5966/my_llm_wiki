---
title: AI 使用五層級：從聊天到 Agent 自動化
date: 2026-06-15
tags: [ai/agent, pkm/workflow]
source: https://codelove.tw/@tony/post/3Kp5Ya
type: idea
status: processed
asset_value: 45
---

## 核心想法

AI 使用方式有五個層級，多數人停在前兩層：

1. **聊天層**：直接問 AI 問題，一問一答
2. **檔案生成層**：要 AI 輸出文件、程式碼、報告
3. **設定檔層**：透過 CLAUDE.md / system prompt 讓 AI 遵守規則
4. **MCP + Skills 層**：AI 能呼叫外部工具、執行結構化工作流
5. **Agent 自動化層**：AI 自主完成多步驟任務，不需要人工介入每一步

## 啟發

現在我的知識庫已經到第 3-4 層（CLAUDE.md + Skills），下一步是第 5 層：讓 raw-pipeline 完全自動化，不需要手動觸發。

可能做法：
- Telegram Connector + 文章轉發 → 自動觸發 knowledge-organizer
- 每週日固定 /schedule raw-pipeline 批次處理

## 參考

- [[claude-cowork-setup-guide]] — Cowork 設定（第 5 層的實作工具）
- [[loop-engineering-6-components]] — Loop Engineering（自動化設計框架）
