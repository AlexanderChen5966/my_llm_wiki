---
title: Sakana Translate：主打日文語感深度翻譯，XCOMET-XL 超越 Claude Opus 4.7
date: 2026-07-17
tags: [ai/workflow]
source: https://www.koc.com.tw/archives/648347
type: idea
status: processed
---

產品發布 + 模型評測新聞。日本 AI 新創 Sakana AI 在對話服務 Sakana Chat 中新增免費翻譯功能 Sakana Translate，支援日/英/中雙向互譯，最大特色是能把商務敬語、網路俚語連同上下文與對話雙方的距離感一併翻譯，而非單純詞彙替換。含基準數據可參考，但缺可操作框架，暫存 ideas。

## 值得留意的參考點
- **翻譯引擎 Namazu**：將海外前沿模型透過 post-training 調整為適合日本市場的獨家模型，概念定調「深度翻譯日文」（傳達文脈/語氣/人際距離感）
- **基準測試（WMT 2024 General Translation + XCOMET-XL 自動評估）**：Sakana Translate 得 **0.835**，僅次於 Gemini 3.1 Pro（0.851）與 GPT-5.5（0.843），**超越 Claude Opus 4.7（0.831）**。以遠小於巨頭的規模達此成績相當突出
- **三大功能**：翻譯（最大約 5,000 字長文、即時串流）/ 潤飾校對（調自然度/禮貌程度/語氣/距離感，差異標示）/ 提問（同畫面追問語感、其他表達、文法解說）
- **語氣切換**：カジュアル/ていねい/簡潔/ネイティブらしく/大阪弁 一鍵切換
- **路線圖**：產業最佳化引擎、檔案翻譯、詞彙表連動、公開 API、SSO/稽核/地端部署進軍企業市場

## 相關連結
- [[sakana-fugu-orchestration-model]] — 同一家 Sakana AI 的編排模型 Fugu（多代理協調）
- [[llm-cost-optimization-strategies]] — 免費/低成本模型選用策略
