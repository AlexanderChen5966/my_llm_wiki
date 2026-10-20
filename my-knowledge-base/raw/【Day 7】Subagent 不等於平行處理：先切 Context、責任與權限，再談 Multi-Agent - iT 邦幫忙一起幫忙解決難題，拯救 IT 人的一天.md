---
title: "【Day 7】Subagent 不等於平行處理：先切 Context、責任與權限，再談 Multi-Agent - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401890"
author:
  - "[[iThome]]"
published:
created: 2026-08-10
description: "前一天我們談 Planning。 Planning 的核心不是列出漂亮步驟，而是把任務轉換成可以追蹤、驗證、重新規劃的執行狀態。 當 Plan 變得更大時，下一個自然問題通常是： 要不要把不同步驟交給..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 7

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 7 篇

## 【Day 7】Subagent 不等於平行處理：先切 Context、責任與權限，再談 Multi-Agent

- 分享至

前一天我們談 Planning。

Planning 的核心不是列出漂亮步驟，而是把任務轉換成可以追蹤、驗證、重新規劃的執行狀態。

當 Plan 變得更大時，下一個自然問題通常是：

> 要不要把不同步驟交給不同 Agent？

例如：

- 一個 Agent 負責 Research
- 一個 Agent 負責 Coding
- 一個 Agent 負責 Review
- 一個 Agent 負責 Testing

看起來很合理。

甚至很像把一個團隊搬進系統裡。

但這裡很容易出現另一個錯誤等號：

> 多一個 Agent = 多一份產能。

其實不一定。

如果兩個 Agent 讀取相同 Context、使用相同工具、重複理解相同問題，最後還需要另一個 Agent 合併結果，那你得到的可能不是平行化，而是：

- 更多 Token
- 更多延遲
- 更多協調
- 更多重複工作
- 更多不一致結果
- 更多權限風險

所以 Subagent 的第一個問題不應該是：

> 可以多開幾個 Agent 嗎？

而是：

> 哪一段 Context、責任與工具範圍，值得被獨立切出去？

今天我們會把 Subagent 拆成四個核心邊界：

1. Context Boundary
2. Responsibility Boundary
3. Capability and Permission Boundary
4. Result Contract

最後再談真正的 Parallelism。

---

## Subagent 是什麼？

最簡單的理解是：

> Subagent 是由另一個 Agent 或控制器啟動，負責完成一個更小任務的獨立 Agent Loop。

它通常有自己的：

- Prompt
- Context
- Tool Set
- Permission
- Budget
- State
- Stop Condition

Parent Agent 會把一個子任務交給它，等待結果，再決定下一步。

```markdown
Parent Agent
    ↓
拆出子任務
    ↓
Subagent
    ↓
獨立執行
    ↓
回傳結果
    ↓
Parent Agent 整合
```

這和單純呼叫一個 Tool 不同。

Tool 通常是固定能力。

Subagent 則有自己的推理循環。

例如：

```markdown
Tool
搜尋 GitHub

Subagent
找出這個 Repository 的主要架構模式，
必要時自行搜尋檔案、閱讀內容、比較多個模組，
最後回傳結論與證據
```

因此，Subagent 可以處理比單一 Tool Call 更開放的子問題。

但它也更昂貴。

---

## 為什麼不直接讓 Parent Agent 做完？

因為 Parent Agent 的 Context 是有限的。

一個大型任務可能同時包含：

- 使用者需求
- 程式碼
- 文件
- Tool Results
- Plan
- Error Logs
- Memory
- Previous Attempts

如果所有細節都塞在同一個 Loop 裡，Parent Agent 很容易被不重要資訊淹沒。

Subagent 最有價值的地方之一，就是：

> 把局部問題放進局部 Context。

例如 Parent Agent 只需要知道：

> 找出目前測試失敗的前三個主要原因。

Subagent 可以自己讀取數十個 Log、測試檔案與 Stack Trace。

最後只回傳：

- Root Cause A
- Root Cause B
- Root Cause C
- 對應證據
- 建議下一步

Parent Agent 不需要看到所有中間過程。

這就是 Context Isolation。

---

## 第一層：Context Boundary

Subagent 最重要的邊界之一，是它能看到哪些 Context。

最直覺的做法是：

> Parent 有什麼，就全部傳給 Child。

但這通常不是好設計。

因為 Parent Context 可能包含：

- 與子任務無關的歷史
- 使用者敏感資訊
- 其他工具輸出
- 其他 Agent 的中間結果
- 不必要的 Prompt
- 不應該下放的 Secret

所以更好的方式是建立最小 Context。

Child 只收到：

- 子任務目標
- 必要背景
- 可使用資源
- 成功條件
- 回傳格式

例如：

```markdown
Parent Task:
修正整個測試套件

Subagent Task:
只分析 payments 模組的失敗測試

Child Context:
payments 相關測試
最近失敗紀錄
相關來源檔案
成功條件

不需要：
其他模組紀錄
部署設定
Email 工具
完整使用者歷史
```

這樣做有三個好處：

1. Token 更少
2. 注意力更集中
3. 權限更容易限制

所以 Subagent 不只是 Parallelism。

它首先是一種 Context Management。

---

## 第二層：Responsibility Boundary

如果 Parent Agent 只是說：

> 幫我研究一下這個問題。

這個子任務仍然太模糊。

Subagent 可能：

- 重複 Parent 已做的事
- 擴大範圍
- 修改不該修改的東西
- 回傳大量無法整合的內容
- 自己重新定義問題

所以 Subagent 需要明確 Responsibility Boundary。

至少要定義：

- Goal: 要完成什麼？
- Scope: 只處理哪一部分？
- Expected Output: 要回傳什麼？
- Completion Condition: 什麼情況算完成？
- Forbidden Actions: 哪些事情不能做？

例如：

```markdown
Goal:
找出 API latency 上升的主要原因

Scope:
只分析過去 24 小時的 Trace

Expected Output:
最多三個候選原因，每個附上證據

Completion:
至少找到一個可驗證的瓶頸

Forbidden:
不能修改 Production 設定
不能重新部署
```

責任越清楚，Parent 越容易整合。

---

## 第三層：Capability 與 Permission Boundary

Day 4 我們已經談過：

> Capability 和 Permission 是兩件事。

這在 Subagent 更重要。

假設 Parent Agent 有：

- File Read
- File Write
- Shell
- Database
- Email
- Deployment

但它啟動的 Research Subagent 只需要：

- File Read
- Search

如果 Child 自動繼承 Parent 所有工具，就會把風險一起放大。

更合理的方式是：

```markdown
Parent Capability
    ↓
根據子任務縮小
    ↓
Child Capability
```

也就是 Delegated Scope。

例如：

```markdown
Parent:
Read + Write + Shell + Deploy

Research Subagent:
Read only

Testing Subagent:
Read + Test Runner

Review Subagent:
Read only

Deployment Subagent:
Deploy only after Approval
```

這就是 Least Privilege 在 Multi-Agent 系統中的延伸。

> 子 Agent 應該獲得完成子任務需要的最小能力，而不是繼承 Parent 的完整權限。

---

## 第四層：Result Contract

Subagent 完成後，要把什麼交回 Parent？

最差的做法是直接把完整 Conversation 塞回 Parent Context。

假設 Child 執行了 15 輪：

- 搜尋 20 個檔案
- 執行 8 次工具
- 產生大量中間假設
- 遇到 3 次錯誤
- 最後得到一個結論

如果全部回傳，Parent Context 很快就會被污染。

更好的方式是定義 Result Contract。

例如：

```markdown
Summary
最終結論

Evidence
支持結論的證據

Artifacts
必要的檔案或資料

Uncertainty
哪些地方仍不確定

Next Action
建議 Parent 做什麼
```

Parent 通常需要的是結果，不是 Child 的完整思考歷程。

這也能降低 Context Cost。

---

## Subagent 和 Tool 的差別

一個常見問題是：

> 什麼時候應該做成 Tool，什麼時候應該做成 Subagent？

可以用一個簡單判斷：

### 適合 Tool

輸入與輸出明確。

流程固定。

不需要多輪推理。

例如：

- 讀檔
- 搜尋
- 查詢資料庫
- 執行測試
- 呼叫 API

### 適合 Subagent

問題需要：

- 多步驟探索
- 多次工具呼叫
- 中途修正策略
- 局部 Context
- 獨立 Budget
- 獨立 Completion Condition

例如：

- 分析一個模組的架構
- 研究三種方案
- Debug 一個局部問題
- Review 一組大型變更

如果一個函式就能完成，不需要再放一個 LLM Loop。

---

## Subagent 不等於 Parallelism

很多架構一看到可以拆子任務，就立刻平行執行。

但平行只有在子任務真正獨立時才有價值。

適合平行的例子：

- 搜尋三個不同資料來源
- Review 三個獨立模組
- 評估三個候選方案
- 分析三組互不依賴的 Log

不適合平行的例子：

```markdown
先找 Root Cause
再決定修改方案
再執行修改
再驗證結果
```

這些步驟存在明確依賴。

如果全部同時啟動，後面的 Agent 只能靠猜。

所以在平行之前，要先判斷：

- 是否讀取相同資源
- 是否修改相同資源
- 是否存在前後依賴
- 是否需要共享最新狀態
- 是否會產生互相衝突的結果

Parallelism 是 Scheduling 問題。

Subagent 是 Responsibility Boundary 問題。

兩者不能畫上等號。

---

## 多 Agent 最容易出現的成本：Context Duplication

假設三個 Subagent 都需要了解同一份 30K Token 的專案背景。

那麼你可能不是花 30K Token。

而是：

```markdown
30K × 3
```

如果每個 Child 還會重複搜尋相同檔案，成本會繼續增加。

因此多 Agent 並不天然更有效率。

可以採取幾個方法降低重複：

### Shared Summary

Parent 先建立一份精簡背景，再下放給 Child。

### Scoped Retrieval

每個 Child 只讀自己需要的資料。

### Artifact Reference

大型資料存放在外部，只傳位置或索引。

### Result Compression

Child 只回傳結論與證據。

### Deduplicated Search

避免多個 Child 重複執行相同搜尋。

如果沒有這些機制，多 Agent 很容易只是把 Context 成本乘上 Agent 數量。

---

## 多 Agent 也會增加 Coordination Cost

假設兩個 Subagent 都提出不同修改方案。

Parent 需要判斷：

- 哪個結果比較可信
- 是否可以合併
- 是否互相矛盾
- 是否需要再開一個 Judge Agent
- 是否需要重新查證

Agent 越多，結果不一定越容易整合。

可以把總成本想成：

```markdown
Execution Cost
+
Context Cost
+
Coordination Cost
+
Verification Cost
```

如果 Coordination Cost 大於平行帶來的收益，多 Agent 反而更慢。

---

## 如何設計 Delegation？

Parent 在啟動 Subagent 前，可以建立一份 Delegation Contract。

至少包含：

- Task: 你要做什麼？
- Scope: 只處理什麼？
- Inputs: 你可以使用哪些資料？
- Tools: 你可以使用哪些能力？
- Budget: 最多可以花多少時間、Token 或 Tool Calls？
- Completion: 什麼條件算完成？
- Return: 你必須回傳哪些欄位？

Delegation 越明確，Parent 越容易控制與評估 Child。

---

## Parent 如何知道 Child 真的完成？

跟昨天 Planning 一樣：

> Child 說完成，不代表真的完成。

Parent 可以檢查：

- Result Contract 是否完整
- 必要 Evidence 是否存在
- Artifact 是否有效
- 子任務 Verification 是否通過
- 是否超出 Scope
- 是否產生未批准副作用

有些系統會加入 Judge。

但 Judge 也不一定要是另一個 LLM。

例如：

- JSON Schema
- Test Result
- File Existence
- Exit Code
- Source Count
- Constraint Check

如果可以用程式驗證，就不需要再多一個模型。

---

## Subagent 失敗時怎麼辦？

Child 可能：

- 超過 Budget
- 工具失敗
- 無法找到證據
- 權限不足
- 任務描述不清楚
- 結果不符合 Contract

Parent 可以有幾種策略：

- Retry: 同一個 Child 再嘗試一次
- Re-delegate: 重新縮小或修改任務
- Alternative Agent: 換另一個模型或能力
- Parent Takeover: Parent 自己接手
- Human Handoff: 交回使用者

重要的是不要讓 Child 無限自己擴大 Scope。

如果權限不足，它應該回報，而不是自行取得更多權限。

---

## Subagent 也需要 Budget

每個 Child 都應該有獨立限制：

- 最大輪數
- 最大 Token
- 最大 Tool Calls
- 最大時間
- 最大 Retry
- 可用工具
- 可用 Context

這能避免一個小子任務消耗整個 Agent 的資源。

Parent 也需要 Global Budget。

否則 Parent 啟動太多 Child 後，可能自己沒有資源完成整合。

---

## 常見的錯誤設計

### 1\. 一有複雜任務就加 Subagent

複雜不代表需要多 Agent。

先看是否真的存在獨立 Context 或責任邊界。

### 2\. Child 繼承 Parent 全部 Context

造成 Token 浪費，也可能洩漏不必要資訊。

### 3\. Child 繼承 Parent 全部工具

違反 Least Privilege。

### 4\. 所有 Subagent 都平行啟動

有依賴關係的任務會產生錯誤或過時結果。

### 5\. Child 回傳完整 Conversation

Parent Context 很快被污染。

應該使用 Result Contract。

### 6\. Parent 直接相信 Child 的「完成」

重要結果仍需要 Verification。

### 7\. 子任務沒有 Budget

一個 Child 可能無限制探索。

### 8\. 為了整合，再開更多 Agent

有時候只是在增加 Coordination Cost。

---

## 什麼時候值得使用 Subagent？

可以先問五個問題：

### 1\. 子任務是否能有清楚的責任範圍？

如果不能，很容易與 Parent 重疊。

### 2\. 子任務是否需要獨立 Context？

如果不需要，直接由 Parent 做可能更便宜。

### 3\. 子任務是否需要多輪推理？

如果只是固定操作，Tool 可能更適合。

### 4\. 結果是否能用明確 Contract 回傳？

如果結果難以整合，Coordination Cost 會很高。

### 5\. 子任務是否真的獨立到值得平行？

如果有強依賴，就不要為了「Multi-Agent」而平行。

---

## 一個簡單的判斷框架

```markdown
固定、明確操作
→ Tool

需要多步探索，但範圍不大
→ Parent Agent 直接處理

需要獨立 Context 與責任
→ Subagent

多個真正獨立的 Subagent
→ 可以考慮 Parallel

固定依賴流程
→ Workflow / Graph
```

這樣就不會把每個複雜問題都自動升級成 Multi-Agent。

---

## 今天新增了什麼能力？

Day 6 我們加入 Planning 與 Execution State。

今天加入：

- Delegation
- Context Boundary
- Responsibility Boundary
- Delegated Permission
- Result Contract
- Child Budget
- Parent Verification
- Parallelism Check

因此，Subagent 不再只是「再叫一個模型」。

它變成一個有明確輸入、權限、預算、責任與回傳格式的獨立執行單位。

---

## 今天的結論

Subagent 最有價值的地方，不是把 Agent 數量變多。

而是把一個大型問題切成更乾淨的邊界。

真正需要設計的是：

- Child 看什麼 Context
- Child 負責什麼
- Child 能使用哪些工具
- Child 擁有哪些 Permission
- Child 可以花多少 Budget
- Child 要用什麼 Contract 回傳
- Parent 如何驗證結果

最重要的原則是：

> 先切責任與 Context，再決定是否需要更多 Agent。

如果兩個任務沒有清楚邊界，多一個 Agent 通常只會增加 Coordination Cost。

下一篇會進入 Skills：

> Context 不夠用時，為什麼不是把所有知識全部塞進 System Prompt，而是讓 Agent 在需要時才載入能力？

**完整系列與範例收錄於： [https://github.com/hardness1020/awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture)**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401557)

[

【Day 6】Agent 的計畫如何真正影響執行？

](https://ithelp.ithome.com.tw/articles/10401557)

系列文

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121) 共 7 篇

目錄

1. 3
	[【Day 3】Agent 選了工具，誰負責真的執行？](https://ithelp.ithome.com.tw/articles/10401298)
2. 4
	[【Day 4】Agent 能使用工具，不代表它應該擁有所有權限](https://ithelp.ithome.com.tw/articles/10401436)
3. 5
	[【Day 5】擴充 Agent 驗證、紀錄與自訂行為](https://ithelp.ithome.com.tw/articles/10401629)
4. 6
	[【Day 6】Agent 的計畫如何真正影響執行？](https://ithelp.ithome.com.tw/articles/10401557)
5. 7
	[【Day 7】Subagent 不等於平行處理：先切 Context、責任與權限，再談 Multi-Agent](https://ithelp.ithome.com.tw/articles/10401890)
[完整目錄](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/cccef095-9dfa-42b9-866d-cee30889b77d_small.jpg)

[笑死，能關防毒幹嘛要做免殺呢？從令牌偽造到把防毒關進沙箱隔離](https://itplus.ithome.com.tw/vod-page/516?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

42 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/12/25/a958401f-7e69-4ada-94ac-9f817c0e4485_small.jpg)

[Netflix 如何用機器學習技術加速影片創作](https://itplus.ithome.com.tw/webinar-page/244?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/14/bfb5bc28-183a-4440-a1ef-aee3c6a7ea5b_small.png)

[Aruba 安全網路架構保護公共資料](https://itplus.ithome.com.tw/vod-page/649?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/ab69ec8c-ae9e-41a7-a787-6a99df4bc15a_small.jpg)

[把 AI 帶入你的資料．啟動完整端到端 GenAI 旅程](https://itplus.ithome.com.tw/vod-page/1231?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

35 分