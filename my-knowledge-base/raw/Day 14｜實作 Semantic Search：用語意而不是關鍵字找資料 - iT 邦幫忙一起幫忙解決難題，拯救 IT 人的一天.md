---
title: "Day 14｜實作 Semantic Search：用語意而不是關鍵字找資料 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10403702"
author:
  - "[[iThome]]"
published:
created: 2026-08-19
description: "零件已經到齊：Day 12 選定了 multilingual-e5-small，Day 13 把向量安置進 Qdrant，Day 10 的評測集在旁邊等著計分。今天把它們組裝成正式的搜尋元件——第一版..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 14

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 14 篇

## Day 14｜實作 Semantic Search：用語意而不是關鍵字找資料

- 分享至

零件已經到齊：Day 12 選定了 `multilingual-e5-small` ，Day 13 把向量安置進 Qdrant，Day 10 的評測集在旁邊等著計分。今天把它們組裝成正式的搜尋元件——第一版語意搜尋上線，並產出官方成績。先把範圍說清楚：今天交付元件與總成績，關鍵字與向量的逐題深度對決留給下一篇。

## 讓語意搜尋長得像一個搜尋器

Day 7 到 Day 9 的三個搜尋器共享同一套介面： `build_index(chunks)` 建索引、 `search(query, chunks, index, top_k)` 回傳帶著 `chunk` 與 `score` 的結果。語意搜尋照這個規格做，回報是立即的—— `evaluate_search.py` 只需要在搜尋器清單裡多登記一行，不用動任何評測邏輯。之後的 Hybrid Search 也會用同樣的方式進場。介面的穩定，讓「多一個方法」的成本從改程式降到加一行。

建立 `scripts/search_semantic.py` ：

```python
from dataclasses import dataclass

from qdrant_client import QdrantClient
from sentence_transformers import SentenceTransformer

from chunk_documents import Chunk

MODEL_ID = "intfloat/multilingual-e5-small"
COLLECTION = "knowledge_chunks"
STORAGE_PATH = "qdrant_data"
QUERY_PREFIX = "query: "

@dataclass(frozen=True)
class SemanticIndex:
    client: QdrantClient
    model: SentenceTransformer
    chunks_by_id: dict

@dataclass(frozen=True)
class SearchResult:
    chunk: Chunk
    score: float

def build_index(chunks: list[Chunk]) -> SemanticIndex:
    """開啟既有的 Qdrant Collection；向量寫入由 Day 13 的建庫腳本負責。"""
    client = QdrantClient(path=STORAGE_PATH)
    model = SentenceTransformer(MODEL_ID)
    return SemanticIndex(
        client=client,
        model=model,
        chunks_by_id={chunk.id: chunk for chunk in chunks},
    )

def search(
    query: str,
    chunks: list[Chunk],
    index: SemanticIndex,
    top_k: int = 3,
) -> list[SearchResult]:
    """查詢前綴在模組內處理，呼叫端不需要知道模型的使用條款。"""
    query_vector = index.model.encode(QUERY_PREFIX + query, normalize_embeddings=True)
    response = index.client.query_points(
        COLLECTION, query=query_vector.tolist(), limit=top_k
    )
    return [
        SearchResult(
            chunk=index.chunks_by_id[point.payload["chunk_id"]],
            score=float(point.score),
        )
        for point in response.points
    ]
```

兩個設計決定。第一， `query: ` 前綴封裝在模組內部——Day 12 說過用錯前綴品質會默默下降，「默默」是最危險的部分，所以模型的使用條款不能指望每個呼叫端都記得，要收進元件裡。第二，這個模組只讀不寫：向量的產生與寫入仍由 Day 13 的 `build_vector_store.py` 負責，建庫是離線工作、查詢是線上工作，兩者的節奏與失敗模式完全不同，分開是刻意的。

## 官方成績

`evaluate_search.py` 登記第四位選手後重跑：

```bash
python scripts/evaluate_search.py
```
```markdown
方法                Hit@1        Hit@3
keyword      9/12 (0.75)   11/12 (0.92)
tfidf       11/12 (0.92)   11/12 (0.92)
bm25        12/12 (1.00)   12/12 (1.00)
semantic    11/12 (0.92)   12/12 (1.00)

依問題類型（Hit@1）：
類型         keyword     tfidf      bm25  semantic
概念理解           3/3       3/3       3/3       3/3
操作查詢           2/3       3/3       3/3       3/3
技術比較           3/3       3/3       3/3       3/3
問題排查           1/3       2/3       3/3       2/3
```

semantic 的官方成績是 Hit@1 11/12、Hit@3 12/12，輸掉的仍然是 q12——和 Day 12 用暴力矩陣算出來的非正式結果完全一致。這個「一致」本身就是資訊：儲存層從 numpy 換成 Qdrant，成績一分不差，Day 13 的一致性檢查在評測集層級再次通過。

排行榜上 BM25 依然以 12/12 領先。單挑，語意搜尋還是沒有贏——但評測集的十二題以術語型問題為主，那是關鍵字的主場。語意搜尋的主場長什麼樣子，評測集還沒考過，所以接下來讓它秀一下。

## 主場秀：評測集沒考的題型

`scripts/semantic_showcase.py` 準備了三條刻意設計的查詢，BM25 與 semantic 各取 Top-1：

```markdown
Q：How does the system verify who I am?
  BM25：     （無結果）
  semantic： spring-security-authentication#004（score=0.825）

Q：文章太長了，想切成小段之後再搜尋，該怎麼做？
  BM25：     nlp-text-cleaning#000（score=4.18）
  semantic： rag-chunking#000（score=0.887）

Q：系統怎麼確認發出請求的人是誰？
  BM25：     spring-security-csrf#000（score=7.32）
  semantic： spring-security-authentication#000（score=0.861）
```

第一題是英文問句。知識庫全是繁體中文，BM25 連一個詞都對不上，直接交白卷；semantic 找到認證文件——有趣的是它挑中的是那份文件裡的 Java 程式碼區塊 Chunk（ `UserDetailsService` 範例），以文件層級來說完全正確，Day 11 展示的跨語言能力第一次在檢索裡兌現。第二題是口語描述：「切成小段」在語料裡對應的術語是「切分」與「Chunk」，字面對不上，BM25 被「搜尋」這個詞帶去了文字清理文件；semantic 正確走到切分文件。第三題是同義改寫：「確認發出請求的人是誰」就是「認證」的白話版，BM25 被「請求」勾去了 CSRF 文件（那份文件講了很多「請求」），semantic 直取認證文件。

要誠實標註：這三題是為了展示語意能力而設計的主場題，不是公平評測——公平的說法是「關鍵字與語意各有主場，而目前的評測集只蓋到其中一邊」。口語改寫與跨語言題型是評測集 v2 的擴充方向，Day 10 說過秤本身要持續校準，這就是校準的具體項目。

## 無答案題：語意分數的宇宙只有 0.06 寬

Day 10 用 BM25 的分數證明過「裸分數當不了拒答門檻」。語意分數的情況更有戲：

```markdown
q13 模型會產生幻覺的原因是什麼？      semantic score=0.84
q14 Kubernetes 的 Pod 不斷重啟要怎麼排查？  semantic score=0.85
```

先看一個荒謬的細節：跟知識庫完全無關的 Kubernetes 題，分數比聽起來很相關的幻覺題還高。再看整體分布：十二題有答案題的 Top-1 分數落在 0.846 到 0.909 之間，而兩題無答案題是 0.84 與 0.85——貼著有答案題的下緣，只有一題有答案題低於 0.85。整個分數宇宙擠在寬度約 0.06 的區間裡，有答案與無答案幾乎不可分。

對照兩種方法的失效形狀很有意思：BM25 是「重疊型」——無答案題的分數混進有答案題的中段（Day 10 的 7.27 壓過四題）；semantic 是「壓縮型」——所有分數擠成一團，一點雜訊就能翻盤。結論相同且更堅定了：無論哪種方法，拒答都不能靠裸分數做決定。Day 22 要面對的問題，今天又多了一份證據。

## 結語

第一版語意搜尋今天正式上線：與既有搜尋器同介面、前綴封裝、讀寫分離，接上評測集拿到官方成績 Hit@1 11/12、Hit@3 12/12，與 Day 12 的預演完全一致。主場秀展示了評測集沒考的三種能力——跨語言、口語改寫、同義換句——也誠實記下這是設計過的展示題；無答案題則揭露語意分數的壓縮型失效，拒答問題的難度再次確認。

下一篇是正面對決：同一組問題、兩種搜尋方法，逐題攤開比較。誰在哪種題型上贏、失敗剖面重疊多少、如果讓兩邊聯手理論上限在哪裡——這些答案會直接決定 Day 16 Hybrid Search 的設計。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10403540)

[

Day 13｜向量資料庫入門：為什麼需要 Qdrant？

](https://ithelp.ithome.com.tw/articles/10403540)

系列文

[讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理](https://ithelp.ithome.com.tw/users/20163705/ironman/9159) 共 14 篇

目錄

1. 10
	[Day 10｜建立第一個搜尋評測集：如何判斷搜尋結果好不好？](https://ithelp.ithome.com.tw/articles/10403167)
2. 11
	[Day 11｜Embedding 是什麼？把文字轉成可以比較的向量](https://ithelp.ithome.com.tw/articles/10403272)
3. 12
	[Day 12｜繁體中文 Embedding 模型怎麼選？](https://ithelp.ithome.com.tw/articles/10403356)
4. 13
	[Day 13｜向量資料庫入門：為什麼需要 Qdrant？](https://ithelp.ithome.com.tw/articles/10403540)
5. 14
	[Day 14｜實作 Semantic Search：用語意而不是關鍵字找資料](https://ithelp.ithome.com.tw/articles/10403702)
[完整目錄](https://ithelp.ithome.com.tw/users/20163705/ironman/9159)

[![.](https://itadstatic.ithome.com.tw/B9/1786008610_6a745422552c3.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9001)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/b94e4ce7-3e28-457d-aa0b-8768f91a6ff8_small.png)

[強化零信任跨雲端資安韌性：透過 Azure AD & Zscaler ZPA 打造無縫且安全的居家辦公體驗](https://itplus.ithome.com.tw/vod-page/535?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

33 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/5/22/0d550918-d10f-4e03-bffd-88ee819565bb_small.jpg)

[温故知新 - MongoDB Atlas 基礎入門](https://itplus.ithome.com.tw/vod-page/832?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

37 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/538f69c4-1ce2-481f-8d9c-a6414c1175bd_small.png)

[用 Side Project 實踐學習：從小專案到業界實戰](https://itplus.ithome.com.tw/vod-page/1376?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

36 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/5/4/409abf67-43a1-498e-b107-d6cf8f433a54_small.png)

[Hard Lessons Learned from Prometheus Exporters: Observability Challenges in Kubernetes Environments](https://itplus.ithome.com.tw/vod-page/1422?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Observability Day 可觀察性技術日 |

43 分