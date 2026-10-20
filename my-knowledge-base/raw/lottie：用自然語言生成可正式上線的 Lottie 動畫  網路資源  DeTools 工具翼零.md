---
title: "lottie：用自然語言生成可正式上線的 Lottie 動畫 | 網路資源 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3944"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-06-15
created: 2026-06-15
description: "lottie 是一個專為 AI Coding Agent 設計的 Text-to-Lottie 動畫生成框架，能透過自然語言建立可正式部署的 Lottie 動畫，大幅降低網站、App 與 SaaS 產品動態設計門檻。"
tags:
  - "clippings"
status: processed
---
lottie 是一個專門為 AI Coding Agent 打造的 Lottie 動畫生成框架，核心目標是讓開發者透過自然語言描述，直接產生可正式部署（Production Ready）的 Lottie 動畫。它的定位並不是傳統動畫編輯器，也不是另一套影片生成 AI，而是一個讓 Claude Code、Codex、Cursor 等 AI Agent 能夠理解動畫需求、撰寫動畫程式並輸出標準 Lottie 檔案的 Skill 系統。

在過去，如果想製作網站或 App 使用的 Lottie 動畫，通常需要經過 Figma、Adobe Illustrator、After Effects，再透過 Bodymovin 匯出 JSON 檔案的流程。對沒有動態設計背景的開發者而言，門檻相對較高。而 diffusionstudio/lottie 試圖把這個流程轉變成類似「描述需求 → AI 產生動畫」的工作模式，讓使用者直接以文字說明動畫效果，例如圖示彈出、路徑繪製、數據變化或 Logo 動畫，再由 Agent 生成對應的 Lottie 動畫。

從專案介紹來看，作者將其稱為 **text-to-lottie** ，並強調其設計目標是生成真正可投入產品使用的動畫，而不只是展示性質的概念作品。這點與許多 AI 動畫生成工具有明顯差異。許多工具最終輸出的是影片，而 Lottie 本身則是一種向量動畫格式，具有體積小、可編輯、跨平台與解析度無關等特性，因此特別適合網站、行動 App、SaaS 產品與 UI 動效應用。

從功能定位來看，lottie 的主要特色包括：

- 使用自然語言生成 Lottie 動畫
- 支援 Claude Code、Codex、Cursor 等 AI Agent
- 可直接將 SVG 素材轉換為動畫
- 適合製作 Logo 動畫與 UI Motion Design
- 支援資料驅動動畫（Data-Driven Animation）
- 輸出標準 Lottie JSON 格式
- 可整合到網站與 App 前端專案
- 開源且可客製化工作流程

這些能力讓它更像是一套 AI Motion Design Workflow，而不是傳統動畫工具。

實際應用上，專案特別適合處理短篇向量動畫。例如：

- SaaS Dashboard 數據動畫
- App Onboarding 動畫
- Loading Animation
- 品牌 Logo Reveal
- Icon Micro Interaction
- 產品功能展示動畫
- Landing Page 視覺效果

官方分享的案例中，甚至可以直接提供 Spotify Logo SVG，然後要求 AI 產生描邊動畫、淡入效果與顏色控制參數。AI 會依據描述產生完整的 Lottie 動畫結構，而非單純輸出影片。

值得注意的是，專案也明確指出其最佳使用範圍。text-to-lottie 最適合：

- 單場景動畫
- 向量圖示動畫
- Motion Graphics
- 資料視覺化動畫

若需求涉及：

- 多鏡頭影片
- 複雜剪輯
- 音訊同步
- 長篇敘事動畫

則更建議使用 Remotion、After Effects 或其他影片製作工具。換句話說，它專注於 UI 與產品動畫，而非電影級內容製作。

從技術發展角度來看，這個專案也反映了 AI 設計工作流正在發生變化。過去 AI Coding Agent 主要負責產生程式碼，而現在開始逐漸涉入：

- UI 設計
- UX 設計
- 品牌設計
- 插畫生成
- 動畫製作

而lottie 則是將 Motion Design 納入 Agent 工作流的重要案例之一。它讓動畫不再完全依賴專業設計工具，而能透過自然語言進行描述與迭代。

與傳統的 Lottie 生態相比，像是 Lottie、LottieFiles 或 lottie-web 著重於動畫播放與管理；lottie 則聚焦在動畫的「生成」階段，希望讓 AI 成為動畫設計師與動態工程師之間的橋樑。

從適用族群來看，這個專案特別適合：

尤其是經常使用 Claude Code 或 Cursor 建立產品原型的人，可以大幅降低製作動態效果的成本與時間。

整體而言，lottie 並不是要取代 After Effects，而是希望讓 AI Agent 能夠直接參與 Lottie 動畫製作流程。它將原本需要設計工具與動畫專業知識的工作，轉變成自然語言驅動的創作模式，讓開發者能夠更快速地產生網站與 App 所需的高品質動態內容。隨著 AI Agent 與設計工作流持續融合，這類 Text-to-Lottie 工具很可能成為未來前端開發的重要輔助工具之一。