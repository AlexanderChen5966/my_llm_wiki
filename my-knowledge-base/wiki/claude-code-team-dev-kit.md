---
title: Claude Code team-dev-kit：用 Skill + Hook 把團隊開發規則變成自動遵守的東西
date: 2026-06-22
tags: [ai/agent, ai/claude]
source: https://codelove.tw/@tony/post/a6GDnq
type: article
status: processed
asset_value: 70
---

團隊開發規則寫在 README 或 Wiki，建立在「人會閱讀、記住、遵守」的前提上——這個前提會失敗。team-dev-kit 把規則從「讓人閱讀的文件」改成「Claude Code 每次自動套用的 Skill」和「commit 當下由機器攔截的 Hook」，從第一天起就讓規則生效，不再靠記憶。

## T（核心論點）

**Skill 引導，Hook 阻擋**：
- **Skill（引導）**：讓 Claude Code 讀懂規則並自動套用，適合需要依脈絡判斷的規則
- **Hook（確定性阻擋）**：commit 當下由機器強制執行，適合絕對不能放行的底線

新成員不需要先背規範才能開始開發。只要說「我想加一個功能，從 Issue 到 PR 幫我做完，照這個 repo 的規則」，Claude Code 就會自動套用 commit 格式、branch 命名、`Closes #N` 關聯。

## B（適用場景）

**適合：**
- 多人、多個 repository 反覆執行同一套規範的團隊
- 有新成員進出、每次都要重複 onboarding 的團隊
- 每次 review 都會出現規約違反指摘的團隊

**不適合：**
- 單人玩票性質的個人 repo（意義不大）

## R（完整架構）

### Skill vs Hook 分工原則

| 放在 Skill | 放在 Hook |
|-----------|----------|
| 會隨脈絡改變、需要判斷的規則 | 絕對不能放行、必須確定擋下來的底線 |
| commit message 格式（scope 要看修改內容）| 機密資訊外洩偵測 |
| PR 流程、文件文體 | lint 強制通過（有零容忍需求時）|
| 效果是機率性的，AI 不一定每次完全遵守 | 效果是確定性的，條件符合就擋下來 |

**一句話**：引導你走向正確做法的是 Skill，絕對不能放行的是 Hook。

### 配置結構（bootstrap 後的檔案）

```
.claude/skills/       ← 業務 skill（只在這個專案啟用）
.claude/settings.json ← PreToolUse 機密資訊 hook
.team-dev-kit/        ← 規則本體（禁止編輯，更新時替換）
.githooks/            ← commit 時的自動檢查
AGENTS.md             ← 專案專屬規則（可編輯）
.gitleaks.toml        ← 機密資訊偵測規則（可編輯）
.github/              ← Issue / PR 模板（可編輯）
```

### 舊文件 → Skill / Hook 對應

| 舊文件（給人看）| 取代後 | 遵守方式 |
|---------------|--------|---------|
| commit 規範 | `git-commit` skill | 說「提交」時，自動生成正確格式 |
| branch 運作、Issue/PR | `github-workflow` skill | 自動建 branch、附加 `Closes #N`、禁止直接 push |
| Issue 建立指南 | `ticket-*` skill 群 | 從對話生成草稿並發行 |
| 文件撰寫規則 | `doc-writing` skill | 自動套用設計文件的文體和結構 |
| 機密資訊處理規則 | pre-commit hook + PreToolUse hook（gitleaks）| commit 前 / 發行前由機器直接**阻擋** |

### 機密資訊防護（雙層）

機密資訊外洩「只要放過一次就無法挽回」，所以兩層都要守：
1. **pre-commit hook**（`gitleaks`）：擋住人直接 `git commit` 的路徑
2. **PreToolUse hook**：擋住 Claude 透過工具發行的路徑

測試時用接近真實的隨機字串，不要用 `AKIAIOSFODNN7EXAMPLE`（文件示例 key，會被 allowlist 放行）。

```bash
# commit 被擋的輸出範例
WRN leaks found: 1
✋ 已偵測到疑似機密資訊或個人資訊，已停止 commit。
```

### gitleaks 客製化（專案疊層）

共通規則在 `.team-dev-kit/base.gitleaks.toml`（禁止編輯），各專案用 `.gitleaks.toml` 薄薄疊一層：

```toml
title = "secret & PII scan (project overlay)"

[extend]
path = ".team-dev-kit/base.gitleaks.toml"

[allowlist]
description = "公司內部 NTP server 固定 IP"
regexTarget = "match"
regexes = ['''\b192\.168\.10\.50\b''']
```

允許規則要盡量具體，不要太寬——誤判消除的洞要縮到最小。

### 快速啟動

加入已配置 team-dev-kit 的 repo：
```bash
git clone <repository URL>
cd <repository>
# 啟動 Claude Code 並允許 Trust，完成
```

首次導入（一次性）：
```bash
curl -fsSL https://raw.githubusercontent.com/aRaikoFunakami/team-dev-kit/main/bootstrap.sh | sh
```

### Skill 新增方式

Skill 就是 Markdown 文字，和現有的 `git-commit` skill 格式相同。
- **專案專屬規則**：寫在 `AGENTS.md` 的「專案專屬規範」段落，Claude Code 會讀並套用
- **跨 repo 規則變更**：回到 team-dev-kit 主 repo 提 PR，合併後各專案重新跑 bootstrap

### Hook 不要加過頭

Hook 加太多 → commit 速度變慢、提示變吵 → 大家用 `--no-verify` 關掉 → 完全沒人用。

**只有能自動修正、且是確定性的規則才放 Hook**；需要判斷的放 Skill。這和把 gitleaks allowlist 寫得很精準是同一個思路。

## C（關鍵行動）

- [ ] 在團隊 repo 執行 bootstrap 一次（需要有人做過這個初始化）
- [ ] 每位新成員 clone 後啟動 Claude Code 並 Trust，規則自動生效
- [ ] 遇到 gitleaks 誤判，在 `.gitleaks.toml` 加精確 allowlist，而非直接改 base 規則
- [ ] 想加新規則：能自動修正且確定性的 → Hook；需要判斷的 → Skill 或 AGENTS.md

## 相關連結

- [[spec-first-ai-coding-workflow]] — Spec-First 工作流：AGENTS.md 的另一個使用場景
- [[loop-engineering-implementation-patterns]] — Loop Engineering 實作：Stop hook 和 pre-commit 的協同
- [[claude-code-advanced-workflow]] — Claude Code 進階工作流
