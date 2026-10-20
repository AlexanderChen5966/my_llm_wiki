---
title: "當你不懂架構時如何使用 AI 建構：生存指南"
source: "https://codelove.tw/@tony/post/x8O8Vq"
author:
published:
created: 2026-09-02
description: "讓我描述一個你可能會有共鳴的時刻。你有了一個 App 的點子。你其實不知道怎麼把它做出來——真的不知道——但你打開了一個 AI 聊天視窗，描述你想要的東西，它就給了你能跑的程式碼。你執行了它。它 *真的能用*。那感覺像魔法，因為它就是魔法：一個你一個月前還寫不出來的東西，現在就在你的螢幕上跑起來..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月02日](https://codelove.tw/@tony/post/x8O8Vq "2026-09-02 09:05:09")

## 當你不懂架構時如何使用 AI 建構：生存指南

讓我描述一個你可能會有共鳴的時刻。

你有了一個 App 的點子。你其實不知道怎麼把它做出來——真的不知道——但你打開了一個 AI 聊天視窗，描述你想要的東西，它就給了你能跑的程式碼。你執行了它。它 *真的能用* 。那感覺像魔法，因為它就是魔法：一個你一個月前還寫不出來的東西，現在就在你的螢幕上跑起來了。

於是你又加了一個功能。再加一個。到了第五個左右，事情開始壞掉了。你修好一個地方，另外兩個地方又不能用了。你請 AI 幫忙，但你甚至沒辦法清楚說明哪裡出錯，因為你自己也已經看不懂這個專案了。每一次修改，都像在黑暗中拆炸彈。

那道牆——不是寫程式本身——才是大多數人卡住的地方。而有件事沒人會告訴你： **AI 降低了「撰寫」程式碼的門檻，但沒有降低「組織」程式碼的門檻。** 這是兩種不同的能力，而現在決定你能不能繼續做下去、還是只能眼睜睜看著專案崩掉的，就是第二種。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788316238&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315496079&amp;bpp=3&amp;bdt=1132&amp;idt=3&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788315495%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905&amp;nras=3&amp;correlator=7406103276324&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1019&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630&amp;oid=2&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這是一份專門為那道牆準備的生存指南。不需要電腦科學學位。你只需要幾個習慣，就能避免 AI 做出來的專案變成義大利麵——這是給從零開始的人看的。

---

## 真正的問題：你生成程式碼的速度，已經超過你理解它的速度

在講怎麼做之前，先理解到底哪裡出了問題，因為這跟新手想的不一樣。

問題幾乎從來不是 AI 寫了「很爛」的程式碼。真正的問題是，AI 讓你在 **還沒理解整個系統怎麼連在一起之前** ，就先做出了一個可以運作的 App。你像是在駕駛一架看不到控制台的飛機。只要自動駕駛還穩著，你就沒事；但只要有地方需要調整，你就會立刻迷路——因為你從來沒學過控制項在哪裡。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788316238&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315496099&amp;bpp=1&amp;bdt=1153&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788315495%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280&amp;nras=4&amp;correlator=7406103276324&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1668&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630&amp;oid=2&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

「架構」聽起來像很可怕、很進階、很電腦科學的東西。其實不是。用白話說，架構其實就是 **你的專案長什麼樣子** ——有哪些部分、每個部分做什麼、它們怎麼彼此連接。結構才是讓專案 *看得懂* 的關鍵。能看懂，你才有辦法繼續做，而不是被淹沒。

所以，這整份指南其實只有一個目標： **在專案變大的過程中，讓你仍然能看懂自己的專案。** 下面所有內容都是為了這件事。

---

## 生存技能 #1：把各個部分分開

這是最重要的習慣，所以要特別重視。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788316238&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315496109&amp;bpp=1&amp;bdt=1163&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788315495%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280&amp;nras=5&amp;correlator=7406103276324&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2318&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630&amp;oid=2&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

如果放任不管，AI 常常會把所有東西都塞進同一個超大檔案裡——畫面顯示的部分、真正執行工作的部分、以及跟資料庫溝通的部分，全都糾纏在一起。一開始它可以正常運作。但當所有東西都放在同一處時， *任何* 修改都可能讓 *全部* 壞掉，因為沒有任何東西是彼此分開的。

解法是把三種東西分開：

- **使用者看到的內容** ——介面、按鈕、版面配置（通常稱為「UI」或「前端」）。
- **App 實際做的事** ——真正的邏輯、規則、計算（「商業邏輯」）。
- **資料從哪裡來** ——跟資料庫或外部服務溝通的部分（「資料層」）。

當這些部分分開之後，你就可以改變某個東西 *長什麼樣子* ，而不碰它 *實際做什麼* ；也可以改變它 *實際做什麼* ，而不破壞它跟 *資料* 的溝通方式。問題會被限制在局部，而不是一路擴散。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788316238&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315496114&amp;bpp=1&amp;bdt=1168&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788315495%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=7406103276324&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2986&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630&amp;oid=2&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**可以貼給 AI 的指令：** *「請把介面、商業邏輯和資料存取分開成不同檔案，不要混在一起。」*

這一句話只要持續使用，就能避免比這份指南其他任何內容都多的崩壞。

---

## 生存技能 #2：一個東西只做一件事

這條規則簡單到你可以直接記在腦中： **一個檔案或一個函式，應該只做一件你能用一句話說清楚的事。**

如果你在描述某個檔案的功能時，必須說「它做這個、還做那個、而且還做別的」——那它做太多事了，之後它就會變成 bug 躲藏、修改出錯的地方。

小而且用途單一的部分是可生存的。你找得到、看得懂，也能安心改其中一個。那種什麼都做的大檔案就像流沙：它裝得越多，每次編輯就越像在賭博。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788316241&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315496122&amp;bpp=1&amp;bdt=1176&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788315495%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=8&amp;correlator=7406103276324&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3857&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=274&amp;eid=95399630&amp;oid=2&amp;psts=AOrYGslvnjZoQLu-Zym66gXGQVP84uLu9BLq4W9lHbUQS3C_z_gnKHPdv-Fw-OeCy5wT-H7YncQ2f-odA-XHml7GKT4_3qn06g0x1fDEVFa1utmGa4JJ7iEslmM7Gki2O6zNdf5Q%2CAOrYGsk0gJ-HpVnTGziWFcH1lwpTcmHhk3_5FYp8sEqb6qT-KOBu3p4rZ0V_nfbR8Ir8gSk81_N0HsJDpqVg0AAhLRLp7_uUUryVd-m6HiXxv5bHhQ%2CAOrYGsmhUKs8emeHyONB9hRUQyetFMqcZDBp2w2tNNpSAE3M9t1TteoR-pZYScblj-BLp8QudOAvJEQ8tC-MUxZyt7b3hsjYfxOCuKFPdErM-3slUi8%2CAOrYGskVRiVtWWlU706zc80FOaRZd3i7FzL6a5OzjXdwUvM2Ju9sy4Ijo3tpkwdaJPTbBXaeDwE4_QGy2C_HIkbcYe012BZ2oeGS1yhS04LK0XqZjls%2CAOrYGsm1yDe7iP8FbHlfx4fEBgHPkrNYFUL2sP5TFim33ynNwmwH8cMDSA9WwDCiYttmRvX8Q7OSxaQT0ojUWZu7OeRZfBHiP0l3KTZdjneEu1lUzew%2CAOrYGskHZM5Vivw_EicYfAKYO7X-UabizGFP-ItL1_VJLq4aadOShqB2LzkEiL2nSzmCelLLFrppvWRFXc_-kc0lLlhdx48X-5sIDpt8sXioiDwjB_Y&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**可以貼給 AI 的指令：** *「每個檔案和函式都應該只有一個清楚的責任。如果有做超過一件事的內容，請拆開。」*

---

## 生存技能 #3：每一份資料只放一個地方

這是新手專案最容易壞掉的方式，所以要特別注意。

隨著你的 App 長大，同一份資訊——例如登入使用者、購物車裡的專案，等等——可能會被 **複製並在好幾個地方各自追蹤** 。接著這些複本就開始不同步了。App 的一部分以為購物車有三個專案，另一部分卻以為只有一個，你的 App 會開始出現一些完全說不通、而且幾乎不可能除錯的行為。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=4060917935&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788316252&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315496126&amp;bpp=1&amp;bdt=1180&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788315495%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=9&amp;correlator=7406103276324&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4383&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=778&amp;eid=95399630&amp;oid=2&amp;psts=AOrYGslvnjZoQLu-Zym66gXGQVP84uLu9BLq4W9lHbUQS3C_z_gnKHPdv-Fw-OeCy5wT-H7YncQ2f-odA-XHml7GKT4_3qn06g0x1fDEVFa1utmGa4JJ7iEslmM7Gki2O6zNdf5Q%2CAOrYGsk0gJ-HpVnTGziWFcH1lwpTcmHhk3_5FYp8sEqb6qT-KOBu3p4rZ0V_nfbR8Ir8gSk81_N0HsJDpqVg0AAhLRLp7_uUUryVd-m6HiXxv5bHhQ%2CAOrYGsmhUKs8emeHyONB9hRUQyetFMqcZDBp2w2tNNpSAE3M9t1TteoR-pZYScblj-BLp8QudOAvJEQ8tC-MUxZyt7b3hsjYfxOCuKFPdErM-3slUi8%2CAOrYGskVRiVtWWlU706zc80FOaRZd3i7FzL6a5OzjXdwUvM2Ju9sy4Ijo3tpkwdaJPTbBXaeDwE4_QGy2C_HIkbcYe012BZ2oeGS1yhS04LK0XqZjls%2CAOrYGsm1yDe7iP8FbHlfx4fEBgHPkrNYFUL2sP5TFim33ynNwmwH8cMDSA9WwDCiYttmRvX8Q7OSxaQT0ojUWZu7OeRZfBHiP0l3KTZdjneEu1lUzew%2CAOrYGskHZM5Vivw_EicYfAKYO7X-UabizGFP-ItL1_VJLq4aadOShqB2LzkEiL2nSzmCelLLFrppvWRFXc_-kc0lLlhdx48X-5sIDpt8sXioiDwjB_Y%2CAOrYGsnrUymDH4zOfSxXcmXCDAuIugl8ZXx2Y1Rz53J7rrV5zGcrNQbq1-d_s4W38qoVJ7p17E1vzLAdiCE-3LSh0djfZPvSUdd4e91uqBPNdSOwdQ&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

生存原則是： **每一份資料只存在於一個地方，其他所有地方都從那裡讀取。** 這叫做單一事實來源（single source of truth）。不要讓 AI 隨便在專案各處複製你的狀態。

**可以貼給 AI 的指令：** *「這份資料應該只有一個單一事實來源。不要在不同元件之間重複複製，所有地方都從同一處讀取。」*

這條能防止最讓人抓狂的 bug 類型：技術上沒有壞，但所有東西都對不起來。

---

## 生存技能 #4：先決定形狀，再開始生成

大多數新手的做法是這樣：想到一個功能，就叫 AI 幫忙做，然後重複。AI 每次都即興產生結構，但各個部分根本接不起來——因為從來沒有人先決定它們應該怎麼長。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1106437894&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788317770&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315496131&amp;bpp=1&amp;bdt=1234&amp;idt=0&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788315495%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280&amp;nras=10&amp;correlator=7406103276324&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5084&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1467&amp;eid=95399630&amp;oid=2&amp;psts=AOrYGslvnjZoQLu-Zym66gXGQVP84uLu9BLq4W9lHbUQS3C_z_gnKHPdv-Fw-OeCy5wT-H7YncQ2f-odA-XHml7GKT4_3qn06g0x1fDEVFa1utmGa4JJ7iEslmM7Gki2O6zNdf5Q%2CAOrYGsk0gJ-HpVnTGziWFcH1lwpTcmHhk3_5FYp8sEqb6qT-KOBu3p4rZ0V_nfbR8Ir8gSk81_N0HsJDpqVg0AAhLRLp7_uUUryVd-m6HiXxv5bHhQ%2CAOrYGsmhUKs8emeHyONB9hRUQyetFMqcZDBp2w2tNNpSAE3M9t1TteoR-pZYScblj-BLp8QudOAvJEQ8tC-MUxZyt7b3hsjYfxOCuKFPdErM-3slUi8%2CAOrYGskVRiVtWWlU706zc80FOaRZd3i7FzL6a5OzjXdwUvM2Ju9sy4Ijo3tpkwdaJPTbBXaeDwE4_QGy2C_HIkbcYe012BZ2oeGS1yhS04LK0XqZjls%2CAOrYGsm1yDe7iP8FbHlfx4fEBgHPkrNYFUL2sP5TFim33ynNwmwH8cMDSA9WwDCiYttmRvX8Q7OSxaQT0ojUWZu7OeRZfBHiP0l3KTZdjneEu1lUzew%2CAOrYGskHZM5Vivw_EicYfAKYO7X-UabizGFP-ItL1_VJLq4aadOShqB2LzkEiL2nSzmCelLLFrppvWRFXc_-kc0lLlhdx48X-5sIDpt8sXioiDwjB_Y%2CAOrYGsnrUymDH4zOfSxXcmXCDAuIugl8ZXx2Y1Rz53J7rrV5zGcrNQbq1-d_s4W38qoVJ7p17E1vzLAdiCE-3LSh0djfZPvSUdd4e91uqBPNdSOwdQ%2CAOrYGsm39ym0rs0Bg9eCa9QHofi8vTodyy4KFT3uOR-62C5JNLlNYojd45KjhBubxuWRNj0t83oM5x-Gv0rRWfA6asWuqGJS2tlcsGYe3Px3HXdabvI&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

真正能改變一切的做法是： **在開始做之前，先把 App 的各個部分命名出來。**

在生成程式碼之前，先寫下主要部分，以及每個部分的責任。對一個簡單的 App 來說，可能是： *登入、使用者個人檔案、主儀表板、付款。* 四個部分，每個都有明確工作。那份清單——事先知道你的各個部分，以及它們各自負責什麼——用白話說，就是架構。你其實已經在做了，並沒有那麼可怕。

然後一次只做 **一個部分** ，完整做好，再進下一個。不要叫 AI 一次生成整個 App——你只會得到一團你看不懂的糾結。先做登入，讓它能跑，理解它，再做下一塊。

**可以貼給 AI 的指令：** *「這是我的 App 主要元件，以及每個元件的用途：［你的清單］。請一次做一個部分，先從［X］開始。」*

---

## 生存技能 #5：讓 AI 解釋它自己

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3034205192&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788317787&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315496134&amp;bpp=1&amp;bdt=1238&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788315495%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=7406103276324&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5821&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2206&amp;eid=95399630&amp;oid=2&amp;psts=AOrYGslvnjZoQLu-Zym66gXGQVP84uLu9BLq4W9lHbUQS3C_z_gnKHPdv-Fw-OeCy5wT-H7YncQ2f-odA-XHml7GKT4_3qn06g0x1fDEVFa1utmGa4JJ7iEslmM7Gki2O6zNdf5Q%2CAOrYGsk0gJ-HpVnTGziWFcH1lwpTcmHhk3_5FYp8sEqb6qT-KOBu3p4rZ0V_nfbR8Ir8gSk81_N0HsJDpqVg0AAhLRLp7_uUUryVd-m6HiXxv5bHhQ%2CAOrYGsmhUKs8emeHyONB9hRUQyetFMqcZDBp2w2tNNpSAE3M9t1TteoR-pZYScblj-BLp8QudOAvJEQ8tC-MUxZyt7b3hsjYfxOCuKFPdErM-3slUi8%2CAOrYGskVRiVtWWlU706zc80FOaRZd3i7FzL6a5OzjXdwUvM2Ju9sy4Ijo3tpkwdaJPTbBXaeDwE4_QGy2C_HIkbcYe012BZ2oeGS1yhS04LK0XqZjls%2CAOrYGsm1yDe7iP8FbHlfx4fEBgHPkrNYFUL2sP5TFim33ynNwmwH8cMDSA9WwDCiYttmRvX8Q7OSxaQT0ojUWZu7OeRZfBHiP0l3KTZdjneEu1lUzew%2CAOrYGskHZM5Vivw_EicYfAKYO7X-UabizGFP-ItL1_VJLq4aadOShqB2LzkEiL2nSzmCelLLFrppvWRFXc_-kc0lLlhdx48X-5sIDpt8sXioiDwjB_Y%2CAOrYGsnrUymDH4zOfSxXcmXCDAuIugl8ZXx2Y1Rz53J7rrV5zGcrNQbq1-d_s4W38qoVJ7p17E1vzLAdiCE-3LSh0djfZPvSUdd4e91uqBPNdSOwdQ%2CAOrYGsm39ym0rs0Bg9eCa9QHofi8vTodyy4KFT3uOR-62C5JNLlNYojd45KjhBubxuWRNj0t83oM5x-Gv0rRWfA6asWuqGJS2tlcsGYe3Px3HXdabvI%2CAOrYGsnuq1pVmDog0AXyd4vcZqhxIfa_iIom-MjKgijbeySes5RmC7mcuF_W44uouTDzYUW4RQxmA48ibx0j4zvKWeNLBkdZz0Iz266hc901N4Nc2A&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這個習慣會把「做東西」變成「學東西」——而且它會慢慢把新手變成真的懂結構的人。

不要只是接受 AI 給你的程式碼然後往下走。你要問它：

- 「你為什麼要這樣設計結構？」
- 「每個部分用白話來說是在做什麼？」
- 「如果我改這裡，還會影響哪些地方？」

你是在把 AI 當成 **家教** ，不是自動販賣機。每一次回答，都會讓你更了解你的專案是怎麼拼起來的——而這正是你一開始所缺少的知識。如果持續這樣做，隨著一個又一個專案累積，你就不再只是會 *生成* 程式碼的人，而是會 *理解* 程式碼的人。這種理解才是整個遊戲的核心。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2765481581&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788317792&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315496139&amp;bpp=1&amp;bdt=1243&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788315495%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=12&amp;correlator=7406103276324&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6453&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2853&amp;eid=95399630&amp;oid=2&amp;psts=AOrYGslvnjZoQLu-Zym66gXGQVP84uLu9BLq4W9lHbUQS3C_z_gnKHPdv-Fw-OeCy5wT-H7YncQ2f-odA-XHml7GKT4_3qn06g0x1fDEVFa1utmGa4JJ7iEslmM7Gki2O6zNdf5Q%2CAOrYGsk0gJ-HpVnTGziWFcH1lwpTcmHhk3_5FYp8sEqb6qT-KOBu3p4rZ0V_nfbR8Ir8gSk81_N0HsJDpqVg0AAhLRLp7_uUUryVd-m6HiXxv5bHhQ%2CAOrYGsmhUKs8emeHyONB9hRUQyetFMqcZDBp2w2tNNpSAE3M9t1TteoR-pZYScblj-BLp8QudOAvJEQ8tC-MUxZyt7b3hsjYfxOCuKFPdErM-3slUi8%2CAOrYGskVRiVtWWlU706zc80FOaRZd3i7FzL6a5OzjXdwUvM2Ju9sy4Ijo3tpkwdaJPTbBXaeDwE4_QGy2C_HIkbcYe012BZ2oeGS1yhS04LK0XqZjls%2CAOrYGsm1yDe7iP8FbHlfx4fEBgHPkrNYFUL2sP5TFim33ynNwmwH8cMDSA9WwDCiYttmRvX8Q7OSxaQT0ojUWZu7OeRZfBHiP0l3KTZdjneEu1lUzew%2CAOrYGskHZM5Vivw_EicYfAKYO7X-UabizGFP-ItL1_VJLq4aadOShqB2LzkEiL2nSzmCelLLFrppvWRFXc_-kc0lLlhdx48X-5sIDpt8sXioiDwjB_Y%2CAOrYGsnrUymDH4zOfSxXcmXCDAuIugl8ZXx2Y1Rz53J7rrV5zGcrNQbq1-d_s4W38qoVJ7p17E1vzLAdiCE-3LSh0djfZPvSUdd4e91uqBPNdSOwdQ%2CAOrYGsm39ym0rs0Bg9eCa9QHofi8vTodyy4KFT3uOR-62C5JNLlNYojd45KjhBubxuWRNj0t83oM5x-Gv0rRWfA6asWuqGJS2tlcsGYe3Px3HXdabvI%2CAOrYGsnuq1pVmDog0AXyd4vcZqhxIfa_iIom-MjKgijbeySes5RmC7mcuF_W44uouTDzYUW4RQxmA48ibx0j4zvKWeNLBkdZz0Iz266hc901N4Nc2A%2CAOrYGsnDnbE69JwhHED-wvn9rToDUry2e6V6YDizrkXGvQpIbRvYnPZSl92bGrRN8efgwG_q8pZ75Dg9NP9MOksaHFViCWnlqlEcQcJb3k-qo7W8TDI&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=10&amp;uci=a!a&amp;btvi=11&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**可以貼給 AI 的指令：** *「在我使用之前，請用簡單的方式解釋它的結構和原因——我想要理解它，不只是讓它能跑。」*

---

## 生存技能 #6：保持無聊且一致

這兩個習慣要放在一起，因為它們是搭配使用的。

**無聊比聰明更好。** 當 AI 提出一個很炫、很複雜、看起來很厲害的解法時，先問自己：有沒有更簡單的版本？對一個還不是架構師的人來說，花俏的版本不是優勢，而是負擔，因為你不理解的複雜度，就是你無法維護或修復的複雜度。簡單、直觀的版本，才是你下個月還能繼續使用的版本。永遠優先選它。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2827335803&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788317799&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315496145&amp;bpp=1&amp;bdt=1249&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788315495%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=13&amp;correlator=7406103276324&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=7103&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=3491&amp;eid=95399630&amp;oid=2&amp;psts=AOrYGslvnjZoQLu-Zym66gXGQVP84uLu9BLq4W9lHbUQS3C_z_gnKHPdv-Fw-OeCy5wT-H7YncQ2f-odA-XHml7GKT4_3qn06g0x1fDEVFa1utmGa4JJ7iEslmM7Gki2O6zNdf5Q%2CAOrYGsk0gJ-HpVnTGziWFcH1lwpTcmHhk3_5FYp8sEqb6qT-KOBu3p4rZ0V_nfbR8Ir8gSk81_N0HsJDpqVg0AAhLRLp7_uUUryVd-m6HiXxv5bHhQ%2CAOrYGsmhUKs8emeHyONB9hRUQyetFMqcZDBp2w2tNNpSAE3M9t1TteoR-pZYScblj-BLp8QudOAvJEQ8tC-MUxZyt7b3hsjYfxOCuKFPdErM-3slUi8%2CAOrYGskVRiVtWWlU706zc80FOaRZd3i7FzL6a5OzjXdwUvM2Ju9sy4Ijo3tpkwdaJPTbBXaeDwE4_QGy2C_HIkbcYe012BZ2oeGS1yhS04LK0XqZjls%2CAOrYGsm1yDe7iP8FbHlfx4fEBgHPkrNYFUL2sP5TFim33ynNwmwH8cMDSA9WwDCiYttmRvX8Q7OSxaQT0ojUWZu7OeRZfBHiP0l3KTZdjneEu1lUzew%2CAOrYGskHZM5Vivw_EicYfAKYO7X-UabizGFP-ItL1_VJLq4aadOShqB2LzkEiL2nSzmCelLLFrppvWRFXc_-kc0lLlhdx48X-5sIDpt8sXioiDwjB_Y%2CAOrYGsnrUymDH4zOfSxXcmXCDAuIugl8ZXx2Y1Rz53J7rrV5zGcrNQbq1-d_s4W38qoVJ7p17E1vzLAdiCE-3LSh0djfZPvSUdd4e91uqBPNdSOwdQ%2CAOrYGsm39ym0rs0Bg9eCa9QHofi8vTodyy4KFT3uOR-62C5JNLlNYojd45KjhBubxuWRNj0t83oM5x-Gv0rRWfA6asWuqGJS2tlcsGYe3Px3HXdabvI%2CAOrYGsnuq1pVmDog0AXyd4vcZqhxIfa_iIom-MjKgijbeySes5RmC7mcuF_W44uouTDzYUW4RQxmA48ibx0j4zvKWeNLBkdZz0Iz266hc901N4Nc2A%2CAOrYGsnDnbE69JwhHED-wvn9rToDUry2e6V6YDizrkXGvQpIbRvYnPZSl92bGrRN8efgwG_q8pZ75Dg9NP9MOksaHFViCWnlqlEcQcJb3k-qo7W8TDI%2CAOrYGsm6OlvEU8zPMGhBXvPzvj6M3QG9Wj4la9i7u48SIIfOTC10SJiiPw7_GID8xpQpoJkrt_44E9agyvaEPpog66rGRCVWt6bRBzvELIXFI7TGCOs&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=11&amp;uci=a!b&amp;btvi=12&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**一致比多樣更好。** 如果你做功能 A 用一種方式、做功能 B 又用完全不同的方式（這在分開的 AI 對話中很容易發生），你的專案就會變成拼布，每個部分都不遵循同樣規則，整體看起來混亂不堪。讓新功能跟現有功能的模式一致。

**可以貼給 AI 的指令：** *「請給我能運作的最簡單版本，不要最聰明的版本。而且請符合我專案裡既有的結構與模式。」*

---

## 在太晚之前，先認得警訊

這裡是你的煙霧警報。當你開始有以下任何感覺時， **先停止加功能** ，先整理結構：

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2635710651&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788317803&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315496148&amp;bpp=1&amp;bdt=1252&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788315495%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788315495%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=14&amp;correlator=7406103276324&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=7752&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=4135&amp;eid=95399630&amp;oid=2&amp;psts=AOrYGslvnjZoQLu-Zym66gXGQVP84uLu9BLq4W9lHbUQS3C_z_gnKHPdv-Fw-OeCy5wT-H7YncQ2f-odA-XHml7GKT4_3qn06g0x1fDEVFa1utmGa4JJ7iEslmM7Gki2O6zNdf5Q%2CAOrYGsk0gJ-HpVnTGziWFcH1lwpTcmHhk3_5FYp8sEqb6qT-KOBu3p4rZ0V_nfbR8Ir8gSk81_N0HsJDpqVg0AAhLRLp7_uUUryVd-m6HiXxv5bHhQ%2CAOrYGsmhUKs8emeHyONB9hRUQyetFMqcZDBp2w2tNNpSAE3M9t1TteoR-pZYScblj-BLp8QudOAvJEQ8tC-MUxZyt7b3hsjYfxOCuKFPdErM-3slUi8%2CAOrYGskVRiVtWWlU706zc80FOaRZd3i7FzL6a5OzjXdwUvM2Ju9sy4Ijo3tpkwdaJPTbBXaeDwE4_QGy2C_HIkbcYe012BZ2oeGS1yhS04LK0XqZjls%2CAOrYGsm1yDe7iP8FbHlfx4fEBgHPkrNYFUL2sP5TFim33ynNwmwH8cMDSA9WwDCiYttmRvX8Q7OSxaQT0ojUWZu7OeRZfBHiP0l3KTZdjneEu1lUzew%2CAOrYGskHZM5Vivw_EicYfAKYO7X-UabizGFP-ItL1_VJLq4aadOShqB2LzkEiL2nSzmCelLLFrppvWRFXc_-kc0lLlhdx48X-5sIDpt8sXioiDwjB_Y%2CAOrYGsnrUymDH4zOfSxXcmXCDAuIugl8ZXx2Y1Rz53J7rrV5zGcrNQbq1-d_s4W38qoVJ7p17E1vzLAdiCE-3LSh0djfZPvSUdd4e91uqBPNdSOwdQ%2CAOrYGsm39ym0rs0Bg9eCa9QHofi8vTodyy4KFT3uOR-62C5JNLlNYojd45KjhBubxuWRNj0t83oM5x-Gv0rRWfA6asWuqGJS2tlcsGYe3Px3HXdabvI%2CAOrYGsnuq1pVmDog0AXyd4vcZqhxIfa_iIom-MjKgijbeySes5RmC7mcuF_W44uouTDzYUW4RQxmA48ibx0j4zvKWeNLBkdZz0Iz266hc901N4Nc2A%2CAOrYGsnDnbE69JwhHED-wvn9rToDUry2e6V6YDizrkXGvQpIbRvYnPZSl92bGrRN8efgwG_q8pZ75Dg9NP9MOksaHFViCWnlqlEcQcJb3k-qo7W8TDI%2CAOrYGsm6OlvEU8zPMGhBXvPzvj6M3QG9Wj4la9i7u48SIIfOTC10SJiiPw7_GID8xpQpoJkrt_44E9agyvaEPpog66rGRCVWt6bRBzvELIXFI7TGCOs%2CAOrYGsmh1TAMMSoBMVYcEb28Fvka9vjh2cL88HjztqIicmzfUQKwKyLjAXgyIbErpC4hXC8xMAfMhLDQD1y7NUGBQKZ4yPsn6l7nHlo5amUwBuE&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=12&amp;uci=a!c&amp;btvi=13&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

- 你因為不知道會壞掉什麼而 **害怕修改** 。
- 一個修正一直 **連帶弄壞別的東西** ，反覆發生。
- 你再也 **找不到** 任何東西在哪裡。
- 檔案一直 **越長越大** ，而且做的事情越來越多。
- 當你看著自己的專案時， **完全看不懂** 。

這些都不代表你失敗了。它們只代表你的專案已經長大到超出原本的結構——這種事每個人都會遇到，包括專業人士。差別在於，現在你知道這種感覺代表什麼，也知道該怎麼做：先停一下，把各個部分分開，在繼續往上堆東西之前，先把理解找回來。

---

## 結論

你不需要電腦科學學位，也能用 AI 做出真正的軟體。那道門檻確實已經消失了，而且不會再回來。

但「我可以生成程式碼」和「我可以做出能承受自己成長的東西」是兩回事，而兩者之間的差距就是 **結構** ——也就是 AI 無法替你提供的那一部分，因為它取決於你這個專案的決策，而只有你能做那些決策。AI 給你程式碼；你必須給它形狀。

好訊息是，這個形狀並不難。它只是一組習慣：把各部分分開、每個東西只做一件事、每份資料只有一個家、先規劃再生成、讓 AI 教你、保持簡單。只要這樣做，並且在過程中持續理解你正在打造的東西，你能做出的成果會比你想像的多得多——而且不會看著它崩塌。

從簡單開始。讓自己始終看得懂它。這就是生存。

---

原文出處： [https://dev.to/james\_anderson\_h/building-with-ai-when-you-dont-know-architecture-a-survival-guide-1ma3](https://dev.to/james_anderson_h/building-with-ai-when-you-dont-know-architecture-a-survival-guide-1ma3)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/x8O8Vq) [8瀏覽](https://codelove.tw/@tony/post/x8O8Vq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/x8O8Vq)

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

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝16 💬4

227

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788315495&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8O8Vq&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY1Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NSJdXSwwXQ..&amp;dt=1788315495056&amp;bpp=10&amp;bdt=110&amp;idt=117&amp;shv=r20260901&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788310688%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788310688%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788310688%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=7406103276324&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=990&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630&amp;oid=2&amp;pvsid=3027119091864651&amp;tmod=668923192&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=166" title="Advertisement" aria-label="Advertisement"></iframe>