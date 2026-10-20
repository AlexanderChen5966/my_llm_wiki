---
title: "思考最近熱門的「AI時代的開發者五種原型」"
source: "https://codelove.tw/@tony/post/3KK7R3"
author:
published:
created: 2026-08-24
description: "[](#ai時代の5つの開発者アーキタイプとは)AI時代的 5 種開發者原型是什麼？===========================================================================================================..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月23日](https://codelove.tw/@tony/post/3KK7R3 "2026-08-23 08:15:10")

## 思考最近熱門的「AI時代的開發者五種原型」

## AI時代的 5 種開發者原型是什麼？

最近，我聽到這樣一個說法：

> 工程師的分類，未來可能不再是後端、前端這種區分，而是可以用包含原型開發者在內的 5 種原型來理解。

我查了一下，原來是由負責 Claude Code 開發的 Boris Cherny 提出的觀點，內容是關於工程師的 5 種原型。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787537219&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK7R3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536295852&amp;bpp=9&amp;bdt=1588&amp;idt=9&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905&amp;nras=3&amp;correlator=4370694625889&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=931&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95399754%2C42533294&amp;oid=2&amp;pvsid=5793937672150027&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

*原型* 職責 **1\. 原型開發者** 大量嘗試新的想法。大多不會正式發布 **2\. 建造者** 把想法與原型推進到可上線品質 **3\. 整理者** 整理 UI、程式碼與系統，進行刪除、簡化、加速 **4\. 成長推進者** 反覆改善已做出的產品，提高 PMF（Product-Market Fit：產品與市場契合度） **5\. 維運者** 維持成熟系統的安全性、可靠性、效能與效率本文整理了這個分類的概要，以及對它的批判性討論。

### 思考背景

Anthropic 分析了約 40 萬筆 Claude Code 工作階段後，觀察到如下分工：

- 人類：約 70% 負責「要做什麼」的規劃與方針判斷
- Claude：約 80% 負責「怎麼實作」的實作判斷

另外，同一份調查也顯示，對該任務具備越高專業性的人，工作階段成功率也越高（這裡的專業性並不是單純的職務或一般寫程式能力，而是 **是否理解這個任務應該達成什麼** 這種任務特定的專業性）。

也就是說，在 AI 時代，具備該領域知識，能定義「要做什麼」與「做到什麼程度才算完成」，並能驗證 AI 成果的能力，似乎正變得更加重要。

與其問「你能做什麼？」，不如說， **發現問題、決定方向、判斷品質、培養產品、承擔責任** 這些角色差異，未來會變得更重要。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3764087313&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787537219&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK7R3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536295886&amp;bpp=2&amp;bdt=1622&amp;idt=2&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280&amp;nras=4&amp;correlator=4370694625889&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1824&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95399754%2C42533294&amp;oid=2&amp;pvsid=5793937672150027&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

### 原始貼文

Cherny 把這 5 種原型當成「未來可能會這樣發展」的假說來提出。

以下是 Anthropic 的 Boris Cherny 在 X 上發表的內容。

## 本文的調查方法與 AI 使用方式

這篇文章是筆者把「5 種原型到底是什麼？」這個疑問交給 GPT-5.6 調查後，得到一份比想像中更清楚的資料，再整理成適合文章使用的內容。

雖然也許任何人都可以用 AI 做出這樣的資料，但我認為作為讓人接觸這種思考方式的契機，仍然值得留下來，因此才刊出。

## 各原型的評價與批判

我整理了各個原型的概要，以及對它們的批判性意見。

特別是批判性意見中，有不少一針見血地點出 5 分類的本質，很值得參考。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787537219&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK7R3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536295914&amp;bpp=1&amp;bdt=1650&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280&amp;nras=5&amp;correlator=4370694625889&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2681&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95399754%2C42533294&amp;oid=2&amp;pvsid=5793937672150027&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## ① 原型開發者

### 概要

這是最容易受惠於 AI 的角色之一，因為它能快速做出大量原型。

像 Claude Code 這類 AI Agent，大幅降低了每次試驗的成本，因此可以大量跑這種循環：

`點子 → 原型 → 回饋 → 丟棄`

Anthropic 內部也有報告指出，會讓多個 Claude 並行運作，同時探索不同方法。

### 批判

矛盾的是， **原型開發者本身的稀缺價值可能會下降。**

2026 年 7 月針對這個分類的一則評論指出，因為 Claude Code 之類的工具降低了原型製作的進入門檻，所以這很可能會成為 5 種角色中競爭最激烈的一種。

也就是說，變得稀缺的，不只是「會做」而已，而是：

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=186534417&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787537219&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK7R3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536295925&amp;bpp=1&amp;bdt=1661&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=4370694625889&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3531&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95399754%2C42533294&amp;oid=2&amp;pvsid=5793937672150027&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**應該試什麼  
應該丟掉什麼  
哪個結果值得相信**

這種 **辨別價值的敏銳度與判斷力** 。

試作成本下降，反而更容易陷入「一直做原型，卻沒有任何一個真正成熟」的狀態。

因此，重要能力的核心，可能不只是實作很多點子，而是 **能嗅出客戶需求的直覺** ，以及 **能從多個試作中篩選出有價值假說的選擇眼光** 。

---

## ② 建造者

### 概要

AI Agent 最擅長的是建造者的「動手部分」。

Anthropic 約 40 萬筆工作階段的分析顯示，人類掌握規劃與方針判斷，AI 負責實作判斷，這個結構相當清楚。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1038984369&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787538617&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK7R3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536295939&amp;bpp=5&amp;bdt=1690&amp;idt=6&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787537221%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=8&amp;correlator=4370694625889&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4421&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=815&amp;eid=95397679%2C95399754%2C42533294&amp;oid=2&amp;psts=AOrYGsnSg-m46FmwkHwg1RyqSFztMXzBQuZWpRHww4p8iI3dRTQqnSyfPKZrKJgrtSzZsAwvLoAPHqxDVeaWfOHkoALhYcfddTBeWILTsV7MJDdBN3cKyBPYNXPNRG50HJmVRQ%2CAOrYGsnNhqt5kzMYOwdw-omhcS3mvA-Lch1lJfeF1FBitrRZpTwUNWMitjgcuhKpZYEA5sRJP8szH91exBLS5LBIjplCVdNnCdzlEaausg%2CAOrYGsntdmX-nHHZMh6bjoC9hbUC5OmfwbIMq71-3RT8urNT_Ai3C0djZ2zaaIEQmStJZcwZDMlA2K93m_k49V8imfhINlwKCbcKeLQw7mhH88-2IA%2CAOrYGsl5FmGc2pJd_Tsxv-vm3uXBYbvfVP0wJtA_QC15_mNQ0rleXgbcMfDWVeCltBijXuHlW67uT_Ty-wM4L7b7X4wq9aDK3LPthPVzgzeGnIvD%2CAOrYGsn8x8nEuaZWPD8T_XzCbKsmagMYk_Ugeh6gHEOIhacAHoGtIbWIAYN2nJ2Pq014Nqy9mw3h050wOfiCzgcSR24Ise66EteqRvbitFB5Drsi%2CAOrYGsmy8NyfruslPIwAXXcveEWDQueADaQau3ytLKuavs_Q0EMXHqxRqGpAnYycREBB8aTJpUT-CJARhm_7xKUKCfAaC-Yf3QDkLOFbGtUwaik&amp;pvsid=5793937672150027&amp;tmod=1573633200&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C42%2C1920%2C42%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

而且，人類對目標領域的專業越高，對 Claude 的指示就越精準，也越能可靠地完成更多工作。

因此，優秀的建造者，可能會從：

`自己寫大量程式碼`

轉變為：

`規格／驗收條件 → 架構／限制 → 測試／驗證標準 → 委託 AI Agent 實作 → 審查`

這樣的開發迴圈設計與監督者。

當然，也可以讓 AI 協助撰寫架構與測試。重點不是人類把所有事情都手動做完，而是 **對於要做什麼，以及滿足哪些條件才算正確，保有最終判斷權** 。

### 批判

最大的問題是：

**「能運作的軟體」與「production-grade 的軟體」之間，距離仍然很大。**

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1621299728&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787538619&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK7R3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536295963&amp;bpp=6&amp;bdt=1714&amp;idt=6&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787537221%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=9&amp;correlator=4370694625889&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5254&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1649&amp;eid=95397679%2C95399754%2C42533294&amp;oid=2&amp;psts=AOrYGsnSg-m46FmwkHwg1RyqSFztMXzBQuZWpRHww4p8iI3dRTQqnSyfPKZrKJgrtSzZsAwvLoAPHqxDVeaWfOHkoALhYcfddTBeWILTsV7MJDdBN3cKyBPYNXPNRG50HJmVRQ%2CAOrYGsnNhqt5kzMYOwdw-omhcS3mvA-Lch1lJfeF1FBitrRZpTwUNWMitjgcuhKpZYEA5sRJP8szH91exBLS5LBIjplCVdNnCdzlEaausg%2CAOrYGsntdmX-nHHZMh6bjoC9hbUC5OmfwbIMq71-3RT8urNT_Ai3C0djZ2zaaIEQmStJZcwZDMlA2K93m_k49V8imfhINlwKCbcKeLQw7mhH88-2IA%2CAOrYGsl5FmGc2pJd_Tsxv-vm3uXBYbvfVP0wJtA_QC15_mNQ0rleXgbcMfDWVeCltBijXuHlW67uT_Ty-wM4L7b7X4wq9aDK3LPthPVzgzeGnIvD%2CAOrYGsn8x8nEuaZWPD8T_XzCbKsmagMYk_Ugeh6gHEOIhacAHoGtIbWIAYN2nJ2Pq014Nqy9mw3h050wOfiCzgcSR24Ise66EteqRvbitFB5Drsi%2CAOrYGsmy8NyfruslPIwAXXcveEWDQueADaQau3ytLKuavs_Q0EMXHqxRqGpAnYycREBB8aTJpUT-CJARhm_7xKUKCfAaC-Yf3QDkLOFbGtUwaik%2CAOrYGsm7nmIoBbUq6RdjNjWkcmfSFbP9JzxlbOBt3-FA2GCCZwkMRMafOW0ufCZoEQCigEF0AepKBWEPp0RN39tTbxAtes7z1oKsNDLgFQ&amp;pvsid=5793937672150027&amp;tmod=1573633200&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C42%2C1920%2C42%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Anthropic 自家員工調查也提到，有些人表示理解 Claude 寫出的程式碼所需的認知負擔增加了，後續除錯與整理的時間也可能變多。

此外，對於複雜且高風險的工作，許多員工仍然會積極驗證 AI 的成果。

在 ExperiencedDevs 等社群中，也能看到多起個別回報，指出 AI 產生的程式碼增加後，審查端的負擔變高了。

這些都只是社群上的個別案例，但 AI 提升實作速度後，設計、審查與治理端反而成為新瓶頸，這個方向也與 Anthropic 內部調查相符。

也就是說，未來建造者所需要的能力，可能會從：

**「快速寫程式的能力」**

轉向：

**「運用 AI 打造不容易壞的系統的能力」**

---

## ③ 整理者

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3970593978&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787539414&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK7R3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536295979&amp;bpp=1&amp;bdt=1742&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787537221%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280&amp;nras=10&amp;correlator=4370694625889&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6163&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2548&amp;eid=95397679%2C95399754%2C42533294&amp;oid=2&amp;psts=AOrYGsnSg-m46FmwkHwg1RyqSFztMXzBQuZWpRHww4p8iI3dRTQqnSyfPKZrKJgrtSzZsAwvLoAPHqxDVeaWfOHkoALhYcfddTBeWILTsV7MJDdBN3cKyBPYNXPNRG50HJmVRQ%2CAOrYGsnNhqt5kzMYOwdw-omhcS3mvA-Lch1lJfeF1FBitrRZpTwUNWMitjgcuhKpZYEA5sRJP8szH91exBLS5LBIjplCVdNnCdzlEaausg%2CAOrYGsntdmX-nHHZMh6bjoC9hbUC5OmfwbIMq71-3RT8urNT_Ai3C0djZ2zaaIEQmStJZcwZDMlA2K93m_k49V8imfhINlwKCbcKeLQw7mhH88-2IA%2CAOrYGsl5FmGc2pJd_Tsxv-vm3uXBYbvfVP0wJtA_QC15_mNQ0rleXgbcMfDWVeCltBijXuHlW67uT_Ty-wM4L7b7X4wq9aDK3LPthPVzgzeGnIvD%2CAOrYGsn8x8nEuaZWPD8T_XzCbKsmagMYk_Ugeh6gHEOIhacAHoGtIbWIAYN2nJ2Pq014Nqy9mw3h050wOfiCzgcSR24Ise66EteqRvbitFB5Drsi%2CAOrYGsmy8NyfruslPIwAXXcveEWDQueADaQau3ytLKuavs_Q0EMXHqxRqGpAnYycREBB8aTJpUT-CJARhm_7xKUKCfAaC-Yf3QDkLOFbGtUwaik%2CAOrYGsm7nmIoBbUq6RdjNjWkcmfSFbP9JzxlbOBt3-FA2GCCZwkMRMafOW0ufCZoEQCigEF0AepKBWEPp0RN39tTbxAtes7z1oKsNDLgFQ%2CAOrYGsk8y82RmXmrqc6GeImT09_BiywsMgKddWHHkxKeTzpHMLP1Wqr5hOLJHYZwM9ezqyvPblxNFYobSb8N1Gh5VL0j7yJb5HhH4lM4YQF-D7ZNLA&amp;pvsid=5793937672150027&amp;tmod=1573633200&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C42%2C1920%2C42%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這是 5 種分類中， **在 AI 時代可能更重要的角色** 。

AI 讓「新增」變得非常低成本，但：

- 這個功能不需要
- 這個抽象層不需要
- 這個畫面可以拿掉

這種「做減法」則需要理解目的、使用者體驗與未來規劃後才能判斷。

Anthropic 內部也有 8.6% 的 Claude Code 任務，是用在重構等「減少日常小摩擦的改善（papercut fixes）」上。

### 批判

有人批評把整理者當成獨立的「後段作業」本身就不恰當。

UI 的精緻化與系統的簡化，並不是建造者把東西做完之後才來收尾的工作，而是應該從設計與實作階段就一併納入。

另外也有評論指出，整理者不是屬於某個特定產品階段，而是從原型到成熟產品都一直需要的角色。

這麼看來，整理者與其說是「做清理的人」，不如說是 **判斷該保留什麼、刪除什麼，以及要簡化到什麼程度的能力** 。這個任務本身就直接連結到產品策略，因此需要高度的產品、需求與行銷理解。

---

## ④ 成長推進者

成長推進者是負責擴大既有產品的人。

隨著 AI 讓實作成本大幅下降，真正的瓶頸就會轉向 **「要改善什麼」** ，因此其相對重要性反而會提高。

有一位實務工作者把這 5 分類套用到自己的個人開發後指出，原型開發者、建造者、整理者、維運者都可以相當程度地 AI 化，但只有成長推進者因為需要真實使用者這個「外部世界」，所以最難自動化。

### 批判

反過來說，這個分類中邊界最模糊的也是成長推進者。

依情境不同會有些差異，但若把成長推進者的工作對應到既有職種，會涵蓋：

- 客戶研究
- 市場選擇
- 定位
- 產品策略
- 銷售與獲客渠道
- 定價設計
- 業務

因此也可以說，「成長推進者」這個詞塞進了太多能力。

另外，如果只是不斷改善局部 KPI，還可能錯過真正需要的產品策略調整。

---

## ⑤ 維運者

在 AI 時代，這不但不會消失，反而可能因為產品數量與產生的程式碼量增加，而讓工作總量上升的角色。

重點包括資安、可靠性、可觀測性、事件應變、成本最佳化、移轉作業等。

對於例行性的監控、測試、資安檢查，AI Agent 較容易接手，但事故發生時的最終責任仍然在於人類。

Anthropic 也說明，當 AI Agent 被賦予越大的權限，潛在損害範圍就會越大，因此在提升 AI Agent 能力的同時，也在加強封鎖與監督。

### 批判

最大的問題是 **不容易被評價** 。

「做了 10 個新功能」很容易看見，但：

「沒有發生故障」  
「沒有引入不必要的複雜性」  
「避免了事件發生」

這些成果卻很難被看見。

最近針對這個分類的評論也指出，維運者是「最不光鮮、最不容易被評價但又不可或缺」的角色，而且如果把角色固定化，可能會出現「維運者是職涯死胡同」這種階層化認知。

## 對 5 種原型分類本身的主要批判

## 批判 1：把人本身分成 5 類的危險性

前 Meta／Microsoft 的 Kun Chen 針對 Cherny 的 X 貼文批評說，一旦選擇了某種原型，人就可能停止反問自己。

Cherny 也同意這點，回應說「角色會隨時間與專案而改變」。

這非常重要。

把分類：

> A 是建造者  
> B 是維運者

這樣當成 **貼在人身上的固定標籤** 是很危險的。

更好的用法應該是：

> 這個專案現在建造者太多，缺少整理者與成長推進者

也就是把它當成 **診斷當下團隊缺少哪種工作能力的框架** ，會更有用。

## 批判 2：偏向矽谷型產品組織

LinkedIn 上，Vivek Bharadwaj 批評這 5 分類太偏向矽谷型產品公司。

他有趣的主張不只是指出「5 分類少了什麼角色」。

他認為，在很多組織裡， **原型開發者、建造者、整理者、維運者，並不是由不同的人分工，而常常是一個建造者在擅長與不擅長之間擺盪，負責整個產品生命週期** 。

因此 Bharadwaj 建議把這 4 個合併成廣義的「建造者」，保留成長推進者作為獨立角色，並再加入以下兩種：

- **Handler** ：實際推動複雜組織運作的人。常出現在 Chief of Staff、業務／事業營運、CX 策略等角色
- **Grown Ups** ：為加速中的組織踩煞車的人。負責法務、財務、風險管理等

從這個批評可以看出，Cherny 的 5 分類雖然很能解釋 **產品的製作、改善與維持** ，但對於實際推動公司運作的組織管理，以及法務、財務、風險管理等治理工作，涵蓋得還不夠。

確實，原本的 5 分類幾乎沒有提到 **組織運作、法務、財務、風險管理、招募、客戶合約、團隊管理** 等主題。

因此，與其把這個分類當成整家公司職種模型，不如把它視為 **產品開發中的 5 種原型** 會更自然。

## 總結

## 與其說是 5 種「職種」，不如說是 5 種「任務憲章」更有用

綜合目前資訊來看，最恰當的理解應該是這樣：

產品狀態重視的原型PMF 之前原型開發者 + 建造者 + 整理者尋找 PMF 並成長中建造者 + 整理者 + 成長推進者 + 一部分維運者已建立強 PMF整理者 + 成長推進者 + 維運者 + 一部分建造者因此，把人類端特別重要的問題，依原型整理如下：

原型人類端要回答的問題原型開發者 **應該試什麼？** 建造者 **應該把什麼、以什麼品質交付到正式環境？** 整理者 **應該保留什麼、刪除什麼、如何簡化？** 成長推進者 **應該改善什麼，才能讓使用者的 PMF 更高？** 維運者 **應該保護什麼、接受哪些風險，以及如何偵測與復原異常？** ---

## 最終評價

觀點評價「工程師這個職種會消失」 **現在還言之過早** 職種邊界變得模糊 **有相當強的跡象** 一個人同時負責多個領域 **Anthropic 內部也已觀察到** 比起實作，判斷力更重要 **非常有說服力** 直接把 5 分類當成職種 **不太建議** 作為團隊／產品分析框架 **非常有用** 這場討論最有價值的地方，其實不是「未來工程師會不會變成原型開發者或建造者」這件事。

真正重要的是， **隨著 AI 降低實作本身的成本，人類端的問題定義、判斷、驗證與責任這些能力，其價值正在相對上升**

若再往前一步，

與其問：

**「你屬於哪一種原型？」**

不如問：

**「現在這個專案缺少哪一種原型？」**

這樣的用法更合理。

如果把它當成固定職種，可能會導致角色僵化與不必要的階層化。

但若把它當成 **診斷團隊能力組合的 5 個軸線** ，那麼它很可能會成為思考 AI 原生開發組織時，非常實用的框架。

## 最後

我也看了幾篇談這 5 種原型的文章，發現有些文章幾乎沒有懷疑這個分類的前提，就直接介紹，感覺有點危險。

這次我請 GPT-5.6 在蒐集資料時「也納入批判性意見」，結果只加了這句話，調查的視角就差很多，這點很有意思。

當一個人被貼上「原型開發者」「建造者」這種標籤時，多少會想接受這個分類。

但如果這個標籤反而成為你認為「這不是我工作」的理由，那就本末倒置了。Kun Chen 的批評，我想正是在指出這一點。

另一方面，這次調查得到的「把它當成一種框架來用」這個結論，確實很有說服力。

如果在組隊時把這 5 種角色寫在白板角落，即使在方向感較模糊的 AI 時代，組織設計的視野也可能會清楚一些。

而這類原型本身，應該也會隨著 AI 發展而被進一步精煉，甚至連前提都會被推翻。

我想之後也會持續定期回顧，觀察 AI 時代的組織與開發者角色會如何變化。

---

原文出處： [https://qiita.com/Toyo\_m/items/ee1f9493d61f1c8c4743](https://qiita.com/Toyo_m/items/ee1f9493d61f1c8c4743)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3KK7R3) [36瀏覽](https://codelove.tw/@tony/post/3KK7R3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3KK7R3)

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

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝9 💬4 ❤️1

190

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

6

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1787536294&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK7R3&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536294412&amp;bpp=3&amp;bdt=97&amp;idt=159&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4370694625889&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95399754%2C42533294&amp;oid=2&amp;pvsid=5793937672150027&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=236" title="Advertisement" aria-label="Advertisement"></iframe>