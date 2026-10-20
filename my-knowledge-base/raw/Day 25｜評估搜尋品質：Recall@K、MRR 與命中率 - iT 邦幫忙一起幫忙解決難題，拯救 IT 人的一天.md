---
title: "Day 25｜評估搜尋品質：Recall@K、MRR 與命中率 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10406131"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "Day 10 建立第一份評測集時，我們只問「前一名或前三名有沒有命中？」這足以比較早期方法，卻無法看出兩件事：需要多份來源時，系統究竟找回多少？第一份正解又排在多前面？ 今天替檢索儀表板補上 Reca..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 25

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 25 篇

## Day 25｜評估搜尋品質：Recall@K、MRR 與命中率

- 分享至

Day 10 建立第一份評測集時，我們只問「前一名或前三名有沒有命中？」這足以比較早期方法，卻無法看出兩件事：需要多份來源時，系統究竟找回多少？第一份正解又排在多前面？

今天替檢索儀表板補上 Recall@K 與平均倒數排名（Mean Reciprocal Rank，MRR），再用同一份評測集重算 BM25、Semantic、Hybrid 與 Reranked 四條管線。等三個指標各自出場後，再把它們放進同一個例子，後面的成績表就不會只剩小數點。

## 三個指標，回答三個不同問題

### Hit@K：至少找到一份嗎？

Hit@K 是最直觀的指標：前 K 名中只要出現任一相關文件，這題就是 1，否則是 0。它適合 RAG 的第一道健康檢查，因為 Context 裡至少要有一份可用證據。

```markdown
Hit@K = 前 K 名是否至少含一份相關文件
```

但 q10「HTTP 401 是什麼意思？」有兩份標準文件。如果 Top-3 只找到其中一份，Hit@3 已經是 1，看不出另一份是否遺失。這時要看 Recall。

### Recall@K：應找的資料找回多少？

```markdown
Recall@K = 前 K 名中的相關文件數 / 這題所有相關文件數
```

標準答案有兩份、只取回一份，Recall@3 是 0.5。大多數題只有一份標準文件時，Recall@K 會和 Hit@K 一樣；它的價值會隨多文件問題增加而變大。未來比較型問題、跨文件摘要與版本差異，都不應只有單一標準來源。

### MRR：第一份正解有多前面？

Reciprocal Rank 是第一份相關文件名次的倒數：第 1 名得 1，第 2 名得 0.5，第 5 名得 0.2，整份排名都沒有則是 0。所有題平均後得到 MRR。

```markdown
RR = 1 / 第一份相關文件的名次
MRR = 所有題 RR 的平均
```

MRR 特別適合目前的問答場景：雖然 RAG 可以帶多個 Chunk，但越前面的來源越可能進入有限 Context，也越容易影響模型回答。

## 用一題把三個指標串起來

假設某題標註兩份相關文件 `A` 、 `B` ，系統前五名是 `[C, A, D, B, E]` 。在 K=3 時，Hit@3 是 1，因為至少找到 `A` ；Recall@3 是 1/2，也就是 0.5；第一份相關文件排第 2，所以 RR 是 1/2。若把 K 放到 5，Recall 會升為 1，但 RR 不變，因為第一份正解的位置沒有改變。

```markdown
排名： C  A  D  B  E
名次： 1  2  3  4  5
相關：    ✓     ✓
```

三個數字回答的是不同問題：Hit 看「能不能開始回答」，Recall 看「該找的是否找齊」，MRR 則看「第一份證據是否夠前面」。如果產品要求答案同時比較兩份規範，Recall 會比 Hit 更重要；若介面只顯示第一筆結果，MRR 或 Hit@1 更貼近使用情境。

## 先在文件層級去重

搜尋器排序的是 Chunk，同一文件可能包辦前兩名。若直接按 Chunk 算，第二個文件會被擠到第 3 名，看起來像排名較差；但評測標籤從 Day 10 起就是文件層級。因此指標計算前先保留每份文件第一次出現的位置：

```python
def ranked_document_ids(results):
    seen, ranked = set(), []
    for result in results:
        document_id = result.chunk.document_id
        if document_id not in seen:
            ranked.append(document_id)
            seen.add(document_id)
    return ranked
```

這項處理必須和標註粒度一致。若未來評測改成精確段落支持度，就要另外建立 Chunk 或 Claim 層級標籤，不能繼續用文件去重掩蓋錯段落。

## 實測結果

在 16 題有答案的 v2 評測集上執行：

```bash
HF_HUB_OFFLINE=1 python scripts/evaluate_retrieval_metrics.py
```

得到：

| 方法 | Hit@1 | Hit@3 | Recall@3 | MRR |
| --- | --- | --- | --- | --- |
| BM25 | 0.750 | 0.812 | 0.812 | 0.809 |
| Semantic | 0.875 | 0.938 | 0.938 | 0.908 |
| Hybrid | 0.875 | 0.938 | 0.938 | 0.922 |
| Reranked | 0.938 | 1.000 | 1.000 | 0.969 |

Reranked 的 Hit@3 與 Recall@3 都是 1.000，表示每題的所有標註文件都在前三個「不同文件」中被找回；MRR 0.969 則表示第一份正解幾乎總在最前面。它仍不是 Hit@1 滿分，原因是 q18 的標註爭議：斷詞文件排在被標為標準答案的 Embedding 文件前面。

Hybrid 與 Semantic 的 Hit@1、Hit@3 相同，MRR 卻是 0.922 對 0.908。這正是 MRR 多提供的資訊：兩者命中的題數相同，Hybrid 把部分正解推得更前面，平均排名較好；只看 Hit@K 會把差異壓掉。

## 指標不會自動告訴你為什麼

表格能發現回歸，不能代替失敗分析。Reranked 的 0.938 到底是模型不足、候選池遺失，還是標籤有問題，必須回到逐題 Trace。Day 17 已經看到三種方法都偏向 q18 的斷詞文件，這個一致性支持「標註模糊」的判斷，但在第二位標註者仲裁前仍保留原標籤。

評測集也很小。16 題裡答錯一題，Hit@1 就變動 0.0625；0.938 和 1.000 的差距沒有統計穩定性。這份表適合做專案內的回歸基準，不適合宣稱某方法普遍優於另一方法。

## 平均分之外，還要看題型切片

目前表格對每題先算指標再平均，也就是每題權重相同的總體平均（Macro Average）。若直接把所有相關文件混在一起計算，相關文件較多的題目會占更大權重，回答的是另一個問題。兩種方法都能使用，但必須把公式與聚合方式寫清楚，否則同樣標示 Recall@3 的兩份報告可能無法比較。

總平均還會隱藏題型差異。評測資料可加上 `category` ，分別觀察錯誤碼、概念解釋、跨文件比較、操作步驟與無答案題。若 Reranker 在概念題很好、精確識別字卻退步，總分可能仍上升；使用者若最常查錯誤碼，這個版本就不一定值得上線。

同樣地，可以切分短查詢、口語查詢、問題改寫後查詢與多輪 Follow-up。切片不是為了產生更多漂亮表格，而是把失敗連回可調整元件。只有看到某一類穩定退步，才有理由檢查斷詞、Rewrite 或候選池。

## K 的選擇也是系統設計

K 越大，Recall 通常只會上升，但這不代表 Context 應無限加入更多 Chunk。檢索評測的 K 是觀察窗口，生成階段的 Top-K 則會影響 Token、延遲與雜訊。可以測 Hit@1、Hit@3、Hit@5 的曲線：若從 3 增到 10 只多找回極少相關文件，卻讓 Reranker 與 LLM 成本大增，就沒有部署價值。

此外，Reranker 能重新排列候選，不能找回第一階段沒召回的文件。因此應同時記錄候選池 Recall 與最終 Top-K Recall。候選池已遺失時要改 BM25、Embedding 或融合；候選池有正解但最後排名錯，才輪到 Reranker。把兩階段混成一個分數，容易在錯的地方調參數。

## 小樣本如何避免過度解讀？

16 題適合教學與回歸，但每錯一題就移動 6.25 個百分點。除了直接列出分子與分母，還可以用 Bootstrap 估計區間，或至少保存逐題結果比較兩版本是否錯在同一題。若 A 與 B 都是 15/16，但失敗題不同，它們的風險輪廓並不相同。

更重要的是建立獨立的新增流程。線上失敗案例加入評測集後，不能再拿同一題無限調參並把它當未見測試；可將它放入回歸集，另外保留一批從未參與調整的 Holdout。當資料仍少時，誠實呈現樣本數與案例，比附上一個看似精密的小數點更重要。

## 評測結果必須可以重現

每次執行應保存資料集版本、文件雜湊、Chunk 設定、詞典、Embedding 與 Reranker 模型 ID、候選池大小、融合參數與隨機種子。若只留下最終 CSV，三週後看到 MRR 從 0.969 變成 0.930，可能根本不知道是程式退步、文件更新還是模型快取不同。

評測腳本也應在發現重複題號、缺少相關文件或標註指向不存在的 `document_id` 時直接失敗。測量工具本身若默默接受壞資料，再精準的公式也只是在量錯的東西。

## 什麼時候要重跑這套評測？

每次變更 Chunk 大小、斷詞詞典、Embedding 模型、RRF 邏輯、Reranker 或候選池大小，都應重跑同一份評測。若結果下降，先檢查失敗題，而不是立刻調權重把總分補回來。新使用者問題若在線上失敗，也要匿名化後加入評測集，讓一次事故變成永久回歸案例。

品質之外，也要保存成本。Reranked 分數最好，計算成本也最高；Day 17 的 CPU 實驗已顯示它比 BM25 慢許多。完整比較應同時記錄 P50/P95 延遲、記憶體與索引建置時間，避免一張準確率表替產品做完所有決定。

## 結語

今天把搜尋評測從「有沒有命中」擴充到覆蓋與排名：Hit@K 看是否至少找到一份，Recall@K 看相關文件找回多少，MRR 看第一份正解排在多前面。文件層級去重後，Reranked 得到 Hit@3 1.000、Recall@3 1.000、MRR 0.969，保留了 Day 17 的優勢，也讓 Hybrid 與 Semantic 在命中率之外的排名差異浮現。

檢索找對資料只是回答品質的上游條件。下一篇要建立另一座秤：回答狀態是否正確、引用是否有效、引用文件是否相關、關鍵參考要點是否涵蓋，並用本機模型跑出第一份端到端成績。那份成績不會是滿分，而失敗本身會告訴我們驗證器與自動評分各自的限制。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10405955)

[

Day 24｜多輪對話的 RAG：如何記住前面的問題？

](https://ithelp.ithome.com.tw/articles/10405955)[下一篇](https://ithelp.ithome.com.tw/articles/10406229)

[

Day 26｜評估回答品質：正確性、引用與幻覺分析

](https://ithelp.ithome.com.tw/articles/10406229)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/11/254a0234-400f-4b72-9159-6b2a4d66c2ea_small.png)

[2021 Q4 - Progress MFT 安全檔案傳輸管理軟體 - MOVEit Automation 培訓課程 (2)](https://itplus.ithome.com.tw/vod-page/510?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

EC NETWORKER |

113 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/21/2cc4afe4-e117-4311-85f9-42961aa95bc5_small.png)

[Three.js實現ROS虛實環境監控系統](https://itplus.ithome.com.tw/vod-page/757?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/10/14/92ffb6f1-e85f-4c4e-8d38-7bb1a646f783_small.png)

[【DevOps入門班】DevOps 實務中不可或缺的產品思維](https://itplus.ithome.com.tw/vod-page/970?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

42 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/20/cfd207ee-1d8f-4fc6-99c4-8eefec2553f3_small.png)

[AI 驅動的敏捷產品開發之旅 - 從靈感到交付](https://itplus.ithome.com.tw/vod-page/1379?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

39 分