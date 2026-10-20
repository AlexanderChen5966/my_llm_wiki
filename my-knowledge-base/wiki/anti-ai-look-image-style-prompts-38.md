---
title: 消除圖像 AI 感的四個手法：只寫 hand-drawn 無效，必須明講「要怎麼崩壞」
date: 2026-09-09
tags: [ai/prompt-engineering, type/how-to]
source: https://codelove.tw/@tony/post/3kV49x（原文 https://qiita.com/maeda-niku18/items/b633dda178c505461080）
type: article
status: processed
asset_value: 36
---

## T（核心論點）

> **只寫 `hand-drawn` 或 `illustration` 沒有用**——這些詞只會把畫面拉回模型心中
> 「常見插畫」的**平均值**，所以必須明確寫出「要怎麼崩壞」。

作者用同一題目（「請畫一位日本少女」）實測 38 組 STYLE 提示詞並排比較，歸納出消除 AI 感最有效的四件事：

1. **直接要求不完美** — `slightly irregular`、`uneven line pressure`、`intentional asymmetry`
2. **借用物理質感** — 紙紋、墨色偏移、網點（印刷／版畫系因此最強）
3. **用數字限制色數與留白** — `limited three-to-five-color palette`、`generous negative space`
4. **寫出 AVOID 禁止清單** — 光澤漸層、完全對稱的臉、塑膠感表面

AI 感的來源是「線條太一致、漸層太平滑、臉太工整、背景莫名多出無意義小物件」，
而放著不管，**物件與色彩通常都會越來越多**——所以限制比描述更有效。

## B（適用場景）

**適用：**
- 用 ChatGPT / GPT Image 做資料、投影片、網站用插圖，需要不被一眼認出是 AI 生成
- 需要跨多張圖維持一致畫風（範本以「區塊」為單位替換不易跑掉）
- 想把 prompt 資產化成可重複套用的模板集

**不適用 / 注意：**
- ⚠️ 作者自陳的元問題：**看太多 AI 圖後會失去判準**，最後「要不要就這樣用」的決定仍在人類
- ⚠️ 操作慣例是**內容用日文（母語）描述、畫風用英文寫**——`linework`、`halftone`、`registration` 這類術語英文較有效
- ⚠️ `flat illustration` 單獨使用最容易變成滑順向量 AI 感，必須另加不完美設定
- ⚠️ 實測中出現「構圖重複」現象，需明確重置

## R（操作步驟）

### 1. 基本用法只有兩步

1. 先用母語寫出想畫的內容
2. 接在後面貼上選定的 `STYLE:` 提示詞

### 2. 完成版範本採四區塊結構

```
[內容描述]

VISUAL STYLE:
hand-drawn editorial illustration, organic irregular linework, subtle variations in line
thickness, simplified shapes, slightly imperfect proportions, restrained details, soft matte
colors, limited harmonious color palette, subtle paper and print texture, intentional
asymmetry, natural spacing, generous negative space.

HUMAN-MADE CHARACTER:
preserve small drawing imperfections, uneven line pressure, slightly irregular curves,
subtle shape variations, natural visual rhythm, understated design decisions, handmade texture.

COMPOSITION:
simple clear composition, one obvious visual focus, only necessary objects, believable
spatial relationships, uncluttered background.

AVOID:
overly smooth vector curves, glossy gradients, plastic-looking surfaces, excessive rim
lighting, hyper-detailed rendering, perfectly symmetrical faces, generic stock illustration
aesthetics, excessive decorative elements, unnecessary background objects, artificial posing,
overly polished digital appearance.
```

🔑 **以區塊為單位替換，畫風才不容易跑掉。**

### 3. 七組可疊加的附加區塊，各治一種症狀

| 區塊 | 治什麼 |
| --- | --- |
| **保留手作感**（最泛用，優先記這個） | 整體過度完美 |
| 避免滑順 AI 插畫 | 平滑向量曲線、光澤、塑膠表面 |
| 避免資訊塞太滿 | 裝飾雜物、無用背景物件 |
| 人物自然 | 假笑、模特兒式擺姿 |
| 兒童插畫自然 | 小孩比例與表情過頭 |
| 背景不 AI | 重複物件、不可能建築 |
| 顏色不 AI | 過飽和彩虹色 |

### 4. 各分類的實測心得（38 種分六類）

- **印刷／版畫系最有力**（復古印刷、絲網、Risograph、版畫、木版畫）——物理不完美直接表現在畫面上
- 🔑 **最推薦的入門風格是 Risograph**：把墨色限制在 **2–4 色**最有效，去 AI 感首選從這個開始試
- **扁平系**：`flat illustration` 單獨用最容易 AI 化；加 `intentional asymmetry` 與
  `subtle print-like texture` 差異極明顯；並要補「讓重複元素不要機械式完全一致」這句
- **手繪／類比畫材系**關鍵單句：色鉛筆／水彩要**「讓白色紙面露出來」**；
  水彩＋鋼筆加 **`imperfect registration`（線條與顏色稍微錯位）**最接近人畫的書本插圖
- 其餘三類：可愛角色（`slightly awkward charming proportions` 是核心）、漫畫遊戲（明寫筆壓變化）、
  實務教材（圖示／教材／教科書，重點在讓人看得出情境）
- 木版畫可加 `flattened perspective` 刻意壓扁空間深度

### 5. 兩種寫法都保留

作者同時提供條列式（STYLE 關鍵字串）與散文式（長文敘述）兩種——
**長文散文式提示在某些情境反而更有效**，如「像是在筆記本上用筆快速畫出來的感覺」這類比喻。

## C（關鍵行動）

- [ ] 把第 2 節的通用四區塊範本存成可複製的 prompt 模板
- [ ] 先只試 Risograph（2–4 色）與「保留手作感」附加區塊，驗證去 AI 感效果
- [ ] 若已在用扁平風，補上 `intentional asymmetry` + `subtle print-like texture` 並前後對比
- [ ] 檢查既有圖像 prompt 是否還在用 `hand-drawn` 這種無效詞

## 原始摘要

作者用同一題目實測 38 組 STYLE 提示詞並排比較，歸納出消除「AI 感」最有效的四個手法：直接要求不完美、
借用物理質感（紙紋／墨色偏移／網點）、用數字限制色數與留白、以及明確寫出 AVOID 禁止項。核心反直覺
結論是只寫 `hand-drawn` 或 `illustration` 沒用，這些詞只會把畫面拉回模型心中「常見插畫」的平均值。
完成版範本採 VISUAL STYLE / HUMAN-MADE CHARACTER / COMPOSITION / AVOID 四區塊結構，另有七組可疊加
的附加區塊各治一種症狀。操作慣例是內容用日文（母語）描述、畫風用英文寫。

## 相關連結
- [[chatgpt-image-prompts-80]] — 🔑 **同為圖像 prompt 模板集但角度不同**：該篇是「按用途分類的 80 組模板」，本篇是「消除 AI 感的四個手法＋四區塊範本」，論點不重疊
- [[ai-flavor-removal-writing-skills]] — 🔑 **跨模態對照（本篇最高價值的連結）**：文字版去 AI 感；本篇是圖像版，兩者的共通判準都是「要明確寫出不完美，而非只說要自然」
- [[ai-flavor-removal-writing-technique-guide]] — 去 AI 感寫作技法指南；與本篇構成「去 AI 感」的文字／圖像雙軌
- [[chatgpt-images-2-5-sketch-and-api-models]] — 目前生成模型已進版至 Images 2.5，本篇提示詞可搭配其 Sketch 指定構圖使用
