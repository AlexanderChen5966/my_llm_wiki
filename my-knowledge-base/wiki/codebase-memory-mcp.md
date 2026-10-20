---
title: Codebase Memory MCP：tree-sitter + Hybrid LSP 的本機程式碼知識圖譜
date: 2026-07-09
tags: [ai/mcp, dev/architecture, type/reference]
source: 合併自「Codebase Memory MCP 實測」「codebase-memory-mcp 教學：AI agent 省 99% token 的程式碼記憶（2026）」與官方 GitHub README（DeusData/codebase-memory-mcp）三篇
type: article
status: processed
asset_value: 66
---

`DeusData/codebase-memory-mcp`：零依賴、100% 本機執行的 MCP server，用 tree-sitter 解析＋Hybrid LSP 為整個程式碼庫建立可查詢的持久知識圖譜，讓 AI coding agent 用結構化查詢取代「grep 全文＋整檔讀入」；官方 arXiv 前印本（arXiv:2603.27277）測試 31 個真實專案，答案品質 83%、token 少 10 倍、tool call 少 2.1 倍；README 內另有五查詢對比數據：120 倍省 token（3,400 vs 412,000）。

## T（核心論點）

AI coding agent 目前理解程式碼庫的預設方式是「grep 關鍵字→讀整個檔案→塞進 context」，對大型專案極度浪費 token 且容易讀到不相關內容。Codebase Memory MCP 把程式碼庫先解析成符號（函式/類別/呼叫關係/HTTP 路由/跨服務連結）的知識圖譜存在本機，agent 之後改用「查詢符號關係」取代「讀全文」。**它本身不含 LLM**——只做結構分析後端，智能層仍由呼叫它的 MCP client（Claude Code 等）負責，這個設計避免了額外 API key/成本/模型設定。

## B（適用場景）

大型/中大型程式碼庫，AI agent 頻繁需要理解「這個函式被誰呼叫」「這個 class 的呼叫鏈」「git diff 影響範圍」等結構性問題；重視本機執行（程式碼不外流）與零額外依賴的團隊；多 Agent CLI 混用環境（一次 install 自動配置 11 種 agent）；作為 token 節省工具鏈的一環（與 Ponytail、pxpipe 同期出現於社群省 token 討論）。

## R（技術重點）

- **解析引擎**：158 種語言的 tree-sitter 語法樹解析（vendored 編譯進單一 binary）+ Hybrid LSP 語意層（結構上模仿 tsserver/pyright/gopls/Roslyn/Eclipse JDT/rust-analyzer 的型別解析演算法，但不啟動真正的 language server process），完整支援 Python/TS-JS-JSX-TSX/PHP/C#/Go/C/C++/Java/Kotlin/Rust 共 10 語言的深度型別解析
- **效能實測**（Apple M3 Pro）：Linux kernel（28M LOC/75K 檔案）完整索引 3 分鐘（481 萬節點/772 萬邊）；Cypher 查詢 <1ms；呼叫鏈追蹤（深度 5）<10ms
- **14 個 MCP 工具**：索引（index_repository/list_projects/index_status）、查詢（search_graph/trace_path/detect_changes/query_graph/get_architecture/search_code）、Cypher-like 唯讀查詢子集、ADR 管理、runtime trace 驗證等
- **零依賴分發**：單一靜態 binary（macOS/Linux/Windows），無需 Docker/API key，一鍵 install 自動偵測並配置 11 種 coding agent（Claude Code/Codex CLI/Gemini CLI/Zed/OpenCode/Antigravity/Aider/KiloCode/VS Code/OpenClaw/Kiro）
- **安全與信任**：100% 本機執行、零遙測；每個 release binary 經 VirusTotal 70+ 引擎掃描、SLSA Level 3 建置溯源、Sigstore cosign 簽章、CodeQL SAST
- **團隊共享圖譜**：可選擇把壓縮後的知識圖譜快照（`.codebase-memory/graph.db.zst`，zstd 壓縮 8-13:1）提交進 repo，隊友 clone 後省去完整重新索引
- **Token 節省數字的兩種版本**：README 內部示範（5 個結構查詢：3,400 vs 412,000 token，省 99.2%）與 arXiv 論文的第三方基準（31 個真實 repo，10 倍省 token、83% 答案品質、2.1 倍少 tool call）——後者是較嚴謹的第三方可驗證數據，前者是官方自行示範的極端案例，不宜直接當作平均可達成的節省率

## C（關鍵行動）

- [ ] 中大型專案若 AI agent 常因讀取整檔而超出 context 或反應慢，可評估導入
- [ ] 以 arXiv 論文的「10 倍省 token」作為較保守的預期基準，而非 README 示範的 99% 上限
- [ ] 多 Agent CLI 混用環境（Claude Code + Codex + Gemini CLI 等）可用一次 install 統一配置，是相對於單一 agent 工具鏈的差異化優勢
- [ ] 本機執行 + 零遙測 + 多層簽章驗證，適合對程式碼外流有疑慮或需要合規稽核的團隊

## 相關連結

- [[claude-fable-5-usage-guide]] — 同批社群討論串的省 Token 脈絡（Ponytail / pxpipe 並列提及）
- [[production-agent-architecture]] — Agent 架構中對 context 管理與 token 效率的一般性討論
- [[codegraph-ai-coding-agent]] — 同類型競品：CodeGraph（tree-sitter AST + SQLite，省 35% 成本/70% tool calls，另有獨立 86% token 節省 benchmark），可對照選型
