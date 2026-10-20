---
title: 30 天打造 Agentic RAG 知識助理：完整回顧與有邊界的 Agentic 定義
date: 2026-09-04
tags: [ai/agent, type/concept]
source: https://ithelp.ithome.com.tw/articles/10407473（「讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理」系列 Day 30，完結篇）
type: article
status: processed
asset_value: 60
---

## T（核心論點）

30 天系列的完整回顧：關鍵從來不是挑一套向量資料庫、寫一段「很強的 Prompt」，而是建立一條能被拆解、測試、拒絕與追溯的資料流（清理→BM25/Embedding 混合檢索→Reranker→LLM 生成→引用驗證→拒答）。文章特別強調此處的「Agentic」**是一組有邊界、可記錄、可回退的受限決策，而非自由規劃、任意呼叫工具的通用 Agent**：問題需要改寫嗎？候選證據足夠嗎？模型輸出符合引用契約嗎？來源含注入語句嗎？每個決策都有輸入、輸出、理由與失敗路徑。把限制寫清楚不是削弱成果，而是讓下一次改善有正確起點——RAG 最危險的不是「還有問題」，而是不知道問題在哪裡卻用流暢回答掩蓋它。

## B（適用場景）

**適用：** 需要一份完整的 Agentic RAG 端到端案例（含實測數據）作為自建知識助理的參考基線；設計拒答機制與引用驗證的具體規則；理解「Agentic」在 RAG 情境下的合理範圍（有邊界的受限決策，而非通用 Agent）。

**不適用 / 注意：** ⚠️ 本篇雖與 Data Machi 系列（Day 21-23 之後接續的 Day 24-30，見 [[langchain-langgraph-basics]] 等條目）同樣以「Agentic AI」為方向，但**作者、iThome 系列 ID 與系列標題完全不同**（本篇作者 ID 20163705，系列共 30 篇已完結；Data Machi 作者 ID 20169646，系列共 24 篇進行中），兩者主題方向接近但為不同作者的獨立系列，不應混為一談，也不建立跨系列連結；知識庫僅 9 份自寫技術文件、17 個 Chunk，評測集僅 18 題檢索題/10 題拒答題/8 題回答題，且缺雙人標註，規模遠小於生產系統，不能直接當作效能基準套用到其他場景。

## R（操作步驟）

**完整系統流程：** 繁體中文 Markdown → 清理與結構化 Chunk → BM25 + Embedding（Qdrant）→ RRF Hybrid Recall → Cross-encoder Reranker → 證據是否足夠判斷（不足則拒答）→ Context Builder → Grounded Prompt → LLM → 格式與引用驗證（無效則拒答）→ 回答與可追溯來源。

**檢索成果演進：** 詞集合基準 Hit@1 僅 0.56（9/16）→ 完整精排（Reranked）達到 Hit@1 0.938、Hit@3/Recall@3 皆 1.000（v2 評測集，16 題）。這份表只證明目前 9 份文件/17 Chunk/16 題評測下的結果，不代表 Reranker 在所有資料集都最好，適合做回歸基準，不是公開排行榜。

**端到端回答實測（本機 qwen3.5:9b，8 題）：** Status Accuracy 7/8、Citation Valid 7/8、Citation Precision 0.812、Reference Coverage 0.812。唯一狀態失敗不是知識錯誤，而是引用宣告欄位與正文引用不一致，系統選擇 **Fail Closed**，不交付來源鏈不一致的答案。

**拒答三層防線：** ①檢索證據充足度 ②模型輸出 `insufficient` 判斷 ③引用與格式驗證。拒答前置閘門保留 16/16 有答案題，擋下 9/10 無答案題；FastAPI `Depends` 問題以 0.144 穿過 0.10 門檻——這個反例讓團隊沒有把門檻硬調到 0.15，也沒有宣稱 Cross-encoder 分數等於可回答機率。

**「Agentic」的具體邊界（六個受限決策）：**
1. 問題需要改寫嗎？移除識別字時回退原句
2. Follow-up 需要補回哪個主題？歷史只協助解析，不當證據
3. 候選證據足夠嗎？不足就在生成前拒答
4. 模型輸出符合引用契約嗎？不符合就 Fail Closed
5. 來源含明確注入語句嗎？命中就隔離，全部隔離則拒答
6. 新文件可以立即回答嗎？先進 Staging，審核與重建索引後才上線

**安全測試：** 離線測試中惡意來源命中兩條注入規則並被隔離，偽造來源 `S999` 被引用驗證器拒絕，合法 `S1` 通過——這只是基本防線，不等同通過紅隊測試，正規表示式會漏掉變形攻擊。

**明確列出的成果邊界（尚未驗證）：** 知識庫規模小、評測集小且缺雙人標註、回答實測僅單一本機模型單次執行、Context 預算仍以字元近似（未用生成模型 Tokenizer）、Prompt Injection 防禦僅基礎規則、文件 API 只進 Staging 未做背景審核與版本切換、API 無認證/授權/Rate Limit/Audit Log，不能公開暴露。

## C（關鍵行動）

- [ ] 設計 RAG 拒答機制時採三層防線（檢索證據充足度／模型 insufficient 判斷／引用格式驗證），不要只靠單一門檻分數
- [ ] 引用驗證做成程式可驗證的契約：模型只能選擇 Context 中存在的 Marker，來源名稱與連結由程式映射，引用鏈斷裂就 Fail Closed 不交付
- [ ] 把「Agentic」限定在有邊界、可記錄、可回退的具體決策點上，而非放任模型自由規劃與任意呼叫工具
- [ ] 任何效能數字（Hit@1、Citation Precision 等）都要同時公開資料規模與評測集大小，避免被誤用為通用基準
- [ ] 明確列出「尚未驗證的邊界」作為報告的一部分，而不是只呈現做得到的部分

## 原始摘要

「讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理」系列完結篇（Day 30），總結從資料清理、BM25/Embedding 混合檢索、Reranker、LLM 生成、引用驗證到拒答機制的完整 Pipeline，列出實測數據與尚未驗證的邊界。作者強調此處的「Agentic」是一組有邊界、可記錄、可回退的受限決策，而非自由規劃、任意呼叫工具的通用 Agent。

## 相關連結

- [[rag-retrieval-augmented-generation]] — RAG 通用概念條目，本篇提供具體實測指標（Hit@1/Recall@3）與拒答三層防線的案例補充
- [[enterprise-rag]] — 企業級 RAG 架構參考，可與本篇的 Staging 審核流程、API 安全邊界對照
- [[chinese-rag-preprocessing-baseline]] — 中文 RAG 前處理基準，與本篇「繁體中文 Markdown 清理與結構化 Chunk」步驟屬同類實務基線
- [[rag-api-fastapi-contract-staging-deployment]] — **系列上一篇（Day 29）**：本篇完結篇總結的 API 服務層，正是該篇 FastAPI 契約與 Staging 審核邊界的具體實作
