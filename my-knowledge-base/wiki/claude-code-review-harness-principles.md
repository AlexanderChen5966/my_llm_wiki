---
title: Claude Code /code-review 六大設計原則
date: 2026-05-25
tags: [ai/claude, pkm/workflow]
source: Facebook 社群貼文
type: article
status: processed
asset_value: 60
---

Claude Code v2.1.147 將 `/simplify` 改名為 `/code-review` 後，從其設計中可提煉出六個值得借鑒的 code review 設計原則，包含 pre-existing bug 分層、REVIEW.md 分離、re-review 收斂規則、verification pass 過濾假陽性、nit 數量上限，以及偵測與 gating policy 解耦。

## T（核心論點）

Anthropic 的 `/code-review` 設計體現了六個重要的 review 工程原則，核心思路是：**把「偵測什麼」和「如何阻擋」分開，把「舊問題」和「新引入問題」分開，把「風格意見」和「可執行行動」分開**。這三對分離原則讓 review 系統更精準、更少疲勞、更容易自動化。

同時注意：`/code-review`（v2.1.147 起）**不再做 cleanup-and-fix**（舊 `/simplify` 會實際重構），只做回報。

## B（適用場景）

**適用：**
- 設計或改進 AI 驅動的 code review 流程
- 評估自己的 review Skill 或 Harness 是否需要更新
- 想把 Harness 的 review 設計升級到接近官方水準

**不適用 / 注意：**
- 六個原則中，部分（如 gating policy 解耦）對單人/小團隊意義較低，可視情況取捨
- `/code-review` 不讀 REVIEW.md，只讀 CLAUDE.md（REVIEW.md 僅供 GitHub App 雲端服務使用）

## R（操作步驟）

### 六個設計原則詳解

**原則一：Pre-existing 嚴重度層級**

把「此 PR 引入的 bug」和「codebase 本來就壞的 bug」分開：
- `Critical` / `Important` / `Minor`：本 PR 引入
- `Pre-existing`：記錄、但不阻擋本 PR
- 避免 reviewer 拿舊問題擋新 PR（scope creep）

**原則二：REVIEW.md 與 CLAUDE.md 分離**

- `CLAUDE.md`：給「寫 code 的 agent」看的通用 context
- `REVIEW.md`：review 專用校準，注入所有 review agent 的最高優先指令
- 內容：本 repo 的 Critical 定義、Nit 上限、哪些路徑跳過、re-review 收斂規則
- 附帶好處：未來啟用 GitHub App 服務，REVIEW.md 自動被讀取

**原則三：Re-review 收斂規則**

建議寫法：「第一輪之後，suppress 新的 Nit，只報 Important」
- 避免一行 fix 被風格意見拖到第 7 輪
- 加一句「re-review 只看 Critical/Important，新 Nit 記錄不阻擋」

**原則四：顯式 verification pass（過濾假陽性）**

多 agent 平行後，加一個 verification 步驟對照真實 code 行為：
- `behavior 主張要附 file:line 引證，不能靠命名臆測`
- 去重 + 信心評分（官方閾值：80 分以上才發）
- 無此步驟時，4 個 agent 容易報重疊或臆測的 finding

**原則五：Nit 數量上限 + tally-first 摘要**

- 最多報 5 個 nit，其餘用數量帶過
- 摘要開頭先給一行統計（如 `2 factual, 4 style`）
- 沒 factual 問題就先講 → 減少 reviewer 疲勞

**原則六：偵測與 gating policy 解耦**

- Check run 永遠 neutral（不 block merge）
- 吐出 machine-readable 的 severity JSON，讓 CI 自行決定是否 gate
- 「偵測歸偵測、gating policy 歸 policy」——未來自動化時省很多事

### 官方 /code-review 運作流程（v2.1.147）

1. Haiku agent 前置檢查（PR 是否 closed/draft/trivial/已 review）
2. Haiku agent 收集 CLAUDE.md（root + 被修改目錄）
3. 5 個平行 Sonnet agent：CLAUDE.md 合規、淺層 bug 掃描、git blame 歷史、歷史 PR comment、程式碼註解合規
4. Haiku agent 對每個 issue 信心評分（0-100）
5. 過濾 < 80 分的 issue
6. 用 `gh pr comment` 發布結果

## C（關鍵行動）

- [ ] 評估現有 review Skill：是否有 pre-existing 分層？是否有 Nit 上限？是否有 re-review 收斂條款？
- [ ] 考慮建立 REVIEW.md（如果未來有開啟 GitHub App 服務的計畫）
- [ ] 如果有多 agent review，加入 verification pass 步驟（80 分信心門檻）
- [ ] 把 gating policy（什麼情況阻擋 merge）從 review 定義中分離出來

## 原始摘要

分析 Claude Code v2.1.147 將 /simplify 改名為 /code-review 的影響，並從中提煉六個值得學習的 code review 設計原則，包含 pre-existing bug 分層、REVIEW.md 分離、re-review 收斂規則、verification pass 過濾假陽性、nit 數量上限，以及偵測與 gating policy 解耦。

## 補充資料：REVIEW.md vs 本地 /code-review 的關鍵差異

來源：個人研究筆記（2026-05-25），調查 Claude Code v2.1.147 `/simplify` 改名為 `/code-review` 後的實際行為。

**結論：本地開發不需要 REVIEW.md。**

| 項目 | REVIEW.md | 本地 /code-review |
|------|-----------|-----------------|
| 讀取時機 | GitHub App Code Review 雲端服務 | 不讀取，只讀 CLAUDE.md |
| 適用方案 | Team / Enterprise 訂閱 | 所有方案 |
| 注入方式 | 逐字注入為最高優先指令 | — |
| 支援 @import | ❌ 不展開引用 | — |

**REVIEW.md 可調整項目**（僅雲端服務有效）：
- 重新定義 🔴 Important 的門檻
- 指定 Cap nits 上限
- Skip 特定 rules
- 加入 repo-specific checks

> 本地跑 `/code-review` 時，所有 review 行為由 `CLAUDE.md` 控制，不會讀取 `REVIEW.md`。

## 相關連結

- [[claude-code-commands-2026]] — 2026 上半年新命令解析（含 /code-review 前身 /simplify）
- [[harness-engineering]] — Harness Engineering 框架，更高層次的 review 系統設計視角
- [[agents-md-guide]] — AGENTS.md 設計原則，與 REVIEW.md 分離概念互補
