---
title: 用 Claude Code 建立 LLM Wiki 取代 RAG
date: 2026-05-25
updated: 2026-08-03
tags: [ai/claude, pkm/workflow, pkm/obsidian]
source: Facebook 社群貼文
type: article
status: processed
asset_value: 75
---

Andrej Karpathy 提出：用 Claude Code + Prompt 建立個人化 LLM Wiki，將 raw/ 目錄的文件自動整理成 wiki/ 知識圖並可在 Obsidian 視覺化，主張此方式取代傳統 RAG（向量資料庫 + Embedding）的複雜架構，成本低、查詢精準且可發布成企業知識庫網頁。

## T（核心論點）

傳統 RAG 的複雜性（向量資料庫、Embedding 模型、碎片化查詢）已被 Coding Agent 大幅消滅。用 Claude Code 將知識整理成互相連結的 Wiki，等同於建立一個 Graph（圖論）資料庫——速度極快、成本極低（只需訂閱 Claude），查詢精準且可附來源。核心分工：**人負責輸入原始素材，AI 負責整理成 wiki，知識庫自動演化**。

## B（適用場景）

**適用：**
- 個人知識庫建立（替代 Notion、Obsidian 手動整理）
- 團隊/企業內部知識管理（可發布成輕量知識庫網頁）
- 研究資料整理（將大量文件自動提取關聯）
- 不想維護複雜 RAG 基礎設施的場景

**不適用 / 注意：**
- 即時外部資料（需要 MCP）
- 需要向量搜尋的精確語意匹配場景（LLM Wiki 依賴 wikilink，非向量）
- 大量文件時仍需 Context 管控（Claude Code 有 context limit）
- 查詢頻率低的資料——只查一次的東西不需要編纂，投資只有在會重複使用時才划算（來源：iThome「WikiBrain」系列 Day01）
- 資料變動極快的領域——來源每週被推翻，編纂出來的頁面會一直過時，維護成本大於收益（同上）
- 完全不想花時間訂規則的情況——這套模式的品質上限很大部分取決於有沒有花時間定義「這座知識庫要怎麼寫」，什麼都不設定也能用，但只是比較整齊的摘要工具（同上）

## R（操作步驟）

### 建立 LLM Wiki 基本步驟

1. **建立專案資料夾**：例如 `AI2027/`
2. **建立原始資料區**：在資料夾內新增 `raw/` 子目錄
3. **匯入所有文件**：將參考資料放入 raw/，支援 Word、Excel、PDF、MD、TXT 等格式
4. **取得核心 Prompt**：進入 Claude Code，複製 Karpathy 的 Prompt 範本（https://reurl.cc/2aLl99）
5. **加入客製化指令**：
   ```
   你是我專屬的 LLM Wiki Agent。請建立 CLAUDE.md 作為 Schema 檔案，
   內含完整的運行規則。建立 index.md 和 log.md，定義資料夾的命名規範。
   產出第一個文摘範例讓我確認。從現在開始，所有互動遵守 Schema 規則，
   全部使用繁體中文。
   ```
6. **增量更新**：有新文章時，放入 raw/ 並對 Claude Code 說「幫我整理新文件」

### LLM Wiki vs 傳統 RAG

| 維度 | 傳統 RAG | LLM Wiki |
|------|---------|---------|
| 基礎設施 | 向量資料庫 + Embedding 模型 | 只需 Claude 訂閱 |
| 查詢方式 | 向量相似度 + 碎片拼湊 | 自然語言 + Graph 遍歷 |
| 知識形式 | 向量碎片（無 context）| 完整 wiki 條目（有 context）|
| 發布 | 複雜 | 一句話讓 Claude 產出網頁 |
| 更新 | 需重新 Embedding | 增量整理 raw/ 即可 |

## C（關鍵行動）

- [ ] 嘗試用三資料夾法（raw/ + wiki/ + CLAUDE.md）建立個人知識庫原型
- [ ] 設計 CLAUDE.md Schema：定義 wiki 條目格式、tag 規範、wikilink 規則
- [ ] 建立 index.md 索引讓知識庫可導航

## 原始摘要

介紹 Andrej Karpathy 提出的概念：用 Claude Code + Prompt 建立個人化 LLM Wiki，將 raw/ 目錄的文件自動整理成 wiki/ 知識圖並可在 Obsidian 視覺化，主張此方式取代了需要向量資料庫與 Embedding 的複雜 RAG，成本低、查詢精準且可發布成企業知識庫網頁。

## 補充視角：LLM Wiki 三層架構圖解（日文 Qiita 文章翻譯）

來源：codelove.tw（原文：Qiita shinnosuke_takami），以插圖拆解 LLM Wiki 架構與 RAG 的差異。

### 結構類比
- **RAG ≒ 倉庫**：原始資料未經編輯直接作為 I/O，各文件結構與粒度差異大
- **LLM Wiki ≒ 圖書館**：使用經過編輯的內容作為 I/O，結構與粒度一致

### 運作類比
- RAG → 每次都用直譯器執行原始碼
- LLM Wiki → 先編譯一次，然後持續讓可執行檔成長

### 三層分工

| 層次 | 名稱 | 內容 | 誰寫 | 誰讀 |
|------|------|------|------|------|
| 第 1 層 | Raw Sources | 文章、論文、圖片等一次資料，不可改動 | 人類 | LLM |
| 第 2 層 | The Wiki | Markdown 檔集合（摘要、實體頁、概念頁、比較表） | LLM | 人類 |
| 第 3 層 | The Schema | 結構、規範與工作流文件（CLAUDE.md、AGENTS.md） | 人類 + LLM | 人類 + LLM |

### 三種操作

| 操作 | 說明 |
|------|------|
| **Ingest** | 每匯入一份新資料，影響 Wiki 的 10～15 個頁面；LLM 讀取內容、擷取重要資訊並整合進既有 Wiki |
| **Query** | LLM 先讀 index.md 找相關頁面，再深入閱讀整合答案；不需 embedding 基礎設施。有價值的答案歸檔為新 Wiki 頁面 |
| **Lint** | 定期健康檢查：修正矛盾、過時主張、孤立頁面、缺少交叉引用、資料缺口 |

### 兩個特殊檔案
- **index.md**：Wiki 全頁目錄（連結 + 一行摘要 + metadata），依類別整理，每次 Ingest 時更新
- **log.md**：append-only 時間線，記錄所有 ingest/query/lint 事件

### RAG vs LLM Wiki 適用情境判斷

| 觀點 | 適合 RAG | 適合 LLM Wiki |
|------|---------|-------------|
| 資料更新頻率 | 高 | 中～低 |
| 資料規模 | 數千～數百萬 | 數百 |
| 知識性質 | 事實查詢 | 概念理解、整合 |
| 想要的答案 | 「原始資料裡寫了什麼」 | 「綜合多份資料後可得出什麼結論」 |

## 補充資料：四工具對照表 + OpenWiki 實作結構（2026-08-03 併入）

來源：〈如果是將 codebase 轉化為知識，LLM Wiki 可能就夠了〉（Qiita）。⚠️ LLM Wiki 核心概念已被 [[synthesis/synthesis-karpathy-llm-wiki-method]]（7 篇整合）完整涵蓋，本節只收**既有內容零命中**的兩塊。

### 為何不只用 CLAUDE.md 或圖譜 RAG（兩種既有做法的限制）

| 做法 | 限制 |
|---|---|
| 圖譜 RAG（Bedrock KB／GitNexus）| **每次提問都要重新沿圖譜走訪組裝**——第 10 次答案不會比第 1 次好，卻付 10 次探索成本 |
| 代理人指示檔（CLAUDE.md/AGENTS.md）| **不自動更新就會過時**——實作變了文件沒跟著變，等於餵錯誤資訊給代理人 |

Anthropic 官方觀察：能有效理解程式碼庫的公司共同做法是 CLAUDE.md **依資料夾分層並保持簡潔**、導入 LSP 提高探索正確性、**由團隊負責人制定標準而非丟給個人**；且**每 3–6 個月要重新檢視**——舊指示可能拖新模型的後腿（與 [[claude-md-optimization-techniques]] 的世代反轉清單同一命題）。

### 四個 LLM Wiki 工具對照

| 工具 | 開發商 | 特點 |
|---|---|---|
| **DeepWiki** | Cognition | 面向公開 GitHub repo，把網址 `github.com` 換成 `deepwiki.com` 即可產生 |
| **AutoWiki** | Factory | 整合進 CI/CD，程式碼更新時自動重新生成 |
| **OpenWiki** | LangChain | OSS，不只程式碼，還能匯入 Gmail/Notion/X；具「Personal Brain」功能 |
| **GBrain** | Garry Tan | 只在 Git repo 內 Markdown 完成的簡易版本 |

> ⚠️ **規模限制**：來源數量超過 **100 筆**時，需搭配搜尋工具使用——LLM Wiki 不是萬用取代方案。

### OpenWiki 實作目錄結構（實測）

```
openwiki/
├── index.md          — 整體入口點
├── INSTRUCTIONS.md    — 要 Wiki 撰寫哪些內容的簡述
├── quickstart.md      — 快速入門頁面
├── architecture/       — 架構總覽（含自己的 index.md）
├── domain/             — 領域知識
└── operations/         — 維運與開發指南
```
每個頁面用 YAML frontmatter 附 `type`/`title`/`description`/`tags`，**適合機器處理**——與本知識庫的 frontmatter 規範是同一設計思路。GitHub Actions 排程自動刷新，**規則明訂「不要手動編輯生成的頁面，改源碼讓它重新生成」**。

> 同概念產品化案例：iThome「為你自己蓋一座會複利的知識庫——WikiBrain」系列（2026-09 起，Day01：https://ithelp.ithome.com.tw/articles/10409229）把同一套 raw/wiki/schema 三層概念做成獨立 MCP 服務型商業產品 WikiBrain（wikibrain.app），核心論點與本條目高度重疊，故不另建條目，僅供參考別人如何把概念產品化。

## 補充視角：schema 的使用者可改機制與「待編纂＝連結反查計算」判準（2026-09-17 併入）

來源：iThome「WikiBrain」系列 Day02（https://ithelp.ithome.com.tw/articles/10409505）。核心論點（三層架構、Ingest/Query/Lint、log.md 格式、10-15 頁規模門檻）已被本條目與 [[synthesis/synthesis-karpathy-llm-wiki-method]] 涵蓋，此處只補兩項增量。

**① schema/ 作為「使用者自己能改」的具體機制**：一般做法把規則塞進系統提示詞，使用者改不到、也無法反映「研究型知識庫」與「專案管理知識庫」規則本該不同的事實。把規則放進 `schema/` 之後，它變成知識庫裡的一頁普通筆記，使用者能直接改，改完下次 agent 進來就照新規則走，不用重新部署。系統這邊的配套只做一件事：讓 agent 動筆前先讀這一層——MCP 的第一個工具 `get_instructions`，描述裡直接寫「動筆前先呼叫本工具」，**且該工具在首次調用時會同時回傳規則本身與目前待編纂的來源清單**，agent 一進來就知道有哪些事該做，不用使用者開口。

**② 「待編纂＝連結反查計算」判準**：Karpathy 原文只在健檢一節提到找「沒有任何頁面連進來的孤兒頁」，本篇作者將同一想法反過來套用在來源層：`raw/` 底下的來源，只要沒有任何 `wiki/` 頁面連回它，就判定為待編纂——**這是算出來的，不是靠狀態欄位標記的**。好處是永遠不會跟現實脫節：手動刪掉一頁 wiki，對應來源自動變回待編纂；自己寫一頁連回去，它自動變成已編纂，沒有旗標與事實對不上的問題。

> **方法論對照**：此判準（連結反查計算）與本知識庫自己的 inbox 判斷機制（CLAUDE.md 訂為 frontmatter `status:` 欄位制）是兩種不同哲學的解法——前者用「圖結構的客觀事實」推導狀態，後者用「顯式旗標」宣告狀態。前者的優點正是本文強調的「不會脫節」；後者的優點是可讀性高、不需要先建好 wikilink 圖譜就能判斷，且能容納「已讀但刻意不建連結」這種介於中間的狀態。兩者各自對應不同的知識庫成熟度與工具鏈，值得作為未來評估是否要引入圖結構判斷時的參考。

## 相關連結

- [[wikibrain-llm-wiki-implementation-patterns]] — WikiBrain Day03+04 操作層實作案例（程式碼守門、工具設計、重試與記帳機制）
- [[wikibrain-compounding-knowledge-base-demo]] — WikiBrain Day05 複利實測：兩篇來源如何讓知識庫「愈長愈強」，附完整步驟與成本數據
- [[synthesis/synthesis-karpathy-llm-wiki-method]] — Karpathy LLM Wiki 方法論知識精粹（本主題 7 篇整合）
- [[karpathy-knowledge-base-method]] — Karpathy 三資料夾法原始設計
- [[karpathy-no-obsidian-setup]] — 不用 Obsidian 的極簡版本
- [[llm-wiki-karpathy-vs-nashsu]] — Karpathy 抽象方法論 vs nashsu 桌面 App 的比較
- [[tpki-tri-pattern-knowledge-index]] — TPKI：整合 LLM Wiki + Hermes + GBrain 的三模式索引架構
- [[enterprise-rag]] — 如果確實需要 RAG 的企業場景參考
- [[claude-code-obsidian-setup]] — Claude Code + Obsidian 完整建構指南
