---
title: "如何避免 RAG 用過時資訊作答？從「Converged」資料庫思考 AI 時代的資料基礎架構"
source: "https://codelove.tw/@tony/post/ag2LM3"
author:
published:
created: 2026-08-13
description: "![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fdffb8c6c-f0f9-4dba-..."
tags:
  - "clippings"
status: "processed"
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月12日](https://codelove.tw/@tony/post/ag2LM3 "2026-08-12 17:15:10")

## 如何避免 RAG 用過時資訊作答？從「Converged」資料庫思考 AI 時代的資料基礎架構

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fdffb8c6c-f0f9-4dba-9565-ced654c5a19e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=23b337337d781184068492723940d64f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fdffb8c6c-f0f9-4dba-9565-ced654c5a19e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=23b337337d781184068492723940d64f)

## 前言

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786583744&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2LM3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786583258312&amp;bpp=21&amp;bdt=3683&amp;idt=21&amp;shv=r20260811&amp;mjsv=m202608070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786583259%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786583259%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786583259%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x852&amp;nras=3&amp;correlator=2580692748476&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1302&amp;biw=1634&amp;bih=852&amp;scr_x=0&amp;scr_y=0&amp;eid=95397133&amp;oid=2&amp;pvsid=6505834522934800&amp;tmod=1216897310&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C852&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

生成式 AI 或 AI 代理的活用，往往也會連帶牽涉到資料庫端的議題：AI 要參照的資料要怎麼保存。  
是把不同形態的資料——關聯式、文件、向量——分別放到各自擅長的專用資料庫裡，還是統一收納到一個資料庫中？  
本文所談的 **融合式資料庫（converged database）** ，就是指這種「統一保存」的做法。

本文以 **Oracle Developers** 部落格的 [**What Is a Converged Database? Definition, Five Tests, and AI Use Cases**](https://blogs.oracle.com/developers/what-is-a-converged-database-definition-five-tests-and-ai-use-cases) （Rick Houlihan，2026 年 6 月）為基礎，整理這個詞「融合式資料庫（converged database）」究竟指什麼，以及為什麼它常和 RAG、AI 代理一起被提起。

> **本文定位** ：本文是依據上述 Oracle 部落格內容，加入筆者自己的理解重新整理並補充而成。 **並非 Oracle 官方立場** 。

## 融合式資料庫是什麼

資料庫處理的資料，不只有表格那樣的結構，也有許多不同的「形態（資料型態／模型）」。  
先從實例來看，為什麼在 AI 活用的脈絡下會出現這個詞。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3764087313&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786583744&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2LM3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786583258353&amp;bpp=9&amp;bdt=3724&amp;idt=9&amp;shv=r20260811&amp;mjsv=m202608070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786583259%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786583259%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786583259%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x852%2C952x280&amp;nras=4&amp;correlator=2580692748476&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2114&amp;biw=1634&amp;bih=852&amp;scr_x=0&amp;scr_y=0&amp;eid=95397133&amp;oid=2&amp;pvsid=6505834522934800&amp;tmod=1216897310&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C852&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

假設有一個小型電商網站。  
即使不是專門為 AI 設計的系統，當你開始思考業務資料與 AI 應用時，通常也會發現一般情境下就已經會出現各種不同形態的資料。

- **訂單／明細** — 以金額與庫存為主，需用嚴格結構與約束來處理的表格資料（關聯式）
- **客戶個人檔案** — 每個人欄位不一樣的巢狀 JSON（文件）
- **詐欺偵測** — 像是「多個帳號共用同一裝置」這類可追蹤關係的圖資料
- **客服工單相似搜尋** — 以語意相近程度找出「和這個工單相似的過往案件」的向量搜尋
- **附近門市** — 依緯度、經度計算的空間（Spatial）資料

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F3b030b16-9bf0-4b44-a3a8-c06b49724c4f.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1a1d2915c4b9e05b1f29f6b03dd04fcd)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F3b030b16-9bf0-4b44-a3a8-c06b49724c4f.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1a1d2915c4b9e05b1f29f6b03dd04fcd)

這類資料的保存方式，通常會依模型分成對應的專用資料庫。  
例如文件資料用 MongoDB、圖資料用 Neo4j、向量資料用 Pinecone，各自以專門的服務或儲存系統來分開處理。  
相對地， **把這些資料統一收納到一個資料庫引擎中** ，就是所謂的 **融合式（converged）** 。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786583748&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2LM3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786583258386&amp;bpp=3&amp;bdt=3757&amp;idt=3&amp;shv=r20260811&amp;mjsv=m202608070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786583259%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786583259%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786583259%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x852%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=6&amp;correlator=2580692748476&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3425&amp;biw=1634&amp;bih=852&amp;scr_x=0&amp;scr_y=30&amp;eid=95397133&amp;oid=2&amp;psts=AOrYGskr5gFA0UV-EWELxadjX84t5C7jb8PgzQx_EdOu2s6ve9orcSeASBU7qRxo2A0oTpF7bjpIXivhg6GXFBEX6-a7w3AcTMJWl2rzBqA2jyrDBC3ajZubnp7DkkoAUm-U3g%2CAOrYGsnmwR3LwjVz4I_EIOcxuFLBAEBMxCg7cCxZ8vkQbM85TsKlHeMK-yfspqQ0KX8uqfIitMwuhklU4V-voJRzwY-4Mc0mGv-uF3hyEOfcHy8MR0M%2CAOrYGslrUCZGDtd-A7nMjfQr45KR6Q_h1meha1A5AHjHDkNt1LzjYGqSixItubi2Hjq0UOFdaWm40WNhDhlK1LuU4MJ6GxYmpUobVPZoRs1_i9Ak8w&amp;pvsid=6505834522934800&amp;tmod=1216897310&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C852&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=4&amp;uci=a!4&amp;btvi=5&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

將原文的定義翻成中文，大致如下：

> 由單一資料庫引擎原生支援多種資料模型，例如關聯式、文件（JSON）、圖、向量、空間、文字，並且它們共享 **同一個最佳化器、同一個交易邊界、同一個一致性模型、同一個安全與治理領域** ，且能透過開發者預期的存取方式來使用，例如 SQL、文件 API、REST。

這段定義很長，但原文本身也說，重點不是列出哪些模型，而是後面那一串特性。  
不過只看這個定義還是不太直觀，所以接下來會從詞源與背景開始一步步說明。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Feb0208f1-4af0-40ab-978e-cd4e3d71ae05.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c8881aa344307bf10a90d6746625f96a)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Feb0208f1-4af0-40ab-978e-cd4e3d71ae05.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c8881aa344307bf10a90d6746625f96a)

## 詞彙的產生經緯

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=186534417&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786584228&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2LM3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786583258403&amp;bpp=8&amp;bdt=3795&amp;idt=9&amp;shv=r20260811&amp;mjsv=m202608070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786583259%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786583259%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786583259%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x852%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=7&amp;correlator=2580692748476&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4573&amp;biw=1634&amp;bih=852&amp;scr_x=0&amp;scr_y=1172&amp;eid=95397133&amp;oid=2&amp;psts=AOrYGskr5gFA0UV-EWELxadjX84t5C7jb8PgzQx_EdOu2s6ve9orcSeASBU7qRxo2A0oTpF7bjpIXivhg6GXFBEX6-a7w3AcTMJWl2rzBqA2jyrDBC3ajZubnp7DkkoAUm-U3g%2CAOrYGsnmwR3LwjVz4I_EIOcxuFLBAEBMxCg7cCxZ8vkQbM85TsKlHeMK-yfspqQ0KX8uqfIitMwuhklU4V-voJRzwY-4Mc0mGv-uF3hyEOfcHy8MR0M%2CAOrYGslrUCZGDtd-A7nMjfQr45KR6Q_h1meha1A5AHjHDkNt1LzjYGqSixItubi2Hjq0UOFdaWm40WNhDhlK1LuU4MJ6GxYmpUobVPZoRs1_i9Ak8w%2CAOrYGsmFoPTssUC4qD6eX5tHjFSIlmAqNXEcTr9fubONKdFPJMlHHoYztEnrejBPTFXb2KjuPXBgsCMcUDxZm7q9Xgh0XEmj7z8G0cYh1kP5-puhTRw&amp;pvsid=6505834522934800&amp;tmod=1216897310&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C852&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Oracle 大約在 2020 年前後開始使用 **融合式資料庫（Converged Database）** 這個詞。  
最初的定義，是 Maria Colgan 所說的「單一引擎原生支援所有現代資料型態與開發典範」。（ [M. Colgan, “What is a Converged Database?](https://sqlmaria.com/2020/03/05/what-is-a-converged-database/),” 2020）  
也就是說，把多個系統整併成比較少的系統（最好是一個），就能減少授權數量、備份數量，以及系統間串接的麻煩，這本質上是一種便利性上的說法。當時的「converged」主要是在講 **consolidation（整併）** 。

但在那之後，經過以下三個變化，這個詞的意義逐漸改變。  
原本只是「整併起來比較方便」的說法，逐漸變成「是否融合在一起，會直接決定能做什麼、不能做什麼」的 **結構性特質** 。以下依序來看。

## 變化 1：SQL 標準的演進

首先是 SQL 標準本身的進化。

過去像「JSON 用 MongoDB、圖資料用 Neo4j」那樣，必須仰賴針對特定模型設計的專用資料庫。  
改變這個局面的，是 SQL 標準的演進。原本只有專用 DB 才擅長的功能，逐漸變成一般 SQL 語法的一部分。

- **SQL:2016** — 語言中加入 JSON 運算子
- **SQL:2023** — 新增原生 JSON 型別，並以新章節（ISO/IEC 9075-16、 **SQL/PGQ** ）把圖的模式比對納入標準 SQL [1](#fn-2)

此外，以圖來追蹤關係的處理（圖探索），也不再只是獨立資料庫類別的能力，而是可以寫在 `FROM` 子句中的一種語法。  
當這些都能作為標準 SQL 的一部分來使用時，為了這些功能再把資料庫拆開的技術理由就變弱了。

## 變化 2：AI 工作負載的出現

第二個變化，是 AI 工作負載的出現，使得同步延遲與資料落差不再能被接受。

RAG 與 AI 代理會在每次回答時從資料庫取出資料，再交給 LLM。這些搜尋同時需要「新鮮度、權限、篩選條件」三種特性；若採用多個專用資料庫再以同步方式串接，這三件事往往都得由應用程式自己補齊，實作成本很高。  
這一點後面會再詳細說明。

## 變化 3：獨立學術支撐的出現

如前所述， **「converged」** 這個詞原本是 Oracle 提出的。  
但後來，這種整併方向本身，也開始出現在不依賴單一廠商的學術文獻中被討論。

關聯式資料庫領域的重要學者 Michael Stonebraker（PostgreSQL 前身 Postgres 的研究者）與 Andrew Pavlo（卡內基美隆大學的資料庫研究者），在 2024 年的 SIGMOD Record 論文 [What Goes Around Comes Around… And Around…](https://db.cs.cmu.edu/papers/2024/whatgoesaround-sigmodrec2024.pdf) 中，檢視了 20 年來資料模型的演變，並得出以下結論：

- 文件資料庫與 RDBMS 正朝著「相互碰撞的軌道」前進，兩者差異會隨時間縮小，未來幾乎難以區分
- 向量資料庫本質上可視為「加上特殊索引（ANN）的文件導向 DBMS」；索引只是其中一項功能，而不是新資料庫架構的根基，因此不足以作為新產品類別的立論依據
- 全文檢索若能在 RDBMS 端持續改善，並整合進 SQL，就能避免另外再立一個獨立產品

也就是說，雖然這個詞一開始來自廠商，但它所對應的架構方向，也已經在獨立論文中被討論。

## 融合式資料庫與 AI 的關係

接下來看 **融合式資料庫與 AI 的關係** 。  
如前一章所述，RAG 與 AI 代理在搜尋時同時需要「 **新鮮度、權限、篩選條件** 」。  
如果用多個專用資料庫搭配同步管線來處理這三件事，就很容易產生 **同步落差** ；而 **融合式資料庫** 正是被拿來當作這種落差的解法。

先看落差是怎麼產生的。  
延續前面的例子，假設訂單資料放在 **關聯式 DB 或 DynamoDB** ，搜尋放在像 OpenSearch 這樣的 **搜尋引擎** ，嵌入向量放在像 **Pinecone** 這樣的 **向量儲存** ，各自用最擅長的專用資料庫處理，再透過 **同步管線** （CDC 或批次）將資料串起來。

在這種架構下，同一份資料會在多個地方存在副本，而在副本尚未同步完成前，搜尋端看到的仍然是舊資料。  
這種落差不是設計瑕疵，而是複製式架構本身的固有特性，甚至在 AI 出現之前就已經被指出。

只是當使用者是人時，這個問題往往還算可以接受。  
例如電商搜尋中的新品，晚幾分鐘才出現，通常還算可容忍；即使結果不夠理想，畫面前也還有人的判斷可以介入。

但 RAG 與 AI 代理不同， **每次回答都會先從資料庫取資料再交給 LLM，因此搜尋品質會直接決定回答品質** 。  
而這類搜尋同時需要以下三種特性：

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fc193802b-57e9-4782-924d-e51d31f112a4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=04bf7ba7eedc4d9b120cfd9219c3588e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fc193802b-57e9-4782-924d-e51d31f112a4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=04bf7ba7eedc4d9b120cfd9219c3588e)

- **新鮮度** — 剛更新的內容要能立刻反映在搜尋結果中。若是獨立的向量資料庫，在同步延遲期間，AI 看到的仍是舊資料
- **權限** — 「這個使用者能不能看這筆資料」要在搜尋過程中就生效。若資料庫只有一個，定義一次就好；但若資料存放在不同系統中，SQL、API、向量搜尋等不同存取路徑都得在應用層重新實作權限，這也可能形成資料外洩的缺口
- **篩選條件** — 能做像「這個客戶的、尚未解決的工單裡，與這個問題相似的案件」這類條件式搜尋。這通常需要把向量相似搜尋與關聯式條件篩選結合起來

在專用儲存系統的組合架構中，這三件事都得各自依賴同步管線或程式碼補強。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F3df5cd20-3ab1-4d14-af22-6476397bf39a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c2b76c907d6869cbee4c5d8879ea4db2)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F3df5cd20-3ab1-4d14-af22-6476397bf39a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c2b76c907d6869cbee4c5d8879ea4db2)

當索引與實際資料不同步時，LLM 不會因此「比較沒把握」。  
它會在依據舊事實的情況下，非常自信地答錯。

原文把這種狀態命名為 **State Vector Dissonance** （帶著強烈自信產生幻覺的狀態） [2](#fn-1) 。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F831fed78-8e57-4aba-8e26-84de1d8ecb0a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ab812089808978eb9a51de31976f3dc7)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F831fed78-8e57-4aba-8e26-84de1d8ecb0a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ab812089808978eb9a51de31976f3dc7)

如果只是聊天機器人，使用者還可能發現它講錯了；但若是會自動執行下單或退款的 AI 代理，這種延遲就會直接變成商業風險。

原文將這個轉變濃縮成以下一句話：

> AI did not create the multi-store consistency problem. It removed the tolerance for it.  
> （AI 並沒有創造多儲存系統的一致性問題，而是讓我們不再能容忍它）  
> — [What Is a Converged Database?](https://blogs.oracle.com/developers/what-is-a-converged-database-definition-five-tests-and-ai-use-cases)

若要解決搜尋端殘留舊資料的問題，也可以朝縮短同步管線的方向努力。但只要是複製傳遞機制，就算再快，資料到達前還是會有時間差，不可能完全為零。  
融合式資料庫採取的不是這條路，而是把資料統一放進單一引擎，直接消除同步管線。沒有複製，自然也就沒有等待資料送達的時間。

那麼，整合到什麼程度才算「沒有管線」？  
下一章的 5 個測試，就是用來判定這件事的標準。

## 判定是否可稱為「融合式」的 5 個測試

「可以儲存多種模型」是產品儲存層的屬性。  
而融合式，指的是其上層所提供的 **保證** 。  
原文用 5 個可驗證的測試，來區分這兩者。

| 測試 | 通過時 | 不通過時 |
| --- | --- | --- |
| **1\. 單一交易邊界** | 關聯式插入、文件寫入、向量更新可以放在同一個 ACID 交易中執行，回滾也會整體一起回復 | 中途失敗時，會留下「只更新了一部分」的半完成狀態 |
| **2\. 單一最佳化器** | 一個包含圖、JSON、向量與關聯式的查詢，只會產生一個已經做過成本評估的執行計畫 | 若資料庫分散，應用程式就得自己硬寫「先查圖資料庫，再逐筆查向量資料庫」的流程，沒有統一的查詢執行腦 |
| **3\. 單一一致性模型** | 不論透過哪個 API，都能成立「寫完立刻可讀」 | 因為有模型間同步管線，資料可見性會延遲 |
| **4\. 單一治理領域** | 同一套權限、列級政策、稽核記錄能套用到所有存取路徑 | SQL、文件 API、向量搜尋、圖等不同路徑都得在應用層各自重做權限，稽核也會分散 |
| **5\. 存取介面共享** | SQL、MongoDB 相容 API、REST 都只是同一引擎的不同投影，操作的是同一份資料 | 即使入口只有一個，只要後面串的是多個引擎，交易、一致性、權限等保證還是各自分離 |

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F2ae582df-b487-440e-8560-1a133f43e11d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4a9f7ee49255bbf746112344d6d52cc6)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F2ae582df-b487-440e-8560-1a133f43e11d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4a9f7ee49255bbf746112344d6d52cc6)

其中第 1 點與第 2 點，透過實際例子會更容易理解。

## 測試 1：4 種寫入放在同一個交易中

原文提供的驗證專案中，首先展示的是將以下 4 種寫入包進同一個交易的範例（節錄）：

```
INSERT INTO orders ...;        -- 關聯式訂單
INSERT INTO order_items ...;   -- 其明細
INSERT INTO events (data) VALUES (JSON('{"type":"order_placed"}'));  -- JSON 文件
UPDATE support_tickets SET embedding = TO_VECTOR('[...]', 8, FLOAT32) WHERE ticket_id = 1;  -- 向量更新
```

這 4 個動作，要嘛全部提交，要嘛全部作廢，沒有只成功一半的中間狀態。

若把同樣的 4 種寫入分散到多個專用儲存系統，則各系統的原子性只會停留在自己的邊界內，並沒有跨系統的交易 API。  
（例如 Pinecone 的官方文件就明確提到，索引更新是最終一致性，剛寫入的資料不會立刻可見。）  
這時候回滾就只能變成應用程式自己針對各種失敗情境去寫、去維護的補償流程。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Ff6bf9192-649a-4e63-a30c-0c2558b6a159.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2d13731d9cce9a71983c079d5748f080)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Ff6bf9192-649a-4e63-a30c-0c2558b6a159.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2d13731d9cce9a71983c079d5748f080)

## 測試 2：單一最佳化器與單一執行計畫

**最佳化器** 是資料庫裡負責根據統計資訊判斷「這個查詢用什麼順序、什麼方法執行最快」的核心腦袋。

如果資料庫是分散的，這個腦袋就無法直接處理跨系統查詢，結果就變成「先查圖資料庫，再在結果上逐筆查向量資料庫」這類順序，必須被硬寫進應用程式。  
原文把這種狀態形容成「連接順序被硬編碼到應用邏輯中的分散系統」。

在融合式的情境中，若對包含圖、JSON、向量與關聯式的語句執行 `EXPLAIN PLAN` ，四種模型會一起出現在 **同一份執行計畫** 中。  
原因在於，處理圖的 `GRAPH_TABLE` 運算子會先被轉換成一般 SQL，再由同一個最佳化器進行成本評估。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fab238dfb-6922-4c52-b9e8-857602920a6b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=20caf5619381cb97603ed152909fcdc5)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fab238dfb-6922-4c52-b9e8-857602920a6b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=20caf5619381cb97603ed152909fcdc5)

## 測試 3～5：一致性、治理與存取介面

- **單一一致性模型** — 因為模型之間沒有同步管線，所以不論從哪個 API 寫入，都能成立「寫完立刻可讀」
- **單一治理領域** — 權限、列級政策、稽核記錄只要定義一次，就能套用到所有入口（SQL、文件 API、向量搜尋、圖）；稽核也能集中成一條
- **存取介面共享** — SQL、MongoDB 相容 API、REST 都只是同一引擎的投影，操作的是同一份資料。若只是把多個引擎包在同一個 gateway 後面，入口看似統一，內部的交易、一致性、權限保證仍然各自為政

---

✓ **到目前為止的整理** ：可以儲存多種模型，只是前提。真正重要的是，這些模型之間的保證——交易、最佳化器、一致性、治理、存取介面——是否跨模型維持為一體。能否同時通過這 5 個測試，就是是否能稱為融合式的分水嶺。

（能儲存多種模型的資料庫通常稱為多模型資料庫。原文有討論它與融合式的比較，但本文因篇幅較大不深入展開。）

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F16dee12c-3b5f-4043-9fa0-b15b4535e403.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e85e4cd71b392cf4a4ff0f0d47fa5a1a)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F16dee12c-3b5f-4043-9fa0-b15b4535e403.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e85e4cd71b392cf4a4ff0f0d47fa5a1a)

## 為什麼文件 DB 會從關聯式 DB 分出去

前面談的是統一保存／融合的方向（convergence），這裡也補充一下分化（divergence）的脈絡。

關於這個分化的原因，不同立場會有不同解釋。

從關聯式 DB 的角度，有人會說 NoSQL 的流行只是行銷先行的繞路。  
前面提到的 Stonebraker 與 Pavlo 論文，則把這個分化主要解釋為對 **阻抗不匹配** 的不滿。  
在程式裡，資料常以「客戶底下有訂單、訂單底下有明細」這種巢狀形式存在，但到了關聯式 DB 中，卻得拆成多個表保存，讀取時再透過 JOIN 組裝回來。  
應用程式與資料庫之間這種形狀上的不一致，就是阻抗不匹配；而文件資料庫可以直接把物件存成 JSON，正好成為這種不滿的出口。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F3e357e4d-795c-45b3-9bdb-73d9d9b41339.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=081e938a76d62f022c5e8053824378fa)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F3e357e4d-795c-45b3-9bdb-73d9d9b41339.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=081e938a76d62f022c5e8053824378fa)

另一方面，本文作者 Rick Houlihan 曾在 AWS 建立並教授 DynamoDB 的 **單表設計** ，可說是「分出去的那一方」的親身參與者；他認為這個分化其實是基於工作負載實測後所做出的設計判斷。  
他引用的根據，是 Amazon CTO Werner Vogels 公開過的、Amazon 從關聯式轉移時的工作負載分析：

- 約 70% 的關聯式操作，只是根據 key 取單筆資料
- 另外約 20% 的操作，只是從單一表回傳多筆資料

也就是說，約 9 成的操作，其實都不需要關聯式資料庫最擅長的 JOIN。  
當存取模式已經如此明確時，比起每次請求都付出 JOIN 的成本，不如預先把資料整理成最終要讀取的樣子，也就是做 **非正規化** ，這樣會更快。  
因此，分出去的理由不是反對理論，而是為了提升讀取效能的設計選擇。

但這種設計選擇帶來的非正規化，也就是「保留副本」與「同一筆資料重複存在」，其實正是關聯理論透過正規化想要移除的 **冗餘性** 。  
如果是為了讀取效能，那麼理論原本想避免的東西，真的可以再加回設計裡嗎？

## 關聯式理論有禁止冗餘嗎

原文在這裡回溯到關聯式模型的提出者 E. F. Codd（柯德）本人的論文。  
Codd 並不是把冗餘當成「絕對禁止」，而是把它視為要依工作負載權衡的選項。

- **1969 年研究報告** — 只有在查詢負載比其他操作重得多的環境下，強烈冗餘才是合理的
- **1970 年論文** — 冗餘是一種交易：用「更多儲存空間與更新時間」換取「查詢時間改善」

如果讀取多，就可以容許重複；如果寫入多，就偏向正規化。  
冗餘從一開始就是理論中的一個調整旋鈕，會隨讀寫比例而改變。  
原文認為，文件資料庫的設計者所調的，其實也是同一個旋鈕。

同一份 1969 年報告裡，Codd 還提出了另一個區分：  
**資料的「邏輯形態」** 與 **「物理儲存方式」** 的分離。

## 將邏輯與物理解耦的機制重新做起來

依照 Codd 的區分， **邏輯模型** （已正規化的關係集合）應保持正規化，而 **儲存表示** 則可以依工作負載來選擇。  
也就是說，邏輯設計不要動，只有資料放法可以隨讀寫需求調整。

但在文件資料庫中，這兩件事是綁在一起的。  
因為保存的文件長相，本身就等於你想讀取的答案長相。  
需要的資料集中在同一處，讀取自然會快；但當存取模式改變時，就得連資料形狀本身一起重做。

原文把「把這個分離重新用現代方式做回來」的機制，稱作 **JSON Relational Duality View** 。  
這不是 SQL 標準內建的功能，而是 Oracle 專有功能。它在 2023 年的 Oracle Database 23c 中登場，並延續到目前（2026 年 8 月）的最新版本 Oracle AI Database 26ai。

**Duality View** 的概念，是把 `customers` 、 `orders` 、 `order_items` 這些表，定義成一份像下面這樣的巢狀 JSON 文件，並用檢視表（ `CREATE JSON RELATIONAL DUALITY VIEW` ）的方式宣告出來。  
之後，便可以透過相容 MongoDB 的文件 API 直接以文件方式讀寫，而底層實體其實仍然是正規化的表格列。  
換句話說，並不是另外存一份 JSON 副本，而是同一份資料同時是「文件」，也是那些表列。

```
{
  "_id": 42,
  "_metadata": { "etag": "E9BA8572B721D85E653B49930B83D911" },
  "email": "customer42@example.com",
  "fullName": "Customer 42",
  "segment": "standard",
  "orders": [
    { "orderId": 90, "status": "delivered", "total": 273.96,
      "items": [ { "line": 1, "productId": 32, "qty": 3, "unitPrice": 273.96 } ] }
  ]
}
```

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fe99df4bc-8d9d-4cc3-9837-cdd9b117d80f.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=209daa9cf8c41681eaa014e1bf86ab33)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fe99df4bc-8d9d-4cc3-9837-cdd9b117d80f.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=209daa9cf8c41681eaa014e1bf86ab33)

原文的驗證腳本中，也示範了透過文件 API 更新客戶屬性，並能用同一個引擎內的 SQL 讀回相同變更。

```
// 用文件 API 更新…
col.updateOne({ _id: 42 }, { $set: { segment: 'vip' } });

// …再用同一個引擎的 SQL 讀回（沒有同步管線）
db.aggregate([{ $sql: 'SELECT segment FROM customers WHERE customer_id = 42' }]);
```

當同時有更新衝突時，則透過文件上的 `etag` （用來偵測變更的標籤）來進行偵測與控制。  
也就是說，文件世界保有文件 API 的使用體驗，關聯式世界則保有約束、統計與 JOIN，而且這些都是建立在同一份資料上。

原文將這個章節的思路總結為「建模領域，投影存取」（Model the domain. Project the access.）。  
（意思是：資料本體以正規化表來設計，而從應用角度看到的樣子，則以投影，也就是檢視表的方式宣告。）  
要不要把關聯資料嵌進文件中，或是拆開後再引用，這類文件設計沒有通用標準答案，得依讀寫比例與存取模式來決定。  
融合式也不改變這點。真正改變的是，未來若要重新調整，成本會低很多。  
如果文件只是正規化列的投影，那麼存取模式改變時，往往只需要修改「投影定義」，而不必遷移整份資料。

## 用單一引擎搭出小型電商的例子

回到一開始提到的小型電商範例。

接下來要確認，上述內容在 Oracle Database 中實際會怎麼實作。

原文已將這個範例實作到驗證儲存庫中（200 位客戶、1,000 筆訂單、300 筆含嵌入向量的客服工單、推薦與裝置圖、門市地點資訊），文中的程式碼範例也都在那裡被實際驗證過。

不是替 5 種資料形態各建一個資料庫，而是只儲存正規化的表。  
同一份資料，不靠副本，也不靠同步管線，就能從 5 種模型來使用。  
每個模型的實現方式，要嘛是在表上加一層「呈現定義」，要嘛是在表上加欄位；但無論哪一種，真正的儲存實體都還是表，不會新增其他儲存位置。

| 模型 | 實作方式 |
| --- | --- |
| **表（關聯式）** | 直接使用正規化表，這就是儲存實體 |
| **JSON（文件）** | 在表之上定義一個以巢狀 JSON 呈現的檢視表（ `CREATE JSON RELATIONAL DUALITY VIEW` ，也就是前述 Duality View） |
| **圖** | 在表之上增加節點與邊的定義（ `CREATE PROPERTY GRAPH` ） |
| **向量** | 在表中加入 `VECTOR` 型別欄位 |
| **空間** | 在表中加入空間型別欄位，用來保存位置資訊 |

在這個基礎上，跨模型查詢就能以單一語句完成。  
例如，以某位客戶為起點，先沿著推薦圖走 1 到 4 跳，找到相關客戶，再把這些客戶的未解決工單依向量相似度排序，最後再做關聯式 join 的例子如下（節錄）：

```
WITH ring AS (
  SELECT DISTINCT cid FROM GRAPH_TABLE (customer_graph
    MATCH (a IS customers) -[IS referrals]->{1,4} (b IS customers)
    WHERE a.customer_id = 10
    COLUMNS (b.customer_id AS cid))
)
SELECT c.customer_id
FROM ring r
JOIN customers c        ON c.customer_id = r.cid
JOIN support_tickets st ON st.customer_id = c.customer_id
WHERE st.status IN ('open','pending')
ORDER BY VECTOR_DISTANCE(st.embedding, :query_vector, COSINE)
FETCH FIRST 10 ROWS ONLY;
```

另外，原文中的這些主張——例如「4 種寫入可放在同一個交易中」、「執行計畫只有一份」、「寫完立刻可讀」——都已經作為公開儲存庫 [converged-database-lab](https://github.com/oracle-devrel/oracle-umt-developer-hub) 裡的腳本實際執行過。  
其中有 5 份驗證腳本、20 個斷言，都是在實際驗證環境中自動執行，且執行狀況是公開的。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Ff79c6bdd-b61b-4495-9792-af8c61bb3d4d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=14186bf201389eebb23e6f3ca32874d1)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Ff79c6bdd-b61b-4495-9792-af8c61bb3d4d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=14186bf201389eebb23e6f3ca32874d1)

## 總結

「融合式資料庫」並不是指能儲存多種資料模型，而是指交易、最佳化器、一致性、治理、存取介面這些保證，能跨模型維持為一體。

本文整理的重點如下：

- **定義** — 融合式資料庫不是指多模型儲存能力（multi-model），而是指交易、最佳化器、一致性、治理、存取介面這些 **保證跨模型合而為一** 。判定標準就是那 5 個測試
- **與 AI 的關係** — RAG 與 AI 代理的搜尋同時需要「新鮮度、權限、篩選條件」，若用多個專用儲存系統再同步串接，這三件事都會變成應用程式要自己補的。融合式的答案不是「更快的同步」，而是 **沒有同步管線**
- **分化背景與 Duality View** — 文件資料庫分出去，原本是為了讓讀取更快。冗餘在關聯理論中也一直是讀寫取捨的一部分；Duality View 則是把「邏輯維持正規化、呈現方式作為投影宣告」這件事重新做回來
- **實作型態** — 真正儲存的只有正規化表；各模型則透過在表上加定義或加欄位來實現。寫入可以收斂到單一交易，跨模型查詢也能收斂成單一執行計畫

補充一點，原文討論的是 **結構與保證** ，不是效能比較。  
「融合式是否比較快」不是本文或原文要回答的問題，閱讀時請把這兩者分開。

如果這篇文章能讓你在思考 AI 時代的資料基礎架構時多一點參考，那就太好了。

## 參考

- [What Is a Converged Database? Definition, Five Tests, and AI Use Cases](https://blogs.oracle.com/developers/what-is-a-converged-database-definition-five-tests-and-ai-use-cases) — Rick Houlihan，Oracle Developers Blog（本文基礎）
- [converged-database-lab（驗證儲存庫）](https://github.com/oracle-devrel/oracle-umt-developer-hub)
- [M. Stonebraker, A. Pavlo, "What Goes Around Comes Around… And Around…"（SIGMOD Record, 2024）](https://db.cs.cmu.edu/papers/2024/whatgoesaround-sigmodrec2024.pdf)
- [M. Colgan, "What is a Converged Database?"（2020）](https://sqlmaria.com/2020/03/05/what-is-a-converged-database/)
- [P. Eisentraut, "SQL:2023 is finished: Here is what's new"](https://peter.eisentraut.org/blog/2023/04/04/sql-2023-is-finished-here-is-whats-new)
- [W. Vogels, "A Decade of Dynamo"（2017）](https://www.allthingsdistributed.com/2017/10/a-decade-of-dynamo.html)
- [E. F. Codd, "A Relational Model of Data for Large Shared Data Banks"（CACM, 1970）](https://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf)
- [J. Lu, I. Holubová, "Multi-model Databases: A New Journey to Handle the Variety of Data"（ACM Computing Surveys, 2019）](https://dl.acm.org/doi/10.1145/3323214)
- [Oracle JSON-Relational Duality Developer's Guide](https://docs.oracle.com/en/database/oracle/oracle-database/26/jsnvu/overview-json-relational-duality-views.html)
- [Oracle Database API for MongoDB](https://docs.oracle.com/en/database/oracle/mongodb-api/mgapi/overview-oracle-database-api-mongodb.html)
1. SQL/PGQ = Property Graph Queries。已於 ISO/IEC 9075-16:2023 中標準化。
2. State Vector Dissonance 不是已有固定中文譯名的學術術語，而是原文作者自行命名的詞。

---

原文出處： [https://qiita.com/yushibats/items/9dd91baaa89c919d0992](https://qiita.com/yushibats/items/9dd91baaa89c919d0992)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/ag2LM3) [53瀏覽](https://codelove.tw/@tony/post/ag2LM3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/ag2LM3)

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

238

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

4

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1786583257&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2LM3&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786583254789&amp;bpp=37&amp;bdt=138&amp;idt=2464&amp;shv=r20260811&amp;mjsv=m202608070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786501442%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2580692748476&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=852&amp;scr_x=0&amp;scr_y=0&amp;eid=95397133&amp;oid=2&amp;pvsid=6505834522934800&amp;tmod=1216897310&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C852&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=2662" title="Advertisement" aria-label="Advertisement"></iframe>