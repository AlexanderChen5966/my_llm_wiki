---
title: AnythingLLM + Ollama 本地 RAG 實作筆記
date: 2026-06-05
tags: [ai/workflow]
source: https://vip.studycamp.tw/t/《筆記》以開源-llm（anythingllm-ollama）實作-rag/8501
type: article
status: processed
asset_value: 48
---

## T（核心論點）

以 AnythingLLM + Ollama + LanceDB 建立 100% 本地、完全免費且保護隱私的 RAG 應用。RAG 的靈魂在於分塊（Chunking）——即使模型支援 1M token 上下文，仍應分塊處理以控制成本與延遲。講師建議手動將 PDF 轉為 Markdown 再上傳，比使用內建爬蟲更能確保嵌入品質。AnythingLLM 適合 RAG「入門」，但 Agent Skills / Agent Flows 不穩定，進階應轉向 LangChain / LangGraph / Flowise。

## B（適用場景）

**適用：** RAG 概念學習與原型驗證、個人知識庫問答（文件總結效果好）、隱私敏感場景的本地方案
**不適用 / 注意：** 不適合大型專案的生產環境、Agent 功能不穩定、Dolphin 等未審查模型不應用於公開聊天機器人

## 原始摘要

課程筆記涵蓋：Ollama 硬體/模型/量化選擇（VRAM 為關鍵限制、Q4 為最佳平衡點、GGUF 為標準格式）、AnythingLLM 設定（Ollama + LanceDB + 內建 Embedder 的 Local-First 堆疊）、分塊策略（長篇 1000-5000 / 短篇 500-1000 / 列表 100-500 tokens，overlap 1-5%）、關鍵參數（Top-K 4-8 / 相似度門檻 0.25-0.75 / 溫度依用途調整）、Dolphin 模型特性（未審查、高服從性，適合嚴格 RAG 指令遵循）。進階工具展望：MCP / LangChain / LangGraph / Flowise / KNIME。

## 相關連結

- [[ollama-2026-complete-guide]] - Ollama 2026 完整教學
- [[ollama-local-llm-setup]] - Ollama + Mac mini M4 自架小型 LLM
- [[local-llm-deployment-overview]] - 本地 LLM 部署概覽
- [[enterprise-rag]] - 企業私有知識庫 RAG 實作
- [[llm-cost-optimization-strategies]] - LLM 成本優化四層策略
- [[context-engineering-fundamentals]] - Context Engineering 三部曲
