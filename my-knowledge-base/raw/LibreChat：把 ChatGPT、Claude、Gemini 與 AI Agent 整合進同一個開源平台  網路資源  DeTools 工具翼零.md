---
title: "LibreChat：把 ChatGPT、Claude、Gemini 與 AI Agent 整合進同一個開源平台 | 網路資源 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3957"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-06-24
created: 2026-06-25
description: "LibreChat 是開源 AI 對話平台，可整合 ChatGPT、Claude、Gemini、Ollama 等模型，支援 AI Agent、RAG 知識庫、MCP 工具與自架部署，打造可完全掌控的企業級 AI 工作環境。"
tags:
  - "clippings"
status: processed
---
LibreChat 是一套功能完整的開源 AI 對話平台，目標是提供類似 ChatGPT、Claude 或 Gemini 的聊天體驗，同時讓使用者能完全掌控模型、資料與部署環境。相較於直接使用各家 AI 服務，LibreChat 更像是一個統一入口（Unified AI Interface），能將多種大型語言模型整合到同一個介面中，並支援進階功能如 AI Agent、檔案分析、RAG 知識庫、MCP 工具調用與多模型切換。

在生成式 AI 快速發展的背景下，許多團隊同時使用 OpenAI、Anthropic、Google 或開源模型。然而不同平台有不同介面、權限管理方式與工作流程，造成使用體驗分散。LibreChat 正是為了解決這個問題而誕生，希望打造一個可自行部署、可高度客製化的 AI 工作平台。

從產品定位來看，LibreChat 並不是另一個大型語言模型，而是一套 AI 前端與協作平台。它負責整合模型、管理對話、處理工具調用與提供使用者介面，而實際推理能力則來自後端串接的模型服務。

其最大的特色之一，就是支援多種主流 AI 模型。

目前可整合的模型與服務包括：

- ChatGPT
- Claude
- Gemini
- Azure OpenAI Service
- Ollama
- OpenRouter
- 各類相容 OpenAI API 的模型服務

使用者可以在同一個介面中切換不同模型，而不需要反覆登入不同平台。

從功能面來看，LibreChat 提供的能力遠超過一般聊天介面。

主要功能包括：

- 多模型整合
- ChatGPT 類似對話介面
- AI Agent 支援
- 檔案上傳與分析
- 多模態對話
- RAG 知識庫整合
- MCP（Model Context Protocol）支援
- 工具調用（Tool Calling）
- 多使用者管理
- 團隊協作功能
- 自架部署能力
- 對話歷史管理

這些功能讓它更像是一個 AI 工作平台，而非單純聊天工具。

其中最受到關注的是 Agent 功能。

近年 AI 發展逐漸從聊天機器人走向 Agent 系統，而 LibreChat 也積極跟進這個趨勢。

透過 Agent 模式，模型可以：

- 呼叫外部工具
- 搜尋文件
- 存取知識庫
- 執行多步驟任務
- 與 MCP Server 整合

因此不只是回答問題，而能實際協助完成工作流程。

對企業應用與進階使用者來說，這項能力尤其重要。

另一項特色是 MCP（Model Context Protocol）支援。

隨著 MCP 成為 AI 工具整合的重要標準，LibreChat 已支援與各種 MCP Server 連接。

例如：

- 文件搜尋
- GitHub 操作
- 資料庫查詢
- 瀏覽器控制
- 自訂企業工具

都能透過 MCP 擴展。

這讓 LibreChat 從單純聊天介面進一步成長為 AI Agent Hub。

在知識管理方面，LibreChat 也提供 RAG（Retrieval-Augmented Generation）能力。

使用者可以上傳：

系統會建立索引並在對話時檢索相關資訊。

對於：

等場景相當實用。

從部署角度來看，自架能力是 LibreChat 最重要的優勢之一。

許多企業因資料隱私與法規要求，不適合直接使用雲端 AI 服務。

LibreChat 支援：

因此能在保留資料控制權的情況下導入 AI 能力。

整體而言，LibreChat 是目前最成熟的開源 AI 對話平台之一。它透過統一介面整合多家模型服務，並進一步加入 Agent、RAG、MCP 與企業部署能力，讓使用者能建立屬於自己的 AI 工作環境。對於希望擺脫單一供應商限制、建立可控且可擴充 AI 平台的團隊而言，LibreChat 提供了一套兼具功能深度與開源彈性的解決方案。