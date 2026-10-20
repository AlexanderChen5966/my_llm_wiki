---
title: "System Prompts Leaks：收錄主流 AI 系統提示詞，探索大型語言模型背後的運作方式 | 網路資源 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3975"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-07-06
created: 2026-07-07
description: "System Prompts Leaks 收錄 ChatGPT、Claude、Gemini、Cursor 等 AI 服務的公開 System Prompt，提供 Prompt Engineering、AI Agent 與大型語言模型研究的重要參考，深入了解 AI 助理的設計思維與運作方式。"
tags:
  - "clippings"
status: processed
---
大型語言模型（LLM）在回答問題時，除了使用者輸入的 Prompt 之外，通常還會搭配一段由開發者預先設定的 **System Prompt（系統提示詞）** ，用來定義 AI 的角色、能力範圍、回應風格以及各種行為規則。不過，這些 System Prompt 大多不會直接公開，因此外界只能透過官方文件或研究分析推測其內容。

**System Prompts Leaks** 是一個收錄公開流傳與研究整理的 AI 系統提示詞（System Prompts）開源專案，目的是保存目前已知來自各種 AI 服務、AI Agent 與大型語言模型的 System Prompt 範例，方便研究人員、開發者與 AI 愛好者了解不同 AI 產品如何透過提示工程（Prompt Engineering）塑造模型行為，以及不同服務之間的設計理念差異。

相較於一般介紹 Prompt 撰寫技巧的教學文章，System Prompts Leaks 更像是一份持續更新的資料庫。專案將目前社群公開討論、官方釋出或經研究整理的 System Prompt 集中管理，方便使用者查閱與比較，也讓 Prompt Engineering 的研究有更多實際案例可以參考。

目前收錄的內容涵蓋多種知名 AI 服務與 AI Agent，例如：

- ChatGPT
- Claude
- Gemini
- GitHub Copilot
- Cursor
- Devin
- Perplexity
- Manus
- Lovable
- Windsurf
- v0
- Bolt
- Replit Agent

除了聊天機器人之外，也包含許多 AI 開發工具、程式設計助理與 AI Agent，可讓讀者觀察不同產品如何透過 System Prompt 定義自身角色、工具使用方式、回答限制與安全規範。

從內容形式來看，每份資料通常會保留原始 Prompt 內容或相關版本，方便閱讀與比較。由於不同產品會持續更新，因此專案也會隨著社群的新發現逐步補充，成為追蹤 Prompt 演進的重要參考來源。

整理這個專案的特色，可以歸納為以下幾點：

- 收錄多個主流 AI 服務的 System Prompt。
- 持續更新社群公開整理的 Prompt 內容。
- 提供 Prompt Engineering 的實際案例。
- 可比較不同 AI 產品的設計思維。
- 協助研究 AI Agent 的角色設定與行為規則。
- 開源維護，方便社群共同補充與修正。

近年 Prompt Engineering 已逐漸從單純撰寫提示詞，發展成 AI 應用設計的重要環節。對 AI 產品而言，System Prompt 不只是開場白，而是影響模型輸出品質的重要組成部分。例如是否允許呼叫工具、如何處理安全限制、回應應保持什麼語氣，以及遇到特殊情境時如何決策，都可能在 System Prompt 中預先定義。

因此，System Prompts Leaks 的價值並不只是「看見 Prompt」，而是提供一個觀察大型語言模型產品設計的方法。透過比較不同公司的 Prompt，可以發現各家對 AI 助理定位、使用者互動方式、安全策略以及工具整合都有不同的思考方向，這些內容對於設計 AI 產品或研究 Prompt Engineering 都具有一定的參考價值。

另一方面，也需要理解這類專案的內容並不代表官方完整實作。由於部分 System Prompt 是社群研究、公開分享或從特定情境中取得，可能與最新版本存在差異，也可能因產品更新而失效。因此，閱讀這些資料時，較適合作為研究 Prompt 設計思路與 AI 系統架構的參考，而不宜視為各項服務的完整內部規格。

從適用情境來看，System Prompts Leaks 特別適合以下族群：

- AI 應用開發者。
- Prompt Engineering 學習者。
- AI Agent 設計者。
- 大型語言模型研究人員。
- 對 AI 系統運作方式感興趣的技術愛好者。
- 希望分析不同 AI 產品設計理念的產品經理。

整體而言，System Prompts Leaks 提供了一個觀察 AI 系統設計的重要窗口。透過整理不同 AI 產品的 System Prompt，讓開發者能更深入理解大型語言模型如何透過提示詞塑造行為與能力，也能作為設計 AI Agent、研究 Prompt Engineering 或分析產品策略的重要參考資料。對於希望深入了解 AI 助理幕後運作方式的人來說，這個開源專案具有相當高的研究價值。