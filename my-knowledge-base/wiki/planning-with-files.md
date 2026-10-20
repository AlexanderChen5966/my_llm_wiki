---
title: planning-with-files：文件驅動 AI Coding Agent 開發工作流
date: 2026-06-02
tags: [ai/agent, ai/workflow]
source: https://tools.wingzero.tw/article/sn/3930
type: article
status: processed
asset_value: 60
---

planning-with-files 是一套以 Markdown 文件為核心的 AI Agent 開發方法論，透過將需求分析、技術設計、執行計畫與進度追蹤獨立成檔案，解決 AI Agent 在大型專案中上下文遺失的問題。不是新工具，而是工程實踐（Engineering Workflow）。

## T（核心論點）

AI Agent 在長期專案中最大的瓶頸不是能力，而是**上下文遺失**。planning-with-files 的解法：把規劃文件當作 Agent 的長期記憶——需求分析、設計決策、任務清單全部寫成 Markdown，Agent 每次啟動都先讀這些檔案，而非只依賴當前對話。本質是把傳統軟體工程的規格書/設計文件概念，重新套用到 AI Agent 工作流。

## B（適用場景）

**適用：**
- 中大型軟體專案（數週到數月開發週期）
- 需要跨 Session 維持一致性的場景
- 多人協作需要可追溯性的團隊
- 任何 AI Coding Agent（Claude Code、Cursor、Copilot Agent、OpenCode）

**不適用 / 注意：**
- 一次性小任務（overhead 太高）
- 需要團隊持續維護文件的紀律，文件過時會導致 Agent 依據錯誤資訊行動
- 不是自動化工具，是規範與流程

## R（核心概念）

### 文件化規劃的四個面向

1. **需求文件**：明確記錄要解決的問題與目標
2. **設計文件**：架構決策、技術選型、API 設計
3. **執行計畫**：拆解後的任務清單，含優先順序與依賴關係
4. **進度追蹤**：每個任務的狀態、完成時間、遇到的問題

### 與其他方案的定位比較

| 方案 | 解決什麼 | 路線 |
|------|---------|------|
| Agent Memory（向量資料庫、MCP Memory）| 自動記住過去經驗 | 隱式記憶 |
| planning-with-files | 維護正式規格與執行藍圖 | 顯式文件 |
| CLAUDE.md / Rules | 專案層級的行為規範 | 靜態配置 |

三者互補：Memory 記錄經驗，planning-with-files 維護規格，CLAUDE.md 定義規範。

### 關鍵設計原則

- **先規劃、後實作**：Agent 開工前必須先完成完整計畫
- **文件即記憶**：所有規劃存在版本控制系統中，跨 Session 持續可用
- **透明可追溯**：人類和 AI 都能透過文件快速理解專案背景
- **不依賴特定模型或服務**：純 Markdown，零基礎設施需求

## C（關鍵行動）

- [ ] 評估目前專案是否有上下文遺失問題，適合導入此方法
- [ ] 與 [[persistent-markdown-planning-skill]] 比較：後者是此方法論的具體 Skill 實作（Manus 風格三檔案）
- [ ] 可與 [[superpowers-framework]] 的 writing-plans Skill 搭配（更強制的任務拆解紀律）

## 原始摘要

DeTools 工具翼零介紹的 planning-with-files 方法論，核心是透過 Markdown 文件驅動 AI Coding Agent 的長期開發工作流，把需求分析、技術設計、執行計畫與進度追蹤全部文件化，解決 AI Agent 在大型專案中上下文遺失的問題。

## 相關連結

- [[persistent-markdown-planning-skill]] — 此方法論的具體 Skill 實作（Manus 風格三檔案持久化規劃）
- [[agentic-engineering-sop]] — 更完整的六階段 SOP（Spec/Plan/Tasks 三層落檔）
- [[spec-driven-ai-workflow]] — 規格先行的另一種實踐路線
- [[claude-code-context-management]] — Claude Code 的 Context 管控策略
- [[agentmemory]] — 隱式記憶方案，與文件化規劃互補
