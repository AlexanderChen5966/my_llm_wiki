---
title: Claude-Code-Game-Studios：49 Agent 遊戲開發工作室架構
date: 2026-06-01
tags: [ai/agent, ai/workflow]
source: https://github.com/Donchitos/Claude-Code-Game-Studios
type: note
status: processed
asset_value: 50
---

開源專案 Claude-Code-Game-Studios，將單一 Claude Code session 擴展為擁有 49 個專業 AI Agent、73 個工作流 Skill 的完整遊戲開發工作室架構，仿真實遊戲公司三層層級（導演/部門主管/專業人員），提供品質把關、設計審查與結構化協作流程。採用「協作而非自動駕駛」哲學——每個決定都由使用者批准。

## T（核心論點）

問題：單一 Claude Code session 沒有結構——沒有人阻止你硬編碼魔法數字、跳過設計文件、或寫義大利麵程式碼。**解法：給 AI session 一個真實工作室的組織結構**。

**三層 Agent 層級（仿真實遊戲公司）：**
- Tier 1（Directors / Opus）：creative-director、technical-director、producer——負責守護願景
- Tier 2（Department Leads / Sonnet）：game-designer、lead-programmer、art-director、qa-lead 等 8 個——負責各領域決策
- Tier 3（Specialists / Sonnet/Haiku）：gameplay-programmer、level-designer、qa-tester 等 30+ 個——執行具體工作

**Engine 支援：**Godot 4 / Unity / Unreal Engine 5，各有對應 Specialist agent 套件

**73 個 Slash Command 涵蓋完整開發週期：**
- 設計：/brainstorm、/design-system、/map-systems
- 架構：/create-architecture、/architecture-review
- Sprint：/create-epics、/create-stories、/dev-story、/sprint-plan
- QA：/qa-plan、/smoke-check、/regression-suite
- 發布：/release-checklist、/launch-checklist、/patch-notes

**12 個 Hooks 自動化安全網：**
- validate-commit.sh：檢查硬編碼值、TODO 格式、JSON 有效性
- validate-assets.sh：檢查命名規範
- detect-gaps.sh：偵測遺漏設計文件
- session-start/stop：Session 開啟/關閉的 git 狀態記錄

**協作協議（非自動駕駛）：**
每個 Agent 遵循：提問 → 呈現 2-4 選項（含 pros/cons）→ 使用者決定 → 起草 → 批准後才執行

## B（適用場景）

**適用：**
- 獨立遊戲開發者需要結構化 AI 協作框架
- 研究大規模多 Agent 架構設計模式的工程師
- 想理解「如何把 AI 組織成虛擬團隊」的架構師

**不適用 / 注意：**
- 主要開發/測試在 Windows（macOS/Linux cross-platform 測試進行中）
- 是模板，不是鎖定框架——預設所有內容可客製化（刪改 Agent、調整 Skill、修改 Hook）
- review 強度有三檔：full（所有 director 關卡）、lean（只有階段關卡）、solo（無審查）

## R（操作步驟）

1. `git clone https://github.com/Donchitos/Claude-Code-Game-Studios.git my-game`
2. 在 Claude Code 開啟 session：`claude`
3. 執行 `/start`——系統詢問你在哪個階段（無想法/模糊概念/明確設計/既有專案）並引導到正確工作流
4. 根據引擎執行 `/setup-engine godot 4.6` 或對應指令
5. 開始設計：`/brainstorm` → `/design-system` → `/create-architecture`
6. 開發迭代：`/create-stories` → `/dev-story` → `/story-done`

## C（關鍵行動）

- [ ] 研究 49 個 Agent 的 Delegation Model 設計，作為其他多 Agent 架構的參考
- [ ] 提取 12 個 Hooks 的設計模式（特別是 validate-commit.sh 的提早退出邏輯）
- [ ] 比較此架構與 Claude-Code-Workflow（CCW）的 beat model 的異同

## 原始摘要

開源專案 Claude-Code-Game-Studios，將單一 Claude Code session 擴展為擁有 49 個專業 AI Agent、72 個工作流 Skill 的完整遊戲開發工作室架構，仿真實遊戲公司層級（導演、部門主管、專業人員），提供品質把關、設計審查與結構化協作流程。

## 相關連結

- [[claude-code-workflow-multi-agent-framework]] — CCW：另一個多 Agent 框架（beat model）
- [[gstack-framework]] — gstack：完整 Sprint 流程框架
- [[agent-framework-stability-comparison]] — 四個框架穩定性哲學比較
- [[harness-engineering]] — Harness Engineering：護城河在環境而非模型
