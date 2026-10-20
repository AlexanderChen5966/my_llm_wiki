---
title: "Day 11｜Embedding 是什麼？把文字轉成可以比較的向量 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10403272"
author:
  - "[[iThome]]"
published:
created: 2026-08-17
description: "第二階段在 Day 10 收官時，留下了關鍵字路線的最後死角：「登入驗證」永遠對不到「認證」，因為字面比對只認得一模一樣的詞。從今天開始的第三階段要處理這道牆——不再比對詞，而是把整段文字變成向量，讓..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 11

1

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 11 篇

## Day 11｜Embedding 是什麼？把文字轉成可以比較的向量

- 分享至

第二階段在 Day 10 收官時，留下了關鍵字路線的最後死角：「登入驗證」永遠對不到「認證」，因為字面比對只認得一模一樣的詞。從今天開始的第三階段要處理這道牆——不再比對詞，而是把整段文字變成向量，讓「語意相近」變成可以計算的數字。今天先把概念與度量弄清楚，並第一次實際算出「語意相似度」。

## 我們其實已經在用向量了

Embedding 常被介紹成一個全新的概念，但對這個系列來說，它其實是老朋友的升級。Day 8 建立 TF-IDF 索引時，每個 Chunk 都被表示成一組「詞 → 權重」的對應——那就是一個向量，維度等於詞彙表的大小（我們的語料是 585 維），每一維對應一個具體的詞，絕大多數維度是零。這種表示法叫稀疏向量，或者更口語的說法：詞袋（Bag of Words）。

稀疏向量的優點是每一維都可以解釋，Day 8 的 `contributions` 能把每一分的來源攤開，靠的就是這個性質。但它有一個殘酷的數學結論：兩個向量只有在共享字面詞的維度上，內積才不為零。「認證」和「登入」是兩個不同的維度，彼此正交——不管權重怎麼調，TF-IDF 和 BM25 都翻不過這道牆，因為牆就蓋在表示法本身。

## Embedding：讓維度不再是詞

Embedding 換掉的正是表示法。它把一段文字交給神經網路模型，輸出一個固定維度的稠密向量——今天示範用的模型輸出 384 維，每一維都有值，而且單獨看任何一維都沒有意義。意義不在維度上，而在空間結構裡：模型的訓練目標，就是讓語意相近的句子在這個空間中方向相近，無關的句子彼此遠離。

模型是怎麼學到這件事的？簡單說，是在大量文本上用「改寫句應該靠近、隨機句應該遠離」這類目標訓練出來的。訓練細節不是本系列的重點，重點是結果：「認證」和「登入」在訓練語料中總是出現在相似的上下文裡，模型因此把它們放在相近的方向上。字面不同、語意相近的文字，第一次有機會被算成「接近」。

## 同一把尺：Cosine Similarity

比較兩個向量最常用的尺是 Cosine Similarity——兩個向量夾角的餘弦值：

```markdown
cosine(a, b) = (a · b) / (|a| × |b|)
```

值域是 −1 到 1，方向相同為 1、正交為 0、方向相反為 −1。用夾角而不用距離的原因：向量的長度會受句子長短、詞頻高低影響，方向才承載「說了什麼」。實務上通常先把向量正規化成長度 1，此時 Cosine 就等於內積，計算也更便宜。

要注意的是，Cosine 這把尺不挑向量——它同樣可以量 Day 8 的稀疏向量。這正好給了我們一個公平的對照實驗：同一批句子、同一把尺，只換表示法，看看數字差在哪裡。

## 實際量一次

建立 `scripts/embedding_demo.py` 。示範模型使用 `paraphrase-multilingual-MiniLM-L12-v2` ——一個小型多語模型，在這裡純粹當黑盒子用，「繁體中文該選哪個模型」是 Day 12 的正題。安裝：

```bash
python -m pip install sentence-transformers
```
```python
import math
from collections import Counter
from pathlib import Path

from sentence_transformers import SentenceTransformer

from search_keyword import setup_dictionary, tokenize

MODEL_NAME = "paraphrase-multilingual-MiniLM-L12-v2"

SENTENCES = [
    "如何驗證使用者的登入密碼？",        # 0：登入用語
    "認證流程會確認帳號身分。",          # 1：認證用語（與 0 同義，字面不同）
    "Markdown 文件要先切成 Chunk。",     # 2：無關主題
    "先切分文件，再建立索引。",          # 3：順序 A
    "先建立索引，再切分文件。",          # 4：順序 B（與 3 邏輯相反）
    "How do I verify a user's login password?",  # 5：英文版的 0
]

PAIRS = [
    (0, 1, "同義改寫（登入 vs 認證）"),
    (0, 2, "無關主題"),
    (3, 4, "順序反轉"),
    (0, 5, "中文 vs 英文"),
]

def sparse_cosine(text_a: str, text_b: str) -> float:
    """以斷詞後的詞頻向量計算 Cosine，代表字面比對的視角。"""
    counts_a, counts_b = Counter(tokenize(text_a)), Counter(tokenize(text_b))
    dot = sum(counts_a[t] * counts_b[t] for t in counts_a.keys() & counts_b.keys())
    norm_a = math.sqrt(sum(v * v for v in counts_a.values()))
    norm_b = math.sqrt(sum(v * v for v in counts_b.values()))
    return dot / (norm_a * norm_b) if norm_a and norm_b else 0.0

if __name__ == "__main__":
    setup_dictionary(Path("dict/user_dict.txt"))

    print("=== 詞頻向量（字面視角）的 Cosine 相似度 ===")
    for i, j, label in PAIRS:
        print(f"{sparse_cosine(SENTENCES[i], SENTENCES[j]):.2f}  {label}")

    model = SentenceTransformer(MODEL_NAME)
    embeddings = model.encode(SENTENCES, normalize_embeddings=True)
    print(f"\n模型：{MODEL_NAME}")
    print(f"向量維度：{embeddings.shape[1]}，前 5 維：{[round(float(x), 3) for x in embeddings[0][:5]]}")

    print("\n=== Embedding 的 Cosine 相似度 ===")
    for i, j, label in PAIRS:
        similarity = float(embeddings[i] @ embeddings[j])
        print(f"{similarity:.2f}  {label}")
```

執行結果：

```markdown
=== 詞頻向量（字面視角）的 Cosine 相似度 ===
0.00  同義改寫（登入 vs 認證）
0.00  無關主題
1.00  順序反轉
0.00  中文 vs 英文

模型：paraphrase-multilingual-MiniLM-L12-v2
向量維度：384，前 5 維：[-0.043, 0.025, -0.052, -0.069, -0.07]

=== Embedding 的 Cosine 相似度 ===
0.58  同義改寫（登入 vs 認證）
-0.01  無關主題
0.98  順序反轉
0.95  中文 vs 英文
```

先看字面視角的殘酷之處：同義改寫和無關主題 **都是 0.00** 。「如何驗證使用者的登入密碼」和「認證流程會確認帳號身分」沒有共享任何一個詞，在詞頻向量的世界裡，它和「Markdown 文件要先切成 Chunk」一樣無關——字面牆不只擋住比對，它讓「同義」和「無關」完全無法區分。

Embedding 把這兩者拉開了：同義改寫 0.58，無關主題 −0.01。牆破了。更驚喜的是中英對照拿到 0.95——同一個問題的中文版和英文版，在向量空間裡幾乎是同一個方向。對中英混雜的技術文件來說，這是關鍵字方法想都不敢想的能力。

但誠實的對照表也要看第三行：順序反轉,詞頻向量給 1.00（詞袋根本不看順序,兩句的詞一模一樣）,Embedding 給 0.98—— **幾乎同樣分不出來** 。「先切分文件再建立索引」和「先建立索引再切分文件」是兩個相反的操作順序,兩種表示法都把它們當成幾乎相同的句子。語意相似不等於邏輯等價,否定、順序、數字這類差異,Embedding 經常視而不見。這個盲點之後在評測與引用階段都會回來找我們。

還有一個數字要先打預防針：0.58 是什麼意思？沒有絕對意義。它不是機率、不是百分比，換一個模型同一對句子可能變成 0.75 或 0.42——相似度分數只在同一個模型內比較時有意義。Day 10 說過「搜尋分數不是機率、不同查詢之間不可比」，這句話在向量世界不但成立，還要再加一句：不同模型之間也不可比。拒答閾值的問題，到了語意搜尋依然沒有免費的答案。

## 這對 RAG 意味著什麼

把今天的機制放進檢索流程，就是 Day 14 要做的語意搜尋：把每個 Chunk 事先算成向量，查詢進來時也算成向量，取 Cosine 最高的 Top-K。但這條路線有代價，而且每一項都是接下來幾天的主題：向量要選對模型（繁體中文的支援程度差異很大，Day 12）；17 個 Chunk 可以暴力算完，一萬個就需要專門的儲存與索引（Day 13 的 Qdrant）；還有一個常被忽略的代價——可解釋性。Day 8 的 `contributions` 能把每一分攤開給你看，Embedding 的 384 維沒有這種帳本，「為什麼這個 Chunk 排第一」變得難以回答。檢索越黑盒，Day 10 那座秤就越重要。

也要先說清楚：Embedding 不是來取代 BM25 的。Day 10 的 q12 靠「登入／失敗」的字面就能答對， `SecurityFilterChain` 這種識別字更是關鍵字的主場——字面精準與語意寬容各有勝場，這是 Day 16 Hybrid Search 的伏筆。

## 結語

今天把「語意相近」變成了可以計算的數字：從 Day 8 就在用的稀疏向量出發，理解字面牆是表示法的數學性質；換上稠密的 Embedding 之後，同義改寫從 0.00 變成 0.58、與無關主題正式分離，中英對照更拿到 0.95。同時記下兩個誠實的觀察：順序反轉兩種表示法都看不見（1.00 與 0.98），相似度分數沒有絕對意義。

下一篇處理一個實際的選擇題：繁體中文的 Embedding 模型怎麼挑？我們會比較幾個候選模型的大小、語言支援與速度，最重要的是——把它們放上 Day 10 的評測集，讓評測集來決定誰上場。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10403167)

[

Day 10｜建立第一個搜尋評測集：如何判斷搜尋結果好不好？

](https://ithelp.ithome.com.tw/articles/10403167)[下一篇](https://ithelp.ithome.com.tw/articles/10403356)

[

Day 12｜繁體中文 Embedding 模型怎麼選？

](https://ithelp.ithome.com.tw/articles/10403356)

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