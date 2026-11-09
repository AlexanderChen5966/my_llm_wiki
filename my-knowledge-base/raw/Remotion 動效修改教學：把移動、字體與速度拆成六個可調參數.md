---
title: "Remotion 動效修改教學：把移動、字體與速度拆成六個可調參數"
source: "https://www.aiposthub.com/ai-animation-motion-controls-prompt-workflow/"
author:
  - "[[Philo]]"
published: 2026-10-24
created: 2026-11-04
description: "beamnxw 分享把 Claude 動畫拆成六組可編輯控制項的做法。本文解釋移動距離、字體、模糊、速度曲線、跟隨與預備動作，附 Remotion 修稿需求範本。"
tags:
  - "clippings"
status: "inbox"
---
「更有動感一點」很難變成精準的修改。把物件移多遠、什麼時候停、標題要留多久寫成數值，代理才能知道這次要改哪一段。

beamnxw 在 X 分享了一套六組控制項的做法，搭配 Remotion 與 GSAP。前者用程式組成影片，後者安排動作隨時間變化，組合時需要讓它們對同一個時間點有一致理解。

這種方式適合已經有第一版動畫、接下來要反覆修節奏的人。先把一個鏡頭改成可調數值，比一開始替整片建立龐大控制台更容易驗證。

## Remotion 六組控制項各自修改什麼？

[原貼](https://x.com/beamnxw/status/2112180280110776797?ref=aiposthub.com) 把動作拆成六個方向。它引用的影片示範另有創作者署名，應保留 himanshutwtxs 的出處，不能把影片當成分享者自己的原創作品。

| 控制項 | 用人話描述修改 |
| --- | --- |
| 移動距離 | 兩個姿勢之間移多遠 |
| 字體配置 | 字體大小、字距與換行 |
| 動態模糊 | 快速移動時留下多少模糊 |
| 速度曲線 | 開始和結束時如何加減速 |
| 跟隨動作 | 主體停下後，尾端多晚才停 |
| 預備動作 | 正式移動前，是否先小幅反向蓄力 |

這些數值集中在一份設定中，修稿就能指向具體項目。例如尾端晚幾格停下，是在改動作的先後關係，無須連配色一起重做。

![Remotion 動效修改教學，原作者公開示範畫面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/motion-controls-source-cover.jpg)

himanshutwtxs（beamnxw 引用示範） 公開的動畫示範畫面，非本站實測。來源： himanshutwtxs（beamnxw 引用示範） 。

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/10/motion-controls-source-demo.mp4" width="1584" height="2016" controls=""></video>

himanshutwtxs（beamnxw 引用示範） 公開示範影片，非本站實測。來源： 原始發布 。

## 先讓動畫對準影片的影格時間

影格就是影片中的一張畫面。要逐格修改，應讓指定影格永遠顯示同樣的狀態，否則預覽看起來正常，輸出時仍可能錯位。

原貼連到 [Remotion 的 GSAP 時間軸整合說明](https://www.remotion.dev/docs/gsap/use-gsap-timeline?ref=aiposthub.com) ，也提供 [官方代理技能](https://github.com/remotion-dev/skills?ref=aiposthub.com) 。實作前讓代理閱讀專案使用版本的文件，再安排時間控制。

第一次只挑一個圖形進場，設定移動距離、進場時間與停留時間。確認逐格拖動時不會跳動，再加入跟隨與模糊等細節。

## 可複製的動效修稿需求

在已有 Remotion 專案的程式代理中送出需求，並讓它先讀取現有程式與版本。這份範本處理修稿，不是從零安裝的指令。

以下為本站改寫範本，並非原作者完整 prompt，尚未做成片實測。

```
請整理現有 Remotion 鏡頭，把以下項目集中成可修改設定：
移動距離、文字大小與字距、動態模糊、速度曲線、
跟隨延遲、預備動作時長。
先閱讀目前版本的 Remotion／GSAP 官方整合文件。
所有動畫以影片影格為準，拖到同一影格必須看到相同畫面。
先只改標題進場：預備動作增加 3 格，輸出與原版比較。
確認後再做第二輪，尾端延後 2 格停下，另存一版供比較。
標題完整出現後停留 1.5 秒，其餘場景保持原樣。
交付設定檔、修改前後的關鍵影格與短片預覽。
```

## 比較時，先一次只改一組數值

如果同時改距離、字體與速度，成片變好後也很難知道是哪個變動有效。保留原版，先只調預備動作，再只調跟隨延遲，會比較容易建立自己的判斷。

檢查標題時，要從完整出現的那一刻開始計算閱讀時間。把慢慢進場的時間也算進去，可能會讓觀眾還沒讀完，文字就離開了。

### 這六組控制項能套用到每一種動畫嗎？

它們是實用的修稿分類，但各鏡頭需要的項目不同。靜態數字卡可能只需要字體與停留時間，無須增加所有效果。

### 設定值調好就保證影片不會出錯嗎？

仍要檢查輸出片段。尤其是時間軸整合、字體載入與畫面邊界，需要在實際影片中確認。

找一個最常被要求重改的鏡頭，先把它變成能用數值描述的動作，這套方法才會開始替你省下修稿時間。

延伸閱讀： [Claude 動畫做完怎麼交付？HTML、SVG 與 MP4 的用途和檢查重點](https://www.aiposthub.com/claude-animation-html-svg-mp4-delivery-formats-guide/)

延伸閱讀： [2026 Claude Code 做影片 Skills 推薦：HyperFrames、Remotion 到剪輯工具怎麼選？](https://www.aiposthub.com/claude-code-video-skills-2026/)