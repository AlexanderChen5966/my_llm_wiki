---
title: "讀後筆記：從「Agentic Engineering Patterns」看 AI 時代的程式開發新思維"
source: "https://www.mxp.tw/12789/"
author:
  - "[[Chun]]"
published: 2026-03-05
created: 2026-05-19
description: "Agentic Engineering Patterns 精華筆記：在 AI 時代重新理解「程式開發」 編輯前言 ：Simon Willison 的這篇文章雖然篇幅精簡，但它勾勒出的其實是一套「如何與 AI 程式代理（coding agents）協作」的工作框架。對於正在使用 Claude Code、OpenAI Co"
tags:
  - "clippings"
status: processed
---
> 本篇文章更新時間：2026/03/05  
> 如有資訊過時或語誤之處，歡迎使用 [Contact](https://www.mxp.tw/contact/) 功能通知或向 [一介資男的 LINE 社群](https://mxp.tw/atechguyline) 反應。  
> 如果本站內容對你有幫助，歡迎 [贊助支持](https://mxp.tw/lw) 。

內容目錄 \[[隱藏](#)\]

## Agentic Engineering Patterns 精華筆記：在 AI 時代重新理解「程式開發」

> **編輯前言** ：Simon Willison 的這篇文章雖然篇幅精簡，但它勾勒出的其實是一套「如何與 AI 程式代理（coding agents）協作」的工作框架。對於正在使用 Claude Code、OpenAI Codex 或其他 AI 工具寫程式的人來說，這些模式值得反覆思考。

來源文章： [Agentic Engineering Patterns - Simon Willison's Weblog](https://simonwillison.net/guides/agentic-engineering-patterns/)

### 核心觀點 (Key Takeaways)

- **寫程式變便宜了** ：AI 讓「嘗試」變得低成本，探索性開發因此更重要。
- **把自己擅長的事集中管理** ：作者強調要「囤積」自己知道怎麼做的事情，因為這些能讓你更有效率地指揮 AI。
- **避免常見反模式** ：例如過度依賴模型、沒有提供足夠上下文等。
- **測試導向的開發會變得更重要** ：TDD（red/green）依然有效，甚至因 AI 的存在而更關鍵。
- **理解程式碼需要新的方法** ：像是線性 walkthrough 或互動式解說。

### 深入解析

<iframe width="1200" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1939980533457134&amp;output=html&amp;h=280&amp;adk=266443921&amp;adf=3185292558&amp;pi=t.aa~a.308136647~rp.1&amp;w=1200&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779160292&amp;rafmt=1&amp;to=qs&amp;pwprc=5379313424&amp;format=1200x280&amp;url=https%3A%2F%2Fwww.mxp.tw%2F12789%2F&amp;host=ca-host-pub-2644536267352236&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTY4IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNjgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE2OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779160293572&amp;bpp=1&amp;bdt=945&amp;idt=-M&amp;shv=r20260518&amp;mjsv=m202605140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D72d0fddc272de5a6%3AT%3D1779160293%3ART%3D1779160293%3AS%3DALNI_MZSBZUSmR4tNyUaqTdvzbR9AYyJEA&amp;gpic=UID%3D000013ff4f08c6d6%3AT%3D1779160293%3ART%3D1779160293%3AS%3DALNI_Mb3212C8TrrOkaDK4hVUSzLUNXznQ&amp;eo_id_str=ID%3D58b60e591395d91e%3AT%3D1779160293%3ART%3D1779160293%3AS%3DAA-AfjYL9xVkTxVEiJtwLS3KnXFD&amp;prev_fmts=0x0%2C1200x280%2C1160x280%2C1200x280%2C1164x280%2C1200x280&amp;nras=2&amp;correlator=8222393996010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=3&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=2149&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390681&amp;oid=2&amp;psts=AOrYGskhD8I8Lbv1JkXwJMqHjPgNH7-TgxR8qYfFkwYbtzLZU5SltIcBy1XXdtsW2F1BBN1BezdwiphZgINIuMWTqgoq2bkCsCHEsfue&amp;pvsid=7590994383353436&amp;tmod=271646502&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1892%2C985%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;pgls=CAEQBBoHMS4xNzguMA..~CAA.~CAA.&amp;ifi=10&amp;uci=a!a&amp;btvi=5&amp;fsb=1&amp;dtd=10" title="Advertisement" aria-label="Advertisement"></iframe>

Simon 這篇文章有點像是一本「索引」，把他近期在研究 coding agents 的心得整理成架構。雖然內容大多是章節標題，但光看分類就能感覺到他正在建立一套方法論。

> 核心精神是：AI 能寫很多程式碼，但人類需要提供方向、建立檢查機制、並掌握知識庫。

<iframe width="1200" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1939980533457134&amp;output=html&amp;h=280&amp;adk=266443921&amp;adf=1393154336&amp;pi=t.aa~a.308119557~rp.1&amp;w=1200&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779160292&amp;rafmt=1&amp;to=qs&amp;pwprc=5379313424&amp;format=1200x280&amp;url=https%3A%2F%2Fwww.mxp.tw%2F12789%2F&amp;host=ca-host-pub-2644536267352236&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTY4IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNjgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE2OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779160293572&amp;bpp=1&amp;bdt=945&amp;idt=-M&amp;shv=r20260518&amp;mjsv=m202605140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D72d0fddc272de5a6%3AT%3D1779160293%3ART%3D1779160293%3AS%3DALNI_MZSBZUSmR4tNyUaqTdvzbR9AYyJEA&amp;gpic=UID%3D000013ff4f08c6d6%3AT%3D1779160293%3ART%3D1779160293%3AS%3DALNI_Mb3212C8TrrOkaDK4hVUSzLUNXznQ&amp;eo_id_str=ID%3D58b60e591395d91e%3AT%3D1779160293%3ART%3D1779160293%3AS%3DAA-AfjYL9xVkTxVEiJtwLS3KnXFD&amp;prev_fmts=0x0%2C1200x280%2C1160x280%2C1200x280%2C1164x280%2C1200x280%2C1200x280&amp;nras=3&amp;correlator=8222393996010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=3&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=2610&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390681&amp;oid=2&amp;psts=AOrYGskhD8I8Lbv1JkXwJMqHjPgNH7-TgxR8qYfFkwYbtzLZU5SltIcBy1XXdtsW2F1BBN1BezdwiphZgINIuMWTqgoq2bkCsCHEsfue&amp;pvsid=7590994383353436&amp;tmod=271646502&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1892%2C985%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;pgls=CAEQBBoHMS4xNzguMA..~CAA.~CAA.&amp;ifi=11&amp;uci=a!b&amp;btvi=6&amp;fsb=1&amp;dtd=11" title="Advertisement" aria-label="Advertisement"></iframe>

以下是我從每個區塊延伸出的理解：

- **寫程式變便宜了** ：當生成程式碼的成本趨近於零，與其糾結於「寫得漂不漂亮」，更重要的是清楚描述意圖、快速實驗、快速測試。這讓我想到過去大家常說「寫程式是最便宜的事情，設計才是最貴的」。現在 AI 讓這句話更加明確。
	程式設計
- **囤積技能與範例** ：作者提到「hoard things you know how to do」，我解讀為：把你知道的指令、腳本、常用 pattern 集中起來，當作 prompt 的素材。AI 很強，但如果給的 context 不夠，它就無法達成高品質的結果。
- **測試與 QA** ：因為 AI 的輸出具有不穩定性，測試反而成為品質保證的核心。文章中列出 Red/Green TDD、先跑測試等模式，代表作者認為未來工程師會更重視「如何驗證程式碼」，而不是「如何撰寫程式碼」。
- **理解代碼的方式也在變** ：像是線性 walkthrough（逐步走過程式碼）或互動問答，都是現在 AI 非常擅長的能力，能用來理解既有系統或不熟悉的程式碼庫。

### 筆者心得與啟發

<iframe width="1200" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1939980533457134&amp;output=html&amp;h=280&amp;adk=266443921&amp;adf=1243956916&amp;pi=t.aa~a.308122700~rp.1&amp;w=1200&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779160292&amp;rafmt=1&amp;to=qs&amp;pwprc=5379313424&amp;format=1200x280&amp;url=https%3A%2F%2Fwww.mxp.tw%2F12789%2F&amp;host=ca-host-pub-2644536267352236&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTY4IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNjgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE2OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779160293572&amp;bpp=1&amp;bdt=946&amp;idt=1&amp;shv=r20260518&amp;mjsv=m202605140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D72d0fddc272de5a6%3AT%3D1779160293%3ART%3D1779160293%3AS%3DALNI_MZSBZUSmR4tNyUaqTdvzbR9AYyJEA&amp;gpic=UID%3D000013ff4f08c6d6%3AT%3D1779160293%3ART%3D1779160293%3AS%3DALNI_Mb3212C8TrrOkaDK4hVUSzLUNXznQ&amp;eo_id_str=ID%3D58b60e591395d91e%3AT%3D1779160293%3ART%3D1779160293%3AS%3DAA-AfjYL9xVkTxVEiJtwLS3KnXFD&amp;prev_fmts=0x0%2C1200x280%2C1160x280%2C1200x280%2C1164x280%2C1200x280%2C1200x280%2C1200x280&amp;nras=4&amp;correlator=8222393996010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=3&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=3723&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390681&amp;oid=2&amp;psts=AOrYGskhD8I8Lbv1JkXwJMqHjPgNH7-TgxR8qYfFkwYbtzLZU5SltIcBy1XXdtsW2F1BBN1BezdwiphZgINIuMWTqgoq2bkCsCHEsfue&amp;pvsid=7590994383353436&amp;tmod=271646502&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1892%2C985%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;pgls=CAEQBBoHMS4xNzguMA..~CAA.~CAA.&amp;ifi=12&amp;uci=a!c&amp;btvi=7&amp;fsb=1&amp;dtd=11" title="Advertisement" aria-label="Advertisement"></iframe>

這篇文章雖然只是骨架，但它點出了 AI 時代工程師的三個關鍵能力：

1. **能夠描述問題（problem framing）** ：因為程式碼由 AI 生成，你要把更多時間花在說清楚「你要什麼」。
2. **能夠驗證品質（testing & QA）** ：測試技能比以往更加基礎。
3. **能夠管理知識（context management）** ：越懂得提供上下文，你越能發揮 AI 的力量。

<iframe width="1200" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1939980533457134&amp;output=html&amp;h=280&amp;adk=266443921&amp;adf=2361509382&amp;pi=t.aa~a.308147063~rp.1&amp;w=1200&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779160292&amp;rafmt=1&amp;to=qs&amp;pwprc=5379313424&amp;format=1200x280&amp;url=https%3A%2F%2Fwww.mxp.tw%2F12789%2F&amp;host=ca-host-pub-2644536267352236&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTY4IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNjgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE2OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779160293577&amp;bpp=1&amp;bdt=950&amp;idt=1&amp;shv=r20260518&amp;mjsv=m202605140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D72d0fddc272de5a6%3AT%3D1779160293%3ART%3D1779160293%3AS%3DALNI_MZSBZUSmR4tNyUaqTdvzbR9AYyJEA&amp;gpic=UID%3D000013ff4f08c6d6%3AT%3D1779160293%3ART%3D1779160293%3AS%3DALNI_Mb3212C8TrrOkaDK4hVUSzLUNXznQ&amp;eo_id_str=ID%3D58b60e591395d91e%3AT%3D1779160293%3ART%3D1779160293%3AS%3DAA-AfjYL9xVkTxVEiJtwLS3KnXFD&amp;prev_fmts=0x0%2C1200x280%2C1160x280%2C1200x280%2C1164x280%2C1200x280%2C1200x280%2C1200x280%2C1200x280%2C1652x938%2C1005x124&amp;nras=7&amp;correlator=8222393996010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=4&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=3400&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=83&amp;eid=95390681&amp;oid=2&amp;psts=AOrYGskhD8I8Lbv1JkXwJMqHjPgNH7-TgxR8qYfFkwYbtzLZU5SltIcBy1XXdtsW2F1BBN1BezdwiphZgINIuMWTqgoq2bkCsCHEsfue%2CAOrYGslg3fH3uNghJsobo5gAddHvl3AHqc75EyYDmY7H9o1XmFOojfCV1ovLJPSlTLGTshdnJdXxl9LLiORVSGrTGVErsOekF-IJopUTDFQ4Ink5xt8MSo8cszfd5g&amp;pvsid=7590994383353436&amp;tmod=271646502&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1892%2C985%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;pgls=CAEQBBoHMS4xNzguMA..~CAA.~CAA.&amp;ifi=13&amp;uci=a!d&amp;btvi=9&amp;fsb=1&amp;dtd=354" title="Advertisement" aria-label="Advertisement"></iframe>

對我而言，最深的啟發是：我們應該開始把 AI 視為「協作工程師」，而不是「工具」。這代表我們要建立流程，讓人與 agent 能相互補強，而不是期待 AI 一次完成所有事情。換句話說，工程思維正在從「寫程式」轉向「編排 agent」。

企業資源規劃 (ERP)

未來我也會嘗試將自己的常用程式設計 Pattern、Prompt、測試規範整理成可重複利用的資源，讓 AI 能更懂我、也更有效率地幫我寫程式。