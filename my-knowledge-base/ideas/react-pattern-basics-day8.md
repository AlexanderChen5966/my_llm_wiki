---
title: ReAct 模式入門（Day 8）：TAO 循環基礎
date: 2026-06-17
tags: [ai/agent]
source: https://ithelp.ithome.com.tw/articles/10385814
type: idea
status: processed
asset_value: 35
---

ReAct 的 TAO 循環（Thought→Action→Observation）入門文章。AI 每步交替「推理」與「工具呼叫」，遇到景點休館能即時排除並動態調整行程——比 Planning Pattern（先規劃再執行）更靈活，因為 Planning 計劃一旦固定只能刪步驟，ReAct 則能邊查邊重排。

核心內容（Python + Gemini 2.5-flash 的 ReAct 循環 Demo，最多 8 回合，解析 `Action:` 動作→呼叫工具→塞 `Observation:`→繼續推理→Final Answer）已被後續更深入的條目涵蓋：[[react-planning-dynamic-adjustment]]（ReAct×Planning 結合）和 [[chain-of-thought-reasoning]]（ReAct 作為推理方法之一）。此為基礎導覽記錄，不另開 wiki 條目避免重複。

相關：[[react-planning-dynamic-adjustment]]、[[chain-of-thought-reasoning]]
