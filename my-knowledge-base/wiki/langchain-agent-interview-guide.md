---
title: LangChain / Multi-Agent / A2A 面試問答：Agent 開發全景
date: 2026-07-03
tags: [ai/agent, ai/mcp, type/concept]
source: https://codelove.tw/@tony/post/qvdeX3
type: article
status: processed
asset_value: 63
---

以阿里面試對話形式，串起 Agent 開發的核心知識：LangChain 三層架構、四種 Agent 開發方式、六大核心模組、Multi-Agent、A2A vs MCP、Transformer/Self-Attention、大模型訓練三階段。適合快速建立 Agent 技術全景與釐清常混淆概念。

## T（核心論點）

Agent 開發的知識版圖可拆成：編排框架（LangChain/LangGraph）→ 開發範式（ReAct/Plan-Execute/Multi-Agent/狀態圖）→ 通訊協議（A2A Agent 對 Agent、MCP Agent 對工具）→ 底層原理（Transformer/訓練）。釐清「A2A 解決協作、MCP 解決能力擴展」是關鍵分野。

## B（適用場景）

- 準備 Agent/LLM 相關技術面試
- 想快速釐清 LangChain vs LangGraph、A2A vs MCP、RLHF vs DPO 等易混概念
- Java 技術棧做 Agent（Spring AI + LangGraph4j）

## R（主要內容）

**LangChain 三層架構**：基礎抽象層（LLM/ChatModel/Prompt/OutputParser）→ 能力層（Chain/Agent/Memory/Retriever）→ 應用層（LangServe 部署/LangSmith 監控/LangGraph 狀態圖）。LangChain 的 Chain 是線性 A→B→C；**LangGraph 升級成圖式**（節點+條件邊+循環），Java 版 LangGraph4j 底層 StateGraph。

**四種 Agent 開發方式**：ReAct（推理↔執行交替，LangChain Agent 就是）、Plan-and-Execute（先規劃後執行，不易走彎但難調整）、Multi-Agent 協作（多角色訊息傳遞，AutoGen/CrewAI）、狀態圖編排（LangGraph，最可控）。

**LangChain 六模組**：Models（統一封裝各家模型）、Prompts（範本變數/Few-Shot）、Indexes（RAG：Loader/Splitter/VectorStore/Retriever）、Memory（BufferMemory 短期 + VectorStore 長期）、Chains（LLMChain = Prompt+Model+OutputParser）、Agents（自主決策選工具）。

**A2A vs MCP**：
- **A2A**（Google）：Agent 對 Agent。每個 Agent 一張「能力名片」（Agent Card，JSON），透過 HTTP API 通訊、Task 為協作單元；解決跨團隊/跨框架 Agent 互操作。
- **MCP**：Agent 對工具。MCP Server 用 JSON Schema 暴露能力，Agent 透過 Client 發現呼叫；換底層實作 Agent 端零改動。

**Transformer**：Encoder（理解輸入）+ Decoder（逐 token 生成）；Self-Attention 讓每位置直接看到所有位置且可平行（解決 RNN/LSTM 長序列衰減+無法平行）。關鍵元件：位置編碼（正弦/RoPE）、LayerNorm、FFN（大部分參數在此）。Self-Attention = softmax(Q·Kᵀ/√d)·V；Multi-Head 平行多組捕捉不同關聯模式。

**訓練三階段**：Pre-training（自監督預測下一 token，算力最大）→ SFT（指令-回答有監督微調）→ RLHF（獎勵模型 + PPO）。**DPO** 砍掉獎勵模型，用偏好資料對直接優化，兩步併一步、更穩定。

## C（對自身的啟發）

- [ ] A2A（Agent 協作）vs MCP（工具擴展）的分野，釐清本 kit 的 MCP 定位
- [ ] 四種開發範式對照 production-agent-architecture 的 FSM/Planner-Executor
- [ ] Java 棧（Spring AI + LangGraph4j）路線，供 Spring Boot 背景參考

## 相關連結

- [[langchain-langgraph-basics]] — LangChain × LangGraph 基礎
- [[a2a-agent-protocol-implementation]] — A2A 協議實作
- [[langchain-mcp-integration]] — LangChain MCP 整合
- [[ai-agent-architecture-patterns]] — 四大 Agent 架構模式
- [[production-agent-architecture]] — 生產級 Agent 工程架構
- [[chain-of-thought-reasoning]] — CoT 推理
