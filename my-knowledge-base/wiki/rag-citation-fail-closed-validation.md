---
title: RAG 引用機制設計：封閉引用鏈與 Fail Closed 驗證
date: 2026-09-03
tags: [ai/agent, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10405135（Agentic RAG 系列 Day 21）
type: article
status: processed
asset_value: 64
---

## T（核心論點）

「請附上來源」不能只是 Prompt 裡的一句要求，必須變成程式可以解析、檢查與映射的**封閉引用機制**：模型只能用 `[S1]` 等短代號指向 `ContextBundle` 中的來源，來源名稱與連結一律由程式從 Metadata 產生，模型不能自行撰寫網址或文件標題。驗證器執行四條規則（代號必須存在於 Context、`answered` 回答至少要有一個正文引用、正文與 `citations` 欄位集合必須完全相等、`insufficient` 不得附來源），任何一條失敗就採 **Fail Closed**：把狀態改判為 `insufficient`，不猜測補全也不悄悄修改模型輸出。引用有效性（Validity）、覆蓋（Coverage）、支持度（Support）是三種不能合併的品質面向，混成單一分數會讓系統不知道該修哪一層。

## B（適用場景）

**適用：** 設計 RAG 系統的引用/來源顯示機制；建立可程式驗證的回答契約（Schema + 四條引用規則）；決定引用不一致時該 Fail Closed 拒答還是嘗試修復；把長引用列表改成緊跟主張句子的 Claim-level 引用格式。

**不適用 / 注意：** 引用驗證只能證明代號存在、格式一致、來源能映射，**不能證明來源文字真的支持回答中的每個主張**——這需要後續的回答評測（見 [[rag-eval-answer-quality-citation-hallucination]]）與人工 Groundedness 檢查；也不能防止惡意來源要求模型偽造引用（見 [[rag-security-prompt-injection-four-layer-defense]]）。

## R（操作步驟）

1. **建立封閉引用鏈**：`[S1] → chunk_id → document_id → title/source_path/source_url`，模型只選擇代號，最終顯示的來源名稱一律由程式從 `ContextBundle` 取出，不讓模型自由填寫文字。
2. **先解析回答契約**：`parse_model_output` 把模型輸出解析成 `ParsedAnswer`（status/answer/citations），容許 Markdown JSON 圍欄但不猜測缺失欄位；格式不合法時回覆「輸出格式無法驗證」。
3. **執行四條引用規則**：① 正文與 `citations` 中的代號都必須存在於本次 Context；② `answered` 回答至少要有一個正文引用；③ 正文出現的引用集合必須和 `citations` 完全一致；④ `insufficient` 回答不得附上來源。
4. **驗證失敗即 Fail Closed**：正文用了 `[S1][S2]` 但 `citations` 只列 `S1` 時，直接把狀態改成 `insufficient`，不猜測模型只是忘了列、也不悄悄刪除正文引用。
5. **來源列表由程式產生**：只把真正通過驗證且被引用的 `ContextSource` 交給介面；絕對檔案路徑只適合內部除錯，外部應改顯示允許的 `source_url`。
6. **引用緊跟相關句子**：Prompt 要求引用貼近所支持的主張，而非集中列在段落尾端，讓「哪一句話由哪份資料支持」保留結構化線索，前端也能讓使用者點擊展開對應 Chunk。
7. **多 Chunk 同源時 Chunk 層與文件層分開處理**：正文保留多個代號（Chunk 層驗證），UI 可依 `document_id` 分組顯示（文件層閱讀），評估時不能把同文件多個 Chunk 誤算成多份獨立來源交叉佐證。
8. **正規表示式刻意收緊格式**：只接受 `[S1]`，不接受 `[s1]`、全形括號等變體；若要放寬應先在解析層正規化，不讓驗證器同時處理多種寫法。
9. **拆開三種引用品質**：Validity（代號是否存在，失敗多半是輸出契約問題）、Coverage（重要主張是否都有引用，不足可能要調 Prompt）、Support（來源內容是否真的支持主張，不足可能來自檢索/Chunk/模型推論），分開才有可修復方向。

## C（關鍵行動）

- [ ] 引用一律用短代號機制，來源文字與連結由程式從 Metadata 映射，不讓模型自由撰寫
- [ ] 建立四條引用驗證規則的單元測試（合法單一引用、合法多引用、正文引用不存在代號、欄位多宣告代號、`answered` 無引用、`insufficient` 附來源）
- [ ] 引用驗證失敗時採 Fail Closed 改判 `insufficient`，不猜測補全、不悄悄修改模型輸出
- [ ] 正規表示式只接受單一內部格式，若需放寬先在解析層正規化再交給驗證器
- [ ] Validity／Coverage／Support 分開追蹤與回報，不合併成單一「引用正確率」分數

## 原始摘要

「讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理」系列 Day 21：建立 RAG 引用驗證機制，模型只能用短代號指向來源，程式解析回答契約後執行四條規則。以 Day 20 的失敗案例（正文用了 S1/S2、citations 只列 S1）示範 Fail Closed：驗證失敗即整體判定為 insufficient，不做猜測式修補。區分引用有效性、覆蓋、支持度三種不可合併的品質面向。本篇是 Agentic RAG Day21→30 系列鏈起點，下一篇（Day 22）處理拒答與資料不足判斷。

## 相關連結

- [[rag-refusal-evidence-gate-threshold-calibration]] — **系列下一篇（Day 22）**：拒答三層防線的第三層沿用本篇的引用驗證機制
- [[agentic-rag-30-day-series-recap]] — 系列 Day 30（完結篇，既有條目）：總結整個 Pipeline，引用本篇的四層拒答機制與 Fail Closed 案例
- [[agentic-rag-day20-first-pipeline-llmclient-protocol]] — **系列上一篇（Day 20）**：本篇的引用驗證器直接源自 Day 20 第一次真實呼叫撞見的「正文引用 [S2] 但 citations 只列 S1」案例
- [[ai-output-verifiable-id-and-status-field]] — 同一原則的 prompt＋人工版：必附 DOI／PMID／頁碼＋必填狀態欄＋引用三讀（2026-09-23 新增）
- [[citation-verifier-multi-field-match-report-not-fix]] — 同原則的非工程師版：skill 對 Crossref／PubMed 五欄比對，只報告不修正（2026-09-29 新增）
