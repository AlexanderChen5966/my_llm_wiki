---
title: LibreChat：開源多模型 AI 對話平台
date: 2026-06-25
tags: [ai/agent, ai/mcp]
source: https://github.com/danny-avila/LibreChat
type: article
status: processed
---

## T（核心論點）

LibreChat 是開源的統一 AI 入口（Unified AI Interface）：把 ChatGPT、Claude、Gemini、Ollama 等多種模型整合進同一介面，並支援 Agent、MCP、Skills、Code Interpreter、多用戶管理與自架部署，定位為可完全掌控的企業/團隊 AI 工作平台。

## B（背景脈絡）

- GitHub：[danny-avila/LibreChat](https://github.com/danny-avila/LibreChat)，開源，Active 狀態
- 解決問題：多家 AI 服務（OpenAI/Anthropic/Google/開源模型）介面分散、統一管理困難
- 定位：AI 前端 + 協作平台，推理能力來自後端串接的模型服務

## R（重要細節）

### 支援的模型與服務

- Anthropic (Claude)、AWS Bedrock、OpenAI、Azure OpenAI、Google Vertex AI
- OpenAI Responses API、Ollama、Groq、Mistral、OpenRouter、Deepseek、Qwen
- 任何相容 OpenAI API 的服務（不需 proxy）

### 核心功能

**Agent & Tools**
- LibreChat Agents：無程式碼自訂 AI 助手，Agent Marketplace
- [Skills](https://www.librechat.ai/docs/features/skills)：可重用 `SKILL.md` 指令包（manual/automatic/always-on）
- [Subagents](https://www.librechat.ai/docs/features/subagents)：隔離子 agent，各自獨立 context window
- MCP（Model Context Protocol）工具整合

**Code Interpreter**
- 沙箱執行：Python, Node.js, Go, C/C++, Java, PHP, Rust, Fortran
- 由 [ClickHouse/code-interpreter](https://github.com/ClickHouse/code-interpreter) 驅動
- 本機執行，不外傳資料

**其他功能**
- Web Search（多搜尋提供者 + 內容抓取 + Jina 重排序）
- Code Artifacts（React/HTML/Mermaid 圖表直接在對話中產生）
- 圖片生成（GPT-Image-1 / DALL-E / Stable Diffusion / Flux）
- Resumable Streams（斷線自動重連，多 Tab 同步）
- 多語系 UI（含繁體中文）
- OAuth2 / LDAP / Email 多用戶驗證
- Admin Panel：瀏覽器端管理用戶/角色/設定

### 與個人知識庫的關聯

LibreChat 的 Skills 架構（`SKILL.md` 指令包）與 Claude Code Skills 設計理念相通，可互相參考；其 Subagents 機制呼應 Claude Code 的 Agent 委派模式。

### 自架部署

- Docker Compose 一鍵部署（含 Admin Panel）
- 支援 S3 + CloudFront（穩定媒體連結、邊緣傳遞）
- 可純本機或雲端部署

## C（行動清單）

- [ ] 評估是否有多模型統一管理需求（個人或團隊）
- [ ] 若需自架：查閱 [Docker 部署文件](https://docs.librechat.ai/)
- [ ] 研究 LibreChat Skills 架構與 Claude Code Skills 的異同

## 相關條目

- [[agent-skills-vs-mcp]] — Skills vs MCP 的選擇
- [[claude-code-plugins-guide]] — Claude Code Plugin 生態
- [[ai-skills-mcp-plugins-agents-comparison]] — 三種 AI 擴充模式比較
- [[codex-plugin-workflow]] — Codex Plugin 工作流對比參考

## 相關連結

- [[odysseus-self-hosted-ai-workspace]] — 另一套自架 AI 工作區（PewDiePie 開源），可與本篇對照選型