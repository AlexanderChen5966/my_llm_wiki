---
title: "Sakana AI Fugu／Fugu Ultra 上線：多代理協調基礎模型"
source: "https://abmedia.io/sakana-fugu-ultra-orchestration-foundation-model"
author:
  - "[[鏈新聞 ABMedia]]"
published: 2026-06-22
created: 2026-06-24
description: "日本 AI 公司 Sakana AI 6/22 釋出 Fugu 與 Fugu Ultra 多代理協調基礎模型，OpenAI 相容單一 API、Ultra 版在 SWE Bench Pro 達 73.7、超越 Claude Opus 4.8 的 69.2。"
tags:
  - "clippings"
status: processed
---
日本 AI 公司 Sakana AI 6 月 22 日推出多代理協調基礎模型「Fugu」與「Fugu Ultra」， [官方公告](https://sakana.ai/fugu-release/) 將其定義為「行為像單一模型的多代理系統」：使用者只需呼叫單一 OpenAI 相容 API，由 Fugu 內部判斷是直接回答、或動態調度多個專家模型協作完成多步任務。Sakana 同步公開兩篇登載於 ICLR 2026 的研究做為理論基礎：TRINITY（演化式 LLM 協調器，採 Thinker／Worker／Verifier 三角分工）與 Conductor（以強化學習方式發掘自然語言協調策略）。

## Fugu Ultra 在 SWE Bench Pro 拿下 73.7，超越 Claude Opus 4.8

根據 Sakana 公布的基準測試：在軟體工程任務 SWE Bench Pro 上，Fugu Ultra 得分 73.7、Fugu 為 59.0， [Claude Opus 4.8](https://abmedia.io/claude-ai-complete-guide-2026) 為 69.2；LiveCodeBench 上 Fugu Ultra 93.2、Opus 4.8 87.8；科學推理 GPQA-D 上 Fugu 與 Fugu Ultra 同為 95.5，勝過 Gemini 3.1 Pro 的 94.3；長上下文檢索 MRCRv2 則 Fugu Ultra 93.6、GPT-5.5 以 94.8 略勝。Sakana 形容 Fugu Ultra「與 Fable 5、Mythos Preview 等領先模型並肩」，但因 Fable 5 與 Mythos Preview 目前尚未公開，並未被納入 Fugu 的協調池。

## Sakana 定位：對抗「單一供應商依賴」風險

Sakana 在公告中明確點出，Fugu 鎖定的市場痛點是單一供應商依賴問題，並以「近期針對 Anthropic 模型的出口管制」為例，強調當企業重度押注單一前沿模型時，地緣政治、產品策略或服務中斷都會直接衝擊應用層。Fugu 將協調池設計為可抽換、可擴充，使用者也可選擇排除特定供應商（僅限 Fugu，Ultra 版不開放此選項）。Sakana 在文末以「最強的 AI 系統不會是孤立的單體，而是協作的生態」總結這項定位。

## 訂閱 20／100／200 美元、企業按量計費，暫不支援歐盟

訂閱制三檔分別為標準版 20 美元、Pro 版 100 美元、Max 版 200 美元，後兩者分別為標準版的 10 倍與 20 倍用量，雙模型都可使用；7 月底前訂閱可享次月免費。企業按量計費以 Fugu Ultra 為例，每百萬 token 輸入 5 美元、輸出 30 美元、快取輸入 0.5 美元，超過 272K context 時提高至 10／45／1 美元。Sakana 表示因 GDPR 合規仍在準備中，Fugu 目前不在歐盟與 EEA 區域提供服務。

風險提示

加密貨幣投資具有高度風險，其價格可能波動劇烈，您可能損失全部本金。請謹慎評估風險。

[![鏈新聞](https://abmedia.io/wp-content/uploads/2026/06/730_2003.png "鏈新聞")](https://www.bitget.com/events/activities/new/1153c9a8c79a5c4fa97a6fd6b978e766?color=dark&languageType=5)