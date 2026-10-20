---
title: DBX：15MB 開源跨平台資料庫管理工具
date: 2026-06-22
tags: [tool]
source: https://tools.wingzero.tw/article/sn/3953
type: reference
status: processed
asset_value: 45
---

DBX 是輕量（15MB）開源資料庫管理工具，用 Tauri + Rust 建構，支援 15 種以上資料庫（MySQL/PostgreSQL/SQLite/MongoDB/Redis/DuckDB/ClickHouse 等）。

**亮點功能：**
- **AI SQL Assistant**：串接 OpenAI/Claude API，根據 Schema 自然語言生成/解釋/優化 SQL
- **ER Diagram 生成**：直接從資料庫結構產生 ERD，支援 SVG 匯出
- **類試算表編輯**：直接在資料表中增刪改，顏色區分未提交變更
- **DuckDB 內建**：拖曳 CSV/Parquet 直接瀏覽，不需先匯入資料庫
- **高風險指令確認**：DROP/DELETE/TRUNCATE 執行前要求確認

取代 DataGrip（付費）/DBeaver（重量）的輕量開源選項。適合個人開發者和不需要企業功能的場景。

GitHub: dbx-dev/dbx
