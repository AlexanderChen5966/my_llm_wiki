---
title: "Anthropic Academy：Claude 官方 13 門線上課程免費上、可領官方證書"
source: "https://www.koc.com.tw/archives/645273"
author:
  - "[[達小編]]"
published: 2026-06-08
created: 2026-06-10
description: "Anthropic Academy 的課程數量已從最初的 13 門持續擴充，與其全部掃過，不如依角色選擇："
tags:
  - "clippings"
status: processed
---
你還在為了學習 Claude 花錢去報名各種課程嗎？很多人不知道 Anthropic 其實在 2026 年 3 月就已經推出 **Anthropic Academy** ，將過去散落在部落格、文件與 GitHub 的學習資源整合為單一平台。13 門課程全數免費、自學制，完成後可獲得官方結業證書。從完全不懂 AI 的一般使用者，到需要建構 Agent 系統的後端工程師，都能找到對應的學習路徑。  

這不只是「又一個線上課程網站」，Anthropic 把自家產品生態系（Claude API、Claude Code、MCP、Cowork）的使用方法全部打包，搭配 Skilljar 平台管理進度與發放證書，目標很明確：降低 Claude 的進入門檻，讓更多人從「聽過 Claude」變成「會用 Claude 做事」。

### 課程分兩大族群：一般使用者 vs 開發者

13 門課程依技術門檻分為兩組：6 門不需要寫程式，適合所有人；7 門針對開發者，需要 Python 或 CLI 經驗。  

### 一般使用者路徑（6 門）

- [**Claude 101**](https://anthropic.skilljar.com/claude-101) ：入門首選。教你用 Claude 處理日常工作，寫信、整理資料、分析文件、草擬內容。不需要技術背景，核心是「怎麼跟 AI 對話才能得到有用的結果」。課程涵蓋 Projects、Artifacts、Skills、Connectors、Enterprise Search、Research Mode 等功能。
- [**Introduction to Claude Cowork**](https://anthropic.skilljar.com/introduction-to-claude-cowork) ：Cowork 是 Claude 直接操作你的檔案、資料夾與應用程式的功能。課程內容包含 Cowork 的任務迴圈（task loop）、Skills 與 Plugins、檔案與研究工作流程，以及如何將 Claude 擴展到 Chrome、Word 等應用中。
- **[AI Fluency 系列](https://anthropic.skilljar.com/ai-fluency-framework-foundations) （4 門）** ：與愛爾蘭科克大學教授 Joseph Feller 及瑞林藝術與設計學院教授 Rick Dakan 合作開發，圍繞「4E 框架」：Effective（有效）、Efficient（高效）、Ethical（合乎倫理）、Safe（安全）。
	- **Framework & Foundations** ：通用版本，教你思考框架而非按鈕操作——AI 的能力邊界在哪、哪些任務該交給 AI、什麼時候人類判斷不可替代
		- **For Students** ：學術研究、職涯規劃、學習策略
		- **For Educators** ：將 AI 融入教學、課程設計、評量策略
		- **Teaching AI Fluency** ：教別人怎麼用 AI，場景設計與學習成效評估

### 開發者路徑（7 門）

- [**Building with the Claude API**](https://anthropic.skilljar.com/claude-with-the-anthropic-api) ：開發者的必修課。從第一個 API 呼叫開始，一路到 Agent 系統架構。涵蓋 API 設定與金鑰管理、多輪對話與 System Prompt 設計、Prompt Engineering（XML 標籤結構、Few-shot Learning）、Tool Use（讓 Claude 呼叫你定義的外部函式）、RAG（讓 Claude 存取你的資料）、Extended Thinking 與 Prompt Caching，以及 Agent 架構（平行化、路由、多步驟編排）。
- [**Claude Code 101**](https://anthropic.skilljar.com/claude-code-101) ：Anthropic 的 Agentic 編碼工具入門。課程涵蓋安裝（終端機、VS Code、JetBrains、Claude Desktop、網頁版）、核准模式、Plan 模式、Explore→Plan→Code→Commit 工作流程、上下文管理策略，以及自訂指令。
- [**Claude Code in Action**](https://anthropic.skilljar.com/claude-code-in-action) ：進階實戰課程。聚焦於日常開發工作流整合——工具系統如何與你的程式碼庫互動、上下文管理策略、自訂指令（自動化重複任務）、MCP Server 整合（連接瀏覽器自動化等外部工具），以及 Thinking Mode vs Planning Mode 的使用時機。
- [**Introduction to Agent Skills**](https://anthropic.skilljar.com/introduction-to-agent-skills) ：Agent Skills 透過 Markdown 檔案（SKILL.md）擴展 Claude Code 的能力。課程教你 Skills 與 CLAUDE.md、Hooks、Subagents 的差異，從零開始撰寫 SKILL.md（frontmatter 格式、觸發條件描述），目錄結構設計（控制上下文視窗消耗），進階配置（限制工具存取、背景腳本），以及團隊共享與企業部署。
- **MCP 系列（2 門）** ：MCP（Model Context Protocol）是 Anthropic 推出的開放標準，讓 AI Agent 透過統一介面連接外部工具與資料來源。
	- [**Introduction to MCP**](https://anthropic.skilljar.com/introduction-to-model-context-protocol) ：用 Python 從零建構 MCP Server 與 Client，學習三大核心原語（Resources、Tools、Prompts），並使用 MCP Server Inspector 進行測試除錯
		- [**MCP: Advanced Topics**](https://anthropic.skilljar.com/model-context-protocol-advanced-topics) ：深入生產環境議題，適合需要部署 MCP Server 的工程師
- **雲端平台課程（2 門）** ：
	- [**Claude with Amazon Bedrock**](https://anthropic.skilljar.com/claude-in-amazon-bedrock) ：在 AWS Bedrock 上部署 Claude，涵蓋 Tool Use、RAG、Agent 架構
		- [**Claude with Google Vertex AI**](https://anthropic.skilljar.com/claude-with-google-vertex) ：在 GCP Vertex AI 上部署，額外涵蓋 Vision、PDF 處理、Citations

### 證書制度：課程結業 ≠ Claude 認證架構師

完成課程可獲得 Anthropic 官方結業證書，但要留意的是，這與 **Claude Certified Architect（CCA）** 是兩回事。Anthropic 在 2026 年 3 月 12 日宣布 CCA Foundations 認證，這是針對解決方案架構師與合作夥伴的專業證照，考試難度與課程結業完全不同。

簡單來說：課程證書證明你「上過課」，CCA 證明你「有能力設計生產級 Claude 應用」，兩者價值不同，所以如果有開課講師說他拿到 Anthropic 官方結業證書要問清楚是哪一種證書。

### 建議學習路徑：依角色選起點

Anthropic Academy 的課程數量已從最初的 13 門持續擴充，與其全部掃過，不如依角色選擇：

- **一般使用者** ：Claude 101 → AI Fluency Framework & Foundations → Claude Cowork。三門就夠日常使用
- **開發者（剛接觸 Claude Code）** ：Claude Code 101 → Claude Code in Action → Building with the Claude API → MCP 系列
- **開發者（已有 Claude Code 經驗）** ：直接從 Claude Code in Action 開始，再依需求補 API 與 MCP 課程
- **企業團隊** ：在上述基礎加上 Agent Skills 課程，以及對應的雲端平台課程（Bedrock 或 Vertex AI，看你用哪家）

### Anthropic Academy 官方頁面

### 小結

Anthropic Academy 的定位很清楚：用免費教育降低一般使用者進入 Claude 生態系的門檻，課程涵蓋的不只是「怎麼用 Claude」，而是整個開發者工具鏈，從 API 呼叫、Claude Code 編碼、MCP 外掛整合到雲端部署。這與 OpenAI 的 ChatGPT 教學、Google 的 AI Essentials 形成三方競爭格局，但 Anthropic 的課程深度明顯更高，特別是 MCP 與 Agent Skills 這兩塊，直接對應自家產品的差異化優勢。13 門免費課程加上官方證書，對個人學習者而言是零成本的 AI 技能投資，有興趣的朋友快去看看吧。