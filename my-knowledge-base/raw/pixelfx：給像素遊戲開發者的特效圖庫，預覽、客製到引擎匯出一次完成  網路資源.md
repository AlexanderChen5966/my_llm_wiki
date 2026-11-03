---
title: "pixelfx：給像素遊戲開發者的特效圖庫，預覽、客製到引擎匯出一次完成 | 網路資源"
source: "https://tools.wingzero.tw/articles/4143"
author:
  - "[[赤紅翼零]]"
published: 2026-11-02
created: 2026-11-03
description: "pixelfx 是一個可互動的像素特效圖庫，收錄攻擊、火焰、爆炸、魔法、天氣與介面動畫等遊戲常用效果。你可以先在瀏覽器中預覽，再調整色盤、速度、背景與鏡像方向，確認視覺效果後複製程式碼或輸出精靈圖，方便整合到網頁遊戲與其他遊戲引擎。"
tags:
  - "clippings"
status: "inbox"
---
網站連結 [https://patch-gg.github.io/pixelfx/](https://patch-gg.github.io/pixelfx/)

製作像素遊戲時，攻擊、受擊、爆炸、魔法或介面提示等動態效果，往往需要逐格繪製與反覆調整。pixelfx 將這些像素風格特效整理成可搜尋、可試播的互動圖庫，讓遊戲開發者先比較效果，再依需要調整顏色與播放方式，最後把程式碼或精靈圖帶進自己的專案。它同時提供瀏覽器展示頁與 JavaScript 程式庫，適合想快速找到參考、直接整合，或進一步製作自訂效果的開發者。

首頁可依火焰、爆炸、水、冰、雷電、科幻、近戰、投射物、狀態效果、天氣、介面、道具、轉場與生物等主題瀏覽，也能用關鍵字縮小範圍。點開單一效果後，畫面會即時播放預覽，並顯示名稱、用途標籤、播放時間與程式碼。預覽面板可以調整播放速度、背景、色盤與鏡像方向，也能重新產生隨機種子；使用者可查看角色動作是否有包含在效果裡，接著複製可用的程式碼或輸出精靈圖。

pixelfx 的特色不只在單個爆炸或火花。部分效果會和角色動作、命中停頓、目標受擊反應連在一起，形成從預備、出招到收勢的完整攻擊表現。這讓開發者能觀察效果如何配合動作節奏，而不只是把特效當成獨立貼圖播放。若遊戲已經有自己的角色動畫，也可參考效果提供的動作時序，安排揮擊、碰撞或受擊停頓。

- 以分類與搜尋快速找到攻擊、環境、介面或轉場效果。
- 在瀏覽器中試播，改變速度、色盤、背景、鏡像方向與隨機種子。
- 查看效果的用途標籤與示範程式碼，方便評估是否適合目前專案。
- 匯出逐格 PNG 精靈圖與 JSON 格式的影格資訊，供 Godot、Unity 等引擎的素材流程使用。
- 搭配 Claude Code 的 pixelfx 技能，搜尋、使用、匯出或從範例延伸製作新效果。

實際整合時，網頁遊戲開發者可直接使用 JavaScript 模組，放進 Canvas 或 Phaser、PixiJS 等工作流程；不使用 JavaScript 的引擎，也可以匯出精靈圖與影格資料，再依自己的專案載入。這個工具提供的是效果程式碼與素材輸出，不會自動替你完成遊戲場景、碰撞判定或整套動畫系統，因此仍需依遊戲的解析度、座標與渲染流程調整。

專案將程式碼與特效標示為 MIT 授權，允許在商業專案中使用，但散布程式或其重要部分時仍須保留授權聲明。另有部分程式依據其他 MIT 專案改作；Patch Pixel 字型則明確不包含在本專案的 MIT 授權範圍內。若要將素材或程式打包發佈，請先閱讀 GitHub 上的 [LICENSE](https://github.com/patch-gg/pixelfx/blob/main/LICENSE) 與 [NOTICE](https://github.com/patch-gg/pixelfx/blob/main/NOTICE.md) ，確認各項來源與授權要求。

整體來說，pixelfx 適合正在製作像素風遊戲、需要快速比較動態特效，或希望把效果轉成可直接整合素材的開發者。它把圖庫預覽、簡單客製、程式碼參考與精靈圖輸出串在一起，能減少從零尋找效果範例的時間；若需要高度符合自家美術風格的內容，則可從現有範例再調整，或使用附帶的技能延伸創作。前往 pixelfx 特效圖庫瀏覽並試玩效果。

<iframe frameborder="0" src="https://4bb365daadb69f9dfed14c008712a48c.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe><iframe frameborder="0" src="https://4bb365daadb69f9dfed14c008712a48c.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe>