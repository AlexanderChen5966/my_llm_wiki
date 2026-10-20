---
title: Fable 5 怎麼用？官方提示詞、effort 分檔、費用試算與長任務 agent 設計
date: 2026-07-08
tags: [ai/claude, ai/prompt-engineering, ai/agent]
source: https://www.bnext.com.tw/article/91415/claude-fable-5-guide
type: article
status: processed
asset_value: 60
---

Fable 5 的能力要靠「effort 分檔 + 驗證迴圈 + 官方修正提示詞」三件事搭配才發揮得出來；沿用前代模型的下指令習慣，只會低估它、或讓它自作主張改東西。

## T（核心論點）

Fable 5 是拿來處理「原本對前代模型太難、太長、太模糊」的端到端長任務，但發揮能力的關鍵不在模型本身，而在使用者有沒有依它的行為特性重新設計 effort 設定、驗證機制與提示詞。所謂「長時序」不是單一 API 請求跑好幾天，而是搭配外圍流程、工具、記憶、跨多回合累積完成的長任務。

## B（適用場景）

**適用：** 長時序自主任務、定義清楚的複雜問題、視覺辨識、企業文件工作流（財務分析/試算表/簡報）、程式碼審查與除錯、模糊需求判斷、併發子代理調度——這 7 項是官方點名相對 Opus 4.8 明顯進步的面向。

**不適用 / 注意：** 只拿去跑簡單任務會低估它的能力範圍；攻擊型資安、生物與生命科學、萃取模型推理摘要三類請求會被安全分類器攔截；前代模型寫的 skills 直接搬過來用常「過度規範」，逐步指揮反而拖累品質。

## R（操作步驟）

1. **設 effort**：由高到低 max/xhigh/high/medium/low，一般從 high 起手，能力吃緊才拉 xhigh；effort 是行為訊號不是 token 預算，硬性上限是 `max_tokens`（涵蓋思考+回答總量，高 effort 建議起點 64K）。
2. **抓費用基準**：輸入 $10/M token、輸出 $50/M token；prompt caching 寫入 5 分鐘 $12.5/M、1 小時 $20/M，命中讀取僅 $1/M（省約 9 成，但仍要收費）；Fable 5 用新版 tokenizer，同段文字比 Opus 4.7 以前多產生約三成 token，估算成本需重新計算。
3. **建驗證迴圈**：長流程中每隔一段間隔，派一個 context 全新的子代理（fresh-context verifier subagent）對照原始規格驗證，比主模型自我批評效果更好。
   > ⚠️ **與 Opus 5 官方建議看似衝突，實際不衝突（2026-09-03 調和）**：Opus 5 明文說「不要用子代理**複查自己的結果**」。差別在**驗證對象**——本條說的是拿**全新 context 對照原始規格**檢查長流程有沒有漂移，不是複查剛剛那一步的產出。
   > 🔑 判準：「這一步對不對」→ 不要派子代理；「整段有沒有偏離原始規格」→ 值得派，但基準必須是**原始規格文件**而非前一個子代理的輸出摘要。完整拆解見 [[claude-opus-5-prompting-guide]]。
4. **套官方修正提示詞**：依實際遇到的行為偏差（想太多不動手／自作主張改東西／該報告卻直接動手／進度真假難辨／對話長就喊停／怕 context 用完提前總結）挑對應的官方提示詞句子加進 system prompt。
5. **搭子代理與記憶**：把互不相依的子任務派給子代理、非同步不卡等；主動建立可寫入的記憶檔案或用官方 memory tool（Fable 5 不會自動跨對話記憶），寫法為一檔一教訓、開頭一行摘要、記錄修正與已驗證做法、避免重複、錯誤即刪。

## C（關鍵行動）

- [ ] 動手前先查最新 effort 檔位與 `max_tokens` 建議值是否有變動（官方指南會更新）
- [ ] 長任務一律先設計驗證迴圈與子代理分工，不要等出錯才補
- [ ] 遷移舊 skills 或估算成本前，先確認是否已依新版 tokenizer 與過度規範問題調整

## 原始摘要

本文整理 Anthropic 官方文件，涵蓋 Fable 5 規格（1M context window、128K 輸出上限、資料保留 30 天不支援 ZDR）、7 種官方點名的適用長任務場景、effort 與費用試算、長任務 agent 的驗證迴圈設計、6 種常見行為偏差的官方修正提示詞、子代理與 memory 搭配方式，以及一個非官方的社群玩法（把 Fable 5 判斷力轉寫成可長期沿用的制度與檔案）。

## 相關連結

- [[claude-opus-5-prompting-guide]] — 🔴 **調和主場**：「不要用子代理複查自己的結果」vs 本篇 verifier 迴圈，差別在驗證對象；並含子代理上限的反方向指定

- [[claude-fable-5-power-tasks]] — Fable 5 限時免費窗口最值得處理的 5 種長上下文任務（互為配套：本篇講怎麼用，該篇講用來做什麼）
- [[loop-engineering-6-components]] — Worker/Verifier 分離架構，與本篇「fresh-context verifier subagent」驗證迴圈同一原理
- [[claude-opus-4-8-release-notes]] — Opus 4.8 的 effort 控制機制對照組
- [[llm-cost-optimization-strategies]] — prompt caching 與分層模型成本優化策略
- [[claude-opus-5-5-release-notes]] — 2026-09-23 Opus 5.5 官方稱大部分工作追平 Fable 5.1；Pro 用 Fable 5.1 需另開 usage credits
