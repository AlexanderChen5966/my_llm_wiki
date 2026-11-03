---
title: AI Coding 環境設定：先 VS Code／Git／GitHub 並完成第一次 Push，最後才接 Claude Code 或 Codex
date: 2026-10-30
tags: [ai/claude, ai/workflow]
source: https://www.aiposthub.com/ai-coding-environment-github-vscode-claude-code-codex/
type: article
status: processed
asset_value: 27
---

## T（核心論點）

AI Coding 的環境目標，是讓 AI 在**可保存、可回復、可同步**的專案裡工作。穩定順序是先建好 VS Code／Git／GitHub／連線並**親手完成第一次 Commit 與 Push**，最後才接入 Claude Code 或 Codex；前面的 Git 與 GitHub 基礎即使換 AI 工具仍可沿用。

🔴 單一來源（aiposthub／Philo），2026-09-30 查詢的版本資訊。

## B（適用場景）
**適用：** 新手第一次建立 AI Coding 環境（Windows／macOS 皆可）。
**不適用 / 注意：**
- Claude Code VS Code 擴充功能要求 VS Code 1.94.0 以上（2026-09-30 查詢），可用付費訂閱或 Console 帳號登入
- 只裝擴充功能不算完成；完成標準是「AI 能讀專案，且你能在 Diff 中檢查修改」
- 涉及刪除、部署、付款、正式環境的操作不應只靠 AI 自動執行

## R（操作步驟）
五個角色：編輯（VS Code）、版本（Git）、雲端（GitHub）、連線（SSH／HTTPS）、AI（Claude Code／Codex）。七步：
1. GitHub 帳號＋Private Repo（先開兩步驟驗證，建議加 README）
2. 安裝 VS Code 與 Git，`git --version` 確認
3. 設定 `user.name`／`user.email`（用 GitHub 帳號 Email 或隱私 Email；密碼與 API key 不進 Git 設定與專案檔）
4. 選 HTTPS（GitHub CLI／Credential Manager）或 SSH（私鑰設 Passphrase、只留本機）
5. `git clone` 並用 VS Code 開啟
6. 改 README → `git status`／`add`／`commit`／`push`，回 GitHub 確認
7. 接入 Claude Code／Codex，第一次只請 AI 解釋 README 或新增簡單檔案，再檢查 Diff

CLAUDE.md 在專案存在後用 `/init` 產生並人工檢查；第一版只留用途與目錄、指令、可改／不可改範圍、完成前檢查，規則過長會降低遵守度。

AI 改錯時：先停止擴大修改，用 `git status` 與 Diff 判斷。

## C（關鍵行動）
- [ ] 依八項完成檢查表逐項確認環境
- [ ] 交付大型任務前，先用小任務驗證 Diff 流程
- [ ] 定義哪些操作必須人工執行

## 原始摘要

2026-09-30 發布於 aiposthub，含工具角色對照表、七步設定、八項完成檢查表與 AI 改錯時的處置原則。

## 相關連結

- [[claude-code-newbie-guide]] — Claude Code 新手上手指南，本篇補環境前置
- [[codex-desktop-2026-onboarding-guide]] — Codex 桌面版 onboarding
- [[claude-code-git-workflow]] — 環境建好後的 Git 工作流
