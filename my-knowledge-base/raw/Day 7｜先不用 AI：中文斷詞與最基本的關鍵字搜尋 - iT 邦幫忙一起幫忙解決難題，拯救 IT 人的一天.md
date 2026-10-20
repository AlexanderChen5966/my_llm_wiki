---
title: "Day 7｜先不用 AI：中文斷詞與最基本的關鍵字搜尋 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402521"
author:
  - "[[iThome]]"
published:
created: 2026-08-13
description: "上一篇文章，我們把九份測試語料切成了 17 個 structured Chunk，也留下一個伏筆：「向量資料庫」被切成「向量／資料庫」還是「向量資料／庫」，會直接決定關鍵字搜尋找不找得到它。今天就來面..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 7

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 7 篇

## Day 7｜先不用 AI：中文斷詞與最基本的關鍵字搜尋

- 分享至

上一篇文章，我們把九份測試語料切成了 17 個 structured Chunk，也留下一個伏筆：「向量資料庫」被切成「向量／資料庫」還是「向量資料／庫」，會直接決定關鍵字搜尋找不找得到它。今天就來面對這個問題——處理中文斷詞，並建立本系列第一個搜尋基準。

先說清楚為什麼要做基準。這個系列的定位是「每個技術選擇都能透過實驗結果驗證」，而驗證需要一個比較的地板。之後的 TF-IDF、BM25、向量搜尋與 Hybrid Search，都要回答同一個問題：你比最簡單的做法好多少？如果沒有先把最簡單的做法做出來，這個問題永遠只能用感覺回答。所以今天不用 Embedding、不用模型，只用斷詞和詞的比對，把地板鋪好。

## 為什麼問句搜不到答案？

最原始的搜尋是子字串比對：查詢文字直接在 Chunk 裡找。在 17 個 Chunk 上實測三個查詢，結果是這樣的：

```markdown
substring「如何設定密碼雜湊」命中 0 個 chunk
substring「密碼雜湊」命中 1 個 chunk
substring「SecurityFilterChain」命中 3 個 chunk
```

子字串比對對精確的識別字很有效——查 `SecurityFilterChain` 可以直接命中三個 Chunk。但使用者不會只查識別字，他們會問問題，而「如何設定密碼雜湊」這個完整字串不會一字不差地出現在任何文件裡，於是命中數是零。要讓問句搜得到答案，就必須把問句拆成詞，再分別比對。

英文拆詞靠空白就夠了，中文的詞之間卻沒有邊界。「如何設定密碼雜湊」要先變成「如何／設定／密碼雜湊」，才有東西可以比對——這就是斷詞（Word Segmentation），也是中文搜尋繞不開的前置步驟。

## jieba 預設詞典切出來的繁體中文

本系列使用 jieba 作為斷詞工具（版本 0.42.1），理由是安裝簡單、速度快，適合先把流程建起來：

```bash
python -m pip install jieba
```

但直接拿預設設定來切繁體中文，結果比 Day 6 猜的還要慘。建立 `scripts/segment_demo.py` ：

```python
from pathlib import Path

import jieba

jieba.setLogLevel(60)  # 關閉載入詞典時的除錯訊息

sentences = [
    "向量資料庫入門",
    "關鍵字搜尋和語意搜尋有什麼差別？",
    "中文斷詞會影響搜尋品質",
    "使用 SecurityFilterChain 保護 API 路徑",
]

def show(title: str) -> None:
    print(f"=== {title} ===")
    for sentence in sentences:
        print(f"{sentence}\n  → {' / '.join(jieba.lcut(sentence))}")
    print()

show("預設詞典")
jieba.load_userdict(str(Path("dict/user_dict.txt")))
show("載入專案詞典後")
```

預設詞典的輸出是：

```markdown
向量資料庫入門
  → 向量 / 資料 / 庫入門
關鍵字搜尋和語意搜尋有什麼差別？
  → 關鍵 / 字 / 搜 / 尋和語 / 意 / 搜尋有 / 什麼 / 差別 / ？
中文斷詞會影響搜尋品質
  → 中文 / 斷 / 詞會 / 影響 / 搜尋品 / 質
```

Day 6 預想的兩種切法都沒出現，實際結果是第三種：「庫入門」被當成了一個詞。第二句更誇張，「搜尋」被攔腰切開，組出「尋和語」「搜尋有」這種不存在的詞。原因不難理解：jieba 的預設詞典以簡體中文與通用語料為主，「搜尋」「品質」這些繁體用語根本不在詞典裡，模型只能靠統計硬猜。

值得慶幸的是英文識別字毫髮無傷： `SecurityFilterChain` 、 `CSRF` 、 `HTTP` 、 `401` 都保持完整，因為 jieba 對連續的英數字元不做切分。對技術文件來說，這保住了最重要的一類查詢線索。

中文詞的問題，今天用最小的方案處理：建立專案詞典 `dict/user_dict.txt` ，收錄語料中的領域詞與預設詞典缺少的繁體詞，例如「搜尋」「品質」「向量資料庫」「斷詞」「密碼雜湊」，共二十個詞，再用 `jieba.load_userdict()` 載入。載入後的輸出：

```markdown
向量資料庫入門
  → 向量資料庫 / 入門
關鍵字搜尋和語意搜尋有什麼差別？
  → 關鍵字搜尋 / 和 / 語意搜尋 / 有什麼 / 差別 / ？
中文斷詞會影響搜尋品質
  → 中文 / 斷詞 / 會 / 影響 / 搜尋 / 品質
```

要誠實說明：手工維護詞典不是可以無限擴張的做法。更完整的選項包括 jieba 的繁體詞典 `dict.txt.big` 與中研院的 CKIP 系列模型，但「哪個斷詞方案讓搜尋品質更好」是一個應該用數據回答的問題——等 Day 10 有了評測集，這些方案都可以放在同一組問題上比較。今天先用二十個詞的專案詞典，把基準建起來。

## 建立搜尋基準

接著建立 `scripts/search_keyword.py` 。它從 Day 6 接手 structured Chunk，斷詞後建立索引，用最直白的方式計分：

```python
from dataclasses import dataclass
from pathlib import Path
import re

import jieba

from chunk_documents import Chunk, structured_chunks
from load_documents import load_documents

# 保留含有文字或數字的 Token，過濾純標點與空白
_WORD_PATTERN = re.compile(r"\w")

def setup_dictionary(path: Path) -> None:
    """載入專案詞典，補上預設詞典缺少的繁體詞與領域詞。"""
    jieba.setLogLevel(60)
    jieba.load_userdict(str(path))

def tokenize(text: str) -> list[str]:
    tokens: list[str] = []
    for token in jieba.cut(text):
        token = token.strip().lower()
        if token and _WORD_PATTERN.search(token):
            tokens.append(token)
    return tokens

@dataclass(frozen=True)
class SearchResult:
    chunk: Chunk
    score: int
    matched: tuple[str, ...]

def build_index(chunks: list[Chunk]) -> dict[str, set[str]]:
    """索引：Chunk 識別碼對應到它包含的詞集合。"""
    return {chunk.id: set(tokenize(chunk.text)) for chunk in chunks}

def search(
    query: str,
    chunks: list[Chunk],
    index: dict[str, set[str]],
    top_k: int = 3,
) -> list[SearchResult]:
    """最簡單的基準：分數 = 查詢詞中出現在 Chunk 裡的詞數。"""
    query_terms = set(tokenize(query))
    results: list[SearchResult] = []

    for chunk in chunks:
        matched = query_terms & index[chunk.id]
        if matched:
            results.append(
                SearchResult(chunk=chunk, score=len(matched), matched=tuple(sorted(matched)))
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
    print(f"Indexed {len(chunks)} chunks from {len(documents)} documents\n")

    queries = [
        ("概念理解", "什麼是 CSRF 攻擊？"),
        ("操作查詢", "如何設定密碼雜湊？"),
        ("技術比較", "關鍵字搜尋和語意搜尋有什麼差別？"),
        ("問題排查", "HTTP 401 是什麼意思"),
    ]

    for question_type, query in queries:
        print(f"[{question_type}] {query}")
        print(f"  查詢詞：{' / '.join(sorted(set(tokenize(query))))}")
        for rank, result in enumerate(search(query, chunks, index), start=1):
            print(
                f"  {rank}. {result.chunk.id:<36} score={result.score}  "
                f"matched={','.join(result.matched)}"
            )
            print(f"     {preview(result.chunk)}")
        print()
```

三個設計決定值得說明。第一， `tokenize()` 同時用於建立索引與處理查詢——文件和查詢必須用同一套切法，否則同一個詞在兩邊被切成不同樣子，永遠比對不上。第二，索引沿用 Day 6 的 Chunk 識別碼，搜尋結果天生就能追溯回原始文件。第三，計分刻意不加任何權重：分數就是「查詢詞中有幾個出現在這個 Chunk」，同分時按識別碼排序。這不是因為權重不重要，而是基準的意義就在於誠實記錄「什麼都不做會怎樣」。

## 用 Day 2 的四種問題實測

Day 2 把使用者問題分成四種情境：概念理解、操作查詢、技術比較與問題排查。現在正好用它們各出一題：

```bash
python scripts/search_keyword.py
```
```markdown
Indexed 17 chunks from 9 documents

[概念理解] 什麼是 CSRF 攻擊？
  查詢詞：csrf / 什麼 / 攻擊 / 是
  1. spring-security-csrf#000             score=3  matched=csrf,攻擊,是
     # CSRF 保護的原理與設定  CSRF（Cr…
  2. nlp-text-cleaning#000                score=2  matched=什麼,是
     # 技術文件的文字清理原則  文字清理的目的不是…
  3. nlp-token#000                        score=2  matched=什麼,是
     # Token 是什麼  Token 是語言模型…

[操作查詢] 如何設定密碼雜湊？
  查詢詞：如何 / 密碼雜湊 / 設定
  1. nlp-token#000                        score=1  matched=設定
     # Token 是什麼  Token 是語言模型…
  2. rag-retrieval-augmented-generation#000 score=1  matched=如何
     # 什麼是 Retrieval-Augmente…
  3. spring-security-authentication#001   score=1  matched=密碼雜湊
     AuthenticationManager 本身…

[技術比較] 關鍵字搜尋和語意搜尋有什麼差別？
  查詢詞：和 / 差別 / 有什麼 / 語意搜尋 / 關鍵字搜尋
  1. rag-embedding#000                    score=3  matched=和,語意搜尋,關鍵字搜尋
     # Embedding 與語意相似度  Embe…
  2. nlp-text-cleaning#000                score=1  matched=關鍵字搜尋
     # 技術文件的文字清理原則  文字清理的目的不是…
  3. nlp-word-segmentation#001            score=1  matched=關鍵字搜尋
     英文以空白分隔單字，中文句子裡的詞之間卻沒有明確…

[問題排查] HTTP 401 是什麼意思
  查詢詞：401 / http / 什麼 / 意思 / 是
  1. nlp-text-cleaning#000                score=3  matched=什麼,意思,是
     # 技術文件的文字清理原則  文字清理的目的不是…
  2. nlp-token#000                        score=2  matched=什麼,是
     # Token 是什麼  Token 是語言模型…
  3. rag-retrieval-augmented-generation#000 score=2  matched=什麼,是
     # 什麼是 Retrieval-Augmente…
```

四題四種結局。概念理解答對了：CSRF 是整個語料庫裡的罕見詞，一命中就把正確的 Chunk 推上第一名。技術比較勉強及格：第一名確實是 Embedding 文件，但仔細看，連「和」都貢獻了一分。

操作查詢開始露餡。真正的答案——認證文件裡談 PasswordEncoder 與密碼雜湊的段落——只排第三，而且它和第一、二名同分：命中「密碼雜湊」得一分，命中「設定」或「如何」也得一分，同分之後按識別碼的字母序排，正解就這樣被排到了最後。

問題排查則是全面潰敗。把排名完整展開會看到：

```markdown
1. nlp-text-cleaning#000                  score=3 matched=什麼,意思,是
2. nlp-token#000                          score=2 matched=什麼,是
3. rag-retrieval-augmented-generation#000 score=2 matched=什麼,是
4. spring-security-authentication#005     score=2 matched=401,http
5. spring-security-csrf#000               score=2 matched=http,是
6. spring-security-filter-chain#000       score=2 matched=401,http
```

真正含有 HTTP 401 的兩個 Chunk 排在第四與第六名，Top-3 裡一個都沒有。第一名是文字清理的文件，它對這個問題毫無幫助，卻靠「什麼」「意思」「是」三個到處都有的詞拿下最高分。還有一個小插曲：正解 Chunk 的原文是「權限不足則是 HTTP 403」，「則是」被斷成一個詞，所以它連「是」這一分都沒拿到——斷詞的細節會從意想不到的方向影響結果。

## 基準告訴我們什麼

四題的失敗模式其實是同一個： **所有詞都值一分** 。「是」出現在 17 個 Chunk 中的絕大多數，「401」只出現在兩個，但在今天的計分裡它們完全等值。這立刻指向兩個直覺：一個詞出現在越少文件裡，它的辨識力應該越高；一個詞在某個 Chunk 裡出現得越頻繁，這個 Chunk 和它的關係應該越深。前者是文件頻率（Document Frequency），後者是詞頻（Term Frequency）——把這兩個直覺變成公式，就是下一篇的 TF-IDF。

但今天的基準絕不是白做的。它用四個問題誠實記錄了「不加權重會發生什麼」，之後每引入一個新方法，都要回到這四題（以及 Day 10 更完整的評測集）證明自己真的有比較好。地板鋪好了，接下來才輪得到樓層。

## 結語

今天完成了三件事：確認子字串比對只對精確識別字有效、用專案詞典修正 jieba 預設詞典切壞繁體詞的問題，並在 17 個 Chunk 上建立了第一個關鍵字搜尋基準。四種問題類型的實測結果一好三壞，失敗原因全部指向同一件事——沒有權重的詞比對，分不出「401」和「是」哪個重要。

下一篇就來解決它：TF-IDF 用詞頻與文件頻率替每個詞算出權重，讓罕見而精準的詞說話大聲一點，讓到處都有的詞安靜下來。我們會親手實作它，並用今天同樣的四個問題檢驗改善了多少。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402481)

[

Day 6｜Chunk 怎麼切才合理？文件切分策略比較

](https://ithelp.ithome.com.tw/articles/10402481)

系列文

[讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理](https://ithelp.ithome.com.tw/users/20163705/ironman/9159) 共 7 篇

目錄

1. 3
	[Day 3｜LLM 到底知道什麼？Token、Context 與知識限制](https://ithelp.ithome.com.tw/articles/10402075)
2. 4
	[Day 4｜建立知識庫：從 Markdown 文件到可處理資料](https://ithelp.ithome.com.tw/articles/10402198)
3. 5
	[Day 5｜繁體中文文件清理：RAG 前不可忽略的 NLP 基礎](https://ithelp.ithome.com.tw/articles/10402199)
4. 6
	[Day 6｜Chunk 怎麼切才合理？文件切分策略比較](https://ithelp.ithome.com.tw/articles/10402481)
5. 7
	[Day 7｜先不用 AI：中文斷詞與最基本的關鍵字搜尋](https://ithelp.ithome.com.tw/articles/10402521)
[完整目錄](https://ithelp.ithome.com.tw/users/20163705/ironman/9159)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/76ac2fbc-7cb0-4298-9b9f-68fadd7c7073_small.jpg)

[供應鏈資安威脅加劇，劃分角色及權利義務為防禦致勝關鍵](https://itplus.ithome.com.tw/vod-page/331?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

32 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/2/5/4c83ac48-ecb0-4d23-ac92-61af37a0069e_small.png)

[Azure Kubernetes Service with GitOps](https://itplus.ithome.com.tw/vod-page/1125?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

92 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/93969e69-3a06-41e5-9bf2-6e3bc6246c5b_small.png)

[小改變，大未來！AI Cloud 讓效能翻倍](https://itplus.ithome.com.tw/vod-page/1223?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

21 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/d20c172e-ebf3-456e-8dfe-a28d69b5f609_small.png)

[超越自動化：基於 Google Cloud 的企業級 AI Agent 協作新模式](https://itplus.ithome.com.tw/vod-page/1204?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

40 分