---
title: LLM Wiki Skill：Claude Code + Obsidian 的第二大腦維護器（claude-skills 版）
date: 2026-11-03
tags: [pkm/workflow, pkm/obsidian, ai/claude]
source: https://alirezarezvani.github.io/claude-skills/skills/engineering/llm-wiki/#iron-rule
type: reference
status: processed
asset_value: 80
---

這是 `alirezarezvani/claude-skills` 專案中 `engineering/llm-wiki` 這個 skill 的說明頁（作者 Alireza Rezvani），把 Karpathy 的 LLM Wiki 模式封裝成可安裝的 agent skill，支援 Claude Code、Codex CLI、Gemini CLI、OpenClaw。本篇是「現成工具包」層級的素材：除了方法論，還附斜線指令、子代理與純標準函式庫的 Python 腳本。

## T（核心論點）

多數「LLM＋文件」流程是 RAG：查詢時撈片段、從零合成、用完即忘。LLM Wiki 反過來走**複利路線**：來源只讀一次，整合進持久、互相連結的 markdown 知識庫並持續維護，交叉引用、矛盾標記與綜合分析在查詢前就已存在。人負責策展與提問，LLM 負責閱讀、歸檔、交叉引用與維護。

> Obsidian 是 IDE，LLM 是程式設計師，wiki 是 codebase。

## B（適用場景）

**適用：**
- 長期累積型主題：數週的研究深潛（論文／文章／報告、論點演進）
- 個人追蹤（目標、健康、日記）、讀書伴讀（章節、人物、主題線）
- 團隊內部 wiki（Slack、會議記錄餵入，由 LLM 做沒人想做的維護）
- 競品分析、盡職調查、課程筆記等「會持續加來源」的場景

**不適用 / 注意：**
- 只要對固定文件做一次性問答（用 RAG）
- 不打算長期加入新來源
- 不想讓 Obsidian 參與流程
- 規模界線（作者自述）：約 100 份來源／數百頁以內，`index.md`＋檔案系統搜尋就夠；超過再接 qmd 或 `wiki_search.py`（BM25）；逾約 500 頁建議外掛檢索層（搭配 rag-design skill）。此為作者經驗值，未見實測數據

## R（操作步驟）

**三層架構**
1. `raw/`：不可變來源（文章、PDF、圖片），LLM 只讀不寫
2. `wiki/`：LLM 擁有，含 `index.md`（內容目錄，每次 ingest 更新）、`log.md`（只追加時間線，格式 `## [YYYY-MM-DD] <op> | <title>`），以及 `entities/`、`concepts/`、`sources/`、`comparisons/`、`synthesis/` 子夾
3. `CLAUDE.md`／`AGENTS.md`：schema（慣例、流程、frontmatter 規則），由人與 LLM 共同演進；Codex／Cursor／Antigravity／OpenCode／Gemini CLI 讀 AGENTS.md，舊 Cursor 讀 `.cursorrules`

**三個核心操作**
1. **Ingest**：讀來源 → 與你討論重點 → 寫來源摘要 → 更新 10–15 個相關頁 → 更新 index → 追加 log
2. **Query**：先讀 `index.md` 再深入相關頁，帶引用合成答案；好答案回存為新頁，讓探索也能複利
3. **Lint**：找矛盾、過時主張、孤兒頁、缺漏交叉引用、被提及卻沒有獨立頁的概念，以及可用網搜補的資料缺口

**隨附工具**
- 斜線指令：`/wiki-init`、`/wiki-ingest <path>`、`/wiki-query`、`/wiki-lint`、`/wiki-log`
- 子代理：`wiki-ingestor`（提案後經你核准才套用）、`wiki-linter`、`wiki-librarian`（index 優先搜尋）
- Python 腳本（僅標準函式庫）：`init_vault`、`ingest_source`、`update_index`、`append_log`、`wiki_search`（BM25）、`lint_wiki`、`graph_analyzer`、`export_marp`
- Obsidian 建議：Web Clipper 剪進 `raw/`、附件資料夾設 `raw/assets/` 並綁快捷鍵下載附件、Graph view 看樞紐與孤兒、Marp（投影片）、Dataview、vault 用 Git 版控
- 鐵則：**LLM 永不編輯 `raw/`**，要修正來源由人自己改再重新 ingest

## C（關鍵行動）

- [ ] 與本庫現行機制逐項對照：本庫已實作 raw/→wiki/ 與 INDEX，但沒有 `log.md` 式的只追加時間線，評估是否需要
- [ ] 本庫的 raw-pipeline／knowledge-organizer／wiki-health-check 已對應 ingest／lint，查詢側（把好答案回存）可再檢視
- [ ] 若要借用，優先看 `references/lint-workflow.md` 與 `graph_analyzer.py` 的孤兒／樞紐統計

## 原始摘要

claude-skills 專案的 llm-wiki skill 說明頁（2026-11-03 剪藏）。內容為單一作者的工具說明，非獨立實測；Karpathy gist 為其自述靈感來源，頁內「Related skills」另提 para-memory-files、obsidian-vault（mattpocock）、rag-design、mcp-design、agent-communication。與本庫既有方法論條目的差別：既有條目多為文章式的概念與案例，本篇是可直接安裝的 skill 包規格（指令、子代理、腳本、模板）。

## 相關連結

- [[llm-wiki-over-rag-claude-code]] — 同一模式的方法論版（Claude Code＋Prompt 取代 RAG）
- [[synthesis/synthesis-karpathy-llm-wiki-method]] — Karpathy LLM Wiki 方法論知識精粹
- [[karpathy-knowledge-base-method]] — Karpathy 三資料夾法原始設計
- [[llm-wiki-karpathy-vs-nashsu]] — 抽象方法論 vs 桌面 App 實作的比較
- [[karpathy-llm-wiki-improvements]] — LLM Wiki 三大結構性缺陷與改進
- [[llm-wiki-code-as-source-of-truth-case-study]] — Lint 兩層區分與 docs drift 案例
- [[wikibrain-llm-wiki-implementation-patterns]] — 操作層實作案例
- [[claude-code-obsidian-setup]] — Claude Code＋Obsidian 建構指南
- [[obsidian-web-clipper]] — 剪藏進 raw/ 的前處理工具
- [[claude-skills-ecosystem]] — claude-skills 專案在 Skills 生態中的定位
