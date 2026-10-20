---
title: "Image Blaster：經由 AI Agent 從概念圖到 3D 場景原型的開源工具 | 網路資源"
source: "https://tools.wingzero.tw/articles/4086"
author:
  - "[[赤紅翼零]]"
published: 2026-10-04
created: 2026-10-05
description: "Image Blaster 是以 Claude Skills 打造的開源 Image to 3D 工具，可將單張圖片轉成 3D 場景、物件模型、Gaussian Splat 與音效素材。"
tags:
  - "clippings"
status: "inbox"
---
網站連結 [https://github.com/neilsonnn/image-blaster](https://github.com/neilsonnn/image-blaster)

Image Blaster 是一個以 Claude Skills 為核心的「單張圖片轉 3D 場景」工具組。它會根據輸入圖片，協調 World Labs 與 FAL 等服務，產生可探索的 3D 環境、物件模型與音效，適合用來快速建立遊戲關卡、場景原型或互動式展示素材。專案 README 表示，從圖片到完整 3D 場景可在五分鐘內完成，定位是加速 3D 製作的起點；實際耗時仍會受到圖片、生成服務與設定影響。

它的使用方式不是開啟網頁、上傳圖片後就直接下載結果，而是先將專案複製到本機，安裝並執行 Claude，再提供 World Labs 和 FAL 的 API 金鑰。接著把圖片放進 `input/` 資料夾，請 Claude 執行 Image Blaster，並可要求它在每個步驟前先確認。這種流程讓使用者能以對話方式啟動生成與逐步確認，但需要具備基本的終端機操作能力，也要自行準備第三方服務的帳號與金鑰。

預設流程會把圖片中的動態物件與靜態環境分開處理：前者轉成 3D 模型，後者則以 Gaussian Splat 呈現場景，另外還會生成環境循環音與物件相關的物理音效。主要輸出包含：

- **3D 物件模型：** 動態物件可輸出為 `.glb` 或 `.obj` ，便於匯入 3D 軟體或遊戲專案。
- **場景資料：** 靜態環境以 `.spz` Gaussian Splat 格式生成，適合呈現具有空間感的場景。
- **音效素材：** 可產生環境音與物件專屬音效，格式為 `.mp3` 。
- **生成參數：** 可調整模型面數、是否產生 PBR 材質，以及 Normal、LowPoly 或純幾何等生成類型。

Image Blaster 使用多種生成模型完成不同環節。World Labs 的 Marble 1.1 負責建立可探索環境；圖片編輯可使用 Nano Banana，或在指定時改用 GPT Image 2；Hunyuan 3D 透過 FAL 產生物件模型；環境音與物件音效則使用 ElevenLabs SFX。這些模型分工使工具能從單張參考圖延伸出多種資產，但成品品質仍取決於原始圖片是否清楚、場景內容是否容易辨識，以及模型生成結果是否符合後續專案的要求。

生成完成後，素材可用於 Unity、Unreal、Godot 等遊戲引擎，也能匯入 Blender、3ds Max、Maya 等 DCC 軟體，或放進 Three.js、Electron 等應用程式。從應用情境來看，Image Blaster 適合遊戲開發者製作關卡概念、創作者重建房間或場景、機器人團隊建立環境素材，以及電影或建築團隊準備視覺參考。它的價值在於縮短從概念圖到可用 3D 素材的前期流程，並非取代完整的建模、拓樸整理、材質修正與場景優化工作。

整體而言，Image Blaster 是將 Claude 對話工作流與多種生成服務串接起來的開源專案，適合想快速探索「圖片如何轉成可用 3D 場景」的開發者與 3D 創作者。開始使用前，需確認電腦已安裝 Claude CLI，並備妥 World Labs 與 FAL 的 API 金鑰；專案採 MIT 授權，可在 GitHub 查看程式碼與使用方式。