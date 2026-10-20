---
title: "Day 17｜推理與行動（ReAct）：代理如何看結果，再決定下一步？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10405480"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "昨天我們說到，代理（Agent）和固定工具使用（Tool Use）最大的差別，是 Agent 不只會選擇工具，還會根據工具回傳的中間結果重新判斷下一步。今天要介紹的 ReAct，就是一種很好理解這個行..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 18

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 18 篇

## Day 17｜推理與行動（ReAct）：代理如何看結果，再決定下一步？

- 分享至

昨天我們說到，代理（Agent）和固定工具使用（Tool Use）最大的差別，是 Agent 不只會選擇工具，還會根據工具回傳的中間結果重新判斷下一步。今天要介紹的 **ReAct** ，就是一種很好理解這個行為的方式：先判斷目前需要什麼資訊，接著採取行動取得真實資料，再觀察結果是否足夠，決定下一步要繼續查詢還是完成回答。

可以先把這個循環簡化成：

```markdown
Reason
判斷現在需要什麼
   ↓
Act
呼叫 Tool 執行
   ↓
Observe
檢查真實結果
   ↓
結果足夠嗎？
   ↓
┌──────────────┬──────────────┐
│      否      │      是      │
│              │              │
│重新判斷下一步│產生最終回答  │
└──────────────┴──────────────┘
```

這個概念看起來很簡單，但它其實解釋了 Agent 為什麼可以處理比固定 Workflow 更模糊的任務。真正關鍵的地方不是模型「想得比較多」，而是 **每一次取得新的外部資訊後，系統都有機會重新判斷原本的計畫是否還合理。**

## Reason：先決定現在最需要知道什麼

假設使用者問：

> 「哪個市場的需求問題最嚴重，而且相關改善專案有沒有在進行？」

如果使用固定 Workflow，我們可能事先寫好所有步驟；但 Agent 可以先判斷，目前第一個真正需要回答的是「哪個市場問題最嚴重」。在還不知道是哪個市場以前，其實沒有必要立刻搜尋所有市場的改善專案。

因此第一個 Reason 可以形成一個很簡單的決策：

```markdown
目前缺少：
問題最嚴重的市場

需要：
各市場需求數據

下一步：
呼叫 Data Tool
```

這裡所謂的 Reason，不是要求模型向使用者輸出一長串內部思考過程，而是讓系統形成 **下一個可執行的決策** ：需要哪一類資訊、應該使用哪個 Tool，以及目前是否已經具備回答所需的證據。

這一點很重要。企業 Agent 的價值不是產生更多「思考文字」，而是讓模糊的使用者需求逐漸轉成明確的執行動作。

## Act：真的去執行，而不是想像結果

完成判斷後，下一步就是 **Act** 。系統實際呼叫 Google Sheets Tool、Database Tool 或其他資料來源，取得各市場的需求數量。

例如：

```markdown
query_request_data(
    period="2026 YTD",
    group_by="market",
    metric="request_count"
)
```

Tool 可能回傳：

```markdown
Taiwan: 1,280
Hong Kong: 1,640
Macau: 320

Source: Google Sheets
Retrieved At: 2026-08-27 10:30
```

這一步非常重要，因為 Agent 所依據的事實應該來自 Tool，而不是由模型自己補出來。模型可以判斷「應該比較各市場需求量」，但真正的 1,280、1,640 與 320 應該來自資料來源與程式計算。

因此可以延續前幾天建立的分工：

> **模型負責判斷需要知道什麼，Tool 負責取得真實結果。**

Tool Result 最好也盡可能保持結構化，例如市場、件數、時間範圍、查詢時間與來源，而不是先由 Tool 包裝成一大段自然語言。結構越清楚，後面的 Observation 與下一步決策通常越容易維持一致。

## Observe：取得結果之後，重新看一次問題

假設 Data Tool 回傳：

```markdown
Taiwan: 1,280
Hong Kong: 1,640
Macau: 320
```

這時 Agent 就得到了一個新的事實： **Hong Kong 是目前需求工單最多的市場。**

但使用者原本不只問哪個市場最嚴重，還問：

> 「相關改善專案有沒有在進行？」

因此 Observation 不是單純把 Tool Result 保存起來，而是要重新比較：

```markdown
目前已知道什麼？
→ Hong Kong 的需求工單最多

原本的任務完成了嗎？
→ 還沒有

還缺什麼？
→ Hong Kong 相關改善專案的狀態

下一步？
→ 查 Project Tool
```

接著 Agent 才執行：

```markdown
get_project_status(
    market="Hong Kong",
    topic="customer request improvement"
)
```

這就是 ReAct 真正有價值的地方： **下一個 Tool Call 的輸入，是從前一個 Tool Result 中得到的，而不是在一開始就全部猜好。**

## Observation 不只是判斷「有沒有資料」

更有趣的情況，是 Tool 回傳的結果不符合原本預期。

假設使用者問：

> 「CDP 改版進度如何？」

Agent 一開始可能判斷這是專案問題，因此直接呼叫：

```markdown
get_project_status(
    project_name="CDP"
)
```

但 Tool 回傳：

```markdown
No project found.
```

如果系統沒有 Observation，可能直接回答：

> 「目前沒有找到 CDP 專案。」

但這個結論其實太快了。「找不到 CDP」不一定代表專案不存在，也可能只是 Trello 或 Jira 使用完整名稱，而不是縮寫。

Agent 可以重新觀察目前狀態：

```markdown
查不到 CDP
      ↓
CDP 可能是內部縮寫
      ↓
需要先知道正式名稱
      ↓
查 Document Tool
```

接著：

```markdown
search_documents("CDP")
```

得到：

```markdown
CDP = Customer Data Platform

Related project:
Customer Data Platform Dashboard Revamp
```

這時再執行：

```markdown
get_project_status(
    project_name="Customer Data Platform Dashboard Revamp"
)
```

最後才取得真正的專案進度。

整個過程可以整理成：

```markdown
Reason
需要查 CDP 專案進度
      ↓
Act
查 Project Tool
      ↓
Observe
沒有找到 CDP
      ↓
Reason
可能需要先確認縮寫
      ↓
Act
查 Document Tool
      ↓
Observe
找到正式專案名稱
      ↓
Reason
現在可以重新查專案
      ↓
Act
查 Project Tool
      ↓
Observe
取得專案狀態
      ↓
Final Answer
```

這就是 ReAct 和單純 Tool Calling 很重要的差異。

## Observation 可能會改變下一個「問題」

前面的例子也帶出一個重要觀念：Observation 不一定只是決定「下一個要用哪個 Tool」，它甚至可能改變下一輪查詢本身。

假設第一次搜尋只知道：

```markdown
Top Category = Delivery Issue
```

第二輪可以根據這個結果，把原本模糊的問題轉成：

```markdown
「Delivery Issue 的正式定義是什麼？」
```

如果文件搜尋又發現：

```markdown
正式名稱：
Last Mile Delivery Issue
```

第三輪查 Project Tool 時，就可以進一步改成：

```markdown
「是否有和 Last Mile Delivery Issue 相關的改善專案？」
```

可以看到，問題是在執行過程中逐漸變得更具體：

```markdown
原始問題
      ↓
取得第一個事實
      ↓
形成更精確的第二個問題
      ↓
取得第二個事實
      ↓
形成更精確的第三個問題
```

這種「觀察 → 補充資訊 → 重新拆題」的能力，才是 Agent 適合處理模糊企業任務的重要原因之一。

## Tool 失敗也是一種 Observation

Observation 不只發生在 Tool 成功的情況下。Timeout、Permission Denied、查不到資料，甚至回傳格式錯誤，本身也都是 Agent 可以觀察的結果。

例如：

```markdown
Project Tool
    ↓
Permission Denied
```

這時 Agent 不應該把 Permission Denied 解讀成：

> 「沒有相關專案。」

因為這兩件事情代表完全不同的狀態。

比較正確的判斷應該是：

```markdown
目前不知道專案是否存在
原因：
沒有權限取得資料
```

最後回答使用者：

> 「目前已確認 Hong Kong 是需求工單最多的市場，但專案系統回傳權限不足，因此暫時無法確認相關改善專案狀態。」

這也再次說明，Observation 的目的不是單純「讀 Tool Result」，而是理解這個結果對目前任務代表什麼。

## ReAct 不代表模型可以無限循環

既然 Agent 可以在 Observation 之後再次 Reason，就會出現一個新的風險：如果模型一直覺得「還可以再查一點」，是不是就可能永遠執行下去？

答案是肯定的，因此真正的企業系統不能只把 ReAct Loop 寫進 Prompt，還需要在程式層設定明確的執行邊界。

例如：

| 限制 | 用途 |
| --- | --- |
| Maximum Steps | 限制最多進行幾輪決策 |
| Maximum Tool Calls | 限制 Tool 呼叫總次數 |
| Timeout | 限制整個任務執行時間 |
| Allowed Tools | 限制 Agent 可以使用的工具 |
| Cost Budget | 控制模型與 API 使用成本 |
| Stop Conditions | 定義哪些情況必須停止 |

例如可以設定：

```markdown
Maximum Steps = 5
Maximum Tool Calls = 5
Timeout = 30 seconds
```

當 Agent 已經嘗試多個來源，仍然找不到答案時，正確的結果不應該是繼續無限查詢，而是停止並告訴使用者目前缺少什麼。

例如：

> 「目前已查詢文件與專案系統，但仍無法確認『CDP Growth Project』所指的專案。請提供完整專案名稱或相關文件，我才能繼續查詢。」

這不是 Agent 失敗，而是正常的停止條件。

## 什麼時候應該停止？

除了達到最大步數之外，Agent 至少還可以有幾種常見停止條件。

### 已經取得足夠證據

例如：

```markdown
問題：
香港需求最多嗎？
相關改善專案進度如何？

已取得：
✓ 各市場需求數字
✓ Hong Kong 為最高
✓ 對應專案名稱
✓ 最新 Project Status
```

這時候就應該停止搜尋，開始回答，而不是因為還有 Confluence、PDF 或其他 Tool 可以使用，就繼續查更多資料。

### 缺少必要資訊，需要使用者確認

例如找到兩個專案：

```markdown
Customer Data Platform Revamp
Customer Data Platform Migration
```

兩者都可能是使用者說的「CDP 專案」。這時候最合理的下一步不是再猜，而是詢問使用者指的是哪一個。

### 沒有權限或來源不可用

如果必要資料來源目前無法存取，就應該保留已成功的部分，說明限制，而不是無止境換工具碰運氣。

### 已經超過合理成本

如果回答一個簡單問題已經用了十幾次模型與 API 呼叫，即使最後可能找得到答案，整體設計也需要重新檢討。

因此 ReAct 的完整概念其實不只是：

```markdown
Reason → Act → Observe → Repeat
```

而更接近：

```markdown
Reason
  ↓
Act
  ↓
Observe
  ↓
是否完成？
  ↓
┌─────────────┬─────────────┐
│     否      │     是      │
│             │             │
│是否還能繼續？│ Final Answer│
└─────────────┴─────────────┘
       ↓
  ┌──────────┬──────────┐
  │   可以   │   不行   │
  │          │          │
  │下一輪    │停止 /    │
  │Reason    │要求確認  │
  └──────────┴──────────┘
```

## ReAct 和固定 Workflow 的差異在哪裡？

現在也可以再回頭和 Day 15 的 Workflow 比較一次。

固定 Workflow 可能是：

```markdown
Sheets
  ↓
Document
  ↓
Project
  ↓
Answer
```

不論中間發生什麼事情，基本路徑都事先定義好了。

ReAct Agent 則更像：

```markdown
查 Sheets
  ↓
Observe
  ↓
決定下一步
  ↓
查 Document
  ↓
Observe
  ↓
決定是否需要 Project
  ↓
可能查 Project
  ↓
Observe
  ↓
回答
```

差異不在 Tool 的數量，而在於：

> **執行路徑是事先固定，還是會根據 Observation 動態改變。**

這也是為什麼不是所有問題都需要 ReAct。固定、重複且高風險的流程，仍然適合使用明確 Workflow；只有當中間結果真的可能改變下一步時，ReAct 才能提供額外價值。

## 從 ReAct 開始，也更需要留下執行紀錄

當 Workflow 是固定的，我們原本就知道系統理論上會按照什麼順序執行。但 Agent 開始根據 Observation 動態改變路徑後，如果沒有留下執行紀錄，發生錯誤時會很難還原到底發生了什麼。

因此至少可以記錄：

```markdown
Step 1
Tool: query_google_sheets
Status: Success

Step 2
Observation:
Hong Kong has highest request count

Step 3
Tool: get_project_status
Input: Hong Kong
Status: No Result

Step 4
Tool: search_documents
Input: Hong Kong improvement project
Status: Success

Step 5
Tool: get_project_status
Input: Delivery Experience Improvement
Status: Success
```

這裡不需要保存模型所有內部推理文字，而是保存 **真正影響系統行為的決策與 Tool 執行軌跡** 。對除錯、成本追蹤與企業稽核來說，這些資訊通常比一段模糊的「Agent 思考過程」更實用。

## 為什麼後面還需要 LangGraph？

目前我們把 ReAct 畫成一個循環：

```markdown
Reason
 ↓
Act
 ↓
Observe
 ↓
Reason
```

當流程很簡單時，使用一般 Agent Loop 就可以處理。但隨著 Data Machi 後面開始加入更多條件，例如工具失敗重試、需要使用者確認、結果驗證、跨來源查詢與最大步數限制，所有狀態如果都藏在一個 Loop 裡，很快就會變得難以理解。

後面介紹 LangGraph 時，我們會把這些隱藏的執行流程進一步拆成：

```markdown
State
目前知道什麼

Node
現在要執行什麼

Edge
接下來要去哪裡
```

也就是把 Agent 原本隱藏的動態循環，逐步轉換成比較容易觀察、控制與測試的 Workflow。

## 實務踩坑｜Agentic 不代表可以無限嘗試

ReAct 很容易讓人覺得：「既然 Agent 可以自己修正，那就讓它一直試到找到答案為止。」

但在企業環境中，這通常不是好的設計。每多一次嘗試，都可能增加模型成本、API 成本、等待時間與錯誤風險，而且某些問題本來就無法靠多查幾次解決。

可以把它想成你把一項模糊任務交給同事時，可能會說：

> 「先查主要資料來源，如果找不到，再確認另一份文件；最多查三個地方，如果還是不確定，就回來告訴我缺什麼。」

而不是：

> 「一直找，找到答案才能回來。」

企業 Agent 需要的也是同樣的原則： **允許它根據結果調整策略，但同時清楚限制可以走多遠。**

---

> **今天的重點：**  
> ReAct 的核心不是「模型會思考」，而是 Agent 在每一次行動之後，都會觀察 Tool 回傳的真實結果，再判斷原本的任務是否已經完成，或是否需要改變下一步。真正可靠的 Agent 不只是會循環，也知道什麼時候應該停止。

下一篇，我們會比較兩個很容易混在一起的角色： **Router 與 Coordinator** 。Router 的工作比較像把問題送到正確入口；Coordinator 則需要持續觀察整段任務，決定下一步，直到問題真正被完成。

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10405442)

[

Day 16｜什麼時候工具使用（Tool Use）才真正變成代理（Agent）？

](https://ithelp.ithome.com.tw/articles/10405442)[下一篇](https://ithelp.ithome.com.tw/articles/10405571)

[

Day 18｜路由器（Router）與協調者（Coordinator）：會分流，不代表會完成任務

](https://ithelp.ithome.com.tw/articles/10405571)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/b4f56e63-ce06-45c8-939d-4ff5fe0a6d6c_small.png)

[從 0 邁向 1，落地微服務導入經驗談](https://itplus.ithome.com.tw/vod-page/540?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/7/12/de852606-66fc-4932-8522-74c09aba6dff_small.jpg)

[打造絢麗網頁，CSS動畫這樣玩起來](https://itplus.ithome.com.tw/vod-page/850?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

35 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/13/af82df28-d4f2-4c2f-8f3f-f0a1ef51469a_small.png)

[如何盡量避免 Throttling 在 K8s 中](https://itplus.ithome.com.tw/vod-page/910?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/13/b6b2aa22-a1bf-4c00-9838-25f14862a151_small.png)

[那些年，我們一起踩過的坑 - EKS 五年企業級實戰分享](https://itplus.ithome.com.tw/vod-page/913?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

30 分