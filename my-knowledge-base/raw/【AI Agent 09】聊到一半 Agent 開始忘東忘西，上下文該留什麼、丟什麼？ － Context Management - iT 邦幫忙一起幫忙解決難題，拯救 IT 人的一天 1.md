---
title: "【AI Agent 09】聊到一半 Agent 開始忘東忘西，上下文該留什麼、丟什麼？ － Context Management - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401895"
author:
  - "[[iThome]]"
published:
created: 2026-08-12
description: "前一天我們談 Skills 與 Progressive Disclosure。 核心觀念是： Context 是工作區，不是倉庫。 Agent 不需要一開始就讀完整知識庫，也不應該把所有 Skill..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 9

1

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 9 篇

## 【AI Agent 09】聊到一半 Agent 開始忘東忘西，上下文該留什麼、丟什麼？ － Context Management

- 分享至

前一天我們談 Skills 與 Progressive Disclosure。

核心觀念是：

> Context 是工作區，不是倉庫。

Agent 不需要一開始就讀完整知識庫，也不應該把所有 Skill 永久留在 Prompt 裡。

但即使我們已經做到動態載入，Agent 執行時間一長，Context 還是會持續變大。

因為每一輪都可能新增：

- 使用者訊息
- 模型回覆
- Tool Call
- Tool Result
- Plan
- Error Log
- Skill
- Artifact 摘要
- Subagent Result
- Verification Result

如果什麼都不刪，Agent 最後一定會碰到兩個問題：

1. Context Window 放不下
2. 就算放得下，模型也不一定看得清楚

所以 Context Management 真正要解決的，不只是：

> Token 超過限制時怎麼辦？

更重要的是：

> 哪些資訊值得繼續佔用模型的注意力？

今天我們會把 Context Management 拆成四個核心機制：

1. Selection
2. Compaction
3. Eviction
4. Rehydration

---

## Context Window 大，不代表可以把所有東西留下

大型模型的 Context Window 越來越大，很容易讓人產生一個直覺：

> 反正放得下，就全部留下。

但這通常會把容量問題變成注意力問題。

假設 Agent 已經執行 80 輪，Context 中包含：

- 最初使用者需求
- 30 次檔案讀取
- 10 次測試結果
- 8 次錯誤
- 4 份不同 Plan
- 3 個已完成 Subagent 的完整輸出
- 20 個已經不相關的 Tool Result

模型理論上可能全部看得到。

但真正需要下一輪決策的資訊，可能只有：

- 目前目標
- 最新 Plan
- 最近一次測試失敗
- 已修改的檔案
- 尚未完成的 Constraint

其他資訊不一定值得繼續留在 Working Context。

所以 Context Window 和 Storage 是兩個不同概念。

Storage 的目標是：

> 不要遺失資料。

Context 的目標是：

> 讓模型在這一輪看到最有用的資料。

---

## Context Management 不是單純截斷

最簡單的做法是：

> 超過 Token 上限，就刪掉最舊訊息。

這可以避免 API Error。

但很容易刪掉真正重要的內容。

例如：

第一輪使用者說：

> 不要修改 `billing/` 。

後面執行 50 輪後，如果這條限制因為太舊而被刪掉，Agent 可能完全失去關鍵 Constraint。

所以真正的 Context Management 不能只靠時間順序。

它需要判斷資訊的角色與價值。

---

## 第一層：Selection

Selection 回答：

> 哪些資訊應該進入下一輪 Context？

不是所有已知資訊都要出現。

可以先把資料分成幾類。

### Goal

使用者真正要完成什麼？

這通常應該高優先保留。

### Constraints

哪些事情不能做？

例如：

- 不可修改某個資料夾
- 不可發送 Email
- 必須使用某個輸出格式
- 必須在特定 Budget 內完成

Constraints 通常比很多中間對話更重要。

### Current State

目前做到哪裡？

例如：

- Plan
- 已完成 Step
- 失敗 Step
- Changed Files
- Pending Approval

### Recent Observation

最新工具結果是否會影響下一步？

最近一次失敗通常很重要。

兩小時前已經解決的錯誤可能不重要。

### Evidence

哪些證據仍然支持目前判斷？

### History

過去發生什麼？

History 不一定全部需要保留。

很多部分可以摘要或移出 Context。

---

## Selection 不是 Memory Retrieval

Context Selection 與 Memory Retrieval 很像，但來源不同。

Context Selection 處理的是：

> 這個任務目前已經產生的資訊，哪些要留在 Working Context？

Memory Retrieval 處理的是：

> 過去 Session、使用者或任務的資訊，哪些要重新載入？

兩者最後都會把資訊放進 Context，但責任不同。

今天先處理同一個任務內的 Working Context。

---

## 第二層：Compaction

有些資訊仍然重要，但不需要完整保留。

這時可以做 Compaction。

例如 Agent 讀了 15 個檔案。

不一定需要保留 15 個完整 Tool Result。

可以壓縮成：

```markdown
已確認：
- Auth 使用 JWT
- Token validation 位於 middleware/auth.py
- Refresh flow 位於 services/token.py
- 目前錯誤集中在 expiry handling
```

原始資料仍可以保存在外部 Storage。

Context 中只保留對下一步有用的 Summary。

這就是：

```markdown
Full Artifact
↓
Task-Relevant Summary
↓
Working Context
```

Compaction 的目標不是單純縮短。

而是保留決策需要的資訊。

---

## 好的 Summary 不是越短越好

如果 Summary 只寫：

> 已分析 Auth 模組，發現一些問題。

Token 很少，但幾乎沒有價值。

有效的 Summary 應該保留：

- 結論
- 關鍵證據
- 尚未解決問題
- 重要 Constraint
- 下一步需要的資訊

可以把它想成：

> 如果原始內容現在被移出 Context，未來的 Agent 還需要知道什麼才能繼續工作？

這個問題比「怎麼把內容縮到最短」更重要。

---

## 第三層：Eviction

有些資訊已經完全不值得留在 Context。

例如：

- 已成功完成的 Tool Call 細節
- 已被新結果推翻的假設
- 已經不再使用的 Skill
- 重複的搜尋結果
- 過時的 Plan
- 已經整合完成的 Child Transcript
- 大量成功 Log

這些內容可以 Evict。

Eviction 不是刪除資料。

它只是：

> 不再讓資料佔用目前模型的 Context。

原始資訊可以保存在：

- Trace Store
- Artifact Store
- Database
- File
- External Log
- Task History

如果未來需要，再重新載入。

---

## Eviction Policy 可以怎麼設計？

可以根據多個訊號決定：

- Recency: 最近是否使用？
- Relevance: 與目前 Plan 是否相關？
- Uniqueness: 是否只是重複資訊？
- Authority: 是否為使用者 Constraint、System Policy 或 Verified Evidence？
- Cost: 這段內容有多大？
- Recoverability: 移除後是否容易重新取得？

例如：

使用者明確設定的 Constraint：

> 不可自動部署 Production。

即使很舊，也應該高優先保留。

而一個 20K Token 的 Test Log，如果可以隨時重新執行，就比較適合 Evict。

---

## 可以把 Context 分成不同層級

一個實用方式是建立 Context Tiers。

### Tier 1: Pinned

幾乎永遠保留。

例如：

- 使用者目標
- 核心 Constraint
- System Policy
- 重要 Permission
- Current Plan

### Tier 2: Active

目前任務正在使用。

例如：

- 最近 Tool Result
- 當前 Step
- 相關 Skill
- 最近 Error
- Active Artifact Summary

### Tier 3: Compressed

仍有價值，但只保留 Summary。

例如：

- 已閱讀檔案
- 已完成 Investigation
- 舊的 Subagent Result
- 過去嘗試

### Tier 4: External

移出 Context，但保留引用。

例如：

- 完整 Log
- 大型文件
- Binary Artifact
- 完整 Conversation
- 舊 Trace

這種分層比單純 FIFO 更穩定。

---

## 第四層：Rehydration

Evict 並不代表永遠不再使用。

如果後面出現新的問題，Agent 可能需要把某段資訊重新載入。

這就是 Rehydration。

例如：

```markdown
目前只保留：
Auth 模組摘要

後來 Agent 發現：
問題可能來自 Refresh Token

系統再載入：
services/token.py
相關舊 Trace
先前的 Test Result
```

這讓 Context 可以保持小，但資訊仍然可恢復。

Rehydration 的關鍵是：

> Evict 之前要留下足夠的索引。

例如：

- Artifact ID
- File Path
- Trace ID
- Search Key
- Summary
- Source Reference

如果什麼都不留下，之後就不知道去哪裡找。

---

## Stub 是一個很實用的技巧

當大型內容被移出 Context，可以留下 Stub。

例如：

```markdown
[Artifact: test-log-042]
內容：完整 integration test log
大小：18,420 tokens
摘要：3 個失敗，主要集中於 payment timeout
需要時可重新讀取
```

這比完全刪掉更好。

模型知道這份資訊存在。

但不需要每一輪都重新支付完整 Context 成本。

---

## Context Compaction 最危險的問題：壓錯東西

如果 Compaction 把重要細節丟掉，後面可能很難恢復。

例如原始內容：

```markdown
使用者：
可以修改設定，但絕對不要修改 Production Secret。
```

Summary 卻寫成：

```markdown
使用者允許修改設定。
```

關鍵 Constraint 消失了。

所以 Compaction 不是純摘要任務。

它必須保留高優先資訊。

可以設計：

### Protected Fields

永遠不能在 Summary 中遺失：

- Goal
- Constraints
- Approval State
- Security Boundaries
- Unresolved Errors
- Verification Result

### Free-form History

可以較自由壓縮：

- 對話細節
- 搜尋過程
- 成功 Tool Calls
- 被淘汰的假設

---

## Compaction 也需要 Verification

如果 Summary 會影響後續執行，它本身就值得驗證。

可以檢查：

- 是否保留所有 Constraint
- 是否保留未完成 Task
- 是否保留重要 Error
- 是否錯誤標記完成
- 是否遺失 Resource Scope
- 是否產生原文不存在的結論

如果 Agent 的 Context 很長，錯誤 Summary 可能比 Context Overflow 更危險。

因為它會讓後面的模型自信地建立在錯誤狀態上。

---

## Observation 的生命週期不同

不是每個 Tool Result 都應該用同一套保留策略。

例如：

### File Read

可能保留摘要與 File Reference。

### Search Result

可能只保留最相關證據與 Source。

### Test Log

失敗時保留關鍵 Error。

成功後只需要保留：

> Tests passed.

### Database Query

保留聚合結論與 Query Reference。

### Build Output

成功時高度壓縮。

失敗時保留錯誤附近內容。

Context Management 最好了解資料類型，而不是把所有 Message 當成相同字串。

---

## Failures 通常比 Success 更值得保留

Agent 成功執行十次讀檔，通常不需要保留十份完整輸出。

但一次重要失敗可能會影響很多後續決策。

因此 Context Priority 可以考慮：

```markdown
Unresolved Failure
>
Current Evidence
>
Current Plan
>
Recent Success
>
Resolved History
```

這不是絕對規則。

但很符合 Production Agent 的需求。

---

## Context Budget 不應該只看 Token 上限

假設模型支援 200K Tokens。

系統不一定要等到 195K 才開始 Compaction。

因為還要預留：

- 下一次模型輸出
- 新 Tool Result
- 新 Skill
- Verification
- Replan
- Subagent Result

所以可以設定 Soft Budget。

例如：

```markdown
Model Limit
200K

Target Working Context
120K

Reserve
80K
```

一旦超過 120K，就開始：

- Evict
- Compact
- Move Artifacts External
- Drop Low-Relevance Content

這樣系統不會每次都在 Overflow 邊緣工作。

---

## Context Management 和 Storage 應該分離

Context 不應該成為唯一資料來源。

如果所有狀態只存在 Prompt 裡，一次 Compaction 就可能造成不可逆資訊損失。

更好的方式是：

```markdown
Storage
保存完整資料

Context Manager
選擇目前需要的 Working Set

Model
根據 Working Set 做下一步決策
```

Storage 可以很大。

Context 應該保持精簡。

這也是為什麼：

> Bigger Context Window 不等於 Better Memory。

---

## 不要讓模型自己決定所有 Eviction

模型可以協助判斷語意相關性。

但有些內容不能由模型自由刪除。

例如：

- System Policy
- User Constraint
- Permission Boundary
- Pending Approval
- Budget
- Verified Failure
- Required Output Schema

這些比較適合由程式標記為 Pinned。

可以把責任拆成：

```markdown
程式碼
決定什麼不能丟

模型
協助判斷什麼目前最相關

Context Manager
執行 Selection、Compaction、Eviction
```

---

## Context Drift

Context 反覆 Summary 後，還有一個問題：

> Summary of Summary。

假設每 20 輪做一次摘要：

```markdown
Raw History
↓
Summary A
↓
再加 20 輪
↓
Summary B
↓
再加 20 輪
↓
Summary C
```

每次壓縮都可能丟失一點資訊。

最後 Context 和原始事實可能越來越遠。

這就是 Context Drift。

可以降低風險的方法：

- 關鍵事實獨立保存
- Constraint 不進自由摘要
- Summary 保留 Source Reference
- 定期從原始資料重新 Consolidate
- 不要無限 Summary of Summary

---

## Context Manager 應該知道任務階段

不同階段需要不同資訊。

例如 Coding Agent：

### Investigation

需要：

- Error Log
- Search Result
- Related Files

### Implementation

需要：

- Current Plan
- Target Files
- Constraints
- Relevant Interfaces

### Verification

需要：

- Changed Files
- Test Commands
- Expected Behavior
- Failure History

Investigation 的大量搜尋結果，到了 Verification 階段可能已經可以壓縮。

所以 Context Selection 也可以和 Plan State 結合。

---

## 常見的錯誤設計

### 1\. Context Window 大，所以全部保留

容量足夠不代表注意力沒有成本。

### 2\. 只刪最舊訊息

重要 Constraint 可能正好很舊。

### 3\. Summary 只追求短

結果把真正需要的資訊一起刪掉。

### 4\. 只 Load 不 Evict

最後仍然變成 Context Hoarding。

### 5\. Evict 後沒有 Reference

未來無法 Rehydrate。

### 6\. 所有 Tool Result 都用相同策略

不同 Observation 的價值與生命週期不同。

### 7\. Summary of Summary 無限進行

容易造成 Context Drift。

### 8\. 所有 Eviction 都交給模型

Policy、Constraint 與安全邊界不應該被模型自由移除。

---

## 一個簡單的 Context Lifecycle

可以把每一段資訊的生命週期整理成：

```markdown
Created
↓
Selected
↓
Active
↓
Compressed
↓
Evicted
↓
Rehydrated if needed
```

不是所有內容都會走完整流程。

有些直接 Pinned。

有些 Tool Result 用完就可以移出。

有些 Evidence 可能整個 Task 都要保留。

重點是：

> Context 應該有生命週期，而不是只會一直增加。

---

## 如何設計第一版 Context Manager？

可以先做四件事。

### 1\. Pinned Context

固定保留：

- Goal
- Constraints
- Current Plan
- Permission
- Pending Approval

### 2\. Recent Window

保留最近幾輪完整 Interaction。

### 3\. Compressed History

更舊內容轉成 Task Summary。

### 4\. External Artifacts

大型內容移到外部，只留下 Reference。

再設定 Soft Token Budget。

一旦超過 Budget，就優先：

1. 移除重複內容
2. 移除已解決成功紀錄
3. 壓縮舊 Observation
4. 移出大型 Artifact
5. 保留 Pinned Context

這已經比「超過上限就砍前半段」可靠很多。

---

## 今天新增了什麼能力？

Day 8 我們加入 Skill Discovery、Loading 與 Eviction。

今天把相同思想擴展到整個 Working Context：

- Selection
- Context Tier
- Compaction
- Eviction
- Stub
- Rehydration
- Soft Budget
- Protected Fields
- Context Drift Control

因此，Agent 不再把 Context 當作無限增長的聊天紀錄。

它開始主動管理：

> 下一輪模型真正需要看到什麼？

---

## 今天的結論

Context Management 不只是處理 Token Overflow。

真正的問題是：

> 哪些資訊值得持續佔用模型的注意力？

一個可靠的 Context Manager 至少要做到：

- Selection：選擇目前需要的內容
- Compaction：壓縮仍有價值的內容
- Eviction：移除暫時不需要的內容
- Rehydration：未來需要時重新載入

最重要的原則是：

> Storage 負責不遺失資料，Context 負責讓模型看見現在最重要的資料。

下一篇會進入 Memory：

> Context 是目前工作的狀態，那跨 Session 之後，哪些資訊才值得真的「記住」？

**完整系列與範例收錄於： [https://github.com/hardness1020/awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture)**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401893)

[

【AI Agent 08】Skill 越加越多，要怎麼讓 Agent 只在需要時才載入？ － Skills

](https://ithelp.ithome.com.tw/articles/10401893)[下一篇](https://ithelp.ithome.com.tw/articles/10401896)

[

【AI Agent 10】關掉視窗就全部忘光，Agent 要怎麼記得事情？ － Memory

](https://ithelp.ithome.com.tw/articles/10401896)

系列文

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121) 共 10 篇

目錄

1. 6
	[【AI Agent 06】Agent 列了一份計畫，計劃如何影響執行流程？ － Planning](https://ithelp.ithome.com.tw/articles/10401557)
2. 7
	[【AI Agent 07】事情太多做不完，多開幾個 Agent 就會比較快嗎？ － Subagents](https://ithelp.ithome.com.tw/articles/10401890)
3. 8
	[【AI Agent 08】Skill 越加越多，要怎麼讓 Agent 只在需要時才載入？ － Skills](https://ithelp.ithome.com.tw/articles/10401893)
4. 9
	[【AI Agent 09】聊到一半 Agent 開始忘東忘西，上下文該留什麼、丟什麼？ － Context Management](https://ithelp.ithome.com.tw/articles/10401895)
5. 10
	[【AI Agent 10】關掉視窗就全部忘光，Agent 要怎麼記得事情？ － Memory](https://ithelp.ithome.com.tw/articles/10401896)
[完整目錄](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/13/93b8e33d-d874-4f2c-af55-6bbfbb602804_small.jpg)

[Stacking Insight](https://itplus.ithome.com.tw/webinar-page/192?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

57 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/bea8b481-12fe-4f54-9b94-8d9e7474113a_small.png)

[企業轉型不焦慮 Gen 立 AI 新實力](https://itplus.ithome.com.tw/vod-page/883?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

23 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/16/dea9b9ed-1cef-42f6-b23f-52a575af395f.png)

[【知己也要知彼 - 強化資安韌性，做好數位風險防護！】](https://itplus.ithome.com.tw/vod-page/930?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/cb434532-d719-43c7-afe9-7a4960386a63_small.png)

[打造永續AI：從Kepler看見能耗，用GreenOps改善未來](https://itplus.ithome.com.tw/vod-page/1319?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

39 分