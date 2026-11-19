---
title: "動態提示詞資源怎麼選？OneShotPrompts 與 Motion Library 的用法與限制"
source: "https://www.aiposthub.com/motion-reference-prompt-libraries/"
author:
  - "[[Philo]]"
published: 2026-11-11
created: 2026-11-11
description: "動態提示詞資源怎麼選？OneShotPrompts 與 Motion Library 的用法與限制。知道「想讓文字動起來」，和能清楚描述動作，是兩個階段。動態提示詞庫能補上語彙，但複製之後仍要決定時間、配色和輸出格式。"
tags:
  - "clippings"
status: "inbox"
---
知道「想讓文字動起來」，和能清楚描述動作，是兩個階段。動態提示詞庫能補上語彙，但複製之後仍要決定時間、配色和輸出格式。

OneShotPrompts 與 Motion Library 都提供動態參考，定位卻不同。前者有技巧說明與提示詞編輯方向，後者讓你從可看的效果挑選提示詞。

我會把它們當成和 AI 對齊畫面的工具，再依自己的內容重寫需求。先做一個動作，通常比一次要求整支影片更容易驗收。

## OneShotPrompts 怎麼幫忙寫提示詞？

[技巧目錄](https://oneshotprompts.com/techniques/?ref=aiposthub.com) 把動態方法整理成分類，能從視覺效果回到具體描述。 [站方介紹](https://oneshotprompts.com/about/?ref=aiposthub.com) 則交代範例與檢查方法，方便讀者知道展示是怎麼來的。

網站的價值在於讓模糊形容變成能討論的動作，例如旋轉、縮放、遮罩或文字出現的先後。遮罩就是控制畫面哪一部分顯示的方法。

看示範時也要讀作者列出的缺點。若結尾停太久，修改要求可以直接變成「結尾停留縮短，並保留最後標題的閱讀時間」，比只說不夠有質感更明確。

![OneShotPrompts 官方介紹圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/motion-reference-prompt-libraries.png)

OneShotPrompts 官方介紹圖。來源： 官方／作者發布頁 。

## Motion Library 可以怎麼搭配？

[Motion Library](https://kittenjam-motion-library.vercel.app/?ref=aiposthub.com) 提供可預覽效果與複製提示詞的入口。它適合從「我需要一個進場動作」這類具體需求開始找。

複製之後先看提示詞有沒有綁定範例的顏色或尺寸。若只想借動作，就補上「沿用專案字體與主題色」，避免把另一個網站的視覺一起搬過來。

資源數量會變，選工具時不必只比誰收錄更多。能否看懂、是否說清楚使用方式，以及示範能不能接上自己的環境，會更直接影響工作。

## 從效果名稱到成品，還要補哪些條件？

先把要交付的東西寫清楚。網頁動畫可以互動，MP4 是固定影格影片，做出前者後，仍可能需要另外渲染或錄製才能交付後者。

一個可操作的提示可以包含：

> 使用這個文字揭露效果，內容換成我的產品名稱，總長三秒。沿用網站字體與配色，第一次載入只播一次，降低動態效果設定下改為直接顯示。先交可預覽版本，再輸出需要的影片尺寸。

這段把內容、時間、觸發及替代顯示一起交代，因此看完成品後能逐項比較。

## 常見問題

### 這些提示詞保證一次成功嗎？

示範有自己的模型與環境條件。使用時仍應記錄版本、修改輪數與輸出結果。

### 能整套搬走素材庫嗎？

先看網站條款。參考個別技巧與整批重製技巧庫，是不同的使用方式。

### 網頁上會動，代表已經能交影片嗎？

要再確認輸出格式、影格率與聲音。先交十秒測片，就能提早發現工具鏈缺口。

## 選一個效果，寫出可比較的版本

固定同一段文字和尺寸，試兩種動態方法，再比較閱讀是否清楚、動作是否有用途。把好用的提示留下，逐漸建立自己的風格規則。

延伸閱讀： [Claude Code 剪影片教學：搭配 HyperFrames 自動加字幕、動畫與輸出 MP4](https://www.aiposthub.com/claude-code-hyperframes-video-editing-guide/)

延伸閱讀： [Motion Prompt 是什麼？144 個動效名稱讓 Claude Code 做動畫，用法與授權](https://www.aiposthub.com/motion-prompt-skill-library-names-claude-code-guide/)