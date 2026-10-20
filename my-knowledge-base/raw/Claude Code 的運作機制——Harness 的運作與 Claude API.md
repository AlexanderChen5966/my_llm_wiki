---
title: "Claude Code 的運作機制——Harness 的運作與 Claude API"
source: "https://codelove.tw/@tony/post/qOZGva"
author:
published:
created: 2026-08-03
description: "本篇文章整理了 Claude Code 的運作機制。過去我曾零散地進行調查，並寫成了幾篇文章。- [驗證了閱讀 Claude Code 文件時覺得在意的幾件事](https://qiita.com/megmogmog1965/items/72dd0aeb6911599bf813)- [調查..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月02日](https://codelove.tw/@tony/post/qOZGva "2026-08-02 17:03:53")

本篇文章整理了 Claude Code 的運作機制。

過去我曾零散地進行調查，並寫成了幾篇文章。

- [驗證了閱讀 Claude Code 文件時覺得在意的幾件事](https://qiita.com/megmogmog1965/items/72dd0aeb6911599bf813)
- [調查 Claude Code 傳給 LLM 的上下文內容](https://qiita.com/megmogmog1965/items/2fe6c20e9ea0f4ca89b6)
- [Claude Code 的工作階段、上下文大小與 token 消耗量之間的關係](https://qiita.com/megmogmog1965/items/575f7c5cbf16c0928401)

這次我把這些內容整合成一篇，盡量以系統化的方式來撰寫。文章會很長，如果你願意的話，請陪我一起看完。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785722407&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqOZGva&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721179880&amp;bpp=7&amp;bdt=954&amp;idt=7&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721187%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721187%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721187%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0&amp;nras=2&amp;correlator=3892052815794&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=1041&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C95397130&amp;oid=2&amp;pvsid=382384716269350&amp;tmod=1434000446&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**驗證環境** ：macOS（Darwin 24.3.0）／Claude Code v2.1.220／截至 2026 年 7 月 28 日。由於官方明確表示工作階段記錄檔的格式屬於「內部規格，且可能因版本而改變」 [1](#fn-1) ，請注意本文的實測值與結構都具有版本相依性。

---

## 1\. 全局架構 — 3 層結構

先提出一個貫穿全文的心智模型。Claude Code 是以以下 **3 層結構** 運作的。

各層的角色分工非常明確。

層級實體角色 **Claude API** Anthropic 的伺服器端模型只負責推論（思考、生成回應、判斷是否使用工具）。 **不保有任何狀態\*\*** Harness **在本機執行的 Claude Code 主體，負責工具執行、上下文組裝、權限管理與工作階段持久化。** 承擔所有狀態管理 **\*\*工作階段記錄** JSONL 檔案對話的持久化層。當 harness 組裝 API 請求時，會從這裡逐次讀出對話歷史。「harness」是官方用語。文件把 Claude Code 稱作 " **agentic harness** "（為 agent 提供的馬具／骨架） [2](#fn-2) 。

> Claude Code serves as the **agentic harness** around Claude: it provides the tools, context management, and execution environment that turn a language model into a capable coding agent.

### 大原則：Messages API 是無狀態的

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3764087313&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785722407&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqOZGva&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721179900&amp;bpp=5&amp;bdt=974&amp;idt=5&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721187%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721187%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721187%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C952x280&amp;nras=3&amp;correlator=3892052815794&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=1987&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C95397130&amp;oid=2&amp;pvsid=382384716269350&amp;tmod=1434000446&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

理解這個 3 層結構時，最重要的原則就是： **Claude API（Messages API）是無狀態的** [3](#fn-3) 。

- API 伺服器完全不保留對話歷史
- 每次呼叫 API 時，harness 都會把系統提示、過去所有訊息、以及這次輸入 **全部一起** 送出
- 所謂「上下文」，就是這個 **每次送出的整個 payload**

請求 payload 的格式如下 [3](#fn-3) 。重點在 `messages` 陣列；到了第 2 回合的請求中，第 1 回合的 user／assistant 訊息會整包包含進去。

```
POST /v1/messages
{
  "model": "claude-...",
  "system": [ /* 系統提示 */ ],
  "tools": [ /* 工具定義 */ ],
  "messages": [
    {"role": "user", "content": "..."},          // 第 1 回合的發言
    {"role": "assistant", "content": [ ... ]},   // 第 1 回合的回應
    {"role": "user", "content": "..."}           // 這次（第 2 回合）的發言
  ]
}
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785722407&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqOZGva&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721179930&amp;bpp=3&amp;bdt=1004&amp;idt=3&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721187%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721187%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721187%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C952x280%2C952x280&amp;nras=4&amp;correlator=3892052815794&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=2856&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C95397130&amp;oid=2&amp;pvsid=382384716269350&amp;tmod=1434000446&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這些對話歷史的來源，就是第 3 章會說明的工作階段記錄（JSONL）。至於重建時 harness 會額外加入哪些內容，則會在第 4 章說明。

### 無狀態的代價消除機制：提示快取

「每次都傳整包」會有計算成本。模型在產生回應前必須處理整個 payload，因此對話越長，每次的處理量就越大。解決這個問題的是伺服器端的 **提示快取** [4](#fn-10) 。

1. **在前一次請求中** ：模型處理整個 payload，並生成回應
2. **在前一次請求之後** ：伺服器不會把處理 payload 的結果丟掉，而是保存起來
3. **在這次請求中** ：如果 payload 開頭部分與前一次相同，伺服器就省略那一段的處理，直接沿用已保存的結果。實際只處理從前一次之後新增的尾端部分

不過，「相同」的判定是從開頭開始的 **完全一致** 。payload 中只要有 1 個位元組與前一次不同，從那裡之後的快取就全部失效。

是否使用快取不是由伺服器自行決定，而是由 harness 在 payload 中指示。要快取的範圍末端區塊會被加上 `cache_control` 標記。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=186534417&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785722407&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqOZGva&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721179970&amp;bpp=1&amp;bdt=1044&amp;idt=1&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721187%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721187%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721187%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C952x280%2C952x280%2C952x280&amp;nras=5&amp;correlator=3892052815794&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=3697&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C95397130&amp;oid=2&amp;pvsid=382384716269350&amp;tmod=1434000446&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

```
{
  "tools": [ /* 工具定義 */ ],
  "system": [
    {"type": "text", "text": "（系統提示）",
     "cache_control": {"type": "ephemeral", "ttl": "1h"}}
  ],
  "messages": [
    {"role": "user", "content": "..."},
    {"role": "assistant", "content": [ ... ]},
    {"role": "user", "content": [
      {"type": "text", "text": "（這次的發言）",
       "cache_control": {"type": "ephemeral", "ttl": "1h"}}
    ]}
  ]
}
```
- payload 會依照 `tools` → `system` → `messages` 的順序串接， **到標記區塊為止的範圍** 會被快取
- 每個請求最多可放 4 個標記。每次請求時，都會把標記重新加在延伸後的對話尾端（過去位置的快取也仍然有效）
- 快取保留時間（TTL）有 5 分鐘或 1 小時兩種，Claude Code 指定的是 1 小時（實測）。從快取讀出的部分，其成本約為一般輸入的 0.1 倍

隨著對話進行，訊息會不斷堆到 payload 尾端，「快取命中／新內容」的分界與標記位置會一路往下移動。

---

## 2\. Agentic Loop 與 thinking 的雙重結構

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1038984369&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785722409&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqOZGva&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721179987&amp;bpp=2&amp;bdt=1060&amp;idt=2&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721179%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721179%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721179%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=7&amp;correlator=3892052815794&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=4756&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=933&amp;eid=95396139%2C95397130&amp;oid=2&amp;psts=AOrYGsmycM7ZqudA3k4cKKg4Ecvlsj4RMZS2a7VeoW_db2WLMS5okkIT0YFn43fa04Kk4BWSASQe1_0SoLY8RmT5GK8x-00tXjtB2Um28_GBtMVpK77C5jRO7Pcldw%2CAOrYGsnlEv07ZrOHhcAl0F4OrcLUG18xkwkq3tIaKbpgVry0X0CfiDZnm7-OgHLmoRxs6T33ITjmgCovnj5IvBkpcGjIBqhGpIQWojYIPaliDNBYfiE%2CAOrYGsnTpqlEfZmiLeusJwd2hAXxWrl3Lq8MP2lCgwmuCPkDSvJtY6I4BcS3fFQIwiIinN1OZkpzMrMKLnKvJ24JOrqG4A7p-nZ3PNAX-TRzj_p-pQ%2CAOrYGsmBlRyXWdmifM7YCs2EUIvlPKE-h-J-3MlOekLOhGA00SUtU8jOz7046LCLB-Aj5jIs4vrn2-eMvVN3eXbI5hnzpcL2GGaNsFRqyib3auba2sc%2CAOrYGsmvXt7hfXWiTIMqU9Z6fEojZTL2CxkqIMNVXNAmXL6hxBno-sdFZZ16R8-Ck2dElL6PblrOpLNhkJ3HZkeMDpvNl9ZgVrPHxSP_U2Y24sInI0Y&amp;pvsid=382384716269350&amp;tmod=1434000446&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1959%2C30%2C1959%2C30%2C1920%2C30%2C1859%2C1050%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Claude Code 會在自動使用工具的同時推進任務。這種自律性由兩種性質不同的 **兩個層次** 構成：反覆執行行動的 **迴圈** （harness 端），以及單次回應內部的 **推論** （伺服器端）。迴圈只有外層這一個。

以下會依照外層（2.1）→ 內層（2.2）的順序詳述。

### 2.1 harness 側的迴圈（外層迴圈）

使用者每發言 1 次（= 1 回合），背後其實會跑多次——複雜任務甚至會跑數十次——API 呼叫。

重點有 3 個。

- **執行迴圈的是 harness**  
	模型只會透過 `tool_use` 區塊要求下一步行動，真正執行工具的是 harness
- **工具執行結果會以 `user` 角色訊息回傳**  
	harness 會建立包含 `tool_result` 區塊的 user 訊息，並追加到 messages 尾端
- **是否繼續迴圈由模型決定**  
	當回應中的 `stop_reason` （停止生成的原因）是 `tool_use` 時，harness 會重複執行工具並重新呼叫。當回傳 `end_turn` 時，回合結束

下圖中的①～③對應的實例，擷取自本文撰寫期間的工作階段記錄（內容略有刪減）。 `tool_use` 與 `tool_result` 以 `id` 對應。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1621299728&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785722412&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqOZGva&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721179999&amp;bpp=2&amp;bdt=1072&amp;idt=2&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721179%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721179%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721179%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=8&amp;correlator=3892052815794&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=5685&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=1905&amp;eid=95396139%2C95397130&amp;oid=2&amp;psts=AOrYGsmycM7ZqudA3k4cKKg4Ecvlsj4RMZS2a7VeoW_db2WLMS5okkIT0YFn43fa04Kk4BWSASQe1_0SoLY8RmT5GK8x-00tXjtB2Um28_GBtMVpK77C5jRO7Pcldw%2CAOrYGsnlEv07ZrOHhcAl0F4OrcLUG18xkwkq3tIaKbpgVry0X0CfiDZnm7-OgHLmoRxs6T33ITjmgCovnj5IvBkpcGjIBqhGpIQWojYIPaliDNBYfiE%2CAOrYGsnTpqlEfZmiLeusJwd2hAXxWrl3Lq8MP2lCgwmuCPkDSvJtY6I4BcS3fFQIwiIinN1OZkpzMrMKLnKvJ24JOrqG4A7p-nZ3PNAX-TRzj_p-pQ%2CAOrYGsmBlRyXWdmifM7YCs2EUIvlPKE-h-J-3MlOekLOhGA00SUtU8jOz7046LCLB-Aj5jIs4vrn2-eMvVN3eXbI5hnzpcL2GGaNsFRqyib3auba2sc%2CAOrYGsmvXt7hfXWiTIMqU9Z6fEojZTL2CxkqIMNVXNAmXL6hxBno-sdFZZ16R8-Ck2dElL6PblrOpLNhkJ3HZkeMDpvNl9ZgVrPHxSP_U2Y24sInI0Y%2CAOrYGslkynetAJNQO0c0FWgtxvFGqGvvFppNuRxvp7q1OZizgscH_KTpdOM1kIhR9VmWJzq2yyDidk9toG7w1PfAVEiIevXBJb-c7f4QLo8bLSrhMw&amp;pvsid=382384716269350&amp;tmod=1434000446&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1959%2C30%2C1959%2C30%2C1920%2C30%2C1859%2C1050%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**① 模型 → harness** ：API 回應。要求執行 Bash 工具，並以 `stop_reason: "tool_use"` 停止生成

```
{
  "role": "assistant",
  "content": [{
    "type": "tool_use",
    "id": "toolu_01Rfm7vm2AAe8YpTdN3t1cvU",
    "name": "Bash",
    "input": {"command": "ls -la /Users/yuusuke-kawatsu/Desktop/tmp/qiita_0728/", ...}
  }],
  "stop_reason": "tool_use"
}
```

**② harness → 模型** ：harness 在本機執行 `ls` ，並將結果以 user 訊息形式追加到 messages 尾端後再次呼叫

```
{
  "role": "user",
  "content": [{
    "type": "tool_result",
    "tool_use_id": "toolu_01Rfm7vm2AAe8YpTdN3t1cvU",
    "content": "total 0\ndrwxr-xr-x@  2 yuusuke-kawatsu ..."
  }]
}
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3970593978&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785722414&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqOZGva&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721180024&amp;bpp=2&amp;bdt=1097&amp;idt=2&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721179%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721179%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721179%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280&amp;nras=9&amp;correlator=3892052815794&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=6618&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=2926&amp;eid=95396139%2C95397130&amp;oid=2&amp;psts=AOrYGsmycM7ZqudA3k4cKKg4Ecvlsj4RMZS2a7VeoW_db2WLMS5okkIT0YFn43fa04Kk4BWSASQe1_0SoLY8RmT5GK8x-00tXjtB2Um28_GBtMVpK77C5jRO7Pcldw%2CAOrYGsnlEv07ZrOHhcAl0F4OrcLUG18xkwkq3tIaKbpgVry0X0CfiDZnm7-OgHLmoRxs6T33ITjmgCovnj5IvBkpcGjIBqhGpIQWojYIPaliDNBYfiE%2CAOrYGsnTpqlEfZmiLeusJwd2hAXxWrl3Lq8MP2lCgwmuCPkDSvJtY6I4BcS3fFQIwiIinN1OZkpzMrMKLnKvJ24JOrqG4A7p-nZ3PNAX-TRzj_p-pQ%2CAOrYGsmBlRyXWdmifM7YCs2EUIvlPKE-h-J-3MlOekLOhGA00SUtU8jOz7046LCLB-Aj5jIs4vrn2-eMvVN3eXbI5hnzpcL2GGaNsFRqyib3auba2sc%2CAOrYGsmvXt7hfXWiTIMqU9Z6fEojZTL2CxkqIMNVXNAmXL6hxBno-sdFZZ16R8-Ck2dElL6PblrOpLNhkJ3HZkeMDpvNl9ZgVrPHxSP_U2Y24sInI0Y%2CAOrYGslkynetAJNQO0c0FWgtxvFGqGvvFppNuRxvp7q1OZizgscH_KTpdOM1kIhR9VmWJzq2yyDidk9toG7w1PfAVEiIevXBJb-c7f4QLo8bLSrhMw%2CAOrYGsmuYEHcxiOyFUQg9mcRzaC0WZ4MCp1JmYwe9wlFNnmV5F_Q8AVlwNVsmvqz32AM3YeVEf5xjIqfBJhCKuAAGQmL6F6rMrmabWkK5vOKHwhmUQ&amp;pvsid=382384716269350&amp;tmod=1434000446&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1959%2C30%2C1959%2C30%2C1920%2C30%2C1859%2C1050%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**③ 模型 → harness** ：根據工具結果作出的回應。以文字完成，並以 `stop_reason: "end_turn"` 結束回合

```
{
  "role": "assistant",
  "content": [{
    "type": "text",
    "text": "（根據 ls 結果所作的回應文字）"
  }],
  "stop_reason": "end_turn"
}
```

官方將這個迴圈說明為「 **gather context** （蒐集資訊）→ **take action** （採取行動）→ **verify results** （驗證結果）」這 3 個階段的重複 [2](#fn-2) 。

### 2.2 伺服器端的 thinking（內層推論）

另一方面，在單次 API 呼叫的 **內部** ，模型同樣會進行推論，這就是 thinking。Claude Code 終端機中以灰色流出的思考文字，其實就是 API 回應中的 **`thinking` 區塊** 。

如圖所示，thinking 是 **一次連續文字生成的前半段** 。模型寫完 thinking token 後，會接著繼續產生輸出 token。伺服器內部並沒有像 2.1 那樣的迴圈。thinking 會讓回應變慢，是因為它必須在正式回答前先生成數百到數萬個推論 token，而這部分也會以 output token 計費。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3355444817&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785722415&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqOZGva&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721180034&amp;bpp=2&amp;bdt=1108&amp;idt=2&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721179%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721179%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721179%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=10&amp;correlator=3892052815794&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=7548&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=3720&amp;eid=95396139%2C95397130&amp;oid=2&amp;psts=AOrYGsmycM7ZqudA3k4cKKg4Ecvlsj4RMZS2a7VeoW_db2WLMS5okkIT0YFn43fa04Kk4BWSASQe1_0SoLY8RmT5GK8x-00tXjtB2Um28_GBtMVpK77C5jRO7Pcldw%2CAOrYGsnlEv07ZrOHhcAl0F4OrcLUG18xkwkq3tIaKbpgVry0X0CfiDZnm7-OgHLmoRxs6T33ITjmgCovnj5IvBkpcGjIBqhGpIQWojYIPaliDNBYfiE%2CAOrYGsnTpqlEfZmiLeusJwd2hAXxWrl3Lq8MP2lCgwmuCPkDSvJtY6I4BcS3fFQIwiIinN1OZkpzMrMKLnKvJ24JOrqG4A7p-nZ3PNAX-TRzj_p-pQ%2CAOrYGsmBlRyXWdmifM7YCs2EUIvlPKE-h-J-3MlOekLOhGA00SUtU8jOz7046LCLB-Aj5jIs4vrn2-eMvVN3eXbI5hnzpcL2GGaNsFRqyib3auba2sc%2CAOrYGsmvXt7hfXWiTIMqU9Z6fEojZTL2CxkqIMNVXNAmXL6hxBno-sdFZZ16R8-Ck2dElL6PblrOpLNhkJ3HZkeMDpvNl9ZgVrPHxSP_U2Y24sInI0Y%2CAOrYGslkynetAJNQO0c0FWgtxvFGqGvvFppNuRxvp7q1OZizgscH_KTpdOM1kIhR9VmWJzq2yyDidk9toG7w1PfAVEiIevXBJb-c7f4QLo8bLSrhMw%2CAOrYGsmuYEHcxiOyFUQg9mcRzaC0WZ4MCp1JmYwe9wlFNnmV5F_Q8AVlwNVsmvqz32AM3YeVEf5xjIqfBJhCKuAAGQmL6F6rMrmabWkK5vOKHwhmUQ%2CAOrYGsmyVo3GpXIJFlPvQLspJFyzTbEKABZzdtPI5gNS3sQXIbOTyAizZkWRmLemg7c0yG8atHaBNzrKF8RTUSuaEHo_S05Xw-hLG7R-RVGjNdUMoA&amp;pvsid=382384716269350&amp;tmod=1434000446&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1959%2C30%2C1959%2C30%2C1920%2C30%2C1859%2C1050%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**啟用方式** — thinking 是由 harness 啟用的，透過請求 payload 的 `thinking` 參數指定 [5](#fn-4) 。

**extended thinking** （Claude 4.5 世代以前）— 由 harness 明確指定思考 token 預算

```
{
  "model": "claude-sonnet-4-5",
  "thinking": {"type": "enabled", "budget_tokens": 10000},
  "messages": [ ... ]
}
```

**adaptive thinking** （Claude 4.6 世代以後）— 是否思考由模型自行判斷，深度則由 `effort` 控制

```
{
  "model": "claude-opus-4-6",
  "thinking": {"type": "adaptive"},
  "output_config": {"effort": "high"},
  "messages": [ ... ]
}
```

**在回應中的呈現方式** — `content` 開頭會插入 `thinking` 區塊。與未啟用 thinking 的差異，只在於是否有這個區塊（以下內容為示意）。

```
{
  "role": "assistant",
  "content": [
    {
      "type": "thinking",
      "thinking": "使用者在詢問建置錯誤的原因。錯誤訊息顯示型別不一致，而前面的 diff 顯示引數順序被對調了。原因就在這裡。先直接講結論。",
      "signature": "CAIS..."
    },
    {
      "type": "text",
      "text": "錯誤的原因是傳給 \`login()\` 的引數順序。..."
    }
  ]
}
```

總結來說， **thinking 不是迴圈，而是一次 API 回應內完成的推論** 。真正負責執行工具、取得結果、再決定下一步的迴圈，始終是 harness 端。

---

## 3\. 工作階段的機制（JSONL）

在第 1 章中我提到，「harness 會在每次呼叫 API 時，從工作階段記錄中讀出對話歷史並重建 payload」。本章將說明這個「JSONL ⇄ payload」的對應關係。

- 工作階段記錄中的項目分為 **對話類** 與 **中繼資料類** ，只有對話類會被重建到 payload 的 messages 中
- `system` 、 `tools` 以及插入 messages 的內容，都不是從記錄檔讀出，而是由 harness 每次重新生成（第 4 章）

理解這個對應關係後，就能統一說明第 5 章中 `/clear` 、 `/compact` 等指令到底做了什麼。

### 3.1 檔案與項目的結構

工作階段記錄以 **1 行 = 1 項目** 的 JSONL 檔案形式儲存在以下位置 [1](#fn-1) （預設 30 天後自動刪除）。

```
~/.claude/projects/<專案路徑的 slug>/<session ID>.jsonl
```

`<專案路徑的 slug>` 是把工作目錄路徑中的非英數字元替換為 `-` 後的結果（例如： `/Users/foo/myapp` → `-Users-foo-myapp` ）。

檔案會在尾端持續追加新行而變長。每一行（= 項目）都透過 `parentUuid` 欄位指向前一個項目，因此 **工作階段記錄的實體其實就是一條以追加方式延伸的鏈結串列（linked list）** 。

```
{"type": "user",       "uuid": "e1", "parentUuid": "...", "message": { ... }}
{"type": "attachment", "uuid": "e2", "parentUuid": "e1", ... }
{"type": "assistant",  "uuid": "e3", "parentUuid": "e2", "message": { ... }}
```

以下是展開單一項目的結構（取自實際內容，部分省略）。

```
{
  "type": "assistant",              // 項目種類（user / assistant / system 其他）
  "uuid": "0794013f-...",           // 這個項目的 ID
  "parentUuid": "05c0ddff-...",     // 前一個項目的 ID
  "sessionId": "a726f404-...",
  "timestamp": "2026-07-28T06:41:07.481Z",
  "message": {                        // 與 API 交換的訊息本體
    "id": "msg_011CdTzL...",          // API 回應的 ID
    "role": "assistant",
    "content": [                      // content 是「陣列」
      {                               //   元素是「區塊」物件
        "type": "text",               //   區塊種類（thinking / text / tool_use ...）
        "text": "..."
      }
    ],
    "usage": { ... }
  }
}
```

對話類項目會幾乎原樣保留與 API 交換過的訊息，存放在 `message` 欄位中。 `content` 是陣列，而其元素就是 **content 區塊** （物件）。第 2 章出現的 `tool_use` 、 `thinking` ，以及一般回應文字的 `text` ，都是這些區塊的 `type` 。

項目的 `type` 並不只有對話類。以 v2.1.220 的實際工作階段來分類，可觀測到的類型如下：

分類type內容 **對話類** （用於重建 API payload） `user` 使用者發言，或 tool\_result `assistant` thinking / text / tool\_use（1 區塊 1 項目） **中繼資料類** （供本機管理使用，不送往 API） `system` compact 邊界、回合耗時等事件記錄 `attachment` hook 執行結果、技能清單差異等 `file-history-snapshot` 檢查點（→ 5.3 `/rewind` ） `ai-title` 、 `last-prompt` 、 `mode` 、 `permission-mode` 等工作階段名稱、輸入歷史、模式狀態等### 3.2 第 2 章的「1 回合」如何堆積到記錄檔中

第 2 章的迴圈，會 **原封不動地留下痕跡在工作階段記錄中** 。以下示範某一回合（使用者發言 → 執行一次工具 → 回應）如何堆積到記錄檔中。項目的結構、排列與鏈結都與實測一致，內容與 uuid 只是說明用範例。

#type區塊內容（例）uuidparentUuid1user—「請調查建置失敗的原因」 `e1` --2attachment— `task_reminder` （中繼資料類） `e2``e1` 3assistant `thinking` 「先執行建置來確認錯誤。」 `e3``e2` 4assistant `text` 「我先執行建置來確認原因。」 `e4``e3` 5assistant `tool_use``Bash {"command": "npm run build"}``e5``e4` 6user `tool_result``Error TS2345: Argument of type 'string' ...``e6``e5` 7attachment `—` hook\_success `（中繼資料類）` e7 ``e6`8assistant`text`「原因是傳給 `login()` 的引數型別不一致。…」`e8`` e7 `` `attachment `` 是 harness 用來記錄管理資訊的中繼資料類項目（種類如 3.1 表格所示）。 `parentUuid` 的鏈結不只經過對話類項目，也會經過中繼資料類項目，形成一條完整的鏈。

需要讀出的規格有兩點。

**① 工具執行結果會以 `user` 項目記錄**

第 6 項雖然是 `user` 類型，但它不是使用者發言，而是 **harness 將工具執行結果（ `tool_result` ）以 user 角色回傳給 API** 的紀錄（即 2.1 所述的回傳機制）。

**② 一次 API 回應會依 content 區塊拆成多個項目**

第 3～5 項雖然分成 3 行，但其實是 **同一次 API 回應** 。JSONL 本身不表達 API 呼叫的階層結構，所有項目都平行排列。哪些項目來自同一次回應，則是透過 `message.id` 相同來識別。

項目區塊message.id3 `thinking``msg_abc123` 4 `text``msg_abc123` （= 同一次 API 回應）5 `tool_use``msg_abc123` （= 同一次 API 回應）---

## 4\. 由 harness 進行的思考控制（上下文注入）

這裡會深入說明第 3 章開頭圖中「harness 每次生成並注入」的部分。

harness 不只是把 JSONL 中的對話單純轉成 messages 陣列而已，還會 **到處注入用來控制模型思考的資訊** 。包含注入在內的上下文（= 整個 payload）內部結構如下圖所示。第 1 章看到的 `system` ／ `tools` ／ `messages` 三個區塊中，各自都配置了注入的元素 [6](#fn-5) 。

在這些內容中，隨著對話推進而單調增加的只有 `messages` 陣列（對話本體），這也是上下文膨脹的主因。你可以隨時用 `/context` 指令查看自己工作階段的組成，系統會顯示各類別的 token 消耗。

**系統提示與 [CLAUDE.md](http://claude.md/) 不會存進工作階段記錄檔。** 這些內容是每次呼叫 API 時由 harness 從磁碟重新讀取並生成的。這也是為什麼你隔天重新開啟工作階段時，最新的 [CLAUDE.md](http://claude.md/) 仍然會反映進來。

### 4.1 注入的主要管道

管道實體 **system 參數** payload 最上層的系統提示。包含角色設定、工具使用原則、回應格式等核心指示（請參考第 1 章的 payload 範例） **`<system-reminder>` 標籤** 插入 user 訊息中的萬用注入標籤。可用於通知模式狀態、技能清單、hook 產生的額外上下文等 **合成歷史** 「假裝模型曾呼叫工具」的虛構歷史 **工具結果包裝** 對 tool\_result 的格式化與暫存 **`<system-reminder>` 的實例** （實測）— 當模型試圖再次 Read 一個已經載入到對話中的檔案時，harness 會注入這段註解作為工具結果。harness 不會真的再讀檔，而是透過這段註解修正模型行為，告訴它直接使用已在上下文中的內容。

```
{
  "type": "tool_result",
  "tool_use_id": "toolu_01Cvcf...",
  "content": "<system-reminder>This file is already in your context (see \"Contents of /Users/.../CLAUDE.md\" above) and has not changed on disk. Use that content instead of re-reading.</system-reminder>"
}
```

**合成歷史的實例** （結構為實測，內容為範例）— 當使用者以 `@README.md` 提及檔案時，payload 會被合成出一組看起來像是模型呼叫了 Read 工具的 tool\_use／tool\_result 配對。

```
{"role": "assistant", "content": [
  {"type": "tool_use", "id": "toolu_x1", "name": "Read", "input": {"file_path": "README.md"}}
]},
{"role": "user", "content": [
  {"type": "tool_result", "tool_use_id": "toolu_x1", "content": "（README.md 的內容）"}
]}
```

**工具結果包裝的實例** （實測）— 當 WebFetch 的輸出過大時，tool\_result 的內容會改成指向暫存檔的參照與前段預覽。

```
<persisted-output>
Output too large (71.1KB). Full output saved to:
~/.claude/projects/<project>/<session ID>/tool-results/toolu_01NC....txt

Preview (first 2KB): ...
</persisted-output>
```

### 4.2 pull 型設計

harness 並不是把「模型可能需要的資訊」全部都注入（push）進去。像 TodoList 的狀態或 Plan 檔案本體，並不會整份放進 messages，而是採用 **在模型需要時，透過工具呼叫去取得** 的（pull）設計。

以 TodoList 為例（內容為示意）：

**如果是 push 型** — 每次請求的 payload 都會持續包含完整清單

```
<system-reminder>目前任務：1. 修正建置（進行中） 2. 新增測試 3. ...（全文）</system-reminder>
```

**pull 型（實際設計）** — 只注入一個小通知，內容則由模型自己去取

```
<system-reminder>任務清單已更新。可以使用 TaskList 工具查看。</system-reminder>
```

> 只有在模型判定有需要時，才會透過 TaskList 的 tool\_use 取得全文。

---

## 5\. 工作階段操作指令的內部動作

當第 3 章（JSONL）與第 4 章（payload 組裝）的知識都具備後，就可以從「 **對 JSONL 做了什麼** 」×「 **下一次 API payload 會變成什麼** 」這兩個軸向，統一解釋各種工作階段操作指令。

先整理成一覽表。

指令JSONL（本機記錄）下一次 API payload（上下文）工作階段 ID `/clear` 建立新檔案。舊檔保留原樣從空白對話重新開始 **新發行** `/compact` 在同一個檔案中 **追加** `compact_boundary` 與摘要項目過去的 messages 會被 1 個摘要取代不變 `/rewind` （還原對話）以過去項目為親代形成 **新分支** 回到選擇時點之前的歷史不變 `/btw` **不寫入任何東西** 目前上下文 + 問題（只呼叫一次、一次性使用）不變 `--continue` / `--resume` 繼續追加到既有檔案從已保存的歷史重建不變 `/branch` / `--fork-session` 把歷史 **複製到新檔案** ，之後改寫入那裡沿用到複製時點為止的歷史 **新發行** 以下將逐一說明各指令的內部動作。

### 5.1 /clear — 建立新的工作階段

`/clear` 並不是「刪除目前的工作階段記錄」的指令。實際上它是：

- 發行新的工作階段 ID，開始記錄到新的 JSONL 檔案
- 舊工作階段的檔案會完整保留，之後可隨時用 `/resume` 回復 [7](#fn-6)
- 從 payload 角度來看，messages 陣列會變成空的，因此上下文會回到初始狀態（系統提示 + [CLAUDE.md](http://claude.md/) 等）

### 5.2 /compact — 將歷史縮成摘要

`/compact` （以及上下文逼近上限時的自動 compaction）與 `/clear` 相反，它是在 **同一個工作階段、同一個檔案內** 只壓縮 messages。實際 JSONL 會追加以下 2 個項目。

**① `compact_boundary` （system 項目）** — 擷取自實際內容：

```
{
  "type": "system",
  "subtype": "compact_boundary",
  "parentUuid": null,
  "logicalParentUuid": "e58c52b2-...",
  "compactMetadata": {
    "trigger": "auto",
    "preTokens": 984519,
    "postTokens": 6384
  }
}
```

從這個項目可以讀出 3 個規格。

- `parentUuid: null` — 這裡會把 parentUuid 鏈 **實體切斷** 。重建 payload 時，不會再追溯到這之前的項目
- `logicalParentUuid` — 但「邏輯上仍是舊鏈延續」的連接資訊會以另一個欄位保留下來（供顯示與 `/rewind` 使用）
- `preTokens: 984519 → postTokens: 6384` — 在這個例子中，約 98 萬 token 的歷史被壓縮成約 6 千 token 的摘要

**② 摘要本體（user 項目， `isCompactSummary: true` ）** ：

```
{
  "type": "user",
  "isCompactSummary": true,
  "message": {
    "role": "user",
    "content": "This session is being continued from a previous conversation that ran out of context. The summary below covers the earlier portion of the conversation. Summary: 1. Primary Request and Intent: ..."
  }
}
```

鏈結結構會變成這樣。

也就是說，compact 後的 payload 會被重建為「 **1 個名為摘要的 user 訊息 + 其後的對話** 」。從模型的角度來看，就像是先讀一段很長的前言，再繼續對話。

要注意的是，compact 只會遺失 messages 內的資訊。哪些內容會被保留下來，官方有整理 [8](#fn-7) 。

要素compaction 後系統提示不受影響（因為本來就不是 messages）專案根目錄的 [CLAUDE.md](http://claude.md/) / Auto memory會從磁碟重新注入 `帶有 paths:` 的 rules / 子目錄 [CLAUDE.md](http://claude.md/) 會先遺失（直到再次讀取該檔案）已啟動技能的正文會重新注入（每個技能上限 5,000 token、總計 25,000 token）### 5.3 `/rewind` — 回到檢查點

`/rewind` （或在空白提示下連按 2 次 `Esc` ）是把對話與程式碼回到過去某個時間點的指令。支撐這個功能的是 **檢查點機制** ，而它其實由兩套彼此獨立的機制組合而成 [9](#fn-8) 。

**① 對話回復 = parentUuid 樹狀分支**

第 3.1 章看到的鏈結串列，精確來說其實是 **可分支的樹** 。當你把對話回溯到某一點並重新下指令時，就會以 **所選時點的項目作為 parent 的新分支** 長出來。

重建到 payload 的，只是從最新項目沿著 `parentUuid` 往回追蹤所能得到的 **單一路徑** 。舊分支（圖中的虛線側）不會被刪除，只是被排除在這次重建之外 [9](#fn-8) 。

**② 程式碼回復 = 檔案快照**

每次使用者送出提示時，harness 都會建立正在編輯檔案的快照。JSONL 中會記錄 `file-history-snapshot` 項目（並透過 `messageId` 對應到哪一則 user 訊息時點），檔案實體則會被暫存到另一個目錄。

```
{
  "type": "file-history-snapshot",
  "messageId": "71cf4a77-...",
  "snapshot": {
    "trackedFileBackups": {
      "storyline.md": { "backupFileName": "ecc7d8cb5d6d337a@v2", "version": 2 }
    }
  }
}
```
```
~/.claude/file-history/<session ID>/
├── 6334ef34043b337c@v1   ← 內容雜湊@版本
└── ecc7d8cb5d6d337a@v2
```

`/rewind` 選單中會出現「Restore code and conversation」「Restore conversation」「Restore code」這 3 個選項，就是因為這兩套機制是彼此獨立的。

限制條件也同樣由這兩套機制的結構所決定 [9](#fn-8) 。

- **Bash 指令造成的檔案修改無法回復** （因為快照只追蹤透過編輯工具所做的變更）
- **子代理的編輯原則上不在回復範圍內** （因為它們是在不同上下文中運作）
- 檢查點最多只保留最近 100 個。它不是 git 的替代品，而是定位為「工作階段內的 undo」

### 5.4 --continue / --resume — 從已儲存的記錄重新開始

工作階段會一直儲存在 JSONL 中，所以所謂「重新開始」就是 **讀取既有檔案、重建 payload，並繼續向同一個檔案追加** [1](#fn-1) 。工作階段 ID 不會改變。

- `claude --continue` ：重新開始目前目錄中最近的工作階段
- `claude --resume` ：開啟工作階段選擇器（也可指定名稱／ID）

另外，如果在兩個終端機同時重新開始同一個工作階段， **兩邊的訊息會混在同一個 JSONL 檔中記錄** （官方也明言如此 [1](#fn-1) ）。由於鏈結會交錯，導致其中一邊看不見另一邊的內容，因此若要平行作業，應該使用下一個 `/branch` 。

### 5.5 /branch / --fork-session — 以記錄複製分支

`/branch` （CLI 中則是 `--continue --fork-session` 等）是把 **對話歷史複製到新的工作階段 ID 檔案，之後改寫入那邊** 的操作 [1](#fn-1) 。

- 原本的工作階段會完整保留（可透過 `/resume` 回去）
- 從 payload 角度來看，會原封不動沿用到複製當下為止的歷史
- `/rewind` 的對話復原是「同一個檔案內的分支」，而 `/branch` 則是「整個檔案分支」

### 5.6 /btw — 不會留在任何地方的邊隨問題

最後是最特殊的 `/btw` 。這是為了「只想針對目前的工作稍微問一下，但又不想弄髒對話歷史」而設計的指令，官方文件的說明其實已經很能表現這個機制 [10](#fn-9) 。

> Side questions have full visibility into the current conversation, so you can ask about code Claude has already read, decisions it made earlier, or anything else from the session. The question and answer are ephemeral: they appear in a dismissible overlay and never enter the conversation history.

用本文的架構來整理如下：

- **payload** ：以目前的完整上下文 + 問題，只呼叫 API 一次。由於與主對話的前段內容相同，提示快取（第 1 章）仍然生效，額外成本很小
- **JSONL** ：問題與回答都 **完全不會記錄** 。是完全揮發性的
- **工具** ：不能使用（只能根據上下文中的知識回答）

官方把這稱為「 **subagent 的反面** 」。

> `/btw` is the inverse of a subagent: it sees your full conversation but has no tools, while a subagent has full tools but starts with an empty context.

`/btw` （side question）subagent上下文主對話的 **全部** 都可見從 **空白** 開始工具不能用可以完整使用對話歷史不會留下記錄（揮發性）只會把結果摘要帶回主對話用途詢問「關於目前這段對話」的問題請求「去新調查某件事」另外，若從 `/btw` 的回答按下 `f` 進行 fork，就會建立一個 **把這段問答真實納入歷史的新工作階段** 。這是把揮發性問答事後持久化的唯一途徑。

---

## 結語

感謝你一路讀完這篇長文。

1. [管理工作階段](https://code.claude.com/docs/en/sessions) — 「項目格式屬於 Claude Code 的內部規格，且會在版本間變更」
2. [Claude Code 的運作方式](https://code.claude.com/docs/en/how-claude-code-works)
3. [Messages API](https://platform.claude.com/docs/en/api/messages) — 「Messages API 可用於單次查詢或無狀態的多回合對話。」
4. [提示快取](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
5. [Thinking](https://platform.claude.com/docs/en/build-with-claude/thinking) （總論）以及 [Extended thinking](https://platform.claude.com/docs/en/build-with-claude/extended-thinking) （舊方式參考）
6. [探索上下文視窗](https://code.claude.com/docs/en/context-window) — 可互動確認何時載入哪些內容的官方頁面
7. [指令](https://code.claude.com/docs/en/commands) — `/clear` ：「以空上下文開始新的對話。... 之後可用 `/resume` 回復先前的對話」
8. [探索上下文視窗](https://code.claude.com/docs/en/context-window) — 「compaction 後會保留下來的內容」章節
9. [檢查點機制](https://code.claude.com/docs/en/checkpointing)
10. [互動模式](https://code.claude.com/docs/en/interactive-mode) — 「使用 /btw 的 side questions」章節

---

原文出處： [https://qiita.com/megmogmog1965/items/7db66f5a5aa306c68eb8](https://qiita.com/megmogmog1965/items/7db66f5a5aa306c68eb8)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qOZGva) [11瀏覽](https://codelove.tw/@tony/post/qOZGva)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qOZGva)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝10 ❤️4

295

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 💬4 ❤️1

58

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1785721179&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqOZGva&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721178994&amp;bpp=3&amp;bdt=85&amp;idt=158&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785402984%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785402984%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785402984%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3892052815794&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C95397130&amp;oid=2&amp;pvsid=382384716269350&amp;tmod=1434000446&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=243" title="Advertisement" aria-label="Advertisement"></iframe><iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260729/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=10&amp;uci=a!a&amp;btvi=5" title="Advertisement" aria-label="Advertisement"></iframe>