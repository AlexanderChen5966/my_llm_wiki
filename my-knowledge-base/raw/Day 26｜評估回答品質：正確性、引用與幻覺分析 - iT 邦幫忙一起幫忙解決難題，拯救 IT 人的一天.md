---
title: "Day 26｜評估回答品質：正確性、引用與幻覺分析 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10406229"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "昨天的檢索成績看起來很漂亮：Reranked Retrieval 在 16 題上得到 Hit@3 與 Recall@3 的 1.000。可是一份正確文件進入 Context，只代表模型「有機會」答對，..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 26

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 26 篇

## Day 26｜評估回答品質：正確性、引用與幻覺分析

- 分享至

昨天的檢索成績看起來很漂亮：Reranked Retrieval 在 16 題上得到 Hit@3 與 Recall@3 的 1.000。可是一份正確文件進入 Context，只代表模型「有機會」答對，並不代表它會選對內容、保留條件、附對來源，甚至不代表它願意回答。

今天把評測往下游推一層，建立第一份回答評測集，檢查狀態、引用、來源精準度與參考要點，再用本機 `qwen3.5:9b` 實際跑完八題。這次衡量的不是模型說得像不像，而是答案能否回到證據。

## 回答評測需要不同標籤

搜尋評測只需要問題與相關文件；回答評測還要知道這題應回答或拒答，以及理想答案至少應包含哪些要點。 `answer_eval.yaml` 的每題包含：

```yaml
- id: a01
  query: HTTP 401 和 403 有什麼差別？
  expected_status: answered
  relevant_docs:
    - spring-security-authentication
    - spring-security-filter-chain
  reference_points:
    - [401, 身分未確認, 未認證]
    - [403, 權限不足, 沒有權限]
```

`reference_points` 每一列是一組可接受說法，只要回答包含其中一個詞就算覆蓋。這只是低成本的回歸檢查，不是完整語意判分；「不等同」和標籤裡的「不等於」意思相近，字串規則仍可能漏判。

八題中六題有答案，涵蓋 401/403、CSRF、Token、Chunk、Embedding 與斷詞；兩題無答案是 Kubernetes 與模型幻覺，預期狀態為 `insufficient` 。

## 四個自動指標

第一版計算：

- Status Accuracy： `answered` / `insufficient` 是否符合標籤。
- Citation Valid：回答是否通過 Day 21 的格式與封閉集合驗證。
- Citation Precision：被引用文件中，有多少屬於標註相關文件。
- Reference Coverage：參考要點有多少被字串規則命中。
```python
def grade_answer(answer, case):
    relevant = set(case["relevant_docs"])
    cited = {source.document_id for source in answer.sources}
    citation_precision = (
        len(cited & relevant) / len(cited) if cited else 0.0
    )
    # reference_points 逐組檢查是否有任一可接受詞出現在回答中
```

沒有直接叫另一個 LLM 當裁判。LLM-as-a-Judge 可以擴大評測，但裁判模型也有偏差、版本漂移與 Prompt 敏感性；在只有八題時，人工逐題審閱比再加一個黑盒子更誠實。

## 本機端到端結果

執行：

```bash
RAG_LLM_MODE=ollama RAG_MODEL=qwen3.5:9b \
HF_HUB_OFFLINE=1 python scripts/evaluate_answers.py
```

結果：

```markdown
status accuracy: 7/8
citation valid: 7/8
citation precision: 0.812
reference coverage: 0.812
```

六題有答案中，CSRF、Chunk、Embedding 與斷詞的主要內容都能由來源支持；兩題無答案也都在生成前被證據閘門拒絕。唯一 Status 失敗的是 a01「HTTP 401 和 403 有什麼差別？」——不是回答事實錯誤，而是 Day 20 看過的引用不一致：正文用了 `S1` 、 `S2` ， `citations` 只列 `S1` ，驗證器因此把它轉成 `insufficient` 。

這個結果說明 Fail Closed 的代價：系統犧牲一次原本可讀的答案，換取引用契約不被悄悄破壞。在產品中可加入一次有上限的格式修復，但修復前後都要留下 Trace；不能為了把 7/8 變成 8/8 就放寬驗證規則。

## 自動評分也會誤判

a03 的回答明確寫出：「Token 是語言模型處理文字的基本單位，但不等同於一個中文字或英文單字」，也提到不同 Tokenizer；人工閱讀是合理答案。但 `reference_points` 接受的是「不等於」或「不一定」，沒有「不等同」，所以 Coverage 只有 0.5。

這不是模型失敗，而是字串評分器失敗。它再次提醒我們：自動指標一定要保留逐題輸出。總分下降時先看內容，不能讓一個粗糙規則成為真理。

Citation Precision 0.812 也需要解讀。部分回答引用了正確主文件之外的補充文件，例如 Token 題同時引用斷詞文件、Chunk 題補充 Token 文件；這些來源並非完全無關，只是沒有被目前文件層級標籤列入。標籤不完整與模型過度引用，在數字上會長得一樣，需要人工判斷。

## 把答案拆成可驗證的主張

人工審閱時，先把答案拆成可驗證的主張（Claim）：

```markdown
Claim 1：401 代表身分尚未確認。
Claim 2：403 代表已認證但權限不足。
Claim 3：未通過 SecurityFilterChain 會回傳 401。
```

再逐一檢查引用的 Chunk 是否支持。只要一個 Claim 沒有來源支持，就屬於 Unsupported Claim；即使整段答案「大致正確」，也不能忽略。引用存在率不能代替 Claim-level Groundedness，這是後續擴充評測最重要的方向。

回答評測還要保存模型 ID、Prompt 版本、知識庫版本、檢索設定與執行時間。今天的數字只代表目前本機 `qwen3.5:9b` 、九份文件與這八題的快照，不能當成模型永久能力。

實際審閱時，再把每個 Claim 標記為 `supported` 、 `contradicted` 、 `not_in_source` 或 `not_verifiable` 。 `supported` 表示指定來源可直接支持； `contradicted` 是來源明確相反； `not_in_source` 則是模型加入來源沒寫的細節。最後一類用於價值判斷或無法從目前資料驗證的說法，不應硬塞成正確或錯誤。

```markdown
Claim：無狀態 API 一律可以停用 CSRF。
來源：只說「完全不依賴 Cookie 時通常可以停用」。
標記：not_in_source / 過度概括
```

這種錯誤不一定是整句憑空捏造，也可能是把條件句縮成絕對句、把「建議」改成「必須」、或把適用版本擴大。若只用關鍵字比對，CSRF、API、停用都命中，反而可能給高分；Claim 審閱才能看出限定條件被刪除。

完成逐條判定後，還要替幻覺分類。至少可以區分來源外新增、錯誤歸因、數字或版本錯誤、相互矛盾、過度概括與遺漏關鍵條件。不同類型有不同修法：來源外新增可能要強化 Prompt 或改模型，錯誤歸因可能是引用位置設計，版本錯誤可能需要改善 Metadata 與檢索過濾，遺漏條件則可能是 Chunk 邊界截斷。

引用合法但文件不相關，屬於 Citation Relevance 問題；文件相關但不支持具體句子，則是 Citation Support 問題。兩者不能只用 Citation Precision 代表。未來的人工表單應記錄 Claim、引用 Marker、來源摘錄、判定與原因，讓審閱者能回到同一證據，而不是只留下「感覺不太對」。

## 誰來判分：規則、人與 Judge 模型

人工也不是沒有偏差的黃金標準。審閱者可能憑自己的專業知識接受來源沒寫的內容，也可能因措辭不同而錯判。開始前要定義：只以知識庫為準還是允許常識、部分支持怎麼算、必要條件漏掉是否整個 Claim 失敗，以及拒答題能不能提供建議。

重要樣本可由兩位審閱者獨立判斷，再對分歧仲裁並記錄理由。這不只是學術上的一致性要求，也能反過來發現題目或來源模糊。Day 17 的 q18 就是典型例子：三個方法一致選到另一份合理文件，問題可能出在標籤，不一定出在模型。

資料量擴大後，可以讓 LLM-as-a-Judge 先做 Claim 拆分、找出可能的來源外陳述，降低人工閱讀量；但它的輸出應是待審線索，不是唯一真值。Judge Prompt、模型版本和溫度都要固定，並以一批人工標註集測量它的誤判方向。

尤其不能讓同一個模型既產生答案又替自己打分，然後把高分當成品質證明。即使使用另一個模型，也可能偏好較長、較流暢或與參考答案措辭相似的回答。對銀行、醫療或法務等高風險知識庫，關鍵案例仍需要領域人員確認。

## 一次回答要能被完整重播

要分析某個錯誤，不能只保存最後一句答案。最小的 Answer Run 應包含原問題、改寫查詢、候選與分數、Reranker 結果、Evidence Decision、Context Marker 對照、Prompt 版本、原始模型輸出、解析與驗證錯誤，以及最後公開結果。敏感內容可以遮罩或存受控環境，但關鍵決策不能只剩一個總分。

評測也應固定模型參數，並記錄是否使用量化版本、本機或遠端服務。生成具有隨機性時，同一題可重跑多次，觀察格式失敗與內容品質是否穩定。單次 7/8 只能說明這次執行結果；若十次中有三次漏列引用，才更接近產品會遇到的可靠性問題。

## 品質門檻應分層，而不是只看平均

上線條件可以要求：所有輸出都必須通過 Schema 與引用 Validity；無答案題不得出現未驗證回答；內容與覆蓋指標則依題型設定最低標準。格式與安全契約適合零容忍，語意品質則需要抽樣審閱和趨勢觀察。若把它們平均成單一 0.85，嚴重的引用失效可能被其他高分項目抵消。

最後要把「評分器錯誤」也列成正式結果。a03 的「不等同」案例不是雜訊，而是測量系統的缺陷。修正標籤或規則後應保留版本，避免新版分數看似提升，實際只是換了一把比較寬鬆的尺。

## 結語

今天建立八題回答評測，第一次把 Retrieval 之後的品質變成可觀察結果：狀態與引用都是 7/8，引用精準度與參考要點覆蓋都是 0.812。最有價值的仍是兩個失敗：引用欄位少列 `S2` ，系統因此安全拒絕；「不等同」被字串規則誤判，則揭露評分器本身也會犯錯。

下一篇不再把所有問題混成一個「回答錯了」，而是建立失敗分層：候選池沒找到、證據閘門錯殺、模型拒答、引用失敗、內容漏要點，各自要修不同元件。只有先定位失敗發生在哪一層，調整 Chunk、Top-K 或 Prompt 才不會變成盲目試參數。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10406131)

[

Day 25｜評估搜尋品質：Recall@K、MRR 與命中率

](https://ithelp.ithome.com.tw/articles/10406131)[下一篇](https://ithelp.ithome.com.tw/articles/10406516)

[

Day 27｜RAG 常見失敗案例：Chunk、Top-K 與 Prompt 的影響

](https://ithelp.ithome.com.tw/articles/10406516)

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