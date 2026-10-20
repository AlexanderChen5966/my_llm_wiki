---
title: Loop Engineering：Claude Code 實作模式（CLAUDE.md 協議 + Stop Hook + Sub-agent）
date: 2026-06-22
tags: [ai/agent, ai/claude]
source: https://codelove.tw/@tony/post/3VzVZ3
type: article
status: processed
asset_value: 78
---

Loop Engineering 落地到 Claude Code 的具體實作：用三個檔案建立最小迴圈架構，把「你親手複製貼上錯誤再重試」這個動作從工作流程中消滅。

## T（核心論點）

**一個最小可用的 Claude Code Loop 只需要三個檔案：**
1. `CLAUDE.md`：定義迴圈協議和停止條件（讓 AI 知道什麼叫「完成」）
2. `.claude/settings.json`：Stop hook 作為硬性約束（讓機器強制執行）
3. `.claude/agents/fixer.md`：打破卡關的 sub-agent（新 context 重診斷）

核心原則：**CLAUDE.md 寫規則，Stop hook 執行，fixer agent 打破卡關。**

## B（適用場景）

- 正在使用 Claude Code 做 coding 任務，想讓 Agent 自動跑測試、自動修錯
- 希望 Claude Code 不要在測試沒通過的情況下宣稱「完成了」
- 想在自己的服務或 App 裡嵌入 Loop 機制（使用 Mastra 框架）

## R（完整實作）

### 最小 Claude Code Loop 三件套

#### ① CLAUDE.md — 迴圈協議（重新定義「完成」）

```markdown
## 迴圈協議

每個任務都要以「迴圈」而不是「直線流程」來執行：

1. 寫出修改
2. 跑檢查：測試 + linter + 型別檢查
3. 失敗了？讀取錯誤、找出原因、修正，然後回到第 2 步
4. 迴圈最多 5 次

停止條件：
- 全部檢查通過 → 回報「完成」，並附上通過的輸出作為證據
- 5 次用完 → 停下來，回報還剩下什麼沒解決
- 同一個錯誤連續出現 2 次 → 停止迴圈，並提示呼叫 @fixer

禁止：沒有檢查輸出就回報「完成」
禁止：透過刪除 assertion 或弱化測試來混過去。要修的是程式碼，不是分數板
```

最後兩個「禁止」是最有效的部分——沒有明確禁止，Claude 在第 3 輪可能偷偷刪掉一行 assertion 把測試過掉了，bug 還在但看起來通過了。

#### ② .claude/settings.json — Stop Hook（硬性約束）

```json
{
  "hooks": {
    "Stop": [{ "hooks": [{ "type": "command", "command": "npm test --silent 2>&1 | tail -20" }] }],
    "PostToolUse": [{ "matcher": "Write|Edit", "hooks": [{ "type": "command", "command": "npx tsc --noEmit --pretty false 2>&1 | head -10" }] }]
  }
}
```

- **PostToolUse**：每次編輯檔案就做型別檢查，讓 AI 邊寫邊修
- **Stop**：當 Claude 想說「完成」的瞬間，強制跑完整測試；失敗的話輸出直接進入對話，強制再跑一輪

這就是 Claude Code `/goal` 指令的本體——Stop hook 扮演能說「不行」的評估角色，只要這一關沒過，迴圈不能結束。

Python 專案換 `pytest -q` + `pyright`；Rust 換 `cargo test --quiet` + `cargo check`。

#### ③ .claude/agents/fixer.md — 打破卡關的 Sub-agent

```markdown
---
name: fixer
description: 用於同一個測試在兩次修正嘗試後仍失敗時，打破卡關的代理
tools: Read, Edit, Grep, Glob, Bash
model: opus
---

你要修正失敗的檢查。禁止猜測。

1. 自己執行失敗的檢查，讀完整個錯誤訊息
2. 從頭到尾讀完失敗路徑上的所有檔案
3. 用一句話寫出：真正原因是什麼
4. 只修那個原因，不做順手重構
5. 重新執行檢查，並回報修正前後的輸出

禁止：刪除測試、放寬 assertion、用 try/catch 吞掉錯誤、把測試 skip 掉
```

Fixer 被主對話呼叫時，**不帶前 4 次失敗的記憶**，從零開始診斷，所以常常一次就能過。這就是 Sub-agents 的核心原則：Writer 和 Checker 不能是同一個 context。

### Mastra Goals — 框架層的 Loop 實作

如果要把迴圈嵌進自己的服務，Mastra 提供內建的 `Goals` 功能：

```typescript
import { Agent } from '@mastra/core/agent';

const worker = new Agent({
  name: 'worker',
  instructions: '把軟體任務完整做到最後',
  model: 'openai/gpt-5.5',
  memory,
  goal: {
    judge: 'openai/gpt-5-mini', // 與作業角色不同的「判定角色」模型
    maxRuns: 30,                // 空轉上限（= 成本上限）
    prompt: '只有在測試通過時才視為完成',
  },
});

await worker.setObjective('實作 /health endpoint 並測試', {
  threadId,
  resourceId,
});
```

`judge`（用不同模型評分）和 `maxRuns`（成本上限）是兩個關鍵設定。Claude Code 的 `/goal` 做的事，幾乎直接落到這兩個設定上。

### cobusgreyling Loop 的 5 個動作（每輪迭代）

| 動作 | 描述 |
|------|------|
| 發現（discovery） | 自己找出這一輪該做什麼 |
| 交接（handoff） | 將任務隔離後交給負責工作的 agent |
| 驗證（verification） | 由另一個 agent 檢查「這樣可以嗎」|
| 持久化（persistence）| 把狀態寫到對話外部（檔案）|
| 排程（scheduling） | 透過 timer，讓它放著也能持續運作 |

**沒有驗證就不算迴圈**——只有生成沒有驗收，那只是不停產出內容的裝置。

### 四個持續運轉的陷阱

| 陷阱 | 症狀 | 防範 |
|------|------|------|
| 漏掉驗證 | 成果在累積，沒人檢查 | 加入獨立於作業角色外的評估角色（Stop hook）|
| 理解退化 | 程式碼越來越多，腦中地圖還停在舊版 | 定期讀輸出，確認自己能說清楚 |
| 放棄判斷 | Loop 一跑起來，人就「算了啦」 | 可以交給系統執行，但不能把判斷一起放掉 |
| token 暴衝 | 使用量劇烈波動，帳單難預測 | 上線前設定預算與最大重試次數上限 |

## C（關鍵行動）

- [ ] 在 CLAUDE.md 加入迴圈協議段落，明確定義停止條件和禁止事項
- [ ] 在 .claude/settings.json 設定 Stop hook（跑完整測試才能宣稱完成）
- [ ] 建立 `.claude/agents/fixer.md`，用於同一個錯誤卡住兩次後的新 context 診斷
- [ ] 設定迴圈最多 5 次上限，超過就報告剩餘問題讓人接手
- [ ] 如果要在自己的框架中實作，評估 Mastra `Goals` 功能（`@mastra/core@1.42.0+`）

### cobusgreyling/loop-engineering — 開源 CLI 工具套件

Cobus Greyling 的開源工具套件（3 支 CLI：`loop-audit` 就緒評分 / `loop-init` 模式鷹架 / `loop-cost` token 估算）與 7 個生產模式（Daily Triage / PR Babysitter / CI Sweeper 等，按 token 成本排序）+ L1→L2→L3 分階段上線，**完整內容見 [[loop-engineering-playbook-cli-tools]]（該篇為此工具鏈與 Tier A/B/C runtime 選型的權威條目）**。本篇聚焦 Claude Code 端的實作（CLAUDE.md 迴圈協議 + Stop hook + fixer sub-agent），與 playbook 的 runtime 選型視角互補。

## 相關連結

- [[loop-engineering-playbook-cli-tools]] — cobusgreyling CLI 工具鏈 + 7 模式 + Tier A/B/C runtime 選型（本篇 CLI 段落的權威來源）

- [[loop-engineering-6-components]] — Loop Engineering 完整框架（概念層）
- [[claude-code-advanced-workflow]] — Claude Code 進階技巧和 /loop 指令
- [[claude-code-team-dev-kit]] — 用 Skill + Hook 把團隊開發規則變成自動遵守的東西
- [[llm-judge-confidence-cascade]] — loop 中 judge 角色的成本與門檻設計（信心升級）
