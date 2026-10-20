---
title: OKF（Open Knowledge Format）：Google 把 LLM-Wiki 變成開放標準
date: 2026-06-22
tags: [ai/agent, pkm/workflow]
source: https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing/
type: reference
status: processed
asset_value: 82
---

OKF 是 Google Cloud 2026-06-12 開源的「知識表示格式」（Apache-2.0，v0.1）。本質是**一個裝滿 Markdown 檔的資料夾 + YAML frontmatter**，把 Karpathy 的 LLM-Wiki 模式正式化為廠商中立的開放標準，讓 AI Agent 不靠專屬 SDK 就能讀懂企業知識。整合自 4 篇來源（Google 官方解讀 / 白話導入指南 / Marie Haynes SEO 視角 / 中文犀利點評）。

## T（核心論點）

限制 Agent 的不再是模型參數，而是**缺乏可信、可更新、可追溯的 Context**。企業知識散落在 DB schema、指標定義、Runbook、API 棄用通知、資深工程師腦中——而且被各家目錄廠商的 SDK 與知識圖譜 schema 鎖死，換個模型就要重洗一次。OKF 的答案是**回到最原始的工具**：純 Markdown + 一點 YAML，用「格式作為唯一契約」解決互操作性，讓知識在任何模型、任何工具之間自由流轉。這正是 [[karpathy-llm-wiki-improvements]] 與 [[synthesis-karpathy-llm-wiki-method]] 描述的 LLM-Wiki 模式被標準化的一步。

## B（適用場景）

**適用：**
- 已有客服機器人 / 內部搜尋 / RAG，想降低舊 PDF 與文件孤島造成的混亂
- 正在做 Agent 或跨工具自動化，需要可審查、可追溯的知識供應鏈
- 想擺脫被單一雲廠商「套牢」、確保資料主權

**不適用 / 注意：**
- 一般內容網站 / 部落格 → 先顧內容品質、結構化資料、`llms.txt` 即可，不必追新規格
- **「使用者需求還沒驗證」時優先級很低**——OKF 解決的是「Agent 不知道公司規則」，不是產品方向問題
- v0.1 草案，格式不會自動救內容品質；欄位太自由（只強制 `type`）會失控，企業內部仍須自訂欄位慣例
- 不要當成唯一資料層：OKF 描述脈絡，不取代 DB / API 文件 / 正式資料目錄

## R（核心結構與三大設計原則）

### 結構：三個「僅有」
1. **僅有 Markdown** — 任何編輯器可讀、GitHub 可渲染、搜尋引擎可索引
2. **僅有檔案** — 可打包成 tar、託管在任何 Git repo、掛載任何檔案系統
3. **僅有 YAML frontmatter** — 唯一強制欄位是 `type`（table / metric / playbook…），其餘（`title` / `description` / `resource` / `tags` / `timestamp`）自由

```text
客服知識包/              ← knowledge bundle（一個資料夾 / repo）
├── 退款規則.md          ← concept document（一個概念一個檔）
├── 補償條件.md
└── 升級人工處理.md
```
> frontmatter 是「索引卡」（Agent 不必讀全文就知道這檔是什麼、能不能引用、是否過期）；body 是「智慧本體」，格式完全自由。

### 三大設計原則
| 原則 | 含意 |
|------|------|
| **極簡（Minimally opinionated）** | 只強制 `type`，規範定義的是互操作性邊界，不是內容模型 |
| **生產/消費解耦** | 人手寫 or 管線自動匯出皆可；GPT / Claude / Gemini 只要認得 MD+YAML 都能讀。格式是唯一契約，兩端工具可獨立替換 |
| **是格式，不是平台** | 不綁雲廠商 / DB / 模型 / Agent 框架，不需帳號或 SDK。資料主權 100% 在自己手裡 |

### 與相鄰概念的分工（最容易混淆）
| 名稱 | 管的層次 |
|------|---------|
| **OKF** | 知識**本身**怎麼整理（內容） |
| `llms.txt` | 告訴 AI 網站有哪些重要頁面（索引入口） |
| **RAG** | 怎麼從知識庫找片段再回答（檢索）——OKF 讓 RAG 來源更乾淨可維護，但不取代它 |
| **MCP** | 讓 Agent 連接工具與服務（傳輸）——見 [[mcp-fastmcp-server]] |

## C（關鍵行動）

- [ ] **非工程試點從「退款政策」起步**：建 3 份知識檔，每份寫清楚「適用誰 / 權威來源 / 最後誰確認 / 多久重看 / Agent 可直接回答哪些情境 / 哪些須轉人工」
- [ ] **資料團隊從「週活躍使用者（WAU）」指標起步**：常被問也常被算錯，寫清楚權威表、公式、排除條件、負責人、確認日期
- [ ] 企業內部**自訂欄位慣例**（負責人 / 權威來源 / 檢查週期 / 保密等級 / Agent 可用範圍 / 是否停用），補足官方刻意留白的彈性
- [ ] 把 OKF 納入既有**權限與審計流程**——它很容易被複製索引，敏感知識（個資 / 營業秘密 / 合約）不可整包直接餵模型
- [ ] 工具鏈：Google 開源了 Enrichment Agent（掃 BigQuery 自動草擬概念檔）、無後端 Visualizer（OKF Bundle → 互動關係圖 HTML）、Sample Bundles（GA4 / Stack Overflow / Bitcoin）

## 原始摘要

- **核心洞察（Karpathy 引用）**：「LLM 不會感到無聊，不會忘記更新交叉引用，並且可以一次性修改 15 個檔案。」人類因嫌繁瑣放棄維護個人 Wiki，而這種枯燥的簿記更新正是 LLM 最擅長的——這是 OKF 與整個 LLM-Wiki 模式的底層動機。
- **SEO 視角（Marie Haynes）**：OKF 可能改變 SEO——從「被搜尋引擎找到」轉向「讓 Agent 能用知識完成任務」。預測會出現專做 OKF bundle 的專家，甚至出現「販售專業知識包」（律師 / 會計 / SEO 把專有流程做成 bundle 出售）的新商業模式。
- **時間線**：2026-06-12 repo 匯入參考工具 → 06-13 官方 blog 發布 → 06-13 修正內部連結改用相對路徑（強調「普通文件在普通工具裡也要可讀」，不綁特定平台）。
- **侷限**：目前解決了**結構互操作性**（怎麼把東西裝進同一個盒子），尚未解決**語義互操作性**（AI 怎麼理解不同行業的黑話）。

## 相關連結
- [[synthesis/synthesis-karpathy-llm-wiki-method]] — **本篇為其來源之一**（2026-09-03 重跑，7→10 篇）：本篇是該方法論被 Google 正式化為開放標準的外部驗證
- [[karpathy-llm-wiki-improvements]] — OKF 標準化的源頭：Karpathy LLM-Wiki 模式
- [[synthesis-karpathy-llm-wiki-method]] — LLM-Wiki 方法論精粹（本知識庫自身的方法基礎）
- [[llm-wiki-over-rag-claude-code]] — 為何 LLM-Wiki 勝過 RAG（OKF 正是這條路的標準化）
- [[llm-wiki-open-source-knowledge-map]] — 開源知識地圖實作
- [[ai-skills-mcp-plugins-agents-comparison]] — OKF / MCP / Skills / llms.txt 的分層比較
- [[mcp-fastmcp-server]] — MCP（傳輸層，與 OKF 內容層互補）
- [[karpathy-knowledge-base-method]] — Karpathy 知識庫方法（健康檢查 / Lint 概念）
