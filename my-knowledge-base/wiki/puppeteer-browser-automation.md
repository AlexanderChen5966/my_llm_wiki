---
title: Puppeteer：用程式控制瀏覽器的自動化核心函式庫
date: 2026-06-30
tags: [dev/testing, ai/agent, type/concept]
source: https://tools.wingzero.tw/article/sn/3961
type: article
status: processed
asset_value: 54
---

Puppeteer 是開源 JavaScript 瀏覽器自動化函式庫，提供高階 API 透過程式控制 Chrome、Chromium 與 Firefox，執行網頁操作、自動化測試、資料擷取、截圖與 PDF 產生。源自 Chrome DevTools 團隊，是 Node.js 生態最具代表性的瀏覽器自動化工具之一，近年也成為 AI Agent 執行網頁任務的重要底層元件。

## T（核心論點）

現代網站大量用 React/Vue/Next.js，內容須等瀏覽器渲染後才出現，傳統 HTML 擷取工具難以應付。Puppeteer **直接與瀏覽器通訊**（透過 Chrome DevTools Protocol 與 WebDriver BiDi），而非模擬瀏覽器行為——能真正啟動瀏覽器、執行 JavaScript、等頁面載入，再點擊/填表/擷取，因此能完成幾乎所有使用者在瀏覽器中的日常操作。

## B（背景與適用場景）

- 自動化測試：開站→登入→點按→填表→驗證畫面，取代重複人工測試
- 網頁資料擷取（Web Scraping）：處理 JS 動態載入的現代網站
- AI Agent：需登入網站、搜尋、填表、完成線上操作的代理任務
- 適用族群：前端/全端、自動化測試工程師、QA、爬蟲與 Agent 開發者；熟 JS/TS 者上手門檻低

## R（主要內容）

**定位**：不是測試框架、也不是爬蟲工具，而是**瀏覽器控制函式庫**，可作為許多開發工具與服務的底層引擎。

**核心功能**：控制 Chrome/Chromium/Firefox、Headless 與 Headful 模式、自動操作網頁/填表、模擬滑鼠鍵盤、截圖、產生 PDF、擷取動態內容、監控網路請求、管理 Cookie/Session/瀏覽器狀態。

**CDP 進階能力**：模擬裝置尺寸/網路速度/CPU、攔截 HTTP Request、修改 Header、控制下載、擷取 Console Log——也常用於網站效能分析與除錯。

**常見應用**：自動產生 PDF 報表、SEO 頁面分析、網站縮圖、自動登入後台、社群平台自動化、批次下載整理。許多線上 PDF 服務與自動化平台以 Puppeteer 為核心。

## C（對自身的啟發）

- [ ] AI Agent 需要瀏覽器操作時，Puppeteer 是穩定的底層選項（對照 Claude in Chrome 的瀏覽器自動化）
- [ ] CDP 截圖/PDF 能力可用於把網頁素材自動存入 raw/
- [ ] 與 Firecrawl 對照：Puppeteer 偏底層可控、Firecrawl 偏為 Agent 封裝好的擷取服務

## 相關連結
- [[open-source-web-scraping-tools-2026]] — 開源擷取工具六選；⚠️ 該篇提醒「先查 DevTools Network」往往比寫爬蟲快
- [[firecrawl-web-scraping-for-agents]] — Firecrawl：為 Agent 設計的爬取
- [[fara-computer-use-agent]] — Computer Use Agent
