---
title: "Skills vs MCP：AI 工具如何演進"
source: "https://codelove.tw/@tony/post/3bp4Jq"
author:
published:
created: 2026-08-03
description: "十八個月前，MCP 還是*那個*最熱門的東西。每個 demo 和聊天機器人連接器，底層都在用 MCP。當我開始看到大家都在談論 *Skills* 時，我就很好奇：MCP 和 Skills 之間有什麼關聯？它們只是同一個問題的不同切面嗎？我在最新的影片裡再次把這些問題帶去問 [@greggyb](..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月31日](https://codelove.tw/@tony/post/3bp4Jq "2026-07-31 12:03:15")

十八個月前，MCP 還是 *那個* 最熱門的東西。每個 demo 和聊天機器人連接器，底層都在用 MCP。當我開始看到大家都在談論 *Skills* 時，我就很好奇：MCP 和 Skills 之間有什麼關聯？它們只是同一個問題的不同切面嗎？

機器學習與人工智慧

我在最新的影片裡再次把這些問題帶去問 [@greggyb](https://dev.to/greggyb) 。

{% card

## 影片內容

- MCP 實際在做什麼：透過自然語言，讓模型能以程式化方式存取 API 和檔案系統
- MCP 的缺點：即使你根本沒用到那些工具，也得載入數百行工具定義，造成上下文膨脹
- Skill 到底是什麼（劇透：其實就是一個包含可重用提示詞的 Markdown 檔案）
- 漸進式揭露：為什麼代理可以「知道有」20 個 skills，卻不需要一次載入所有完整指令
- 為什麼 MCP 和 Skills 不是競爭關係，以及 Skills 甚至可以教你的代理 *如何* 使用 MCP 工具  
	{% endcard

這支影片裡我最喜歡的一句話是：現在最熱門的程式語言就是英文。Skills 讓非開發者也能撰寫、反覆調整並分享指令，讓整個團隊的代理更快，而且完全不需要寫程式。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785721245&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3bp4Jq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721204913&amp;bpp=8&amp;bdt=987&amp;idt=8&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721193%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721193%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721193%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1619x969&amp;nras=3&amp;correlator=1916150511592&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=1281&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396138&amp;oid=2&amp;pvsid=5906346862570676&amp;tmod=1434000446&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=40247" title="Advertisement" aria-label="Advertisement"></iframe>

[https://www.youtube.com/watch?v=T6PQIPMlj3E](https://www.youtube.com/watch?v=T6PQIPMlj3E)

![](https://www.youtube.com/watch?v=T6PQIPMlj3E)

你現在有在使用 Skills 嗎？我很好奇大家目前的狀況。

---

原文出處： [https://dev.to/googleai/skills-vs-mcp-how-ai-tools-have-evolved-3pmk](https://dev.to/googleai/skills-vs-mcp-how-ai-tools-have-evolved-3pmk)

機器學習與人工智慧

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3bp4Jq) [31瀏覽](https://codelove.tw/@tony/post/3bp4Jq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3bp4Jq)

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

📝10 ❤️4

295

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 💬4 ❤️1

58

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="350" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=1414751727&amp;adf=3620188380&amp;pi=t.aa~a.4029027559~rp.3&amp;abgtt=6&amp;w=350&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785721245&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=350x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3bp4Jq&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721204948&amp;bpp=2&amp;bdt=1021&amp;idt=2&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721193%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721193%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721193%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1619x969%2C952x280&amp;nras=4&amp;correlator=1916150511592&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=1160&amp;ady=1079&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396138&amp;oid=2&amp;pvsid=5906346862570676&amp;tmod=1434000446&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=40221" title="Advertisement" aria-label="Advertisement"></iframe>

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1200" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=3809598800&amp;adf=1056458448&amp;pi=t.aa~a.1416458249~rp.1&amp;abgtt=6&amp;w=1200&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785721245&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=1200x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3bp4Jq&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721204961&amp;bpp=1&amp;bdt=1035&amp;idt=1&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721193%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721193%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721193%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1619x969%2C952x280%2C350x280&amp;nras=5&amp;correlator=1916150511592&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=210&amp;ady=2978&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396138&amp;oid=2&amp;pvsid=5906346862570676&amp;tmod=1434000446&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=40213" title="Advertisement" aria-label="Advertisement"></iframe>

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1785721204&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3bp4Jq&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721204048&amp;bpp=1&amp;bdt=121&amp;idt=102&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721193%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721193%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721193%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1916150511592&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396138&amp;oid=2&amp;pvsid=5906346862570676&amp;tmod=1434000446&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=138" title="Advertisement" aria-label="Advertisement"></iframe><iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260729/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=6&amp;uci=a!6&amp;btvi=4" title="Advertisement" aria-label="Advertisement"></iframe>

Java (程式語言)