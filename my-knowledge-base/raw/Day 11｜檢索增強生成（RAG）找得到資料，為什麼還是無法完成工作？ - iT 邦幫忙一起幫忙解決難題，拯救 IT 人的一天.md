---
title: "Day 11｜檢索增強生成（RAG）找得到資料，為什麼還是無法完成工作？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10404533"
author:
  - "[[iThome]]"
published:
created: 2026-08-28
description: "當完成第一個 PDF 檢索增強生成（RAG）之後，很容易產生一個期待：只要把公司資料都放進知識庫，AI 是不是就能回答所有問題？這個想法看起來很合理，但真正放進企業情境後，很快就會碰到能力邊界。 假設..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 12

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 12 篇

## Day 11｜檢索增強生成（RAG）找得到資料，為什麼還是無法完成工作？

- 分享至

  
當完成第一個 PDF 檢索增強生成（RAG）之後，很容易產生一個期待：只要把公司資料都放進知識庫，AI 是不是就能回答所有問題？這個想法看起來很合理，但真正放進企業情境後，很快就會碰到能力邊界。

假設主管問：「最近三個月某項指標持續下降，現在最新數字是多少？相關改善專案做到哪裡？」這個問題其實同時包含三種資訊需求。第一種是「這個指標到底怎麼定義」，答案可能存在制度文件或分析說明中；第二種是「現在最新數字是多少」，資料可能存在 Google Sheets 或資料庫；第三種則是「改善專案進度到哪裡」，資訊可能存在 Trello、Jira 或其他專案管理工具。

其中只有第一種問題最適合交給 RAG。這也是為什麼學會文件搜尋之後，下一步不是把更多資料全部塞進知識庫，而是先理解： **不同資料來源，本來就應該用不同的方法取得。**

## 文件知識與即時狀態，是兩種不同問題

RAG 擅長回答的是「文件裡寫了什麼」。例如公司制度、產品手冊、名詞定義、歷史報告、操作說明與過去案例，這些資訊通常已經存在於某份文件中，而且更新頻率相對沒有那麼高。

例如：

- 「國內出差住宿費上限是多少？」
- 「這個 KPI 在公司內部怎麼定義？」
- 「產品退貨政策有哪些限制？」
- 「過去是否有類似客訴案例？」

這類問題的核心是先找到正確文件，再根據文件內容回答，因此非常適合 RAG。

但另外一類問題就不一樣了，例如：

- 「昨天銷售額是多少？」
- 「目前有幾張未完成工單？」
- 「這週哪個市場成長最快？」
- 「現在庫存還剩多少？」
- 「這個專案目前有哪些逾期任務？」

這些問題不是在問「文件怎麼寫」，而是在問 **系統現在的狀態** 。資料可能每小時、每天，甚至每分鐘都會改變，因此如果仍然使用 RAG 的方式處理，就會開始出現問題。

## 為什麼最新數字不適合直接放進 RAG？

假設我們把每天的銷售數字全部轉成 Embedding，再存進向量資料庫。技術上並不是做不到，但這樣做很可能把問題變得更複雜。

例如使用者詢問：

> 「昨天台灣的銷售額是多少？」

這種問題真正需要的是一個精確條件：

```markdown
region = Taiwan
date = yesterday
metric = sales
```

接著系統應該回傳一個確切數值。但向量搜尋的設計目標是找出「語意相近」的內容，而不是執行精確條件篩選。因此搜尋結果有可能找回「上週台灣銷售」、「昨天香港銷售」或「台灣本月銷售」等語意接近、但不是正確答案的資料。

對文件搜尋來說，「相似」往往很有價值；但對商業數字而言，使用者通常要的是：

> **對的日期、對的市場、對的條件，以及對的數字。**

因此結構化資料通常更適合透過 SQL、Spreadsheet API 或其他資料工具查詢，而不是依賴向量相似度。

## RAG 不應該變成萬用資料層

企業資料本來就有不同特性，因此不應該為了讓架構看起來統一，就把所有資料都轉換成 Embedding。

可以先用下面這張表理解差異：

文字Markdown

CSVExcel

統計圖表

| 資料類型 | 常見內容 | 更新頻率 | 適合的取得方式 |
| --- | --- | --- | --- |
| 文件知識 | 規範、手冊、政策、歷史報告 | 低到中 | RAG |
| 結構化資料 | 銷售、訂單、會員、庫存 | 中到高 | SQL / Sheets Tool |
| 即時狀態 | 工單、專案任務、物流狀態 | 高 | API |
| 外部服務 | 天氣、匯率、搜尋結果 | 持續變動 | External API |
| 系統操作 | 建立任務、更新狀態、寄送訊息 | 即時 | Tool / API Action |

文件可能一季才更新一次，但交易資料可能每分鐘都在變；文件適合透過語意理解找到相關段落，數字則需要精確篩選與計算。另一方面，不同資料來源也可能有不同權限，例如一般制度文件可以開放多人查詢，但財務數據可能只能讓特定角色存取。

因此更合理的企業 AI 架構不是建立一個「什麼都塞進去的向量資料庫」，而是保留每種資料最適合的存取方式，再讓 AI 負責理解問題、選擇資料來源與整合結果。

## 同一個問題，可能需要同時使用多種資料

再回到一開始主管提出的問題：

> 「最近三個月某項指標持續下降，現在最新數字是多少？相關改善專案做到哪裡？」

如果把它拆開，其實可能需要三個不同來源：

```markdown
問題：
最近三個月某項指標持續下降，
現在最新數字是多少？
相關改善專案做到哪裡？

        ↓

1. 指標定義
   → 文件知識
   → RAG

2. 最近三個月數據
   → Google Sheets / Database
   → Data Tool

3. 改善專案進度
   → Trello / Jira
   → Project Tool
```

最後真正有價值的答案，不只是把三份資料分別列出來，而是由模型把這些資訊重新組織成一個可以協助決策的回答。

例如：

```markdown
這項指標的正式定義是……
來源：KPI Definition.pdf

最近三個月分別為 78%、74%、69%，
呈現連續下降。
來源：Sales Performance Sheet

目前改善專案共有 5 項任務，
其中 3 項完成、1 項進行中、1 項逾期。
來源：Project Board
```

到這個階段，AI 的角色已經不只是「找答案」，而開始變成「協調不同系統取得資訊」。

這也是 Tool Use 開始出現的地方。

## 下一步：讓 AI 會使用工具

當大型語言模型可以呼叫外部函式、API 或資料服務時，它的能力就從單純產生文字，進一步延伸到取得資料甚至執行操作。這個概念通常稱為 **Tool Use（工具使用）** 。

可以先把最基本的流程理解成：

```markdown
使用者問題
      ↓
判斷需要哪種資料
      ↓
┌──────────────┬──────────────┬──────────────┐
│ 文件知識     │ 精確數字     │ 即時狀態     │
│              │              │              │
│ RAG          │ Sheets / SQL │ External API │
└──────────────┴──────────────┴──────────────┘
      ↓
取得結果
      ↓
LLM 整理答案
```

例如使用者問：

> 「昨天台北店的銷售額是多少？」

模型不需要自己知道答案，而是可以判斷這是一個結構化數據問題，接著呼叫：

```markdown
get_sales(
    location="Taipei",
    date="yesterday"
)
```

工具可能回傳：

```markdown
{
  "sales": 1584200,
  "currency": "TWD"
}
```

最後模型再把結果轉換成自然語言：

> 昨天台北店的銷售額為新台幣 1,584,200 元。

這和 RAG 的差異就在於：RAG 是從既有知識中找出相關內容，而 Tool Use 則可以讓模型 **主動向外部系統取得目前需要的資訊** 。

## Tool Use 不只是查資料，也可以執行動作

工具的能力並不只限於讀取資訊。

假設使用者詢問：

> 「目前有哪些逾期任務？」

AI 可以呼叫 Project Tool 查詢專案狀態。但如果下一句使用者說：

> 「幫我把這三項任務的截止日期延後到下週五。」

這時候系統需要做的就不只是「Read」，而是「Write」。

可以把工具大致分成兩種：

| 類型 | 例子 |
| --- | --- |
| Read Tool | 查詢銷售、取得文件、查看任務 |
| Action Tool | 建立任務、更新狀態、寄送訊息 |

Action Tool 的風險通常比 Read Tool 更高，因為它真的會改變外部系統的狀態。因此企業 AI 後面還需要處理確認機制、權限、操作紀錄與失敗回復等問題。

這些我們之後會再慢慢加入。現階段先理解最核心的差異就好： **AI 不再只是回答問題，而開始有能力和其他系統互動。**

## RAG 和 Tool Use 不是競爭關係

看到這裡，很容易又產生另一個問題：

> 「既然 Tool Use 這麼強，那是不是後面就不需要 RAG？」

並不是。

RAG 和 Tool Use 解決的是不同問題，因此更常見的做法是把兩者一起使用。

甚至從 Agent 的角度來看，RAG 本身就可以被包裝成一個 Tool。例如系統可能擁有：

```markdown
Document Search Tool
Sales Data Tool
Project Status Tool
Web Search Tool
```

當使用者提出問題後，Coordinator 或 Agent 再判斷應該使用哪一項工具。

例如：

```markdown
「公司的退貨政策是什麼？」
    ↓
Document Search Tool

「昨天銷售多少？」
    ↓
Sales Data Tool

「目前專案進度如何？」
    ↓
Project Status Tool
```

如果問題更複雜，也可能同時使用多個工具：

```markdown
「銷售最近下降的原因是什麼，
相關改善計畫做到哪裡？」

        ↓

Sales Data Tool
        +
Document Search Tool
        +
Project Status Tool
        ↓
整合結果
        ↓
回答
```

因此真正重要的問題不是：

> 「RAG 和 Tool Use 哪個比較好？」

而是：

> **「這個問題需要哪一種資料取得方式？」**

這個觀念會成為後面 Agent 架構非常重要的基礎。

## 從「回答問題」走向「完成工作」

Day 06 到 Day 10，我們主要建立的是 AI 的「知識取得能力」。有了 RAG 之後，模型可以在回答前先查看企業文件，不必只依賴原本訓練時知道的內容。

但從 Day 11 開始，我們要再往前一步。企業工作往往不是單純找到一段知識就結束，而是需要查詢數字、比較狀態、取得最新資訊，甚至進一步更新系統。

可以把這個能力演進理解成：

```markdown
LLM
只靠模型原有知識回答
       ↓

RAG
回答以前先查企業文件
       ↓

Tool Use
可以查詢外部資料與系統
       ↓

Agent
根據任務決定要使用哪些工具
       ↓

Workflow
把多個步驟串成完整工作流程
```

這也是 Data Machi 接下來幾天要逐步建立的能力。

---

> **今天的重點：**  
> RAG 解決的是「去哪裡找文件知識」，但企業工作還包含即時資料、精確計算與系統操作，這些問題需要 Tool Use。真正的企業 AI，不是把所有資料塞進同一個知識庫，而是知道不同問題應該去哪一個系統取得答案。

下一篇，我們會把 Tool Use 再拆開來看：模型到底怎麼知道有哪些工具、什麼時候該呼叫，以及 LangChain 在這個過程中究竟扮演什麼角色。

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10404250)

[

Day 10｜實作：建立第一個能引用來源的 PDF 檢索增強生成（RAG）

](https://ithelp.ithome.com.tw/articles/10404250)[下一篇](https://ithelp.ithome.com.tw/articles/10404726)

[

Day 12｜什麼是工具使用（Tool Use）？讓 AI 從「回答」開始學會「使用工具」

](https://ithelp.ithome.com.tw/articles/10404726)

系列文

[Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI](https://ithelp.ithome.com.tw/users/20169646/ironman/9207) 共 16 篇

目錄

1. 12
	[Day 11｜檢索增強生成（RAG）找得到資料，為什麼還是無法完成工作？](https://ithelp.ithome.com.tw/articles/10404533)
2. 13
	[Day 12｜什麼是工具使用（Tool Use）？讓 AI 從「回答」開始學會「使用工具」](https://ithelp.ithome.com.tw/articles/10404726)
3. 14
	[Day 13｜實作：讓 AI 查 Google Sheets，而不是自己猜數字](https://ithelp.ithome.com.tw/articles/10404930)
4. 15
	[Day 14｜企業知識散落各處：先畫出你的企業知識地圖（Knowledge Map）](https://ithelp.ithome.com.tw/articles/10405115)
5. 16
	[Day 15｜實作：讓 AI 同時查數字與文件，完成第一次跨來源回答](https://ithelp.ithome.com.tw/articles/10405380)
[完整目錄](https://ithelp.ithome.com.tw/users/20169646/ironman/9207)

[![](https://itadstatic.ithome.com.tw/B9/1787130677_6a8573357d6ff.png)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9004)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/20/9d319579-71e5-4ad3-9cc6-57594bd4e2da_small.jpg)

[敏捷是解藥？還是毒藥？](https://itplus.ithome.com.tw/vod-page/147?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/16/d0cb7b9b-cb75-433c-8918-2776af9545ae.png)

[運用全國航照影像及 AI 判釋技術建置農地資料庫](https://itplus.ithome.com.tw/vod-page/934?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/20/03844dc4-88b1-4263-b1e1-07fa4a84d295_small.png)

[用 n8n 跟 Line 打造你自己的名片管理王](https://itplus.ithome.com.tw/vod-page/1404?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

92 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/45db82c3-a6bd-43df-ae7e-c8ad489c4bc0_small.png)

[AI 讓效率提升，但誰來保護品質的人？](https://itplus.ithome.com.tw/vod-page/1467?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

38 分