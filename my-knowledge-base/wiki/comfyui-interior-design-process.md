---
title: AI 室內設計工作流（三）：ComfyUI 節點、工作流與生成控制實戰
date: 2026-07-24
tags: [ai/workflow, type/how-to]
source: https://tw-blog.reroom.ai/ai-comfyui-workflow
type: article
status: processed
asset_value: 48
---

ReRoom AI 室內設計系列 **Part 3/4**，系列中資訊密度最高的操作核心篇。從生成第一張圖出發，講解節點（Node）概念、文生圖/圖生圖工作流、KSampler 各參數，並示範用 **ControlNet / LoRA / IPAdapter + ComfyUI Manager** 消除生成隨機性、精準控制圖片結構——這正是把 AI 生圖應用到室內設計的關鍵。

## T（核心論點）

ComfyUI 是節點式工作流工具：節點是最基礎單位，節點間的連線代表資料傳輸（同顏色輸出接同顏色輸入，有防呆機制）。要把 AI 生圖用於室內設計，必須**盡可能消除生成的隨機性**——靠 ControlNet 等 Custom Node 與模型，讓 AI 遵循特定圖像「結構」而非只靠提示詞。

## B（適用場景）

**適用：** 已裝好 ComfyUI（見 [[comfyui-installation]]），想理解節點/工作流並做結構控制的使用者。
**注意：** Mac 版若只生成黑畫面，需依教學調整 KSampler 設定；不同生成模型（SDXL/FLUX）搭配的 ControlNet 版本不同，下載前要對齊版本。

## R（核心內容）

**文生圖工作流（六節點）**：① Load Checkpoint 載入大模型 → ② CLIP Text Encode 輸入正/負向提示詞（綠色正/紅色負）→ ⑤ Empty Latent Image 設尺寸與張數（batch_size）→ ③ KSampler 運算 → ④ VAE Decode 解碼成可見圖 → Preview/Save Image。

**KSampler 參數**：`Seed`（固定則產同圖）/ `control_after_generate`（fixed/randomize/increment/decrement）/ `Steps`（去噪次數，20 起始）/ `CFG Scale`（提示詞依從度，標準 6–9，越高越死板）/ `Sampler`（依模型調整）/ `Denoise`（圖生圖改變幅度，越低越保留原圖）。

**圖生圖工作流**：把 Empty Latent Image 換成「參考圖 → VAE Encode → Latent」即可（KSampler 只吃 Latent），用 Denoise 調重繪幅度。工作流可 Workflow → Save 存起來重用。

**生成控制三手段**：
- **LoRA**：輕量微調模型，搭 Checkpoint 使用，特化出指定角色/物品/風格（如室內設計、水墨風）。
- **ControlNet**：讓 AI 依參考圖「結構」生成——Pose(OpenPose)/Canny 邊緣/Depth 深度/Scribble 塗鴉/**MLSD 直線檢測（適合建築結構）**。
- **IPAdapter**：模仿參考圖的風格/顏色/臉部特徵。

**下載與安裝**：Checkpoint（如 SDXL 的 Juggernaut XL）從 Civitai 下載放 `models/checkpoints`；ControlNet 模型從 Hugging Face 下載放 `models/controlnet`（注意 sdxl 專用）；用右上角 **ComfyUI Manager → Custom Node Manager** 安裝 Custom Node（如 ArtVenture / controlnet_aux / Comfyroll Studio），裝完 Restart。ControlNet 工作流較圖生圖多了 Load ControlNet Model / ControlNet Preprocessor / Apply ControlNet 三節點。

## C（關鍵行動）

- [ ] 先跑通文生圖六節點，再練圖生圖（換 VAE Encode），最後加 ControlNet
- [ ] 室內設計優先用 ControlNet 的 MLSD/Depth/Canny 保持建築與空間結構
- [ ] 用 Manager 裝 Custom Node，模型放對資料夾（checkpoints / controlnet）
- [ ] 多試 Denoise 與 CFG，觀察對結構控制力度的影響

## 原始摘要

作者 Wayne Huang（ReRoom AI），2025-02-27。含生成第一張圖、節點連線防呆、文生圖/圖生圖工作流、KSampler 六參數、SD/SDXL 模型說明、Civitai/Hugging Face 下載、ComfyUI Manager 裝三個 Custom Node、ControlNet 五種控制類型表與工作流編輯。系列 Part 4（Krita 實際串接）尚未收錄。

## 相關連結

- [[comfyui-interior-design-workflow]] — 系列 Part 1：總覽
- [[comfyui-installation]] — 系列 Part 2：安裝
- [[comfyui-getting-started]] — ComfyUI 入門整合條目（KSampler 參數/資料型別/checkpoint 比 prompt 更影響外貌，通用深入視角）
- [[chatgpt-image-prompts-80]] — 圖像生成提示詞結構公式（雲端版），對照 CLIP 提示詞
