---
title: Can I Run AI：本地 AI 模型硬體需求查詢工具
date: 2026-06-15
tags: [ai/agent]
source: https://tools.wingzero.tw/article/sn/3941
type: idea
status: processed
asset_value: 38
---

## 核心功能

Can I Run AI 是 AI 時代的「Can You Run It」。下載大型語言模型前，先確認自己的 GPU/RAM 夠不夠。

- 支援：Llama / Qwen / Gemma / Mistral / DeepSeek 等開源模型
- 評估：VRAM / RAM 需求 + 不同量化版本（Q2-Q8）比較
- 輸出：直觀的相容性結果，而非原始規格表

## 適用場景

- 購買顯示卡前（12G vs 16G vs 24G VRAM 如何選）
- 評估是否可以在本機跑特定模型
- 企業評估推理伺服器配置

## 注意

結果是估算，不是絕對保證。受推理框架、OS、驅動程式、背景程式影響。

## 相關

- [[ollama-local-llm-setup]] — Ollama 本地 LLM 設定
- [[local-llm-deployment-overview]] — 本地 LLM 部署概覽
