---
title: "JSON格式也有收納為TOON"
source: "https://vocus.cc/article/691a87c2fd897800018836d6"
author:
  - "[[Xamns]]"
published: 2025-11-17
created: 2026-05-25
description: "最近AI趨勢從一般的AI到現在的Agent, 那麼在做多代理（Multi-Agent）與大模型應用時，由於訊息傳遞會用到JSON但是也會相對的消耗大量的token。主要因為在大模型裡，每一句話、每個字、每個欄位名都是成本—— 不只是「花錢」，也是「能源、碳排、算力」的消耗。 所以最近剛好看到出了"
tags:
  - "clippings"
---
最近AI趨勢從一般的AI到現在的Agent, 那麼在做多代理（Multi-Agent）與大模型應用時，由於訊息傳遞會用到JSON但是也會相對的消耗大量的token。  
  
主要因為在大模型裡，每一句話、每個字、每個欄位名都是成本—— 不只是「花錢」，也是「能源、碳排、算力」的消耗。 所以最近剛好看到出了TOON這個新的資料格式，或許現階段會有新的發展，但也不知會發展如何，所以可以先了解了一下，研究了一下  
  
我看了一下JSON格式:

```javascript
{
  "reviews": [
    {
      "id": 201,
      "guest": "Linda Wu",
      "rating": 4,
      "comment": "Clean room, friendly staff",
      "verified": true
    },
    {
      "id": 202,
      "guest": "Michael Chen",
      "rating": 5,
      "comment": "Amazing breakfast!",
      "verified": true
    },
    {
      "id": 203,
      "guest": "Sara Kim",
      "rating": 3,
      "comment": "Good location but noisy",
      "verified": false
    }
  ]
}
```

為什麼會看到廣告

還有新的TOON格式
```javascript
reviews[3]{
  id, guest, rating, comment, verified:

  201, Linda Wu, 4, Clean room, friendly staff, true
  202, Michael Chen, 5, Amazing breakfast!, true
  203, Sara Kim, 3, Good location but noisy, false
}
```

## JSON 像「衣櫃塞滿衣服」

這是我覺得最好懂的比喻。

把 JSON 想成你家衣櫃：

- T-shirt、外套、襪子、內褲全部堆在一起
- 一打開就什麼都看得到
- 但很佔空間
- 要找一件衣服要翻半天
- 雖然資訊豐富，但其實很亂
- 關鍵是： ****衣服＋衣服的名稱全部混在一起****

## TOON 像「精準收納櫃」

好像經過收納的設計

> ****先做架構，再放物品。****

就像你把衣服分進抽屜：

有了分類後，每件衣服只要知道要去哪個抽屜就好。

![zoomable](https://images.vocus.cc/4d22cc1f-40c4-4ff8-9ab4-2046056d3a8c.png)

整個下來呢然可以看到TOON就是JSON的壓縮版，會在處理時透過JSON=>encode=>TOON, 然後再TOON=>decode=>JSON，也是會消耗一些算力，如果訊息的Token量很大時，雖然encode成TOON, 但在decode為JSON時其算力的消耗也是一種考量的問題了。

****PS:****  
如果這篇對你有幫助，我偶爾會把這類心得領悟，再把筆記整理成更完整的版本， 放在電子報「LexLogic 律博坊」給訂閱者。  
不承諾頻率，只有真的心得領悟才發。  
👉 [免費訂閱：](https://lexlogic.beehiiv.com/) [LexLogic 律博坊](https://lexlogic.beehiiv.com/)