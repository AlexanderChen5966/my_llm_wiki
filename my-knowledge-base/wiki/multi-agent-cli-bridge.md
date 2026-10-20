---
title: CCB：基於 tmux 的多 Agent CLI 工作區
date: 2026-06-02
tags: [ai/agent, ai/workflow]
source: https://github.com/SeemSeam/claude_codex_bridge
type: note
status: processed
asset_value: 45
---

## T（核心論點）

CCB（claude_codex_bridge）是一個基於 tmux 的可視化多 agent CLI 工作區，能在同一個專案中同時運行 Claude、Codex、Gemini、OpenCode、Droid 等不同供應商的 CLI agent。核心價值是角色分離（main 規劃 / worker 實作 / reviewer 審查）、平行進度、以及混合供應商模型分層，解決單一 agent 在上下文焦點、執行效率、工具權限方面的局限。

## B（適用場景）

**適用：**
- 需要多 agent 角色分離的開發專案（規劃 / 實作 / 審查）
- 跨供應商 agent 協作（Claude + Codex + Gemini 混用）
- 需要 worktree 隔離的平行開發
- 偏好可視化終端介面（tmux pane 可點擊切換）

**不適用 / 注意：**
- 不是自動 DAG 生成器，複雜工作流需手動設計
- 需要 Python 3.10+、tmux、至少一個 agent CLI
- 目前不支援原生 Windows（需 WSL）
- 與 Claude Code 原生 subagent / Hive 定位不同（見比較表）

## R（操作步驟）

1. **安裝**：下載 Release 包 → `./install.sh install`（或 `ccb update` 更新）
2. **建立專案配置** `.ccb/ccb.config`：
   ```
   version = 2
   entry_window = "main"
   [windows]
   main = "main:codex"
   work = "worker1:codex(worktree), worker2:claude(worktree)"
   review = "reviewer:claude, qa:gemini"
   ```
3. **啟動**：`ccb`（重新附接）或 `ccb -n`（重建）
4. **Agent 間協作**：
   - `/ask reviewer review the latest parser changes` — 提交工作給目標 agent
   - `ask --callback reviewer` — 需要子結果返回時用 callback
   - `ask --silence worker1` — 獨立工作不需返回
5. **動態重載**：`ccb reload --dry-run` 預覽 → `ccb reload` 套用

### 三種多 Agent 方案比較

| 方案 | 定位 | 最適合 |
|------|------|--------|
| Claude Code 原生 subagent | 單一供應商內的委派 | 主要使用 Claude Code |
| Hive / OpenHive | 生產級工作流 harness | 需要狀態恢復、可觀察性、成本控制 |
| CCB | 可視化本地 CLI 工作區 | 混合供應商的本地開發 |

## C（關鍵行動）

- [ ] 評估 CCB 是否適合目前的多 agent 開發需求
- [ ] 若採用，設計第一個 `.ccb/ccb.config`（可用 `ccb-config` skill 輔助）

## 原始摘要

CCB（claude_codex_bridge）是一個基於 tmux 的多 agent CLI 工作區工具，能在同一個專案中同時運行 Claude、Codex、Gemini 等不同供應商的 CLI agent，支援角色分離、平行執行、worktree 隔離與 agent 間委派協作。

## 相關連結

- [[parallel-code-multi-agent-gui]] — 另一個多 agent 平行工具（GUI 版本）
- [[codex-plugin-for-claude-code]] — OpenAI 官方的 Claude Code 內 Codex 整合
- [[ai-coding-agent-governance-dotbabel]] — dotbabel 跨 CLI 治理工具
- [[research/dev-workflow-optimization-tools]] — 開發流程優化工具評估中有 CCB
- [[synthesis-multi-agent]] — Multi-Agent 系統知識精粹（本主題 7 篇整合）
