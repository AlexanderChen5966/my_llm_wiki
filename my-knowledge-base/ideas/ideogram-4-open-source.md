---
title: Ideogram 4.0 開源圖像生成模型（93億參數）
date: 2026-06-15
tags: [ai/agent]
source: https://www.koc.com.tw/archives/645075
type: idea
status: processed
asset_value: 45
---

Ideogram AI（Google Imagen 核心團隊出走創業）發布首款開放權重圖像生成模型，93億參數，Apache 2.0 授權，支援本地部署與微調。

**技術亮點：**
- 架構：全單流 Diffusion Transformer（DiT），34 層，文字 + 圖像 token 同一序列處理
- 文字編碼器：Qwen3-VL-8B-Instruct（僅文字模式，從 13 中間層提取）
- 提示詞格式：結構化 JSON（可定義 style / subject / composition 等欄位）
- 效果：Design Arena 開放模型排行第一，排版盲測優於 ChatGPT

**適用場合：** 需要高品質開源圖像生成、本地部署、商業授權無顧慮的場景。
