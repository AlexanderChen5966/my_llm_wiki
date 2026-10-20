---
title: Agentic Engineering Patterns（Simon Willison 指南）
date: 2026-05-19
tags: [ai/agent, ai/workflow]
source: https://simonwillison.net/guides/agentic-engineering-patterns/
type: reference
status: processed
asset_value: 72
---

Simon Willison（Django 共同創作者、LLM 研究者）整理的 coding agent 最佳實踐指南，涵蓋原則層（鼓勵產出更好的程式碼）、實戰層（Git、Subagents）、測試 QA、程式碼理解，以及帶有詳細標注的真實 prompt 案例。與 Karpathy（品味哲學）和 Addy Osmani（工程紀律）構成 Agentic Engineering 三大聲音。

## T（核心論點）

Simon Willison 的核心主張：
1. **Writing code is cheap now**：程式碼生成成本趨近零，但好程式碼仍有成本——需要建立新的工程習慣
2. **Hoard things you know how to do**：積累你知道如何做的事物（可重組的「囤積品」），coding agents 讓重組能力更強大
3. **AI should help us produce better code**：AI 應該提升程式碼品質，而不是讓你更快地積累技術債
4. **Anti-patterns**：避免把未審閱的程式碼強加給協作者

## B（適用場景）
**適用：** 已在使用 Claude Code 或 Codex 的工程師，想建立更系統化的操作習慣
**不適用：** 完全不會寫程式的 Vibe Coder（這個指南假設有基本工程背景）

## R（操作步驟）

### 五大原則（Principles）
1. **什麼是 Agentic Engineering**（vs Vibe Coding 的區別在工程紀律）
2. **程式碼已經變便宜**：要建立對應的新習慣——review、測試、品質把關
3. **囤積你知道如何做的事**：建立個人的可組合技術庫，再用 Agent 重組
4. **AI 應該幫你產出更好的程式碼**：避免技術債、embrace compound engineering loop（複利工程迴圈）
5. **反模式**：不要把未審閱的 AI 程式碼推給隊友

### 與 Coding Agents 合作
- **理解 Agent 如何運作**：LLM + 系統 prompt + 工具在迴圈中運行
- **Git 整合**：core concepts + rewriting history + 具體 prompt 模式
- **Subagents**：
  - Claude Code 的 Explore subagent（探索模式）
  - Parallel subagents（平行執行）
  - Specialist subagents（分工角色）

### Testing & QA
- **Red/Green TDD**：先寫紅燈測試、再讓 Agent 通過
- **先跑既有測試**：在 Agent 動工前先確認基準
- **Agentic 手動測試**：
  - 瀏覽器自動化（browser automation for web UI）
  - Showboat 記錄測試過程

### 理解程式碼
- **線性 walkthrough**：讓 Agent 逐段解釋程式碼
- **互動式說明**：特定主題的深入解說（含 word cloud 理解範例）

### 帶標注的 Prompt 範例
- GIF 優化工具（WebAssembly + Gifsicle）：展示完整 follow-up prompt 策略
- 新增 blog-to-newsletter 內容類型：真實案例的 prompt 演進過程
- Prompts I Use：Simon 個人常用 prompt 集

## C（關鍵行動）
- [ ] 建立「程式碼囤積」習慣：把解決過的問題（尤其可重組的部分）記錄下來，供 Agent 重組使用
- [ ] 每次讓 Agent 產出程式碼後，執行 Red/Green TDD 確認品質
- [ ] 避免 anti-pattern：AI 產出的程式碼在 PR 前必須人工審閱

## 相關連結
- [[agentic-engineering-mindset]] — Karpathy 的 Agentic Engineering 哲學觀（品味與鬼魂比喻）
- [[harness-engineering]] — 更系統性的 Agent 環境工程（Guides × Sensors 框架）
- [[agents-md-guide]] — AGENTS.md 設計：context file 的效果研究與最佳實踐
- [[agentic-ai-learning-roadmap]] — 8 個等級成熟度評估 + 7 階段學習路徑
- [[spec-driven-ai-workflow]] — 規格驅動工作流（解決「缺乏規格共識」問題）
