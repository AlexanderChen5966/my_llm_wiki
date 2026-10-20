---
title: Microsoft mcp-for-beginners：MCP 學習路徑與 2026-07-28 破壞性版本線
date: 2026-08-25
tags: [ai/mcp, type/how-to]
source: https://github.com/microsoft/mcp-for-beginners
type: reference
status: processed
asset_value: 64
---

## T（核心論點）

學 MCP 不必自己拼湊教材：Microsoft 這套 MIT 課程用「概念 → 建 server/client → 除錯與規模化 → 生產級資料庫實戰」四階段把路徑排好，六語言各有可跑的範例；而它最該被記住的一則情報不是課綱，是版本線——課程對齊 spec **2025-11-25**，而 **2026-07-28 RC 會把傳輸層改成 stateless 並廢棄 Roots / Sampling / Logging**，對既有 MCP server 是破壞性變更。

## B（適用場景）

**適用：**
- 要從零建 MCP server / client，且希望有官方 spec 對齊、跨語言（C# / Java / JavaScript / TypeScript / Rust / Python）的範例
- 正在盤點既有 MCP server 是否會被 2026-07-28 RC 打到（尤其用了 Roots / Sampling / Logging 的實作）
- 需要生產級參考架構：FastMCP + PostgreSQL、Row Level Security 多租戶、pgvector 語意搜尋、Azure Container Apps 部署

**不適用 / 注意：**
- 進階模組（5.1 / 5.12 / 5.13、Module 10、11 大半）綁 Azure 生態（Entra ID、Foundry Toolkit、Container Apps、Application Insights），不用 Azure 的人得自行換掉這一層
- 課程風格偏「初學者鼓勵式」，敘事鋪陳長，找特定答案時不如直接跳 Module 表對應的 Guide
- 倉庫含 50+ 語言翻譯，整包 clone 過大——務必用 sparse checkout
- 前置條件：至少一種語言的基本程式能力、client-server 與 REST/HTTP 概念；AI/ML 背景為選配

## R（操作步驟）

1. 用 sparse checkout 取得倉庫（避開翻譯目錄）：
   ```bash
   git clone --filter=blob:none --sparse https://github.com/microsoft/mcp-for-beginners.git
   cd mcp-for-beginners
   git sparse-checkout set --no-cone "/*" "!translations" "!translated_images"
   ```
2. 先讀版本線再讀課綱：Module 1.1「What's Changing in MCP (2026-07-28 RC)」——確認自己的 server 有沒有踩到 stateless transport 與三項 deprecation。
3. Foundation（Module 0–2）：00 概念總覽 → 01 核心概念 → 02 安全威脅與最佳實務。
4. Building（Module 3，共 15 個實作單元）：3.1 第一個 server → 3.2 client → 3.3 接 LLM 的 client → 3.4 VS Code 整合 → 3.5 stdio server → 3.6 HTTP streaming → 3.8 測試 → 3.9 部署 → 3.11 簡易 auth 與 RBAC → 3.12 MCP Hosts（Claude Desktop / Cursor / Cline 設定）→ 3.13 MCP Inspector 除錯 → 3.14 Sampling → 3.15 MCP Apps。
5. Growing（Module 4–5）：4.1 cursor-based pagination；5.14 Context Engineering、5.15 自訂 Transport、5.16 progress notification / cancellation / resource template、5.17 對抗式多 Agent 推理（兩個 agent 用同一組 MCP tool 各持一方，由 judge agent 評分）。
6. Mastery（Module 6–11）：Module 11 是 13 個 lab 的完整實戰路徑——01 架構 → 02 Row Level Security 多租戶 → 04 PostgreSQL schema → 05 FastMCP server → 06 tool 與 schema introspection → 07 Azure OpenAI + pgvector 語意搜尋 → 09 VS Code 整合 → 10 Docker / Azure Container Apps 部署 → 11 Application Insights 監控。
7. 找路用 `study_guide.md`（視覺化課綱地圖與分程度學習路線），追更新用 `changelog.md`。

## C（關鍵行動）

- [ ] 盤點既有 MCP server 是否使用 Roots / Sampling / Logging——這三項在 2026-07-28 RC 被 deprecate，且 transport 改為 stateless，遷移工作要提前排
- [ ] clone 前先用 sparse checkout 排除 `translations` 與 `translated_images`，否則下載量暴增
- [ ] 課程對齊的是 spec 2025-11-25（現行穩定版），照抄範例前先確認自己要對齊哪一版；MCP 用日期式版號（YYYY-MM-DD）

## 原始摘要

本剪報實際抓到的是 `microsoft/mcp-for-beginners` 的主 README（檔名指向的 13-mcp-inspector 子頁未落地）。MIT 授權開源課程，跨 C# / Java / JavaScript / TypeScript / Rust / Python 六語言。

**版本線（最關鍵情報）**：課程對齊 **MCP Specification 2025-11-25**（最新穩定版）。下一版 **2026-07-28** 的 release candidate 預定 2026-07-28 發布，四大變動為：(1) 傳輸層 stateless 化、(2) 正式化 Extensions 框架（MCP Apps、Tasks）、(3) 強化 authorization、(4) **deprecate Roots、Sampling、Logging**。MCP 採日期式版號以明確追蹤協定版本。

**四階段結構**：Foundation（Module 0–2，概念與安全）／Building（Module 3，15 個實作單元）／Growing（Module 4–5，除錯、多模態、Azure、規模化）／Mastery（Module 6–11，社群貢獻、早期採用心得、最佳實務、案例、Workshop、13-lab 資料庫實戰）。另有 Module 12 講 Copilot App 中的 MCP 使用。

**Module 11 實戰鏈路**：零售分析情境，FastMCP + PostgreSQL，含 Row Level Security 多租戶、pgvector + Azure OpenAI 語意搜尋、Docker / Azure Container Apps 部署、Application Insights 監控與效能／安全強化。

範例分兩級：Basic MCP Calculator（六語言）與 Advanced（含 Java with Spring 的 Container App 範例）。另有 `aka.ms/letslearnmcp-{csharp,java,javascript,python}` 影片系列。倉庫透過 GitHub Action 自動維持 50+ 語言翻譯同步。

## 相關連結

- [[microsoft-ai-agents-beginners-course]]
- [[mcp-stateless-architecture]]
- [[mcp-stateless-migration-state-ownership]]
- [[mcp-fastmcp-server]]
