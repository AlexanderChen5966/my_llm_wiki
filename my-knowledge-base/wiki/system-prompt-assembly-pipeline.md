---
title: System Prompt 是每輪組裝的 Runtime Artifact：六層來源與 Priority
date: 2026-08-13
tags: [ai/agent, ai/prompt-engineering]
source: https://ithelp.ithome.com.tw/articles/10401899（《30 天從零拆解 AI Agent》Day 11）
type: article
status: processed
asset_value: 78
---

同系列前作：Day 08 見 [[agent-skills-vs-mcp]]、Day 09 見 [[agent-context-management]]、Day 10 見 [[agent-memory-lifecycle]]。本篇是這條線的收束——前三天各自建立的 Skills、Context、Memory，今天要決定它們**怎麼進入模型**。

## T（核心論點）

> **System Prompt 不是一份文件，而是 Harness 每一輪根據系統狀態組裝出來的執行介面（Runtime Artifact）。**

最難的地方不是寫一句更厲害的 Instruction，而是決定：**這一輪模型到底應該看到哪些規則、能力與狀態？**

這也把 [[agent-context-management]] 的原則推到 System Prompt 身上：**Context 是工作區，不是倉庫**——System Prompt 同樣不該變成「所有可能有用資訊的永久垃圾桶」。目標不是最大化資訊量，而是「在這一輪提供足夠而一致的控制資訊」。

## B（適用場景）

**適用：**
- 自建 Agent 已經同時有 Memory / Skills / Permission / Tools / Plan / State，開始出現「不知道模型這輪看到什麼」的 Debug 困境
- 一份巨大 System Prompt 越改越長、同一條規則在多處重複時的重構依據
- 對照理解 Claude Code 為何把 CLAUDE.md、Skill、Hook、Permission 分開放（見 [[claude-code-harness-internals]]）

**不適用 / 注意：**
- 單純 Chatbot 不需要——固定 System Prompt 就夠，Assembly 是長時間執行系統的需求
- 🔴 **Prompt Policy ≠ Runtime Policy**：Prompt 裡的 Policy 只是「告訴模型邊界在哪」，真正的執行限制必須存在 Permission、Approval、Sandbox 裡。兩者應一致但責任不同（同 [[agent-control-layer-permission-hooks-planning]] 的「Prompt 不是安全邊界」）
- 原文的 Priority 排序作者自己聲明「不是唯一答案，但系統一定要有答案」

## R（六層來源與組裝順序）

```
Policy → Role → Current Task → Current Capability
→ Relevant Skills → Relevant Memory → Runtime State → Recent Context → Model Call
```

每一層有各自的來源與生命週期，這張表是本篇最可操作的產出：

| Section | Source | Lifetime |
|---|---|---|
| Policy | System | Long-lived |
| Role | Agent Config | Long-lived |
| Task | Task State | Task |
| Tools | Registry + Permission | Dynamic |
| Skills | Skill Router | Dynamic |
| Memory | Memory Recall | Dynamic |
| Runtime State | Harness | Per turn |
| Recent Context | Context Manager | Per turn |

六層各自的判準：

1. **Policy** — 不可被任務內容覆蓋的規則（不自行提升權限、高風險操作需 Approval、不洩漏 Secret、必須留 Audit）。特性是穩定、高優先級、最好有 Harness Enforcement
2. **Identity and Role** — 「這個 Agent 在系統中是誰」。⚠️ 不要把 Task-specific 行為寫進 Role（「每次都要先搜尋 20 個檔案」是 Skill 不是 Identity），Identity 越膨脹越會變成另一個巨大 Prompt
3. **Task** — 含 Goal / Scope / Constraint / Expected Output / Completion Condition。**這比完整 Conversation History 更重要**——若 Context Manager 已整理出 Task State，就放「目前有效的 Task Representation」而不是整段聊天紀錄
4. **Capability and Permission** — Tool Registry 有 30 個工具 ≠ 給模型 30 個。先過 Task 需要 → 再過 Permission 允許 → 真正暴露 3 個。同時改善 Tool Selection / Token / Security / Failure Rate
5. **Skills and Memory** — 兩者都**不該永久存在**於 System Prompt，應動態載入（Skill 回答「這類任務怎麼做」、Memory 回答「過去哪些資訊該影響現在」）。任務從寫貼文換成修 Kubernetes，這些內容就不該繼續佔 Context
6. **Runtime State** — 最容易被忽略的一層：Current Plan / Step / Remaining Budget / Pending Approval / Changed Files / Last Failure / Environment。更新極快，寫死在固定 Prompt 裡一定過期

### 三個關鍵機制

**Priority 必須明確。** 當 Policy 說「不可部署 Production」、Memory 說「這使用者以前常要求自動部署」、User 說「只要 Plan 不要真部署」、Skill 說「測完可以 Deploy」——沒有明確排序模型只能自己猜。原文的一種排法：

```
System Policy > Current User Constraint > Current Task State
> Runtime Permission > Skill Guidance > Relevant Memory > General Preference
```

**一條規則只保留一個 Source of Truth。** 「Deploy 前要 Approval」同時寫在 System Prompt、Skill、Tool Description、Memory、Runtime 看起來很安全，但 Runtime 改成「Staging 免審、Production 才要」時，Skill 還是舊規則，模型就會看到互相衝突的 Instruction。正確做法是 Permission Engine 決定，Prompt Assembly 只注入結果。

**Prompt Provenance。** 每個組裝出的 Section 保留來源與版本（Section: user_preferences / Source: memory / Memory IDs: mem_102, mem_188）。不一定給模型看，但 Harness 要留 Trace——否則出錯時無法分辨是 Skill 選錯、Memory Recall 錯、Permission 過期、Task Summary 錯，還是模型判斷錯。（與 [[agent-skills-vs-mcp]] 的「Loading 必須是可追蹤事件」同一要求）

### Static Prefix / Dynamic Suffix

把 Prompt 切兩段：Static Prefix（Policy / Role / Core Formatting）與 Dynamic Suffix（Task / Tools / Permission / Skills / Memory / Runtime State）。好處是易 Cache、易比較變化、易 Debug。⚠️ 前提是**真的穩定的內容才放 Static**——不要為了 Cache 把會變的資訊錯誤固定。

### Trust Boundary

Assembly Pipeline 必須保留來源身份：System Policy（Trusted）／User Input（User-controlled）／**Tool Result（Untrusted external content）**／Skill（Trusted internal）。外部 Tool Result 裡的「Ignore previous instructions…」不能被錯誤提升到 System Section——這是 Prompt Injection Defense 的基礎之一（見 [[prompt-injection-vs-jailbreak]] 的「不可信 Source × 高權限 Sink」判準）。

## C（關鍵行動）

- [ ] 檢查自己的 Agent 設定：有沒有同一條規則寫在三個以上地方？先找出誰是 Source of Truth
- [ ] Prompt 中描述的 Capability 是否等於 Runtime 真正允許的？不一致時模型會反覆嘗試永遠不可能成功的行動
- [ ] Runtime State 有沒有被寫死？（Current Step / Remaining Budget / Last Failure 這類）

## 補充資料

### Prompt Assembly 是可測試的

測的不是模型輸出，而是「**我們有沒有給模型正確的工作環境**」。原文的 Test Case 形式：

- Task「Review PR 不要改碼」→ Expected：read tools、無 write tools、Review Skill、read-only Permission、無 deployment Skill
- Task「部署 Staging」→ Expected：Deployment Skill/Tool、Staging Permission、無 Production Approval 要求
- Task「寫 LinkedIn 貼文」→ Expected：Writing Skill、user preference Memory、無 code tools、無 unrelated project facts

### 八個評估指標

Prompt Size / Relevant Section Rate（載入的 Section 有多少真被使用）/ Unused Skill Rate / Tool Overexposure / Memory Over-recall / Conflict Rate / Assembly Latency / **Success per Context Token**（增加一段 Context 後成功率提升是否值得成本）。

### Context Budget 的三級壓縮

超過 Budget 時不能說「全部都重要」：

- 幾乎不能刪：Policy、Current User Constraint、Permission、Current Task Goal
- 可以縮：Skill Examples、Memory Details、Tool Descriptions、Old Plan History
- 可以移出：過時 Tool、不相關 Skill、Low-confidence Memory、Completed History

⚠️ **Tool Schema 本身也佔 Context**，所以 Tool Selection 同時是安全問題與 Context Optimization 問題。

### System / User / Tool 三種 Message Role 的分工

System 放 Policy、Role、Stable Instructions、Current Capability、Control Information；User 放使用者需求與提供的內容；Tool 放 Tool Result、Observation、Runtime Evidence。什麼都塞進 System，模型無法區分「這是永遠的規則，還是某一次工具回傳」——**Message Role 本身也是 Context Architecture**。

### 八個常見錯誤設計

一份巨大 System Prompt 管所有任務 / 所有 Tools 永遠暴露 / Skill 與 Memory 永久存在（失去 Progressive Disclosure 價值）/ Runtime State 寫死 / 同一規則複製到多個 Section / 沒有 Prompt Provenance / Context Budget 不分 Priority / 外部內容被提升成高權限 Instruction。

### 系列後續

Day 12 進入 Error Recovery：什麼情況該 Retry、什麼情況該換策略、什麼情況應該直接停止。完整系列與範例：https://github.com/hardness1020/awesome-agent-architecture（見 [[awesome-agent-architecture-from-scratch]]）

## 相關連結
- [[synthesis/synthesis-claude-code-internals]] / [[synthesis/synthesis-prompt-engineering-evolution]] — **本篇為兩份精粹的來源**（2026-09-03 重跑）：🔑「System Prompt 是每輪組裝的 Runtime Artifact」同時是無狀態推論鏈的上一層、與提示詞演化的終點
- [[system-prompts-leaks-repo]] — 主流 AI 產品系統提示詞蒐集庫，可用來對照本篇的組裝管線在真實產品裡長什麼樣
- [[agent-context-management]] — 同系列 Day 09，四層 Context 機制
- [[agent-memory-lifecycle]] — 同系列 Day 10，Memory 生命週期
- [[agent-skills-vs-mcp]] — 同系列 Day 08，Progressive Disclosure 四層
- [[agent-control-layer-permission-hooks-planning]] — Capability ≠ Permission、Prompt 不是安全邊界
- [[agent-era-prompt-structure]] — Agent 時代提示詞結構
- [[prompt-injection-vs-jailbreak]] — Trust Boundary 的風險判準
- [[agent-tool-runtime-seven-layers]] — Tool Call 的七層責任分工
