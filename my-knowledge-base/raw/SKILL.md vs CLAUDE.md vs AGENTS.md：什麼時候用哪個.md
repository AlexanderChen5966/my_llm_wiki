---
title: "SKILL.md vs CLAUDE.md vs AGENTS.md：什麼時候用哪個"
source: "https://www.termdock.com/blog/skill-md-vs-claude-md-vs-agents-md/zh"
author:
  - "[[Termdock Team]]"
published: 2024-01-01
created: 2026-06-05
description: "完整比較 SKILL.md、CLAUDE.md、AGENTS.md 三種設定檔 — 各自的用途、哪些工具會讀取、如何分層配置讓 AI agent 效能最佳化。"
tags:
  - "clippings"
status: processed
---
## 一句話結論

**CLAUDE.md** 是給 Claude Code 的專案 context。 **AGENTS.md** 是同樣的東西但跨工具通用。 **SKILL.md** 是任務層級的能力，按需載入。三者解決不同問題。應該同時使用。

只有時間維護一個檔案的話，選 AGENTS.md。它是 2026 年最接近通用標準的 AI coding agent 設定檔，Codex CLI、Copilot CLI、Gemini CLI、Cursor、Claude Code 都認得。但「只一個檔案」是假經濟。真正的效能來自正確的分層。

本文講清楚什麼放哪裡、哪些工具讀哪些檔案、如何避開多數開發者踩到的效能陷阱。

## 比較總表

|  | SKILL.md | CLAUDE.md | AGENTS.md | .cursorrules |
| --- | --- | --- | --- | --- |
| **用途** | 任務能力 | 專案 context | 專案 context | 編輯器規則 |
| **Scope** | 單一任務（相關時才載入） | 整個 session（永遠載入） | 整個 session（永遠載入） | 整個 session（永遠載入） |
| **存放位置** | `.claude/skills/` | 專案根目錄 | 專案根目錄 | 專案根目錄（舊版）或 `.cursor/rules/` |
| **誰會讀** | Claude Code、Codex CLI、Copilot CLI、Gemini CLI | Claude Code | Codex CLI、Copilot CLI、Gemini CLI、Cursor、Claude Code | Cursor |
| **可包含 script** | 可以 | 不行 | 不行 | 不行 |
| **自動觸發** | 是（根據 description 比對） | 永遠 | 永遠 | 永遠 |
| **建議大小** | 每個 skill < 500 行 | < 100 行 | < 100 行 | < 50 行 |

**重點：** CLAUDE.md 和 AGENTS.md 做同一件事 -- 持久性專案 context，注入到每次對話。差別在工具相容性。SKILL.md 本質上不同：條件式載入，只有當前任務符合 skill 的 description 才會載入。這種按需載入正是 skill 存在的理由 -- 你不該把所有東西塞進 CLAUDE.md。

`.cursorrules` 列在這裡是為了完整性。Cursor 仍支援但已遷移到 `.cursor/rules/` 的 `.mdc` 格式，提供更細粒度的基於檔案 pattern 的規則。同時用 Cursor 和 CLI agent 時，`.cursor/rules/` 分開維護 -- 格式是 Cursor 專屬的，不能移植到其他工具。

## CLAUDE.md：你專案的 AI 新人文件

CLAUDE.md 是 Claude Code 每次 session 開始時讀的檔案。把它想成新人入職文件 -- 不是給新員工，是給每次對話都沒有記憶的 AI agent。

### 該放什麼

- **架構概覽** -- 框架、資料庫、驗證方案、主要依賴
- **程式碼慣例** -- 命名模式、錯誤處理風格、import 慣例
- **硬性限制** -- agent 絕不能做的事（修改 migration、使用特定套件、破壞特定 API）
- **build 和測試指令** -- 如何跑測試、lint、build

### 不該放什麼

- 任務特定 workflow（屬於 SKILL.md）
- 冗長參考文件（改用連結）
- Agent 從程式碼推斷得到的資訊（ `tsconfig.json` 已經告訴它你用 TypeScript）
- Linter 規則（linter 會確定性地強制執行；CLAUDE.md 不會）

### ETH Zurich 的研究發現

2026 年 2 月，ETH Zurich 的 Thibaud Gloaguen 等人發表了 ["Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?"](https://arxiv.org/abs/2602.11988) -- 第一篇嚴格評估這類 context 檔案是否有幫助的研究。結果讓很多人意外：

- LLM 生成的 context 檔案比完全不提供，任務成功率 **降低** 約 3%。
- 人工撰寫的 context 檔案只提升約 4%。
- 加入 context 檔案讓推理成本增加超過 20%。

核心問題：詳盡的 codebase 概述跟既有文件高度重複，過度限制的指令反而讓 agent 更難解決實際任務。Agent 已經會讀程式碼 -- 重述程式碼裡已有的資訊只是浪費 context window、增加噪音。

**實務意涵：** CLAUDE.md 保持精簡。100 行以下。每行都要通過測試：「拿掉這行，agent 會犯一個自己無法恢復的錯誤嗎？」

### 實際範例

```markdown
## Project Overview
E-commerce API serving 50k DAU. Monorepo with API server and admin dashboard.

## Architecture
- Runtime: Node.js 22, TypeScript strict
- Framework: Fastify 5
- Database: PostgreSQL 16 via Drizzle ORM
- Auth: Custom JWT with refresh token rotation
- Queue: BullMQ on Redis

## Conventions
- Error handling: Result<T, E> pattern (see src/lib/result.ts)
- All endpoints validated with Zod schemas in src/schemas/
- Database queries go in src/repositories/, never in route handlers
- Tests: Vitest for unit, Supertest for integration

## Hard Constraints
- Never modify files in src/db/migrations/
- Never use default exports except in route files
- All new endpoints need integration tests before merge

## Commands
- Test: pnpm test
- Lint: pnpm lint
- Build: pnpm build
- Migrate: pnpm db:migrate
```

30 行。告訴 agent 有效工作需要的所有資訊，不重述程式碼已經呈現的內容。

## AGENTS.md：跨工具的專案 Context

AGENTS.md 和 CLAUDE.md 用途相同 -- 持久性專案 context -- 但工具相容性更廣。最初由 OpenAI 的 Codex CLI 推廣，現在是由 Linux Foundation 下 [Agentic AI Foundation](https://agents.md/) 託管的開放標準，已被超過 60,000 個開源專案採用。

### 哪些工具會讀

截至 2026 年 3 月：

- **Codex CLI** （OpenAI）-- 每次任務前讀取
- **Copilot CLI** （GitHub）-- 自動發現並載入
- **Gemini CLI** （Google）-- 原生支援
- **Cursor** -- 與 `.cursor/rules/` 一起讀取
- **Claude Code** （Anthropic）-- 作為補充 context 讀取

Claude Code 同時讀 CLAUDE.md 和 AGENTS.md。Codex CLI 讀 AGENTS.md 但不讀 CLAUDE.md。這個不對稱性創造了實用策略：

### 策略：AGENTS.md 作為唯一真相來源

AGENTS.md 當專案 context 的唯一真相來源。CLAUDE.md 最小化 -- 只放 Claude Code 專屬指令（像 compaction 行為或權限提示），其他全部引用 AGENTS.md。

```markdown
# CLAUDE.md

Read AGENTS.md for project architecture and conventions.

## Claude Code-Specific
- When compacting, preserve the full list of modified files
- Prefer subagents for research tasks over inline exploration
```
```markdown
# AGENTS.md

## Project Overview
E-commerce API serving 50k DAU. Monorepo with API server and admin dashboard.

## Architecture
- Runtime: Node.js 22, TypeScript strict
- Framework: Fastify 5
- Database: PostgreSQL 16 via Drizzle ORM
...
（跟上面完整 CLAUDE.md 範例一樣的內容）
```

消除重複。多 AI 工具的團隊得到一致行為。只用 Claude Code 的團隊不損失任何東西 -- Claude Code 兩個都讀。

### 巢狀 AGENTS.md

AGENTS.md 和 CLAUDE.md 都支援目錄層級巢狀。 `frontend/AGENTS.md` 可包含只在 agent 處理該目錄檔案時適用的前端慣例。Monorepo 中不同 package 有不同慣例時用這個。

## SKILL.md：按需載入的專業能力

這裡才是有趣的地方。SKILL.md 跟 CLAUDE.md、AGENTS.md 有本質差異。關鍵： **skill 只在相關時才載入。**

啟動 Claude Code session 時，CLAUDE.md 和 AGENTS.md 立即注入 context window。每個 token 都算在整個 session 的 context 預算裡，不管你在做什麼。

Skill 不一樣。每個 skill 在 YAML frontmatter 有 `description` 欄位。Claude Code 讀所有 skill description（短字串，成本低），只在當前任務符合某個 description 時才載入完整 skill 指令。所以你不該把所有東西塞進 CLAUDE.md -- 專業知識該放在 skill 裡按需載入，保持基礎 context 精簡。

深入理解 skill 系統及如何打造有效 skill，看 [Agent Skills 指南](https://www.termdock.com/zh/blog/agent-skills-guide) 。

### SKILL.md 的結構

每個 skill 住在 `.claude/skills/` 下自己的資料夾。裡面必須有 `SKILL.md` ，包含 YAML frontmatter 和 markdown 指令。

```
.claude/skills/
  database-migration/
    SKILL.md
  api-endpoint/
    SKILL.md
  pr-review/
    SKILL.md
```

具體範例 -- 資料庫 migration skill：

```markdown
---
name: database-migration
description: >
  Use this skill when creating, modifying, or reviewing database migrations.
  Triggers on: new migration files, schema changes, Drizzle ORM modifications,
  database column additions or removals, index changes.
---

## Database Migration Workflow

### Before Creating a Migration
1. Read the current schema in src/db/schema.ts
2. Check existing migrations in src/db/migrations/ for naming conventions
3. Verify no pending migrations exist: run \`pnpm db:status\`

### Creating the Migration
1. Modify the schema in src/db/schema.ts first
2. Generate the migration: \`pnpm db:generate\`
3. Never write migration SQL by hand — always use the generator
4. Review the generated SQL for correctness

### After Creating a Migration
1. Run \`pnpm db:migrate\` on the dev database
2. Run integration tests: \`pnpm test:integration\`
3. If tests fail, do NOT modify the migration file — drop and regenerate

### Naming Convention
- Format: NNNN_descriptive_name.sql
- Examples: 0042_add_user_preferences.sql, 0043_create_audit_log_table.sql

### Hard Rules
- Never modify an existing migration file that has been committed
- Never delete a migration file
- Always test migrations against a fresh database: \`pnpm db:reset && pnpm db:migrate\`
```

30 多行 migration 專屬知識。放在 CLAUDE.md 的話每次 session 都消耗 context -- 即使你在做前端元件。作為 skill，只在你要求建 migration 或偵測到 migration 相關檔案時才載入。

### Skill 的獨特能力

三項能力讓 SKILL.md 與 context 檔案截然不同：

1. **Script 執行。** Skill 可包含 Claude Code 執行的 script。Deploy skill 可能有 pre-deploy checklist script。CLAUDE.md 和 AGENTS.md 都做不到。
2. **根據 description 自動觸發。** Frontmatter 的 `description` 告訴 Claude Code 何時啟動 skill。Description 寫得稍微「強勢」一點 -- Claude Code 傾向少觸發而非多觸發。
3. **存取控制。** Skill 支援 `disable-model-invocation: true` （只有使用者能觸發）、 `user-invocable: false` （只有 Claude Code 觸發，作為背景知識）、 `allowed-tools` （限制 Claude Code 在 skill 內能用的工具）。Context 檔案不具備這種細粒度控制。

## 分層策略

三種檔案同時使用，用最少 context 消耗達到最大效果：

**第一層：AGENTS.md（永遠載入、跨工具）** 專案架構、程式碼慣例、硬性限制、build 指令。100 行以下。每個 AI 工具都讀的通用專案 context。

**第二層：CLAUDE.md（永遠載入、Claude Code 專屬）** 最小化。共享 context 引用 AGENTS.md。只加 Claude Code 專屬指令 -- compaction 行為、subagent 偏好、權限提示。20 行以下。

**第三層：Skill（按需載入、任務專屬）** 專業 workflow，相關時才載入。資料庫 migration、PR review 流程、deploy checklist、API endpoint 模式、測試慣例。每個 skill 可以很詳細（最多 500 行），因為只在需要時才進 context。

**結果：** 基礎 context 精簡（CLAUDE.md + AGENTS.md 加起來 120 行以下），按需可獲深度能力。這架構尊重 ETH Zurich 研究（永遠載入的 context 最小化），同時在 agent 需要時提供豐富任務專屬指引。

這種 workspace 層級的設定管理 -- context 檔案、skill、工具設定在開發環境中保持同步 -- 正是 [Termdock](https://www.termdock.com/zh) 的 workspace sync 大顯身手的地方。切換專案時，整個 context 設定（終端、環境變數、git 狀態）跟著你切。

Try Termdock — Workspace Sync works out of the box. [Free download →](https://github.com/termdock/termdock-issues/releases)

## 決策流程圖

要為 AI agent 寫一條新指令時，依序問：

**「這跟專案的每一個任務都有關嗎？」** 是 -> AGENTS.md（或 CLAUDE.md 如果只用 Claude Code） 不是 -> 繼續。

**「這是有多個步驟的專業 workflow 嗎？」** 是 -> 在 `.claude/skills/` 建 SKILL.md 不是 -> 繼續。

**「需要在 workflow 中執行 script 或命令嗎？」** 是 -> SKILL.md。唯一支援 script 執行的選項。 不是 -> 繼續。

**「你用多種 AI coding 工具嗎？」** 是 -> 共享 context 放 AGENTS.md，Claude 專屬放 CLAUDE.md，任務 workflow 放 SKILL.md。 不是（只用 Claude Code）-> CLAUDE.md 放專案 context，SKILL.md 放任務 workflow。

**「這是 Cursor 專屬的編輯器行為嗎？」** 是 -> `.cursor/rules/` 的 `.mdc` 檔。跟 agent context 檔案分開。

中間地帶預設放 AGENTS.md。覆蓋面最廣的檔案。Claude Code 讀 AGENTS.md 而不是 CLAUDE.md 沒任何壞處 -- 兩個都讀。

## 常見錯誤

### 錯誤 1：把所有東西塞進 CLAUDE.md

最常見的失敗模式。開發者寫 300 行以上的 CLAUDE.md，包含程式碼標準、架構文件、workflow 指令、style guide。ETH Zurich 研究直接證明：越長的 context 檔案效能越差。Agent 遵循部分指令、忽略其他，這種不一致比完全沒有 context 檔案還糟。

**解法：** 無情精簡。專業 workflow 移到 skill。跨工具 context 移到 AGENTS.md。CLAUDE.md 剩下的東西應該一個螢幕看完。

### 錯誤 2：完全不用 Skill

很多開發者學會 CLAUDE.md 就停了。從沒發現 skill 存在，或覺得額外複雜度不值得。這等於把大量能力留在桌上。

Skill 是區分「agent 知道你專案架構」（CLAUDE.md）和「agent 知道如何執行你的特定 workflow」（skill）的關鍵。上面的 migration 範例包含新人要花好幾小時才能學會的機構知識。有 skill，agent 瞬間就有 -- 而且只在相關時才載入。

用 Claude Code 或任何相容 skill 的 agent，從 2-3 個最常見 workflow 的 skill 開始。完整教學看 [Agent Skills 指南](https://www.termdock.com/zh/blog/agent-skills-guide) 。

### 錯誤 3：跨檔案重複內容

CLAUDE.md 寫「用 Vitest 跑測試」。AGENTS.md 寫「用 Vitest 跑測試」。測試 skill 也寫「用 Vitest 跑測試」。同一條指令三份拷貝代表換測試框架時改三個地方 -- 必然有一個被漏。

**解法：** 單一真相來源。AGENTS.md 持有正式版專案 context。CLAUDE.md 引用它。Skill 引用它。Skill 需要專案層級資訊時寫「遵循 AGENTS.md 中的測試慣例」而不是重述。

### 錯誤 4：用 LLM 生成 Context 檔案

ETH Zurich 研究特別測了：讓 LLM 生 context 檔案，結果比手寫更差。LLM 生成的檔案冗長、泛用、充滿 agent 從 codebase 已能看到的重複資訊。

手寫 context 檔案。你比任何 LLM 都更了解專案的怪癖、限制和慣例。 [AI CLI 工具指南](https://www.termdock.com/zh/blog/ai-cli-tools-guide) 有可直接改用的 CLAUDE.md 範本 -- 但要改寫它，不要生成。

### 錯誤 5：忽略跨工具相容性

團隊今天用 Claude Code，明天可能改用 Copilot CLI 或 Codex CLI。把所有 agent context 都建在 CLAUDE.md 等於綁死。AGENTS.md 是對沖 -- 今天就跨工具，標準在 Linux Foundation 下成熟後支援只會更廣。

DH

Danny Huang· [Follow on Threads →](https://www.threads.com/@cyh.289)

Free Download

### Ready to streamline your terminal workflow?

Multi-terminal drag-and-drop layout, workspace Git sync, built-in AI integration, AST code analysis — all in one app.

[Download Termdock →](https://github.com/termdock/termdock-issues/releases)

#skill-md#claude-md#agents-md#context-engineering#agent-skills

## 相關文章

2026年3月16日 ·agent-skills

### [2026 Agent Skills 完全指南：建立、分享與保護 AI Agent 能力](https://www.termdock.com/zh/blog/agent-skills-guide)

Agent Skills 的一切：SKILL.md 格式規範、490K+ 技能生態系、安全風險與最佳實踐，涵蓋 Claude Code、Codex CLI 與 Copilot。

agent-skillsskill-mdclaude-codecodex-clicopilotai-agents

2026年3月16日 ·ai-cli-tools

### [2026 AI CLI 工具完全指南：從安裝到多 Agent 工作流](https://www.termdock.com/zh/blog/ai-cli-tools-guide)

完整涵蓋 2026 年所有主流 AI 終端寫程式工具的指南。包含 Claude Code、Gemini CLI、Copilot CLI、Codex CLI、aider、Crush、OpenCode、Goose、Amp 的安裝教學、定價分析、context engineering、多 Agent 工作流、MCP 整合、終端模擬器搭配，以及安全最佳實踐。

ai-cliclaude-codegemini-clicopilot-cliterminaldeveloper-tools

·ai-cli-tools

### [CLAUDE.md 撰寫指南：AI CLI 工具的 Context Engineering](https://www.termdock.com/zh/blog/claude-md-writing-guide)

從零開始寫出真正提升 AI agent 表現的 CLAUDE.md。五個區塊、實際範例、token 預算控制、測試策略完整教學。

claude-mdcontext-engineeringclaude-codeworkflowai-clibest-practices