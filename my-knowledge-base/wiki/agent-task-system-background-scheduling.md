---
title: 長任務基礎設施：Task System、Background、Scheduling 與 Worktree
date: 2026-08-17
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10402414 ／ 10402415 ／ 10402416（《30 天從零拆解 AI Agent》Day 13／14／15）
source_2: https://ithelp.ithome.com.tw/articles/10402418（同系列 Day 16：Worktree 與 Workspace Isolation）
type: article
status: processed
asset_value: 84
---

四天合併（Day 13–16）。同系列前作：Day 08 見 [[agent-skills-vs-mcp]]、Day 09 見 [[agent-context-management]]、Day 10 見 [[agent-memory-lifecycle]]、Day 11 見 [[system-prompt-assembly-pipeline]]、**Day 12 見 [[agent-error-recovery-taxonomy]]**——當 Agent 有了 Retry／Fallback／Approval／Replan／Human Handoff 之後，「任務還能只存在記憶體裡嗎」就成了必答題。

🔑 **本條目與 [[loopx-stateful-control-plane]] 是同一命題的理論版與產品版**：那邊是一個現成的 stateful control plane，這邊是它背後每一個機制的設計理由。

## T（核心論點）

三個層層遞進的「不是等號」，是本篇最核心的內容：

> **① `task_id` ≠ Task System** — task_id 只是在辨識任務，不代表系統真的能暫停、恢復、追蹤與恢復。
> **② Background Thread ≠ Background Execution** — Thread 只把工作移出目前 Call Stack。
> **③ Cron ≠ Scheduling** — Scheduling 不只是「什麼時候跑」，而是「**某個 Task 在未來什麼條件下，應該被可靠建立或喚醒**」。

而貫穿三天的分工原則仍是同一句：

> **Model 決定下一步，Harness 決定狀態是否真的可以轉移。**

## B（適用場景）

**適用：**
- Agent 需要等待 Approval／外部 API／Subagent，或跨數小時執行
- 任務會跑 20–30 分鐘，不能讓 HTTP Request 一直掛著
- 需要「每天整理 Inbox」「每週一檢查依賴更新」這類週期任務

**不適用 / 注意：**
- ⚠️ Agent 只跑 5 秒的場景不需要這一整套
- 🔴 **不要期待 Queue 幫你提供 Exactly-once Side Effect**（見下方 At-least-once）

## R（Day 13：Task System 的八件事）

`Task State / Persistence / Checkpoint / Resume / Cancellation / Ownership / Idempotency / Audit`

### Task 不是 Conversation

Conversation 是訊息歷史；**Task 是一個需要被完成的工作單位**。一個 Task 可能跨越多個 Conversation Turn，**也可能完全沒有使用者互動**。典型生命週期：

```
Created → Running → Waiting for Subagent → Running → Waiting for Approval → …
```

### 🔑 Task State 不是模型自己說了算

模型可以輸出「我已經完成」，**但 Task System 不能直接把 State 改成 Completed**。應先檢查：Completion Condition／Verification／Required Artifacts／Approval State／**Pending Side Effects**／Remaining Subtasks。

（這與 [[ai-era-quality-assurance-test-oracle]] 的「不要只把實作 Agent 自己說的『我成功了』當成 Quality Gate」是同一條原則的不同層次）

### Persistence：任務不能只活在 Process Memory

**服務重啟時，正在等待 Approval 的任務不應該消失。** 最少要持久化：`task_id`／`status`／`goal`／`current_step`／`plan`／`pending_action`／`tool_execution_history`／`approval_state`／`budget`／`checkpoint`／`created_at`／`updated_at`。

🔑 **這些資料讓 Agent 不依賴某一個 Python Process——否則任何 Deploy、Crash 或 Scale-out 都可能直接讓任務遺失。**

### Checkpoint 回答「如果現在停掉，下次從哪裡繼續」

好的 Checkpoint **不需要保存所有 Runtime 物件**，只要足夠重建狀態：

```
Current Step: 等待 Production Deploy Approval
Pending Tool: deploy_service (service=payments, environment=production, version=v1.8.2)
Verification Completed: tests passed, build passed
Next Action: resume deploy after approval
```

### 🔴 Resume 不是重新跑一次

> **如果 Agent 在付款 API 後 Timeout，Resume 時直接從頭執行，可能造成重複付款。**

所以 Resume 需要先知道：哪些 Step 已完成／哪些 Tool 已有 Side Effect／哪些結果已確認／**哪些 Operation 仍不確定**。

🔑 **這就是為什麼 Task System 和 Idempotency 必須一起設計**（Idempotency Key 與 Execution Record 見 [[agent-error-recovery-taxonomy]]）。

### Cancellation 是一種狀態轉移，也可能需要 Cleanup

使用者說「停止這個任務」時，系統不能只是停止下一次模型呼叫，還要考慮：**已經執行的 Side Effect 怎麼辦**／Background Tool 還在跑嗎／Subagent 要不要一起 Cancel／已建立的暫存資源要不要清掉／已排程的後續任務要不要取消。

## R（Day 14：Background Execution）

長任務直接在 Request Handler 裡跑會遇到：Request Timeout／Process Restart／Client Disconnect／Worker 被佔滿／無法 Retry／無法 Resume／難以 Scale。

基本架構把責任切開——**API 的責任是建立任務，Worker 的責任是執行任務**，如此 Client 不需保持 Connection：

```
API → Create Task → Enqueue Job → Background Worker
    → Load Task State → Run Agent Step → Persist Checkpoint → Continue/Pause/Complete
```

### 🔑 Worker 不應該一次持有整個 Task

長任務要切成可 Checkpoint 的 Step（`Load → Run one agent turn → Persist → Decide next state → Re-enqueue if needed`），這樣 **Worker Crash 時只損失一小段工作**。

> **如果一個 Worker 連續跑 40 分鐘都不存狀態，那仍然不是真正 Durable。**

### Queue 不只是「排隊」

它建立 Producer 與 Worker 之間的邊界（Producer 快速接受、Worker 依能力慢慢處理），並支援 Retry／Priority／Delay／Dead Letter／**Backpressure**／Concurrency Limit。⚠️ **對 Agent 特別重要，因為不同 Task 的成本差異很大。**

### Lease：避免同一個 Task 被跑兩次

多個 Worker 同時看到同一個 Task 時需要 Lease 或 Lock（例：worker_A 取得 task_123 lease，有效 60 秒）。Worker 要定期 **Heartbeat**；**Worker 掛掉後 Lease Expire，其他 Worker 才能接手**。

### 🔴 At-least-once Delivery：Worker 必須假設同一個 Job 會被執行兩次

很多 Queue 只保證「至少送達一次」，不保證只送一次。因此需要：Tool Call 帶 `operation_id`／State Transition 檢查版本／**Completed Task 不應再次執行**／Side Effect 要能 Deduplicate。

### Backpressure 是重要的 Production Control

一瞬間進來 10,000 個 Agent Task 時不該全部同時跑，因為每個 Task 都會消耗 Model Tokens／API Rate Limit／CPU／Browser／Database／External Tool Quota。可配合 Worker Concurrency／Per-user Limit／Per-tool Limit／Priority／Budget。

## R（Day 15：Scheduling）

Cron 很重要，但真正的 Agent Scheduling 還要處理：**Timezone／Missed Run／Duplicate Run／Concurrency／Retry／Catch-up／Task Ownership／Dynamic Schedule／Cancellation／Execution Budget**。

### 🔑 Schedule 和 Task 要分開

- **Schedule** 描述「什麼時候建立或喚醒 Task」
- **Task** 描述「這次執行目前進行到哪裡」

```
Schedule: Every Monday 09:00
  Task Run #1: Completed
  Task Run #2: Failed
  Task Run #3: Running
```

> **一個 Schedule 可以產生很多 Task Run——不要把所有歷史都塞在同一個 Task 裡。**

### One-shot 與 Recurring

One-shot 只執行一次（「兩小時後提醒我重新檢查」）；Recurring 週期執行（「每天早上整理市場新聞」）。⚠️ **Recurring 需要另外處理**：上一次何時跑／下一次何時跑／**前一次還沒完成怎麼辦**／失敗要不要補跑。


## R（Day 16：Worktree —— 平行 Coding Agent 的 Workspace Isolation）

當 Agent 可以 Background、Recurring、Parallel 執行後，下一個實際問題是「**多個 Task 可以同時修改同一個 Repository 嗎**」。所有 Agent 共用同一個 git checkout 在 Demo 很方便，進 Production 就會出現：Agent A 改到 Agent B 的檔案／Test 結果互相污染／Uncommitted Change 混在一起／Branch 切換互相覆蓋／Cleanup 不知道該刪誰／**Verification 無法對應正確版本**。

### 🔴 第四個「不是等號」：Branch ≠ Isolation

> **每個 Agent 用不同 Branch 並不等於隔離——如果它們仍共用同一個 Working Directory，Branch 切換本身就會互相干擾。**

真正需要的是三件事都不同：**不同 Branch ＋ 不同 Working Tree ＋ 不同 Runtime State**。所以是「一個 Task 一個 Worktree」，各自擁有 Branch／Filesystem／Uncommitted Changes／Test State／Build Artifact。

### Isolation 解決的四類問題

- **Filesystem Isolation** — 每個 Agent 看到自己的修改
- **Version Isolation** — 每個 Task 綁定明確 Base Commit
- **Verification Isolation** — Test 是針對自己的 Working Tree
- **Cleanup Isolation** — Task 完成後可以安全移除整個 Workspace

⚠️ **Base Commit 必須記錄**（連同 `branch_name`、`worktree_path`），因為 Parent Repository 可能在 Task 執行期間繼續更新——**最後 Merge 時你要知道「這個 Agent 是基於哪個版本工作的」，否則 Conflict 很難診斷**。

### 🔴 Worktree 不是 Security Sandbox（本節最重要的邊界）

Worktree 可以隔離 **Git Working Files**，但**不能自動隔離** Home Directory／Network／Secrets／Process／Docker Socket／Cloud Credential。

> **Worktree Isolation 解決版本與檔案協作，不等於 Sandbox。如果 Agent 可以執行 Shell，仍然需要 OS 層級的沙箱。**

（這正是 [[agent-permission-and-os-sandbox]] 那條「字串檢查 vs OS 強制」分層的另一種表述——**同一週的兩個系列從不同方向講到同一件事**）

### 三個實作取捨

1. **Shared Dependency Cache** — 完全隔離會慢（每個 Agent 重新下載 npm／pip wheels／build cache）。原則是 🔑 **Share Cache，不要 Share Mutable Workspace**：共享 package cache 與 compiler cache，隔離 source tree、build output、test temp files
2. **Merge 不是 Copy Files** — 不該把 Child Worktree 的檔案直接覆蓋到 Main，要透過 Git Merge／Cherry-pick／Patch，因為需要保留 Commit／Diff／Conflict／Review／Audit。🔑 **Agent 產出的 Change 應該是一個可檢查的 Artifact**
3. **Resource Scope 與 Workspace Isolation 結合** — 建立 Worktree 時一併定義 `allowed_paths`／`read_only_paths`／`forbidden_paths`（例如 allow `src/payments/**`、forbid `infra/**`、`secrets/**`）

### ⚠️ Isolation 只是把衝突延後到安全的位置

即使 Worktree 隔離，兩個 Agent 改同一個檔案最後仍會 Merge Conflict。接下來要決定：自動 Merge？讓 Parent Agent Resolve？開新 Subagent Resolve？交給人類？**這已經進入 Coordination。**

### ⚠️ Dirty State 不要默默忽略

Task 開始前 Base Repo 若已有 Uncommitted Changes，可以拒絕建立 Task／先 Snapshot／建立 Patch／明確指定 Base Commit——**否則 Agent 最後產生的 Diff 會混入使用者原本的修改**。

## C（關鍵行動）

- [ ] 檢查自己的「背景執行」是不是只是一個 Thread——Process Crash 後任務還在嗎？
- [ ] Resume 前先問：哪些 Side Effect 已經發生、哪些狀態仍不確定？
- [ ] 把 Schedule 與 Task Run 分開存，不要把週期歷史塞進同一筆
- [ ] 平行改同一個 repo 時：一個 Task 一個 Worktree，並記錄 Base Commit
- [ ] 別把 Worktree 當沙箱——它不隔離網路、Secrets 與 Home Directory

## 補充資料

### 與本知識庫的實務對照

本庫的 Skill 流程（raw-pipeline／knowledge-organizer）目前是「一次 session 跑完」的模型，沒有 Checkpoint 與 Resume——**這在批次中斷時就會整批重做**。本篇的三個判準恰好指出可改進處：

- Task State 應持久化（例如批次處理進度寫進 outputs/）
- 「模型說完成」不等於完成 → 對應本庫「標 `.processed` 前要先確認條目真的建好」
- 🔴 **本 session 曾發生的實例**：批次標記 `status: processed` 時寫壞 frontmatter，正是缺少「State Transition 要檢查版本／完成後驗證」這一步（該次的教訓已存入 Claude 的 auto-memory：批次改寫 frontmatter 必須用 YAML parser 並於寫入後回驗）

### Parent Task 與 Child Task

使用 Subagent 時最好把 Child 也視為 Task（有自己的 State、Checkpoint 與 Ownership），而非單純的函式呼叫。這與 [[subagent-four-boundaries]] 的 Result Contract 是同一個設計方向。

### 系列脈絡

Day 12（Error Recovery）→ Day 13（Task System）→ Day 14（Background Execution）→ Day 15（Scheduling）是一條完整的「長任務可靠性」線：**Recovery 決定下一步怎麼改，Task System 決定狀態存在哪裡，Background 決定誰來跑，Scheduling 決定何時被喚醒，Worktree 決定它們在哪裡動手而不互相踩到。**

## 相關連結

- [[loopx-stateful-control-plane]] — 🔑 **產品版對照**：Objective／Gate／Todo／Evidence／Quota 六種持久狀態、bounded agent slice、Claims/Leases（本篇的 Lease 概念）
- [[agent-error-recovery-taxonomy]] — 同系列 Day 12：Idempotency Key、Execution Record、Human Handoff
- [[system-prompt-assembly-pipeline]] — 同系列 Day 11：Runtime State 每輪重新組裝
- [[agent-loop-stop-conditions]] — 停止條件與中斷後的狀態不明處理
- [[agent-session-management]] — Session 與狀態管理
- [[claude-code-session-handoff]] — `/compact`／`/rewind`／`/resume` 的限制（跨不出這台機器）
- [[subagent-four-boundaries]] — Child Task 的邊界與 Result Contract
- [[production-agent-architecture]] — 生產級 Agent 的 FSM 狀態機與冪等設計
- [[ai-era-quality-assurance-test-oracle]] — 「模型說完成」不能當 Quality Gate
