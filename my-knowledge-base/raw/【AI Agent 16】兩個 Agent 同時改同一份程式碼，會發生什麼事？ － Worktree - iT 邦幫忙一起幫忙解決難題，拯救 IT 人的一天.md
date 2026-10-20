---
title: "【AI Agent 16】兩個 Agent 同時改同一份程式碼，會發生什麼事？ － Worktree - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402418"
author:
  - "[[iThome]]"
published:
created: 2026-08-18
description: "前一天我們談 Scheduling。 當 Agent 可以 Background、Recurring、Parallel 執行後，Coding Agent 很快會遇到一個實際問題： 多個 Task 可以..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 16

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 16 篇

## 【AI Agent 16】兩個 Agent 同時改同一份程式碼，會發生什麼事？ － Worktree

- 分享至

前一天我們談 Scheduling。

當 Agent 可以 Background、Recurring、Parallel 執行後，Coding Agent 很快會遇到一個實際問題：

> 多個 Task 可以同時修改同一個 Repository 嗎？

最簡單的做法是：

```markdown
所有 Agent
↓
同一個 git checkout
```

這在 Demo 中很方便。

但進入 Production 後，會出現：

- Agent A 改到 Agent B 的檔案
- Test 結果互相污染
- Uncommitted Change 混在一起
- Branch 切換互相覆蓋
- Cleanup 不知道該刪誰
- Verification 無法對應正確版本

所以真正的 Parallel Coding Agent，需要 Workspace Isolation。

---

## Branch 不等於 Isolation

第一個錯誤等號是：

> 每個 Agent 用不同 Branch，就隔離了。

如果它們仍然共用同一個 Working Directory，Branch 切換本身就會互相干擾。

真正需要的是：

- 不同 Branch
- 不同 Working Tree
- 不同 Runtime State

Git Worktree 很適合這種需求。

---

## 一個 Task 一個 Worktree

概念上：

```markdown
Repository
├── main
├── worktree-task-101
├── worktree-task-102
└── worktree-task-103
```

每個 Task 有自己的：

- Branch
- Filesystem
- Uncommitted Changes
- Test State
- Build Artifact

這樣 Agent A 刪檔，不會直接影響 Agent B。

---

## Isolation 解決的不只是 Git Conflict

Worktree Isolation 主要解決四類問題：

- Filesystem Isolation: 每個 Agent 看到自己的修改。
- Version Isolation: 每個 Task 綁定明確 Base Commit。
- Verification Isolation: Test 是針對自己的 Working Tree。
- Cleanup Isolation: Task 完成後可以安全移除整個 Workspace。

---

## Base Commit 很重要

建立 Worktree 時要記錄：

```markdown
base_commit
branch_name
worktree_path
```

因為 Parent Repository 可能在 Task 執行期間繼續更新。

最後 Merge 時你要知道：

> 這個 Agent 是基於哪個版本工作的？

否則 Conflict 很難診斷。

---

## Worktree 不是 Security Sandbox

這個邊界要很清楚。

Worktree 可以隔離：

- Git Working Files

但不能自動隔離：

- Home Directory
- Network
- Secrets
- Process
- Docker Socket
- Cloud Credential

所以：

> Worktree Isolation 解決版本與檔案協作，不等於 Sandbox。

如果 Agent 可以執行 Shell，仍然需要 Day 4 的 Sandbox。

---

## Shared Dependency Cache

完全隔離每個 Workspace 可能很慢。

例如每個 Agent 都重新下載：

- npm packages
- pip wheels
- build cache

可以共享 Cache。

但要注意：

> Share Cache，不要 Share Mutable Workspace。

例如：

```markdown
Shared:
package cache
compiler cache

Isolated:
source tree
build output
test temp files
```

這樣兼顧速度與隔離。

---

## Merge 不是 Copy Files

Task 完成後，不應該直接把 Child Worktree 的檔案覆蓋到 Main。

應該透過 Git Merge / Cherry-pick / Patch。

因為需要保留：

- Commit
- Diff
- Conflict
- Review
- Audit

Agent 產出的 Change 應該是一個可檢查的 Artifact。

---

## Parallel Agent 最難的是 Conflict

假設 Agent A 修改：

```markdown
payments/service.py
```

Agent B 也修改同一個檔案。

即使 Worktree 隔離，最後還是會 Merge Conflict。

Isolation 只是把衝突延後到安全的位置。

接下來要決定：

- 自動 Merge？
- 讓 Parent Agent Resolve？
- 開新 Subagent Resolve？
- 交給人類？

這已經進入 Coordination。

---

## Resource Scope

每個 Coding Task 可以在建立 Worktree 時一起定義：

```markdown
allowed_paths
read_only_paths
forbidden_paths
```

例如：

```markdown
allowed:
src/payments/**
tests/payments/**

forbidden:
infra/**
secrets/**
```

這樣 Permission 可以和 Workspace Isolation 結合。

---

## Dirty State

Task 開始前，Base Repo 如果已有 Uncommitted Changes 怎麼辦？

不要默默忽略。

可以：

- 拒絕建立 Task
- 先 Snapshot
- 建立 Patch
- 明確指定 Base Commit

不然 Agent 最後產生的 Diff 會混入使用者原本的修改。

---

## Cleanup

Task 結束後 Worktree 要不要立刻刪？

不一定。

可以分：

- Completed + Merged: 可以清除
- Failed: 可能保留一段時間方便 Debug
- Waiting for Review: 應該保留
- Cancelled: 先保存 Diff，再清理

Worktree 本身也是 Task Artifact。

---

## Background Task 與 Worktree Lease

如果 Task 被 Background Worker 執行，Worktree 也需要 Ownership。

例如：

```markdown
task_123
owns
/worktrees/task_123
```

不要讓兩個 Worker 同時操作。

這可以和 Task Lease 綁定。

---

## 常見錯誤設計

### 1\. 所有 Agent 共用一個 Checkout

修改互相污染。

### 2\. 不記 Base Commit

Merge 時不知道差異來源。

### 3\. 把 Worktree 當 Sandbox

Shell 仍然可能越界。

### 4\. 完成後直接 Copy Files

沒有 Diff、Commit、Audit。

### 5\. 不處理 Dirty State

混入使用者原本修改。

### 6\. Task Cancel 後直接刪

可能把有價值的 Patch 一起丟掉。

---

## 第一版 Worktree Manager

至少需要：

```markdown
task_id
repo
base_commit
branch
worktree_path
allowed_paths
status
created_at
```

並支援：

- create
- validate
- diff
- test
- commit
- merge
- cleanup

這樣 Coding Agent 才能真正安全平行。

---

## 今天的結論

Parallel Coding 的關鍵不是多開幾個 Agent。

而是：

> 每個 Agent 是否擁有可追蹤、可驗證、可合併的獨立工作空間。

最重要的原則：

> 隔離修改，集中整合。

下一篇會進入 Coordination：

> 當多個 Agent 都有自己的 Task、Context、Worktree 與結果後，誰來決定它們如何一起完成同一個目標？

**完整系列與程式碼範例收錄於 [https://github.com/hardness1020/awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture)**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402416)

[

【AI Agent 15】有些事要每天做，有些事要等一下再做，系統怎麼做？ － Scheduling

](https://ithelp.ithome.com.tw/articles/10402416)

系列文

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121) 共 16 篇

目錄

1. 12
	[【AI Agent 12】Agent 出錯的時候，為什麼一直重試也救不回來？ － Error Recovery](https://ithelp.ithome.com.tw/articles/10402025)
2. 13
	[【AI Agent 13】Agent 任務做到一半中斷了，還能接著做下去嗎？ － Task System](https://ithelp.ithome.com.tw/articles/10402414)
3. 14
	[【AI Agent 14】一個任務要跑三十分鐘，總不能讓使用者一直等著吧？ － Background Execution](https://ithelp.ithome.com.tw/articles/10402415)
4. 15
	[【AI Agent 15】有些事要每天做，有些事要等一下再做，系統怎麼做？ － Scheduling](https://ithelp.ithome.com.tw/articles/10402416)
5. 16
	[【AI Agent 16】兩個 Agent 同時改同一份程式碼，會發生什麼事？ － Worktree](https://ithelp.ithome.com.tw/articles/10402418)
[完整目錄](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

[![.](https://itadstatic.ithome.com.tw/B9/1786896772_6a81e18491c26.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/18/64587017-e3a7-4fc5-a342-546a00537119_small.jpg)

[資安弱點日新月異？讓曝險管理平台協助您一站瞭解公司資訊安全性！【宏碁資訊網路學堂】](https://itplus.ithome.com.tw/vod-page/90?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

ＡＥＢ大補帖 |

32 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/6/30/a4ed85be-4687-4815-afca-9f9344d1f9a2_small.png)

[轉型智慧工廠最關鍵的一步](https://itplus.ithome.com.tw/vod-page/136?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

22 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/21/2cc4afe4-e117-4311-85f9-42961aa95bc5_small.png)

[Three.js實現ROS虛實環境監控系統](https://itplus.ithome.com.tw/vod-page/757?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/4/22/b5b904db-45ff-48be-8564-84355597a21e_small.jpg)

[如何善用GenAI優勢 打造自動化IT服務體驗 - with ServiceNow & ChatGPT](https://itplus.ithome.com.tw/vod-page/810?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

40 分