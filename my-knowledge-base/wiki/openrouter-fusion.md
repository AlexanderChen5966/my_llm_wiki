---
title: OpenRouter Fusion：多模型平行答題 + 裁判合成，預算面板擊敗單一頂級模型
date: 2026-06-23
tags: [ai/agent, ai/prompt-engineering]
source: https://www.koc.com.tw/archives/646493
type: note
status: processed
---

## T（主題脈絡）

2026 年中，OpenRouter 推出實驗性功能「Fusion」：讓多個 AI 模型同時回答同一個問題，由一個獨立裁判模型分析各方回答的共識點、矛盾處、獨特洞察與遺漏，再由合成模型寫出最終答案。這個架構讓三款平價模型的組合在深度研究任務中擊敗了 GPT-5.5 和 Claude Opus 4.8。

## B（核心主張）

**多模型交叉驗證本身就是強力的品質提升機制**，與使用哪些模型關係次要。同一個模型「與自身融合」，分數就能從 58.8% 跳到 65.5%（+6.7pp）。不需要換更貴的模型，換一個思考架構就夠了。

## R（關鍵洞察）

### 運作流程三步驟

1. **平行回答**：使用者 prompt 同時發送給多個模型（每個啟用網路搜尋）
2. **裁判分析**：獨立裁判模型讀取所有回答，結構化分析：共識點 / 矛盾處 / 獨特洞察 / 遺漏之處
3. **合成輸出**：合成模型根據裁判分析寫出最終答案

API 呼叫方式與單一模型完全相同（一次 API call，伺服器端完成）：

```js
{
  "model": "openrouter/fusion",
  "messages": [{"role": "user", "content": "你的問題"}]
}
```

### DRACO 基準測試結果（深度研究任務）

| 類型 | 組合 | 分數 |
|------|------|------|
| Fusion 最強 | Fable 5 + GPT-5.5（Opus 4.8 合成）| **69.0%** |
| 單一最強 | Claude Fable 5 | 65.3% |
| **Fusion 預算** | Gemini 3 Flash + Kimi K2.6 + DeepSeek V4 Pro（Opus 4.8 合成）| **64.7%** |
| 自我融合 | Opus 4.8 + Opus 4.8（自身 × 自身）| 65.5% |
| 單一 | Claude Opus 4.8 | 58.8% |
| 單一 | GPT-5.5 | 60.0% |

**預算面板（64.7%）的成本只有 Fable 5 的約一半**，距離最強單一模型不到 1 個百分點。

### 自訂面板 API

```js
{
  "model": "openrouter/fusion",
  "messages": [...],
  "plugins": [{
    "id": "fusion",
    "model": "google/gemini-3-flash-preview",
    "analysis_models": [
      "google/gemini-3-flash-preview",
      "moonshotai/kimi-k2.6",
      "deepseek/deepseek-v4-pro"
    ]
  }]
}
```

### 作為嵌入式工具

Fusion 也可作為「伺服器工具」嵌入現有工作流，基礎模型自動判斷何時啟動 Fusion：

```js
{
  "tools": [{"type": "openrouter:fusion"}]
}
```

適合只在高難度任務（深度研究、架構分析）時才觸發 Fusion，其餘問題走單一模型，節省成本。

### 限制

- 目前為實驗性功能，速度較單一模型慢（多模型平行 + 裁判分析）
- DRACO 是深度研究基準，Fusion 在其他任務類型的效益未必相同
- 預算面板仍需 Opus 4.8 做合成，並非完全低成本

## C（連結與延伸）

- [[synthesis-llm-cost-routing]] — LLM 成本優化：任務路由（弱規劃者 + 強執行者）vs 多模型 Fusion 的不同取徑
- [[claude-code-dynamic-workflows]] — Claude Dynamic Workflows（平行 subagent 派出），概念上與 Fusion 的平行回答類似
- [[loop-engineering-6-components]] — Worker + Verifier 分離架構（與 Fusion 的「回答者 + 裁判」架構互相呼應）
- [[model-fusion-dual-ai-decision]] — **不用寫程式的手動版**：「雙 AI 對帳法」三步（只用在不可逆決策／兩個不同廠牌獨立作答／只讀共識・分歧・丟棄），並收錄本頁 Fusion 跑分（Fable 5 + GPT-5.5 = 69.0% vs 單一 65.3%）與 Aider architect-editor 85%、Devin Fusion 成本降 30–40%；🔴 三組數字均為廠商自行公布、尚未第三方驗證
