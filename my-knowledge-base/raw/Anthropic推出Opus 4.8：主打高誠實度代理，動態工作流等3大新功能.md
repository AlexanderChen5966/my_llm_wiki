---
title: "Anthropic推出Opus 4.8：主打高誠實度代理，動態工作流等3大新功能"
source: "https://www.bnext.com.tw/article/91083/anthropic-claude-opus-4-8-honesty-dynamic-workflows-effort-control"
author:
  - "[[李先泰]]"
published:
created: 2026-06-01
description: "Anthropic 於 5 月 28 日發表旗艦模型 Claude Opus 4.8，僅距 4.7 版 41 天，並維持原價。"
tags:
  - "clippings"
status: processed
---
> **重點一** ：Anthropic 5 月 28 日發表旗艦模型 Opus 4.8，距 Opus 4.7 僅 41 天。  
>   
> **重點二** ：核心改進是「誠實度」：Anthropic 表示 Opus 4.8 較少做出無依據宣稱，讓程式碼瑕疵通過不糾正的比率約為 Opus 4.7 的四分之一。  
>   
> **重點三** ：同步推出動態工作流（Claude Code）、effort 控制與 Messages API 中段 system message 功能。

Anthropic 在美國時間 5 月 28 日發表旗艦級模型 Claude Opus 4.8，距前代 Opus 4.7 僅 41 天，更新節奏明顯快於 Anthropic 近來常見週期。

Sonnet 與 Haiku 最近一次更新分別約在三個月與七個月前。新版定價維持與 4.7 相同，每百萬 token 輸入 5 美元、輸出 25 美元，但 fast mode（高速模式，以 2.5 倍速回應）的價格降至前代的三分之一。

據《TechCrunch》報導，這波快節奏的更新背後，可能與 OpenAI、Google 近期密集推出 Codex、Gemini Flash 等新品帶來的市場壓力有關。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

值得注意的是，Anthropic 在公告中罕見地把自家新模型描述為「漸進但具體的改進」（modest but tangible improvement），而非以行銷詞包裝。

## 主推「誠實度」:知道自己不知道

Opus 4.8 最主要的賣點不是基準分數，而是「誠實度」。Anthropic 在公告中指出，早期測試者發現新模型「更傾向標示自己工作上的不確定處，較少做出無依據的宣稱」。

對沖基金 Bridgewater（橋水）公司在證言中表示，最大的差別是 Opus 4.8 會主動標出分析輸入與輸出的問題，「這是其他模型常常漏掉、最後丟給使用者自己抓的事。」

根據 Anthropic 的 system card，新版在多項幻覺評測中的錯答率較低，主要是因為它在「沒把握的題目」上更傾向選擇不答；同時，它讓自己寫的程式碼瑕疵通過不糾正的比率，約為 Opus 4.7 的四分之一。

資深 AI 評論者 Simon Willison 對這份公告的反應是：「看到一個 AI 實驗室誠實地把新版本描述成微幅改進，真的很清新。」他另一個欣賞的點是 Anthropic 把訓練「誠實度」當成本次主軸。

## 同步上線：動態工作流、effort 控制、API 中段 system message

除了模型本身，Anthropic 同日也推出三項配套:

第一是動態工作流（Dynamic Workflows），可在 Claude Code 中協調數十至數百個並行子代理（subagents），用於跨大型代碼庫的搬遷、稽核或重寫；以研究預覽（research preview）形式開放給 Max、Team、Enterprise 方案，Enterprise 需管理員手動啟用。

第二是 effort 控制（effort control）。在 [claude.ai](http://claude.ai/) 與 Cowork 上，使用者可在模型選單旁，自由調整 Claude 要花多少「力氣」回應。拉高就思考更久、品質更好；拉低就回得更快、消耗用量上限的速度也較慢。所有方案皆可使用。

第三是 Messages API 開放在 messages 陣列中插入 system 訊息。簡言之，開發者可在對話進行中更新 Claude 的指令，不必重發整段 system prompt，這對長時間運作的代理特別有用，可在過程中動態調整權限、token 預算或環境設定。

Simon Willison 補充，Opus 4.8 的 prompt cache（提示快取）最低門檻也從 4.7 的 4,096 token 降到 1,024 token，省下不少重複費用。

## Mythos 仍未鬆綁，Anthropic 預告「數週內」開放

Anthropic 在公告結尾透露下一步動向，更高能力的 Mythos 級模型目前仍因網路安全考量，僅透過 Glasswing 計畫對少數企業夥伴開放，主要用於資安工作。公司表示，正在快速開發必要的安全防護，預期「在未來幾週」可把 Mythos 級模型開放給所有客戶。

本次 Opus 4.8 是 Anthropic 把「誠實度」與「協調代理」拉到產線層級的中繼站；至於能力更高的 Mythos 級模型，仍要等 Anthropic 完成資安防護並正式擴大開放後，才能觀察實際影響。

> 延伸閱讀： [Google Workspace換Logo了！新版哪裡不同？一次看懂「漸層」設計背後的AI品牌戰略](https://www.bnext.com.tw/article/91081/google-workspace-logo-refresh-ai-branding-trend)

資料來源:[TechCrunch](https://techcrunch.com/2026/05/28/anthropic-releases-opus-4-8-with-new-dynamic-workflow-tool/) 、 [Anthropic 官方公告](https://www.anthropic.com/news/claude-opus-4-8) 、 [Axios](https://www.axios.com/2026/05/28/anthropic-opus-release-mythos) 、 [Simon Willison 部落格](https://simonwillison.net/2026/May/28/claude-opus-4-8/)

本文初稿為AI編撰，整理．編輯/ 李先泰