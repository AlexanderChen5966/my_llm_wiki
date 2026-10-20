---
status: processed
---
# Claude Code REVIEW.md vs 本地 /code-review 研究筆記

> 調查日期：2026-05-25
> 起因：Claude Code v2.1.147 將 `/simplify` 改名為 `/code-review`，評估是否需要建立 REVIEW.md

---

## 結論

**本地開發不需要 REVIEW.md。** `REVIEW.md` 只有 GitHub App Code Review 雲端服務（Team/Enterprise 訂閱）才會讀取。本地 `/code-review` 指令只讀 `CLAUDE.md`。

---

## REVIEW.md 是什麼

- 放在 repo root 的純 Markdown 設定檔
- 內容被**逐字注入**雲端 Code Review 服務所有 agent 的 system prompt，為最高優先指令
- **不支援** `@import` 語法（不展開引用檔案）
- 用途：調整 severity、cap nits、skip rules、repo-specific checks

### REVIEW.md 可調整項目

| 項目 | 說明 |
|------|------|
| Severity 定義 | 重新定義 🔴 Important 的門檻（如 docs repo 可收窄定義）|
| Nit 數量上限 | 如「最多 5 個 Nit，其餘在 summary 用計數帶過」|
| Skip rules | 路徑（generated code、lockfile）、branch pattern、finding category |
| Repo-specific checks | 自訂規則（如「新 API route 必須有 integration test」）|
| Verification bar | 要求 agent 附 file:line 證據才可發 finding |
| Re-review convergence | 避免重複 review 時越來越多 nit |
| Summary shape | 自訂 review summary 格式 |

### REVIEW.md 範例（官方）

```markdown
# Review instructions

## What Important means here
Reserve Important for findings that would break behavior, leak data,
or block a rollback.

## Cap the nits
Report at most five Nits per review.

## Do not report
- Anything CI already enforces: lint, formatting, type errors
- Generated files under `src/gen/` and any `*.lock` file
- Test-only code

## Always check
- New API routes have an integration test
- Log lines don't include PII
- Database queries are scoped to the caller's tenant
```

---

## 本地 /code-review 指令（v2.1.147）

### 運作方式

1. Haiku agent 前置檢查（PR 是否 closed/draft/trivial/已 review）
2. Haiku agent 收集 **CLAUDE.md 檔案**（root + 被修改目錄）
3. Haiku agent 摘要 PR 變更
4. **5 個平行 Sonnet agent** 各司其職：
   - Agent #1：CLAUDE.md 合規性
   - Agent #2：淺層 bug 掃描（只看 diff，不讀額外 context）
   - Agent #3：git blame 歷史脈絡
   - Agent #4：歷史 PR comment
   - Agent #5：程式碼註解合規
5. Haiku agent 對每個 issue 信心評分（0-100）
6. 過濾 < 80 分的 issue
7. 用 `gh pr comment` 發布結果

### 關鍵事實

- **完全不讀 REVIEW.md**
- 唯一外部設定來源 = CLAUDE.md（含 @import 展開的 AGENTS.md、DESIGN.md 等）
- 不跑 build/lint/typecheck（假設 CI 會做）
- 閾值 80 分；低於 80 的 finding 全部丟棄

### 不會 flag 的東西（false positive 定義）

- Pre-existing issues
- Pedantic nitpicks
- Linter/compiler 會抓的（import、type error、格式）
- 通用程式品質問題（除非 CLAUDE.md 明確要求）
- CLAUDE.md 要求但被 lint ignore comment 壓制的
- 使用者未修改的行上的問題

---

## GitHub App Code Review 雲端服務

### 適用對象
- Team / Enterprise 訂閱
- 不支援 Zero Data Retention 組織

### 運作方式
- PR open / push / 手動 `@claude review` 觸發
- 多 Agent 並行分析 diff + 完整 codebase context
- 結果：inline PR comment + check run severity table
- 平均 $15-25/次

### 讀取設定檔
- **CLAUDE.md**：通用專案 context，違規標記為 Nit
- **REVIEW.md**：review 專用最高優先指令

### Severity 等級
| 標記 | 等級 | 意義 |
|------|------|------|
| 🔴 | Important | 應在 merge 前修復的 bug |
| 🟡 | Nit | 小問題，值得修但不 block |
| 🟣 | Pre-existing | 非本 PR 引入的既有 bug |

### Check run 行為
- 結論永遠是 neutral（不 block merge）
- 可用 `gh api` 讀取 severity JSON 做 CI gate

---

## 三者對照

| 機制 | 適用場景 | 讀 REVIEW.md | 讀 CLAUDE.md | 費用 |
|------|---------|:---:|:---:|------|
| GitHub App Code Review | PR 自動/手動觸發（雲端） | ✅ | ✅（違規=Nit） | $15-25/次 |
| 本地 `/code-review` | Terminal 手動執行 | ❌ | ✅ | 一般 token 用量 |
| 自訂 `stack-review.skill` | `/stack-review` 觸發 | ❌ | 間接（skill 內建規則） | 一般 token 用量 |

---

## 對 b2b-manager 專案的決定

1. **不建立 REVIEW.md**：目前未使用 GitHub App Code Review 服務
2. **現有 CLAUDE.md + .claude/rules/ 已足夠**引導本地 `/code-review`
3. **自訂 skill 重命名為 `stack-review`**：避免和內建 `/code-review` 衝突
4. 若未來啟用 GitHub App 服務，再建立 REVIEW.md，把最重要的 5-10 條規則（screenutil、sortColumnIndex+1、OAuth2 直接呼叫）放入

---

## 資料來源

- [Code Review - Claude Code Docs](https://code.claude.com/docs/en/code-review)
- [claude-code/plugins/code-review/README.md](https://github.com/anthropics/claude-code/blob/main/plugins/code-review/README.md)
- [claude-code/plugins/code-review/commands/code-review.md](https://github.com/anthropics/claude-code/blob/main/plugins/code-review/commands/code-review.md)
- 本機安裝路徑驗證：`/Users/alexander/.claude/plugins/marketplaces/claude-plugins-official/plugins/code-review/commands/code-review.md`