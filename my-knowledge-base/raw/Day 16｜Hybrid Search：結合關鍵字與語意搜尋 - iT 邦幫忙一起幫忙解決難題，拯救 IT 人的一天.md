---
title: "Day 16｜Hybrid Search：結合關鍵字與語意搜尋 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10404056"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "Day 15 的逐題對決留下一張立案文件：BM25 與語意搜尋的失敗集合幾乎不相交，oracle 聯集指出 Hit@1 的天花板是 15/16。今天實作 Hybrid Search，目標很明確——往那..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 16

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 16 篇

## Day 16｜Hybrid Search：結合關鍵字與語意搜尋

- 分享至

Day 15 的逐題對決留下一張立案文件：BM25 與語意搜尋的失敗集合幾乎不相交，oracle 聯集指出 Hit@1 的天花板是 15/16。今天實作 Hybrid Search，目標很明確——往那個天花板走。先劇透結局：目標沒有達成，而且失敗的方式比成功更有教學價值。

## 為什麼不能把分數加起來

融合的第一直覺是加權平均：BM25 分數乘個權重，加上語意分數乘個權重。但 Day 15 結尾指出過障礙——BM25 給 6.51、Cosine 給 0.862，兩種單位。那正規化到同一區間再加呢？這裡有兩個陷阱。BM25 的分數尺度隨查詢變動（Day 10 看過，同一方法不同題的 Top-1 從 3.30 到 13.65），min-max 正規化的上下界不穩定。語意分數的問題更隱蔽：Day 14 量過，它的分數宇宙只有 0.06 寬——把 0.845 到 0.909 拉伸到 0 至 1，等於把雜訊放大到滿刻度。壓縮型分布做正規化，是把「幾乎分不出」偽裝成「差很多」。

Reciprocal Rank Fusion（RRF）用一個優雅的動作繞開整個問題：不碰分數，只用名次。每個方法各自產出排名，一個 Chunk 的融合分數是：

```markdown
RRF(chunk) = Σ 每個方法 1 / (k + 該方法給它的名次)
```

名次沒有單位問題——第 1 名就是第 1 名，不管它背後是 6.51 還是 0.862。常數 k 控制名次差距的影響力：k 越大，第 1 名和第 5 名的貢獻差越小；慣例值是 60。代價也要先講明白：名次抹掉了「贏多少」的資訊，第 1 名領先 0.3 分和領先 0.001 分，在 RRF 眼裡一樣。這個代價等一下會結出果實。

## 實作

`scripts/search_hybrid.py` 是第五位選手，同一套介面。每個融合結果帶著 `ranks` 帳本——這個 Chunk 在兩邊各排第幾，延續 Day 8 以來「每一分都有帳可查」的傳統：

```python
def search(
    query: str,
    chunks: list[Chunk],
    index: HybridIndex,
    top_k: int = 3,
    k: int = RRF_K,
) -> list[SearchResult]:
    """Reciprocal Rank Fusion：只用名次，不碰兩邊各自的分數。"""
    bm25_results = search_bm25.search(query, chunks, index.bm25_index, top_k=len(chunks))
    semantic_results = search_semantic.search(
        query, chunks, index.semantic_index, top_k=len(chunks)
    )

    fused: dict[str, float] = {}
    ranks: dict[str, dict[str, int]] = {}
    for name, results in (("bm25", bm25_results), ("semantic", semantic_results)):
        for rank, result in enumerate(results, start=1):
            chunk_id = result.chunk.id
            fused[chunk_id] = fused.get(chunk_id, 0.0) + 1.0 / (k + rank)
            ranks.setdefault(chunk_id, {})[name] = rank

    chunks_by_id = {chunk.id: chunk for chunk in chunks}
    ordered = sorted(fused.items(), key=lambda pair: (-pair[1], pair[0]))
    return [
        SearchResult(
            chunk=chunks_by_id[chunk_id],
            score=score,
            ranks=tuple(sorted(ranks[chunk_id].items())),
        )
        for chunk_id, score in ordered[:top_k]
    ]
```

值得注意的細節：BM25 對某個 Chunk 完全沒有匹配詞時，那個 Chunk 不會出現在它的排名裡，於是在 RRF 裡拿不到 BM25 的任何貢獻——「缺席」本身也是一種訊號。

## 成績：天花板沒有兌現

```markdown
方法                Hit@1        Hit@3
keyword      9/16 (0.56)   12/16 (0.75)
tfidf       11/16 (0.69)   13/16 (0.81)
bm25        12/16 (0.75)   13/16 (0.81)
semantic    14/16 (0.88)   15/16 (0.94)
hybrid      14/16 (0.88)   15/16 (0.94)
```

hybrid 是 14/16——和 semantic 打平，離 15/16 的天花板差一題。但總分打平底下藏著一筆交易：hybrid 把 semantic 輸掉的 q12 撿了回來，卻把 semantic 原本第一名的 q16 弄丟了。融合不是加法，是交易。這兩筆帳的 `ranks` 帳本，值得逐筆攤開。

## 兩筆帳的解剖

先看贏的那筆。q12「登入一直失敗，該從哪裡開始查？」：

```markdown
1. spring-security-authentication#002   rrf=0.0315  ranks={'bm25': 1, 'semantic': 6}
2. rag-retrieval-augmented-generation#000            ranks={'bm25': 6, 'semantic': 2}
3. spring-security-authentication#001                ranks={'bm25': 3, 'semantic': 7}
```

正解拿 BM25 第 1、semantic 第 6，照樣登頂——因為它的對手（semantic 排第 1 的斷詞文件）在 BM25 的排名裡 **完全缺席** ，一分跨方法支持都拿不到。單邊冠軍打敗了無援的領先者。

再看輸的那筆。q16「文章太長了，想切成小段之後再搜尋，該怎麼做？」：

```markdown
1. nlp-word-segmentation#001   rrf=0.0323  ranks={'bm25': 2, 'semantic': 2}
2. rag-chunking#000            rrf=0.0315  ranks={'bm25': 6, 'semantic': 1}
```

正解（切分文件）是 semantic 的第 1 名，但 BM25 把它排在第 6；斷詞文件則在兩邊都拿第 2。 **兩個第二名，打敗了一個第一名。** RRF 的本質是獎勵跨方法的共識，而 q16 展示了它的陰暗面：共識可以是平庸的共識。兩個方法都「覺得還行」的錯誤答案，贏過一個方法「非常確定」的正確答案——還記得 RRF 抹掉「贏多少」的那個代價嗎？semantic 在 q16 對切分文件的信心，就是被抹掉的那部分。

把兩筆帳並排，RRF 的性格就完整了：當錯誤答案缺乏跨方法支持時，它幫你贏；當錯誤答案在兩邊都有中等支持時，它讓你輸。

## 參數救不了結構

也許調 k 能救 q16？實測 k 從 1 到 300：

```markdown
k=1    Hit@1=14/16
k=5    Hit@1=14/16
k=10   Hit@1=14/16
k=60   Hit@1=14/16
k=300  Hit@1=14/16
```

紋絲不動。這筆交易是結構性的，不是參數問題。加權 RRF（讓 semantic 的名次比 BM25 值錢）確實存在，也確實可能在這 16 題上湊出 15/16——但 Day 15 才說過，在 16 題上調參數讓數字變漂亮，和看到成績改標籤是同一種病。權重先留在慣例值，等評測集大到調參有統計意義的那天再說。

## 重新定義 Hybrid 的工作

那 Hybrid 是白做了嗎？把 Hit@K 曲線拉出來看：

```markdown
Hit@1   14/16
Hit@3   15/16
Hit@5   16/16
Hit@10  16/16
```

**Hit@5 是 16/16** ——包括那個誰都救不了的 q18（正解在 hybrid 排名第 5）。這是本系列第一個做到「每一題的正解都在前五名」的方法，而且兩個母體都做不到：BM25 對英文題整份排名都是空的，任何 K 都到不了 16；semantic 的 q18 在第 6，Hit@5 只有 15。

這改寫了 Hybrid 的職位描述。它不是更準的 Top-1 排序器——它是更完整的 **召回器** 。檢索系統的經典分工正是如此：粗排負責「把對的東西撈進池子」，精排負責「把池子裡最對的排到最前面」。Hybrid 交出了一個完美的池子，缺的是一個看得懂細節的裁判——一個能真正閱讀查詢與 Chunk 內容、而不是只看向量夾角的模型。那就是 Reranker，下一篇的主角。q16 與 q18 能不能被它救回來，16/16 是不是真的可及，明天用實驗回答。

## 結語

今天用 RRF 實作了 Hybrid Search：不融合分數、只融合名次，優雅地繞開單位問題。成績誠實記錄——Hit@1 停在 14/16，天花板沒有兌現，因為 RRF 用 q16 換了 q12：獎勵共識的機制，同時獎勵了平庸的共識，而 k 掃描證明這是結構而非參數。但 Hit@5 = 16/16 重新定義了它的價值：Hybrid 是本系列第一個完整的召回器，每一題的答案都已經在池子裡。

下一篇讓 Reranker 登場：用一個交叉編碼模型逐一閱讀「查詢與候選 Chunk」的配對，做真正的精排。粗召回與精排的分工一旦完成，檢索這條線就走到了本系列的完全體。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10403784)

[

Day 15｜Keyword Search vs. Vector Search：兩種搜尋方法的比較

](https://ithelp.ithome.com.tw/articles/10403784)[下一篇](https://ithelp.ithome.com.tw/articles/10404181)

[

Day 17｜Reranker：搜尋結果太多時，如何重新排序？

](https://ithelp.ithome.com.tw/articles/10404181)

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