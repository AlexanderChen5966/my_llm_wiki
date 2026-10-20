---
title: "Day 12｜繁體中文 Embedding 模型怎麼選？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10403356"
author:
  - "[[iThome]]"
published:
created: 2026-08-17
description: "上一篇用一個當黑盒子的模型示範了 Embedding 如何翻過字面牆，也留下承諾：模型不能永遠是黑盒子，今天要正式選型。選型的原則延續整個系列的態度——排行榜可以拿來初篩，但最後讓自己的評測集決定。..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 12

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 12 篇

## Day 12｜繁體中文 Embedding 模型怎麼選？

- 分享至

上一篇用一個當黑盒子的模型示範了 Embedding 如何翻過字面牆，也留下承諾：模型不能永遠是黑盒子，今天要正式選型。選型的原則延續整個系列的態度——排行榜可以拿來初篩，但最後讓自己的評測集決定。

## 排行榜能信多少？

挑 Embedding 模型，多數人會先看 MTEB 或它的中文版 C-MTEB 排行榜。榜單是有用的初篩工具，但有三件事要想清楚。第一，榜單的評測語料不是你的語料——在新聞與百科上的高分，不保證在繁體中文技術文件上成立。第二，中文榜單以簡體語料為絕對主力，繁體中文的表現基本上是盲區，只能自己驗。第三，榜首模型通常很大，而本系列堅持所有實驗在筆電 CPU 上可重現，所以候選全部限定小模型，大模型（如 bge-m3）列為之後的升級選項。

今天的三個候選，各有各的脾氣：

**`paraphrase-multilingual-MiniLM-L12-v2`** （118M 參數，384 維）：Day 11 的黑盒子。多語言，但注意它的訓練目標是「改寫句配對」——判斷兩句是不是同義，和「查詢找文件」不完全是同一件事。

**`BAAI/bge-small-zh-v1.5`** （24M 參數，512 維）：中文特化、檢索特化，三者中最小。一個有趣的細節：它官方建議的查詢前綴是「为这个句子生成表示以用于检索相关文章：」——一句簡體中文。模型的出身寫在使用說明裡，繁體中文對它來說是不是主場，值得懷疑，正好用實驗回答。

**`intfloat/multilingual-e5-small`** （118M 參數，384 維）：多語言、檢索特化。它強制要求前綴：查詢加 `query: `、文件加 `passage: `。這不是裝飾——檢索特化模型把查詢與文件當成兩種不同的角色編碼（查詢短而口語，文件長而正式，兩者本來就不該進同一個模子），用錯或漏用前綴，品質會默默下降。這叫非對稱編碼，是拿檢索模型時第一個要查清楚的使用條款。

比較的公平原則：每個模型都用它自己的建議姿勢上場，該加的前綴加好，然後全部面對同樣的十二題。

## 評測程式

`scripts/compare_embeddings.py` 對每個候選做四件事：把 17 個 Chunk 編碼成向量（計時）、把十二題查詢逐一編碼、取 Cosine 最高的前三名用 Day 10 的規則判定命中，最後加一個繁簡探針——同一句話的繁體版與簡體版，各模型給的相似度。核心邏輯如下：

```python
for candidate in CANDIDATES:
    model = SentenceTransformer(candidate.model_id)

    passage_texts = [candidate.passage_prefix + chunk.text for chunk in chunks]
    model.encode(["暖身用的句子"])  # 排除第一次推論的初始化成本
    start = time.perf_counter()
    chunk_vectors = model.encode(passage_texts, normalize_embeddings=True)
    encode_seconds = time.perf_counter() - start

    for question in answerable:
        query_vector = model.encode(
            candidate.query_prefix + question["query"], normalize_embeddings=True
        )
        scores = chunk_vectors @ query_vector
        ranked = sorted(zip(chunks, scores), key=lambda pair: -pair[1])
```

一個量測陷阱值得記錄：第一版沒有暖身那一行，結果第一個受測模型多背了 PyTorch 初始化的帳，「慢」了四倍。兩個架構幾乎相同的模型測出四倍差距，這種不合理的數字就是量測有問題的警報。

## 結果

```bash
python scripts/compare_embeddings.py
```
```markdown
模型                      參數(M)   維度  編碼17chunk   Hit@1   Hit@3
minilm-multilingual          118    384       0.08s   11/12   11/12
bge-small-zh-v1.5             24    512       0.10s   11/12   12/12
multilingual-e5-small        118    384       0.12s   11/12   12/12
（參考）BM25                    -      -           -   12/12   12/12

繁簡同句的 Cosine 相似度：
  minilm-multilingual      0.982
  bge-small-zh-v1.5        0.902
  multilingual-e5-small    0.953

Hit@1 失敗題：
  [minilm-multilingual]   q07 關鍵字搜尋和語意搜尋有什麼差別？  Top-1=rag-retrieval-augmented-generation
  [bge-small-zh-v1.5]     q07 關鍵字搜尋和語意搜尋有什麼差別？  Top-1=nlp-word-segmentation
  [multilingual-e5-small] q12 登入一直失敗，該從哪裡開始查？    Top-1=nlp-word-segmentation
```

第一個結論可能出乎很多人意料： **沒有任何一個向量模型打贏 BM25** 。三個模型的 Hit@1 都是 11/12，BM25 是 12/12。語意搜尋不是關鍵字搜尋的自動升級版——至少在這批以技術術語為主的題目上不是。

失敗題目更有戲。minilm 和 bge 摔在同一題：q07「關鍵字搜尋和語意搜尋有什麼差別？」—— **語意搜尋答不好「語意搜尋是什麼」** 。這不是笑話，而是有結構性原因的：這一題的查詢和標準答案文件共享「關鍵字搜尋」「語意搜尋」兩個精確術語，字面比對本來就是此時最強的訊號；而向量模型看到的是「這個查詢在講搜尋」，於是把同樣大談搜尋的斷詞文件、RAG 文件都拉得很近，反而把訊號稀釋了。查詢與文件共享精確術語時，關鍵字是主場；術語對不上時，才輪到語意出頭。

互補性不只是理論，這批數據裡就有證據：Day 10 keyword 輸掉的 q04 與 q10，三個向量模型的 Hit@1 全對；連 TF-IDF 都被「該」騙走的 q12，minilm 和 bge 也都答對了——向量修好了字面方法的失敗題，卻在術語題上跌倒。兩邊的失敗剖面幾乎不重疊，這就是 Day 16 Hybrid Search 存在的理由，現在它有了實證基礎。

繁簡探針則出現一個反轉：對繁簡差異最敏感的，竟然是唯一的中文特化模型——bge 只給 0.902，兩個多語模型都在 0.95 以上。一個可能的解釋是多語模型的訓練把各種語言（包括繁簡兩種寫法）壓進同一個共享空間，繁簡反而被對映在一起；中文特化模型以簡體為主場訓練，繁體字對它是相對陌生的 Token。無論解釋為何，工程結論很實際：0.902 依然是高相似度，本系列語料全為繁體、查詢與文件一致，影響有限；但若知識庫日後混入簡體文件，模型選擇必須重測。

速度方面，三個模型編碼 17 個 Chunk 都在 0.1 秒上下，這個量級分不出高下——等語料上萬時再回來計較。

## 今天的選擇

淘汰先講：minilm 的 Hit@3 少一題（q07 連前三都進不了），且它本來就不是檢索特化，出局。剩下 bge 與 e5，十二題打成平手—— **評測集分不出它們的高下，這時候的選擇是工程判斷，不是實驗結論** ，這句話要誠實寫下來。

本系列選 `multilingual-e5-small` ，理由有三：檢索特化的訓練目標與我們的用途一致；多語言能力對中英混雜的技術文件是實質優勢（Day 11 的中英對照 0.95 就是它這一系模型的強項）；繁簡穩定度居中偏穩。 `bge-small-zh-v1.5` 以五分之一的參數量打平 e5，是資源受限時的優秀替代——它會被記在設定檔裡，README 承諾過的「模型透過設定檔抽換」從今天開始是實際需求，不是口號。語料擴充之後，這場選型賽要重跑。

還要替向量方法說一句公道話：11/12 輸給 BM25，不代表語意搜尋不行。向量的強項——口語改寫、同義詞、跨語言查詢——在十二題裡的代表性還不足，這是評測集要繼續成長的方向，不是方法的天花板。

## 結語

今天把三個候選模型放上了 Day 10 的秤：沒有向量模型打贏 BM25、語意搜尋摔在「語意搜尋是什麼」這一題、中文特化模型反而對繁簡最敏感——三個反直覺的結果，全部來自十四題的小評測集，這正是先建秤再選型的價值。最終 `multilingual-e5-small` 因檢索特化與多語能力入選， `bge-small-zh-v1.5` 列為輕量替代，選擇過程與限制都留了紀錄。

下一篇處理規模問題：17 個 Chunk 可以用一個矩陣乘法暴力算完，一萬個、十萬個 Chunk 呢？向量需要一個家——我們會認識向量資料庫 Qdrant，以及它的 Collection、Point 與 Payload 如何對應到這個系列一路堅持的 Metadata 與可追溯性。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10403272)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/f9d1c7c7-1e58-4501-ba69-50694296e234_small.png)

[Cloud Security — 雲端安全治理與發展策略](https://itplus.ithome.com.tw/vod-page/543?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/2eec9da7-363d-4529-b494-eafa785e4b28_small.png)

[SUSE 雲原生方案和開源創新](https://itplus.ithome.com.tw/vod-page/1323?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/0bb018fc-4843-48d9-aa8e-1d21f64af0ab_small.png)

[遍佈全球的卓越安全性和連線性](https://itplus.ithome.com.tw/vod-page/1207?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/7af9ca54-eb0e-4870-ae40-31e0a55df8c4_small.png)

[數位治理與智慧城市：臺中市的轉型與未來機遇](https://itplus.ithome.com.tw/vod-page/1242?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

31 分