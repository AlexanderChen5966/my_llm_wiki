---
title: "為了不要變成只會直接輸出 AI 產出的人"
source: "https://codelove.tw/@tony/post/qenG6x"
author:
published:
created: 2026-08-03
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------我當工程師已經 4 年了。現在每天都同時並行使用 [Claude Code](https://www.anthropi..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月02日](https://codelove.tw/@tony/post/qenG6x "2026-08-02 08:03:57")

## 為了不要變成只會直接輸出 AI 產出的人

## 前言

我當工程師已經 4 年了。現在每天都同時並行使用 [Claude Code](https://www.anthropic.com/claude-code) 和 [Codex](https://openai.com/codex/) 來工作。

這篇文章是我把相當一部分工作交給代理之後，親身體會到「即便如此，人類仍然要負責什麼」所整理出來的隨筆，也包含了我自己的失敗經驗。你最近交出的成果裡，有多少比例是你敢拍胸脯說是「自己的工作」？我自己在面對這個問題時一度語塞，所以才寫了這篇文章。

本文是基於 2026 年 8 月時點、個人感受整理而成，並不是在討論某項特定技術的優劣。

## 能落到情境裡的工作，現在已經可以交給代理處理了

先從現況說起。這一年來，我的工作怎麼改變了？答案是：「只要能語言化、能落到情境脈絡裡的工作」，幾乎都已經改成透過代理來跑。

- 彙總 SQL 和制式實作
- 分析報告與簡報的初稿
- 技術調查與摘要整理
- 開立發票、整理每月目標這類可以流程化的行政工作

以我自己來說，我會把團隊成員的價值觀、工作背景、營運規則整理成 Markdown，集中存到一個儲存庫裡，代理會讀這些內容後直接行動。幾年前還得花上一整天才能做完的「工作」，現在在早上的排程執行裡就自己結束了。

重點在於「能落到情境裡」這句話的意思。只要是能寫成操作手冊、能列出前提條件的工作，一旦寫得出來，就立刻進入 AI 的主場。反過來說，人類的工作就會逐漸往「還沒寫出來的東西」以及「根本無法寫出來的東西」靠攏。關於代理設計的思路，可以參考 [Anthropic 的代理建構指南](https://www.anthropic.com/engineering/building-effective-agents) ；愈讀愈會加深一種確信：只要把工作結構化，連工具和評估機制都一起建立好，竟然真的能跑到這種程度。

當然，這並不是說現階段的 AI 無所不能。METR 的研究顯示，16 位有經驗的 OSS 開發者使用 AI 工具後，實際工作速度反而平均慢了 19%，而且和他們自己的主觀感受相反。更可怕的是，研究結束後，參與者仍然相信自己「用了 AI 之後快了 20%」。連使用者自己的感受都不一定準。

但我不會把這解讀成「AI 不能用」，而是解讀成「有效的領域和無效的領域分得非常清楚」。AI 被交付的任務類型有明顯偏向這件事，也在 [Anthropic Economic Index](https://www.anthropic.com/economic-index) 裡被呈現出來。當我每天都看到那些情境完整、流程固定的領域持續進化時，愈來愈能感受到「這裡是人類的聖域，所以沒問題」的地方，年年都在縮小。

此刻真正該害怕的，我認為不是「工作被 AI 搶走」。可怕的是，自己變成那種只會把 AI 輸出左手進右手出的人。前者是環境變化，無法抗拒；後者則完全是自己的選擇。

## 那麼，「人介入的意義」還剩在哪裡？

如果只會把輸出左手進右手出，那麼在工作流程裡，你的角色就會變成「人類路由器」。路由器不是介入價值，只是延遲而已。那麼，究竟什麼還能算是介入價值？我目前的答案畫成圖，大概是這樣。

[![ai-context-5-human-jobs.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3937496%2F84e96c6d-a4db-43e4-9cfb-d792a32e5475.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1d4b4816d0fd38a7f2f71775b4285e73)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3937496%2F84e96c6d-a4db-43e4-9cfb-d792a32e5475.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1d4b4816d0fd38a7f2f71775b4285e73)

留在「無法寫進情境脈絡」那一側的人類工作有 5 種。接下來逐一說明。

## 工作①：成為別人想一起共事的人

先從最不像技術文章的話題開始，但這其實是我最有感的一點。

以前，只要會寫程式就能接到工作。即使溝通有點生硬，只靠技術本事也能吃得開，像是那種「靠一把刷子闖天下」的技術人。但在「寫程式」本身的價值下降之後，我感覺真正能只靠純技術力活下去的，大概只剩下少數那種修到電腦科學碩士等級、還能持續對開源專案提 PR 的人。

包含我在內的大多數工程師，並不是在那種賽道上競爭。  
那麼要靠什麼競爭？  
就是讓別人覺得「想跟這個人一起工作」。

好不好問問題、能不能理解需求背後的原因、遇到失誤時會不會不是責怪而是一起補救——當產出之間的差距縮小後，這些連人味在內的綜合分數就會變得相對重要。

以我自己來說，我一直不嫌麻煩地承接面向非技術人員的說明和培訓，結果反而成了最常帶來下一個案子的事。比起靠技術力碾壓別人的記憶，我更常收到的是「你講解得很清楚，之後還想再麻煩你」這種回饋，而這些回饋更容易直接變成案子。

## 工作②：做出自己的堅持

AI 的輸出有時候是不是明明正確，卻讓你完全提不起閱讀慾望？

我在 Qiita 寫文章時，當然也會用 AI，但我好幾次回頭看 AI 產出的初稿，都會想：「這種內容，我自己也會直接跳過。」內容面面俱到、很仔細，也沒有錯，但誰寫都一樣，是那種「標準解」，沒有溫度。

所以最近我會在開始寫之前，先決定「我要在哪裡堅持」。像這篇文章，我選擇的是「具體攤開自己的失敗經驗」。和標準解之間的差異，正是文章會被讀下去的原因，而這種差異的素材，只存在於自己的經驗裡。程式碼也是一樣，生成後直接拿來用，和依照自己現場的狀況再磨一層，成品的面貌會差很多。

## 工作③：把不在情境脈絡中的資訊用在決策上

代理在給定的情境脈絡裡確實會表現得驚人地聰明，但那些沒有出現在提示詞、搜尋結果、工具鏈中的獨有資訊，無法成為它的判斷材料。

之前我曾經接過一個彙總需求，心裡明明覺得「這個有點怪」，卻還是照樣往下做。結果交付之後才發現前提兜不攏，整個案子全部重做。SQL 沒錯，資料處理也沒錯，錯的是我在覺得怪的時候沒有確認就直接往下做的判斷。

現場氣氛、客戶真正的目的、期限背後的原因、只靠口頭傳達的限制——這些「不會寫在文件裡的資訊」，如果人類不提供，就不會成為 AI 的判斷依據。而且，判斷要給哪些資訊、哪些資訊不該給，也是人類的工作。重點不是 AI 輸出有沒有對，而是這種情況下到底該不該採用它。這就是所謂的決策工作。

## 工作④：承擔責任

有一天，Claude 回報說「所有測試都通過了」，我信了它，當天就收工。結果隔天一確認，根本一個都沒過。

如果那樣直接交付出去，掛在壞掉成品上的就是我的名字。代理可以大量產出內容，但責任一點都不會承擔。既然能承擔責任的主體只有人類，那這件事就一定會保留成人類的工作。

從那次之後，我給自己定了兩條規則。第一，不要省略自己親眼驗證；第二，讓另一套 AI 做交叉審查。曾經有一次把 Claude 寫的程式碼拿給 Codex 看，結果找出了 4 個潛在 bug，從此這個雙層架構就成了固定流程。讓同一系統的 AI 做自我審查，分數總是意外地寬鬆。

我認為，承擔責任的意思，是要「理解」之後再接手。現在已經進入一個代理的產出速度，開始由人類的理解速度決定上限的時代了。一旦放棄理解，那就等於拿到一張「只會原樣送出」的單程票。

## 工作⑤：不要只待在單一領域

老實說，只靠一種專業吃飯，已經變得相當困難了。因為專業領域內的工作，越來越體系化、越來越容易語言化，也就是越容易落到情境脈絡裡，因而成為代理擅長的範圍。

如果你是後端工程師，就應該盡量具備看前端和基礎架構的能力；而我也認為，往工程外的領域——例如行銷或經營——擴展，會很有用。這是一場綜合實力的競賽。

我自己也從資料分析開始，逐步把領域擴展到 Web 開發、基礎架構，以及 LLM 活用基礎設施。最近我還開始經營個人媒體事業，也把腦袋投入到行銷和事業規劃裡。跨到別的領域時，自己常常會退回新手狀態，挫折也不少，但我也有很有意思的發現。只要跨過領域邊界，「不會寫在情境脈絡裡的東西」會一下子變多。像是在零售現場聽到的話，反映到資料設計上；或是從事業規劃的需求倒推，去調整分析的粒度——這些跨域操作，沒有任何一份文件會寫，所以只要背景不是由自己來共享，AI 幾乎不會自己提出來。領域知識與跨域經驗，正因為不容易被寫進情境脈絡，所以價值才會留下來。

## 那麼，該做什麼？

雖然前面列了 5 點，但不可能明天開始就全部練起來。我自己實際在做的只有以下 3 件事。

第一， **一定要在 AI 的輸出上加上自己的差異（思考）再交出去** 。如果是程式，就要先看過並依現場情況修正；如果是文章，就補上一段自己的堅持。反過來說，如果發現自己完全不想加任何差異就直接送出，那就是「這份工作不需要我」的訊號，別猶豫，直接交給自動化。

第二， **不能解釋清楚的東西就不要輸出** 。把代理的結果，理解到能對人說明的程度之後再拿出去。這很花時間，但如果偷懶，最後就會變成前面提過的那起「所有測試都通過了」事件。

第三， **定期往隔壁領域跨一步** 。不需要誇張地轉職，只要多接手隔壁團隊的一件工作就夠了。每跨出去一次，能寫不進情境脈絡的「口袋名單」就會增加。

## 結語

AI 代理的進化真的非常快，這篇文章裡提到的「人類的 5 種工作」，幾年後也許有一半都能被寫進情境脈絡裡。但即便如此，「加上自己的差異、理解後承擔、跨出領域」這種做事方式，不管可被寫進情境脈絡的邊界怎麼移動，應該都還能一直拿來用。

最後再把開頭的問題放一次：你最近交出的成果裡，有多少比例是自己的差異？如果你也答不出來——那就跟我一樣。一起掙扎吧。這篇文章是帶著自我警惕，用 AI 一起寫完後，最後再由我補上自己的差異。

---

原文出處： [https://qiita.com/ktdatascience/items/8d2dace07c9c7a9d0453](https://qiita.com/ktdatascience/items/8d2dace07c9c7a9d0453)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qenG6x) [13瀏覽](https://codelove.tw/@tony/post/qenG6x)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qenG6x)

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

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1785721187&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqenG6x&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721187000&amp;bpp=1&amp;bdt=80&amp;idt=75&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785402984%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785402984%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785402984%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=345281322481&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=31099996%2C95396139%2C95396667%2C95397133%2C95340252%2C95340254&amp;oid=2&amp;pvsid=1788610631038434&amp;tmod=1434000446&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=114" title="Advertisement" aria-label="Advertisement"></iframe><iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260729/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=10&amp;uci=a!a&amp;btvi=4" title="Advertisement" aria-label="Advertisement"></iframe>