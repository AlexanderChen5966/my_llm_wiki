---
title: AI Agent 不能交辦的 6 件事：權限矩陣 + AGENTS.md 模板
date: 2026-06-15
tags: [ai/agent]
source: https://codelove.tw/@tony/post/xXQ5W3
type: article
status: processed
asset_value: 70
---

AI Agent 確實按指示執行，卻可能造成災難——不是因為 AI 不聽話，而是「凡是無法輕易復原的操作，一律要設人類檢查點」這個原則沒有落實。

## T（核心論點）

**判斷哪些任務應該交給 AI，是與 Agent 合作最重要的技能。**

真實事故：「整理 repo 並刪除未使用的檔案」→ Agent 把 `.gitignore` 管控外的部署設定目錄也刪了→ 一小時找回備份。

簡單判斷規則：**無法輕易復原 → 必須人工確認**

## B（適用場景）

設計 Agent 工作流時的邊界劃定，以及為專案建立 AGENTS.md。

## R（完整內容）

### 6 件絕對不能交給 AI Agent 的事

**1. 破壞性的檔案操作**
- `rm -rf`、`git clean -fd`、`git reset --hard`
- 風險：Agent 看到「clean up」就可能刪掉你沒打算刪的東西

**2. 資料庫寫入/刪除**
- 沒有 `WHERE` 條件的 `DELETE`、`DROP TABLE`、`TRUNCATE`、正式環境 migration
- 風險：`WHERE` 條件打一個字錯，整張表被刪

**3. 基礎設施（雲端）**
- `terraform apply`、`kubectl delete`、`aws iam *`、`gcloud iam *`
- 風險：權限變更悄悄發生，等到某個東西壞掉才被發現

**4. 正式環境部署**
- 決定是否部署永遠由你做，不是 Agent
- 風險：Agent 不知道是否有進行中的事故或維護排程

**5. 身分驗證/安全性邏輯**
- 驗證流程、授權規則、token 處理、session 管理
- 風險：這類 bug 不出現在單元測試，只在事件報告中暴露（可能是數個月後）

**6. Secrets、.env 檔、API 金鑰**
- 不讓 Agent 讀取或寫入

### 權限矩陣

| 任務 | Agent 自主度 | 人工審查 | 原因 |
|------|------------|---------|------|
| 小型函式重構 | 高 | 不需要 | 容易 revert，風險低 |
| 新增單元測試 | 高 | 通常不需要 | CI 會立即偵測 |
| API 行為變更 | 中 | 需要 | 影響客戶端契約 |
| 新增相依套件 | 中 | 需要 | 安全漏洞/授權風險 |
| 資料庫 migration | 低 | 必須 | 錯誤可能造成資料遺失 |
| 身分驗證/安全性 | 低 | 必須 | Bug 難以在測試中發現 |
| 基礎設施/雲端 | 非常低 | 必須 | 權限設定錯誤難追查 |
| 正式環境部署 | 無 | 必須+手動確認 | Agent 不知道當前維護狀態 |

### AGENTS.md 模板

放在 repo 根目錄，告訴所有 Agent「哪些不能碰」：

```markdown
# AGENTS.md

## Project
[專案簡介與技術堆疊]

## Setup
```bash
npm install
npm run dev
npm test
npm run lint
```

## Coding rules
- 只做最小必要變更，不要重構無關程式碼
- 如果改變了行為，就新增或更新測試
- 不要碰超出任務範圍的檔案
- 1 個 commit 只處理 1 件事

## Safety rules
執行 blocked_commands.md 中的指令前，一律先請求確認。
如果不確定是否安全 → 停下來確認。

## Definition of done
- 測試通過
- 差異能用一句話說明
- 有最終報告

## Final report format
1. 變更概要
2. 變更的檔案
3. 執行的測試與結果
4. 風險或前提條件
5. 尚未完成的事項
```

### blocked_commands.md

另存獨立檔案，執行前需批准的指令清單：

```markdown
## 破壞性的檔案操作
- rm -rf / git clean -fd / git reset --hard

## Git 操作
- git push --force / git push --force-with-lease

## 資料庫操作
- DROP TABLE / TRUNCATE TABLE / 沒有 WHERE 的 DELETE
- 會修改正式環境 schema 的 migration

## 雲端/基礎設施
- terraform apply / kubectl delete / aws iam * / gcloud iam *

## Secrets
- 讀寫 .env 檔的指令
- 接觸 API 金鑰或認證資訊的指令
```

### Two-Agent Loop

中等以上複雜度任務，用 2 個 Agent：

**Agent 1（Implementer）：**
「你是資深工程師。只做最小必要變更。如果改變行為就新增測試。完成後提供最終報告（概要/變更檔案/測試/風險/未完成事項）。」

**Agent 2（Reviewer）：**
「你是程式碼審查者，對實作沒有情感依附。
審查這個 diff。檢查：bug 與邊界案例、缺少的測試、安全性問題、非預期的行為變更、超出範圍的內容。
輸出：Critical issues（必須修正）/ Minor issues（可選）/ 提醒人類注意的事項。
**不要重寫程式碼。只標記問題。**」

Reviewer 對程式碼沒有「自我意識」，能找出 Implementer 漏掉的 bug。

## C（關鍵行動）

- [ ] 在所有專案 repo 建立 `AGENTS.md` 和 `blocked_commands.md`
- [ ] 定義 Agent 自主度等級：每類任務的審查規則
- [ ] 重構或安全相關任務啟用 Two-Agent Loop

## 相關連結

- [[agents-md-guide]] — AGENTS.md 完整指南（效果研究 +4% vs -3%）
- [[ai-agent-architecture-patterns]] — AI Agent 架構模式（安全攻擊防禦）
- [[agentic-engineering-sop]] — Agentic Engineering SOP（三層安全網）
- [[claude-code-12-rules-error-reduction]] — CLAUDE.md 12 條規則（錯誤率 41% → 3%）
- [[claude-code-agents-md-loading-rules]] — Claude Code 2.1.277 起才會自動讀 `AGENTS.md`（且有 `CLAUDE.md` 時不讀），放規則檔前先確認判定表
