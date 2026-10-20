---
title: 為什麼選 LLM 而非強化學習作為 Agent 基礎
date: 2026-06-16
tags: [ai/agent]
source: https://ithelp.ithome.com.tw/articles/10382280
type: note
status: processed
asset_value: 45
---

iThome 系列 Day 3，概念性文章（無程式碼），解釋為何 Agentic AI 浪潮選擇 LLM 而非強化學習（RL，如 AlphaGo）作為核心引擎。

**核心論點：**
- RL（如 AlphaGo）需要明確定義的獎勵函數，適合封閉、單一評判標準的環境（棋盤遊戲輸贏分明）
- 開放性人類任務（如旅遊規劃）目標多維、難以量化成單一獎勵訊號，RL 難以泛化
- LLM 的三大優勢：
  1. **通用性**：不需針對每個任務重新訓練
  2. **語言介面**：不需設計獎勵函數，用自然語言描述目標即可
  3. **可組合性**：能搭配工具、記憶、規劃等模式靈活擴展，而非從零訓練

定位為系列基礎概念章節，後續 Day 4 起才進入 Tool Use、Memory、Multi-Agent 等具體模式，無需獨立 wiki 條目展開。
