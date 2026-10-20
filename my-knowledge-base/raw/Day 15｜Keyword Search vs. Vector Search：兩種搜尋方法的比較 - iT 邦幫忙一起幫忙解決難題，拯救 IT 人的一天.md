---
title: "Day 15｜Keyword Search vs. Vector Search：兩種搜尋方法的比較 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10403784"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "上一篇結尾承諾了正面對決，但直接開打有個問題：BM25 在評測集 v1 已經是 12/12 滿分，比賽還沒開始就結束了。而 Day 14 自己承認過，v1 的十二題以術語型問題為主，那是關鍵字的主場—..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 15

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 15 篇

## Day 15｜Keyword Search vs. Vector Search：兩種搜尋方法的比較

- 分享至

上一篇結尾承諾了正面對決，但直接開打有個問題：BM25 在評測集 v1 已經是 12/12 滿分，比賽還沒開始就結束了。而 Day 14 自己承認過，v1 的十二題以術語型問題為主，那是關鍵字的主場——拿一份偏袒某一方的考卷宣布勝負，沒有資訊量。所以今天的第一件事不是比賽，是把秤校準。

## 先把秤校準：評測集 v2

評測集升級到 v2：新增四題，涵蓋 v1 缺席的三種題型——跨語言（q15，英文問句）、口語改寫（q16「切成小段」、q17「確認發出請求的人是誰」），以及同義詞困境（q18「為什麼我用不同的講法搜尋，就找不到同一份文件？」）。前三題來自 Day 14 的主場秀，第四題是全新題。現在共 16 題有答案、2 題無答案。

「把 semantic 的展示題轉正，這對 BM25 公平嗎？」——這個質疑值得正面回答。評測集的職責是代表真實使用者，而真實使用者就是會打英文、會用白話、會換個講法再問一次；v1 缺這些題型，對 semantic 的偏頗並不比現在對 BM25 的偏頗更正當。Day 10 說評測集是活資產、要持續校準，這就是校準。同時照舊誠實記錄糾纏：q15 到 q17 是昨天已經看過 semantic 表現的題目，帶有洩題性質；q18 是兩個方法都沒見過的全新題——記住這個區別，等一下它會變得重要。

## 逐題對決

`scripts/compare_methods.py` 對每一題取兩個方法的完整排名，記錄「第一個正解文件出現的名次」——比 Hit@K 的二元判定更能看出差距的形狀：

```python
def rank_of_first_relevant(results, relevant_docs: list[str]) -> int | None:
    for rank, result in enumerate(results, start=1):
        if result.chunk.document_id in relevant_docs:
            return rank
    return None
```
```markdown
逐題對決（首個正解的名次；–＝整份排名中都沒有）
題號     BM25  semantic   題目
q01       1         1   什麼是 CSRF 攻擊？
q02       1         1   Token 和一般認知的字數有什麼不同？
q03       1         1   Embedding 把文字變成什麼？
q04       1         1   如何設定密碼雜湊？
q05       1         1   想停用 CSRF 保護，該怎麼設定？
q06       1         1   怎麼避免專有名詞在斷詞時被切碎？
q07       1         1   關鍵字搜尋和語意搜尋有什麼差別？
q08       1         1   HTTP 401 和 403 差在哪裡？
q09       1         1   固定長度切分和依段落切分怎麼選？
q10       1         1   HTTP 401 是什麼意思？
q11       1         1   搜尋常常找不到含有專有名詞的文件，可能的原因是什麼？
q12       1         3   登入一直失敗，該從哪裡開始查？
q15       –         1   How does the system verify who I am? *
q16       6         1   文章太長了，想切成小段之後再搜尋，該怎麼做？ *
q17       2         1   系統怎麼確認發出請求的人是誰？ *
q18       5         6   為什麼我用不同的講法搜尋，就找不到同一份文件？ *
（* 為 v2 新增題）
```

表格的形狀比任何總分都會說話。v1 區（q01–q12）是一片 1，唯一的例外是 semantic 在 q12 排第 3；v2 區整個反轉——BM25 對英文題交白卷（整份排名裡連一個相關 Chunk 都沒有，因為沒有任何詞對得上），口語題掉到第 6，改寫題第 2；semantic 前三題全部第 1。主場的結構就這麼直白：查詢與文件共享術語時，關鍵字幾乎不會錯；術語對不上時，關鍵字直接失明。

## 總成績、聯手上限與代價

```markdown
方法              Hit@1    Hit@3
bm25            12/16    13/16
semantic        14/16    15/16
oracle 聯集       15/16    15/16

BM25 的 Hit@1 失敗題：['q15', 'q16', 'q17', 'q18']
semantic 的 Hit@1 失敗題：['q12', 'q18']
兩者共同失敗：['q18']

平均單次查詢：BM25 0.2 ms、semantic 31.0 ms
```

考卷校準後，semantic 以 14/16 首度超過 BM25 的 12/16——注意這不是方法變強了，是考卷變完整了。真正的重點在 oracle 那一行：「聯集」是假想的完美裁判，任何一個方法答對就算對。它不是可部署的方法，但它回答一個關鍵問題——融合兩種方法，天花板在哪裡？答案是 Hit@1 15/16：比 BM25 高 3 題、比 semantic 高 1 題。兩個方法的失敗集合幾乎不相交（BM25 輸的四題 semantic 拿回三題，semantic 輸的 q12 BM25 是滿分），這就是 Day 16 Hybrid Search 的立案依據——不是信仰，是這張表。

另外兩個數字也值得停留。其一，Hit@3 這一列 oracle 與 semantic 同為 15/16，也就是說在「前三名找得到」這個標準下，融合對 semantic 沒有增益——融合的油水集中在 Top-1 精度。對 RAG 而言 Top-1 恰好很重要：上下文位置有限，排最前面的 Chunk 對答案的影響最大。其二，速度差了 150 倍（0.2 ms 對 31 ms）——semantic 的成本花在查詢端的向量編碼，這在離線建庫時可以預先付清，查詢時卻躲不掉。工程上這決定了兩者在高流量場景的角色分配。

## q18 的解剖：一題暴露三件事

唯一連 oracle 都救不了的 q18，值得單獨解剖。把 semantic 的前六名攤開：

```markdown
rag-chunking#000                         0.862
nlp-word-segmentation#001                0.862
nlp-word-segmentation#000                0.850
nlp-text-cleaning#000                    0.848
rag-retrieval-augmented-generation#000   0.845
rag-embedding#000                        0.845
```

第一件事：前六名的分數從 0.845 到 0.862，寬度 0.017——Day 13 那個 0.004 的故事又來了。在這種密度下，名次幾乎是雜訊排出來的，模型只知道「這些都在講搜尋找不到東西」，分不出誰才是答案。這種「粗篩有效、細排無力」的局面，正是 Day 17 Reranker 的存在理由。

第二件事：兩個方法都把斷詞文件排在最前面，而它其實是個 **合理但未被標註** 的答案——中文裡「換個講法」會切出不同的詞，斷詞差異本來就是「不同講法找不到同一份文件」的正當原因之一。標準答案只標了 Embedding 文件，這是標註的模糊地帶，Day 10 警告過的「出題者＝標註者」偏差真實上演。

第三件事是方法論的：發現這個模糊之後，最誘人的動作是回頭把標籤改寬（把斷詞文件也列為正解），數字立刻變漂亮。我們不這麼做——看到成績之後才調整標籤，是在讓秤遷就成績，這和調參過擬合是同一種病。正確的處置是把爭議記錄在評測集的註記裡，等語料與題目擴充後由第二位標註者裁決。q18 維持原判，兩個方法都繼續掛零。

## 這場對決真正的結論

「關鍵字和向量哪個好」是個錯誤的問題，這張逐題表給出的是主場歸屬：術語、識別字、錯誤代碼，關鍵字幾乎不敗；口語、同義改寫、跨語言，向量壓倒性勝出；而兩邊都弱的題型（q18 這種「主題對了但細節分不出」的問題）依然存在，誰也不是萬靈丹。

於是 Day 16 的任務清楚了：讓兩個方法聯手，往 15/16 的天花板靠近。但有一個技術難題擋在門口——BM25 給 q18 第一名打 6.51 分，semantic 打 0.862 分， **兩種分數是不同的單位** ，直接相加毫無意義，正規化也只是把蘋果榨成看起來像橘子汁。下一篇會用一個優雅的辦法繞開這個問題：不融合分數，融合名次。

## 結語

今天把評測集校準到 v2，然後讓兩種搜尋方法在同一張考卷上逐題對決：semantic 以 14/16 領先 BM25 的 12/16，oracle 聯集指出融合的天花板是 15/16，而失敗集合幾乎不相交證明了融合有實質油水。q18 的解剖則一次暴露三件事——語意分數的壓縮讓細排形同雜訊、標註存在模糊地帶、以及看到成績後不改標籤的紀律。

下一篇實作 Hybrid Search：用名次融合讓關鍵字與語意各自發揮主場優勢，目標很具體——把 Hit@1 從 14/16 推向 15/16。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10403702)

[

Day 14｜實作 Semantic Search：用語意而不是關鍵字找資料

](https://ithelp.ithome.com.tw/articles/10403702)[下一篇](https://ithelp.ithome.com.tw/articles/10404056)

[

Day 16｜Hybrid Search：結合關鍵字與語意搜尋

](https://ithelp.ithome.com.tw/articles/10404056)

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

[![.](https://itadstatic.ithome.com.tw/B9/1787886940_6a90fd5c11c52.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/c6641cd4-2da2-434a-9150-928cbd81330e_small.png)

[Synology C2 從 VM 轉型到 Kubernetes 的心得分享](https://itplus.ithome.com.tw/vod-page/565?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/27/abb05e0d-0c8c-4252-ab8d-f0f67444cb32.png)

[BizDevOps 概論](https://itplus.ithome.com.tw/vod-page/789?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/21/7acc6345-0bec-4ca3-86b8-4964c5a0e7dc_small.png)

[哇嗚，原來 Vue.js 還能這樣寫！](https://itplus.ithome.com.tw/vod-page/754?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

39 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/d92205d9-3c20-4ef7-891c-ef23d4dd56be_small.png)

[DevOps 再進化：導入 AI Agent 提升組織 20% 效率，4 個導入案例分析](https://itplus.ithome.com.tw/vod-page/1197?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

30 分