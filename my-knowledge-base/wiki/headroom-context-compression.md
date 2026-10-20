---
title: Headroom：AI Agent 的 Context 壓縮層
date: 2026-06-15
tags: [ai/agent, ai/mcp]
source: https://github.com/chopratejas/headroom
type: reference
status: processed
asset_value: 68
---

Headroom 在 tool outputs / logs / RAG chunks / files 到達 LLM 之前做壓縮，實測可節省 60-95% tokens，且答案品質不降低（benchmark GSM8K ±0.000、TruthfulQA +0.030）。三種接入模式：Library（代碼內嵌）/ Proxy（零改動）/ Agent Wrap（一行指令）。

## T（核心論點）

**AI agent 最貴的不是「產生答案」，而是探索成本（大量 tool outputs、logs、RAG chunks）**。Headroom 在這些內容進入 LLM 之前做壓縮，而非在 LLM 層做任何改動，因此答案品質不受影響。

關鍵差異：Headroom 的壓縮是**可逆的**（CCR 機制），原始內容永不刪除，LLM 可按需 `headroom_retrieve` 取回。

## B（適用場景）

- 每天重度使用 AI coding agent，tool outputs 消耗大量 token
- 同時跨多個 agent（Claude + Codex + Gemini），需要共享 memory
- 需要可逆壓縮（原始資料必須保留）
- 不想改現有代碼，只需零改動接入

## R（完整內容）

### 官方 Benchmark（依內容類型）

| 內容類型 | 壓縮前 | 壓縮後 | 節省率 |
|---------|-------|-------|--------|
| JSON 陣列（100 筆） | 3,163 tokens | 297 | **90.6%** |
| JSON 陣列（500 筆） | 9,526 | 1,614 | **83.1%** |
| Shell 輸出（200 行） | 3,238 | 469 | **85.5%** |
| 建置日誌（200 行） | 2,412 | 148 | **93.9%** |
| grep 結果 | — | — | **0%（設計如此）** |
| Python 原始碼 | — | — | **0%（設計如此）** |

grep 和程式碼 0% 是刻意設計：「本來就已精簡的格式若再壓縮，會破壞正確性」。

### ⚠️ 實際使用的數字（與宣傳值的落差）

官方統計 50,000+ 次真實 session（2026 年 3-4 月）：

| 百分位 | 實際節省率 |
|--------|-----------|
| P25 | 4.8% |
| **P50（中位數）** | **4.8%** |
| P75 | 6.9% |
| 平均 | 11.3% |

**中位數只有 4.8%**，因為真實 session 大量混有「短對話回合」。**包含大量 file 讀取或 shell 輸出的重度工具使用 session，減量可達 40-80%。**

開發者實測案例：Claude Code 月費 $287 → 導入 Headroom 後降至 $110（自述數字）。

### 效果有限的場景

- 從原始碼生成設計文件（程式碼在 analyze/review/debug 關鍵字觸發時自動受保護，不壓縮）
- 從設計文件生成程式碼（RAG 文件直接通過，零壓縮；文字壓縮增加延遲）

### 工作負載節省（實測）

| 工作負載 | 壓縮前 | 壓縮後 | 節省 |
|---------|-------|-------|------|
| Code search（100 筆結果）| 17,765 tokens | 1,408 | **92%** |
| SRE incident debugging | 65,694 | 5,118 | **92%** |
| GitHub issue triage | 54,174 | 14,761 | **73%** |
| Codebase exploration | 78,502 | 41,254 | **47%** |

### 三種接入模式

```bash
# 1. Agent Wrap（最簡單，一行）
headroom wrap claude

# 2. Proxy（零改動，任何語言）
headroom proxy --port 8787

# 3. Library（代碼內嵌）
pip install "headroom-ai[all]"
from headroom import compress
```

### 壓縮引擎架構

```
輸入（tool outputs / logs / RAG / files）
    ↓
ContentRouter（偵測類型，選引擎）
├─ SmartCrusher   → JSON（陣列/巢狀物件）
├─ CodeCompressor → AST-aware（Python/JS/Go/Rust/Java/C++）
└─ Kompress-base  → 純文字（HuggingFace 模型）
    ↓
CacheAligner（穩定 prefix，命中 provider KV cache）
    ↓
壓縮後 prompt → LLM（Anthropic / OpenAI / Bedrock）
    ↓
CCR 本地儲存（可逆，LLM 按需取回原始內容）
```

### MCP 工具（3 個）

- `headroom_compress`：壓縮輸入
- `headroom_retrieve`：取回原始內容
- `headroom_stats`：查看壓縮統計

### Cross-Agent Memory

多個 agent 共用同一個壓縮記憶 store，自動 dedup：

```python
# Claude 寫入
SharedContext().put("task_context", data)

# Codex 讀取
ctx = SharedContext().get("task_context")
```

### `headroom learn`

挖掘失敗 session，自動把修正建議寫入 `CLAUDE.md` / `AGENTS.md` / `GEMINI.md`——讓錯誤只犯一次。

### Agent 相容性

Claude Code ● / Codex ● / Cursor ● / Aider ● / Copilot CLI ● / OpenClaw ●

### 相較其他方案

| | 覆蓋範圍 | 部署 | 本地 | 可逆 |
|--|---------|------|------|------|
| **Headroom** | 所有 context | Proxy/Library/MCP | ✓ | **✓** |
| RTK | CLI 輸出 | CLI wrapper | ✓ | ✗ |
| OpenAI Compaction | 對話歷史 | Provider-native | ✗ | ✗ |

### ⚠️ 企業使用前的安全注意事項

- **遙測預設 ON**：`HEADROOM_TELEMETRY=on`（匿名使用統計），企業環境建議關閉：`export HEADROOM_TELEMETRY=off`
- **可能對 LAN 開放**：proxy 模式文件範例有 `--host 0.0.0.0`（對所有 LAN 介面開放），但預設環境變數是 `HEADROOM_HOST: 127.0.0.1`（僅本機），不要照抄文件範例
- 壓縮處理本身是本地端完成，不會額外把資料送到外部伺服器

## C（關鍵行動）

- [ ] 試跑 `headroom wrap claude` 觀察**實際**（而非理論）token 節省（預期 P50 約 5%）
- [ ] 重度工具使用 session（大量 file 讀取 / shell 輸出）才是 Headroom 的發揮場景
- [ ] 若跨 Claude + Codex，設定 SharedContext 共享記憶
- [ ] 啟用 `headroom learn`，讓失敗 session 自動更新 CLAUDE.md
- [ ] 企業環境：`export HEADROOM_TELEMETRY=off`

## 相關連結

- [[codegraph-ai-coding-agent]] — CodeGraph：語義導航層（與 Headroom 互補，前者減少 tool calls，後者壓縮輸入）
- [[context-engineering-fundamentals]] — Context Engineering 基礎概念
- [[llm-cost-optimization-strategies]] — LLM 成本優化四層策略
- [[claude-code-context-management]] — Claude Code context 管控
