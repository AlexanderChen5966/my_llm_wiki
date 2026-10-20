---
title: "Lottie 動畫不用自己做了，讓 AI 從動畫設計開始，到將想法實作成 Lottie JSON"
source: "https://muki.tw/lottie-skill/"
author:
  - "[[MUKI space*]]"
published: 2026-06-26
created: 2026-08-14
description: "最近這兩天，我在 Threads 上連續分享了兩篇關於用 A..."
tags:
  - "clippings"
status: "processed"
---
Lottie 動畫不用自己做了，讓 AI 從動畫設計開始，到將想法實作成 Lottie JSON.md

最近這兩天，我在 Threads 上連續分享了兩篇關於用 AI 做 Lottie 動畫的實驗，沒想到反應比我預期的還要熱烈。很多人對這兩個 Lottie Skill 很有興趣，也有人私訊問我它們之間到底怎麼搭配、能做到什麼程度。於是我乾脆把這兩篇內容整理融合，寫成一篇比較完整的心得文章

這篇文會分成三個部分：

1. 先介紹這兩個 Skill 在整個工作流程裡各自扮演的角色
2. 分享我實際用它們做出的兩個案例：SVG 小動畫 & 五子棋對弈
3. 最後聊聊我理想中的工作情境：用 **粗淺的對話** 驅動 **複雜完善的 Skill**

---

## 用 AI 做 Lottie 動畫的兩個關鍵 Skill

這次的主角有兩個：

它們不是互相取代，而是很剛好地互補，一個想動態與節奏，一個負責產出真的可以用的 Lottie JSON

### motion-design-skill：像一本動態設計的教科書

我自己把 motion-design-skill 想像成是一本動態設計的教科書，它不會直接幫你產出動畫檔案，而是先把一些重要的動態設計原則，餵給 AI Agent 理解，包括：

- 時間的拿捏（快、慢、停頓）
- 節奏的安排（循環、漸進、強弱對比）
- 編排（畫面中元素怎麼出場、互動）
- 情緒張力（緊張、輕鬆、期待、高潮）

簡單說，它的目的不是輸出作品，而是讓 AI 在寫動畫之前，先做出更合理、更有品味的動態決策

所以如果你把他當成 After Effects 那樣的輸出工具，一定會失望；但如果你把它當成一個會動態設計的導演，請他先幫你想腳本、節奏、分鏡，那就有意思多啦～

### text-to-lottie：把想法變成可用的 Lottie JSON

相反地，text-to-lottie 這個 Skill 就是可落地的工具，它的重點是：

- 接受文字、SVG 等素材描述
- 幫你生成標準的 Lottie JSON
- 內建播放器可以即時預覽與微調
- 類似我們在 Lottie 官網編輯器裡做的那些事

因為輸出的是標準 Lottie JSON，所以你可以很放心地跨平台使用，包含 Web、React Native、iOS、Android、Flutter … 幾乎所有主流前端與 App 場景都能直接吃

換句話說：

- motion-design-skill 幫你想得更專業
- text-to-lottie 幫你做出成品

---

AD

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="722" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-6816776861246904&amp;output=html&amp;h=280&amp;slotname=2619535264&amp;adk=536108718&amp;adf=3680071312&amp;pi=t.ma~as.2619535264&amp;abgtt=7&amp;w=722&amp;fwrn=4&amp;fwrnh=0&amp;lmt=1786685364&amp;rafmt=1&amp;armr=3&amp;format=722x280&amp;url=https%3A%2F%2Fmuki.tw%2Flottie-skill%2F&amp;host=ca-host-pub-2644536267352236&amp;fwr=0&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786685364825&amp;bpp=1&amp;bdt=1579&amp;idt=-M&amp;shv=r20260812&amp;mjsv=m202608100101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dabed5c4f6c137a51%3AT%3D1786685192%3ART%3D1786685192%3AS%3DALNI_MYfJJNV1SVrdt4l6IPLqwFH3KHkcg&amp;gpic=UID%3D000014df72091a95%3AT%3D1786685192%3ART%3D1786685192%3AS%3DALNI_MYFojP7AHvvhSEstSBqIy9HMPHDYQ&amp;eo_id_str=ID%3D61820914debd3376%3AT%3D1786685192%3ART%3D1786685192%3AS%3DAA-AfjbBDTvN3lpfNfXahxBDXTZo&amp;prev_fmts=0x0&amp;nras=1&amp;correlator=293393125179&amp;frm=20&amp;pv=1&amp;rplot=4&amp;u_tz=480&amp;u_his=12&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=322&amp;ady=2410&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=359&amp;eid=122880127%2C122880367&amp;oid=2&amp;pvsid=6351557821694859&amp;tmod=1284125063&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fmuki.tw%2Fjournal%2F&amp;fc=1920&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=M%7C%7CeEbr%7Cp&amp;abl=XS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;plas=216x816_l%7C241x816_r&amp;bz=1.15&amp;pgls=CAEaBTcuMC4x~CAEQBBoHMS4xODIuMA..&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=3" title="Advertisement" aria-label="Advertisement"></iframe>

## 實驗一：用 text-to-lottie 玩 SVG 小動畫

在第一篇分享裡，其實我只用了 diffusionstudio/lottie 這個 Skill。

做法大概是這樣：

1. 從 heroicons 抓了一些 SVG 圖示
2. 再用 AI 生出一些簡單的 SVG 素材
3. 把這些 SVG 搭配文字說明丟給 text-to-lottie
4. 請它幫我產生對應的 Lottie JSON 動畫

在這個流程裡，我主要是在測試幾件事：

- text-to-lottie 處理 SVG 的穩定度
- 產生出來的 Lottie JSON 是否乾淨、可維護
- 用自然語言描述動畫邏輯，效果到底如何

成品如下：

<video height="2160" width="3838" controls="" src="https://muki.tw/wordpress/wp-content/uploads/2026/06/svg.mp4"></video>

這一篇比較像是技術驗證，我證明了只要用文字 + SVG，再加上一個合適的 Skill，就能不費吹灰之力做出還不錯的 Lottie 動畫，這跟以前我一個人打開編輯器慢慢拉 keyframe 的年代相比，真的是感嘆時代不同了 ><

---

## 實驗二：用 motion-design-skill 規劃一場有情緒張力的五子棋對弈

第二篇是更有挑戰性的一個實驗，這次我想做的不是單純的 icon 動畫，而是一個 **有情緒、有故事感** 的博弈動畫：一個俯視視角的五子棋棋盤，上下兩邊各有一雙手，一來一往地下棋，夾雜思考與對峙，最後分出勝負

我給 AI 的指令大概是這樣：

> 用 /motion-design 幫我規劃一個俯視的棋盤，然後上下兩邊各有兩隻手在玩五子棋，一來一往的，從放下第一個棋子開始到遊戲分出勝負，來回大概 8 秒的時間，但不要一下就分出勝負，來回還是要有一點難度

這裡可以看到，我給的是一個很人類、很口語的描述

- 我沒有管精準的座標、關鍵幀時間點
- 我講的是你來我往、有一點難度、不要太快結束... 這樣自然的語言
- 我強調的是棋局的節奏和觀眾的感受

motion-design-skill 在這裡就扮演導演的角色，他會幫我思考哪些地方節奏可以快一點？哪些地方需要慢下來？什麼時候畫面需要有一點停頓，讓勝負的情緒拉滿？

接著再把這樣的動態規劃，轉交給 text-to-lottie 去實際實作成 Lottie JSON

最後的成果，就是一個大約 8 秒鐘左右、節奏有起伏的五子棋對弈動畫，請參考下方影片。對我來說，這已經超出單純生成動畫的範疇，更接近用文字指導一個懂動畫的團隊幫我做影片

<video height="1080" width="1080" controls="" src="https://muki.tw/wordpress/wp-content/uploads/2026/06/gomoku.mp4"></video>

---

## 我的理想情境：粗淺對話搭配複雜 Skill

一直以來，我偏好的都是用粗淺的日常對話，搭配複雜完善的 Skill，產生出足夠專業、足夠實用的成果

這是我喜歡的情境，因為我覺得不是每個人都懂得 After Effects、Figma Motion、或各式各樣的動畫插件；但很多人對自己心中的畫面有很清楚的感受與想像 （這就是甲方跟乙方嗎 XXD）

現在有了 AI，模糊的職能邊界，所以如果能把專業的動態設計知識和實際的工程產出，透過 SKILL 處理，使用者就能只專心在說出自己要什麼，而不是被工具牽著走：Ｄ

---

## 兩個 Skill 的分工總結

最後，用一句話幫這兩個 Skill 定位：

- motion-design-skill：幫我想動態的概念與動畫設計方向
- text-to-lottie：幫我把這些想法做成可用的 Lottie JSON

前者是教科書＋導演，後者是工程師＋剪接師，把這兩個串起來，就形成了一條從一句話的想法，到可以上線使用的動畫資產的工作流

以上就是我這兩天實驗這兩個 Lottie Skill 的整理與心得，希望這篇融合版的分享，能讓你更清楚理解它們各自的定位，以及怎麼搭配使用，或許也能給你一些靈感，開始試著用一句句對話，做出屬於你自己的動畫世界

muki@journal ~/AI 應用 $ 0