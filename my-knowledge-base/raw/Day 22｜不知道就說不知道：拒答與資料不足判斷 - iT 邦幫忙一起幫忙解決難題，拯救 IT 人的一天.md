---
title: "Day 22｜不知道就說不知道：拒答與資料不足判斷 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10405359"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "「不知道就說不知道」是這個系列從 Day 2 就留下的承諾，直到今天才真正有條件實作。一路走來，BM25 的有答案與無答案分數混在一起，Semantic Search 又把分數擠進狹窄區間；Day 1..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 22

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 22 篇

## Day 22｜不知道就說不知道：拒答與資料不足判斷

- 分享至

「不知道就說不知道」是這個系列從 Day 2 就留下的承諾，直到今天才真正有條件實作。一路走來，BM25 的有答案與無答案分數混在一起，Semantic Search 又把分數擠進狹窄區間；Day 17 的 Cross-encoder 終於拉開兩者，卻只測了兩題無答案題。拿兩個樣本決定門檻，像看兩次晴天就宣布不會下雨。

所以今天先不追求一個漂亮的「最佳門檻」。我們會建立三層拒答流程，把無答案題從 2 題擴充到 10 題，再看看第一層究竟能守住多少。

## 拒答不是模型的一句自我聲明

先看完整的三層防線：

```markdown
檢索證據閘門 → LLM insufficient 狀態 → 引用與格式驗證
```

第一層在呼叫 LLM 前檢查精排候選。沒有結果，或 Top-1 Cross-encoder 分數低於暫定門檻，就直接拒答，省下生成成本。第二層讓模型實際閱讀 Context 後判斷內容是否足夠；即使檢索分數過關，來源沒有回答問題時，模型仍應回覆 `insufficient` 。第三層則沿用 Day 21：沒有合法引用的回答不能通過。

三層各自處理不同失敗：檢索閘門擋下明顯離題，模型判斷處理「主題相近但沒有答案」，引用驗證則阻止沒有合法證據的輸出流出。它們是接力，不是三次重複打分。

## 第一層的實作

```python
@dataclass(frozen=True)
class EvidenceDecision:
    answerable: bool
    reason: str
    top_score: float | None

def decide_evidence(results, min_reranker_score=0.10):
    if not results:
        return EvidenceDecision(False, "no_retrieval_result", None)

    top_score = float(results[0].score)
    if top_score < min_reranker_score:
        return EvidenceDecision(
            False,
            "reranker_score_below_threshold",
            top_score,
        )
    return EvidenceDecision(True, "candidate_evidence_found", top_score)
```

`0.10` 不是普遍真理，只是 Day 17 觀察到「16 題有答案最低約 0.16、兩題無答案最高約 0.01」後設定的實驗值。程式把它做成參數，也保留理由與原始分數，避免日後只看到一個 `True/False` 卻不知道怎麼決定。

## 將無答案題擴充到十題

新的拒答評測同時包含遠域與近域問題：Kubernetes、Docker Compose、PDF OCR、SQL Injection 是明顯超出範圍；FastAPI、OAuth2 Client、Qdrant 分散式叢集則和目前技術主題相近，卻沒有被九份知識庫文件涵蓋。

```yaml
- id: n03
  query: FastAPI 的 Depends 要怎麼使用？
- id: n05
  query: Spring Security OAuth2 Client 要怎麼設定？
- id: n10
  query: Qdrant 如何建立分散式叢集？
```

評測必須以「知識庫有沒有寫」為準，不能因為系列文章或模型本身知道 FastAPI，就把它視為可回答。

## 實測結果：9/10，而不是滿分

在 16 題有答案題與 10 題無答案題上執行：

```bash
HF_HUB_OFFLINE=1 python scripts/evaluate_refusal.py
```

結果如下：

```markdown
有答案保留率：16/16
無答案拒答率：9/10
```

唯一漏網的是 FastAPI：

```markdown
n03 score=0.1441 decision=answer  FastAPI 的 Depends 要怎麼使用？
```

Reranker 把它和知識庫中的技術內容判得足夠相關，0.144 甚至接近有答案題最低分。這個反例非常重要：Cross-encoder 比 Bi-encoder 更能判斷「是否在回答」，但它仍不是知識庫可回答性的機率。它看到程式、設定與技術問句的相似形狀，就可能給出偏高分數。

最誘人的修正是把門檻從 0.10 調到 0.15，十題立刻全拒。但有答案題的最低分只有約 0.16，中間只剩極薄的縫；用同一批 26 題調到剛好滿分，是過度擬合，不是可靠策略。因此保留 0.10 與 9/10，讓第二層模型判斷處理 FastAPI 這種近域無答案題。

## 拒答也要有一致輸出

第一層拒答時不呼叫 LLM，回覆固定文字：

```markdown
目前知識庫沒有足夠資料可以回答這個問題。
```

同時保存 `decision_reason` ，讓 API 或 Log 區分 `no_retrieval_result` 、 `reranker_score_below_threshold` 與後續的模型拒答。使用者不需要看到內部分數，但維運人員需要知道是哪一層做出決定。

第二層模型拒答則必須輸出 `status=insufficient` 且不帶引用；如果它一面說資料不足、一面附上 `S1` ，Day 21 的驗證器會拒絕。這使「拒答」也遵守資料契約，而不是任意一句禮貌回覆。

## 兩類錯誤都要衡量

拒答評測不能只看擋下多少無答案題。門檻調得很高，可以得到漂亮的拒答率，卻會把真正有答案的問題一起拒絕。因此今天同時回報：

- 有答案保留率：系統沒有錯殺的比例。
- 無答案拒答率：系統正確擋下的比例。

目前是 16/16 與 9/10。這比一個合併後的「準確率」更容易看出風險方向。知識庫擴充後，兩個數字都必須重跑，門檻也不能沿用成永遠不變的常數。

## 門檻要校準，也要看錯誤代價

如果拿今天的 26 題逐一嘗試門檻，再選出表現最好的數字，等於先看過考題才決定規則。正式做法至少要把資料分成校準集與測試集：前者用來選門檻，後者只在決策確定後驗收。資料量再增加時，還應依問題類型分層抽樣，避免測試集剛好全是容易辨識的遠域問題。

對每個候選門檻，可以整理成混淆矩陣：

```markdown
系統回答    系統拒答
知識庫有答案          保留成功    錯誤拒答
知識庫無答案          漏網回答    正確拒答
```

錯誤拒答和漏網回答的代價並不相同。內部技術助理若答錯會造成操作風險，可以偏向保守；查詢入口若頻繁錯拒，使用者則可能直接放棄系統。門檻選擇因此是產品風險決策，不是單純把 Accuracy 調到最大。今天保留兩個方向的原始計數，正是為了讓這個取捨可見。

此外，遠域題與近域題必須分開觀察。「明天天氣如何」和「FastAPI 的 Depends 怎麼用」對目前知識庫都沒有答案，難度卻完全不同。遠域題通常在詞彙與語意上就明顯離開知識庫，第一層分數閘門容易擋下；近域題會共享 Python、API、依賴注入等技術語彙，檢索器可能找到看似合理的片段。

因此評測報告除了整體拒答率，還應按 `near_domain` 、 `far_domain` 或產品自訂類別切片。若整體是 90%，但近域題只有一半被擋下，使用者實際遇到的風險會被平均值掩蓋。FastAPI 反例也提醒我們：新增文件後，原本的無答案題可能變成有答案題，標籤與索引版本必須一起更新。

FastAPI 穿過第一層後，第二層也不能只是再看一次分數。模型要回答的是「提供的 Context 是否明確包含問題所需資訊」，而不是「這些文字和問題是否相關」。Prompt 可以要求它先辨認回答所需的核心要點，再檢查來源是否具備；缺少任一關鍵條件就回傳 `insufficient` 。這個判斷仍可能出錯，所以後面還有引用驗證與回答評測，模型不是最終裁判。

系統也不應把模型原有知識混進這一步。即使模型熟悉 FastAPI，只要 Context 沒有說明 `Depends` ，就必須拒答。這條規則是 Closed-book RAG 的核心：我們評估的是「根據目前知識庫能否回答」，而不是「模型曾在訓練資料看過多少」。

拒答訊息本身也應幫助使用者往下走。固定文字能確保一致，但產品介面不必只留下死路。系統可以安全地建議縮小問題、補充錯誤訊息，或指出目前知識庫涵蓋的主題範圍；若允許文件上傳，也可以引導使用者新增待審核資料。資料不足時若反而顯示一串低相關來源，只會讓使用者誤以為「雖然沒答案，但這些大概有用」。

維運端則要記錄查詢、索引版本、Top-1 分數、決策層級與理由，但避免把機密原文直接寫入一般 Log。定期查看分數分布和拒答原因，可以發現知識庫漂移：例如某次重新建索引後， `reranker_score_below_threshold` 突然大量上升，就應先檢查模型或資料版本，而不是立刻降低門檻。

## 結語

今天實作了檢索證據、模型狀態與引用驗證三層拒答流程，並把無答案評測擴充到十題。第一層門檻保留全部 16 題有答案題，也擋下 9/10 無答案題；漏網的 FastAPI 題拿到 0.144，清楚證明「近域但未收錄」仍會穿過分數閘門，也阻止我們為了滿分把門檻硬調到 0.15。

下一篇處理使用者問題本身。真實問題不一定像評測集一樣完整，有時是口語、有時只有錯誤碼，也可能用文件沒有的說法。問題改寫可以改善搜尋，但它也可能改變原意；我們會保留原問題、保護識別字，並為改寫失敗準備回退路徑。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10405135)

[

Day 21｜讓答案附上來源：RAG 引用機制設計

](https://ithelp.ithome.com.tw/articles/10405135)[下一篇](https://ithelp.ithome.com.tw/articles/10405503)

[

Day 23｜問題改寫：提升使用者問題的搜尋效果

](https://ithelp.ithome.com.tw/articles/10405503)

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