---
title: "不能只懂寫Code！Anthropic揭Vibe coding真相：比起coding，「本業知識」才是最大槓桿"
source: "https://www.bnext.com.tw/article/91272/anthropic-vibe-coding-2026"
author:
  - "[[李先泰]]"
published: 2026-06-17
created: 2026-06-22
description: "Anthropic 分析 40 萬次 Claude Code 對話，發現決定成敗的不是會不會寫程式，而是你懂不懂手上那件事。"
tags:
  - "clippings"
status: processed
---
[![Meet大南方招商倒數](https://bnextmedia.s3.hicloud.net.tw/pumpkin/image/photo/2026-06/img-1780295648-35690.gif)](https://meetgreatersouth.tw/exhibitor/?utm_campaign=2026mgs&utm_source=web_bn&utm_medium=logo_banner&utm_content=166740&utm_term=channel_all)

2026.06.17 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

Anthropic 分析 40 萬次 Claude Code 對話，發現決定成敗的不是會不會寫程式，而是你懂不懂手上那件事。

[＃Anthropic](https://www.bnext.com.tw/tags/Anthropic) [＃Claude](https://www.bnext.com.tw/tags/Claude)

---

> 如果你是會計、律師、行銷、PM，過去想做一件「需要寫點程式」的事，標準答案是：找工程師，或自己學。  
>   
> 現在 Anthropic 用 40 萬次真實對話的資料告訴你，這個前提可能正在鬆動，而鬆動的方式跟多數人想的不太一樣。

6 月 16 日，Anthropic 發表研究報告 [《Agentic coding and persistent returns to expertise》](https://www.anthropic.com/research/claude-code-expertise) （agentic coding 指「交辦式寫程式」，你下指令、AI 自己讀檔案跑指令把事做完）。

報告分析 2025 年 10 月到 2026 年 4 月、約 23.5 萬名使用者、約 40 萬次 Claude Code 互動對話，它想回答一個很多人焦慮的問題： **沒受過正式程式訓練的人，真的能指揮 AI 完成複雜技術工作嗎？**

報告給的答案是肯定的，但真正值得注意的，是報告的結論： **會不會寫程式沒那麼重要，而「懂不懂你手上那件事」反而更重要了。**

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

## 「人人都能寫程式」，這句話只對了一半

過去一年，「vibe coding」（憑感覺寫程式，你用自然語言描述想要什麼，AI 直接生出能跑的程式碼，你不必真的看懂每一行）紅遍開發圈。順著這股風潮，最常見的敘事是：寫程式的門檻被夷平了，人人都是工程師。

這個敘事對誰最有利？對 AI 工具廠商、對「不必再請工程師」的老闆，都是好故事。但報告的資料把它修正成一個更務實的版本。

Anthropic 替每次對話的使用者，依據逐字稿在「新手到專家」五個等級上評分。要注意，這個專業度跟職稱、跟一個人聰不聰明是兩回事，而且是任務特定的。

報告舉了一個關鍵例子：一個從沒用過 Python 的會計，只要能明確告訴 Claude 對帳規則該怎麼設、還能在月結時抓出 AI 漏掉的邊界錯誤，他在那個任務上就是專家；反過來，一個資深工程師第一次問 Rust 語言的問題，他就是新手。

換句話說，這裡講的「專業」不是會不會 coding，而是你對「要解決的問題本身」懂多少。這也是為什麼把報告滑坡成「人人都能取代工程師」會讀錯，領域知識本身就是一種需要長年累積的專業判斷，它沒有消失，只是換了個位置變成瓶頸。

## 分工長什麼樣：你出題，AI 答題

報告最清楚的一張圖，是人與 AI 的決策分工。Anthropic 把每個決策拆成「規劃」（做什麼、用哪種方法、怎樣算完成）和「執行」（改哪個檔、寫什麼程式、用哪種語言）。結果是：平均而言，人做了約 70% 的規劃決策，Claude 做了約 80% 的執行決策。

![Claude's share of planning and execution decisions](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/bpwj-1781687542.jpg?w=1200&output=webp)

在各個對話階段中，歸因於 Claude 而非使用者的規劃決策（要做什麼）與執行決策（如何去做）之佔比分佈。在典型的對話階段中，使用者約做出 70% 的規劃決策，而 Claude 則做出約 80% 的執行決策。

圖／ Claude

白話說，人負責出題與驗收，AI 負責動手。而且使用者愈內行，這個分工愈傾向「放手」：報告發現，新手的每一句指令平均觸發 Claude 約 5 個動作、產出約 600 字；專家的每句指令則觸發約 12 個動作、約 3,200 字。內行人敢一次把更大塊的事交出去，因為他知道怎麼描述、也知道怎麼驗收。

![專家的Claude比較忙](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/7w4x-1781687686.jpg?w=1200&output=webp)

研究發現，當使用者本身對該領域（例如程式開發、金融分析等）越內行，Claude 就會表現得越「積極」。

圖／ Claude

這是這份報告第一個反直覺的地方：AI 愈強，內行人的槓桿不是縮小，而是放大。

## 真正拉開差距的數字，藏在成功率裡

Anthropic 用兩種方式衡量「這次對話成功了沒」。最寬鬆的是「至少部分成功」；最嚴格的叫 verified success（核實成功，意思是不只 AI 自己判斷有完成，還要有 git 提交、測試通過、或使用者明確認可這類看得到的硬證據）。

照最嚴格的標準看：新手對話的核實成功率只有 15%，中階到專家則跳到 28% 到 33%。寬鬆標準下，新手 77%、中階以上 91% 到 92%。

![專業人士成功率更高](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/yirc-1781687897.jpg?w=1200&output=webp)

左圖（整體趨勢）： 不論任務內容為何，隨著使用者專業度提升，任務最終達成目標的成功率也隨之增加，顯示專業知識對 AI 的引導至關重要。 中圖與右圖（遇到困難時的表現）： 針對過程中遭遇挫折（如程式出錯或測試失敗）的任務，圖表分析了使用者能否將任務「救回」並成功完成。結果顯示，專家不僅遇到困難的機率較低，且即便遇到挫折，成功扭轉局勢的機率也顯著高於新手。

圖／ Claude

但這裡有個細節，報告特別強調：大部分增益集中在「新手變中階」這一段，從中階再爬到專家，曲線就平掉了。用報告的原話，只要對一個領域有基本掌握、能上手的程度，就能拿到大多數好處，深度精通只多一點點。

差距還體現在「卡關時誰撐得住」。當對話遇到麻煩（報錯、測試失敗、反覆嘗試），新手有 19% 直接放棄、一行程式都沒寫；其他人放棄率只有 5% 到 7%。報告的詮釋是： **把 AI 導回正軌的能力，本身就是專業的一部分。**

## 一個被低估的發現：職業差異，比你以為的小很多

如果寫程式背景真的那麼關鍵，那軟體工程師應該大幅領先才對。資料不是這樣。

在會產生程式碼的對話裡，軟體相關職業的核實成功率約 34%，其他職業約 29%，只差 5 個百分點，而且這個差距七個月來沒有擴大也沒有縮小。

報告統計了資料中前十大職業，每一個的成功率都落在軟體工程師的 7 個百分點以內。更反直覺的是，管理職的核實成功率甚至略高於軟體工程師。

報告自己給了兩個可能解釋：一是管理者「指揮、交辦、定義任務」的技能本來就能轉移到指揮 AI 上；二是衡量方式的偏誤，因為核實成功部分仰賴使用者在對話裡明講「對，就是這樣」，而管理者可能比較習慣把話講清楚。

這七個月還有一組變化值得看：花在 debug（抓蟲、修壞掉的程式）的對話占比，從 33% 一路掉到 19%，幾乎砍半；相對地，operating software（部署、設定、實際把軟體跑起來）從 14% 升到 21%，寫作與資料分析從約 10% 翻倍到約 20%。

報告用「對照接案市場行情」的方式估算每次任務的價值（它特別聲明這只是相對比較，不該被當成精確金額），結果是平均任務價值在這段期間上升約 27%（報告摘要另寫約 25%）。

## 這份報告沒說、但更該想的事

這份報告有它老實承認的限制：它看不到真實世界的結果，無法得知一次對話寫出來的程式碼後來到底有沒有被用；它也排除了「非互動式」用法（例如把 Claude Code 嵌進自動化流程），而那是相當大一塊。所有分類都來自模型讀逐字稿的判斷。所以這是一張「早期快照」，不是定論。

更值得知識工作者放在心上的，是報告結尾埋的那個提問。Anthropic 說，他們會持續追蹤一件事：如果「領域知識的回報」哪天開始下降，那就代表模型開始能自己供應使用者目前還得自己帶進來的那種判斷力。

這份報告的啟示是：你不必為了「不會寫程式」而焦慮著去補一門程式課，更划算的投資，是把你本來就在做的那個領域弄得更透、把「什麼叫做對」說得更清楚。

先把問題想清楚，再交給 AI 加速；先能驗收，再敢放手。

資料來源： [Anthropic — Agentic coding and persistent returns to expertise](https://www.anthropic.com/research/claude-code-expertise)

本文初稿為 AI 編撰，整理．編輯／李先泰

關鍵字： [＃Anthropic](https://www.bnext.com.tw/tags/Anthropic) [＃Claude](https://www.bnext.com.tw/tags/Claude)

[![](https://edm.bnext.com.tw/epaper/banner/bn_add_line.png)](https://lin.ee/ptiP6Md) [![](https://edm.bnext.com.tw/epaper/banner/fc_new_web.png)](https://fc.bnext.com.tw/?utm_source=web_bn&utm_medium=article_bottom)

<iframe allow="autoplay *; encrypted-media *; fullscreen *; clipboard-write" frameborder="0" height="175" src="https://embed.podcasts.apple.com/us/podcast/%E6%95%B8%E4%BD%8D%E9%97%9C%E9%8D%B5%E5%AD%97245-ai-%E6%99%82%E4%BB%A3%E9%82%84%E9%9C%80%E8%A6%81-pm-%E5%97%8E-%E7%95%B6%E7%95%8C%E7%B7%9A%E6%B6%88%E5%A4%B1-%E7%AE%A1%E7%90%86%E8%80%85%E9%82%84%E5%89%A9%E4%BB%80%E9%BA%BC/id1555393924?i=1000773477712"></iframe>

往下滑看下一篇文章

<video title="Advertisement" src="https://rr5---sn-ipoxu-un5d.googlevideo.com/videoplayback?expire=1782134054&amp;ei=psQ4avjlNqSW1d8PyvCHqAE&amp;ip=60.250.84.232&amp;id=18371bf0223c789d&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=526&amp;met=1782105254,&amp;mh=Bj&amp;mm=31&amp;mn=sn-ipoxu-un5d&amp;ms=au&amp;mv=m&amp;mvi=5&amp;pl=21&amp;rms=au,au&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;vprv=1&amp;rqh=1&amp;dur=78.994&amp;lmt=1760762257668657&amp;mt=1782104700&amp;cpn=u2LHa6Cr0QaNloLP&amp;txp=5532534&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,vprv,rqh,dur,lmt&amp;sig=AHEqNM4wRQIgfA-VDPYkdQE-VMM6ddpXS_aWbvpdToLNtdITMoIJjTMCIQDHPBQkzr1U3rOBEvgSeDCV4J0vN8Lo3UqlNuMxD59Nbw==&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pl,rms&amp;lsig=APaTxxMwRgIhAIb-eyqKgczvzsQrGQh4VpA_tSg8sI_qVXl-uLb7NFDlAiEA4ypyff0vKRKb3-iSIdyxtqcU0_1Sv43_W4uKWfH3pmg=" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.772.0_zh_tw.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_807888481%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C745150931%2Cnull%2C%5Bnull%2C1%5D%5D%2C%5Bnull%2C749060184%2Cnull%2C%5Bnull%2C128%5D%5D%2C%5B841585769%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45774999%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2Cnull%2C45792627%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45765927%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45661356%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C500%5D%5D%2C%5Bnull%2C45767902%2Cnull%2C%5Bnull%2C500%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761216%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B1%2C%5B%5B31089630%5D%2C%5B31089631%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B31097690%5D%2C%5B31097691%2C%5B%5B846355750%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95345206%5D%2C%5B95345207%2C%5B%5B45661356%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B100%2C%5B%5B95356068%5D%2C%5B95356069%2C%5B%5B45685601%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5B%5D%5D%5D%5D%2C%5B95356070%2C%5B%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5B%5D%5D%5D%5D%2C%5B95356071%2C%5B%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95382403%2C%5B%5B45794243%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95386528%2C%5B%5Bnull%2C45767902%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95390088%5D%2C%5B95390089%2C%5B%5B45784857%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B10%2C%5B%5B95391980%5D%2C%5B95391981%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fpagead%2Fmanaged%2Fjs%2Factiveview%2Fcurrent%2Frx_omid_video.js%5C%22%5D%5D%5D%5D%2C%5B95391982%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fomsdk%2Freleases%2Flive%2Fomid_monitoring_bin.js%5C%22%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95392425%5D%2C%5B95392426%2C%5B%5B45761216%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95393784%5D%2C%5B95393785%2C%5B%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=331207400615069&amp;pvsid=3106842963331837&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F91272%2Fanthropic-vibe-coding-2026%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="169"></iframe>

即時熱門文章

[1 SpaceX收購Cursor！在600億美元重磅交易背後，黃紹儒怎靠「GitHub履歷」當上創始工程師？](https://www.bnext.com.tw/article/91262/cursor-spacex-600-billion-deal-taiwan-engineer) [2 Gemini完整教學地圖！35篇實測整理，Notebooks、Canvas、Workspace、提示詞架構全打包](https://www.bnext.com.tw/article/91249/gemini-learning-map) [3 韓國拿下Anthropic關鍵AI授權，台灣卻被跳過？三個商業現實一次說清](https://www.bnext.com.tw/article/91286/anthropic-skips-taiwan-korea-mythos-ai-license) [4 提示詞越改越爛？Anthropic工程師：問題不在模型，4步驟救回失準AI系統](https://www.bnext.com.tw/article/91260/prompt-optimization-4-steps-anthropic) [5 記憶體最大買家換人了！威剛董座：別再用4年循環看產業，Google、Microsoft才是新主力](https://www.bnext.com.tw/article/91250/ai-memory-industry-cycle-adata) [6 何麗梅捐100張台積電給政大，為何不捐現金？她曝捐股哲學：優質股會增值，股息是活水](https://www.bnext.com.tw/article/91288/ho-li-mei-tsmc-stock-donation-nccu-chungni-fund)

![從缺工到食安：古吉系統科技如何推動餐飲業數位化的下一波浪潮？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/kpyf-1781070202.jpg?w=900&output=webp)

2026.06.11 | [新零售](https://www.bnext.com.tw/categories/newretail)

從缺工到食安：古吉系統科技如何推動餐飲業數位化的下一波浪潮？

古吉系統科技於 Computex 2026 展出 3 款台灣首創、自主研發的自助服務設備，不僅展現其系統整合實力，也在國際市場上展現高度差異化優勢。

---

分享

Computex 2026 圓滿落幕，除了 AI 與機器人應用之外，服務業數位化也成為展場上的另一項焦點。其中，自助服務設備品牌古吉系統科技展出的多款新品，以「雙機整合」概念吸引不少參觀者目光。無論是雙面自助點餐機、POS 與自助點餐二合一雙系統機，或桌上型 1 托 2 點餐機，皆為台灣首創、自主研發的創新設計，不僅展現古吉深厚的產品研發與系統整合能力，放眼國際市場也具有高度差異化。

古吉系統科技總經理吳三奇表示，古吉長期聚焦於如何用更高效率、更低成本的方式協助業者解決營運痛點，希望透過持續創新，滿足不同場域與業態的需求，進一步推動服務業數位轉型。

![#3 從缺工到食安：古吉系統如何推動餐飲業數位化的下一波浪潮？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/8gvu-1781070203.jpg?w=1200&output=webp)

圖／ 數位時代

## 從連鎖品牌走向街邊小店，自助設備迎來新一波普及潮

事實上，相較於連鎖餐廳、速食店或手搖飲品牌，自助設備在傳統市場、市集攤販、街邊小吃等中式餐飲場域的普及率一直不高。古吉系統科技總經理吳三奇分析，背後原因主要來自資訊落差、經營習慣與成本考量三大因素。

不過，隨著食藥署修正《食品良好衛生規範準則》，明定餐飲從業人員在調理即食食品時，手部不得接觸現金，這樣的市場現況正逐漸出現改變。關鍵在於，傳統餐飲業者的人力本就有限，「老闆一人負責點餐、備餐兼找零」的作業模式已成常態，即便有意增聘人力，也往往受限於缺工問題而不易實現。

在此背景下，自助點餐機/售票機、自助付款機等自助設備成了最佳解答。吳三奇形容，業者導入自助設備就像多聘請一位櫃檯人員，不僅能協助處理點餐、收款等重複性工作，符合法規要求及降低第一線人力負擔，也能減少找錯錢、收到假鈔，以及尖峰時段來不及應對顧客需求等問題，讓業者能將更多心力投入餐點製作與服務品質提升。

![](https://www.youtube.com/watch?v=IsGjHWDcvS0)

## 從早餐店到主題樂園，自助設備如何解決不同產業的營運痛點？

而從古吉服務的客戶案例來看，自助設備所解決的問題，不只是作業效率而已，而是涵蓋人流分散、收款管理與消費體驗等不同面向，且應用場景橫跨餐飲業、觀光休閒等各種服務場域。

像連鎖早餐品牌晨間廚房便透過自助設備解決尖峰時段的人流問題。由於早午餐產業的顧客高度集中在特定尖峰時段，且普遍不願久候，因此，點餐與出餐效率往往直接影響門市營運表現。為此，晨間廚房在原有的 POS 與手機點餐系統外，再導入古吉自助點餐機進行分流顧客，不僅提供更多元的點餐選擇，也有效降低櫃檯壓力並提升整體點餐與出餐效率。

而手搖飲品牌茶之魔手則透過自助收款設備改善門市收銀流程，其將自助收款機整合既有 POS 系統，店員只需負責點餐即可，由消費者自行完成付款流程，不僅減少收款、找零所耗費的時間，也能降低錯誤風險，進而快速消化排隊人流。

值得注意的是，自助設備的應用也早已跨出餐飲產業。例如主題樂園遠雄海洋公園便導入自動售票機並整合園區消費系統，遊客在購票的同時，還可取得折價卷，折抵在園區內的消費，不僅降低售票窗口的人力需求，也讓遊客從購票到入園的流程更加順暢。

![#1 從缺工到食安：古吉系統如何推動餐飲業數位化的下一波浪潮？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/z2c1-1781070202.jpg?w=1200&output=webp)

圖／ 數位時代

## 從店家痛點出發，打造跨產業的一站式解決方案

從餐飲到觀光，不同產業面臨的營運挑戰雖然各不相同，卻都能在古吉的解決方案中找到對應答案，關鍵在於，古吉從市場需求角度出發，建立起涵蓋點餐、收款、叫號、廚房作業及營運管理的一站式解決方案，並透過多元產品組合滿足不同場域需求。

吳三奇進一步說明，古吉系統科技早在 2014 年便投入研發自助點餐系統，長期與第一線店家合作的過程中，發現許多業者面臨相似的經營難題，例如：人力不足、店面空間有限等。因此，古吉的產品開發始終圍繞著「省空間、更有效率、降低成本」三大目標，希望透過科技協助店家減輕人力負擔，同時優化營運流程。

正因如此，古吉系統科技不斷研發新產品，目前市場上多數品牌僅能提供 3 至 5 種機型，古吉卻已發展出 12 至 15 種不同尺寸與安裝形式的設備，讓業者能依照自身業態與空間條件選擇最適合的配置。

![](https://www.youtube.com/watch?v=XrsR7EvMexM)

這樣的研發思維，也體現在古吉 2026 年推出的多款新產品上。例如：雙面自助點餐機採用兩個螢幕共用一台主機的設計，可同時服務兩位顧客；桌上型 1 托 2 點餐機則讓兩台點餐機共用一台付款設備，在有限空間中提升服務量能；而 POS 與自助點餐二合一雙系統機，則兼具店員服務與顧客自助操作兩種模式，可依現場需求靈活調整，無論是店員點餐、顧客自助結帳，或由顧客自行完成點餐與付款，都能有效提升整體營運效率。

「我們不是為了追求產品數量而開發新產品，而是從店家真正遇到的問題出發。」吳三奇表示，正因為持續思考如何解決店家在人力與空間上的限制，古吉才能持續創新產品，並成為台灣商家導入數量最高的自助點餐機品牌。

展望未來，吳三奇認為，隨著食安規範逐步落實，加上缺工問題短期內難以緩解，自助設備在服務業中的角色也將持續轉變，從過去提升效率的選配工具，逐漸成為維持營運、兼顧合規與優化顧客體驗的基本配備，並進一步推動餐飲業展開新一波數位轉型。

![#0 從缺工到食安：古吉系統如何推動餐飲業數位化的下一波浪潮？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/2fpc-1781070202.jpg?w=1200&output=webp)

圖／ 數位時代

面對市場需求持續升溫，古吉系統科技也將持續投入產品創新，包括導入 AI 應用、縮減設備體積，以及開發更多符合不同場域需求的解決方案。同時，古吉也正積極布局日本、香港、越南等海外市場，希望將台灣自主研發的自助服務技術推向國際，讓更多企業透過科技提升營運效率與服務品質。

即時熱門文章

[1 SpaceX收購Cursor！在600億美元重磅交易背後，黃紹儒怎靠「GitHub履歷」當上創始工程師？](https://www.bnext.com.tw/article/91262/cursor-spacex-600-billion-deal-taiwan-engineer) [2 Gemini完整教學地圖！35篇實測整理，Notebooks、Canvas、Workspace、提示詞架構全打包](https://www.bnext.com.tw/article/91249/gemini-learning-map) [3 韓國拿下Anthropic關鍵AI授權，台灣卻被跳過？三個商業現實一次說清](https://www.bnext.com.tw/article/91286/anthropic-skips-taiwan-korea-mythos-ai-license) [4 提示詞越改越爛？Anthropic工程師：問題不在模型，4步驟救回失準AI系統](https://www.bnext.com.tw/article/91260/prompt-optimization-4-steps-anthropic) [5 記憶體最大買家換人了！威剛董座：別再用4年循環看產業，Google、Microsoft才是新主力](https://www.bnext.com.tw/article/91250/ai-memory-industry-cycle-adata) [6 何麗梅捐100張台積電給政大，為何不捐現金？她曝捐股哲學：優質股會增值，股息是活水](https://www.bnext.com.tw/article/91288/ho-li-mei-tsmc-stock-donation-nccu-chungni-fund)

X

掌握最新 AI 發展趨勢！

立即訂閱《數位時代》日報、《一天一AI》圖解日報 訂閱即同意 [巨思文化隱私權政策](https://account.bnextmedia.com.tw/privacy-policy)

[![代理式商務連動百兆商機](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/8e6m-1779880883.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130167)