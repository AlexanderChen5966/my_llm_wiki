---
title: Multi-Agent 合奏模式：Supervisor / Pipeline / Group Chat
date: 2026-06-15
tags: [ai/agent]
source: https://codelove.tw/@tony/post/3K15Ya
type: idea
status: processed
asset_value: 45
---

## 核心想法

Multi-Agent 三種組織模式：

1. **Supervisor 模式**：一個主 Agent 分配任務給專責 Agent，類似專案經理 + 團隊
2. **Pipeline 模式**：Agent A 輸出 → Agent B 處理 → Agent C 產出，線性流水線
3. **Group Chat 模式**：多個 Agent 在共享對話框中協作，適合需要多方意見的任務

文章有 Gemini API 實作的 Planner + Foodie + Transport + Coordinator 四 Agent 範例（Python）。

## 啟發

現在的 raw-pipeline 是三 Agent 軟編排（A→B→C），本質上是 Pipeline 模式。如果要升級，可以考慮加入 Supervisor Agent 處理異常情況（例如無法判斷分類時自動向上回報）。

## 參考

- [[ai-agent-architecture-patterns]] — AI Agent 架構模式（Workflow vs ReAct 等更完整的架構比較）
