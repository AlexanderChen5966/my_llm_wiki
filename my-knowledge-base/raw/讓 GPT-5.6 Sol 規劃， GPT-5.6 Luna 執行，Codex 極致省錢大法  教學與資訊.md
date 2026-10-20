---
title: "讓 GPT-5.6 Sol 規劃， GPT-5.6 Luna 執行，Codex 極致省錢大法 | 教學與資訊"
source: "https://tools.wingzero.tw/articles/4006"
author:
  - "[[赤紅翼零]]"
published: 2026-08-03
created: 2026-08-04
description: "赤紅翼零的教學、工具與各種資訊網，還會有一些實驗性質的東西。"
tags:
  - "clippings"
status: processed
---
原來 Codex 可以啟用 **Luna Max 子代理（Subagent）** 。

- **GPT-5.6 Sol** 負責決策與規劃
- **GPT-5.6 Luna** 負責執行與處理大量工作

讓 **ChatGPT Plus** 也能發揮接近 **Pro 方案 20 倍** 的使用效果。

首先，前往 **設定 → 組態（Configuration）→ 可用推理強度（Available Reasoning Effort）** ，將 **Max** 選項開啟。

接著，把下面這段提示詞傳送給 **Sol** ：

請在 `~/.codex/agents/luna-worker.toml` 建立一個名為 `luna_worker` 的自訂代理程式。

使用以下設定：

```
model = "gpt-5.6-luna"
model_reasoning_effort = "max"
```

請為它加入適合執行範圍明確之委派工作的描述與指令。保留我其餘的設定不變，並根據我目前安裝的 Codex 版本驗證此設定是否有效。接著顯示設定變更差異（diff），之後在需要執行子代理任務時使用 `luna_worker` 。