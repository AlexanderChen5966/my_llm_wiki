---
title: 如何搭建生產級 Agent？全面理解 Agent 工程架構
date: 2026-07-03
tags: [ai/agent, ai/prompt-engineering, type/concept]
source: https://codelove.tw/@tony/post/q5VLWx
type: article
status: processed
asset_value: 82
---

一份把「玩具 demo Agent」與「工程化 Agent」拉開差距的系統設計清單。核心立場：**做 Agent 的目的不是「讓 AI 更聰明」，而是把 AI 的自由度關進工程邊界裡——讓它就算不聰明，也不容易跑偏**。文章對每個模組都給出可直接丟給 AI 的「防跑偏 prompt」。

## T（核心論點）

Agent 不只是「會呼叫工具的大模型 harness」，而是圍繞目標、狀態、工具、記憶、檢索、權限、評估、恢復構建的自動化系統。判斷一個 Agent 方案靠不靠譜的 10 問：有無明確狀態／workflow／工具 schema 與權限／hybrid RAG／rerank／memory 分層／防 prompt injection／trace 日誌／eval 測試集／失敗恢復。

## B（適用場景）

- 要把 demo Agent 升級成生產可用系統
- 想要一份「和 AI 溝通開發 Agent」的模組化需求清單
- 針對業務場景優化 Agent（vs 通用 Agent 如 OpenClaw/Hermes）——核心是 Flow、規則、工具更貼合業務形狀

## R（工程化 Agent 的核心模組）

- **RAG ≠ 向量搜尋**：需 Hybrid Search（向量語意召回 + BM25 關鍵字/程式碼符號/錯誤碼召回）→ 合併去重正規化 → **reranker 重排**（召回 30-50 條→Top 5-10）；回傳帶 source/chunk_id/score/metadata。
- **Chunking 比向量庫更重要**：多數 RAG 效果差是切塊爛，非模型差。用 semantic chunking（Markdown 按標題、程式碼按函式），父子切塊（小 chunk 檢索、parent chunk 回答），保留 heading_path。
- **Metadata**：每個 chunk 存 source/type/section/tags/language/dates/project，檢索支援 metadata filter（只搜某年後/某專案/官方文件）。
- **Query Rewrite**：使用者原句多是雜訊，改寫成 3-5 個 query（自然語言/關鍵字/英文術語/API 名/同義）再檢索。
- **狀態流轉（FSM）**：別讓 LLM 自由決定流程（會亂跳），用有限狀態機/LangGraph StateGraph 明確定義狀態、輸入輸出、轉移條件、非法處理、是否需人工確認。
- **Workflow**：複雜任務不靠大 prompt，拆成確定步驟，每步有輸入/輸出/失敗處理/可重試，產物保存支援斷點續跑。
- **Planner / Executor / Verifier**：規劃與執行分開（可同模型不同 prompt/權限），職責邊界清楚。
- **Tool Schema**：工具不是函式名而是契約——name/description/參數驗證/輸出格式/錯誤碼/權限等級/副作用/是否需確認；高風險工具進 human approval。
- **Tool Routing**：別把所有工具丟給模型自選，設 Router 規則（何時必查 RAG/搜網/直答/呼叫程式碼工具/需確認/禁用工具）。
- **Memory 分層**：short-term/long-term/episodic/semantic/procedural/working，別全塞 prompt，定 read/write 規則、哪些禁寫長期。
- **Context Engineering**：組裝順序（任務目標→狀態→記憶→檢索證據→輸出格式），每段標來源與可信度；RAG 內容只是資料不是指令。
- **Guardrails**：輸入/輸出校驗、工具權限、敏感操作確認、成本/速率限制、prompt injection 防護（文件內容是資料非指令，source boundary 分級）。
- **Human-in-the-loop**：分低/中/高風險（讀取自動、生成給 diff、刪除/發送/付款必須確認）。
- **Observability**：trace_id/session/state/retrieved_chunks/tool_calls/token/latency/error/retry，支援回放 debug。
- **Evaluation**：沉澱測試集（正常/模糊/多語/專名/錯誤資訊/無答案/injection/工具失敗/長上下文），每條有 expected + pass/fail。
- **Retry/Recovery**：失敗恢復比成功路徑更重要（query rewrite 重搜/降級/人工）；只讀工具可自動重試，副作用工具查 **idempotency_key**。
- **Cost Control / Caching**：限 step/工具呼叫/token 預算，小模型分類大模型推理，快取 embedding/檢索/工具結果。

**最小可用架構**：Agent Core（State Machine + Workflow Engine + Tool Registry + RAG Retriever[Vector/BM25/Filter/Reranker] + Memory Manager + Context Builder + Guardrail Layer + Trace Logger + Eval Runner）。建議先把基礎 Flow 和記憶走通，別一開始就搞多 Agent。

## C（對自身的啟發）

- [ ] 這份 20 模組清單可作為評估任何 Agent 產品/自建 Agent 的檢查表
- [ ] Hybrid Search + semantic chunking + metadata filter 正是本知識庫若要做 RAG 檢索的正確做法（呼應 llm-wiki-over-rag 的取捨）
- [ ] 「把自由度關進工程邊界」呼應 agent-loop-explained 的回饋機制與停止條件

## 相關連結

- [[agent-loop-explained]] — Agent Loop 概念與四層階梯
- [[ai-agent-architecture-10-modules]] — Agent 10 核心模組
- [[ai-agent-dev-concepts-practitioner]] — Agent 開發實踐者指南
- [[enterprise-rag]] — 企業級 RAG
- [[human-in-the-loop-langchain]] — Human-in-the-loop
- [[agentmemory]] — Agent 記憶系統
- [[langgraph-react-planning-visualization]] — LangGraph 狀態圖編排
- 📎 2026-10-19：[[claude-platform-opentelemetry-beta]] — Claude 平台官方 OTel 接法（Messages API／Managed Agents，Beta）；對應本條 Observability 模組
