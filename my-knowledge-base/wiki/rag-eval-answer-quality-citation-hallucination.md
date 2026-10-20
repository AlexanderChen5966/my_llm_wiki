---
title: RAG 回答評測：Claim-level Groundedness 與評分器自身的誤判
date: 2026-09-03
tags: [ai/agent, type/concept]
source: https://ithelp.ithome.com.tw/articles/10406229（Agentic RAG 系列 Day 26）
type: article
status: processed
asset_value: 80
---

## T（核心論點）

檢索找到正確文件只代表模型「有機會」答對，不代表它會選對內容、保留條件、附對來源，甚至願意回答。建立回答評測需要四個自動指標（Status Accuracy、Citation Valid、Citation Precision、Reference Coverage），但**不直接叫另一個 LLM 當裁判**——八題規模下人工逐題審閱比再加一個有偏差、會漂移的黑盒裁判更誠實。本機端到端測得 Status Accuracy 7/8、Citation Valid 7/8，唯一失敗是 Day 21 已知的引用不一致案例，系統正確 Fail Closed；但**自動評分器本身也會誤判**——字串規則的 `reference_points` 沒收錄「不等同」這個可接受詞，導致合理答案被誤判未覆蓋，證明測量系統本身也需要被檢查。最重要的方法論是把答案拆成可驗證的 **Claim**，逐一標記 `supported`／`contradicted`／`not_in_source`／`not_verifiable`，才能抓出「條件句被壓縮成絕對句」這類過度概括型幻覺；引用存在率不能代替 Claim-level Groundedness。

## B（適用場景）

**適用：** 建立 RAG 回答評測集（含 expected_status 與 reference_points）；設計 Status Accuracy/Citation Valid/Citation Precision/Reference Coverage 四個自動指標；把答案拆解成可驗證主張並分類幻覺類型；判斷何時該用 LLM-as-a-Judge、何時人工審閱更誠實；設計分層品質門檻（格式與引用零容忍、語意品質抽樣審閱）。

**不適用 / 注意：** 本篇的自動指標是低成本回歸檢查，不是完整語意判分；八題規模下不使用 LLM-as-a-Judge，資料量擴大後才考慮讓 Judge 模型先做 Claim 拆分降低人工閱讀量，但其輸出應是待審線索而非唯一真值。

## R（操作步驟）

1. **回答評測需要不同標籤**：`answer_eval.yaml` 每題含 `expected_status`（answered/insufficient）、`relevant_docs`、`reference_points`（每組可接受說法陣列，只要包含其中一詞就算覆蓋）。
2. **四個自動指標**：Status Accuracy（狀態是否符合標籤）、Citation Valid（是否通過 Day 21 格式與封閉集合驗證）、Citation Precision（被引用文件中屬於標註相關文件的比例）、Reference Coverage（參考要點被字串規則命中的比例）。
3. **不用 LLM 當唯一裁判**：裁判模型也有偏差、版本漂移與 Prompt 敏感性，八題規模時人工逐題審閱更誠實。
4. **實測結果與解讀**：Status Accuracy/Citation Valid 皆 7/8，唯一失敗是 a01「HTTP 401 和 403 有什麼差別」——不是事實錯誤，是正文用了 `S1`/`S2` 但 `citations` 只列 `S1`，驗證器正確 Fail Closed；產品中可加一次有上限的格式修復，但不能為了湊 8/8 就放寬驗證規則。
5. **自動評分器會誤判**：a03 答案明確寫出「不等同」，人工閱讀合理，但 `reference_points` 只接受「不等於」，Coverage 因此只有 0.5——這是評分器失敗，不是模型失敗，總分下降時要先看內容。
6. **拆解答案為可驗證 Claim**：逐句列出 Claim（如「401 代表身分尚未確認」），逐一檢查引用的 Chunk 是否支持，一個 Claim 沒有來源支持即為 Unsupported Claim，即使整段「大致正確」也不能忽略。
7. **四種 Claim 標記**：`supported`（來源直接支持）、`contradicted`（來源明確相反）、`not_in_source`（模型加入來源沒寫的細節）、`not_verifiable`（價值判斷或無法從資料驗證）。
8. **幻覺分類**：來源外新增、錯誤歸因、數字/版本錯誤、相互矛盾、過度概括、遺漏關鍵條件，各自對應不同修法（Prompt、Metadata、Chunk 邊界）。
9. **Citation Relevance 與 Citation Support 是兩件事**：引用合法但文件不相關屬 Citation Relevance 問題；文件相關但不支持具體句子屬 Citation Support 問題，不能只用 Citation Precision 代表兩者。
10. **一次回答要能完整重播**：最小 Answer Run 應保存原問題、改寫查詢、候選與分數、Reranker 結果、Evidence Decision、Context Marker 對照、Prompt 版本、原始模型輸出、解析與驗證錯誤、最終公開結果。
11. **品質門檻分層**：格式與引用 Validity 零容忍，語意品質用抽樣審閱與趨勢觀察；不可合併成單一平均分掩蓋嚴重失效；評分器本身的錯誤也要列成正式結果並版本化。

## C（關鍵行動）

- [ ] 回答評測集除相關文件外，還要標註 expected_status 與 reference_points（可接受說法陣列）
- [ ] 資料量小時優先人工逐題審閱，不急著引入 LLM-as-a-Judge 當唯一裁判
- [ ] 人工審閱時把答案拆成 Claim，逐一標記 supported/contradicted/not_in_source/not_verifiable
- [ ] 幻覺分類記錄（來源外新增/錯誤歸因/版本錯誤/矛盾/過度概括/遺漏條件），對應不同修法
- [ ] 品質門檻分層設計：格式與引用零容忍，語意品質抽樣審閱；評分器自身的誤判也列為正式結果並版本化

## 原始摘要

「讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理」系列 Day 26：建立八題回答評測集，以本機模型跑出 Status Accuracy 7/8、Citation Valid 7/8、Citation Precision 0.812、Reference Coverage 0.812。唯一 Status 失敗即 Day 21 的引用不一致問題，被 Fail Closed 正確攔截；另揭露字串規則評分器本身也會誤判。下一篇（Day 27）建立失敗分層，判斷 Chunk、Top-K、Prompt 各自的責任。

## 相關連結

- [[rag-eval-recall-mrr-hit-rate-dedup]] — **系列上一篇（Day 25）**：檢索評測完成後，本篇把評測往下游推一層
- [[rag-failure-diagnosis-chunk-topk-prompt]] — **系列下一篇（Day 27）**：本篇的 Status 失敗案例是該篇失敗分層診斷的具體範例之一
- [[rag-citation-fail-closed-validation]] — 系列 Day 21：本篇的 Citation Valid 指標與唯一失敗案例都沿用該篇的四條引用驗證規則
- [[llm-judge-confidence-cascade]] — 分層評審：便宜評審信心夠才採用、不確定升級強模型；與本篇「不用 LLM 當唯一裁判」同一方向（CMU 研究，原論文待查）
