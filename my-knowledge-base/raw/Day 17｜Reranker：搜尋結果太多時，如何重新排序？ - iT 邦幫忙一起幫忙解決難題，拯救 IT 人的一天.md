---
title: "Day 17｜Reranker：搜尋結果太多時，如何重新排序？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10404181"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "Day 16 結束時，Hybrid Search 交出了一個完美的候選池——每一題的正解都在前五名裡——但它自己排不好序：獎勵共識的 RRF 在 q16 讓兩個第二名打敗了一個第一名。池子是滿的，缺的..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 17

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 17 篇

## Day 17｜Reranker：搜尋結果太多時，如何重新排序？

- 分享至

Day 16 結束時，Hybrid Search 交出了一個完美的候選池——每一題的正解都在前五名裡——但它自己排不好序：獎勵共識的 RRF 在 q16 讓兩個第二名打敗了一個第一名。池子是滿的，缺的是一個真正讀得懂內容的裁判。今天讓 Reranker 登場，回答 Day 16 留下的兩個問題：q16 和 q18 救得回來嗎？16/16 可及嗎？結果不只回答了這兩題，還帶來一個沒人預告的發現。

## 分開讀，與一起讀

要理解 Reranker，先回頭看語意搜尋的一個結構性限制。Embedding 模型是雙塔式（Bi-encoder）的：查詢和文件 **各自** 被壓縮成一條向量，兩者唯一的互動是最後那一個內積。壓縮是有損的——文件裡哪句話回答了查詢裡哪個疑問，這種細節在各自壓縮的過程中就丟失了。Day 13 和 Day 15 反覆看到的「分數擠在 0.017 裡、排名近乎雜訊」，正是這個結構的必然結果：雙塔只能判斷「主題像不像」，判斷不了「是不是在回答這個問題」。

交叉編碼器（Cross-encoder）把結構反過來：查詢和候選 Chunk **串成同一條輸入** 送進模型，注意力機制讓兩邊的每個 Token 直接互動——它是真的在閱讀這一對文字，輸出一個「這段內容與這個問題的相關程度」的分數。代價同樣是結構性的：因為輸入是「查詢＋文件」的配對，沒有任何東西可以離線預先計算，每次查詢都要對每個候選跑一次完整推論。讓它讀一萬個 Chunk 不現實，讀五個綽綽有餘。

於是兩階段架構的分工就完整了：粗召回（便宜、廣、可預算）負責把對的東西撈進池子，精排（昂貴、準、只讀池子）負責把最對的排到最前面。Day 16 的結尾不是妥協，是檢索系統的標準形狀。

## 實作

Reranker 選用 `BAAI/bge-reranker-base` （中英雙語、278M 參數，CPU 可跑）。誠實聲明：這次沒有像 Day 12 那樣辦選型賽——先驗證兩階段架構本身的價值，Reranker 的選型賽記入待辦，等語料擴大後照舊上秤。

`scripts/search_reranked.py` 是第六位選手。它先向 Hybrid 要一個大小為 5 的候選池（Day 16 證明這個池子覆蓋率 100%），再讓 Cross-encoder 逐對閱讀後重排：

```python
def search(
    query: str,
    chunks: list[Chunk],
    index: RerankedIndex,
    top_k: int = 3,
    pool_size: int = POOL_SIZE,
) -> list[SearchResult]:
    """兩階段檢索：Hybrid 粗召回出候選池，Cross-Encoder 逐對閱讀後精排。"""
    pool = search_hybrid.search(query, chunks, index.hybrid_index, top_k=pool_size)
    if not pool:
        return []

    pairs = [(query, candidate.chunk.text) for candidate in pool]
    scores = index.reranker.predict(pairs)

    reranked = sorted(
        zip(pool, scores, range(1, len(pool) + 1)),
        key=lambda triple: -triple[1],
    )
    return [
        SearchResult(chunk=candidate.chunk, score=float(score), pool_rank=pool_rank)
        for candidate, score, pool_rank in reranked[:top_k]
    ]
```

模型輸出經過 sigmoid，分數落在 0 到 1 之間。每個結果照例帶著帳本： `pool_rank` 記錄它在精排前的池內名次，等一下看它翻盤用。

## 成績單

```markdown
方法                Hit@1        Hit@3
keyword      9/16 (0.56)   12/16 (0.75)
tfidf       11/16 (0.69)   13/16 (0.81)
bm25        12/16 (0.75)   13/16 (0.81)
semantic    14/16 (0.88)   15/16 (0.94)
hybrid      14/16 (0.88)   15/16 (0.94)
reranked    15/16 (0.94)   16/16 (1.00)
```

Hit@1 到達 15/16——Day 15 用 oracle 算出的天花板，兌現了。Hit@3 是 16/16，本系列第一個滿分欄位。Day 7 那個 9/16（以 v2 計）的詞集合基準到今天的 15/16，中間每一步——權重、飽和、向量、融合、精排——都有各自的一題一題的證據。

## 兩題的精排帳本

q16「文章太長了，想切成小段之後再搜尋，該怎麼做？」，就是 RRF 輸給平庸共識的那題：

```markdown
1. rag-chunking#000              score=+0.96（池內原名次 2）
2. nlp-text-cleaning#000         score=+0.13（池內原名次 5）
3. nlp-word-segmentation#001     score=+0.04（池內原名次 1）
```

閱讀裁判毫不猶豫：正解 0.96，靠共識登頂的斷詞文件被打到 0.04、從第 1 摔到第 3。RRF 抹掉的那個資訊——semantic「非常確定」的信心——Cross-encoder 用實際閱讀還了回來，而且給出的差距是 24 倍，不是名次上的一格。

q18「為什麼我用不同的講法搜尋，就找不到同一份文件？」則是另一個結局：

```markdown
1. nlp-word-segmentation#000     score=+0.55（池內原名次 1）
2. rag-embedding#000             score=+0.15（池內原名次 5）
3. rag-chunking#000              score=+0.11（池內原名次 3）
```

斷詞文件又贏了——這是第三次。詞頻方法（BM25）、向量方法（e5）、閱讀方法（Cross-encoder），三種原理完全不同的模型，一致把斷詞文件排在標準答案前面。Day 15 說這是標註的模糊地帶，今天的證據已經壓倒性：這不是方法的失敗，是標註的失敗。依照 Day 15 立下的紀律，我們仍然不在看到成績後改標籤——q18 維持原判，但它升級為雙標註仲裁的第一優先案。換個角度說：在標註沒有爭議的 15 題上，檢索線已經全對；而被標為正解的 Embedding 文件也從池內第 5 爬到第 2，這正是 Hit@3 滿分的來源。

## 池子大小與帳單

兩個工程問題順手驗證。候選池要多大？

```markdown
pool=3   Hit@1=15/16
pool=5   Hit@1=15/16
pool=10  Hit@1=15/16
```

完全不敏感——裁判不會因為多看幾個爛候選就被帶偏，也不需要更大的池子才找得到答案。這讓 pool\_size 成為一個純粹的延遲旋鈕。

帳單方面，平均單次查詢 111 ms（含粗召回與精排），對比 semantic 的 31 ms、BM25 的 0.2 ms——成本階梯又上了一級。但注意精排成本的結構：它等於池子大小乘上單對推論時間， **與語料總量無關** 。語料從 17 個 Chunk 長到十萬個，粗召回層有 Qdrant 的索引撐著，精排永遠只讀五個——這是兩階段架構在成本上的優雅之處。

## 沒人預告的發現：分數終於分開了

系列一路記錄著一個懸案：搜尋分數當不了拒答依據。BM25 是重疊型失效（無答案題的 7.27 混進有答案題中段），semantic 是壓縮型失效（整個宇宙 0.06 寬）。抱著不指望的心情，把兩題無答案題也丟給精排管線：

```markdown
16 題有答案題的 Top-1：最低 +0.16／中位 +0.96／最高 +1.00

q13 模型會產生幻覺的原因是什麼？          Top-1 score=+0.00
q14 Kubernetes 的 Pod 不斷重啟要怎麼排查？  Top-1 score=+0.01
```

分開了。有答案題最低 0.16，無答案題最高 0.01，中間是一條乾淨的空隙；而且有答案題的中位數高達 0.96——分布是雙峰的，不是擠成一團的。原因回到雙塔與交叉的本質差異：Bi-encoder 只能回答「這段文字跟問題像不像」，q14 的 Kubernetes 題和斷詞文件都在「排查問題」的語意雲裡，所以 0.85；Cross-encoder 讀完之後回答的是「這段文字 **是不是在回答** 這個問題」——不是，就是不是。「主題相關」與「可以回答」是兩件事，只有真的閱讀才分得開。

照例要澆冷水：兩題無答案題撐不起任何閾值設定，0.01 與 0.16 之間的空隙可能是運氣。但分布的 **形狀** ——分離對比壓縮——是結構性的訊號。Day 22 設計拒答機制時，精排分數正式成為第一候選方案，屆時要先擴充無答案題再驗證。這是評測集第三次告訴我們它接下來該往哪裡長。

## 檢索線的完全體

到今天，本系列的檢索管線走到了完全體：文件清理（Day 5）與切分（Day 6）打底，斷詞（Day 7）餵給 BM25（Day 9），Embedding（Day 12）住進 Qdrant（Day 13），RRF（Day 16）把兩路召回融成候選池，Cross-encoder（Day 17）精排定序。每一層都有當初的實驗數據說明它為什麼在那裡，Hit@1 從 0.56 一路爬到 0.94 的軌跡全程可重現。

下一篇進入第四階段——生成。檢索找到的 Chunk 要怎麼交給 LLM：上下文怎麼組、順序怎麼排、Prompt 怎麼寫，讓模型的回答建立在這些辛苦找來的資料上，而不是它自己的想像。RAG 的 R 完工了，該寫 G 了。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10404056)

[

Day 16｜Hybrid Search：結合關鍵字與語意搜尋

](https://ithelp.ithome.com.tw/articles/10404056)[下一篇](https://ithelp.ithome.com.tw/articles/10404634)

[

Day 18｜RAG 的核心：如何把搜尋結果交給 LLM？

](https://ithelp.ithome.com.tw/articles/10404634)

系列文

[讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理](https://ithelp.ithome.com.tw/users/20163705/ironman/9159) 共 29 篇

目錄

1. 25
	[Day 25｜評估搜尋品質：Recall@K、MRR 與命中率](https://ithelp.ithome.com.tw/articles/10406131)
2. 26
	[Day 26｜評估回答品質：正確性、引用與幻覺分析](https://ithelp.ithome.com.tw/articles/10406229)
3. 27
	[Day 27｜RAG 常見失敗案例：Chunk、Top-K 與 Prompt 的影響](https://ithelp.ithome.com.tw/articles/10406516)
4. 28
	[Day 28｜RAG 安全性：Prompt Injection 與惡意文件](https://ithelp.ithome.com.tw/articles/10406994)
5. 29
	[Day 29｜把 RAG 包裝成 API：建立可使用的知識助理服務](https://ithelp.ithome.com.tw/articles/10407057)
[完整目錄](https://ithelp.ithome.com.tw/users/20163705/ironman/9159)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/5/24/8195977b-fb7a-48d5-b34c-c805d9a04f65_small.jpg)

[使用 Azure DevOps 面對企業數以百計的系統設計經驗分享](https://itplus.ithome.com.tw/webinar-page/227?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

34 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/14/ffef8fe7-46a7-44eb-a226-bb48986877c1_small.png)

[掌握平台服務 ，擁有完美資料保護](https://itplus.ithome.com.tw/vod-page/923?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/b79a2ccc-cb72-410a-b319-3603c90679c5_small.png)

[在 Kubernetes 環境中進行開源軟件治理的實踐](https://itplus.ithome.com.tw/vod-page/1315?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

32 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/25720d48-2bcc-4bfb-aaec-3fd2c544cd1a_small.png)

[不是我的系統，但與我有關！團隊從「各自為政」到「群體決策」的協作實踐](https://itplus.ithome.com.tw/vod-page/1478?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

26 分