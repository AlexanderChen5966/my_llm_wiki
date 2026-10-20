---
title: Can I Run AI：本地 AI 模型硬體需求查詢工具
date: 2026-06-24
tags: [ai/agent, tool]
source: https://tools.wingzero.tw/article/sn/3941
type: reference
status: processed
asset_value: 38
---

Can I Run AI 是 AI 時代的「Can You Run It」——下載大型語言模型前，先確認自己的 GPU/RAM 是否符合需求，避免下載後跑不動。

## T（核心論點）

本地 LLM 的硬體需求差異極大（Q2 量化 vs Q8 原始差 3-4 倍 VRAM）。此工具提供直觀的相容性判斷，而非讓使用者自行換算原始規格表。

## B（適用場景）

- 購買顯示卡前（12G / 16G / 24G VRAM 應選哪一級）
- 評估是否可在本機跑特定開源模型（Llama / Qwen / Gemma / Mistral / DeepSeek）
- 企業評估推理伺服器配置需求

## R（使用方式）

輸入模型名稱 + 本機 GPU 規格 → 輸出：
- VRAM / RAM 需求
- 不同量化版本（Q2–Q8）比較
- 直觀相容性結果（可跑 / 剛好 / 不足）

**注意**：結果是估算，受推理框架（llama.cpp / Ollama）、OS、驅動程式、背景程式影響，不是絕對保證。

## 相關連結

- [[ollama-local-llm-setup]] — Ollama 本地 LLM 設定（搭配使用）
- [[local-llm-deployment-overview]] — 本地 LLM 部署概覽
