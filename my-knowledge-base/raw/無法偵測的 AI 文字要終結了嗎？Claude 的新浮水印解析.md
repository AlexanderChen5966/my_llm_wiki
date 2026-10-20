---
title: "無法偵測的 AI 文字要終結了嗎？Claude 的新浮水印解析"
source: "https://codelove.tw/@tony/post/3KKAR3"
author:
published:
created: 2026-08-17
description: "過去幾個小時，整個世界——或者至少是我的 LinkedIn 動態——都在討論同一件事：Anthropic 簽署了 **EU AI Act（歐盟人工智慧法案）關於 AI 生成內容透明度的實務守則**。這是不是代表那些有趣的貓咪故事、LinkedIn 貼文和無聊文件的生成時代結束了？我們完蛋了嗎？我..."
tags:
  - "clippings"
status: "processed"
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月15日](https://codelove.tw/@tony/post/3KKAR3 "2026-08-15 09:06:00")

## 無法偵測的 AI 文字要終結了嗎？Claude 的新浮水印解析

過去幾個小時，整個世界——或者至少是我的 LinkedIn 動態——都在討論同一件事：Anthropic 簽署了 **EU AI Act（歐盟人工智慧法案）關於 AI 生成內容透明度的實務守則** 。

這是不是代表那些有趣的貓咪故事、LinkedIn 貼文和無聊文件的生成時代結束了？我們完蛋了嗎？我們是不是得像原始人一樣，手動把那些無聊東西一字一句寫出來，否則某個神祕偵測器就會把我們抓包？😅

我實在忍不住想寫這篇！網路上已經針對這個話題產生了相當多的迷思，所以我們快速來看一下目前確實已知的事實。順帶一提，這些資訊其實在 Claude 的官方頁面上就很容易找到，而且那裡的文件比很多業界文章都更好讀。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786936590&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KKAR3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932843440&amp;bpp=4&amp;bdt=1044&amp;idt=4&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936390%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908&amp;nras=3&amp;correlator=5217252052750&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1000&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95398731%2C122880368%2C42533293&amp;oid=2&amp;pvsid=7624991924468046&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

而且你會看到，真的沒什麼好怕的。另外……Anthropic 甚至還不是第一個做這件事的公司。

## 等等，為什麼我們需要這條法律？AI 生成文字在歐洲變成違法了嗎？

當然沒有！

如果你像我一樣是歐盟公民，你還是可以請 AI 幫你回覆一封無聊的電子郵件，或生成一首荒謬的詩，你不會因此被抓去關。😉

目標不是禁止 AI，而是讓 AI 生成與被操控的內容能被機器辨識，並且在特定情境下對使用者保持透明——尤其是涉及 deepfake，或與公共利益相關的內容時。

這裡還有一個重要細節：AI Act 也為某些 AI 生成或經過修改的文字提供例外，如果內容已經經過人工審查或編輯控制，且由自然人或法人負責其出版責任，則不在此限。

所以，不，歐盟並沒有對你用 ChatGPT 寫的電子郵件宣戰。

## 到目前為止，我們實際知道些什麼？

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3764087313&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786936590&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KKAR3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932843451&amp;bpp=1&amp;bdt=1055&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936390%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280&amp;nras=4&amp;correlator=5217252052750&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1813&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95398731%2C122880368%2C42533293&amp;oid=2&amp;pvsid=7624991924468046&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

以下是截至今天我們確定知道的事：

- Anthropic 確實簽署了 **EU AI Act 第 50(2) 條 AI 生成內容透明度實務守則** ，並且作為其實施的一部分，正在提供機器可讀的標記。相關的透明度義務已於 2026 年 8 月 2 日開始適用。所以，是的，我們現在已經進入這個階段了。
- 2026 年 8 月 2 日之後推出的 Claude 模型，從一開始就具備標記能力。較舊的模型不一定已經具備，Anthropic 正在逐步推行。
- 這套系統是全球適用的。沒錯，即使在歐盟以外也一樣。
- 它適用於透過 **Claude、Claude Platform/API、Claude Code、Claude Cowork 和 Claude Tag** 使用的支援 Claude 模型。
- 如果你是透過 AWS、Google Cloud、Microsoft Foundry 或其他支援平台使用 Claude？標記是套用在模型層級的，所以換個介面並不會讓它憑空消失。
- 這裡涉及兩種不同機制：生成文字的不可感知浮水印，以及支援檔案的來源證明中繼資料。

## 所以……這個浮水印到底是怎麼運作的？

Anthropic 稱之為一種 **「嵌入在文字中、無法被感知的浮水印」** 。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786936590&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KKAR3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932843460&amp;bpp=2&amp;bdt=1063&amp;idt=2&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936390%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280&amp;nras=5&amp;correlator=5217252052750&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2665&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95398731%2C122880368%2C42533293&amp;oid=2&amp;pvsid=7624991924468046&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

根據 Anthropic 的說法，它可以在複製貼上後保留下來，甚至在一定程度的編輯後仍可能存在。它是在模型層級實作，而不是 Claude 前端事後額外加上的東西。

但重點來了： **Anthropic 目前還沒有明說它到底怎麼運作！**

公司表示會有更技術性的文件陸續釋出，所以……等它出來後，我也會再寫一篇追蹤。😀

我們可以合理推測，它可能是一種 **統計式 token 浮水印** 。這類系統不是靠奇怪的空白字元或看不見的 Unicode 字元，這種東西一下就能刪掉。相反地，模型在生成時會對某些 token 產生輕微且具有統計性的偏好。

對單一句子來說，這種訊號可能無法偵測，或者偵測結果不可靠。但在較長的文字中，統計特徵就可能開始浮現。

所以，粗略來說，你可以想像成這樣（ **如果 Anthropic 的浮水印是這樣實作的話** ）：

**一般生成**

```
P(next_token | context)
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=186534417&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786936590&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KKAR3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932843467&amp;bpp=1&amp;bdt=1071&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936390%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=5217252052750&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3527&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95398731%2C122880368%2C42533293&amp;oid=2&amp;pvsid=7624991924468046&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

vs.

**帶浮水印的生成**

```
P(next_token | context) + secret statistical bias
```

但再次強調： **Anthropic 目前還沒有證實 Claude 的浮水印就是這樣運作的。**

這只是根據現有文字浮水印技術所做的合理推測。🙂

## Claude 是唯一會對文字做浮水印的模型嗎？

不是！

Google 其實已經做這件事好幾年了。

Google DeepMind 早在 2024 年就推出了 **SynthID Text** ，其運作方式大致和我上面描述的一樣：在 token 生成過程中加入浮水印，形成一個之後可以被偵測的統計訊號。

至於 OpenAI，則表示他們已開發文字浮水印技術；雖然他們已經在圖片、影片和音訊等內容上使用來源證明或浮水印技術，但尚未公開推出與一般 GPT 文字回覆相當的浮水印機制。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1038984369&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786936637&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KKAR3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932843475&amp;bpp=1&amp;bdt=1080&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936390%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x272%2C334x250&amp;nras=8&amp;correlator=5217252052750&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4424&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=834&amp;eid=95398731%2C122880368%2C42533293&amp;oid=2&amp;psts=AOrYGsnlzvWSwSAGSFqCdLEq8iKU9sI3E_bOhS9PyEGmDFzZLeBnnh-2_zQ166do13UD0Wh-5ucPbHHBgp19Sq3gUtQdB6BOaPPFEOg6gnx4bfnoE0aEv1Hrleq2QU-o4Ur3%2CAOrYGsnlxhId4eX4iszLdoHTLgF15ItQ-FUeSbGE8v89XrHsGDtWQFQLaUUADQ_crSu3jY_ilAkK_qw17Mfxbhy8RI1z6sfsJPd6nGu3ePWU8o_SSF4%2CAOrYGskagqu8cFn0uqkqeMRXGJ8oqnhrfsBjG-G4keZHaKHJjmXNZu3x65sv26KS2m_-0RU0k_pZmeXOcvdv6OJBpRxVKO8d3OS2AOuoK8P_q3M%2CAOrYGsmCw_Jp7BFnhWSbNlf-4Gun_jozTiz7HZVpY5xAjfo0rxVlSPsaplRyul6TV3yWJlgIC5mW3EbBTiKE5VzS8Ls4ulJdnEPSc0ACO09QCsE%2CAOrYGsmvlMrDhs9V4wLEICZ0ZZ7-x0MJomqmOKXWjIOgbTRAT3k5DPtF8lKNTtP4dxPHqI9VdWR1DX7gAVuDdyzOYMqVEkJNO9A1PO66rIj6VHg%2CAOrYGsmi37l2TBu0XE87qvrVLaspfkR3MIHkuXPNyYy-5ND77Vk3C92xW02aFMjsJUZSfgzO3c9vKk6uToo2brG0FHcg44L06vd832X1xXoFky0Z&amp;pvsid=7624991924468046&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

所以，Anthropic 當然不是第一個想到對 LLM 輸出加浮水印的人。

## 每個人都能用浮水印偵測器嗎？

目前還不知道。

對於支援的檔案，情況會簡單一些，因為 Anthropic 使用來源證明中繼資料，例如 C2PA 憑證，可以透過適當的驗證工具來檢查。

但文字就更有意思了。

Anthropic 表示，未來會讓 **第三方與研究人員** 使用浮水印偵測功能，但我們還不清楚這些第三方到底會是誰、存取方式會是什麼樣子，或一般使用者之後是否會拿到簡單的公開偵測器。

所以 GPTZero 和類似的垃圾工具，可能還會再有一陣子工作可做。

## 這對我們到底意味著什麼？

一切會因此變得一清二楚嗎？每篇 LLM 生成的文章旁邊，之後都會突然出現一個超大紅色的 **作弊者！** 警告嗎？xD 當然不會！

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1621299728&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786936646&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KKAR3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932843481&amp;bpp=2&amp;bdt=1086&amp;idt=2&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936390%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x272%2C334x250%2C952x280&amp;nras=9&amp;correlator=5217252052750&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5289&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=1716&amp;eid=95398731%2C122880368%2C42533293&amp;oid=2&amp;psts=AOrYGsnlzvWSwSAGSFqCdLEq8iKU9sI3E_bOhS9PyEGmDFzZLeBnnh-2_zQ166do13UD0Wh-5ucPbHHBgp19Sq3gUtQdB6BOaPPFEOg6gnx4bfnoE0aEv1Hrleq2QU-o4Ur3%2CAOrYGsnlxhId4eX4iszLdoHTLgF15ItQ-FUeSbGE8v89XrHsGDtWQFQLaUUADQ_crSu3jY_ilAkK_qw17Mfxbhy8RI1z6sfsJPd6nGu3ePWU8o_SSF4%2CAOrYGskagqu8cFn0uqkqeMRXGJ8oqnhrfsBjG-G4keZHaKHJjmXNZu3x65sv26KS2m_-0RU0k_pZmeXOcvdv6OJBpRxVKO8d3OS2AOuoK8P_q3M%2CAOrYGsmCw_Jp7BFnhWSbNlf-4Gun_jozTiz7HZVpY5xAjfo0rxVlSPsaplRyul6TV3yWJlgIC5mW3EbBTiKE5VzS8Ls4ulJdnEPSc0ACO09QCsE%2CAOrYGsmvlMrDhs9V4wLEICZ0ZZ7-x0MJomqmOKXWjIOgbTRAT3k5DPtF8lKNTtP4dxPHqI9VdWR1DX7gAVuDdyzOYMqVEkJNO9A1PO66rIj6VHg%2CAOrYGsmi37l2TBu0XE87qvrVLaspfkR3MIHkuXPNyYy-5ND77Vk3C92xW02aFMjsJUZSfgzO3c9vKk6uToo2brG0FHcg44L06vd832X1xXoFky0Z%2CAOrYGsmFcooHHNBls1JEKUTkALLZ6feRNF2Sos94ahLfsFy5Z_Hb__dyp9LgG_OPHQL9gorJKuW-8Qc2raQcYDFjGrB6U4O2Qtjf_bbo_ACM0epUtQ&amp;pvsid=7624991924468046&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Anthropic 自己也公開談到這項技術的限制。這類問題我們在 DEV 上其實已經討論過幾千次了。

首先，浮水印存在並 **不** 一定代表作者不是自己寫的。

也許他們是自己寫了整篇內容，然後請 Claude 幫忙編輯、修正文法、潤飾措辭，或翻譯成另一種語言。即使實際想法，甚至大部分原始文字都來自人類作者，最後輸出仍可能帶有浮水印。

所以，是的，最後文章還是可能一樣混亂、冗長，充滿一大堆離題內容，跟我在這裡發的貼文一模一樣。

Anthropic 也明確指出，反過來的情況同樣可能發生。

有人可以把完全由 AI 生成的垃圾內容大幅修改、改寫、翻譯、混入其他內容，或用其他方式把它改到浮水印不再可偵測。

所以：

**偵測到浮水印 ≠ AI 寫了全部內容**

而且

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3970593978&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786936659&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KKAR3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932843488&amp;bpp=1&amp;bdt=1093&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936390%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x272%2C334x250%2C952x280%2C952x280&amp;nras=10&amp;correlator=5217252052750&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6109&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=2480&amp;eid=95398731%2C122880368%2C42533293&amp;oid=2&amp;psts=AOrYGsnlzvWSwSAGSFqCdLEq8iKU9sI3E_bOhS9PyEGmDFzZLeBnnh-2_zQ166do13UD0Wh-5ucPbHHBgp19Sq3gUtQdB6BOaPPFEOg6gnx4bfnoE0aEv1Hrleq2QU-o4Ur3%2CAOrYGsnlxhId4eX4iszLdoHTLgF15ItQ-FUeSbGE8v89XrHsGDtWQFQLaUUADQ_crSu3jY_ilAkK_qw17Mfxbhy8RI1z6sfsJPd6nGu3ePWU8o_SSF4%2CAOrYGskagqu8cFn0uqkqeMRXGJ8oqnhrfsBjG-G4keZHaKHJjmXNZu3x65sv26KS2m_-0RU0k_pZmeXOcvdv6OJBpRxVKO8d3OS2AOuoK8P_q3M%2CAOrYGsmCw_Jp7BFnhWSbNlf-4Gun_jozTiz7HZVpY5xAjfo0rxVlSPsaplRyul6TV3yWJlgIC5mW3EbBTiKE5VzS8Ls4ulJdnEPSc0ACO09QCsE%2CAOrYGsmvlMrDhs9V4wLEICZ0ZZ7-x0MJomqmOKXWjIOgbTRAT3k5DPtF8lKNTtP4dxPHqI9VdWR1DX7gAVuDdyzOYMqVEkJNO9A1PO66rIj6VHg%2CAOrYGsmi37l2TBu0XE87qvrVLaspfkR3MIHkuXPNyYy-5ND77Vk3C92xW02aFMjsJUZSfgzO3c9vKk6uToo2brG0FHcg44L06vd832X1xXoFky0Z%2CAOrYGsmFcooHHNBls1JEKUTkALLZ6feRNF2Sos94ahLfsFy5Z_Hb__dyp9LgG_OPHQL9gorJKuW-8Qc2raQcYDFjGrB6U4O2Qtjf_bbo_ACM0epUtQ%2CAOrYGsnCGMjUcFGCVeEPrXvnSk1lbRGMYDL1EmZlVx_MRoiG9NGmhZxka6MMqf4wQXcEnDHCzdvoFqTBYDMUOKFQ3CAvXlsibSduhEa2xw917-YEug&amp;pvsid=7624991924468046&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**沒偵測到浮水印 ≠ 人類寫了全部內容**

這是非常重要的區別。

## 那圖片、音訊和影片呢？

對於支援的檔案，來源證明會比較容易驗證，因為我們可以使用內嵌中繼資料和內容憑證。不過，這也不是萬無一失。

例如，如果有人把一張 AI 生成圖片截圖了，中繼資料並不會神奇地在所有可能的轉換中都保留下來。

這也正是為什麼來源證明很有用，但它不是什麼完美的、通用的 AI 偵測器。

## 那程式碼到底會怎麼辦？

這大概是我最感興趣的部分。對於生成的程式碼，他們到底打算怎麼處理浮水印？因為在程式碼裡，模型能選擇的等價 token 空間比自然語言小得多。

在自然語言中，要表達同一個意思，往往有幾十種完全合理的說法。但如果我們有像這樣的程式碼：

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3355444817&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786936671&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KKAR3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932843495&amp;bpp=1&amp;bdt=1100&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936390%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936390%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x272%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=5217252052750&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6938&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=3336&amp;eid=95398731%2C122880368%2C42533293&amp;oid=2&amp;psts=AOrYGsnlzvWSwSAGSFqCdLEq8iKU9sI3E_bOhS9PyEGmDFzZLeBnnh-2_zQ166do13UD0Wh-5ucPbHHBgp19Sq3gUtQdB6BOaPPFEOg6gnx4bfnoE0aEv1Hrleq2QU-o4Ur3%2CAOrYGsnlxhId4eX4iszLdoHTLgF15ItQ-FUeSbGE8v89XrHsGDtWQFQLaUUADQ_crSu3jY_ilAkK_qw17Mfxbhy8RI1z6sfsJPd6nGu3ePWU8o_SSF4%2CAOrYGskagqu8cFn0uqkqeMRXGJ8oqnhrfsBjG-G4keZHaKHJjmXNZu3x65sv26KS2m_-0RU0k_pZmeXOcvdv6OJBpRxVKO8d3OS2AOuoK8P_q3M%2CAOrYGsmCw_Jp7BFnhWSbNlf-4Gun_jozTiz7HZVpY5xAjfo0rxVlSPsaplRyul6TV3yWJlgIC5mW3EbBTiKE5VzS8Ls4ulJdnEPSc0ACO09QCsE%2CAOrYGsmvlMrDhs9V4wLEICZ0ZZ7-x0MJomqmOKXWjIOgbTRAT3k5DPtF8lKNTtP4dxPHqI9VdWR1DX7gAVuDdyzOYMqVEkJNO9A1PO66rIj6VHg%2CAOrYGsmi37l2TBu0XE87qvrVLaspfkR3MIHkuXPNyYy-5ND77Vk3C92xW02aFMjsJUZSfgzO3c9vKk6uToo2brG0FHcg44L06vd832X1xXoFky0Z%2CAOrYGsmFcooHHNBls1JEKUTkALLZ6feRNF2Sos94ahLfsFy5Z_Hb__dyp9LgG_OPHQL9gorJKuW-8Qc2raQcYDFjGrB6U4O2Qtjf_bbo_ACM0epUtQ%2CAOrYGsnCGMjUcFGCVeEPrXvnSk1lbRGMYDL1EmZlVx_MRoiG9NGmhZxka6MMqf4wQXcEnDHCzdvoFqTBYDMUOKFQ3CAvXlsibSduhEa2xw917-YEug%2CAOrYGsmzjNZ8CDfaZijJI8bE3hNAx097Q8XNLPsMI421OMIGdwsBcylimzOBBYZfLWP9UgK7p8U3NQVN2EJQOw9KuPmsql8ymNWyNaMWNt91SVCcSaw&amp;pvsid=7624991924468046&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

```javascript
const result = await fetchData();
```

那到底要怎麼對這段內容加入統計浮水印？

當然，還是有一些可變項：變數名稱、格式等等。但可替換的合法空間顯然比自然語言受限得多，而 formatter、重構、壓縮，甚至只是小小的人為修改，都可能讓這個問題變得更有趣。

Claude Code 已包含在 Anthropic 的標記推行範圍內，但 Anthropic 目前還沒有公布足夠多的技術細節，讓我們知道對生成的原始碼來說，這個文字浮水印到底會怎麼表現。

而我真的很想知道他們是怎麼解決這個問題的。

## 所以，你應該在意嗎？

如你所見，Claude 對生成文字加浮水印，其實是一個合乎邏輯且重要的步驟，雖然顯然還不完美。

它大概不會改變我們日常生活太多。

當然，社群媒體上滿天飛的 AI 垃圾內容很惱人。不過很遺憾，我覺得這個浮水印也不會神奇地解決那個問題。😐

我更感興趣的是 **這個浮水印到底是怎麼實作的** 。

Anthropic，我在等文件！

---

原文出處： [https://dev.to/sylwia-lask/the-end-of-undetectable-ai-text-claudes-new-watermark-explained-45g2](https://dev.to/sylwia-lask/the-end-of-undetectable-ai-text-claudes-new-watermark-explained-45g2)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3KKAR3) [14瀏覽](https://codelove.tw/@tony/post/3KKAR3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3KKAR3)

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

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1786932842&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KKAR3&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932842553&amp;bpp=1&amp;bdt=74&amp;idt=102&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786932726%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5217252052750&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95398731%2C122880368%2C42533293&amp;oid=2&amp;pvsid=7624991924468046&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=144" title="Advertisement" aria-label="Advertisement"></iframe>