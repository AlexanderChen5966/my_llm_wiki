---
title: "Claude Animation Base 教學：用手繪角色範本做15秒動作故事"
source: "https://www.aiposthub.com/ai-animation-character-base-prompt-workflow/"
author:
  - "[[Philo]]"
published: 2026-10-24
created: 2026-11-05
description: "Claude Animation Base 提供 p5.js 手繪角色動畫起步範本。從 X 分享追到原始專案，看懂先讀指南、做分鏡、逐鏡渲染與接觸表檢查，附自有角色需求單。"
tags:
  - "clippings"
status: processed
---
角色動畫最容易在換鏡頭時失去一致：上一幕的表情、比例或動作，到了下一幕就接不上。Claude Animation Base 把角色與動畫規則整理成起步專案，讓新片有共同基礎。

這個範本使用 p5.js 等程式繪圖工具，把角色、畫面與時間寫進可以修改的檔案。因為它也附有製作指南，代理可以先規劃故事，再逐鏡完成。

它適合想做短小角色故事、願意保留程式專案的人。製作自己的片時，應換成自有角色或已取得權利的圖像。

## Claude Animation Base 的真正模板在哪裡？

[Saman Ahmed 的 X 分享](https://x.com/ScaleWthAI/status/2106386087463772495?ref=aiposthub.com) 列出這個專案，內容與使用方式則以 [JohnHeibel 的原始專案](https://github.com/JohnHeibel/ClaudeAnimationBase?ref=aiposthub.com) 為準。

作者把它從較早的 [PDoomVideo](https://github.com/JohnHeibel/PDoomVideo?ref=aiposthub.com) 經驗整理出來，提供角色、場景、時間設定與輸出工具。這篇聚焦可重用的角色模板，而不是重做原本的音樂影片。

專案建議先讀 ANIMATION\_GUIDE.md，再開始製作。它也說明水彩填色在部分硬體上可能較慢，因此不能用別人的片長直接推算自己的渲染時間。

![Claude Animation Base 教學，原作者公開示範畫面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/character-base-source-cover.webp)

JohnHeibel 公開的動畫示範畫面，非本站實測。來源： JohnHeibel 。

## 先用一個看得懂的動作練習

第一次不要同時安排很多角色和場景。可以先做「角色想拿到桌上的物件，失敗一次後換方法成功」，讓觀眾能從動作理解前因後果。

每個鏡頭先寫起點和終點，再確認它如何接到下一幕。完成幾個鏡頭後，把關鍵畫面排成接觸表，也就是一次看多張縮圖的總覽，容易發現角色忽大忽小或場景突然改變。

| 階段 | 先確認的事 |
| --- | --- |
| 角色設定 | 比例、表情與主要特徵 |
| 分鏡 | 每幕發生一件什麼事 |
| 動作 | 預備、執行與反應能否看懂 |
| 接觸表 | 前後鏡頭的角色與方向是否一致 |

## 可複製的角色動畫需求

先取得原始專案，在 Claude Code 開啟該資料夾，再讓它讀指南與自己的角色素材。這樣代理才能沿用範本中的場景和輸出工具。

以下為本站依模板做法改寫，非原作者逐字 prompt，尚未做成片實測。

```
先讀 Claude Animation Base 的 ANIMATION_GUIDE.md。
使用我提供的［自有角色設定圖］，做一支 15 秒動作故事。
故事是角色想拿到高處的書，第一次沒拿到，換方法後成功。
先給分鏡，確認角色比例、動作起點與每幕結束狀態。
按鏡頭製作，每完成一段就輸出接觸表檢查連續性。
不要沿用模板角色的品牌身分，也不要新增未提供的角色。
若水彩效果太慢，先提出保留風格的簡化方案。
交付 MP4、場景程式、角色素材與時間設定。
```

## 渲染完成後，再看角色是否真的有意圖

動作流暢仍可能看不懂角色在做什麼。先靜音觀看，確認角色的目標、失敗與反應能從姿勢讀出來，再決定是否需要旁白。

接著檢查換鏡頭的位置，尤其是角色面向與物件所在方向。觀眾能接續追蹤同一件事，故事才會成立。

### 模板中的角色能直接放進商業作品嗎？

不要由程式可取得推定角色可商用。保留技術做法，替換成自己有權使用的角色比較清楚。

### 需要很強的顯示卡才能開始嗎？

不同繪圖效果的需求不同。先渲染短段確認速度，必要時減少水彩等較重效果，再決定整片風格。

從一個角色、一個目標與一次轉折開始，比直接要求完整卡通更容易練出能連續製作的流程。

延伸閱讀： [Claude 動畫做完怎麼交付？HTML、SVG 與 MP4 的用途和檢查重點](https://www.aiposthub.com/claude-animation-html-svg-mp4-delivery-formats-guide/)

延伸閱讀： [2026 Claude Code 做影片 Skills 推薦：HyperFrames、Remotion 到剪輯工具怎麼選？](https://www.aiposthub.com/claude-code-video-skills-2026/)