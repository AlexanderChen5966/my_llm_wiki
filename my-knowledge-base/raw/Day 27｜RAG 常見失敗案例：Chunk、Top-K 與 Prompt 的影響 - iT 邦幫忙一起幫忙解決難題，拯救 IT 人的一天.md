---
title: "Day 27｜RAG 常見失敗案例：Chunk、Top-K 與 Prompt 的影響 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10406516"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "當 RAG 答錯時，最直覺的三個動作通常是：換更大的模型、把 Top-K 調高、再補幾句 Prompt。問題是，同一句「回答錯了」，背後可能是完全不同的故障：正解沒進候選池、證據閘門錯殺、模型拿到正解..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 27

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 27 篇

## Day 27｜RAG 常見失敗案例：Chunk、Top-K 與 Prompt 的影響

- 分享至

當 RAG 答錯時，最直覺的三個動作通常是：換更大的模型、把 Top-K 調高、再補幾句 Prompt。問題是，同一句「回答錯了」，背後可能是完全不同的故障：正解沒進候選池、證據閘門錯殺、模型拿到正解卻拒答、引用格式不合法，或答案只漏了一個關鍵條件。修錯層不只沒用，還會製造新的變因。

今天先不急著修參數，而是建立失敗追蹤（Failure Trace），用這個系列已經發生過的案例，逐層判斷 Chunk、Top-K、Prompt 與標註各自該負什麼責任。

## 先定位失敗層

`failure_analysis.py` 將失敗依資料流排序：

```python
def diagnose(results, relevant_docs, decision, answer, reference_coverage):
    retrieved_docs = {r.chunk.document_id for r in results}
    relevant = set(relevant_docs)

    if relevant and not (retrieved_docs & relevant):
        return FailureDiagnosis("retrieval", "candidate pool missed labels")
    if relevant and not decision.answerable:
        return FailureDiagnosis("evidence_gate", "answerable question rejected")
    if answer.validation_errors:
        return FailureDiagnosis("output_validation", "; ".join(answer.validation_errors))
    if relevant and answer.status != "answered":
        return FailureDiagnosis("generation", "model returned insufficient")
    if not relevant and answer.status != "insufficient":
        return FailureDiagnosis("refusal", "unsupported question answered")
    if relevant and reference_coverage < 1.0:
        return FailureDiagnosis("answer_content", "reference point missing")
    return FailureDiagnosis("success", "no automatic failure signal")
```

順序很重要。如果候選池沒有相關文件，後面模型再強也沒有證據可用；只有候選池正確，才值得檢查 Prompt 與回答。

## Chunk 失敗：上游碎片被下游放大

Day 6 的 structured Chunking 留下一個孤兒標題，Day 9 的 BM25 長度正規化立刻把它放大：超短 Chunk 只命中一個普通詞，卻因長度很短拿到高分。這是典型的 Retrieval Failure，修法不是告訴 LLM「不要相信短標題」，而是讓標題和正文一起切分，或在索引時加入最小內容長度。

另一種 Chunk 失敗是太大。大 Chunk 可能同時談 401、403、CSRF 與認證，檢索容易命中，模型卻拿到大量不相關內容；太小則只剩「它會影響搜尋結果」這種失去主詞的句子。Chunk 大小必須透過檢索與回答評測共同驗證，不能只看平均字數是否漂亮。

## Top-K 失敗：多帶資料不等於多一層保險

Day 18 的 401/403 Context 取 3 個來源，其中 `S3` 是 CSRF 文件。它和 Spring Security 主題相關，卻不是回答狀態碼差異的必要證據。如果把 `max_sources` 從 3 增加到 5，模型得到更多背景，也得到更多分心機會、Token 成本與引用選擇。

反過來， `max_sources=1` 雖然簡潔，遇到跨文件比較就可能漏證據。Top-K 的兩端是：

- 太小：正確證據被截掉，回答不完整或拒答。
- 太大：干擾來源增加，Prompt 變長，模型可能過度引用。

`context_ablation.py` 固定同一個問題，分別以 1、3、5 個來源執行，列出答案與引用。這種單變量實驗比一次同時改 Chunk、K 與 Prompt 更有資訊量。若結果不穩定，還要多次執行或固定模型版本，不能用單次生成下結論。

## Prompt 失敗：資料正確，輸出契約仍可能破裂

Day 26 的 a01 是最清楚的 Prompt / Output Validation Failure。正解在候選池第一名，證據閘門通過，回答事實也合理；但正文引用 `S1` 、 `S2` ， `citations` 只列 `S1` ，最後被 Fail Closed 拒絕。

這種問題調 Chunk 或 Top-K 都沒有用。正確修法可能是：加強格式示例、使用服務端結構化輸出、驗證失敗後做一次有上限的修復，或選擇更能遵守 Schema 的模型。無論哪一種，都要保留原始輸出與驗證錯誤，不能把修復過程藏起來。

## 拒答失敗：近域問題穿過門檻

Day 22 的 FastAPI `Depends` 問題沒有任何相關文件，Reranker 卻給 0.144，通過 0.10 門檻。這是 Evidence Gate False Positive，不代表最後一定會幻覺，因為模型還有機會閱讀 Context 後回覆 `insufficient` 。

若只看到它穿過門檻就把閾值提高到 0.15，可能在下一批資料錯殺低分的可回答題。修法應該是擴充近域無答案題、評估第二層模型判斷，必要時訓練或選擇專門的 Answerability 模型，而不是追著一題調常數。

## 標註失敗：模型一致錯，可能是秤歪了

q18「不同講法為何找不到同一份文件」被標成 Embedding 文件，但 BM25、E5 與 Cross-encoder 都把斷詞文件排在前面。不同講法造成斷詞差異，本來就是合理答案。這種情況的 Failure Trace 會說 Retrieval 未命中標準文件，卻不能立刻斷言模型錯。

自動診斷只能指出「結果和標籤不一致」，最後仍需第二位標註者仲裁。評測程式本身不能決定標籤真偽，這是所有數據驅動優化都要保留的人類責任。

## 一張修復對照表

| 症狀 | 優先證據 | 應先檢查 | 不該先做 |
| --- | --- | --- | --- |
| 正解不在候選池 | Relevant doc rank | Chunk、斷詞、Retriever | 調回答 Prompt |
| 正解在池中但被拒答 | Gate score、status | 拒答策略與模型判斷 | 增加知識庫外事實 |
| 回答引用不存在來源 | Validation errors | Schema、引用修復 | 放寬為任意網址 |
| 引用正確但漏要點 | Claim / reference points | Context 與生成 Prompt | 只增加 Top-K |
| 所有方法都偏離標籤 | 多模型排名、文件內容 | 標註仲裁 | 看分數後直接改標籤 |

## 失敗追蹤要保存到哪一層？

只有 `failure_type=retrieval` 仍不足以重現問題。每次執行至少要保存原問題與改寫查詢、資料與索引版本、候選 Chunk 和各階段分數、去重後文件排名、證據閘門理由、Context 實際內容、Prompt 版本、模型原始輸出、解析錯誤與最終狀態。

```json
{
  "query_id": "a01",
  "search_query": "HTTP 401 和 403 差異",
  "index_version": "kb-v1",
  "retrieved_docs": ["spring-security-authentication"],
  "decision": "candidate_evidence_found",
  "validation_errors": ["inline citations and citations field do not match"],
  "failure_type": "output_validation"
}
```

Trace 不應無限制保存完整機密文件，也不應把金鑰與 System Prompt 寫入一般 Log。可以保存文件 ID、Chunk 雜湊與受控環境中的內容快照，再以 Trace ID 串起 API、Retrieval 與 LLM 呼叫。目標是讓維運人員能重播決策，而不是建立另一份沒有權限管理的資料副本。

## 用控制變因找真正原因，也要檢查交互作用

一次把 Chunk 從 300 改成 800、Top-K 從 3 改成 8，又換一版 Prompt，即使分數提高也不知道是哪項造成。較可靠的實驗矩陣會固定資料、模型與評測，只改一個因素：

| 實驗 | Chunk | Top-K | Prompt | 要觀察的問題 |
| --- | --- | --- | --- | --- |
| Baseline | 目前設定 | 3 | v1 | 回歸基準 |
| C1 | 較小 | 3 | v1 | 指涉是否被切斷 |
| K1 | 目前設定 | 1 | v1 | 跨文件證據是否遺失 |
| K2 | 目前設定 | 5 | v1 | 干擾與過度引用是否增加 |
| P1 | 目前設定 | 3 | v2 | 格式與要點是否改善 |

每個版本都同時看檢索、回答、引用、拒答與延遲。Prompt v2 若讓 Citation Valid 上升，卻讓拒答題開始硬答，就不是無條件改善。所有實驗也要保存逐題差異，避免總平均掩蓋一類問題的退步。

不過，控制變因只能先定位主效果，RAG 各層並非完全獨立。Chunk 變大後，同樣 Top-K 會消耗更多 Context；Top-K 增加後，原本有效的 Prompt 可能因來源太多而漏列引用；加入 Reranker 又會改變證據閘門的分數分布。確認單項影響後，仍需要少量組合實驗檢查交互作用。

因此修復順序應由上游到下游：先保證資料與 Chunk 正確，再看候選召回與排序，之後校準 Evidence Gate，最後才調生成與輸出。若來源一開始就切錯，後面模型偶爾答對只是依靠既有知識或運氣，不能算系統修復。

也要知道何時停止調參。當失敗來自標籤爭議、資料缺漏或樣本太少時，繼續微調權重只會對評測集過度擬合。這時應新增或仲裁資料，而不是要求模型配合錯誤的尺。若某次調整只修好一題、同時讓另一題退步，也要先判斷兩題代表的真實流量與風險，不以單一總分決勝。

Failure Analysis 的目的不是找到一組永遠最好的參數，而是建立可重複的診斷流程：發現、定位、提出假設、做受控實驗、檢查副作用、保留回歸案例。做到這一步，RAG 優化才從試手氣變成工程。

## 結語

今天把「回答錯了」拆回真正發生的層：Chunk 可能製造碎片，Top-K 可能漏證據或帶進干擾，Prompt 可能讓正確內容輸出成不合法結構，拒答門檻會放過近域問題，而評測標籤本身也可能有爭議。 `FailureDiagnosis` 不會自動修好系統，但能阻止我們在錯誤元件上浪費調參。

下一篇進入安全性。知識庫文件不只可能「不相關」，還可能故意寫入指令，要求模型忽略規則、洩漏 Prompt 或偽造引用。這不是一般品質失敗，而是不可信資料跨越指令邊界的攻擊；防禦必須從 Prompt、來源處理、輸出驗證與系統權限一起設計。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10406229)

[

Day 26｜評估回答品質：正確性、引用與幻覺分析

](https://ithelp.ithome.com.tw/articles/10406229)[下一篇](https://ithelp.ithome.com.tw/articles/10406994)

[

Day 28｜RAG 安全性：Prompt Injection 與惡意文件

](https://ithelp.ithome.com.tw/articles/10406994)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/4/30/f49229f9-d26f-4f63-935f-130e4de630c6_small.jpg)

[AI 時代資料處理殺手鐧：Microsoft Fabric + Power BI Copilot 讓你用說的畫圖表](https://itplus.ithome.com.tw/vod-page/824?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/37b66aa1-ff69-4b7c-9ea1-9a23aeddaf8d_small.jpg)

[國泰 2024 SRE Shift-Right 的發展策略](https://itplus.ithome.com.tw/vod-page/1029?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

SRE CONFERENCE |

35 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/10/31/0c286c67-9d3e-4338-89cf-1637ba816e60_small.png)

[健保數位轉型新篇章：雲端架構、資訊治理與醫療服務革新](https://itplus.ithome.com.tw/vod-page/1265?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣醫院資訊主管會議 |

39 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/5/4/b7bc8d02-d14e-4cd7-8c9e-5c8c23df32e0_small.png)

[Agentic SRE 的崛起：以 kagent 與 A2A 協議建構下一代觀察性平台](https://itplus.ithome.com.tw/vod-page/1428?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Observability Day 可觀察性技術日 |

38 分