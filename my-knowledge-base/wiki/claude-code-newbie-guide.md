---
title: Claude Code 新手入門：四種入口差異 + Portfolio 重設計實戰
date: 2026-06-15
tags: [ai/claude]
source: https://ithelp.ithome.com.tw/articles/10400090
type: article
status: processed
asset_value: 55
---

Claude Code 新手必知：它和 Claude.ai / Claude Desktop / API 的根本差異，以及如何正確扮演「Art Director」角色而非讓 AI 自己決定一切。

## T（核心論點）

**Claude.ai 跟你聊 code，Claude Code 幫你動 code。** 同樣的模型，工具集差很多：

| 工具 | 能力邊界 |
|------|---------|
| Claude.ai（網頁版）| 對話 + 上傳檔案，看不到整個專案 |
| Claude Desktop | 可掛 MCP server，本質還是對話 |
| **Claude Code** | 讀整個 repo、直接改檔案、跑 Bash |
| Anthropic API | 給自己的 App 呼叫，不是給人聊天用 |

Claude Code 解決了聊天版的四個痛點：
1. 讀得到整個 repo
2. 直接改檔案，不需要你複製貼上
3. 跑得了 Bash（`npm install`、`git diff`、跑測試）
4. 透過 `CLAUDE.md` 記得你的專案

## B（適用場景）

完全沒用過 Claude Code、想從頭了解工具差異的人，或想學習如何與 AI 協作設計的人。

## R（完整內容）

### 安裝

```bash
# Mac/Linux（官方推薦，一行安裝，不需要 Node.js）
curl -fsSL https://claude.ai/install.sh | bash

# Windows (PowerShell)
irm https://claude.ai/install.ps1 | iex

# Homebrew
brew install --cask claude-code

# 驗證安裝
claude doctor
```

**登入選擇：**
- Pro/Max/Team/Enterprise 訂閱 → 直接用
- API Key → 按 token 用量計費
- 免費 Claude.ai 帳號 → **不能用 Claude Code**

### Portfolio 重設計案例：三個關鍵洞察

**洞察 1：讓 AI 先分析，再動手**

第一個 prompt 不必完美，給方向 + 對話權：
```
「你去深入看一下我的個人網頁，並搜尋大老們做個人網頁的好想法，
我們可以討論。有問題也一起提出來。」
```
Claude Code 會自己讀 repo、跑 Web Search，然後回來對話。這是聊天版做不到的。

**洞察 2："purple problem"（AI 配色陷阱）**

Bolt、Lovable 等 vibe coding 工具十有八九輸出「深藍紫漸層 + 螢光紫 accent」，因為訓練資料大量基於 Tailwind 預設 palette + 2023-2024 年的 Vercel/Linear 風。

**避免方法**：不問「你建議什麼配色？」，而是先在 coolors.co / Realtime Colors 找到喜歡的方向，然後用自己的話描述：
```
「我想走深色底配暖色重點，像在讀紙本雜誌，
不要 v0/Bolt 那種紫漸層科技風。先出一版讓我看。」
```

**洞察 3：你是 Art Director，不是 Designer**

- 給方向、給比喻、給反例
- 讓 AI 先出一版
- 你用眼睛判斷，不是你自己挑色號

Portfolio v2 最終：一個 commit，27 個檔案改動（刪的比加的多），一個下午完成。

### 四個最重要的 Slash 指令

| 指令 | 用途 |
|------|------|
| `/help` | 看所有指令清單 |
| `/model` | 切換模型（Opus/Sonnet/Haiku）|
| `/clear` | 清對話重開（長對話鬼打牆時用）|
| `/usage` | 看用量和費用 |

### 模型選擇建議

- 複雜架構決策 → Opus
- 日常 coding → Sonnet
- 簡單修字、改 typo → Haiku

⚠️ API Key 記得到 console.anthropic.com 設 monthly spending limit，預設無上限。

## C（關鍵行動）

- [ ] 安裝 Claude Code，跑 `claude doctor` 確認環境
- [ ] 設定 Anthropic Console 的 spending limit
- [ ] 下次要 AI 幫你做設計時，先自己找 reference，再用「方向+比喻+反例」的方式描述

## 相關連結
- [[claude-code-best-practices-index]] — 索引型：8 章完整地圖與 🟢🟡🔴 難度分級，補本庫網狀單點深入的線性缺口
- [[claude-code-billing-usage]] — Claude Code 計費說明（API Key vs 訂閱）
- [[claude-cowork-setup-guide]] — Claude Cowork 設定（進階：CLAUDE.md + 自動化）
- [[claude-design-tool]] — Claude Design（UI 設計工作流）
- [[claude-code-commands-reference]] — 三層命令體系速查表
