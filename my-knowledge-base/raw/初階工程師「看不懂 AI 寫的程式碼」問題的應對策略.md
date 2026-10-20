---
title: "初階工程師「看不懂 AI 寫的程式碼」問題的應對策略"
source: "https://codelove.tw/@tony/post/a9yK1a"
author:
published:
created: 2026-09-09
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------我從新進工程師開始到現在，已經滿 1 年了。當我請生成式 AI 協助實作時，實際寫程式的時間確實明顯縮短了，這點我..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月08日](https://codelove.tw/@tony/post/a9yK1a "2026-09-08 17:03:19")

## 初階工程師「看不懂 AI 寫的程式碼」問題的應對策略

## 前言

我從新進工程師開始到現在，已經滿 1 年了。

當我請生成式 AI 協助實作時，實際寫程式的時間確實明顯縮短了，這點我很有感。

另一方面，像我這樣的初階工程師，因為要理解生成出來的程式碼需要花時間， **開發整體並沒有如預期般變快** ，我也深刻感受到這一點。

因此，我先嘗試用 AI 做了一份用來閱讀生成程式碼的審查指南。  
只要按照指南，就能確認每一段處理各自是在做什麼。  
但即便如此，還是無法理解它們彼此怎麼串起來、以及為什麼會是那樣的架構。（一敗）

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788920809&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa9yK1a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788915042639&amp;bpp=3&amp;bdt=1074&amp;idt=3&amp;shv=r20260903&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788915042%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;prev_fmts=0x0%2C1522x969&amp;nras=3&amp;correlator=981112525910&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=110&amp;ady=985&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774%2C42533293&amp;oid=2&amp;pvsid=8835902332214585&amp;tmod=1361289636&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

最後，還是得花時間把程式碼重讀一遍，而且即使如此，自己也不確定是不是真的理解了，結果還是會請前輩幫忙 review。  
即使實作時間變短了， **這樣也不能說整體開發變得更有效率。**

於是我開始思考，問題或許不在於程式碼該怎麼讀，而是 **在請 AI 實作之前的準備** 。  
如果能先整理好要做的功能需求與處理流程，並把它變成可以用自己的話說明的狀態，就能把 AI 產生的程式碼和自己的預期做比較。

這篇文章會介紹我這個初階工程師， **如何透過和 AI 來回討論，整理需求與實作範圍，並記錄成 ADR，讓生成的程式碼更容易理解** 。

目標不是 **「讓輸出的程式碼更容易讀」** ，而是 **「讓自己能預想輸出會長什麼樣子」**

## 實踐本文內容後

- Code Review 的次數明顯減少了
- 對前輩的指正，比較能說明自己的意圖
- 對 AI 產出的程式碼，也更有「我看得懂」的感覺
- 和 AI 的來回對話變少了
- 自己比較容易回想起「當初為什麼這樣做」
- 對於實作前應該考慮的重點，也能有個大致掌握

## 為什麼生成的程式碼不容易理解

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788920809&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa9yK1a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788915042649&amp;bpp=1&amp;bdt=1085&amp;idt=0&amp;shv=r20260903&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788915042%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;prev_fmts=0x0%2C1522x969%2C952x280&amp;nras=4&amp;correlator=981112525910&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=110&amp;ady=1998&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774%2C42533293&amp;oid=2&amp;pvsid=8835902332214585&amp;tmod=1361289636&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

回到主題。

我認為，生成的程式碼難以理解的原因，出在請 AI 實作之前的階段。

如果腦中沒有整體處理流程，就無法想像處理會以什麼順序進行、每段程式碼各自扮演什麼角色。  
在這種狀態下讀程式碼， **就只會變成一段一段確認眼前的處理，最後反而失去處理彼此之間的連結。**

實作範圍不清楚，也是讓程式碼變複雜的原因之一。  
如果沒有先決定要實作什麼、不要實作什麼，就草率地把需求丟給 AI， **有時會連本來沒打算要的例外處理或抽象化一起做進去。**

但因為請求的一方自己也沒有畫出完成後的樣子，所以即使生成的程式碼偏離了自己的預期， **也不會察覺到那個「落差」。**

結果就是拿到一份比需要更複雜的實作。

「AI 好像思考得太多了……」  
「感覺改動比想像中還長……」

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788920809&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa9yK1a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788915042656&amp;bpp=1&amp;bdt=1092&amp;idt=1&amp;shv=r20260903&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788915042%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;prev_fmts=0x0%2C1522x969%2C952x280%2C952x280&amp;nras=5&amp;correlator=981112525910&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=110&amp;ady=2822&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774%2C42533293&amp;oid=2&amp;pvsid=8835902332214585&amp;tmod=1361289636&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

在整體流程與實作範圍都模糊不清的情況下，根本沒辦法用自己的話說明生成的程式碼在做什麼。  
每次讀程式碼都得問 AI「這段程式在幹嘛」，結果原本靠實作省下來的時間，又被拿去理解程式碼了。  
而且問得越多，使用 AI 的成本也會跟著增加。

### 如果請求的是 CSV 輸出

例如，假設你用下面這個 prompt 請 AI 實作：

```
讓使用者列表可以下載成 CSV
```

請求方原本想的是：從既有的列表取得流程中取出必要欄位，然後只做成 CSV 格式回傳的簡單修改。

但如果沒有把範圍說清楚，像是「只需要 CSV」「資料量不大」「不需要檔案儲存或非同步處理」， **AI 有時會把未來擴充的可能性也一併考慮進去。**

例如，可能會多出像下面這些機制：

- 用來切換 CSV 與 JSON 的介面
- 依輸出格式產生對應類別的 Factory
- 用來儲存檔案的 storage
- 用背景處理的 queue
- 失敗時的重試機制

原本只要一個處理就能完成的改動，結果膨脹成跨越多個類別與設定檔的實作。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788920809&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa9yK1a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788915042663&amp;bpp=2&amp;bdt=1099&amp;idt=2&amp;shv=r20260903&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788915042%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;prev_fmts=0x0%2C1522x969%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=981112525910&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=110&amp;ady=3840&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774%2C42533293&amp;oid=2&amp;pvsid=8835902332214585&amp;tmod=1361289636&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

如果未來真的要支援多種輸出格式或大量資料，這些機制或許是必要的。  
但如果目前的需求只有 CSV 輸出，那這些都只是還不需要的擴充性。  
這裡我把針對目前需求做得過頭的設計，稱為 **過度工程化（Overengineering）** 。

近來 AI 越來越強，反而常常給人一種「好心辦壞事」的感覺

如果請求方沒有先決定實作範圍，即使能問 AI 每個類別是做什麼的，也沒辦法判斷這些類別是不是這次真的需要。  
結果就會因為「AI 的實作」和「使用者的預期」之間產生落差，讓生成程式碼更難理解，理解與 review 都會花更多時間。

## 解決方向

要理解生成的程式碼，不只是拿到程式後再想辦法讀懂，還必須在 **請 AI 實作之前，就先把自己的預期說清楚** 。  
首先，要先能用自己的話說明想要達成的需求與處理流程。

這裡不需要把實作細節全部決定好。  
在實作前，只要能說明以下 3 點即可：

- **要解決什麼問題**
- **要實現什麼**
- **這次不處理什麼**

只要能說明這 3 點，就能看出哪些範圍可以交給 AI。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788923418&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa9yK1a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788915042676&amp;bpp=2&amp;bdt=1200&amp;idt=2&amp;shv=r20260903&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788915042%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;prev_fmts=0x0%2C1522x969%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=8&amp;correlator=981112525910&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=110&amp;ady=5054&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=1194&amp;eid=95399629%2C95400774%2C42533293&amp;oid=2&amp;psts=AOrYGskew-CX19fh8OAUKFh2v8fAO32XmPAs2-QdV3C5bAtFts1zOKul1ly2aqoDt-LAlJJsezbA5SM4ggO5HsC9GRvtsMGGLl55JtqFtgHJUy4gDS5-k8TJFO5f3WOxwtCI%2CAOrYGsnX3UFhSkYwMuTkLEJrWB6mt91SLNa_UUbSz2mOyQBsKqDY-56BEzNcDm8CkM0V51I9wIsOhJWG-Paj04hDVUEp74OB328lZnEmVVQV8yJ7%2CAOrYGslr1eR1jRInBW7iY6XobxSJ5A3SVmMzwB50q8ve5w7N2RoeHJXgupwpPp9yGofdibD48NO_yKCDUpJNC-GMKdO-tuca-qQkhW73pQSJQnKcmw%2CAOrYGskEzesvr2yG23c74IlHM7V1lQbvtfuzE5AKaieOLwbF1S7sBIsrjGblN6uKu3RkmOs42vc1RYl2mW2yG2MXkYLk4he2N7mJGbY%2CAOrYGsnW8JsiuSmOwf--DD8eYl4-ZOdAuRY3ycX1SjTStAeDxZFCXjSVwgd4wCg_fHkVc_x6-LptnB6iCe86xpqo5fX1gVRA-sZsS5Q&amp;pvsid=8835902332214585&amp;tmod=1361289636&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1762%2C1050%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

有了這個準備，就能把生成出來的實作和自己的預期做比較。  
剛才那個 CSV 輸出的例子，也能判斷 JSON 切換或非同步處理不在本次範圍內。  
因此可以請 AI 刪掉不必要的實作，更容易避免過度工程化。

這樣一來，原本在 review 前才會發現、但自己又說不清楚的部分，可以先被找出來並確認；前輩也不需要從頭讀解程式碼意圖，review 的負擔自然會降低。

## 我的解法

為了讓每次實作前的準備都能用同一套流程進行，我註冊了 `dig-and-write-adr` Skill。  
它是一個先用 `dig` Skill 和 AI 確認實作方針，接著把決定內容記錄成 `.md` 的 Skill。  
各個 Skill 的細節之後再說明。

```
---
name: dig-and-write-adr
description: 透過徹底提問來精煉計畫與設計，同時也建立 ADR 與術語表。
disable-model-invocation: true
---

執行 Skill 工具的「dig」，在結束並輸出整理格式後，將對話內容以 ADR 的形式記錄成 md。
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=4060917935&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788923944&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa9yK1a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788915042683&amp;bpp=1&amp;bdt=1213&amp;idt=1&amp;shv=r20260903&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788915042%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788915042%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;prev_fmts=0x0%2C1522x969%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=9&amp;correlator=981112525910&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=110&amp;ady=5951&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=2103&amp;eid=95399629%2C95400774%2C42533293&amp;oid=2&amp;psts=AOrYGskew-CX19fh8OAUKFh2v8fAO32XmPAs2-QdV3C5bAtFts1zOKul1ly2aqoDt-LAlJJsezbA5SM4ggO5HsC9GRvtsMGGLl55JtqFtgHJUy4gDS5-k8TJFO5f3WOxwtCI%2CAOrYGsnX3UFhSkYwMuTkLEJrWB6mt91SLNa_UUbSz2mOyQBsKqDY-56BEzNcDm8CkM0V51I9wIsOhJWG-Paj04hDVUEp74OB328lZnEmVVQV8yJ7%2CAOrYGslr1eR1jRInBW7iY6XobxSJ5A3SVmMzwB50q8ve5w7N2RoeHJXgupwpPp9yGofdibD48NO_yKCDUpJNC-GMKdO-tuca-qQkhW73pQSJQnKcmw%2CAOrYGskEzesvr2yG23c74IlHM7V1lQbvtfuzE5AKaieOLwbF1S7sBIsrjGblN6uKu3RkmOs42vc1RYl2mW2yG2MXkYLk4he2N7mJGbY%2CAOrYGsnW8JsiuSmOwf--DD8eYl4-ZOdAuRY3ycX1SjTStAeDxZFCXjSVwgd4wCg_fHkVc_x6-LptnB6iCe86xpqo5fX1gVRA-sZsS5Q%2CAOrYGslkFxIJl_J0NMjNij-F6jG8rmdi3hhayoZxqfgAvNs_MB75CxxV19_ie_43uMA6wd-SFBGn16pAV4zfA2qrJ8fVgQZB5QFevr9gW3lJjLJzIg&amp;pvsid=8835902332214585&amp;tmod=1361289636&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1762%2C1050%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

我參考的是這個 `grill-with-docs` Skill。

### 1\. 實作前先和 AI 對齊認知（dig 階段）

在請 AI 實作之前，先確認需求與處理流程。  
決定這次要實作到哪裡、這次不實作什麼，目標是讓 AI 和自己都能想像出同一個完成樣貌。

不過，經驗尚淺的初階工程師，要一個人把所有該討論的論點都找出來是很困難的。  
就算自己覺得已經整理得差不多了，還是可能漏掉例外情況的行為，或對既有功能造成的影響。  
所以，我會在實作前的來回討論中使用 `dig` Skill。

用以下格式呼叫我這次做的 `dig-and-write-adr` ，就可以觸發 `dig` ：

```
/dig-and-write-adr <question>
```

接著，AI 會提出用來決定實作方針的問題，以及 4 個選項。  
你不需要自己從零開始思考所有檢討項目。  
可以一邊從提示的選項中選擇方針，一邊進行對話，因此即使經驗不多，也比較容易發現漏掉的考量。  
在回答問題的過程中，需求、處理流程與實作範圍會逐漸變得明確，也能減少和 AI 之間的認知落差。

#### 範例

```
$ /dig-and-write-adr 想讓使用者列表可以下載成 CSV
---

### Q1: 「使用者列表」是指既有領域中的哪個列表？

這個 codebase 裡沒有 \`User\` entity 或認證使用者清單；若以人物列表來看，只有飼主（\`Owner\`）和獸醫師
（\`Vet\`）。如果對象弄錯，CSV 的欄位、流程與 API 設計都會跟著改變。

- **A** — 飼主列表（\`/owners\`）
- **B** — 獸醫師列表（\`/vets.html\`）
- **C** — 飼主與獸醫師兩者
- **D** — 新增認證使用者／利用者管理功能

**建議: A** — 因為目前 UI 中有可搜尋的人物列表，而且姓名、地址、城市、電話號碼這些 CSV 需要的欄位也都齊備。

CSV 輸出對象:
❯ A — 飼主列表
  B — 獸醫師列表
  C — 飼主與獸醫師兩者
  D — 新的認證使用者管理
  Other (type your answer)
```

如果有看不懂的詞，也可以隨時提問，AI 會補充說明。

```
$ 既有領域是什麼？
---

「既有領域」是指這個應用程式目前正在處理的業務對象／概念。以 Spring Petclinic 來說，主要是 **飼主
（Owner）・寵物（Pet）・就診（Visit）・獸醫師（Vet）**。

也就是說，剛才的問題是在確認「使用者」這個詞，在這個應用程式裡到底指的是誰。

### Q1: 想下載成 CSV 的人物是誰？

- **A** — 飼主列表
- **B** — 獸醫師列表
- **C** — 飼主與獸醫師兩者
- **D** — 登入使用者等新種類的使用者

**建議: A** — 因為可以直接把目前「Find owners」畫面顯示的列表，自然加上 CSV 下載功能。

CSV 要輸出的人物:
❯ A — 飼主
  B — 獸醫師
  C — 飼主與獸醫師
  D — 新種類的使用者
  Other (type your answer)
```

它問得滿細的，會有點累

### 2\. 將對齊後的結果記錄成 ADR（輸出到 md 階段）

在 `dig` 階段決定好的內容，會以 **ADR（Architecture Decision Record）** 的形式記錄成 Markdown 檔。  
（我目前是用 Obsidian 管理。只要裝了 `Obsidian Skill` ，它就會自動把相關檔案串成連結，這點很方便。）

只要把要實作的內容、不要實作的內容、採用的處理流程，以及選擇這個方針的理由留下來，之後回頭看也能追溯當初的判斷過程。

以 CSV 輸出的例子來說，會記錄像下面這樣的內容。

adr/ [user-list-csv-export.md](http://user-list-csv-export.md/)

```
# 使用者列表的 CSV 輸出

## 決策

- 對象是飼主（\`Owner\`）。
- 使用既有的列表取得流程。
- 輸出格式僅限 CSV。
...

## 非目標

- 檔案儲存
- 非同步處理
- JSON 等非 CSV 的輸出格式
...

## 理由

由於目前資料量不大，而且暫時沒有要求 CSV 以外的輸出格式，因此
etc ...
```

在實作過程中如果迷路了，可以回到 ADR，確認生成的程式碼是否符合當初決定的方針。  
就算 AI 遺失了前文脈絡，只要重新讀入 ADR，就能再次共享實作範圍與方針。  
對要 review 的前輩工程師來說，這也是一份能輔助理解程式碼以外實作意圖與判斷理由的資料。

## 實作前準備的 Before/After

例：在 `spring-petclinic` 中實作使用者列表 CSV 輸出功能

### Before：直接請求實作

```
$ 讓使用者列表可以下載成 CSV
```

只把這句話丟給 AI，然後直接請它實作。  
生成的程式碼裡，包含了為了支援多種輸出格式的介面與 Factory、儲存檔案的機制、非同步處理等等。

但因為在請求前沒有先決定實作範圍，所以無法判斷哪些才是目前需求真正需要的。  
如果逐一問 AI 每個類別的用途，雖然可以理解程式的角色，卻還是無法說明它為什麼是這次實作所必需。  
到最後，還是會在沒有充分理解複雜架構的情況下，請前輩來 review。

「為什麼會有 Factory！」  
「這個非同步處理是怎樣！」  
「『幫我解釋這段程式碼』」

### After：先決定方針，再請求實作

在請求實作前，先使用 `dig-and-write-adr` ，一邊回答 AI 的問題，一邊整理需求與處理流程。

```
$ /dig-and-write-adr 想讓使用者列表可以下載成 CSV
```

接著 AI 會回傳幾個問題。

```
### Q3: CSV 的一列與欄位要如何構成？

\`Owner\` 除了有 ID、名字、姓氏、地址、城市、電話號碼之外，還會有多隻寵物。  
如果把寵物也算進來，就不再是「一位飼主一列」這麼單純的列表了。

- **A** — 一位飼主一列，輸出 \`ID, First Name, Last Name, Address, City, Telephone\`
- **B** — 依照畫面顯示，輸出 \`Name, Address, City, Telephone, Pets\`（多個寵物名稱串在同一個儲存格）
- **C** — 每隻寵物一列，重複輸出飼主資訊
- **D** — 個別指定欄位

**建議: A** — 這樣會得到以飼主列表為基礎、較正規化且容易處理的 CSV，也不用另外定義多隻寵物的呈現規則。ID 也能作為後續處理中穩定的識別欄位。

CSV 的列／欄位構成:
❯ A — 飼主基本 6 項（建議）
  B — 與畫面相同欄位＋寵物名稱
  C — 每隻寵物一列
  D — 個別指定欄位
  Other (type your answer)
```

像是只輸出 CSV、只輸出 6 個欄位、沿用既有的列表取得流程、不做檔案儲存與非同步處理，這些都會在回答 AI 的問題時逐一決定。

「基本 6 項雖然是建議，但以這次需求來看，其實還需要寵物名稱啊。」

將決定內容與理由記錄成 ADR，然後以那份 ADR 作為前提，請 AI 開始實作。

這時候，ADR 就成為判斷基準。  
如果生成的程式碼裡包含切換輸出格式的機制或非同步處理，就能判斷這些不是本次需求所需。

[petclinic-user-csv-export-ADR.md](http://petclinic-user-csv-export-adr.md/)

```
## Context

Spring Petclinic 中沒有用於認證的 \`User\` 模型。
代表人物的既有模型是飼主 \`Owner\` 與獸醫師 \`Vet\`。
這次所說的「使用者列表」已確定是指客戶列表相當的 \`Owner\`。
...

## 決定因素

- 要能取得畫面上的頁面，而不是只限目前顯示的一頁
- 不能把數萬筆資料全部保留在記憶體中

etc ...
```

因為事先理解了需求與處理流程，所以可以用自己的話說明每個生成出來的處理是在做什麼。  
在 review 時，也能把程式碼與 ADR 一起交給前輩，讓前輩能在理解實作意圖與判斷理由後再看程式碼。

兩者都是讓 AI 幫忙產生程式碼，差別在於 **實作前的判斷基準** 。  
只要有判斷基準，就更能看懂生成的程式碼，也更能看出哪些實作是不必要的。

## 總結

我認為，AI 生成的程式碼之所以難懂，原因不只是程式本身很難而已。

如果在實作前沒有先整理好需求與處理流程，就沒辦法判斷生成的程式碼是否符合自己的預期、是否真的屬於這次需求所需。

加入這個步驟之後，讀 AI 生成的程式碼就不只是單純地讀，而是能 **對照自己事先決定的方針來閱讀。**  
如果不只知道每段程式碼的功能，還能說明「為什麼這次需要這樣實作」，在請人 review 時也能更清楚地共享實作意圖。

與其事後思考「產生程式碼之後該怎麼辦」，不如從實作前的需求定義開始，就先緩解「程式碼難懂」的問題。

---

原文出處： [https://qiita.com/im\_yoneda/items/f27e52d2f852ba017a7e](https://qiita.com/im_yoneda/items/f27e52d2f852ba017a7e)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/a9yK1a) [40瀏覽](https://codelove.tw/@tony/post/a9yK1a)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/a9yK1a)

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

📝25

369

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788915041&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa9yK1a&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788915041658&amp;bpp=17&amp;bdt=204&amp;idt=80&amp;shv=r20260903&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788838193%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788838193%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788838193%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;nras=1&amp;correlator=981112525910&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774%2C42533293&amp;oid=2&amp;pvsid=8835902332214585&amp;tmod=1361289636&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1522%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=231" title="Advertisement" aria-label="Advertisement"></iframe>