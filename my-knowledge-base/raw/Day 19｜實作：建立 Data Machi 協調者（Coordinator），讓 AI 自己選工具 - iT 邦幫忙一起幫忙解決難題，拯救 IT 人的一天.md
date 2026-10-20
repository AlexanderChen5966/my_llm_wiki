---
title: "Day 19｜實作：建立 Data Machi 協調者（Coordinator），讓 AI 自己選工具 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10405573"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "前面我們已經建立 Google Sheets Tool、PDF RAG，也理解 Router 和 Coordinator 的差異。今天要把這些能力真正收進同一個協調層，讓使用者不需要知道底層有哪些工具..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 20

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 20 篇

## Day 19｜實作：建立 Data Machi 協調者（Coordinator），讓 AI 自己選工具

- 分享至

前面我們已經建立 Google Sheets Tool、PDF RAG，也理解 Router 和 Coordinator 的差異。今天要把這些能力真正收進同一個協調層，讓使用者不需要知道底層有哪些工具，更不需要自己選擇「我要查文件」還是「我要查試算表」。

這就是 Data Machi 第一版 Coordinator 的目標： **使用者只需要正常描述工作問題，系統負責判斷應該去哪裡找資料、按照什麼順序執行，以及最後如何整合結果。**

整體架構可以先理解成：

如果未來加入 Confluence、Trello 或其他資料來源，也只是讓 Coordinator 多出幾項可以選擇的 Tool，而不是要求使用者學習新的操作方式。

## Coordinator 不是另一個資料工具

Coordinator 本身不負責計算數字，也不負責搜尋 PDF。它更像一位管理整段任務的專案經理：先理解使用者想完成什麼，再把工作交給適合的 Tool，取得結果後檢查資訊是否足夠，最後才整理成回答。

例如使用者問：

> 「今年哪一類需求最多？」

Coordinator 應該知道這是一個需要精確統計的問題，因此交給 Google Sheets Tool。

如果使用者問：

> 「需求工單的正式定義是什麼？」

這時候需要的是文件知識，因此交給 Document Tool。

而如果使用者問：

> 「今年哪一類需求最多？那一類的正式定義是什麼？」

Coordinator 就不能只選其中一個 Tool，而需要先查 Sheets，取得 Top Category，再把這個結果傳給 Document Tool。

```markdown
使用者問題
    ↓
Coordinator
    ↓
Google Sheets Tool
    ↓
Top Category = Delivery Issue
    ↓
Coordinator
    ↓
Document Tool
query = "Delivery Issue definition"
    ↓
取得正式定義
    ↓
Coordinator
    ↓
整合回答
```

因此 Coordinator 真正負責的是 **任務決策與結果整合** ，而不是取代底層 Tool。

## 先把每個 Tool 的責任寫清楚

Coordinator 能不能正確選擇工具，很大一部分取決於 Tool Description 是否足夠清楚。

假設系統有兩個 Tool：

```markdown
Tool A
Description:
回答公司的問題

Tool B
Description:
查詢公司的資訊
```

對模型來說，兩者幾乎沒有明確差異。當使用者問「今年總工單多少」時，它可能隨機選到 Document Tool；問「工單如何定義」時，也可能跑去查 Google Sheets。

因此 Tool Description 應該清楚說明「什麼時候應該使用」以及「什麼時候不適合使用」。

例如：

```markdown
Tool Name:
query_google_sheets

Description:
當使用者需要查詢結構化資料、
精確數字、統計、篩選、分組或排序時使用。
不適合回答政策、制度與文件定義問題。
```

Document Tool 則可以定義為：

```markdown
Tool Name:
search_documents

Description:
當使用者詢問政策、定義、SOP、
手冊、歷史報告或其他文件知識時使用。
不適合計算最新營運數字。
```

Project Tool 則可能是：

```markdown
Tool Name:
get_project_status

Description:
當使用者詢問專案狀態、負責人、
截止日期、未完成任務或目前進度時使用。
```

Tool Description 不是單純寫給開發者看的文件，它同時也是 Coordinator 做決策的重要訊號。

## Tool 的責任最好不要彼此重疊太多

除了 Description，Tool 本身的能力範圍也應該盡量清楚。

假設有：

```markdown
search_everything()
```

這個 Tool 同時能搜尋 PDF、Google Sheets、Trello 和 Confluence，看起來很方便，但對 Coordinator 來說反而失去了選擇資料來源的能力，也很難知道最後答案真正來自哪裡。

比較好的方式是：

```markdown
query_structured_data()
search_documents()
get_project_status()
```

讓每個 Tool 都代表一種清楚的企業能力。

可以整理成：

| Tool | 負責的問題 | 不適合處理 |
| --- | --- | --- |
| Google Sheets Tool | 數字、篩選、統計、排序 | 政策與文件定義 |
| Document Tool | 規範、定義、SOP、文件內容 | 即時營運數字 |
| Project Tool | 任務、Owner、Deadline、狀態 | 正式政策定義 |

Tool Boundary 越清楚，Coordinator 做錯選擇的機率通常也越低。

## 第一版先測三種最基本的路由

Coordinator 第一版不用追求非常複雜的 Agent 行為。可以先確認三種最基本的問題是否能穩定處理。

### 第一類：純數據問題

例如：

> 「今年共有多少需求工單？」

預期流程：

```markdown
User
 ↓
Coordinator
 ↓
Google Sheets Tool
 ↓
Result
 ↓
Answer
```

### 第二類：純文件問題

例如：

> 「需求工單的正式定義是什麼？」

預期流程：

```markdown
User
 ↓
Coordinator
 ↓
Document Tool
 ↓
Result
 ↓
Answer
```

### 第三類：跨來源問題

例如：

> 「哪一類需求最多？那一類的正式定義是什麼？」

預期流程：

```markdown
User
 ↓
Coordinator
 ↓
Google Sheets Tool
 ↓
Top Category
 ↓
Coordinator
 ↓
Document Tool
 ↓
Definition
 ↓
Coordinator
 ↓
Answer
```

如果這三種類型都能穩定分流，就已經完成一個非常重要的基礎。此時再逐步加入更多 Tool，會比一開始就建立一個「什麼都能做」的 Agent 更容易驗證。

## Tool Result 不要只保存成自然語言

Coordinator 呼叫 Tool 之後，另一個重要設計是： **Tool Result 最好保存成結構化資料，而不是只留下文字。**

例如 Google Sheets Tool 可以回傳：

```markdown
{
  "tool": "google_sheets",
  "query": {
    "year": 2026,
    "group_by": "category",
    "metric": "request_count"
  },
  "result": {
    "top_category": "Delivery Issue",
    "count": 328
  },
  "source": "ticket_data",
  "queried_at": "2026-08-27T16:30:00+08:00"
}
```

Document Tool 也可以有類似結構：

```markdown
{
  "tool": "document_search",
  "query": "Delivery Issue definition",
  "result": {
    "definition": "...",
    "file": "Customer Service Taxonomy.pdf",
    "page": 8
  },
  "queried_at": "2026-08-27T16:30:05+08:00"
}
```

Coordinator 最後可以把這些結果轉換成自然語言，但系統內部仍然保留真正的原始資料。

這樣做至少有三個好處：第一，多輪對話時可以判斷舊結果能不能直接沿用；第二，Verification 可以重新比較回答與 Tool Result 是否一致；第三，發生錯誤時可以知道是哪一個 Tool 或哪一個步驟出了問題。

如果所有 Tool Result 最後都只被混成 Conversation History 裡的一段文字，很快就會難以區分：

> 哪一部分是 Tool 真正查到的？哪一部分又只是模型產生的？

## Coordinator 也需要留下執行軌跡

當只有一個 Tool 時，除錯通常很簡單。但 Coordinator 開始動態選工具之後，我們還需要知道一個問題實際走了什麼路徑。

例如可以保留：

```markdown
Request ID:
abc123

Step 1
Node: Coordinator
Decision: Google Sheets Tool

Step 2
Tool: Google Sheets
Status: SUCCESS
Duration: 1.2s

Step 3
Node: Coordinator
Decision: Document Tool

Step 4
Tool: Document Search
Status: SUCCESS
Duration: 0.8s

Step 5
Node: Coordinator
Decision: FINAL ANSWER
```

這就是 Trace 的概念。

如果現在還沒有使用 LangGraph 或其他視覺化 Trace 工具，也不需要因此停止實作。第一版先使用 Structured Log 記錄：

- Request ID
- 執行節點
- Tool Name
- Query Parameters
- Tool Result Status
- Duration
- Error Type

就已經很有價值。

例如：

```markdown
request_id=abc123
node=tool_execution
tool=google_sheets
status=success
duration_ms=1240
```

當系統日後開始出現「有時候會回答錯，但不知道為什麼」的問題時，這些紀錄會比單純閱讀最終回答更容易找出真正原因。

## Tool 失敗時，要知道是哪一層失敗

Coordinator 還需要處理一個很重要的情境：某個 Tool 沒有成功。

例如跨來源問題：

```markdown
Google Sheets → SUCCESS
Document RAG  → FAILED
```

比較不好的回覆是：

> 「抱歉，發生錯誤。」

因為使用者完全不知道已經成功取得哪些資訊。

比較好的處理方式是：

> 「今年需求最多的類別是 Delivery Issue，共 328 件。目前已取得數據結果，但文件搜尋暫時失敗，因此還無法確認這個類別的正式定義。」

這樣既保留已經成功取得的資訊，也清楚指出失敗來源。

系統內部可以記錄：

```markdown
google_sheets:
status = SUCCESS

document_search:
status = FAILED
error = INDEX_UNAVAILABLE
```

Coordinator 的責任之一，就是讓一個 Tool 的失敗不一定等於整個任務完全失敗。

這也延續 Day 15 提過的 **Partial Success** 概念。

## 先從單一 Coordinator 開始

當 Data Machi 開始有：

```markdown
Google Sheets
PDF RAG
Trello
Confluence
```

很容易冒出另一個想法：

> 「是不是每一個 Tool 都應該有自己的 Agent？」

例如：

```markdown
Data Agent
Document Agent
Project Agent
Knowledge Agent
```

然後再建立一個 Supervisor Agent 管理它們。

技術上當然可以，但第一版通常沒有必要。

如果目前所有 Tool 都可以由同一個模型理解，而且 Tool 數量還在可管理範圍，一個 Coordinator 通常更容易除錯：

```markdown
Coordinator
        /      |      \
       /       |       \
Sheets Tool  RAG Tool  Project Tool
```

相較之下，多 Agent 架構可能變成：

```markdown
Supervisor
       /      |       \
Data Agent  RAG Agent  Project Agent
    ↓          ↓          ↓
  Tool        Tool        Tool
```

每多一層 Agent，就會增加新的 Prompt、模型呼叫、狀態傳遞與錯誤來源。

因此現階段可以遵守一個簡單原則：

> **Tool 能解決的問題先做成 Tool；單一 Coordinator 能管理的問題，先不要拆成 Multi-Agent。**

## 什麼時候才可能需要拆成多個 Agent？

雖然現在不急著做 Multi-Agent，但還是可以先理解什麼情況下它可能開始有價值。

第一種是 **Tool 數量非常多，單一 Coordinator 的選錯率開始明顯上升** 。假設系統有 50 個 Tool，全部直接丟給同一個模型做選擇，可能開始難以穩定分類。

第二種是 **不同角色真的需要不同能力與 Prompt** 。例如一個角色專門分析資料，另一個角色專門做法務審查，而且兩者可以使用的資料與權限完全不同。

第三種是工作本身存在明確的角色交接，例如：

```markdown
Research
    ↓
Draft
    ↓
Review
    ↓
Reject / Approve
```

這類流程才可能值得把不同角色拆成獨立 Agent。

但如果只是：

```markdown
查 Sheets
查 PDF
查 Trello
```

通常三個 Tool 加上一個 Coordinator 已經足夠。

Multi-Agent 並不是 Agent 成熟度的指標，而只是某些架構問題的解法。

## 不要急著一次把所有資料來源加入

Coordinator 第一版可以只管理：

```markdown
Google Sheets Tool
+
PDF RAG
```

先確認兩個 Tool 都可以穩定分流，再逐步加入 Project Tool、Confluence Tool 或其他來源。

這樣做的原因很簡單。假設一開始就提供：

```markdown
Sheets
PDF
Confluence
Trello
Jira
Database
Web Search
```

使用者問：

> 「Delivery Issue 怎麼樣？」

結果 Coordinator 選錯 Tool 時，我們很難知道是 Description 寫得不好、工具太多、Prompt 不清楚，還是問題本身需要 Clarification。

如果只有兩個 Tool，測試與修正都會容易很多。

因此：

> **Tool 數量增加，不等於 Agent 更成熟。**

真正需要追蹤的是：

- Tool Selection 是否正確
- 執行順序是否合理
- 資料能否追溯
- 錯誤是否可觀察
- 使用者任務是否真的完成

## 模糊問題不能只是硬選一個 Tool

正常問題測試成功後，下一步要故意問一些模糊問題。

例如：

> 「最近 Delivery Issue 怎麼樣？」

這個問題可能至少有三種意思：

```markdown
1. 最近 Delivery Issue 有多少件？
→ Data Tool

2. Delivery Issue 是什麼？
→ Document Tool

3. Delivery Issue 改善專案進度如何？
→ Project Tool
```

如果前面對話已經提供足夠 Context，Coordinator 可以根據上下文理解使用者指的是哪一種。

但如果完全沒有 Context，就不應該只是看到 `Delivery Issue` 關鍵字，隨機挑一個 Tool。

更合理的處理可能是：

> 「你想看 Delivery Issue 最近的數量趨勢，還是相關改善專案進度？」

這就是 Clarification。

因此 Coordinator 的正確行為不只有：

```markdown
選 Tool A
選 Tool B
選 Tool C
```

還包括：

```markdown
目前資訊不足
→ 先追問
```

這也會是後面讓 Agent 變可靠的重要能力。

## 實務驗收｜Coordinator 到底有沒有選對 Tool？

Coordinator 完成後，最重要的不是看 Demo 能不能回答一兩題，而是建立一套固定 Routing Test。

這組測試不看回答文筆，而是檢查：

> **系統有沒有把問題送到正確的資料來源？需要多步驟時，有沒有按照正確順序執行？**

第一版可以從下面這組題目開始：

| 測試題目 | 預期路徑 |
| --- | --- |
| 今年共有多少需求工單？ | Google Sheets |
| 需求工單的正式定義是什麼？ | Document Tool |
| 哪一類需求最多？ | Google Sheets |
| 最多的那一類，正式定義是什麼？ | Google Sheets → Document |
| 相關改善專案目前做到哪裡？ | Project Tool |
| 哪個市場問題最多？相關專案進度如何？ | Google Sheets → Project Tool |
| 把剛才的結果整理成表格。 | 沿用既有結果，不重新查 |
| 你確定嗎？請重新查一次最新數字。 | 強制重新查 Source of Truth |

每一題可以先人工定義 Expected Path，再記錄 Coordinator 實際走的 Actual Path。

例如：

| ID | 測試題目 | 預期路徑 | 實際路徑 | 任務完成 |
| --- | --- | --- | --- | --- |
| ROUTE-01 | 今年共有多少需求工單？ | Sheets | Sheets | 是 |
| ROUTE-02 | 需求工單如何定義？ | Document | Document | 是 |
| ROUTE-03 | 哪類最多？它如何定義？ | Sheets → Document | Sheets → Document | 是 |

接著才可以開始計算 Routing Accuracy。

```markdown
Routing Accuracy
=
完全符合預期路徑的題數
÷
全部 Routing Test 題數
```

例如：

```markdown
20 題測試
18 題完全符合預期路徑

Routing Accuracy = 18 / 20 = 90%
```

但這裡要注意，複合問題只有「整條路徑都正確」才能算通過。

例如預期：

```markdown
Sheets → Document
```

實際卻只有：

```markdown
Sheets
```

即使第一個 Tool 選對了，也不能算完整通過，因為使用者的第二個子任務沒有完成。

## Routing Accuracy 不等於 Task Completion

另一個需要分開看的指標，是 **Task Completion** 。

假設 Coordinator 正確選擇：

```markdown
Sheets → Document
```

但 Document Tool 因為權限問題失敗。

這時：

```markdown
Routing = 正確
Task Completion = 失敗或部分成功
```

反過來，也可能剛好得到正確答案，但 Routing 過程並不穩定。

例如使用者問文件問題，Coordinator 卻先查了 Google Sheets，再查 Document，最後仍然得到正確答案。

```markdown
Answer = Correct
Routing = 不理想
```

如果只看最終答案，很容易把這種不穩定路徑忽略掉。

因此可以至少保留兩個欄位：

| 指標 | 要回答的問題 |
| --- | --- |
| Routing Correct | Tool 與執行順序是否正確？ |
| Task Completed | 使用者原本的任務是否完成？ |

未來甚至還可以加入 Answer Correctness、Source Accuracy、Latency 與 Cost，但第一版先把 Routing 和 Completion 分開，就已經比只看「AI 有沒有回答」完整很多。

## 把 Routing 失敗分成固定類型

當 Coordinator 開始出現錯誤時，不要只記：

```markdown
FAILED
```

更有價值的是把錯誤分類，因為不同錯誤代表需要修改的地方不同。

| 錯誤類型 | 代表問題 |
| --- | --- |
| 選錯 Tool | 問題被送到不適合的資料來源 |
| 漏掉 Tool | 複合問題只完成部分子任務 |
| 多叫 Tool | 呼叫不必要的來源，增加成本與延遲 |
| 順序錯誤 | 後一步依賴前一步，卻提前執行 |
| 該追問卻分流 | 必要資訊不足，但 Coordinator 自行猜測 |
| 不必要重查 | 應沿用舊結果卻重新取得資料 |
| 錯用舊結果 | 資料應更新卻沒有重新查詢 |

例如：

```markdown
ROUTE-17

Question:
「哪類工單最多？那一類怎麼定義？」

Expected:
Sheets → Document

Actual:
Document

Failure Type:
選錯 Tool + 漏掉 Tool
```

另一題：

```markdown
Question:
「把剛剛結果翻成英文。」

Expected:
Reuse existing result

Actual:
Sheets → Document

Failure Type:
不必要重查
```

持續累積這些失敗案例之後，就能看出 Coordinator 真正薄弱的地方。

如果大部分錯誤都是「數據問題跑去 Document」，可能 Tool Description 需要修改；如果大量錯誤出現在模糊問題，可能需要 Clarification；如果常常漏掉第二個 Tool，則可能是 Compound Query 的任務拆解能力不足。

這比單純追求一個漂亮的 95% Accuracy 更有價值。

## 模糊測試最能看出 Coordinator 是否真的理解任務

完成明確問題之後，還應該刻意加入一些不像測驗題的真實語句。

例如：

> 「最近 Delivery Issue 怎麼樣？」

> 「那去年呢？」

> 「這個有改善嗎？」

> 「幫我再確認一下。」

> 「整理成主管能看的版本。」

這些問題沒有完整說明 Source、Metric 或 Tool，卻很接近日常工作對話。

一個好的 Coordinator 應該能根據 Context 判斷：

```markdown
Reuse?
Re-query?
New Tool?
Clarification?
```

而不是只依靠：

```markdown
delivery → Project Tool
year → Sheets
definition → RAG
```

這也是 Routing Test 最後真正需要測試的能力： **Coordinator 到底是在理解工作任務，還是在做比較複雜的關鍵字分類。**

## 今天完成後，Data Machi 發生了什麼變化？

到 Day 18 為止，使用者其實仍然需要理解系統背後存在不同 Tool。今天開始，這個複雜度逐漸被 Coordinator 接手。

原本可能是：

```markdown
使用者
  ↓
我要查數字
  ↓
Sheets

使用者
  ↓
我要查文件
  ↓
RAG
```

現在變成：

```markdown
使用者
  ↓
正常描述工作問題
  ↓
Coordinator
  ↓
選擇適合的 Tool
  ↓
取得結果
  ↓
必要時繼續下一個 Tool
  ↓
整合回答
```

這也是企業 AI 產品很重要的一個 UX 轉變：使用者不應該需要理解底層技術架構，才能把工作完成。

但 Coordinator 能自己選 Tool 之後，新的問題也會開始出現：某些任務可以平行查詢，某些任務必須先後執行；某些資訊可能來自會議、語音或新的企業來源；而執行路徑越複雜，就越需要一個更清楚的 State 與 Workflow 管理方式。

---

> **今天的重點：**  
> Coordinator 的價值不是增加另一個 AI 角色，而是把「該使用哪個 Tool、按照什麼順序執行，以及目前資訊是否足夠」這些判斷集中管理。第一版不需要追求大量 Tool 或 Multi-Agent，先讓少數 Tool 的選擇穩定、結果可以追蹤，而且 Routing 能被固定測試驗證，才是更重要的基礎。

今天的里程碑是： **使用者第一次不需要知道 Data Machi 底層有哪些 Tool，系統會自己判斷資料應該去哪裡取得。**

下一篇，我們會把 Coordinator 放進更複雜的跨來源任務，進一步處理 **哪些查詢可以平行執行、哪些必須按照順序，以及會議內容如何進入同一條工作流程。**

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10405571)

[

Day 18｜路由器（Router）與協調者（Coordinator）：會分流，不代表會完成任務

](https://ithelp.ithome.com.tw/articles/10405571)[下一篇](https://ithelp.ithome.com.tw/articles/10405767)

[

Day 20｜一個問題要查三個來源，代理（Agent）應該怎麼安排？

](https://ithelp.ithome.com.tw/articles/10405767)

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