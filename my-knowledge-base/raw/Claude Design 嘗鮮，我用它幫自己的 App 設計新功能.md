---
title: "Claude Design 嘗鮮，我用它幫自己的 App 設計新功能"
source: "https://medium.com/ddsakura-blog/claude-design-%E5%98%97%E9%AE%AE-%E6%88%91%E7%94%A8%E5%AE%83%E5%B9%AB%E8%87%AA%E5%B7%B1%E7%9A%84-app-%E8%A8%AD%E8%A8%88%E6%96%B0%E5%8A%9F%E8%83%BD-8b1791e8c325"
author:
  - "[[ddsakura]]"
published: 2026-04-19
created: 2026-06-09
description: "身為一個獨立開發者，最痛苦的事情之一就是：你有想法，但你不是設計師。你大概知道要做什麼功能，也大概知道它應該長怎樣，但要真的「畫」出來，就會開始卡住。我自己做了一個餐廳食記 iOS app Fudo Note，每次想加新功能，腦袋裡有畫面，但要怎麼「畫」出來？只能靠想像力，然後做"
tags:
  - "clippings"
status: processed
---
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*lbaxOU2dRr05bJQ803zB3A.jpeg)

身為一個獨立開發者，  
最痛苦的事情之一就是：你有想法，但你不是設計師。  
你大概知道要做什麼功能，也大概知道它應該長怎樣，  
但要真的「畫」出來，就會開始卡住。  
我自己做了一個餐廳食記 iOS app Fudo Note，  
每次想加新功能，腦袋裡有畫面，但要怎麼「畫」出來？  
只能靠想像力，然後做出來覺得哪裡怪怪的。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*dgUaNu-bNXi96jC7I5jV9w.png)

最近 Anthropic 發布了 Claude Design，  
號稱可以把文字描述變成設計稿，直接挑戰 Figma 。  
身為 Claude 的使用者，我第一時間就來試試了。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*DImWKL6QrGOQ2rPMeVu7vQ.png)

Claude Design 是什麼？  
簡單說，  
就是一個在 claude.ai 網頁版裡的設計工具。  
你描述你要什麼，它幫你生成設計稿等。

目前這個服務還是 Research Preview 階段，Pro、Max、Team、Enterprise 方案都可以用，  
直接去 [claude.ai/design](https://claude.ai/design) 就能開始。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*xgbjITXZNaHqXfLXgQ6GqA.png)

要注意的是：它的使用量是獨立計算，而且看起來消耗蠻快的。

目前我的工作流是：先討論，再設計，再實作

Step 1：在 Claude 對話裡先把需求想清楚  
因為 Claude Design 的使用量消耗不少，所以我蠻怕還沒試到好的結果，就先用完了，尤其是在需求還很模糊的時候，例如「我要一個新的瀏覽頁」這種。

所以我的做法是：先在一般的 Claude 對話裡把需求討論清楚。

以我的 Fudo Note 為例，我拿了幾張現有畫面的截圖，跟 Claude 討論：  
\* 現在的 Browse 跟 Favorite 頁 layout 幾乎一樣，有點重複  
\* Favorite 頁可以怎麼改才有自己的個性？  
\* 底部 tab 的名稱要改成什麼？

## Get ddsakura’s stories in your inbox

Join Medium for free to get updates from this writer.

討論過程中想出來的方向：把 Favorite 頁改成「足跡 Journal」 — — 一個有數字成就感、排行榜、最愛分類的個人飲食回顧頁面。tab 名稱從「最愛」改成「足跡」，中文直覺、意象也好。

這個討論過程是在一般對話裡完成的，所以省了不少 Claude Design 的 token。

Step 2：整理出具體的 prompt，帶截圖進 Claude Design

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*digalKXWW0RZQMlryx9t0A.png)

需求想清楚之後，我讓 Claude 幫我整理出一個可以直接丟進 Claude Design 的 prompt，包含：

\* 頁面結構（由上到下每個區塊）  
\* 視覺風格參考（Spotify Wrapped 的成就感 + Apple Health 的數據視覺化）  
\* 具體的內容元素（統計數字、排行榜、橫向 scroll 卡片）

同時上傳一張最相關的截圖當風格參考。

Step 3：在 Claude Design 裡微調

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*VVAJhU9TakJUX11_cnIurA.png)

因為產出的設計頂部的漸層風格跟既有的 Browse 頁不一致。我直接在對話裡說「參考之前上傳的 Browse 截圖，把頂部改成純紅色實心風格」，不需要重新上傳截圖，它就修好了。

Step 4：截圖給 Claude Code，直接產出 SwiftUI code  
設計稿確認之後，不需要經過 Figma，直接把 mockup 截圖貼給 Claude Code，說「根據這張設計圖實作」，它就能產出 SwiftUI code。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*U8N4jsBjbubRJqMd7TIsnw.png)

不過 Claude Design 中有提供 Handoff to Claude Code，  
之後應該也會再試試看這條流程。

Claude Design 現在還是 Research Preview，  
有很多地方可以加強，  
但對開發者來說還是蠻有幫助的。  
尤其是一個人開發的情境下，  
有一個可以「討論設計」的工具，  
其實差很多。

目前 Claude Design 僅支援 claude.ai 網頁版，  
App 尚未整合（搞不好隔幾天就推出？）。  
Pro 方案可用，但注意用量。