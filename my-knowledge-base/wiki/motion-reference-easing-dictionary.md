---
title: モーションリファレンス：782 種動態的日文動態設計辭典（動作、時長、緩動查表）
date: 2026-10-30
tags: [dev/]
source: https://motion-reference.takayustudio.jp/
type: reference
status: processed
asset_value: 30
---

## T（核心論點）

這是一個可依**動作名稱、速度、緩動與手感**查找的日文動態設計辭典，收錄 782 種 demo（涵蓋 UI、遊戲、捲動、表單、手勢、SVG），每條標註時長與行為描述。內容是**條目清單而非論述**，價值在於提供 UI／網頁動效的語彙與參數參考。

## B（適用場景）
**適用：** 寫動效需求或對 AI 下動畫指令時，需要精準名詞與時長數字；想查某種手感（例如 overshoot、spring、mask wipe）的行為定義。
**不適用 / 注意：**
- 剪藏內容夾雜「語學アプリを試す」「ロゴ制作を依頼」等 demo 用 UI 文字，屬雜訊
- 無原理說明；數值（毫秒）為各 demo 的示範值，非標準
- 主題與 [[claude-motion-beta-2026-10]]（Claude Motion 產品）不同，僅同屬「動態」領域

## R（操作步驟）
分類：出現退場、移動、縮放、旋轉、彈跳反彈、清單網格、注目強調、發光、特效切換、Web 捲動、Web 互動、版面轉場、載入資料；另有原則、緩動、彈簧、鏡頭、轉場。文字動態 228 種附 CSS、GSAP、After Effects 作法。

代表條目：fade＋slide 600ms、overshoot 600ms、bounce 落地 700ms、spring 追隨 1800ms、card flip 760ms、modal morph 2200ms。

可從描述歸納的設計準則：
1. 回彈幅度逐次遞減
2. 轉場不產生空白幀
3. 進度條數值與寬度同步
4. 卷動進度與畫面 1:1 對應

## C（關鍵行動）
- [ ] 下動畫提示詞時引用辭典中的動作名與時長，取代「做得流暢一點」
- [ ] 需要時直接擷取對應條目的 CSS／GSAP 作法

## 原始摘要

2026-10-27 剪藏的日文網頁（takayustudio），內容為 782 條動態範例清單，每條含類別、時長與一句行為描述。

## 相關連結

- [[claude-motion-beta-2026-10]] — Claude 寫程式做動畫的產品，可用本辭典的語彙下指令
- [[claude-code-pure-code-animation-practice]] — 純程式碼動畫實作，辭典可作為動效詞彙來源
