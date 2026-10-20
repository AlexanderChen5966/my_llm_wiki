---
title: Headroom 實踐者視角：0% 本地實測與採用決策框架
date: 2026-06-17
tags: [ai/agent]
source: https://vocus.cc/article/6a125377fd897800010c43a1
type: idea
status: processed
asset_value: 45
---

Slow Signals 的 Headroom 實踐筆記，補充了 [[headroom-context-compression]] wiki 條目缺少的幾個重要細節：

**0% 本地實測的解釋**：官方 benchmark 漂亮（47-92%），但用純 library 模式 `compress()` 測小樣本全是 0%。原因是安全閘門：短訊息（<300 tokens）不壓、source code 預設 passthrough、optional dependency 缺失時 fallback 原文。0% 不是壞掉，是保護機制在工作。正確測法：走 proxy/wrap 模式，用完整使用模式（非小樣本）量測。

**CodeGraph / agentmemory / Headroom 的層次比較**：
- CodeGraph = 專案地圖（先理解 codebase 結構）
- agentmemory = 長期筆記本（跨 session 知識記憶）
- Headroom = 當下任務行李整理員（本輪 tool output 壓縮，避免 context 爆掉）

**7 天驗證節奏**：Day1 裝非關鍵專案 → Day2 JSON/API 密集任務 → Day3 除錯大量 log → Day4 issue triage → Day5 比對 token 省率 → Day6 日常任務 → Day7 決定全開或只在特定場景開。

**採用判斷**：每天有大量 tool call + 長 log/JSON/測試輸出 = 值得試；短問短答/Web ChatGPT = 無感；公司環境需先確認本地 proxy 合規。
