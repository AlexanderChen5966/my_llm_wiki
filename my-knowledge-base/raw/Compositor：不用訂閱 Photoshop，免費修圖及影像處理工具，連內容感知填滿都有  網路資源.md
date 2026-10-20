---
title: "Compositor：不用訂閱 Photoshop，免費修圖及影像處理工具，連內容感知填滿都有 | 網路資源"
source: "https://tools.wingzero.tw/articles/4072"
author:
  - "[[赤紅翼零]]"
published: 2026-09-19
created: 2026-09-22
description: "Compositor 是免費開源的 macOS Photoshop 替代方案，提供 Layers、Masks、Curves、Content-Aware Fill、Spot Healing、Clone Stamp 與 Remove Background 等功能。"
tags:
  - "clippings"
status: "inbox"
---
網站連結 [https://github.com/robbietilton/Compositor](https://github.com/robbietilton/Compositor)

Compositor 是一款專門為 macOS 開發的免費開源圖片編輯器，定位就是「Photoshop alternative for Mac」。作者希望保留 Photoshop 使用者熟悉的影像合成與後製工作流程，同時提供免費、沒有訂閱費，而且能自由修改原始碼的選擇。

它並不是只有裁切、濾鏡等簡單功能，而是朝完整 Desktop Image Editor 發展。從 Layers、Masks、Adjustment Layers，到 Content-Aware Fill、Spot Healing、Clone Stamp、Curves 與 Remove Background，都已經包含在目前功能中。

比較值得注意的功能包括：

- Layers & Folders：支援圖層、群組、Blend Mode 與 Opacity。
- Layer Masks：可繪製、填滿、反轉、模糊與 Feather，也支援 Clipping Mask。
- Adjustment Layers：包含 Hue/Saturation、Levels、Curves、Exposure、Gradient Map 與 Grain。
- Non-destructive Transform：縮放、旋轉、翻轉時保留圖片完整解析度。
- 完整選取工具：包含 Rectangle、Ellipse、Lasso、Polygonal Lasso 與 Magic Wand。
- Content-Aware Fill：可以智慧填補選取區域，甚至用來延伸圖片邊界。
- 修圖工具：提供 Spot Healing Brush、Clone Stamp、Blur、Brush 與 Gradient。
- Remove Background：內建背景移除功能。
- 常用濾鏡：Gaussian Blur、Motion Blur、Noise、Lens Correction 等。
- Photoshop 式快捷鍵：盡量降低 Photoshop 使用者轉換工具的學習成本。

其中 Content-Aware Fill 相當值得注意。除了移除畫面中的物件，它還可以延伸原始圖片邊緣；搭配 Spot Healing Brush 與 Clone Stamp，Compositor 已經能處理不少常見照片修飾與後製需求。

圖層系統也是它和一般輕量圖片編輯器最大的差異之一。除了普通 Layer，還支援 Folder Mask、Clipping Mask、Adjustment Layer，以及多圖層一起 Transform；甚至可以直接把 Layer 從一個開啟中的 Project 拖到另一個 Project。

Compositor 也刻意維持 Photoshop 使用者熟悉的操作邏輯。像 `⌘E` 可以 Merge、Option-drag 可以複製 Layer，Crop、Transform、Canvas Size、Image Size 等概念也相當接近傳統專業圖片編輯軟體。

檔案方面，目前可以匯入 JPEG、PNG、HEIC 與 TIFF，也支援直接拖入 Screenshot 或其他 App 的圖片；輸出則提供 JPEG Live Preview 與 Copy Merged 等功能。值得注意的是，目前 README 並沒有列出 PSD 匯入／輸出支援，因此如果工作流程高度依賴 Photoshop PSD 檔案，這點需要先確認。

目前 Compositor 要求 macOS 26；如果要自行從 Source Build，則需要 Xcode 26。整個專案採用 MIT License，因此除了免費使用，也能直接下載 Xcode Project，依照自己的工作流程修改或增加功能。

整體而言，Compositor 可以理解成一款正在快速發展中的 Mac 原生開源 Photoshop 替代方案。它目前的優勢不是要宣稱已經完整複製 Photoshop 所有功能，而是把 Photoshop 使用者最熟悉的一批核心工具——Layers、Masks、Curves、Selections、Healing、Clone Stamp、Content-Aware Fill 與快捷鍵——重新建立成一套免費、開源的 macOS 圖片編輯器。