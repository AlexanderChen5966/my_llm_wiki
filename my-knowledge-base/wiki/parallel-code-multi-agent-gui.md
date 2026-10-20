---
title: Parallel Code：多 Agent 平行 GUI 工作區
date: 2026-06-02
tags: [ai/agent, ai/workflow]
source: https://github.com/johannesjo/parallel-code
type: note
status: processed
asset_value: 40
---

## T（核心論點）

Parallel Code 是一個開源 GUI 應用程式（Electron），讓多個 AI 編碼代理（Claude Code、Codex、Gemini、Copilot、Antigravity）在各自獨立的 git worktree 中平行工作。核心主張：「十個 agent，十個 branch，一個下午」——每個任務自動建立 branch + worktree，零衝突平行開發，完成後合併回 main。

## B（適用場景）

**適用：**
- 需要同時派多個 agent 處理多個 feature 的平行開發
- 偏好 GUI 操作（面板佈局、拖曳排序）而非 tmux
- 需要 diff 審查 + inline review comments 功能
- 想從手機監控 agent 進度（QR code + Wi-Fi/Tailscale）

**不適用 / 注意：**
- macOS 和 Linux only
- 需要至少一個 AI coding CLI 已安裝
- Antigravity CLI 不支援 Docker 隔離模式（需原生執行）
- 與 CCB 的差異：Parallel Code 是 GUI + worktree 隔離；CCB 是 tmux + 角色協作

## R（操作步驟）

1. **下載安裝**：從 [Releases](https://github.com/johannesjo/parallel-code/releases) 下載 .dmg（macOS）或 .AppImage/.deb（Linux）
2. **安裝至少一個 AI CLI**（Claude Code / Codex / Gemini / Antigravity / Copilot）
3. **開啟 Parallel Code**，指向一個 git repo
4. **建立任務**（Ctrl+N）：自動建 branch + worktree + symlink node_modules
5. **平行派遣**：每個面板一個 agent，各自獨立工作
6. **審查合併**：內建 diff viewer + per-commit 導航 → merge to main

### 特色功能

- **Focus Mode**（Ctrl+Shift+F）：單任務雙欄佈局
- **Steps Tracking Panel**：工程管理風格的 agent 進度時間軸
- **Notes Panel**：記筆記 → 直接發送為 prompt
- **PR CI Status Watcher**：GitHub checks 完成時桌面通知
- **Coverage Radar**：Changed Files 面板顯示測試覆蓋率
- **Docker Sandboxing**：`.parallel-code/Dockerfile` 隔離執行
- **AI Arena Mode**：讓 agent 競速比較

## C（關鍵行動）

- [ ] 試用 Parallel Code，評估 GUI vs tmux（CCB）哪個更適合自己的工作流
- [ ] 嘗試對同一個功能派遣不同 agent（Claude vs Codex）比較結果

## 原始摘要

Parallel Code 是一個開源 GUI 應用程式，能讓多個 AI 編碼代理（Claude Code、Codex、Gemini）在各自獨立的 git worktree 中平行工作，提供面板佈局、diff 審查、合併管理與手機監控等功能。

## 相關連結
- [[cc-gui-intellij-claude-code-plugin]] — 另一種 GUI 路線：不做獨立介面而是嵌進 IntelliJ IDEA
- [[multi-agent-cli-bridge]] — CCB：tmux 版的多 agent 工作區（CLI 取向）
- [[codex-plugin-for-claude-code]] — 在 Claude Code 內直接呼叫 Codex
- [[claude-code-dynamic-workflows]] — Claude 原生的平行 subagent 機制
- [[synthesis-multi-agent]] — Multi-Agent 系統知識精粹（本主題 7 篇整合）
