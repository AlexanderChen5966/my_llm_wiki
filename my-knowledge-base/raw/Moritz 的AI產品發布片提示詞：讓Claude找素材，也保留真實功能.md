---
title: "Moritz 的AI產品發布片提示詞：讓Claude找素材，也保留真實功能"
source: "https://www.aiposthub.com/ai-animation-saas-launch-prompt-workflow/"
author:
  - "[[Philo]]"
published: 2026-10-24
created: 2026-11-05
description: "AI產品發布片怎麼做？Moritz Kremb 公開用 Opus 5.5 製作 SaaS 發布片的提示詞，並回覆使用 HyperFrames。本文整理選產品、找真實素材、功能核對與可編輯交付，附本站改寫需求。"
tags:
  - "clippings"
status: "processed"
---
AI產品發布片讓 Claude 自己選一個產品、找素材並製作，適合測試它如何理解產品。要把同一做法用在自己的品牌，仍需提供正確功能與可使用的素材。

Moritz Kremb 的公開案例把選產品、找真實圖片與製作動態短片放在同一份需求中。它的啟發是讓代理先研究產品，而不是拿泛用圖卡填滿影片。

這種做法適合已有官網、介面與明確價值的軟體產品。第一次先選一項功能，讓畫面從問題一路走到結果。

## AI產品發布片原作者的 prompt 和工具能查到嗎？

[Moritz 的原貼](https://x.com/moritzkremb/status/2103066071838466494?ref=aiposthub.com) 展示成片， [後續留言](https://x.com/moritzkremb/status/2103066083905417476?ref=aiposthub.com) 公開初始需求。他也 [回覆使用 HyperFrames](https://x.com/moritzkremb/status/2103181107466567815?ref=aiposthub.com) ，因此不必由影片外觀猜測工具。

原始需求強調找可辨識的產品、使用真實素材，以及呈現功能與好處。這些是公開可讀的方法，成片品質與製作速度則屬於作者那一次的結果。

把方法搬到自己的產品時，建議直接指定產品與資料來源，讓研究範圍更可控。

![Moritz 的AI產品發布片提示詞，原作者公開示範畫面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/saas-launch-source-cover.jpg)

moritzkremb 公開的動畫示範畫面，非本站實測。來源： moritzkremb 。

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/10/saas-launch-source-demo.mp4" width="1920" height="1080" controls=""></video>

moritzkremb 公開示範影片，非本站實測。來源： 原始發布 。

## 真實素材還要搭配真實操作

介面圖片是真的，影片裡的操作仍可能被演錯。例如用一個按鈕直接跳到產品其實做不到的結果，會讓宣傳內容失真。

先給代理一份短表，讓每個鏡頭都有可確認依據：

| 畫面 | 應提供的依據 |
| --- | --- |
| 使用前 | 觀眾遇到的具體問題 |
| 操作中 | 真實按鈕與正確步驟 |
| 使用後 | 可以驗證的結果 |
| 品牌收尾 | 正確 Logo 與下一步 |

素材從網站取得，也要記錄原始頁面與權利。公開可見的圖片，不會因為被代理找到就自動變成可自由重用。

## 可複製的SaaS發布片需求

先在已具備 HyperFrames 工作環境的 Claude Code 專案中放入產品資料，再送出需求。代理缺少素材時，先讓它列出清單。

以下是本站依公開方法改寫的範本，非原作者逐字 prompt，尚未做成片實測。

```
替［我的產品／官網］做一支 30 秒產品發布片。
先讀官網與我提供的功能說明，列出可以確認的三項資訊。
影片只聚焦［一項核心價值］，展示真實的操作與結果。
素材優先使用我提供的介面與品牌檔案，外部素材附來源。
先做場景表，說明每個鏡頭要證明哪一件事。
動態效果服務於操作理解，文字完整出現後留出閱讀時間。
先給短段預覽，再交付 MP4 與可修改的 HyperFrames 專案。
```

## 驗收時，請找沒看過產品的人試看

熟悉產品的人會自己補上中間步驟，第一次看到的人比較容易發現影片跳太快。看完後請對方說出「產品幫誰做了什麼」，可以快速檢查訊息是否留下。

再核對介面文字和功能結果。這些內容正確後，才有意義去微調動態的速度與精緻度。

### 一句 prompt 就代表完全沒有前置條件嗎？

不代表。模型、工具環境、素材取得與後續執行都會影響結果，公開貼文的簡短需求不能代表全部製作條件。

### 可以讓 Claude 自己選任意品牌練習嗎？

可以作為私人練習方向；公開使用時仍要處理素材與品牌呈現。用自己的產品資料做第一支片，最容易把功能與權利一起釐清。

先做一段能被陌生觀眾說清楚的產品操作，再增加鏡頭與視覺變化，發布片會更接近實際用途。

延伸閱讀： [Claude 動畫做完怎麼交付？HTML、SVG 與 MP4 的用途和檢查重點](https://www.aiposthub.com/claude-animation-html-svg-mp4-delivery-formats-guide/)

延伸閱讀： [2026 Claude Code 做影片 Skills 推薦：HyperFrames、Remotion 到剪輯工具怎麼選？](https://www.aiposthub.com/claude-code-video-skills-2026/)