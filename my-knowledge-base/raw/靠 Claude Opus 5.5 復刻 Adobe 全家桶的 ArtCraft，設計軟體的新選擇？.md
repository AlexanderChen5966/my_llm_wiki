---
title: "靠 Claude Opus 5.5 復刻 Adobe 全家桶的 ArtCraft，設計軟體的新選擇？"
source: "https://www.aiposthub.com/ji-tian-fu-ke-photoshop-gong-neng-de-photocraft-dui-shang-yi-ge-ren-zuo-liao-shi-duo-nian-de-photopea-xian-zai-shui-bi-jiao-neng-yong-2/"
author:
  - "[[Hank]]"
published: 2026-10-14
created: 2026-10-19
description: "PhotoCraft 在幾天內復刻出 Photoshop 的主要功能，對上一個人做了十多年的 Photopea，現在誰比較能用？另有 Karpathy 的 AI 漏斗、Reflection AI 與 Nvidia、Grok Bot 路由、.agent 網域搶奪等本週 AI 大事。"
tags:
  - "clippings"
status: "inbox"
---
謝謝郵報讀者熱烈報名 **AI郵報 OPEN LAB 2026**  
隨著講者、品牌與社群夥伴陸續加入，我們只能說： **12/5，會比你想的還有趣。**

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/image-8-1.png)

PhotoCraft 開啟葛飾北齋《神奈川沖浪裏》的編輯畫面，右側有曲線調整層、圖層面板與圖層樣式，介面刻意對齊 Photoshop｜圖片來源：storytold/photocraft 專案 README

精彩 AI 短講、Live Demo、產品體驗、創作挑戰，還有《2026 AI郵報讀者調查》大公開。留點時間聊天，你想認識的合作夥伴，說不定就在現場。  
已報名的朋友，準備等我們陸續解鎖更多內容。  
還在觀望的朋友， **早鳥 NT$500 只到 10/31，趕緊把握時間。**  
約上朋友，或一個人來。12/5，我們現場見！

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/image-9-1.png)

PhotoCraft 開啟葛飾北齋《神奈川沖浪裏》的編輯畫面，右側有曲線調整層、圖層面板與圖層樣式，介面刻意對齊 Photoshop｜圖片來源：storytold/photocraft 專案 README

---

## 本週焦點事件

1. 靠 Claude Opus 5.5 復刻 Adobe 全家桶的 ArtCraft，設計軟體的新選擇？
2. Karpathy 自己估算：全世界只有約 5,000 人看得到真正的前沿 AI
3. Reflection AI 是誰？前 DeepMind 研究員創辦、主打美國開放權重旗艦，Nvidia 傳出想把它買下來
4. Grok Bot 要開始呼叫 Claude Opus 5.5：Musk 的個人 AI 助理，背後不再只有 Grok
5. .agent、.agi、.asi 被搶成熱門網域：OpenAI 兩年前花千萬買 chat.com，現在換成搶 AGI

---

## 靠 Claude Opus 5.5 復刻 Adobe 全家桶的 ArtCraft，設計軟體的新選擇？

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/issue-20261014-photocraft-demo.jpg)

PhotoCraft 開啟葛飾北齋《神奈川沖浪裏》的編輯畫面，右側有曲線調整層、圖層面板與圖層樣式，介面刻意對齊 Photoshop｜圖片來源：storytold/photocraft 專案 README

10 月 3 日，ArtCraft 的 Brandon Thomas 一口氣公開了七款對應 Adobe 產品的 **開源應用** ：Photoshop、Illustrator、Premiere Pro、Lightroom、Acrobat、After Effects 與 InDesign。到 10 月 11 日， **七個 repo 合計 86,265 顆星** ，其中對應 Photoshop 的 PhotoCraft 有 44,649 顆，10 月 14 日已經來到約 5.9 萬顆。

根據 [Linux Compatible 的報導](https://www.linuxcompatible.org/story/artcraft-the-free-opensource-adobe-suite-that-racked-up-43000-stars-in-days/?ref=aiposthub.com) ，這七款應用都是用 Rust 寫成，主要工具是 Claude Opus 5.5，建置時間有「幾天」與「數週」兩種說法。PhotoCraft 最近 100 筆 commit 中，有 76 筆帶有 Co-authored-by: Claude Opus 5.5 的標記，專案有 199 位貢獻者、合併了 1,004 個 PR。

那目前這個復刻版到什麼程度了呢？已經可以取代設計師的起家厝 Adobe 了嗎？目前整套的功能廣度約 76%（3/4 Adobe 的功能，Artcraft 都有 ），「可用於真實工作」約 45%，AI 功能約 10%、外掛與腳本約 15%、效能約 25%， **README 也寫明它還不是日常專業工作可用的 Photoshop 替代品。**

如果你是設計人，看到這裡大概會想到另一個名字：Photopea。是我個人非常喜歡使用，也非常敬佩的一個軟體。

Photopea 是 Ivan Kutskir 一個人做的瀏覽器版修圖軟體，介面與工作流程長得很像 Photoshop，直接在網頁裡開 PSD 檔，所以過去在外面沒辦法處理時，很多時候都是靠 Photopea 來止渴。Kutskir 因為常用 Photoshop，想在瀏覽器裡讀取.psd，先寫了一個把圖層分別匯出的小工具，這個小工具後來長成一個免費的修圖軟體。

我最佩服的地方是他的營運方式，Photopea 的廣告占收入將近九成，訂閱方案可以去廣告，另有授權與 API 整合的月費收入；2021 年初的年收入逼近 100 萬美元，日活躍用戶約 30 萬；沒有辦公室、沒有員工、沒有行銷預算，成長靠口碑與搜尋。

兩者現階段怎麼比，可以拆成幾個面向。這是依兩邊公開的描述整理，沒有做過同一批檔案的並排測試。

| 面向 | PhotoCraft（ArtCraft） | Photopea |
| --- | --- | --- |
| 成熟度 | 2026-09-30 建立 repo，專案自述 alpha，「可用於真實工作」約 45% | 約 2012–2013 年起步，持續營運十多年，一人維護 |
| 怎麼用 | 下載桌面版（macOS、Windows、Linux、FreeBSD）或網頁版，需自行安裝 | 打開瀏覽器就能用 |
| PSD 相容 | 專案自稱 135 檔中 134 檔可往返，仍有 Photoshop 不一定能開的檔案 | 長期支援 PSD，含圖層、遮罩、智慧型物件、圖層樣式、文字圖層 |
| 授權與取得 | 原始碼公開，MIT 或 Apache-2.0，可自行修改 | 免費加廣告，訂閱可去廣告，另有企業自架版 |
| 功能範圍 | 一次鋪開七款（含影片、PDF、版面設計），但每款完成度不一 | 以修圖為主，近年加入 AI 去背與生成功能 |
| 商業模式 | 桌面版免費，商業 AI 工作室走點數 | 廣告為主，訂閱與授權為輔 |

對一個接案設計師來說，現階段能交件的是 Photopea：十多年的相容性細節是它的資產，PSD 邊角案例、字型、效果都能使用。而 PhotoCraft 的優勢在別處：原始碼可以拿來改、不靠廣告、一次涵蓋七種工具，以及每週都在快速變動的速度。如果你需要的是「現在就能開客戶的 PSD、調完交出去」，用 Photopea；如果你想看一個開源的 Photoshop 長到哪裡，或想自己改它，值得追 PhotoCraft。

### 觀察筆記

我自己一直很敬佩 Photopea 的創辦人 Ivan Kutskir。一個人花了十多年，把一款瀏覽器修圖工具做到日活數十萬，而且幾乎不需要團隊、融資或行銷預算。這在過去的軟體產業，絕對是非常了不起的成就。

但看到 ArtCraft，我反而開始思考另一件事：在 AI 時代，像 Photopea 這樣花了十多年累積起來的產品優勢，還能維持多久？當然，現在的 PhotoCraft 還遠遠稱不上成熟。Photoshop、Photopea 累積多年的檔案相容性、操作細節與專業工作流程，不是 AI 幾天就能完整複製的。這些都是實際使用者花了無數時間測試、回報、修正，才建立起來的價值。

但真正讓我驚訝的，其實不是 ArtCraft 復刻出了多少 Adobe 的功能，而是它竟然可以在這麼短的時間內，做出七款已經具備一定功能的開源軟體。更重要的是，這些產品 **不再只能由原本的開發者決定下一步該做什麼。**

任何人都可以拿著原始碼，搭配 AI 修改、新增或優化自己需要的功能。今天你覺得某個操作流程不順手，過去可能只能等待官方更新，或尋找替代工具；但未來，你或許可以直接讓 AI 幫你改出一個更符合自己需求的版本。

這讓開源軟體在擴展性、客製化，甚至產品迭代速度上，出現了過去難以想像的優勢。尤其當 AI Coding 的門檻持續降低，能參與軟體開發的人也不再只有專業工程師。

更進一步想，在 AI 時代，Photoshop 本身會不會也是一個逐漸過時的產品？更不用說以模仿 Photoshop 為核心的 Photopea。這句話可能有點激進。我並不是說 Photoshop 明天就會被淘汰，現階段專業美工軟體仍然有大量難以取代的功能，尤其是精細編輯、複雜專案管理，以及那些不能出錯的專業工作流程。

但問題或許不是它們會不會被取代，而是被取代的速度，會不會遠比我們想像得更快？甚至我有時候會覺得，現在市面上可能有 70% 的軟體產品，都已經開始面臨某種程度的過時風險。這個數字當然只是我個人的直覺，甚至可能過於樂觀，但我認為這不只發生在傳統桌面軟體，連這幾年蓬勃發展的 Web App、SaaS，都未必能置身事外。

說回來 ArtCraft，我認為它真正值得關注的，不是它能不能成為下一個 Photoshop，而是它讓我們看到：當開發軟體的成本持續下降，軟體的價值可能會從「提供一套完整功能」，逐漸轉向「能不能隨著使用者的需求持續改變」。

---

## Karpathy 自己估算：全世界只有約 5,000 人看得到真正的前沿 AI

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/issue-20261014-karpathy.png)

Andrej Karpathy 在史丹佛的一場演講｜圖片來源：Gladwin Analytics，Wikimedia Commons，CC BY 3.0

Andrej Karpathy 在 10 月 11 日於 X 發了一串貼文，用心算的口氣給出一組數字：全世界大約只有 5,000 人，能在實驗室內部接觸到真正的前沿 AI；約 60 億人，也就是世界人口的四分之三，幾乎沒用過 AI；另有 10 到 20 億人只用免費聊天機器人問快速問題。他把這道落差稱為「sharp funnel」，尖銳的漏斗。

Karpathy 是前 OpenAI 共同創辦人，今年 5 月起加入 [Anthropic 的預訓練團隊](https://aiweekly.co/alerts/karpathy-joins-anthropic-to-lead-pre-training-research?ref=aiposthub.com) ，所以他說的那 5,000 人，他自己就站在裡面。這串貼文接續他 4 月談「人們怎麼判斷 AI」的舊文，當時的說法是兩群人各說各話，現在擴成一座有層次的漏斗：最上面是實驗室內部的 5,000 人，用的是能跑數週的 agent 群、能處理開放數學問題的系統； **往下約 2,000 萬人（0.2%）用付費前沿模型做程式等工作** ；再往下是 10 到 20 億的免費版使用者；最寬的底端是 60 億幾乎沒碰過的人。

他把落差歸給三件事。第一是存取層級，誰能用到最新、最貴的模型。第二是問題大小，能力要在長時間、大任務上才看得出來，一個快速問答看不出差別。第三是模型最強的地方，剛好是答案可以被檢查、又有商業價值的領域，像程式與數學。把三件事疊在一起，就能理解為什麼漏斗兩端的人談 AI 會雞同鴨講。

這有點像聊「台灣的食物」，一邊的人吃過主廚特製的無菜單料理，另一邊只吃過便利商店的微波便當，兩個人都沒說謊，講的卻是兩種東西。但這道落差跟一般的科技落差不一樣。電力、網路的早期也有人先用到、有人晚用到，可是 AI 不是專用在某一個領域的工具，它同時伸進程式、法律、醫療、教育、研究與軍事，各個領域的決策者都得靠它的資訊做判斷，而能判斷「它現在做得到什麼」的人，照 Karpathy 的估計只有 5,000 個。

體感上，這件事情很恐怖。如果你是會替團隊決定 AI 預算的人，或是常要跟身邊朋友解釋 AI 的人，先問對方用的是漏斗的哪一層，比反駁他的印象有用。Karpathy 把這個狀態叫做 **「weirdness of the moment」** ，多數人仍低估 AI，少數人已經感覺到曲線在往上直衝。漏斗的哪一層，決定了你看到的 AI 長什麼樣子。

---

## AI 郵報學院近期開課：

### 用 iPad 打造帶著走的 AI 創意提案流程

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/image-10.png)

Reflection AI 品牌圖｜圖片來源：Reflection AI 官網

由具 6 年獨立接案經驗、曾操刀 Team Wang Design、Hegen 古北町與 2025 空山祭等視覺動畫專案的視覺設計師許勝雄，親自帶你把 iPad 變成一座隨身的 AI 創意工作站。

課堂以五個實戰環節帶你操作：

- 用 GPT 拆解客戶需求，整理出提案方向與生圖提示
- 用 Krea 把文字概念快速轉成可以討論的視覺畫面
- 用 Affinity × Procreate 修正 AI 生圖瑕疵、補足細節
- 用 Shapr3D 把平面概念延伸成商品、空間或物件的 3D 呈現
- 用 Notion 整理素材、版本與提案內容

下課後，你將帶走一套「需求 → 發想 → 生圖 → 精修 → 3D → 提案整理」的 iPad AI 創意工作流，以及課程講義與資源包，不必回到電腦前，也能隨時推進提案與視覺製作。

特別適合：

- 接案設計師、自由工作者與數位遊牧工作者
- 想把 AI 生圖、修圖與 3D 概念加入工作流程的設計工作者
- 需要快速把文字想法轉成視覺的品牌與行銷企劃

### 用 Claude 不開剪輯軟體剪好口播影片

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/image-11.png)

Reflection AI 品牌圖｜圖片來源：Reflection AI 官網

由 AIPost 影片、AI Workflow 研究員 Philo，親自帶你用說的指揮 Claude，完成整套口播後製流程。

課程以四個實戰環節帶你操作：

- 在 Claude 桌面版 Code 分頁裝好 FFmpeg、Whisper 與繁中字型，學會下一句好指令
- AI 粗剪與精剪：剪空白、刪重拍、去口頭禪，並用剪點檢查頁逐刀驗收
- 用 Whisper 做繁中與英文字幕，再加入 B-roll、推鏡、字卡與配樂
- 用 Claude 製作 SVG 動畫片頭、數據長條圖與片尾模板

下課後，你將帶走一個專屬於自己的剪片 Skill，以及品牌風格檔與可替換文字的片尾模板，下一支影片用一句話就能開始剪，不必每次重新交代流程。

特別適合：

- 想開始拍口播、但不想先學剪輯軟體的短影音新手
- 每週需要產出多支影片的小編與行銷人員
- 想更有效率完成口播後製的講師與店家

---

## Reflection AI 是誰？前 DeepMind 研究員創辦、主打美國開放權重旗艦，Nvidia 傳出想把它買下來

![](https://cdn.sanity.io/images/sp40emik/production/003c45d4755800ad7e2b4c81076e43eff6e7cdf8-1200x630.png)

Reflection AI 品牌圖｜圖片來源：Reflection AI 官網

10 月 5 日，Reflection AI 發表了第一個開放權重模型 [Beam](https://thenextweb.com/news/reflection-ai-beam-open-weight-model?ref=aiposthub.com) ：總參數約 5,010 億，每次推論只啟動約 230 億，一百萬 token 的 context，採 Apache 2.0 授權，權重預計 10 月內釋出。五天後，FT 報導 Nvidia 正與這家公司進行早期談判， [選項包括收購、acqui-hire、加碼投資或算力合作](https://aiweekly.co/alerts/nvidia-in-early-talks-to-acquire-reflection-ai-or-deepen-stake-acqui-hire?ref=aiposthub.com) 。

如果你平常追的是 OpenAI、Anthropic、Google，Reflection 可能是比較陌生的名字。它總部在紐約，2024 年 3 月由兩位前 Google DeepMind 研究員 Misha Laskin（CEO）與 Ioannis Antonoglou（總裁兼 CTO）創辦，最早的定位是自主寫程式的 agent，現在的定位是做開放權重前沿模型的美國實驗室。在今年 5 月，Reflection AI 已同意向美國能源部與戰爭部供應模型，並稱正在訓練 Roadmap 上下一個更大的模型。

Beam 用的是 MoE（Mixture-of-Experts）架構。可以想成大型醫院的掛號：病人不會被全院醫師同時看診，只會進到對應科別的幾位醫師手上，所以 5,010 億參數裡，每個 token 只動用約 230 億，推論成本比同規模的稠密模型低。據 The Next Web 報導，Reflection 用 6,144 張 Nvidia GB300 做預訓練（23.8 兆 token，不到四週），另用 10,500 張做強化學習，今夏還在 6 月與 SpaceX 簽下 63 億美元、7 月與雲端業者 Nebius 簽下 10 億美元的算力合約。

而 Beam 這個開源權重模型，在進階推理上與中國 Z.ai 的 GLM-5.2 相當，推論算力少三到四倍，不過這是估算，只計入活躍參數與產出 token；在 SWE-bench Verified 上 Beam 為 80.9，高於 Thinking Machines 的 Inkling（77.6）；而在 Reflection 自己的表格裡，Moonshot 的 Kimi K3 在 Terminal Bench 上更高，88.3 對 80.1。

Nvidia 在這個故事裡的位置是三合一：已投資約 8 億美元，據報導稍早一輪 20 億美元的募資由它領投；同時是訓練 Beam 的 GPU 供應者；現在又傳出可能直接買下它。FT 提到的 acqui-hire，指授權技術、聘走團隊，但不取得公司本體，據報可避開冗長的反壟斷審查。談判仍在早期，Reflection 拒絕評論。標題上常見的 250 億美元，是最近一輪募資的 pre-money 估值，不是收購價。

### 觀察筆記

我自己蠻推崇本地開源 AI，最大的原因是不想被巨頭綁架，所以 Laskin 說「要擁有智慧，它必須開放」這句話我很有共鳴。問題在於 Beam 的 5,010 億參數，就算每次只啟動 230 億，也不是一般個人電腦跑得動的東西。開放權重讓人「有權」下載，不代表「有能力」跑，中間隔著幾百張 GPU 的距離，所以之前常常傳出一些 meme，說什麼老闆聽到某個模型開源，就想吩咐工程師來跑，但沒想到模型極大，光是機房建置可能就要花上千萬。

所以對個人來說，擁有智慧大概還是得從小模型開始；Beam 這類大型開放權重，主要服務的是有機房的企業與政府。所以我現在的結論是，開放權重確實把綁架我的籌碼從模型廠商手上拿走一半，另一半還在 GPU 供應商那裡 XD

---

## Grok Bot 要開始呼叫 Claude Opus 5.5：Musk 的個人 AI 助理，背後不再只有 Grok

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/image-6.png)

【深度專題】Context Engineering 是什麼？為什麼 Manus 團隊花千萬美金踩坑，只為搞懂「怎麼喂模型」？

10 月 7 日，Elon Musk 在 X 宣布，Grok Bot 之後會為每一項任務使用「最佳的後端模型」，名單包括 Anthropic 的 Claude Opus 5.5、 **圖像工具 Midjourney (好久沒看到 Midjourney 了 XD)** 、音樂工具 Suno，以及其他領先的 API。

Grok Bot 是 xAI 在 8 月推出的常駐型 AI 助理。所謂「路由」，指的是一個入口接收任務之後，依任務類型決定交給哪個模型處理。以前 Grok Bot 後面只有 xAI 自己的模型，現在 Musk 宣布，文字與推理可以交給 Claude，圖像交給 Midjourney，音樂交給 Suno。

在 agent 產品裡處理 token 費用，也不是 Grok Bot 一家在做。Manus 在 9 月 28 日發表的 [Manus 2.0](https://manus.im/blog/introducing-manus-2-0?ref=aiposthub.com) 換上自研的 agent 框架 Cascade，官方稱在「一次測試配置」中，比舊系統少用 23.2% 的 token、完成時間縮短 28.2%、執行成本降低 32%。頁面沒有公布測試的任務集與成功率，也沒有說 Cascade 把任務分給不同模型，說的是需要時才引入專門能力，讓上下文保持精簡。隔天推出的 [Manus Flex](https://manus.im/blog/introducing-manus-flex?ref=aiposthub.com) 則讓使用者帶自己的 API 金鑰，由 OpenRouter、Fireworks、Modal 等供應商負責推論，並直接向使用者計費。這兩個動作都在處理 token 費用，與 Grok Bot 把任務路由到別家模型的做法相鄰，但切入的位置不同：Manus 動的是 agent 框架與計費方式，Grok Bot 動的是後端模型的來源。

### 觀察筆記

看到 Musk 宣布 Grok Bot 未來會直接調用 Claude Opus 5.5、Midjourney、Suno 等第三方模型，我第一時間想到的，反而是之前 Manus 團隊分享的 Context Engineering 七大原則。

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/image-7.png)

【深度專題】Context Engineering 是什麼？為什麼 Manus 團隊花千萬美金踩坑，只為搞懂「怎麼喂模型」？

當時 Manus 提出一個很有意思的觀點：與其花大量資源訓練自己的基礎模型，不如思考如何更有效率地使用現有模型。

從上下文管理、工具調用、記憶機制，到 KV Cache 的最佳化，真正影響 Agent 表現的，未必只有模型本身的智力，而是整套系統如何運作。現在回頭看 Grok Bot，連擁有自家基礎模型的 xAI，都開始公開擁抱第三方模型。這件事對我來說其實蠻有指標性的。

**當大家都能使用最好的模型，Agent 產品究竟還剩下什麼差異？**

我認為答案可能逐漸從 Model，轉移到 Agent Harness、Context Engineering，以及整套任務協調的架構上。畢竟，把 Claude、Midjourney、Suno 的 API 串在一起並不困難；真正困難的是，如何讓它們理解同一個任務、交換正確的資訊、管理執行狀態，並在成本、速度與品質之間找到平衡。

過去我們經常拿背後使用哪個模型，來判斷一款 AI 產品強不強。但未來，也許更值得關注的，是它能否在不同模型之間自由切換，甚至當新的模型出現時，不需要重新打造整個產品，就能立即享受技術進步帶來的紅利。

---

## .agent、.agi、.asi 被搶成熱門網域：OpenAI 兩年前上億買 chat.com，現在換成搶 AGI

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/issue-20261014-icann.svg)

ICANN 標誌，這一輪新頂級網域申請由 ICANN 公布名單

ICANN 在 10 月 7 日公布了新頂級網域申請輪的名單：481 位申請者共提出 1,615 件申請，其中 AI 相關的後綴最擁擠。依 [CircleID 的統計](https://circleid.com/posts/openai-meta-and-registry-operators-seek-new-gtlds-as-.agent-draws-13-applications?ref=aiposthub.com) ，.agent 有 13 件申請，申請者包括 OpenAI、Meta、Radix、Google 關聯的 Charleston Road Registry 與 XYZ.COM。

頂級網域（TLD）是網址最後面那一段，像.com、.org。ICANN 是管理全球網域系統的非營利組織，這一輪是自 2012 年以來首度大規模開放新後綴申請。如果你是做品牌或商標的人，可能對「爭奪集合」這個詞不陌生：多人申請同一個字串時，要經過協商、拍賣或其他程序，決定最後由誰取得。

名單公布不等於核准，現階段沒有任何一個後綴可以註冊。OpenAI 提出 15 件主要申請，包含.openai、.chatgpt、.codex、.gpt、.agent、.agi、.asi 與.mcp，其中.openai、.chatgpt、.codex 等是品牌專屬字串。Meta 提出 18 件，名單有.facebook、.instagram、.whatsapp 與.meta。

把時間拉長一點，這場搶網域不是第一次。2024 年底，OpenAI 買下 chat.com，據 [The Block 引述的報導](https://www.theblock.co/post/388975/crypto-com-ceo-kris-marszalek-bought-ai-com-for-70-million-in-crypto-plans-super-bowl-debut?ref=aiposthub.com) 價格在 1,550 萬美元以上；這個網域原本在 HubSpot 共同創辦人 Dharmesh Shah 手上，他 2023 年以約 1,550 萬美元買下，OpenAI 的實際成交價沒有公開。2025 年 4 月，Crypto.com 執行長 Kris Marszalek 以約 7,000 萬美元、用加密貨幣買下 ai.com，2026 年 2 月才公開，被認為是有紀錄以來金額最高的網域交易；買家是 Marszalek，不是 OpenAI。一個是 chat，一個是 ai，這次排隊的字串換成 agent、agi 與 asi。

接下來的時程已經排好：10 月 8 日到 21 日，申請者可以更換字串；11 月 17 日是「字串確認日」，ICANN 會公布最終字串與更新後的爭奪集合，並開放社區意見與異議期；再往後是評估、爭奪解決、簽約與委派，這幾步的日期還沒有公布。

---

喜歡這期內容嗎？有哪一則讓你特別有感？  
歡迎回信或是 Instagram 告訴我們，我們會偷偷讀大家的回覆的！

[AI 郵報 Instagram](https://www.instagram.com/aiposthub/?ref=aiposthub.com)

我們下週見  
— AI 郵報編輯團隊