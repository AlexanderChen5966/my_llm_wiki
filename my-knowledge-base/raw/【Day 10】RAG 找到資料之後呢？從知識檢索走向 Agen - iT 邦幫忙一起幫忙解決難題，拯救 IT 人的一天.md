---
title: "【Day 10】RAG 找到資料之後呢？從知識檢索走向 Agen - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402383"
author:
  - "[[iThome]]"
published:
created: 2026-08-12
description: "學完 RAG 的運作原理之後，很容易產生一個過度樂觀的預期： 「只要把所有企業資料放進知識庫，AI 就能回答所有問題、完成所有工作。」 這個想法有一半是對的，另一半則是危險的誤解。 RAG 確實解決了..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 10

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從零開始打造企業 AI 知識工作流系列 第 11 篇

## 【Day 10】RAG 找到資料之後呢？從知識檢索走向 Agen

- 分享至

學完 RAG 的運作原理之後，很容易產生一個過度樂觀的預期：

> 「只要把所有企業資料放進知識庫，AI 就能回答所有問題、完成所有工作。」

這個想法有一半是對的，另一半則是危險的誤解。

RAG 確實解決了一個非常重要的問題：讓大型語言模型可以在回答之前，取得原本不在訓練資料中的企業私有知識。它可以搜尋 PDF、政策文件、操作手冊與內部規範，再根據找到的內容整理答案。

但 RAG 的核心仍然是一條相對單純的資訊流：

```markdown
使用者問題
   ↓
搜尋知識庫
   ↓
取得相關內容
   ↓
生成答案
```

它擅長回答「文件裡寫了什麼」，卻不會自然知道下一步是不是應該查詢 Google Sheets、取得最新專案進度、執行一段計算，甚至更新某個企業系統。

換句話說，RAG 解決的是 **知識取得問題** ，而不是整個企業工作的執行問題。

---

## 一個真實場景：RAG 找到了資料，但還是不夠

假設一位業務主管詢問 AI：

> 「我們的某項核心指標最近三個月持續下降，原因是什麼？相關的改善專案目前進行到哪裡了？」

這看起來只是一個問題，但真正要回答完整，可能需要查詢完全不同類型的資料來源。

首先，系統可能需要從 Google Sheets 或資料庫取得最近三個月的實際指標，確認下降幅度、發生時間與不同市場的差異。

接著，它可能需要查詢 Confluence 或 PDF 文件，確認這個指標的正式定義、計算方式，以及過去是否曾有相關分析。

如果想回答「原因是什麼」，還可能需要取得不同維度的數據，例如流量、轉換率、客群結構或產品表現，再做進一步拆解。

最後，當主管問到「改善專案目前進行到哪裡」，系統還需要到 Trello、Jira 或其他專案管理系統取得最新任務狀態。

因此，完整流程可能是：

```markdown
問題：
「核心指標最近三個月下降的原因是什麼？
相關改善專案進度如何？」

            ↓

Google Sheets / Database
→ 指標最近三個月的數據

Confluence / PDF
→ 指標定義與歷史背景

Analytics Tool
→ 拆解可能原因

Trello / Jira
→ 改善專案目前進度

            ↓

整合成最終回答
```

單一 RAG 知識庫無法自然代表所有這些系統。

它能回答的通常是：

> 「在我目前可以搜尋的文件裡，有哪些內容和這個問題相關？」

但企業真正想知道的是：

> 「在整個企業資訊環境中，我需要去哪裡取得資料，才能回答這個問題？」

這兩者之間就是 RAG 與 Agent 之間的重要差距。

---

## RAG 真正擅長的是什麼？

理解 RAG 的邊界，並不是在否定它的價值。相反地，只有知道它最擅長什麼，才能把它放在正確的位置。

RAG 特別適合「答案已經存在於文件中，只需要找到並整理」的任務。

例如：

### 文件問答

> 「差旅報銷規範允許搭乘商務艙的條件是什麼？」

答案本來就存在於差旅政策文件中，系統只需要找出相關段落並整理。

### 規範查詢

> 「根據員工手冊，試用期績效評核流程為何？」

這也是典型的文件型知識問題。

### 長文件摘要

> 「幫我把這份 50 頁的產品規格書整理成一頁重點。」

主要工作是從既有內容中搜尋、歸納與摘要。

### 歷史知識搜尋

> 「公司過去有沒有處理過類似的客訴案例？」

只要這些案例已經被記錄在知識庫中，RAG 就可以協助找到相關內容。

這些問題有一個共同特徵：

> **答案本身已經存在於某份相對靜態的內容中。**

RAG 要做的是把正確內容找出來，再交給模型理解。

---

## RAG 什麼時候開始力不從心？

當問題超出「搜尋既有文件」時，RAG 就需要其他能力配合。

例如：

### 查詢即時數據

> 「今天的訂單轉換率是多少？」

這不是文件搜尋問題，而是即時資料查詢問題。最合理的方式可能是查詢資料庫、BI 系統或 API，而不是先把每筆交易轉成 Embedding 存進向量資料庫。

### 執行計算

> 「比較今年與去年同期的市場成長率。」

即使 RAG 可以找到數字，仍需要正確執行計算。這通常更適合 SQL、Python 或其他分析工具。

### 執行系統操作

> 「幫我建立一張高優先級工單。」

RAG 可以告訴你高優先級工單的規則，但它無法只靠文件搜尋建立真正的工單。這需要連接 Jira、Trello 或其他系統 API。

### 查詢最新專案狀態

> 「這個改善專案目前進行到哪裡？」

文件可能記錄專案背景，但最新進度通常存在專案管理系統中。

### 多步驟任務

> 「先查差旅規範，確認這筆費用是否符合政策；如果符合，幫我建立報銷申請。」

這已經不只是 RAG，而是一段包含查詢、判斷與操作的完整工作流程。

因此，可以把不同需求整理成：

| 使用者需求 | 主要能力 |
| --- | --- |
| 「差旅規範怎麼寫？」 | RAG |
| 「今天有多少筆報銷申請？」 | Data Tool |
| 「幫我計算平均報銷金額」 | Analytics Tool |
| 「幫我建立報銷申請」 | Tool Use |
| 「查規範後，判斷是否符合，再建立申請」 | Agent / Workflow |

RAG 很重要，但只是其中一塊。

---

## 一個重要觀念：RAG 可以是 Agent 的一項工具

這是整個 Data Machi 系列很重要的架構觀念之一：

> **RAG 和 Agent 不是同一個層次的東西。RAG 可以是 Agent 手上的一項工具。**

很多人在剛開始接觸 AI 架構時，容易把 RAG、Tool Use 與 Agent 看成彼此競爭的方案：

> 「我們應該做 RAG，還是做 Agent？」

其實這個問題本身就不太正確。

RAG 解決的是：

> 「我要怎麼從企業文件中找到相關知識？」

Agent 解決的則是：

> 「面對這個問題，我下一步應該做什麼？」

Agent 可以判斷：

- 需不需要查文件
- 是否應該查數據
- 哪一個工具最適合
- 第一次結果是否完整
- 是否需要補充查詢
- 是否應該執行下一個動作
- 任務是否已經完成

因此，在 Agent 的視角中，RAG 只是一個可以被呼叫的能力。

就像一位分析師可以使用搜尋系統、Excel、SQL 與專案管理工具一樣，Agent 也可以同時擁有多個 Tool。

---

## Data Machi 實戰：兩層架構的設計哲學

Data Machi 將「決定做什麼」和「實際執行某一項查詢」拆成兩個不同層次。

上層是 **Coordinator / Agent Layer** ，主要負責理解問題、規劃任務與選擇工具。

下層則是各種專門 Tool，例如：

- `search_document_base` ：搜尋 PDF 知識庫
- `query_google_sheets` ：取得結構化數據
- `search_confluence` ：搜尋 Wiki 與企業文件
- `get_trello_cards` ：取得專案狀態

  
*圖 1　Data Machi 中 Coordinator 與不同企業工具的兩層架構*

這張圖有一個比原始版本更重要的地方：在「整合工具結果」之後，多了一個「資訊是否足夠？」的判斷。

因為真正的 Agent 不只是呼叫一次工具再回答，而是可能根據第一輪結果重新判斷：

> 「目前資料足夠嗎？還需要再查一次嗎？」

這也是 Agentic Workflow 與單純 Tool Use 的重要差異。

---

## 上層與下層到底差在哪裡？

以 `search_document_base` 為例，它的職責其實非常單純：

> 根據一個 Query，到 PDF 知識庫中找出最相關的文件內容。

它不應該負責判斷：

- 使用者真正想解決什麼問題
- 要不要先查 Google Sheets
- 找不到時應不應該改查 Confluence
- 是否需要再取得 Trello 進度
- 最後資料是否足以回答問題

這些都屬於 Coordinator 的工作。

例如：

```markdown
使用者：
「日本市場最近的轉換率為什麼下降？
改善專案目前進行到哪裡？」

Coordinator：
1. 需要最新轉換率數據
2. 需要相關指標定義
3. 需要專案進度
4. 可能需要多個工具

        ↓

query_google_sheets
→ 取得最近三個月數據

search_document_base / Confluence
→ 取得轉換率定義與過去分析

get_trello_cards
→ 取得改善專案進度

        ↓

Coordinator：
檢查資訊是否完整
        ↓

整合回答
```

這種職責分工非常重要。

如果把所有決策都塞進 RAG Tool 中，這個工具很快就會變得又大又難維護；如果又把所有工具的邏輯塞進 Coordinator，系統也會變成一個難以測試的巨大 Agent。

更好的方式是：

> **Agent 負責判斷「要做什麼」，Tool 負責可靠地完成「某一件事情」。**

---

## 為什麼不能把所有企業資料都塞進同一個向量資料庫？

既然 RAG 可以搜尋資料，一個很自然的想法就是：

> 「那乾脆把 Google Sheets、專案狀態、營運數字、文件全部轉成向量，不就只需要一個搜尋系統？」

技術上不是完全做不到，但通常不是最好的設計。

不同類型的企業資料，本身就有完全不同的查詢需求。

### 更新頻率不同

一份企業政策可能三個月或一年才更新一次，很適合建立 Embedding 索引。

但即時營運數據可能每分鐘、每秒都在變。如果每次資料更新都重新 Embedding，就會產生大量沒有必要的成本。

### 查詢方式不同

文件問題通常是：

> 「哪一段內容跟我的問題最相關？」

這非常適合語意搜尋。

但數據問題通常是：

> 「台灣市場最近三個月的轉換率平均是多少？」

這更適合：

```sql
SELECT AVG(conversion_rate)
FROM ...
WHERE market = 'TW'
AND date BETWEEN ...
```

精確數字應該使用適合結構化資料的工具，而不是靠向量相似度。

### 權限需求不同

一般員工手冊可能可以讓全公司查詢，但財務數據、人員資料與敏感專案可能只有特定角色可以存取。

如果所有內容都直接混在一個向量資料庫中，權限控制會變得更加複雜。

### 準確性要求不同

例如：

> 「公司文化重視什麼？」

這類問題可以接受語意檢索後的摘要。

但：

> 「2026 年 7 月營收是多少？」

答案必須是精確數字，差一點都不行。

因此，真正可靠的企業 AI 架構應該尊重不同資料本身的特性，而不是硬把所有資訊轉換成同一種格式。

> **不是所有資料都應該進向量資料庫，也不是所有問題都應該用 RAG 解決。**

---

## 那 Tool Use 又是什麼？

Tool Use 可以理解成：

> **讓模型不只是讀文字，而是能呼叫外部能力。**

例如，一個模型可以擁有：

```markdown
search_document_base()
query_google_sheets()
run_sql()
get_trello_cards()
create_ticket()
send_email()
```

每個 Tool 都是一個明確的能力。

當使用者問：

> 「今年台灣市場的工單數是多少？」

AI 不需要猜，也不需要從文件中找，而是呼叫數據工具。

如果使用者說：

> 「幫我建立一張高優先級工單。」

則可以呼叫建立工單的 Tool。

RAG 本身也可以被包裝成：

```markdown
search_document_base(query)
```

從 Agent 的角度來看，它和其他 Tool 沒有本質上的衝突。

---

## 從 RAG 到 Agentic RAG

當 Agent 開始能夠主動使用 RAG，而不是每次固定搜尋一次，架構就進一步走向 **Agentic RAG** 。

傳統 RAG 通常比較像：

```markdown
Question
   ↓
Search Once
   ↓
Generate Answer
```

Agentic RAG 則可能是：

```markdown
Question
   ↓
Search
   ↓
結果足夠嗎？
   ↓
否
   ↓
重新改寫 Query
   ↓
再次搜尋
   ↓
還需要其他來源嗎？
   ↓
是
   ↓
呼叫另一個 Tool
   ↓
整合結果
   ↓
回答
```

也就是說，RAG 從一條固定 Pipeline，變成 Agent 可以反覆使用的一項能力。

---

## Agentic RAG 可以多做哪些事情？

### 第一次搜尋不夠時，自動調整 Query

假設使用者問：

> 「離職之後帳號怎麼處理？」

第一次搜尋：

```markdown
Query：
離職後帳號處理
```

沒有找到足夠結果。

Agent 可以根據企業術語重新改寫：

```markdown
Query：
employee offboarding access revocation
```

或：

```markdown
Query：
離職 權限撤銷 帳號停用
```

再搜尋一次。

這比單次向量搜尋更有彈性。

### 搜尋過程中切換工具

例如使用者問：

> 「公司規定離職帳號要多久關閉？目前還有哪些離職員工帳號沒關？」

第一段是政策問題：

```markdown
search_document_base()
```

第二段則是即時系統數據：

```markdown
query_identity_system()
```

Agent 可以先查規範，再查目前系統狀態。

### 主動判斷答案是否完整

搜尋文件後，Agent 可能發現：

> 找到了帳號停用時間，但沒有找到特殊權限帳號的處理方式。

這時可以決定：

- 再搜尋一次
- 查詢另一個知識來源
- 向使用者說明目前缺少的資訊
- 詢問是否需要繼續查詢

而不是看到任何 Retrieval Result 就立刻產生答案。

### 將前一步結果帶入下一步

例如：

```markdown
Step 1：
搜尋政策
→ 高風險系統必須於離職當日停權

Step 2：
取得高風險系統清單
→ Okta、SAP、Production DB

Step 3：
查詢這三個系統目前帳號狀態

Step 4：
找出尚未停用的人員
```

這已經超越單純文件問答，而進入真正的多步驟工作流程。

---

## Agentic RAG 不是讓 RAG 變成萬能

「Agentic RAG」這個詞很容易讓人誤以為：

> 只要讓 RAG 多搜尋幾次，它就可以完成所有任務。

其實並不是。

Agentic RAG 的核心是：

> **讓 Agent 更有策略地使用 Retrieval。**

它仍然需要其他 Tool 才能處理：

- 即時資料
- 精確計算
- 系統寫入
- API 操作
- 工作流程
- 通知與追蹤

所以一套企業 Agent 最後可能像這樣：

```markdown
Agent
                  │
    ┌─────────────┼─────────────┐
    │             │             │
   RAG          Data Tool    Action Tool
    │             │             │
找文件知識      查即時數據      執行操作
```

RAG 是「知道」，Tool Use 讓 AI 可以「做」，而 Workflow 則讓整個過程變得可控制與可重複。

---

## 踩坑筆記：工具越多，不代表 Agent 越強

當理解 Tool Use 之後，另一個很常見的錯誤是：

> 「那我把所有 API 都接給 Agent，它不就更強？」

其實 Tool 越多，工具選擇反而可能變得更困難。

例如，同時提供：

```markdown
search_pdf
search_confluence
search_google_drive
search_notion
search_sharepoint
search_slack
query_database
query_bigquery
query_sheets
```

使用者只問：

> 「去年日本市場的需求趨勢如何？」

Agent 必須先判斷哪個工具才是真正的資料來源。

如果工具描述不清楚、責任重疊，模型就可能：

- 選錯工具
- 重複搜尋多個來源
- 增加不必要成本
- 得到互相衝突的答案
- 無法知道哪個來源是正式資料

因此，Tool 設計也需要清楚的責任邊界。

好的 Tool 通常應該具備：

- 清楚的功能名稱
- 明確的適用範圍
- 簡單的輸入參數
- 結構化的輸出格式
- 可理解的錯誤訊息
- 明確的權限控制

Agent 才能做出更穩定的工具選擇。

---

## 本日重點

今天最重要的觀念不是「RAG 不夠好」，而是：

> **RAG 解決的是知識檢索問題，不是整個企業工作的執行問題。**

RAG 非常適合搜尋規範、手冊、歷史案例與文件知識，但當任務涉及即時數據、精確計算、專案狀態或系統操作時，就需要搭配其他 Tool。

因此，可以把整個能力層次理解成：

```markdown
RAG
→ 讓 AI 找到企業知識

Tool Use
→ 讓 AI 取得資料或執行操作

Agent
→ 讓 AI 判斷下一步應該使用什麼工具

Agentic Workflow
→ 用流程控制讓多步驟任務可靠執行
```

而在 Data Machi 中， `search_document_base` 只負責「搜尋 PDF 知識庫」，Coordinator 才負責理解完整問題、決定是否需要 RAG、選擇其他工具，以及判斷資料是否足以回答。

這兩個層次不應混為一談。

從下一篇開始，我們會正式進入 **Tool Use** ，看看 AI 如何從「會讀文件」進一步變成「會使用企業系統」的工作夥伴。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402347)

[

【Day 9】當 PDF 不只是文字：RAG 如何讀懂掃描頁、表格與流程圖？

](https://ithelp.ithome.com.tw/articles/10402347)

系列文

[Data Machi 30 天學習系列：從零開始打造企業 AI 知識工作流](https://ithelp.ithome.com.tw/users/20169646/ironman/9078) 共 11 篇

目錄

1. 7
	[【Day 6】什麼是 RAG？把 AI 從閉卷變成開卷考試](https://ithelp.ithome.com.tw/articles/10401845)
2. 8
	[【Day 7】一份 PDF 如何變成 AI 可以搜尋的知識庫？](https://ithelp.ithome.com.tw/articles/10402032)
3. 9
	[【Day 8】關鍵字搜尋 vs 語意搜尋：RAG 到底是怎麼找到答案的？](https://ithelp.ithome.com.tw/articles/10402035)
4. 10
	[【Day 9】當 PDF 不只是文字：RAG 如何讀懂掃描頁、表格與流程圖？](https://ithelp.ithome.com.tw/articles/10402347)
5. 11
	[【Day 10】RAG 找到資料之後呢？從知識檢索走向 Agen](https://ithelp.ithome.com.tw/articles/10402383)
[完整目錄](https://ithelp.ithome.com.tw/users/20169646/ironman/9078)

[![.](https://itadstatic.ithome.com.tw/B9/1786008610_6a745422552c3.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9001)

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