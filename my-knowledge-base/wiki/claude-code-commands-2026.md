---
title: Claude Code 2026 新命令完整解析
date: 2026-05-14
tags: [ai/claude]
source: https://hackmd.io/@BASHCAT/SJ7kShMJGx
type: article
status: processed
asset_value: 80
---

Claude Code 在 2026 年 3-5 月推出 30 個版本、新增 10+ 個 slash commands，整體架構從「Claude 的 CLI」演進為「會自己呼叫雲端、會自己跑 PR、會自己 review 的工作流引擎」。核心主軸：雲端化、長任務自走、Skills 統一架構。

## T（核心論點）
2026 年 Claude Code 的命令演進有一條清晰主軸：本地保留即時互動，重活丟雲端跑（`/ultraplan`、`/ultrareview`、`/autofix-pr`），長任務交給 agent 自走（`/goal`），常用流程封裝成 Skill。CLI 工具正在變成 Anthropic 的 agent 平台前端。

## B（適用場景）
**適用：** 已熟悉 Claude Code 基本操作、想提升長任務自動化程度的用戶
**不適用：** 剛入門用戶（先掌握基本 `/help`、`/clear`、`/compact` 即可）

## R（12 個關鍵新命令）

### 雲端工作流三劍客
| 命令 | 作用 | 注意事項 |
|------|------|---------|
| `/ultraplan` | 在瀏覽器審視草稿再決定是否執行，避免吃掉本地 context | 適合大型重構規劃 |
| `/ultrareview` | 多 agent 並行 review（6 維度），找到 bug 再跑驗證才報告 | Pro/Max 3 次免費，之後 $5~20/次 |
| `/autofix-pr` | 雲端監視 PR，CI 失敗或 reviewer 留言自動修復推送 | 只在純測試型 CI 使用，避免觸發 deploy |

### 長任務 Agent
```bash
/goal 所有單元測試通過且 coverage > 80%
/goal PR #1234 通過所有 CI 檢查並獲得 LGTM
```
`/goal` 設定完成條件後跨多輪持續工作。**必須搭配 auto mode**，否則遇到敏感 tool call 就斷掉。

### Bundled Skills（prompt-based，可自動觸發）
| 命令 | 用途 |
|------|------|
| `/batch <instruction>` | 自動拆解成 5-30 個並行 unit，每個在 git worktree 跑，做完開 PR |
| `/simplify [focus]` | 並行派出 3 個 review agent，合併結果直接修 code |
| `/loop [interval] [prompt]` | Cron job 的 AI 版，監測 CI 或定期任務 |
| `/fewer-permission-prompts` | 自動掃 transcript 找常用 read-only 命令加進 allowlist |
| `/usage` | 取代 `/stats` + `/cost`，查費用消耗 |
| `/team-onboarding` | 新同事入職 + 工作流複盤 |

## C（關鍵行動）
- [ ] **必學**：`/goal`（長任務）、`/ultrareview`（merge 前防線）、`/batch`（批次並行）
- [ ] **命令 vs Skills 統一**：新功能全部寫成 SKILL.md（有 `context: fork`、`allowed-tools` 等進階控制），舊 `.claude/commands/*.md` 仍可用但建議遷移
- [ ] **Plugin Marketplace 選 plugin 四原則**：先看官方目錄 → GitHub star > 200 → SKILL.md 內容清晰 → 不要 require remote MCP server

## 退場命令
- `/vim` (v2.1.92 移除) → 改用 `/config` → Editor mode
- `/pr-comments` (v2.1.91 移除) → 直接對話「幫我看 PR #1234 的 comments」
- `Ctrl+O`：從「focus 切換」改為「詳細日誌切換」
- `Ctrl+U`：從「刪除至行首」改為「清空整個輸入緩衝區」

## 相關連結
- [[claude-code-commands-reference]] — 完整命令速查表（2.1.74 版起）
- [[claude-skills-ecosystem]] — GitHub Skills 生態全景圖
- [[claude-skills-guide]] — Skills 設計原理
- [[claude-pilot-suite]] — 三模式執行手冊（Haiku/Sonnet/Opus）

## 補充資料（來源：Claude Code 2026 新命令大盤點 — 除了 claude-api 你還該認識這 12 個）

### 新增命令版本對照表

| 版本 | 月份 | 新命令 |
|------|------|--------|
| 2.1.101 | 4 月初 | `/team-onboarding` |
| 2.1.108 | 4 月中 | `/recap`、`/undo`（alias）|
| 2.1.110 | 4 月中 | `/tui`、`/focus` |
| 2.1.111 | 4 月底 | `/ultrareview`、`/fewer-permission-prompts`、`/effort` 滑塊 |
| 2.1.118 | 4 月底 | `/usage`（合併 `/stats` + `/cost`）|
| 2.1.139 | 5 月中 | `/goal`、`/scroll-speed`、Agent 視圖 |
| 4 月（版本不詳）| - | `/autofix-pr`、`/ultraplan`、`/powerup` |

### /ultrareview 計費細節

Pro/Max 用戶有 **3 次免費**（一次性，不會每月重置），用完後每次 review 大約 **$5~$20 美金**（依 diff 大小）。六個維度並行審查：correctness、security、architecture、tests、performance、style。第 3 步驗證是關鍵——每個發現的 bug 再跑一次驗證，確認是真 bug 才寫進報告。

### /goal 實戰注意事項

設定完成條件後，Claude 跨多輪持續工作直到達成。**關鍵前提：必須搭配 auto mode**，否則 Claude 遇到敏感 tool call 彈窗確認就斷掉，goal 的 loop 中止。每輪結束後，內建 evaluator agent 檢查條件，在 status view 和 transcript 上顯示「為什麼還沒達成」。

### /batch 深度說明

官方描述：自動拆解成多個獨立工作單元（實測通常 5-30 個，依任務規模），每個在獨立 git worktree 中用 background subagent 跑，完成後開 PR。是目前並行處理最強的內建命令。

### .claude/commands/ vs Skills 統一架構

Anthropic 沒有強制遷移，但升格 Skills 為主要抽象，slash command 變成 Skill 的其中一個能力。新 Skills 的關鍵 frontmatter：

```yaml
---
name: my-deploy-check
description: When to use this skill
allowed-tools: Read, Grep, Bash   # 精細權限控制
context: fork                      # 整個 skill 在 isolated subagent 跑
---
```

`context: fork` 讓整個 Skill 在隔離 subagent 執行，不汙染主對話 context，是目前最重要的進階設定。

### Plugin Marketplace 四大市集

| 平台 | 規模 | 特色 |
|------|------|------|
| Anthropic 官方 | 嚴選 | 安全第一，數量少 |
| tonsofskills.com | 425 plugins / 2,810 skills | 量大，品質參差 |
| SkillsMP | 1.2M+ skills | 按職業分類瀏覽 |

選 Plugin 原則（依優先順序）：
1. 先看官方目錄，能解決就解決
2. GitHub star > 200
3. SKILL.md 內容清晰、frontmatter 正確
4. **絕對避免** require remote MCP server 的 Plugin

### 精選學習優先清單

| 優先度 | 命令 | 理由 |
|--------|------|------|
| 必學 | `/goal` | 長任務 game changer，要搭配 auto mode |
| 必學 | `/ultrareview` | merge 前最後防線，有免費額度先試 |
| 必學 | `/batch` | 一次處理 N 個檔案，並行 + 自動 PR |
| 強推 | `/simplify` | 髒 code 後的清理利器，3 個 agent 並行審查 |
| 強推 | `/team-onboarding` | 從 30 天歷史產生入職指南 |
| 看情況 | `/ultraplan` | 大型重構規劃適用 |
| 看情況 | `/autofix-pr` | 只在純測試型 CI 才安全 |
