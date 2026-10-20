---
title: "Codex 5.6 必須知道的技巧：清理上下文，大幅減少無效 Token 消耗 | 教學與資訊"
source: "https://tools.wingzero.tw/articles/4028"
author:
  - "[[赤紅翼零]]"
published: 2026-08-18
created: 2026-08-18
description: "Codex 長時間執行程式開發、除錯與重構任務時，過多 Context 可能造成 Token 浪費與判斷干擾。本文分享 AI Agent 上下文管理技巧與實用 Prompt，教你定期清理失效資訊、保留核心決策與進度，建立精簡有效的任務摘要，提升 Codex 長任務的執行效率與準確性。"
tags:
  - "clippings"
status: "processed"
---
你可能會以為，Agent 對話得越久、累積的資訊越多，能力就會越強。

但實際情況往往相反。

當一個開發任務持續很長時間，上下文會不斷堆積各種已經沒有價值的資訊，例如：

- 已經放棄的方案
- 失敗的嘗試與測試
- 大量除錯 Log
- 重複產生的內容
- 已經完成、不再需要參考的資訊
- 與目前開發方向無關的歷史紀錄
- 已經被新決策取代的舊內容

這些資訊不只會持續消耗 Token，更麻煩的是，它們可能干擾 Agent 的判斷，讓 Agent 繼續參考過時資訊，甚至沿著早已放棄的方向繼續執行。

### 正確做法：定期幫 Codex「整理上下文」

不要讓 Context 最後變成一個什麼都往裡面丟的垃圾桶。

當一個任務進行了一段時間後，可以主動要求 Codex 整理目前狀態，把真正重要的資訊留下來，其餘內容清除、忽略或標記為失效。

可以直接使用下面這段 Prompt：

> 請整理目前任務的上下文，清理無效資訊，刪除或標記已廢棄方案、已完成且無需繼續參考的內容、與目前目標無關的歷史紀錄，以及低價值的重複輸出。
> 
> 接著重新產生一份精簡的任務摘要，只保留：
> 
> 1. 目前目標
> 2. 核心限制與要求
> 3. 已確認的決策
> 4. 目前進度
> 5. 尚未解決的問題
> 6. 下一步執行計畫
> 
> 後續執行任務時，請以這份最新摘要作為主要依據，不再依賴已失效或被取代的歷史資訊。

### 為什麼值得這樣做？

清理上下文之後，可以降低舊資訊對 Agent 的干擾，避免它沿著錯誤或已經放棄的方向繼續執行；同時也能讓上下文持續聚焦在目前真正的任務目標上。

尤其是在長時間的程式開發、除錯、重構或大型專案中，這種做法可以減少大量低價值 Token 消耗，也能降低因歷史資訊越積越多，而導致 Agent 判斷品質下降的情況。

**很多時候，你缺的不是更多 Token，而是一個乾淨、精簡，而且仍然有效的 Context。**

<iframe frameborder="0" src="https://1f4011d3dfa0de330616bcf52e03c9ca.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>