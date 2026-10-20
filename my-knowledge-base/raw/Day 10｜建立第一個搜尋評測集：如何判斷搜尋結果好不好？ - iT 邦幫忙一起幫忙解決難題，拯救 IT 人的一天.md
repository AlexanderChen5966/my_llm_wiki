---
title: "Day 10｜建立第一個搜尋評測集：如何判斷搜尋結果好不好？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10403167"
author:
  - "[[iThome]]"
published:
created: 2026-08-17
description: "過去三天，關鍵字搜尋從詞集合比對演進到 TF-IDF 再到 BM25，每一步都拿同樣四個問題檢驗。但這個做法撐不了多久：四題是作者順手挑的，數量太少；更麻煩的是，三天下來每個方法都在這四題上被反覆觀察..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 10

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 10 篇

## Day 10｜建立第一個搜尋評測集：如何判斷搜尋結果好不好？

- 分享至

過去三天，關鍵字搜尋從詞集合比對演進到 TF-IDF 再到 BM25，每一步都拿同樣四個問題檢驗。但這個做法撐不了多久：四題是作者順手挑的，數量太少；更麻煩的是，三天下來每個方法都在這四題上被反覆觀察與討論，結論早就有過度擬合的嫌疑。在引入向量搜尋之前，今天先暫停新方法，把「搜尋結果好不好」變成可以重複計算的數字——建立本系列第一個搜尋評測集。

評測集的組成很單純：一批固定的問題、每題的標準答案，以及一個判定命中的規則。它的價值在於之後的每個選擇——換 Embedding 模型、調 Hybrid Search 權重、改切分策略、換斷詞詞典——都可以放上同一座秤，而不是各說各話。

## 三個設計決定

**第一，標準答案標在文件層級，不標 Chunk。** 這是今天最重要的決定。我們的 Chunk 識別碼是 `文件id#序號` ，序號跟著切分策略走——一旦 Day 6 的 structured(400) 換成別的策略，所有 Chunk 識別碼都會重排，標在 Chunk 上的答案立刻作廢。而 Day 6 承諾過「所有切分策略都要用同一組問題重新接受檢驗」，如果標籤本身依賴某個切分策略，這個比較就自我矛盾了。標在文件層級（Day 4 設計的穩定 `id` ），判定規則改成：取回的 Chunk 所屬的 `document_id` 出現在標準答案裡就算命中。文件粒度比較粗，但它讓評測集在切分策略變動下依然有效。

**第二，題目沿用 Day 2 的四種類型，每類三題。** 概念理解、操作查詢、技術比較、問題排查各三題，共十二題。出題有三個原則：不照抄文件原句（否則測的是複製貼上，不是搜尋）；允許口語與同義說法（「差在哪裡」而不是「有什麼差異」）；允許多重標準答案（HTTP 401 同時出現在兩份文件裡，取到任何一份都算對）。

**第三，放入知識庫回答不了的問題。** Day 2 承諾過：問題超出知識庫範圍時，系統應該說資料不足，而不是硬掰。這個能力也需要測試資料——所以評測集裡有兩題「無答案題」，標準答案是空的。其中一題刻意設計得很陰險：「模型會產生幻覺的原因是什麼？」聽起來完全在這個系列的射程內，但知識庫的九份文件裡沒有任何一份談幻覺。評測集測的是知識庫的邊界，不是系列文章聊過什麼。這兩題不計入命中率，但要記錄搜尋分數——它們是 Day 22 設計拒答機制時的第一批證據。

## 評測資料的格式

評測集放在 `data/eval_questions.yaml` ，延續 Day 4 的資料契約精神——人可以直接閱讀與增修，程式也能穩定解析：

```yaml
- id: q01
  type: 概念理解
  query: 什麼是 CSRF 攻擊？
  relevant_docs: [spring-security-csrf]

- id: q10
  type: 問題排查
  query: HTTP 401 是什麼意思？
  relevant_docs: [spring-security-authentication, spring-security-filter-chain]

- id: q13
  type: 無答案
  query: 模型會產生幻覺的原因是什麼？
  relevant_docs: []
```

十四題的完整清單在專案的 `data/` 目錄裡。要誠實記錄一件事：十二題中有三題直接沿用 Day 7 以來的種子題，而專案詞典裡的「密碼雜湊」「關鍵字搜尋」正是當時為這些題目補的詞——評測集與系統之間存在這種歷史糾纏，新增的九題才算第一次見面。這也是為什麼評測集要持續擴充：題目越多、來源越雜，單一題目的糾纏就越稀釋。

## 評測程式

`scripts/evaluate_search.py` 把 Day 7、8、9 的三個搜尋器放上同一座秤。指標用最直觀的命中率：Hit@1（第一名就命中）與 Hit@3（前三名內命中）。更完整的 Recall@K 與 MRR 留到 Day 25，現在先讓數字動起來：

```python
from pathlib import Path

import yaml

import search_bm25
import search_keyword
import search_tfidf
from chunk_documents import structured_chunks
from load_documents import load_documents
from search_keyword import setup_dictionary

TOP_K = 3
TYPES = ["概念理解", "操作查詢", "技術比較", "問題排查"]

def load_questions(path: Path) -> list[dict]:
    return yaml.safe_load(path.read_text(encoding="utf-8"))

def make_searchers(chunks):
    keyword_index = search_keyword.build_index(chunks)
    tfidf_index = search_tfidf.build_index(chunks)
    bm25_index = search_bm25.build_index(chunks)
    return {
        "keyword": lambda query: search_keyword.search(query, chunks, keyword_index, top_k=TOP_K),
        "tfidf": lambda query: search_tfidf.search(query, chunks, tfidf_index, top_k=TOP_K),
        "bm25": lambda query: search_bm25.search(query, chunks, bm25_index, top_k=TOP_K),
    }

def hit_at(results, relevant_docs: list[str], k: int) -> bool:
    return any(result.chunk.document_id in relevant_docs for result in results[:k])
```

主程式對每個方法跑完十二題、統計整體與分類命中率、列出失敗題目，最後印出兩題無答案題的 Top-1 分數，完整程式碼在 `code/day10/` 。

## 首次計分

```bash
python scripts/evaluate_search.py
```
```markdown
方法                Hit@1        Hit@3
keyword      9/12 (0.75)   11/12 (0.92)
tfidf       11/12 (0.92)   11/12 (0.92)
bm25        12/12 (1.00)   12/12 (1.00)

依問題類型（Hit@1）：
類型         keyword   tfidf   bm25
概念理解           3/3     3/3    3/3
操作查詢           2/3     3/3    3/3
技術比較           3/3     3/3    3/3
問題排查           1/3     2/3    3/3
```

三天的方法演進第一次有了正式比分：0.75 → 0.92 → 1.00。更有價值的是分類表——Day 8 說過評測不能只看單一數字，這裡就看到了原因：三個方法在概念理解與技術比較上根本分不出高下，差距全部集中在操作查詢與問題排查。如果只看總分，會以為 BM25「全面領先」；看分類才知道它贏在哪一類問題。

## 解剖失敗的題目

keyword 輸掉的三題中，q04 與 q10 是 Day 7 就認識的老朋友（所有詞都值一分）。真正的新案例是 q12「登入一直失敗，該從哪裡開始查？」——它連 TF-IDF 都騙倒了：

```markdown
TF-IDF 的 Top-3：
  nlp-text-cleaning#000                  4.57  [從:1.73 + 該:2.83]
  rag-retrieval-augmented-generation#000 3.47  [從:3.47]
  rag-embedding#000                      3.18  [失敗:1.73 + 登入:1.45]

BM25 的 Top-3：
  spring-security-authentication#002     4.26  [失敗:2.31 + 登入:1.95]
  nlp-text-cleaning#000                  2.94  [從:1.17 + 該:1.77]
  spring-security-authentication#001     2.74  [從:1.48 + 登入:1.26]
```

TF-IDF 的冠軍靠什麼贏？「該」和「從」。「該」在 17 個 Chunk 裡只出現一次，TF-IDF 把它當成和「密碼雜湊」同級的珍稀詞，給了 2.83 分——Day 8 記錄的小語料 IDF 雜訊（當時的受害者是「如何」和「和」），這次真的偷走了一整題。第三名還有個彩蛋：Embedding 文件靠「失敗／登入」擠進前三，因為它的內文正好拿「搜尋登入驗證比對不到認證」當例子——我們自己寫進語料的同義詞示範，字面撞上了考題。

BM25 答對這題的原因，把 Day 9 的兩個修正各用了一半：RSJ+1 的 IDF 對 df=1 的詞比較克制（「該」從 2.83 降到 2.48），長度正規化則放大了認證文件裡那個含有「登入」「失敗」的短段落。方法的改進不是抽象的——它就落在這種一題一題的翻盤裡。

## 無答案題：分數能當拒答的依據嗎？

兩題無答案題，三個方法都照樣交出了「答案」：

```markdown
q13 模型會產生幻覺的原因是什麼？
  keyword  score=5.00   top1=rag-retrieval-augmented-generation
  tfidf    score=18.81  top1=rag-retrieval-augmented-generation
  bm25     score=7.27   top1=rag-retrieval-augmented-generation

q14 Kubernetes 的 Pod 不斷重啟要怎麼排查？
  keyword  score=2.00   top1=nlp-text-cleaning
  tfidf    score=3.89   top1=nlp-text-cleaning
  bm25     score=2.79   top1=nlp-word-segmentation
```

遠離知識庫的 q14 分數確實偏低，看起來「用分數設個門檻就能拒答」。但近域的 q13 立刻戳破這個想法：它的 BM25 分數是 7.27，而十二題有答案題的 Top-1 分數從 3.30 到 13.65 都有—— **其中四題比 7.27 還低** 。如果把拒答門檻設在 7.27，系統會正確拒絕 q13，同時錯殺四題本來答得出來的問題。搜尋分數不是機率，不同查詢之間的分數根本不可比。「知識庫沒有答案」需要比裸分數更聰明的判斷，這筆帳記給 Day 22。

## 這座秤本身的限制

評測集讓比較變得可能，但它自己也要被誠實檢視。出題和標註都是同一個人，題目的用詞習慣難免偏向作者，真實使用者的問題會更口語、更破碎。十二題只能偵測大差距——0.92 和 1.00 之間差的是「一題」，不是統計顯著的結論；BM25 的滿分更不該被解讀成「已經夠好」，而是「這批題目已經考不倒它」。語料只有九份文件，很多失敗模式（大量近似文件、跨文件比較）根本還沒機會出現。

所以評測集是活的資產，不是一次性交付：語料擴充時要加題，之後每次遇到線上失敗案例，第一件事就是把它變成一道新題目。秤本身也要持續校準。

## 結語

今天沒有引入任何新的搜尋方法，卻可能是第二階段最重要的一天：十四題的評測集（十二題有答案、兩題無答案）、文件層級的標準答案、Hit@1 與 Hit@3 的第一份計分表。三天的方法演進正式定格為 0.75 → 0.92 → 1.00，失敗題目的解剖確認了小語料 IDF 雜訊的實際殺傷力，無答案題則證明裸分數當不了拒答門檻。

下一篇進入第三階段：Embedding。關鍵字路線留下的最後死角——「登入驗證」永遠對不到「認證」的字面牆——要用另一種思路處理：不再比對詞，而是把整段文字變成向量，讓「語意相近」變成可以計算的距離。評測集已經就位，向量方法一來就得先過這十四題。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10403002)

[

Day 9｜BM25：比 TF-IDF 更適合文件搜尋的方法

](https://ithelp.ithome.com.tw/articles/10403002)[下一篇](https://ithelp.ithome.com.tw/articles/10403272)

[

Day 11｜Embedding 是什麼？把文字轉成可以比較的向量

](https://ithelp.ithome.com.tw/articles/10403272)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/65abf984-dca0-4ad7-8c77-2bcead82aa94_small.jpg)

[將漏洞扼殺於搖籃之中：利用紅隊為開發注入能量](https://itplus.ithome.com.tw/vod-page/165?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

27 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/8/8/ec71502f-2910-41da-93ad-7d294e38023f_small.jpg)

[在企業內部播下生成式 AI 認知的種子：實務推動者的經驗分享](https://itplus.ithome.com.tw/webinar-page/271?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

39 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/16/e6a3899f-2409-4f59-9604-7449e2183377_small.png)

[【看見攻擊！與政府共築聯防監控 – 以「預想被駭」檢視防護架構有效性】](https://itplus.ithome.com.tw/vod-page/928?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

32 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/20/e59716a1-34f5-4a83-9fde-ef1c2175b736_small.png)

[不是用 AI，而是和 AI 一起工作：一場來自開發現場的 AI 流程導入](https://itplus.ithome.com.tw/vod-page/1405?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

32 分