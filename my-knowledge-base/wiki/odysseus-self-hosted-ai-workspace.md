---
title: Odysseus：PewDiePie 開源自架 AI 工作區
date: 2026-06-15
tags: [ai/agent, pkm/workflow]
source: https://github.com/PewDiePie/odysseus
type: reference
status: processed
asset_value: 65
---

Odysseus 是 PewDiePie 發布的開源自架 AI 工作區，MIT 授權，3 天內突破 42K GitHub Stars。核心理念：**隱私優先，無遙測，所有資料留在本地**。使用 JavaScript 撰寫，Docker 一行指令啟動。

## T（核心論點）

Odysseus 的定位是「個人 AI 作業系統」：把 Chat / Agent / 研究 / 文件 / 日曆 / Email 整合在一個本地自架平台，而非依賴多個分散的 SaaS 工具。

## B（適用場景）

**適用：**
- 需要隱私優先（不想資料上傳到雲端 SaaS）
- 想整合多個 LLM 後端（OpenAI / Anthropic / Ollama）到單一介面
- 需要自架 AI 研究/文件管理工作區

**不適用：**
- 需要企業級 SSO 或合規功能
- 不熟悉 Docker 的使用者（有一定技術門檻）

## R（功能模組總覽）

### 快速啟動

```bash
docker compose up -d --build
# → localhost:7000
```

### 核心功能模組

| 模組 | 功能 |
|------|------|
| **Chat** | 連接任意 LLM 後端（OpenAI / Anthropic / Ollama 等）|
| **Agent** | 基於 opencode，具備工具使用能力的 AI Agent |
| **Cookbook** | 270+ 模型選擇，自動硬體偵測（GPU/CPU）|
| **Deep Research** | 多步驟深度研究（來自 Tongyi 研究功能）|
| **Compare** | 多模型對比輸出 |
| **Documents** | Markdown / HTML / CSV 文件管理 |
| **Memory + Skills** | ChromaDB + fastembed 語意記憶，跨 session 持久化 |
| **Email** | IMAP / SMTP 整合 |
| **Notes + Tasks** | 本地筆記與任務管理 |
| **Calendar** | CalDAV 協定支援 |
| **Mobile** | PWA 手機版 |

### 技術特性

- **隱私設計**：無遙測，本地優先
- **Memory**：ChromaDB + fastembed（語意搜尋）
- **Agent**：opencode-based（非自製）
- **語言**：JavaScript

## C（關鍵行動）

- [ ] 評估是否用 Odysseus 取代目前的多工具組合（Chat/研究/文件分散）
- [ ] 比較 Odysseus vs AnythingLLM：兩者都有本地 RAG，但 Odysseus 更全面

## 相關連結

- [[anythingllm-ollama-rag-local]] — AnythingLLM 本地 RAG（功能子集，更輕量）
- [[local-llm-deployment-overview]] — 本地 LLM 部署方案對比
- [[ollama-2026-complete-guide]] — Ollama 設定（Odysseus Cookbook 的底層）
- [[hermes-agent]] — Hermes Agent（另一個開源 AI 工作區方案）
