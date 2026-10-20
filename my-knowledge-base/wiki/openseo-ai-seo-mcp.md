---
title: OpenSEO：把 SEO 分析封裝成 MCP + AI Skills 的開源工具
date: 2026-07-03
tags: [ai/agent, ai/mcp, type/concept]
source: https://tools.wingzero.tw/article/sn/3966
type: article
status: processed
asset_value: 55
---

OpenSEO 是專為 AI Agent 打造的開源 SEO 工具，把關鍵字研究、SERP 分析、排名追蹤、網站健檢、競對分析封裝成可直接安裝的 **AI Skills**，並內建 **MCP Server**，讓 Claude Code、Codex 等 Agent 直接呼叫真實 SEO 資料完成分析。是 Semrush / Ahrefs 的開源替代。

## T（核心論點）

LLM 本身無法取得即時 SEO 數據。OpenSEO 的核心價值是把 SEO 能力封裝成 AI 可直接呼叫的技能（Skills）+ MCP 工具，讓模型不只給建議，而是能取得真實資料並完成分析——把重複性 SEO 工作交給 Agent 自動化。

## B（適用場景）

- SEO 顧問、數位行銷團隊、技術 SEO 工程師
- AI Agent 開發者、想建 AI SEO Workflow 的企業、Indie Hacker
- 規劃新網站內容（自動關鍵字研究+SERP）、營運期定期追排名/競對/產報告

## R（主要內容）

**定位**：不是單純網站分析工具，而是完整 AI SEO 工具，支援自架部署，透過 MCP Server 提供給各種支援 MCP 的 Agent。

**功能**：關鍵字研究、SERP 分析、排名追蹤、網域分析、競對分析、反向連結分析、網站健檢、MCP Server 整合、AI Agent Skills、自架部署。

**兩大特色**：
- **內建 Skills 系統**：官方把常見 SEO 工作封裝成可直接安裝的 Skills，Agent 不需每次重設計 Prompt 或查詢流程即可執行關鍵字分析、跨站表現比較、競對資料整理。
- **MCP 支援**：內建 MCP Server，Agent 需查關鍵字/分析網站/取排名時直接經 MCP 呼叫，貼近即時資料而非模型推測。

**成本**：開源自架、依實際使用量選資料來源；SEO 資料主要來自第三方供應商，查詢成本依串接服務而定（非固定訂閱）。

## C（對自身的啟發）

- [ ] 「把領域能力封裝成 Skills + MCP 供 Agent 呼叫」是本知識庫 kit 的同一設計哲學（對照 paper-discovery 經 MCP 搜尋）
- [ ] Skills（本地封裝工作流）+ MCP（連即時外部資料）的分工，呼應 agent-skills-vs-mcp
- [ ] 開源自架 + 第三方資料按量計費的模型，可作為工具選型參考

## 相關連結

- [[agent-skills-vs-mcp]] — Skills vs MCP 分工
- [[mcp-fastmcp-server]] — MCP Server 建置
- [[firecrawl-web-scraping-for-agents]] — 為 Agent 設計的網頁擷取
- [[trek-self-hosted-travel-planner]] — 自架應用內建 MCP 供 Agent 操作（同類案例）
