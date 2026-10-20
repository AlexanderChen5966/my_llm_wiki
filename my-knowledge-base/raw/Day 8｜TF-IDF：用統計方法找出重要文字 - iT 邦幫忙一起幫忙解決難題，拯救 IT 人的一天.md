---
title: "Day 8｜TF-IDF：用統計方法找出重要文字 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402766"
author:
  - "[[iThome]]"
published:
created: 2026-08-14
description: "上一篇文章，我們的關鍵字搜尋基準在四種問題上一好三壞，而且所有失敗都指向同一個原因：計分時每個詞都值一分，「是」和「401」完全等值。結尾留下了兩個直覺——一個詞出現在越少文件裡，辨識力越高；在一個..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 8

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 8 篇

## Day 8｜TF-IDF：用統計方法找出重要文字

- 分享至

上一篇文章，我們的關鍵字搜尋基準在四種問題上一好三壞，而且所有失敗都指向同一個原因：計分時每個詞都值一分，「是」和「401」完全等值。結尾留下了兩個直覺——一個詞出現在越少文件裡，辨識力越高；在一個 Chunk 裡出現越多次，關係越深。今天就把這兩個直覺變成可以計算的數字，也就是 TF-IDF。

## 從兩個直覺到一條公式

第一個直覺叫詞頻（Term Frequency，TF）：查詢詞在某個 Chunk 裡出現的次數。「密碼雜湊」在認證文件的段落裡出現兩次，在其他 Chunk 裡零次，這個差異應該反映在分數上。

第二個直覺叫文件頻率（Document Frequency，DF）：一個詞出現在多少份文件裡。「是」幾乎每份文件都有，「401」只在兩個 Chunk 出現——DF 越高的詞，辨識力越低。因為我們要的是「越罕見越值錢」，所以取倒數再取對數，得到逆文件頻率（Inverse Document Frequency，IDF）：

```markdown
idf(詞) = ln( 總文件數 / 出現該詞的文件數 )
```

取對數有兩個作用：把 DF 從 1 到全部之間的巨大差距壓進合理的範圍，以及讓「出現在所有文件裡的詞」自動得到接近零的權重。最後，一個詞對某個 Chunk 的分數就是兩者相乘，查詢的總分則是所有查詢詞的加總：

```markdown
score(查詢, chunk) = Σ tf(詞, chunk) × idf(詞)
```

有一個定義要先說清楚：公式裡的「文件」，在本系列指的是 Chunk，不是原始 Markdown 檔案。因為搜尋與排序的單位是 Chunk，文件頻率自然也以 17 個 Chunk 為母體計算。

## 語料真實的 IDF 長什麼樣子

在寫搜尋程式之前，先看這批語料算出來的實際數值最有感覺：

```markdown
詞            df   idf
是             10   0.53
什麼             4   1.45
如何             1   2.83
搜尋             7   0.89
http           3   1.73
401            2   2.14
csrf           1   2.83
密碼雜湊           1   2.83
```

「是」出現在 17 個 Chunk 中的 10 個，idf 只剩 ln(17/10) = 0.53；「401」只在 2 個 Chunk 出現，idf 是 2.14；「csrf」和「密碼雜湊」都只出現在 1 個 Chunk，拿到最高的 2.83。Day 7 分不出來的「重要程度」，現在直接從語料統計出來了，不需要任何人工標註。

但這張表也埋了一個伏筆：「如何」的 idf 竟然也是 2.83，和「密碼雜湊」一樣高。在只有 17 個 Chunk 的語料裡，「如何」剛好只出現一次，統計上就和真正的珍稀詞無法區分。這個問題等實測時再回來看它闖了什麼禍。

## 實作

建立 `scripts/search_tfidf.py` 。它直接重用 Day 7 的 `tokenize()` 與專案詞典——斷詞規則一旦變了，所有分數都會跟著變，因此整個系列會維持同一套斷詞：

```python
import math
from collections import Counter
from dataclasses import dataclass
from pathlib import Path

from chunk_documents import Chunk, structured_chunks
from load_documents import load_documents
from search_keyword import setup_dictionary, tokenize

@dataclass(frozen=True)
class TfIdfIndex:
    token_counts: dict[str, Counter]
    idf: dict[str, float]
    total_chunks: int

def build_index(chunks: list[Chunk]) -> TfIdfIndex:
    """詞頻與文件頻率都以 Chunk 為單位計算。"""
    token_counts = {chunk.id: Counter(tokenize(chunk.text)) for chunk in chunks}

    document_frequency: Counter = Counter()
    for counts in token_counts.values():
        document_frequency.update(counts.keys())

    total = len(chunks)
    idf = {term: math.log(total / df) for term, df in document_frequency.items()}
    return TfIdfIndex(token_counts=token_counts, idf=idf, total_chunks=total)

@dataclass(frozen=True)
class SearchResult:
    chunk: Chunk
    score: float
    contributions: tuple[tuple[str, float], ...]

def search(
    query: str,
    chunks: list[Chunk],
    index: TfIdfIndex,
    top_k: int = 3,
) -> list[SearchResult]:
    """分數 = 每個查詢詞的 TF × IDF 加總，並記錄每個詞的貢獻。"""
    query_terms = set(tokenize(query))
    results: list[SearchResult] = []

    for chunk in chunks:
        counts = index.token_counts[chunk.id]
        contributions = []
        for term in sorted(query_terms):
            tf = counts.get(term, 0)
            if tf == 0:
                continue
            contributions.append((term, tf * index.idf[term]))

        if contributions:
            score = sum(weight for _, weight in contributions)
            results.append(
                SearchResult(chunk=chunk, score=score, contributions=tuple(contributions))
            )

    results.sort(key=lambda result: (-result.score, result.chunk.id))
    return results[:top_k]

def preview(chunk: Chunk, width: int = 24) -> str:
    text = chunk.text.replace("\n", " ")
    return text[:width] + ("…" if len(text) > width else "")

if __name__ == "__main__":
    setup_dictionary(Path("dict/user_dict.txt"))

    documents = load_documents(Path("knowledge-base"))
    chunks = [chunk for document in documents for chunk in structured_chunks(document)]
    index = build_index(chunks)
    print(f"Indexed {len(chunks)} chunks, vocabulary={len(index.idf)} terms\n")

    queries = [
        ("概念理解", "什麼是 CSRF 攻擊？"),
        ("操作查詢", "如何設定密碼雜湊？"),
        ("技術比較", "關鍵字搜尋和語意搜尋有什麼差別？"),
        ("問題排查", "HTTP 401 是什麼意思"),
    ]

    for question_type, query in queries:
        print(f"[{question_type}] {query}")
        for rank, result in enumerate(search(query, chunks, index), start=1):
            detail = " + ".join(f"{term}:{weight:.2f}" for term, weight in result.contributions)
            print(f"  {rank}. {result.chunk.id:<38} score={result.score:.2f}")
            print(f"     {detail}")
            print(f"     {preview(result.chunk)}")
        print()
```

和 Day 7 相比有兩個升級。第一，索引從「詞集合」變成 `Counter` 詞頻表——這正是從「有沒有出現」進化到「出現幾次」的資料結構對應。第二， `SearchResult` 多了 `contributions` 欄位，記錄每個查詢詞各貢獻了幾分。這不只是為了寫文章方便：當搜尋結果出乎意料時，能看到「這個 Chunk 是靠哪個詞得分的」，是排查問題最快的方法。順帶一提，scikit-learn 的 `TfidfVectorizer` 一行就能建好索引，這裡親手實作的理由，是要把中文斷詞接進流程，並且讓每一分都可以被解釋。

## 同樣四題，重新開賽

```bash
python scripts/search_tfidf.py
```
```markdown
[概念理解] 什麼是 CSRF 攻擊？
  1. spring-security-csrf#000               score=26.56
     csrf:19.83 + 攻擊:5.67 + 是:1.06
  2. rag-retrieval-augmented-generation#000 score=4.49
     什麼:2.89 + 是:1.59
  3. nlp-text-cleaning#000                  score=2.51
     什麼:1.45 + 是:1.06

[操作查詢] 如何設定密碼雜湊？
  1. spring-security-authentication#001     score=5.67
     密碼雜湊:5.67
  2. rag-retrieval-augmented-generation#000 score=2.83
     如何:2.83
  3. nlp-token#000                          score=1.45
     設定:1.45

[技術比較] 關鍵字搜尋和語意搜尋有什麼差別？
  1. rag-embedding#000                      score=7.40
     和:2.83 + 語意搜尋:2.83 + 關鍵字搜尋:1.73
  2. nlp-text-cleaning#000                  score=1.73
     關鍵字搜尋:1.73
  3. nlp-word-segmentation#001              score=1.73
     關鍵字搜尋:1.73

[問題排查] HTTP 401 是什麼意思
  1. spring-security-authentication#005     score=7.75
     401:4.28 + http:3.47
  2. spring-security-filter-chain#000       score=7.34
     401:2.14 + http:5.20
  3. nlp-text-cleaning#000                  score=5.34
     什麼:1.45 + 意思:2.83 + 是:1.06
```

先看 Day 7 慘敗的兩題。問題排查徹底翻盤：真正含有 HTTP 401 的兩個 Chunk 衝上第一、二名，Day 7 靠「什麼／意思／是」奪冠的文字清理文件掉到第三。 `contributions` 把原因寫得一清二楚——「401」一次貢獻 2.14 分，「是」出現兩次也只值 1.06 分。操作查詢同樣翻盤：「密碼雜湊」以 idf 2.83 直接把正解推上第一名，Day 7 它還在和「如何」「設定」同分並列。

概念理解不只答對，領先幅度還從 Day 7 的一分之差擴大到 26.56 比 4.49——「csrf」在那個 Chunk 裡出現了 7 次，7 × 2.83 就貢獻了 19.83 分。技術比較也維持第一。四題的 Top-1 命中，從 Day 7 的 2/4 變成 4/4。

`contributions` 還揭露了兩件光看排名看不到的事。第一，問題排查第二名的 `http:5.20` 是 3 次出現 × 1.73——但那個 Chunk 的正文只提到一次 HTTP，另外兩次來自 Java 程式碼區塊裡的參數名 `http` 與 `http.build()` 。程式碼區塊會參與比對，有時是助力，有時是雜訊，這件事以後評測時要記得。第二，技術比較第一名裡「和」竟然貢獻了 2.83 分，比「關鍵字搜尋」的 1.73 還高；操作查詢的第二名也是靠「如何:2.83」上位的無關文件——正是 IDF 表裡埋的那個伏筆兌現了。

## TF-IDF 還沒解決的事

第一個問題是線性的 TF。「csrf」出現 7 次就拿 7 倍分數，這次剛好幫了正解，但它意味著一個把關鍵詞重複塞滿的長 Chunk 可以無上限地累積分數——出現第 7 次真的和第 1 次一樣有資訊量嗎？直覺上，從 0 次到 1 次是質變，從 6 次到 7 次幾乎沒有增加什麼。這個「報酬遞減」的直覺，TF-IDF 沒有表達出來。

第二個問題是長度。長 Chunk 天生包含更多詞、更多次數，累積分數的機會就是比短 Chunk 多，而目前的公式完全沒有替長度做任何正規化。這兩個問題正是 BM25 要處理的：用飽和函數讓 TF 的報酬遞減，用長度正規化拉平長短 Chunk 的先天差距——這是下一篇的主題。

第三個問題 BM25 也救不了：「如何」與「和」的 idf 高到離譜，是因為語料只有 17 個 Chunk，統計本身就是雜的。停用詞表可以手動壓掉這類詞，語料變大後估計也會自然變準，但在小語料上，IDF 的數字要抱持保留態度——這提醒我們 Day 10 的評測集不能只看單一數字。最後，同義詞的死角依然原封不動：查「登入驗證」永遠比對不到只寫「認證」的文件，統計方法再怎麼加權都無法跨越字面。這道牆，要等 Embedding 來翻。

## 結語

今天用兩個統計量回答了 Day 7 留下的問題：TF 衡量詞和 Chunk 的關係深淺，IDF 從語料本身算出每個詞的辨識力，兩者相乘讓「401」終於比「是」大聲。同樣四題，Top-1 命中從 2/4 變成 4/4，而且 `contributions` 讓每一分都有帳可查——包括程式碼區塊裡的變數名偷偷得分、「如何」在小語料裡被高估這些意料之外的細節。

下一篇是 BM25：它在 TF-IDF 的骨架上加入 TF 飽和與文件長度正規化，是關鍵字搜尋這條路線的集大成者，也是之後和向量搜尋對決的傳統陣營代表。我們會說明它的公式與參數，並觀察它在同樣的問題上和 TF-IDF 有什麼不同。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402521)

[

Day 7｜先不用 AI：中文斷詞與最基本的關鍵字搜尋

](https://ithelp.ithome.com.tw/articles/10402521)

系列文

[讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理](https://ithelp.ithome.com.tw/users/20163705/ironman/9159) 共 8 篇

目錄

1. 4
	[Day 4｜建立知識庫：從 Markdown 文件到可處理資料](https://ithelp.ithome.com.tw/articles/10402198)
2. 5
	[Day 5｜繁體中文文件清理：RAG 前不可忽略的 NLP 基礎](https://ithelp.ithome.com.tw/articles/10402199)
3. 6
	[Day 6｜Chunk 怎麼切才合理？文件切分策略比較](https://ithelp.ithome.com.tw/articles/10402481)
4. 7
	[Day 7｜先不用 AI：中文斷詞與最基本的關鍵字搜尋](https://ithelp.ithome.com.tw/articles/10402521)
5. 8
	[Day 8｜TF-IDF：用統計方法找出重要文字](https://ithelp.ithome.com.tw/articles/10402766)
[完整目錄](https://ithelp.ithome.com.tw/users/20163705/ironman/9159)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/20/470c0e3f-ff77-455f-8ef6-d1d02066a595_small.jpg)

[當原子設計遇上網頁建構器](https://itplus.ithome.com.tw/vod-page/400?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

41 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/22/75dd639b-5c57-4a6c-ba11-532716856cb3_small.png)

[從紅隊角度看零信任資安](https://itplus.ithome.com.tw/vod-page/625?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

零信任資安講堂 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/a27b8e33-68f8-4e7d-acc1-8ccef9b35f49_small.png)

[雲端時代資安新典範：一站式平台全面守護企業 AI 應用](https://itplus.ithome.com.tw/vod-page/1189?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/90e86b92-de73-4ad4-8998-dd8f36eb0489_small.png)

[當AI 帶來思考外包x認知負債，要如何不讓AI 奴役你](https://itplus.ithome.com.tw/vod-page/1479?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

40 分