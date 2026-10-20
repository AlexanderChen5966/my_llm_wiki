---
title: "語音AI新霸主？Gemini 3.8 Live實測奪冠還能「邊聊邊做事」"
source: "https://www.bnext.com.tw/article/92265/google-gemini-3-8-live-extended-thinking-launch"
author:
  - "[[李先泰]]"
published: 2026-09-16
created: 2026-09-17
description: "Google發表Gemini 3.8 Live雙模型，主打「邊想邊說、邊聊邊做事」，具備即時視覺與背景執行能力，更在語音代理測試奪冠。"
tags:
  - "clippings"
status: processed
---
[![](https://pk-img.learnin.tw/prod/photos/2026-09/img-1789358747-56590.gif)](https://edm.bnext.com.tw/martech-trends/?utm_source=web_bn&utm_medium=logo_banner&utm_campaign=2026_martech&utm_content=175748&utm_term=channel_all)

2026.09.16 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

## 語音AI新霸主？Google推出Gemini 3.8 Live雙模型，實測奪冠還能「邊聊邊做事」

Google發表Gemini 3.8 Live雙模型，主打「邊想邊說、邊聊邊做事」，具備即時視覺與背景執行能力，更在語音代理測試奪冠。

[＃Google](https://www.bnext.com.tw/tags/Google) [＃Gemini](https://www.bnext.com.tw/tags/Gemini)

---

Google在美國時間2026年9月15日發表Gemini 3.8 Live與Gemini 3.8 Live Extended Thinking兩款語音對話模型。

這次升級的核心是「邊想邊說、邊聊邊做事」：模型可以在對話不中斷的情況下，於背景執行工具與API呼叫，並近即時讀取鏡頭或螢幕畫面。開發者9月15日起可在Gemini API與Google AI Studio使用，一般用戶則能在Gemini Live與Search Live用到新模型。

## 兩款模型怎麼分工：一款跑量、一款想深

Google官方部落格說明，兩款模型定位不同。Gemini 3.8 Live主打規模與成本效率，強項是流暢對話與「視覺理解」（官方稱visual grounding，也就是看著畫面回答問題）。Gemini 3.8 Live Extended Thinking則鎖定高複雜度任務，具備多步推理能力。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

Extended Thinking最大的特色是「同時推理與說話」。過去語音AI遇到需要思考的問題，常會停頓一段時間再回答；新模型可先用「我查一下」這類口語回應確認指令，再於背景推理並回報進度，減少等待時的對話中斷。

兩款模型的共通能力有三項： **近即時處理視覺輸入、支援97種語言並可在對話中自動偵測與切換、在背景執行工具與API呼叫而不打斷交談。** 官方示範影片中，Gemini 3.8 Live能看著棋盤即時下棋，Extended Thinking則把手繪草圖加上語音回饋，直接產出可運作的React（網頁前端框架）元件。

## 語音代理測試拿第一，費率要看API定價頁

依官方公布的測試成績，Gemini 3.8 Live Extended Thinking在Artificial Analysis的Speech to Speech Quality Index（語音對語音品質指數）以82.6分拿下總排名第一；在τ-Voice（語音代理任務完成率測試）拿到68.6%，在Sierra的τ-Voice-banking（銀行客服情境測試）拿到35.1%。

這兩項測試考的是語音AI能否真的把客服或銀行流程從頭走完，而不只是對話順不順。推理能力方面，Big Bench Audio（音訊推理測試）得分97.7%。

Gemini 3.8 Live則在Speech Agent Arena（用戶偏好排行）排名第二，Google強調這款模型是在維持成本效率的前提下達到這個成績。至於ServiceNow的EVA-Bench語音代理測試，Google只提到兩款模型在準確度與對話品質之間取得平衡，未給出具體數字。

值得注意的是，Google在公告正文只稱兩款模型「價格具競爭力」，未詳列費率，但官方API定價頁已公布兩款模型的收費。付費方案下，音訊輸入每100萬token為3美元（約新台幣95元），音訊輸出每100萬token為12美元（約新台幣382元）；文字輸入與輸出則分別為0.75美元與4.5美元。兩款模型採同一價目表，Extended Thinking的思考token計入輸出計費。

## 誰能用、何時能用：Workspace功能綁訂閱方案

兩款模型都從9月15日起陸續推出，但開放對象與階段不同，分列如下：

- **開發者** ：兩款模型皆已在Gemini API與Google AI Studio上線。
- **企業** ：兩款模型皆為Gemini Enterprise私人預覽；Gemini Enterprise for Customer Experience標示即將推出，Extended Thinking另將開放Workspace企業客戶，Google未給時間表。
- **一般用戶** ：Gemini 3.8 Live進入Search Live；Extended Thinking進入Gemini Live，Workspace的Docs Live限Google AI Pro與Ultra訂戶，Gmail Live與Keep Live開放所有Google AI訂戶。

換言之，想在Docs裡用語音請AI改文件，得先是Google AI Pro或Ultra訂戶；未訂閱的用戶能用到的是Gemini Live與Search Live。企業端則還在私人預覽階段，尚未全面開放。

在開發者生態上，Agora、Fishjam、LangChain、LiveKit、Pipecat、Vercel與Vision Agents等七家開發平台已串接Gemini Live API，替開發者處理即時音訊串流的底層基礎設施。企業合作夥伴則包括Salesforce、Genspark與Lumeris。Google另表示，兩款模型生成的所有音訊都嵌入SynthID水印，人耳聽不出來，但可用工具偵測為AI生成內容。

> 延伸閱讀： [跟AI的對話被看光光？一次搞懂ChatGPT、Claude的9大隱私防線，4步驟保護機密不外洩](https://www.bnext.com.tw/article/92235/chatgpt-claude-privacy-9-stops-opt-out-training)

資料來源： [Google官方部落格](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-8-live-gemini-3-8-live-extended-thinking/) 、 [Gemini Live API開發文件](https://ai.google.dev/gemini-api/docs/live-api?hl=zh-tw) 、 [Gemini API定價頁](https://ai.google.dev/gemini-api/docs/pricing) 、 [Gemini 3.8 Live模型文件](https://ai.google.dev/gemini-api/docs/models/gemini-3.8-live) 、 [Gemini 3.8 Live Extended Thinking模型文件](https://ai.google.dev/gemini-api/docs/models/gemini-3.8-live-extended-thinking)

本文初稿為AI編撰，整理．編輯/ 李先泰

關鍵字： [＃Google](https://www.bnext.com.tw/tags/Google) [＃Gemini](https://www.bnext.com.tw/tags/Gemini)

[![](https://edm.bnext.com.tw/epaper/banner/bn_add_line.png)](https://lin.ee/ptiP6Md) [![](https://edm.bnext.com.tw/epaper/banner/fc_new_web.png)](https://fc.bnext.com.tw/?utm_source=web_bn&utm_medium=article_bottom)

<iframe allow="autoplay *; encrypted-media *; fullscreen *; clipboard-write" frameborder="0" height="175" sandbox="allow-forms allow-popups allow-same-origin allow-scripts allow-storage-access-by-user-activation allow-top-navigation-by-user-activation" src="https://embed.podcasts.apple.com/tw/podcast/%E6%95%B8%E4%BD%8D%E6%99%82%E4%BB%A3-business-next/id1555393924"></iframe>

往下滑看下一篇文章

<video controls=""></video>

Current Time 01:41

Duration 05:38

輝達129億美元收購Hugging Face！黃仁勳為何押注開放模型？3個理由看懂交易｜20260904

<iframe src="https://imasdk.googleapis.com/js/core/bridge3.789.0_zh_tw.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_1520346231%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B945081832%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45830425%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B83323870%5D%2C%5B83323871%2C%5B%5B45830425%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B50%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95396036%5D%2C%5B95396037%2C%5B%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95396834%5D%2C%5B95396835%2C%5B%5B45816264%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398837%5D%2C%5B95398838%2C%5B%5Bnull%2C960449109%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95401453%5D%2C%5B95401454%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95402052%5D%2C%5B95402053%2C%5B%5B957164676%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=3593158951196099&amp;pvsid=8198940933942835&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F92265%2Fgoogle-gemini-3-8-live-extended-thinking-launch%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="315" height="177"></iframe>

開啟音效

<iframe frameborder="0" width="0" height="0" src="https://player.svc.litv.tv/v3/ex.html?id=ppn-Ym5leHRfd2Vi:60CD98306BA776E1F6.-0"></iframe>

tw\_bnext \[Dynamic Article\]-20260911-08:03

00:00

00:00

00:44

 <video controls=""><source src="https://gnetwork.gliastudios.com/gnetwork/bnext.com.tw/bnext.com.tw-1789094744.946341.mp4?verify=1789608776-%2FYQuayqJDTU0Z4204OFjY%2BAb8RfmAdUQd0DS6BrjQeY%3D"> <source src="https://gnetwork.b-cdn.net/studio_backend/bnext.com.tw/bnext.com.tw-1789094744.946341.mp4?token=RmLHpGlVBNZX1ReZHO_BYnDZSkjx3Y1xqYh6YQhS3Xo&amp;expires=1789695176"></video><video title="Advertisement" src="https://rr1---sn-ipoxu-un5e7.googlevideo.com/videoplayback?expire=1789638186&amp;ei=qkWrao7zHObnpskPrYyGoAY&amp;ip=60.250.84.232&amp;id=bc3d983bd128cb66&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=209&amp;met=1789609386,&amp;mh=NS&amp;mm=31&amp;mn=sn-ipoxu-un5e7&amp;ms=au&amp;mv=m&amp;mvi=1&amp;pl=21&amp;rms=au,au&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;vprv=1&amp;rqh=1&amp;dur=10.123&amp;lmt=1784517776277051&amp;mt=1789608998&amp;cpn=Rtv_rHYP3_6BkqW1&amp;txp=6318224&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,vprv,rqh,dur,lmt&amp;sig=AE0s2JYwRQIgf0CrIOHqGlmR0w6NLzFGIl6sL5DXrNy4K0Qk2xxmotMCIQC9R_dssjpsumin3pQFpcsUOIHx6JJIKNH9AOTqOONV7A==&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pl,rms&amp;lsig=APaTxxMwRgIhAIpVi1eUA2TEZG7U71mZkrB5kxysc05aNPKM6u-TtH6wAiEAmmbtpAzuMPECOm27ugSl_bu_6hK_1ofWWxGQzRPkez4=" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.789.0_zh_tw.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_1520346233%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B945081832%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45830425%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B83323870%5D%2C%5B83323871%2C%5B%5B45830425%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B50%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95396036%5D%2C%5B95396037%2C%5B%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95396834%5D%2C%5B95396835%2C%5B%5B45816264%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398837%5D%2C%5B95398838%2C%5B%5Bnull%2C960449109%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95401453%5D%2C%5B95401454%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95402052%5D%2C%5B95402053%2C%5B%5B957164676%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=1396286277672550&amp;pvsid=8198940933942835&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F92265%2Fgoogle-gemini-3-8-live-extended-thinking-launch%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="168.75"></iframe>

play\_arrow

volume\_mute

AD

即時熱門文章

[1 賣螺絲也能做到22.9億元！振宇五金從1家開到98家，靠什麼讓70萬人買單？](https://www.bnext.com.tw/article/92219/ald-hardware-100-stores-growth-strategy) [2 光鼎股價一年飆400%！從LED跨足功率半導體，昔日血淚教訓成轉型關鍵：最快不一定最好](https://www.bnext.com.tw/article/92252/para-6226-led-power-semiconductor) [3 跟AI的對話被看光光？一次搞懂ChatGPT、Claude的9大隱私防線，4步驟保護機密不外洩](https://www.bnext.com.tw/article/92235/chatgpt-claude-privacy-9-stops-opt-out-training) [4 曾連虧10年、公司剩20多人⋯漢測股價如今為何一年狂翻50倍、登上興櫃股王？](https://www.bnext.com.tw/article/92225/han-test-7856-probe-card-cpo) [5 重塑世代的起點！BMW Neue Klasse 以傳奇之名開創未來移動新紀元](https://www.bnext.com.tw/article/91885/bmw115287) [6 Fed升息箭在弦上！800萬房貸一年恐多繳1.2萬？7大族群誰最痛、誰反而受惠](https://www.bnext.com.tw/article/92221/fed-rate-hike-mortgage-deposit-stock-guide)

![不只給技術，更幫忙找市場！中華電信5G加速器如何助零邁移動拓展低碳移動新商機？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/a8d1-1788334687.jpg?w=900&output=webp)

2026.09.14 | [5G通訊](https://www.bnext.com.tw/categories/5g)

不只給技術，更幫忙找市場！中華電信5G加速器如何助零邁移動拓展低碳移動新商機？

在商務與旅遊接送市場中，走出一條差異化道路的零邁移動，借力中華電信 5G 加速器提升服務品質，帶動 C 端業務營收成長 40%。

---

分享

企業減碳的下一步，正開始瞄準員工每天「怎麼移動」。

隨著全球淨零浪潮持續推進，企業的減碳計畫已經從辦公室與廠房節能、建置或採購綠電，逐步延伸至供應鏈與員工通勤。瞄準這波「低碳移動」新商機，零邁移動（ZEMO）以自組純電動車隊切入商務接送市場，並借力中華電信的技術、通路與資源，強化服務能力，將既有服務推向更多應用場景，為下一階段的成長找到更多機會。

### 看見低碳移動商機，零邁用演算法解開電動車隊營運難題

零邁移動創辦人呂學儒認為，電動車是低碳運具，非常適合作為營業用車，但市場上卻鮮少見到由電動車組成的商用車隊，背後的問題不在車輛本身，而是缺少一套適合電動車的車隊管理和營運機制。

畢竟，營業用電動車不像私人用車，可以自行安排用車方式，必須事先做好排程規劃，從客戶需求、接送時間、行駛路線、車輛剩餘電量、充電站位置與充電時間，每一項因素都要考量在內，才能做出最佳安排，確保車隊的營運效率與服務品質。

「所以我們自行研發管理系統，運用演算法與 AI 進行車輛調度與行程規劃，並採取全預約制，減少空車等候時間，」呂學儒說，除了建立精準調度機制，解決電動車商用化的營運難題，零邁移動也進一步思考，如何把「低碳」從車輛本身的特色，轉化為企業看得見、能運用的價值。

因此，零邁移動將每趟行程所使用的電能轉為碳排數據，提供企業作為溫室氣體盤查與永續報告的依據；同時積極提高自身的綠電使用比例，在電動車原有的低碳優勢上，進一步放大企業每一趟用車所能創造的減碳效益。

### 從連線、車隊管理到溝通，中華電信助零邁強化營運底座

靠著純電車隊與演算法，零邁移動建立起一套企業低碳移動的服務模式，並獲得華碩、微星、宏碁、友達光電、緯創、元太等 30 多家上市櫃企業與機構的青睞。

然而，這只是立足市場第一步，隨著企業客戶與服務趟次持續增加，零邁移動下一階段不只要接觸更多客戶、拓展更多應用場景，更要讓背後的營運能力跟得上市場成長速度，2025年加入中華電信5G加速器，正好為零邁移動補上這塊關鍵拚圖。

雙方的合作，先從強化零邁移動既有服務量能開始，透過中華電信三項解決方案，為團隊日後擴大營運規模打下基礎。

其中，保持車輛的穩定連線，是車隊管理系統的運作基礎。目前零邁移動全車隊與充電站皆採用中華電信物聯網 5G 聯網方案，確保車輛在移動過程中可以穩定回傳地理位置與行車資訊。

![零邁移動創辦人呂學儒](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-08/xmjk-1788141603.jpg?w=2000&output=webp)

零邁移動創辦人呂學儒認為，中華電信不僅連線速度與穩定度高，網路覆蓋範圍也相對完整，能降低車輛在移動過程中遇到訊號死角的機率，這對車隊管理來說尤為重要。

圖／ 數位時代

呂學儒表示，中華電信不僅連線速度快與穩定度高，網路覆蓋範圍也相對完整，能降低車輛在移動過程中遇到訊號死角的機率，「對我們來說，車子能不能隨時保持連線，是非常關鍵的一件事。」他說明，唯有連線不中斷，車輛才能即時回傳訊息，讓後端管理系統隨時掌握最新車況，一旦遇到突發狀況，也能即時因應與調度，避免影響後續行程與乘客服務。

在穩定連線之上，中華電信進一步協助零邁移動提升車隊管理的精細度。透過中華電信車訊平台和車聯網設備，零邁移動不只能即時掌握車輛位置、車況與行車影像，也能蒐集更深層的電動車數據與駕駛行為，並持續回饋至演算法，讓團隊可以做出更好的規劃與管理。

換句話說，這些數據的價值不只在於掌握車輛當下的運行狀態，更能從長期累積的數據中找出最佳使用方式，進一步延長車輛與電池壽命、降低長期營運成本。「這也是為什麼一般營業車跑到 30 萬公里，電池可能就頂不住了，我們可以撐到 50 萬公里的原因之一，」呂學儒分享。

除了強化車隊管理能力，零邁移動也藉由與中華電信的合作進一步優化服務流程，希望降低重複性工作，讓有限的人力發揮更大價值。零邁移動在自家雲端派遣系統上，整合中華電信 TAS 電話語音自動外撥與智慧語音服務，自動發送提醒或通知訊息給乘客，還能理解與判讀乘客需求，並進行初步分類與處理，再將需要真人處理的問題交由客服接手，讓客服人力從「重複撥電話」轉向「處理複雜或棘手的服務需求」。

### 從 B2B 走向 B2B2C，中華電信助零邁加速打開 C 端新市場

除了從技術端強化零邁移動的服務能力，5G 加速器也從市場端協助團隊接觸更多客戶、拓展新的應用場景。

「最大的改變是，市場對 ZEMO 的認知不一樣了，這讓團隊有機會觸及過去難以合作的企業客戶，推動商業模式從 B2B 走向 B2B2C。」呂學儒分享，過去外界多半將零邁移動定位為企業接送服務，但在 5G 加速器促成下，零邁移動與中華電信福委會合作，提供集團員工自費乘車服務。

這項合作也為購物中心、百貨公司等面向消費市場的企業帶來新的啟發—原來ZEMO也能成為加值服務的一環，為顧客創造差異化的消費體驗。「以往跟他們談合作，對方往往找不到合作切入點，現在完全不同了，」他說。

當服務對象從少數企業客戶走向大量個人使用者，對客服能量、車隊監控與營運穩定度的要求也隨之提高。前述導入的中華電信解決方案，正好補強了這項營運能力，讓團隊更有底氣將服務推向一般大眾，進一步帶動消費端業務快速成長，其中又以品牌官網 D2C的營收表現最為顯著。

呂學儒說明，過去零邁移動的消費端營收，主要來自與第三方業者的合作，顧客比較少直接透過品牌官網下單，加入中華電信 5G 加速器後，推升品牌信任與市場能見度，也讓更多消費者認識零邁移動。反映在營收結構上，透過品牌官網直接銷售給一般大眾的 D2C 營收占比，從2025年的0% 成長到 2026 年的30%。

展望未來，零邁移動希望藉由與 5G 加速器的合作，將應用場景延伸至偏鄉長者共乘、長照接送，或是與自駕車串接，讓「移動」創造更多可能。對零邁而言，5G加速器帶來的不只是技術與客戶，更讓一家具備市場基礎的新創，有機會把既有能力帶進更大的商業市場，找到下一階段的成長機會。

![呂學儒透露，加入中華電信 5G 加速後，帶動 C 端業務快速成長，如今來自 C 端的營收已逼近 40](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/f1eg-1788334645.jpg?w=2000&output=webp)

呂學儒透露，加入中華電信 5G 加速後，帶動 C 端業務快速成長，如今來自 C 端的營收已逼近 40%。

圖／ 中華電信

即時熱門文章

[1 賣螺絲也能做到22.9億元！振宇五金從1家開到98家，靠什麼讓70萬人買單？](https://www.bnext.com.tw/article/92219/ald-hardware-100-stores-growth-strategy) [2 光鼎股價一年飆400%！從LED跨足功率半導體，昔日血淚教訓成轉型關鍵：最快不一定最好](https://www.bnext.com.tw/article/92252/para-6226-led-power-semiconductor) [3 跟AI的對話被看光光？一次搞懂ChatGPT、Claude的9大隱私防線，4步驟保護機密不外洩](https://www.bnext.com.tw/article/92235/chatgpt-claude-privacy-9-stops-opt-out-training) [4 曾連虧10年、公司剩20多人⋯漢測股價如今為何一年狂翻50倍、登上興櫃股王？](https://www.bnext.com.tw/article/92225/han-test-7856-probe-card-cpo) [5 重塑世代的起點！BMW Neue Klasse 以傳奇之名開創未來移動新紀元](https://www.bnext.com.tw/article/91885/bmw115287) [6 Fed升息箭在弦上！800萬房貸一年恐多繳1.2萬？7大族群誰最痛、誰反而受惠](https://www.bnext.com.tw/article/92221/fed-rate-hike-mortgage-deposit-stock-guide)

[![台達電全解讀](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/e5i9-1785123864.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130221)