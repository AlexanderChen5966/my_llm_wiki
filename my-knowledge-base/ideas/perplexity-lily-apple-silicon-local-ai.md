---
title: Perplexity Lily：開源 Apple Silicon 本機 AI 推論引擎
date: 2026-09-03
tags: [ai/agent]
source: https://www.aiposthub.com/perplexity-lily-apple-silicon-local-ai/
type: note
status: processed
asset_value: 42
---

Perplexity 開源 Lily，一套僅支援 Apple Silicon（M5+）與 Qwen3.6-35B-A3B 特定 4-bit 量化版本的專用本機推論引擎（Rust + Metal），放棄通用相容性換取效能，是 Perplexity Hybrid Compute（雲端＋本機混合運算，敏感檔案留本機處理）架構的技術基礎。核心可轉用判準：decode 吞吐量比 MLX 0.32.2 穩定快約 23.6%～31.9%，但 prefill 僅短提示領先，32K 以上長文反而落後（128K 慢 19.9%）——效能領先幅度會隨基準框架版本更新變動，不可只看單一時間點的測試數字下定論。一般使用者應優先用官方 App 而非自行編譯，Lily 的 OpenAI 相容 API 僅支援純文字、非串流、greedy decoding，無法取代 Ollama/LM Studio 等通用工具。若要深入查原文，重點看 prefill/decode 分階段最佳化手法與跟 MLX-LM 的架構比較表。

## 相關連結
- [[local-llm-deployment-overview]] — 本地 LLM 部署概覽，Lily 屬於其中專用推論引擎的最新個案
- [[ollama-2026-complete-guide]] — 通用本地 LLM 工具，與 Lily 的「專用換效能」取向形成對照
