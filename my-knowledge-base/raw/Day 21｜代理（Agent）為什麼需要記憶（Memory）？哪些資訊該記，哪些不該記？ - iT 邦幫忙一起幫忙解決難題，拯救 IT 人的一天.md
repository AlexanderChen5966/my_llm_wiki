---
title: "Day 21｜代理（Agent）為什麼需要記憶（Memory）？哪些資訊該記，哪些不該記？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10406864"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "做到這裡，Data Machi 已經可以選擇工具、跨來源查詢，也開始能處理一段完整的工作流程。但只要使用者開始多輪追問，新的問題就會出現：系統到底應該記住多少資訊？ 假設使用者第一句問：「今年哪一類工..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 22

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 22 篇

## Day 21｜代理（Agent）為什麼需要記憶（Memory）？哪些資訊該記，哪些不該記？

- 分享至

做到這裡，Data Machi 已經可以選擇工具、跨來源查詢，也開始能處理一段完整的工作流程。但只要使用者開始多輪追問，新的問題就會出現： **系統到底應該記住多少資訊？**

假設使用者第一句問：「今年哪一類工單最多？」系統查詢後得到 `Delivery Issue` 。下一句使用者只說：「那去年呢？」如果 Agent 完全沒有記憶，它甚至不知道「那」指的是哪一個問題；但另一個極端，是把所有歷史訊息、Tool Result 與模型回答無限塞回 Prompt，這樣不只會增加 Token、成本與等待時間，也可能讓過去已經失效的資訊干擾現在的判斷。

因此 Memory 真正要解決的問題不是「如何把所有東西記住」，而是：

> **現在這一輪工作真正需要哪些上下文？哪些資訊可以安全沿用？哪些資訊雖然以前查過，但現在必須重新確認？**

整體可以先理解成：  

## Memory 不是把聊天紀錄全部留下來

談到 AI Memory 時，很容易直接想到 Conversation History，也就是把前面所有使用者與模型的對話重新放進下一次 Prompt。但企業 Agent 真正需要的 Memory 往往不只一種，而且不同資訊的用途完全不同。

至少可以先區分三類。

| Memory 類型 | 主要用途 | 例子 |
| --- | --- | --- |
| Recent Conversation | 理解近期追問、代名詞與上下文 | 「那去年呢？」中的「那」 |
| Summary Memory | 壓縮較長對話，保留核心任務背景 | 目前正在分析工單趨勢 |
| Tool Result Memory | 保存真正查詢回來的事實 | Google Sheets 查到的數字 |

Recent Conversation 比較偏向「理解語言」，例如知道「那個市場」、「剛才那個專案」到底指的是什麼；Summary Memory 則是在對話變長後，保留真正仍然有用的背景，避免一直把數十輪聊天全部送回模型；Tool Result Memory 則完全不同，它保存的是外部系統真正查詢回來的資料。

這三者如果全部混成一段自然語言，很快就會產生一個問題：

> **這句話到底是使用者說的、模型推測的，還是 Tool 真正查到的？**

因此企業 Agent 的 Memory 最好不是只有一個「聊天紀錄」欄位，而是依照資訊性質分開管理。

## Conversation ID：先知道現在是哪一段工作

多輪對話的第一個基礎，是每一段對話都有明確的 **Conversation ID** 。Backend 收到新訊息時，應該只讀取這一段對話需要的上下文，而不是把同一個使用者過去所有工作一次載入。

例如：

```markdown
Conversation ID:
conv_001

User:
今年哪一類工單最多？

Result:
Delivery Issue
```

下一輪：

```markdown
Conversation ID:
conv_001

User:
那去年呢？
```

Coordinator 就知道這句追問屬於前一個任務。

但如果另一段對話是：

```markdown
Conversation ID:
conv_002

User:
幫我整理昨天會議的 Action Items
```

兩段工作就不應該共享相同的短期 Context。

這對多人環境更重要。如果 Conversation、Session 或 User Context 沒有隔離好，就可能發生 A 使用者的查詢結果被 B 使用者沿用，這不只是回答錯誤，而可能直接變成資料權限問題。

因此 Memory 的第一條原則不是「記住」，而是：

> **先確認這份記憶到底屬於誰、哪一段 Conversation，以及哪一個工作任務。**

## Recent Conversation：讓 Agent 聽得懂追問

Recent Conversation 的主要用途，是讓 Agent 理解使用者沒有重複講完整條件的追問。

例如第一輪：

> 「2026 年 7 月台灣共有多少筆需求？」

系統取得：

```markdown
Period:
2026-07

Market:
Taiwan

Request Count:
328
```

第二輪使用者只問：

> 「那其中完成的呢？」

這句話真正代表的是：

```markdown
沿用：
Period = 2026-07
Market = Taiwan

新增條件：
Status = Completed
```

如果沒有前文，這個問題根本無法獨立理解。

因此 Recent Conversation 不一定需要保存很長。只要能讓 Coordinator 正確解讀目前的省略語句、代名詞與延續條件，就已經發揮作用。

但要注意，「理解前文」不代表「直接沿用前一個答案」。在這個例子中，因為使用者增加了 `Status = Completed` ，仍然需要重新查詢 Data Tool，只是查詢條件可以從前文延續。

這也是 Memory 與 Cache 很容易被混淆的地方： **記住條件，不代表一定沿用結果。**

## Summary Memory：長對話不需要全部重播

如果一段 Conversation 已經進行了 30 輪，每次都把所有訊息完整送進模型，Prompt 很快就會膨脹。

這時可以使用 **Summary Memory** ，把較長對話整理成一份較精簡的工作背景。例如前面可能經過十幾輪討論：

```markdown
使用者正在分析 2026 年需求工單。

已確認：
- 目前分析範圍為 Taiwan 與 Hong Kong
- 主要關注 Delivery Issue
- 使用者希望比較 2025 與 2026
- 數據來源為 Google Sheets
- 定義來源為 Customer Service Taxonomy.pdf
```

這份 Summary 的目標不是保存每一句話，而是保留後面仍然會影響工作的資訊。

可以把它理解成：

```markdown
完整對話
    ↓
抽取仍然重要的工作背景
    ↓
Summary Memory
```

如果前面有一段閒聊、重複確認或已經失效的條件，就不需要永久留在 Summary 中。

因此好的 Summary Memory 應該不斷回答：

> **哪些資訊之後還會影響下一步？**

而不是：

> 「怎麼把整段聊天縮短一點？」

## Tool Result Memory：保存真正查回來的事實

第三類 Memory 是 Data Machi 特別重要的一層： **Tool Result Memory** 。

假設 Google Sheets Tool 查詢：

```markdown
Query:
Year = 2026
Market = Taiwan
Metric = Request Count
```

得到：

```markdown
Request Count = 328
```

如果只讓模型回答：

> 「2026 年台灣共有約 330 筆需求。」

然後 Memory 裡只留下這句自然語言，後面就會失去原始精度。

更理想的方式，是保存：

```markdown
Tool:
google_sheets

Query:
year = 2026
market = Taiwan
metric = request_count

Raw Result:
328

Source:
ticket_data

Queried At:
2026-09-01T21:30:00+08:00
```

這樣下一輪如果使用者說：

> 「台灣比香港多幾 %？」

Coordinator 可以使用真正的 `328` 進行計算，而不是使用模型先前說的「約 330」。

因此對數字、日期、人名、狀態與 ID 這類事實，應該遵守一個原則：

> **原始 Tool Result 是事實層，模型整理後的回答只是 Presentation Layer。**

## 不要讓模型自己的回答變成新的事實來源

這也是 Memory 很容易出現的一個風險。

假設第一輪模型錯誤地回答：

> 「專案截止日期是 9 月 30 日。」

即使 Tool 原始結果其實沒有 Deadline，如果下一輪系統只是把前面的 Assistant Message 當成 Memory，就可能開始把「9 月 30 日」視為既定事實。

第三輪使用者再問：

> 「距離 Deadline 還有幾天？」

系統甚至可能繼續用這個錯誤資訊計算。

因此最好把 Conversation Message 與 Tool Result 分開：

```markdown
Conversation
├─ User Message
├─ Assistant Message
│
└─ Tool Results
   ├─ Tool Name
   ├─ Query
   ├─ Raw Result
   ├─ Source
   └─ Queried At
```

如果 Assistant Message 和 Tool Result 不一致，後續應該優先相信可驗證的 Source，而不是模型曾經說過什麼。

這能避免 Memory 逐漸把一次生成錯誤放大成後續多輪對話中的「假事實」。

## 哪些資訊可以直接沿用？

Memory 真正困難的地方，不是保存，而是決定 **Reuse 還是 Re-query** 。

例如前一輪已經從 PDF 找到：

> 「Delivery Issue 是指配送延遲、缺件與配送狀態異常。」

下一輪使用者說：

> 「可以用比較簡單的方式解釋嗎？」

這時通常可以沿用既有 Document Result，因為使用者只是修改表達方式。

```markdown
Existing Document Result
        ↓
Reuse
        ↓
Simplify wording
```

再例如：

> 「幫我整理成三點。」

或：

> 「換成英文。」

這些也屬於 Presentation Change，通常不需要重新查 Source。

可以先整理成：

| 使用者要求 | 建議行為 |
| --- | --- |
| 換成英文 | 沿用結果 |
| 整理成表格 | 沿用結果 |
| 縮短成三點 | 沿用結果 |
| 用簡單方式解釋 | 沿用結果 |
| 引用剛才的來源 | 沿用已保存 Metadata |

這些操作不改變事實條件，因此重新查詢通常只會增加成本與不一致風險。

## 哪些資訊一定要偏向重新查詢？

另一類情況則不應直接依賴舊 Result。

例如：

> 「現在專案進度呢？」

> 「今天銷售多少？」

> 「最新庫存呢？」

這類問題中的資料本身會隨時間變動。

即使前一輪五分鐘前才查過，使用者如果明確要求：

> 「再查一次最新狀態。」

系統也應該重新查 Source of Truth。

可以用兩個問題快速判斷：

1. **這份資訊本身是否可能變動？**
2. **使用者是否明確要求最新、現在或重新確認？**

只要其中一個答案是「是」，就應該提高 Re-query 的優先級。

例如：

| 資料 | 一般新鮮度 |
| --- | --- |
| 公司政策定義 | 相對穩定 |
| 歷史報告 | 穩定 |
| KPI Definition | 相對穩定 |
| 專案狀態 | 可能快速變動 |
| 庫存 | 高度動態 |
| 今日銷售 | 高度動態 |
| 工單狀態 | 持續變動 |

因此 Memory 不只要知道「以前查過什麼」，也要知道：

> **這個結果現在還新鮮嗎？**

## Memory 也應該有保存期限

可以把 Tool Result 想成超市裡不同保存期限的食品。有些資料可以放很久，有些很快就過期。

例如：

```markdown
Policy Definition
TTL = 24 hours
```

或：

```markdown
Project Status
TTL = 5 minutes
```

又或者：

```markdown
Inventory
TTL = 1 minute
```

這裡的 TTL（Time To Live）只是示意，不代表所有企業都應該使用相同數字。真正應該根據資料更新頻率、業務風險與使用者對即時性的期待決定。

例如一份每季才更新的政策文件，沒有必要每 30 秒重新查一次；但物流狀態或庫存，如果沿用昨天的 Result 回答「現在還有多少」，就很容易造成錯誤。

因此可以把 Reuse 判斷整理成：

```markdown
有舊 Result？
    ↓
是否仍在有效期限？
    ↓
┌──────────────┬──────────────┐
│      是      │      否      │
│              │              │
│使用者要求最新？│重新查詢      │
└──────────────┴──────────────┘
       ↓
┌──────────────┬──────────────┐
│      是      │      否      │
│              │              │
│重新查詢      │安全沿用      │
└──────────────┴──────────────┘
```

這樣 Memory 才不會變成另一種 Cache Bug。

## 「那去年呢？」到底應該記什麼？

現在回到最一開始的例子。

第一輪：

> 「今年哪一類工單最多？」

Tool Result：

```markdown
Period:
2026

Top Category:
Delivery Issue

Count:
328
```

第二輪：

> 「那去年呢？」

Coordinator 不應該直接沿用：

```markdown
Delivery Issue = 328
```

因為使用者修改的是時間條件。

它應該從 Memory 中取出：

```markdown
Original Task:
找出工單最多的 Category

Original Period:
2026
```

再重新組成：

```markdown
Task:
找出工單最多的 Category

Period:
2025
```

然後重新呼叫 Data Tool。

所以這一輪真正沿用的是：

> **問題結構與其他條件。**

而不是：

> **舊的數據結果。**

這個差異很重要。

## 「那它的定義呢？」則是另一種跨 Tool 追問

第一輪：

> 「今年哪一類工單最多？」

得到：

```markdown
Top Category:
Delivery Issue
```

下一輪使用者問：

> 「那它的定義呢？」

這次 Memory 要解決的是代名詞：

```markdown
它
=
Delivery Issue
```

接著 Coordinator 判斷，這是一個文件知識問題，因此呼叫：

```markdown
search_documents(
    query="Delivery Issue definition"
)
```

這就是 Memory 與 Coordinator 合作的地方。

Memory 提供：

```markdown
目前使用者指的是誰 / 什麼條件
```

Coordinator 則決定：

```markdown
下一步應該用哪個 Tool
```

## 「幫我整理成三點」則完全不需要重新查

如果前一輪已經有：

```markdown
Top Category:
Delivery Issue

Definition:
...

Project Status:
In Progress
```

使用者下一句只是：

> 「幫我整理成三點。」

這時 Coordinator 應該直接使用 Existing Results：

```markdown
Existing Results
    ↓
Reformat
    ↓
3 Bullet Points
```

而不是重新執行：

```markdown
Sheets
→ Document
→ Project
```

這不只是節省 API Call，也能保持 Conversation Consistency。

## 第一版可以先測三種追問

如果要驗證 Data Machi 的 Memory 是否開始真正工作，不需要一開始就做很複雜的 Long-term Memory。先把三種最常見追問做好，就已經非常有價值。

### 第一類：修改條件

第一輪：

> 「2026 年 7 月共有多少筆需求？」

第二輪：

> 「那 6 月呢？」

應該：

```markdown
沿用：
Metric = Request Count
其他 Scope

修改：
Month = June

→ Re-query
```

### 第二類：跨 Tool 延續

第一輪：

> 「哪一類需求最多？」

第二輪：

> 「那它的定義呢？」

應該：

```markdown
沿用：
Top Category

切換 Tool：
Data Tool → Document Tool
```

### 第三類：只調整呈現方式

第一輪：

> 「哪個市場問題最多？相關專案如何？」

第二輪：

> 「幫我整理成三點。」

應該：

```markdown
Reuse Existing Results
→ 不重新查詢
```

如果這三類都能穩定處理，Memory 就已經不只是「保存聊天紀錄」，而是真正開始參與工作上下文的管理。

## 可以用四輪測試完整驗證 Memory

實際測試時，可以設計一段固定 Conversation。

### Round 1：完整問題

> 「2026 年 7 月台灣共有多少筆需求？」

系統應該正常查詢 Data Tool，並把 Query Conditions 與 Result 存進目前 Conversation State。

### Round 2：省略條件的追問

> 「那其中完成的呢？」

Coordinator 應該理解：

```markdown
Period = 2026-07
Market = Taiwan
Status = Completed
```

並重新查詢。

### Round 3：資訊不足的模糊問題

> 「那另一個呢？」

如果目前上下文無法確定「另一個」指市場、Category 還是 Status，就應該 Clarify，而不是自行猜測。

### Round 4：要求最新資料

> 「我剛更新資料了，再查一次最新結果。」

即使上一個 Result 還存在 Memory，系統也應該：

```markdown
force_refresh = true
```

重新呼叫 Source Tool。

這四輪分別測試：

```markdown
Context Resolution
Condition Reuse
Clarification
Freshness / Re-query
```

如果全部正常，Memory 才真的開始支援工作流程。

## 常見失敗模式：記太多與記太少都會出問題

Memory 並不是越多越好。

### 保存太多

如果每一輪都把完整對話、所有 Tool Results、所有模型回答全部送回模型，Prompt 會越來越長。結果可能是：

- Token 成本增加
- 回答延遲增加
- 舊條件干擾新問題
- 模型注意力被無關資訊分散

例如 Conversation 已經從 Taiwan 切換到 Hong Kong，但早期大量 Taiwan 資訊仍然存在 Prompt，就可能造成新的 Query Parameter 判斷錯誤。

### 保存太少

另一個極端是只保存最後一句訊息。

使用者問：

> 「那去年呢？」

系統只能看到：

```markdown
Current Message:
那去年呢？
```

完全不知道前面在比較什麼，就只能要求使用者重講一次。

這會讓多輪對話失去價值。

因此好的 Memory 策略不是：

```markdown
Remember Everything
```

也不是：

```markdown
Remember Nothing
```

而是：

```markdown
保留仍然影響任務的資訊
+
需要時重新取得事實
```

## 另一個高風險錯誤：不同 Conversation 互相污染

如果系統把不同 Conversation ID 的 Memory 混在一起，會產生非常難察覺的錯誤。

例如：

```markdown
Conversation A
Market = Taiwan
```

另一段：

```markdown
Conversation B
Market = Hong Kong
```

使用者在 Conversation B 問：

> 「那去年呢？」

結果系統錯誤抓到 Conversation A 的 Context，就可能跑去查 Taiwan。

因此 Memory Key 至少可能需要包含：

```markdown
User ID
+
Conversation ID
```

如果工作權限更複雜，還可能需要：

```markdown
Organization
Workspace
Session
```

這也是為什麼 Memory 在企業產品裡不只是 LLM Feature，同時也是資料與權限架構的一部分。

## Memory 和 LangGraph State 有什麼關係？

前幾天我們已經開始多次提到 State。其實目前整理的很多 Memory 資訊，之後都可以成為 LangGraph State 的一部分。

例如：

```markdown
State
├─ current_question
├─ conversation_id
├─ recent_context
├─ task_summary
├─ query_conditions
├─ tool_results
├─ sources
├─ queried_at
├─ missing_information
└─ task_status
```

當 Workflow 執行時，不同 Node 可以讀取需要的 State，再把新結果寫回去。

例如：

```markdown
Coordinator Node
    ↓
讀取 recent_context
    ↓
理解「那去年呢？」
    ↓
更新 query_conditions
    ↓
Data Tool Node
    ↓
重新查詢
    ↓
更新 tool_results
```

這樣 Memory 不再只是聊天介面的一個功能，而會成為整個 Agent Workflow 中的一部分。

## 實務踩坑｜「最新」是 Memory 最容易犯錯的地方

使用者通常最容易發現的一種錯誤，就是：

> 「我明明叫你查現在最新狀態，你卻回答昨天的資料。」

這種錯誤會非常傷信任。

因此只要使用者出現：

- 最新
- 現在
- 目前
- 剛剛
- 我剛更新
- 再查一次
- 重新確認

這類語句時，Coordinator 都應該提高 Re-query 的優先級。

例如：

```markdown
User:
「目前 Project Status 呢？」

Existing Memory:
Status = In Progress
Queried At = 3 days ago
```

這時不應該直接沿用。

而是：

```markdown
Project Tool
→ Re-query
→ Updated Status
```

好的 Memory 不是讓 AI 少查資料，而是：

> **讓 AI 知道什麼時候可以不用重做，以及什麼時候絕對不能偷懶。**

---

> **今天的重點：**  
> 好的 Memory 不是「記得越多越好」，而是把不同用途的 Context 分開管理：近期對話用來理解追問，Summary 保留長任務背景，Tool Result 保存可驗證的事實。真正重要的是知道 **什麼可以沿用、什麼需要修改條件後重新查詢，以及什麼資訊已經過期，不能再當成現在的答案。**

下一篇，我們會處理另一個很真實的問題： **當資訊不足時，Agent 到底應該自己再查一次，還是先問使用者？** 這會帶我們進入 Clarification，也就是讓 Agent 知道什麼時候「不要猜」。

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10405767)

[

Day 20｜一個問題要查三個來源，代理（Agent）應該怎麼安排？

](https://ithelp.ithome.com.tw/articles/10405767)[下一篇](https://ithelp.ithome.com.tw/articles/10407137)

[

Day 22｜不知道答案時，代理（Agent）應該追問、重查，還是拒答？

](https://ithelp.ithome.com.tw/articles/10407137)

系列文

[Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI](https://ithelp.ithome.com.tw/users/20169646/ironman/9207) 共 23 篇

目錄

1. 19
	[Day 18｜路由器（Router）與協調者（Coordinator）：會分流，不代表會完成任務](https://ithelp.ithome.com.tw/articles/10405571)
2. 20
	[Day 19｜實作：建立 Data Machi 協調者（Coordinator），讓 AI 自己選工具](https://ithelp.ithome.com.tw/articles/10405573)
3. 21
	[Day 20｜一個問題要查三個來源，代理（Agent）應該怎麼安排？](https://ithelp.ithome.com.tw/articles/10405767)
4. 22
	[Day 21｜代理（Agent）為什麼需要記憶（Memory）？哪些資訊該記，哪些不該記？](https://ithelp.ithome.com.tw/articles/10406864)
5. 23
	[Day 22｜不知道答案時，代理（Agent）應該追問、重查，還是拒答？](https://ithelp.ithome.com.tw/articles/10407137)
[完整目錄](https://ithelp.ithome.com.tw/users/20169646/ironman/9207)

[![.](https://itadstatic.ithome.com.tw/B9/1787886940_6a90fd5c11c52.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/20/0ac0a5b1-6150-475d-a775-4a622e682061_small.png)

[架構師也要 DevOps - 談服務模型的持續交付](https://itplus.ithome.com.tw/vod-page/767?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

51 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/4030b9f5-13ea-4f0d-becf-ea140471545b_small.png)

[在 Nutanix 混合多雲平台運行任何應用程式、資料、AI、Kubernetes](https://itplus.ithome.com.tw/vod-page/878?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/2f39ec89-fae9-41ea-8a96-bc97d11be2b7_small.png)

[用六頂思考帽挑戰敏捷框架，思考 AI 世代的敏捷未來](https://itplus.ithome.com.tw/vod-page/1069?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

43 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/2/5/3c32986b-66c3-4896-b66a-d2e0234cf09b.png)

[Kubernetes for Data Engineers: Building Scalable, Reliable Data Pipelines](https://itplus.ithome.com.tw/vod-page/1117?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

28 分