---
title: "Glif 拼貼動畫教學：用腳本與視覺風格做出解說短片"
source: "https://www.aiposthub.com/ai-animation-glif-cutout-prompt-workflow/"
author:
  - "[[Philo]]"
published: 2026-10-24
created: 2026-10-28
description: "Glif 作者分享用腳本或想法製作拼貼解說動畫的公開卡片。本文整理可讀入口、腳本準備、視覺規則與驗收方式，附本站改寫需求，不將展示片視為已實測成果。"
tags:
  - "clippings"
status: processed
---
想把抽象概念做成解說片，拼貼可以讓名詞變成可移動、可連接的圖像。Glif 作者 Fabian 的公開示範，就是把語言模型概念做成剪報般的畫面。

這種做法的重點是先知道每句話要呈現哪個關係，再交代畫面風格。否則即使每張圖都像剪報，觀眾仍可能不知道它們為什麼被放在一起。

本文依公開卡片與示範整理使用準備，並提供本站另寫的需求。卡片可讀代表入口存在，實際生成、帳號條件與費用仍需在使用時確認。

## Glif 作者公開了哪個入口？

[Fabian 的 X 貼文](https://x.com/fabianstelzer/status/2093081833911058772?ref=aiposthub.com) 展示以 H3 Max 製作的解說動畫，並在 [後續留言](https://x.com/fabianstelzer/status/2093081902240383351?ref=aiposthub.com) 提供 [Explainer Motion Graphics Video 卡片](https://glif.app/chat/cards/explainer-motion-graphics-video--LjZUBa3F?ref=aiposthub.com) 。

這個案例可以看出腳本、想法與視覺方向的組合，但沒有證明任何輸入都能一次得到同樣效果。作者提到的花費也和片長有關，不能當成固定報價。

挑選素材時，可以借用剪報、線條和標記的表現方式，並換成自己的圖片、文字與配色，建立容易持續使用的視覺規則。

![Glif 拼貼動畫教學，原作者公開示範畫面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/glif-cutout-source-cover.jpg)

fabianstelzer 公開的動畫示範畫面，非本站實測。來源： fabianstelzer 。

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/10/glif-cutout-source-demo.mp4" width="1344" height="768" controls=""></video>

fabianstelzer 公開示範影片，非本站實測。來源： 原始發布 。

## 先把講稿改成畫面關係

例如要解釋「資料先整理，再交給模型回答」，可以先用幾張文件形成一疊，再由箭頭送進一個處理區。這比同時放上資料、機器人與很多標籤更容易看懂。

寫腳本時，把每一段補上這幾項資訊：

| 內容 | 要寫清楚的事 |
| --- | --- |
| 口播 | 這一句要讓觀眾理解什麼 |
| 主圖像 | 用哪個物件代表這個概念 |
| 關係 | 物件如何連接、分開或改變 |
| 文字 | 哪個詞需要留在畫面上 |

如果一個畫面無法用一句話說清楚，先拆成兩幕。生成的成本與修稿量，常常會隨同一幕的資訊數量增加。

## 可複製的拼貼動畫需求

先從上方原作者卡片查看現行使用說明，再把下方文字作為腳本與風格需求。本文沒有驗證帳號生成流程，因此不指定介面按鈕。

以下為本站依公開案例改寫的範本，非官方卡片的完整原始 prompt，尚未做成片實測。

```
把以下講稿做成 40 秒拼貼解說動畫：［講稿］。
觀眾是第一次接觸這個概念的人，畫面使用紙張、剪報和線條。
先為每句話列出一個主圖像與它要表達的關係。
同一概念使用相同圖像與顏色，避免每幕更換象徵物。
先給分鏡與短段預覽，不先生成整片。
使用我提供的素材；需要外部圖片時先列來源與費用。
保留畫面文字和字幕，方便我核對專有名詞。
```

## 看完後，先檢查觀念有沒有被畫錯

圖像能讓人更快理解，也可能讓人誤會。例如把模型畫成裝滿原始文件的箱子，可能暗示一種不正確的運作方式，應回頭核對腳本的意思。

再檢查剪報邊緣、文字與箭頭。裝飾如果遮住主體，或線條指向不明，會讓觀眾花時間猜畫面，反而失去解說的用途。

### 這篇是否已在 Glif 生成過影片？

沒有。本文驗證公開來源與入口，提供準備方法和需求範本，未聲稱完成卡片出片實測。

### 可以直接複製示範的旁白和剪報嗎？

應使用自己的腳本與有權使用的素材。視覺做法可以作為參考，原片的影像、聲音與文字仍各有來源。

先把一段最難解釋的話改成兩個物件之間的動作，再用短段預覽測試觀眾能否看懂。

延伸閱讀： [Claude 動畫做完怎麼交付？HTML、SVG 與 MP4 的用途和檢查重點](https://www.aiposthub.com/claude-animation-html-svg-mp4-delivery-formats-guide/)

延伸閱讀： [2026 Claude Code 做影片 Skills 推薦：HyperFrames、Remotion 到剪輯工具怎麼選？](https://www.aiposthub.com/claude-code-video-skills-2026/)