---
title: "【Day 8】關鍵字搜尋 vs 語意搜尋：RAG 到底是怎麼找到答案的？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402035"
author:
  - "[[iThome]]"
published:
created: 2026-08-10
description: "使用者問： 「離職後帳號怎麼處理？」 但公司文件真正的標題可能叫做： 「離職者權限撤銷流程」 兩句話描述的其實是同一件事：員工離開公司後，應如何移除帳號與系統存取權限。 對人來說，我們很容易理解這兩句..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 9

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從零開始打造企業 AI 知識工作流系列 第 9 篇

## 【Day 8】關鍵字搜尋 vs 語意搜尋：RAG 到底是怎麼找到答案的？

- 分享至

  
使用者問：

> 「離職後帳號怎麼處理？」

但公司文件真正的標題可能叫做：

> 「離職者權限撤銷流程」

兩句話描述的其實是同一件事：員工離開公司後，應如何移除帳號與系統存取權限。

對人來說，我們很容易理解這兩句話在談同一個概念；但對搜尋系統而言，問題沒有這麼簡單。

如果搜尋過度依賴字面上的關鍵字，「帳號怎麼處理」與「權限撤銷」因為使用不同詞彙，最相關的文件可能反而排不到前面。這正是企業搜尋中非常常見的情況：

> **使用者通常用自己的語言描述問題，但企業文件使用的是公司的正式術語。**

因此，在建立 RAG 系統時，只讓 AI「能讀文件」還不夠，我們還必須解決下一個問題： **系統到底要用什麼方式，把正確的文件找出來？**

---

## 關鍵字搜尋：先看「你輸入了哪些字」

傳統關鍵字搜尋，也常被稱為 **Lexical Search（詞彙搜尋）** 。常見方法包括 BM25。

它主要根據查詢中的詞彙，與文件中實際出現的字詞進行比較，判斷哪些內容最相關。

用一個簡化案例來看：

```markdown
使用者查詢：
「離職後帳號怎麼處理？」

文件 A：
「離職後帳號處理方式與系統停用規則」
→ 「離職」、「帳號」、「處理」等字詞高度重疊
→ 很容易被找到

文件 B：
「離職者權限撤銷流程」
→ 主題高度相關
→ 但「帳號處理」與「權限撤銷」措辭不同

文件 C：
「年度帳號密碼更新政策」
→ 出現「帳號」
→ 但真正主題與離職無關
```

關鍵字搜尋最直觀的優勢，是對「明確字詞」非常敏感。

例如：

- 員工編號
- 產品 SKU
- 合約號碼
- 系統名稱
- 法條條號
- 專案代碼
- 文件名稱
- 特定縮寫

如果使用者搜尋：

```markdown
EMP-01837
```

或：

```markdown
ISO 27001 A.5.18
```

我們通常不希望系統去理解它們「在語意上像什麼」，而是希望精確找到包含這個識別碼或條號的內容。

在這類情境中，關鍵字搜尋通常非常可靠。

---

## 關鍵字搜尋的限制：使用者不一定知道公司的正式用詞

關鍵字搜尋最大的問題，是它通常假設：

> 使用者知道文件裡用了什麼詞。

但在企業環境中，這個假設經常不成立。

例如，人資文件可能寫：

> 「離職者資訊系統存取權限撤銷」

員工實際上卻可能問：

> 「人離職後帳號會怎麼處理？」

資訊部門寫的是：

> 「Production Incident Escalation Procedure」

業務同仁卻可能問：

> 「正式環境出問題要找誰？」

同一件事情在不同角色眼中，會有完全不同的說法。

除此之外，企業還常出現：

- 中文與英文混用
- 正式名稱與口語簡稱並存
- 不同部門使用不同術語
- 內部縮寫大量存在
- 文件標題與內文表達方式不同
- 使用者只知道問題，不知道文件名稱

因此，如果 RAG 完全依賴關鍵字搜尋，最後很容易變成：

> 使用者必須先學會公司的搜尋語言，才能使用 AI。

這顯然不是自然語言問答真正想達到的效果。

---

## 語意搜尋：先理解「你其實想找什麼」

語意搜尋（Semantic Search）採用不同的思路。

它不只是看兩段文字有沒有使用相同的字，而是嘗試判斷：

> **這兩段文字是不是在談相近的概念？**

這背後最常使用的技術，就是 Day 07 提到的 **Embedding** 。

系統會將使用者問題與文件 Chunk 都轉換成向量。如果兩段文字在語意上接近，它們在向量空間中的位置通常也會比較靠近。

例如：

```markdown
「離職後帳號怎麼處理？」
             ↓
        語意相近
             ↓
「離職者權限撤銷流程」
             ↓
        語意相近
             ↓
「員工離職時停用系統存取權限」

             ↓
        語意較遠
             ↓
「年度帳號密碼更新政策」
```

雖然「帳號怎麼處理」與「權限撤銷」沒有使用完全一樣的詞彙，但它們都在描述員工離職後取消存取權的流程，因此 Embedding 有機會把它們判斷為相似內容。

這也是語意搜尋最重要的價值：

> **搜尋的不只是字，而是意思。**

---

## 為什麼語意搜尋特別適合 RAG？

RAG 的使用者通常不是在搜尋一個已知文件名稱，而是在直接問問題。

例如：

> 「離職員工是不是當天就要停掉系統權限？」

真正相關的文件可能叫做：

```markdown
Employee Offboarding Access Revocation Policy
```

也可能叫做：

```markdown
人員異動帳號停用與權限回收標準作業流程
```

如果只使用字面比對，使用者很可能根本不知道應該輸入 `Offboarding` 、 `Access Revocation` 或「人員異動」。

語意搜尋則允許使用者保留自己的自然語言表達方式，由系統負責把問題映射到企業文件中的正式語言。

這特別適合：

- 問句形式的查詢
- 同義詞
- 口語與正式用語差異
- 中英文混用
- 不熟悉企業術語的新員工
- 使用者不知道文件名稱的情況

因此，Embedding 幾乎成為現代 RAG 系統的重要基礎之一。

---

## 同一個問題，兩種搜尋方式差在哪裡？

可以先用一張表整理兩者的差異：

| 面向 | 關鍵字搜尋 | 語意搜尋 |
| --- | --- | --- |
| 主要依據 | 查詢詞與文件詞彙的匹配程度 | 問題與文件內容的概念相似度 |
| 常見技術 | BM25、倒排索引 | Embedding、Vector Search |
| 最擅長 | SKU、編號、人名、正式術語 | 自然語言、同義詞、不同措辭 |
| 是否依賴正式用詞 | 較高 | 較低 |
| 精確代碼搜尋 | 強 | 通常較弱 |
| 處理口語問題 | 較弱 | 較強 |
| 可解釋性 | 容易看出哪些詞命中 | 需要搭配相似度與結果檢查 |
| 典型風險 | 同義詞找不到 | 語意太寬、找到看似相關內容 |

這也代表：

> 關鍵字搜尋並沒有因為 Embedding 出現就變得過時。

它們擅長解決的是不同問題。

---

## Data Machi 如何使用語意搜尋？

在 Data Machi 中，PDF 文件會先經過 Day 07 提到的處理流程：

```markdown
PDF
 ↓
Parse
 ↓
Chunk
 ↓
Embedding
 ↓
Vector Index
```

當使用者提出問題時，系統也會將問題轉換成 Embedding，再到向量索引中尋找語意最接近的 Chunk。

教學上可以簡化成：

```python
def search_documents(query: str, top_k: int = 5):
    query_vector = embedding_model.embed_query(query)

    results = vector_store.similarity_search_with_score(
        query_vector,
        k=top_k,
    )

    return [
        {
            "text": doc.page_content,
            "source": doc.metadata.get("source"),
            "page": doc.metadata.get("page"),
            "score": score,
        }
        for doc, score in results
    ]
```

因此，即使使用者問：

```markdown
離職後帳號怎麼處理？
```

系統仍可能找到：

```markdown
文件：離職者權限撤銷流程

內容：
員工離職生效後，應於指定時間內撤銷企業帳號與
各應用程式的存取權限……
```

模型再根據這個 Chunk 產生最後的回答。

這就是語意搜尋在 RAG 中最直接的用途。

---

## 但語意搜尋也可能「理解過頭」

語意搜尋的強項是找到「意思接近」的內容，但這個特性本身也可能成為問題。

假設使用者搜尋：

```markdown
EMP-01837
```

這是一個員工識別碼。

如果系統使用純向量搜尋，它可能把這個代碼轉換成 Embedding，再去找「語意相近」的文字。

但問題是：

> `EMP-01837` 根本沒有什麼需要理解的語意。

我們真正需要的是：

> **哪一筆資料精確包含 EMP-01837？**

同樣地，以下查詢通常也更適合精確比對：

```markdown
ISO 27001 A.5.18
```
```markdown
SKU-48219
```
```markdown
INC-2026-00183
```
```markdown
HR-POL-013
```

這些代碼、編號、條號與專有名稱，本身就是重要資訊。若只依靠語意相似度，反而可能降低準確度。

因此，真正成熟的企業搜尋策略，不會把所有問題都丟給 Embedding。

而是先判斷：

> **這個問題需要「精確找字」，還是「理解意思」？**

---

## Hybrid Search：為什麼企業 RAG 常常兩種都要？

企業搜尋最常見的下一步，就是 **Hybrid Search（混合搜尋）** 。

Hybrid Search 不要求我們在關鍵字搜尋與語意搜尋之間二選一，而是讓兩種方法同時運作，再將結果整合。

例如使用者搜尋：

> 「Okta 離職帳號撤銷流程」

這句話同時包含兩種資訊：

- `Okta` 是非常重要的精確產品名稱
- 「離職帳號撤銷流程」則是自然語言描述

如果只用語意搜尋，系統可能找到很多一般性的離職權限文件，卻不一定把包含 Okta 的內容排在最前面。

如果只用關鍵字搜尋，又可能因為文件寫的是「Offboarding Access Revocation」而錯過相關段落。

Hybrid Search 可以同時利用兩者：

```markdown
使用者問題
「Okta 離職帳號撤銷流程」
        ↓
 ┌──────────────────┐
 │ Keyword Search   │
 │ 精確找出 Okta     │
 └────────┬─────────┘
          │
          │
 ┌────────▼─────────┐
 │ Semantic Search  │
 │ 理解離職與撤權概念 │
 └────────┬─────────┘
          │
          ↓
     Rank Fusion
          ↓
   最終搜尋結果排序
```

實際系統通常會平行執行兩種搜尋，再使用某種融合策略重新排序。

例如：

```markdown
Keyword Results
1. Okta 使用者帳號管理規範
2. Okta SSO 設定指南
3. 員工離職系統權限流程

Semantic Results
1. 離職者權限撤銷流程
2. 員工帳號停用規範
3. IT Offboarding Checklist
```

經過融合後，最終可能變成：

```markdown
Final Ranking

1. 離職者 Okta 權限撤銷流程
2. 員工離職系統權限管理規範
3. Okta 使用者帳號管理規範
```

這種搜尋方式通常比純關鍵字或純向量搜尋更穩定。

---

## 搜尋前還可以先縮小範圍：Metadata Filter

除了文字與語意之外，企業通常還掌握許多額外資訊。

例如使用者問：

> 「台灣員工離職後 Okta 權限怎麼處理？」

這句話其實已經隱含：

- 地區：Taiwan
- 主題：IT Security
- 系統：Okta
- 文件應該仍有效

如果知識庫 Metadata 有紀錄：

```markdown
region = TW
policy_type = IT Security
status = Active
```

系統就可以先套用 Filter，只留下：

> 台灣 + IT Security + 仍有效

的文件，再進行關鍵字與語意搜尋。

這樣可以避免找到：

- 香港的流程
- 已失效的 2023 年政策
- 其他部門的帳號管理規範
- 測試文件
- 未正式發布的草稿

因此，一個成熟的企業 RAG Retrieval Pipeline，可能逐漸從最初的：

```markdown
Question
   ↓
Embedding
   ↓
Vector Search
```

演進成：

```markdown
Question
   ↓
Query Understanding
   ↓
Metadata Filter
   ↓
Keyword Search + Semantic Search
   ↓
Fusion
   ↓
Reranking
   ↓
Relevant Chunks
   ↓
LLM
```

這也再次說明，真正的 RAG 並不是只有「Embedding + Vector Database」。

檢索本身就是一個需要設計的系統。

---

## Fusion 與 Reranking 又是什麼？

當 Keyword Search 與 Semantic Search 都產生一組結果後，我們還需要決定：

> 哪些文件應該真正排在最前面？

這就是 Fusion 與 Reranking 的角色。

### Fusion：先把多種搜尋結果合併

Fusion 的目的是將不同搜尋方式的排名整合成一份結果。

例如，一個文件在 BM25 排第 2，在向量搜尋排第 1，那它很可能值得排在最前面。

另一個文件雖然關鍵字命中很多，但語意搜尋排名很低，可能只是碰巧出現相同詞彙。

Fusion 可以綜合這些訊號。

### Reranking：再重新判斷誰最相關

第一輪搜尋的目的通常是「快速找出候選文件」。

Reranking 則會再使用更精細的方法，重新比較使用者問題與候選 Chunk 的真正相關性。

流程可以理解成：

```markdown
知識庫 100,000 Chunks
        ↓
快速搜尋
        ↓
候選 Top 20
        ↓
Reranker
        ↓
真正最相關 Top 5
```

這種做法比直接對所有文件使用昂貴模型有效率，也能提升最後提供給 LLM 的上下文品質。

---

## 踩坑筆記：相似度高，不代表一定正確

做語意搜尋時，很容易看到一個 Similarity Score，然後設定：

```markdown
similarity > 0.8
→ 相關
```
```markdown
similarity < 0.8
→ 不相關
```

但這種作法通常過於簡化。因為不同 Embedding Model 的分數分布可能完全不同。甚至同一個模型在不同語言、文件類型與資料集上，也可能有不同的合理區間。

例如，在某個模型中：

```markdown
0.82 = 非常相關
```

但在另一個模型中，真正相關的內容可能普遍只有：

```markdown
0.63 ~ 0.72
```

因此，Similarity Score 並不是可以直接跨系統套用的絕對標準。

更好的方法是使用真實問題建立 Retrieval Evaluation Dataset。

---

## 如何評估搜尋到底準不準？

假設我們準備以下企業真實問題：

```markdown
Q1：離職後 Okta 帳號怎麼處理？
Q2：高優先級需求 SLA 是多久？
Q3：台灣市場的出差住宿費上限？
Q4：INC-2026-00183 的處理規則？
```

接著，由人工標記每個問題真正應該找到哪些文件或 Chunk。

例如：

```markdown
Q1
→ 應該找到：
offboarding_policy.pdf / Page 7

Q2
→ 應該找到：
request_priority_definition.pdf / Page 3
```

然後再評估不同搜尋策略。

### Recall：正確資料有沒有被找回來？

如果正確文件應該有 5 個，而系統找到了其中 4 個：

```markdown
Recall = 4 / 5 = 80%
```

Recall 關心的是：

> **有沒有漏掉應該找到的資料？**

### Precision：找回來的資料有多少是真的相關？

如果系統回傳 5 個結果，其中只有 3 個真正相關：

```markdown
Precision = 3 / 5 = 60%
```

Precision 關心的是：

> **搜尋結果中有多少雜訊？**

### Top-K

還需要評估應該取回多少個 Chunk。

例如：

```markdown
Top-K = 3
```

可能非常精準，但容易漏掉背景資訊。

```markdown
Top-K = 20
```

Recall 可能提高，但也可能加入大量無關內容。因此，Top-K 並不是越高越好，最佳設定需要根據真實資料測試。

---

## 不要用「感覺」調 RAG 搜尋

很多 RAG 專案最初的調整方式是：

> 「我問了幾個問題，看起來搜尋結果還不錯。」

這對 Demo 足夠，但很難支撐真正的企業應用。

更好的方式是：

1. 蒐集真實使用者問題
2. 建立正確文件標註
3. 測試 Keyword Search
4. 測試 Semantic Search
5. 測試 Hybrid Search
6. 比較不同 Embedding Model
7. 比較不同 Top-K
8. 調整 Threshold
9. 測試 Metadata Filter
10. 評估 Recall 與 Precision

最後得到的不是：

> 「這個模型感覺比較好。」

而是：

> 「Hybrid Search 在 Top-5 Retrieval 上，正確文件 Recall 從 78% 提升到 91%。」

這才是真正能支持系統設計的判斷。

---

## 那到底應該用哪一種搜尋？

其實沒有必要把它想成：

> Keyword Search **vs** Semantic Search

更實用的問題是：

> **這個查詢裡，有哪些部分需要精確比對？哪些部分需要語意理解？**

例如：

### 適合關鍵字搜尋

```markdown
EMP-01837
```
```markdown
ISO 27001 A.5.18
```
```markdown
SAP
```
```markdown
POL-IT-001
```

這些內容的價值就在精確字面本身。

### 適合語意搜尋

```markdown
離職後帳號要怎麼處理？
```
```markdown
出差住宿最多可以報多少？
```
```markdown
正式環境出問題要找誰？
```

使用者描述的是「概念」，而不是明確文件名稱。

### 適合 Hybrid Search

```markdown
Okta 離職權限撤銷流程
```
```markdown
ISO 27001 密碼管理規範
```
```markdown
SAP 台灣市場採購流程
```

這些問題同時包含必須精確保留的專有詞彙，以及需要理解的自然語言意圖。

---

## 本日重點

今天只需要記住一件事：

> **關鍵字搜尋找的是「相同的字」，語意搜尋找的是「相近的意思」。**

企業 RAG 中，兩者通常不是競爭關係，而是互補。

當使用者搜尋員工編號、SKU、文件代碼或標準條號時，關鍵字搜尋通常更可靠；當使用者使用口語描述問題、不知道正式文件名稱，或不同團隊使用不同術語時，語意搜尋則更有價值。

成熟的企業搜尋系統往往會進一步結合：

- Keyword Search
- Semantic Search
- Metadata Filter
- Rank Fusion
- Reranking
- Retrieval Evaluation

因此，一套好的 RAG 系統真正追求的，不只是「全部改成向量搜尋」，而是：

> **該精確時精確，該理解語意時理解語意。**

回到最開始的例子：

```markdown
使用者說：
「離職後帳號怎麼處理？」

企業文件寫：
「離職者權限撤銷流程」
```

人類一眼就知道它們在談同一件事，好的企業搜尋系統，也應該逐步具備這種能力。

下一篇，我們會處理 RAG 的另一個常見難題：如果 PDF 裡不是乾淨的文字，而是掃描頁面、表格、流程圖或系統截圖，AI 又要如何理解這些多模態文件？

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402032)

[

【Day 7】一份 PDF 如何變成 AI 可以搜尋的知識庫？

](https://ithelp.ithome.com.tw/articles/10402032)

系列文

[Data Machi 30 天學習系列：從零開始打造企業 AI 知識工作流](https://ithelp.ithome.com.tw/users/20169646/ironman/9078) 共 9 篇

目錄

1. 5
	[【Day 4】Prompt 寫再長也不會自動變成可靠的工作流程](https://ithelp.ithome.com.tw/articles/10401394)
2. 6
	[【Day 5】Data Machi 的誕生：從分析工具到企業知識工作流系統](https://ithelp.ithome.com.tw/articles/10401721)
3. 7
	[【Day 6】什麼是 RAG？把 AI 從閉卷變成開卷考試](https://ithelp.ithome.com.tw/articles/10401845)
4. 8
	[【Day 7】一份 PDF 如何變成 AI 可以搜尋的知識庫？](https://ithelp.ithome.com.tw/articles/10402032)
5. 9
	[【Day 8】關鍵字搜尋 vs 語意搜尋：RAG 到底是怎麼找到答案的？](https://ithelp.ithome.com.tw/articles/10402035)
[完整目錄](https://ithelp.ithome.com.tw/users/20169646/ironman/9078)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/6/30/e0751b47-3bdb-44da-b39e-5826ab8b839b_small.png)

[驅動智慧採購新引擎 賦能供應鏈韌性](https://itplus.ithome.com.tw/vod-page/134?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/14/c4e87f20-22cf-4149-ad38-6bee5519e59d_small.jpg)

[新版 CMMC 2.0 最新發展](https://itplus.ithome.com.tw/vod-page/332?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/10/20/b0040965-a1af-4d5a-9a72-4ac8c2014ba6_small.png)

[2023 亞利安科技 Solution Day 議程 - 數位身分認證在資安合規下之發展與應用](https://itplus.ithome.com.tw/vod-page/718?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

CipherTech 亞利安科技 |

43 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/8/8/e2b16221-7234-4115-bb83-abe5c96e6958_small.jpg)

[在 AI IDE 輔助的時代，那些仍然重要的事。以 SwiftUI 開發 iOS App 為例](https://itplus.ithome.com.tw/webinar-page/272?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

40 分