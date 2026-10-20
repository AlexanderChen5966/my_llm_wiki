---
title: "zeuikli/TPKI-Tri-Pattern-Knowledge"
source: "https://github.com/zeuikli/TPKI-Tri-Pattern-Knowledge"
author:
  - "[[zeuikli]]"
published:
created: 2026-06-02
description: "Contribute to zeuikli/TPKI-Tri-Pattern-Knowledge development by creating an account on GitHub."
tags:
  - "clippings"
status: processed
---
## TPKI — Tri-Pattern Knowledge Index

> 讓 AI Agent 用 **最多 3 次讀取** 定位任何知識節點的純 Markdown 索引架構。 此 repo 同時是 **規格文件** 與一套可直接使用的 **參考 scaffold** 。

```
BRAIN.md（Agent 入口查詢表）→ <dir>/INDEX.md（目錄索引）→ 具體檔案
```

不需向量資料庫、嵌入模型或特殊搜尋引擎——任何能讀 Markdown 的 Agent（Claude Code、Cursor、Copilot…）都能直接使用，並納入 git 管理。

---

## 這個 repo 有什麼

| 檔案 / 目錄 | 角色 | 給誰讀 |
| --- | --- | --- |
| [`BRAIN.md`](https://github.com/zeuikli/TPKI-Tri-Pattern-Knowledge/blob/main/BRAIN.md) | Agent 入口：Intent→Node 查詢表、三層節點地圖、Typed Edge 索引 | 🤖 Agent |
| [`README.md`](https://github.com/zeuikli/TPKI-Tri-Pattern-Knowledge/blob/main/README.md) | 本檔，repo 導覽說明 | 🧑 人類 |
| [`TPKI-README.md`](https://github.com/zeuikli/TPKI-Tri-Pattern-Knowledge/blob/main/TPKI-README.md) | TPKI 架構概念介紹（優缺點、比較、30 秒上手） | 🧑 人類 |
| [`TPKI-SPEC.md`](https://github.com/zeuikli/TPKI-Tri-Pattern-Knowledge/blob/main/TPKI-SPEC.md) | 完整規格 v1.0（檔案格式、導入 6 步、維護規則） | 🧑 + 🤖 |
| `research/papers/INDEX.md` | 不可變原始論文索引（ `raw:indexed` ） | 🤖 Agent |
| `research/reports/INDEX.md` | LLM 彙整報告索引（ `wiki:generated` ） | 🤖 Agent |
| `playbooks/INDEX.md` | 操作 SOP / 流程索引（ `schema:runbook` ） | 🤖 Agent |

```
TPKI-Tri-Pattern-Knowledge/
├── BRAIN.md                      # Agent 入口（必有）
├── README.md                     # 人類入口（本檔）
├── TPKI-README.md                # 架構概念介紹
├── TPKI-SPEC.md                  # 完整規格 v1.0
├── research/
│   ├── papers/INDEX.md           # raw:indexed
│   └── reports/INDEX.md          # wiki:generated
└── playbooks/INDEX.md            # schema:runbook
```

---

## 怎麼用

**Agent** ：從 [`BRAIN.md`](https://github.com/zeuikli/TPKI-Tri-Pattern-Knowledge/blob/main/BRAIN.md) 開始 → 依查詢表跳到對應 `INDEX.md` → 再讀具體檔案（最多 3 步）。

**人類** ：先讀 [`TPKI-README.md`](https://github.com/zeuikli/TPKI-Tri-Pattern-Knowledge/blob/main/TPKI-README.md) 理解動機與取捨，需要落地細節再看 [`TPKI-SPEC.md`](https://github.com/zeuikli/TPKI-Tri-Pattern-Knowledge/blob/main/TPKI-SPEC.md) 。

**新增知識** （Self-Update，SPEC §1.6）：放入檔案後同步更新 ① `BRAIN.md` 查詢表 ②對應 `INDEX.md` 條目與數量 ③ Typed Edges（若有新關係）。

---

## 三個來源模式

TPKI 整合三個已公開模式，各取核心（致謝歸於原作者，TPKI 非原創演算法）：

- **LLM Wiki** （Andrej Karpathy）— Raw → Wiki → Schema 三層分類，確定性索引優先於 RAG
- **Hermes Agent** （Nous Research, MIT）— Skills 作為 procedural memory，built-in learning loop
- **GBrain** （Garry Tan, MIT）— Typed edges 知識圖譜，zero-LLM 建圖

---

## License

MIT。可自由採用、修改命名慣例、只用部分模式、用於商業專案。公開分享你的 TPKI 實作時請標注來源。