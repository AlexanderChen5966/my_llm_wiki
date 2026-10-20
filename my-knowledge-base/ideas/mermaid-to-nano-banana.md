---
title: Mermaid 結構化 → Nano Banana 商務圖解清稿
date: 2026-06-17
tags: [ai/prompt-engineering]
source: https://codelove.tw/@tony/post/3kklB3
type: idea
status: processed
asset_value: 30
---

用 Mermaid 當「結構中介語言」，固定結構後再交給 Gemini Nano Banana Pro/2 清稿成商務圖解，消除工程師在「技術圖」和「商務簡報圖」之間手動重畫的成本。

**核心洞見：** 不要一開始就把長文丟給 AI——讓 AI 摘要會把關鍵元素刪掉。先用 Mermaid 固定骨架，再只讓 AI 做外觀清稿。

**系統提示詞（4句）：**
```
將輸入內容摘要並結構化，以日文哥德體生成資訊圖表圖片。請使用官方圖示進行視覺化。若輸入內容超過 30 個字，請先摘要並結構化，再進行圖片生成。背景為白色，不要標題。
```

**模型選擇（2026 年）：**
- Nano Banana（Gemini 2.5 Flash Image）：日文字容易崩壞
- **Nano Banana Pro**（Gemini 3 Pro Image）：適合資訊圖表 + 多語文字
- **Nano Banana 2**（Gemini 3.1 Flash Image）：Flash 速度 + 接近 Pro 品質

**踩雷：** 官方圖示精準度不保證（手動替換）；20-30 節點以上建議切成子圖（subgraph）分批處理。

**注意：** 輸入含機密架構資訊前先確認公司資料處理政策。
