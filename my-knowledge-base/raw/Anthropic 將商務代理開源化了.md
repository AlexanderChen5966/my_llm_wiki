---
title: "Anthropic 將商務代理開源化了"
source: "https://codelove.tw/@tony/post/apoBm3#google_vignette"
author:
published:
created: 2026-09-04
description: "9月2日，Anthropic 公開了 **[anthropics/commerce-agents](https://github.com/anthropics/commerce-agents)**。採用的是 **Apache 2.0** 授權。這是一份「用 Claude 製作購物代理時的參考實作..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月03日](https://codelove.tw/@tony/post/apoBm3 "2026-09-03 17:15:09")

9月2日，Anthropic 公開了 **[anthropics/commerce-agents](https://github.com/anthropics/commerce-agents)** 。採用的是 **Apache 2.0** 授權。

這是一份「用 Claude 製作購物代理時的參考實作」，而且 **可直接執行的程式碼都已經放進去了。** 我整理一下裡面有哪些內容。

> 驗證環境：Windows 10 / Python 3.11.9 / Node 22.19 / 測量截至 2026-09-03

---

## 代理有 2 種

誰來用｜能做什麼  
**Shopping Agent** ｜購物者｜目錄搜尋、商品比較、加入購物車、訂單追蹤／退貨 Q&A  
**Merchant Agent** ｜店家人員｜銷售分析、庫存警示、價格／促銷提案、活動草案

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788503502&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoBm3%23google_vignette&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjYiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY2Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NiJdXSwwXQ..&amp;dt=1788503502791&amp;bpp=1&amp;bdt=774&amp;idt=-M&amp;shv=r20260902&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788503502%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250&amp;nras=2&amp;correlator=1905677708056&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1016&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400775%2C95401045%2C122880756%2C31061691%2C31061692&amp;oid=2&amp;psts=AOrYGsmkcJpVWZ8dYMRj2zFln4uXHXCfTBqP78O1jPKWMTiDDYxwOmFNRjF6AGSj7H_scSm-LWhd0r5CJBz2AIgX9wbxTPD2SXTUhhNJORhWjQltjA&amp;pvsid=2889163926097059&amp;tmod=2031534224&amp;uas=0&amp;nvt=2&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=48" title="Advertisement" aria-label="Advertisement"></iframe>

特色是 **前台與後台兩邊都包含了。** 不只是給購物者用，還附上了 **店家管理介面用的代理** 。

---

## 內含 4 種產業的實作

用相同骨架，提供了 4 種不同產業版本。

```
retail         ACME Store     API :8000  /  店面 :3000
travel         ACME Travel    API :8001
telecom        ACME Mobile    API :8002
entertainment  ACME Tickets   API :8003
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788503502&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoBm3%23google_vignette&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjYiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY2Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NiJdXSwwXQ..&amp;dt=1788503502791&amp;bpp=1&amp;bdt=775&amp;idt=-M&amp;shv=r20260902&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788503502%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280&amp;nras=3&amp;correlator=1905677708056&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1655&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400775%2C95401045%2C122880756%2C31061691%2C31061692&amp;oid=2&amp;psts=AOrYGsmkcJpVWZ8dYMRj2zFln4uXHXCfTBqP78O1jPKWMTiDDYxwOmFNRjF6AGSj7H_scSm-LWhd0r5CJBz2AIgX9wbxTPD2SXTUhhNJORhWjQltjA&amp;pvsid=2889163926097059&amp;tmod=2031534224&amp;uas=0&amp;nvt=2&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=49" title="Advertisement" aria-label="Advertisement"></iframe>

`--merchant` 會切換到店家入口網站（:3100）， `--all` 則會兩邊一起啟動。

---

## 有 3 種執行方式

**這部分我覺得最有參考價值。** 同一個提示詞、同一套工具，卻用 **3 種不同的方式** 來撰寫。

執行形式｜差異  
**Messages API** ｜代理的迴圈由 **自己撰寫** ，作為參考實作  
**Agent SDK** ｜迴圈交給 **SDK** 處理  
**Managed Agents** ｜在主機端執行，呼叫 **自己的 MCP 伺服器**

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788503502&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoBm3%23google_vignette&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjYiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY2Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NiJdXSwwXQ..&amp;dt=1788503502791&amp;bpp=1&amp;bdt=775&amp;idt=-M&amp;shv=r20260902&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788503502%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280&amp;nras=4&amp;correlator=1905677708056&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2304&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400775%2C95401045%2C122880756%2C31061691%2C31061692&amp;oid=2&amp;psts=AOrYGsmkcJpVWZ8dYMRj2zFln4uXHXCfTBqP78O1jPKWMTiDDYxwOmFNRjF6AGSj7H_scSm-LWhd0r5CJBz2AIgX9wbxTPD2SXTUhhNJORhWjQltjA&amp;pvsid=2889163926097059&amp;tmod=2031534224&amp;uas=0&amp;nvt=2&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=49" title="Advertisement" aria-label="Advertisement"></iframe>

當你在猶豫「代理的迴圈該自己實作，還是交給 SDK」時， **可以把同一題材的 3 種實作拿來對照閱讀。**

---

## 規模

```
檔案        640
Python       213
TypeScript   202
Markdown      62
授權        Apache License 2.0
```

`requirements.txt` 會以 **editable** 方式安裝 7 個套件。這是從儲存庫內的目錄直接安裝的設計， **沒有註冊到 PyPI。**

---

## 官方列出的效果

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788503502&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoBm3%23google_vignette&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjYiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY2Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NiJdXSwwXQ..&amp;dt=1788503502791&amp;bpp=1&amp;bdt=774&amp;idt=-M&amp;shv=r20260902&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788503502%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=5&amp;correlator=1905677708056&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3109&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400775%2C95401045%2C122880756%2C31061691%2C31061692&amp;oid=2&amp;psts=AOrYGsmkcJpVWZ8dYMRj2zFln4uXHXCfTBqP78O1jPKWMTiDDYxwOmFNRjF6AGSj7H_scSm-LWhd0r5CJBz2AIgX9wbxTPD2SXTUhhNJORhWjQltjA&amp;pvsid=2889163926097059&amp;tmod=2031534224&amp;uas=0&amp;nvt=2&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=5&amp;fsb=1&amp;dtd=50" title="Advertisement" aria-label="Advertisement"></iframe>

> **Retailers** running shopping agents on Claude have seen carts **up to 35% larger** and shoppers **60% more likely** to complete a purchase.  
> （在 Claude 上運行購物代理的零售業者，看到購物車金額 **最高增加 35%** ，而消費者完成購買的機率 **提高 60%** ）

**這裡有寫 up to（最高／最多）。** 這不是平均值。由於母數與條件沒有公開，最好不要直接把這個數字套用到自家情境。

---

## 在 Windows 上執行時卡在三個地方

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788503503&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoBm3%23google_vignette&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjYiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY2Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NiJdXSwwXQ..&amp;dt=1788503502773&amp;bpp=1&amp;bdt=756&amp;idt=0&amp;shv=r20260902&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788503502%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124&amp;nras=7&amp;correlator=1905677708056&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3742&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=162&amp;eid=95399630%2C95400775%2C95401045%2C122880756%2C31061691%2C31061692&amp;oid=2&amp;psts=AOrYGsmkcJpVWZ8dYMRj2zFln4uXHXCfTBqP78O1jPKWMTiDDYxwOmFNRjF6AGSj7H_scSm-LWhd0r5CJBz2AIgX9wbxTPD2SXTUhhNJORhWjQltjA%2CAOrYGslVP6IktqQT2YcNgoFO0PKwhir5NSd6JjZ-ODqgIez8yMr6UrvF5f_erMIReRxy0PddpQbP7i68zTNM6X_XcBL6ZZRSNQ2gGGig0XzuIb0uJlfGe4GdDjj2akUixbxbmng%2CAOrYGslb47LRZXXWoacKb7V0dk2NhQt9c0bGGshsdFogtUn3f4NTeF5OiqKAf9TO5yZxd5WyCIxLAB5GPXt3VwDFZN4-rAgBqHN_dfQ4Mp7p3GY%2CAOrYGslFs1duG8asPUOdiL2K19dRuPUhN1_1ZGZxqbN5FuGJ4qao4p7QlpaFv8rLVpCWoGytoPJL2dQ8QvnaQwYl-D8s5fWia9uv2Hn6lLP1AFgL%2CAOrYGskBSulXoODZPZ-Fou9IWV_hTascu8RVx0VDqmD9IGHijr482QJVvhZoLxNHQ40H_QaCoqgcDHMdKLwb1xz_B9DvU7BbuQRvc9_JYkelo3s%2CAOrYGskjQcigo3q7oEH5RiGd0dQEl2UAMiAgQCKuK4PD0gsVaSnyUYVi_l7KKhMFYexw-UU8n7zoCwMab5CpErKulon7tlMrHkrGKmqfiVSIhcZfDw&amp;pvsid=2889163926097059&amp;tmod=2031534224&amp;uas=0&amp;nvt=2&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=7&amp;fsb=1&amp;dtd=812" title="Advertisement" aria-label="Advertisement"></iframe>

**README 的前提條件只有 Python 3.11+ / Node 22 / API 金鑰這 3 項** 。我本機都符合。 **README 與文件中完全沒有提到 Windows 或 WSL。**

結果， `python scripts/run_demo.py retail` 沒有跑完整。

**1\. 使用者名稱裡有日文時，會找不到套件。**

```
ModuleNotFoundError: No module named 'commerce_common'
```

editable install 產生的 `.pth` 檔會以 **UTF-8** 寫入。用十六進位檢視內容時，是 `e8ab8f e8a8aa` （=「諏訪」）。 **但 Python 讀取這個檔案時，使用的是作業系統的 locale 編碼。** 在日文 Windows 上是 `cp932` 。路徑因此亂掉而無法解析。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=4060917935&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788503510&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoBm3%23google_vignette&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjYiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY2Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NiJdXSwwXQ..&amp;dt=1788503502774&amp;bpp=1&amp;bdt=758&amp;idt=0&amp;shv=r20260902&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788503502%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C952x280&amp;nras=8&amp;correlator=1905677708056&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4411&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=858&amp;eid=95399630%2C95400775%2C95401045%2C122880756%2C31061691%2C31061692&amp;oid=2&amp;psts=AOrYGsmkcJpVWZ8dYMRj2zFln4uXHXCfTBqP78O1jPKWMTiDDYxwOmFNRjF6AGSj7H_scSm-LWhd0r5CJBz2AIgX9wbxTPD2SXTUhhNJORhWjQltjA%2CAOrYGslVP6IktqQT2YcNgoFO0PKwhir5NSd6JjZ-ODqgIez8yMr6UrvF5f_erMIReRxy0PddpQbP7i68zTNM6X_XcBL6ZZRSNQ2gGGig0XzuIb0uJlfGe4GdDjj2akUixbxbmng%2CAOrYGslb47LRZXXWoacKb7V0dk2NhQt9c0bGGshsdFogtUn3f4NTeF5OiqKAf9TO5yZxd5WyCIxLAB5GPXt3VwDFZN4-rAgBqHN_dfQ4Mp7p3GY%2CAOrYGslFs1duG8asPUOdiL2K19dRuPUhN1_1ZGZxqbN5FuGJ4qao4p7QlpaFv8rLVpCWoGytoPJL2dQ8QvnaQwYl-D8s5fWia9uv2Hn6lLP1AFgL%2CAOrYGskBSulXoODZPZ-Fou9IWV_hTascu8RVx0VDqmD9IGHijr482QJVvhZoLxNHQ40H_QaCoqgcDHMdKLwb1xz_B9DvU7BbuQRvc9_JYkelo3s%2CAOrYGskjQcigo3q7oEH5RiGd0dQEl2UAMiAgQCKuK4PD0gsVaSnyUYVi_l7KKhMFYexw-UU8n7zoCwMab5CpErKulon7tlMrHkrGKmqfiVSIhcZfDw%2CAOrYGslj8wC8rTXDKaYU6rJtqyBvD1Hv0zL5APvHqmg5VrG0kM8xwQ4eaOxuOIFjuOfTVAKW8OUEZzvQ0jsK5oMHAZdx2rr0VRedPS_XvucLlE9qcg&amp;pvsid=2889163926097059&amp;tmod=2031534224&amp;uas=0&amp;nvt=2&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=8&amp;fsb=1&amp;dtd=7760" title="Advertisement" aria-label="Advertisement"></iframe>

**把同一個 commit、同一套步驟改放到 `C:\tmp\ca` 之後，import 就成功了。** 差別只在路徑裡有沒有日文字。

**2\. 就算改成 ASCII 路徑，店面應用程式也啟動不了。**

```
OSError: [WinError 193] %1 不是有效的 Win32 應用程式。
```

`run_demo.py` 是直接啟動 `node_modules/.bin/next` 。在 Windows 上這樣呼叫需要的是 `next.cmd` 。 **API（:8000）有成功啟動** ，但店面端（:3000）沒有起來。

**3\. 後續清理時還會發生第二次當機。**

```
AttributeError: module 'os' has no attribute 'killpg'
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1106437894&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788503512&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoBm3%23google_vignette&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjYiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY2Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NiJdXSwwXQ..&amp;dt=1788503502775&amp;bpp=1&amp;bdt=758&amp;idt=0&amp;shv=r20260902&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788503502%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C952x280%2C952x280&amp;nras=9&amp;correlator=1905677708056&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5082&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1513&amp;eid=95399630%2C95400775%2C95401045%2C122880756%2C31061691%2C31061692&amp;oid=2&amp;psts=AOrYGsmkcJpVWZ8dYMRj2zFln4uXHXCfTBqP78O1jPKWMTiDDYxwOmFNRjF6AGSj7H_scSm-LWhd0r5CJBz2AIgX9wbxTPD2SXTUhhNJORhWjQltjA%2CAOrYGslVP6IktqQT2YcNgoFO0PKwhir5NSd6JjZ-ODqgIez8yMr6UrvF5f_erMIReRxy0PddpQbP7i68zTNM6X_XcBL6ZZRSNQ2gGGig0XzuIb0uJlfGe4GdDjj2akUixbxbmng%2CAOrYGslb47LRZXXWoacKb7V0dk2NhQt9c0bGGshsdFogtUn3f4NTeF5OiqKAf9TO5yZxd5WyCIxLAB5GPXt3VwDFZN4-rAgBqHN_dfQ4Mp7p3GY%2CAOrYGslFs1duG8asPUOdiL2K19dRuPUhN1_1ZGZxqbN5FuGJ4qao4p7QlpaFv8rLVpCWoGytoPJL2dQ8QvnaQwYl-D8s5fWia9uv2Hn6lLP1AFgL%2CAOrYGskBSulXoODZPZ-Fou9IWV_hTascu8RVx0VDqmD9IGHijr482QJVvhZoLxNHQ40H_QaCoqgcDHMdKLwb1xz_B9DvU7BbuQRvc9_JYkelo3s%2CAOrYGskjQcigo3q7oEH5RiGd0dQEl2UAMiAgQCKuK4PD0gsVaSnyUYVi_l7KKhMFYexw-UU8n7zoCwMab5CpErKulon7tlMrHkrGKmqfiVSIhcZfDw%2CAOrYGslj8wC8rTXDKaYU6rJtqyBvD1Hv0zL5APvHqmg5VrG0kM8xwQ4eaOxuOIFjuOfTVAKW8OUEZzvQ0jsK5oMHAZdx2rr0VRedPS_XvucLlE9qcg%2CAOrYGslziHWZmos6ZULM9gCOS4t_aY94c72gges8MB6SCYJ2pu1n0kdttq3Gw5a4NAHzLuBhj0eEB4EIFlaoe7hS40Stwf78bn5OcL8&amp;pvsid=2889163926097059&amp;tmod=2031534224&amp;uas=0&amp;nvt=2&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=9&amp;fsb=1&amp;dtd=9496" title="Advertisement" aria-label="Advertisement"></iframe>

`os.killpg` 是 **Unix 專用** ，Windows 沒有這個函式。原本是要處理錯誤，結果連清理流程本身也一起崩了。

**更麻煩的是，顯示錯誤訊息時也會崩。** 訊息裡有 em dash（ `—` ）， `cp932` 無法寫入。

```
UnicodeEncodeError: 'cp932' codec can't encode character '\u2014'
```

**加上 `PYTHONIOENCODING=utf-8` 之後，才終於能看到真正的錯誤。** 在加這個之前，我一直無法追到根因。

---

## 就算只讀，還是有價值

**雖然 Windows 上無法完整跑完 demo 很可惜，但我認為這個儲存庫的價值不在這裡。**

- **同一題材用 3 種執行形式來寫** ，很適合做設計比較
- 包含 **店家端代理** 的範例相當少見
- 文件把目錄、購物車、付款、訂單歷史的 **串接模式** 整理得很完整

如果只是想看程式碼， `git clone` 就夠了。 **如果要實際執行，建議用 WSL2 或 Linux。**

---

## 總結

- 9 月 2 日，Anthropic 以 **Apache 2.0** 公開了 **commerce-agents**
- 內含 **購物者與店家人員兩種** 代理
- 附有 **零售、旅遊、電信、娛樂 4 種產業** 的實作
- 以 **Messages API / Agent SDK / Managed Agents** 3 種形式寫出同一個題材，這是重點
- 官方提到的「購物車最多增加 35%」帶有 **up to**
- **在 Windows 上 `run_demo.py` 無法跑完。** README 裡完全沒提 Windows
	- **日文使用者名稱** → `.pth` 被 `cp932` 讀壞，導致 `ModuleNotFoundError`
		- **即使是 ASCII 路徑** → 直接啟動 `next` 而出現 `WinError 193`
		- **清理流程** → 因為沒有 `os.killpg` 而二次崩潰
- **連錯誤訊息顯示都會被 `cp932` 搞壞。** 不加 `PYTHONIOENCODING=utf-8` 甚至看不到真正原因

**只看內容的話，Windows 也沒問題。要執行的話，放到 WSL2 會比較快。**

---

**參考**

- [anthropics/commerce-agents](https://github.com/anthropics/commerce-agents) — 本體（Apache 2.0）
- [Building Commerce Agents with Claude](https://claude.com/blog/claude-for-commerce-agents) — 官方公告（2026-09-02）

※ 引用保留原文與中文譯文並列。譯文以易讀為優先，精確措辭請參考原文。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3034205192&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788503522&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoBm3%23google_vignette&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjYiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY2Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NiJdXSwwXQ..&amp;dt=1788503502775&amp;bpp=1&amp;bdt=759&amp;idt=1&amp;shv=r20260902&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788503502%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C952x280%2C952x280%2C952x280&amp;nras=10&amp;correlator=1905677708056&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6707&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=3104&amp;eid=95399630%2C95400775%2C95401045%2C122880756%2C31061691%2C31061692&amp;oid=2&amp;psts=AOrYGsmkcJpVWZ8dYMRj2zFln4uXHXCfTBqP78O1jPKWMTiDDYxwOmFNRjF6AGSj7H_scSm-LWhd0r5CJBz2AIgX9wbxTPD2SXTUhhNJORhWjQltjA%2CAOrYGslVP6IktqQT2YcNgoFO0PKwhir5NSd6JjZ-ODqgIez8yMr6UrvF5f_erMIReRxy0PddpQbP7i68zTNM6X_XcBL6ZZRSNQ2gGGig0XzuIb0uJlfGe4GdDjj2akUixbxbmng%2CAOrYGslb47LRZXXWoacKb7V0dk2NhQt9c0bGGshsdFogtUn3f4NTeF5OiqKAf9TO5yZxd5WyCIxLAB5GPXt3VwDFZN4-rAgBqHN_dfQ4Mp7p3GY%2CAOrYGslFs1duG8asPUOdiL2K19dRuPUhN1_1ZGZxqbN5FuGJ4qao4p7QlpaFv8rLVpCWoGytoPJL2dQ8QvnaQwYl-D8s5fWia9uv2Hn6lLP1AFgL%2CAOrYGskBSulXoODZPZ-Fou9IWV_hTascu8RVx0VDqmD9IGHijr482QJVvhZoLxNHQ40H_QaCoqgcDHMdKLwb1xz_B9DvU7BbuQRvc9_JYkelo3s%2CAOrYGskjQcigo3q7oEH5RiGd0dQEl2UAMiAgQCKuK4PD0gsVaSnyUYVi_l7KKhMFYexw-UU8n7zoCwMab5CpErKulon7tlMrHkrGKmqfiVSIhcZfDw%2CAOrYGslj8wC8rTXDKaYU6rJtqyBvD1Hv0zL5APvHqmg5VrG0kM8xwQ4eaOxuOIFjuOfTVAKW8OUEZzvQ0jsK5oMHAZdx2rr0VRedPS_XvucLlE9qcg%2CAOrYGslziHWZmos6ZULM9gCOS4t_aY94c72gges8MB6SCYJ2pu1n0kdttq3Gw5a4NAHzLuBhj0eEB4EIFlaoe7hS40Stwf78bn5OcL8%2CAOrYGslN5YpOl6nSyFWzN6D3wCz6FgHnNPbX3olReKbfZwb6EvdcxD22BhZwBPktD9uLY_y8pUQFRCIEtLhzfOyENR9FBV64E9-moY6CMXgBil_CgyY&amp;pvsid=2889163926097059&amp;tmod=2031534224&amp;uas=0&amp;nvt=2&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=10&amp;uci=a!a&amp;btvi=10&amp;fsb=1&amp;dtd=19361" title="Advertisement" aria-label="Advertisement"></iframe>

## 相關文章

- [裝了星數 6 萬的 Agent Skill，結果每次呼叫都要 9 萬 token](https://qiita.com/jqit_suwa/items/817a26e02cd1f08c7edd) — 同樣是把公開實作拿到本機執行的案例
- [Claude Fable 5.1，快取讀取變成四分之一；但 tool\_choice 會回 400](https://qiita.com/jqit_suwa/items/c0f5b86dfb8a3106cbaf) — 在建立代理時很有影響的規格變更

---

JQIT 的工程師中有 95% 以上是從未有經驗者錄用。  
如果有興趣，也歡迎來公司網站看看。

▶ [公司網站](https://www.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=corporate)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2765481581&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788503525&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoBm3%23google_vignette&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjYiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY2Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NiJdXSwwXQ..&amp;dt=1788503502777&amp;bpp=1&amp;bdt=761&amp;idt=0&amp;shv=r20260902&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788503502%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=1905677708056&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=7380&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=3764&amp;eid=95399630%2C95400775%2C95401045%2C122880756%2C31061691%2C31061692&amp;oid=2&amp;psts=AOrYGsmkcJpVWZ8dYMRj2zFln4uXHXCfTBqP78O1jPKWMTiDDYxwOmFNRjF6AGSj7H_scSm-LWhd0r5CJBz2AIgX9wbxTPD2SXTUhhNJORhWjQltjA%2CAOrYGslVP6IktqQT2YcNgoFO0PKwhir5NSd6JjZ-ODqgIez8yMr6UrvF5f_erMIReRxy0PddpQbP7i68zTNM6X_XcBL6ZZRSNQ2gGGig0XzuIb0uJlfGe4GdDjj2akUixbxbmng%2CAOrYGslb47LRZXXWoacKb7V0dk2NhQt9c0bGGshsdFogtUn3f4NTeF5OiqKAf9TO5yZxd5WyCIxLAB5GPXt3VwDFZN4-rAgBqHN_dfQ4Mp7p3GY%2CAOrYGslFs1duG8asPUOdiL2K19dRuPUhN1_1ZGZxqbN5FuGJ4qao4p7QlpaFv8rLVpCWoGytoPJL2dQ8QvnaQwYl-D8s5fWia9uv2Hn6lLP1AFgL%2CAOrYGskBSulXoODZPZ-Fou9IWV_hTascu8RVx0VDqmD9IGHijr482QJVvhZoLxNHQ40H_QaCoqgcDHMdKLwb1xz_B9DvU7BbuQRvc9_JYkelo3s%2CAOrYGskjQcigo3q7oEH5RiGd0dQEl2UAMiAgQCKuK4PD0gsVaSnyUYVi_l7KKhMFYexw-UU8n7zoCwMab5CpErKulon7tlMrHkrGKmqfiVSIhcZfDw%2CAOrYGslj8wC8rTXDKaYU6rJtqyBvD1Hv0zL5APvHqmg5VrG0kM8xwQ4eaOxuOIFjuOfTVAKW8OUEZzvQ0jsK5oMHAZdx2rr0VRedPS_XvucLlE9qcg%2CAOrYGslziHWZmos6ZULM9gCOS4t_aY94c72gges8MB6SCYJ2pu1n0kdttq3Gw5a4NAHzLuBhj0eEB4EIFlaoe7hS40Stwf78bn5OcL8%2CAOrYGslN5YpOl6nSyFWzN6D3wCz6FgHnNPbX3olReKbfZwb6EvdcxD22BhZwBPktD9uLY_y8pUQFRCIEtLhzfOyENR9FBV64E9-moY6CMXgBil_CgyY%2CAOrYGskF_h2PZy9Zpeb-M4_DjwMPnm4SGMAgon_eNnSOoMgLZMZ-HPEcymhuUZiHtkijO8zSU0DjIcm5QRwVdg8TOBtuWUOvnf12d9vO_bRr48CEo7M&amp;pvsid=2889163926097059&amp;tmod=2031534224&amp;uas=3&amp;nvt=2&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=11&amp;uci=a!b&amp;btvi=11&amp;fsb=1&amp;dtd=22273" title="Advertisement" aria-label="Advertisement"></iframe>

我們也有工程師招募。若你有興趣，歡迎點進去看看。

▶ [招募網站](https://recruitment.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=recruit)

---

原文出處： [https://qiita.com/suwa\_nobu/items/1c0f27da8eb685f6ba05](https://qiita.com/suwa_nobu/items/1c0f27da8eb685f6ba05)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/apoBm3) [52瀏覽](https://codelove.tw/@tony/post/apoBm3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/apoBm3)

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

📝20 💬4

294

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1200" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=3809598800&amp;adf=3132389021&amp;pi=t.aa~a.1416458249~rp.1&amp;abgtt=6&amp;w=1200&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788503527&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=1200x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoBm3%23google_vignette&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjYiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY2Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NiJdXSwwXQ..&amp;dt=1788503502785&amp;bpp=1&amp;bdt=769&amp;idt=1&amp;shv=r20260902&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788503502%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788503502%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C952x280%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=12&amp;correlator=1905677708056&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=217&amp;ady=8585&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=5034&amp;eid=95399630%2C95400775%2C95401045%2C122880756%2C31061691%2C31061692&amp;oid=2&amp;psts=AOrYGsmkcJpVWZ8dYMRj2zFln4uXHXCfTBqP78O1jPKWMTiDDYxwOmFNRjF6AGSj7H_scSm-LWhd0r5CJBz2AIgX9wbxTPD2SXTUhhNJORhWjQltjA%2CAOrYGslVP6IktqQT2YcNgoFO0PKwhir5NSd6JjZ-ODqgIez8yMr6UrvF5f_erMIReRxy0PddpQbP7i68zTNM6X_XcBL6ZZRSNQ2gGGig0XzuIb0uJlfGe4GdDjj2akUixbxbmng%2CAOrYGslb47LRZXXWoacKb7V0dk2NhQt9c0bGGshsdFogtUn3f4NTeF5OiqKAf9TO5yZxd5WyCIxLAB5GPXt3VwDFZN4-rAgBqHN_dfQ4Mp7p3GY%2CAOrYGslFs1duG8asPUOdiL2K19dRuPUhN1_1ZGZxqbN5FuGJ4qao4p7QlpaFv8rLVpCWoGytoPJL2dQ8QvnaQwYl-D8s5fWia9uv2Hn6lLP1AFgL%2CAOrYGskBSulXoODZPZ-Fou9IWV_hTascu8RVx0VDqmD9IGHijr482QJVvhZoLxNHQ40H_QaCoqgcDHMdKLwb1xz_B9DvU7BbuQRvc9_JYkelo3s%2CAOrYGskjQcigo3q7oEH5RiGd0dQEl2UAMiAgQCKuK4PD0gsVaSnyUYVi_l7KKhMFYexw-UU8n7zoCwMab5CpErKulon7tlMrHkrGKmqfiVSIhcZfDw%2CAOrYGslj8wC8rTXDKaYU6rJtqyBvD1Hv0zL5APvHqmg5VrG0kM8xwQ4eaOxuOIFjuOfTVAKW8OUEZzvQ0jsK5oMHAZdx2rr0VRedPS_XvucLlE9qcg%2CAOrYGslziHWZmos6ZULM9gCOS4t_aY94c72gges8MB6SCYJ2pu1n0kdttq3Gw5a4NAHzLuBhj0eEB4EIFlaoe7hS40Stwf78bn5OcL8%2CAOrYGslN5YpOl6nSyFWzN6D3wCz6FgHnNPbX3olReKbfZwb6EvdcxD22BhZwBPktD9uLY_y8pUQFRCIEtLhzfOyENR9FBV64E9-moY6CMXgBil_CgyY%2CAOrYGskF_h2PZy9Zpeb-M4_DjwMPnm4SGMAgon_eNnSOoMgLZMZ-HPEcymhuUZiHtkijO8zSU0DjIcm5QRwVdg8TOBtuWUOvnf12d9vO_bRr48CEo7M%2CAOrYGskqiqn51ycgMu0ZXxp01NHuhGPdql_CiXCJCajB5-j0kc9NmcnELlC_L-Z6dfbMOw6Lx9OrbkPdOQs4Fnagplnyn1Xz26FXtAhnVWp5Lmk&amp;pvsid=2889163926097059&amp;tmod=2031534224&amp;uas=3&amp;nvt=2&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=12&amp;uci=a!c&amp;btvi=12&amp;fsb=1&amp;dtd=24323" title="Advertisement" aria-label="Advertisement"></iframe>

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788503502&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoBm3%23google_vignette&amp;pra=5&amp;aiof=11&amp;asro=0&amp;itsi=0&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuNjYiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjY2Il0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny42NiJdXSwwXQ..&amp;dt=1788503502245&amp;bpp=3&amp;bdt=229&amp;idt=27&amp;shv=r20260902&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788483040%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788483040%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788483040%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1905677708056&amp;rume=1&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400775%2C95401045%2C122880756%2C31061691%2C31061692&amp;oid=2&amp;pvsid=2889163926097059&amp;tmod=2031534224&amp;uas=0&amp;nvt=2&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=1920%2C70%2C1920%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=36" title="Advertisement" aria-label="Advertisement"></iframe>