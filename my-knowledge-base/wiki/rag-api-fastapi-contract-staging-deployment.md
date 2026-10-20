---
title: RAG 包裝成 API：FastAPI 契約、模型生命週期與部署前檢查清單
date: 2026-09-03
tags: [ai/agent, dev/architecture, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10407057（Agentic RAG 系列 Day 29）
type: article
status: processed
asset_value: 64
---

## T（核心論點）

把 RAG Pipeline 包成 FastAPI 服務時，真正要固定的不是畫面，而是**輸入輸出契約、模型生命週期、錯誤語意，以及未審核文件不能立即參與回答的安全邊界**。四個端點（`/health`、`/v1/search`、`/v1/answers`、`/v1/documents`）中搜尋與回答分開很重要，Debug 時可先確認檢索結果再檢查生成。`status=insufficient` 是正常業務結果，**不應回 HTTP 500**——API 設計必須區分「不知道答案」和「服務壞了」。Embedding 與 Reranker 只在 FastAPI Lifespan 啟動時載入一次；文件匯入回 202 且 `indexed=false`，延續 Day 28 的 Staging 審核邊界。從「能呼叫」到「能部署」還有四道工程題：狀態碼是否反映失敗層級、同步問答是否有併發控制、文件匯入是否可追蹤、存活/就緒/回答品質是否分開監控——**API 能啟動不等於已符合生產安全**。

## B（適用場景）

**適用：** 把命令列 RAG Pipeline 包裝成 HTTP API；設計 API 契約測試（用 FakeService 替換模型，不付完整推論成本）；判斷 HTTP 狀態碼該如何反映拒答/待審核/服務故障等不同語意；設計文件匯入的 Staging 與非同步審核流程；部署前的安全與可靠性檢查清單。

**不適用 / 注意：** 今天完成的是邊界清楚的服務層，模型與搜尋品質仍由 Day 25、26 的獨立評測負責；本機開發預設只綁 `127.0.0.1`，目前沒有認證/授權/Rate Limit/TLS/Audit Log，不應直接改成 `0.0.0.0` 暴露到公開網路。

## R（操作步驟）

1. **四個端點對應三類能力**：`GET /health`（健康檢查）、`POST /v1/search`（回傳精排後 Chunk）、`POST /v1/answers`（回傳驗證後回答與來源）、`POST /v1/documents`（文件進 Staging，尚不索引）；搜尋與回答分開讓 Debug 時能先確認找到什麼再檢查生成。
2. **用 Schema 把輸入邊界寫清楚**：`question` 限 2-500 字、`top_k` 限 1-10、`max_sources` 限 1-5，避免單次請求無限制消耗模型與 Context；回應不直接暴露本機絕對路徑，只回傳 Marker、標題與 Chunk ID。
3. **狀態語意要正確**：`status=insufficient` 是正常業務結果應回 HTTP 200，模型服務故障或索引損壞才是系統錯誤（503/504）；API 設計必須明確區分「不知道答案」和「服務壞了」。
4. **模型只在服務啟動時載入**：FastAPI Lifespan 建立 `RagPipeline`，關閉時釋放 Client；測試用 `FakeService` 替換，不載入完整模型即可驗證路由/Schema/狀態碼/Staging 行為。
5. **文件匯入先進 Staging**：ID 限小寫英數與連字號，正文限 20-50,000 字，暫存檔加 `os.replace()` 原子寫入，Metadata 標記 `status: draft`；API 回 HTTP 202，`indexed=false` 是誠實的非同步契約而非未完成的藉口。
6. **契約測試涵蓋四端點基本行為**：健康端點、搜尋 200、回答來源 Marker、文件匯入回 202 且 `indexed=false`；用 FastAPI Test Client 與暫存 Staging 目錄。
7. **狀態碼要反映失敗層級**：422（輸入不符合 Schema）、202（已接受待審核）、200 + insufficient（證據不足但正常完成）、503/504（上游模型逾時）；內部例外不能原樣回傳，服務端以 Trace ID 對應詳細 Log，外部只收到穩定錯誤碼。
8. **同步問答需併發控制**：用 Semaphore 限制生成併發，設定佇列長度與 Request Timeout；同步阻塞的模型函式應放入受控 Worker Thread/Process，量測 P50/P95/P99 延遲與拒絕數。
9. **文件匯入應是可追蹤工作**：回 202 後應另回傳 `job_id`，讓呼叫者查詢 pending/reviewing/indexed/failed；索引更新用版本化 Collection，新版本完整建置並跑 Smoke Test 後才原子切換讀取別名。
10. **存活、就緒、回答品質是三件事**：`/health` 只代表 Process 還活著；Readiness 需確認索引可讀、模型已載入、Staging 目錄可用；Readiness 通過仍不代表回答品質合格，離線評測應在部署前執行，線上再監看拒答率與引用驗證失敗率。
11. **版本化與相容性**：`/v1` 是 API 契約版本，不等於模型版本；新增可選欄位通常相容，改變既有欄位型別（如 `sources` 從陣列改字串）會破壞客戶端，應保存 OpenAPI Snapshot 避免重構時無意改壞介面。

## C（關鍵行動）

- [ ] 搜尋與回答端點分開設計，讓 Debug 能先確認檢索結果再檢查生成
- [ ] `status=insufficient` 回 HTTP 200，只有真正的服務故障才回 5xx，明確區分「不知道答案」和「服務壞了」
- [ ] 用 FastAPI Lifespan 管理模型生命週期，避免每個 Request 重新載入 Embedding/Reranker
- [ ] 文件匯入回 202 且 indexed=false，並規劃 job_id 可追蹤的背景審核工作
- [ ] 部署前完成檢查清單：非 Root 執行、TLS/CORS、Body 大小上限、認證授權限流、備份與索引切換回滾演練、Graceful Shutdown

## 原始摘要

「讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理」系列 Day 29：用 FastAPI 把 RAG 搜尋、問答、文件匯入包成四個端點，重點在固定輸入輸出契約、模型只在啟動時載入、錯誤語意分層，以及文件匯入先進 Staging 不立即污染索引。文末列出從「能呼叫」到「能部署」還需解決的四道工程題。本篇是 Agentic RAG Day21→30 系列鏈末端銜接篇，下一篇（Day 30，既有條目）是完結篇總結整個系列。

## 相關連結

- [[rag-security-prompt-injection-four-layer-defense]] — **系列上一篇（Day 28）**：本篇的 Staging 審核端點實作延續該篇的安全邊界設計
- [[agentic-rag-30-day-series-recap]] — **系列下一篇（Day 30，既有條目，完結篇）**：本篇的 API 服務層是系列總結的最後一塊拼圖
