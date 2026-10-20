---
title: Firecrawl：幫 AI Agent 裝上網路眼睛的爬取基礎設施
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://vocus.cc/article/6a194068fd8978000190b7b8
type: article
status: processed
asset_value: 55
---

LLM 自己讀網頁常遇到三個痛點：JS 動態渲染抓不到內容、原始 HTML 太雜訊浪費 token、結構化資料需要額外解析。Firecrawl 把「網頁→LLM 可用內容」這段基礎設施標準化，讓 AI Agent 能穩定地把網路當作可靠的資料來源。

## T（核心論點）

Firecrawl 是專為 LLM/Agent 設計的網頁抓取服務，核心價值是把「爬蟲」與「資料清理」合併成一支 API：自動處理 JS 渲染、用自然語言描述要抽取的結構化欄位（`/extract`）、輸出乾淨的 LLM-ready Markdown，官方宣稱可比傳統爬蟲+清理流程省下約 10 倍 token。v2.9+ 新增 `/interact`（瀏覽器操作）與 `/agent`（自主多步驟）端點，並提供 MCP 整合供 Claude Code 直接呼叫。

## B（適用場景）

**適用：**
- Agent 需要即時抓取外部網站內容作為上下文（新聞、文件、商品頁）
- 目標網站大量依賴 JS 渲染（SPA、無限捲動）
- 需要從非結構化網頁中抽取結構化欄位（價格、規格、聯絡資訊）

**不適用：**
- 已有穩定 API 可直接取得資料的來源（沒必要多一層爬取）
- 純靜態、簡單 HTML 且自行解析成本極低的場景

## R（核心能力與實測）

### 三項核心能力

| 能力 | 說明 |
|------|------|
| JS 渲染 | 自動執行 JavaScript，取得 SPA/動態網站完整內容 |
| 自然語言結構化抽取（/extract） | 用一句話描述要抓的欄位，無需手寫 selector |
| LLM-ready Markdown 輸出 | 自動去除導覽列/廣告等雜訊，輸出乾淨 Markdown |

### Token 節省實測

一行 API 呼叫取代「請求網頁 + BeautifulSoup 解析 + 手動清理雜訊 + 格式化」整套流程，官方數據顯示輸出 token 量可降至傳統方式的約 1/10，主因是雜訊（導覽列、廣告、重複樣板）被在伺服器端先行濾除。

### 三個實戰場景（Playbook）

1. **新聞/部落格摘要 Agent**：抓取文章 Markdown 後直接餵給 LLM 摘要，不需自建解析器
2. **競品資訊監控**：用 `/extract` 描述「價格」「庫存狀態」等欄位，定期抓取結構化資料做比對
3. **MCP 整合進 Claude Code**：透過 MCP server 讓 Claude Code 在對話中直接呼叫 Firecrawl 抓取與分析網頁

### v2.9+ 新端點

- `/interact`：模擬瀏覽器操作（點擊、輸入、滾動），處理需要互動才能載入內容的頁面
- `/agent`：自主多步驟任務（例如「找出這個網站所有產品分類並各抓一頁範例」），由 Firecrawl 自行規劃抓取路徑

### 與傳統爬蟲方案比較

| 維度 | 傳統爬蟲（requests+BS4/Selenium） | Firecrawl |
|------|------------------------------|-----------|
| JS 渲染 | 需自行整合 Selenium/Playwright | 內建支援 |
| 結構化抽取 | 需手寫 selector/regex | 自然語言描述即可 |
| 輸出格式 | 原始 HTML，需自行清理 | 直接輸出 LLM-ready Markdown |
| 維護成本 | 網站改版易失效，需持續維護 selector | API 抽象掉版面變動細節 |

### 限制

- 對極度依賴複雜互動才能解鎖的內容（多步驟登入、驗證碼）仍需 `/interact` 額外編排，非單次呼叫可解決
- 屬於第三方付費服務，需評估長期呼叫成本與資料隱私邊界

## C（關鍵行動）

- [ ] Agent 需要抓網頁內容時，優先評估用 Firecrawl 取代自建 requests+解析流程，降低維護成本與 token 消耗
- [ ] 結構化資料抽取改用 `/extract` 自然語言描述欄位，而非手寫 selector，減少網站改版造成的維護負擔
- [ ] 在 Claude Code 中透過 MCP 整合 Firecrawl，讓研究/資料蒐集任務可直接在對話中觸發抓取

## 相關連結
- [[open-source-web-scraping-tools-2026]] — 6 個開源替代方案對照（URL→Markdown 到會自我修復的爬蟲），可與本篇的商用服務對比選型
- [[supervisor-multi-agent-langchain]] — Multi-Agent 系統中可將 Firecrawl 包成工具供 Agent 呼叫
- [[puppeteer-browser-automation]] — Puppeteer：底層瀏覽器自動化（對照擷取方式）
