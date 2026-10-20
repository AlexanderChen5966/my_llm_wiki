---
title: Claude Code 進階工作流：Boris Cherny 的 15 個技巧
date: 2026-05-20
tags: [ai/claude, ai/workflow, ai/agent]
source: https://www.bnext.com.tw/article/90750/claude-code-advanced-workflow
type: article
status: processed
asset_value: 60
---

Claude Code 之父 Boris Cherny 揭露的 15 項進階技巧，核心主題：讓 AI 自己排程、自己決策、自己跨設備工作——人的角色從執行者退到架構師。

## T（核心論點）
Claude Code 真正的進階用法不是「更快打指令」，而是讓它在你不在的時候繼續跑：用 `/schedule` 定期任務、`/loop` 監控循環、Hooks 自動觸發、`/batch` 大規模並行——你設計系統，它運作系統。

## B（適用場景）
**適用：** 已熟悉基本 Claude Code 操作，想進入「AI 自動化工作流」階段的開發者
**不適用：** 剛開始用 Claude Code 的新手（先看 [[claude-code-commands-reference]]）

## R（15 個技巧）

### 跨設備與行動
1. **行動版 Claude Code**：iOS / Android app 可直接操作，出門在外不需開電腦
2. **`/teleport`（跨設備傳送）**：把桌面工作階段遷移到手機，也可從 claude.ai 網頁版拉進終端機

### 自動化排程
3. **`/loop`（循環執行）**：讓 Skill 按固定間隔自動重複，例如 `/loop 5m /babysit` 每五分鐘監控一次
4. **`/schedule`（雲端排程）**：建立雲端排程工作，最長支援一週的定時任務——AI 在你睡覺時工作

### Hooks（事件觸發）
5. **Hooks 系統**：特定事件觸發 Shell 命令，四種類型：
   - `SessionStart`：每次對話開始時執行（自動讀取 lessons.md）
   - `PreToolUse`：工具呼叫前執行（安全驗證、lint 檢查）
   - `PermissionRequest`：權限請求前執行（自訂批准邏輯）
   - `Stop`：Claude 停止時執行（自動摘要、通知）

### 遠端協作
6. **Cowork Dispatch（遠端控制）**：允許從 claude.ai 或其他裝置遠端控制正在運行的 Claude Code
7. **Chrome 擴充功能**：Claude 可操作 Chrome 做視覺驗證（截圖確認 UI 正確性）
8. **Desktop App**：Claude Code 桌面 app 自動啟動本地 Web Server，整合更順暢

### 工作階段管理
9. **`/branch`（工作階段分叉）**：建立對話分支實驗不同方向，不影響主線——A/B 測試工作流
10. **`/btw`（側邊問題）**：提問但不寫入對話歷史，不消耗 Context（例：「/btw 這邊用什麼設計模式？」）

### Git 工作流整合
11. **Git Worktrees（`claude -w`）**：多個 Worktree 同時開不同 Claude 工作階段，真正並行開發
12. **`/batch`（大規模並行）**：自動拆解任務並行執行，每個分支開 PR——一次提交 20 個功能

### SDK 效能優化
13. **`--bare` 模式**：SDK 啟動速度提升 10 倍，適合 CI/CD 整合（跳過 UI 初始化）
14. **`--add-dir`（跨 Repo 存取）**：在一個工作階段同時存取多個 Git 倉庫，無需切換目錄

### 自訂 Agent
15. **`--agent`（自訂 Agent）**：建立有自己系統 Prompt 的 Agent，搭配 `/voice` 語音輸入操控

## C（關鍵行動）
- [ ] 設定 `SessionStart` Hook 自動讀取 `tasks/lessons.md`，每次對話帶入歷史教訓
- [ ] 用 `/schedule` 把重複的監控任務（CI 健康、備份驗證）自動化
- [ ] 大型功能開發用 Git Worktrees + `/batch` 並行，縮短迭代週期
- [ ] `/btw` 取代「先停下來問問題」的習慣，保持 Context 乾淨

## 相關連結
- [[claude-code-commands-reference]] — 完整命令速查（含 `/loop`、`/schedule`、`/btw` 的語法）
- [[claude-md-guide]] — Boris Cherny 的 CLAUDE.md 六大核心引擎
- [[claude-code-context-management]] — Context 管控策略（配合 Hooks 使用）
- [[harness-engineering]] — Hooks 是 Harness 工程的重要組成部分
