---
title: "新的 RAG「Decision RAG」速度快！便宜！又好用！🍜"
source: "https://codelove.tw/@tony/post/q5V5Qx"
author:
published:
created: 2026-11-05
description: "[](#%EF%B8%8F-%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)⚖️ 前言----------------------------------------------------------最近像 Jev 這類專門用於判斷的模型陸續出現了呢。不需要生成文章..."
tags:
  - "clippings"
status: "inbox"
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [11月04日](https://codelove.tw/@tony/post/q5V5Qx "2026-11-04 17:15:10")

## ⚖️ 前言

最近像 Jev 這類專門用於判斷的模型陸續出現了呢。不需要生成文章，只會回傳「是」的可信程度。最近有篇論文提出把這種判斷模型拿來做 RAG 的檢索，叫做「DecisionRAG」，所以我就用自己的方式消化整理了一下。後半段也和一般的 RAG（以下記為 NaiveRAG）做了比較驗證。

## 🎯 目標讀者

- 熱愛 RAG 的人
- 對判斷模型有興趣的人
- 對向量搜尋的精準度不滿意的人

## 😢 傳統 RAG 的問題點

[![02-bottleneck.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fd52cb9ba-5be7-4ae4-a33a-7a3016686e45.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=902c258138e0a03402c2fa40ca70fd11)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fd52cb9ba-5be7-4ae4-a33a-7a3016686e45.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=902c258138e0a03402c2fa40ca70fd11)

NaiveRAG 會透過關鍵字匹配或向量相似度來找文件。這種搜尋常常會撈到多餘的內容，或漏掉真正的答案。搜尋階段漏掉的答案，無論 LLM 再怎麼聰明也補不回來。那麼就把 LLM 等級的智慧帶進搜尋吧！！這就是 DecisionRAG 的想法。話雖如此，若在搜尋時使用 LLM，成本和時間都會增加。正當大家為此苦惱時，就出現了判斷模型（Decision Model）。那就把它拿來用在搜尋上吧。

## 📚 階層式追蹤

[![03-tree.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F8c37b001-299a-4cb3-9c72-14c80c95d571.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=446ab1edbc719906c2a96e3396b0c65a)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F8c37b001-299a-4cb3-9c72-14c80c95d571.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=446ab1edbc719906c2a96e3396b0c65a)

大家去圖書館找書時，不會一本一本把所有書都翻開吧？通常會先從電影區→導演的書架→作品，一層一層縮小範圍。DecisionRAG 也是一樣，先把文件做成樹狀結構，再由各層的判斷模型決定「要開哪個書架」。因為被捨棄的書架內容完全不看，所以即使文件量龐大也能很快，這就是它的機制。

## ⚡ 聰明卻很快！

[![04-positioning.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Faf1d28d6-94fd-49c8-ac03-2ae1410a9fd2.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=924c0e2496baca386526aa64df390b67)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Faf1d28d6-94fd-49c8-ac03-2ae1410a9fd2.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=924c0e2496baca386526aa64df390b67)

NaiveRAG 很快，但常常只因為語意接近就撈到不是答案的內容。AgenticRAG 雖然因為 LLM 會反覆搜尋而很聰明，但速度慢、成本高。GraphRAG 則能沿著人或物的關聯追蹤，但要建立圖結構很麻煩。

DecisionRAG 讓判斷模型不輸出文章，只輸出可信度。因此就能用接近 NaiveRAG 的速度，發揮 AgenticRAG 的智慧。

## 🗂️ 在搜尋前先將文件整理成階層

[![05-prepare.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F6a5ee5d0-912b-46f9-91e0-a70b3c294f8d.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3c80614bdabc5531421a4ba6ce6f2430)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F6a5ee5d0-912b-46f9-91e0-a70b3c294f8d.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3c80614bdabc5531421a4ba6ce6f2430)

DecisionRAG 的前提是資料本身具有像「類別 → 文件 → chunk」這樣的階層。若是手冊或公司內部 Wiki，只要把資料夾或標題直接當成階層即可。若是沒有結構的資料，論文中則是先用分群把資料分成群組，再讓 LLM 為各群組撰寫說明文字。

## 🔁 搜尋方法

[![06-loop.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Ff8572e55-0e06-48b0-8a40-fba521d4b11d.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b7d274df2c16f83a431e0e85f45f2920)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Ff8572e55-0e06-48b0-8a40-fba521d4b11d.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b7d274df2c16f83a431e0e85f45f2920)

1. 取得階層資料的子層資訊
2. 由判斷模型比對問題與子層說明文字並給出機率
3. 只保留前 k 筆（top-k），進入下一層

重複這個流程直到最底層，最後把留下的 chunk 交給生成用 LLM 就可以了。

## 🧪 實際驗證！真的做出來量測看看

前面一直在解說論文，但總會想：「真的有這麼美的事嗎？」所以我實際做出來量測看看。

### 🌐 範例網站

歡迎到這個也用於驗證的網站，親自體驗速度與精準度。

如果流量集中，畫面可能會顯示「流量集中中」並停止，敬請見諒🙇♂️

[![08-system-screen.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fc56d374b-d3f4-497f-977d-92baed1be394.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d8bffb3da42b0fb8777b615e038f993d)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Fc56d374b-d3f4-497f-977d-92baed1be394.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d8bffb3da42b0fb8777b615e038f993d)

### 🏗️ 驗證環境

[![09-architecture.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F2ad69a47-c202-4dcf-850b-01cfc16225a0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a5aa5dbbd8e4e51af0c4706c5492a9d7)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F2ad69a47-c202-4dcf-850b-01cfc16225a0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a5aa5dbbd8e4e51af0c4706c5492a9d7)

DecisionRAG 使用判斷模型 Jev（jev-1.14.0），並把階層放在 DynamoDB 中。NaiveRAG 則是用 Titan v2 做嵌入，從 S3 Vectors 檢索，再用 Cohere Rerank 3.5 做重新排序。兩者的回答模型都是 Claude Haiku 5.5。

### 🌲 測試資料與階層

[![10-dataset.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F8a844090-a9f6-49d8-88bd-1b2c14b3aa0d.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bf5eab2bcecdf932eb6f34789f146c88)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F8a844090-a9f6-49d8-88bd-1b2c14b3aa0d.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bf5eab2bcecdf932eb6f34789f146c88)

文件發行來源　chunk 數量　模型  
就業規則　厚生勞動省　77  
育兒・介護休業等相關規則的規定範例　厚生勞動省　51  
法令 11 部（勞動基準法等）　e-Gov 法令 API　805  
法令修訂通知　根據 e-Gov 的修法履歷製作　65

DecisionRAG 必須有階層結構（樹狀）才能運作。這次將約 1000 個 chunk 分成 4 個階層：大分類（8）→ 文件（31）→ 章（120）→ 條（998）。

### 📝 評估方式

[![11-questions.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F950642e1-b265-42c5-a8f6-bf8b25c892e8.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2c5a4cc5811d43acf9ce8ec3b427dce5)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F950642e1-b265-42c5-a8f6-bf8b25c892e8.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2c5a4cc5811d43acf9ce8ec3b427dce5)

我把各種不同類型的問題打亂順序，做了 3 輪驗證。

種類　題數　例子  
直白查詢　12　新錄用員工的試用期是幾個月？  
改寫查詢　12　爺爺的喪禮時要請假，公司能休幾天？  
多跳查詢　12　如果是自己提出離職，最晚要提前幾天提出？另外，公司要解雇員工時，法律規定要提前幾天通知？  
數值型　8　課長的職務津貼每個月多少錢？  
時間型　10　請告訴我上個月刊登的所有公告。  
無法回答　10　員工餐廳的每日定食多少錢？

評估指標的意義  
指標　定義  
Hit@1　第 1 名的 chunk 是否正確  
Recall@5　交給回答 LLM 的 chunk 中，包含正解的比例  
TTFT　從送出問題到收到回答第一個 token 為止的時間  
Cost　每個 query 的費用（USD）

### 📊 結果

[![12-result.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Faaafaea9-237d-4c4d-aaad-10639b2ba237.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3d2a2731f81d1b917ee765e5b1d68efd)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2Faaafaea9-237d-4c4d-aaad-10639b2ba237.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3d2a2731f81d1b917ee765e5b1d68efd)

方式　Hit@1　Recall@5　TTFT　Cost/query  
DecisionRAG　0.91　0.91　1.63 秒　$0.0013  
NaiveRAG　0.72　0.78　1.65 秒　$0.0024

這次驗證中，DecisionRAG 在各方面都勝過了 NaiveRAG。

還不能說它比 NaiveRAG 更快。標題中的「快！」請理解成和 NaiveRAG 差不多快的意思。

成本內訳中，DecisionRAG 的 Jev 花費為 $0.0011，回答花費為 $0.0002。NaiveRAG 則是 Rerank 花費 $0.0020，回答花費 $0.0004。其實 NaiveRAG 的費用大多都花在 Rerank 上了……

各類型查詢的 Hit@1 如下。

種類　DecisionRAG　NaiveRAG  
直白查詢　0.97　0.92  
改寫查詢　0.92　0.58  
多跳查詢　0.78　0.75  
數值型　0.96　0.75  
時間型　0.96　0.56  
無法回答　0.90　0.77

尤其在改寫與時間型問題上差距特別大。

### 🤔 DecisionRAG 要採用嗎？

[![13-paraphrase.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F447223d9-1998-4422-b069-1ccec1be00e0.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=908e1b8393bcaf2aa59a7d531eb2189f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F447223d9-1998-4422-b069-1ccec1be00e0.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=908e1b8393bcaf2aa59a7d531eb2189f)

我認為最大的優點是對改寫問題很強。而且變得更聰明了，成本卻幾乎砍半。另外，因為每個階層都會留下分數，所以之後可以追查是在哪一層掉下去的，也更容易調校，這點也很不錯。對於因為表達方式或措辭不同而一直無法做出好精準度的向量搜尋，我認為很有效。反過來說，如果向量搜尋本來就有足夠的精準度，或是沒有使用 Rerank 的情況，導入的好處就比較薄弱。

## 🌱 階層的新鮮度決定回答品質

[![07-freshness.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F5f798633-4791-4e65-8069-eccd214cca5e.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=eb35f71311ea79cd0f05a5ca896ad18a)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2933489%2F5f798633-4791-4e65-8069-eccd214cca5e.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=eb35f71311ea79cd0f05a5ca896ad18a)

我認為 DecisionRAG 的關鍵與其說是搜尋，不如說是階層建構。文件會日積月累地增加，所以需要定期整理分類。搜尋時交給判斷模型，人的重點則放在資料分類的維護上。若在新增資料時，也讓判斷模型先分辨「這比較像既有的書架？還是需要新書架？」就不必每次都由人工判斷，而能在適當時機進行調整。維持階層的新鮮度，會直接反映在回答準確度上。

階層資料要由 LLM 建立，還是由判斷模型建立，做法有很多種，但無論如何都會產生成本。這個成本並沒有算進這次驗證的數字裡。若要採用，建議把階層建構的運作與成本也一併納入評估。

## 🍜 總結

把搜尋交給判斷模型後，精準度比 NaiveRAG 更高，成本則幾乎減半。速度也差不多。雖然標題寫得像「快、便宜、好用」的萬能 RAG，但像 GraphRAG 那種把搜尋結果再接到下一次搜尋的 hop 型搜尋，我認為它還是不太擅長。其他方面也可能會依資料或問題而有適合與不適合的情況。因為成本低，建議先小規模驗證，再考慮導入。

我認為 DecisionRAG 的出現，抓成「RAG 的選項又多了一個」這樣的理解就剛剛好。

RAG 常常會處理機密性很高的資料，因此像 Jev 這類外部 API 有時不太好使用。如果是像 Strands Decider2B 這種可以在自家環境中執行的模型，就能避免把資料送出外部，但因為要讓伺服器持續運作，並不是只按使用量計費。希望像 OpenAI 的 Decisions API 這種按量計費的判斷模型，也能由大型雲端廠商推出呢。

---

原文出處： [https://qiita.com/kikuziro/items/cc82e6e4e4c215eac99f](https://qiita.com/kikuziro/items/cc82e6e4e4c215eac99f)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/q5V5Qx) [72瀏覽](https://codelove.tw/@tony/post/q5V5Qx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/q5V5Qx)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

🔥 [🧠 我試用了 100 個 Claude Skills。這些是最好的。](https://codelove.tw/@tony/post/x216vq)

##### 🏆 本月排行榜

🥇

[boluoji](https://codelove.tw/@boluoji123)

📝17 💬3 ❤️6

354

🥈

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝5 💬2 ❤️1

213

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [川川｜全客製電商：開店平台限制多？我們幫你寫一套自己的。➡️ 了解更多](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1793860943&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fq5V5Qx&amp;pra=5&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguODAiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguODAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjgwIl1dLDBd&amp;dt=1793860943139&amp;bpp=3&amp;bdt=541&amp;idt=356&amp;shv=r20261104&amp;mjsv=m202610300101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793347388%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793347388%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793347388%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=7823694280145&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1553&amp;bih=907&amp;scr_x=0&amp;scr_y=0&amp;eid=95404665%2C95407883%2C95408709%2C95409305%2C122882008&amp;oid=2&amp;pvsid=2701191768841132&amp;tmod=582157393&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1553%2C907&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=387" title="Advertisement" aria-label="Advertisement"></iframe>