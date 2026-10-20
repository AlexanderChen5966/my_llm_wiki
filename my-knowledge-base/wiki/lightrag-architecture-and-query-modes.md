---
title: LightRAG：知識圖譜 + 向量雙層架構、五種查詢模式與 LLM 角色分工
date: 2026-07-27
tags: [ai/workflow, dev/architecture]
source: https://github.com/hkuds/lightrag
type: article
status: processed
asset_value: 48
---

> **編註（indexer 說明）**：分類階段給的 asset_value 為 48，低於 wiki 條目的 50 門檻。但這是 LightRAG 官方 GitHub README 本體，內容涵蓋完整雙層架構原理、五種查詢模式的適用場景、四角色 LLM 配置建議、效能評測數據與生產環境部署細節，技術深度與可操作性都達到 wiki 條目水準，因此依專業判斷升級為完整 wiki 條目（詳見報告中的理由說明）。同主題的中文導讀文章 [[lightrag-knowledge-graph-vector-search-intro]]（asset_value 18）維持精簡筆記，兩篇互為雙向連結。

HKUDS（香港大學資料密集型系統實驗室）推出的輕量級知識圖譜 RAG 框架，EMNLP2025 論文。採雙層架構同時管理知識圖譜與向量嵌入，橋接傳統向量 RAG 與圖譜 RAG 兩種取向，在維持高擴展性的同時大幅降低索引與查詢的運算開銷。

## T（核心論點）

LightRAG 的核心價值主張是**用遠低於 GraphRAG 的 LLM 呼叫次數，達到比傳統向量 RAG 更完整的跨文件語意理解**。它不依賴社群報告或多跳推理來處理複雜查詢（這是 GraphRAG 昂貴的來源），而是用雙層檢索（實體細節 + 抽象概念）與增量圖譜合併，讓大規模資料集也能維持高品質檢索，即使搭配 30B 開源 LLM 也能有出色表現。

## B（適用場景）

**適用：**
- 需要跨文件、多實體關聯推理的知識庫（法律、金融等需要全局理解或邏輯推理的垂直領域）
- 知識庫會持續新增/刪除文件，需要增量更新而非每次重建索引
- 已有向量 RAG 但發現「問題涉及多個人物/事件關聯」時效果不佳
- 想要生產級部署（PostgreSQL/MongoDB/OpenSearch/Neo4j/Milvus 等多種儲存後端選擇）

**不適用 / 注意：**
- 官方明確建議：**正式整合走 REST API（LightRAG Server），SDK 僅用於嵌入式應用或學術研究**
- 預設檔案持久化的記憶體資料庫僅供開發除錯，正式環境必須換成 PostgreSQL/MongoDB/OpenSearch 等
- Embedding 模型選定後基本不能換（換了要重新嵌入所有 chunk/entity/relation，官方未提供重嵌入工具）
- 部署前務必設定 `LIGHTRAG_API_KEY` 或 `AUTH_ACCOUNTS`，否則所有端點公開；Ollama 相容的 `/api/*` 路由預設對外開放

## R（架構、查詢模式與關鍵配置）

**雙層架構**：文件匯入階段，LLM 從文件擷取實體與關聯建成知識圖譜，同時保留向量索引。查詢時同時利用向量相似度與圖譜關聯路徑，讓檢索結果兼顧「精確事實」與「概念脈絡」。

**五種查詢模式**（`mix` 為預設）：

| 模式 | 適用場景 |
|------|---------|
| `local` | 鎖定特定實體/具體事實的問答，精確匹配局部上下文 |
| `global` | 跨文件推理、宏觀主題、趨勢分析，走關係鏈覆蓋廣泛主題 |
| `hybrid` | 合併 local + global，同時檢索具體實體與全局關係 |
| `naive` | 傳統向量相似度檢索，不使用知識圖譜 |
| `mix` | 融合 local/global/naive，結果最全面但延遲略高於 naive |

**四角色 LLM 配置**（獨立設定，效能與成本平衡的關鍵）：

- **EXTRACT**：逐 chunk 做實體關聯抽取，需求量大，用非 thinking 的快速便宜模型（GPT-5.6-luna / Claude Haiku / Gemini-mini，本地可用 Qwen3-30B-A3B-Instruct）
- **QUERY**：處理長且雜訊多的檢索上下文寫最終答案，需比 EXTRACT 更強，可用 thinking 模型
- **KEYWORD**：輕量延遲敏感步驟，必須非 thinking 模型
- **VLM**：多模態文件解析，任何支援圖片輸入的主流模型即可（本地可用 Qwen3.6-35B-A3B）

**Embedding**：建議選低維度快速模型，本地部署首選 `BAAI/bge-m3`；**啟用 Rerank** 可顯著提升查詢品質（本地 `BAAI/bge-reranker-v2-m3`，延遲增加 1-2 秒，可隨時更換不像 embedding 那樣綁死）。

**四類後端儲存**：KV_STORAGE（LLM 快取/chunk/抽取結果）、VECTOR_STORAGE（向量）、GRAPH_STORAGE（知識圖譜）、DOC_STATUS_STORAGE（文件清單）——生產環境可選單一後端（PostgreSQL/MongoDB/OpenSearch）全包，或分別用 Milvus/Qdrant（向量）+ Neo4j/Memgraph（圖譜）。

**併發調校**：`MAX_ASYNC_LLM`（LLM 併發上限）、`MAX_PARALLEL_INSERT`（建議約為 `MAX_ASYNC_LLM` 的 1/3）、`EMBEDDING_BATCH_NUM`（提高可大幅減少 embedding API 呼叫次數）。

**LLM 逾時排解三因**：模型太慢（<50 tokens/秒時調高 `*_LLM_TIMEOUT`，注意實際生效逾時是設定值的兩倍）、chunk 產生實體過多（用 `OPENAI_LLM_MAX_TOKENS` 限制輸出，`max_output_tokens < LLM_TIMEOUT × tokens_per_second`）、模型陷入輸出迴圈（重跑一次通常解決）、參考文獻章節產生大量低價值實體（`CHUNK_P_DROP_REFERENCES=true` 搭配段落分塊策略自動捨棄）。

**效能評測**（vs NaiveRAG/RQ-RAG/HyDE/GraphRAG，農業/CS/法律/混合四領域）：LightRAG 在 Comprehensiveness、Diversity、Empowerment、Overall 四項指標上對 NaiveRAG/RQ-RAG/HyDE 全面大幅領先（例如法律領域 Overall 84.8% vs NaiveRAG 的 15.2%）；對 GraphRAG 則互有領先，多數領域仍勝出但幅度較小（部分 Mix 領域 GraphRAG 略勝）。

## C（關鍵行動）

- [ ] 選型時先問：需要跨文件關聯推理嗎？需要嗎→LightRAG；只需精確事實檢索→可用更輕量的 naive 向量 RAG（參考 [[rag-retrieval-augmented-generation]]）
- [ ] 正式部署一律走 REST API（LightRAG Server），不要把 SDK 直接嵌入生產程式碼
- [ ] Embedding 模型選型前確認清楚——這是唯一「幾乎不能換」的元件，選錯要重嵌入整個知識庫
- [ ] 四角色 LLM 分開配置：EXTRACT/KEYWORD 用便宜快速模型省成本，QUERY 用更強模型保答案品質
- [ ] 生產環境務必設定 API Key／關閉未認證的公開端點，並把預設記憶體儲存換成 PostgreSQL/MongoDB 等正式後端

## 相關連結

- [[lightrag-knowledge-graph-vector-search-intro]] — 中文導讀文章（wingzero），面向初次接觸者的概念級介紹，可作為本篇技術細節前的入門對照
- [[rag-retrieval-augmented-generation]] — RAG 基本流程與三種檢索方式，LightRAG 是其「向量 RAG 遇到跨文件關聯查詢就失效」痛點的圖譜化解法
- [[enterprise-rag-smb-architecture]] — 中小企業級 RAG 工程骨架（混合檢索+Rerank+鄰居 Chunk 補全），可與 LightRAG 的圖譜/向量雙層設計互相對照
- [[langchain-rag-architectures]] — LangChain 三種 RAG 架構，可比較框架層 vs LightRAG 專用引擎的取捨

## 原始摘要

HKUDS/LightRAG GitHub 官方 README（EMNLP2025 論文 "LightRAG: Simple and Fast Retrieval-Augmented Generation"）。詳述其結合知識圖譜與向量索引的雙層架構、五種查詢模式（local/global/hybrid/naive/mix）、安裝部署方式、LLM 角色分工建議（EXTRACT/QUERY/KEYWORD/VLM）及效能評測數據，顯示其在多個領域大幅優於 NaiveRAG、GraphRAG 等基準。
