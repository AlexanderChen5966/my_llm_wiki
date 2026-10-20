---
title: 工作拆解＋人機分工提示詞模板（五部分拆解表＋三級分工框架）
date: 2026-09-14
tags: [ai/prompt-engineering, ai/workflow]
source: https://www.bnext.com.tw/article/92222/openai-data-agent-work-decomposition
type: note
status: processed
asset_value: 27
---

可遷移到任何重複性專業工作的「工作拆解＋人機分工」提示詞方法。核心做法分三步：①用固定 prompt 請 AI 把工作拆成 5 部分（資料與背景、處理步驟、判斷原則、使用工具、交付標準），一次只問一題，整理成工作拆解表；②依三級界定人機分工——AI 可直接做（明確給資料與產出要求）、AI 先做人確認、必須由人判斷；③用過去真實案例實測，檢查哪步做錯、哪裡理解偏差，把缺漏的資料/步驟/判斷規則補回拆解表，穩定後再接資料庫/CRM/公司範本擴大自動化範圍。可直接複製的拆解 prompt 原文：「請協助我把這項工作拆成 5 個部分：資料與背景、處理步驟、判斷原則、使用工具、交付標準…一次只問我一題…最後整理成一份工作拆解表」。

（本篇僅萃取提示詞模板與分工框架，OpenAI Data Agent／金融業專屬版 ChatGPT 的產品新聞本體不收錄。）

## 相關連結
- [[ai-data-analysis-prompting]] — 同類「可直接複製的結構化提示詞」筆記，皆屬把模糊工作轉成可執行步驟的方法
