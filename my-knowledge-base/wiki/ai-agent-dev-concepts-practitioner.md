---
title: AI Agent 開發實踐者指南：概念、工具設計、沙箱與開發流程
date: 2026-06-24
tags: [ai/agent, dev/architecture]
source: https://codelove.tw/@tony/post/qenpnx
type: article
status: processed
asset_value: 82
---

AI Agent 不是「讓 SDK 幫你呼叫 API」——它是把「模型 + 工具 + 記憶 + 推理 + 回饋循環」有規劃地組成的工程化系統。SDK 提供原材料，但業務閉環、狀態管理、安全護欄都要開發者自己設計。

## T（核心論點）

**先定義任務邊界，再選框架**——很多人一上來就問「用 LangGraph 還是 OpenAI SDK」，但重要的是你的 Agent 需要完成什麼任務、需要什麼能力、允許什麼權限。

**Workflow vs Agent 選擇判斷**：

| 面向 | Workflow（固定流程）| Agent（動態流程）|
|------|-------------------|----------------|
| 路徑 | 每步固定（發票OCR→提取金額→寫表格）| 不確定（依結果調整下一步）|
| 驗收 | 規則明確 | 需要判斷標準 |
| 適合 | 重複、可預期的結構任務 | 多步決策、根據環境回饋調整 |

> Anthropic 建議：**能用 Workflow 解決的不要硬上 Agent**，只有任務路徑不確定、需要多步決策時，Agent 才真正有價值。

## B（適用場景）

適合 Agent 的任務三特徵：
1. **多步完成**：任務需要搜尋→閱讀→整理→生成→核對
2. **路徑不固定**：根據中間結果調整策略
3. **有明確成功標準**：沒有可驗收條件，Agent 無法判斷何時停止

## R（五大核心模組）

### 工具（Tools）

工具不是 `add_tool(search)` 就結束：
- **設計可靠介面**：參數 schema、回傳值結構、錯誤處理
- **處理髒資料/失敗 fallback**：API 超時、格式變化、限流
- **工具原則**：先讓 Agent 讀，再讓 Agent 寫，最後才讓 Agent 執行高風險動作
- **工具數量**：工具越多，模型選錯的機率越高，權限風險越大

### 記憶（Memory）

- **短期**：當前對話歷史與思考過程
- **長期**：向量資料庫（Embedding 為核心）+ 混合檢索（向量+BM25+RRF）
- Embedding 選擇影響準確率，不同語言場景能力有差異

### 推理（Reasoning）

- **ReAct**：Thought→Action→Observation 循環（最基本）
- **Plan-and-Execute**：先整體規劃再逐步執行（適合複雜多步任務）
- **Reflection**：執行後自我批評改進
- **Multi-Agent**：角色分工合作（但通訊成本高、除錯困難，先跑通單 Agent 再升）

### 狀態管理與工作流程

Agent 需要記錄每步狀態（思考/工具呼叫/結果）+ 支援循環/持久化/人工確認（HITL）。沒有良好狀態管理，AI 容易縫縫補補埋地雷。

### 沙箱（Sandbox）

程式碼 Agent、瀏覽器 Agent、檔案操作 Agent 都高度依賴沙箱，否則是「裸奔」：

| 沙箱類型 | 隔離強度 | 適用 |
|---------|---------|------|
| Docker Container | 中 | 本地開發 |
| microVM | 高 | 生產環境（比容器更隔離、比傳統 VM 更輕）|
| E2B / Modal / Daytona | 雲端 | 雲端 Agent Sandbox 平台 |

沙箱必須隔離：檔案系統、程序、資源、金鑰（短期 token）、命令攔截，並支援快照回滾。

## C（九步開發流程）

1. **定義任務**：先確認 Agent 要做什麼（邊界 > 智慧）
2. **能力清單**：把任務拆成具體能力模組
3. **設計最小必要工具**：逐步開放，每工具邊界明確不重疊
4. **設計 Agent 迴圈**：`while not done` + 最大步數/成本/工具超時/失敗重試
5. **RAG 與知識庫**：Agentic RAG 支援多輪檢索、換關鍵字、判斷資料不足再繼續查
6. **加入人工確認（HITL）**：執行前確認/不確定時確認/結果審閱
7. **錯誤處理**：超時重試/換關鍵字/降級為一般回答/超過步數停止並總結
8. **加護欄**：輸入/輸出/工具呼叫/Prompt injection（外部內容只能當資料不能當指令）
9. **可觀測性**：記錄 trace（思考/工具呼叫/回傳/耗時/Token）

## SDK 選型參考

| SDK | 特點 | 適用 |
|-----|------|------|
| **LangGraph** | 底層、狀態化、長任務，持久化+HITL 最強 | 複雜狀態控制、生產環境 |
| **OpenAI Agents SDK** | 簡單任務最快上手，handoff 任務移交 | OpenAI 生態、客服轉接場景 |
| **Google ADK** | Google Cloud/Gemini 生態原生 | GCP 部署、監控觀測 |
| **Genkit** | 多語言（含 Dart），App 整合好 | Flutter/Dart 專案、輕量場景 |
| **CrewAI** | 角色化多代理快速搭建 | 原型、中小專案 |

## 相關連結

- [[ai-agent-architecture-patterns]] — 四大架構模式（Workflow/ReAct/Plan&Execute/Dual-Agent）
- [[ai-agent-architecture-10-modules]] — Agent 10 個核心模組詳解
- [[agentic-engineering-sop]] — 六階段開發 SOP（Spec/Plan/Tasks 三層落檔）
- [[langchain-langgraph-basics]] — LangGraph 具體實作
- [[ai-agent-forbidden-tasks-agents-md]] — Agent 不能交辦的 6 件事與權限矩陣
- [[concepts/agent-workflow-taxonomy]] — Agent 工具六大分類地圖
