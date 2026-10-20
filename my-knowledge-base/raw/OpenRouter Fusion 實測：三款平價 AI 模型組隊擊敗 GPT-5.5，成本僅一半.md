---
title: "OpenRouter Fusion 實測：三款平價 AI 模型組隊擊敗 GPT-5.5，成本僅一半"
source: "https://www.koc.com.tw/archives/646493"
author:
  - "[[達小編]]"
published: 2026-06-19
created: 2026-06-23
description:
tags:
  - "clippings"
status: processed
---
AI 模型供應平台 OpenRouter 近日推出實驗性功能「 [Fusion](https://openrouter.ai/fusion) 」，讓多個 AI 模型同時回答同一個問題，再由一個「裁判模型」分析各方回答後融合出最佳答案。根據 OpenRouter 公布的基準測試結果，三款平價模型組成的「預算面板」在深度研究任務中擊敗了 GPT-5.5 和 Claude Opus 4.8 兩大頂級單一模型，成績僅落後最強單一模型 Claude Fable 5 不到 1%，成本卻只有約一半。這意味著，開發者或許不再需要砸大錢使用最昂貴的模型，只要聰明地組合幾款便宜模型，就能達到甚至超越頂級模型的表現，算是 AI 界的「三個臭皮匠勝過一個諸葛亮」的案例。  
![OpenRouter Fusion 實測：三款平價 AI 模型組隊擊敗 GPT-5.5，成本僅一半 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/06/20260619002144_0_ea33ab.jpg "OpenRouter Fusion 實測：三款平價 AI 模型組隊擊敗 GPT-5.5，成本僅一半 - 電腦王阿達")

## OpenRouter Fusion 運作原理：一個問題，多模型平行回答

Fusion 的核心概念借鑑自人類團隊協作的邏輯：讓多位「專家」各自提出觀點，再由一位「主持人」綜合分析後給出最佳答案。

具體流程分為三個步驟：首先，使用者的提示詞會同時發送給多個模型，每個模型都啟用網路搜尋功能；接著，一個獨立的「裁判模型」讀取所有回答，結構化分析各模型的共識點、矛盾處、獨特洞察與遺漏之處；最後，由一個合成模型根據這份分析撰寫最終答案。

對開發者而言，整個過程在伺服器端完成，只需一次 API 呼叫，使用方式與呼叫單一模型完全相同：

```js
{
  "model": "openrouter/fusion",
  "messages": [{"role": "user", "content": "你的問題"}]
}
```

使用者也可以自訂面板組合，選擇要用哪些模型進行分析、用哪個模型負責合成。

### DRACO 基準測試：便宜模型組合擊敗頂級單一模型

OpenRouter 選擇了 Perplexity AI 開發的 [DRACO](https://arxiv.org/abs/2602.11685) 深度研究基準測試來驗證 Fusion 的實力。DRACO 包含 100 個跨 10 個領域（法律、醫學、金融、產品比較等）的複雜研究任務，每項任務依據約 39 個加權標準評分，涵蓋事實準確性、深度、呈現品質與引用品質。特別的是，DRACO 對錯誤答案施加負分，無法靠「寫得多」來灌分。

測試結果如下：

![OpenRouter Fusion 實測：三款平價 AI 模型組隊擊敗 GPT-5.5，成本僅一半 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/06/20260619002411_0_d1c4f9.jpg "OpenRouter Fusion 實測：三款平價 AI 模型組隊擊敗 GPT-5.5，成本僅一半 - 電腦王阿達")

| 類型 | 模型組合 | DRACO 分數 |
| --- | --- | --- |
| Fusion | Fable 5 + GPT-5.5（Opus 4.8 合成） | **69.0%** |
| Fusion | Opus 4.8 + GPT-5.5 + Gemini 3.1 Pro（Opus 4.8 合成） | 68.3% |
| Fusion | Opus 4.8 + GPT-5.5（Opus 4.8 合成） | 67.6% |
| Fusion | Opus 4.8 + Opus 4.8（自我融合） | 65.5% |
| 單一模型 | Claude Fable 5 | 65.3% |
| Fusion（預算） | Gemini 3 Flash + Kimi K2.6 + DeepSeek V4 Pro（Opus 4.8 合成） | **64.7%** |
| 單一模型 | DeepSeek V4 Pro | 60.3% |
| 單一模型 | GPT-5.5 | 60.0% |
| 單一模型 | Claude Opus 4.8 | 58.8% |

最引人注目的結果有兩個。第一，由 Gemini 3 Flash、Kimi K2.6 和 DeepSeek V4 Pro 三款平價模型組成的「預算面板」拿到 64.7%，不僅擊敗了 GPT-5.5（60.0%）和 Opus 4.8（58.8%），更僅落後最強單一模型 Fable 5（65.3%）不到 1 個百分點，成本卻大約只有一半。  
![OpenRouter Fusion 實測：三款平價 AI 模型組隊擊敗 GPT-5.5，成本僅一半 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/06/20260619002523_0_19e6a5.jpg "OpenRouter Fusion 實測：三款平價 AI 模型組隊擊敗 GPT-5.5，成本僅一半 - 電腦王阿達")

第二，即使讓同一個模型「自己跟自己融合」也有顯著效果。Opus 4.8 與自身的副本組成面板、再由 Opus 4.8 擔任裁判合成，分數從單獨執行的 58.8% 飆升至 65.5%，一口氣進步 6.7 個百分點。這代表 Fusion 的效益有很大一部分來自「交叉驗證與綜合」這個動作本身，而不僅僅是混合不同模型架構。

### 自訂面板：彈性配置你的 AI 夢幻隊

Fusion 提供三種使用模式。在聊天室介面中，使用者可以直接選擇「品質」或「預算」預設面板，也能完全自訂 [\[1\]](https://openrouter.ai/blog/announcements/fusion-beats-frontier/) 。

自訂面板的 API 設定方式如下 [\[1\]](https://openrouter.ai/blog/announcements/fusion-beats-frontier/) ：

```js
{
  "model": "openrouter/fusion",
  "messages": [{"role": "user", "content": "..."}],
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

此外，Fusion 也能作為「伺服器工具」嵌入現有工作流程。開發者只需在工具陣列中加入 `{"type": "openrouter:fusion"}` ，原本使用的基礎模型就會自動判斷何時需要啟動 Fusion 來處理需要深度研究或架構分析的問題。

### 使用限制與注意事項

OpenRouter 在官方文件中坦率列出 Fusion 的限制：

首先是成本問題。Fusion 的費用等於所有面板模型加上裁判模型的 token 用量總和，雖然 OpenRouter 不加收手續費（按供應商原價計費），但多模型平行推理的花費自然比單一模型高出數倍。

其次是速度。由於需要平行呼叫多個模型再加上合成步驟，Fusion 的回應時間約為單一模型的 2 到 3 倍。

第三是基準測試的可信度問題。OpenRouter 自行承認，DRACO 測試中的 Fable 5 分數（65.3%）僅基於 100 個任務中的 93 個，因為有 7 個任務被其內容過濾機制擋下，與其他完成全部 100 個任務的模型相比並不完全公平。此外，測試過程中曾發現模型透過網路搜尋找到了 DRACO 的評分標準，OpenRouter 後來封鎖了相關網站並重新測試。評分使用的裁判模型（Gemini 3.1 Pro Preview）也與 DRACO 原論文不同，因此分數不能直接與原論文比較。

MakerStack 在 [評測](https://makerstack.co/reviews/openrouter-fusion-review) 中給予 Fusion 7.7 分（滿分 10 分），認為它最適合需要高品質輸出的研究型或高風險提示詞場景，對於日常大量生產工作流程則未必划算。

### OpenRouter Fusion 測試頁面（請點我）

## 對 AI 產業的啟示

Fusion 的出現代表一種思維轉變：與其追求單一更強的模型，不如讓多個現有模型「團隊作戰」。OpenRouter 在官方部落格中將此比喻為「模型的神經多樣性」（neurodiversity, but for models），強調不同模型各自擅長的領域互補後，整體表現能超越任何個體。

對一般開發者而言，最實際的啟示是：如果預算有限，與其單獨使用一個昂貴的頂級模型，不如考慮用 Fusion 組合多個平價模型。在 DRACO 測試中，三款預算模型的組合以約一半的成本達到了接近 Fable 5 的水準，這個性價比優勢不容忽視。

不過 Fusion 目前仍處於實驗階段（beta），OpenRouter 尚未將其標記為正式產品。對於需要穩定性的生產環境，這是一個需要納入考量的因素。

[**資料來源**](https://openrouter.ai/blog/announcements/fusion-beats-frontier/)