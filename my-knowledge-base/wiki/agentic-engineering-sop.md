---
title: Agentic Engineering 六階段實戰 SOP
date: 2026-05-21
tags: [ai/agent, ai/claude]
source: https://israynotarray.com/ai/20260517/3851629074/
type: article
status: processed
asset_value: 64
---

Karpathy 2026 年 4 月在 Sequoia Ascent 演講將 Vibe Coding 升級為 Agentic Engineering：「Vibe coding raises the floor. Agentic engineering is about extrapolating the ceiling.」Ray 的六階段實戰 SOP 把這個理念轉化為可操作的工作流，涵蓋 Spec/Plan/Tasks 三層落檔、三種 AI 員工配置、開發紀律、三層安全網與兩層 Memory 系統。

## T（核心論點）

Agentic engineering 的本質是：**你不再親手寫 code，而是監督一群 AI 員工的工程師**。你的工作是設計 spec、監督 plan、檢查 diff、寫測試、做評估、管權限。CodeRabbit 2025-12 報告（分析 470 個 PR）：AI 寫的 code 比人多 1.7x major bugs、XSS 漏洞 2.74x——紀律是接住 AI 失誤的網。

## B（適用場景）

**適用：** 生產級服務；需要長期維護的專案；重度使用 Claude Code / Cursor / Codex 的工程師
**不適用：** 週末 demo、PoC、一次性腳本（用 Vibe Coding 就夠，整套 SOP 過重）；Token 成本預算有限的場景

## R（六階段 SOP）

### 三角色七步驟協作圖

三個角色：使用者 / Leader（主 Claude）/ 員工（Sub-agent + Skill）

| Step | 角色 | 動作 |
|------|------|------|
| ① | 使用者 | 提需求 |
| ② | Leader | Plan Mode 拆解需求，必要時呼叫 Sub-agent 探索，產出 tasks.md |
| ⚠ | Leader | 需求模糊時主動 AskUserQuestion 反問（Anthropic 官方 interview pattern）|
| ③ | 員工 | 開發（UI 切給 ui-engineer），同輪補單元測試 |
| ④ | 員工 | 另開 session 或 sub-agent 做 Code Review（fresh context 避免 self-confirmation bias）|
| ⑤ | 員工 | typecheck + test + build 三段驗證 |
| ⑥⑦ | 使用者 | 回報 + 更新 ROADMAP → 你 review diff + commit |

**核心原則：使用者只在 ① ⑥ ⑦ 三個步驟親自參與。**

### 第一階段：規劃 — Spec/Plan/Tasks 三層落檔

| 層級 | 檔案 | 更新頻率 |
|------|------|---------|
| Spec（規格）| `CLAUDE.md`（@import `AGENTS.md`）| 幾乎不變 |
| Plan（計畫）| `docs/ROADMAP.md` | 每個 sprint 更新 |
| Tasks（任務）| `docs/tasks.md` | 每次工作更新 |

**CLAUDE.md @import 機制**：第一行 `@AGENTS.md`，再用 `@docs/ROADMAP.md` 引入計畫。Claude 啟動自動載入，遞迴最多 5 hops 深。此為 Anthropic 對 CLAUDE.md 的擴充語法，**AGENTS.md 規格本身不一定支援**。

**AGENTS.md**：2025-08 OpenAI 主導的開放標準，捐給 Linux Foundation AAIF，2026-05 已有 6 萬+ 開源專案使用，支援 Cursor、Codex、Devin、Gemini CLI 等 20+ 工具。Claude Code 只讀 CLAUDE.md，AGENTS.md 用於跨工具團隊共用。

**兩檔共用做法**：CLAUDE.md 第一行 `@AGENTS.md`（或 `ln -s AGENTS.md CLAUDE.md`），AGENTS.md 放規範本體，CLAUDE.md 再加 Claude Code 專屬規則。

ROADMAP.md 的 `## 明確排除的事` 區塊很關鍵——AI 容易自己腦補功能越界，明文列出不做的事可卡住範圍。

### 第二階段：員工分工 — 三種配置

| 類型 | 位置 | 特性 | 適用場景 |
|------|------|------|---------|
| **Skill** | `.claude/skills/<name>/SKILL.md` | 共用主 agent context | 重複標準動作（API 慣例、commit 規範）、自動偵測觸發 |
| **Sub-agent** | `.claude/agents/<name>.md` | 獨立 context + 獨立工具權限 + 可指定模型 | 深度搜尋、code review、UI 處理、換模型省錢 |
| **Agent Teams** | 多 worktree + git 協調 | 跨 session 平行 | 大型專案（sweet spot 3-5 個 parallel）|

三個推薦 Sub-agent 範本：
- **security-reviewer**（model: opus）：抓 Injection / 認證授權 / secrets / 敏感資料
- **test-writer**（model: sonnet）：列 happy path + edge case + error case，Vitest 格式
- **ui-engineer**（model: sonnet）：Tailwind class、design system token、RWD 規範

注意：Claude 自動 routing 到 sub-agent 不穩定，重要分工建議在 prompt 明確指定。

### 第三階段：開發紀律 — 三條鐵則

1. **同一輪補測試**：每改 code 同輪就補 `.test.ts`，不拖到下次——事後補的測試容易變成「為了 pass 而 pass」，蓋不到 edge case
2. **UI 切員工**：動 Tailwind class / 元件樣式 → 呼叫 ui-engineer sub-agent，主 agent 主導方向、員工執行細節
3. **一個邏輯一個 commit**：Conventional Commits（feat/fix/test/refactor/docs/chore）+ 不堆大塊，確保可以乾淨回滾

### 第四階段：收尾驗證 — 三層安全網

| 層 | 工具 | 抓到的問題 |
|----|------|-----------|
| 第一層 | `typecheck + test + build` | 型別錯 / 行為錯 / 打包爆 |
| 第二層 | Writer/Reviewer 雙 Claude | Confirmation bias、edge case、安全漏洞 |
| 第三層 | AI 引用 cross-check | 套件版本、API 規格、雲端後台 UI 過期 |

Writer/Reviewer 模式：Session A 實作 → Session B 用 fresh context review（或設成 security-reviewer sub-agent）→ Session A 根據意見修正。

### 第五階段：失誤救援 — 回滾決策表

| 情境 | 動作 |
|------|------|
| 剛改的東西 review 不對 | `/rewind`（or `Esc+Esc`）回上個 checkpoint |
| 單一檔案錯，方向 OK | `git restore <檔案>` 重做 |
| 多檔案壞，方向 OK | `git restore` 多個檔案 |
| 套錯架構 / 誤解需求 | 砍 plan 重寫，從規劃階段重來 |
| 補的測試蓋不到意圖 | 砍 test、重看 plan、重補 |
| typecheck/test 大規模爆 | 高機率方向錯，停手檢查 plan |

⚠ `/rewind` 只追蹤 Claude 改過的檔案，Bash 指令造成的變更（npm install、rm 等）需靠 git 恢復，所以 git commit 紀律不能省。

### 第六階段：Memory 系統 — 兩層架構

| 層級 | 位置 | 內容 |
|------|------|------|
| 全域層 | `~/.claude/CLAUDE.md` 或 `~/.claude/rules/<topic>.md` | 跨專案通用規則（套件管理、測試紀律、commit 格式）|
| 專案層 | 專案 `AGENTS.md` / `CLAUDE.md` | 只在這專案用的設計決策、踩過的雷 |

`~/.claude/rules/` 支援 YAML frontmatter `paths:` 做 path-scoped rules（只在匹配路徑時載入）。

**寫 rule 的關鍵**：禁令式（不要 X）只能擋雷，**正向 pattern**（成功的 plan 應包含目標 + 任務拆解 + 不做的事 + 驗收條件四塊）才能引導對的產出。兩者混搭最有效。

**踩坑封 SOP 元 pattern**：踩到雷 → review 抓到 → 砍 plan 重寫 → 寫進 AGENTS.md + `~/.claude/rules/` → 大幅降低再踩機率。

## C（關鍵行動）

- [ ] 新專案先寫 CLAUDE.md 憲法（Project Overview + Build/Test + Code Style + Security + 明確排除的事）
- [ ] 設計三種員工：security-reviewer / test-writer / ui-engineer sub-agent
- [ ] 制度化「同一輪補測試」：開工前 prompt 明確說清楚
- [ ] Writer/Reviewer 雙 Claude 模式：重要功能收尾必跑
- [ ] 踩到雷就寫進 `~/.claude/rules/` 或 AGENTS.md，把雷轉化成 rule

## 相關連結
- [[ai-agent-reverse-workflow-decomposition]] — 反向拆解工作流的微型組織設計法，可當 SOP 建立前的需求盤點
- [[agentic-engineering-mindset]] — Karpathy 哲學觀：Bounded Context 護城河，品味與理解不可外包
- [[agentic-engineering-patterns]] — Simon Willison 實用工程模式
- [[agents-md-guide]] — AGENTS.md 完整指南（+4% vs -3% 效果研究）
- [[claude-code-context-management]] — Context 管控：六成存檔 + /clear 重開
- [[spec-driven-ai-workflow]] — 規格驅動工作流：spec-kit / OpenSpec
- [[claude-code-five-layer-architecture]] — Claude Code ADK 五層架構

## 補充資料：CEO + 員工模式與複雜任務挑戰

來源：Ray 貓，《實戰分享我是怎麼指揮 Claude Code 用少量時間征服大型複雜任務》，2026-05。

### CEO + 員工 Sub-agent 協作模式

**角色設定**：

- **CEO Agent（主 Agent）**：分解任務、規劃方向、分派給 Worker Agent、整合結果
- **Worker Agent（員工）**：執行 CEO 分派的具體子任務，在獨立 context 中完成

與六階段 SOP 中的「Leader + Sub-agent 員工」模式相同，但 Ray 貓特別點出大型複雜任務的三大挑戰，以及應對策略：

### 三大挑戰與應對

| 挑戰 | 觸發條件 | 應對策略 |
|------|---------|---------|
| **Context 崩塌** | 主 Agent context 超過 70K+ token，無法維持全局意識 | 任務開始前強制落檔 tasks.md；CEO 只讀摘要，不讀細節 |
| **目標漂移** | Worker 在長任務中脫離原始目標，自由發揮 | 每個 Worker 的 prompt 必須明確包含驗收條件；CEO 定期 check diff |
| **狀態追蹤與錯誤恢復** | Worker 失敗時 CEO 不知道哪步出錯、進度到哪 | tasks.md 作為共享狀態板；每個子任務完成後立即更新狀態 |

**SDD 作為基礎**：Spec-Driven Development 在 CEO 模式下不可或缺——CEO 必須先有清晰的規格，才能把任務拆給 Worker，否則 Worker 會在沒有邊界的情況下自由發揮。對應六階段 SOP 的「第一階段：Spec/Plan/Tasks 三層落檔」。

**70K token 門檻**：這是 Context 崩塌的實測警戒線，對應六階段 SOP 中「六成存檔」的 Context 管理原則（見 [[claude-code-context-management]]）。
