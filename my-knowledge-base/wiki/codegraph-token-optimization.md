---
title: CodeGraph - AI 寫程式省 80% Token 的 AST 索引工具
date: 2026-06-05
tags: [ai/workflow, ai/agent]
source: https://vocus.cc/article/6a0d4512fd897800017e53db
type: article
status: processed
asset_value: 48
---

## T（核心論點）

CodeGraph 是一個開源工具，透過掃描本地程式碼建立抽象語法樹（AST）並存入資料庫，讓 AI 需要查詢程式碼時可以精準定位函式和變數，而非讀取整個檔案。實測可節省約 86% 的 Token 消耗。它會自動監測檔案變動同步更新資料庫，並在 `.codegraph` 目錄中更新 LLM 對應的參考 MD（如 CLAUDE.md），讓 AI 自動使用而不需人工配置。

## B（適用場景）

**適用：** Token 耗盡焦慮的 Vibe Coding 使用者、中大型專案的 AI 輔助開發
**不適用 / 注意：** 需要 npm 環境安裝，小型專案可能 overhead 不值得

## 原始摘要

安裝方式：`npx @colbymchenry/codegraph`，在專案中 `codegraph init -i` 初始化，之後背景自動運作。作者 Scott Hsiao 實測確認可節省 86% Token。原理是 AST 索引取代全文讀取，與 [[claude-code-context-management]] 中「過濾源頭」的概念一致。

## 補充資料

### 官方 README Benchmark（2026-05-29 Opus 4.8 驗證）

GitHub 官方 README 提供更完整的 benchmark 數據，跨 7 個真實開源專案（7 種語言），每個專案 4 次 median：

**平均：25% cheaper / 57% fewer tokens / 23% faster / 62% fewer tool calls**

| 專案 | 語言 | 成本 | Tokens | 時間 | Tool calls |
|------|------|------|--------|------|------------|
| VS Code | TypeScript ~10k files | 33% cheaper | 70% fewer | 27% faster | 80% fewer |
| Excalidraw | TypeScript ~640 | 27% cheaper | 61% fewer | 26% faster | 70% fewer |
| Django | Python ~3k | 23% cheaper | 70% fewer | 28% faster | 77% fewer |
| Tokio | Rust ~790 | 35% cheaper | 70% fewer | 37% faster | 79% fewer |
| OkHttp | Java ~645 | 11% cheaper | 48% fewer | 26% faster | 70% fewer |
| Gin | Go ~110 | 15% cheaper | 35% fewer | 9% faster | 47% fewer |
| Alamofire | Swift ~110 | 28% cheaper | 46% fewer | 7% faster | 13% fewer |

核心原理：`codegraph_context` 映射區域 → `codegraph_explore` 取得相關原始碼，通常 zero file reads 就能回答架構問題。無 CodeGraph 時 Agent 把預算花在 grep/find/Read 探索上。

### 安裝方式（不需 Node.js）

```bash
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh

# 初始化專案
cd your-project && codegraph init -i
```

### CLI 參考

| 指令 | 用途 |
|------|------|
| `codegraph init -i` | 初始化並建立索引 |
| `codegraph status` | 檢查索引健康狀態 |
| `codegraph affected` | 追蹤受影響的測試檔案 |
| `codegraph serve --mcp` | 啟動 MCP server |

支援 20+ 語言（TypeScript / Python / Go / Rust / Java / Swift / Dart 等），支援 8 個 Agent（Claude Code / Cursor / Codex CLI / Hermes Agent / Gemini CLI / Kiro 等），支援 14 個 Web 框架的路由識別。

## 相關連結

- [[synthesis/synthesis-android-flutter-ai-toolchain]] — Android/Flutter AI 工具鏈知識精粹（本主題 12 篇整合，本篇 benchmark 為 Layer 2 依據）
- [[claude-code-context-management]] - Context 管控全攻略：過濾策略與 Token 節省
- [[llm-cost-optimization-strategies]] - LLM 成本優化四層策略
- [[dual-tool-strategy]] - 雙工具策略省 60-70% 費用
- [[understand-anything-code-knowledge-graph]] - Understand Anything：互補的程式碼視覺化理解工具
- [[code-review-graph]] — 同類 code graph 省 token 工具（本地 blast-radius 分析縮減 review context）
