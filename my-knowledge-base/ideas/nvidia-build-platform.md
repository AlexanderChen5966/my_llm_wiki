---
title: NVIDIA Build：NVIDIA AI 模型與 Agent 開發平台
date: 2026-06-22
tags: [ai/agent]
source: https://tools.wingzero.tw/article/sn/3951
type: reference
status: processed
asset_value: 38
---

NVIDIA Build 是 NVIDIA 的生成式 AI 開發入口，整合模型試用、API 存取、NIM 微服務下載和 Agent 開發工具到同一平台。

**核心概念 — NIM（NVIDIA Inference Microservices）：**
預先完成 CUDA 配置/TensorRT 最佳化的容器化推理服務，讓企業快速將模型投入正式環境，不需自行配置 GPU 環境。

**平台整合範圍：**
- LLM（Llama/Mistral/Gemma/Nemotron）、視覺、影片、語音模型
- RAG 解決方案、Agent Framework、Tool Calling、Multi-Agent Architecture
- 瀏覽器直接試用模型，不需本地部署

**適用對象：**
已使用 NVIDIA GPU 的企業，從 Build 上測試的成果能直接延伸到正式生產（NIM 微服務 → GPU 基礎設施）。
