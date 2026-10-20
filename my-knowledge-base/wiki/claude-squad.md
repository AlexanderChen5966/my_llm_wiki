---
title: Claude Squad：多 AI Agent 終端管理工具
date: 2026-06-02
tags: [ai/agent, ai/claude]
source: https://github.com/smtg-ai/claude-squad
type: article
status: processed
asset_value: 50
---

Claude Squad 是一個終端應用程式，基於 tmux + git worktree 管理多個 AI Agent（Claude Code、Codex、Gemini CLI、Aider 等）在隔離工作區中同時執行任務。安裝為 `cs` 命令，支援背景自動執行（yolo mode）、變更審查、分支推送。

## T（核心論點）

當你需要同時處理多個開發任務時，每個 AI Agent 需要自己的獨立環境才不會互相衝突。Claude Squad 用 **tmux 隔離終端 + git worktree 隔離程式碼**，讓你在一個 TUI 介面中建立、切換、監控多個 Agent session，每個 session 在自己的分支上工作。

## B（適用場景）

**適用：**
- 同時跑多個獨立開發任務（bug fix + feature + refactor）
- 需要在不同 AI Agent 間切換比較（Claude vs Codex vs Gemini）
- 想讓 Agent 在背景自動完成後再審查結果

**不適用 / 注意：**
- 單一任務直接用 Claude Code 即可
- 需要 Agent 之間協作溝通的場景（各 session 互相隔離）
- 需要安裝 tmux 和 gh（GitHub CLI）

## R（操作步驟）

### 安裝

```bash
# Homebrew
brew install claude-squad
ln -s "$(brew --prefix)/bin/claude-squad" "$(brew --prefix)/bin/cs"

# 或手動安裝
curl -fsSL https://raw.githubusercontent.com/smtg-ai/claude-squad/main/install.sh | bash
```

### 基本使用

```bash
cs                    # 啟動（預設用 claude）
cs -p "codex"         # 指定用 Codex
cs -p "gemini"        # 指定用 Gemini CLI
cs -p "aider ..."     # 指定用 Aider
cs -y                 # 自動接受模式（yolo mode）
```

### 快捷鍵

| 按鍵 | 功能 |
|------|------|
| `n` / `N` | 新建 session（N 附帶 prompt）|
| `D` | 刪除 session |
| `↵` / `o` | 進入 session |
| `ctrl-q` | 離開 session |
| `s` | commit + push 分支 |
| `c` | checkout（commit 並暫停）|
| `r` | 恢復暫停的 session |
| `tab` | 切換預覽/diff 檢視 |

### Profile 設定

在 `~/.claude-squad/config.json` 設定多個 profile：

```json
{
  "default_program": "claude",
  "profiles": [
    { "name": "claude", "program": "claude" },
    { "name": "codex", "program": "codex" },
    { "name": "aider", "program": "aider --model ollama_chat/gemma3:1b" }
  ]
}
```

### 運作原理

1. **tmux**：每個 Agent 在獨立 tmux session 中執行
2. **git worktree**：每個 session 在獨立 worktree 分支上工作，互不干擾
3. **TUI 介面**：統一管理、導覽、審查所有 session

## C（關鍵行動）

- [ ] 與 [[multi-agent-cli-bridge]] 比較：CCB 更強調角色分離與混合 Agent 協作
- [ ] 與 [[parallel-code-multi-agent-gui]] 比較：Parallel Code 提供 GUI + 手機監控
- [ ] 評估是否適合目前的多任務並行開發需求

## 原始摘要

smtg-ai/claude-squad 是一個基於 tmux + git worktree 的終端管理工具，可同時管理多個 AI Agent（Claude Code、Codex、Gemini CLI、Aider）在隔離工作區中執行任務，支援背景自動執行、變更審查與分支推送。

## 相關連結

- [[multi-agent-cli-bridge]] — CCB：基於 tmux 的多 Agent CLI 工作區（更強調角色分離）
- [[parallel-code-multi-agent-gui]] — GUI 版本的多 agent 平行開發工具
- [[codex-plugin-for-claude-code]] — 跨供應商 agent 協作的另一種方式
- [[gstack-framework]] — gstack 的 `/pair-agent` 也支援跨 AI 代理協作
