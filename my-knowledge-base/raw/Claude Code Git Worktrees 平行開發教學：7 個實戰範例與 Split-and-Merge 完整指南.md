---
title: "Claude Code Git Worktrees 平行開發教學：7 個實戰範例與 Split-and-Merge 完整指南"
source: "https://moksaweb.com/claude-code-git-worktrees/"
author:
  - "[[Moksa]]"
published: 2026-04-23
created: 2026-08-19
description: "Claude Code Git Worktrees 平行開發完整教學，詳解 --worktree flag 自動化機制、origin/HEAD 分支規則、.worktreeinclude 設定、Sub-Agent isolation 模式，附 7 個實戰範例與 Split-and-Merge 生產力 3 倍提升的完整流程。"
tags:
  - "clippings"
status: "processed"
---
Claude Code Git Worktrees 是 Anthropic 為平行開發設計的內建功能，讓你可以同時跑多個 Claude Code Session 各自在獨立的 Git Worktree 中工作，互不干擾。這個看似簡單的功能其實解決了 AI 助理時代最大的效率瓶頸：以前一個 Claude 只能處理一個任務，現在透過 Claude Code Git Worktrees 可以同時讓三四個 Claude 平行處理不同 Feature Branch。Reddit 社群實測顯示，善用 Claude Code Git Worktrees 的開發者每天能完成的任務量是傳統單 Session 工作流的 3-5 倍。這不只是速度快而已，更是工作節奏的根本改變——等待變成了平行推進、單執行緒變成了多執行緒、被動等 AI 變成了主動協調多個 AI。這篇文章會完整拆解 Claude Code Git Worktrees 的機制、使用模式、進階優化和常見陷阱。

> **Moksa 觀點：** 我一開始用 Claude Code 時最大的痛點是「一次只能做一件事」。改完 Feature A 等跑測試的時候我就在發呆，不敢開新 Session 因為 Branch 切來切去會亂掉。學會 Claude Code Git Worktrees 之後整個工作流翻轉——Feature A 在 worktree-feature-a 跑測試、Bugfix B 在 worktree-bugfix-b 寫 Code、Documentation Update 在 worktree-docs 改文件，三個 Claude 同時工作我在中間輪流看進度。這不是噱頭，是真正的生產力革命。但 Claude Code Git Worktrees 也有不少坑，特別是 cleanup、merge conflict、node\_modules 重複這些實務問題，這篇都會講清楚。

## 1\. 什麼是 Claude Code Git Worktrees？

Claude Code Git Worktrees 是 Claude Code 內建的平行開發功能，底層使用 Git 原生的 Worktree 機制。Git Worktree 本身是 Git 2.5 版引入的功能，允許同一個 Repository 在不同目錄各自 Checkout 不同的 Branch。傳統上這功能多半用在 Backport 或並行維護不同版本，但在 AI 助理時代它變成平行開發的最重要基礎設施。Claude Code 在這個基礎上做了兩層封裝：自動化 Worktree 建立、統一放在 `.claude/worktrees/` 目錄、搭配 Cleanup 機制，讓你一行指令就能啟動隔離的平行 Session。

使用 Claude Code Git Worktrees 最簡單的方式是在啟動時加上 `--worktree` （或簡寫 `-w` ）flag。這個 flag 是整個功能的入口，所有自動化機制都從這裡觸發。這個 flag 接受一個名稱作為 Worktree 目錄名和 Branch 名。例如執行 `claude --worktree feature-auth` 會建立 `.claude/worktrees/feature-auth/` 目錄和 `worktree-feature-auth` Branch，然後在這個隔離環境中啟動 Claude Code Session。你也可以省略名稱讓系統自動產生隨機名稱，例如 bright-running-fox 這種人類可讀的組合。

Claude Code Git Worktrees 預設從 `origin/HEAD` 這個 Git Reference 分支出來，也就是 Remote Repository 的預設分支（通常是 main）。這個 Reference 在你第一次 Clone Repository 時由 Git 自動設定，之後即使 Remote 的預設分支改了你本機也不會自動更新。如果你的 Repository 預設分支從 master 換成 main，要手動執行 `git remote set-head origin -a` 讓本機 Reference 同步，否則新的 Worktree 會從舊的預設分支分出來導致意外結果。

## 2\. Claude Code Git Worktrees 怎麼運作？

Claude Code Git Worktrees 的運作核心是把 Git 的 Worktree 概念和 Claude Code Session 生命週期結合。Anthropic 工程團隊花了不少心思在這個整合上，目標是讓開發者感覺不到複雜度——你只需要記得加 `--worktree` ，其他所有細節都自動處理。當你執行 `claude --worktree feature-a` 時，Claude Code 會依序完成以下動作：

- 檢查 `.claude/worktrees/feature-a/` 目錄是否已存在，不存在就用 `git worktree add` 建立
- 從 `origin/HEAD` 建立新分支 `worktree-feature-a` （命名規則是固定的 worktree 前綴加上名稱）
- 自動複製 `.worktreeinclude` 檔案中列出且被 `.gitignore` 忽略的檔案到新 Worktree（例如 `.env` 、`.env.local` 這類設定檔）
- 切換到新 Worktree 目錄並啟動 Claude Code Session
- Session 結束時根據是否有修改決定是否保留 Worktree

這個機制讓你可以在主 Repository 保持乾淨的情況下，同時推進多個 Feature，不需要反覆切換 Branch 造成思緒打斷，也不用擔心某個 Feature 的 uncommitted changes 影響其他工作。對需要頻繁 Context Switch 的開發者來說，這個效率提升非常顯著。每個 Worktree 有自己的工作目錄、自己的 Branch 狀態、自己的 Staged Changes，互相完全隔離。更重要的是它們共用同一個 `.git` 目錄，所以 Commit History、Remotes、Tags 都是同步的，不會浪費磁碟空間在 `.git` 上。

Claude Code Git Worktrees 的 Cleanup 邏輯也很聰明，考慮了開發者實際會遇到的各種情境。當你結束 Session 時系統會判斷：如果 Worktree 中沒有任何 uncommitted changes、untracked files 和 unpushed commits，就直接自動刪除整個 Worktree 和 Branch，不留痕跡。如果有任何變更，會提示你選擇保留或刪除——保留的話你可以日後回來繼續，刪除則會丟棄所有未儲存的修改。

另外 Claude Code 還會在每次 Session 啟動時自動清理「孤兒 Worktree」——那些因為當機或中斷造成遺留的 Subagent Worktrees。這個自動清理只會移除符合三個條件的 Worktree：沒有 uncommitted changes、沒有 untracked files、沒有 unpushed commits。你用 `--worktree` 手動建立的 Worktree 永遠不會被自動清理，避免意外丟失你的工作。清理週期由 `cleanupPeriodDays` 設定控制，預設是 30 天。

## 3\. Claude Code Git Worktrees 適合哪些使用場景？

以下是 Reddit 社群 r/ClaudeCode 和 Anthropic 官方範例中最常見的 6 個 Claude Code Git Worktrees 使用場景，每個場景都有明確的生產力提升空間：

- **平行多 Feature 開發** ：同時推進 2-3 個獨立 Feature，每個 Feature 在自己的 Worktree 中，主 Branch 保持乾淨。適合個人開發者在 Review 等待期間推進多條線。
- **Bug Fix 和 Feature 並行** ：主 Session 寫 Feature，同時開一個 Worktree 修緊急 Bug。緊急 Bug 修完可以立刻 Merge 或部署，不用中斷 Feature 開發的思路。
- **Multi-Agent 平行執行** ：搭配 Sub-Agent 的 `isolation: worktree` 設定，讓多個 Sub-Agent 各自在獨立 Worktree 中工作。Main Agent 指派任務後子 Agent 各自完成，最後 Main Agent 統一 Review 和 Merge。
- **實驗性重構** ：要做大規模重構但怕失敗時，開一個 Worktree 實驗。如果實驗成功就 Merge，失敗就直接刪除 Worktree，主分支完全不受影響。
- **比較不同實作方案** ：同一個功能用不同方法實作比較優劣。開三個 Worktree 分別做 Redux、Zustand、Jotai 的狀態管理版本，最後比較效能和程式碼品質再決定。
- **Code Review 和開發分離** ：當你在 Review 同事 PR 時開 Worktree Checkout 該分支，不用切換主 Branch 打斷你當前的開發進度。Review 完直接刪 Worktree，原本的工作繼續。

## 如何開始使用 Claude Code Git Worktrees？

### 確認 Remote HEAD 設定正確

執行 git remote set-head origin -a 讓本機的 origin/HEAD 同步 Remote 的預設分支，確保新 Worktree 從正確的 Base Branch 分出來。

### 建立.worktreeinclude 檔案

在 Project Root 建立.worktreeinclude，列出要自動複製到新 Worktree 的 gitignored 檔案例如.env 和.env.local。

### 啟動 Worktree Session

執行 claude –worktree feature-a 建立名為 feature-a 的 Worktree 並在其中啟動 Claude Code。

### 完成後正確清理

Session 結束時系統會自動清理無變更的 Worktree，有變更時選擇保留或刪除。定期執行 git worktree list 檢查是否有遺留的 Worktree。

Non-Git Version Control 系統（例如 SVN、Perforce、Mercurial）也能用 Claude Code Git Worktrees 的架構，但需要透過 WorktreeCreate 和 WorktreeRemove Hook 自訂建立和清理邏輯。當你設定這些 Hook 後， `--worktree` flag 的預設 Git 行為就被取代成你的自訂腳本。這讓 Claude Code Git Worktrees 的平行開發模式不只限於 Git 使用者，企業環境中仍在用 Perforce 或 SVN 的團隊也能享受到相同的生產力提升。注意當使用自訂 Hook 時 `.worktreeinclude` 檔案不會被自動處理，必須在 Hook 腳本內自行複製需要的設定檔。

## 4\. Claude Code Git Worktrees 實戰範例

**範例 1：基本的 Worktree 指令操作**

```
# Claude Code Git Worktrees basic commands
# These are the most common commands you will use daily

# Start Claude Code in a new worktree with a named branch
# Creates .claude/worktrees/feature-auth and worktree-feature-auth branch
claude --worktree feature-auth

# Short flag version (equivalent to --worktree)
claude -w feature-auth

# Auto-generated random name like bright-running-fox
claude --worktree

# Start Claude in an existing worktree (no new creation)
cd .claude/worktrees/feature-auth
claude

# List all current worktrees in the repository
git worktree list

# Output looks like this
# /path/to/repo                  abcd123 [main]
# /path/to/repo/.claude/worktrees/feature-auth  efgh456 [worktree-feature-auth]
# /path/to/repo/.claude/worktrees/bugfix-123    ijkl789 [worktree-bugfix-123]

# Remove a specific worktree manually
git worktree remove .claude/worktrees/feature-auth

# Clean up orphaned worktrees that git thinks still exist
git worktree prune

# Check how much disk space worktrees use
du -sh .claude/worktrees/*

# Quick tip: alias for common worktree operations
# Add these to your .bashrc or .zshrc for faster workflow
alias cw='claude --worktree'
alias wtlist='git worktree list'
alias wtclean='git worktree prune && git worktree list'

# Example usage after setting aliases
# cw feature-auth        # Start Claude in feature-auth worktree
# wtlist                 # See all active worktrees
# wtclean                # Clean up orphaned references
```

**範例 2：.worktreeinclude 複製 gitignored 檔案**

```
# Project root .worktreeinclude file
# Uses gitignore syntax to list files that should be copied
# when creating a new worktree

# Only files that match a pattern AND are gitignored will be copied
# Tracked files are never duplicated (they come from git anyway)

# Environment variables for local development
.env
.env.local
.env.development

# Local configuration overrides
config/local.json
config/secrets.local.json

# Development database and cache files
data/local.db
.cache/

# IDE and editor configurations
.vscode/settings.json
.idea/workspace.xml

# Local SSL certificates for HTTPS dev
certs/local.pem
certs/local.key

# Test fixtures with local paths
tests/fixtures/local-config.json

# Why use .worktreeinclude instead of committing these files
# Files in .gitignore are usually sensitive or environment-specific
# Committing them causes security issues or breaks other developers
# But without them each new worktree needs manual setup
# .worktreeinclude gives you the convenience without the risk
```

**範例 3：Sub-Agent 用 Worktree Isolation 平行工作**

```
# Sub-Agent definition with worktree isolation
# Save as .claude/agents/feature-implementer.md
# The isolation field creates a fresh worktree for each invocation

# YAML frontmatter section
name: feature-implementer
description: Implements features in isolated git worktrees for parallel development
model: claude-sonnet-4-6
isolation: worktree
tools:
  - Read
  - Edit
  - Write
  - Bash

# System prompt below the frontmatter
# You implement features in an isolated git worktree

# Verify you are in the assigned worktree by running pwd first
# The path should include .claude/worktrees/

# Your workflow for each feature
# Step 1 Read the feature specification carefully
# Step 2 Implement the feature with test coverage
# Step 3 Run all relevant tests until they pass
# Step 4 Commit your work with descriptive messages
# Step 5 Return a summary to the main agent

# Do not push to remote
# The main agent handles merge coordination across worktrees
# If you encounter conflicts report them to the main agent
# Do not try to resolve conflicts with other worktrees

# Usage example in a main session
# Ask Claude to implement three features in parallel
# It will spawn three feature-implementer sub-agents
# Each gets its own worktree like
#   .claude/worktrees/feature-implementer-abc123
#   .claude/worktrees/feature-implementer-def456
#   .claude/worktrees/feature-implementer-ghi789
# No conflicts between them since they are isolated
```

**範例 4：Worktree 磁碟優化的 settings.json 配置**

```
# settings.json configuration for Claude Code Git Worktrees
# Focus on disk usage optimization in large monorepos

# Symlink heavy directories to avoid duplication
# Without these each worktree gets full copy of node_modules
# With symlinks worktrees share the same files on disk

worktree:
  symlinkDirectories:
    - node_modules
    - .next
    - .turbo
    - .cache
    - .yarn
    - dist
    - build
    - coverage
    - .pytest_cache
    - .venv
    - target

  # Sparse checkout for monorepos with 100+ packages
  # Only the listed paths are materialized on disk
  # Much faster to create and smaller footprint
  sparsePaths:
    - packages/my-app
    - packages/shared-utils
    - packages/api-types
    - configs
    - scripts

# Cleanup behavior control
cleanupPeriodDays: 14

# Disk usage comparison without symlinkDirectories
# Main repo node_modules   2.5 GB
# Worktree 1 node_modules   2.5 GB duplicated
# Worktree 2 node_modules   2.5 GB duplicated
# Worktree 3 node_modules   2.5 GB duplicated
# Total disk usage          10 GB wasting 7.5 GB

# Disk usage WITH symlinkDirectories set
# Main repo node_modules   2.5 GB actual files
# Worktree 1 symlink        0 bytes (just a link)
# Worktree 2 symlink        0 bytes
# Worktree 3 symlink        0 bytes
# Total disk usage          2.5 GB saving 75 percent

# Important caveat for symlinkDirectories
# Some tools do not handle symlinks well examples
#   Windows filesystems require admin permission
#   Some linters may follow symlinks and report wrong paths
#   Docker volumes may behave unexpectedly
# Test your specific tooling before committing to this config
```

**範例 5：手動建立進階 Worktree**

```
# Manual worktree management for advanced scenarios
# Use when you need more control than --worktree flag provides

# Create a worktree outside the repository
# Useful when you want complete separation from main repo
git worktree add ../project-feature-a -b feature-a

# Create worktree checking out an existing remote branch
# Useful for reviewing someone else PR locally
git worktree add ../project-review-pr-123 origin/feature-branch-from-colleague

# Create a detached worktree for investigating history
git worktree add --detach ../project-investigate abc123def456

# Create a worktree from a specific tag for reproducing a bug
git worktree add ../project-v2.1 v2.1.0

# Start Claude Code in any of these manually created worktrees
cd ../project-feature-a
claude

# Or pass the path directly
claude --add-dir ../project-feature-a

# Common worktree lifecycle commands
git worktree list                  # Show all active worktrees
git worktree remove PATH           # Remove a specific worktree
git worktree prune                 # Remove references to deleted worktrees
git worktree lock PATH             # Prevent accidental removal
git worktree unlock PATH           # Allow removal again
git worktree move OLD_PATH NEW_PATH  # Move a worktree to new location

# Check worktree disk usage
for wt in $(git worktree list --porcelain | grep ^worktree | cut -d' ' -f2); do
  echo "Path $wt"
  du -sh $wt
done
```

**範例 6：WorktreeCreate Hook 自訂邏輯**

```
# Custom WorktreeCreate hook for special scenarios
# Goes in .claude/settings.json hooks section
# Replaces default git worktree add logic entirely

# Use cases that justify custom hooks
# 1 Non-git version control like SVN Mercurial Perforce
# 2 Branching from specific base other than origin/HEAD
# 3 Running setup scripts automatically after worktree creation
# 4 Integrating with internal tooling or build systems

# Example hook configuration
# hooks.WorktreeCreate hook runs when --worktree flag is used
# Receives worktree name and base branch as arguments
# Must create the worktree directory and return success

# Hook command example for custom base branch
# input=$(cat)
# name=$(echo $input | jq -r .worktree_name)
# base=$(echo $input | jq -r .base_branch // "main")
# git worktree add .claude/worktrees/$name -b worktree-$name origin/$base
# cd .claude/worktrees/$name
# npm install  # Auto install dependencies
# cp ../../.env .env  # Copy env file manually
# exit 0

# For non-git systems like Perforce P4
# input=$(cat)
# name=$(echo $input | jq -r .worktree_name)
# p4 client -t my-template -o | p4 client -i
# p4 sync //depot/project/...@main
# cp -r .p4config .claude/worktrees/$name/
# exit 0

# When using custom hook the .worktreeinclude file is NOT processed
# You must handle file copying inside your hook script
# This gives you full control but also full responsibility

# Testing your custom hook before relying on it
# 1 Run claude --worktree test-name and verify the worktree is created
# 2 Check git worktree list shows the new worktree
# 3 Check all expected files are copied
# 4 Check Claude Code starts successfully in the worktree
# 5 Try cleaning up by exiting and verify hook removes things correctly
```

**範例 7：實務 Split-and-Merge 流程**

```
# Split-and-Merge pattern for parallel feature development
# This is the most productive way to use Claude Code Git Worktrees

# Scenario you have three independent tasks to complete today
# Task A Update user registration flow
# Task B Refactor payment processing module
# Task C Add new product recommendation feature

# Step 1 Start three parallel Claude Code sessions
# Terminal 1
claude --worktree task-a-user-registration
# Inside Claude describe the task and let it work

# Terminal 2
claude --worktree task-b-payment-refactor
# Work on the refactor independently

# Terminal 3
claude --worktree task-c-recommendations
# Claude adds the new feature in this isolated worktree

# Step 2 Monitor progress in each terminal
# Each session works independently no blocking
# You can switch between them and review progress
# Ask questions guidance in whichever session needs help

# Step 3 When a task completes verify the work
cd .claude/worktrees/task-a-user-registration
npm test
npm run lint
git log --oneline -5

# Step 4 Merge completed work back to main
# Go back to main repository
cd /path/to/main/repo
git checkout main
git pull --rebase

# Merge each completed worktree
git merge worktree-task-a-user-registration
git merge worktree-task-b-payment-refactor
git merge worktree-task-c-recommendations

# Step 5 Clean up merged worktrees
# Exit each Claude Code session
# Claude prompts to remove worktree choose Remove
# Or manually clean up
git worktree remove .claude/worktrees/task-a-user-registration
git worktree remove .claude/worktrees/task-b-payment-refactor
git worktree remove .claude/worktrees/task-c-recommendations

# Step 6 Delete the feature branches after merge
git branch -d worktree-task-a-user-registration
git branch -d worktree-task-b-payment-refactor
git branch -d worktree-task-c-recommendations

# Productivity multiplier
# Without parallel worktrees 3 tasks x 2 hours each = 6 hours total
# With parallel worktrees 3 tasks x 2 hours (parallel) = 2 hours total
# 3x speedup because work happens concurrently

# Advanced tips for split-and-merge productivity
# 1 Keep tasks truly independent
#   Tasks that touch overlapping files cause merge conflicts
#   Design task boundaries carefully before starting
#   A good heuristic is one task per package or one task per feature area
#
# 2 Use naming conventions
#   Worktree names should describe the task purpose
#   Good examples task-oauth-refresh bugfix-payment-race feature-search-v2
#   Bad examples test1 tmp foo new-stuff
#
# 3 Establish a monitoring rhythm
#   Check each terminal every 15 minutes
#   Claude may need clarification or hit errors
#   Long-running tasks benefit from periodic check-ins
#
# 4 Coordinate commits strategically
#   Let each worktree commit its own atomic changes
#   Do not try to cross-commit between worktrees
#   Merge back to main in a specific order by risk level
#
# 5 Test before merging
#   Run full test suite in each worktree first
#   Never merge broken code back to main
#   Integration tests in main repo after merge is even safer
#
# Common pitfall avoidance
# Pitfall A Three sessions editing the same config file
# Solution Designate one worktree as owner of each shared file
#
# Pitfall B Running out of Rate Limit across three Claudes
# Solution Stagger task starts to spread API calls over time
#
# Pitfall C Forgetting which terminal does what
# Solution Use tmux with named panes or iTerm2 tab colors
```

## 5\. Claude Code Git Worktrees 的常見陷阱有哪些？

使用 Claude Code Git Worktrees 最容易踩的 6 個雷，這些是我和 Reddit 社群在實際生產環境中反覆驗證的經驗，建議在正式導入 Worktree 工作流之前先理解這些陷阱：

- **origin/HEAD 沒同步導致從舊分支分出** ：本機的 `origin/HEAD` 是 Clone 當下的預設分支，之後 Remote 改成新分支不會自動更新。新 Worktree 會從舊分支分出來，Merge 時產生混亂。解決方式是定期執行 `git remote set-head origin -a` 。
- **gitignored 檔案遺漏** ：新 Worktree 是 Fresh Checkout，不會自動帶入 `.env` 、`.env.local` 等 gitignored 檔案。結果 Claude 一執行 `npm run dev` 就失敗因為缺環境變數。用 `.worktreeinclude` 定義要複製的檔案可以解決。
- **node\_modules 重複佔硬碟** ：每個 Worktree 都 `npm install` 一份 node\_modules 會很快把磁碟灌爆。用 `worktree.symlinkDirectories` 設定 Symlink 節省空間，但要注意某些工具對 Symlink 支援不好。
- **Merge Conflict 沒處理好** ：三個 Worktree 同時改同一個檔案，Merge 時必然衝突。要事先設計好任務切分（按 Package 切、按模組切），避免多個 Worktree 動同一份程式碼。或讓主 Agent 序列化 Merge。
- **忘記清理產生孤兒 Worktree** ：Session 當機或手動 kill 可能留下孤兒 Worktree。 `cleanupPeriodDays` 會自動清理但只會清無變更的。定期執行 `git worktree list` 和 `git worktree prune` 確保乾淨。
- **WSL 環境相容性問題** ：Windows 上的 WSL 跑 Claude Code Git Worktrees 時，Symlink 和檔案權限可能有問題。建議 Windows 用戶用 Native Windows 環境，或確保 WSL 專案放在 WSL 的原生檔案系統（例如 `/home/` ）而不是 `/mnt/c/` 。

## 6\. 哪裡可以找到更多 Claude Code Git Worktrees 資源？

以下 3 支官方影片展示 Claude Code Git Worktrees 的平行開發模式，搭配本篇文章的實戰範例一起看最能理解完整工作流程和設計思路：

![Youtube video](https://i.ytimg.com/vi/gv0WHhKelSE/hqdefault.jpg)

Claude Code Best Practices — 平行開發最佳實務

![Youtube video](https://i.ytimg.com/vi/uhJJgc-0iTQ/hqdefault.jpg)

Building More Effective AI Agents — Agent 平行協作

![Youtube video](https://i.ytimg.com/vi/fHWFF_pnqDk/hqdefault.jpg)

Vibe Coding in Prod — Worktree 在生產環境的應用

- [Moksa Claude Code 完整教學系列](https://moksaweb.com/category/claude-code/)
- [Claude Code Common Workflows 官方文件](https://code.claude.com/docs/en/common-workflows)
- [Git Worktree 官方文件](https://git-scm.com/docs/git-worktree)

## 7\. Claude Code Git Worktrees 常見問題 FAQ

### Claude Code Git Worktrees 跟一般 git worktree 有什麼差別？

底層都是 Git 的 Worktree 功能，但 Claude Code Git Worktrees 多了自動化層。用 –worktree flag 時自動建立目錄在.claude/worktrees/ 下、自動從 origin/HEAD 分支、自動複製.worktreeinclude 列出的 gitignored 檔案、Session 結束時自動判斷是否清理。一般 git worktree add 需要你手動處理這些細節。

### 可以同時跑幾個 Claude Code Git Worktrees Session？

理論上沒有硬性限制，取決於你的機器資源。實務上建議同時 2-4 個 Session 是最佳平衡點。每個 Session 都消耗 CPU、RAM 和 API Rate Limit。超過 4 個你很難有效監督每個 Session 的進度，反而容易出錯。另外 Anthropic API 的 Rate Limit 也會成為瓶頸。

### Worktree 的 Branch 可以 Push 到 Remote 嗎？

可以。Worktree 的 Branch 和一般 Branch 完全一樣，差別只在 Working Directory 位置。你可以 git push -u origin worktree-feature-a 把 Worktree 的 Branch 推到 Remote，用同事 Pull 下來 Review。不過建議 Rename 成更有意義的名稱再 Push，例如 feature/user-auth-v2 比 worktree-feature-a 更清楚。

### Subagent isolation worktree 模式真的能完全隔離嗎？

檔案系統層級是完全隔離的，每個 Subagent 在自己的 Worktree 工作互不影響。但要注意幾個共享資源 1 Git 的 Commit History 是共享的雖然 Branch 獨立 2 Git Remote 設定共享所以 Push 時互相可見 3 node\_modules 如果設定了 symlinkDirectories 會共享但這是優化不是隔離問題。一般使用情境下隔離已經足夠。

### Worktree 如何 Merge 回主分支最安全？

建議流程是 1 Worktree 完成所有任務並通過測試 2 回到主 Repository 切到 main 分支並 git pull –rebase 確保最新 3 Merge 該 Worktree Branch 用 git merge –no-ff worktree-feature-a 保留 Merge 歷史 4 如果有 Conflict 在主 Repo 解決不要回 Worktree 5 Merge 成功後刪除 Worktree 和 Branch。一次只 Merge 一個 Worktree 避免混淆。

### WSL Windows 環境有什麼特別注意事項？

WSL 有幾個已知問題 1 Symlink 在 WSL 跨 Windows 檔案系統 /mnt/c/ 時不穩定 2 檔案權限 mapping 可能混亂 3 某些 Git Hook 可能執行有問題。建議把專案放在 WSL 原生檔案系統 /home/ 而不是 /mnt/c/。如果必須在 Windows 檔案系統工作考慮關閉 symlinkDirectories 設定改用 hard copy 雖然吃磁碟但更穩定。

### .worktreeinclude 和.gitignore 的關係是什麼？

.worktreeinclude 只對被.gitignore 忽略的檔案生效。意思是.worktreeinclude 列了檔案 A 而.gitignore 沒忽略 A 時複製不會發生因為 A 本來就會自動隨 checkout 出現。只有當 A 被.gitignore 忽略但又需要在 Worktree 中存在時.worktreeinclude 才發揮作用。這個設計避免了重複複製 tracked files。

### Worktree cleanup 會不會誤刪我的工作？

不會。Claude Code Git Worktrees 提供的自動清理機制設計上有三個嚴格的保護條件都必須滿足才會刪 1 沒有 uncommitted changes 2 沒有 untracked files 3 沒有 unpushed commits。任何一個條件不滿足自動清理就跳過該 Worktree。而且 –worktree 手動建立的 Worktree 永遠不會被自動清理只會被 Session 結束時提示清理。你的未完成工作是安全的。