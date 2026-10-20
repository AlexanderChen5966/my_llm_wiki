---
title: Gemini Omni 影片生成：六維度 Prompt 框架與 30 個模板
date: 2026-06-24
tags: [ai/prompt-engineering, ai/agent]
source: https://codelove.tw/@tony/post/3K55Ya
type: reference
status: processed
asset_value: 42
---

Gemini Omni（音視覺同步）與 Veo 3.1（無聲高畫質）的分工框架，以及六維度 Prompt 結構——提供影片生成時的結構化思考框架，可套用在 App Store 截圖影片、產品展示等場景。

## T（核心論點）

影片生成 Prompt 與文字/圖片 Prompt 的關鍵差異：需要同時描述**六個維度**，缺少任一維度會讓 AI 自行填補，結果往往不符預期。

## B（適用場景）

**Gemini Omni（音視覺同步）適合：**
- 短 Reels、社群短片
- 人聲旁白、有情境感的內容
- 需要畫面與音效同步的場景

**Veo 3.1（高品質無聲影片）適合：**
- 產品展示、精緻 B-roll
- App Store 截圖影片
- 需要高畫質但不需要聲音的場景

## R（六維度 Prompt 結構）

| 維度 | 描述 | 範例 |
|------|------|------|
| **場景** | 拍攝地點與環境 | 「咖啡廳靠窗座位，下午自然光」|
| **動作** | 主體在做什麼 | 「慢慢端起杯子，看著窗外」|
| **情緒** | 整體氛圍與感受 | 「寧靜、放鬆、沉思」|
| **音效** | 背景聲或配樂 | 「輕柔爵士樂 + 環境咖啡廳聲」|
| **鏡頭** | 鏡頭語言 | 「中景慢慢推進，淺景深」|
| **時長** | 目標秒數 | 「6 秒」|

八大類 30 個模板（Reels / 產品 Demo / Logo Reveal / 訪談 / 教學 / 廣告 / 短劇 / 環境氛圍）涵蓋常見影片類型。

## 補充資料｜Gemini Omni 1.1 Flash 正式版（2026-09-01 併入）

來源：https://www.bnext.com.tw/article/92032（官方公告 2026-08-27）。已列入 Gemini API 付費層正式推出版本（**免費層暫不提供**）。

🔑 **本次更新的核心不是畫質，而是把影片生成轉成「可反覆修改的工作流」**：

- 🔑 **場景延伸「回看 10 秒」**：舊模型銜接影片時**只參考最後 1 秒**，生成內容容易走鐘；Omni 1.1 可將輸入影片結尾前**多達 10 秒**納入上下文，大幅提升視覺一致性與敘事連貫度。延伸**以 10 秒為單位**進行，**累計總長最高 40 秒**
- **指定首尾影格**：在兩個關鍵影格之間自動補算連續影像——適合複雜環繞運鏡、變焦轉場、無縫循環素材
- **360p 快速預覽**：相較標準 720p，生成速度提升**最高達 60%**（官方註明基於系統吞吐量比較），**成本僅約三分之一**；瞄準快速原型、分鏡迭代，確認效果後再出 1080p 或 4K
- **輸入端參考影片**：最多 3 支，每支最長 3 秒
- ⚠️ **舊預覽版端點 `gemini-omni-flash-preview` 預計 2026-09-30 終止服務**

## 補充資料｜Cinematique：把「鏡頭」維度換成可查詢的電影技法（2026-09-09 併入）

來源：〈Cinematique：AI 影片不知道怎麼運鏡？150+ 電影技法與 Prompt 免費查〉，
tools.wingzero.tw，2026-09-09，<https://tools.wingzero.tw/articles/4057>（站點 <https://vvsvs.pro/cinematique>）。

六維度框架最難自己寫的一格是**鏡頭**——多數人只能寫「cinematic、8K、film look」這類無方向的關鍵字，
等於把該維度留給 AI 自行填補。Cinematique 是免費 Cinematic Prompt Library，正好補上這一格的詞彙來源：

- 收錄 150+ 種電影技法，分為 Camera Angle / Camera Movement / Lens / Lighting / Film Stock /
  Composition / Narrative Technique，可依 Category、Difficulty、Mood 瀏覽；每個 Prompt 保留
  `[Subject]` 佔位，替換主體即可套用，且不綁模型（Midjourney / Runway / Veo 皆可帶過去測）
- 每條技法附「為什麼用這顆鏡頭、能傳達什麼」的解說，可反向校準六維度中的**情緒**格
- 展開的粒度即是六維度所要求的精確度：Dutch Angle 不只寫「畫面拍歪」，而是 Camera Tilt ＋
  Diagonal Lines ＋ Shadow ＋ Film Grain ＋ Anamorphic Distortion；Dolly Shot 會描述攝影機平順
  移向主體、以及前中後景的 Parallax——正是「不描述就會走鐘」的那類細節
- 分鏡層級的用法：先排 `Establishing Shot → Medium Shot → Close-Up → Insert Shot →
  Extreme Close-Up`，再逐鏡選 Lens / Lighting / Camera Movement，比每幕都丟「cinematic scene」
  更容易維持一致的影片語言（與 Omni 1.1「回看 10 秒」的一致性訴求同向）

🔑 **可轉用到 Prompt／Skill 資產化的判準：Prompt Library 該按「可重複套用的技法」分類，而不是按題材分類。**
一般 Prompt 站按「科幻城市／人像／產品攝影」等題材整理，查詢時只能靠題材聯想；Cinematique 按技法分類，
同一個 Low Angle 或 Dolly Shot 可套用在人像、汽車、商品、建築、動畫角色上，查詢行為變成**技法檢索**。
自建 prompt／Skill 庫時同理：以「可跨題材重複套用的手法」當分類軸，資產的複用率才會高。

## C（關鍵行動）

- [ ] 製作 App Store 截圖影片時：先套六維度框架再生成，Veo 3.1 優先
- [ ] 製作社群內容時：Gemini Omni + 六維度，重點放在「情緒」和「音效」
