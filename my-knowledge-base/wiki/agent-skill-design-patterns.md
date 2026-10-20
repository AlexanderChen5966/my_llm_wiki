---
title: Google 5 大 Agent Skill 設計模式
date: 2026-05-13
tags: [ai/agent, ai/claude]
source: https://www.bnext.com.tw/article/90356/ai-agent-skill-is-cool-due
type: article
status: processed
asset_value: 78
---

## T（核心論點）
Google Cloud 整理的 5 大 Skill 設計模式，提供可重複使用的模組化架構——不同場景選對模式，才能讓 AI 代理「聊得清楚、做得準確」。

## B（適用場景）
**適用：** 設計新 Skill 時的架構選型；評估現有 Skill 是否用了正確模式
**不適用：** 已有明確需求的 Skill（直接寫就好，不需要過度設計）

## R（操作步驟）
選擇設計模式的判斷流程：
1. **單一工具型**：Skill 只呼叫一個外部工具/API
2. **流程串接型**：多個步驟依序執行（A→B→C）
3. **路由型**：根據輸入條件選擇不同子流程
   - （2026-10-08 補）路由出口不只「選哪個子流程」：GodsBoy/jev-agent-skill-router 回傳三類——選某 Skill／無需 Skill／需要覆核；OOMOL 建議把請求、候選、結果與不確定性做成面板，可發現描述過於相似、職責交叉或觸發範圍過寬的 Skill（原文建議，未實測）。→ [[jev-implementation-guide]] 社群案例
4. **並行型**：多個子任務同時執行（適合 review 場景）
5. **反思型**：執行後自我評估，不符合則重試

## C（關鍵行動）
- [ ] 先判斷 Skill 的核心模式，再設計架構
- [ ] 軟編排型 Skill 通常結合流程串接 + 並行兩種模式
- [ ] 反思型適合品質要求高的場景（但耗 Token）

## 原始摘要
Google Cloud Tech 整理的 5 大模式：單一工具、流程串接、路由、並行、反思。文章附有每種模式的程式碼範例與適用情境對照表。

## 補充資料（來源：《Skill 檔案怎麼寫？Google 提出 5 大 Agent Skill 設計模式》，數位時代，2026-03-20）

### Tool Wrapper 官方範例程式碼

Tool Wrapper 是最基礎的模式——把特定服務 / 框架的使用規則封裝成獨立 Skill，讓 Agent 在真正需要時才載入。以下為 Google 官方 FastAPI Expert Skill 範例：

```yaml
# skills/api-expert/SKILL.md
---
name: api-expert
description: FastAPI development best practices and conventions. Use when building, reviewing, or debugging FastAPI applications, REST APIs, or Pydantic models.
metadata:
  pattern: tool-wrapper
  domain: fastapi
---

You are an expert in FastAPI development. Apply these conventions to the user's code or question.

## Core Conventions

Load 'references/conventions.md' for the complete list of FastAPI best practices.

## When Reviewing Code
1. Load the conventions reference
2. Check the user's code against each convention
3. For each violation, cite the specific rule and suggest the fix

## When Writing Code
1. Load the conventions reference
2. Follow every convention exactly
3. Add type annotations to all function signatures
```

**Tool Wrapper 的設計要點**：
- `description` 明確寫出適用工具 / 框架名稱，讓 Claude 能精確判斷觸發時機
- 把大量操作說明拆出放進 `references/`，Skill 本體只寫「何時載入哪個參考」
- `metadata.pattern` 和 `metadata.domain` 是 Google 建議的可選欄位，便於團隊管理多個 Skill 時分類

**適用情境**：Agent 需要與多種 API 或特定框架互動、不想把每一種 API 說明塞進單一系統提示詞的場景。與 Sequential Workflow（流程串接）的差異：Tool Wrapper 強調「知識封裝」，Sequential Workflow 強調「步驟編排」。

## 補充資料：第六種模式——自我成長型（Self-Evolution）

來源：《GitHub星數突破4.8萬，Hermes Agent能取代龍蝦嗎？》（天下雜誌，2026-04-13）

### 第 6 種設計模式：自動生成型 Skill（Learning Loop）

Hermes Agent（Nous Research，GitHub 4.8 萬顆星）提出一種新的 Skill 設計模式——Agent 從任務執行中**自動生成 Skill 文件**，無需人工撰寫。

**觸發條件（任一滿足即生成新 Skill）：**
- 工具被調用超過 5 次
- 犯錯後自我修復
- 被用戶糾正
- 採取了不明顯但有效的路徑

**生成的 Skill 格式：** 遵循 agentskills.io 開放標準，理論上可跨平台相容（OpenClaw、Claude Code、Cursor）。⚠️ **補充（2026-08-14，見 [[skill-decision-what-to-automate]]）：理論相容 ≠ 行為一致**——開放格式解決的是 Skill 能不能被「移動」，不保證每個 AI 用相同方式「理解」它；不同工具的安裝位置不同，不同模型對觸發條件與例外的理解也不同，**甚至同一模型在服務商更新後結果就會變**。企業做法是連同「搭配的工具、模型、測試案例」一起記錄，模型更新後重跑驗證。更新策略使用「Patch 補丁」方式，只修改有問題的部分，不重寫整個文件（節省 token 且不破壞有效部分）。

### Hermes 四層記憶系統（Skill 設計的參考框架）

| 層級 | 內容 | 讀取時機 |
|------|------|---------|
| 第 1 層（常駐提示詞）| MEMORY.md + USER.md（上限 3,575 字）| 每次 session 開始 |
| 第 2 層（會話存檔）| SQLite 全文索引對話記錄 | 按需 LLM 總結後注入 |
| 第 3 層（Skill 文件）| 技能名稱+描述常駐，全文按需讀取 | 按需 |
| 第 4 層（Honcho）| 跨 session 被動累積的用戶偏好 | 長期個人助理使用 |

**設計洞察：** 第 3 層的設計（名稱+描述常駐，全文按需）與 Claude Skills 的 Progressive Disclosure 完全一致——即使 Skill 庫從 40 個增加到 200 個，context 成本也幾乎不變。

> ⚠️ **前提注記（2026-09-03）**：上面這句是**架構層的推論**，實務上有一道官方上限——**Skill 列表整體預算 = context window 的 1%**（`skillListingBudgetFraction`）。超過後**冷門 Skill 的 description 會被砍、觸發率無聲下降**（見 [[skill-md-writing-techniques]]，官方查證）。
>
> 🔑 所以正確說法是：「**在總 description 量未撞到 1% 預算的前提下**，context 成本幾乎不隨 Skill 數量成長」。40→200 個是否仍成立，取決於每個 description 的長度，而不是數量本身。

### OpenClaw vs Hermes 的 Skill 設計哲學差異

| 面向 | OpenClaw（靜態配置）| Hermes（Learning Loop）|
|------|------------------|----------------------|
| Skill 來源 | 人工撰寫 | 自動從任務執行中生成 |
| 記憶方式 | 靜態設定檔 | 動態 SQLite + 四層記憶 |
| 失敗學習 | 不自動學習 | 發現更好路徑時自動 Patch 更新 |
| 適用場景 | 工作流清晰、規則穩定 | 長期個人助理、需要自我進化 |

**社群共識（Reddit/YouTube/X）：** 兩者互補——OpenClaw 負責「做事」（多管道互動、生態整合），Hermes 負責「動腦」（持久化記憶、自動生成技能、高層次推理）。常見設定是把 Hermes 當作高級規劃器，在 OpenClaw 工具上運作。

## 相關連結

- [[skill-md-writing-techniques]] — 🔑 **前提來源**：Skill 列表 1% context 預算上限、超限後冷門 Skill 觸發率無聲下降的三種調整手段
- [[claude-skills-guide]] — Axton Liu 的實測版（能力包 + 軟編排）
- [[addyosmani-engineering-skills]] — 工程師角度的 Skill 設計
- [[claude-skills-deep-dive]] — Progressive Disclosure 三層機制（與 Hermes 第 3 層設計呼應）
- [[skillopt-skill-optimization]] — SkillOpt：把 skill 文件當可訓練狀態最佳化
- [[custom-gpt-gems-to-skills-migration]] — 自訂 GPT／Gems 遷移到 Skills 的跨廠商對照（2026-10-08 快照）
