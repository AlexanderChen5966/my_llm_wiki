---
title: "Claude Video：結合 Claude AI 與 FFmpeg，自動完成影片剪輯的開源 AI 工作流程 | 網路資源"
source: "https://tools.wingzero.tw/articles/4007"
author:
  - "[[赤紅翼零]]"
published: 2026-08-04
created: 2026-08-05
description: "Claude Video 是結合 Anthropic Claude 與 FFmpeg 的開源 AI 影片剪輯工具，透過自然語言即可自動完成影片裁切、字幕、格式轉換、合併等後製工作。本文介紹 Claude Video 的 AI Agent 架構、核心功能、使用流程，以及它如何降低 FFmpeg 的使用門檻，打造更高效率的影片自動化工作流程。"
tags:
  - "clippings"
status: processed
---
網站連結 [https://github.com/bradautomates/claude-video](https://github.com/bradautomates/claude-video)

Claude Video 是一個開源 AI 影片剪輯專案，利用 Anthropic Claude 作為智慧代理（AI Agent），搭配 FFmpeg 影片處理工具，讓 AI 能理解自然語言指令並自動完成影片剪輯工作。使用者不需要熟悉影片編輯軟體或 FFmpeg 指令，只需描述希望達成的效果，例如剪除片頭片尾、加入字幕、裁切畫面或合併多段影片，Claude 便會分析需求並產生對應的剪輯流程。

相較於傳統 AI 影片生成工具著重於從文字生成新影片，Claude Video 的定位更偏向「AI 自動化影片編輯」。它不是建立全新的影片，而是透過 AI 理解既有素材，協助完成影片後製與加工，因此特別適合需要大量處理影片內容的創作者與開發者。

Claude Video 的核心概念，是讓 Claude 直接操作 FFmpeg。FFmpeg 本身功能相當完整，但指令繁多，對一般使用者而言有一定學習門檻。Claude Video 則將這些複雜指令交由 AI 處理，使用者只需以自然語言描述需求，AI 即會規劃適合的 FFmpeg 指令並執行影片處理流程。

整個工作流程大致如下：

1. 匯入影片素材。
2. 使用自然語言描述剪輯需求。
3. Claude 分析需求並規劃處理流程。
4. 自動產生 FFmpeg 指令。
5. 執行影片剪輯並輸出成品。

這種方式讓影片後製更接近與 AI 對話，而非記憶大量命令列參數，對於沒有影音技術背景的使用者來說，能有效降低操作門檻。

專案目前支援的影片處理能力包括：

- 剪除指定片段
- 合併多段影片
- 裁切影片畫面
- 調整影片尺寸與解析度
- 加入字幕
- 擷取影片片段
- 轉換影片格式
- 處理音訊與影片同步
- 執行各種 FFmpeg 支援的影音操作

由於所有操作最終仍由 FFmpeg 執行，因此理論上只要 FFmpeg 能完成的工作，都能透過 Claude Video 逐步擴充支援。這也讓專案具有高度彈性，而不是受限於固定功能。

另一項特色，是 Claude Video 採用 AI Agent 的工作模式，而非單純將 Prompt 對應到一條指令。Claude 會根據使用者需求分析任務，必要時拆解為多個步驟，再依序完成。例如同時要求剪除前後空白、加入字幕並輸出為社群媒體尺寸時，AI 能整合多項 FFmpeg 操作，而不需要使用者逐一輸入指令。

整理 Claude Video 的特色，可歸納為以下幾點：

- 使用 Claude AI 理解自然語言剪輯需求
- 自動產生 FFmpeg 指令
- 支援影片剪輯、裁切、格式轉換與字幕處理
- 降低 FFmpeg 的使用門檻
- 採用 AI Agent 工作流程，自動規劃多步驟任務
- 完全開源，可依需求自行擴充功能

從使用情境來看，Claude Video 適合需要經常處理影片的創作者與技術團隊。例如 YouTuber 可快速完成影片裁切與格式轉換；教育工作者可批次整理教學影片；企業能建立自動化影片處理流程；開發者則可將 Claude Video 整合到自己的 AI 影音系統中，作為自然語言控制影片編輯的介面。

若與 Adobe Premiere Pro、DaVinci Resolve 或 CapCut 等傳統剪輯軟體相比，Claude Video 並非提供完整的視覺化編輯介面，而是著重於自然語言操作與自動化流程。對於重複性高、規則明確的影片處理工作，例如批次轉檔、固定格式輸出或自動裁切，AI Agent 能有效減少人工操作時間。

另一方面，若與近年興起的 AI Agent 專案相比，Claude Video 展示了大型語言模型在影音工作流程中的另一種應用方式。它並不自行生成影片，而是作為 FFmpeg 的智慧控制層，讓使用者能透過對話完成原本需要大量命令列操作的工作。這種設計也反映 AI Agent 正逐漸從文字處理延伸至影音內容製作。

整體而言，Claude Video 是一個將 Claude AI 與 FFmpeg 結合的開源影片自動化工具，透過自然語言理解與 AI Agent 規劃能力，簡化影片剪輯流程。對於希望降低 FFmpeg 學習門檻、建立自動化影音處理流程，或探索 AI 在影片後製應用的開發者與內容創作者而言，是一個具有參考價值的開源專案。