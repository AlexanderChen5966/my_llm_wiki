---
title: SKILL.md 九大類型與 Gotchas 方法論（Anthropic 內部實踐）
date: 2026-06-15
tags: [ai/claude, ai/agent]
source: https://www.anthropic.com/engineering/skill-gotchas
type: article
status: processed
asset_value: 85
---

Anthropic 工程師 Thariq Shihipar 揭露的 SKILL.md 內部實踐：Anthropic 內部已有**數百個** active Skills，九大類型涵蓋完整工程生命週期，而 **Gotchas 段落是整個 SKILL.md 中信號密度最高的內容**。

## T（核心論點）

SKILL.md 的三個核心設計原則：
1. **資料夾 > 單一檔案**：Progressive Disclosure 避免 context 一次全載入
2. **description 是寫給 model，不是給人看**：含觸發詞（如 `babysit`、`audit`），告訴 AI 什麼時候應該載入這個 Skill
3. **Gotchas 段落 = 最高信號內容**：記錄 Claude 不斷踩的坑，累積成不可替代的知識資產

## B（適用場景）

**適用：**
- 設計新 Skill 時決定類型與結構
- 寫 description 時決定要包含哪些觸發詞
- 組建 Gotchas 段落，記錄 AI 的失敗模式

**不適用：**
- 單次任務（SKILL.md 是為重複工作流設計的，見 [[skill-md-writing-techniques]] 的決策五問）

## R（詳細內容）

### 九大 Skill 類型

| 類型 | 用途 | 典型場景 |
|------|------|---------|
| **Library Reference** | 特定 Library 使用方法與最佳實踐 | 「用我的公司內部 UI 組件庫建 Button」 |
| **Product Verification** | 驗證產品行為是否符合預期 | PR 前的功能正確性檢查 |
| **Data Analytics** | 資料分析流程標準化 | SQL 查詢 + 視覺化報表 |
| **Business Automation** | 業務流程自動化 | 合約審查、報表生成 |
| **Code Scaffolding** | 新功能/模組的骨架生成 | 「建一個新的 API endpoint」 |
| **Code Quality** | 程式碼品質審查 | lint、安全性掃描、效能評估 |
| **CI-CD** | 持續整合/部署流程 | 部署腳本、環境配置 |
| **Runbooks** | 操作手冊自動化 | 事故響應、例行維護 |
| **Infra Ops** | 基礎設施操作 | 資源配置、監控設定 |

### Gotchas 段落設計原則

Gotchas 不是「怎麼做的說明」，而是「AI 不做提醒就會出錯的地方」：

```markdown
## Gotchas

- 呼叫 deploy 前必須先跑 `npm run build:prod`，不是 `build`（少 `:prod` 會部署 dev bundle）
- 這個 API 的分頁從 page=0 開始，不是 page=1（Swagger 文件錯的）
- 環境變數 FOO_BAR 在 staging 和 prod 的格式不同（prod 有前綴 `arn:`）
```

每個 Gotcha 應包含：
1. 錯誤的做法（或易踩的坑）
2. 正確的做法
3. 為什麼容易搞錯（若非顯而易見）

**Gotchas 是 SKILL.md 中隨時間自然成長的部分**：每次 Claude 出錯、被人類糾正後，把那個錯誤加進 Gotchas。三個月後，你的 Gotchas 段落就是比任何文件都更精準的「AI 容易犯的錯誤清單」。

### 資料夾結構（Progressive Disclosure）

```
skills/deploy/
  SKILL.md          ← description + 簡要流程（永遠載入）
  references/
    environment-vars.md   ← 詳細環境變數（按需載入）
    rollback-procedure.md ← 回滾手冊（按需載入）
```

description 中指定 `context:` 欄位控制哪些子檔案會被載入。

### On-Demand Hooks

- `/careful`：阻擋破壞性指令（如 `DROP TABLE`、`rm -rf`），強制要求人工確認
- `/freeze`：限制 Claude 只能修改特定資料夾（防止跨模組誤改）

### Description 觸發詞設計

```yaml
description: |
  Use this skill to babysit the staging deployment process, audit 
  environment configs, or troubleshoot failed deploys.
  Do NOT use for local development tasks.
```

`babysit`、`audit`、`troubleshoot` 這類詞是觸發詞（告訴 AI 何時應該主動建議載入這個 Skill）。

### 從小開始的成長模型

```
初始：幾行說明 + 1 個 Gotcha
三個月後：完整流程 + 10+ Gotchas + 子文件架構
```

不要一開始就寫完整，讓 Skill 隨實際使用積累成長。

## C（關鍵行動）

- [ ] 盤點現有 Skills，對照九大類型分類
- [ ] 每次 Claude 出錯就加一條 Gotcha（立即行動，不要等）
- [ ] 檢查現有 SKILL.md 的 description 是否包含觸發詞

## 相關連結

- [[skill-md-writing-techniques]] — SKILL.md 技術規格：frontmatter / ≤500行 / Google 5 大模式 / Claude eval 方法
- [[loop-engineering-6-components]] — Loop Engineering 中 Skills 組件的角色
- [[claude-agents-skill-file-comparison]] — SKILL.md vs CLAUDE.md vs AGENTS.md 三檔分層
- [[agent-skill-practical-guide]] — Skills 的描述觸發機制實戰
