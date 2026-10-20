---
title: Sakana AI Fugu：編排模型才是下一個前沿
date: 2026-06-24
tags: [ai/agent, dev/architecture]
source: https://techorange.com/2026/06/23/sakana-fugu-ai/
type: article
status: processed
asset_value: 68
---

Sakana AI 的 Fugu 不是又一個更大的語言模型，而是一個「編排模型」（orchestration model）：被訓練來調度一個可抽換的代理池，把複雜請求拆解委派給多個專家模型再整合輸出，對外呈現為單一 OpenAI 相容 API。日本 AI 的第三條路——不堆規模、不開源，而是「成為協調所有模型的那一層」。

## T（核心論點）

**路由（Routing）vs 編排（Orchestration）的本質差異**：

| 概念 | 機制 | 比喻 |
|------|------|------|
| **路由** | 分析 Prompt → 挑一個最佳模型 → 整個任務交過去 | 空中交通管制員（一次性決策）|
| **編排** | 拆解任務 → 多模型分工 → 邊推理邊委派 → 整合輸出 | 指揮家（全程協調）|

Fugu 屬於後者，技術基礎是 Sakana ICLR 2026 兩篇論文：
- **TRINITY**：Thinker / Worker / Verifier 三角分工，演化式 LLM 協調器
- **The Conductor**：強化學習讓模型自行摸索協調策略（非人工寫死路由規則）

## B（定位：對抗單一供應商依賴）

Fugu 鎖定的市場痛點是**模型供應鏈風險**——Anthropic 因出口管制下架 Fable 5 / Mythos 5 的事件，讓企業意識到押注單一模型的脆弱性。Fugu 把協調池設計為可抽換、可排除特定供應商（標準版 Fugu 支援，Ultra 版不開放）。

David Ha 的核心命題：「**編排模型才是下一個前沿，超越更大的模型。**」

## R（兩個版本 + 基準表現）

| 版本 | 特性 | 適合任務 |
|------|------|---------|
| **Fugu** | 低延遲，代理池較淺 | 日常寫程式、程式碼審查、聊天 |
| **Fugu Ultra** | 深度代理池，多步驟協作 | AI 研究、資安分析、多步驟專利調查 |

**SWE-Bench Pro 評分（軟體工程基準）**：

| 模型 | 分數 |
|------|------|
| Fugu Ultra | **73.7** |
| Claude Opus 4.8 | 69.2 |
| GPT-5.5 | 58.6 |
| Fugu | 59.0 |

其他：LiveCodeBench 93.2（Fugu Ultra）> Fable 5 89.8；GPQA-D 95.5 > Gemini 3.1 Pro 94.3。

**注意事項（重要）**：
- 數字由 Sakana 自行公布，尚未第三方獨立驗證
- Fugu 是調度系統，分數可能來自呼叫 Opus/GPT-5.5 再合成——本質是「模型的模型」成績
- Fable 5 與 Mythos Preview 因不對外開放，未被納入比較池（Fable 5 在 SWE-Bench Pro 仍達 80.0）

**Mark Studios 實測對比**（Three.js 遊戲開發）：

| | Fugu Ultra | Claude Opus 4.8 |
|--|-----------|----------------|
| 時間 | 22 分鐘 | 79 分鐘 |
| Token | ~8.9 萬 | ~94 萬 |
| 費用 | ~$7.32 | ~$37.85 |
| 品質 | 有小瑕疵 | 較佳 |

→ **速度與成本 Fugu 勝；品質與完整性 Opus 勝**

## 定價

| 方案 | 費用 |
|------|------|
| 標準訂閱 | $20/月 |
| Pro 訂閱 | $100/月 |
| Max 訂閱 | $200/月 |
| 按量（Ultra）| 輸入 $5/M tokens，輸出 $30/M tokens；超 272K context 漲至 10/45 |

目前不支援歐盟/EEA（GDPR 合規準備中）。

## C（關鍵洞見）

**調度層的天花板由池中模型的原始能力決定**——當 Fable 5 被排除在外，Fugu 能逼近卻難以超越前沿。

**Box CEO Aaron Levie 的觀察**：隨著前沿模型快速更迭，大量價值將集中在能有效進行模型路由與編排的中介層。

**批評（Prime Intellect Elie Bakouch）**：Fugu 建立在封閉模型之上，使用者無法掌握底層究竟使用哪些模型，未必等同於真正的 AI 主權。

## 相關連結

- [[concepts/agent-workflow-taxonomy]] — Agent 工具六大分類（Fugu 屬於編排層）
- [[openrouter-fusion]] — OpenRouter Fusion：類似多模型合成策略，但不同架構（路由+裁判 vs 端到端編排訓練）
- [[synthesis-llm-cost-routing]] — LLM 成本路由 Synthesis（相關主題）
