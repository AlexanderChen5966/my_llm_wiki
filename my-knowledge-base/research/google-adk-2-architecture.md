---
title: Google ADK 2.0 架構研究：從 Agent 到 Graph
date: 2026-05-19
tags: [ai/agent, ai/workflow]
source: /Users/alexander/notes/my-knowledge-base/raw/AI Agent  Google ADK 2.0 Beta Version 研究— 從 Agent 到 Graph：Google ADK 2.0 到底改了什麼？.md
type: article
status: processed
asset_value: 45
---

核心論點：Google ADK 2.0 從 Agent-centric 轉向 Graph-centric 架構，引入有向無環圖（DAG）作為 Agent 協作的基礎結構，使複雜多 Agent 任務更易於組合和追蹤。

**主要變化**：
- ADK 1.x：以 Agent 為中心，用 orchestrator-worker 模式
- ADK 2.0：以 Graph 為中心，Agent 成為 Graph 中的節點，邊定義資料流向和觸發條件

**與個人工具棧的距離**：Google ADK 是 Python-first 框架，主要與 Google Cloud 生態整合，與目前使用的 Claude Code / MCP 工具棧較遠。

**存入 research/ 原因**：
- 使用頻率低（非核心工具棧）
- Graph-centric 架構是值得追蹤的設計趨勢
- 若日後 ADK 成為常用工具可升級為 wiki 條目

**相關條目**：[[agentic-ai-learning-roadmap]]（Stage 7 多 Agent 協作）、[[harness-engineering]]（Agent 環境設計原則）
