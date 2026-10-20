---
title: "ArtCraft復刻7款Adobe軟體暴紅！軟體工程師再推「開源版Office」，平替款Word、Excel、PPT全免費"
source: "https://www.bnext.com.tw/article/92597/artcraft-open-source-office-clones"
author:
  - "[[李先泰]]"
published: 2026-10-13
created: 2026-10-13
description: "繼用Claude復刻Adobe後，Brandon Thomas再推出對應Word、Excel與PPT的免費開源軟體，展現一人挑戰大廠核心產品的潛力。"
tags:
  - "clippings"
status: "inbox"
---
軟體工程師 [Brandon Thomas](https://www.linkedin.com/in/possibilistic) 在9月底用Anthropic的AI模型Claude，以程式語言Rust從頭重寫7款Adobe軟體並免費開源。相隔約一周，他把同一套做法搬到微軟Office，推出對應Excel的GridCraft、對應PowerPoint的DeckCraft與對應Word的WordCraft， [全部放上GitHub供人免費下載](https://github.com/storytold/gridcraft) 。

訂閱制軟體最核心的工作流程，但現在一名開發者加上AI就能在短時間內做出免費開源的對應版本，這無疑這對想保護自家產品的大型軟體公司是一大隱憂。

## Word、Excel、PPT也有免費開源版，做到什麼程度？

三款軟體的GitHub專案在10月7日上線，以Rust寫成，採MIT與Apache-2.0雙授權，屬於寬鬆的開源授權，個人與企業都能免費使用、修改。軟體支援macOS、Windows、Linux與BSD，也能透過WebAssembly（讓程式直接在瀏覽器裡執行的技術）在網頁上使用。 [WordCraft](https://getartcraft.com/apps/wordcraft) 可讀寫Word的.docx檔， [DeckCraft](https://getartcraft.com/apps/deckcraft) 也能開啟與儲存PowerPoint檔案。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

跟Office最大的差異，在於設計之初就把AI當成使用者。DeckCraft的專案說明寫道，每個按鈕同時也是一道指令，可以透過命令列或MCP（讓AI代理串接外部工具的開放標準）操作；WordCraft與 [GridCraft](https://getartcraft.com/apps/gridcraft) 的說明頁也附上把軟體接進Claude Code的指令。

換言之，使用者可以讓AI代理在不開啟介面的情況下，直接產出文件、試算表與簡報。

不過完成度仍有落差。科技媒體《TechRadar》就評估，這三款軟體的開發進度還不如先前的Adobe複製品，DeckCraft的專案頁也標示仍處於alpha階段、朝beta版本推進。截至10月13日，WordCraft在GitHub累積約9,300顆星（使用者收藏關注的指標），GridCraft約3,900顆、DeckCraft約2,600顆。

## ArtCraft是誰？從AI影像工具到7款Adobe複製品

ArtCraft是Thomas創立的開源AI圖像與影片創作平台，主打「不需訂閱」，可在同一個介面串接Seedance 2.5、Nano Banana 2等生成模型，讓創作者自己構圖、再交給模型算圖。

➜ [不用再花幾十小時研究AI工具!3 小時建立屬於自己的工作系統](https://biz-online.bnextmedia.com.tw/courses/aiworksystem?utm_source=web_bn&utm_medium=middle_text&utm_campaign=aiworksystem&utm_content=177900&utm_term=channel_5 "不用再花幾十小時研究AI工具!3 小時建立屬於自己的工作系統")

據了解，Thomas曾任「Storyteller.ai」執行長3年，自稱有15年程式開發經驗。ArtCraft暴紅後，外界批評這些軟體是靠vibe coding（用口語指揮AI寫程式）堆出來的「AI垃圾」，Thomas曾在X上回應自己本來就會寫程式。

Thomas在Reddit發文介紹，這7款軟體是以Claude Opus 5.5作為AI程式助手完成，屬於「clean-room（無塵室）重新實作」：他聲稱未使用Adobe程式碼、從零寫起，並據此主張沒有著作權問題。7款軟體在9月30日至10月1日陸續上線，對照如下：

Thomas做這件事的動機很直接。他曾在Reddit上寫道，他和Adobe關係很差，因為曾多次被收取「解約費」，他給自己的目標是一個月內讓功能追平原版軟體。

## 對Adobe、微軟的威脅在哪？

真正的壓力來自開發成本。Thomas認為專業創作軟體變得太貴太封閉，也看準現代AI程式工具讓個人開發者突然有能力重寫這類大型軟體。從GitHub紀錄來看，ArtCraft從7款Adobe軟體到3款Office軟體只隔約一周，10月7日還同步建立對應Autodesk AutoCAD的CADCraft、對應Avid Pro Tools的SoundCraft專案。

Thomas對現有的開源軟體也不客氣。外界批評他與其用AI另起爐灶，不如支持LibreOffice、GIMP、Thunderbird等既有開源專案，他在X上回應：「GIMP很爛，一直都很爛，沒有理由去拯救壞掉的軟體。」他也提到，LibreOffice曾在期末考期間、報告交件那一周，毀掉他寫好的40頁報告。

但要真正撼動Office仍有門檻。Reddit網友指出LibreOffice與Google Docs免費多年，並未明顯衝擊Office銷售，也質疑單一開發者能否跟上除錯與改版的速度；也有《Tom's Hardware》讀者留言指出，企業採購軟體看重技術支援，這類專案較適合個人工作室使用。

但無論如何，AI正在把重寫大型商用軟體的成本壓低，確實已經讓「免費開源替代品」從口號變成GitHub上可下載的專案。

> 延伸閱讀： [寵物攝影機賣了10年，如今訂閱收入超過硬體！Furbo如何靠「AI服務」讓飼主買單？](https://www.bnext.com.tw/article/92570/furbo-tomofun-ai-subscription-pet-camera)

資料來源： [《Tom's Hardware》](https://www.tomshardware.com/software/microsoft-office/creator-of-free-adobe-clones-unveils-open-source-microsoft-office-excel-powerpoint-and-word-replications-autodesk-tools-and-other-subscription-based-services-may-come-next) 、 [《TechRadar》](https://www.techradar.com/computing/office-suites/developer-who-vibe-coded-free-adobe-clones-reveals-apps-to-take-down-microsoft-office-next-and-says-there-is-no-reason-to-save-broken-software) 、 [《Notebookcheck》](https://www.notebookcheck.org/Photoshop-Premiere-y-mas-Un-desarrollador-experimentado-en-Rust-recrea-la-suite-de-Adobe-con-ayuda-de-la-IA-como-software-de-codigo-abierto.1419775.0.html) 、 [ArtCraft官網](https://getartcraft.com/) 、 [ArtCraft GitHub](https://github.com/storytold) 、 [DeckCraft專案說明](https://github.com/storytold/deckcraft)

本文初稿為AI編撰，整理．編輯/ 李先泰