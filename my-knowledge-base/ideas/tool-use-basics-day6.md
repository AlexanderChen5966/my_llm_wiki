---
title: Tool Use 基礎：四步驟模式與 RAG+工具組合示範
date: 2026-06-16
tags: [ai/agent]
source: https://ithelp.ithome.com.tw/articles/10384461
type: note
status: processed
asset_value: 40
---

iThome 系列 Day 6，Tool Use 的入門篇，內容已大致被更進階的 [[langchain-tool-use]]（Day 17）涵蓋，本篇保留作為基礎概念補充。

**Tool Use 四步驟模式：**
識別任務需要工具 → 呼叫工具（結構化參數）→ 接收結構化結果 → 整合成自然語言回覆

**示範 1：** 用 `google.generativeai`（非 LangChain）搭配簡單 if/else 模擬工具呼叫偵測，做最小可行的天氣查詢工具。

**示範 2：** RAG + Tool Use 組合——用 `sentence_transformers` + `faiss` 建立小型景點知識向量庫（美泉宮資訊），與模擬天氣工具一起餵入 Gemini，讓模型綜合「景點知識」與「即時天氣」給出行程建議（例如下雨時建議室內行程）。

定位為基礎示範，核心模式已在 [[langchain-tool-use]] 有更完整的 LangChain 框架實作，不需重複建立完整 wiki 條目。
