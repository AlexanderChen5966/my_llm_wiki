---
title: LLM Wiki：Karpathy 原版方法論 vs nashsu 產品實作比對
date: 2026-05-20
tags: [pkm/workflow, ai/agent]
source: https://github.com/nashsu/llm_wiki
type: reference
status: processed
asset_value: 72
---

Karpathy 的 LLM Wiki 是一份抽象設計模式文件；nashsu/llm_wiki 是將此模式產品化的跨平台桌面應用。兩者保留同一三層架構，但在 Ingest 品質、知識圖譜、主動研究三個面向有根本性差異。

## T（核心論點）

Karpathy 提出「人負責輸入、AI 全權維護 wiki」的方法論骨架；nashsu 在此骨架上加入兩步驟 CoT Ingest、4 訊號知識圖譜、自動缺口偵測與 Deep Research，把「被動整理」升級為「主動擴張」。

## B（適用場景）

**適用：**
- 評估是否要從「Claude Code 自訂實作」切換或整合 nashsu 桌面 app
- 理解 nashsu 的哪些功能可以補進現有工作流（尤其是知識圖譜視覺化與 Deep Research）
- 向他人解釋 Karpathy LLM Wiki 的演化路徑

**不適用 / 注意：**
- nashsu 是桌面應用（需安裝），無法直接取代 Claude Code + Obsidian 的純文字工作流
- nashsu Local API 整合需要桌面 app 同時運行

## R（操作步驟）

### 三層架構對照（兩者共同保留）

| 層 | Karpathy 原版 | nashsu 實作 |
|---|---|---|
| Raw Sources | 不可變，LLM 只讀 | 同，加 SHA256 增量快取避免重複 ingest |
| Wiki | `[[wikilink]]`、YAML frontmatter、index.md | 同，加 overview.md（全域摘要，每次 ingest 自動更新）|
| Schema | CLAUDE.md / AGENTS.md | schema.md（結構規則）+ **purpose.md**（目標與研究方向）|

### 關鍵差異（nashsu 新增或根本改變）

**1. Purpose.md — 知識庫的「為什麼」**
Karpathy 原版只有 schema（怎麼做），nashsu 新增 purpose.md 定義目標、關鍵問題、研究範圍。LLM 每次 ingest 和 query 都讀它，確保產出方向一致。

**2. 兩步驟 CoT Ingest（品質根本提升）**
```
Step 1（分析）：LLM 讀來源 → 識別實體/概念/論點、找與現有 wiki 的連結、偵測矛盾
Step 2（生成）：依分析結果寫 wiki 頁面、更新 index.md / log.md、標記需人工審查項目
```
Karpathy 原版是一步讀寫，nashsu 拆為兩步，品質顯著提升。

**3. 知識圖譜（從 wikilink 到圖分析）**

| 訊號 | 權重 | 說明 |
|---|---|---|
| 直接連結（wikilink）| ×3.0 | 頁面間的 `[[]]` 引用 |
| 來源重疊 | ×4.0 | 共享同一原始來源檔（YAML `sources[]`）|
| Adamic-Adar | ×1.5 | 共同鄰居加權（鄰居度越低越有意義）|
| 類型親和 | ×1.0 | 同類型頁面（entity↔entity）加分 |

加上 **Louvain 社群偵測**——自動找出哪些頁面自然聚集（不依賴預設分類），低凝聚力社群（< 0.15）標記警告。

**4. 主動知識缺口偵測 + Deep Research**
Karpathy 原版的 Lint 是手動觸發、被動掃描；nashsu 自動偵測三種缺口：
- 孤立頁面（degree ≤ 1）
- 稀疏社群（cohesion < 0.15）
- 橋接節點（連接 3+ 社群的關鍵頁面）

發現缺口後可一鍵觸發 Deep Research：LLM 讀 overview.md + purpose.md 產生搜尋策略 → web search（Tavily/SerpApi/SearXNG）→ 合成為新 wiki 頁面。

**5. 查詢管線升級（vs Karpathy 的簡單讀取）**
```
Phase 1：Token 化關鍵字搜尋（支援 CJK bigram）
Phase 1.5：向量語義搜尋（可選，LanceDB，recall 58.2% → 71.4%）
Phase 2：圖譜擴展（以搜尋結果為種子，2-hop 遍歷）
Phase 3：Context Budget 控制（60% wiki / 20% 對話歷史 / 5% index / 15% system）
```

**6. 級聯刪除（Karpathy 無此機制）**
刪除來源 → 移除對應 wiki 摘要頁 → 清 index.md → 清死 wikilink。共享實體（多個來源都引用）只移除來源欄位，不刪頁面。

### 整合現有工作流的可行路徑

nashsu 提供 Local HTTP API（`127.0.0.1:19828`）+ 配套 Claude Code Skill：
```bash
npx skills add https://github.com/nashsu/llm_wiki_skill.git --skill llm_wiki_skill
```

整合後 Claude Code 可查詢 nashsu wiki 的混合搜尋（keyword + vector）、圖譜遍歷、來源重掃，且**觸發紀律嚴格**——只在明確提到「LLM Wiki / my wiki / 知识库」時觸發，不會誤搶其他查詢。

## C（關鍵行動）

- [ ] 評估是否安裝 nashsu 桌面 app：知識圖譜視覺化和 Deep Research 是否解決當前痛點
- [ ] 若安裝，試驗 llm_wiki_skill 接入 Claude Code，測試混合搜尋品質
- [x] 若不安裝，參考 nashsu 的「兩步驟 CoT Ingest」邏輯，優化現有 raw-pipeline Agent A/C 的提示詞品質（已完成：Agent C v1.3.0 加入補充前 T 層對齊驗證；alignment-check Skill 作為事後深度驗證層）

## 原始摘要

nashsu/llm_wiki README 明確說明：「原始文件是抽象設計模式；本專案是附帶大量擴充的具體實作。」完整保留 Karpathy 三層架構、三核心動作（Ingest/Query/Lint）、wikilink 語法、YAML frontmatter、Obsidian 相容性，但根本性升級了 ingest 品質（CoT）、知識結構可視化（圖譜）和主動擴張能力（Deep Research）。

## 相關連結

- [[karpathy-knowledge-base-method]] — Karpathy 原版方法論與三資料夾法
- [[karpathy-no-obsidian-setup]] — 不用 Obsidian 的極簡實作版

## 補充資料（來源：nashsu/llm_wiki GitHub README）

### nashsu 完整技術細節

**技術棧**：Tauri v2（Rust 後端）+ React 19 + TypeScript + Vite，圖形引擎 sigma.js + graphology + ForceAtlas2，向量搜尋 LanceDB（Rust embedded，可選）。

**跨平台支援**：macOS（Apple Silicon + Intel .dmg）、Windows（.msi）、Linux（.deb / .AppImage），GitHub Actions CI/CD 自動建置。

### 4 訊號知識圖譜完整權重

| 訊號 | 權重 | 說明 |
|------|------|------|
| Direct link | ×3.0 | `[[wikilinks]]` 直接連結 |
| Source overlap | ×4.0 | 共用同一原始來源（最高權重）|
| Adamic-Adar | ×1.5 | 共享鄰居節點（稀有共同鄰居權重更高）|
| Type affinity | ×1.0 | 同類型頁面加分（entity↔entity）|

**Louvain Community Detection**：自動發現知識群集，計算群組凝聚度評分（< 0.15 標記為警告），12 色視覺區分。

### Local HTTP API 端點（完整列表）

`http://127.0.0.1:19828`（token 保護，僅限 localhost）：
- `GET /api/v1/health` — 伺服器狀態（無需認證）
- `GET /api/v1/projects` — 列出專案
- `POST /api/v1/projects/{id}/search` — 混合搜尋（keyword + vector）
- `GET /api/v1/projects/{id}/graph` — wikilinks 知識圖譜
- `POST /api/v1/projects/{id}/sources/rescan` — 觸發重新掃描

**Agent Skill 一鍵安裝**：
```
npx skills add https://github.com/nashsu/llm_wiki_skill.git --skill llm_wiki_skill
```

### Vector Search 效益

啟用向量搜尋（LanceDB）後，整體 recall 從 58.2% 提升到 71.4%。預設關閉，在設定中獨立配置端點、API Key 和模型。

### 已驗證規模

nashsu 團隊在生產環境驗證：4,000+ 筆記、20,000+ 語意關係的知識庫，七階段 pipeline 含崩潰安全檢查點。
- [[claude-code-obsidian-setup]] — 本知識庫的 Claude Code + Obsidian 實作
- [[obsidian-knowledge-base-planning]] — 本知識庫整體規劃文件
- [[dual-tool-strategy]] — 多工具整合策略參考
