---
title: "【AI Agent 15】有些事要每天做，有些事要等一下再做，系統怎麼做？ － Scheduling - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402416"
author:
  - "[[iThome]]"
published:
created: 2026-08-17
description: "前一天我們談 Background Execution。 有了 Queue、Worker、Task State 之後，Agent 已經可以在使用者離線後繼續執行。 下一個問題是： 如果任務不是現在執行..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 15

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 15 篇

## 【AI Agent 15】有些事要每天做，有些事要等一下再做，系統怎麼做？ － Scheduling

- 分享至

前一天我們談 Background Execution。

有了 Queue、Worker、Task State 之後，Agent 已經可以在使用者離線後繼續執行。

下一個問題是：

> 如果任務不是現在執行呢？

例如：

- 明天早上 9 點整理報告
- 每週一檢查依賴更新
- 每天晚上整理 Inbox
- 30 分鐘後重新確認 Deployment
- 每月產生 Usage Report

很多系統會直接說：

> 加 Cron 就好

Cron 很重要。

但真正的 Agent Scheduling 還需要處理：

- Timezone
- Missed Run
- Duplicate Run
- Concurrency
- Retry
- Catch-up
- Task Ownership
- Dynamic Schedule
- Cancellation
- Execution Budget

所以 Scheduling 不只是「什麼時候跑」。

而是：

> 某個 Task 在未來什麼條件下，應該被可靠建立或喚醒。

---

## Schedule 和 Task 要分開

Schedule 不是 Task。

Schedule 描述：

> 什麼時候建立或喚醒 Task？

Task 描述：

> 這次執行目前進行到哪裡？

例如：

```markdown
Schedule:
Every Monday 09:00

Task Run #1:
Completed

Task Run #2:
Failed

Task Run #3:
Running
```

一個 Schedule 可以產生很多 Task Run。

不要把所有歷史都塞在同一個 Task 裡。

---

## One-shot 和 Recurring

先分兩類。

### One-shot

只執行一次。

例如：

> 兩小時後提醒我重新檢查。

### Recurring

週期執行。

例如：

> 每天早上整理市場新聞。

Recurring Schedule 需要另外處理：

- 上一次何時跑
- 下一次何時跑
- 前一次還沒完成怎麼辦
- 失敗要不要補跑

---

## Timezone 不是小事

如果使用者說：

> 每天早上 9 點。

9 點是哪個 Timezone？

如果系統只保存 UTC Timestamp，遇到 Daylight Saving Time 後可能跑錯時間。

所以 Schedule 應該保存：

- Original Timezone
- Local Time Rule
- UTC Resolution

例如：

```markdown
timezone:
America/Los_Angeles

rule:
09:00 every weekday
```

Scheduler 每次根據 Timezone 計算下一個 UTC Time。

---

## Missed Run

假設系統維護 3 小時。

原本 9:00 的 Task 沒跑。

10:30 系統恢復後應該：

- 立即補跑？
- 跳過？
- 只補最近一次？
- 全部補？

不同 Task 答案不同。

例如：

- Daily Summary: 晚一點跑仍有價值
- Stock Alert: 過時的 Alert 可能沒有價值
- Billing Job: 通常不能漏

所以 Schedule 需要 Missed-run Policy。

---

## Overlap

假設每天 9 點跑一個 Agent。

昨天的 Task 還在跑。

今天 9 點到了。

要不要再開一個？

可能策略：

- Allow:兩個都跑
- Skip:如果前一個沒完成就跳過
- Queue:等前一個完成
- Replace:取消舊的，改跑新的
- Merge:合併成同一個新 Task

這是 Scheduling Policy，不應該交給模型臨時猜。

---

## Exactly-once Schedule 不等於 Exactly-once Side Effect

Scheduler 可能因 Failure 重複觸發。

所以每次 Run 應該有唯一：

```markdown
schedule_id
run_id
scheduled_at
```

產生 Task 時要 Deduplicate。

否則同一個早上 9 點可能建立兩個一樣的 Task。

---

## Dynamic Scheduling

Agent 也可能自己建立未來任務。

例如：

> 這個 API 目前 Rate Limit，30 分鐘後再試。

或者：

> 使用者還沒回覆，明天下午再 Follow-up。

這時需要一個 Scheduling Tool。

但要注意：

> Agent 可以提出 Schedule，不代表它可以無限制建立 Schedule。

Harness 應該控制：

- 是否允許
- 最遠可以排多久
- 最大 recurring 次數
- 允許頻率
- Owner
- Budget
- Cancellation

否則 Agent 很容易建立大量未來工作。

---

## Schedule 需要 Scope

一個 recurring Agent 可能每次都需要：

- 相同 Skill
- 相同 Tool Set
- 相同 Permission
- 相同 Data Scope

這些應該被保存成 Schedule Template。

但每次真正 Run 時，仍然要重新確認：

- Permission 是否仍有效
- Tool 是否存在
- User 是否仍授權
- Memory 是否更新
- Environment 是否改變

不要把建立 Schedule 當下的 Runtime State 永久 Freeze。

---

## Scheduling 和 Memory

Recurring Task 很容易和 Memory 混在一起

例如：

> 每週一幫我整理 GitHub Issue。

這是一個 Schedule。

而：

> 我偏好摘要只列 P0/P1 問題。

這是 Memory。

每次 Schedule Run 時可以 Recall Memory。

但兩者應該分開管理。

---

## Condition-based Scheduling

不是所有未來執行都由時間觸發。

也可能是：

- PR merged
- Deployment finished
- Email arrived
- Price threshold reached
- File changed
- Approval granted

這比較像 Event Trigger。

架構上可以統一成：

```markdown
Trigger
↓
Create / Resume Task
```

Time Trigger 只是其中一種 Trigger。

---

## 常見錯誤設計

### 1\. Cron = Scheduling System

沒有 Ownership、State、Retry、Missed-run Policy。

### 2\. 不保存 Timezone

DST 後時間漂移。

### 3\. Overlap 沒有 Policy

Recurring Task 重複修改同一資源。

### 4\. Agent 可以無限制排程

造成未來成本失控。

### 5\. Schedule 保存舊 Permission

幾個月後還用過期權限跑。

### 6\. Missed Run 全部補

可能一次觸發大量過時任務。

---

## 第一版 Scheduling System

至少保存：

```markdown
schedule_id
owner
timezone
trigger
task_template
overlap_policy
missed_run_policy
enabled
next_run_at
```

每次 Run：

1. 建立唯一 run\_id
2. 重新計算 Permission
3. 建立 Task
4. Enqueue
5. 更新 next\_run\_at
6. 記錄 Event

---

## 今天的結論

Scheduling 不只是 Cron Expression。

真正的問題是：

> 未來某個時間或事件發生時，如何可靠建立一個新的 Task Run？

最重要的原則：

> Schedule 定義何時開始，Task System 定義開始之後怎麼活。

下一篇會談 Worktree Isolation：

> 如果多個 Coding Agent 同時修改同一個 Repository，怎麼避免互相踩檔案？

**完整系列與程式碼範例收錄於 [https://github.com/hardness1020/awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture)**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402415)

[

【AI Agent 14】一個任務要跑三十分鐘，總不能讓使用者一直等著吧？ － Background Execution

](https://ithelp.ithome.com.tw/articles/10402415)

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