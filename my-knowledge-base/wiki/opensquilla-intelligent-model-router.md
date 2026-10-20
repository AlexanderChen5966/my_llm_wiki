---
title: OpenSquilla — 微核心 AI Agent 與 SquillaRouter 智慧模型路由
date: 2026-06-17
tags: [ai/agent, ai/mcp]
source: https://github.com/opensquilla/opensquilla
type: article
status: processed
asset_value: 62
---

每次 AI Agent 呼叫都付 Opus 的價格，但「查一個 function 文件」和「重構 2000 行 class」的難度天差地遠。OpenSquilla 是微核心 AI Agent，核心差異在 SquillaRouter：一個 on-device LightGBM+ONNX 分類器，每個 turn 自動判斷難度並路由到最便宜夠用的模型。

> 🔑 **同機制的商業實作對照（2026-08-05 補）**：Cognition 的 Devin Fusion 用「**lightweight classifiers during task execution** to signal when we need to switch to the main agent or use a different model entirely」，與本篇 SquillaRouter 是**同一個機制的兩種實作**——且 Devin 額外把切換時機對齊 **context compaction 點**，讓換模型不額外付 cache 代價。詳見 [[fable-5-delegation-cost-benchmark]]。
> **共同缺口也相同：兩者的路由準確率都無公開驗證。** 另注意本篇是微核心 agent **本體**、非 Claude Code 外掛，採用等於換 harness；Claude Code 上手動實作角色分級路由的配方見 [[fable5-subagent-model-routing-design]]。

## T（核心論點）

**OpenSquilla 的差異化是「right-sizing 每個 AI 呼叫」**。它不是另一個 agent loop，而是帶有智慧路由層的微核心架構：

- **SquillaRouter**：on-device 分類器（LightGBM + ONNX），根據 turn 長度、語言、程式碼、關鍵字與語意嵌入，把每個 turn 路由到 T0–T3 四個等級
- 路由決策在本機完成，prompt 不離開本地就已決定用哪個模型
- 同等任務品質下，官方 benchmark 成本約 1/9（$0.688 vs OpenClaw 的 $6.233）

Slow Signals 的「五件套」框架把它定位為第五件、最根本的一件：deepclaude（換後端）→ agentmemory（省重複背景 token）→ headroom（壓縮 tool output）→ ds4（完全本地）→ OpenSquilla（自動路由）。

## B（適用場景）

**最適合：**
- 每天大量使用 AI coding agent 且帳單讓人深呼吸的個人或團隊
- 需要同時接多個 LLM 後端、不想被單一廠商鎖定的架構
- 想要 Web UI + CLI + 聊天頻道統一行為的整合環境

**謹慎場景：**
- 高風險任務（金融交易、安全漏洞分析、醫療程式碼）：路由誤判把難題送小模型可能造成錯誤方向；手動指定模型更安全
- OpenSquilla 目前有 decision logging 但尚無公開路由準確率 benchmark

## R（完整內容）

### 架構核心：SquillaRouter 四層路由

| 等級 | 適用任務 | 範例 |
|------|---------|------|
| T0 | 極簡查詢 | 查天氣、查 function 文件 |
| T1 | 一般問答 | 簡單 debug、說明 error message |
| T2 | 中等複雜 | 寫測試、API endpoint 設計 |
| T3 | 複雜推理 | 架構重構、複雜 race condition debug |

**Adaptive reasoning**：只有路由分數高的 turn 才啟用 extended reasoning，system prompt 也依任務複雜度縮放。

### 20+ LLM 後端支援

OpenRouter、OpenAI、Anthropic、Ollama、DeepSeek、Gemini、DashScope/Qwen、Moonshot、Mistral、Groq、Zhipu、SiliconFlow、vLLM、LM Studio 等，primary + fallback 選擇。

### 統一 gateway 架構

- Web UI + CLI + 聊天頻道（Slack、Telegram、Discord、Feishu、DingTalk、WeCom、Matrix、QQ）共用同一個 TurnRunner
- tool dispatch、retry、decision logging 行為三端完全一致
- `127.0.0.1:18791` Starlette ASGI server

### 內建功能

- **持久記憶**：MEMORY.md + dated Markdown notes，SQLite FTS + sqlite-vec 語意搜尋，on-device ONNX embedding，可選 exponential decay + dream consolidation
- **分層安全沙盒**：Standard/Strict/Locked 三等級，Linux 用 Bubblewrap 隔離；denial ledger 自動暫停自主執行
- **15 個內建 Skills**：coding、GitHub、cron、PPTX/DOCX/XLSX/PDF、摘要、tmux、天氣等，按需載入
- **MCP 雙向**：可作為 MCP client 也可作為 MCP server（需 `mcp` extra）
- **排程**：SchedulerEngine + in-tree cron parser，`opensquilla cron` 管理

### PinchBench 1.2.1 基準（25 任務）

| Agent | 模型 | 平均分數 | 總成本 |
|-------|------|---------|--------|
| OpenSquilla | Router（Opus4.7+GLM5.1+DS4 Flash）| 0.9251 | **$0.688** |
| OpenClaw | Claude Opus 4.7 | 0.9255 | $6.233 |

### 安裝

```bash
# 推薦路徑（含 SquillaRouter）
uv tool install --python 3.12 "opensquilla[recommended] @ https://github.com/opensquilla/opensquilla/releases/download/v0.3.1/opensquilla-0.3.1-py3-none-any.whl"
opensquilla onboard
opensquilla gateway run
```

Windows 有 portable zip（無需 Python）：解壓後執行 `Start OpenSquilla.cmd`。

### 四種 AI Agent 設計哲學（Slow Signals 框架）

```
重量級單模型（Claude Code / Codex / Cursor）
    ↓ 付出：帳單直接跳
劫持式換後端（deepclaude）
    ↓ 付出：無法任務層級路由
完全本地（ds4）
    ↓ 付出：一次性硬體成本
微核心 + 智慧路由（OpenSquilla）
    ↓ 達成：自動選對的力氣做對的事
```

## C（關鍵行動）

- [ ] 安裝 `opensquilla[recommended]`，啟用 SquillaRouter（需 node 在 PATH）
- [ ] `opensquilla onboard` → 設定 provider + API key → `opensquilla gateway run`
- [ ] 觀察 decision logging：哪些 turn 被送到哪個 tier，驗證路由準確性
- [ ] 高風險任務手動指定模型，不交給路由器決定
- [ ] 搭配 [[headroom-context-compression]] 和 [[agentmemory]] 形成完整的「五件套」成本控制

## 相關連結

- [[headroom-context-compression]] — Headroom tool output 壓縮（五件套第三件）
- [[agentmemory]] — 跨 session 持久記憶，省重複背景 token
- [[deepclaude-cost-optimization]] — 劫持式換後端（五件套第一件）
- [[synthesis-llm-cost-routing]] — 任務路由成本優化理論（AgentOpt：弱規劃+強執行 74.27% vs 純 Opus 31.71%）
- [[openhuman-personal-ai-agent]] — OpenHuman 同樣有模型路由與持久記憶，偏向個人助手場景
- [[jev-system-one-model-concept]] — TypeSafe Jev：以專用「判斷模型」取代 LLM 做前置分流／路由的另一條路線（實作見 [[jev-implementation-guide]]）
