---
title: ChatGPT Image 2.0 提示詞大全：80 組模板與五類框架
date: 2026-06-24
tags: [ai/prompt-engineering]
source: https://www.bnext.com.tw/article/84558/chatgpt-image-prompt-80
type: reference
status: processed
asset_value: 52
---

ChatGPT Image 2.0 的圖像生成能力大幅提升，但結果好壞取決於 Prompt 的結構。80 組模板按用途分 5 類，核心原則一致：**主體 + 風格 + 情緒/氛圍 + 細節 + 用途**。

> 更新版本：此為 80 組版（前一版 70 組已處理）。新增了更多品牌設計與照片風格轉換模板。

> ⚠️ **模型已進版（2026-09-09 加註）**：本條目的提示詞是針對 **ChatGPT Image 2.0** 撰寫的**前代**版本。
> OpenAI 已於 2026-09 推出 **Images 2.5**（生成延遲最多降 50%、修圖更精準、新增 `@Sketch` 手繪草圖等
> 四項新功能），多輪編輯一致性與可用的下指令方式都與 2.0 不同。
> 本條目的結構公式（主體＋風格＋配色＋情緒＋構圖＋用途規格）仍成立，但套用前請先確認新模型的行為差異
> ——詳見 [[chatgpt-images-2-5-sketch-and-api-models]]。

> 📌 **已有更新更完整的 130 組版本（2026-09-10 加註）**：同站另一篇（bnext article/90908，2026-09-10）
> 已整理到 **130 組**，五大類架構相同但**照片風格轉換類增加到約 88 組**，並附一組可組合的微調模組
> （人物姿勢與表情／服裝／髮型妝容／燈光方向／質感色彩／底片感與鏡頭／背景版面）。
> 兩者是**不同文章、不同分工，不是取代關係**：
> - **本條目 ＝ 框架論述**——Image 2.0 的「設計語境」判準（要說清**用途**與**設計規格**）、
>   提示詞結構公式與高頻風格關鍵字。想知道「一句好的提示詞為什麼好」看這裡。
> - [[chatgpt-image-prompts-130]] **＝ 提示詞合集與查詢入口**——五大類、130 組的分類架構、
>   每類代表範例與選用判準。想「找一組現成的來改」看那裡。

## T（核心論點）

Image 2.0 能理解「設計語境」，不只是繪圖——所以 Prompt 要說清楚**用途**（LINE 貼圖 vs 產品海報 vs 簡報封面）和**設計規格**（尺寸、留白、清晰度）。

## B（五大類別場景）

| 類別 | 核心用途 | 代表模板 |
|------|---------|---------|
| **品牌社群類** | IG 貼文、產品展示、品牌識別 | 品牌 Logo 展示圖、產品白底圖、IG 輪播首圖 |
| **工作應用類** | 簡報封面、報告圖表、提案插圖 | 科技感數據視覺化、簡報封面頁、會議封面 |
| **生活實用類** | 食譜卡、活動海報、LINE 貼圖 | LINE 貼圖（8 格/16 格）、食譜封面、插畫菜單 |
| **靈感點子類** | 創意插畫、概念圖、旅遊海報 | 城市旅遊復古海報、毛線材質地圖、手繪概念稿 |
| **照片風格轉換類** | 現有照片改風格 | 吉卜力風、水彩插畫、彩色鉛筆素描 |

## R（提示詞結構公式）

**完整版公式**（複雜圖）：
```
[主體描述] + [視覺風格] + [配色方案] + [情緒/氛圍] + [構圖方式] + [用途與規格]
```

**簡易版公式**（快速圖）：
```
[主體] + [風格關鍵字] + [用途]
```

### 高頻風格關鍵字

| 風格 | 適用場景 |
|------|---------|
| `flat design, pastel colors` | 品牌社群、APP 插圖 |
| `photorealistic, clean white background` | 產品展示 |
| `watercolor illustration` | 食譜、生活風格 |
| `retro travel poster` | 旅遊海報、懷舊設計 |
| `Ghibli-style` | 創意插畫、場景氛圍 |
| `knitted texture, top-down view` | 地圖類、特色插圖 |
| `minimalist, blueprint` | 技術說明、簡報 |

### LINE 貼圖專用格式

```
Create 8 cute sticker-style illustrations of [角色/主題].
Each sticker should show [表情/動作1], [表情/動作2]...
Style: simple lines, white outline, transparent background
Format: 8 panels, 370x320px each, suitable for LINE stickers
```

## C（關鍵行動）

- [ ] 製作品牌素材時：先定義 **用途** 再寫 Prompt，不要只描述「畫什麼」
- [ ] LINE 貼圖：用「8 panels」或「16 panels」格式一次生成，加 `white outline` 確保裁切乾淨
- [ ] 照片風格轉換：加 `maintain original composition, only change art style` 保留構圖

## 相關連結

- [[chatgpt-image-prompts-130]] — **更新更完整的 130 組合集**（bnext article/90908，2026-09-10）：
  本條目是框架論述，該條目是提示詞合集與查詢入口
- [[chatgpt-images-2-5-sketch-and-api-models]] — **後續版本**：Images 2.5 的四大新功能與 API 模型（本條目為 2.0 前代提示詞）
- [[gemini-omni-video-prompts]] — Gemini 影片生成六維度框架（影片版 Prompt 結構）
- [[ai-prompt-map-100-workflows]] — AI 100 個工作流 Prompt 地圖
- [[comfyui-getting-started]] — 本地節點式圖像生成（ComfyUI）：雲端提示詞技巧之外的本地生圖工作流，可對照 CLIP 提示詞寫法
