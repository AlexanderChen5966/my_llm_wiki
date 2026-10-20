---
title: "【Day 7】一份 PDF 如何變成 AI 可以搜尋的知識庫？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402032"
author:
  - "[[iThome]]"
published:
created: 2026-08-10
description: "你把一份 PDF 上傳到系統後，這份文件就會自動變成 AI 能理解的知識嗎？ 答案是：還差得遠。 對系統來說，一份剛上傳的 PDF 一開始只是一個檔案。它可能包含文字、表格、圖片、頁首頁尾、欄位排版，..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 8

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從零開始打造企業 AI 知識工作流系列 第 8 篇

## 【Day 7】一份 PDF 如何變成 AI 可以搜尋的知識庫？

- 分享至

你把一份 PDF 上傳到系統後，這份文件就會自動變成 AI 能理解的知識嗎？

答案是：還差得遠。

對系統來說，一份剛上傳的 PDF 一開始只是一個檔案。它可能包含文字、表格、圖片、頁首頁尾、欄位排版，甚至只是掃描後的影像。AI 並不會因為「檔案已經上傳」，就自動知道哪些內容重要、某一段規則位在哪一頁，或使用者提問時應該搜尋哪個部分。

要讓文件真正變成一個可搜尋的知識庫，中間通常至少要經過四個關鍵步驟：

1. **Parse｜解析文件**
2. **Chunk｜切割內容**
3. **Embed｜轉換成向量**
4. **Index｜建立搜尋索引**

這四個步驟共同決定了一套 RAG 系統最後「找不找得到正確內容」。

---

## 從 PDF 到知識庫的完整流程

整個流程可以先簡化成：

```markdown
PDF
 ↓
Parse
 ↓
Chunk
 ↓
Embed
 ↓
Index
 ↓
可搜尋的知識庫
```

看起來只有四個步驟，但每一個環節都有可能影響最後的搜尋品質。

如果解析失敗，後面處理的是錯誤文字；如果 Chunk 切得不好，完整概念會被拆散；如果 Embedding 品質不佳，相似內容可能無法被找出；如果索引管理混亂，系統則可能搜尋到過期或重複資料。

因此，RAG 的品質並不只取決於最後使用哪個大型語言模型。

很多時候，真正決定搜尋效果的，是模型回答之前的資料處理流程。

---

## 第一步：Parse｜把 PDF 轉成可以處理的內容

第一步是解析（Parsing）。

這一步的核心目標很簡單：

> **把 PDF 的外殼打開，取出後續系統可以處理的內容。**

一般文字型 PDF 相對簡單，系統通常可以直接取得頁面中的文字。但企業文件經常比想像中更複雜，例如：

- 掃描型 PDF
- 表格
- 多欄排版
- 圖表與圖片
- 頁首頁尾
- 頁碼
- 表單
- 合併儲存格
- 文字與圖片混合的操作手冊

如果解析工具無法正確還原文件結構，後續的 RAG 即使做得再好，也只是在錯誤的資料上搜尋。

例如，一個表格原本是：

| Priority | SLA |
| --- | --- |
| High | 1 day |
| Medium | 3 days |
| Low | 5 days |

如果解析後變成：

```markdown
Priority SLA High Medium Low 1 day 3 days 5 days
```

人可能還能勉強猜出意思，但系統已經失去了原本「Priority 與 SLA 一一對應」的結構。

因此，Parse 並不只是「把文字抓出來」，還需要盡可能保留文件原本的語意結構。

---

## 第二步：Chunk｜把長文件切成可搜尋的片段

取得文字後，下一步通常不會把整份 PDF 直接丟進向量資料庫，而是先把內容切成較小的片段，也就是 **Chunk** 。

為什麼需要切割？

假設一份公司手冊有 100 頁，而使用者只問：

> 「員工出差申請要提前幾天？」

真正相關的內容可能只存在其中一個段落。如果每次搜尋都把完整 100 頁文件交給模型，不但效率差、成本高，也會加入大量不相關資訊。

因此，我們會先將文件拆成很多 Chunk，再讓搜尋系統從中找出最相關的幾個片段。

概念上像這樣：

```markdown
PDF 文件

├── Chunk 1：差旅申請適用對象
├── Chunk 2：差旅申請流程
├── Chunk 3：出發日前五個工作天提出
├── Chunk 4：住宿費用上限
├── Chunk 5：交通費報銷方式
└── ...
```

當使用者詢問申請時間時，系統只需要取回 Chunk 3，以及可能相關的前後段落，而不是整份文件。

---

## 為什麼 Chunking 這麼重要？

Chunking 是整個知識庫建立流程中最容易被低估的環節之一，也是最直接影響搜尋品質的因素之一。

因為 Chunk 大小存在一個很明顯的取捨。

### Chunk 太大

如果每個 Chunk 包含太多文字，雖然上下文完整，但搜尋命中後會同時帶回大量不相關資訊。

可能造成：

- 模型難以聚焦真正答案
- Prompt 變得更長
- Token 成本增加
- 重要內容被大量背景資訊稀釋

### Chunk 太小

如果 Chunk 切得太細，搜尋雖然更精準，卻可能把一個完整概念拆開。

例如原文是：

> 高優先級需求需在一個工作天內完成初步評估。若涉及資安事件，則必須立即升級至主管處理。

如果剛好從中間切開：

```markdown
Chunk A：
高優先級需求需在一個工作天內完成初步評估。

Chunk B：
若涉及資安事件，則必須立即升級至主管處理。
```

當使用者問「高優先級需求遇到資安事件怎麼處理？」時，單獨取回任何一個 Chunk 都可能缺少完整背景。

因此，Chunking 的核心並不是切得愈細愈好，而是：

> **在檢索精準度與上下文完整性之間找到平衡。**

---

## Data Machi 的做法：固定大小切割加上重疊區段

Data Machi 目前採用一種常見的基礎策略： **固定大小 Chunk + Overlap** 。

例如，可以設定：

- Chunk Size：512 tokens
- Chunk Overlap：50 tokens

概念如下：

```markdown
原始內容：

[............... Chunk A：tokens 1–512 ...............]
                             [............... Chunk B：tokens 463–974 ...............]

                             ↑
                    tokens 463–512 重疊
```

為什麼需要 Overlap？

因為自然語言的概念不一定剛好在 Chunk 邊界結束。透過保留一部分重疊內容，可以降低句子、定義或因果關係被完全切斷的機率。

重疊的好處包括：

- Chunk A 的結尾資訊也會出現在 Chunk B 開頭
- 跨 Chunk 的概念不容易完全斷裂
- 搜尋命中相鄰 Chunk 時，上下文較完整
- 模型更容易理解前後關係

例如：

```markdown
Chunk A：
...高優先級需求需要在一個工作天內完成初步評估。
若涉及資訊安全事件...

Chunk B：
若涉及資訊安全事件，必須立即通知資訊安全主管，
並啟動事件處理流程...
```

即使查詢只命中 Chunk B，模型仍能看到前一段關鍵條件。

固定大小加 Overlap 是相對容易實作、也適合初期系統的策略，但它並不是唯一方法。後續若文件類型變得更複雜，也可以考慮依照標題、段落、語意或文件結構進行切割。

---

## 第三步：Embed｜把文字轉換成向量

完成 Chunking 後，系統已經得到很多文字片段，但目前仍然無法進行真正的「語意搜尋」。

下一步是 Embedding，也就是把每個文字 Chunk 轉換成一組數字向量。

概念上可以想成：

```markdown
「員工出差申請需要提前五個工作天提出」

↓

Embedding Model

↓

[0.018, -0.324, 0.712, 0.041, ...]
```

這串數字不是人工設計的分類標籤，而是模型根據文字語意產生的數值表示。

語意相近的句子，在向量空間中的位置通常也會比較接近。

例如：

```markdown
「出差申請要提前多久？」
「員工出差需要提前幾天提出？」
「出差申請的提前申請期限」
```

雖然三句話使用的文字不同，但意思非常接近，因此它們的 Embedding 也應該位於相近的位置。

這正是語意搜尋能運作的核心。

---

## 第四步：Index｜建立可以快速搜尋的索引

如果知識庫只有十個 Chunk，系統每次提問時逐一比較所有向量，可能還可以接受。

但企業文件可能產生：

- 1,000 個 Chunk
- 10,000 個 Chunk
- 100,000 個 Chunk
- 甚至更多

這時候，就需要建立向量索引（Vector Index），讓系統可以快速找出與使用者問題最接近的內容，而不是每次重新從頭比較所有資料。

Data Machi 目前使用 **FAISS** 作為向量索引工具。

建立完成後，知識庫可以簡化理解為：

```markdown
Chunk 1 → Embedding Vector
Chunk 2 → Embedding Vector
Chunk 3 → Embedding Vector
Chunk 4 → Embedding Vector
...
         ↓
      FAISS Index
```

當使用者提出問題時，問題本身也會使用同一個 Embedding Model 轉換成向量。

接著，系統將 Query Vector 與索引中的文件向量進行相似度搜尋：

```markdown
使用者問題
   ↓
Embedding
   ↓
Query Vector
   ↓
FAISS 搜尋
   ↓
Top-K 最相近 Chunk
   ↓
提供給 LLM
```

例如：

> 「員工出差要提前多久申請？」

可能搜尋到：

```markdown
Top 1
Similarity: 0.91
「出差申請應於出發日前五個工作天提出。」

Top 2
Similarity: 0.79
「主管應於收到出差申請後兩個工作天內完成審核。」

Top 3
Similarity: 0.65
「海外出差須另外附上行程與預算說明。」
```

接著，RAG 系統會將這些結果提供給大型語言模型，讓模型根據取回內容產生回答。

---

## 四個步驟如何串在一起？

到這裡，我們可以重新整理一次整個流程。

### 建立知識庫時

```markdown
PDF 文件
   ↓
Parse
取得文字與文件結構
   ↓
Chunk
切成適合搜尋的片段
   ↓
Embed
將每個 Chunk 轉換成向量
   ↓
Index
存入向量索引
   ↓
知識庫建立完成
```

### 使用者提問時

```markdown
使用者問題
   ↓
Embed Query
   ↓
搜尋向量索引
   ↓
找出 Top-K 相關 Chunk
   ↓
把 Chunk + 問題提供給 LLM
   ↓
生成有來源依據的回答
```

因此，一套 RAG 系統其實可以分成兩個階段：

1. **Ingestion：把文件變成可以搜尋的知識**
2. **Retrieval：根據問題把相關知識找回來**

前者通常在文件上傳或更新時執行，後者則在每次使用者提問時執行。

---

## Data Machi 實戰：為什麼 Metadata 很重要？

建立向量索引時，如果只保存 Embedding，而沒有保存原始文件資訊，之後搜尋結果即使找到了正確內容，也不知道它來自哪裡。

因此，Data Machi 在建立每個 Chunk 時，也會保存相關 Metadata。

例如：

```json
{
  "doc_id": "travel_policy_v3",
  "source": "差旅管理辦法_v3.pdf",
  "page": 3,
  "chunk_index": 7,
  "content_type": "text",
  "text": "出差申請應於出發日前五個工作天提出，並附上行程規劃與預算估算……"
}
```

這些 Metadata 的作用非常重要。

### doc\_id

代表文件的唯一識別碼。

即使檔案名稱被修改，系統仍然可以透過 `doc_id` 知道這是哪一份文件。

### source

保留原始檔案名稱，方便使用者查看來源。

例如：

```markdown
差旅管理辦法_v3.pdf
```

### page

記錄這個 Chunk 原本位於哪一頁，讓答案可以引用：

> 根據《差旅管理辦法》第 3 頁……

### chunk\_index

記錄 Chunk 在文件中的順序。

這對於需要往前或往後取得更多上下文時很有幫助。

### content\_type

說明內容類型，例如：

- text
- table
- image
- caption

當系統未來開始處理多模態文件時，這個欄位會更加重要。

### text

保留真正提供給模型閱讀的內容。

---

## Metadata 不只是為了引用來源

Metadata 的作用其實比「顯示文件名稱」更廣。

未來可以透過 Metadata 進行更精確的搜尋限制。

例如，只搜尋：

```markdown
department = HR
```

或：

```markdown
version = latest
```

或：

```markdown
effective_date >= 2026-01-01
```

也可以依照使用者權限限制文件：

```markdown
access_level = internal
```

因此，當知識庫逐漸擴大時，Metadata 會成為文件治理與搜尋品質的重要基礎。

這也是為什麼建立知識庫時，不應該只思考：

> 我要存哪些文字？

還要思考：

> **未來搜尋這些文字時，我需要知道哪些背景資訊？**

---

## Data Machi 的 FAISS 索引快取機制

Embedding 通常是整個文件處理流程中相對昂貴的步驟之一。

如果系統每次重新啟動，都重新把所有 PDF Parse、Chunk，再重新產生所有 Embedding，不但浪費時間，也會產生不必要的 API 成本。

因此，Data Machi 會把建立完成的 FAISS Index 序列化並保存到磁碟。

第一次建立知識庫時：

```markdown
PDF
 ↓
Parse
 ↓
Chunk
 ↓
Embed
 ↓
建立 FAISS Index
 ↓
儲存索引到磁碟
```

服務重新啟動時，如果系統確認原始文件沒有變更，就可以直接載入既有索引：

```markdown
啟動服務
 ↓
檢查知識庫是否有變更
 ↓
沒有變更
 ↓
從磁碟載入 FAISS Index
 ↓
直接開始搜尋
```

不需要重新 Embed。

這樣可以明顯降低：

- 啟動時間
- Embedding API 呼叫次數
- API 成本
- 不必要的重複運算

但這也帶來另一個新的問題：

> **系統怎麼知道文件到底有沒有變更？**

這就進入知識庫維運的範圍。

---

## 知識庫維運的三個關鍵問題

把 PDF 轉成向量並建立 Index，只是第一步。

真正長期運作後，會開始遇到文件新增、修改與刪除的問題。如果沒有明確策略，知識庫很容易出現重複內容、過期版本或幽靈資料。

### 1\. 文件更新後，舊版本怎麼處理？

假設目前知識庫中存在：

```markdown
差旅管理辦法_v2.pdf
```

後來使用者上傳：

```markdown
差旅管理辦法_v3.pdf
```

如果系統只是把 v3 加進去，而沒有處理 v2，搜尋時就可能同時找到兩個版本。

最危險的是：

> 使用者問最新規範，但搜尋結果剛好把舊版本排在前面。

因此，版本管理是企業 RAG 非常重要的一部分。

可以透過 Metadata 保留：

```json
{
  "document_type": "travel_policy",
  "version": "3",
  "effective_date": "2026-07-01",
  "is_active": true
}
```

讓系統知道哪些內容仍然有效。

### 2\. 文件被修改後，如何避免重複 Chunk？

如果同一份文件重新上傳，每次都直接新增 Chunk，就可能變成：

```markdown
Chunk 001：舊版本
Chunk 002：舊版本
Chunk 003：新版本
Chunk 004：新版本
```

最後搜尋到的內容可能互相矛盾。

因此，Data Machi 可以替每份文件設定唯一的 `doc_id` ，更新時使用「先刪後建」的方式：

```markdown
偵測到 travel_policy_v3 更新
        ↓
刪除 doc_id = travel_policy_v3 的所有舊 Chunk
        ↓
重新 Parse
        ↓
重新 Chunk
        ↓
重新 Embed
        ↓
加入新 Index
```

這種策略相對容易理解，也能確保同一份文件在索引中只有一個有效版本。

### 3\. 文件刪除了，索引也要同步刪除

如果使用者從知識庫移除一份文件，向量索引中的資料也必須跟著消失。

否則就會發生一個很奇怪的情況：

> 文件明明已經刪掉了，AI 卻還是能搜尋到裡面的內容。

這種「幽靈文件」在企業環境中除了造成錯誤答案，也可能變成權限與資訊安全問題。

因此，文件生命週期應該與索引同步管理：

```markdown
新增文件 → 建立 Chunk 與 Index
更新文件 → 移除舊資料並重新建立
刪除文件 → 同步從 Index 移除
```

知識庫不是一次性建立，而是一個需要持續維護的資料產品。

---

## 一個容易忽略的問題：文件版本也是知識的一部分

假設公司有兩份文件：

```markdown
差旅管理辦法_2025.pdf
差旅管理辦法_2026.pdf
```

如果兩份都寫著「差旅申請期限」，但規定不同，單靠語意搜尋可能同時把兩份都找回來。

因此，企業 RAG 不能只問：

> 哪一段文字最接近使用者問題？

還需要進一步考慮：

> 哪一份文件才是目前有效的事實來源？

這涉及：

- 文件版本
- 生效日期
- 部門
- 市場
- 權限
- 文件類型
- 是否已失效

換句話說，真正成熟的知識庫不只有向量，也需要 Metadata 與文件治理機制。

---

## 踩坑筆記：RAG 搜尋不準，先不要急著換模型

當 RAG 搜尋結果不好時，很容易第一時間認為：

> 「是不是 Embedding Model 不夠強？」

或：

> 「是不是要換更大的 LLM？」

但搜尋品質差，原因可能出現在整條 Pipeline 的任何位置。

例如：

### Parse 出錯

```markdown
原文：
Priority High：1 個工作天

解析後：
Priority
High
1
個
工作天
```

模型再強也很難理解。

### Chunk 切錯

一個完整規則被拆成兩個互不相干的 Chunk。

### Metadata 不完整

搜尋到了正確規則，但無法辨認它是舊版本還是新版本。

### Embedding 不適合文件語言

如果文件主要是繁體中文與企業縮寫，Embedding Model 的效果可能與純英文文件不同。

### Retrieval 設定不合理

Top-K 太小可能漏掉重要背景；Top-K 太大則可能帶回過多雜訊。

因此，在調整模型之前，更好的檢查順序通常是：

```markdown
Parse 正確嗎？
   ↓
Chunk 合理嗎？
   ↓
Metadata 完整嗎？
   ↓
Embedding 效果如何？
   ↓
Retrieval 參數合理嗎？
   ↓
最後才看 LLM
```

這也是建立 RAG 時很重要的一個觀念：

> **Garbage in, garbage out。**

前面的資料處理如果不可靠，後面再強大的模型也無法穩定補救。

---

## 本日重點

今天只需要記住一件事：

> **PDF 上傳完成，不代表知識庫建立完成。**

一份文件要成為 AI 可以搜尋的企業知識，至少需要經過四個階段：

1. **Parse** ：把 PDF 轉成可以處理的內容
2. **Chunk** ：將長文件切成適合搜尋的片段
3. **Embed** ：把文字轉換成能比較語意的向量
4. **Index** ：建立可以快速搜尋的向量索引

而一套真正能長期使用的企業知識庫，還需要處理 Metadata、文件版本、更新、刪除與索引快取。

因此，RAG 並不只是「把 PDF 丟進向量資料庫」。

真正的問題是：

> **如何讓企業文件以正確、完整、可追溯，而且可持續維護的方式，變成 AI 可以使用的知識。**

下一篇，我們會進一步比較 **語意搜尋與傳統關鍵字搜尋** 的差異，理解為什麼使用者明明沒有輸入文件中的原始關鍵字，RAG 仍然有機會找出正確內容，以及為什麼有些情境下，關鍵字搜尋反而不能被完全取代。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401845)

[

【Day 6】什麼是 RAG？把 AI 從閉卷變成開卷考試

](https://ithelp.ithome.com.tw/articles/10401845)[下一篇](https://ithelp.ithome.com.tw/articles/10402035)

[

【Day 8】關鍵字搜尋 vs 語意搜尋：RAG 到底是怎麼找到答案的？

](https://ithelp.ithome.com.tw/articles/10402035)

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