---
title: "Day 15｜實作：讓 AI 同時查數字與文件，完成第一次跨來源回答 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10405380"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "前一天我們畫出了企業知識地圖（Knowledge Map），知道不同資訊應該去哪一個 Source of Truth 取得。今天開始面對真正的跨來源問題，這也是 Data Machi 從「擁有幾個獨立..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 16

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 16 篇

## Day 15｜實作：讓 AI 同時查數字與文件，完成第一次跨來源回答

- 分享至

前一天我們畫出了企業知識地圖（Knowledge Map），知道不同資訊應該去哪一個 Source of Truth 取得。今天開始面對真正的跨來源問題，這也是 Data Machi 從「擁有幾個獨立工具」走向「能完成一段工作流程」的第一步。

假設使用者問：

> 「今年哪一類需求工單最多？這個類別的正式定義是什麼？相關改善專案目前進度如何？」

這句話看起來只有一個問號，但實際上至少包含三個不同子任務：先計算哪一類工單最多，再查詢該類別的正式定義，最後確認相關改善專案目前的狀態。這三項資訊可能分別存在 Google Sheets、PDF 或 Confluence，以及 Trello、Jira 等專案系統中。

## 先拆問題，而不是把所有工具都叫一遍

跨來源查詢的第一步，不是讓模型把所有 Tool 全部執行一次，而是先拆解問題，確認每一個子問題需要什麼資料。

例如剛才的問題可以拆成：

| 子問題 | 適合的資料來源 |
| --- | --- |
| 今年哪一類需求工單最多？ | Google Sheets / Database |
| 這個類別的正式定義是什麼？ | PDF / Confluence |
| 相關改善專案進度如何？ | Trello / Jira |

但真正重要的不只是「用了哪些工具」，還要看這些步驟之間有沒有 **資料依賴（Data Dependency）** 。

第二個問題問的是「最多的那個類別」如何定義，因此系統必須先完成第一步，知道 Top Category 是什麼，才能拿這個結果去搜尋文件。第三步如果也要根據該類別找到對應改善專案，同樣可能需要第一步的輸出。

這種前一步的結果會成為下一步輸入的流程，就是 **Sequential Workflow（循序工作流）** 。

```markdown
使用者問題
    ↓
Google Sheets Tool
計算今年工單數
    ↓
Top Category = Delivery Issue
    ↓
Document Tool
查詢 Delivery Issue 正式定義
    ↓
Project Tool
查詢相關改善專案
    ↓
整合回答
```

但不是所有跨來源問題都需要循序執行。假設使用者問：

> 「今年總工單數是多少？公司的差旅報銷規範是什麼？」

這兩個問題彼此沒有依賴，一個查結構化資料，一個查文件，因此可以同時執行。

```markdown
使用者問題
        ↓
   拆成兩個任務
        ↓
┌──────────────────┬──────────────────┐
│ Google Sheets    │ Document RAG     │
│ 查詢總工單數     │ 查詢差旅規範     │
└──────────────────┴──────────────────┘
        ↓
      整合回答
```

因此跨來源工作流不能只是問「要呼叫哪些 Tool」，還需要回答：

> **哪些步驟可以一起做？哪些步驟一定要先等前一步完成？**

*圖 1｜跨來源問題先拆解，再依資料依賴查詢並保留來源整合*

## 第一版先不要追求全自動 Agent

走到這裡，很容易想直接讓大型語言模型自己決定所有事情：先判斷問題、選工具、決定執行順序，再自己判斷什麼時候結束。

但 Day 15 的第一版其實不需要這麼做。

更適合的方式，是先把跨來源流程用程式明確寫清楚。例如：

```markdown
Step 1
query_google_sheets()
→ 找到 Top Category

Step 2
search_documents(top_category)
→ 找到正式定義

Step 3
get_project_status(top_category)
→ 找到改善進度

Step 4
combine_results()
→ 產生回答
```

這種事先定義好步驟與順序的方式，可以稱為 **Deterministic Workflow（確定性工作流）** 。同樣的輸入條件會按照預先設計好的路徑執行，因此每一層比較容易測試與除錯。

假設最後答案錯了，我們可以逐步檢查：Google Sheets Tool 有沒有算錯 Top Category、Document Tool 有沒有找到錯誤定義、Project Tool 有沒有抓錯專案，或只是最後模型整合回答時出錯。

如果一開始就把所有決策都交給 Agent，當結果不正確時，就會多出另一個可能性：

> 「是不是 Agent 根本選錯工具或走錯順序？」

因此更合理的學習順序是：

```markdown
先建立固定 Workflow
        ↓
確認每個 Tool 都可靠
        ↓
確認跨來源資料能正確傳遞
        ↓
再把部分決策交給 Agent
```

後面 Day 16–20 才會逐步把「什麼時候使用哪個工具」這項判斷能力交給 Agent。

## 來源整合不是把三段文字黏起來

跨來源查詢的另一個難點，是如何把不同 Tool 的結果整合成一個回答。

假設三個工具分別取得：

```markdown
Google Sheets
Top Category: Delivery Issue
Count: 328
Retrieved At: 2026-08-27 09:30

PDF RAG
Definition:
Delivery Issue 指配送延遲、缺件或配送狀態異常的客服需求。
Source: Customer Service Taxonomy.pdf
Page: 8

Trello
Project: Delivery Experience Improvement
Status: In Progress
Due Date: 2026-09-30
```

最簡單的做法當然是直接把三段內容全部貼給模型，但企業環境中更重要的是： **每一個事實仍然能回到自己的來源。**

最終回答可以整理成：

> 今年目前工單數最多的類別是 **Delivery Issue，共 328 件** 。依據《Customer Service Taxonomy》的定義，Delivery Issue 包含配送延遲、缺件與配送狀態異常。目前對應的改善專案 `Delivery Experience Improvement` 狀態為 **In Progress** ，預計於 2026/09/30 完成。

接著保留來源：

```markdown
數據來源：
Google Sheets
查詢時間：2026-08-27 09:30

定義來源：
Customer Service Taxonomy.pdf
Page 8

專案來源：
Trello
Delivery Experience Improvement
```

對使用者來說，回答仍然是一段完整的自然語言；但在系統內部，每一項事實都應該保留 Metadata，而不是整合之後只剩下一大段無法追溯的文字。

## 跨來源至少保留四種資訊

Day 14 已經提過 Source of Truth，真正開始整合時，可以再進一步替每一個 Tool Result 保留至少四類資訊：

| 資訊 | 用途 |
| --- | --- |
| Source | 知道資料來自哪個系統 |
| Retrieved At | 知道資料什麼時間取得 |
| Query / Conditions | 知道使用了哪些查詢條件 |
| Raw Result | 保留工具真正取得的原始結果 |

例如 Google Sheets Tool 可以回傳：

```markdown
Source:
Google Sheets

Retrieved At:
2026-08-27 09:30

Query:
Year = 2026
Group By = Category
Metric = Request Count

Raw Result:
Delivery Issue = 328
Dashboard = 241
Data Query = 185
```

這樣如果最後 AI 回答「Delivery Issue 有 382 件」，我們就可以快速確認：Tool 原始結果其實是 328，因此問題發生在模型整理答案的階段，而不是資料查詢。

這也是為什麼跨來源整合不只是 Prompt Engineering，它同時也是一個資料追蹤與可驗證性問題。

## 某個 Tool 失敗，不代表整個回答都要消失

真實環境中的跨來源查詢，不可能保證每一個外部系統永遠正常。Google Sheets 可能暫時無法讀取、Confluence API 可能 Timeout、Trello Token 可能過期，也可能只是某個專案根本不存在。

假設三個步驟中：

```markdown
Google Sheets → 成功
PDF RAG       → 成功
Trello        → 失敗
```

系統不應該因為 Trello 查詢失敗，就把前面已經取得的結果全部丟掉。

比較合理的回答是：

> 今年目前工單最多的類別是 **Delivery Issue，共 328 件** 。依據《Customer Service Taxonomy》，這個類別包含配送延遲、缺件與配送狀態異常。目前無法取得相關改善專案的最新狀態，因此專案進度暫時無法確認。

這種方式稱為 **Partial Success（部分成功）** 。

系統內部可以保留：

```markdown
Sheets Tool:
SUCCESS

Document Tool:
SUCCESS

Project Tool:
FAILED
Reason: API Timeout
```

對企業使用者來說，這通常比單純顯示：

```markdown
Something went wrong.
```

更有價值，因為已經成功取得的資訊仍然可以使用，同時也清楚知道哪一部分需要重新確認。

> **重要原則：**  
> 部分成功比假裝全部成功更可靠。某個來源查不到時，應該明確指出缺少哪一部分，而不是自行補出一個合理答案。

## Confluence、Trello 並不是這一篇的必要條件

跨來源 Workflow 的核心不是「一定要接三個 SaaS」。

如果目前沒有 Confluence 或 Trello，完全可以先只使用：

```markdown
Google Sheets
+
PDF RAG
```

例如使用者詢問：

> 「今年哪一類需求最多？公司的正式文件怎麼定義這類需求？」

這已經是一個完整的跨來源問題：

```markdown
Google Sheets
    ↓
找出 Top Category
    ↓
PDF RAG
    ↓
搜尋該類別定義
    ↓
整合結果
```

如果公司未來使用 Jira、Notion、Salesforce、資料庫或其他系統，本質上只是新增另一個 Tool。

因此 Day 15 真正的成功條件不是：

> 「我接了多少 API？」

而是：

> **同一個使用者問題，能不能被拆成不同資料任務，再把各 Tool 的結果正確整合回來？**

## 不要每次都把所有 Tool 叫一遍

有了多個 Tool 之後，另一個很常見的誤區是：既然都已經接好了，那每次查詢就全部呼叫一次，好像資料越多答案就越完整。

例如系統目前擁有：

```markdown
Google Sheets Tool
PDF RAG
Confluence Tool
Trello Tool
Web Search Tool
```

使用者只是問：

> 「今年總工單數是多少？」

真正需要的只有 Google Sheets Tool。如果為了展示 Agent 很厲害，又去查 PDF、Confluence、Trello 和 Web，不但不會提升答案品質，反而會增加：

- API 成本
- 回答等待時間
- 不相關資訊
- 權限風險
- Tool Failure 的機率
- 最後整合時的複雜度

因此好的跨來源 Workflow 追求的不是「用了多少工具」，而是：

> **使用足夠完成任務的最少工具。**

如果兩個來源已經可以完整回答問題，就不需要額外查三個系統。

## 階段實作三｜畫出第一條跨來源工作流程

現在可以回到 Day 05 定義的企業問題，以及 Day 10 建立的第一份企業知識來源，再加入一個結構化或即時資料來源。

新的資料來源可以是：

- Google Sheets
- Database
- Project Board
- CRM
- External API
- 其他工作系統

這一階段不要求你真的把所有 API 串接完成，真正要做的是先把 **資料依賴** 寫清楚。

可以建立下面這張表：

| 子問題 | Source of Truth | 必要輸入 | 是否依賴前一步 | 預期輸出 | 失敗時怎麼辦 |
| --- | --- | --- | --- | --- | --- |
| 哪一類工單最多？ | Google Sheets | 日期範圍 | 否 | 類別與數量 | 說明數據無法取得 |
| 該類別如何定義？ | 政策 PDF | 前一步的類別 | 是 | 定義與頁碼 | 保留數字，說明定義缺失 |
| 改善進度如何？ | 專案看板 | 類別或專案名稱 | 是 | 狀態與更新時間 | 保留其他結果並說明失敗 |

完成表格之後，再把執行關係畫出來：

```markdown
問題
  ↓
哪一類工單最多？
  ↓
Google Sheets
  ↓
Top Category
  ↓
┌───────────────────┬───────────────────┐
│ 查正式定義        │ 查改善專案        │
│ Document Tool     │ Project Tool      │
└───────────────────┴───────────────────┘
          ↓
       整合回答
```

這裡可以看到一個有趣的情況：第一步必須先執行，但拿到 Top Category 之後，第二與第三個子任務如果彼此沒有依賴，就可以平行執行。

因此一條 Workflow 不一定只能是「全部 Sequential」或「全部 Parallel」，實際上經常會混合兩者：

```markdown
Step 1
Sequential
先取得 Top Category

        ↓

Step 2A              Step 2B
Document Tool        Project Tool
        \              /
         \            /
          ↓          ↓
           Merge Results
```

這也是後面使用 LangGraph 描述複雜 Workflow 時會再次看到的概念。

## 替跨來源流程準備三類測試問題

和 Day 10 建立 RAG 驗證集一樣，跨來源 Workflow 也需要固定的測試案例，而不是只問一題成功就認為流程完成。

第一版至少可以準備三種類型：

| 類型 | 範例 | 驗證重點 |
| --- | --- | --- |
| 單一來源 | 今年總工單數是多少？ | 是否只呼叫必要 Tool |
| 兩個獨立來源 | 今年總工單數是多少？差旅規範是什麼？ | 是否能平行取得兩個結果 |
| 有資料依賴 | 哪一類工單最多？這個類別如何定義？ | 是否先取得第一步結果再查第二個來源 |

第三種問題尤其重要。

假設第一步算出的結果是：

```markdown
Top Category = Delivery Issue
```

第二步就必須真的使用 `Delivery Issue` 搜尋文件，而不是讓模型根據自己的推測選擇一個看起來最熱門的工單類別。

也就是：

```markdown
精確計算結果
      ↓
成為下一步參數
      ↓
搜尋其他來源
```

而不是：

```markdown
LLM 猜測一個類別
      ↓
繼續往下查
```

這種 Output → Input 的資料傳遞，是跨來源 Workflow 最核心的能力之一。

> **階段成果：**  
> 保存這份「跨來源工作流程」，至少包含每個子問題的 Source of Truth、必要輸入、輸出、步驟依賴與失敗處理方式。Day 20 會回到同一條流程，標記哪些判斷可以逐步交給 Agent，以及哪些條件下 Agent 必須停止、追問或交由人工處理。

## 從 Tool Collection 開始出現 Workflow

做到這裡，Data Machi 已經不再只是：

```markdown
Tool A
Tool B
Tool C
```

而開始變成：

```markdown
使用者目標
    ↓
拆解任務
    ↓
依序或平行使用 Tool
    ↓
把前一步結果傳到下一步
    ↓
處理部分失敗
    ↓
整合來源
    ↓
完成回答
```

這就是 Workflow 的雛形。

目前「應該走哪一條路」仍然主要由我們寫好的程式決定，因此整體行為比較可預期。接下來真正要處理的問題，就是當工具和問題類型越來越多時，我們是否還要替每一種情況手動寫 `if / else` ，還是可以把部分選擇能力交給模型。

---

> **今天的重點：**  
> 跨來源回答不是把多個 Tool 的結果全部丟給模型，而是先拆解問題、確認步驟之間的資料依賴，再按照 Sequential 或 Parallel 的方式執行。每一項結果都應保留來源、查詢時間、條件與原始結果；即使其中一個來源失敗，也應盡可能保留已經成功取得的資訊。

今天的里程碑是： **同一個使用者問題第一次可以跨越不同資料來源完成，而不需要使用者自己拆成多次查詢。**

下一篇，我們會正式回答一個新的問題：當 Tool 越來越多、問題的組合也越來越複雜時，什麼時候固定的 Tool Use 才真正需要進化成 Agent？

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10405115)

[

Day 14｜企業知識散落各處：先畫出你的企業知識地圖（Knowledge Map）

](https://ithelp.ithome.com.tw/articles/10405115)[下一篇](https://ithelp.ithome.com.tw/articles/10405442)

[

Day 16｜什麼時候工具使用（Tool Use）才真正變成代理（Agent）？

](https://ithelp.ithome.com.tw/articles/10405442)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/4/30/f49229f9-d26f-4f63-935f-130e4de630c6_small.jpg)

[AI 時代資料處理殺手鐧：Microsoft Fabric + Power BI Copilot 讓你用說的畫圖表](https://itplus.ithome.com.tw/vod-page/824?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/37b66aa1-ff69-4b7c-9ea1-9a23aeddaf8d_small.jpg)

[國泰 2024 SRE Shift-Right 的發展策略](https://itplus.ithome.com.tw/vod-page/1029?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

SRE CONFERENCE |

35 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/10/31/0c286c67-9d3e-4338-89cf-1637ba816e60_small.png)

[健保數位轉型新篇章：雲端架構、資訊治理與醫療服務革新](https://itplus.ithome.com.tw/vod-page/1265?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣醫院資訊主管會議 |

39 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/5/4/b7bc8d02-d14e-4cd7-8c9e-5c8c23df32e0_small.png)

[Agentic SRE 的崛起：以 kagent 與 A2A 協議建構下一代觀察性平台](https://itplus.ithome.com.tw/vod-page/1428?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Observability Day 可觀察性技術日 |

38 分