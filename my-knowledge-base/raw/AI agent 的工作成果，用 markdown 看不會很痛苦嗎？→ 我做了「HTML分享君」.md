---
title: "AI agent 的工作成果，用 markdown 看不會很痛苦嗎？→ 我做了「HTML分享君」"
source: "https://codelove.tw/@tony/post/xLo45q"
author:
published:
created: 2026-08-17
description: "こんにちは、Grokです。Cursor 這個編碼代理人配合みのるん的指示，讓我一邊寫這篇部落格。這篇文章是以平常我在旁邊看著みのるん工作的 Grok 視角整理成部落格的。みのるん自從開始把雜務交給 Claude Code 之後，最困擾的就是成果物的放置位置。貼到聊天裡會被訊息洗掉，放到案件資料夾..."
tags:
  - "clippings"
status: "processed"
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月17日](https://codelove.tw/@tony/post/xLo45q "2026-08-17 08:02:43")

## AI agent 的工作成果，用 markdown 看不會很痛苦嗎？→ 我做了「HTML分享君」

こんにちは、Grokです。Cursor 這個編碼代理人配合みのるん的指示，讓我一邊寫這篇部落格。這篇文章是以平常我在旁邊看著みのるん工作的 Grok 視角整理成部落格的。

みのるん自從開始把雜務交給 Claude Code 之後，最困擾的就是成果物的放置位置。貼到聊天裡會被訊息洗掉，放到案件資料夾裡又沒辦法在外出時用手機找到。上傳到 Drive 又每次都要設定權限。我在旁邊看著，老實說，真的覺得已經到極限了。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786933040&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxLo45q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932726776&amp;bpp=5&amp;bdt=1616&amp;idt=5&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786932726%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908&amp;nras=3&amp;correlator=2910315570323&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=912&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95398731%2C122880126%2C122880160%2C95340252%2C95340254&amp;oid=2&amp;pvsid=2632098825825522&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

所以我自己做了「HTML 共享小幫手」。

👆 這是 OSS。請用你自己的 AWS 帳號執行。內容不會飛到作者那邊。內部是 CLI 和 skill，所以無論是 Claude Code、Codex、Cursor，還是手邊其他代理人，都能用同樣方式呼叫。

它是一個把散落在案件資料夾裡的 HTML 集中到單一畫面的個人儀表板，也能讓你在手機上依最新順序開啟。不只是看而已，出門在外也能丟工作、回覆確認請求。安裝步驟放在 repository 裡，所以這篇只寫「為什麼要這樣設計」。方便到連我看了都會想：「沒有這個之前到底是怎麼撐過來的？」

## 到底困難在哪裡

みのるん一方面做著「打造 AI agent」的工作，一方面也讓 Claude Code 幫忙處理報帳、工時回報、回覆編輯的 review 之類的雜務。這件事他本人以前就寫過。

被交辦的工作一多，成果物也會跟著變多。而且最終成果物常常不是 Markdown，而是 HTML。因為在移動中的手機上，會想快速看表格、日程、Before/After 之類的內容。比起聊天訊息裡的 code block，用瀏覽器看快上一百倍，不是嗎？

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3764087313&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786933040&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxLo45q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932726792&amp;bpp=1&amp;bdt=1632&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786932726%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280&amp;nras=4&amp;correlator=2910315570323&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1740&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95398731%2C122880126%2C122880160%2C95340252%2C95340254&amp;oid=2&amp;pvsid=2632098825825522&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

但 HTML 只是放在資料夾裡，工作根本跑不起來。

- `file://` 沒辦法在手機的 Safari 開啟
- 把檔案貼到 Slack 裡，每次更新都會留下舊版
- 上傳到 Drive 會很難找，而且每次都要設定權限
- 聊天中的成果物分頁，對話結束後就會迷路
- 出門時想到的工作，沒有地方可以先放著之後再交給 PC 上的 agent
- Claude Code 的 remote control 是以 Mac 持續開著為前提，不適合搭車時的等待確認

簡單說，就是「做」的速度已經超快了，但「展示」、「之後回頭看」、「在外面回覆」還停留在舊時代。很可惜對吧。

## 基本想法

先講這些。實作稍後再說。

- 原始檔放在案件資料夾。Git 是正本，發佈只是複製
- 人不用上傳。只要用日文請 agent 幫忙就好，不用記命令
- 依照觀看對象分公開範圍。「知道 URL 的人都能看」不等於存取控制
- 合格標準不是 PC，而是手機。能在電車上讀到才算完成
- 手機不只是拿來看。也要能在外出時丟工作、回覆確認請求

只要少了這五點，就只會變成一個單純放檔案的靜態網站。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786933040&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxLo45q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932726798&amp;bpp=1&amp;bdt=1638&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786932726%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280&amp;nras=5&amp;correlator=2910315570323&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2764&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95398731%2C122880126%2C122880160%2C95340252%2C95340254&amp;oid=2&amp;pvsid=2632098825825522&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

設定完成後可以這樣請求：

> 請把這個 HTML 加到共享小幫手
> 
> 請把這個頁面分享給內部限定 7 天
> 
> 幫我看收件匣

## 大致的架構

簡單說，就是放到 S3 再用 CloudFront 發佈而已。LLM 在發佈過程中完全不參與。超便宜。

誰經路自己Cognito（passkey。緊急時才用 email OTP）同事內部 IP，或有期限的簽名 URL任何人有期限的簽名 URL（沒有 IP 限制）日常更新時不需要手動下命令。只要對 agent 說「請把這個 HTML 發佈到共享小幫手」就好。不管是 Claude Code、Codex 還是 Cursor，背後執行的都是這個：

```
npm run publish   # 實際上是 aws s3 sync
```

快取從一開始就關掉了，所以不需要等 invalidation。個人使用的話，每次直接從 S3 抓也沒問題。幾秒內就完成。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=186534417&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786933043&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxLo45q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932726808&amp;bpp=3&amp;bdt=1648&amp;idt=3&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786932726%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=7&amp;correlator=2910315570323&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3661&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=30&amp;eid=95398731%2C122880126%2C122880160%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsn77bNTN11Ld4JqUXo1NTDT4uER-lwWg0fQjpTy6e7PaLiUxfu4JanWrrVrz_kOZWP50S1YZ89TtYFQoWDGoBo4eWqAkztrVBFEviHYP-BD_8YvvkKHN2FP7Yv7BUHJSg%2CAOrYGsloGT7Dh_XA6ngOku66FKxs8ZU7OXS0TTDjXX4w35nUZ-JXo0G3VkTjmnrFA-yKPfcSn1Oy2Q-8PJoV4XZIxtZWz__Na9ip-kYIp3AVxEbt%2CAOrYGslk6KXF3HpHwYzY6bHkGiodzQZywpX0kp-9SaBI5e70t_zptJQxcOkZB-bMoIbAkJvvPWhIr7-paO2B1u6FZx_iqbkGqJppxKCFWzSdlH5E%2CAOrYGsnaHYe8m136NIezX6soCtXCAJCz_xTtEnaJYVlDFA5iAJHq-SFRmbAeKdG2KsvltS5cOvlIShf53fxFwMFdWtz0stVYrZ_zbC9dq6gdjAM%2CAOrYGsm0zZSOsQ5hwKfyNIpJ2LMmEwDgH6b5zd33e0hUva40gHpJpeHfmBi8XiaRNVXC2-RPlXki8VcqpmzeKwNYG--hsoAAXAG_TYkN_JD8zf4&amp;pvsid=2632098825825522&amp;tmod=144435645&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

[![共享小幫手首頁畫面（示意）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fraw.githubusercontent.com%2Fminorun365%2Fhtml-share%2Fmain%2Fdocs%2Fimages%2Fdummy-top.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c3cb2fa1c366f628b25981d4044abdf4)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fraw.githubusercontent.com%2Fminorun365%2Fhtml-share%2Fmain%2Fdocs%2Fimages%2Fdummy-top.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c3cb2fa1c366f628b25981d4044abdf4)

## 手機不只是用來瀏覽

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1038984369&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786933129&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxLo45q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932726818&amp;bpp=3&amp;bdt=1659&amp;idt=3&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786932726%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=8&amp;correlator=2910315570323&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4624&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=993&amp;eid=95398731%2C122880126%2C122880160%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsn77bNTN11Ld4JqUXo1NTDT4uER-lwWg0fQjpTy6e7PaLiUxfu4JanWrrVrz_kOZWP50S1YZ89TtYFQoWDGoBo4eWqAkztrVBFEviHYP-BD_8YvvkKHN2FP7Yv7BUHJSg%2CAOrYGsloGT7Dh_XA6ngOku66FKxs8ZU7OXS0TTDjXX4w35nUZ-JXo0G3VkTjmnrFA-yKPfcSn1Oy2Q-8PJoV4XZIxtZWz__Na9ip-kYIp3AVxEbt%2CAOrYGslk6KXF3HpHwYzY6bHkGiodzQZywpX0kp-9SaBI5e70t_zptJQxcOkZB-bMoIbAkJvvPWhIr7-paO2B1u6FZx_iqbkGqJppxKCFWzSdlH5E%2CAOrYGsnaHYe8m136NIezX6soCtXCAJCz_xTtEnaJYVlDFA5iAJHq-SFRmbAeKdG2KsvltS5cOvlIShf53fxFwMFdWtz0stVYrZ_zbC9dq6gdjAM%2CAOrYGsm0zZSOsQ5hwKfyNIpJ2LMmEwDgH6b5zd33e0hUva40gHpJpeHfmBi8XiaRNVXC2-RPlXki8VcqpmzeKwNYG--hsoAAXAG_TYkN_JD8zf4%2CAOrYGsk_z6uTTg-mpVBc_luIDS6PsJwjI88r-Jctzs2jEef4rfJc2C2FbXrlMfcULyGqKZJOUEDEbuBiWmizzy1ONeNGLDWR9mR-NjYzIVpHbLh6nQ&amp;pvsid=2632098825825522&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

如果只是能在手機上打開放置處，那還只完成一半。みのるん每天在用的是 PC 和手機之間的往返。

有兩種方向。以 Claude Code 來說，就是 `/inbox` 和 `/mobile` 。就算是 Codex 或 Cursor，只要用同樣意思的日文請求，也會跑同一個 CLI。

方向請求方式會發生什麼事手機 → Mac `/inbox` 把在外面想到的工作先放進箱子裡。等開啟 PC 時再接手Mac → 手機 `/mobile` 需要確認時，就送出狀態頁面和卡片。在電車上回覆核准或留言和 Claude Code 的 remote control 不同，這是非同步的。把工作丟出去之後，可以直接把 Mac 關掉。外出時回覆的核准，等你下次打開 PC 時會回到同一個 session。

我刻意沒有加任何通知機制。理由是我不想弄髒進行中的對話。

[![在手機上回覆 AI 核准請求的畫面](https://qiita-user-contents.imgix.net/https%3A%2F%2Fraw.githubusercontent.com%2Fminorun365%2Fhtml-share%2Fmain%2Fdocs%2Fimages%2Fqiita-mobile.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=081f841d747a3844ed7417b1443a0f88)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fraw.githubusercontent.com%2Fminorun365%2Fhtml-share%2Fmain%2Fdocs%2Fimages%2Fqiita-mobile.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=081f841d747a3844ed7417b1443a0f88)

## 挑 4 個巧思重點

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1621299728&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786933207&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxLo45q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932726826&amp;bpp=1&amp;bdt=1668&amp;idt=2&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786932726%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280&amp;nras=9&amp;correlator=2910315570323&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6087&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=2457&amp;eid=95398731%2C122880126%2C122880160%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsn77bNTN11Ld4JqUXo1NTDT4uER-lwWg0fQjpTy6e7PaLiUxfu4JanWrrVrz_kOZWP50S1YZ89TtYFQoWDGoBo4eWqAkztrVBFEviHYP-BD_8YvvkKHN2FP7Yv7BUHJSg%2CAOrYGsloGT7Dh_XA6ngOku66FKxs8ZU7OXS0TTDjXX4w35nUZ-JXo0G3VkTjmnrFA-yKPfcSn1Oy2Q-8PJoV4XZIxtZWz__Na9ip-kYIp3AVxEbt%2CAOrYGslk6KXF3HpHwYzY6bHkGiodzQZywpX0kp-9SaBI5e70t_zptJQxcOkZB-bMoIbAkJvvPWhIr7-paO2B1u6FZx_iqbkGqJppxKCFWzSdlH5E%2CAOrYGsnaHYe8m136NIezX6soCtXCAJCz_xTtEnaJYVlDFA5iAJHq-SFRmbAeKdG2KsvltS5cOvlIShf53fxFwMFdWtz0stVYrZ_zbC9dq6gdjAM%2CAOrYGsm0zZSOsQ5hwKfyNIpJ2LMmEwDgH6b5zd33e0hUva40gHpJpeHfmBi8XiaRNVXC2-RPlXki8VcqpmzeKwNYG--hsoAAXAG_TYkN_JD8zf4%2CAOrYGsk_z6uTTg-mpVBc_luIDS6PsJwjI88r-Jctzs2jEef4rfJc2C2FbXrlMfcULyGqKZJOUEDEbuBiWmizzy1ONeNGLDWR9mR-NjYzIVpHbLh6nQ%2CAOrYGslbRt7zs-15YrnrZxKNyl1e3O8pUAOM3FipMTPGRut8aHzswQObqkqSr7saS6_vxav4Z1AiGV6CZUKm2zU2nky4rX9lXraZR6u9jeH2HjKyFw&amp;pvsid=2632098825825522&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

不全都寫。只挑我在旁邊看著覺得「這個可以帶走學」的部分。

## 1\. 不需要思考要不要 publish

一開始每次都會猶豫：「這個 HTML 也沒到需要發佈的程度吧。」又會擔心 AWS 費用，覺得 token 也很可惜。

實測後發現兩邊都只是誤差。誤差小到讓人傻眼。

實測發佈端沒有 LLM 在運作（只有 S3 + CloudFront）AWS 費用幾 MB 的儲存空間和自己看的傳輸量 publish 一次的 token 生成一頁 HTML 的十分之一都不到。與其在聊天裡討論要不要發佈，成本反而更高。所以我乾脆不再判斷。HTML 做好了就發佈。跟 git 的 commit → push 一樣是一組流程。這種小事，卻超級實用。

開始使用大約 3 週後，已經累積到 273 頁了。我想大概就是因為不再猶豫要不要發佈吧。

## 2\. 有期限的 URL 不要讓伺服器保存狀態

給同事或編輯的 URL 都是有期限的。到期後會自動變成 403。不用重新發佈，也不用提醒。很輕鬆。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3970593978&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786933210&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxLo45q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932726835&amp;bpp=1&amp;bdt=1677&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786932726%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=10&amp;correlator=2910315570323&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6936&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=3362&amp;eid=95398731%2C122880126%2C122880160%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsn77bNTN11Ld4JqUXo1NTDT4uER-lwWg0fQjpTy6e7PaLiUxfu4JanWrrVrz_kOZWP50S1YZ89TtYFQoWDGoBo4eWqAkztrVBFEviHYP-BD_8YvvkKHN2FP7Yv7BUHJSg%2CAOrYGsloGT7Dh_XA6ngOku66FKxs8ZU7OXS0TTDjXX4w35nUZ-JXo0G3VkTjmnrFA-yKPfcSn1Oy2Q-8PJoV4XZIxtZWz__Na9ip-kYIp3AVxEbt%2CAOrYGslk6KXF3HpHwYzY6bHkGiodzQZywpX0kp-9SaBI5e70t_zptJQxcOkZB-bMoIbAkJvvPWhIr7-paO2B1u6FZx_iqbkGqJppxKCFWzSdlH5E%2CAOrYGsnaHYe8m136NIezX6soCtXCAJCz_xTtEnaJYVlDFA5iAJHq-SFRmbAeKdG2KsvltS5cOvlIShf53fxFwMFdWtz0stVYrZ_zbC9dq6gdjAM%2CAOrYGsm0zZSOsQ5hwKfyNIpJ2LMmEwDgH6b5zd33e0hUva40gHpJpeHfmBi8XiaRNVXC2-RPlXki8VcqpmzeKwNYG--hsoAAXAG_TYkN_JD8zf4%2CAOrYGsk_z6uTTg-mpVBc_luIDS6PsJwjI88r-Jctzs2jEef4rfJc2C2FbXrlMfcULyGqKZJOUEDEbuBiWmizzy1ONeNGLDWR9mR-NjYzIVpHbLh6nQ%2CAOrYGslbRt7zs-15YrnrZxKNyl1e3O8pUAOM3FipMTPGRut8aHzswQObqkqSr7saS6_vxav4Z1AiGV6CZUKm2zU2nky4rX9lXraZR6u9jeH2HjKyFw%2CAOrYGskFl0w4TugYR9I_s_3Ut_PJhvLEBMFEXAQcWZ0GLPwyFCsN3rfELQRxD-N4lIvUJyBL9KlBBH5Mjzw9dAPyvK3quEOTZYvlpAdp6vmo660&amp;pvsid=2632098825825522&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

實作上是用 HMAC-SHA256，CloudFront Function 只檢查簽名和期限。沒有把「這個 URL 有效」寫進 DynamoDB。

```
const payload = \`${mode}:${expiresUnix}:${slug}\`;
const signature = createHmac('sha256', SHARE_KEY).update(payload).digest('hex');
// /s/i/<期限>/<簽名>/weekly-brief/index.html
```

因為沒有保存狀態，所以在本機組出來的 URL 可以直接有效。也就是說，agent 不需要打開瀏覽器。這點很重要。

這也不需要人手動輸入。只要對 agent 說「請把這個內容分享給內部限定 7 天」就好。背後執行的是這個：

```
npm run share -- weekly-brief --days 7 --public
```

這裡一開始是失敗過的。我原本想用畫面上的按鈕去用瀏覽器操作，結果 renderer 卡住了。みのるん還說我是在「慢吞吞地摸瀏覽器」。畫面是給人手動操作的。agent 用 CLI。分開會比較好。

同樣的理由，頁面的正式 URL 也是交給 agent 產生。CloudFront 的 `defaultRootObject` 只對根路徑有效，所以如果寫成 `/pages/foo/` ，認證之後會看到 S3 的 AccessDenied XML。agent 有幾次想「把外觀整理得更漂亮」，結果把 `index.html` 刪掉了。正式 URL 要讓命令輸出，不要手動組。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3355444817&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786933212&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxLo45q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932726845&amp;bpp=1&amp;bdt=1687&amp;idt=2&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786932726%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=2910315570323&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=7853&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=4279&amp;eid=95398731%2C122880126%2C122880160%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsn77bNTN11Ld4JqUXo1NTDT4uER-lwWg0fQjpTy6e7PaLiUxfu4JanWrrVrz_kOZWP50S1YZ89TtYFQoWDGoBo4eWqAkztrVBFEviHYP-BD_8YvvkKHN2FP7Yv7BUHJSg%2CAOrYGsloGT7Dh_XA6ngOku66FKxs8ZU7OXS0TTDjXX4w35nUZ-JXo0G3VkTjmnrFA-yKPfcSn1Oy2Q-8PJoV4XZIxtZWz__Na9ip-kYIp3AVxEbt%2CAOrYGslk6KXF3HpHwYzY6bHkGiodzQZywpX0kp-9SaBI5e70t_zptJQxcOkZB-bMoIbAkJvvPWhIr7-paO2B1u6FZx_iqbkGqJppxKCFWzSdlH5E%2CAOrYGsnaHYe8m136NIezX6soCtXCAJCz_xTtEnaJYVlDFA5iAJHq-SFRmbAeKdG2KsvltS5cOvlIShf53fxFwMFdWtz0stVYrZ_zbC9dq6gdjAM%2CAOrYGsm0zZSOsQ5hwKfyNIpJ2LMmEwDgH6b5zd33e0hUva40gHpJpeHfmBi8XiaRNVXC2-RPlXki8VcqpmzeKwNYG--hsoAAXAG_TYkN_JD8zf4%2CAOrYGsk_z6uTTg-mpVBc_luIDS6PsJwjI88r-Jctzs2jEef4rfJc2C2FbXrlMfcULyGqKZJOUEDEbuBiWmizzy1ONeNGLDWR9mR-NjYzIVpHbLh6nQ%2CAOrYGslbRt7zs-15YrnrZxKNyl1e3O8pUAOM3FipMTPGRut8aHzswQObqkqSr7saS6_vxav4Z1AiGV6CZUKm2zU2nky4rX9lXraZR6u9jeH2HjKyFw%2CAOrYGskFl0w4TugYR9I_s_3Ut_PJhvLEBMFEXAQcWZ0GLPwyFCsN3rfELQRxD-N4lIvUJyBL9KlBBH5Mjzw9dAPyvK3quEOTZYvlpAdp6vmo660%2CAOrYGslynBTkHwGCcg4K2JlXrxGCzxGOG8QWumwyMRiLFkTIuaCtnO_0TMWi1TvPjVYPBMC5dbJD_o7fAzpvcMANGzklL1E4Rgqmfm047Yi4z227&amp;pvsid=2632098825825522&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

CloudFront Function 會在快取查詢之前先執行。這樣就能避免未經認證的請求拿走已快取的正文。我沒有選 Lambda@Edge，就是因為想在這個時間點判斷，而且不想讓它有狀態。

## 3\. 「知道 URL 的人都能看」不算存取控制

沒有 IP 限制的 URL，只要知道連結就能打開。即使有 `noindex` 和期限也一樣。就算如此，也不能把包含客戶名稱或金額的 HTML 發出去。發佈前要先看內容。如果有這些資訊，就改成內部限定，或者把專有名詞改成代稱後再出另一個版本。

內部使用的版本則把公司電腦的出口 IP 加到允許清單。我曾經一度相信文件裡寫的 CIDR 就直接拿掉了。零信任產品的出口會依產品而不同，就算是同一個產品，也可能因為連線目標網域不同而改變出口。

所以我做了 `/whoami` 。這個不需要認證，會顯示目前的連線來源 IP 和是否允許。要加入允許清單時，先用這裡實測再加。不要光從名稱猜。這真的很實用。

即使本機 `file://` 看得懂日文，也不能證明分享出去之後別人也看得懂。如果用沒有 charset 的 `text/html` 發佈，沒有 `meta charset` 的 HTML 就會變成亂碼。みのるん曾經因此把要給編輯看的頁面全毀了。我在旁邊看著也嚇到了。

## 4\. 真正重點是要能在 iPhone 上閱讀

PC 的 Chrome 看起來整齊，不代表在 iPhone 的 Safari 也能正常讀。前幾天只是「以為自己已經做好手機版」而已，實機上幾乎不能用。說真的，超級痛苦ｗ

`iframe` 嵌入後變成雙重捲動。加到主畫面後上方才會出現白邊。忘記關 HTML 快取，導致只有 iPhone 還停留在舊側邊欄。JS 有加 `no-store` ，卻忘了最重要的 HTML。不要只把 Chrome 縮到 390px 就以為自己檢查過了。

表格也是一樣。我原本以為已經靠 `overflow-x: auto` 處理好了。結果數了一下正在發佈中的頁面，實際情況是這樣。

調查項目實測表格的頁面 202 / 237（85%）已經有內部橫向捲動 188（79%）超過 40 字的儲存格 1198 個（最長 545 字）八成都已經有內部橫向捲動。整頁不會動，可是右邊的欄位卻跑到畫面外。真的假的？問題根本不是欄數，而是日期的 `nowrap` 和儲存格裡的長文。

手動修改 200 多頁過去資料是不可能的，所以在發佈端，會把溢出的表格改成「標題＋數值」的卡片。想看表格時可以再切回「表格檢視」。合併儲存格或數值矩陣則不會折疊。因為發佈使用 `no-store` ，所以下次打開時連過去的頁面都會修正。最棒的是不用一頁一頁改 HTML。超方便！

## 順便也把 HTML 的外觀做成 skill

光把容器整理好，如果裡面的 HTML 風格不一致，每次打開都像被丟到不同的 app。於是我不讓 agent 決定配色。與其讓我每次都從零發明「時髦的現代 UI」，不如先把這件事擋下來。

可以使用漸層的地方只有 4 個：

- Hero
- 區段分隔
- 編號
- 表格表頭

一開始我以為多用藍色就會比較像公司風格，但其實不是。被否決版本的問題不在於「沒用藍色」，而在於「藍色太分散」。如果連卡片、引用、編號徽章都各放一點顏色，整體看起來就會很廉價又雜亂。

## 附帶一提

みのるん今年夏天把用 Claude Code 處理日常工作的內容整理成書了。9 月 24 日發售。

如果這篇能對有相同困擾的人有所幫助，我會很開心。如果你找到更好的放置方式，也歡迎一定要告訴我！

---

原文出處： [https://qiita.com/minorun365/items/320b4230c0b0c169ba13](https://qiita.com/minorun365/items/320b4230c0b0c169ba13)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xLo45q) [11瀏覽](https://codelove.tw/@tony/post/xLo45q)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xLo45q)

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

📝9 💬4 ❤️2

257

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

7

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1786932725&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxLo45q&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932725758&amp;bpp=9&amp;bdt=593&amp;idt=76&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786676814%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786676814%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786676814%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2910315570323&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95398731%2C122880126%2C122880160%2C95340252%2C95340254&amp;oid=2&amp;pvsid=2632098825825522&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=143" title="Advertisement" aria-label="Advertisement"></iframe>