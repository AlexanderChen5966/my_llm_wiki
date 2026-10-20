---
title: "交付速度與 AI 導入後的維護成本"
source: "https://codelove.tw/@tony/post/qenNBx"
author:
published:
created: 2026-08-28
description: "交付變得太快了。問題是，維護成本卻還是跟以前一樣。## 目錄- [帳單到來的那一天](#帳單到來的那一天)- [我們只是在慶祝便宜的那一部分](#我們只是在慶祝便宜的那一部分)- [如果這東西凌晨 3 點壞掉，我能不用重開聊天視窗就除錯嗎？](#如果這東西凌晨-3-點壞掉我能不用重開聊天..."
tags:
  - "clippings"
status: "processed"
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月28日](https://codelove.tw/@tony/post/qenNBx "2026-08-28 10:03:13")

交付變得太快了。問題是，維護成本卻還是跟以前一樣。

## 目錄

## 帳單到來的那一天


我先坦白一件事。

今年有個功能，我一個上午就交付了。真的是一個上午。分析公式的 CRUD：客戶想自己設定蟲害發生率的計算方式，而不是要我們直接寫死在程式裡。Migration、4 條新路由、一個 949 行的畫面，以及最棒的部分——把 208 行寫死的計算策略，換成 50 行去呼叫公式評估器。上午 11:17 前端 PR 就開了，11:32 後端 PR 也開了。我中午去吃飯時還覺得自己很厲害。

兩週後，3 種新的分析類型出現了。然後有人（就是我）進到伺服器裡，把那 3 種寫死的策略又加回去了，放回那個通用引擎在 14 天前已經淘汰掉的 map 裡。這個 diff 是從舊版檔案開始的。寫那段的人根本沒找到新路徑，也沒理解已經有新路徑了。

27 天後我又回去了，因為當某個分析類型沒有公式時，程式會 `throw new Error(JSON.stringify({...}))` 。一個把 JSON 塞進錯誤訊息裡的例外，HTTP handler 還得把它打開才能知道該對使用者說什麼。修這件事花了 35 行程式碼和 175 行測試。測試比程式碼多 5 倍，卻是在補一個我本來就應該在那個「交付完成」的早上想清楚的情境。

今天，5 個月後，那個檔案有 268 行，而且還留著我寫的註解： `// Keep for backwards compatibility with tests` 。一個 16 個專案的 map，沒有任何一行正式程式碼會讀它。它之所以還在，是因為 14 個測試斷言依賴它，而沒人（包括我）有空把它拆掉。那個測試檔有 351 行，比它測試的服務還長。

我省下的是那個上午。後面三次回頭去改，沒有省到。

喔，先說清楚，免得有人誤會：我每天都在用 AI，什麼都用。這不是那種「AI 很爛」的說法。不是那回事。

## 我們只是在慶祝便宜的那一部分

想想看，AI 真的把什麼變便宜了。

樣板碼、DTO、migration、你已經寫過 40 次的那種 CRUD、新模組的骨架、理解一個你從沒打開過的函式庫。這些都變得便宜得離譜，而我一點都不想把它們還回去。

但注意：這些全都是程式碼 **誕生** 的那一刻。

可軟體不是一直都在誕生。軟體的一生，是被閱讀、被除錯、在週五被緊急修改、以及被上個月才加入團隊的人解釋。這一部分到今天還是跟 2019 年一樣貴。也許更貴，因為現在程式碼更多了。

所以當你看著成果說「我現在效率是以前的 3 倍」時，這可能是真的。但你大概只是在量那個本來就已經是流程中最快的部分。

## 如果這東西凌晨 3 點壞掉，我能不用重開聊天視窗就除錯嗎？

如果這篇文章我只能留下問一句話，那我會選這個標題。你在 merge 之前先問自己這句。

如果答案是否定的，那你還沒完成。你只是把問題往前推了。

還有一個更嚴苛的版本：你能不能把它為什麼要那樣寫，解釋給一個從沒看過這段程式的人聽？因為如果你做不到，那這個檔案真正的 owner 就不是你。模型也不會在事故當天跟你一起待命，這點我可以保證。

再說一次，重點不是什麼都要手刻。重點是接受之前先讀。真的要讀，逐行讀，必要時手指著螢幕讀。

讀 200 行大概 10 分鐘。除錯你從沒讀過的 200 行，大概是一整天。這筆帳從來沒變過，也不會變。

而且有件事是我後來才發現的：AI 幾乎從來不會只給你最小解。它通常會給你完整解。帶著你沒要求的抽象層、一個為了「彈性」而加的可選參數、甚至一個在你的專案裡根本不存在的使用情境所包的介面。你要的是函式，它還你一個框架。

而那些每一行，某一天都會變成團隊裡某個人得去讀的東西。這種話不會寫在任務卡上，但它就在那裡。

## 瓶頸只是換了地方

這個我是在團隊裡親眼看到，然後花了一段時間才理解的。而且它是兩面都會切到的。

第一刀切在 review。我們把寫的人速度拉高很多，但 review 的速度完全沒有增加。還是那顆人腦，一天還是 24 小時，現在卻要處理排隊中的 40 個檔案 PR。

接著就會發生排隊變長時一貫會發生的事。reviewer 開始只掃一下變更摘要（diff），看到測試有過，就回一句「我看可以」（也就是大家熟悉的 LGTM， *looks good to me* ）。這裡每個人都做過，包括我。別裝了。

但這樣一來，你就讓一段沒人真的寫透的程式，被沒人真的看透的人批准，最後送進有客戶在付錢的 production。等它壞掉時，牽涉其中的兩個人都不知道它到底在幹嘛。

把 review 交給 AI 也不是解法。就我的看法，它確實有助於抓一些小問題，真的有幫助。但 review 不只是用來找 bug；它至少還有一個功能，是讓團隊裡至少兩個人知道那段程式存在。

第二刀切在你的價格上，這才是最讓我在意的。

那天你花 2 小時交出原本要 2 天的東西之後，那件事的期限就變成 2 小時了。永遠都是。沒人會回頭問一句：「等等，但這裡面有多少是 AI 一次就對了，有多少只是運氣？」標準只會往上，不會再降回去。

然後真正麻煩的問題就來了：那種全世界都沒有現成答案、需要你坐下來想 3 天的東西。可現在你欠的速度，說真的，你以前根本沒那麼快過。

如果你是接案、按範圍收費，那就更慘。你剛把自己的價格通貨緊縮了，還把維護責任白白攬在自己身上。我上午交付，之後又回到那個檔案 3 次。那個上午我有收費；後面 3 次，我是用自己的時間在付帳。

也就是在這裡我卡住了：我要怎麼繼續用這個好用到不行的東西，同時又不把進入程式碼的控制權、以及我工作的價格，白白送掉？

## 我現在怎麼做

現在我是以接案開發者的身分工作，而我整理出了一套思考流程，幫助自己處理這類情況。所謂這套流程，其實是我在工作中使用 AI 時犯了很多錯之後，慢慢形成的。

我描述問題，不描述程式碼。以前我會直接說「寫一個做 X 的函式」，然後拿到那種完整解。現在我會先說：

- 專案裡已經有什麼，
- 限制條件是什麼，
- 最重要的是，我 **不要** 什麼。

這大幅減少了亂七八糟的抽象層。我還會繼續：

我會明確要求最小版本。「做出能解決問題的最簡單版本，不要泛化。」你根本想不到，當你這樣要求時，能少掉多少程式碼。

PR 的說明我自己從頭寫。怎麼做可以讓 AI 產生，沒問題；但為什麼要這樣做，是唯一沒辦法之後再重生一次的部分，而這偏偏就是 6 個月後的我最需要的東西。

如果 PR 大到像怪物一樣，我就把它拆開。就算麻煩也一樣。AI 出現之後，一個人能放進腦子裡的規模並沒有變大。

測試則是集中在真正會痛的地方。快樂路徑 AI 可以自己罩住，而那剛好也是最不容易壞的部分。真正會壞的是網路失敗、payload 格式錯誤、或那種只存在於老客戶那邊的舊資料。痛點都在那裡。

至於價格，也有一件事變了：我不再估程式碼，而是估問題被解決的成本。不是我今天早上要花多久去寫，而是這件事接下來一個月能不能站得住。

## 說到底，工作變成了選擇

我不知道這是不是正確的解讀，這只是我到目前為止在日常使用中看到的樣子。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1106437894&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787904201&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqenNBx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1787900626283&amp;bpp=1&amp;bdt=-M&amp;idt=0&amp;shv=r20260825&amp;mjsv=m202608250101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787904025%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787904025%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787904025%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C350x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=241338877532&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4849&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1229&amp;eid=95399629%2C95400778%2C122880597%2C95340253%2C95340255&amp;oid=2&amp;psts=AOrYGsnXvDup2SBOywyGB9515VDrynzR_1aE0OrNdyKn3g-Q5WDl5IHg6HtgMtRfO3jSPmiNTI8Uxrc35qJ4rQ0iVBoB6wEbkAOvIC6aT_eOfGQx3QqQk0FXe2o3u--zkobeec9i%2CAOrYGskYGQBpHLt8iUp5WFqAnMV7Rao6pZKQglnWqu6iimnOVToO14ghm7IKD2nlF0Y4b0K05hN-hXzbPYOKeT2dWHdEZYa4T6VAS66zOoY%2CAOrYGsm3UGlfJMD3xF-GNv3Yuy40PTe2XnZNlMWvEZMaWKiqTV38GF3GQMDw8oRMjrUQsuiAg1Hut7_Vkx-gR-1hFhoP6JyCqvD_-Gi_NSzx02FyYw%2CAOrYGsnNgufrfox7qttUsqx2Ha8gH_z0M4AkNIVA4-BSvPx8_9MlOstTrLy2aZ7KWqxBSl3Q0kv7WlWdcXLw-e2zjxsC6FM2bdKRpmlXgeCp-IxHxTM&amp;pvsid=7731929571675920&amp;tmod=1772502848&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

但我覺得 AI 沒有終結工程工作，它只是終結了工程裡機械性的那一部分。而當機械部分被拿掉後，剩下的就是我們一直以來最能拖著不處理的東西：理解系統、帶著判斷做決定，以及之後在 production 中把它撐住。

只會打字快的人失去了優勢。能看著 200 行生成的程式碼，直接說「這裡不對，而且原因是這個」的人，價值變高了，不是變低。注意看這種人到底在做什麼：他評估了選項，然後選出自己願意承擔哪一種缺點。市場把這叫做 trade-off，而這其實就是現在留給我們的工作。要嘛現在泛化，付出複雜度的成本；要嘛今天先硬寫死，之後再付代價。不存在沒有成本的選擇，只有睜大眼睛做出的選擇，和自動模式下做出的選擇。

交付速度很容易在 daily 裡展示。維護成本則會一直藏著，直到它找到你為止。

所以，當然可以生成，請盡量生成。但在簽名之前，先讀一遍你生成出來的東西。

*從社群到社群。*

記得喝水，我們下篇文章見。

---

原文出處： [https://dev.to/he4rt/velocidade-de-entrega-e-custo-de-manutencao-pos-ia-5gei](https://dev.to/he4rt/velocidade-de-entrega-e-custo-de-manutencao-pos-ia-5gei)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qenNBx) [8瀏覽](https://codelove.tw/@tony/post/qenNBx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qenNBx)

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

📝14 💬4

210

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

6

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="350" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=1414751727&amp;adf=2365317702&amp;pi=t.aa~a.4029027559~rp.3&amp;abgtt=6&amp;w=350&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787904142&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=350x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqenNBx&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1787900626335&amp;bpp=2&amp;bdt=-M&amp;idt=2&amp;shv=r20260825&amp;mjsv=m202608250101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787904025%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787904025%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787904025%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=241338877532&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=1167&amp;ady=974&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400778%2C122880597%2C95340253%2C95340255&amp;oid=2&amp;pvsid=7731929571675920&amp;tmod=1772502848&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=9&amp;uci=a!9&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1787900625&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqenNBx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1787900624916&amp;bpp=16&amp;bdt=324&amp;idt=79&amp;shv=r20260825&amp;mjsv=m202608250101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787879054%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787879054%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787879054%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=241338877532&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400778%2C122880597%2C95340253%2C95340255&amp;oid=2&amp;pvsid=7731929571675920&amp;tmod=1772502848&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=128" title="Advertisement" aria-label="Advertisement"></iframe>