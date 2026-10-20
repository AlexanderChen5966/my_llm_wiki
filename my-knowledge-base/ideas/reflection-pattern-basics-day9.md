---
title: Reflection 模式入門（Day 9）：Review/Critique/Refine 循環
date: 2026-06-17
tags: [ai/agent]
source: https://ithelp.ithome.com.tw/articles/10386571
type: idea
status: processed
asset_value: 45
---

Reflection Pattern 基礎：AI 完成任務後執行 Review（回顧）→ Critique（檢討）→ Refine（改進）三步循環，讓模型不只解決當下問題，還能事後修正輸出。本文 Demo：AI 先生成「景點各停 5 小時」的行程導致 22:30 才結束，觸發 Reflection 後自動縮短時間、改為 18:00 前可行版本。

與 [[reflection-memory-learning-loop]] 的差異：本文是 Reflection 的獨立基礎版（單輪任務結束後自我修正），後者是 LangChain v1.0 將反思結果提煉成可重用決策準則後寫入 Memory，形成跨輪次的學習循環。兩者互補而非重複——前者解決「這次更好」，後者解決「下次不再犯」。

也可搭配參考 [[llm-memory-pattern]]（Write/Retrieve/Apply 三步循環）理解記憶如何延伸 Reflection 的生命週期。
