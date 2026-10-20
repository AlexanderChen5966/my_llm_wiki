---
title: "Anthropic推Claude Tag！每個Slack頻道都有自己的AI隊員，還會觀察、盯進度"
source: "https://www.bnext.com.tw/article/91311/anthropic-claude-tag-slack-team-collaboration-ai"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-06-24
created: 2026-06-24
description: "Anthropic發布全新團隊協作產品Claude Tag，率先以Beta形式提供給Claude Enterprise與Team用戶。"
tags:
  - "clippings"
status: processed
---
[![Meet大南方招商倒數](https://bnextmedia.s3.hicloud.net.tw/pumpkin/image/photo/2026-06/img-1782067822-54531.gif)](https://meetgreatersouth.tw/exhibitor/?utm_campaign=2026mgs&utm_source=web_bn&utm_medium=logo_banner&utm_content=166740&utm_term=channel_all)

2026.06.24 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

Anthropic發布全新團隊協作產品Claude Tag，率先以Beta形式提供給Claude Enterprise與Team用戶。

[＃Claude](https://www.bnext.com.tw/tags/Claude)

---

> 重點一：Anthropic 推出團隊協作新功能 Claude Tag，讓使用者在 Slack 以標註 **@Claude** 方式指派任務並協作完成工作。
> 
> 重點二：Claude Tag 可存取經授權的工具、資料與程式碼庫，在多個頻道中建立長期記憶與情境，主動追蹤專案進度並異步執行任務。
> 
> 重點三：Claude Tag 目前在 Slack 上以 Beta 形式開放給 Claude Enterprise 與 Team 付費客戶，管理者可細緻控管頻道權限、花費上限與操作紀錄，並提供從舊版 Slack App 的遷移方案。

AI 新創公司 Anthropic 宣布推出全新團隊協作產品 Claude Tag，率先在 Slack 平台上提供 Beta 測試。透過在頻道內標註 **@Claude** ，團隊成員可將工作任務直接委派給模型處理，讓 AI 以類似「虛擬同事」的角色參與每日協作流程。Anthropic 指出，內部產品團隊已有約 65% 的程式碼由內部版本的 Claude Tag 產出，這種工作模式正逐步從工程領域擴散至產品數據分析、客服票務處理以及疑難技術問題排查等多種業務情境。

Claude Tag 的核心設計，是在特定 Slack 頻道中運行單一共享的 @Claude 實體，所有成員都能看到它正在處理的任務並接續對話，使其運作方式更接近一位「多人共用」的團隊成員，而非孤立的一對一對話機器人。

使用者只需以自然語言下達需求，Claude 便會自動拆解任務、呼叫已授權的工具與資料來源，並在完成後於 Slack 訊息串中回報結果。除了在公開頻道被標註，使用者也能透過私訊和 Claude 互動，使用個人化設定的工具與連結器，滿足較為私密或個人工作流的需求。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

## 跨頻道記憶：不用每次從頭解釋

Anthropic 將 Claude Tag 定位為 Claude Code 發展路線上的重要一步，強調其可在多頻道、多工具環境中建立長期記憶與情境感知。當 Claude 被加入特定 Slack 頻道並取得授權後，會持續「跟隨」頻道內的討論脈絡，累積對專案背景、專有名詞與團隊習慣的理解，減少每次互動都需從頭解釋的成本。若系統管理員允許，Claude 也能從其他指定頻道與外部資料來源自動學習，但不會跨越私密頻道的界線，以避免將敏感資訊暴露於不相干的團隊成員。

除了被動回應指令，Claude Tag 還提供「主動式」（ambient）模式。啟用後，Claude 會根據所連結的頻道與工具，自動挑出可能與團隊成員相關的訊息，主動彙整更新、提出提醒，或在發現某個討論線索被擱置時重新跟進，協助追蹤尚未完成的任務。這種設計讓 AI 不僅是被動回覆的助手，也能在長時間專案中扮演主動協調者的角色。

由於 Claude 支援異步與長時間任務，使用者可以同時指派多個工作給不同頻道中的 Claude，自己則專注處理其他優先事項，讓 AI 在背景持續運行數小時甚至數天，逐步完成專案目標。Anthropic 表示，這種大量「分派給多個 Claude」並行運作的模式，已成為內部團隊日常工作習慣的一部分。

## 細緻的權限與資源管控

針對企業與組織的安全需求，Anthropic 在 Claude Tag 設計上強調精細的權限與資源管理。系統管理員可為不同用途建立各自獨立的 Claude 身分，並指定其能存取的 Slack 頻道、外部工具與資料庫。例如，配置給銷售團隊的 Claude 不會與工程團隊的 Claude 共用記憶或資料，避免營收數據或客戶資訊流入不相關的技術頻道。同時，管理者能為整個組織與個別頻道設定每月 Token 消耗上限，並透過操作紀錄檢視每一項由 @Claude 執行的任務及其發起人，方便追蹤與稽核。

Claude Tag 現階段在 Slack 上以 Beta 形式開放，適用對象為 Claude Enterprise 與 Claude Team 客戶。組織若要導入，需由管理員先將 Claude Tag 與 Slack 工作區配對，再依序開啟所需工具與連結器、設定月度預算上限，並於私密頻道中進行測試，以確認運作與安全設定無誤。

Anthropic 同時宣布，Claude Tag 將取代既有的「Claude in Slack」應用程式，管理者可在 30 天內選擇遷移，並可獲得一筆導入期優惠的啟用額度，鼓勵整個公司廣泛試用。官方說明指出，Claude Tag 採用 Opus 4.8 模型技術，並提供詳細技術文件與產品頁面，協助開發與 IT 團隊進一步整合既有系統。

> 延伸閱讀： [Codex推出Record and Replay！只要示範一次工作流程，就能讓AI記住你的SOP](https://www.bnext.com.tw/article/91292/openai-codex-record-and-replay-skills-automation-youtube-and-workflows)  
> [輝達花市場2倍薪水找「簡報視覺設計師」！AI能生成PPT，為何這份工作反而更值錢？](https://www.bnext.com.tw/article/91308/nvidia-presentation-designer)

資料來源： [Anthropic](https://www.anthropic.com/news/introducing-claude-tag)

本文初稿為AI編撰，整理．編輯/黃若彤

關鍵字： [＃Claude](https://www.bnext.com.tw/tags/Claude)

[![](https://edm.bnext.com.tw/epaper/banner/bn_add_line.png)](https://lin.ee/ptiP6Md) [![](https://edm.bnext.com.tw/epaper/banner/fc_new_web.png)](https://fc.bnext.com.tw/?utm_source=web_bn&utm_medium=article_bottom)

<iframe allow="autoplay *; encrypted-media *; fullscreen *; clipboard-write" frameborder="0" height="175" src="https://embed.podcasts.apple.com/us/podcast/%E6%95%B8%E4%BD%8D%E9%97%9C%E9%8D%B5%E5%AD%97245-ai-%E6%99%82%E4%BB%A3%E9%82%84%E9%9C%80%E8%A6%81-pm-%E5%97%8E-%E7%95%B6%E7%95%8C%E7%B7%9A%E6%B6%88%E5%A4%B1-%E7%AE%A1%E7%90%86%E8%80%85%E9%82%84%E5%89%A9%E4%BB%80%E9%BA%BC/id1555393924?i=1000773477712"></iframe>

往下滑看下一篇文章

tw\_bnext \[Dynamic Article\]-20260619-08:03

arrow\_forward\_ios

閱讀文章

00:00

00:09

00:44

 <video controls=""><source src="https://gnetwork.gliastudios.com/gnetwork/bnext.com.tw/bnext.com.tw-1781856288.285663.mp4?verify=1782277637-4GUgCo6ChW0Vic4vNg3FovwgazySdJYB93wjdoL%2BFkA%3D"> <source src="https://gnetwork.b-cdn.net/studio_backend/bnext.com.tw/bnext.com.tw-1781856288.285663.mp4?token=lVHuuEPEHzX57TfxnYNzwRv1pdnQoa8COqX_sLJi8yw&amp;expires=1782364037"></video><video title="Advertisement" src="https://rr2---sn-ipoxu-un5y.googlevideo.com/videoplayback?expire=1782306929&amp;ei=8Wc7asTSOaGSvcAPkf61iQs&amp;ip=60.250.84.232&amp;id=a812e019be51e482&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=186&amp;met=1782278129,&amp;mh=-F&amp;mm=31&amp;mn=sn-ipoxu-un5y&amp;ms=au&amp;mv=m&amp;mvi=2&amp;pl=21&amp;rms=au,au&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;vprv=1&amp;rqh=1&amp;dur=15.046&amp;lmt=1770539439068059&amp;mt=1782277752&amp;cpn=58qfIQok9IbCR93J&amp;txp=5432534&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,vprv,rqh,dur,lmt&amp;sig=AHEqNM4wRAIgeFoIpituDwv_Z1qtxH-4F2DIO1falQawn9-Ow2cP1-kCIFe6MHhNZVWH1K0X8Ckt6qxgN4qliO7P92o3gWe9SyQr&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pl,rms&amp;lsig=APaTxxMwRQIhAKsT-ak_Fl5Mk7U16wabuemiN_5MSeDdmU7ex0wpDXqCAiAo6lNeBoIFjeHhsE_rhxW3_NgkVB3gbxNIrpfIHXD9hQ==" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.773.0_zh_tw.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_134962803%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C745150931%2Cnull%2C%5Bnull%2C1%5D%5D%2C%5Bnull%2C749060184%2Cnull%2C%5Bnull%2C128%5D%5D%2C%5B841585769%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45774999%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2Cnull%2C45792627%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45765927%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45661356%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C500%5D%5D%2C%5Bnull%2C45767902%2Cnull%2C%5Bnull%2C500%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761216%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B1%2C%5B%5B31089630%5D%2C%5B31089631%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B100%2C%5B%5B31097690%5D%2C%5B31097691%2C%5B%5B846355750%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95345206%5D%2C%5B95345207%2C%5B%5B45661356%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B100%2C%5B%5B95356068%5D%2C%5B95356069%2C%5B%5B45685601%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5B%5D%5D%5D%5D%2C%5B95356070%2C%5B%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5B%5D%5D%5D%5D%2C%5B95356071%2C%5B%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95382403%2C%5B%5B45794243%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95386528%2C%5B%5Bnull%2C45767902%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95390088%5D%2C%5B95390089%2C%5B%5B45784857%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B10%2C%5B%5B95391980%5D%2C%5B95391981%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fpagead%2Fmanaged%2Fjs%2Factiveview%2Fcurrent%2Frx_omid_video.js%5C%22%5D%5D%5D%5D%2C%5B95391982%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fomsdk%2Freleases%2Flive%2Fomid_monitoring_bin.js%5C%22%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95392425%5D%2C%5B95392426%2C%5B%5B45761216%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95393784%5D%2C%5B95393785%2C%5B%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%2C95356068%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=1339930424071294&amp;pvsid=6217407163528049&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F91311%2Fanthropic-claude-tag-slack-team-collaboration-ai%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="168.75"></iframe>

即時熱門文章

[1 從缺工到食安：古吉系統科技如何推動餐飲業數位化的下一波浪潮？](https://www.bnext.com.tw/article/91219/guccisys115203) [2 記憶體最大買家換人了！威剛董座：別再用4年循環看產業，Google、Microsoft才是新主力](https://www.bnext.com.tw/article/91250/ai-memory-industry-cycle-adata) [3 Gemini完整教學地圖！35篇實測整理，Notebooks、Canvas、Workspace、提示詞架構全打包](https://www.bnext.com.tw/article/91249/gemini-learning-map) [4 00407A 6/24掛牌！首檔不配息主動ETF解析：成分股有哪些、費用率怎麼算、誰適合買？](https://www.bnext.com.tw/article/91304/00407a-etf) [5 還沒正式發布，GPT-5.6 Pro一句話就造出了《模擬市民》！OpenAI在測試什麼？](https://www.bnext.com.tw/article/91303/gpt-5-6-pro-openai-the-sims-reasoning-25-percent) [6 SpaceX收購Cursor！在600億美元重磅交易背後，黃紹儒怎靠「GitHub履歷」當上創始工程師？](https://www.bnext.com.tw/article/91262/cursor-spacex-600-billion-deal-taiwan-engineer)

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

[1 從缺工到食安：古吉系統科技如何推動餐飲業數位化的下一波浪潮？](https://www.bnext.com.tw/article/91219/guccisys115203) [2 記憶體最大買家換人了！威剛董座：別再用4年循環看產業，Google、Microsoft才是新主力](https://www.bnext.com.tw/article/91250/ai-memory-industry-cycle-adata) [3 Gemini完整教學地圖！35篇實測整理，Notebooks、Canvas、Workspace、提示詞架構全打包](https://www.bnext.com.tw/article/91249/gemini-learning-map) [4 00407A 6/24掛牌！首檔不配息主動ETF解析：成分股有哪些、費用率怎麼算、誰適合買？](https://www.bnext.com.tw/article/91304/00407a-etf) [5 還沒正式發布，GPT-5.6 Pro一句話就造出了《模擬市民》！OpenAI在測試什麼？](https://www.bnext.com.tw/article/91303/gpt-5-6-pro-openai-the-sims-reasoning-25-percent) [6 SpaceX收購Cursor！在600億美元重磅交易背後，黃紹儒怎靠「GitHub履歷」當上創始工程師？](https://www.bnext.com.tw/article/91262/cursor-spacex-600-billion-deal-taiwan-engineer)

[![代理式商務連動百兆商機](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/8e6m-1779880883.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130167)