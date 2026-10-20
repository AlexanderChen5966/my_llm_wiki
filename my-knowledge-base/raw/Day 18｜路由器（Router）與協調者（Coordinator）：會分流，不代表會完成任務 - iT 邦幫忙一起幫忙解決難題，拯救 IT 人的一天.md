---
title: "Day 18｜路由器（Router）與協調者（Coordinator）：會分流，不代表會完成任務 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10405571"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "當系統只有兩三個工具時，最直覺的設計通常是先做一個 Router：看到數據問題就送到 Google Sheets，看到文件問題就送到 RAG，看到專案問題就送到 Trello 或 Jira。這種方式簡..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 19

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 19 篇

## Day 18｜路由器（Router）與協調者（Coordinator）：會分流，不代表會完成任務

- 分享至

  
當系統只有兩三個工具時，最直覺的設計通常是先做一個 Router：看到數據問題就送到 Google Sheets，看到文件問題就送到 RAG，看到專案問題就送到 Trello 或 Jira。這種方式簡單、容易理解，也很適合作為第一版。

但當問題開始跨來源，Router 很快就會不夠用。原因是它通常只回答一件事：「這個問題應該送去哪裡？」真正的任務卻可能需要先查 A，再根據 A 的結果決定是否查 B，最後還要整合不同來源，並判斷目前資料到底夠不夠。

這時候就需要另一個角色： **Coordinator（協調者）** 。

## Router 的角色比較像總機

Router 的工作主要是分類與分流。例如系統先判斷使用者輸入屬於哪一種類型，再交給對應 Tool。

```markdown
使用者問題
    ↓
Router
    ↓
┌─────────────────┬─────────────────┬─────────────────┐
│ data_query      │ document_query  │ project_query   │
│                 │                 │                 │
│ Sheets Tool     │ RAG Tool        │ Project Tool    │
└─────────────────┴─────────────────┴─────────────────┘
```

例如：

> 「今年台灣有多少工單？」

可以被分類成：

```markdown
data_query
```

接著直接送到 Google Sheets Tool。

另一個問題：

> 「公司的差旅政策怎麼規定？」

則可以分類為：

```markdown
document_query
```

再交給 RAG。

這類問題的共同特徵是： **一個問題通常對應一個主要 Tool，而且執行路徑很清楚。**

因此 Router 很適合處理單一意圖。它的好處是成本低、行為容易預測，也很方便測試。

## 跨來源問題，Router 很快就會遇到限制

假設使用者問：

> 「找出今年問題最大的市場，說明這個問題的正式定義，並確認是否有相關改善專案。」

如果 Router 必須從下面三個類別中選一個：

```markdown
data_query
document_query
project_query
```

它就會陷入困難，因為正確答案其實是：

> **三個都需要，而且還有執行順序。**

這個問題可能需要先查 Google Sheets：

```markdown
找出問題最大的市場
→ Hong Kong
```

接著再根據 `Hong Kong` 或 Top Issue 查正式文件：

```markdown
查詢問題定義
→ PDF / Confluence
```

最後才查相關改善專案：

```markdown
查詢 Project Status
→ Trello / Jira
```

也就是：

```markdown
使用者問題
    ↓
查數據
    ↓
取得結果
    ↓
根據結果查文件
    ↓
根據結果查專案
    ↓
整合回答
```

Router 可以決定「送去哪裡」，但它通常不負責管理這整段任務。

## Coordinator 比較像專案經理

如果把 Router 想成總機，Coordinator 就比較像一位專案經理。

它不只是把任務分派出去，而是需要持續掌握：

- 使用者真正想完成什麼
- 已經取得哪些資料
- 哪些資訊仍然缺少
- 下一步應該使用哪一個 Tool
- 是否可以沿用之前的結果
- 某個 Tool 失敗後要不要改變策略
- 目前是否已經足夠回答

可以把 Coordinator 的工作簡化成：

```markdown
理解任務
    ↓
查看目前已有資訊
    ↓
判斷還缺什麼
    ↓
選擇 Tool
    ↓
取得結果
    ↓
更新目前狀態
    ↓
任務完成了嗎？
    ↓
┌──────────────┬──────────────┐
│      否      │      是      │
│              │              │
│ 繼續下一步   │ 整合回答     │
└──────────────┴──────────────┘
```

這其實和前一天介紹的 ReAct 很接近。不同的是，今天我們開始把這個能力放回整體架構中，理解為什麼 Data Machi 需要一個 Coordinator 來管理多個 Tool。

## Coordinator 必須知道「目前已經知道什麼」

Coordinator 和 Router 最大的差異之一，就是它需要維持任務狀態。

假設使用者先問：

> 「今年哪個市場的需求問題最多？」

系統查到：

```markdown
Hong Kong
Request Count = 1,640
```

接著使用者只說：

> 「那相關專案呢？」

這句話本身其實資訊非常少。

如果當成一個全新的 Query：

```markdown
「那相關專案呢？」
```

系統根本不知道「那」到底指什麼。

但 Coordinator 如果保留前一輪資訊，就可以理解：

```markdown
Previous Result:
Top Market = Hong Kong

Current User Message:
「那相關專案呢？」

Resolved Intent:
查詢 Hong Kong 相關改善專案
```

接著再呼叫：

```markdown
get_project_status(
    market="Hong Kong"
)
```

這就是 Context 在 Coordinator 中的重要性。

## Context 不等於把所有聊天紀錄全部丟給模型

提到上下文，很容易想到「那就把整段 Conversation History 全部塞進 Prompt」。

但真正需要保存的，不一定只是完整對話文字。對 Coordinator 更有價值的是 **已經被確認過、後續仍然需要使用的任務資訊** 。

例如：

```markdown
Current Task:
分析今年需求問題

Selected Period:
2026 YTD

Top Market:
Hong Kong

Top Category:
Delivery Issue

Used Tools:
Google Sheets Tool
Document Tool

Pending:
Project Status
```

這種結構化資訊，比單純把前面二十輪對話全部交給模型更容易管理。

後面介紹 LangGraph State 時，我們會看到這些資訊如何進一步變成 Workflow 的 State。

## 使用者每講一句話，都需要重新查資料嗎？

Coordinator 還有一個很重要、但不容易被注意到的責任：

> **判斷現在到底需不需要重新工作。**

假設前一輪已經查到：

```markdown
Hong Kong Request Count = 1,640
Source = Google Sheets
Retrieved At = 16:00
```

使用者下一句說：

> 「幫我整理成表格。」

這時候沒有必要重新呼叫 Google Sheets。

Coordinator 應該知道目前只是 **呈現方式改變** ：

```markdown
Existing Result
    ↓
Reuse
    ↓
Format as Table
```

如果每次都重新查資料，不只增加 API Call 和等待時間，還可能因為資料剛好在兩次查詢之間更新，造成同一段對話前後數字不一致。

因此 Coordinator 除了知道「什麼時候做事」，也要知道：

> **什麼時候不要做事。**

## 可以把使用者訊息分成幾種不同類型

為了判斷需不需要重新查詢，可以先把常見訊息簡單分成幾類。

| 使用者訊息 | 類型 | Coordinator 的處理方式 |
| --- | --- | --- |
| 「今年哪個市場工單最多？」 | 全新問題 | 執行新的資料查詢 |
| 「那去年呢？」 | 延續追問 | 沿用原本問題，但修改時間條件並重新查資料 |
| 「整理成表格」 | 格式調整 | 沿用既有結果，不重新查 |
| 「換成英文」 | 呈現調整 | 沿用既有結果，只轉換語言 |
| 「你確定嗎？再查一次」 | 重新查核 | 強制重新查原始來源 |
| 「那香港呢？」 | 條件修改 | 保留任務，但更換市場後重新查 |
| 「換一個問題，看看會員數」 | 新查詢 | 建立新的任務狀態 |

這個分類其實不一定需要做得非常複雜，但它提醒我們：不同訊息代表的「工作量」完全不同。

例如：

```markdown
「整理成表格」
```

真正需要的是：

```markdown
Reformat
```

而不是：

```markdown
Re-query Google Sheets
→ Re-run RAG
→ Re-check Project
→ Generate Table
```

後者只是浪費資源。

## 什麼時候一定要重新查？

反過來，有些訊息則明確代表使用者需要最新資料。

例如：

> 「你確定嗎？再查一次。」

或：

> 「我剛剛更新 Sheet 了，重新確認。」

Coordinator 應該把這類訊息理解為：

```markdown
force_refresh = true
```

也就是即使 Cache 還沒有過期，也應該重新查詢 Source of Truth。

另一種情況是條件改變：

> 「那去年呢？」

如果前面的結果是 2026，這次必須重新使用：

```markdown
period = 2025
```

查詢，而不能只把「2026」文字替換成「2025」。

因此可以簡化成：

```markdown
只是改格式？
→ Reuse

只是翻譯？
→ Reuse

問題條件改變？
→ Re-query

要求重新確認？
→ Re-query

完全新問題？
→ New Task
```

## Router 與 Coordinator 怎麼一起工作？

Router 和 Coordinator 並不一定只能二選一。

一個實際系統中，很可能是 Coordinator 內部先使用 Router，快速判斷目前問題大概屬於哪一類，再決定後續行動。

例如：

```markdown
使用者問題
    ↓
Coordinator
    ↓
Router / Domain Classification
    ↓
初步判斷：
Project Query
    ↓
Coordinator
    ↓
目前資訊足夠嗎？
    ↓
需要先查文件定義嗎？
    ↓
選擇下一個 Tool
```

Router 可以提供一個初步的 **Domain Hint** ，但 Coordinator 才負責最後的執行策略。

這個差異很重要。

假設 Router 判斷：

```markdown
project_query
```

但 Project Tool 回傳：

```markdown
No project found
```

Coordinator 不應該因為第一步分類成 Project Query，就直接宣告「沒有專案」。它可以根據 Observation 改變方向，例如先去 Document Tool 查正式名稱。

因此：

> **Router 的判斷是建議，不一定是最終命令。**

## Domain Hint 是起點，不是絕對命令

假設使用者說：

> 「CDP 現在進度怎麼樣？」

初步 Router 很合理地會判斷：

```markdown
Domain = Project
```

接著查：

```markdown
get_project_status("CDP")
```

但結果：

```markdown
No Result
```

這時候 Coordinator 可以重新評估：

```markdown
可能原因：

1. 專案不存在
2. CDP 是縮寫
3. Project Tool 使用不同名稱
4. 目前沒有權限
```

如果 Tool Result 表示只是查不到名稱，Coordinator 可以改用：

```markdown
search_documents("CDP")
```

找到：

```markdown
CDP = Customer Data Platform
Project Name = Customer Data Platform Revamp
```

再重新查 Project Tool。

如果一開始的 Router 是硬規則：

```markdown
project_query
→ 只能用 Project Tool
```

第一次分類錯誤或資訊不足，就可能一路錯到底。

比較好的架構是：

```markdown
Router
→ 提供初步方向

Observation
→ 提供新的證據

Coordinator
→ 可以修正方向
```

這也延續了 Day 17 ReAct 的概念。

## Coordinator 需要保留哪些資訊？

走到這裡，可以先整理 Coordinator 最基本的 State。

至少可能包括：

| State | 用途 |
| --- | --- |
| Current Question | 目前使用者的問題 |
| Conversation Context | 最近對話與指涉關係 |
| Intent / Domain Hint | 初步判斷問題類型 |
| Tool History | 已使用哪些 Tool |
| Tool Results | 已取得哪些結果 |
| Query Conditions | 日期、市場、Category 等條件 |
| Sources | 結果來自哪裡 |
| Retrieved At | 資料取得時間 |
| Missing Information | 目前還缺什麼 |
| Task Status | 任務是否完成 |

例如：

```markdown
Current Question:
「那相關專案呢？」

Context:
Previous Top Market = Hong Kong

Domain Hint:
Project

Tool History:
query_google_sheets

Existing Result:
Hong Kong = 1,640 requests

Missing:
Improvement Project Status

Task Status:
IN_PROGRESS
```

有了這些資訊，Coordinator 才能知道下一步應該執行 Project Tool，而不是從頭重新查一次所有資料。

## Coordinator 還要知道任務是否真的完成

Router 的任務通常在分流之後就完成了：

```markdown
問題
 ↓
分類
 ↓
送到 Tool
 ↓
Router 工作結束
```

Coordinator 則需要持續追蹤到：

```markdown
使用者目標是否已經被滿足？
```

假設使用者問：

> 「找出需求最多的市場，並確認是否有相關改善計畫。」

目前只得到：

```markdown
Top Market = Hong Kong
```

即使 Sheets Tool 已經成功，整個任務仍然是：

```markdown
IN_PROGRESS
```

直到又取得：

```markdown
Improvement Project = Delivery Experience Improvement
Status = In Progress
```

才可以變成：

```markdown
COMPLETED
```

這也說明 Coordinator 關注的不是某一次 Tool Call 是否成功，而是：

> **使用者原本的任務是不是已經完成。**

## Router 與 Coordinator 怎麼選？

可以用下面這張表快速比較：

| 比較 | Router | Coordinator |
| --- | --- | --- |
| 主要任務 | 分類與分流 | 管理整段任務 |
| 常見問題 | 這題要去哪裡？ | 下一步應該做什麼？ |
| 是否保留中間結果 | 通常較少 | 需要 |
| 是否處理跨來源 | 有限 | 適合 |
| 是否觀察 Tool Result | 通常不需要 | 需要 |
| 是否重新決策 | 少 | 會 |
| 是否處理對話上下文 | 有限 | 需要 |
| 是否判斷 Reuse / Re-query | 通常不處理 | 需要 |
| 適合場景 | 單一意圖 | 多步驟、跨來源任務 |

如果每個問題都只需要一個明確 Tool，Router 通常就已經足夠。沒有必要因為 Agent 很熱門，就硬加上一層 Coordinator。

但當系統開始需要：

```markdown
多步驟
+
跨來源
+
前後文
+
結果重用
+
Observation
+
重新決策
```

Coordinator 的價值就會開始變得很明顯。

## Data Machi 為什麼不是一開始就做萬能 Coordinator？

回頭看整個系列的順序，其實是刻意的。

我們先建立：

```markdown
PDF RAG
```

再建立：

```markdown
Google Sheets Tool
```

接著才處理：

```markdown
Cross-source Workflow
```

然後才加入：

```markdown
Agent
ReAct
Coordinator
```

這個順序的原因，是希望每一項底層能力都能先被單獨驗證。

如果從 Day 06 就建立一個「萬能 Agent」，讓它自己處理 PDF、Sheets、Project、Memory 和 Verification，最後回答錯誤時，我們很難知道是哪一層有問題。

現在則可以清楚拆分：

```markdown
資料讀錯？
→ Tool

工具選錯？
→ Coordinator

前一步結果沒有帶到下一步？
→ State

沒必要卻重新查？
→ Reuse Policy

資料不足卻回答？
→ Completion / Verification
```

這也是為什麼企業 Agent 架構真正重要的不是「自動化程度有多高」，而是每一層責任是否清楚。

## 實務踩坑｜知道什麼時候「不做事」

Coordinator 最容易被忽略的一項能力，就是知道什麼時候可以直接沿用既有結果。

假設：

```markdown
16:00
使用者：
今年台灣工單多少？

結果：
328
```

16:01 使用者說：

> 「幫我整理成一句給主管看的話。」

這時候如果 Coordinator 再去查一次 Sheets，可能新資料剛好進來：

```markdown
16:01
New Result = 329
```

於是同一段對話會變成：

```markdown
第一輪：
328

第二輪：
329
```

使用者反而可能困惑：

> 「為什麼我只是叫你改格式，數字就變了？」

因此資料重用不只是節省成本，也和 **對話一致性** 有關。

比較合理的設計是回答：

> 「今年台灣目前共有 328 筆工單，是需求量最高的市場之一。」

如果使用者真的希望重新確認，則明確說：

> 「再查一次最新數字。」

這時才重新存取 Source of Truth。

---

> **今天的重點：**  
> Router 解決的是「這個問題應該送去哪裡」，Coordinator 解決的是「這個任務接下來應該怎麼做，直到使用者的目標真正完成」。好的 Coordinator 不只會選 Tool，還要知道目前已經知道什麼、什麼時候需要重新查，以及什麼時候其實什麼都不用做。

下一篇，我們會真正把 **Google Sheets Tool 與 PDF RAG 掛到同一個 Coordinator** 底下，讓使用者只需要用自然語言提出問題，不再需要自己決定應該使用哪一個工具。

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10405480)

[

Day 17｜推理與行動（ReAct）：代理如何看結果，再決定下一步？

](https://ithelp.ithome.com.tw/articles/10405480)[下一篇](https://ithelp.ithome.com.tw/articles/10405573)

[

Day 19｜實作：建立 Data Machi 協調者（Coordinator），讓 AI 自己選工具

](https://ithelp.ithome.com.tw/articles/10405573)

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

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/39d43552-9f45-4c2e-852b-cc37e0fad1dd_small.jpg)

[我們一起準時?下班吧！LINE 台灣開發團隊資安文化與技能養成](https://itplus.ithome.com.tw/vod-page/330?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/bea8b481-12fe-4f54-9b94-8d9e7474113a_small.png)

[企業轉型不焦慮 Gen 立 AI 新實力](https://itplus.ithome.com.tw/vod-page/883?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

23 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/267afab7-37d2-4e3f-808b-d01140a494c2_small.png)

[Azure Pipeline Agent 整合KEDA，提升持續交付彈性](https://itplus.ithome.com.tw/vod-page/1311?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/20/6278bae9-63a7-4abf-a34e-6583de0a8e05_small.png)

[GitHub Copilot 如何重構DevOps](https://itplus.ithome.com.tw/vod-page/1382?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

36 分