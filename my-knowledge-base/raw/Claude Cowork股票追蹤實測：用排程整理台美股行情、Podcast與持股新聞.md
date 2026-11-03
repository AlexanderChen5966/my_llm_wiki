---
title: "Claude Cowork股票追蹤實測：用排程整理台美股行情、Podcast與持股新聞"
source: "https://www.bnext.com.tw/article/92691/claude-cowork-stock-tracking"
author:
  - "[[韋惟珊]]"
published: 2026-10-19
created: 2026-10-20
description: "同事用三個 Bot 分頭盯台股、美股和理財 Podcast，還能交叉比對持股有沒有被點名。我換成 Claude 實測，最後做成兩段排程和一張共用看板。"
tags:
  - "clippings"
status: processed
---
[![](https://pk-img.learnin.tw/prod/photos/2026-09/img-1790646651-30210.gif)](https://edm.bnext.com.tw/martech-trends/?utm_source=web_bn&utm_medium=logo_banner&utm_campaign=2026_martech&utm_content=175751&utm_term=channel_all)

2026.10.19 | [職場/工作術](https://www.bnext.com.tw/categories/digitalskill)

## 怎麼用AI幫忙盯盤？實測Claude追行情、整理理財Podcast，做法一次學！

同事用三個 Bot 分頭盯台股、美股和理財 Podcast，還能交叉比對持股有沒有被點名。我換成 Claude 實測，最後做成兩段排程和一張共用看板。

[＃Claude](https://www.bnext.com.tw/tags/Claude) [＃Grok](https://www.bnext.com.tw/tags/Grok) [＃提示詞（prompt）](https://www.bnext.com.tw/tags/%E6%8F%90%E7%A4%BA%E8%A9%9E%EF%BC%88prompt%EF%BC%89) [＃一天一AI](https://www.bnext.com.tw/tags/%E4%B8%80%E5%A4%A9%E4%B8%80AI)

---

早安呀！

持股行情、相關新聞、理財Podcast，每天都想追，光是把資料找齊就得花不少時間。同事最近跟我分享，他用Grok Bot搭了一套理財bot盯盤術。

他開了台股、美股各一個bot，餵給它們自己的持股，讓它們每天盤前盤後抓行情、持股漲跌和相關新聞。另外再開一個Podcast bot，找指定理財節目的最新一集，整理重點。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

最後，請Podcast bot把資訊傳給台股、美股bot，交叉比對：自己的持股有沒有被節目點名？哪些地方要特別留意？

有趣的是，同事一開始只給很粗略的指令，但AI在設計過程中會不斷反問「這樣符合你的預期嗎」，還提醒他一些原本沒想到的細節。

我聽完就在想，這真是多Agent協作的好例子！那能不能用Claude重現？

我先請Claude規劃3個子代理，特別要求它： **先反問我細節，不要急著執行。**

結果討論到最後，Claude建議我把流程改成兩段排程：一段追蹤持股行情和相關新聞；另一段在有新Podcast時，整理重點、比對持股。兩段任務的結果，再彙整到同一個「每日行情」儀表板。

從3個bot分頭查，到兩段排程接力做，這次實測最有意思的，就是跟AI一起把需求聊清楚的過程。我整理成圖卡，分享給你。

還想看我用AI來解哪些題？ [點此留言跟我說](https://bnext.surveycake.biz/s/kyG4L) ，我都會看！也可以許願，想讓我為 [一天一AI PRO](https://1day1ai.bnextmedia.com.tw/?utm_source=1day1ai_daily&utm_medium=bnext&utm_campaign=20261019) 設計怎樣的工作流～

## AI盯盤怎麼分工？台股、美股與Podcast交叉比對持股

![三個AI分頭查，最後交叉比對](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-7f138d0c7899828b.jpg)

同事把資料蒐集分給3個bot：台股、美股bot追蹤行情與新聞，Podcast bot整理指定節目的最新一集，等資料回來再交叉比對持股。

台股、美股bot每天盤前盤後抓大盤、持股漲跌和相關新聞；Podcast bot則整理節目重點與注意事項，再把資訊傳給另外兩個bot，標出自己的持股有沒有被提到、有哪些事值得留意。

## Claude Cowork提示詞：規劃3個子代理，先釐清需求再執行

![提示詞：請Claude Cowork規劃三個子代理](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-48c79b9f88e51adb.jpg)

想讓Claude Cowork幫忙設計流程，可以先交代持股、資料範圍與預期成果，再明確要求「先反問，不要執行」。我用的是這段提示詞：

> 我的持股：台股＿＿、美股＿＿。請規劃3個子代理，分別查台股、美股最新交易日的持股表現、相關新聞和大盤表現，以及指定Podcast＿＿的最新一集，想辦法整理出節目重點。
> 
> 我沒有逐字稿，也不會提供音檔連結，請你自行尋找可合法取得的公開來源。
> 
> 三份結果都回來後，再交叉比對Podcast是否提到我的持股，附上來源、可核對的時間碼及需要查證的地方，最後彙整成一份完整文件。若來源不支援時間碼，請明確註記，不要推算或補造。
> 
> 先不要執行，請反問我需要確認的細節，協助完善整個流程。

## 用Claude追蹤股票：先確認交易時間、持股與ETF比對規則

![問「該怎麼設計」，它會回你沒想到的面向](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-6baa5cb93cbcb4c7.jpg)

用Claude追蹤股票，得先講清楚「什麼時候查」和「怎樣才算命中」，才不會蒐集了一堆資料，卻漏掉自己真正想看的資訊。

我切到Cowork，用上面的提示詞來討論。Claude提醒我，台股、美股的交易時間不同，Podcast也有自己的上架時間，排程不能隨便設。

它還反問：節目提到股票代號才算命中，還是提到企業名稱也算？如果討論到持有ETF裡的成分股，要不要列入？

這些細節我一開始都沒想到，卻會直接影響最後的追蹤結果。

## Claude Cowork排程任務：定時更新股市行情，有新Podcast再分析

![要每天自己跑？Claude建議拆成兩段排程任務](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-3edbe8bf0363b6a3.jpg)

這次討論中，Claude建議把長期追蹤拆成兩段排程：定時更新行情，以及有新Podcast時才做內容分析，最後把結果寫進同一份共用資料。

第一段，依台股、美股各自的盤後時間抓大盤、持股表現和新聞，並檢查Podcast有沒有新集數。

第二段，在排程檢查到新集數後，再取得可用音檔、轉成文字、整理重點，並和持股交叉比對；沒有新集數就略過。

兩段任務的結果都更新到「每日行情」儀表板，就不用讓3個獨立bot互相傳話。

排程設定時，也要確認任務需要的資料存取與核准方式，避免執行到一半停下來等你確認。先手動試跑一次，確認抓資料、轉錄與更新儀表板都能完成，再交給排程持續跑。

## Claude整理Podcast摘要實測：從RSS找音檔，轉錄並附上時間碼

![沒有Podcast逐字稿？Claude自己找到RSS](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-150e43d83e571934.jpg)

這次實測沒有提供逐字稿或音檔連結，Claude仍從節目的RSS找到音檔，完成下載、轉錄與Podcast重點整理，還附上時間碼。

許多公開Podcast節目的RSS會提供每集音檔網址。我只告訴Claude「找最新一集，想辦法整理重點」，它就自己找到RSS、下載音檔，還安裝了轉錄工具。

最後花了25分鐘，完成一集50分鐘節目的轉錄與重點整理，連時間碼都有附上！

不過，這是這次實測的結果。你想追蹤的節目能不能找到可用來源、順利轉錄，還是得實際跑一次看看。

## AI持股追蹤注意事項：不自動下單，股票代號與轉錄疑點要查證

![只幫忙追蹤，不拿來下單！聽不清楚的要標出來](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-9f6316d471431f25.jpg)

這套AI持股追蹤流程用來蒐集與整理資訊，沒有讓AI幫忙下單。Podcast轉錄後，也要把聽得清楚的內容和根據上下文推測的內容分開，方便回頭確認。

同事的用法是幫自己盯狀況，我覺得這條線很值得守住。另外還有一個眉角：語音轉文字時，股票代號和人名容易出現誤差，AI可能會從上下文推測。

我會要求Claude把「轉寫較可靠的」和「推測的」分開列，附上對應的原始音檔時間碼，讓我知道該回去聽哪一段確認。

排程也不用一開始就追求完美。先跑幾天，發現有漏掉或判斷不準的地方，再請Claude一起調整流程和指令。

[訂閱《一天一AI》日報](https://account.bnextmedia.com.tw/guest/subscribe?new_ids=44&utm_source=web_bn&utm_medium=post&tr_medium=website&tr_site_name=1day1ai&tr_space=post) ，每天跟你分享超實用的AI小技巧！

> 延伸閱讀：
> 
> [Grok bot旅遊行程檢查教學：找出交通與證件風險，做成長輩看得懂的手冊](https://www.bnext.com.tw/article/92670/grok-ai-travel-itinerary-check)
> 
> [Claude、ChatGPT接力教學！用Google Drive共用專案，不用重講進度](https://www.bnext.com.tw/article/92640/claude-chatgpt-google-drive-handoff)

[![](https://cdn.bnextmedia.com.tw/img/16x9.png "2026 AI品牌力｜從被讀懂到被推薦")](https://eventgo.bnextmedia.com.tw/event/detail/3a6wvrnagi2ych9eca71?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "2026 AI品牌力｜從被讀懂到被推薦")

[2026 AI品牌力｜從被讀懂到被推薦](https://eventgo.bnextmedia.com.tw/event/detail/3a6wvrnagi2ych9eca71?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "2026 AI品牌力｜從被讀懂到被推薦")

[

活動詳情

](https://eventgo.bnextmedia.com.tw/event/detail/3a6wvrnagi2ych9eca71?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "2026 AI品牌力｜從被讀懂到被推薦")

關鍵字： [＃Claude](https://www.bnext.com.tw/tags/Claude) [＃Grok](https://www.bnext.com.tw/tags/Grok) [＃提示詞（prompt）](https://www.bnext.com.tw/tags/%E6%8F%90%E7%A4%BA%E8%A9%9E%EF%BC%88prompt%EF%BC%89) [＃一天一AI](https://www.bnext.com.tw/tags/%E4%B8%80%E5%A4%A9%E4%B8%80AI)

往下滑看下一篇文章

![選手「失禁💩」也要衝線奪冠！Hyrox 比賽到底多瘋狂？｜解數一下](https://src.trvdp.com/images/576a73bd94ff430fe070548e6fad3bd1da0479d2_2.jpg)

即時熱門文章

[1 全家喊「壓力很大」！統一、全聯到全家，為何都在搶當你花點數的第一站？](https://www.bnext.com.tw/article/92690/familymart-uni-president-pxmart-loyalty-points-ecosystem) [2 2026九合一選舉懶人包！11/28投票要帶什麼？18歲能投嗎？選票張數、投票所查詢一次看](https://www.bnext.com.tw/article/92598/2026-nine-in-one-elections-voting-guide) [3 「牛肉麵＋免費豆漿紅茶」為何到處都有？雙北35家原汁牛肉麵店地圖曝光，背後經營模式大追查](https://www.bnext.com.tw/article/92701/beef-noodle-shop-free-soy-milk-tea-same-menu) [4 AI只花4小時就做出爆紅遊戲！「80億分之1」模擬人生：如果重新出生，你會在哪個國家？](https://www.bnext.com.tw/article/92647/1in8b-life-simulation-game) [5 65歲免健保費三讀！資格、上路時間一次看，年輕人每年多繳3千元？](https://www.bnext.com.tw/article/92705/65-elderly-nhi-premium-exemption-2026) [6 00406A配息0.138元，每月存1萬、8年後月領3萬？100萬試算，一招滾出3400萬！](https://www.bnext.com.tw/article/92696/00406a-dividend-november-2026-compound-calculator)

![台灣不只要用國際標準，更要參與討論！TWNIC如何培養下一代網路人才，接軌國際技術社群？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/um3r-1791791181.jpg?w=900&output=webp)

2026.10.13 | [資訊安全](https://www.bnext.com.tw/categories/security)

台灣不只要用國際標準，更要參與討論！TWNIC如何培養下一代網路人才，接軌國際技術社群？

當網路標準持續演進，台灣如何從採用走向參與？TWNIC從人才培育到校園實作，協助下一代網路人才接軌IETF，累積參與國際技術討論的能力。

---

分享

打開瀏覽器、觀看串流影片，看似日常的上網行為，背後其實仰賴一套套國際網路標準運作。當網路技術持續演進，台灣除了採用這些標準，能不能進一步參與它們形成的過程？

「即便有很多事情可能要做10年才會有效果，但我想我們應該擔任這樣的角色。」TWNIC執行長余若凡指出，台灣硬體產業在全球已有亮眼成果，但對底層網路技術與國際標準的關注、參與仍有成長空間。對TWNIC而言，除了管理「.tw」、「.台灣」網域名稱與台灣IP位址資源，也希望成為台灣與國際網路社群之間的橋梁。

![TWNIC 執行長余若凡](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/xbp1-1791799133.jpg?w=2000&output=webp)

圖／ TWNIC 執行長余若凡

這也是TWNIC與國立虎尾科技大學合作舉辦IETF研習課程的背景。從IETF組織運作、RFC標準形成，到IPv6-only等新一代網路技術，課程不只協助參與者理解國際標準，更希望培養實際參與與落地的能力。透過這場合作，TWNIC也試圖串起國際社群、校園人才與產業需求，讓台灣從採用標準，進一步走向參與標準。

## 硬體做到世界級，下一步讓台灣人才走進標準制定現場

IETF（Internet Engineering Task Force，網際網路工程任務組）作為全球網際網路技術標準形成的重要場域，許多技術規範最終透過RFC文件發布。對TWNIC而言，台灣若能讓更多企業、學校、研究者與工程師參與，不只能更早掌握國際網路技術變化，也有機會把台灣經驗帶進國際討論。

TWNIC因此一方面把國際議題帶回台灣，另一方面也把台灣人才送出去。透過TWNIC Academy學員計畫，每年資助8位學員參與重要國際組織，時間長達一年；部分學員返台後更自主形成社群，近期一場交流活動便吸引近40位參加，背景從網路工程延伸至政策、法律及社會科學。「我們希望最終有更多人可以參與。」余若凡說。從在台培訓、實際參與國際組織到返台形成社群，TWNIC希望讓人才培育不只停留在單次課程，而能持續累積參與國際標準討論的經驗。

## 會設定網路還不夠，學校補上「讀懂標準」的能力

但真正想要走進IETF，光是「會用網路技術」還不夠。國立虎尾科技大學電機工程系教授兼秘書室主任秘書蘇暉凱指出，大學課堂教授TCP/IP或網路設定，多半從教科書或整理過的教材入手；真正參與國際標準討論，面對的卻是RFC等第一手技術規範。

他將其中的差距歸納成三種能力：規範閱讀、實作驗證與技術表達。工程師除了讀懂規範，也要能透過實作與工具驗證實際網路行為，最後以精準的技術英文語言與國際社群溝通。「這三種能力不是短期間就能養成。」蘇暉凱指出，因此需要逐步融入大學與研究所的課程及專題。

這也凸顯學校在合作中的角色，TWNIC負責連結國際社群、技術趨勢與參與機會，學校則把這些標準帶進人才培育與實作現場。兩者合作，才能讓「參與國際標準」從少數專家的工作，逐漸變成年輕工程師可以累積的能力。

![國立虎尾科技大學電機工程系教授兼秘書室主任秘書蘇暉凱](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/u21q-1791778108.jpg?w=2000&output=webp)

圖／ 國立虎尾科技大學電機工程系教授兼秘書室主任秘書蘇暉凱

## IPv6走向主流，校園先成為產業轉型的試驗場

IPv6正是這套合作模式的具體案例。余若凡指出，全球IPv6使用率已正式跨過五成，迎來關鍵轉折。包含越南、捷克等國皆已將全面轉向IPv6排入國家政策時程，主流行動裝置與微軟Windows作業系統也已原生支援。過去許多環境仍採取IPv4與IPv6並行的架構，隨技術、終端與轉換機制逐漸成熟，進一步走向IPv6-only的條件也開始成形。IPv6-only並不代表與既有IPv4服務切割，而是透過NAT64、DNS64等轉換機制維持連線。對產業而言，真正的問題也逐漸從「技術能不能做到」，轉向是否有足夠經驗與信心部署。

為此，TWNIC 從人才培育到校園實作，持續推動 IPv6-only 環境的應用與部署，除了自行建置 IPv6-only Demo Site，也逐步於北、中、南三地拓展示範場域與實作據點。余若凡觀察，相較於企業在商業效益尚未明朗前，對導入新技術往往較為謹慎，校園更適合作為技術驗證與人才培育的場域。透過 TWNIC 推動的實作機制，讓師生在實際環境中理解 IPv6-only 如何運作，一方面培養未來進入產業的人才，另一方面也能累積可供業界參考的部署經驗。對蘇暉凱而言，這意味著學生不能只學會 IPv6 位址格式，更要從網路維運、資安到應用發展，建立面對 IPv6 環境的完整思維。

從Demo Site、教育訓練、技術活動到實作手冊，TWNIC希望逐步降低新技術的理解與部署門檻。當IPv6走向主流，台灣面對的已不只是「網路要不要升級」；如何讓更多人才讀懂國際標準、具備實作能力，甚至走進IETF參與討論，才是台灣從標準「使用者」走向「參與者」的下一步。

![IETF研習課程](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/a1jc-1791791302.jpg?w=2000&output=webp)

TWNIC將於10月29至31日2026年臺灣網際網路研討會（TANET 2026）暨國際計算機會議（ICS）設攤展示，規劃「新世代IPv6-Only Wi-Fi無線網路體驗與建置」及「TWNIC IPv6智慧工作室（IPv6 Smart Studio）」兩大主題。歡迎各界蒞臨參觀與體驗！  
詳細介紹請參閱： [https://twnic.tw/blog/contents.php?id=253&lang=zh-tw](https://twnic.tw/blog/contents.php?id=253&lang=zh-tw)

即時熱門文章

[1 全家喊「壓力很大」！統一、全聯到全家，為何都在搶當你花點數的第一站？](https://www.bnext.com.tw/article/92690/familymart-uni-president-pxmart-loyalty-points-ecosystem) [2 2026九合一選舉懶人包！11/28投票要帶什麼？18歲能投嗎？選票張數、投票所查詢一次看](https://www.bnext.com.tw/article/92598/2026-nine-in-one-elections-voting-guide) [3 「牛肉麵＋免費豆漿紅茶」為何到處都有？雙北35家原汁牛肉麵店地圖曝光，背後經營模式大追查](https://www.bnext.com.tw/article/92701/beef-noodle-shop-free-soy-milk-tea-same-menu) [4 AI只花4小時就做出爆紅遊戲！「80億分之1」模擬人生：如果重新出生，你會在哪個國家？](https://www.bnext.com.tw/article/92647/1in8b-life-simulation-game) [5 65歲免健保費三讀！資格、上路時間一次看，年輕人每年多繳3千元？](https://www.bnext.com.tw/article/92705/65-elderly-nhi-premium-exemption-2026) [6 00406A配息0.138元，每月存1萬、8年後月領3萬？100萬試算，一招滾出3400萬！](https://www.bnext.com.tw/article/92696/00406a-dividend-november-2026-compound-calculator)

[![一次搞懂工業AI](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/b4gn-1790235909.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130276)