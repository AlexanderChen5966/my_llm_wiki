---
title: "Claude幫自己寫Code！Anthropic兩周讓App快3倍：如何用4撇步，讓AI自己找問題跟驗收？"
source: "https://www.bnext.com.tw/article/92386/anthropic-claude-ai-3x-faster-performance-sprint"
author:
  - "[[李先泰]]"
published: 2026-09-28
created: 2026-09-29
description: "Anthropic用Claude衝刺兩周，13項核心指標平均快3.1倍、3,000多次修改零事故。關鍵是先建量化基準讓AI驗收指標，工程師把關上線。"
tags:
  - "clippings"
status: processed
---
[![](https://pk-img.learnin.tw/prod/photos/2026-09/img-1790646651-30210.gif)](https://edm.bnext.com.tw/martech-trends/?utm_source=web_bn&utm_medium=logo_banner&utm_campaign=2026_martech&utm_content=175749&utm_term=channel_all)

2026.09.28 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

Anthropic用Claude衝刺兩周，13項核心指標平均快3.1倍、3,000多次修改零事故。關鍵是先建量化基準讓AI驗收指標，工程師把關上線。

[＃Claude Code](https://www.bnext.com.tw/tags/Claude%20Code)

---

> 叫AI「幫我把這個改快一點」，就像叫人減肥卻不給他體重計，改了一堆，沒人說得出到底有沒有變好。
> 
> Anthropic在2026年9月23日公開的一篇工程文章，示範了另一種用法： **先給AI一台體重計，再讓它自己想辦法把數字壓下去。**

實驗對象是Anthropic自家的 [claude.ai](https://claude.ai/) 網頁版與Claude桌面App。Anthropic表示，旗下團隊在2026年8月的兩周內合併超過3,000次程式修改，沒有發生任何一起影響用戶的事故，也沒有任何一次需要把已上線的修改退回；依真實用戶監測資料，團隊挑選的13項核心操作指標平均（幾何平均）快了3.1倍，但不代表每項操作都快3倍。最有感的例子是網頁版：從打開到可以打字，由3.1秒降到0.55秒。

文章共同作者之一、Anthropic工程師Raymond Wang在官方開發者部落格發表 [〈How we made claude.ai 3x faster in two weeks〉](https://claude.dev/blog/how-we-made-claude-ai-faster/) ，公開整套做法。團隊把工作搬進一個 [Slack](https://slack.com/) 頻道，大量實作交給 [Claude Tag](https://claude.com/product/tag) （Anthropic放進Slack的AI隊友功能，beta版）：找出拖慢速度的地方、設計測試、提交修改、盯著每次上線；工程師負責定目標、做取捨、批准每一筆修改。

### 第1步：給AI一份職務說明

衝刺開始前，團隊在頻道裡給Claude一份常駐指令，內容像一份新人的職務說明：負責什麼、要主動做什麼、要跟誰回報。以下依原文改寫成中文範本，方括號處換成自己的產品：

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

```
@Claude 你的工作是負責［產品名稱］效能相關的一切事務。
你的職責包括：
1. 監控每次部署，找出效能退步
2. 評估現有監測數據是否準確、完整
3. 維護一份整理好的監控儀表板
4. 主動修正觀察到的問題與容易處理的改善點
5. 提出值得投入的效能專案
6. 隨時向人類隊友回報
這個頻道的最終目標，是讓你盡可能自主運作，但我們知道今天還做不到。
```

這份說明把AI的角色從「等人交辦」改成「對結果負責」。最後一句同時設下期待與邊界：方向是自主，但人還在迴圈裡。團隊接著列了約20個改善專案，請Claude估算每個專案能省下多少時間，據此訂出目標；結果第3天就達成13項目標裡的12項。

### 第2步：先找一把量得準的尺

體重計不準，減再多也看不出來。工程師Sam問Claude，除了秒數還能量什麼，Claude列出好幾種每次量都一樣的指標，例如程式執行的指令數、畫面重新繪製的次數。11分鐘後，5條討論串已經各自針對一種指標開工。

每把新尺都要先過一關：證明把它壓低，用戶真的會感覺變快；證明不了就撤掉，不讓AI在沒有意義的數字上白忙。可以這樣下：

```
請針對［要改善的流程］，提出3到5種在測試環境可重複量測、結果穩定的指標（例如執行次數、重新渲染次數），不要只用實際秒數。
接著逐一證明：把這個指標壓低，是否真的能換到用戶感受得到的速度提升。
無法證明的指標，請直接列出並建議撤除。
```

### 第3步：讓AI自己試、自己看結果

每條討論串都由人開頭，描述哪段操作很慢，通常附上截圖或錄影。接下來AI跑一個固定循環：先設計能重現問題的測試，修改後確認測試過關，送上線後看真實用戶的數據；變快了就把門檻往下調，沒變快就撤回重來，然後去找下一個慢的地方。工程團隊可以直接這樣下：

```
這條討論串只處理［某個流程的某一段］。請依序：
1. 追蹤這段流程，找出或建立能重現問題的基準測試
2. 在測試環境取得改善後，依風險拆成數個修改送審，用戶看得到的變動一律加功能開關
3. 部署後讀取真實用戶數據
4. 有改善就把基準門檻往下調並鎖住；沒改善就關掉開關、重新嘗試
5. 完成後，在同一個流程裡找下一個慢點，不要停
```

![Anthropic將效能改善拆成持續循環](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/jbds-1790239395.png?w=2000&output=webp)

Anthropic將效能改善拆成持續循環：AI負責建立基準、拆分修改與讀取數據，工程師負責設定目標、審查修改及控制部署風險，再依實際成效調整門檻或重新嘗試。

圖／ 數位時代

實例是側邊欄跳動。有人分享一段錄影：頁面載入後，側邊欄的對話列表會陸續彈出、位置亂跳。既有監控卻都沒抓到，因為業界常用來衡量畫面跳動的指標，每次只算出約0.008分，遠低於0.1的警戒線。

工程師Issac提議直接讀瀏覽器記錄的原始跳動資料，Claude據此設計新的監測方式，並寫了一個專抓跳動的自動測試：舊版跑20次全部失敗，修正版跑20次全部通過。上線後看真實數據，發現31%的網頁載入在已經能操作之後，畫面還在跳動，Claude再一批一批修掉主要原因。

循環能轉這麼快，前提是安全措施先裝好。每筆修改都經過自動審查，並至少由一名工程師批准；動手改之前先寫好檢查，確保原本的功能不會壞掉；用戶看得到的改動都裝上「功能開關」，出問題可以一鍵關掉。風險高的改動則像餐廳推新菜，先給員工試用，再開放給1%用戶，最後才全面上線。兩周內團隊建了近200個功能開關，衝刺結束前已清掉一半以上。

### 第4步：人負責推一把、拍板體驗、定方向

原文說得很白：這個循環很有生產力，但並不自主。人的工作分三塊。

**推一把。** Claude預設謹慎，會把發現先記成待辦、估時程時留緩衝。有一次Claude說修改「這周會送出」，Raymond Wang回它：現在送，我馬上處理上線，「please be braver」（請更大膽一點）。Claude改口一小時內送出。目標達成後進度開始放慢，Sam逐條留言：目標不是終點。對應的指令可以這樣寫：

```
目標不是終點。能現在送出的修改就現在送出，不要只記成待辦；時程請給實際可行的最快估算，不要預留緩衝。還有哪些方向沒試過？大膽一點。
```

**拍板體驗。** 每條討論串都有一名工程師負責，只要是用戶看得到的改動，Claude就附上改前改後的截圖或錄影，由人拍板，例如表格該一格一格出現還是整列出現、載入時的灰色預留框該立刻出現還是等半秒。

**定方向。** 每條討論串刻意只盯一件事，由人決定先做哪裡、哪些討論串該合併、何時收手。一個900行的修改，只為每次送出訊息省2毫秒，被一句話打回：不值得為此多維護一套額外工具。

### 哪些情境最適合這套做法？

- 問題能變成穩定的數字，例如等待時間、錯誤率
- 團隊已有自動化測試，能把門檻設成只降不升
- 產品能用功能開關與分階段上線，出錯可以立刻關掉

### 這套方法有什麼前提與限制？

先看工具。團隊用的是Anthropic內部研究模型，原文形容「大致相當於Opus 5.5」，外部用戶拿到的模型不一定有同樣表現；Claude Tag在2026年6月23日以beta版上線，開放給Team與Enterprise方案，上線公告寫明搭配Opus 4.8。

再看組織，這群工程師有權限隨時批准、立刻上線，如果你的公司每筆修改要走好幾層簽核，速度會明顯打折。

數字也要保留。3.1倍是13項指標的平均值（幾何平均），計算方式是比較2026年8月13日與8月27日的第75百分位，白話說，在所有測量樣本中，約75%的等待時間不超過這個值。各項落差很大：桌面App從完全關閉到打開只快1.9倍，Claude Cowork雲端模式送出訊息則快了19倍。

《RuntimeWire》指出，這是公司自行公布的結果，文章沒有公開流量樣本與計算細節，也無法說明變快是否帶動留存或營收。Anthropic自己也承認還沒做完：體驗最慢的5%、其他使用流程與超長對話，都還有改善空間。

### 結語

先造一把尺，再讓AI去爬；先證明尺量得對，再把門檻鎖住；最後，人仍須決定往哪裡爬、爬到哪裡停，並負責審查修改與控制上線風險。如果你手上的問題也能變成一個數字，就能用同一套邏輯試試看。

> 延伸閱讀： [遊戲業為何集體「炒冷飯」？東京電玩展直擊：老IP回歸、三麗鷗跨界搶市場，3大趨勢一次看](https://www.bnext.com.tw/article/92324/tokyo-game-show-2026-three-trends)

資料來源： [Anthropic工程部落格](https://claude.dev/blog/how-we-made-claude-ai-faster/) 、 [Anthropic官方公告（Claude Tag）](https://www.anthropic.com/news/introducing-claude-tag) 、 [《RuntimeWire》](https://runtimewire.com/article/anthropic-claude-app-performance-sprint)

本文初稿為AI編撰，整理．編輯/ 李先泰

關鍵字： [＃Claude Code](https://www.bnext.com.tw/tags/Claude%20Code)

[![](https://edm.bnext.com.tw/epaper/banner/bn_add_line.png)](https://lin.ee/ptiP6Md) [![](https://edm.bnext.com.tw/epaper/banner/fc_new_web.png)](https://fc.bnext.com.tw/?utm_source=web_bn&utm_medium=article_bottom)

<iframe allow="autoplay *; encrypted-media *; fullscreen *; clipboard-write" frameborder="0" height="175" sandbox="allow-forms allow-popups allow-same-origin allow-scripts allow-storage-access-by-user-activation allow-top-navigation-by-user-activation" src="https://embed.podcasts.apple.com/tw/podcast/%E6%95%B8%E4%BD%8D%E6%99%82%E4%BB%A3-business-next/id1555393924"></iframe>

往下滑看下一篇文章

<video controls=""></video>

Current Time 03:09

Duration 04:20

Claude大改版！Cowork併入Chat，不用再切模式｜20260917

開啟音效

<iframe frameborder="0" width="0" height="0" src="https://player.svc.litv.tv/v3/ex.html?id=ppn-Ym5leHRfd2Vi:60CD98306BA776E1F6.-0"></iframe><video title="Advertisement" src="https://gcdn.2mdn.net/videoplayback/id/5180a5b96c32dcd8/itag/37/source/web_video_ads/xpc/EgVovf3BOg%3D%3D/ctier/L/acao/yes/ip/0.0.0.0/ipbits/0/expire/1822204904/sparams/ip,ipbits,expire,id,itag,source,xpc,ctier,acao/signature/099FB780EAF60FC42BEDB374D7DE8BE6934A7850.8AF07A2D8710EF4A2794E3F64827982A7F4219D4/key/ck2/file/file.mp4" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.791.0_en.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_717837779%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B957164676%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45803173%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45830425%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B83323870%5D%2C%5B83323871%2C%5B%5B45830425%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B50%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95394594%5D%2C%5B95394595%2C%5B%5B45803173%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398837%5D%2C%5B95398838%2C%5B%5Bnull%2C960449109%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95401453%5D%2C%5B95401454%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C83323636%2C95332046%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=1328566195936345&amp;pvsid=2886640532951369&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F92386%2Fanthropic-claude-ai-3x-faster-performance-sprint%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="169"></iframe>

即時熱門文章

[1 記憶體真的缺貨？陳俊聖：DDR4、一般DDR5很充裕，真正缺的是高階零組件](https://www.bnext.com.tw/article/92343/acer-memory-ddr4-ddr5-shortage) [2 Claude官方課程有中文版了！22門全免費，涵蓋入門到API開發，完課還能拿證書](https://www.bnext.com.tw/article/91963/claude-academy-chinese-courses) [3 把LINE變成待辦小幫手！4步驟教你用Google Apps Script串接Gemini，一句話自動排入日曆](https://www.bnext.com.tw/article/91909/line-ai-todo-gas-gemini) [4 ChatGPT指令大全！日系美術插畫風、照片轉成羊毛氈、裝滿你的記憶鐵盒⋯140組提示詞一次整理](https://www.bnext.com.tw/article/90908/chatgpt-image-2-prompt-guide-complete) [5 Gemini完整教學地圖！39篇實測整理，Notebooks、Spark、提示詞架構全打包](https://www.bnext.com.tw/article/91249/gemini-learning-map) [6 Gemini Notebook教學地圖！AI筆記本、簡報製作、財報解讀28篇精華用法一次看](https://www.bnext.com.tw/article/91193/notebooklm-complete-guide-and-use-cases)

![當技術不再是門檻，內容產業如何贏在未來？  2026 亞洲新媒體高峰會「REIMAGINE」前瞻思考十年後的文創生態與賽道](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/2vdj-1789441503.jpg?w=900&output=webp)

2026.09.16 |

當技術不再是門檻，內容產業如何贏在未來？ 2026 亞洲新媒體高峰會「REIMAGINE」前瞻思考十年後的文創生態與賽道

---

分享

近來每場產業論壇都在談AI，大多數談的是「怎麼用AI」。這次，我們想換個角度：「為什麼用AI？用來做什麼？十年後內容生態會是什麼樣貌？我們如何佈局？」今年NMEA（新媒體暨影視音發展協會）以「REIMAGINE 想像新視界」為主題，將於11月25日至26日舉行2026第八屆亞洲新媒體高峰會，號召影視內容產業跳脫短期焦慮，以長遠眼光看待未來，邀請影視音、科技、資本與文化決策者進行前瞻對話。研討的重點不在技術的創新，而是希望邀請大家發揮想像、開拓文化創意的未來新視野。

## REIMAGINE，啟動想像新視界

「REIMAGINE」意即重新想像，不僅指向內容形式的翻新，也意味著既有流程、組織與商業模式的重組；「想像新視界」，則取「世界」、「視野」與「視聽」等複合意涵，呼應新媒體暨影視音發展的無限可能。

「內容產業最近太焦慮了。」NMEA理事長李芃君指出，對內容從業者與企業決策者而言，當下的焦慮已不再僅是「如何使用不斷迭代的新工具」，而是更關鍵的生存命題：當創作技術門檻被弭平，內容的真正價值該如何被重構？台灣的產業優勢是什麼？協會每年透過年度關鍵字呼應產業發展節奏，走過七屆高峰會，從早期的鼓勵擁抱變革，到去年的韌性突圍；第八屆峰會，NMEA理事長李芃君指出，「我們不想再強調現下追逐AI的焦慮，希望將視野拉長至未來十年，邀請產業領袖談談他們心中對未來的想像與願景。十年後這個產業會呈現什麼樣貌？確立中長期目標，能讓我們更有方向性地穩步前行。」NMEA 秘書長袁支翔則表示：「這將是歷屆以來，思維最自由、最具探索性的一次高峰會！」

## 五大主軸匯流，勾勒產業共同目標

「AI帶來結構性的改變，它滲透進整個產銷的價值鏈。」李芃君說明，從前期腳本開發、現場拍攝到後期製作，AI的應用日益深入。同時，消費者的娛樂與觀影行為在改變，在步調緊湊、碎片化時間日益增多的生活趨勢下，敘事策略必須順應變化。

在這兩波大趨勢的交互影響下，無論是內容創作、傳播、資本或技術都在尋求新的機會。藉由技術點燃感動，是文化科技的價值與契機，李芃君以太空望遠鏡為喻：哈伯與韋伯望遠鏡同樣觀測外太空，因科技世代不同，展現出截然不同的星空樣貌。「科技是文化的放大鏡，也是轉譯工具，協助我們更好地傳遞對一個故事或創意的描述。台灣是科技島，也是多元創意的孕育地，文化科技絕對是關鍵的核心競爭力。」

本屆高峰會的議程延續NMEA的優良傳統，由全體理監事與產業專家共創研擬，將邀集超過五十位講者、展開五大主軸：「跨域資本新局、內容跨界匯流、AI創作浪潮、多螢幕時代的競合，以及在地娛樂生態系演化」的精采對話。協會也預告多項亮點議程：以AI創作浪潮為例，今年全聯中元節廣告《歸途》，以AI生成視覺搭配真人實拍，引發市場對科技表現與人文情感平衡的討論，高峰會也將邀請主創團隊，分享第一手的創意策略與成果。

內容跨界匯流方面，三立集團內容營運長王淑娟將剖析現象級節目《請世界吃桌》的創作過程，分享如何整合多元資源將在地文化推向國際；在地娛樂生態系則聚焦台灣主要都會區，探討如何與國際影視、音樂、藝文合作，延伸在地文化特色，自然帶動長期的城市品牌價值。此外，高峰會也邀請貴賓分享體育賽事凝聚全民文化認同與 Team Taiwan 的感動經驗，以及木棉花、尖端出版與巴哈姆特組成的「動漫金三角」，對談動漫產業鏈的跨界協作路徑。

## NMEA 引領產業對話

回顧過去一年，台灣文化內容展現豐沛活力，如：紀錄片《冠軍之路》記錄中華隊在世界棒球12強賽奪冠的歷程，深化了全民的情感共鳴；截至今年8月20日，國片票房累計達12億元，創下疫情後的高峰。要持續延續這股動能，制度端的協力不可或缺。NMEA長期扮演產業與政府之間的溝通橋樑，持續關注各項政策的推動成效。

放眼國際，日本已將動漫、遊戲、影視與音樂等內容產業列為國家重點產業；台灣則在2023年《文化創意產業發展法》修法時，已將以台灣內容為主的影視音產業提升至國家戰略地位。近期，面對政府發布的「AI新十大建設推動方案」，李芃君表示協會將持續建言，期盼將文化內容產業的需求一併納入國家AI藍圖。

李芃君強調，經濟成長若缺少文化底蘊支撐，難以永續強化國際地位與影響力；在產業生命力最旺盛的時刻，更必須同步鍛造屬於台灣獨特的文化價值與靈魂。「文創產業是由一群有使命感的眾人組成，我們希望能讓台灣的文化力持續提升！」隨著協會即將在2027年邁向成立十週年，她也期許，更多想像中的美好將持續從產業對話中發生。

歡迎加入 NMEA 新媒體暨影視音發展協會： [https://www.nmea.asia/join.html](https://www.nmea.asia/join.html)

即時熱門文章

[1 記憶體真的缺貨？陳俊聖：DDR4、一般DDR5很充裕，真正缺的是高階零組件](https://www.bnext.com.tw/article/92343/acer-memory-ddr4-ddr5-shortage) [2 Claude官方課程有中文版了！22門全免費，涵蓋入門到API開發，完課還能拿證書](https://www.bnext.com.tw/article/91963/claude-academy-chinese-courses) [3 把LINE變成待辦小幫手！4步驟教你用Google Apps Script串接Gemini，一句話自動排入日曆](https://www.bnext.com.tw/article/91909/line-ai-todo-gas-gemini) [4 ChatGPT指令大全！日系美術插畫風、照片轉成羊毛氈、裝滿你的記憶鐵盒⋯140組提示詞一次整理](https://www.bnext.com.tw/article/90908/chatgpt-image-2-prompt-guide-complete) [5 Gemini完整教學地圖！39篇實測整理，Notebooks、Spark、提示詞架構全打包](https://www.bnext.com.tw/article/91249/gemini-learning-map) [6 Gemini Notebook教學地圖！AI筆記本、簡報製作、財報解讀28篇精華用法一次看](https://www.bnext.com.tw/article/91193/notebooklm-complete-guide-and-use-cases)

[![一次搞懂工業AI](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/b4gn-1790235909.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130276)