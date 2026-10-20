---
title: LoopX：長時間 Agent 的持久狀態層，Evidence 與 Gate 才是重點
date: 2026-08-13
tags: [ai/agent, ai/workflow]
source: https://tools.wingzero.tw/articles/4022（赤紅翼零，2026-08-13）／專案 https://github.com/huangruiteng/loopx
type: article
status: processed
asset_value: 62
---

## T（核心論點）

> **LoopX 解決的不是「怎麼讓 AI Agent 更聰明」，而是「當 Agent 已經能工作之後，怎麼讓它持續工作而不失去目標、證據與控制」。**

官方定位是 **provider-neutral、stateful control plane**（輕量的 loop engineering state kernel），並明確聲明**它不是另一套 Agent Framework、也不是綁定特定供應商的 Orchestration Runtime**。真正執行工作的仍是 Codex、Claude Code、Cursor 或 Shell Agent。

它與 `while true` / Cron / 定時 Prompt 的分野是本篇最鋒利的一段：

> **定時器可以回答「什麼時候再叫醒 Agent」，卻無法回答「現在還有沒有值得做的事」、「證據是否足夠」、「是否必須等待人類批准」、「下一個 Agent 是否有權限接手」。**

## B（適用場景）

**適用：**
- 任務已跨越數小時、數天、多個 PR 或多輪實驗，單純依賴模型 Context 的限制越來越明顯
- 需要管理多 Agent 團隊的權限與接手順序
- Issue／PR loop（分析 → 改碼 → 測試 → 開 PR → 等 Reviewer → 隔天依意見修改）
- Research／Benchmark／Experiment 的長期迭代

**不適用 / 注意：**
- 🔴 **不是一般聊天機器人使用者需要安裝的工具**；也不宜視為 AutoGen／LangGraph 的替代品（那些處理 Agent 如何組合、節點如何流動、工具如何呼叫；LoopX 專注 durable state／governance／evidence／handoff）
- 🔴 **官方公開的「200 小時以上」要正確解讀**：OpenViking Contribution Sequence 與 Auto ML Showcase 指的是**專案從開始到後續 Review／更新所跨越的真實時間（elapsed loop lifetime）**，不是模型連續執行 200 小時，也不宣稱能完全無人監督地控制正式生產環境
- 🔴 官方明確表示 **Dangerous Permissions、Publishing、Production Writes 與 Final Ownership 仍應保留在人類手上**——它不把「完全自主」當成唯一目標
- ⚠️ 本條目來源為中文介紹文＋官方 README 定位敘述，未實測；資產值保守估計

## R（六種持久狀態與運作方式）

持久狀態層保存六類資訊：**Objective / Gates / Todos / Scope / Evidence / Quota**。

運作模式的關鍵在**有邊界**：

```
Agent 執行一個 bounded agent slice（不是「一直跑到不知道什麼時候停」）
→ 完成後寫回 Evidence、Handoff 資訊與下一個 Todo
→ 由 Quota 與目前狀態判斷下一次是否應該繼續
```

八項主要設計：

- 保存 Objective／Gate／Todo／Scope／Evidence／Quota 等長期狀態
- 支援 Codex、Claude Code、Cursor 與自訂 Agent Runtime
- 每次執行 **bounded turn**，而非無限制持續運作
- 保留 **Evidence Log**，讓後續 Agent 能確認前一步到底完成了什麼
- 支援 **Handoff**，讓不同 Agent 或工作階段可以接手
- 透過 **Claim、Lease 與 Task Boundary** 管理多 Agent 工作權限
- 遇到需要人類判斷的 **Gate 時停止並詢問**，而不是自行越權決策
- 適合 Engineering／Research／Benchmark／Experiment／Operations 等長期 Loop

### Evidence 是理解這套設計的關鍵

一般 Agent 很容易留下「我已經修好了」、「測試通過」這類自然語言描述，但長期任務需要的是**可以回頭檢查的證據**。

🔑 **Evidence 納入持久狀態的意義：後續工作根據目前證據、Todo 與 Gate 決定下一步，而不是完全相信上一輪 Agent 留下的一段摘要。** 這與 [[agent-context-management]] 的「壓錯東西比 Overflow 更危險」是同一個擔憂的兩種解法——那邊靠 Protected Fields，這邊靠不可竄改的證據記錄。

### Quota-aware：排程器還在跑，但已經沒有值得做的事

長時間 Agent 最大的風險之一，是**排程器正常運作但 Agent 已經沒有真正有價值的工作可做，結果只是持續消耗 Token 與運算資源**。LoopX 把 Quota 與下一次 Wake 納入控制狀態，讓系統根據**工作進展**決定是否繼續。

### Agent-native Kanban 與 Peer 模型

官方的比喻：每項工作是 Kanban 上一張卡片，但卡片除了 Todo 還帶 **Identity、Authority、Evidence、Continuation**；Claim／Gate／Monitor／Writeback 等操作負責改變狀態。⚠️ **看得到的 Board 只是狀態投影，真正的 Source of Truth 是 LoopX 保存的資料。**

多 Agent 情境把已註冊的 Agent 視為 **Peers**，**不要求永遠存在一個固定的 Leader Agent**——誰可處理、目前由誰持有、下一步交給誰，由 Claims、Leases、Capabilities、Task Boundaries 與 Typed Continuation 決定。🔑 **比較像在建立 Agent 團隊的工作制度，而不是讓幾個模型互相聊天。**

## C（關鍵行動）

- [ ] 評估自己的長任務時先問那四個定時器答不出來的問題（還有沒有值得做的事／證據夠不夠／要不要等人／下一個有沒有權限）
- [ ] 檢查現行做法是否只留「測試通過」這種自然語言宣稱，而沒有可回頭檢查的 Evidence
- [ ] 引用任何「長時間自主運行」數字前先確認是 elapsed lifetime 還是連續執行時間

## 補充資料

### 為什麼多日工程特別需要這一層

範例流程：第一輪分析問題 → 第二輪改碼 → 跑測試 → 建 PR → 等 Reviewer。隔天 Reviewer 要求修改後，Agent 需要理解前一天的 Scope、測試結果與決策，**而不是重新閱讀整個 Repository 後猜測當時為什麼這樣改**。

需求可能改變、測試證據會過期、PR 會收到意見、某些操作必須等 Owner 批准——🔑 **只依靠聊天 Context 加上一個定時器，很難可靠管理這種工作。**

### Auto Research Demo

官方提供 KNN Demo，讓 proposer、executor、evaluator/promoter 等 Agent 平行迭代，同時保存 Todo、Quota、Evidence 與 **Targeted Wake**。原文的觀察值得記：這類流程若只靠聊天紀錄，很快就會累積大量難以判讀的 Context；改成明確記錄**假設、結果與下一步**，比較接近真正的研究工作流。

### Gate 的行為規範

碰到需要 Owner、Safety、Publication 或 Private-data Decision 的 Gate 時，Agent 應**提出具體問題並等待決策**——不是自己決定，也不是含糊地跳過。（與 [[agent-control-layer-permission-hooks-planning]] 的「Prompt 不是安全邊界」一致：Gate 是狀態機層的硬停止，不是提示詞層的請求）

## 相關連結

- [[loop-engineering-playbook-cli-tools]] — ⚠️ **最接近的對照**：那篇問「迴圈該住在哪個 runtime」（終端工具 vs 生產級 runtime，判準是跑多久／服務誰／能碰什麼），LoopX 則是**不換 runtime、在外面加一層狀態**的第三條路；那篇列的 runtime 需求（凌晨無人也要跑、mid-run 崩潰要存活、暫停等審核三天）正是 LoopX 的 Quota／Evidence／Gate 各自對應的問題
- [[loop-engineering-6-components]] — Loop 的六個元件（概念層）
- [[agent-loop-explained]] — Loop 概念總覽與四層階梯（Prompt→Context→Harness→Loop）
- [[agent-loop-stop-conditions]] — 同一問題的最小實作版：`MAX_TURNS`／連續失敗／停在日記完整處；LoopX 的 bounded turn 是它的制度化版本
- [[claude-code-session-handoff]] — Handoff 的產品面（`/compact`／`/rewind`／`/resume` 三個共同限制，其中「跨不出這台機器」正是 LoopX 想補的）
- [[concepts/agent-handoff-memory-patterns]] — 跨 Session 與跨工具狀態傳遞的概念地圖
- [[agent-control-layer-permission-hooks-planning]] — Capability vs Permission、「列出步驟不等於擁有計畫」須有 Execution State
- [[subagent-four-boundaries]] — 多 Agent 的邊界與 Result Contract
- [[agent-session-management]] — Session 狀態管理
