---
title: "Day 9｜BM25：比 TF-IDF 更適合文件搜尋的方法 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10403002"
author:
  - "[[iThome]]"
published:
created: 2026-08-17
description: "上一篇文章，TF-IDF 把四題的 Top-1 命中從 2/4 拉到 4/4，但結尾記下了兩個沒解決的問題：TF 是線性的，「csrf」出現 7 次就拿 7 倍分數，報酬沒有上限；長度沒有正規化，長..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 9

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 9 篇

## Day 9｜BM25：比 TF-IDF 更適合文件搜尋的方法

- 分享至

上一篇文章，TF-IDF 把四題的 Top-1 命中從 2/4 拉到 4/4，但結尾記下了兩個沒解決的問題：TF 是線性的，「csrf」出現 7 次就拿 7 倍分數，報酬沒有上限；長度沒有正規化，長 Chunk 天生有更多累積分數的機會。今天的主角 BM25 正是針對這兩點的修正。它是傳統資訊檢索的集大成者，也是 Elasticsearch 與 Lucene 至今的預設排序公式——關鍵字搜尋這條路線，走到 BM25 就算走到了主流的終點。

## 先解決線性 TF：讓報酬遞減

Day 8 已經指出關鍵直覺：一個詞從 0 次到 1 次是質變，從 6 次到 7 次幾乎沒有新資訊。BM25 用一個飽和函數表達這件事，把原本的 `tf` 換成：

```markdown
tf × (k1 + 1) / (tf + k1 × norm)
```

`k1` 控制飽和的速度， `norm` 是等一下要談的長度正規化（先當作 1）。這個分數在 `tf` 增加時會遞增，但永遠不會超過 `(k1 + 1) × idf` 這個上限。用 Day 8 那個出現 7 次的「csrf」實際算一遍（k1 = 1.5，長度固定為平均值）：

```markdown
tf   TF-IDF    BM25
 1     2.83    2.49
 2     5.67    3.55
 3     8.50    4.14
 4    11.33    4.52
 5    14.17    4.78
 6    17.00    4.97
 7    19.83    5.12
 → BM25 上限 (k1+1)×idf = 6.21
```

TF-IDF 一路線性衝到 19.83；BM25 從第 1 次到第 2 次還能加 1.06 分，從第 6 次到第 7 次只剩 0.15 分，再怎麼重複也頂不破 6.21。「出現很多次」仍然加分，但不再能靠洗詞無限膨脹。

## 再解決長度：正規化

第二個修正藏在分母的 `norm` 裡：

```markdown
norm = 1 − b + b × (chunk 長度 / 平均長度)
```

長度以斷詞後的 Token 數計算，我們這批語料 17 個 Chunk 的平均是 73.1 個 Token。比平均長的 Chunk，分母變大、得分被壓低；比平均短的 Chunk 則被放大。參數 `b` 控制這個效果的強度：b = 0 完全不管長度，b = 1 完全按比例懲罰，慣例值是 0.75。

直覺是：一個 300 Token 的 Chunk 提到一次「401」，和一個 30 Token 的 Chunk 提到一次「401」，後者的密度高得多，關聯性大概也更強。這個假設大多數時候成立——但等一下的實測會看到它的副作用。

## IDF 也換了一個版本

BM25 慣用的 IDF 和 Day 8 的 `ln(N/df)` 不同，來自 Robertson–Spärck Jones 的機率模型：

```markdown
idf(詞) = ln( (N − df + 0.5) / (df + 0.5) + 1 )
```

公式尾巴的「+1」不是裝飾。用語料實際算一遍三種版本：

```markdown
詞       df  ln(N/df)   RSJ 原式   RSJ+1
是       10      0.53     -0.34    0.54
搜尋       7      0.89      0.34    0.88
http      3      1.73      1.42    1.64
401       2      2.14      1.82    1.97
如何       1      2.83      2.40    2.48
```

注意「是」那一列：它出現在 17 個 Chunk 中的 10 個，超過一半，RSJ 原式算出來是 **負的** −0.34——含有「是」的 Chunk 會被倒扣分數，這顯然不合理。Lucene 採用的「+1」變形保證 IDF 永遠為正，本系列跟進。也注意「如何」依然高達 2.48：Day 8 提到的小語料雜訊，換公式並不會消失。

## 完整公式與實作

三個零件組起來，就是 BM25 的全貌：

```markdown
score(查詢, chunk) = Σ idf(詞) × tf × (k1 + 1) / ( tf + k1 × (1 − b + b × len/avglen) )
```

建立 `scripts/search_bm25.py` ，延續 Day 8 的骨架——同一套斷詞、同一種索引結構、同樣保留每個詞的得分貢獻：

```python
import math
from collections import Counter
from dataclasses import dataclass
from pathlib import Path

from chunk_documents import Chunk, structured_chunks
from load_documents import load_documents
from search_keyword import setup_dictionary, tokenize

@dataclass(frozen=True)
class Bm25Index:
    token_counts: dict[str, Counter]
    lengths: dict[str, int]
    average_length: float
    idf: dict[str, float]
    total_chunks: int

def build_index(chunks: list[Chunk]) -> Bm25Index:
    """長度以斷詞後的 Token 數計算；IDF 使用避免負值的 +1 變形。"""
    token_counts: dict[str, Counter] = {}
    lengths: dict[str, int] = {}
    for chunk in chunks:
        tokens = tokenize(chunk.text)
        token_counts[chunk.id] = Counter(tokens)
        lengths[chunk.id] = len(tokens)

    document_frequency: Counter = Counter()
    for counts in token_counts.values():
        document_frequency.update(counts.keys())

    total = len(chunks)
    idf = {
        term: math.log((total - df + 0.5) / (df + 0.5) + 1)
        for term, df in document_frequency.items()
    }
    average_length = sum(lengths.values()) / total
    return Bm25Index(
        token_counts=token_counts,
        lengths=lengths,
        average_length=average_length,
        idf=idf,
        total_chunks=total,
    )

@dataclass(frozen=True)
class SearchResult:
    chunk: Chunk
    score: float
    contributions: tuple[tuple[str, float], ...]

def term_score(
    tf: int, idf: float, length: int, average_length: float, k1: float, b: float
) -> float:
    normalizer = k1 * (1 - b + b * length / average_length)
    return idf * tf * (k1 + 1) / (tf + normalizer)

def search(
    query: str,
    chunks: list[Chunk],
    index: Bm25Index,
    top_k: int = 3,
    k1: float = 1.5,
    b: float = 0.75,
) -> list[SearchResult]:
    query_terms = set(tokenize(query))
    results: list[SearchResult] = []

    for chunk in chunks:
        counts = index.token_counts[chunk.id]
        length = index.lengths[chunk.id]
        contributions = []
        for term in sorted(query_terms):
            tf = counts.get(term, 0)
            if tf == 0:
                continue
            weight = term_score(tf, index.idf[term], length, index.average_length, k1, b)
            contributions.append((term, weight))

        if contributions:
            score = sum(weight for _, weight in contributions)
            results.append(
                SearchResult(chunk=chunk, score=score, contributions=tuple(contributions))
            )

    results.sort(key=lambda result: (-result.score, result.chunk.id))
    return results[:top_k]
```

`k1` 與 `b` 做成參數而不是寫死，等一下要轉動它們觀察行為。

## 同樣四題，第三次開賽

```bash
python scripts/search_bm25.py
```
```markdown
[概念理解] 什麼是 CSRF 攻擊？
  1. spring-security-csrf#000               score=8.17
     csrf:4.67 + 攻擊:2.88 + 是:0.62
  2. rag-retrieval-augmented-generation#000 score=2.37
     什麼:1.61 + 是:0.76
  3. nlp-word-segmentation#000              score=2.31
     什麼:2.31

[操作查詢] 如何設定密碼雜湊？
  1. spring-security-authentication#001     score=3.30
     密碼雜湊:3.30
  2. spring-security-authentication#003     score=2.34
     設定:2.34
  3. rag-retrieval-augmented-generation#000 score=1.88
     如何:1.88

[技術比較] 關鍵字搜尋和語意搜尋有什麼差別？
  1. rag-embedding#000                      score=5.36
     和:2.01 + 語意搜尋:2.01 + 關鍵字搜尋:1.33
  2. nlp-text-cleaning#000                  score=1.17
     關鍵字搜尋:1.17
  3. nlp-word-segmentation#001              score=1.13
     關鍵字搜尋:1.13

[問題排查] HTTP 401 是什麼意思
  1. spring-security-authentication#005     score=6.30
     401:3.44 + http:2.86
  2. spring-security-filter-chain#000       score=5.65
     401:2.54 + http:3.11
  3. nlp-text-cleaning#000                  score=3.36
     什麼:0.99 + 意思:1.77 + 是:0.60
```

Top-1 維持 4/4，但排名底下的世界變了不少。概念理解的冠軍分數從 26.56 回落到 8.17，「csrf」的貢獻從 19.83 壓到 4.67——飽和函數把靠重複膨脹的分數擠掉了，正解依然穩坐第一，只是贏得更有節制。問題排查那題，第二名靠程式碼區塊裡 3 個 `http` 累積的優勢也被飽和壓縮（Day 8 貢獻 5.20，現在 3.11），第一、二名的相對差距反而拉開了。

更有意思的是兩個新面孔，而且它們指向同一件事。概念理解的第三名換成了 `nlp-word-segmentation#000` ——就是 Day 6 記錄過的那個孤兒標題 Chunk，全長只有 8 個 Token，遠短於平均的 73.1，長度正規化把它命中的一個「什麼」放大成 2.31 分。操作查詢的第二名換成了 `authentication#003` （「以下是一個最小的自訂設定：」，7 個 Token），取代了 Day 8 靠「如何」上位的無關文件。後者看起來是進步——第二名從無關文件變成正解隔壁的段落——但得分的真正理由是「超短 Chunk 被長度正規化放大」，結果對了，理由卻可疑。Day 6 的切分瑕疵，在 Day 9 以新的形式回到了排名裡： **檢索流程的每一層都會把上游的問題放大** 。

## 轉動參數會發生什麼

`b` 的效果用概念理解那題最清楚。把 b 從 0 轉到 1，看 Top-3 怎麼變：

```markdown
b=0.0   csrf#000(9.44) | rag-rag#000(2.88)  | text-cleaning#000(2.16)
b=0.75  csrf#000(8.17) | rag-rag#000(2.37)  | word-segmentation#000(2.31)
b=1.0   csrf#000(7.83) | word-segmentation#000(2.98) | rag-rag#000(2.24)
```

b = 0 時完全不管長度，孤兒標題 Chunk 連前三都進不了；b = 0.75 它爬到第三；b = 1 它直接衝上第二。長度正規化不是免費的——它獎勵密度，也就順帶獎勵了那些「短到只剩標題」的碎片。

`k1` 則控制 TF 飽和的速度，看「csrf」的貢獻隨 k1 變化：

```markdown
k1=0.0  csrf 貢獻=2.48（等於 idf，出現幾次都一樣）
k1=0.5  csrf 貢獻=3.36
k1=1.5  csrf 貢獻=4.67
k1=5.0  csrf 貢獻=7.09（越來越接近線性）
```

k1 = 0 的極端很有啟發性：TF 完全飽和，分數退化成「有沒有出現」乘上 IDF——正好是 Day 7 基準的加權版。k1 越大，行為越接近 Day 8 的線性 TF。換句話說，這條路線前三天的方法，其實都住在 BM25 的參數空間裡。

本系列採用 k1 = 1.5、b = 0.75 的慣例值。要強調的是：今天轉動參數只是為了理解行為，不是在調參——在沒有評測集之前，任何「這組參數比較好」的結論都只是對四個問題的過度擬合。

## 關鍵字路線走到這裡

從 Day 7 到今天，同一批 Chunk、同一套斷詞、同樣四個問題，方法從詞集合比對演進到 TF-IDF 再到 BM25，每一步的改善和代價都留下了可檢查的紀錄。BM25 之後，這條路線還剩三個死角：同義詞的字面牆依舊在（查「登入驗證」永遠對不到只寫「認證」的文件）；斷詞與切分的品質問題會被排序層放大；小語料的 IDF 雜訊只能靠更多資料或停用詞表緩解。

但下一步不是急著引入向量搜尋。四個問題太少了——今天好幾個結論都建立在「看這四題的排名變化」上，這種觀察撐不起「哪個方法更好」的判斷。先建立一個像樣的評測集，讓 Recall 與排名品質變成可以重複計算的數字，之後每引入一個新方法才有公正的裁判。這就是下一篇的工作。

## 結語

今天完成了 BM25：用飽和函數讓 TF 報酬遞減（「csrf」的 19.83 分回到 4.67），用長度正規化拉平長短 Chunk 的先天差距，並換上不會出現負值的 RSJ+1 版 IDF（「是」在原式下會是 −0.34）。四題維持 4/4，參數實驗則揭露了一體兩面：長度正規化壓制了長 Chunk 的膨脹，也放大了 Day 6 遺留的碎片 Chunk。

下一篇，我們暫停引入新方法，先做這個系列一直在鋪墊的事：建立第一個搜尋評測集。用 Day 2 的四種問題類型設計問題、替每一題標註標準答案，讓「搜尋結果好不好」從逐題肉眼判讀，變成可以重複計算、可以比較的數字。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402766)

[

Day 8｜TF-IDF：用統計方法找出重要文字

](https://ithelp.ithome.com.tw/articles/10402766)[下一篇](https://ithelp.ithome.com.tw/articles/10403167)

[

Day 10｜建立第一個搜尋評測集：如何判斷搜尋結果好不好？

](https://ithelp.ithome.com.tw/articles/10403167)

系列文

[讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理](https://ithelp.ithome.com.tw/users/20163705/ironman/9159) 共 12 篇

目錄

1. 8
	[Day 8｜TF-IDF：用統計方法找出重要文字](https://ithelp.ithome.com.tw/articles/10402766)
2. 9
	[Day 9｜BM25：比 TF-IDF 更適合文件搜尋的方法](https://ithelp.ithome.com.tw/articles/10403002)
3. 10
	[Day 10｜建立第一個搜尋評測集：如何判斷搜尋結果好不好？](https://ithelp.ithome.com.tw/articles/10403167)
4. 11
	[Day 11｜Embedding 是什麼？把文字轉成可以比較的向量](https://ithelp.ithome.com.tw/articles/10403272)
5. 12
	[Day 12｜繁體中文 Embedding 模型怎麼選？](https://ithelp.ithome.com.tw/articles/10403356)
[完整目錄](https://ithelp.ithome.com.tw/users/20163705/ironman/9159)

[![.](https://itadstatic.ithome.com.tw/B9/1786896772_6a81e18491c26.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/21/999a0a5b-2bf0-4591-a6bf-4d2d622a1e05_small.png)

[使用真勇者的版控流程－－主幹開發，大幅提高程式穩定性](https://itplus.ithome.com.tw/vod-page/776?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/1/4/e182a067-2b88-4cad-9556-9257a6a1e9b8_small.png)

[【以Cloud Native雲端原生工具打造雲端安全】](https://itplus.ithome.com.tw/vod-page/799?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

安碁資訊｜資安防護服務．企業營運夥伴 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/86cf085b-df39-4592-b445-0c8314cf97ff_small.png)

[前端可觀測性 - Grafana 在下的一盤大棋](https://itplus.ithome.com.tw/vod-page/1040?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

27 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/b5c33eaa-9086-4e86-9382-a01910a707db_small.png)

[我們與「敏捷」的距離](https://itplus.ithome.com.tw/vod-page/1360?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

48 分