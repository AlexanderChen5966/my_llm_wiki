---
title: "AI家家酒【Mac】×【Comfy UI】玩小型產圖AI"
date: 2025-12-20
tags: [ai/tool]
source: "https://www.cc.ntu.edu.tw/chinese/epaper/home/20251220_007505.html"
author: 陳俊佑（臺灣大學計算機及資訊網路中心程式設計組行政專員）
type: article
status: processed
created: 2026-07-22
---

# AI家家酒【Mac】×【Comfy UI】玩小型產圖AI

> 作者：陳俊佑 / 臺灣大學計算機及資訊網路中心程式設計組行政專員
> 發表日期：2025-12-20
> 來源：臺大計中電子報 https://www.cc.ntu.edu.tw/chinese/epaper/home/20251220_007505.html

## 前言

作者自我介紹為柚子哥，購買 Mac mini 後開始研究 AI 產圖應用。從 Stable Diffusion 研究到 Comfy UI，因著迷於參數調整與視覺化節點設計，週末經常熬夜至凌晨。雖然 16GB RAM 的 Mac mini 效能有限，但仍滿足基本產圖需求。本文分享最簡易的使用方法與介面介紹。

## 一、圖像生成模型相關知識學習

推薦學習臺大李宏毅教授的 YouTube 影片，涵蓋 Stable Diffusion、DALL-E、Imagen 背後的共同原理，以及 Diffusion Model 的運作機制。

## 二、Comfy UI

Comfy UI 將 Stable Diffusion 的複雜流程視覺化、節點化，降低使用門檻。支援文轉圖、圖轉圖、LoRA、圖轉影片等功能，透過下載網路資源並輸入相同參數，可複現他人成果。

## 三、ComfyUI 安裝

至 ComfyUI 官網 https://www.comfy.org 下載並安裝。

## 四、ComfyUI 介面

介面包含排程清單、節點清單、模型清單、已儲存工作流程、工作流程節點清單與官方模板等功能區塊。

## 五、起手式 — 官方模板

從官方 Image Generation 範例開始。首次執行時自動下載所需模型。

- **Load Checkpoint**：載入 Stable Diffusion 模型，輸出 MODEL、CLIP 和 VAE 供後續節點使用。
- **CLIP Text Encode**：將提示詞轉換為嵌入向量。包含正面提示詞（希望出現的內容）與負面提示詞（希望避免的內容）。
- **Empty Latent Image**：建立雜訊矩陣，width 與 height 決定圖像尺寸，batch_size 設定一次生成的圖片數量。
- **K-Sampler**：核心採樣器，整合模型、提示詞、雜訊畫布進行去雜訊計算。主要參數包括：
  - `seed`：決定初始雜訊排列
  - `control after generate`：種子變化方式（固定、遞增、遞減、隨機）
  - `steps`：去噪計算次數，15-35 為常見設定
  - `cfg`：提示詞遵循強度，通常 7-12
  - `denoise`：雜訊移除強度，文轉圖固定 1.0
  - `sampler_name`：去雜訊計算方式（Euler、DPM++ 2M、DPM++ SDE 等）
  - `scheduler`：去雜訊速度策略（Karras、Simple、Exponential 等）
- **VAE Decode**：將潛在表示（草稿）轉換為實際圖像，接收 K-Sampler 輸出與 VAE 解碼規則。
- **Run**：執行按鈕，開始圖像生成過程。

## 六、結語

作者比喻自己進入 ComfyUI 世界如同「劉姥姥進了大觀園」，初期困惑於眾多節點，但透過官方範例快速上手。後續開始蒐集提示詞與模型，最終導致硬碟堆滿數十個 Checkpoint、上百個 LoRA 與無數 Embedding，形成「數位模型的墳場」。警告讀者避免過度沉迷於蒐集模型的癖好。

## 致謝

感謝臺大計中程式組洪嘉駿的耐心解答與知識分享。

## 參考資料

列舉 ComfyUI 官網、GitHub、李宏毅教授 YouTube 頻道，及相關學術論文連結。

---

> 註：本文正文由 WebFetch 從原網頁擷取整理，如需逐字原文請對照來源連結。
