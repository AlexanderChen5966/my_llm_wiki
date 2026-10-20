---
title: 讓 AI 查資料而不是猜數字：Tool Use 分工原則與 Data Tool 實作五道防線
date: 2026-09-01
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10404726（Day 12）／https://ithelp.ithome.com.tw/articles/10404930（Day 13）（《Data Machi 30 天：從 Chat 到 Product，打造真正能工作的企業 AI》）
type: article
status: processed
asset_value: 74
---

同系列前作：Day 04 見 [[enterprise-ai-four-layers-prompt-policy-tool-workflow]]、Day 05 見 [[enterprise-ai-five-stage-evolution]]、Day 6–8 併入 [[rag-retrieval-augmented-generation]]、Day 9 見 [[multimodal-document-processing-three-layers]]、Day 10 見 [[rag-as-agent-tool]]、Day 11 併入 [[rag-as-agent-tool]] 補充、Day 14 見 [[enterprise-knowledge-map-source-of-truth]]。

**Day 12（原理）與 Day 13（實作）合併為一條目**——Day 13 通篇在驗證 Day 12 提出的同一條分工原則，原文自己也寫「這個分工和 Day 12 提過的原則完全一致」。

## T（核心論點）

> 🔑 **模型負責理解意圖與選擇能力，程式負責執行事實。**

Tool Use 真正帶來的改變不是「讓 LLM 知道更多」，而是：

> **讓模型知道自己什麼時候不應該直接回答，而應該先去使用外部能力取得資訊。**

而 Function Calling 這個名字容易誤導——**模型並沒有執行程式**。它只是產生一個「要呼叫哪個工具、參數是什麼」的決定，真正執行的是你寫好的後端。

## B（適用場景）

**適用：** 設計 Data Tool／Sheets／SQL 查詢工具；決定哪些計算該交給程式；為非技術使用者設計不需要知道資料 Schema 的查詢介面；排查 Service Account 權限問題。

**不適用 / 注意：**
- ⚠️ Day 13 的實作以 Google Sheets 為例，但五道防線（欄位別名／不猜欄位／時效標註／快取／一次只驗一層）與資料來源無關。
- 🔴 **Service Account Credential 只能存在後端**——不要放進 React 或 Vercel 前端，前端程式最終在使用者瀏覽器執行。憑證同樣屬於 Secret，不應進入 GitHub Repository。

## R（操作步驟）

### 一、責任分工表：哪一段給模型、哪一段給程式

| 工作 | 比較適合誰負責 |
|---|---|
| 理解使用者想問什麼 | **LLM** |
| 判斷需要哪一個 Tool | **LLM** |
| 篩選資料 | SQL / Python / API |
| 數值計算 | SQL / Python |
| 權限檢查 | Backend / System |
| 整理成自然語言 | **LLM** |

> 🔑 設計原則：**讓模型處理模糊的語言問題，讓程式處理明確且需要精準執行的事情。**

### 二、為什麼精確計算不能交給 LLM

問「這個月平均訂單金額是多少？」——模型可以理解使用者想知道 AOV、也知道該取得 Sales 與 Orders，但**最後的加總、除法與篩選邏輯應該由 SQL 或 Python 執行**。

原因不是模型不會算數，而是：

> 🔑 **企業場景要求可重現、可驗證，而且每次使用相同條件都應該得到相同結果。**

延伸推論（Day 13）：假設「Active Customer」在系統中有明確定義，就應該把**計算邏輯保留在 SQL、Python 或資料工具中**，而不是每一次都讓 LLM 根據文字描述重新理解一次。

### 三、一個好的 Tool 至少要說清楚四件事

| 要回答的問題 | 例子 |
|---|---|
| **Name**｜它叫什麼？ | `get_project_status` |
| **Description**｜什麼情況使用？ | 查詢專案目前進度、負責人與截止日期 |
| **Parameters**｜需要什麼輸入？ | 專案名稱或 Project ID |
| **Return**｜會得到什麼？ | 狀態、負責人、截止日期與資料來源 |

⚠️ 如果系統裡有十幾個工具而每個 Description 都只寫「查詢資料」，模型就很難判斷哪一個適合當下的問題（此點與 [[rag-as-agent-tool]] 的「工具越多不代表 Agent 越強」同源）。

### 四、Data Tool 實作的五道防線（Day 13）

**① 欄位別名層——讓使用者不需要知道資料 Schema**

沒有這一層，使用者得說「請 group by `ticket_category`，然後 filter `request_market = TW`」；有了這一層，他只要問「今年台灣哪一類需求最多？」

| 使用者語言 | 真實欄位 |
|---|---|
| 市場、國家 | `request_market` |
| 建立日期、日期 | `created_at` |
| 類型、需求類別 | `ticket_category` |
| 狀態、進度 | `request_status` |
| 負責人 | `owner_name` |

日期同理——「今年」「上個月」都應該在工具層轉成明確條件，而不是交給模型自由心證。

**② 🔴 不存在的欄位，不要自己猜一個替代品**

問「哪一個工單的處理成本最高？」而資料裡沒有成本欄位時，正確回答是：

> 「目前這份資料沒有『處理成本』欄位，因此無法根據現有資料回答。如果有成本資料來源，需要另外連接或加入對應欄位。」

> 🔑 **明確承認資料不存在，比使用錯誤欄位產生一個看似合理的答案更可靠。**

**③ 回答要告訴使用者資料有多新**

一旦系統開始使用 Cache 或外部資料來源，「這個數字是什麼」和「**這個數字是什麼時間取得的**」會同樣重要：

> 「截至 2026/08/24 15:30 取得的資料，台灣目前共有 328 筆工單，其中 Dashboard 類型最多，共 124 筆。」

**④ 快取不是為了偷懶，而是控制成本與延遲**——但要留一條讓使用者強制重查的路（「請查目前最新狀態」）。

**⑤ 一次只驗證一層**——先用匿名資料驗證，不要直接碰正式資料。

### 五、Service Account 權限問題判斷表

| 現象 | 優先檢查 |
|---|---|
| 找不到 Spreadsheet | Sheet ID 是否正確 |
| Permission Denied | Sheet 是否分享給 Service Account |
| Credentials Error | Service Account Credential 是否正確載入 |
| API 未啟用 | Google Sheets API 是否已啟用 |
| 可以連線但沒有資料 | Worksheet 名稱、Range 或欄位設定 |

### 六、怎麼驗證 Data Tool 真的成功

用三題遞進式測試：`目前共有多少筆工單？`（無條件彙總）→ `2026 年台灣有多少筆工單？`（多條件篩選）→ `2026 年台灣哪一類工單最多？`（篩選＋分組＋排序）。

## C（關鍵行動）

- [ ] 任何數值計算先問：這段邏輯需不需要「每次相同條件得到相同結果」——需要就交給 SQL／程式
- [ ] Data Tool 一定要有**欄位別名層**，否則使用者被迫學 Schema
- [ ] 明確要求工具在欄位不存在時**回報缺什麼**，而不是找相近欄位替代
- [ ] 有快取就一定要在回答裡標**資料取得時間**
- [ ] Service Account 憑證只放後端、不進 repo；接資料前先用匿名資料驗一層
- [ ] Tool 的 Description 要寫到「模型能單獨判斷什麼時候用它」

## 相關連結

- [[rag-as-agent-tool]] — **同系列 Day 10＋11**：RAG 是 Agent 手上的一項 Tool、能力對照表、為什麼不該把所有資料塞進同一向量庫
- [[enterprise-knowledge-map-source-of-truth]] — **同系列 Day 14**：接哪些系統、衝突時相信誰
- [[agent-tool-runtime-seven-layers]] — 🔑 Tool Call 只是工作單，其餘七層（Registry／Schema 驗證／Permission Gate／執行／Timeout／Output Normalization／Audit）全屬 Harness；本篇的四要素對應其中的 Registry 與 Schema 層
- [[langchain-tool-use]] — Tool Use 的框架層實作
- [[agent-tool-authorization-gatekeeper]] — 權限檢查為什麼必須在模型外部
- [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] — **同系列 Day 04**：Prompt 描述期待、Workflow 才是保證
- [[query-strategy]] — 查詢策略選擇
- [[chinese-rag-preprocessing-baseline]] — 同系列 Day 5–7 的前處理三部曲
