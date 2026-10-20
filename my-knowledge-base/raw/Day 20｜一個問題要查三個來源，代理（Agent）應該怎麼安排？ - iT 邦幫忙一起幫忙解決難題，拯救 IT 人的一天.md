---
title: "Day 20｜一個問題要查三個來源，代理（Agent）應該怎麼安排？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10405767"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "前一天我們已經讓 Coordinator 開始自行選擇工具。接下來真正困難的問題，不再是「還能接多少個 Tool」，而是：當一個任務同時需要多個資料來源時，這些工具應該按照什麼順序執行？ 真實企業工作..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 21

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 21 篇

## Day 20｜一個問題要查三個來源，代理（Agent）應該怎麼安排？

- 分享至

前一天我們已經讓 Coordinator 開始自行選擇工具。接下來真正困難的問題，不再是「還能接多少個 Tool」，而是： **當一個任務同時需要多個資料來源時，這些工具應該按照什麼順序執行？**

真實企業工作很少只呼叫一次 API 就結束。某些資訊彼此獨立，可以同時查詢；某些步驟則必須先取得前一步的結果，才能知道下一步要查什麼。如果 Agent 沒有處理好這些資料依賴，即使所有 Tool 都正常，最後仍然可能得到一個看起來完整、實際上執行順序錯誤的答案。

## 哪些 Tool 可以平行？哪些一定要按照順序？

判斷多工具流程時，第一個問題不是「有幾個 Tool」，而是：

> **下一個步驟的輸入，是否依賴前一個步驟的輸出？**

如果兩個任務彼此沒有依賴，就可以考慮 **Parallel Execution（平行執行）** 。例如使用者同時問：

> 「今年總共有多少工單？公司的差旅報銷規範是什麼？」

第一個問題需要查 Google Sheets，第二個問題需要查文件，兩者完全獨立，因此可以一起執行。

```markdown
使用者問題
    ↓
拆成兩個子任務
    ↓
┌──────────────────┬──────────────────┐
│ Google Sheets    │ Document Tool    │
│ 查今年工單總數   │ 查差旅報銷規範   │
└──────────────────┴──────────────────┘
    ↓
整合結果
```

平行執行的主要好處是降低整體等待時間。假設 Sheets 查詢需要 2 秒、Document Search 需要 3 秒，如果依序執行可能需要約 5 秒；如果兩個工作彼此獨立，同時執行則可以接近較慢的那一個，也就是約 3 秒。

但另一種問題：

> 「今年哪一類需求成長最高？這個類別的正式定義是什麼？」

就不能直接平行執行。第二個問題中的「這個類別」必須先知道第一步的結果，因此需要 **Sequential Execution（循序執行）** 。

```markdown
Google Sheets Tool
    ↓
找出成長最高的類別
    ↓
Top Category = Delivery Issue
    ↓
Document Tool
    ↓
查詢 Delivery Issue 的正式定義
    ↓
整合回答
```

因此可以用一個很簡單的原則判斷：  
  
這不只是效能問題。如果第二步還不知道真正的 Category，就先自行猜一個條件去查 Document Tool，最後即使每一個 Tool 都成功，整段流程仍然是錯的。

## 複雜問題要先拆成子問題

使用者不會按照 Tool 的結構說話。他不會說：

> 「請先執行 Google Sheets Tool，再執行 Document Tool，最後執行 Project Tool。」

更常見的問題會是：

> 「最近三個月核心指標為什麼下降？公司對這個指標的正式定義是什麼？改善專案做到哪裡？」

Coordinator 第一件事情不是呼叫 Tool，而是先進行 **Decomposition（問題拆解）** 。

例如可以拆成：

| 子問題 | 需要的能力 | 可能來源 |
| --- | --- | --- |
| 最近三個月指標怎麼變化？ | 精確數據與趨勢計算 | Google Sheets / Database |
| 這個指標正式怎麼定義？ | 文件知識搜尋 | PDF / Confluence |
| 是否已有改善專案？ | 專案狀態查詢 | Trello / Jira |

拆完之後，還需要檢查它們之間的依賴關係。如果「正式定義」只是針對一個已知 KPI，數據查詢和文件搜尋可以同時進行；但如果要先從數據中找出「下降最嚴重的 KPI」，後面的定義與專案查詢就必須等第一步完成。

因此真正的執行規劃不是：

```markdown
問題
→ Tool A
→ Tool B
→ Tool C
```

而比較像：

```markdown
問題
    ↓
拆成子問題
    ↓
判斷資料依賴
    ↓
決定 Parallel / Sequential
    ↓
執行 Tool
    ↓
整合結果
```

這也是 Coordinator 從「選 Tool」往「安排工作」前進的一步。

## 什麼是 Agentic Search？

傳統搜尋可以想成：

```markdown
問題
  ↓
搜尋一次
  ↓
找到資料
  ↓
回答
```

但企業任務常常沒有辦法只搜尋一次就得到完整答案。例如第一次查到某個指標異常，才知道下一步要查哪個市場；找到市場之後，又發現還需要正式專案名稱，才有辦法繼續查 Project Tool。

這種搜尋方式可以理解成 **Agentic Search（代理式搜尋）** ：

```markdown
拆問題
  ↓
搜尋
  ↓
觀察結果
  ↓
資訊足夠嗎？
  ↓
┌──────────────┬──────────────┐
│      否      │      是      │
│              │              │
│重新拆題或補查│整合回答      │
└──────────────┴──────────────┘
```

例如：

> 「找出最近表現最差的市場，確認是否有相關改善專案。」

第一次查詢：

```markdown
Data Tool
→ Hong Kong 表現最差
```

第二次查詢：

```markdown
Project Tool
query = Hong Kong
→ 沒有找到專案
```

Agent 觀察後發現，問題可能不是沒有專案，而是 Project Tool 不使用市場名稱作為索引。因此第三步可以改查文件：

```markdown
Document Tool
→ 找到 Hong Kong 改善計畫名稱：
HK Customer Experience Improvement
```

最後再查：

```markdown
Project Tool
query = HK Customer Experience Improvement
→ Status = In Progress
```

真正有價值的地方，不是系統多查了幾次，而是：

> **每一次新的搜尋，都建立在前一次已經取得的資訊之上。**

這就是 Agentic Search 和單次 Search 最大的差異。

## 一個完整案例：從會議錄音到行動項目

除了查詢資料，多步驟 Workflow 也可以用在「把非結構化輸入轉成真正工作成果」。

會議錄音就是一個很好的例子。

使用者真正想完成的通常不是：

> 「把音訊變成文字。」

而是：

> 「會議結束後，幫我整理決策、行動項目、Owner 和 Deadline，並讓後續工作可以追蹤。」

因此完整流程可能是：

```markdown
會議錄音
    ↓
音訊格式整理
    ↓
Speech-to-Text
    ↓
逐字稿
    ↓
LLM 結構化整理
    ↓
Summary / Decisions / Action Items
    ↓
建立 Project Tasks
    ↓
後續追蹤
```

可以看到，Speech-to-Text 只是一個 Tool。真正有價值的是整條 Workflow 把「一段會議」轉換成可以執行與追蹤的工作結果。

## 語音轉文字和內容整理是兩件不同的工作

以 Data Machi 為例，可以讓 Whisper 類型的語音模型負責 Speech-to-Text，再讓 Gemini 負責將逐字稿整理成會議摘要與行動項目。

兩個階段應該分開測試：

```markdown
Audio
  ↓
Speech-to-Text
  ↓
Transcript
```

先確認 Transcript 是否正確，再進入：

```markdown
Transcript
  ↓
Gemini
  ↓
Structured Meeting Result
```

如果逐字稿本身是空的、漏字很多或完全亂碼，問題就出在音訊或 Speech-to-Text；如果 Transcript 完整，但最後的 Summary 或 Action Items 不合理，才需要檢查後面的 LLM Prompt 與資料結構。

不要把這兩層一次包成：

```markdown
Audio
→ AI
→ 結果
```

否則最後錯誤時，很難知道是哪一個節點有問題。

## 音訊格式也是 Workflow 的一部分

瀏覽器錄音可能產生 WebM / Opus，手機上傳的音檔則可能是 M4A、AAC 或其他格式，而語音辨識 API 不一定支援所有輸入。

因此在 Speech-to-Text 前，可以先加入一個 Audio Pre-processing Node：

```markdown
Uploaded Audio
      ↓
檢查格式
      ↓
支援？
      ↓
┌────────────┬────────────┐
│    是      │     否     │
│            │            │
│直接轉錄    │ffmpeg 轉檔 │
└────────────┴────────────┘
      ↓
Speech-to-Text
```

這個步驟看起來和 AI 沒有太大關係，但它正好說明企業 Workflow 的一個重要觀念： **真正穩定的產品通常由 AI 與一般程式邏輯共同組成，而不是每一步都交給模型。**

第一版測試可以先準備一段約 10–20 秒、不包含真實姓名、客戶資料或公司機密的匿名音檔，再分別確認 Audio Processing、Speech-to-Text 與 LLM Structuring 三層。

## 逐字稿不要只叫模型「幫我整理」

如果只是把 Transcript 丟給模型並說：

> 「請幫我整理會議內容。」

每一次輸出可能都不太一樣，後續也很難自動化。

更好的方式是定義固定資料結構，例如：

```markdown
Summary
會議摘要

Decisions
已確認的決策

Action Items
需要執行的工作

Assignee
負責人

Due Date
截止日期
```

例如：

```markdown
Summary:
討論新版 Dashboard 上線安排。

Decisions:
9 月第一週開始進行 UAT。

Action Items:
更新 API 權限文件。

Assignee:
User A

Due Date:
2026-09-02
```

如果逐字稿沒有提到 Assignee，結果就應該是：

```markdown
Assignee: null
```

如果沒有說 Deadline：

```markdown
Due Date: null
```

而不是讓 AI 自己補上一個看起來合理的日期。

這裡的原則是：

> **結構化輸出讓後續自動化變得穩定；允許空值，則讓不知道的事情保持不知道。**

## Action Item 為什麼一定要保留來源？

假設 AI 從一場會議產生：

> 「更新 API 權限文件。」

如果直接把這句話建立成 Trello Card，幾週之後看到任務的人可能完全不知道它為什麼存在。

因此比較完整的 Action Item 可以保存：

```markdown
Task:
更新 API 權限文件

Source:
2026-08-28 Product Meeting

Transcript Reference:
00:12:35–00:13:10

Created At:
2026-08-28 15:30
```

如果原始 Transcript 也有 ID，還可以進一步保存：

```markdown
source_transcript_id = meeting_20260828_001
```

未來使用者問：

> 「為什麼有這個任務？」

系統才有機會回答：

> 「這個 Action Item 來自 8 月 28 日的 Product Meeting，在會議約 12:35 時確認需要更新 API 權限文件。」

這就是 Traceability。

它也讓 AI 不只停留在 Find、Understand、Decide 或 Act，還能進一步把工作成果連回 Track。

## 每一個 Workflow Node 都應該留下結果

多來源 Workflow 越長，就越不適合只保存最後回答。

例如一個會議流程可能有：

```markdown
Audio Processing
Speech-to-Text
Meeting Structuring
Task Creation
```

每一步最好保留：

| 資訊 | 用途 |
| --- | --- |
| Input | 這個步驟收到什麼 |
| Output | 實際產生什麼 |
| Source | 資料從哪裡來 |
| Status | Success / Failed |
| Timestamp | 什麼時間執行 |
| Error | 如果失敗，原因是什麼 |

例如：

```markdown
Node:
speech_to_text

Input:
meeting_audio_001.webm

Output:
transcript_001

Status:
SUCCESS

Duration:
4.8s
```

下一步：

```markdown
Node:
meeting_structuring

Input:
transcript_001

Output:
3 decisions
4 action items

Status:
SUCCESS
```

再下一步：

```markdown
Node:
task_creation

Input:
4 action items

Output:
3 tasks created
1 task skipped

Status:
PARTIAL_SUCCESS

Reason:
1 action item has no confirmed assignee
```

這些資料後面會成為 LangGraph State 與 Trace 的重要基礎。

## 一個來源失敗時，不一定要讓整條 Workflow 失敗

假設一個跨來源問題需要：

```markdown
Google Sheets
PDF RAG
Trello
```

執行結果是：

```markdown
Sheets → SUCCESS
RAG    → SUCCESS
Trello → TIMEOUT
```

Coordinator 可以保留前兩個結果，最後告訴使用者：

> 「目前已取得數據與正式定義，但 Project Tool 暫時無法回應，因此還不能確認改善專案的最新狀態。」

而不是：

> 「系統發生錯誤。」

這就是 Day 15 已經提過的 Partial Success。當 Workflow 變長之後，Partial Success 會更加重要，因為步驟越多，「每一個來源都同時成功」的機率自然越低。

## Agent 什麼時候應該停止查詢？

Agentic Search 最大的風險之一，就是模型永遠覺得：

> 「也許還可以再查一個來源。」

但企業工作流的目標不是把所有可能資料都查完，而是用足夠的證據完成使用者的任務。

可以先定義幾種停止條件。

### 已經取得問題所需的關鍵證據

例如使用者問：

> 「哪個市場問題最多？相關改善專案有沒有在進行？」

目前已經取得：

```markdown
✓ 各市場工單數
✓ 問題最大的市場
✓ 相關 Project Name
✓ 最新 Project Status
```

這時就應停止，而不是因為還有 Confluence 可以查，就繼續呼叫。

### 額外查詢不會明顯降低不確定性

如果三個正式來源都沒有說明專案延遲原因，再多查另一份不相關文件未必有價值。這時更適合明確回答：

> 「目前紀錄沒有提供延遲原因。」

### 已達到執行限制

例如：

```markdown
Maximum Steps = 5
Maximum Tool Calls = 6
Timeout = 30 seconds
```

達到限制後就應停止並回報目前狀態。

### 必要資訊只能由使用者提供

例如：

```markdown
找到兩個相似專案
↓
無法判斷使用者指哪一個
```

這時下一步不應再搜尋，而應 Clarify。

因此停止條件本身也是 Agent Decision Policy 的一部分。

## 階段實作四｜定義 Agent 的決策邊界

現在可以重新拿出 Day 15 建立的跨來源 Workflow。這一次先不要新增更多 Tool，而是替目前的 Agent 標記： **哪些判斷可以自由決定，哪些規則必須固定。**

可以先完成這張表：

| 決策項目 | 你需要定義的邊界 |
| --- | --- |
| 可用 Tool | Agent 可以選哪些 Tool？哪些永遠不可用？ |
| 問題拆解 | 一個問題最多可以拆成多少子任務？ |
| Parallel Execution | 哪些工作允許同時執行？ |
| Sequential Execution | 哪些輸出一定要先取得？ |
| Maximum Steps | 最多允許多少輪決策？ |
| Tool Calls | 最多允許多少次外部呼叫？ |
| Clarification | 缺少哪些必要條件時必須詢問？ |
| Stop Conditions | 取得哪些證據後必須停止？ |
| Partial Success | 某來源失敗時哪些結果仍可交付？ |
| Human Approval | 哪些 Action 必須取得人工確認？ |

接著用至少三種情境走一次整個流程。

### 情境一：所有資料正常

```markdown
問題
 ↓
拆題
 ↓
Tool A
 ↓
Tool B
 ↓
完成
```

確認每一步是否真的必要，以及是否有不必要的 Tool Call。

### 情境二：其中一個來源失敗

```markdown
Tool A → SUCCESS
Tool B → FAILED
Tool C → SUCCESS
```

確認系統是否能保留 Partial Result，而不是整體失敗。

### 情境三：問題缺少必要條件

例如：

> 「幫我看一下那個 Project。」

如果沒有任何前文能判斷是哪個 Project，就應該停止並 Clarify，而不是隨便挑一個專案。

在紀錄過程中，不需要保存模型所有內部思考，只需要記錄真正影響 Workflow 的內容：

```markdown
Selected Tool
Tool Result
Task Status
Next Action
Stop / Continue
```

例如：

```markdown
Step 1
Selected Tool: Google Sheets
Result: Hong Kong is highest

Step 2
Selected Tool: Project Search
Result: No project found

Step 3
Next Action: Search project definition

Step 4
Result: Project name resolved

Step 5
Selected Tool: Project Search
Result: In Progress

Task Status: COMPLETE
```

這些才是之後設計可觀察 Agent Workflow 真正需要的資訊。

> **階段成果：**  
> 保存這份「Agent 決策邊界」，包含可用 Tool、問題拆解限制、Parallel / Sequential 規則、最大執行步數、Clarification 條件、停止條件、Partial Success 與人工介入點。Day 25 會把這份決策規格進一步轉成 **State、Node、Edge 與 Failure Path** ，形成可以實際實作與驗收的 Workflow Blueprint。

## 延伸理解｜從固定 Parallel 到 Dynamic Fan-out

目前大部分例子在設計階段就知道要查幾個來源。例如：

```markdown
Sheets
+
Document
+
Project
```

因此可以事先決定哪些步驟平行、哪些循序。

但有些任務只有真正執行之後，才知道會產生多少下游工作。

例如一份長文件經過解析後，可能得到：

```markdown
8 個章節
```

系統想要替每一章產生摘要，就可能動態產生 8 個工作：

```markdown
Chapter 1 → Summary
Chapter 2 → Summary
Chapter 3 → Summary
...
Chapter 8 → Summary
```

這種模式稱為 **Dynamic Fan-out（動態分派）** 。

接著等待每一個工作完成後，再進行：

```markdown
8 個摘要
    ↓
Fan-in
    ↓
整份文件總結
```

另一個例子是 Agentic Research。系統可能先把一個研究問題拆成四個子題：

```markdown
Research Question
      ↓
┌────────┬────────┬────────┬────────┐
│ Topic A│ Topic B│ Topic C│ Topic D│
└────────┴────────┴────────┴────────┘
      ↓
平行研究
      ↓
彙整結果
```

這類模式特別適合彼此獨立的子任務。

但判斷原則仍然和前面完全相同：

> **只有彼此獨立的工作才適合 Parallel；只要後一步的輸入依賴前一步的輸出，就必須保留 Sequential。**

另外，即使工作邏輯上可以平行，也不代表技術上一定適合平行。還需要考慮 API Rate Limit、同一份資料的並行寫入、成本與 Transaction。

例如：

```markdown
同時建立 100 個 API Request
```

可能因為 Rate Limit 全部失敗；兩個 Agent 同時修改同一張 Project Card，也可能造成資料覆蓋。

因此真正的判斷不是：

> 「能不能平行？」

而是：

> **「這些工作彼此獨立嗎？外部系統允許這樣平行執行嗎？」**

## 從多工具開始走向真正的 Workflow

到 Day 20 為止，Data Machi 已經不只是：

```markdown
Coordinator
  ↓
選一個 Tool
```

而開始變成：

```markdown
理解任務
    ↓
拆成子問題
    ↓
判斷資料依賴
    ↓
Parallel / Sequential
    ↓
執行多個 Tool
    ↓
觀察中間結果
    ↓
必要時補查
    ↓
判斷是否停止
    ↓
整合結果
```

這時候也會開始出現新的問題：當流程跑到一半時，系統到底應該記住哪些資訊？哪些 Tool Result 可以沿用？哪些數據到了下一輪已經過期？使用者只說一句「那去年呢？」時，到底是沿用前面的條件，還是重新查詢？

這些問題已經不是 Tool Selection 可以單獨解決的，而會把我們帶到下一個主題： **Memory 與 State。**

---

> **今天的重點：**  
> 多工具架構真正困難的地方不是 Tool 數量，而是如何拆解問題、判斷資料依賴、安排 Parallel 與 Sequential Execution、保留中間結果，並知道什麼時候應該停止。Agent 的價值也不是呼叫更多 API，反而是在有限的決策邊界內，把多個能力組成真正可以完成工作的 Workflow。

下一篇，我們會進入多輪對話：當使用者接著問「那去年呢？」時，Agent 到底應該記住什麼？哪些資訊可以直接沿用，又有哪些資料必須重新查詢？

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10405573)

[

Day 19｜實作：建立 Data Machi 協調者（Coordinator），讓 AI 自己選工具

](https://ithelp.ithome.com.tw/articles/10405573)[下一篇](https://ithelp.ithome.com.tw/articles/10406864)

[

Day 21｜代理（Agent）為什麼需要記憶（Memory）？哪些資訊該記，哪些不該記？

](https://ithelp.ithome.com.tw/articles/10406864)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/13/98626f1d-bff1-4fd9-8c11-ab98c61e4aba_small.png)

[從數據看台灣企業雲原生採用大趨勢](https://itplus.ithome.com.tw/vod-page/902?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

34 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/82012903-d870-4cc8-95fe-7384a2f6d4e5_small.png)

[資料守護之道，混合雲備份引領前行](https://itplus.ithome.com.tw/vod-page/587?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/de169d45-4ac0-41cc-9d53-7cdb2e2c37fd_small.png)

[手把手搭建 Etcd 與 K8s Components 工作坊](https://itplus.ithome.com.tw/vod-page/1333?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

83 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/53c00416-e5f2-41f8-b086-754db8bb1ea9_small.png)

[告別傳統辦公室！Azure Virtual Desktop 打造高效、安全的雲端辦公體驗](https://itplus.ithome.com.tw/vod-page/1220?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

28 分