---
title: AI 室內設計工作流（一）：Krita + ComfyUI 系列總覽
date: 2026-07-24
tags: [ai/workflow, type/concept]
source: https://tw-blog.reroom.ai/comfy-krita
type: article
status: processed
asset_value: 36
---

ReRoom AI「Krita 繪圖軟體 + ComfyUI」室內設計工作流系列的**入口總覽篇（Part 1/4）**。主流文生圖工具只能靠文字生成、無法控制圖片結構，對室內設計效用不大；此工作流以 Krita（開源繪圖軟體）＋ComfyUI（節點式 AI 生圖）組合，能從案場圖或模型圖出發，**維持空間結構比例、依範例圖生成類似風格、自由增減畫面物件**。

## T（核心論點）

室內設計需要的是「能精準重繪空間」的生成工具，而非只能吐渲染圖的文生圖。Krita 的圖層功能 + ComfyUI 的模組化影像處理，讓設計師完整掌握生成圖中的每個元素，把心力從視覺化的繁瑣步驟中解放出來。

## B（適用場景）

**適用：** 想導入 AI 的室內設計師；想自行做室內改造但無 3D 渲染/平面設計軟體經驗的自宅/租屋改造者。
**硬體需求：** ComfyUI 官方建議 **Nvidia 30 系中高階以上、VRAM 越高越好**（10 系～50 系皆可跑，低階顯卡單張生成時間會很長）；AMD/Intel 顯卡有額外限制不推薦。Krita 本體無特殊需求，其 AI 插件最低推薦 6GB VRAM。

## R（系列架構）

- **Krita**：免費開源繪圖軟體（類 Photoshop），是主要操作介面，透過生成式 AI 插件把 ComfyUI 工作流納入。
- **ComfyUI**：像拼裝積木的 AI 圖像工具，用組合「積木（節點）」生成圖像，可選風格/設定顏色/改細節。
- **系列四篇**：(1) 本篇總覽 →（2）[[comfyui-installation]] 安裝 →（3）[[comfyui-interior-design-process]] ComfyUI 節點/工作流實作 →（4）Krita + ComfyUI 實際串接應用（尚未收錄）。

## C（關鍵行動）

- [ ] 先確認顯卡是否達 Nvidia 30 系/VRAM 門檻，再決定是否走本地 ComfyUI 路線
- [ ] 依序讀完系列，先建立節點/工作流概念再串接 Krita
- [ ] 若只是偶爾娛樂，雲端生成平台就夠（見 [[comfyui-getting-started]] 的使用者定位判斷）

## 原始摘要

作者 Wayne Huang（ReRoom AI），2025-02-18。系列 Part 1，介紹 Krita + ComfyUI 組合能維持空間結構、依範例生成風格、增減物件，並說明硬體需求與後續四篇內容預告。

## 相關連結

- [[comfyui-installation]] — 系列 Part 2：ComfyUI 安裝
- [[comfyui-interior-design-process]] — 系列 Part 3：節點、工作流、ControlNet/LoRA 實作
- [[comfyui-getting-started]] — ComfyUI 入門整合條目（概念/安裝/使用者定位/節點參數，通用視角），與本室內設計系列互補
- [[can-i-run-ai-tool]] — 跑本地模型前確認 GPU/VRAM
