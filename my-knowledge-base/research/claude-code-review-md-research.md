---
title: Claude Code REVIEW.md vs 本地 /code-review 研究筆記
date: 2026-05-25
tags: [ai/claude, pkm/workflow]
source: Claude Code 官方文件 + 本機安裝驗證
type: note
status: processed
asset_value: 60
---

## T（核心論點）

**本地開發不需要 REVIEW.md。** REVIEW.md 只有 GitHub App Code Review 雲端服務（Team/Enterprise 訂閱）才會讀取；本地 `/code-review` 指令完全不讀 REVIEW.md，只讀 CLAUDE.md（含 @import 展開的 AGENTS.md 等）。

三種 review 機制的本質差異：偵測範圍、設定來源、費用結構完全不同。

## B（適用場景）

**適用：**
- 評估是否需要建立 REVIEW.md
- 設計本地 /code-review 校準策略
- 比較三種 review 機制的適用場景

**不適用 / 注意：**
- REVIEW.md 不支援 @import（不展開引用），必須直接寫
- 本地 /code-review 不跑 build/lint/typecheck（假設 CI 會做）
- 閾值 80 分以下的 finding 全部丟棄

## R（三者對照表）

| 機制 | 適用場景 | 讀 REVIEW.md | 讀 CLAUDE.md | 費用 |
|------|---------|:---:|:---:|------|
| GitHub App Code Review | PR 自動/手動觸發（雲端）| ✅ | ✅（違規=Nit）| $15-25/次 |
| 本地 `/code-review` | Terminal 手動執行 | ❌ | ✅ | 一般 token 用量 |
| 自訂 `stack-review.skill` | `/stack-review` 觸發 | ❌ | 間接（skill 內建規則）| 一般 token 用量 |

### 本地 /code-review 流程（v2.1.147）

5 個平行 Sonnet agent：CLAUDE.md 合規、淺層 bug 掃描（diff only）、git blame 歷史、歷史 PR comment、程式碼註解合規 → Haiku 信心評分（過濾 <80 分）→ `gh pr comment` 發布

### REVIEW.md 可調整項目（雲端服務專用）

| 項目 | 說明 |
|------|------|
| Severity 定義 | 重新定義 Important 的門檻 |
| Nit 數量上限 | 如「最多 5 個 Nit」|
| Skip rules | 路徑、branch pattern、finding category |
| Repo-specific checks | 自訂規則（如「新 API route 必須有 integration test」）|
| Verification bar | 要求附 file:line 證據 |
| Re-review convergence | 避免重複 review 時越來越多 nit |

## C（關鍵行動）

- [ ] 對 b2b-manager：不建立 REVIEW.md，現有 CLAUDE.md + .claude/rules/ 已足夠
- [ ] 自訂 skill 命名為 `stack-review`，避免與內建 `/code-review` 衝突
- [ ] 若未來啟用 GitHub App 服務，建立 REVIEW.md 放入最重要的 5-10 條規則

## 原始摘要

研究筆記：釐清 REVIEW.md 的適用範圍——本地 /code-review 指令完全不讀 REVIEW.md（只讀 CLAUDE.md），REVIEW.md 僅供 GitHub App 雲端 Code Review 服務（Team/Enterprise 訂閱）使用，並整理三種 review 機制的對照表與針對 b2b-manager 專案的決定。

## 相關連結

- [[claude-code-review-harness-principles]] — /code-review 六大設計原則（從這份研究衍生）
- [[agents-md-guide]] — AGENTS.md 指南，與 REVIEW.md 的分層設定概念互補
- [[claude-md-guide]] — CLAUDE.md 撰寫技巧
