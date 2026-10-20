---
title: Unsloth Fine-Tune 實作：Google Gemma 2 9B 新聞標題生成
date: 2026-06-05
tags: [ai/workflow]
source: https://medium.com/@simon3458/google-gemma-2-fine-tune-news-generation-unsloth-tool-ab4a094909df
type: note
status: processed
asset_value: 18
---

## T（核心論點）

透過 Unsloth 工具在 Google Colab T4 GPU 上對 Gemma 2 9B 模型進行 Fine-Tune，以公視新聞資料集（1,400 筆）訓練新聞標題生成能力。Unsloth 號稱重寫後的模型計算結果與原始版本完全一致（無近似計算），簡化了 Fine-Tune 流程。實驗結果 Training Loss 持續下降，Fine-Tune 後模型在標題生成品質上有明顯提升。

## B（適用場景）

**適用：** 硬體資源有限的個人開發者（僅需 Google Colab T4 GPU）、中文 NLP 任務的模型微調實驗、學習 Fine-Tune 流程的入門練習
**不適用 / 注意：** 目前 Unsloth 僅支援單卡運算、資料集僅 1,400 筆屬小規模實驗

## 原始摘要

作者 Simon Liu（Google GenAI GDE）的 Medium 實作記錄。使用 Alpaca Prompt 格式，以新聞內容為 input、標題為 output 進行訓練。資料集來源：HuggingFace `AWeirdDev/zh-tw-pts-articles-sm`（公視新聞）。微調後模型已上傳 HuggingFace：`Simon-Liu/gemma2-9b-zhtw-news-title-generation-finetune`。

## 相關連結

- [[local-llm-deployment-overview]] - 本地 LLM 部署概覽
- [[ollama-2026-complete-guide]] - Ollama 2026 完整教學
