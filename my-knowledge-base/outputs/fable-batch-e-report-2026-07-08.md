---
title: Fable 批次 E 報告 — 四個個人專案健檢
date: 2026-07-08
tags: [ai/claude, pkm/workflow, project/knowledge-base]
type: reference
status: processed
---

# 批次 E 報告：四個個人專案健檢

> 執行：2026-07-08（Fable 5）｜範圍：kyoto-assistant / train-booking / preschool-finder-crawler / DownloadVideoPythonProject
> 結論：**四專案整體健康；唯一需修的是 kyoto 三份 RAG 方案文件缺狀態標記（提案未實施但看起來像現行文件）；train-booking 是四者中治理最乾淨的**
>
> ✅ **修正狀態（2026-07-08）**：E-1 已修——kyoto 三份 RAG 方案文件（ChromaDB / ChromaDB v2 / PageIndex）各加「📋 提案・尚未實施」狀態標頭（含三者互為替代選項與未實作佐證說明）。preschool MVP 狀態行為可選項，未動。

---

## 專案別結果

### 1️⃣ kyoto-assistant

| 檢查項 | 結果 |
|--------|------|
| `mcp_server/config.json` vs `server.py` | ✅ **九個 tool 一一對應**（hello/echo/attractions/restaurants/open_status/weather/route/plan/save）|
| `.env.example` vs config 宣告的 APIs | ✅ 四個 key 完整對應（3 required + 1 optional）|
| spec/ v4.1 系列 | ✅ 總覽 + Part1 + Part2 檔名版本一致（未逐字深查）|

**⚠️ E-1｜三份 RAG 方案文件無狀態標記（本批唯一需修項）**
`docs/` 的「執行任務-ChromaDB方案」「執行任務-ChromaDB整合方案(v2)」「執行任務-PageIndex方案」三份：
- 均為 **2026-03-03 同日產出的方案比較稿**（mtime 16:37 → 16:39 → 17:05，v2 最晚）
- **無任何日期／版本／採用狀態標頭**，讀者無從判斷哪份現行
- 佐證：`requirements.txt` 無 chromadb / pageindex 套件、src 無引用 → **三份都是「提案、尚未實施」**，但文件外觀像可執行的任務書
- **修法**：三份各加一行狀態標頭「📋 提案・尚未實施（2026-03-03 方案比較稿，與另兩份互為替代選項）」，或在 docs/ 放一份 README 說明三者關係

### 2️⃣ train-booking — ✅ 全數通過（四者中最乾淨）

| 檢查項 | 結果 |
|--------|------|
| CLAUDE.md 的 MCP Tools 表 vs `mcp_server.py` 實作 | ✅ **7 個 tool 一一對應** |
| 需求文件 v7.0 → 架構修訂版 | ✅ 修訂版（07-07）開頭明確聲明「對應原文件 v7.0、只重寫第三章與 F3、其餘沿用」——取代關係清楚 |
| PROGRESS.md | ✅ 更新至 07-07、明確指向修訂版、狀態誠實（待 dry-run 排練）|
| CLAUDE.md 核心原則 | ✅ 「LLM 不進搶票熱路徑」與修訂版設計一致 |

> 文件鏈環環註明（需求 → 修訂 → 進度 → CLAUDE.md），是個人專案文件治理的好範本。

### 3️⃣ preschool-finder-crawler — ✅ 通過

| 檢查項 | 結果 |
|--------|------|
| 「地圖整合與獨立資料庫架構」vs「欄位實測與整合規劃」| ✅ **互補而非矛盾**（前者定 DDL/RPC/爬蟲層、後者驗證可抓欄位與 App 功能）；表名 `childcare_centers` 等交叉一致 |
| 架構文件實作進度 | ✅ 有「實作進度（2026-07-03）」標記，Supabase migration 已落地並指向設定紀錄 |
| CHANGELOG | ✅ 活躍（07-03 裁罰爬蟲完成）|

**⚠️ 註記**：MVP 需求文件狀態行停在「v1.9｜06-08｜Phase 3 開發中」，與 CHANGELOG 的 07-03 進度相比略舊——若 Phase 狀態已推進可順手更新（非錯誤）。

### 4️⃣ DownloadVideoPythonProject — ✅ 通過

| 檢查項 | 結果 |
|--------|------|
| 兩個 skill vs `Skill存檔/general/` 副本 | ✅ **sign-off-generator-skill 與 media-processor 兩處逐字相同**（無版本漂移）|
| skill 結構完整性 | ✅ media-processor 含 SKILL.md + scripts×4 + README + LICENSE；sign-off 單檔完整 |

> description 觸發語意品質未深查（兩處同步為本批檢查重點，已通過）。

---

## 建議修正

1. **E-1**（唯一）：kyoto 三份 RAG 方案文件各加狀態標頭（事實標記：「提案・尚未實施」）
2. （可選）preschool MVP 文件狀態行更新

> 修正狀態：⬜ 待確認
