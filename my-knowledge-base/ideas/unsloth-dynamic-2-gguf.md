---
title: Unsloth Dynamic 2.0 GGUF — 動態量化方法更新
date: 2026-06-17
tags: [ai/agent]
source: https://www.ithome.com.tw/news/174118
type: idea
status: processed
asset_value: 48
---

Unsloth 推出 Dynamic v2.0 量化方法，改變了 GGUF 量化模型的精度與檔案大小權衡方式。

**主要變化：**
- 針對每個可量化層動態選擇量化型別（非固定統一量化）
- 擴展至 MoE 與非 MoE 架構（之前只支援 MoE）
- 新版校準資料集（約 150 萬 Token，提升對話品質）
- 新增量化格式：Q4_NL、Q5.1、Q5.0、Q4.1、Q4.0（提升 Apple Silicon/ARM 相容性）

**實測結果（Gemma 3 27B）：**
- Dynamic 2.0 Q4_K_XL：MMLU 71.47，15.64 GB
- Google QAT 版：MMLU 70.64，17.2 GB
- Dynamic 2.0 以略小的檔案換到更高的分數

**對本地 LLM 使用者的意義：**
後續 Unsloth 發布的 GGUF 模型（供 llama.cpp / LM Studio 使用）都會採用 Dynamic 2.0，相同品質下模型檔可能更小，或相同大小下準確度更高。關注 [[ollama-local-llm-setup]] 和 [[ollama-2026-complete-guide]] 的本地 LLM 用戶可留意模型更新。
