---
title: 開發者端的 Generative UI：A2UI 1.0 協定與 Open Intelligent UI 模板——畫面能動，不代表資料與動作可信
date: 2026-11-19
tags: [ai/agent, dev/architecture, type/concept]
source: https://www.aiposthub.com/google-a2ui-1-generative-native-ui-protocol/（aiposthub／Philo〈A2UI 1.0 正式推出：AI 用原生元件回答，Android 與 Gemini 有哪些版本限制？〉，2026-11-12；依 Google 2026-11-11 公告與官方 Quickstart 整理，未實測）
source_2: https://www.aiposthub.com/open-intelligent-ui-copilotkit-interactive-answers/（aiposthub／Philo〈Open Intelligent UI 開源：讓 AI 回答變成能操作的圖表與工具〉，2026-10-11；依 CopilotKit 10-09 介紹與 2026-10-11 README 整理，未實測）
type: article
status: processed
asset_value: 54
---

兩篇都是**開發者端**的生成式介面（Generative UI）：讓 AI 的回答不只是文字，而是可點選、可調參數的卡片、表單、圖表或小工具。A2UI 是 Google 的**開放協定**（代理描述畫面、應用端用自己的元件畫），Open Intelligent UI 是 CopilotKit 的**開源模板**（隔離網頁區域呈現客製 HTML）。兩篇皆為二手整理、作者未實測；版本條件為 2026-11-12 快照。使用者端的同類產品（ChatGPT 互動回答）見 [[chatgpt-intelligent-ui-interactive-answers]]。

## T（核心論點）

> **生成式介面改的是「答案怎麼被操作」，不是「答案準不準」。** 畫面能顯示、按鈕能按，不代表背後的資料來源、執行權限與提交結果已經安排好。

三個支撐點：

1. **元件與資料責任留在應用端**：A2UI 由開發者決定代理可用的元件目錄（catalog）、敏感值在伺服器端綁定；代理只挑「這次用哪些元件組合」
2. **顯示、查詢、執行是三件事**：帳單卡片能顯示金額≠代理有權付款；調整數量看計算結果≠真的送出訂單。會改外部系統的動作要有權限檢查與明確確認點
3. **一張沒有來源的圖表，只是把錯誤變得更有說服力**：要分清使用者提供、工具查到、模型估算三種值；範例數字必須標成範例

## B（適用場景）

**適用：**
- 已有明確操作流程與設計系統的產品，想讓代理在對話中直接給可操作的確認卡、比較表、表單
- 評估要用「協定＋原生元件」（A2UI）還是「模板＋隔離 HTML」（Open Intelligent UI）
- 規劃跨平台（Web／Flutter／Android／iOS）共用同一份代理邏輯

**不適用 / 注意：**
- 🔴 **A2UI 協定 1.0 ≠ 各平台都 1.0**（2026-11-12 快照）：Android Jetpack Compose 顯示程式仍 **0.9.1**、1.0 尚在安排；Lynx 0.9 路徑調整中；Gemini 現有環境 **0.9.1**，Gemini Enterprise 的 1.0 是後續方向
- 官方 A2UI Quickstart（Python 伺服器＋React／Flutter 客戶端的旅遊規劃器）**明說不是生產範本**
- Open Intelligent UI 預設要 **OpenAI（chat-latest）與 Jev 兩組 API 金鑰**（Jev 負責選呈現方式），開源不等於模型呼叫免費；金鑰只留瀏覽器記憶體，但**請求仍經過應用伺服器**，只能在信任的部署環境用

## R（操作步驟）

### 兩者怎麼分

| | A2UI 1.0（Google） | Open Intelligent UI（CopilotKit） |
|---|---|---|
| 性質 | 開放協定 | 開源介面＋代理模板（MIT） |
| 畫面怎麼來 | 代理送**畫面描述**，應用端對到自己註冊的**原生元件** | 隔離網頁區域呈現**客製 HTML、樣式與互動程式** |
| 品牌一致性 | 由應用端元件與設計系統維持 | 由模板與生成內容決定，需另外約束 |
| 平台 | Web：React／Angular／Lit；Flutter；iOS SwiftUI；Android Compose（版本見上） | Node.js、Python 環境 |
| 代理端工具 | Python／TypeScript／Dart SDK；Python Macros 可展開固定結構卡片與綁定 | CopilotKit 傳遞代理回應，可改接自己的代理（屬開發工作） |
| 1.0 新增 | 雙向函式呼叫（點選回傳代理／後端）、無障礙支援 | — |

- **A2UI 與 MCP Apps 分工**：A2UI 管畫面描述與原生顯示；MCP Apps 讓互動內容能在**支援的 AI 助手內**承載。助手能載入介面≠介面能讀公司資料，資料連線、登入身分、操作授權仍要逐項接
- 官方示例（Google 展示，非作者實測）：Atlassian 讓 Jira 內的代理用卡片詢問「要準備多少測試案例」，人在原卡片選擇；Alation 把查詢與資料結果放進工作環境——價值在**交接位置**，不是證明代理不需要人

### 第一次導入的驗收法（兩篇共同建議）

1. 先挑**只讀**或「條件調整＋確認」的單一流程（例：固定資料轉比較表、查詢條件調整），不要一開始接付款或大量資料修改
2. 修改一次選項，確認**代理後續回答用的是新值**（卡片更新了人數、提交出去卻是舊值，外觀再完整也算失敗）
3. 測資料暫時取不到的情況：畫面要交代錯在哪、能否重試，不能只留空白卡片；重新查詢中／已完成／失敗要分得出來
4. 多人協作時，卡片開著期間資料被別人改，提交前要決定是否重查（這是應用流程設計，不是協定會替你決定的）
5. 編輯中與已送出要在畫面上分清楚，避免把預覽當成真正執行
6. 手機上實際操作一次；跨平台時先列出各端**共同支援**的功能，決定哪些互動要降級
7. 擴大前比較純文字流程與新介面各需幾次澄清、使用者是否清楚最後提交了什麼

## C（關鍵行動）

- [ ] 若在 Flutter 專案評估 A2UI，先核對當下 Flutter 顯示程式與目標承載環境支援的協定版本
- [ ] 任何生成的圖表／數字欄位，設計上要能標示來源類型（使用者提供／工具查詢／模型估算）
- [ ] 會改動外部系統的按鈕，一律走另外的權限檢查與確認步驟，不靠按鈕名稱

## 原始摘要

**A2UI 1.0**（2026-11-11）：Google 推出生成式介面的開放協定穩定版，代理描述畫面、應用端用自己的元件呈現；新增雙向函式呼叫與無障礙支援，提供多平台顯示程式與 Python／TypeScript／Dart 代理 SDK。元件目錄與伺服器端資料綁定把品牌與敏感值留在應用端，但 Android Compose、Lynx、Gemini 仍在 0.9.x，協定升版不等於全平台完成升級。

**Open Intelligent UI**（2026-10-09 介紹）：CopilotKit 開源模板，把 AI 回答轉成圖表、表格與操作工具；MIT 授權，預設需 OpenAI 與 Jev 金鑰。作者強調介面更好懂不代表資料可靠，建議先從來源明確的只讀比較表試起，再逐步加入會改外部系統的功能。

**合併判斷（2026-11-19）**：既有 [[chatgpt-intelligent-ui-interactive-answers]] 論點相同（改的是答案長怎樣、不是準不準），但它是 ChatGPT **使用者端**的產品快照（方案、叫法、實測），受眾不同；本條是**開發者端**的協定與模板，故另建並雙向連結。[[flutter-google-io-2026]]、[[research/android-ai-integration-appfunctions]] 只簡短提過 A2UI 早期版本。

## 相關連結

- [[chatgpt-intelligent-ui-interactive-answers]] — 使用者端同類產品：ChatGPT 互動回答（2026-10-08 快照），同一條「介面≠正確」論點
- [[self-hosted-agent-workspace-approval-licensing]] — 同為 CopilotKit 產品的 OpenBot 1.0（自架代理工作區的核准與授權分層）
- [[ai-native-interface-patterns]] — AI-native 介面模式庫，生成式介面是其中一種
- [[flutter-google-io-2026]] — Google I/O 2026 首次介紹 A2UI Protocol
- [[research/android-ai-integration-appfunctions]] — Android 端 A2UI Compose Renderer 與 AppFunctions
- [[mcp-stateless-architecture]] — MCP Apps 規格：伺服器直接回傳互動介面
- [[claude-dashboards-beta-2026-10]] — Claude 側「回答長成儀表板」的產品功能
