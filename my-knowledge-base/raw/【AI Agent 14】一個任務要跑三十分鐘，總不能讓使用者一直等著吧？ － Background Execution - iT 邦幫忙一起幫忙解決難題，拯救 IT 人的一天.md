---
title: "【AI Agent 14】一個任務要跑三十分鐘，總不能讓使用者一直等著吧？ － Background Execution - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402415"
author:
  - "[[iThome]]"
published:
created: 2026-08-17
description: "前一天我們把 Agent Task 變成一個真正有 State 的系統。 下一個問題自然是： 如果任務要跑 20 分鐘，誰來執行？ 很多 Demo 會直接在 Request Handler 裡呼叫 A..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 14

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 14 篇

## 【AI Agent 14】一個任務要跑三十分鐘，總不能讓使用者一直等著吧？ － Background Execution

- 分享至

前一天我們把 Agent Task 變成一個真正有 State 的系統。

下一個問題自然是：

> 如果任務要跑 20 分鐘，誰來執行？

很多 Demo 會直接在 Request Handler 裡呼叫 Agent：

```markdown
HTTP Request
↓
Agent Loop
↓
Tool Calls
↓
完成
↓
HTTP Response
```

如果 Agent 只跑 5 秒，沒問題。

但任務一旦變長，這種設計會遇到：

- Request Timeout
- Process Restart
- Client Disconnect
- Worker 被佔滿
- 無法 Retry
- 無法 Resume
- 難以 Scale

所以 Long-running Agent 需要 Background Execution。

---

## Background Job 不是 Thread

第一個錯誤等號是：

> 開一個 Background Thread，就有 Background Execution。

Thread 只把工作移出目前 Call Stack。

它沒有解決：

- Process Crash
- Job Persistence
- Retry
- Ownership
- Queue
- Scheduling
- Worker Scale
- Result Delivery

真正的 Background Execution 需要一個 Durable Job Model。

---

## 基本架構

可以拆成：

```markdown
API
↓
Create Task
↓
Enqueue Job
↓
Background Worker
↓
Load Task State
↓
Run Agent Step
↓
Persist Checkpoint
↓
Continue / Pause / Complete
```

API 的責任是建立任務。

Worker 的責任是執行任務。

這樣 Client 不需要保持 Connection。

---

## Queue 解決什麼？

Queue 不只是「排隊」。

它負責建立 Producer 和 Worker 之間的邊界。

Producer 可以快速接受任務。

Worker 可以根據能力與資源慢慢處理。

Queue 也可以支援：

- Retry
- Priority
- Delay
- Dead Letter
- Backpressure
- Concurrency Limit

對 Agent 特別重要，因為不同 Task 的成本差異很大。

---

## Worker 不應該一次持有整個 Task

長任務最好切成可 Checkpoint 的 Step。

例如：

```markdown
Load Task
↓
Run one agent turn
↓
Persist
↓
Decide next state
↓
Re-enqueue if needed
```

這樣 Worker Crash 時，只損失一小段工作。

如果一個 Worker 連續跑 40 分鐘都不存狀態，那仍然不是真正 Durable。

---

## Lease

如果多個 Worker 同時看到同一個 Task，誰可以執行？

需要 Lease 或 Lock。

例如：

```markdown
worker_A
取得 task_123 lease
有效 60 秒
```

只要 Lease 有效，其他 Worker 不應該同時執行。

Worker 需要定期 Heartbeat。

如果 Worker 掛掉，Lease Expire，其他 Worker 才能接手。

這避免同一個 Agent Task 被重複跑兩次。

---

## At-least-once Delivery

很多 Queue 只能保證：

> Job 至少會送達一次。

不一定只送一次。

因此 Worker 必須假設：

> 同一個 Job 可能被執行兩次。

這又回到 Idempotency。

例如：

- Tool Call 需要 operation\_id
- State Transition 要檢查版本
- Completed Task 不應再次執行
- Side Effect 要能 Deduplicate

不要期待 Queue 幫你提供 Exactly-once Side Effect。

---

## Backpressure

如果一瞬間進來 10,000 個 Agent Task，系統不應該全部同時跑。

因為每個 Task 可能會消耗：

- Model Tokens
- API Rate Limit
- CPU
- Browser
- Database
- External Tool Quota

Queue 可以配合：

- Worker Concurrency
- Per-user Limit
- Per-tool Limit
- Priority
- Budget

建立 Backpressure。

這是一個很重要的 Production Control。

---

## Priority

不是所有 Task 都一樣重要。

例如：

```markdown
P0
Production Incident

P1
Interactive User Request

P2
Batch Analysis

P3
Nightly Cleanup
```

如果 Background Queue 完全 FIFO，一個大 Batch 可能把 Interactive Request 卡住。

所以 Priority 應該是 Task Metadata。

---

## Retry 應該在 Task 還是 Queue？

答案通常是兩層。

### Queue Retry

處理 Worker 層級失敗：

- Worker Crash
- Network Failure
- Temporary Infrastructure Error

### Agent Recovery

處理任務語意失敗：

- Tool Validation Error
- Strategy Failure
- Verification Failure
- Permission Denied

不要把所有 Agent Failure 都交給 Queue 重送。

否則一個 Permission Denied Task 可能被重新執行十次。

---

## Result Delivery

Task 在 Background 完成後，使用者怎麼知道？

常見方式：

- Polling
- WebSocket
- Server-Sent Events
- Notification
- Email
- Inbox
- Callback
- Event Bus

重要的是：

> Execution 和 Delivery 應該分離。

Worker 完成 Task，不代表一定要當場把結果送給 Client。

---

## Progress Update

Long-running Agent 最糟糕的體驗之一是：

> Running...

然後 20 分鐘沒有任何訊息。

Task 可以保存 Progress：

```markdown
Step 3/7
正在驗證修正

Completed:
- 找到 root cause
- 修改 payment timeout handling

Current:
- running integration tests
```

Progress 不一定要由模型自由生成。

可以從 Task State、Tool Calls、Plan Step 結構化產生。

---

## Background Task 和 Subagent

Subagent 不一定等於 Background。

一個 Child 可以同步執行。

也可以被放到 Queue。

如果 Child Task 很重，放到 Background Worker 有幾個好處：

- 獨立 Retry
- 獨立 Budget
- 平行執行
- 可以 Cancel
- Parent 不必一直佔 Worker

這時 Parent 等的是 Child Task State，而不是一個 Function Return。

---

## Cancellation Propagation

Parent Task 被 Cancel 時，需要決定：

- Child Task 是否一起取消
- Running Tool 是否可以 Interrupt
- Queue Job 是否撤回
- Background Process 是否停止
- 已產生 Artifact 是否保留

Cancellation 應該明確定義 Scope。

---

## Dead Letter Queue

如果同一個 Job 因 Infrastructure 問題重試多次仍失敗，可以進 Dead Letter Queue。

這表示：

> 系統無法自動恢復，需要人工檢查。

Dead Letter 不應該偷偷消失。

它應該有：

- Task ID
- Failure Reason
- Retry Count
- Last Checkpoint
- Worker Trace

---

## 常見錯誤設計

### 1\. Background = Thread

Process 一掛全部消失。

### 2\. Worker 不 Checkpoint

Crash 後整段重跑。

### 3\. 假設 Queue Exactly Once

造成重複 Side Effect。

### 4\. 沒有 Lease

同一 Task 被多 Worker 執行。

### 5\. Queue Retry 取代 Agent Recovery

語意錯誤被無腦重送。

### 6\. 沒有 Backpressure

流量一來把 Model/API 全部打爆。

---

## 第一版 Background Execution

至少加入：

- Durable Queue
- Worker
- Task Persistence
- Lease
- Heartbeat
- Idempotency
- Checkpoint
- Retry Policy
- Priority
- Cancellation

這樣 Agent 才真正能離開同步 Request。

---

## 今天的結論

Background Execution 的核心不是「把程式丟到背景跑」。

而是：

> 讓 Task 可以離開原本 Request，仍然被可靠執行、暫停、重試、取消與恢復。

下一篇會談 Scheduling：

> Background Task 是現在執行，那如果是明天早上、每週一、或條件成立時才執行呢？

**完整系列與程式碼範例收錄於 [https://github.com/hardness1020/awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture)**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402414)

[

【AI Agent 13】Agent 任務做到一半中斷了，還能接著做下去嗎？ － Task System

](https://ithelp.ithome.com.tw/articles/10402414)[下一篇](https://ithelp.ithome.com.tw/articles/10402416)

[

【AI Agent 15】有些事要每天做，有些事要等一下再做，系統怎麼做？ － Scheduling

](https://ithelp.ithome.com.tw/articles/10402416)

系列文

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121) 共 15 篇

目錄

1. 11
	[【AI Agent 11】每個任務需要的背景都不一樣，System Prompt 怎麼可以寫死？ － System Prompt](https://ithelp.ithome.com.tw/articles/10401899)
2. 12
	[【AI Agent 12】Agent 出錯的時候，為什麼一直重試也救不回來？ － Error Recovery](https://ithelp.ithome.com.tw/articles/10402025)
3. 13
	[【AI Agent 13】Agent 任務做到一半中斷了，還能接著做下去嗎？ － Task System](https://ithelp.ithome.com.tw/articles/10402414)
4. 14
	[【AI Agent 14】一個任務要跑三十分鐘，總不能讓使用者一直等著吧？ － Background Execution](https://ithelp.ithome.com.tw/articles/10402415)
5. 15
	[【AI Agent 15】有些事要每天做，有些事要等一下再做，系統怎麼做？ － Scheduling](https://ithelp.ithome.com.tw/articles/10402416)
[完整目錄](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

[![.](https://itadstatic.ithome.com.tw/B9/1786896772_6a81e18491c26.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/f9d1c7c7-1e58-4501-ba69-50694296e234_small.png)

[Cloud Security — 雲端安全治理與發展策略](https://itplus.ithome.com.tw/vod-page/543?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/2eec9da7-363d-4529-b494-eafa785e4b28_small.png)

[SUSE 雲原生方案和開源創新](https://itplus.ithome.com.tw/vod-page/1323?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/0bb018fc-4843-48d9-aa8e-1d21f64af0ab_small.png)

[遍佈全球的卓越安全性和連線性](https://itplus.ithome.com.tw/vod-page/1207?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/7af9ca54-eb0e-4870-ae40-31e0a55df8c4_small.png)

[數位治理與智慧城市：臺中市的轉型與未來機遇](https://itplus.ithome.com.tw/vod-page/1242?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

31 分