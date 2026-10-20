---
title: Claude Code /goal 指令：分離執行與評估的自動完成機制
date: 2026-06-02
tags: [ai/claude, ai/agent]
source: https://www.aiposthub.com/claude-code-goal-command-tutorial-automation-guide/, https://www.blocktempo.com/anthropic-claude-code-goals-evaluator-model-separates-agent-task-completion/
type: note
status: processed
asset_value: 48
---

## T（核心論點）

Claude Code v2.1.139 推出的 `/goal` 指令解決了 AI 代理「自己評判自己」的架構缺陷。核心設計是將任務執行與完成判斷分拆給兩個獨立模型：執行模型（Claude Opus/Sonnet）負責工作，評估模型（Claude Haiku）負責判斷完成條件是否成立。這是介於「完全手動陪盯」和「危險的跳過全部」之間的中間路徑。

## B（適用場景）

**適用：**
- 有明確可驗證終點的開發任務（測試通過、build 成功、lint 乾淨）
- 多輪自動執行（不確定要幾輪，但知道「做完」長什麼樣）
- 搭配 Auto Mode 實現真正的「設好就走」

**不適用 / 注意：**
- 模糊條件（如「code is clean」）會導致提前收工
- 每個 session 只能有一個 active goal
- 底層是 Stop hook，需 workspace 信任 + `disableAllHooks` 未啟用
- 與 `/loop`（固定時間間隔觸發）和 Stop hook（永久設定）用途不同

## R（操作步驟）

1. **寫出可驗證條件**（三要素）：
   - 可量測的終點：`npm test exits 0`
   - 驗證方式：`git status is clean`
   - 不能動的範圍：`no other test file is modified`
2. **設定 goal**：`/goal all tests in test/auth pass and the lint step is clean`
3. **最強組合**：`claude --enable-auto-mode` → `Shift+Tab` 切 Auto Mode → `/goal <條件>`
   - Auto Mode 省工具確認 + /goal 省輪次推進 = 真正「設好就走」
4. **安全上限**：可加 `or stop after 20 turns` 防止無限迴圈
5. **管理指令**：`/goal clear`、`/goal pause`、`/goal resume`

### 三種模式接力

| 模式 | 解決的問題 | 適合時機 |
|------|-----------|---------|
| **Plan Mode** | 避免 Claude 誤解任務範圍 | 動工前，陌生 codebase |
| **/goal** | 省去輪次之間的手動推進 | 動工中，目標明確 |
| **Auto Mode** | 省去輪次內的工具確認 | 搭配 /goal 使用 |

### /goal vs /loop vs Stop hook

| 工具 | 觸發條件 | 停止條件 | 適合情境 |
|------|---------|---------|---------|
| /goal | 上一輪結束 | 模型確認條件成立 | 有明確驗證條件的任務 |
| /loop | 固定時間間隔 | 手動或 Claude 自決 | 等待外部狀態（CI、API） |
| Stop hook | 上一輪結束 | 自訂腳本或 prompt | 跨 session 的固定規則 |

## C（關鍵行動）

- [ ] 練習撰寫「評估模型看得懂」的可驗證條件
- [ ] 嘗試 /goal + Auto Mode 組合跑一個完整任務
- [ ] 區分 /goal、/loop、Stop hook 的使用時機

## 原始摘要

（來源一）Claude Code v2.1.139 推出的 /goal 指令讓使用者設定完成條件後，由獨立的評估模型（Claude Haiku）判斷任務是否達成，實現「設好就走」的多輪自動執行，並詳細解說如何與 Plan Mode、Auto Mode 搭配使用。

（來源二）Anthropic 為 Claude Code 推出 /goals 指令，核心設計是將任務執行與完成判斷分拆給兩個獨立模型，解決 AI 代理「自己評判自己」導致提早收工的架構性缺陷。

## 相關連結

- [[claude-code-dynamic-workflows]] — Dynamic Workflows 平行 subagent 機制
- [[claude-code-advanced-workflow]] — Boris Cherny 進階技巧包含 /loop 排程
- [[claude-code-commands-2026]] — 2026 上半年新命令總覽
- [[codex-advanced-tips-11]] — Codex 也有 /goal（實驗性功能），社群曾跑 14 小時不停
- [[agentic-engineering-sop]] — Agentic Engineering 六階段 SOP 中的自動化執行環節
