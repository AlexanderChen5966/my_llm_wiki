---
title: TPKI：三模式知識索引架構
date: 2026-06-02
tags: [pkm/framework, ai/agent]
source: https://github.com/zeuikli/TPKI-Tri-Pattern-Knowledge
type: article
status: processed
asset_value: 75
---

TPKI（Tri-Pattern Knowledge Index）是一套純 Markdown 的知識索引架構，整合 LLM Wiki、Hermes Agent、GBrain 三種模式的核心設計，讓 AI Agent 透過最多 3 次讀取（3-hop）定位任何知識節點。不需向量資料庫或 embedding，任何能讀 Markdown 的 Agent 都能使用。

## T（核心論點）

知識管理的瓶頸不是儲存，而是**定位**。TPKI 解法：建立三層跳轉架構 `BRAIN.md → INDEX.md → 具體檔案`，讓 Agent 以確定性路徑（非語意搜尋）在 3 步內找到任何知識節點。整合三個已公開模式各取核心：LLM Wiki 的三層分類、Hermes Agent 的 Skills 程序記憶、GBrain 的 Typed Edges 知識圖譜。

## B（適用場景）

**適用：**
- 中小規模知識庫（數百個節點），需要 Agent 快速定位
- 不想部署向量資料庫或 embedding 基礎設施
- 需要 git 版本控制的知識管理
- 跨 Agent 通用（Claude Code、Cursor、Copilot、Codex）

**不適用 / 注意：**
- 大規模知識庫（數千節點以上），BRAIN.md 查詢表會過長
- 需要模糊語意搜尋的場景（TPKI 是確定性索引，非向量搜尋）
- 需要即時外部資料的場景（仍需 MCP）

## R（架構與操作）

### 三層跳轉架構

```
BRAIN.md（Agent 入口查詢表）
  → Intent→Node 映射、三層節點地圖、Typed Edge 索引
  → <dir>/INDEX.md（目錄索引）
    → 具體檔案
```

### 檔案角色

| 檔案 | 角色 | 讀者 |
|------|------|------|
| `BRAIN.md` | Agent 入口：Intent→Node 查詢表 | Agent |
| `README.md` | 人類入口：repo 導覽 | 人類 |
| `TPKI-README.md` | 架構概念介紹 | 人類 |
| `TPKI-SPEC.md` | 完整規格 v1.0 | 人類 + Agent |

### 三個來源模式

| 模式 | 來源 | TPKI 取用核心 |
|------|------|-------------|
| LLM Wiki | Andrej Karpathy | Raw → Wiki → Schema 三層分類，確定性索引優先於 RAG |
| Hermes Agent | Nous Research (MIT) | Skills 作為 procedural memory，built-in learning loop |
| GBrain | Garry Tan (MIT) | Typed edges 知識圖譜，zero-LLM 建圖 |

### 節點類型

- `raw:indexed` — 不可變原始資料（論文、文章）
- `wiki:generated` — LLM 彙整報告
- `schema:runbook` — 操作 SOP / 流程

### Self-Update 規則（SPEC §1.6）

新增知識時同步更新三處：
1. `BRAIN.md` 查詢表
2. 對應 `INDEX.md` 條目與數量
3. Typed Edges（若有新關係）

## C（關鍵行動）

- [ ] 評估 TPKI 的 BRAIN.md → INDEX.md → 檔案 模式是否可融入現有知識庫的 wiki/INDEX.md 架構
- [ ] 比較 TPKI 與現有知識庫的差異：TPKI 多了 Typed Edges（語意關係標註）和 Intent→Node 映射，這是現有架構缺少的
- [ ] 參考 TPKI-SPEC.md 完整規格，考慮引入 Typed Edges 提升 wikilink 的語意精確度

## 原始摘要

zeuikli/TPKI-Tri-Pattern-Knowledge 是一套純 Markdown 知識索引架構，整合 LLM Wiki + Hermes Agent + GBrain 三種模式，讓 AI Agent 用最多 3 次讀取定位任何知識節點，不需向量資料庫。

## 相關連結

- [[llm-wiki-over-rag-claude-code]] — LLM Wiki 原始概念：取代 RAG 的知識管理方式
- [[karpathy-knowledge-base-method]] — Karpathy 三資料夾法（TPKI 的 LLM Wiki 來源）
- [[hermes-agent]] — Hermes Agent 框架（TPKI 的 Skills 記憶來源）
- [[gstack-framework]] — gstack 的 GBrain（TPKI 的 Typed Edges 來源）
- [[karpathy-llm-wiki-improvements]] — LLM Wiki 三大缺陷修正，TPKI 的 Typed Edges 呼應其中的「類型化 wikilink」
- [[synthesis-karpathy-llm-wiki-method]] — Karpathy LLM Wiki 方法精粹
