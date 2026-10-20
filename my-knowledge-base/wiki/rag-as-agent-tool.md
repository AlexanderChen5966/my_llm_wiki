---
title: RAG 是 Agent 手上的一項 Tool：Coordinator / Tool 兩層架構
date: 2026-08-12
tags: [ai/agent]
source: https://ithelp.ithome.com.tw/articles/10402383（《Data Machi 30 天學習系列：從零開始打造企業 AI 知識工作流》Day 10）
type: article
status: processed
asset_value: 48
---

同系列前作：Day 05 見 [[enterprise-ai-five-stage-evolution]]、Day 6–8 併入 [[rag-retrieval-augmented-generation]]、Day 9 見 [[multimodal-document-processing-three-layers]]。本篇是 RAG → Agent／Tool Use 的**橋接篇**。

## T（核心論點）

「我們應該做 RAG，還是做 Agent？」——**這個問題本身就不太正確**。

> **RAG 和 Agent 不是同一個層次的東西。RAG 可以是 Agent 手上的一項工具。**

- RAG 解決的是：我要怎麼從企業文件中**找到相關知識**？
- Agent 解決的是：面對這個問題，我**下一步應該做什麼**？

因此：

> **RAG 解決的是知識取得問題，不是整個企業工作的執行問題。**

## B（適用場景）

**適用：**
- 判斷一個需求該用 RAG、Data Tool 還是 Agent
- 設計多來源查詢系統時的職責切分
- 反駁「把所有企業資料都塞進同一個向量資料庫」的直覺

**不適用 / 注意：**
- ⚠️ 「Agentic RAG」這個詞很容易讓人誤以為「只要讓 RAG 多搜幾次就能完成所有任務」——**它的核心只是讓 Agent 更有策略地使用 Retrieval**
- 🔴 **工具越多不代表 Agent 越強**（見下方踩坑筆記）

## R（操作步驟）

### 一、能力對照表：這個需求該用什麼？

| 使用者需求 | 主要能力 |
|---|---|
| 「差旅規範怎麼寫？」 | **RAG** |
| 「今天有多少筆報銷申請？」 | Data Tool |
| 「幫我計算平均報銷金額」 | Analytics Tool |
| 「幫我建立報銷申請」 | Tool Use |
| 「查規範後，判斷是否符合，再建立申請」 | **Agent / Workflow** |

**RAG 擅長**：答案已存在於相對靜態內容中的任務——文件問答、規範查詢、長文件摘要、歷史知識搜尋。
**RAG 力不從心**：即時數據、精確計算、系統操作（建工單）、最新專案狀態、多步驟任務。

### 二、Coordinator / Tool 兩層架構

```
上層 Coordinator / Agent Layer
  理解問題 → 規劃任務 → 選擇工具 → 判斷「資訊是否足夠？」

下層 Tool
  search_document_base  → 搜尋 PDF 知識庫
  query_google_sheets   → 取得結構化數據
  search_confluence     → 搜尋 Wiki 與企業文件
  get_trello_cards      → 取得專案狀態
```

`search_document_base` 的職責非常單純：**根據一個 Query 到 PDF 知識庫找出最相關內容**。它**不應該**判斷使用者真正想解決什麼、要不要先查 Sheets、找不到時該不該改查 Confluence、最後資料夠不夠回答——這些全是 Coordinator 的工作。

> **Agent 負責判斷「要做什麼」，Tool 負責可靠地完成「某一件事情」。**

🔑 架構圖上比原始版本多出的關鍵一步：整合工具結果之後多了一個「**資訊是否足夠？**」的判斷迴圈——**這正是 Agentic Workflow 與單純 Tool Use 的分野**。

不這樣拆的後果：把所有決策塞進 RAG Tool → 這個工具很快變得又大又難維護；把所有工具邏輯塞進 Coordinator → 系統變成難以測試的巨大 Agent。

### 三、為什麼不能把所有企業資料塞進同一個向量資料庫

| 差異 | 說明 |
|---|---|
| **更新頻率** | 企業政策三個月到一年才更新一次，適合建 Embedding 索引；即時營運數據每分每秒在變，每次更新都重新 Embedding 是不必要的成本 |
| **查詢方式** | 文件問題是「哪一段內容跟我的問題最相關」（語意搜尋）；數據問題是「台灣市場最近三個月轉換率平均多少」（`SELECT AVG(...)`）。**精確數字應該用結構化工具，不是向量相似度** |
| **權限需求** | 員工手冊全公司可查，財務數據／人員資料／敏感專案只有特定角色；全混在一個向量庫裡權限控制會非常複雜 |
| **準確性要求** | 「公司文化重視什麼」可以接受語意檢索後的摘要；「2026 年 7 月營收是多少」**差一點都不行** |

> **不是所有資料都應該進向量資料庫，也不是所有問題都應該用 RAG 解決。**

### 四、從 RAG 到 Agentic RAG

```
傳統 RAG：Question → Search Once → Generate Answer

Agentic RAG：Question → Search → 結果足夠嗎？
             → 否 → 重新改寫 Query → 再次搜尋
             → 還需要其他來源嗎？ → 是 → 呼叫另一個 Tool
             → 整合結果 → 回答
```

四種具體能力：
1. **第一次搜尋不夠時自動改寫 Query**——「離職後帳號處理」查不到，改成 `employee offboarding access revocation` 或「離職 權限撤銷 帳號停用」再查
2. **搜尋過程中切換工具**——先 `search_document_base()` 查政策，再 `query_identity_system()` 查目前系統狀態
3. **主動判斷答案是否完整**——找到停用時間但沒找到特殊權限帳號的處理方式，可以再搜、換來源、或誠實說明缺什麼，而不是看到任何 Retrieval Result 就立刻產生答案
4. **把前一步結果帶入下一步**——查政策得到「高風險系統須當日停權」→ 取得高風險系統清單（Okta、SAP、Production DB）→ 查這三個系統帳號狀態 → 找出尚未停用的人員

最終架構：
```
              Agent
    ┌───────────┼───────────┐
   RAG      Data Tool   Action Tool
 找文件知識   查即時數據    執行操作
```
**RAG 是「知道」，Tool Use 讓 AI 可以「做」，Workflow 讓過程可控制與可重複。**

### 🔴 踩坑筆記：工具越多不代表 Agent 越強

同時提供 `search_pdf`／`search_confluence`／`search_google_drive`／`search_notion`／`search_sharepoint`／`search_slack`／`query_database`／`query_bigquery`／`query_sheets`，使用者只問「去年日本市場的需求趨勢如何？」——Agent 得先判斷哪個才是真正的資料來源。

工具描述不清楚、責任重疊時，模型會：選錯工具／重複搜尋多個來源／增加不必要成本／得到互相衝突的答案／無法知道哪個來源是正式資料。

**好的 Tool 應具備**：清楚的功能名稱、明確的適用範圍、簡單的輸入參數、結構化的輸出格式、可理解的錯誤訊息、明確的權限控制。

## 補充資料｜同系列 Day 11：RAG 不應該變成萬用資料層（2026-09-01 併入）

來源：https://ithelp.ithome.com.tw/articles/10404533（Day 11）。**與本條目論點高度重疊**（連範例場景「最近三個月某指標持續下降／最新數字／改善專案進度」都相同），故不另建條目，只收其兩處新料。

**① 五種資料類型的取得方式對照**（比本條目「四道差異」更細的一張分類表）：

| 資料類型 | 常見內容 | 更新頻率 | 適合的取得方式 |
|---|---|---|---|
| 文件知識 | 規範、手冊、政策、歷史報告 | 低到中 | **RAG** |
| 結構化資料 | 銷售、訂單、會員、庫存 | 中到高 | SQL / Sheets Tool |
| 即時狀態 | 工單、專案任務、物流狀態 | 高 | API |
| 外部服務 | 天氣、匯率、搜尋結果 | 持續變動 | External API |
| 系統操作 | 建立任務、更新狀態、寄送訊息 | 即時 | Tool / API Action |

判準一句話：**RAG 擅長回答「文件裡寫了什麼」；另一類問題問的是「系統現在的狀態」。** 後者需要的是精確條件篩選（`region = Taiwan` / `date = yesterday` / `metric = sales`），而**向量搜尋的設計目標是找語意相近，不是執行精確篩選**——會找回「上週台灣銷售」、「昨天香港銷售」這些語意接近但不正確的資料。

**② Read Tool 與 Action Tool 的風險分級**：

| 類型 | 例子 |
|---|---|
| Read Tool | 查詢銷售、取得文件、查看任務 |
| **Action Tool** | 建立任務、更新狀態、寄送訊息 |

🔴 **Action Tool 的風險比 Read Tool 高，因為它真的會改變外部系統的狀態**——因此企業 AI 還需要處理確認機制、權限、操作紀錄與失敗回復。（本庫對這四項的深入見 [[agent-tool-authorization-gatekeeper]] 與 [[agent-control-layer-permission-hooks-planning]]。）

## C（關鍵行動）

- [ ] 遇到「該做 RAG 還是 Agent」的爭論，先用上面的能力對照表把需求拆成 RAG / Data / Analytics / Action / Workflow
- [ ] 資料要不要進向量庫，先過四道檢查：更新頻率、查詢方式、權限需求、準確性要求
- [ ] Tool 只負責可靠完成一件事——別把「該不該查、查不到怎麼辦」寫進 Tool 裡
- [ ] Agentic Workflow 的最小差異點是「資訊是否足夠？」的判斷迴圈，設計時別漏掉
- [ ] 加 Tool 前先問：它與現有 Tool 的責任邊界劃得清楚嗎？描述能讓模型單獨判斷什麼時候用嗎？

## 原始摘要

能力層次：`RAG → 讓 AI 找到企業知識；Tool Use → 讓 AI 取得資料或執行操作；Agent → 讓 AI 判斷下一步用什麼工具；Agentic Workflow → 用流程控制讓多步驟任務可靠執行`。範例場景「核心指標三個月下降的原因是什麼？改善專案進度如何？」需要同時查 Google Sheets（數據）、Confluence／PDF（定義與歷史）、Analytics（拆解原因）、Trello／Jira（專案進度）——單一 RAG 知識庫無法自然代表所有這些系統。

## 相關連結

- [[data-tool-implementation-field-alias]] — **同系列 Day 12＋13**：Tool Use 分工原則與 Data Tool 五道防線（欄位別名／不猜欄位／時效標註）
- [[enterprise-knowledge-map-source-of-truth]] — **同系列 Day 14**：Source of Truth 與衝突裁決表

- [[rag-retrieval-augmented-generation]] — **同系列 Day 6–8**：RAG 四步 pipeline、Hybrid Search、可追溯性；本篇接續回答「找到資料之後呢」
- [[enterprise-ai-five-stage-evolution]] — **同系列 Day 05**：Chat→RAG→Tool Use→Agent→Agentic Workflow→產品；本篇是第 2→3→4 段箭頭的細部展開
- [[multimodal-document-processing-three-layers]] — 同系列 Day 9：PDF 的視覺結構處理
- [[agent-tool-runtime-seven-layers]] — Tool 的責任邊界在執行層的完整規格
- [[agent-skills-vs-mcp]] — 工具／能力太多時的 Progressive Disclosure 解法，直接對應本篇「工具越多不代表 Agent 越強」
- [[agent-loop-explained]] — 「資訊是否足夠？」的判斷迴圈即 Agent Loop 的核心
- [[enterprise-rag]] / [[enterprise-rag-smb-architecture]] — 企業 RAG 的落地架構
- [[langchain-tool-use]] — Tool Use 的框架層實作
- [[hand-coding-agent-file-tools]] — ⚠️ 張力對照：小型高頻變動專案**不該上 RAG**，關鍵字搜尋已夠用
- [[query-strategy]] — 查詢策略選擇
