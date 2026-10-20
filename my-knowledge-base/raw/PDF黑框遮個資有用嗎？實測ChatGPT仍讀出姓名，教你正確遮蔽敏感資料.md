---
title: "PDF黑框遮個資有用嗎？實測ChatGPT仍讀出姓名，教你正確遮蔽敏感資料"
source: "https://www.bnext.com.tw/article/92585/pdf-black-box-ai-privacy"
author:
  - "[[韋惟珊]]"
published: 2026-10-12
created: 2026-10-13
description: "黑框遮住文字，不代表個資已刪除。實測ChatGPT、Claude仍讀出被遮住的內容，教你用圖片、PDF遮蔽工具與原始檔替換，處理敏感資料。"
tags:
  - "clippings"
status: "inbox"
---
2026.10.12 | [資訊安全](https://www.bnext.com.tw/categories/security)

## PDF蓋黑框，ChatGPT竟然還讀得到？傳給AI前，個資要這樣遮才安全

黑框遮住文字，不代表個資已刪除。實測ChatGPT、Claude仍讀出被遮住的內容，教你用圖片、PDF遮蔽工具與原始檔替換，處理敏感資料。

[＃隱私與資安](https://www.bnext.com.tw/tags/%E9%9A%B1%E7%A7%81%E8%88%87%E8%B3%87%E5%AE%89) [＃個資](https://www.bnext.com.tw/tags/%E5%80%8B%E8%B3%87) [＃一天一AI](https://www.bnext.com.tw/tags/%E4%B8%80%E5%A4%A9%E4%B8%80AI)

---

傳含有個資的PDF給AI之前，你會先用黑框把姓名蓋起來嗎？

我拿一份假文件實測，把姓名、身分證字號、電話都用黑色方塊蓋住，再把同一份PDF分別傳給ChatGPT、Claude、Gemini，請它們列出文件內容。

結果發現，光蓋黑框根、本、不、保、險！這次只有可愛龍Gemini沒讀出被蓋住的個資。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

為什麼明明遮住了，還能被讀出來？想隱去個資，再讓AI幫忙分析，該怎麼做？我整理成圖卡了！

覺得實用的話，歡迎 [點此連結](https://bnext.surveycake.biz/s/kyG4L) 跟我分享心得，或是轉寄給你的朋友，邀請他 [點此訂閱一天一AI電子報](https://account.bnextmedia.com.tw/guest/subscribe?new_ids=44&utm_source=web_bn&utm_medium=post&tr_medium=website&tr_site_name=1day1ai&tr_space=post) ～

![同一份 PDF 加上黑框，AI 其實讀得到？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-138ec09a4a3850f3.jpg?w=900&output=webp)

## PDF蓋上黑框，ChatGPT、Claude還讀得到嗎？

這次實測中，ChatGPT、Claude仍讀出了黑框底下的個資，只有Gemini的回覆沒有列出那些內容。

我製作了一份包含假姓名、身分證字號、電話的文件，用PDF編輯功能在個資上畫黑色方塊，儲存後，分別傳給3個AI工具，請它們列出文件裡的所有內容。

不過，這次沒讀出，不代表下次也讀不到，更不能據此認定某個AI工具無法讀取被遮住的文字。重點是：文件裡的個資，究竟有沒有真的被移除？

![黑框蓋在文字上面，只能防人類的眼睛](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-243b504e6c0b6132.jpg?w=900&output=webp)

## 為什麼PDF加了黑框，還可能讀到個資？

在PDF裡畫黑色方塊，可能只是把文字蓋住，並沒有刪除底下的內容。人眼看不見，程式仍可能讀得到。

如果PDF還保留原始文字資料，AI或其他程式就有機會直接擷取內容。遮住文字和移除文字，是兩回事。

![簡單文件可以截圖、加黑框後，存成圖片](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-d0ba7dffeb1ba0df.jpg?w=900&output=webp)

## 一頁文件怎麼遮個資？先截圖，再遮蓋並另存圖片

如果只有簡單的一頁資料，可以先截圖，再用小畫家等圖片工具，以「不透明」的實心方塊完整蓋住個資，另存成新的PNG或JPG圖片。

重新開啟圖片，放大檢查姓名、電話等資訊是否完整遮住，邊緣有沒有露出文字。確認後，傳給AI的是這張處理過的新圖片。

記得檢查頁面其他地方，有沒有重複出現的個資。

![PDF 可以套用遮蔽工具，真正移除文字](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-097ddd6272ba95da.jpg?w=900&output=webp)

## 多頁PDF怎麼遮個資？使用遮蔽工具，套用後再儲存

長文件可以使用Adobe Acrobat Pro等具備永久移除內容功能的PDF遮蔽工具，刪除選取的敏感文字或圖片。

操作時，標記要遮蔽的範圍後，還要完成「套用遮蔽」，並使用清除隱藏資訊的功能，處理可能殘留的隱藏文字、註解或中繼資料，最後儲存處理後的檔案。

別只看到畫面上出現黑框，就以為已經完成。詳細操作可參考 [Adobe官方遮蔽教學](https://experienceleague.adobe.com/en/docs/document-cloud-learn/acrobat-learning/advanced-tasks/protect/redact) 。

![最穩的做法：原始檔換成代號，再匯出文件](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-9b0ab655dee5f646.jpg?w=900&output=webp)

## 有Word或Google文件原始檔？先把個資換成代號再匯出

如果有原始檔，可以先用「尋找與取代」把個資換成代號，再匯出一份供AI分析的文件。例如，把「王小明」換成「客戶A」。

身分證字號、電話等格式固定的資料，也可以透過搜尋規則協助找出。例如，Google文件支援在「編輯→尋找與取代」中勾選「使用規則運算式」，搜尋符合指定模式的文字。操作方式可參考Google官方說明。

搜尋常見格式的台灣身分證字號，可以使用：\[A-Z\]\[12\]\\d{8}

這個規則用來找出「1個大寫英文字母、第1碼為1或2的9位數字」組合，不能驗證字號真偽，也可能漏掉含空格或其他格式的內容，搜尋後仍要人工檢查。

不熟悉規則運算式的話，可以請AI用假資料協助設計，再回到自己的文件操作。確認個資都已處理，再匯出新檔給AI使用。

![傳之前最後檢查：搜尋個資，再複製貼上](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-708ba585d9f00ca6.jpg?w=900&output=webp)

## 上傳PDF前怎麼檢查？搜尋個資，再複製貼上確認

重新開啟準備上傳的PDF，搜尋原本要遮住的姓名、電話等資訊，再全選文字、複製並貼到空白筆記，檢查是否還能看到個資。

如果還找得到，代表內容沒有移除乾淨，先別上傳。

即使搜尋不到、貼上也沒出現，仍不能只靠這項檢查判定處理完成。若使用PDF遮蔽工具，還要確認已套用遮蔽、清除隱藏資訊並儲存，最後再逐頁檢查有沒有遺漏。

[訂閱《一天一AI》日報](https://account.bnextmedia.com.tw/guest/subscribe?new_ids=44&utm_source=web_bn&utm_medium=post&tr_medium=website&tr_site_name=1day1ai&tr_space=post) ，每天跟你分享超實用的AI小技巧！

> 延伸閱讀：  
> [AI味怎麼去除？「去AI味改稿師」skill幫你潤稿，還能學你的寫作風格](https://www.bnext.com.tw/article/92556/remove-ai-writing-style-chatgpt-claude-skill)
> 
> [ChatGPT、Claude可以一次叫兩個工作流！多個Skill怎麼一起用？3步驟教學](https://www.bnext.com.tw/article/92530/use-multiple-ai-skills-workflows)

關鍵字： [＃隱私與資安](https://www.bnext.com.tw/tags/%E9%9A%B1%E7%A7%81%E8%88%87%E8%B3%87%E5%AE%89) [＃個資](https://www.bnext.com.tw/tags/%E5%80%8B%E8%B3%87) [＃一天一AI](https://www.bnext.com.tw/tags/%E4%B8%80%E5%A4%A9%E4%B8%80AI)

往下滑看下一篇文章

<video title="Advertisement" src="https://gcdn.2mdn.net/videoplayback/id/cf0dcbc72f93962e/itag/22/source/web_video_ads/xpc/EgVovf3BOg%3D%3D/ctier/L/acao/yes/ip/0.0.0.0/ipbits/0/expire/1823404440/sparams/ip,ipbits,expire,id,itag,source,xpc,ctier,acao/signature/28E1AAC8435A0F7DE2E750C223219E8BA19E4ACA.6A38B1572BF6B5B3E10A0CFB80AFAF212B6EF9D6/key/ck2/file/file.mp4" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.793.0_en.html#compression_dictionary_version=%22m202610080101%22&amp;deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_1879188790%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45848619%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B957164676%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45803173%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45830425%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B83323870%5D%2C%5B83323871%2C%5B%5B45830425%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95404887%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B50%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95394594%5D%2C%5B95394595%2C%5B%5B45803173%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398837%5D%2C%5B95398838%2C%5B%5Bnull%2C960449109%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95401453%5D%2C%5B95401454%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95405338%5D%2C%5B95405339%2C%5B%5B45816264%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95405391%5D%2C%5B95405392%2C%5B%5B45848619%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=513147677571681&amp;pvsid=6726352199974428&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F92585%2Fpdf-black-box-ai-privacy%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="169"></iframe>

即時熱門文章

[1 玻璃基板技術大突破！韓廠攻克填銅瓶頸，半天製程縮至幾分鐘，欣興、群創進度到哪了？](https://www.bnext.com.tw/article/92573/jntc-tgv-glass-substrate-copper-filling) [2 Muse是什麼？實測Meta爆紅AI助理：免費額度大方，3個玩法比ChatGPT更像助理！](https://www.bnext.com.tw/article/92505/muse-tutorial-hands-on) [3 Gemini也開始推Skills！常用指令打「/」就能直接叫出來，免費帳號也能用](https://www.bnext.com.tw/article/92466/gemini-skills-how-to-write-free) [4 台股ETF受益人破1800萬，但你真的懂ETF嗎？清大教授拆解4大迷思：這類型別長抱！](https://www.bnext.com.tw/article/92504/active-passive-etf-leverage-risk) [5 德宏砸12億踩雷、連虧14季，股價竟從9元飆上400元！玻纖布老廠如何翻身？](https://www.bnext.com.tw/article/92584/glotech-5475-glass-fiber-quartz-cloth) [6 Claude Opus 5.5做影片指令大全！像素風、食譜、3D遊戲到產品宣傳片，10種風格提示詞一次看](https://www.bnext.com.tw/article/92576/claude-opus-5.5)

![OpenAI 重磅講者親臨高雄｜掌握前瞻 AI 與產業轉型關鍵趨勢](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/8tci-1790150783.jpg?w=900&output=webp)

2026.09.23 | [雲端運算與服務](https://www.bnext.com.tw/categories/cloudcomputing)

OpenAI 重磅講者親臨高雄｜掌握前瞻 AI 與產業轉型關鍵趨勢

2026亞馬遜港都AI創新日10月23日將於高雄盛大登場，集結OpenAI與跨產業代表，聚焦AI轉型與應用，分享從資料治理、流程設計到部署的實務方法。

---

分享

隨著 AI 工具逐漸普及，真正拉開差距的，已不只是技術或算力的強弱，是企業能否整理資料、重新設計流程，並把 AI 融合既有工作方式，持續轉化為營運價值。

Amazon Web Services（AWS）將於10月23日於高雄舉辦「2026 亞馬遜港都 AI 創新日」邀請 OpenAI 與製造、金融、交通、零售及運動育樂等領域的實務代表，從技術趨勢、資料治理、流程設計與組織轉型等面向，拆解 AI 落地前必須處理的現實問題，協助企業重新檢視自身的導入條件與下一步布局。

## 企業下一步，不只是再導入一套 AI 工具，而是讓 AI 進入日常營運

模型可以採購，系統可以建置，但資料是否整理到位、流程能否重新設計、組織是否清楚由誰負責，才是決定技術能否持續發揮價值的關鍵。

高雄近年推動數位轉型，同樣面對這項核心課題。亞灣周邊聚集扣件、石化、鋼鐵等傳統產業，場域複雜、既有系統眾多，正因如此，更能檢驗一套 AI 導入方法是否具備複製與擴大的條件。城市發展也從建置算力基礎，進一步走向讓技術真正進入產業與治理現場。

AWS 連續六年深耕高雄，今年以「雲騰智慧大南方 齊創 AI 好未來」為主軸，將討論重心從雲端與算力基礎，推進到應用全面升級，協助城市從「有 AI」走向「AI 有感」。

政府治理是否更即時、產業流程是否真正改變、市民與使用者能否感受到服務提升，才是「AI 有感」的具體檢驗。延續「高雄試點、全臺普及、雲端出海國際」的發展路徑，10月 23 日登場的亞馬遜港都 AI 創新日要談的不只是企業「有沒有 AI」，而是導入之後企業的真實改變。

[![2026 亞馬遜港都 AI 創新日](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/3kxj-1790150927.jpg?w=2000&output=webp)](hhttps://kaohsiung-ai-cloud-roadshow-2026.com/?utm_source=bnext&utm_medium=article&utm_campaign=2026khsummit&utm_content=AWS)

2025 亞馬遜港都 AI 創新日現場，資料照

圖／ AWS

## OpenAI 重磅講者親臨高雄，從技術前沿看見企業的關鍵課題

今年活動特別邀請 OpenAI 東南亞暨台灣合作夥伴總監 Mark Jeffrey，以「前瞻 AI 如何助力產業領袖開創新局」為題，分享在 AI 技術快速演進之下，企業應如何重新思考未來布局。

面對 AI 能力邊界持續擴張，哪些應用已具備規模化條件？哪些仍需要更完整的資料、治理與組織準備？這是正在規畫下一階段 AI 投資的企業必須先回答的關鍵課題。

上午議程將從資料、組織與金融三個面向，拆解 AI 落地前必須處理的現實問題。「數據為本，AI 啟航」先回到資料基礎：外部模型跑得再快，若企業自身的欄位定義、存取權限與資料品質尚未整理到位，技術仍難以轉化為可靠判斷。

「打造 AI-Ready 組織的關鍵布局」論壇則聚焦組織當責。AI 專案通常橫跨資訊、營運與管理部門，真正困難的不只是把系統建起來，而是由誰推動流程改造、由誰認列成果，以及當 AI 產出的判斷出現問題時，責任最後應由誰承擔。公部門、製造業與雲端服務業者如何從不同角色處理這些問題，對其他組織同樣具有參考價值。

金融創新論壇將討論 AI、虛擬資產與企業轉型。金融場域對資料安全、合規、風控與可追溯性的高度要求，也提醒企業：效率不能取代治理，AI 更不能成為責任模糊的理由。能否在高監管、高風險環境中建立清楚邊界，將直接影響 AI 應用能不能被長期採用。

[![2026 亞馬遜港都 AI 創新日](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/6r5m-1790151141.jpg?w=2000&output=webp)](hhttps://kaohsiung-ai-cloud-roadshow-2026.com/?utm_source=bnext&utm_medium=article&utm_campaign=2026khsummit&utm_content=AWS)

2025 年活動論壇現場，資料照

圖／ AWS

## 從六種產業場景，看 AI 如何走進真實流程

真正具備參考價值的 AI 案例，不只要說明「做了什麼」，還要回答幾個更實際的問題：解決哪一段流程、使用哪些資料、在哪個節點交給 AI、誰負責最後判斷，又如何確認投入確實產生價值。

下午議程規劃橫跨智慧支付、製造、媒體科技、智慧交通、零售製造與運動育樂六種產業場景。每個場景面對的資料條件、即時要求與風險門檻都不相同。

| 產業場景 | 議程規劃與關鍵課題 |
| --- | --- |
| 智慧支付 | 藍新金流運用 AWS AI 自建智能 KYC 與交易監控；處理身份辨識、交易監控與風險判斷 |
| 製造 | 以語意層串接資料孤島，讓 AI Agent 正確理解分散在不同系統的資料 |
| 媒體科技 | 會說台語的 AI Agent：從醫院櫃台到第一線的落地實戰 |
| 零售製造 | 零售製造轉型；把技術接進實際營運與使用者體驗 |

這些差異進一步說明，AI 沒有一套可以直接複製到所有產業的標準答案。企業真正需要的，不只是更多案例，而是看見不同產業如何依資料條件、即時要求與風險門檻，把 AI 融入實際營運與使用者體驗。

現場同步打造「產業賦能走廊」，邀集超過 15 組雲端生態系夥伴設置展示攤位，聚焦智慧製造、金融科技與城市治理的真實痛點與解法。無論企業目前卡在技術、資料、治理，還是組織協作，不同問題點，都能找到對應的解方。

10 月 23 日，從智慧製造的產線優化、金融科技的風控升級，到運動產業的會員體驗創新，看 AI 如何從概念走進產業現場，成為百工百業轉型升級的實際動能。

[![10/23高雄展覽館盛大舉行](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/pu43-1790150560.png?w=2000&output=webp)](https://kaohsiung-ai-cloud-roadshow-2026.com/?utm_source=bnext&utm_medium=article&utm_campaign=2026khsummit&utm_content=AWS)

10/23高雄展覽館盛大舉行

圖／ AWS

> 「2026 亞馬遜港都 AI 創新日」，掌握 AI 賦能百工百業、領航產業升級的實務趨勢  
> [立即報名](https://kaohsiung-ai-cloud-roadshow-2026.com/?utm_source=bnext&utm_medium=article&utm_campaign=2026khsummit&utm_content=AWS)

即時熱門文章

[1 玻璃基板技術大突破！韓廠攻克填銅瓶頸，半天製程縮至幾分鐘，欣興、群創進度到哪了？](https://www.bnext.com.tw/article/92573/jntc-tgv-glass-substrate-copper-filling) [2 Muse是什麼？實測Meta爆紅AI助理：免費額度大方，3個玩法比ChatGPT更像助理！](https://www.bnext.com.tw/article/92505/muse-tutorial-hands-on) [3 Gemini也開始推Skills！常用指令打「/」就能直接叫出來，免費帳號也能用](https://www.bnext.com.tw/article/92466/gemini-skills-how-to-write-free) [4 台股ETF受益人破1800萬，但你真的懂ETF嗎？清大教授拆解4大迷思：這類型別長抱！](https://www.bnext.com.tw/article/92504/active-passive-etf-leverage-risk) [5 德宏砸12億踩雷、連虧14季，股價竟從9元飆上400元！玻纖布老廠如何翻身？](https://www.bnext.com.tw/article/92584/glotech-5475-glass-fiber-quartz-cloth) [6 Claude Opus 5.5做影片指令大全！像素風、食譜、3D遊戲到產品宣傳片，10種風格提示詞一次看](https://www.bnext.com.tw/article/92576/claude-opus-5.5)

[![一次搞懂工業AI](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/b4gn-1790235909.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130276)