---
title: Cognition 發表 Devin Voice 與自研 SWE-2 模型：全雙工語音結對編程
date: 2026-09-11
tags: [ai/agent]
source: https://www.aiposthub.com/cognition-devin-voice-swe-2-deep-dive-2026/
type: note
status: processed
asset_value: 18
---

Cognition 於 2026-09-10 發表 Devin Voice（全雙工語音結對編程，<250ms 延遲、毫秒級隨意打斷）與自研 SWE-2 模型（以 Kimi K3 2.8 兆參數 MoE 為基座做大規模強化學習，FrontierCode 1.1 Main 拿下 50.0%，逼近 Anthropic Fable 5.1 的 50.9%，推理成本卻降低約 64–70%）。核心可轉用之處是架構模式而非產品本身：「解耦式語音層」——前端輕量語音模型（GPT-Live-1）負責即時對話與打斷，後端重型模型（SWE-2）在隔離沙盒中做深度推理與程式碼實作，兩者透過非同步協議銜接，解決了語音延遲與編程深度之間的兩難；另一個可轉用點是評測方法論的批評視角：FrontierCode 以「可合併性」(Mergeability，維護者標準細則＋自動化靜態/動態檢查) 取代 SWE-bench，回應後者已出現的過度擬合與資料污染問題。內容屬第三方競品（Devin vs Claude Code/Cursor/Codex）新聞性深度解析，非本庫核心工具鏈，僅作架構模式與評測方法論的輕量參考。若要深入查 FrontierCode 評測細節或三段推理努力檔位（Medium/High/Max）設計，回原文。
