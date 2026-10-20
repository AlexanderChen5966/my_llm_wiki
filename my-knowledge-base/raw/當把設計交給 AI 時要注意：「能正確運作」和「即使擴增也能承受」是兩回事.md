---
title: "當把設計交給 AI 時要注意：「能正確運作」和「即使擴增也能承受」是兩回事"
source: "https://codelove.tw/@tony/post/x2vnkx"
author:
published:
created: 2026-09-07
description: "在使用生成式 AI 推進 Push 通知功能設計時，我注意到了一個在效能面上令人介意的架構。從設計上來看，**處理 1 位通知對象時，最多可能對 DB 查詢約 11 次**。通知對象 DB 查詢1 人約 11 次100 人約 1,100 次1,000 人約 11,000 次這次不是在說正式環..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月05日](https://codelove.tw/@tony/post/x2vnkx "2026-09-05 17:05:07")

## 當把設計交給 AI 時要注意：「能正確運作」和「即使擴增也能承受」是兩回事

在使用生成式 AI 推進 Push 通知功能設計時，我注意到了一個在效能面上令人介意的架構。

從設計上來看， **處理 1 位通知對象時，最多可能對 DB 查詢約 11 次** 。

通知對象 DB 查詢1 人約 11 次100 人約 1,100 次1,000 人約 11,000 次這次不是在說正式環境發生了障礙，而是 **在設計階段就發現問題並重新檢討** 的故事。

## 1,000 人就 11,000 次。問題在哪裡？

對 DB 的查詢，不只是單純計算一次而已。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788742929&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx2vnkx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788742929180&amp;bpp=1&amp;bdt=558&amp;idt=-M&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788742928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788742928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788742928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250&amp;nras=2&amp;correlator=3630711439562&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=943&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;pvsid=3289694897795644&amp;tmod=1931121027&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=34" title="Advertisement" aria-label="Advertisement"></iframe>

會發生應用程式向 DB 發出查詢、DB 搜尋資料、再回傳結果這樣的互動。

例如，假設要在公家機關確認 1,000 人的資料。

原本只要

> 請幫我一次整理這 1,000 人的資料

就能完成，卻變成每 1 個人都要跑 11 次櫃檯確認的感覺。

如果是 1,000 人，那就是 11,000 次往返。

雖然電腦或伺服器不會立刻壞掉，但 DB 的通訊、查詢、連線等待會增加，甚至可能讓其他 API 的處理也變慢。

## 檢視設計時讓我在意的點

在確認送來的設計內容時，我在意的是， **針對每位對象的處理中，DB 存取重複出現了好幾次** 。

每一個處理本身都很自然。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788742929&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx2vnkx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788742929180&amp;bpp=1&amp;bdt=557&amp;idt=-M&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788742928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788742928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788742928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280&amp;nras=3&amp;correlator=3630711439562&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1784&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;pvsid=3289694897795644&amp;tmod=1931121027&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=35" title="Advertisement" aria-label="Advertisement"></iframe>

```
確認通知對象
確認通知條件
取得裝置資訊
確認是否已通知
保存結果
```

但是，如果對每位對象都重複執行這些步驟，

```
每人約 11 次 × 對象人數
```

DB 查詢次數就會這樣增加。

因此我把人數乘上去一算，發現 100 人約 1,100 次、1,000 人約 11,000 次，於是決定重新檢討設計。

## 即使一個一個發送通知，也不需要一個一個去查 DB

Push 通知本身，確實需要針對每位對象分別發送。

但是，通知所需的資訊不需要每次都去 DB 重新取得。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788742929&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx2vnkx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788742929180&amp;bpp=1&amp;bdt=557&amp;idt=-M&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788742928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788742928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788742928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280&amp;nras=4&amp;correlator=3630711439562&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2627&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;pvsid=3289694897795644&amp;tmod=1931121027&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=35" title="Advertisement" aria-label="Advertisement"></iframe>

例如可以設計成：

```
先從 DB 一次取得所需資訊
↓
再針對對象逐一發送 Push 通知
↓
最後將結果彙總寫回 DB
```

重點不在於 SQL 一定要寫幾條，而是：

**避免出現「對象人數增加 10 倍，DB 查詢次數也跟著增加 10 倍」的架構**

## 使用 AI 做設計時的感想

這次的設計並不是「因為 AI 所以出錯」的故事。

就算是人類自行設計，也同樣可能發生這種問題。

只是，使用生成式 AI 時，從設計到實作的速度會非常快。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788742929&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx2vnkx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788742929180&amp;bpp=1&amp;bdt=558&amp;idt=-M&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788742928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788742928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788742928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=5&amp;correlator=3630711439562&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3450&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;pvsid=3289694897795644&amp;tmod=1931121027&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=5&amp;fsb=1&amp;dtd=36" title="Advertisement" aria-label="Advertisement"></iframe>

因此不能只確認：

> 能不能動

就直接往下走，還需要另外檢查：

> 如果是 100 人呢？  
> 如果是 1,000 人呢？  
> SQL 會發出幾次？  
> 有沒有在迴圈內存取 DB？

我也覺得，讓 AI 不只負責實作，而是

> 請只針對效能面進行審核

來做 review，會很有效。

## 總結

這次在 Push 通知功能的設計階段，

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788746049&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx2vnkx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788742929165&amp;bpp=1&amp;bdt=571&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C970x90&amp;nras=7&amp;correlator=3630711439562&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4340&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=727&amp;eid=95399630%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslNsICWoEX8o5j0iC6mj8Ft9KGQ3Xm2A52OahcMJA8HvN1h2giMzE3HKtrr0l3wnXA9-irUHTolFslFKVoLKzmQgeXmQS52CBj14vOYMB_i9b7AdHGB2wqn8Go%2CAOrYGskqmaT3d1l87AELt6fXdWS9SLIVnE01mdA_8-e-VOnI1k8Se9kK1HhUXc3KAZmwlROc2BKhmMv3Yp-_qE7SpdBl0XUc2_Vgr1Z6YA%2CAOrYGsnG4QnSt6wfBPzSmW-beyiZf8vumwnnvXK4sYn-dlAaNpYXT3Gb62Lky0Zk7rPM2sAaMGvKF-P1Z8H-IrTBYei53tlGPctXU6mX%2CAOrYGskwwbY5Ur6vxhbqUAUBZV_DKmg-uYlNwBTr-YkaXRndQ-ew3O3d97tgFnRUjt1WR8qFRR-72ovmvFZIs9y5IKU_hL8Z_VN-N6Af%2CAOrYGsmBIfdKQfsYH_QbdrbkScDn3tr50_ablvHL9G6F1fs_-UX74Q0EIJRX5_djyELn_rrLDf7eSJ1zjxVTf1pavno5aALzWMCKJoO7&amp;pvsid=3289694897795644&amp;tmod=1931121027&amp;uas=3&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**發現了每位對象最多可能產生約 11 次 DB 查詢的架構**

。

這次學到的事很簡單：

> **「能正確運作」和「使用者增加後仍能承受」是兩回事。**

使用 AI 可以提升開發速度。

也因此，在設計完成後，最好再確認一次：

**「如果規模變成 100 倍會怎樣？」**

我認為這件事比以往更加重要。

---

原文出處： [https://qiita.com/adgjmptw0/items/365a4437aad4c939d1cc](https://qiita.com/adgjmptw0/items/365a4437aad4c939d1cc)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/x2vnkx) [15瀏覽](https://codelove.tw/@tony/post/x2vnkx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/x2vnkx)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝21

324

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788742928&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx2vnkx&amp;pra=5&amp;asro=0&amp;itsi=9&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788742928713&amp;bpp=4&amp;bdt=90&amp;idt=44&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788742562%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788742562%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788742562%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3630711439562&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;pvsid=3289694897795644&amp;tmod=1931121027&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=48" title="Advertisement" aria-label="Advertisement"></iframe>