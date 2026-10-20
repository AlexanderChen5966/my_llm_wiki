---
title: Planning Pattern 基礎：Plan-and-Execute 與其限制
date: 2026-06-16
tags: [ai/agent]
source: https://ithelp.ithome.com.tw/articles/10385063
type: note
status: processed
asset_value: 48
---

iThome 系列 Day 7，Planning Pattern 入門篇，是 [[react-planning-dynamic-adjustment]]（Day 21）演進前的基礎版本。

**Plan-and-Execute 四步驟：**
定義目標 → 分析限制 → 生成計畫（LLM 輸出 JSON 步驟列表，含 title/place/minutes/indoor 欄位）→ 執行並調整

**示範 1：** AI 生成 3 個景點的 JSON 行程，執行時模擬下雨，戶外移動時間 ×1.5 延長。

**示範 2：** 加入 `check_open(place)` 函式，當景點（美景宮）顯示「關閉」時，執行迴圈直接 `continue` 跳過該步驟——**這正是 Plan-and-Execute 的關鍵限制：只能刪除失敗步驟，無法像 ReAct 一樣重新規劃替代方案**。

此限制正是 Day 21 ReAct × Planning 結合要解決的問題，本篇作為對照組保留，核心解法已在 [[react-planning-dynamic-adjustment]] 完整展開，不需重複建立 wiki 條目。
