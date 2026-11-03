---
title: "如何透過 Jev 將 PR 中的「為什麼」擷取到 CHANGELOG？🤔"
source: "https://codelove.tw/@tony/post/3V2b73"
author:
published:
created: 2026-10-20
description: "Hoi hoi! 👋我是 @nyaomaru，一位目前正在打造 AI 輔助 CHANGELOG 產生器，並且在實驗 Jev 的前端工程師。😸最近，我一直在思考一個問題。我們要怎麼從 Pull Request 中抽取出這段內容```md## WhyThe previous imp..."
tags:
  - "clippings"
status: "inbox"
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [10月12日](https://codelove.tw/@tony/post/3V2b73 "2026-10-12 08:01:48")

## 如何透過 Jev 將 PR 中的「為什麼」擷取到 CHANGELOG？🤔

Hoi hoi! 👋

我是 @nyaomaru，一位目前正在打造 AI 輔助 CHANGELOG 產生器，並且在實驗 Jev 的前端工程師。😸

最近，我一直在思考一個問題。

我們要怎麼從 Pull Request 中抽取出這段內容

```md
## Why

The previous implementation could leave stale release data after an update.
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792480808&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2b73&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792480808910&amp;bpp=1&amp;bdt=961&amp;idt=-M&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792480805%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250&amp;nras=2&amp;correlator=1718895052137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=962&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95404665%2C95405780%2C122881556&amp;oid=2&amp;psts=AOrYGsm31munFWDkyyeffMMLaTxPU-CCyEaPUg8pb2ihE4ZFIeXf7tuEIcDjIIAmQFPYRRh81wzpibx-iOZO0DNYVuyQgbhh17FcRg01cTmaOK6pIw&amp;pvsid=4852424726548517&amp;tmod=1079284778&amp;uas=3&amp;nvt=3&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=19" title="Advertisement" aria-label="Advertisement"></iframe>

並把它放進 CHANGELOG 裡？

選擇雲端服務

```md
### Fixed

- Fix cache invalidation

  Why: The previous implementation could leave stale release data after an update.
```

取得 **WHAT** 相對容易。  
取得 **WHY** 就困難得多。  
而且我不想讓 AI 只是憑空編出一個看起來不錯的理由。

讓我們看看 `changelog-bot` 目前是怎麼處理這件事的！👀

[https://github.com/nyaomaru/changelog-bot](https://github.com/nyaomaru/changelog-bot)

---

## 📝 WHAT 很容易，WHY 很難。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792480808&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2b73&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792480808910&amp;bpp=1&amp;bdt=961&amp;idt=-M&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792480805%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C952x280&amp;nras=3&amp;correlator=1718895052137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=1801&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95404665%2C95405780%2C122881556&amp;oid=2&amp;psts=AOrYGsm31munFWDkyyeffMMLaTxPU-CCyEaPUg8pb2ihE4ZFIeXf7tuEIcDjIIAmQFPYRRh81wzpibx-iOZO0DNYVuyQgbhh17FcRg01cTmaOK6pIw&amp;pvsid=4852424726548517&amp;tmod=1079284778&amp;uas=3&amp;nvt=3&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=19" title="Advertisement" aria-label="Advertisement"></iframe>

假設某個 PR 包含以下內容。

```md
## Description

Extract shared runtime configuration into ProviderBase.

Update OpenAI, Anthropic, and Gemini adapters to use the shared implementation.

This reduces duplicated setup logic and makes adding new providers easier.
```

前兩句主要是在描述 **WHAT** 和 **HOW** 。

但這一句就不一樣了

```
This reduces duplicated setup logic and makes adding new providers easier.
```

這句就更接近 **WHY** 。

我們當然可以直接把整個 PR 丟給 LLM，然後問：

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792480808&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2b73&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792480808910&amp;bpp=1&amp;bdt=962&amp;idt=-M&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792480805%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280&amp;nras=4&amp;correlator=1718895052137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=2639&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95404665%2C95405780%2C122881556&amp;oid=2&amp;psts=AOrYGsm31munFWDkyyeffMMLaTxPU-CCyEaPUg8pb2ihE4ZFIeXf7tuEIcDjIIAmQFPYRRh81wzpibx-iOZO0DNYVuyQgbhh17FcRg01cTmaOK6pIw&amp;pvsid=4852424726548517&amp;tmod=1079284778&amp;uas=3&amp;nvt=3&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=20" title="Advertisement" aria-label="Advertisement"></iframe>

```
Why was this change made?
```

但模型可能會總結、改寫，或推斷出一個原文根本沒有明說的理由。

對 CHANGELOG 來說，我想要的是更嚴格的做法。

選擇雲端服務

也許 WHY extraction 根本不是一個生成問題。

也許它其實是一個 **選擇問題** 。

---

## ✂️ 先找出可能的 WHY 候選句

在呼叫模型之前， `changelog-bot` 會先預處理 PR 說明內容。

它會尋找像這些訊號：

```ts
const STRONG_CANONICAL_SECTION_NAMES = new Set([
  "why",
  "reason",
  "because",
  "motivation",
  "context",
  "background",
  "problem",
  "rationale",
]);

const RATIONALE_MARKER_RE =
  /\b(why|because|so that|in order to|reason|rationale|motivation|to avoid|to prevent|context|problem)\b|,\s*so\b/i;
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792480809&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2b73&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792480808869&amp;bpp=1&amp;bdt=921&amp;idt=1&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792480805%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C728x90&amp;nras=6&amp;correlator=1718895052137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=3797&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=346&amp;eid=95404665%2C95405780%2C122881556&amp;oid=2&amp;psts=AOrYGsm31munFWDkyyeffMMLaTxPU-CCyEaPUg8pb2ihE4ZFIeXf7tuEIcDjIIAmQFPYRRh81wzpibx-iOZO0DNYVuyQgbhh17FcRg01cTmaOK6pIw%2CAOrYGsk5kWfPS-Yl6o05bRFBU6LoAEfzJXAXwvKoz8_FAcRPUqnMM1w31MbnYh2eYNxl3MdF4Vr2XwF1Zn7ny5YNagv2d-ufjkzjg4PjKwyZqwIX3dTOWUNot5QYyNk-Ce_XHp0%2CAOrYGsn2RduxwZLBf19ljgT-6uixRCZomtDx_B8meDoVVRWTlIDAzibbwbViRJtRbgwwrNjus5HeWd0EIogwFtR_SmeLM8motiiceplugg%2CAOrYGsm_IhddIywZcZYJ_CcR6QZlGOB3uz_xi20ROCvNSNq4ajfjtkTiQh8dE-9HEMkqXgZC30P7g-Oh9E4AmsJQbyROWtKHljMleEWl%2CAOrYGskEvtmbwzqLONaU-6QpBmtd7UQnyoQvQl2lGBekR5lAfcW7ZNk8rcNC1bk6Rxhuo2Qkime9H5LF4A3dQuTcRmm4bm8tlgpR9Or0&amp;pvsid=4852424726548517&amp;tmod=1079284778&amp;uas=3&amp;nvt=3&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=6&amp;fsb=1&amp;dtd=720" title="Advertisement" aria-label="Advertisement"></iframe>

它也會移除像核取方塊、佔位符、以及無關欄位這類模板雜訊。

接著它會建立一些小型候選片段

```ts
const candidates = buildWhyCandidateSnippets(
  extracted.sections,
  body,
  options.maxCharsPerPr,
  !extracted.hasTargetSection,
);
```

所以與其把整個大型 PR 內容送進去，我們可能只會拿到

```ts
[
  "Extract shared runtime configuration into ProviderBase.",
  "This reduces duplicated setup logic and makes adding new providers easier.",
];
```

這樣一來，問題就小很多了。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792481304&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2b73&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792480808872&amp;bpp=1&amp;bdt=924&amp;idt=0&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792480805%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C728x90%2C952x280&amp;nras=7&amp;correlator=1718895052137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=4740&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=1256&amp;eid=95404665%2C95405780%2C122881556&amp;oid=2&amp;psts=AOrYGsm31munFWDkyyeffMMLaTxPU-CCyEaPUg8pb2ihE4ZFIeXf7tuEIcDjIIAmQFPYRRh81wzpibx-iOZO0DNYVuyQgbhh17FcRg01cTmaOK6pIw%2CAOrYGsk5kWfPS-Yl6o05bRFBU6LoAEfzJXAXwvKoz8_FAcRPUqnMM1w31MbnYh2eYNxl3MdF4Vr2XwF1Zn7ny5YNagv2d-ufjkzjg4PjKwyZqwIX3dTOWUNot5QYyNk-Ce_XHp0%2CAOrYGsn2RduxwZLBf19ljgT-6uixRCZomtDx_B8meDoVVRWTlIDAzibbwbViRJtRbgwwrNjus5HeWd0EIogwFtR_SmeLM8motiiceplugg%2CAOrYGsm_IhddIywZcZYJ_CcR6QZlGOB3uz_xi20ROCvNSNq4ajfjtkTiQh8dE-9HEMkqXgZC30P7g-Oh9E4AmsJQbyROWtKHljMleEWl%2CAOrYGskEvtmbwzqLONaU-6QpBmtd7UQnyoQvQl2lGBekR5lAfcW7ZNk8rcNC1bk6Rxhuo2Qkime9H5LF4A3dQuTcRmm4bm8tlgpR9Or0%2CAOrYGsmDhGNOjDsuVyXpD2CD1t-2II9bbRLhP8LtnZVcJXRfpVftqXifJctLOnNxpyjJs_Jas6Wxf-nsqjvwkgVvg2AokKcw_q9wjAT7i91tg-g&amp;pvsid=4852424726548517&amp;tmod=1079284778&amp;uas=1&amp;nvt=3&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

哪一個候選句才是真正的 WHY？

---

## 🧠 用 Jev 問兩個問題

這就是我開始實驗 [`Jev`](https://typesafe.ai/) 的地方。

我不會請 `Jev` 直接寫出一個 WHY 句子。

相反地， **每個候選句都會用兩個問題來評估** 。

第一個是

```ts
question: "Does the target candidate explicitly state why this changelog change was made?";
```

基本上就是在問：

> 這真的是一個理由嗎？

但這還不夠。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=4060917935&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792481308&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2b73&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792480808874&amp;bpp=1&amp;bdt=926&amp;idt=0&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792481304%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792481304%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792481304%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C728x90%2C952x280%2C952x280&amp;nras=8&amp;correlator=1718895052137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=5615&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=2134&amp;eid=95404665%2C95405780%2C122881556&amp;oid=2&amp;psts=AOrYGsm31munFWDkyyeffMMLaTxPU-CCyEaPUg8pb2ihE4ZFIeXf7tuEIcDjIIAmQFPYRRh81wzpibx-iOZO0DNYVuyQgbhh17FcRg01cTmaOK6pIw%2CAOrYGsk5kWfPS-Yl6o05bRFBU6LoAEfzJXAXwvKoz8_FAcRPUqnMM1w31MbnYh2eYNxl3MdF4Vr2XwF1Zn7ny5YNagv2d-ufjkzjg4PjKwyZqwIX3dTOWUNot5QYyNk-Ce_XHp0%2CAOrYGsn2RduxwZLBf19ljgT-6uixRCZomtDx_B8meDoVVRWTlIDAzibbwbViRJtRbgwwrNjus5HeWd0EIogwFtR_SmeLM8motiiceplugg%2CAOrYGsm_IhddIywZcZYJ_CcR6QZlGOB3uz_xi20ROCvNSNq4ajfjtkTiQh8dE-9HEMkqXgZC30P7g-Oh9E4AmsJQbyROWtKHljMleEWl%2CAOrYGskEvtmbwzqLONaU-6QpBmtd7UQnyoQvQl2lGBekR5lAfcW7ZNk8rcNC1bk6Rxhuo2Qkime9H5LF4A3dQuTcRmm4bm8tlgpR9Or0%2CAOrYGsmDhGNOjDsuVyXpD2CD1t-2II9bbRLhP8LtnZVcJXRfpVftqXifJctLOnNxpyjJs_Jas6Wxf-nsqjvwkgVvg2AokKcw_q9wjAT7i91tg-g%2CAOrYGskgr_A76uyMAPHsMdeBjKXGiJtp0XRM0PvitSIMnNHSOSsjBy37tb4JcGKamqDKczYw52pIazEmL9sFjPhKMeBWIxRanS4LRAyEYjWN8Aw&amp;pvsid=4852424726548517&amp;tmod=1079284778&amp;uas=1&amp;nvt=3&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

想像一下

```
We changed the cache invalidation logic.

The README was updated because the previous explanation was confusing.
```

第二句確實包含一個理由。

但它解釋的是 README 的修改，不是 cache invalidation 的修改。

所以我們還會再問

```ts
question: "Does the target candidate state a reason that applies to the identified changelog change?";
```

現在每個候選句就有兩個訊號

```ts
{
  probability: explicitWhyProbability,
  relevanceProbability: changeRelevanceProbability,
}
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1106437894&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792481310&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2b73&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792480808876&amp;bpp=1&amp;bdt=928&amp;idt=0&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792481304%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792481304%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792481304%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C728x90%2C952x280%2C952x280%2C952x280&amp;nras=9&amp;correlator=1718895052137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=6501&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=3012&amp;eid=95404665%2C95405780%2C122881556&amp;oid=2&amp;psts=AOrYGsm31munFWDkyyeffMMLaTxPU-CCyEaPUg8pb2ihE4ZFIeXf7tuEIcDjIIAmQFPYRRh81wzpibx-iOZO0DNYVuyQgbhh17FcRg01cTmaOK6pIw%2CAOrYGsk5kWfPS-Yl6o05bRFBU6LoAEfzJXAXwvKoz8_FAcRPUqnMM1w31MbnYh2eYNxl3MdF4Vr2XwF1Zn7ny5YNagv2d-ufjkzjg4PjKwyZqwIX3dTOWUNot5QYyNk-Ce_XHp0%2CAOrYGsn2RduxwZLBf19ljgT-6uixRCZomtDx_B8meDoVVRWTlIDAzibbwbViRJtRbgwwrNjus5HeWd0EIogwFtR_SmeLM8motiiceplugg%2CAOrYGsm_IhddIywZcZYJ_CcR6QZlGOB3uz_xi20ROCvNSNq4ajfjtkTiQh8dE-9HEMkqXgZC30P7g-Oh9E4AmsJQbyROWtKHljMleEWl%2CAOrYGskEvtmbwzqLONaU-6QpBmtd7UQnyoQvQl2lGBekR5lAfcW7ZNk8rcNC1bk6Rxhuo2Qkime9H5LF4A3dQuTcRmm4bm8tlgpR9Or0%2CAOrYGsmDhGNOjDsuVyXpD2CD1t-2II9bbRLhP8LtnZVcJXRfpVftqXifJctLOnNxpyjJs_Jas6Wxf-nsqjvwkgVvg2AokKcw_q9wjAT7i91tg-g%2CAOrYGskgr_A76uyMAPHsMdeBjKXGiJtp0XRM0PvitSIMnNHSOSsjBy37tb4JcGKamqDKczYw52pIazEmL9sFjPhKMeBWIxRanS4LRAyEYjWN8Aw%2CAOrYGskBJRNyu-Jwcq43KZEofyf3HpmFHxwzFOCJV-0ePfNZl23qu5WnWVGAb0pzA9mi4PH4CF_OJXls5nQKWopLtRH_yinEoI8017TojUXAxmxCCA&amp;pvsid=4852424726548517&amp;tmod=1079284778&amp;uas=1&amp;nvt=3&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

所以真正的問題就變成：

```
這是一個理由嗎？

而且

這是這個變更的理由嗎？
```

---

## 🎯 選出候選句

`changelog-bot` 會比較各個候選句的機率

選擇雲端服務

```ts
const bestCandidate = candidateProbabilities.reduce((best, candidate) =>
  combinedWhyCandidateProbability(candidate) >
  combinedWhyCandidateProbability(best)
    ? candidate
    : best,
);
```

接著兩個最低門檻都必須通過

```ts
const accepted =
  selectedCandidate !== undefined &&
  bestCandidate.probability >= JEV_MIN_EXPLICIT_WHY_PROBABILITY &&
  bestCandidate.relevanceProbability >= JEV_MIN_CHANGE_RELEVANCE_PROBABILITY;
```

如果候選句沒通過

```ts
if (!accepted) continue;
```

就不會加入 WHY。

而我認為這很重要。

有時候正確答案是

```
There is no reliable WHY in this PR.
```

這比憑空編造一個理由要好得多。

---

## 🔒 保留原始 PR 文字

這大概是我最喜歡的部分。

當某個候選句被接受時

```ts
items.push({
  prNumber: item.prNumber,
  why: selectedCandidate,
  confidence: mappedConfidence,
});
```

注意這裡

```ts
why: selectedCandidate;
```

最終的 WHY 是原始 PR 文字。

不是新生成的文句。

所以整個流程就變成

```
PR description
      ↓
candidate extraction
      ↓
Jev evaluates candidates
      ↓
select candidate
      ↓
original source text
      ↓
CHANGELOG
```

`Jev` 負責判斷。

而 PR 作者仍然提供那句話。😸

---

## 🧱 WHY 在 v1 流程裡的位置在哪裡？

在處理 WHY extraction 的同時，我也在重新設計 `changelog-bot` 的 v1 流程。

選擇雲端服務

核心理念是 **先確定性，再 AI** 。

即使 AI 被停用或失敗，完整的 CHANGELOG 也應該能夠產生。

所以 AI 應該是增強結果，而不是主導結果。

目標中的 v1 流程大致如下

```
source changes
      ↓
ReleaseChange[]
      ↓
deterministic classification
      ↓
complete ReleaseDraft
      ↓
optional editorial enrichment
      +
optional WHY enrichment
      ↓
ReleaseResult
      ↓
deterministic renderer
      ↓
CHANGELOG
```

因此，WHY extraction 是一個獨立、可選的增強階段。

概念上可以看成

```
ReleaseDraft / ReleaseChange[]
      ↓
find eligible WHY targets
      ↓
collect PR rationale candidates
      ↓
LLM or Jev
      ↓
trust / confidence validation
      ↓
attach structured WHY notes
      ↓
ReleaseResult
```

這裡有一個很重要的改變：WHY 引擎 **不需要** 控制最終的 Markdown。

學習程式開發

目前，部分流程仍然是先從產生出的 Markdown 中找目標，再把 WHY 文字補上去。

對 v1 來說，我想把這個邊界拿掉。

改成

```ts
{
  prNumber,
  why: selectedCandidate,
  confidence,
}
```

先成為結構化的 release 資料。

然後再由一個確定性的 renderer 決定它最後長什麼樣子

```md
- Fix cache invalidation
  - Why: The previous implementation could leave stale release data after an update.
```

這樣責任就更清楚了

```
WHY 引擎
→ 找出證據

Renderer
→ 格式化證據
```

這也是我在 v1 遷移中一直遵守的一個更大的原則的一部分：

> 確定性程式碼負責 CHANGELOG 結構。AI 只負責增強內容。

這個遷移的第 1 到第 4 階段已經實作完成，而結構化渲染與增強邊界則屬於下一階段。

查詢字典

你可以在這裡看到完整的 v1 設計 👇

[https://github.com/nyaomaru/changelog-bot/blob/main/v1-design.md](https://github.com/nyaomaru/changelog-bot/blob/main/v1-design.md)

---

## 🧪 但門檻值該設多少？

當然，一旦我們有了機率，另一個問題就出現了。

```
0.50?
0.60?
0.80?
```

我不想只因為「感覺合理」就隨便選一個。

所以我建立了一個評估架構，先用 14 個案例組成的小型 corpus。

例如

```ts
{
  candidates: [
    "Separate changelog execution into input-resolution and output-finalization phases.",
    "This simplifies the main orchestration flow and runs independent GitHub lookups concurrently.",
  ],
  expectedSelectedCandidateIndex: 1,
}
```

我們也需要負面案例

```ts
{
  candidates: [
    "Extract shared provider runtime configuration into ProviderBase.",
    "Refactor OpenAI, Anthropic, and Gemini adapters to inherit it.",
  ],
  expectedSelectedCandidateIndex: null,
}
```

這裡有很多實作層面的資訊。

查詢字典

但沒有明確、強而有力的 WHY。

所以 `null` 才是正確結果。

---

## 🤝 後來 John 加入了這個專案

當我在做這個實驗時，@johnnylemonny 作為貢獻者加入了這個專案。🎉

[https://github.com/johnnylemonny](https://github.com/johnnylemonny)

John 提議把這個小型 benchmark 擴充到大約 50 個案例，涵蓋

```
Explicit Rationale
Implementation-Only
Template & Noise
Multilingual PRs
```

我很喜歡這個想法，但我希望保留一個明確的規則：

> Ground truth 應該要獨立於當前的 Jev 門檻值。

所以 benchmark 會先定義某個候選句到底是不是真正有效。

然後我們再去評估不同的門檻值。

John 在 PR [#212](https://github.com/nyaomaru/changelog-bot/pull/212) 中實作了擴充後的語料庫。🙌

benchmark 從 14 個案例增加到 50 個，包含了很多棘手的負面案例。

他也加入了更細的門檻掃描

```ts
[0.4, 0.45, 0.5, 0.55, 0.6, 0.65, 0.7, 0.75, 0.8, 0.85, 0.9];
```

現在我們可以比較 precision、recall 和 F1，而不是只說：

> 0.8 感覺很安全！👍

那就實際測量吧。😸

---

## 🧩 目前的流程

今天，WHY extraction 大致長這樣

```
Pull Request
     ↓
Normalize PR body
     ↓
Extract WHY candidates
     ↓
Remove noise
     ↓
Local trust check
     ↓
Jev:
  explicit rationale?
  relevant to this change?
     ↓
Apply threshold
     ↓
Candidate or none
     ↓
Original PR text
     ↓
CHANGELOG
```

為了一行小小的 `Why:`，步驟還真不少。😂

但也正是這點讓這個問題變得有趣。

---

## 🎯 我的學習心得

當我開始做這個功能時，我以為核心問題是

```
How can AI write a good reason?
```

現在我認為，更有趣的問題是

```
How can we find evidence that the reason already exists?
```

所以我目前的方法很簡單

```
Use deterministic code to find evidence.

Use Jev to evaluate the evidence.

Keep the original evidence as the output.
```

而且多虧了 John 的貢獻，我們現在有更好的方法來測試這種做法到底有沒有效。

當有人看了你的實驗，然後說：

> 要不要我們把這個正式測試看看？

接著還真的跟你一起把它做出來，開源才會好玩。🙌

`changelog-bot` 仍在持續演進，而 Jev 支援也還是實驗性功能。

選擇雲端服務

我很好奇這種做法接下來會走到哪裡！

[https://github.com/nyaomaru/changelog-bot](https://github.com/nyaomaru/changelog-bot)

感謝閱讀！

我們下一篇文章見！👋😸

---

原文出處： [https://dev.to/nyaomaru/how-do-we-extract-the-why-from-a-pr-into-a-changelog-with-jev-52pk](https://dev.to/nyaomaru/how-do-we-extract-the-why-from-a-pr-into-a-changelog-with-jev-52pk)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3V2b73) [180瀏覽](https://codelove.tw/@tony/post/3V2b73)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3V2b73)

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

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [REA：讓你的 AI 代理反向工程你希望擁有的應用程式功能](https://codelove.tw/@tony/post/qenz7x)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Jev是什麼？理解它與傳統型 LLM 與 AI 代理人的差異](https://codelove.tw/@tony/post/3DRvQq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝11 💬1

342

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商事務所：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1792480808&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2b73&amp;pra=5&amp;aiof=14&amp;asro=0&amp;itsi=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODAiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODAiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44MCJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792480808017&amp;bpp=4&amp;bdt=69&amp;idt=38&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792480805%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792480805%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=1718895052137&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95404665%2C95405780%2C122881556&amp;oid=2&amp;pvsid=4852424726548517&amp;tmod=1079284778&amp;uas=0&amp;nvt=3&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=48" title="Advertisement" aria-label="Advertisement"></iframe>