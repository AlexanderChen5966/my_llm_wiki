---
title: 用 AWS AgentCore 低程式碼建構 RAG 與 AI Agent（實作教學）
date: 2026-07-03
tags: [ai/agent, ai/mcp, type/how-to]
source: https://codelove.tw/@tony/post/anbXGa
type: article
status: processed
asset_value: 50
---

30 分鐘～1 小時的低程式碼實作，用 AWS Bedrock + AgentCore 建出「RAG 知識庫 + AI Agent + 前端」全套。關鍵亮點：**受管 KB 背後的 LLM 與嵌入模型由 AWS 管理，即使新帳號 Bedrock 配額為 0 也能用**；Agent 模型走 Mantle（Bedrock 新基礎架構）繞過配額限制。

## T（核心論點）

AgentCore 把「RAG 管線 + Agent Runtime + Gateway 工具連接」封裝成主控台可視化流程，用 Strands Agents + AgentCore Runtime 組合，低程式碼即可部署可用的檢索增強 Agent，並用 Streamlit + Cloudflare Tunnel 快速接前端。

## B（適用場景）

- 想快速體驗 AWS 上的 managed RAG + Agent（企業內部文件問答）
- 新 AWS 帳號 Bedrock 配額 0、無 Frontier Model 權限，仍想動手
- 評估 AWS AgentCore（Gateway/Harness/Runtime）作為 Agent 平台

## R（實作流程）

**RAG 編**：S3 建 bucket 上傳內部文件 → Bedrock 主控台 建置>知識庫>Create Managed KB → 選 S3 來源 → 自動建 RAG 並首次同步（受管 KB 的 LLM/嵌入由 AWS 託管，繞過配額 0）。

**AI Agent 編**：
- **AgentCore Gateway**：連接 Agent 與 RAG，Inbound Auth 選 IAM 權限、目標類型 Connectors 選 KB。
- **AgentCore Harness**：部署 Strands Agents + AgentCore Runtime，API 來源選 Bedrock **Mantle**（繞過配額）、模型選 GLM 5（Claude/GPT 通常未授權），啟用工具>閘道（自動配好 IAM 政策）。
- 測試：KB「測試知識庫」、Harness「測試 Harness」驗證 RAG 與 Agent+RAG。

**瀏覽器工具**（選配，會暴增 token）：Harness Playground 加瀏覽器工具，可讓 Agent 一邊讀畫面一邊操作。

**前端**：CloudShell 寫 Streamlit `frontend.py`（boto3 `invoke_harness` 串流），`streamlit run` + Cloudflare Tunnel（`cloudflared tunnel --url http://localhost:8501`）對外公開，貼 Harness ARN 即可對話。

**收尾**：受管 KB 有儲存計費，做完要輸入 "delete" 刪除。

## C（對自身的啟發）

- [ ] AgentCore 的 Gateway（Agent↔工具/RAG）概念，對照 MCP 的工具連接抽象
- [ ] 「受管 KB 繞過配額」是無 GPU/新帳號體驗 RAG 的實用路徑
- [ ] 全伺服器無狀態架構，費用主要是 LLM 推論 token——成本模型清楚

## 相關連結

- [[production-agent-architecture]] — 生產級 Agent 架構（RAG/Gateway 對照）
- [[enterprise-rag]] — 企業級 RAG
- [[rag-retrieval-augmented-generation]] — RAG 基礎
