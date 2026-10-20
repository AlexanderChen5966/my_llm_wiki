---
title: "AI 動畫怎麼做？拆解 Claude Opus 5.5 首週爆紅的像素巫師、沙畫與 3D 城堡"
source: "https://rar.design/posts/claude-opus-5-5-animation-guide"
author:
  - "[[設計師 Riven]]"
published: 2026-09-29
created: 2026-09-30
description: "Claude Opus 5.5 做動畫靠程式碼把每一格畫面算出來，不需要圖檔或動畫軟體。首週最常見四種做法：單檔 HTML 動畫、程式碼轉 MP4、操控 Blender、物理模擬。本文拆解十幾支代表作、prompt 寫法，以及該用 Claude Code 還是 Claude Design。"
tags:
  - "clippings"
status: processed
---
四種做法、十支作品範例，該用 Claude Code 還是 Claude Design，和讓成品拉開差距的三個習慣

![設計師 Riven](https://s.teachifycdn.com/image/width=128,quality=80,format=webp/lecturer/avatar/fd0b78a9-03c9-4ad3-ae0a-a8965aa676ea/d822e5e9-519b-4f2e-b049-807272104edd.png)

[設計師 Riven](https://rar.design/@riven)

2026年9月29日 下午 8:52

AI 設計

AI MOTION · 2026

![Anthropic 官方影片〈Claude Opus 5.5 turns graphite into gravity〉畫面：鉛筆素描的投石機被做成 3D 木製模型，擊倒 10 塊積木中的 7 塊](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/44351905-253e-41bd-b7b7-7f3e411ac983/72038d0c-c9f0-4ce6-a7ef-d6767d9998ca.jpg)

Anthropic 官方影片〈Claude Opus 5.5 turns graphite into gravity〉畫面：鉛筆素描的投石機被做成 3D 木製模型，擊倒 10 塊積木中的 7 塊

▲ Anthropic 官方示範〈turns graphite into gravity〉：Opus 5.5 把一張鉛筆畫的投石機立成 3D 模型，拋射與倒塌全部交給物理模擬（圖／Anthropic）

QUICK ANSWER

Claude Opus 5.5 做動畫靠程式碼把每一格畫面算出來，不用圖檔。首週最常見四種做法：單檔 HTML 動畫、程式碼轉 MP4、操控 Blender、物理模擬，有講明介面的多半在 Claude Code 完成。成品好壞看三件事：讓它看自己的渲染、先寫分鏡再分工、每一格只由時間決定。

CONTENTS

01

[發表當晚，大家先拿它做動畫](#chapter-1)

02

[四種做法：從單檔 HTML 到 Blender](#chapter-2)

03

[用 Claude 網頁版、Claude Code 還是 Claude Design？](#chapter-3)

04

[首週值得看的十支作品](#chapter-4)

05

[prompt 要寫多細？三種寫法都做得出來](#chapter-5)

06

[成品拉開差距的三個工作習慣](#chapter-6)

07

[成本、速度與還做不到的事](#chapter-7)

08

[設計師可以怎麼開始](#chapter-8)

CHAPTER 01 · WHAT HAPPENED

## 發表當晚，大家先拿它做動畫

![Anthropic 官方影片〈Introducing Claude Opus 5.5〉畫面：黑色背景上的 Opus 5.5 字樣與琥珀色弧形表面](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/6576ee3d-9a04-46e5-9c6e-4c487b16b4e3/a51bf40a-022a-4dd4-998e-51430485981a.jpg)

Anthropic 官方影片〈Introducing Claude Opus 5.5〉畫面：黑色背景上的 Opus 5.5 字樣與琥珀色弧形表面

▲ 官方發表影片〈Introducing Claude Opus 5.5〉的畫面（圖／Anthropic）

Claude Opus 5.5 在台灣時間 9 月 23 日凌晨發表。官方公告出來幾分鐘後，X 上就有人貼出一段 32 秒的手繪風動畫：紙張質感的背景上，一隻橘色方塊小角色跑過畫面，接著是稜鏡分光、向日葵的螺旋和整片星系。這支片的觀看次數已經超過 170 萬，整支沒有用到任何一張圖檔，每一格都是程式碼畫出來的。

接下來三天，設計圈和開發圈都在傳同一類作品：施法的像素巫師、上萬片磁磚拼成的馬賽克、全鎮的人都寄請求給 Claude 的剪紙風短片、兩分鐘的沙畫美國史、Blender 裡的湖畔城堡。Anthropic 的發表文也提到，有測試者讓幾個 Claude 模型用同一段 prompt 做遊戲，Opus 5.5 在畫面和完成度上拿到最高分。

我把發表後這幾天公開了做法或 prompt 的 Opus 5.5 動畫翻了一輪，挑出十幾支來拆。這篇整理四件事：大家走了哪幾條技術路線、prompt 要寫多細、哪些工作習慣讓成品拉開差距，以及成本和限制。模型本身的規格和價格，我在〈 [Claude Opus 5.5 為什麼更強還更便宜？](https://rar.design/posts/claude-opus-5-5-guide) 〉整理過，這篇只談動畫。

170 萬+

VIEWS

手繪風短片的觀看次數

13,081

TILES

馬賽克動畫的磁磚片數

35 分鐘

MINUTES

Blender 城堡從 prompt 到渲染

CHAPTER 02 · FOUR ROUTES

## 四種做法：從單檔 HTML 到 Blender

把這批作品攤開來看，技術上可以分成四條路。分法很簡單，看成品最後交出去的形式：一個網頁、一支影片、一個 3D 專案，或一套物理規則。

ROUTE 01 · SINGLE HTML

單檔 HTML 即時動畫

成品｜一個.html，瀏覽器打開就會動

代表｜像素巫師、13,081 片馬賽克

適合｜網頁動效、互動展示、小遊戲

ROUTE 02 · CODE TO MP4

程式碼轉影片

成品｜逐格渲染後合成的 MP4

代表｜〈small print〉、沙畫美國史、P(doom) MV

適合｜社群短片、說明動畫、MV

ROUTE 03 · 3D SOFTWARE

操控 Blender

成品｜Blender 專案檔加渲染鏡頭

代表｜湖畔城堡

適合｜需要 3D 光影與材質的鏡頭

ROUTE 04 · SIMULATION

物理模擬

成品｜可重播、可調參數的模擬場景

代表｜素描投石機

適合｜產品示意、教學、互動實驗

### 一｜單檔 HTML：瀏覽器打開就會動

這是最多人走的路。成品只有一個 HTML 檔，畫面用 Canvas 2D、SVG 或 WebGL 即時算出來。遊戲設計師 Majid Manzarpour 的像素巫師就是這一類：128×96 的畫布、固定約 24 色的色盤，巫師待機、集氣、施法、收招，一輪一輪循環。他說整個過程大約 30 分鐘，額度幾乎沒動到。

Anthropic 員工 Dan Fein 的馬賽克更誇張，13,081 片磁磚全部用程式碼畫、用程式碼動，星星在水面的倒影最後變成 22 條小魚。這條路的好處是成品本身就是網頁，可以直接放進作品集，也能加上滑鼠互動；要交影片的話，還得另外錄。

### 二｜程式碼轉 MP4：一格一格渲染成影片

要交影片的人走這條。做法是先把動畫寫成網頁，再用無頭瀏覽器一格一格截圖，交給 FFmpeg 合成 MP4。AI 實測創作者 Vox 的〈small print〉用的是 AI 影片公司 HeyGen 開源的 HyperFrames，整支片寫在一個 index.html 裡。故事是 Claude 每天收到一堆請求，它把每則請求裡屬於人的那一小段圈起來，例如「一隻手，寶寶睡了」，丟進玻璃罐；到了晚上，那些字變成星星，連成星座。配樂是 Opus 5.5 用 Python 合成的。

AI agent 開發者 Michael Guo 的沙畫美國史則是一段 prompt 到底：兩分鐘的片長裡，有 1776 年的自由鐘、1869 年的鐵路和 1969 年的登月，配樂和音效也是模型自己做的。他沒用 Blender，也沒用 Three.js，整支片只用掉 Pro 方案五小時額度的三成。

用影片模型做過 motion graphics 的人，會發現這條路的手感很不一樣：改一個顏色、拉長兩秒，只要改一行程式重新渲染，不必整支重抽。我在〈 [Seedance 2.5 能做 motion graphics 嗎？](https://rar.design/posts/seedance-2-5-motion-graphics-prompt-tutorial) 〉踩過的坑，像品牌名讓畫面變寫實、重複的詞讓結尾冒出人群，都來自影片模型對文字的聯想，程式碼動畫不會遇到。

### 三｜操控 Blender：建模、打光、渲染都在 3D 軟體裡

3D AI 創作者 Stefan Vaskevich 讓 Opus 5.5 透過 MCP 連進 Blender，一段 prompt 要它全部用程序化建模做出湖畔城堡、水面倒影和山後的煙火，渲染一個 10 秒鏡頭，還要錄下自己的建模縮時。結果是 35 分鐘、19.96 萬個輸出 token，換算 API 價格約 13.3 美元。

同一題 GPT-6 Astra 用了 28 分鐘、約 14.5 美元。Stefan 自己的評語是配色 Opus 比較好，建築細節 Astra 多一點。AI 在 Blender 裡寫 Python 建模的基本功，可以參考〈 [不會 Blender 也能建 3D？](https://rar.design/posts/gpt-6-astra-3d-blender-guide) 〉。

### 四｜物理模擬：動作交給物理去算

最後一條是 Anthropic 官方自己示範的。影片裡，Claude 一個零件一個零件讀懂一張鉛筆畫的投石機，把畫上的積木當作 40 公釐寬來定比例，把整台機器立成紙上的木頭模型。放開之後，每一次拋射和倒塌都是物理模擬，沒有任何逐格擺位。第一次打太近，它把配重從 0.48 公斤加到 0.68 公斤，第二次擊倒 10 塊裡的 7 塊。

對做動態設計的人來說，這是關鍵影格之外的另一種思路：你設定重量、材質和力，動作由物理規則算出來。

![](https://www.youtube.com/watch?v=uMsZ21ubIMM)

▲ Anthropic 官方影片〈Claude Opus 5.5 turns graphite into gravity〉

CHAPTER 03 · WHICH CLAUDE

## 用 Claude 網頁版、Claude Code 還是 Claude Design？

這批作品底下最常見的留言，是問到底在哪裡做的。多數作者沒講；有講明的，Claude Code 佔大多數。P(doom) MV 的兩版都在 Claude Code 生成，科技新聞網站 BitsMinds 的測試也是在 Claude Code 開最高強度跑；沙畫美國史的 Michael Guo 則在回覆裡說 Claude Code 越來越耐用，整支片只花掉 Pro 方案五小時額度的三成，用的是訂閱，沒走 API。

原因很實際：這波動畫都得實際跑程式，一支片常常要跑一兩個小時。寫 Python 合成配樂、用 FFmpeg 把網頁逐格輸出成影片、渲染完截圖回頭檢查、平行開子代理分章節畫，這些在 Claude Code 裡最順。沙畫的配樂就是這樣做的：Opus 5.5 寫了一支 Python，把每個音符的起拍、長度、音高、力度一條條列出來，存成 MIDI，再用 FluidSynth 配上免費音色庫出聲。

CLAUDE.AI · 網頁版與 App

Claude 網頁版

適合｜單檔 HTML 小動畫、互動展示、想直接丟連結給人看

首週例子｜安提基特拉遊戲，直接發成 artifact

CLAUDE CODE · 公開作品最多

Claude Code

適合｜要輸出 MP4、要它看樣片自己修、要跑一兩個小時、要開子代理

首週例子｜P(doom) MV、沙畫美國史、BitsMinds 測試

CLAUDE DESIGN · 畫布

Claude Design

適合｜UI 動效、行銷用短動態、想在畫布上邊看邊調

首週例子｜熱門作品裡還沒出現

Claude 網頁版也做得出來。Anthropic 員工 Edwin Arbus 的安提基特拉遊戲，成品直接發成 claude.ai 的 artifact 連結，點開就能玩；九月中 Cowork 併進聊天之後，大一點的任務也能直接丟。不過像素巫師那則底下有人說，他用 Cowork 跑同一段 prompt，做出來沒那麼好，打算換 Claude Code 再試。

Claude Design 在首週的熱門作品裡一支都沒出現，它比較適合在畫布上邊看邊調的 UI 動效和行銷短動態，兩者的分工我在〈 [Claude Code /design 還是 Claude Design？](https://rar.design/posts/claude-code-design-command-guide) 〉整理過。走 Blender 那條路的話，還要另外接 MCP，Stefan 用的是 Blender 官方的 MCP。

CHAPTER 04 · SHOWCASE

## 首週值得看的十支作品

前面拆過的作品之外，這十支也值得點開看。每張卡片附上做法和看點，點下去會開原作。[SHORT FILM · 手繪風](https://x.com/devteamdrew/status/2102436464323661880)

[

手繪風短片

DreW｜創意程式創作者

32 秒、紙張質感的手繪線條：橘色方塊小角色、稜鏡分光、向日葵螺旋、整片星系。配樂讓 Opus 5.5 迭代了三、四次，要求它改用真實樂器的音色。

觀看超過 170 萬｜拿自己前一支影片當參考

看原作 ↗

](https://x.com/devteamdrew/status/2102436464323661880)[

SHORT FILM · 剪紙風

〈What do you love?〉

Kevin Ngo｜創意程式創作者

全鎮的人都寄請求給 Claude，只有一個女孩寄來一個問題：你喜歡什麼？畫面是純 JavaScript 畫在 canvas 上，聲音用瀏覽器的 Web Audio API 即時生成。

28 秒｜觀看超過 60 萬

看原作 ↗

](https://x.com/kevin_t_ngo/status/2102437977435893771)[

SHORT FILM · 毛氈手作

〈A snail, wish by wish〉

Dan Fein｜Anthropic 員工

一隻小蝸牛許了四個願，穿過暴風雨回到牠的花。毛氈、線和珠子全用程式碼畫，縫針走了 223 針，每一針都有自己的音。

76 秒｜和 13,081 片馬賽克同一位作者

看原作 ↗

](https://x.com/dfeinition/status/2102441574164783149)[

BRAND MOTION · 風格拆解

把發表影片拆成三個 skill

Misbah Syed｜LiteLLM 開發者關係

先請 Opus 5.5 模仿官方發表影片，一次做出一支 9 秒的片，再把做法整理成三個開源 skill：拼貼地平線的發表短片、素描變物理模擬的影片、實驗桌上的 3D 公式解說。全部在無頭瀏覽器逐格渲染，不用任何生圖或生影片模型。

拿去做了 LiteLLM 支援 Opus 5.5 的公告片

看原作 ↗

](https://github.com/misbahsy/claude-horizon-animation)[

VOXEL · 體素

Opus 5.5 的體素自畫像

bluedev｜自稱 OpenAI 頭號粉絲

讓 Opus 5.5 用體素把自己做出來，待機動畫和小細節都有。作者的評語是，它對 3D 的理解是目前最好的。

15 秒

看原作 ↗

](https://x.com/blueemi99/status/2102511304456212763)[

PIXEL ART · 對決

像素場景：Opus 5.5 對 GPT-6 Astra

Dev Ed｜程式教學 YouTuber

同一題交給兩個模型做像素場景，他點名 Opus 5.5 版噴泉的水流動畫和洞穴裡蝙蝠的眼睛，覺得 Astra 版太普通。留言區也有人比較喜歡 Astra。

兩個模型並排比較

看原作 ↗

](https://x.com/developedbyed/status/2102522318559858721)[

GAME · 即時生成

安提基特拉機械遊戲

Edwin Arbus｜Anthropic 員工

以史上第一台計算機為題的探索遊戲，畫面和聲音全在瀏覽器裡即時生成：2,047 條魚、25,000 片草葉、47,000 顆粒子，還有一座 30 個齒輪、1,923 齒的機械模型。

3MB 單一 HTML｜發成 claude.ai artifact

看原作 ↗

](https://x.com/edwinarbus/status/2102463453176979794)[

GAME · 光影

瀏覽器版 Minecraft〈Lumen Vale〉

Noah Wachnik｜影像與 AI 創業者

一句 prompt 要它在瀏覽器做出能玩的 Minecraft，還要進階光影。水面有波光，光線會隨一天的時間變化。

最高強度跑 1 小時 37 分｜約 15 美元 API

看原作 ↗

](https://x.com/noahwachnik/status/2102470200415166699)[

MOTION · 火箭發射

HALCYON-3 火箭發射

BitsMinds｜科技新聞網站

題目只說要好看，Opus 5.5 把倒數、點火、分節、整流罩分離一路做到進入 203×207 公里的軌道，關機時速度停在時速 28,024 公里，鏡頭還會切角度、推近拉遠。

76 分鐘｜全程不准看自己的成品

看原作 ↗

](https://www.bitsminds.com/news/claude-opus-5-5-vs-fable-5-1-vs-astra-6-build-off-2026)[

DATA VIZ · 官方示範

〈地出〉照片的 3D 重建

Anthropic｜官方示範

Claude 沿著 1968 年〈地出〉照片的月球地平線描了 2,781 個點，比對月球高程資料，推算出拍攝時刻是 1968 年 12 月 24 日 16:39:39（UTC），再用 3D 重建場景，和原照交替閃爍對照。

官方發表影片

看原作 ↗

](https://www.youtube.com/watch?v=Ov-B6K1EsaI)

這十支裡面，我最想推給做品牌的設計師的是 Misbah Syed 那支。他把一支影片的動態手法拆成可以重複使用的 skill，之後每支公告片都照同一套規則出。品牌如果有固定的動態語言，也能用同樣的方式包起來。

CHAPTER 05 · PROMPTS

## prompt 要寫多細？三種寫法都做得出來

看完這批作品，最意外的是 prompt 的長度落差：最短的只有三句，最長的像一份規格書，兩邊都做出了好東西。

### 短版：題材、長度、風格，其他交給它

Michael Guo 的沙畫 prompt 只寫了三件事：兩分鐘、美國 250 年的歷史、要生動有品味並配上合適的音樂和音效。分鏡、節奏、畫面選擇全部由模型決定。這種寫法適合大家有共同記憶的題材，歷史、節慶、品牌故事都是。你放掉控制權，換來它的判斷。

### 長版：畫面規格一條一條鎖死

Majid 的像素巫師 prompt 分成五段，每一段都在鎖死一件事：

PROMPT ANATOMY · 像素巫師的五段 prompt

RENDERING · 畫面規格

128×96 邏輯畫布，整數倍放大；禁止反鋸齒、漸層與模糊；全圖只用約 24 色。

CHARACTER · 角色

約 24×32 像素，尖帽、長鬍子、雙色長袍、寶石法杖；姿勢拆成法杖角度、舉手、歪頭、袍擺四個參數。

ANIMATION · 動作

待機、集氣、施法、收招的循環狀態機；關鍵姿勢之間用緩動；每格量化回像素格，讀起來像每秒 8 到 12 張。

SCENE · 場景

夜空、閃爍的星星、月亮、石地板；寶石對巫師打 1 像素的輪廓光。

QUALITY BAR · 驗收

任何視窗大小都清晰、無縫循環、穩定 60fps、剪影一眼讀得出來。

最關鍵的是動作段裡那一句：姿勢參數平滑地算，但每一格都量化回像素格，讓動作讀起來像每秒 8 到 12 張的點陣動畫，程式本身照樣跑 60fps。這就是傳統動畫一拍二、一拍三那種帶頓點的節奏，只是翻譯成程式聽得懂的話。會寫這句的人，懂的是動畫。

### 第三種：只給一條世界觀規則

玻璃馬賽克那段 prompt 走第三條路，出處是 Dan Fein。他分享時說這不是他的原版，大家可以在上面加自己的故事。範本裡沒有分鏡，只有一條物理設定：這面金箔玻璃馬賽克的磁磚從來沒黏牢，可以掀起、翻面、飛走再扣回原位；畫面裡的角色都是一群磁磚，魚會游，是因為牠的磁磚在游。

品牌創業家 Chris Riley 什麼故事都沒加，原封不動丟給 Opus 5.5，跑出一支 80 秒的片：魚群躍出牆面潛進海裡，鶴和鴿子飛出牆，整面牆翻成夜晚，星星剝落連成星座，最後全部螺旋散開再扣回原位。這些場景都是模型自己編的。Dan Fein 自己那支 13,081 片的版本，則是在類似的規則上加了故事，星星的倒影最後變成 22 條小魚。

三種寫法我會這樣選：要特定畫風，寫長版；要故事感，寫短版；想要一種沒見過的動態語言，就給它一條世界觀規則。

CHAPTER 06 · THE DIFFERENCE

## 成品拉開差距的三個工作習慣

同一個模型，有人做出能直接發的成品，有人做出一團糊。把幾支作品的製作過程攤開，差別集中在三個習慣。

### 習慣一：讓它看自己的樣片

動畫公司每天會看前一天的渲染樣片，所有人盯著畫面挑問題。Chris Riley 公開的 agent 回報裡，Opus 5.5 做的就是這件事：渲染完逐格看，再列出自己改了什麼。

金色的魚貼在金色牆上糊成一團，它改成銅橘色玻璃加藍色描邊；飛行中的魚群太鬆，它收緊隊形、減少抖動；移動的光把太陽和月亮洗白，它把反光收小。最後還老實列出兩個沒解決的問題：日夜翻轉那一秒天空偏白，夜晚的海太接近黑色。Vox 的〈small print〉也一樣，配樂合成完，它回頭一秒一秒檢查渲染結果，再修一輪。

反例是 BitsMinds 的測試。他們給三個模型同樣三道動畫題，規定不准開瀏覽器看自己的成品。Opus 5.5 以 10 分拿下總分，交流道那題卻輸在一條線：每條匝道的出入口都被白色路緣線直接畫過去，看起來像匝道從路面底下滑出來。這條線在程式碼裡完全合理，只有看圖才抓得到。

NO LOOK · 不看樣片

寫 brief

↓

長時間推理

↓

一次寫完整個檔案

↓

交件：視覺錯誤留在成品裡

DAILIES LOOP · 樣片迴圈

寫 brief

↓

渲染

↓

逐秒截圖檢查

↓

修正，回報改了什麼、還剩什麼

實務上，要在能跑程式、能截圖的環境裡做，例如 Claude Code。prompt 最後加一段：「渲染完每秒截一格檢查，列出你改了什麼、還剩哪些問題。」這份回報就是你的驗收清單。

### 習慣二：先寫分鏡，再分工

發表那週登上 X 趨勢的 P(doom) 音樂 MV，長 156.6 秒，除了歌曲本身，畫面、分鏡和程式全部由 Opus 5.5 產出。作者只給兩個方向：用 Claude Code 吉祥物 Clawd 的造型，每句歌詞都要有有趣的畫面和轉場。

這支片做了兩版，第二版開工前，Opus 5.5 先寫了兩份文件：一份分鏡表，把每句歌詞對上秒數、畫面內容和轉場；一份動畫指南，交代給它平行啟動的子代理。全片拆成九個章節，每個章節一個檔案，由不同的子代理同時畫，最後在無頭瀏覽器裡逐格算圖，再用 FFmpeg 合成影片。

![](https://www.youtube.com/watch?v=8j-hR4fJywU)

▲ P(doom) 音樂 MV，畫面由 Opus 5.5 用 p5.js 水彩筆刷逐格繪製（影片／OtherReality）

那份動畫指南值得做動態的人讀一遍。它規定角色換表情不准硬切，要先閉眼、擠壓伸展一下，再彈出新表情；線條每秒重新抖動 12 次，做出手繪動畫線條微微浮動的質感；每個鏡頭都要有事情發生；合唱段的主角要佔畫面高度四成。這些是動畫原理課的內容，現在寫成了給 AI 看的交接文件。

設計師最值得學的是順序：先要它交分鏡表和美術設定，你看過、改過，再讓它開工。文件改一句只要幾秒，渲染完再改要重跑幾十分鐘。

### 習慣三：每一格只由時間決定

P(doom) 的動畫指南還有一條技術規定：每個鏡頭都只能由時間決定，不准用隨機數，也不准讓前一格的狀態影響下一格。原因很實際，九個章節平行算圖，每一格的渲染順序是亂的，只有「給定秒數就一定畫出同一格」，拼起來才不會出錯。

這條規則到處都看得到。HyperFrames 把確定性渲染列為核心設計，同樣的輸入每次都輸出同樣的畫面；社群開發者替 Claude Design 動畫做的 MP4 匯出工具，也要求拿掉 CSS 關鍵影格、Math.random() 和讀取現在時間的寫法，全部改由時間驅動。

翻成設計師能直接貼進 brief 的一句話：「所有動態都由時間 t 決定，同一個 t 永遠畫出同一格；需要隨機感，就用固定種子的亂數。」有這句，之後要轉影片、要抽某一秒出來改，都不會卡住。

“

讓 AI 看得到自己的樣片，比多寫一千字 prompt 有用。

CHAPTER 07 · LIMITS

## 成本、速度與還做不到的事

公開案例

耗時

花費

Blender 湖畔城堡

35 分鐘

API 約 13.3 美元

兩分鐘沙畫美國史

未公開

Pro 五小時額度約三成

像素巫師

約 30 分鐘

額度幾乎沒動

BitsMinds 三道題（最高強度）

每題 59 到 77 分鐘

未公開

最高推理強度下，它很慢。BitsMinds 的三道題 Opus 5.5 分別花了 59、63、77 分鐘，是三個模型裡最久的，時間幾乎都花在動筆前的思考：交流道那題它想了 57 分鐘，寫檔只用了一分半。AI 評測機構 Artificial Analysis 的統計也顯示，最高強度下它平均每題要用掉約 11.9 萬個輸出 token。建議從中等強度開始試，Chris Riley 的玻璃馬賽克用的就是中等。

評估的方法也要換。BitsMinds 的遊樂園題有個細節很有意思：GPT-6 Astra 的旋轉木馬畫得最精緻，鞍墊和鬃毛都有，但整座木馬沒有在轉，只是左右晃 3 度；Opus 5.5 的木馬畫得沒那麼細，卻是透視正確、馬匹跟著屋頂一起轉的一台機器。動畫要看動起來對不對，截一張圖比畫工，很容易看走眼。

另外三個限制要先知道。第一，這批作品都是作者滿意才貼出來的，失敗的版本沒人貼。第二，它擅長風格化的 2D、像素、幾何、程式化 3D 和物理模擬，要寫實的人物表演與實拍質感，影片生成模型還是比較直接。第三，它合成得出配樂和音效，要有人聲的歌還是得另外準備，P(doom) 那支用的就是現成的歌。

CHAPTER 08 · GETTING STARTED

## 設計師可以怎麼開始

如果你現在就想試，我會照這個順序：

1

先決定成品形式

要互動就做單檔 HTML；要發社群就走轉 MP4；要 3D 光影才接 Blender。

2

用五段寫 brief

畫面規格、風格與色票、角色與動作、時間規則、驗收標準。描述動作時用你熟的動畫詞彙：緩動、預備動作、擠壓伸展、一拍二。

3

先要分鏡表和美術設定

看過、改過再開工，改文件比改渲染便宜。

4

讓它看樣片、交回報

在能渲染和截圖的環境跑，要求它逐秒檢查，列出改了什麼、還剩什麼。

5

最後才調推理強度

從中等開始；畫面邏輯複雜、又沒辦法讓它看畫面時，再往上調。

把上面五步濃縮成一份可以直接改的 brief，方括號換成你的內容就能用：

BRIEF TEMPLATE · 可以直接改的動畫 brief

做一支［秒數］秒、［比例］的動畫，輸出成單一 HTML 檔，只用 Canvas 2D 和原生 JavaScript，不用任何圖檔、字型或音檔。

畫面：［畫風，例如手繪線條加紙張質感］，全片只用［色票］這幾個顏色。

角色與動作：［角色描述］。動作分成［待機、預備、主動作、收尾］幾個狀態，狀態之間用緩動，換表情不要硬切。

時間規則：所有動態都由時間 t 決定，同一個 t 永遠畫出同一格；需要隨機感，就用固定種子的亂數。

驗收：渲染完每秒截一格檢查，列出你改了什麼、還剩哪些問題，確認沒問題再交件。

要輸出影片，就在 Claude Code 裡跑，最後加一句「用 HyperFrames 渲染成 MP4」；第一次用之前，先照 HyperFrames 官方說明把它的 skills 裝好。

要替動畫配旁白，可以參考我用 Claude Code 接 Fish Audio 做聲音分身的做法，完整步驟在〈 [AI 聲音分身怎麼做？](https://rar.design/posts/ai-voice-clone-claude-code-fish-audio) 〉。

把這十幾支作品看完，我最大的感想是動畫原理沒有過時，只是換了用法。以前你要自己一格一格畫出預備動作和緩動，現在你要把它們寫清楚，交代給一個會寫程式的動畫組，再坐進看片室挑問題。這一波最佔便宜的，是本來就懂動畫的設計師。

TAKEAWAYS · 一分鐘懶人包

→

Opus 5.5 的動畫全是程式碼，成品有四種形式：單檔 HTML、MP4、Blender 專案、物理模擬。

→

prompt 可以短到三句，也可以長到一份規格書；要特定畫風寫長版，要故事感寫短版。

→

讓它渲染後逐格檢查並交回報，比在 prompt 裡多寫規則有效。

→

長片先要分鏡表和動畫指南，確認後再讓它分工開工。

→

brief 加一句「每一格只由時間決定」，之後轉影片、抽秒改稿都順。

→

評估 AI 動畫要看動起來對不對，只比截圖會看走眼。

RELATED READS · 延伸閱讀[Claude Opus 5.5 為什麼更強還更便宜？](https://rar.design/posts/claude-opus-5-5-guide)

[

模型本身的跑分、價格與寫作升級

→](https://rar.design/posts/claude-opus-5-5-guide)[

Seedance 2.5 能做 motion graphics 嗎？

用影片模型做動態的另一條路，附十一版修稿實錄

→](https://rar.design/posts/seedance-2-5-motion-graphics-prompt-tutorial)[

用 AI 做 3D 遊戲要從哪開始？

Three.js 機車競速遊戲，從美術規格做到可玩版本

→](https://rar.design/posts/chatgpt-codex-3d-racing-game-tutorial)[

不會 Blender 也能建 3D？

AI 寫 Python 在 Blender 裡從零建模的進場路線

→](https://rar.design/posts/gpt-6-astra-3d-blender-guide)[

灰模直接變實拍？Higgsfield 進駐 Blender

Blender 外掛、MCP Bridge 與 previz 工作流

→](https://rar.design/posts/higgsfield-blender-plugin-guide)[

Claude Code /design 還是 Claude Design？

終端機和畫布兩種做法怎麼分工

→](https://rar.design/posts/claude-code-design-command-guide)[

AI 聲音分身怎麼做？

替動畫配旁白：10 秒錄音複製自己的聲音

→](https://rar.design/posts/ai-voice-clone-claude-code-fish-audio)

EXTRA · LINKS 延伸資源[Introducing Claude Opus 5.5](https://www.anthropic.com/claude-opus-5-5)

[

Anthropic 官方發表文

↗](https://www.anthropic.com/claude-opus-5-5)[

HyperFrames

HeyGen 開源的 HTML 轉影片框架（GitHub）

↗](https://github.com/heygen-com/hyperframes)[

P(doom) MV 原始碼

分鏡表、動畫指南與算圖腳本（GitHub）

↗](https://github.com/JohnHeibel/PDoomVideo)[

BitsMinds 三模型動畫測試

交流道、遊樂園、火箭發射三道題的完整成品

↗](https://www.bitsminds.com/news/claude-opus-5-5-vs-fable-5-1-vs-astra-6-build-off-2026)[

像素巫師與完整 prompt

Majid Manzarpour（X）

↗](https://x.com/majidmanzarpour/status/2102476258948927543)[

玻璃馬賽克短片與 agent 回報

Chris Riley（X）

↗](https://x.com/LCSlates/status/2102503027340988559)![AI 覺醒設計應用攻略](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/63e96ac9-97ce-4dfc-be50-154f731f1cc9/e5ca45f0-206a-478f-af1a-96960be47d6a.jpg)

AI 覺醒設計應用攻略

[

SUBSCRIPTION

AI 覺醒設計應用攻略

AI 工具每週都在更新，我幫你過濾雜訊，只留對設計師真正有用的應用。

了解訂閱 →](https://rar.design/courses/aiwaken)

FAQ · 常見問題

## 常見問題

### Q：Claude Opus 5.5 可以直接輸出影片檔嗎？

A：不行，它輸出的是程式碼。要 MP4，常見做法是把動畫寫成網頁，再用 HyperFrames 這類工具在無頭瀏覽器裡逐格渲染，用 FFmpeg 合成影片。

### Q：不會寫程式，也能用 Opus 5.5 做動畫嗎？

A：可以。首週爆紅的作品，作者多半一行程式都沒寫。關鍵是把畫面規格、動作狀態和驗收標準寫清楚，懂動畫原理比懂程式更有幫助。

### Q：做一支動畫大概要花多少？

A：看長度和推理強度。公開案例裡，Blender 湖畔城堡約 13.3 美元 API 成本；兩分鐘沙畫用掉 Pro 方案五小時額度約三成；像素巫師約 30 分鐘，額度幾乎沒動。

### Q：Opus 5.5 和 GPT-6 Astra 做動畫誰比較好？

A：各有擅長。BitsMinds 的三道動畫題，Opus 5.5 以 10 分對 8 分領先，強在動作邏輯和整體完成度；Astra 速度較快，細部描繪常被認為更精緻。

### Q：做動畫要用 Claude 網頁版、Claude Code 還是 Claude Design？

A：公開作品裡有講明介面的，多半在 Claude Code 完成，因為要跑程式、渲染影片、長時間工作。單檔 HTML 小動畫在網頁版就能做；UI 動效和行銷短動態，適合在 Claude Design 的畫布上邊看邊調。

### Q：程式碼動畫跟影片生成模型差在哪？

A：程式碼動畫可以精準修改，改顏色、改秒數只要重新渲染；影片生成模型擅長寫實質感和人物表演，但要改細節通常得重新生成。

### Q：用 Opus 5.5 做動畫要開多高的推理強度？

A：建議從中等開始。Chris Riley 的 80 秒玻璃馬賽克用的就是中等；最高強度會花大量時間思考，適合沒辦法讓它看畫面、必須一次寫對的情況。