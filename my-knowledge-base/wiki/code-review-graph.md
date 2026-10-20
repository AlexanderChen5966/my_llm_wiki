---
title: code-review-graph：本地優先的程式碼知識圖譜，AI Review 只讀該讀的
date: 2026-07-20
tags: [ai/mcp, dev/architecture]
source: https://github.com/tirth8205/code-review-graph
type: reference
status: processed
asset_value: 72
---

`tirth8205/code-review-graph`（CRG）：本地優先（local-first）的程式碼智慧圖譜，透過 Tree-sitter 把 codebase 解析成 AST 圖（節點＝函式/類別/import，邊＝呼叫/繼承/測試覆蓋），存進本地 SQLite，review 時用「blast-radius 分析」只把改動真正影響的最小檔案集透過 MCP 餵給 AI，避免 AI 重讀整個專案。一行 `code-review-graph install` 自動偵測並設定所有支援平台。MIT 授權。

> ⚠️ **數據以本條目（GitHub README 英文原始）為準**。合併的兩篇中文來源數據與 README 不一致（詳見下方「三來源數據差異」），一律以 README 為權威。

## T（核心論點）

AI code review 的最大成本不是品質而是 token 浪費——AI 為理解上下文常重讀大量與改動無關的檔案。CRG 用 static analysis 先建一次完整依賴圖，把「哪些 context 是必要的」這個原本靠人手動拿捏的判斷自動化：改動一個檔案，圖會追蹤每個 caller/dependent/test 算出 blast radius，AI 只讀這些。對大型 monorepo 而言這不只是省錢，更是讓 AI review 從「不可用」變「可用」的關鍵。

## B（適用場景）

**適用：** 重度使用 Claude Code/Cursor/Codex 等做 review 的工程師；需快速掌握變更影響的 Tech Lead；大型專案/monorepo 維護者；想快速理解陌生架構的開源貢獻者；對 token 成本敏感的個人/新創；重視系統化 review（風險評估/測試覆蓋/死碼偵測）的團隊。
**不適用 / 注意：** 小 repo、瑣碎單檔改動（graph context 可能超過直接讀檔）、一次性問題——官方 FAQ 明列「When NOT to use it」。初始 build 對大型 monorepo 需數分鐘（一次性成本，後續增量更新 < 2 秒）。

## R（重點整理）

**技術管線**：Repository → Tree-sitter Parser → SQLite Graph → Blast Radius → Minimal Review Set。500 檔專案初始 build ~10 秒；2,900 檔專案增量 re-index < 2 秒（SHA-256 hash 找 dependents，只重解析變動檔）。

**安裝與 CLI**：`pip install code-review-graph` → `code-review-graph install`（自動偵測並寫入各平台 MCP 設定 + 注入 graph-aware 指令）→ `build`。核心命令：build/update/status/watch/visualize/wiki/detect-changes/register/daemon/serve。graph 存 `.code-review-graph/`（git 預設忽略）。有對稱的 uninstall（僅移除 CRG 自有檔案）。

**平台與工具面（README 權威數據）**：
- 支援 **15+ 平台**：Codex、Claude Code、CodeBuddy Code、Cursor、Windsurf、Zed、Continue、OpenCode、Antigravity、Gemini CLI、Qwen、Qoder、Kiro、GitHub Copilot（含 CLI）。
- **30 個 MCP 工具**（AI 自動使用）+ 5 個 MCP Prompts 模板（review/architecture/debug/onboard/pre-merge）。可用 `--tools`/`CRG_TOOLS` 在 token 受限環境限縮工具集。
- 語言覆蓋極廣（Python/JS/TS/Go/Rust/Java/C/C++/C#/Ruby/Kotlin/Swift/PHP/Scala/Solidity/Dart... + Jupyter/Databricks `.ipynb` + Vue/Svelte SFC），可透過 `.code-review-graph/languages.toml` 自加語言不需 fork。PHP 有框架感知（Composer PSR-4 / Blade / Laravel Route+Eloquent）。

**Benchmark（README 權威數據，6 個真實 repo / 13 commits，確定性可複現）**：
- Token 效率：**中位每問減量 ~82x**（whole-corpus baseline vs graph query），範圍 38x-528x。**528x 是最佳單一案例（fastapi），非典型**。典型 agent 問題圖回傳 ~2,000-3,500 tokens。
- Impact 準確度：**平均 F1 0.71**（precision 0.578）。「recall 1.0」是 graph-derived 循環上界（ground truth 來自同一張圖），非真「100% recall」；另有誠實的 co-change 模式並列，預期數字會低很多。
- 已知弱點（作者自陳）：小單檔改動 graph context 可能超過直接讀檔；search MRR 0.35 排序待改善；flow detection 33% recall（JS/Go 較弱）；impact 分析刻意保守（寧可誤報也不漏依賴）。

**GitHub Action / CI**：同一分析可作 composite Action，在 PR 貼 sticky 風險評分留言（可選 `fail-on-risk` 作 merge gate），且仍 local-first——圖在 CI runner 上建與查，不外送原始碼；零 telemetry，雲端 embedding 為 opt-in。

**適用情境補充（取自中文來源，情境非數據）**：解決 6 類問題（token 浪費/缺變更影響分析/增量更新困難/跨語言統一分析不易/缺結構化風險評估/新人上手困難）；適合 6 類使用者（AI 輔助開發工程師/Tech Lead/monorepo 維護者/開源貢獻者/重視品質團隊/成本敏感個人與新創）。

**⚠️ 三來源數據差異記錄**：本批次合併三份來源，數據不一致——
- 主份 GitHub README（英文，本條目採用）：中位 ~82x、最高 528x、F1 0.71、15+ 平台、30 MCP 工具、語言數以現行 parser 為準。
- Tony.Wu's Blog（中文）：稱平均 8.2x、最高 49x、23 種語言、28 MCP 工具。
- RepoInside（中文）：稱平均 8.2x、19 種語言（含 Vue/Solidity/Jupyter）。
中文兩份的 8.2x 可能對應官方另一個 benchmark（realistic agent baseline，非 whole-corpus baseline）或舊版本；**本條目一律以 README 為準**。

## C（關鍵行動）

- [ ] 對重度 AI review 的大型專案評估導入：`pip install code-review-graph && code-review-graph install`
- [ ] 引用效益時用「中位 ~82x、F1 0.71」而非「528x」或中文來源的「8.2x」
- [ ] 小 repo / 瑣碎單檔改動不需導入（參考官方「When NOT to use」）
- [ ] CI 場景可加 GitHub Action 作 PR 風險 gate，注意仍 local-first 不外送原始碼

## 原始摘要

三份來源合併：主份 GitHub README（英文原始，數據權威）+ Tony.Wu's Blog（中文，blast-radius 說明與 Claude Code review 工作流）+ RepoInside（中文，6 類問題/6 類使用者列表）。明確標注中文兩份數據（8.2x/49x/23 或 19 語言/28 工具）與 README（82x/528x/15+ 平台/30 工具）不一致，條目數據一律以 README 為準。

## 相關連結

- [[codegraph-token-optimization]] — CodeGraph：同類 AST 索引省 token 工具（可對照兩者 benchmark 方法差異）
- [[codegraph-ai-coding-agent]] — CodeGraph 語義導航層，與 CRG 同屬 code knowledge graph 路線
- [[understand-anything-code-knowledge-graph]] — 程式碼轉互動式知識圖譜，與 CRG 互補（視覺理解 vs review context 縮減）
- [[claude-code-review-harness-principles]] — /code-review 設計原則，可與 CRG 的 blast-radius review 搭配
- [[codebase-memory-mcp]] — 另一個 tree-sitter 本機知識圖譜 MCP，同類省 token 方案
