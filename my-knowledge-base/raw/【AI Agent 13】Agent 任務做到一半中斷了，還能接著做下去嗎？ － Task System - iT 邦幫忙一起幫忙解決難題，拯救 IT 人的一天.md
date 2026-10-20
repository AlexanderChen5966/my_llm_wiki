---
title: "【AI Agent 13】Agent 任務做到一半中斷了，還能接著做下去嗎？ － Task System - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402414"
author:
  - "[[iThome]]"
published:
created: 2026-08-17
description: "前一天我們談 Error Recovery。 當 Agent 開始有 Retry、Fallback、Approval、Replan、Human Handoff 之後，一個問題會變得很明顯： 任務還能只..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 13

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 13 篇

## 【AI Agent 13】Agent 任務做到一半中斷了，還能接著做下去嗎？ － Task System

- 分享至

前一天我們談 Error Recovery。

當 Agent 開始有 Retry、Fallback、Approval、Replan、Human Handoff 之後，一個問題會變得很明顯：

> 任務還能只存在記憶體裡嗎？

如果 Agent 執行到一半需要等待使用者批准、外部 API 回覆、另一個 Subagent 完成，甚至隔幾個小時再繼續，那麼「一次 Request 進來、一次 Response 回去」的模型就不夠了。

這時需要一個真正的 Task System。

很多早期 Agent 會把 Task System 簡化成：

> 一個 `while` loop 加一個 task\_id。

但 task\_id 只是在辨識任務，不代表系統真的能暫停、恢復、追蹤與恢復。

真正的 Task System 至少要處理：

1. Task State
2. Persistence
3. Checkpoint
4. Resume
5. Cancellation
6. Ownership
7. Idempotency
8. Audit

---

## Task 不是 Conversation

Conversation 是訊息歷史。

Task 是一個需要被完成的工作單位。

一個 Task 可能跨越多個 Conversation Turn，也可能完全沒有使用者互動。

例如：

```markdown
目標：
整理一份競品研究

Task 生命週期：
Created
↓
Running
↓
Waiting for Subagent
↓
Running
↓
Waiting for Approval
↓
Running
↓
Completed
```

如果只用 Conversation History 表示這個流程，很快會出現問題：

- 不知道目前 State
- 不知道等待誰
- 不知道是否可恢復
- 不知道是否已經執行某個 Side Effect
- 不知道誰可以 Cancel

所以 Task 應該是一個獨立的一級物件。

---

## Task 最少需要哪些 State？

第一版可以先定義：

```markdown
Created
Running
Waiting
Blocked
Completed
Failed
Cancelled
```

但實務上 Waiting 最好再有 Reason。

例如：

```markdown
waiting_for_approval
waiting_for_subagent
waiting_for_external_event
waiting_for_schedule
waiting_for_user_input
```

原因很簡單：

> 「暫停」不是一種完整狀態。

系統需要知道為什麼停，以及什麼事件能讓它重新開始。

---

## Task State 不是模型自己說了算

模型可以輸出：

> 我已經完成。

但 Task System 不能直接把 State 改成 Completed。

它應該先檢查：

- Completion Condition
- Verification
- Required Artifacts
- Approval State
- Pending Side Effects
- Remaining Subtasks

這和前面談過的原則一致：

> Model 決定下一步，Harness 決定狀態是否真的可以轉移。

---

## Persistence：任務不能只活在 Process Memory

如果服務重新啟動，正在等待 Approval 的任務不應該消失。

Task 最少需要持久化：

- task\_id
- status
- goal
- current\_step
- plan
- pending\_action
- tool\_execution\_history
- approval\_state
- budget
- checkpoint
- created\_at
- updated\_at

這些資料讓 Agent 不依賴某一個 Python Process。

否則任何 Deploy、Crash 或 Scale-out 都可能直接讓任務遺失。

---

## Checkpoint：Resume 的基礎

Checkpoint 回答：

> 如果現在停掉，下次從哪裡繼續？

一個好的 Checkpoint 不需要保存所有 Runtime 物件。

它需要保存足夠重建狀態的資訊。

例如：

```markdown
Current Step:
等待 Production Deploy Approval

Pending Tool:
deploy_service

Arguments:
service=payments
environment=production
version=v1.8.2

Verification Completed:
tests passed
build passed

Next Action:
resume deploy after approval
```

有了 Checkpoint，系統才能真正做到 Pause / Resume。

---

## Resume 不是重新跑一次

這是很重要的差別。

如果 Agent 在付款 API 後 Timeout，Resume 時直接從頭執行，可能造成重複付款。

所以 Resume 需要先知道：

- 哪些 Step 已經完成
- 哪些 Tool 已經有 Side Effect
- 哪些結果已確認
- 哪些 Operation 仍不確定

這就是為什麼 Task System 和 Idempotency 必須一起設計。

---

## Cancellation

使用者說：

> 停止這個任務。

系統不能只是停止下一次模型呼叫。

還需要考慮：

- 已經執行的 Side Effect 怎麼辦？
- Background Tool 還在跑嗎？
- Subagent 要不要一起 Cancel？
- 已建立的暫存資源要不要清掉？
- 已經排程的後續任務要不要取消？

Cancellation 是一種狀態轉移，也可能需要 Cleanup。

---

## Parent Task 和 Child Task

當使用 Subagent 時，最好把 Child 也視為 Task。

例如：

```markdown
Parent Task
研究三個競品

Child A
分析 Competitor A

Child B
分析 Competitor B

Child C
分析 Competitor C
```

Parent 需要知道：

- 哪些 Child 完成
- 哪些失敗
- 哪些 Cancel
- 是否要 Partial Result
- 是否要等待全部完成

這讓 Multi-Agent Coordination 從「很多 Loop」變成可管理的 Task Graph。

---

## Task Ownership

誰可以：

- Resume
- Cancel
- Approve
- Retry
- Escalate
- Modify Scope

也應該是 Task System 的一部分。

例如：

```markdown
Owner:
user_123

Approver:
team_admin

Executor:
agent_worker_7
```

不要把「知道 task\_id」等同於「可以控制 Task」。

---

## Task Event Log

Task 最好採用 Event Log。

例如：

```markdown
TASK_CREATED
MODEL_CALLED
TOOL_REQUESTED
TOOL_COMPLETED
APPROVAL_REQUIRED
APPROVAL_GRANTED
TASK_RESUMED
TASK_COMPLETED
```

Event Log 有兩個好處：

1. Debug
2. Rebuild State

如果 Final State 壞掉，可以從 Event Replay 重建。

這也是 Observability 的基礎。

---

## 常見錯誤設計

### 1\. Task 只是 task\_id

沒有 State Machine、Persistence、Checkpoint。

### 2\. Waiting 沒有原因

不知道什麼事件可以 Resume。

### 3\. Resume 等於重新執行

很容易重複 Side Effect。

### 4\. Model 自己決定 Completed

缺少 Verification。

### 5\. Cancel 只停 Agent Loop

Background Job 和 Child Task 還在跑。

### 6\. 沒有 Ownership

任何拿到 ID 的人都能操作。

---

## 第一版 Task System

第一版至少要有：

```markdown
Task
- id
- status
- goal
- current_step
- checkpoint
- owner
- budget
- pending_action
- updated_at
```

再加上：

- state transition validation
- persistent storage
- event log
- cancel
- resume
- completion verification

這就已經比單純「長時間跑一個 Agent Loop」可靠很多。

---

## 今天的結論

一個 Production Agent 的 Task，不是一個 Function Call。

它是一個有生命週期、有狀態、有持久化、有 Side Effect History 的 State Machine。

最重要的原則是：

> Conversation 保存互動，Task System 保存工作。

下一篇會進入 Background Execution：

> 當任務需要跑很久，什麼東西應該離開同步 Request，進入 Background Worker？

**完整系列與程式碼範例收錄於 [https://github.com/hardness1020/awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture)**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402025)

[

【AI Agent 12】Agent 出錯的時候，為什麼一直重試也救不回來？ － Error Recovery

](https://ithelp.ithome.com.tw/articles/10402025)[下一篇](https://ithelp.ithome.com.tw/articles/10402415)

[

【AI Agent 14】一個任務要跑三十分鐘，總不能讓使用者一直等著吧？ － Background Execution

](https://ithelp.ithome.com.tw/articles/10402415)

系列文

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121) 共 14 篇

目錄

1. 10
	[【AI Agent 10】關掉視窗就全部忘光，Agent 要怎麼記得事情？ － Memory](https://ithelp.ithome.com.tw/articles/10401896)
2. 11
	[【AI Agent 11】每個任務需要的背景都不一樣，System Prompt 怎麼可以寫死？ － System Prompt](https://ithelp.ithome.com.tw/articles/10401899)
3. 12
	[【AI Agent 12】Agent 出錯的時候，為什麼一直重試也救不回來？ － Error Recovery](https://ithelp.ithome.com.tw/articles/10402025)
4. 13
	[【AI Agent 13】Agent 任務做到一半中斷了，還能接著做下去嗎？ － Task System](https://ithelp.ithome.com.tw/articles/10402414)
5. 14
	[【AI Agent 14】一個任務要跑三十分鐘，總不能讓使用者一直等著吧？ － Background Execution](https://ithelp.ithome.com.tw/articles/10402415)
[完整目錄](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

[![.](https://itadstatic.ithome.com.tw/B9/1786896772_6a81e18491c26.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/21/999a0a5b-2bf0-4591-a6bf-4d2d622a1e05_small.png)

[使用真勇者的版控流程－－主幹開發，大幅提高程式穩定性](https://itplus.ithome.com.tw/vod-page/776?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/1/4/e182a067-2b88-4cad-9556-9257a6a1e9b8_small.png)

[【以Cloud Native雲端原生工具打造雲端安全】](https://itplus.ithome.com.tw/vod-page/799?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

安碁資訊｜資安防護服務．企業營運夥伴 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/86cf085b-df39-4592-b445-0c8314cf97ff_small.png)

[前端可觀測性 - Grafana 在下的一盤大棋](https://itplus.ithome.com.tw/vod-page/1040?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

27 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/b5c33eaa-9086-4e86-9382-a01910a707db_small.png)

[我們與「敏捷」的距離](https://itplus.ithome.com.tw/vod-page/1360?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

48 分