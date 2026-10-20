---
title: Agentic RAG Day20：串成第一個 RAG Pipeline，意外撞見契約違反案例
date: 2026-09-03
tags: [ai/agent, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10405040
type: article
status: processed
asset_value: 60
---

> **系列條目**：「讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理」系列 Day 20，
> 承接 [[agentic-rag-day19-generation-contract-prompt-design]]（Day 19）建立的生成契約，本篇首次
> 把檢索、精排、Context 建構與 LLM 生成串成完整 Pipeline。**這次真實呼叫直接撞見了 Day19 契約
> 被違反的案例，是既有 [[rag-citation-fail-closed-validation]]（Day 21）引用驗證機制的動機來源。**

## T（核心論點）

> **第一次真實呼叫就暴露 Day19 訂下的契約沒被完整遵守——這證明「Prompt 不是驗證器」，只用眼睛
> 看答案是否合理，攔不住結構性的不一致。** 用本機 `qwen3.5:9b` 透過 Ollama 跑出第一個 RAG 回答時，
> 回答正文用了 `[S1]` 與 `[S2]`，但 `citations` 欄位只列 `["S1"]`。這個真實案例**直接是** Day 21
> 引用驗證器（Fail Closed 機制）的動機來源與測試案例：既有條目 [[rag-citation-fail-closed-validation]]
> 描述的四條規則，正是為了攔住這種「正文引用比宣告欄位多」的情況而設計。

透過 `LlmClient` Protocol 把 Pipeline 抽象化，不綁死單一模型供應商（Ollama / OpenAI 相容 /
`StaticLlmClient` 離線替身），並保留 `RawRagAnswer`（原始輸出＋Context）供除錯與重播，不急著
把錯誤吞掉美化。

## B（適用場景）

**適用：** 第一次把 RAG 各階段（檢索/精排/Context/生成）串成端到端 Pipeline；需要讓 Pipeline
不綁死單一 LLM 供應商，同時保留離線測試能力；需要區分「模型服務故障」與「知識庫沒答案」兩種
不同的錯誤語意；需要為多階段查詢建立可觀察、可重播的追蹤資料。

**不適用 / 注意：** 「可以跑通的 RAG」不等於「可以直接交付的 RAG」——第一版仍缺三道關卡：
JSON 能否解析、引用是否合法、資料是否足以回答，這三件事不能交給模型自己宣布成功；換成更大的
模型也不會自動解決資料流問題，來源映射、拒答、評測與服務錯誤仍需應用程式負責。

## R（操作步驟）

1. **用 Protocol 抽象化模型供應商**：定義 `LlmClient(Protocol)` 只要求 `generate(messages) -> str`，
   實作三種 Client：本機 `OllamaClient`（qwen3.5:9b）、`OpenAICompatibleClient`、離線測試用
   `StaticLlmClient`（固定回應，不需 GPU/網路）
2. **區分兩種 K**：檢索 top_k 至少 5（精排候選池，讓 Reranker 有選擇空間），Context 預設只放
   3 個來源（生成預算，節制交給模型的來源量），兩種 K 服務不同目的
3. **第一次真實呼叫並檢查結果**：跑通後不能只看答案讀起來是否合理，要逐欄核對正文引用與
   `citations` 欄位是否一致——本次發現正文含 `[S1][S2]`，`citations` 只列 `["S1"]`
4. **保留 `RawRagAnswer` 不急著美化**：保存原始模型輸出與實際 Context，讓除錯能回答「檢索到
   什麼／模型看到什麼／模型原始輸出是什麼」三層問題，不把三層壓成一個黑盒子
5. **拆成七個可觀察階段**：question → retrieval_query → candidate_results → reranked_results →
   context_bundle → messages → raw_model_output（Day20 尚未做 parsed/validated 這一層，留給 Day21）
6. **區分模型服務故障與知識庫沒答案**：統一用 `LlmClientError` 表示連線拒絕/逾時/服務 500/JSON
   損壞，上層 API 應回 502/503；只有可回答性規則或模型合法輸出的 `insufficient` 才是正常拒答，
   不可把所有 Exception 都吞成 insufficient
7. **Timeout 分層設計**：連線 Timeout、讀取 Timeout、整體請求 Deadline 意義不同，最外層建立
   Deadline 後再分配剩餘時間給檢索與生成
8. **用 `StaticLlmClient` 做離線契約測試**：驗證 Retrieval 之後的 Messages、JSON 解析、引用映射
   與 API 回應，不需要啟動 Ollama；另一組替身可故意回傳壞 JSON、`S999` 或逾時，確認系統採
   Fail Closed
9. **分層記錄延遲與成本**：斷詞、BM25、Query Embedding、Qdrant、Cross-encoder、Prompt 序列化、
   LLM 推論皆需分層記錄，不能只看端到端一個數字；來源數增加不一定讓回答變好，卻一定增加
   Prompt Token 與 Prefill 時間

## C（關鍵行動）

- [ ] 串 RAG Pipeline 時用 Protocol/介面抽象化 LLM 供應商，同時準備離線替身供測試
- [ ] 檢索候選池（top_k）與生成 Context 上限（max_sources）分開設計，不要用同一個 K
- [ ] 第一次真實生成結果務必逐欄核對正文引用與宣告欄位是否一致，不能只憑閱讀判斷「答案合理」
- [ ] 保留原始模型輸出與 Context（不急著美化），供除錯與後續重播使用
- [ ] 用獨立錯誤型別區分「模型服務故障」與「知識庫沒答案」，避免使用者誤判資料不存在

## 原始摘要

「Agentic RAG」系列 Day 20（article_id 10405040），承接 Day19 的 Prompt 契約，首次把檢索、精排、
Context 建構與 LLM 生成串成完整 Pipeline，用本機 qwen3.5:9b 透過 Ollama 產生第一個 RAG 回答。透過
LlmClient Protocol 抽象化模型供應商（Ollama / OpenAI 相容 / StaticLlmClient 離線替身），並保留
RawRagAnswer（原始輸出＋Context）供除錯與重播。第一次真實呼叫就暴露 Day19 訂下的引用契約未被
完整遵守：回答正文用了 [S2]，但 citations 欄位只列 S1，證明 Prompt 不是驗證器。文末明確銜接下一篇
（Day 21，article_id 10405135）：把來源從模型自由書寫的文字，變成程式可驗證與映射的資料。

## 相關連結

- [[agentic-rag-day19-generation-contract-prompt-design]] — 系列上一篇（Day 19），建立本篇跑出第一個
  回答所依據的生成契約
- [[rag-citation-fail-closed-validation]] — 既有條目（Day 21）：本篇撞見的「正文引用 [S2] 但
  citations 只列 S1」案例，直接是該條目引用驗證器（Fail Closed 機制）的動機來源與真實測試案例
