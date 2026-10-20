---
title: AI 工具比較互動 HTML Dashboard：2 個 Prompt 技巧
date: 2026-06-25
tags: [ai/prompt-engineering, ai/tools]
source: https://www.bnext.com.tw/article/91309/3kduxgdvqjk9ummr7sqc
type: idea
status: processed
---

用 2 個 Prompt 請 AI（Claude/ChatGPT/Gemini 皆可）搜集各大 AI 工具比較資料，再輸出成可互動的 HTML Dashboard，點選任務類型即顯示推薦工具。

## 兩個關鍵 Prompt

**Prompt 1：資訊搜集**
```
你是 AI 工具研究員。幫我整理以下AI工具（ChatGPT、Claude、Gemini、Copilot、Perplexity、Grok）的比較資料。我常請AI做的任務包含腦力激盪、撰寫文案、做簡報、整理資料表、搜集資料等，需依照我想要的任務情境，輕鬆看出適合哪些AI工具、推薦理由（一句）、注意事項（一句）、這個任務需要哪些功能、要不要付錢。功能可能包含網路搜尋、圖片生成等，其他的你幫我思考，資料若不確定請標 ⚠️需確認，並附上資料來源的連結供我查證。
```

**Prompt 2：輸出互動網頁**
```
做成互動 HTML dashboard，我以後點選任務，即時顯示推薦工具卡片（首選/可用）和相關理由，並根據我的任務狀況推薦我訂閱項目和使用建議。開始之前，先說明這個任務如何向你提問更好？
```

## 各家 AI 引導風格差異

- **Gemini**：要求說清楚角色/情境/核心任務/限制條件
- **Claude**：釐清如何推薦工具、訂閱是否保留變更彈性
- **ChatGPT**：問最細（訂閱項目/工作項目/預算上限），成品最複雜

## 使用建議

第一版通常不完美，視覺化後更容易評估缺口，再針對性追問 AI 修改。
