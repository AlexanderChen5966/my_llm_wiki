---
title: gnhf：讓 AI Agent 整晚自主迭代的 Orchestrator
date: 2026-05-28
tags: [ai/agent, ai/workflow]
source: https://tools.wingzero.tw/article/sn/3923
source_2: https://github.com/kunchenguid/gnhf
type: article
status: processed
asset_value: 27
---

## 核心論點

gnhf（good night, have fun）是 autonomous orchestration runtime，讓 AI coding agent 在你睡覺時持續背景開發：每次迭代做一個小變更、自動 commit、記錄過程，失敗自動 rollback——醒來就有一個 branch 充滿乾淨的 commits。

## 運作方式

1. 使用者設定目標並執行 `gnhf` 指令
2. Agent 自主迭代：小變更 → commit → 記錄 → 失敗則 rollback
3. 持續循環直到用戶停止或達到 runtime 上限
4. 醒來後查看 branch + 完整 log

## 與其他工具的差異

| 工具 | 定位 |
|------|------|
| Claude Code `/loop` | 單一 session 迴圈，需保持終端機開著 |
| Claude Code `/schedule` | 雲端排程，每次獨立任務 |
| **gnhf** | 本地長時間 autonomous loop，自動 commit + rollback |

## 適用場景

- 不複雜但需要大量迭代的任務（bug fix、重構、文件更新）
- 有清晰「完成標準」可讓 agent 自行判斷的任務
- 願意接受 agent 自主決策風險的開發者

## 注意

不適合需要人工審核每個步驟的任務。Agent 會自主 commit，需要確認 git history 是否乾淨。

## 相關連結

- [[claude-code-advanced-workflow]] — /loop 排程與長任務工作流（類似概念）
- [[agentic-engineering-sop]] — Agentic Engineering SOP（設定清楚完成標準的必要性）
- [[harness-engineering]] — Harness 工程（gnhf 是 Sensor+Guardrail 層的實作）
