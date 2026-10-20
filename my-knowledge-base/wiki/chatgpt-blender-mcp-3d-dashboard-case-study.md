---
title: 沒寫一行程式碼！我用 ChatGPT 5.6 (Sol) 做了一個智慧充電站 3D 大螢幕
date: 2026-07-16
tags: [ai/prompt-engineering, ai/workflow]
source: https://codelove.tw/@tony/post/qZrjAq
type: article
status: processed
asset_value: 58
---

從設計稿到能在瀏覽器打開、點擊充電樁跳出狀態面板的 3D 視覺化大螢幕，全程沒寫一行程式碼、沒在 Blender 手動拖過控制項。工具鏈：Codex（ChatGPT 5.6 Sol）+ Blender + blender-mcp + 既有大螢幕腳手架。

> 📎 2026-10-19：據 aiposthub〈Blender MCP 教學〉（2026-10-12）轉述，blender-mcp（ahujasid）已改名 **MCP for Blender**（第三方，非 Blender 官方），安裝與註冊指令可能和舊教學不同（**未核對 repo**），見 [[claude-codex-video-toolchain-selection]] 第 9 節附註；3D 作品結構與外觀分開驗收見 [[ideas/ai-3d-code-structure-vs-appearance-acceptance]]。

## T（核心論點）

拉開效果差距的關鍵不是換更強的模型，而是把「驗收標準」寫進提示詞——第一版切圖提示詞只描述任務（RGB 無 Alpha + 背景色校驗，兩個要求自相矛盾），模型完全照做但產出無法使用；第二版把「描述任務」改成「描述任務 + 每一步可執行的驗收標準」（透明度按死 PNG-32 RGBA、清晰度分路處理、雙底檢查流程），品質提升幅度比換模型明顯得多。沒有判定標準時，模型會挑最省事的路走，給你一個看起來合格但無法用的結果。

## B（適用場景）

**適用：** 用 AI 生成鏈路（設計稿→素材切割→3D 建模→前端整合）產出視覺化大螢幕；任何「AI 產出看起來對但實際不能用」的提示詞除錯情境。
**不適用 / 注意：** 作者指出 GPT-5.6 Sol 在 3D 建模仍無法真正還原設計圖的工業級質感；適合原型與展示場景，非最終生產級成品。

## R（四步驟鏈路）

1. **設計稿**：多輪定稿，並為切圖預留透明背景邊界（源頭設計稿沒留乾淨邊界，後面切圖會很費工）
2. **切圖**：第一版 27 個素材因提示詞自相矛盾（要求 RGB 無 Alpha 又要透明背景）全部白切；第二版把驗收標準寫死——PNG-32 RGBA 且空白區 Alpha=0、原圖>64px 走精確摳取/更小則走「受約束高清重建」（只理清邊緣不得憑空加細節）、每個素材切完在純白純黑背景下驗邊、一圖一資產禁止宮格拼圖
3. **建模與烘焙**：用 blender-mcp 讓 Codex 在 Blender 裡下發可重複執行的建模腳本；第一版材質是平的（無 PBR），重做時補齊金屬度/粗糙度分離、地坪微紋理、玻璃透射；烘焙前先按「將來要不要互動」分組——會被點擊的物體（車輛/充電樁/電池櫃）各自單獨烘焙保留獨立材質通道，純背景物體合併圖集減少渲染開銷
4. **拼成網頁**：接入既有大螢幕腳手架（`registerBlock` 註冊區塊、`autofit.js` 做等比縮放），互動物件包一層 Three.js 渲染元件掛到對應區塊

**實測數據**：源 GLB 約 77.3MB，Web 優化後壓到 25.32MB，107 張內嵌 WebP，23 個互動根節點完整保留。

## C（關鍵行動）

- [ ] 提示詞除了描述任務，務必補上可執行、可驗證的驗收標準（而非只有形容詞式要求）
- [ ] 檢查提示詞內部是否有自相矛盾的要求（如同時要求「無 Alpha」又要「透明背景」）
- [ ] 3D 場景烘焙前，先按「未來是否需要互動」分組決定獨立烘焙 vs 合併圖集，避免前端接入時大規模返工

## 相關連結

- [[ai-web-design-system-guide]] — AI 協作 Web 設計系統完整框架，可補充本篇未涉及的需求前期規劃面向
- 📎 2026-10-19：[[ideas/ai-3d-code-structure-vs-appearance-acceptance]] — 3D 結構（接觸、連續、穿模、文字）與外觀分開驗收，與本篇「驗收標準寫進提示詞」同方向；[[claude-codex-video-toolchain-selection]] — MCP for Blender 改名與安全模式（aiposthub 轉述）
