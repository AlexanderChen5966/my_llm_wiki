---
title: "JoyAI-Image：自由調整觀看角度，讓 AI 重繪圖片 | 網路資源"
source: "https://tools.wingzero.tw/articles/4128"
author:
  - "[[赤紅翼零]]"
published: 2026-10-27
created: 2026-10-27
description: "JoyAI-Image 是京東開源的多模態影像模型家族，結合圖片理解、文字生成圖片與依指令編輯圖片等能力，並主打空間推理、文字排版、多視角生成與局部修改。專案採 Apache 2.0 授權，提供模型權重與推論程式；實際部署需要 CUDA GPU。"
tags:
  - "clippings"
status: "inbox"
---
網站連結 [https://github.com/jd-opensource/JoyAI-Image](https://github.com/jd-opensource/JoyAI-Image)

JoyAI-Image 是京東開源的多模態影像模型家族，將看圖理解、文字生成圖片與指令式圖片編輯放在同一個專案中。它的特色不只在依照提示產生新圖，也嘗試理解畫面裡的物件、位置與相互關係，讓使用者能以較明確的指令調整畫面內容、物件方向或觀看視角。對需要反覆修改視覺稿、製作分鏡或探索不同構圖的人來說，這種「理解、生成、再編輯」的整合流程值得留意。

專案 README 將架構描述為 8B 多模態大型語言模型（MLLM）搭配 16B 多模態 Diffusion Transformer（MMDiT）：前者負責理解圖片與指令，後者負責生成或變換影像。官方也提供不同用途的模型版本，包括 JoyAI-Image-Und 圖片理解模型、JoyAI-Image-Edit 指令式修圖，以及支援多張圖片組合與一致性處理的 JoyAI-Image-Edit-Plus。依官方文件，編輯模型也能在不提供輸入圖片時執行文字生圖。

它主打的能力包含長文字與複雜版面呈現、多視角生成，以及依照文字指令進行空間編輯。README 提到的操作類型包括移動物件、旋轉物件和調整相機視角；例如可以要求改變主體在畫面中的位置、呈現物件另一側，或在保留場景內容的前提下改變觀看角度。這讓模型除了日常修圖，也能用於概念設計、產品情境圖、漫畫版面、視覺分鏡與影像資料探索。

對開發者而言，GitHub 專案提供推論程式、依賴設定、示範圖片與參數說明；模型權重則可從官方模型頁面取得。README 列出的本機環境需要 Python 3.10 以上與 CUDA GPU，並依賴特定版本的 PyTorch、Transformers、Diffusers 等套件。由於模型規模不小，實際使用前應確認顯示卡記憶體、權重下載空間及套件相容性；只想試用的人也可先查看官方模型頁面的線上推論入口或示範空間。

使用 JoyAI-Image 時也要理解它的範圍：它主要產生或編輯圖片，不會直接輸出可在 Blender、Unity 或 three.js 使用的 3D 網格模型。多視角與空間編輯結果可作為視覺參考或前期素材，但主體一致性、畫面文字、局部細節和視角是否符合預期，仍需要人工檢查。它適合想在本機研究多模態模型、建立影像處理流程，或測試可控修圖能力的開發者與創作者；若只想快速修一張圖，硬體與安裝門檻可能偏高。

專案標示採用 Apache 2.0 授權。實際下載與整合前，建議一併查看 GitHub 上的授權文件與各模型頁面的最新說明；README 也提醒，引用或使用其基礎開源專案時應遵循各自的授權與引用規範。

可從 [JoyAI-Image GitHub 專案](https://github.com/jd-opensource/JoyAI-Image) 查看程式碼與 README，並前往 [JoyAI-Image-Edit 模型頁](https://huggingface.co/jdopensource/JoyAI-Image-Edit) 了解權重與推論方式；授權細節請參考 [Apache 2.0 LICENSE](https://github.com/jd-opensource/JoyAI-Image/blob/main/LICENSE) 。

<iframe frameborder="0" src="https://844899acf5c491b46225b698550e635d.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="250" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe><iframe frameborder="0" src="https://844899acf5c491b46225b698550e635d.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe>