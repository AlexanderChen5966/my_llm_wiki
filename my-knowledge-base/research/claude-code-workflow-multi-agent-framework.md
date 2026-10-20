---
title: Claude-Code-Workflow：JSON 驅動多 Agent cadence-team 開發框架
date: 2026-06-01
tags: [ai/agent, ai/workflow]
source: https://github.com/catlog22/Claude-Code-Workflow
type: note
status: processed
asset_value: 64
---

開源框架 Claude-Code-Workflow（CCW），以 JSON 驅動的多 Agent cadence-team 開發架構，支援 Gemini、Qwen、Codex、Claude 多 CLI 智慧切換，提供 Skill 型工作流、Session 生命週期管理、終端儀表板與視覺化 Orchestrator 編輯器。架構特色是 event-driven beat model（心跳模型），讓 Coordinator 只在需要時喚醒，實現高效率並行。

## T（核心論點）

CCW 解決「多 CLI / 多 Agent 協作」的編排問題，核心創新：

1. **語意化 CLI 呼叫**：使用者在 prompt 中自然語言指定工具（「用 Gemini 分析 auth 模組」），系統自動呼叫對應 CLI
2. **Team Architecture v2（Beat Model）**：Event-driven 心跳模型——Coordinator 只在收到 callback/resume 時喚醒，避免持續輪詢。獨立任務並行執行，不需 Coordinator roundtrip
3. **37 個 Skill + 22 個 Agent**：涵蓋 workflow-lite-plan（輕量）到 brainstorm（多角色分析）到 workflow-tdd-plan（TDD）的完整開發週期
4. **Session 生命週期管理**：start/resume/complete/sync，解決長時間開發工作流的狀態持久化問題

**架構四層：**
- Workflow Skills（觸發詞層）
- Team Architecture v2（多 Agent 協調層）
- Queue Scheduler Service（背景執行層）
- Terminal Dashboard & Orchestrator（視覺化監控層）

**多 CLI 協作模式：**
- Collaborative：Gemini + Codex 共同分析安全漏洞
- Parallel：三個 CLI 同時分析架構
- Pipeline：Gemini 設計 → Codex 實作 → Claude 審查

## B（適用場景）

**適用：**
- 同時使用多個 AI CLI 工具（Claude Code、Gemini CLI、Codex）的開發者
- 需要 Session 持久化、跨多天執行的長型開發任務
- 想有視覺化儀表板監控多 Agent 執行狀態的團隊

**不適用 / 注意：**
- 安裝需要 npm（`npm install -g claude-code-workflow`）
- Codex 配置需要在 `~/.codex/config.toml` 啟用多個 features flag
- CodexLens（本地語意搜尋）仍在開發中，部分功能不穩定
- ACE Tool 需要額外設定（Augment Context Engine）

## R（操作步驟）

1. 安裝：`npm install -g claude-code-workflow && ccw install -m Global`
2. Codex 配置：在 `~/.codex/config.toml` 啟用 `multi_agent`、`multi_agent_v2`、`enable_fanout`
3. 選擇工作流 Skill：
   - 輕量任務 → `workflow-lite-plan`
   - 完整 Session → `workflow-plan`
   - TDD → `workflow-tdd-plan`
   - 腦力激盪 → `brainstorm`
4. Session 管理：`/workflow/session:start` → 工作 → `/workflow/session:complete`
5. 視覺化：`ccw view` 開啟 Terminal Dashboard

## C（關鍵行動）

- [ ] 評估是否有需要多 CLI 協作的場景
- [ ] 安裝 CCW 並試用 `workflow-lite-plan` 一個小功能
- [ ] 對比與 gstack、Superpowers 框架的定位差異

## 原始摘要

開源框架 Claude-Code-Workflow，以 JSON 驅動的多 Agent cadence-team 開發架構，支援 Gemini、Qwen、Codex、Claude 多 CLI 智慧切換，提供 Skill 型工作流、Session 生命週期管理、終端儀表板與視覺化 Orchestrator 編輯器。

## 相關連結

- [[gstack-framework]] — gstack：23+ Skill 完整 Sprint 流程
- [[superpowers-framework]] — Superpowers：15 個 Skill 強制 TDD 工程紀律
- [[gemini-cli-subagent]] — Gemini CLI 接入 Claude Code 的設定
- [[dual-tool-strategy]] — 雙工具策略：Gemini CLI 探索 + Claude Code 執行
- [[claude-code-game-studios-multi-agent]] — 另一個大型多 Agent 框架（遊戲開發）
