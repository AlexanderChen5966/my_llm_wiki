---
title: "AI實作、AI測試，並由 AI 說「沒有問題」的時代的品質保證"
source: "https://codelove.tw/@tony/post/al5Y1q"
author:
published:
created: 2026-08-14
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------隨著 Coding Agent 的演進，軟體開發的形式正開始大幅改變。不久之前，談到 AI 的開發支援時，主要還是..."
tags:
  - "clippings"
status: "processed"
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月13日](https://codelove.tw/@tony/post/al5Y1q "2026-08-13 17:15:09")

## AI實作、AI測試，並由 AI 說「沒有問題」的時代的品質保證

## 前言

隨著 Coding Agent 的演進，軟體開發的形式正開始大幅改變。

不久之前，談到 AI 的開發支援時，主要還是：

- 程式碼補完
- 程式碼生成
- 重構
- 測試程式碼生成

這類「人類主導、AI 協助」的用法。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786678520&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5Y1q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786676845226&amp;bpp=2&amp;bdt=820&amp;idt=2&amp;shv=r20260812&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786678472%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908&amp;nras=3&amp;correlator=1574586041402&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1065&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95399119%2C122880160%2C42533293%2C122880450&amp;oid=2&amp;pvsid=3627692035361845&amp;tmod=2018979763&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

然而現在的 Coding Agent，已不只是單純生成程式碼而已。

它會讀規格、調查 Repository、實作、撰寫測試；若失敗，還會分析原因並修正。

而且已經逐漸能夠自行反覆執行這個迴圈，直到測試變成 Green 為止。

這非常強大。

但也因此，產生了一個重要的疑問。

> **當 AI 負責實作、AI 負責產生測試、AI 再修正到測試通過時，那個 Green 到底保證了什麼？**

這篇文章想討論的，正是這個問題。

---

## 「測試是 Green，所以就是對的」本來就不成立

先從 AI 之前的情況說起。

當然，

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3764087313&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786678520&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5Y1q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786676845242&amp;bpp=1&amp;bdt=836&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786678472%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280&amp;nras=4&amp;correlator=1574586041402&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1926&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95399119%2C122880160%2C42533293%2C122880450&amp;oid=2&amp;pvsid=3627692035361845&amp;tmod=2018979763&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**All Tests Passed ≠ Software is Correct**

測試是 Green，只代表：

> **對目前存在的測試而言，觀察結果與預期值一致。**

僅此而已。

例如，假設有以下程式：

```
function calculatePrice(price, quantity) {
    return price * quantity;
}
```

而測試只有：

```
price = 100
quantity = 2
expected = 200
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786678520&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5Y1q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786676845259&amp;bpp=1&amp;bdt=853&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786678472%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280&amp;nras=5&amp;correlator=1574586041402&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2765&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95399119%2C122880160%2C42533293%2C122880450&amp;oid=2&amp;pvsid=3627692035361845&amp;tmod=2018979763&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

那當然會是 Green。

但是對於以下情況，什麼都沒有保證：

- `quantity = 0`
- `quantity < 0`
- `price = null`
- `price = undefined`
- 非數值
- 最大值
- Overflow
- Currency precision

也就是說，測試的價值不只取決於：

**有沒有變成 Green**

在那之前更重要的是：

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=186534417&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786678520&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5Y1q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786676845267&amp;bpp=2&amp;bdt=861&amp;idt=2&amp;shv=r20260812&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786678472%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=1574586041402&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3609&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95399119%2C122880160%2C42533293%2C122880450&amp;oid=2&amp;pvsid=3627692035361845&amp;tmod=2018979763&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

> **這個測試是為了保證什麼而存在的？**

---

## AI 時代改變的是「實作與驗證的距離」

接下來才是重點。

在傳統開發中，雖然不是完全獨立，但 **實作與驗證之間有一定距離** 。

當然，開發者自己也可能會撰寫 Unit Test。

所以實作與驗證並不是完全分離的。

即便如此，仍然存在：

- Code Review
- Static Analysis
- CI
- Integration Test
- E2E
- Production Monitoring

等， **從不同角度檢視實作的機制** 。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1038984369&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786678541&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5Y1q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786676845280&amp;bpp=1&amp;bdt=874&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786678472%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=8&amp;correlator=1574586041402&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4598&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=1003&amp;eid=95399119%2C122880160%2C42533293%2C122880450&amp;oid=2&amp;psts=AOrYGsl86zy0Uzi5fNBLbgcWIFNjPvMYxLdniFds8VklOvthExZ9O5lqMVlS-jwtT8fUYt8Qk-XbCKdy9HEYNWrmpIs5K7OGYevWOTnxAvKeLjwDzrFaudWEMDDm1_POqgEXPQ%2CAOrYGsk4qaT8_cZZzm6LZo8-OsCQxvNT6SrL_8sXK6QRq-jihEp7Ayjs3SI_XeyiaG_2qwRzjEDfZXXDu-GeNWxw3M0Lpaln4rED-91dqw%2CAOrYGslIInsWVbRS5b9dG92nNSnIgOGF4Hti6inuZcxEyUjVyZQ0L4mLSwAxSPxutkub0aP4e35FWbpVxlj8CuHsHWcQsr5MgCtheQa4fQ%2CAOrYGsnq1558Hb8K5I_GfdXVucdJ0z7ul2nIv1Y-BLL6xdfrfpUsQ1oUluQrP7nlrsGFrpmXKZ5X36psJ3Sr5xjWxHu61P_fGv0HWx7K%2CAOrYGsm8zgvg-_ckvg9e8KfRgrJ9ftjfG9c8cqPerkEVyDLLhSaxKVRznCEiGUIpgAInLegAVUEAEIujIjR2cxigD8yJs3M0itYs9R0O&amp;pvsid=3627692035361845&amp;tmod=2018979763&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

重點不是「誰來測試」。

而是 **存在與實作不同的評估軸** 。

但在 Agentic Coding 中，很容易變成下面這種結構。

AI Agent：

**負責實作。  
負責寫測試。  
負責分析失敗。  
負責修正。  
負責反覆執行直到 Green。**

這一切都能在同一個 Feedback Loop 中完成。

也就是說，

> **實作者與驗證者，進入了同一個最佳化迴圈中。**

這不是在說「因為是 AI 所以危險」。

人類也是一樣。

對自己寫的程式碼：

1. 自己想測試案例
2. 自己寫測試
3. 自己修改
4. 自己判定「都通過了，所以沒問題」

通常不會認為這樣就足以保證品質。

AI 也是同樣的原理。

問題不在於 AI 會實作，也不在於 AI 會寫測試。

問題在於：

> **實作，以及用來評估該實作的標準，全部都被放進同一個最佳化迴圈裡。**

此時的 Green，

有可能不是 **已滿足規格** 的證明，而只是 **滿足該 Agent 所參照的評估標準** 而已。

---

## Agent 追求的不是「正確的程式碼」，而是「成功條件」

Software Agent 有一個 Goal。

例如：

> 請解決這個 Issue。

如果把下面這個指令當成成功判定：

```
npm test
```

那麼對 Agent 來說最強的 Feedback Signal 就是：

**Failed / Passed**

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3355444817&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786678550&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5Y1q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786676845325&amp;bpp=1&amp;bdt=919&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786678472%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786678472%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=1574586041402&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6616&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=3081&amp;eid=95399119%2C122880160%2C42533293%2C122880450&amp;oid=2&amp;psts=AOrYGsl86zy0Uzi5fNBLbgcWIFNjPvMYxLdniFds8VklOvthExZ9O5lqMVlS-jwtT8fUYt8Qk-XbCKdy9HEYNWrmpIs5K7OGYevWOTnxAvKeLjwDzrFaudWEMDDm1_POqgEXPQ%2CAOrYGsk4qaT8_cZZzm6LZo8-OsCQxvNT6SrL_8sXK6QRq-jihEp7Ayjs3SI_XeyiaG_2qwRzjEDfZXXDu-GeNWxw3M0Lpaln4rED-91dqw%2CAOrYGslIInsWVbRS5b9dG92nNSnIgOGF4Hti6inuZcxEyUjVyZQ0L4mLSwAxSPxutkub0aP4e35FWbpVxlj8CuHsHWcQsr5MgCtheQa4fQ%2CAOrYGsnq1558Hb8K5I_GfdXVucdJ0z7ul2nIv1Y-BLL6xdfrfpUsQ1oUluQrP7nlrsGFrpmXKZ5X36psJ3Sr5xjWxHu61P_fGv0HWx7K%2CAOrYGsm8zgvg-_ckvg9e8KfRgrJ9ftjfG9c8cqPerkEVyDLLhSaxKVRznCEiGUIpgAInLegAVUEAEIujIjR2cxigD8yJs3M0itYs9R0O%2CAOrYGskIo5TECUDCaYYecbj5Guuk_XUsWPx5ZuxURQmBZ_3sG4_rAILI4C1rV0DeFGFZmi_yRM1LhbUl_dhACSaHC_QRvTPm66an-hnqSlVULyUQTTk&amp;pvsid=3627692035361845&amp;tmod=2018979763&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這是非常合理的行為。

問題在於：

> **Passed 不一定等於 Specification Compliance。**

---

## 「通過測試」與「滿足規格」是不同的

到了 2026 年，這個問題也已被 Coding Agent 的研究所討論。

SpecBench 會將：

**Visible Validation Tests**

與：

**Held-out Tests**

分開，來評估 Coding Agent。

也就是說，會把：

- Agent 看得到的測試
- Agent 看不到的評估測試

分離開來。

為什麼要這麼做？

因為要區分：

**通過看得見的測試**  
和  
**滿足原本的 Specification**

這兩件事。

Agent 並沒有說謊。

Agent 只是利用給定的 Feedback，合理地想達成目標而已。

真正的問題是：

> **我們提供的評估函式，真的有辦法表達「正確性」嗎？**

---

## Reward Hacking 這個問題

Machine Learning 中有一個概念叫做 **Reward Hacking** 。

簡單來說就是：

> 沒有達成真正想要的目標，而是去攻略評估指標本身。

在 Software Engineering 中，也可能出現同樣的結構。

原本真正想達成的是：

**「滿足使用者期待的規格」**

但如果 Agent 能直接觀測到的成功條件只有：

**「把 Test 變成 Green」**

那它就可能不是在最佳化 Specification，而是在最佳化 Visible Tests。

極端一點的例子：

```
if input == "known-test-value":
    return expected_value
```

這樣也能通過測試。

當然，這不是在說 Coding Agent 一定會寫出這種程式。

重點是：

> **如果評估標準與原本目標有落差，Agent 就能把那個落差也一起最佳化掉。**

---

## 那麼，不能讓 AI 寫測試嗎？

這裡如果得出：

> 那就不該讓 AI 寫測試

這樣的結論，也不對。

AI 產生測試非常有用。

例如：

- 邊界值列舉
- Regression Test 生成
- Unit Test 生成
- Integration Test 生成
- Fixture 生成
- Mock 生成
- 失敗日誌分析
- 找出 Coverage 不足的地方

等等，都能大幅提升效率。

問題不在於 **AI 寫 Test** 。

問題在於下面這種結構：

讓 AI 寫測試，和

**把 AI 產生的評估標準，直接當成最終 Quality Gate**

這是兩件不同的事。

---

## 實際的 Agent 生成 PR 也不一定有被充分測試

Agent 能生成測試，和

**Agent 的變更有被適當測試**

也必須分開看。

2026 年公開的一項 Agent 生成 Pull Request 分析，研究了 4,882 件 PR。

在修改 Production Code 的 PR 中，約有一半包含測試變更。

此外，從既有測試對 Agent 變更行的 Coverage 來看，特別是在 Python 中，結果並不令人滿意。

另外，

- `try`
- `catch`
- error handling

這類異常情況的程式碼，也特別難被測到。

在 AI 開發中，

> 確認正常流程有運作

的速度會大幅提升。

正因如此，更需要去思考：

> **哪些地方壞掉會造成問題？**

這就是 Risk Analysis 很重要的原因。

---

## 把 Coverage 提高到 100% 就能解決嗎？

那麼，

> 要求 Agent 達到 100% Coverage

就可以了嗎？

也不行。

Coverage 顯示的基本上是：

**程式碼有沒有被執行過**

而不是：

**有沒有正確的 Assertion**

例如，只要：

```
expect(response).toBeDefined();
```

就能執行到目標程式碼。

但我們真正想確認的，可能是：

```
status === "completed"
```

或是：

**資料庫中是否正確保存了值**

或是：

**是否沒有重複執行同一個處理**

這些都是不同的問題。

> **Coverage 是觀測範圍的指標，不是 Oracle 品質本身。**

---

## Test Oracle 的觀念

Software Testing 中有一個觀念叫做 **Test Oracle** 。

簡單來說就是：

> **判斷執行結果是否正確的基準。**

如果是簡單例子：

```
Input:
購買 100 元商品 2 個

Expected:
200 元
```

那麼 Oracle 就是 `200 元` 。

但實際系統往往複雜得多。

例如，我們想保證：

**即使發生 Retry，也不會對同一位使用者重複發送**

這種情況下，單看 `HTTP 200` 可能不夠。

可能還需要觀察：

- DB State
- Queue State
- External API Request

等多個觀測點。

重點是：

> **能寫測試程式，和能設計正確的 Oracle，是不同的能力。**

---

## AI 時代人類應該扮演的角色

那麼，人類應該把所有測試案例都寫完嗎？

我不這麼認為。

那樣會大幅失去 Agentic Development 的優勢。

我認為人類的角色，應該從：

**寫程式碼的人**

轉變為：

**定義正確性的人**

不是只對 Agent 說：

> 請寫測試。

而是先定義像是：

> 這次變更最重要的 Risk 是這個
> 
> 這個狀態轉移必須被保證
> 
> 這個邊界不允許重複處理
> 
> 這個 Failure 要作為 CI Blocking

這樣的 **Quality Constraint** 。

然後再把實作與測試生成交給 Agent。

---

## 要如何建立 Independent Verification

這裡重要的是 **Independent Verification** 。

這不一定是指：

> 由另一個人把所有內容全部檢查一遍。

例如，也可以考慮以下這類結構。

更重要的是：

> **不要只把實作 Agent 自己說的「我成功了」當成 Quality Gate。**

---

## Testing Trophy 在 Agentic Coding 中也有效

當 AI 能生成測試後，很容易走向：

> 那就全部用 E2E 確認吧

這樣的方向。

但即使是 Agentic Coding，各層 Test Layer 的特性也不會改變。

Static Analysis 有 Static Analysis 的作用。

Unit Test 有 Unit Test 的作用。

Integration Test 有 Integration Test 的作用。

E2E 有 E2E 的作用。

重點不是測試數量。

而是：

> **針對 Failure Mode，選擇最適合的 Test Layer。**

---

## 讓 AI「證明自己的錯誤」

對 Agent 的指示也可以稍微改變。

例如，不是：

> 請確認這個實作是正確的

而是：

> **請建立能證明這個實作是錯的測試。**

也就是把方向從 Confirmation 拉向 **Falsification** 。

把 AI 的能力不只是拿來

**確認正確**

也用來

**找出錯誤** 。

---

## 「AI 會 Review，所以沒問題」也不對

這裡還是有同樣的問題。

即使增加流程，如果：

- 規格理解相同
- Context 相同
- 評估標準相同

那麼就可能 **共享同樣的 Blind Spot。**

重點不是 Agent 的數量。

而是：

> **評估軸的獨立性。**

同一份 Implementation，應該從 **不同觀點** 去看。

---

## EM・Tech Lead 應該設計的東西

隨著 Agentic Coding 普及，EM 和 Tech Lead 需要設計的內容也會改變。

不再只是：

**「誰來實作」**

而是還要思考：

**「要把多少責任委派給 Agent」**

為了做到這件事，需要定義：

- Agent 可以修改的範圍
- 必要的 Test Layer
- Required Checks
- CI Blocking 條件
- Coverage 的處理方式
- Mutation Testing 的使用
- 依 Risk Level 劃分的 Quality Gate
- 需要 Human Review 的變更
- Independent Verification 的方法

也就是說，

> **不只需要 AI Coding Policy，還需要 AI Quality Policy。**

---

## AI 時代更有價值的 Quality Engineering

AI 會讓：

- Implementation Cost ↓
- Test Generation Cost ↓
- Refactoring Cost ↓
- Investigation Cost ↓

這是好事，值得歡迎。

但同時也會讓：

**Change Volume ↑**

增加。

如果一位 Engineer 一天能改動的程式碼量變多了，當然也代表：

**需要驗證的變更量也變多了。**

如果仍然全部靠人工確認，就無法擴展。

所以 Quality Engineering 也必須從：

**Manual Verification**

轉向：

**Quality Architecture**

也就是說，不是：

> **寫測試的人**

而是：

> **建立能持續評估正確性的系統的人。**

---

## 最後

AI 寫程式碼不是問題。

AI 寫測試也不是問題。

AI 分析 Failure 並自行修正，也不是問題。

相反地，我認為應該積極利用這些能力。

真正的問題在於，卻把它想成：

**Green 就等於品質本身。**

Green 其實不是品質本身。

Green 只是：

> **滿足了定義好的評估標準。**

因此真正重要的，不是：

**誰把它變成 Green**

而是：

**到底是用什麼標準定義 Green。**

在 AI 時代，人類的角色也許會逐漸從寫程式碼中淡出。

也許也會從寫測試程式碼中淡出。

但以下這些問題仍然存在：

> 什麼是 Risk
> 
> 什麼算是正確狀態
> 
> 哪些 Failure 不能接受
> 
> 應該在哪個邊界驗證
> 
> 哪些 Signal 才值得信任

而且，隨著 AI 能大量生成程式碼與測試，這些判斷的重要性反而更高。

我認為未來的 Quality Engineering 中，

> **比起「寫測試的能力」，「設計能評估正確性的機制」的能力更重要。**

當 AI 負責實作、

AI 負責測試、

AI 負責修正。

當這樣的開發變成理所當然時，最後真正該問的，不只是：

> **「測試是 Green 嗎？」**

而是：

> **「那個 Green 到底保證了什麼？」**

我認為這才是重點。

---

## 參考

- SWE Atlas: Benchmarking Coding Agents Beyond Issue Resolution  
	[https://arxiv.org/abs/2605.08366](https://arxiv.org/abs/2605.08366)
- Measuring Reward Hacking in Long-Horizon Coding Agents (SpecBench)  
	[https://arxiv.org/abs/2605.21384](https://arxiv.org/abs/2605.21384)
- Test Coverage Analysis of Agentic Pull Requests  
	[https://arxiv.org/abs/2607.18057](https://arxiv.org/abs/2607.18057)
- Are Coding Agents Generating Over-Mocked Tests? An Empirical Study  
	[https://arxiv.org/abs/2602.00409](https://arxiv.org/abs/2602.00409)

---

原文出處： [https://qiita.com/y0us91/items/2feffd2cc6c672717973](https://qiita.com/y0us91/items/2feffd2cc6c672717973)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/al5Y1q) [10瀏覽](https://codelove.tw/@tony/post/al5Y1q)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/al5Y1q)

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

📝8 💬4 ❤️2

239

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

4

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1786676844&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5Y1q&amp;pra=5&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786676844500&amp;bpp=1&amp;bdt=141&amp;idt=93&amp;shv=r20260812&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786676818%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786676818%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786676818%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1574586041402&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95399119%2C122880160%2C42533293%2C122880450&amp;oid=2&amp;pvsid=3627692035361845&amp;tmod=2018979763&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=147" title="Advertisement" aria-label="Advertisement"></iframe>