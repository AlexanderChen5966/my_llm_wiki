---
title: "QA工程師的工作變了——用 Claude Code 和 Playwright，從測試到票單管理全部交給 AI 的故事"
source: "https://codelove.tw/@tony/post/x0n6ga"
author:
published:
created: 2026-08-24
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------初次見面！我是 QA 工程師 SA！我作為 QA 事業部的代表，每天都在與軟體品質正面對話。突然問大家一個問題..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月22日](https://codelove.tw/@tony/post/x0n6ga "2026-08-22 17:02:18")

## QA工程師的工作變了——用 Claude Code 和 Playwright，從測試到票單管理全部交給 AI 的故事

## 前言

初次見面！我是 QA 工程師 SA！

我作為 QA 事業部的代表，每天都在與軟體品質正面對話。

突然問大家一個問題：你們有過「理所當然的工作」在某一天突然整個改變的經驗嗎？

我有。而且，這還是幾個月前的事。

---

## 以前手動作業的故事

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787539651&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx0n6ga&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536300477&amp;bpp=6&amp;bdt=1709&amp;idt=6&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787537221%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905&amp;nras=3&amp;correlator=2338764222318&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1061&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95397679%2C95399755%2C122880397%2C95390277&amp;oid=2&amp;pvsid=3938954146228696&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

在我加入這個團隊之前，我待的是一個手動建立測試情境、手動執行測試的團隊。

我們會把測試案例一項一項寫到 Excel 或試算表裡，實際操作瀏覽器，一邊看一邊確認——就是這樣的世界。甚至連情境測試這個概念都不存在，整個現場都以「人來操作」為前提。

之後我調到現在的團隊，第一次接觸到了 Playwright。

能把 E2E 測試當作程式碼來用 Playwright 管理，光是這點就讓我覺得「這環境也太厲害了」，但在當時，還是有一部分工作得交給人來做。

測試結束後，打開瀏覽器，找 GitHub 的 issue，寫留言，然後把狀態更新成 done。

雖然很瑣碎、很花時間，但我當時覺得「這就是工作」。

---

## Claude Code × GitHub 串接帶來的改變

轉捩點是，我遇見了 **Claude Code** 。

Claude Code 是一個可以讓 AI 撰寫程式碼、操作終端機的工具。把它和 GitHub 串接之後，整個世界都變了。

現在，我只要用自然語言對 AI 下指令——

- Playwright 測試會自動執行
- 測試結果會自動留言到 GitHub 的 issue
- 狀態會自動變成 done

這整個流程， **完全不需要人介入就能完成。**

第一次跑起來的時候，老實說我笑出來了。「欸，這樣真的能動喔？」那種感覺到現在我都還忘不了。

---

## 從 QA 現場看到的 AI 應用真心話

當然，一開始並不是「太棒了！超讚！」這樣的反應。

「AI 真的能執行測試、守住品質嗎？」「如果它做出非預期的行為，誰要負責？」——這些不安，不只是團隊裡有，我自己心裡也有。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787539651&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx0n6ga&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536300505&amp;bpp=1&amp;bdt=1738&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787537221%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280&amp;nras=5&amp;correlator=2338764222318&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2776&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95397679%2C95399755%2C122880397%2C95390277&amp;oid=2&amp;pvsid=3938954146228696&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

實際用下來，AI 並不是萬能的。如果指令下得不夠清楚，它也可能不會按照預期行動。作為前提的設計，以及 QA 該如何判斷的標準，終究還是必須由人來掌握。

但反過來說，我也感受到， **人類該做的工作的解析度變高了** 。把單純重複的工作交給 AI，我們就能更專注在「該測什麼」「哪裡有風險」這些本質性的思考上。

---

## AI 時代不能落後

作為 QA 事業部的代表，這件事我可以很明確地說。

**AI 的浪潮，確實已經來到 QA 現場了。**

實際上，能熟練運用 AI 的 QA 工程師和不會用 AI 的 QA 工程師之間，已經開始出現很大的差距了。這不是危言聳聽，而是我在現場親身感受到的真實情況。

「這跟我的工作沒關係」「還早得很」——當你這樣想的時候，現場其實已經悄悄在改變了。工具在變，需求的技能在變，工作的定義本身也在改變。

而且我甚至覺得，這樣的時代也許真的會到來：

**「不會用 AI 的工程師，已經不算工程師」**

聽起來很誇張嗎？但我是真的認為，這是離我們不遠的未來。既然如此，哪有不搭上這波大浪的選擇！！

QA 工程師正從那種一邊點點點操作畫面、一邊執行測試的時代，邁入全新的階段！！

不需要害怕。先試一件小事看看吧。做一個小小的自動化就好。只要對 AI 下達一個指令就好。那種「真的跑起來了」的體驗，肯定會成為下一步的起點。

---

## 結語——其實這篇文章，也是和 AI 一起寫的

最後，我要坦白一件事。

這篇文章， **其實是和 AI 共同創作的。**

從構思架構到整理文章內容，我都是一邊和 AI 對話一邊完成的。明明在談「測試自動化」的話題，結果就連寫文章這件事本身，也讓 AI 參與進來了。

老實說，我自己覺得還挺有趣的。

這就是現在的現實。AI 已經不只進入寫程式的領域，甚至也進入了「思考」和「傳達」的領域。

所以我才會這麼想：

**AI 時代不能落後——這不是別人的事，而是你的事。**

感謝你讀到最後！

---

原文出處： [https://qiita.com/a\_sato\_JQIT/items/a82fccf6d52c05156384](https://qiita.com/a_sato_JQIT/items/a82fccf6d52c05156384)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/x0n6ga) [66瀏覽](https://codelove.tw/@tony/post/x0n6ga)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/x0n6ga)

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

📝9 💬4 ❤️1

190

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

6

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="350" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=1414751727&amp;adf=3620188380&amp;pi=t.aa~a.4029027559~rp.3&amp;abgtt=6&amp;w=350&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787539651&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=350x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx0n6ga&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536300540&amp;bpp=2&amp;bdt=1772&amp;idt=2&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787537221%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787537221%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=7&amp;correlator=2338764222318&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=1167&amp;ady=1079&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95397679%2C95399755%2C122880397%2C95390277&amp;oid=2&amp;pvsid=3938954146228696&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=7&amp;uci=a!7&amp;btvi=5&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1200" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=3809598800&amp;adf=1056458448&amp;pi=t.aa~a.1416458249~rp.1&amp;abgtt=6&amp;w=1200&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787539659&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=1200x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx0n6ga&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536300547&amp;bpp=1&amp;bdt=1780&amp;idt=0&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787539653%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787539653%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787539653%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C350x280%2C1005x124%2C334x250%2C952x280&amp;nras=10&amp;correlator=2338764222318&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=217&amp;ady=5011&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1418&amp;eid=95396000%2C95397679%2C95399755%2C122880397%2C95390277&amp;oid=2&amp;psts=AOrYGsnxerBgvbCp8x4IHu65pb0n5Tks0ITa5vpoE3hA6HySSppVM3J-eFqbQ03yxfA0wu5btcD1DyAzghpt5_9IRiAbjle03b6OPD--mZAb688HAI39zfEb7hpXZJ98pppsNQ%2CAOrYGsnG1EWFovKcj7Vn0wYvbZgReTuaiBGbJOVOvglI9yDIdlWu1eYOG2uvLnqPmEoyO7U6xoCpoA3UJkxC4OvkLyF2Rvys_g49zc1_CjCehjqZaWQ%2CAOrYGslC7NLxjeDNbsSXW95dY9paSZQ5mTyLcSNnOkWxv8srv_z1jXmV8n6bWuo9HRLeyCrZ4_jD5nnRdL6b7UymuBesIVH4gsKY6L62OIV_4AgNxOg&amp;pvsid=3938954146228696&amp;tmod=1573633200&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1920%2C42%2C1920%2C42%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1787536299&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx0n6ga&amp;pra=5&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536299113&amp;bpp=1&amp;bdt=263&amp;idt=55&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2338764222318&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95397679%2C95399755%2C122880397%2C95390277&amp;oid=2&amp;pvsid=3938954146228696&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=98" title="Advertisement" aria-label="Advertisement"></iframe>