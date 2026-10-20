---
title: ds4：antirez 手寫的 DeepSeek V4 Flash 專屬推理引擎（本機零成本 250K context）
date: 2026-06-17
tags: [ai/agent, ai/claude]
source: https://vocus.cc/article/6a197c00fd89780001a5aa7f
type: article
status: processed
asset_value: 55
---

## T（核心論點）

Redis 作者 antirez（Salvatore Sanfilippo）為 DeepSeek V4 Flash 從頭手寫的專屬推理引擎。不是包裝 llama.cpp/Ollama，而是從 tokenizer 開始自己來。理由：DeepSeek V4 Flash 的 MoE 架構 + 極短 thinking mode + 1M token context 三者組合，讓通用引擎無法真正發揮。96GB RAM MacBook 可跑到 250K context，月費 NT$0，完全離線，MIT 授權。

## B（適用場景）

**兩類真正適合的用戶：**

1. **隱私合規需求的開發者**：金融、醫療、法律領域，程式碼不能離開本機。對他們來說 ds4 不是省錢工具，是「終於有解法了」的工具。

2. **硬體已到位的 AI 從業者**：手上已有高規格機器（96GB+ RAM），想榨乾硬體，把閒置資源變成隨手可用的離線 AI 助理。

**不適合：**
- 一般開發者（96GB RAM 門檻是真實的結構性限制）
- 需要最新模型特性的場景（ds4 只支援 DeepSeek V4 Flash 這條線）

## R（完整方案）

### 為什麼要造「這個」輪子

通用引擎（llama.cpp/Ollama）的 KV cache 是為 dense model 設計的，拿來跑 DeepSeek V4 Flash 有三個根本問題：

| DeepSeek V4 Flash 特性 | 通用引擎的問題 | ds4 的解法 |
|----------------------|--------------|-----------|
| MoE 架構（284B 參數，每次只啟動部分專家）| KV cache 吃不下這個架構，算力浪費 | 專為 MoE 設計的 KV cache 管理 |
| Thinking mode 只有其他模型的 1/5 | 通用引擎無法優化 thinking 長短 | 感知 thinking 長度，按複雜度動態配置 |
| 1M token context window | 消費級硬體無法在記憶體內處理 | KV cache **持久化到硬碟**（可跨 session 讀寫）|

> 不是造輪子。是發現這台車的輪子形狀跟別人不一樣。

### 規格數字

| 指標 | 數字 |
|------|------|
| 本地最大 context | 250K tokens（96GB RAM 實測）|
| 理論 context | 1M tokens |
| 月費 | NT$0（硬體成本另計）|
| 硬體門檻 | 96GB RAM（Mac）/ NVIDIA GPU |
| 授權 | MIT |
| GitHub 星數 | 12.4K（23 天，2026-05-29）|

### 功能

- **OpenAI-compatible server API**：可作為 Claude Code 或其他 coding agent 的後端，直接替換 API endpoint
- **內建 CLI 聊天介面**：也可直接當 coding agent 使用
- **三種硬體後端**：Mac Metal / NVIDIA CUDA / AMD ROCm（社群維護）
- **KV cache 硬碟持久化**：跨 session 記憶，不需要每次重新 load context

### ds4 vs deepclaude 的策略定位

| | deepclaude | ds4 |
|---|---|---|
| 跑在哪 | 雲端 API（OpenRouter / DeepSeek）| 本機（MacBook / GPU）|
| 做什麼 | 沿用 Claude Code 工作流，換便宜後端 | 完全離線跑一個模型 |
| 月費 | ~NT$416 | NT$0（硬體成本另計）|
| 隱私 | 資料過雲端 | 完全不離開本機 |
| 門檻 | 任何開發者 | 96GB+ RAM 機器 |

**互補策略**：白天用 deepclaude 雲端方案（方便），離線 / 處理敏感資料時用 ds4。不是二選一，是看場景切換。

### antirez 押注的是什麼

ds4 只支援 DeepSeek V4 Flash。若 DeepSeek V5 架構完全不同，ds4 價值歸零。antirez 在 README 寫：「我們期待 DeepSeek 發布更新版本的 V4 Flash」——這是對這條模型線長期價值的公開押注，不是保證 ROI 的工程決策。

> ds4 代表的不只是一個工具，而是「從租 AI → 擁有 AI」這條路上的第一個認真嘗試。

## C（關鍵行動）

- [ ] 如果有 96GB RAM 機器，測試 ds4 替換 Claude Code 後端：`openai_api_base=http://localhost:PORT`
- [ ] 有隱私合規需求的 codebase，評估離線方案可行性
- [ ] 關注 DeepSeek V4 Flash 更新線（ds4 的長期價值與這條線綁定）

## 相關連結

- [[agentmemory]] — Slow Signals 五件套第二件：跨 session 持久記憶（與 ds4 互補的離線策略）
- [[headroom-context-compression]] — Slow Signals 五件套第三件：送進模型前先壓縮
- [[deepclaude-cost-optimization]] — Slow Signals 五件套第一件：換便宜後端的雲端方案
- [[seven-agent-software-factory]] — CLAUDE.md 作為跨 session 記憶（與 ds4 KV cache 持久化互補的不同層次解法）
