---
title: "Pixel Plays：零圖片像素遊戲的行數、授權與 Claude 查證"
source: "https://www.aiposthub.com/pixel-plays-zero-image-pixel-games-numbers-rules-claude-check/"
author:
  - "[[Philo]]"
published: 2026-11-13
created: 2026-11-19
description: "Pixel Plays 是 X 上超過 18 萬次觀看的開源像素遊戲。整理 3,557 行、圖片 0 張、MIT 與音樂例外、README 十條視覺規則與試玩連結，並查證「Claude 做的」。"
tags:
  - "clippings"
status: "inbox"
---
Pixel Plays 是一個開源的小遊戲專案：五個在瀏覽器就能玩的像素遊戲。作者 Amir Mushich 10 月 31 日把它貼在 X，現在超過 18 萬次觀看；貼文標題寫的是用 Claude Opus 5.5 做的，專案的 README 則說它沒有任何圖片素材。我們回到 GitHub 和可以玩的網頁，把能核對的逐項核對（資料時間：2026 年 11 月 10 日，台北）。先說結論：

- **數字準確。** 程式碼 3,557 行（5 個遊戲頁 3,067＋engine.js 343＋shell.js 147）、圖片檔 0 張、外部套件 0 個。專案另有 5 個.mp3 音樂檔（約 12.3 MB），不是圖片。
- **授權有例外。** 程式碼是 MIT 授權；音樂是用 Suno 生成的，作者寫明只能在這個專案裡使用，不在 MIT 內。
- **畫面是程式碼一個點一個點算出來的。** 畫布只有 360×280 個點，用整數倍放大。README 另有十條視覺規則（固定 30 度鏡頭、又長又硬的影子、暗處偏紫亮處偏暖黃……），下面整理成可以直接用的寫法。
- **「Claude 做的」目前是作者自述加提交標記。** 專案檔案裡沒有出現 Claude，也沒有提示詞；但兩筆提交說明最後都有一行「Co-Authored-By: Claude Opus 5.5」，沒有簽章，看不出 Claude 寫了多少。

每個說法都標明來源：作者本人（X 貼文、GitHub 專案）、第三方，或本站實測與整理。觀看數與星數只代表關注度，不代表品質。AI郵報另有同主題的 Reel。

## Pixel Plays 是什麼？誰做的？

| 項目 | 內容 | 來源 |
| --- | --- | --- |
| 作者貼文 | [X 貼文](https://x.com/AmirMushich/status/2116509069477621819?ref=aiposthub.com) ，2026/10/31 20:34（台北）。標題：“3D pixel games with Claude Opus 5.5 (OS)”（AI郵報自譯：用 Claude Opus 5.5 做的 3D 像素遊戲；OS 指開源）。附上程式碼與試玩網址，影片約 30 秒。11/10 22:10 顯示 18.58 萬次觀看 | 作者本人 |
| 專案 | [GitHub amirmushichge/pixel-plays](https://github.com/amirmushichge/pixel-plays?ref=aiposthub.com) ：5 個遊戲＋共用的 engine.js、shell.js；10/31 提交 2 筆；11/10 顯示 449 顆星、43 個 fork；授權 MIT | 作者本人 |
| 試玩 | [https://pixel-plays-ruby.vercel.app/](https://pixel-plays-ruby.vercel.app/?ref=aiposthub.com) （打開會導向第一個遊戲） | 作者本人 |
| 署名 | README 寫：Concept by abhi. Game by AmirMushich. Visual reference for the style: shawn.'s Small plays（AI郵報自譯：概念，abhi；遊戲，AmirMushich；畫風的視覺參考，shawn 的 Small plays）；並寫 “no assets or subjects were copied”（沒有複製任何素材或主題）。作者 X 貼文也列了同兩位的「靈感署名」。本站沒有看原作品，沒有比對「沒有複製」 | 作者本人（README） |
| 專案自我介紹 | Small browser games as isometric pixel dioramas. No build step, no dependencies.（AI郵報自譯：用等角像素立體模型呈現的小型瀏覽器遊戲。不需要建置，沒有相依套件） | 作者本人（README） |

## 五個遊戲怎麼玩？試玩連結

下表的玩法是本站依各遊戲內的 Rules 視窗整理的中文（英文原文在遊戲裡），畫面是我們用瀏覽器打開公開網頁截的。遊戲的背景音樂在第一次點擊後才載入，頁面右下角有 sound 開關。

| 遊戲 | 玩法（本站整理） | 操作 | 試玩 |
| --- | --- | --- | --- |
| Balote（滾球） | 對電腦。用紅球比藍球更靠近黃色小球；離黃球較遠的一方先投。8 顆投完後，比對手最佳那顆更近的球每顆 1 分，先到 5 分贏 | 往後拖再放開，拉越長投越遠（鍵盤：方向鍵＋空白鍵） | [試玩](https://pixel-plays-ruby.vercel.app/balote/?ref=aiposthub.com) |
| Stari Most（老橋跳水） | 從老橋跳水 5 次，要垂直入水（腳或頭先）；入水越直分數越高，每翻一圈倍率增加，平拍入水 0 分；風會推動旋轉 | 按住蹲下、放開起跳；空中按住縮身翻轉、放開伸展 | [試玩](https://pixel-plays-ruby.vercel.app/stari-most/?ref=aiposthub.com) |
| Rooftop Laundry（屋頂收衣服） | 暴風雨要來了，把曬衣繩上的衣服拖進近角的籃子；大件分數高但拖得慢；陣風會吹走衣服，雨會淋濕還沒收的（濕的分數較低） | 拖曳（鍵盤：空白鍵把下一件送進籃子） | [試玩](https://pixel-plays-ruby.vercel.app/laundry/?ref=aiposthub.com) |
| Tie-Break（紅土網球） | 球員自己跑，你只決定擊球的時機。球彈地後會有一個圈掃過對面場地，顯示球會落哪；黃圈界內、黑圈界外。先到 7 分且領先 2 分贏 | 點一下、輕觸或空白鍵；發球：點一下拋球、再點一下擊球 | [試玩](https://pixel-plays-ruby.vercel.app/tie-break/?ref=aiposthub.com) |
| Mini Ramp（滑板坡道） | 60 秒。像盪鞦韆一樣出力：下坡按住、上坡放開；速度夠會衝到空中，越高分越多；空中按住抓板加分，落地前要放開，否則摔倒 | 按住與放開（手指、滑鼠或空白鍵） | [試玩](https://pixel-plays-ruby.vercel.app/mini-ramp/?ref=aiposthub.com) |

![Balote 的草地沙場](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/pixelplays_g_balote.jpg)

Balote

![Stari Most 的老橋與河](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/pixelplays_g_stari.jpg)

Stari Most

![Rooftop Laundry 的屋頂](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/pixelplays_g_laundry.jpg)

Rooftop Laundry

![Tie-Break 的紅土球場](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/pixelplays_g_tiebreak.jpg)

Tie-Break

![Mini Ramp 的滑板坡道](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/pixelplays_g_ramp.jpg)

Mini Ramp

圖：五個遊戲的畫面（AI郵報用瀏覽器截取；專案程式碼為 MIT 授權，作者 Amir Mushich；音樂不在授權內，本站沒有使用）

## 貼文的數字準不準？

這篇的起點是 IG 帳號 your.engineer.ben 對這個專案的 [整理貼文](https://www.instagram.com/p/DfeZxJXgc7f/?ref=aiposthub.com) （約 11 月 8 日），它列了幾個數字與說法。我們把每一項都回 GitHub 與可玩的網頁重查，結果是：11 項裡 10 項準確，1 項要加註。

| 說法 | 結果 | 依據 |
| --- | --- | --- |
| X 上 17.9 萬次觀看 | 準確（發文當時） | X 頁面 11/10 22:10 顯示 18.58 萬 |
| 作者說是用 Claude Opus 5.5 做的 | 準確 | 作者 X 貼文標題 |
| 5 個遊戲＋引擎共 3,557 行 | 準確，要說明算法 | 5 個遊戲頁 3,067＋engine.js 343＋shell.js 147（換行數）；不含 serve.mjs、index.html、README 等 7 個小檔 |
| 圖片檔 0 張 | 準確 | 19 個檔案逐一看過；另有 5 個.mp3 |
| 外部套件 0 個 | 準確 | 沒有 package.json；README 寫 no dependencies；沒有載入外部程式庫 |
| Repo 裡沒寫 Claude | 要加註 | 14 個文字檔裡 Claude 0 次；但兩筆提交說明都有 Co-Authored-By: Claude Opus 5.5，GitHub 貢獻者名單也列了 Claude |
| 也沒附提示詞 | 準確 | 專案與提交說明裡都沒有提示詞或對話紀錄 |
| 「Claude 做的」目前是作者的說法 | 準確 | 只有作者貼文與作者自己倉庫的提交標記，沒有第二個獨立來源 |
| MIT 開源（音樂除外） | 準確 | README 與 LICENSE；MUSIC.md 說明音樂不在內 |
| 瀏覽器打開就能玩 | 準確 | 本站實測五個遊戲都能開、能操作 |
| README 的視覺規則：低解析度、固定 30 度鏡頭、長硬陰影、暗部偏紫亮部偏暖 | 準確 | README 共十條，這四點在第 2、3、5、6 條；engine.js 有對應的常數與註解 |

這則整理貼文沒提到、但我們查到的三件事：README 與作者貼文都署名了概念與畫風參考的兩位；專案有 5 個 Suno 生成的音樂檔；專案頁面有 2 個開放中的 PR（見授權一節）。

### 3,557 行是怎麼算出來的？

| 檔案 | 行數（換行數） |
| --- | --- |
| balote/index.html | 524 |
| laundry/index.html | 674 |
| mini-ramp/index.html | 579 |
| stari-most/index.html | 655 |
| tie-break/index.html | 635 |
| 五個遊戲頁小計 | **3,067** |
| engine.js（共用的繪圖與鏡頭） | 343 |
| shell.js（選單、規則視窗、署名、音樂） | 147 |
| 合計 | **3,557** |

這是本站讀 GitHub 的原始檔後數換行符號得到的結果（資料取自提交 0c2a70f 時的檔案）；GitHub 頁面上顯示的行數可能比這個數字多 1。其餘 7 個小檔（README、LICENSE、MUSIC.md、serve.mjs、index.html、vercel.json、.gitignore）共約 106 行，不在 3,557 內；第一筆提交一共新增 3,663 行，剛好是兩者相加。

### 圖片 0 張、外部套件 0 個，怎麼確認的？

| 類型 | 數量 | 說明 |
| --- | --- | --- |
| 圖片檔 | **0** | 沒有.png.jpg.svg.gif.webp；程式裡也沒有 new Image、\<img>、data:image |
| 網頁.html | 6 | 根目錄 1＋五個遊戲各 1 |
| 腳本.js／.mjs | 3 | engine.js、shell.js、serve.mjs（本機伺服器） |
| 文字與設定 | 5 | README.md、MUSIC.md、LICENSE、.gitignore、vercel.json |
| 音樂.mp3 | 5 | 共 12,298,667 位元組（約 12.3 MB） |
| 合計 | 19 | 與第一筆提交「19 files changed」相符 |

![GitHub 專案根目錄的檔案列表，沒有圖片檔](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/pixelplays_gh_files.jpg)

圖：GitHub 專案根目錄檔案列表（英文介面，AI郵報只裁切；2026/11/10 擷取）

本站另外用瀏覽器打開遊戲、看網路請求：打開 /balote/ 與 /mini-ramp/ 並點一下，載入的是 1 個網頁、2 個腳本（shell.js、engine.js）和 1 個音樂檔，沒有任何圖片、字型或資料請求。五個遊戲的 canvas 大小都是 360×280。（本站實測，2026/11/10。）

## 沒有圖片，畫面怎麼來的？

README 的說法是：每個遊戲都是一頁純 JavaScript，帶著自己的「軟體繪圖程式」（software rasteriser），沒有素材圖。白話說，就是用程式自己算出畫面上每個點的顏色，再畫到一張很小的畫布上，最後放大給你看。我們對照程式檔與實測，看得到的有這些：

- **畫布只有 360×280 個點。** engine.js 第 5 行寫 `const W = 360, H = 280`，實測五個遊戲都是這個大小；用最近鄰（不平滑）放大成整數倍，所以每個點都是清楚的方塊。
- **有深度緩衝（depth buffer）。** engine.js 檔頭說明共用的引擎提供「正交鏡頭、深度緩衝、色階、陰影、邊線與風」。深度緩衝是記下每個點離鏡頭多遠，誰在前面、誰被擋住，由它決定。
- **用規律排列的小點做出漸層。** engine.js 裡有 4×4 的 Bayer 抖點表。README 寫「四階明暗」；程式裡的色階陣列有 5 個顏色（索引 0 到 4），README 與程式碼的數字差 1，不影響看到的效果。
- **繪圖核心是共用的。** README 的一句話寫「每個遊戲帶自己的繪圖程式」，但 README 另一處與 engine.js 都顯示核心是五個遊戲共用的 engine.js。
![放大 12 倍的像素畫面：樹與人物都由方塊組成](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/pixelplays_z_pixels.jpg)

圖：Balote 畫面局部放大 12 倍（AI郵報截取並放大，沒有任何加工）：每個點都是一個方塊

## README 的十條視覺規則，可以怎麼用？

這是 README「Visual rules」那一段（中文為本站翻譯）。對照遊戲畫面與 engine.js，我們寫了「看得到的樣子」；最右欄是拿去用時怎麼驗收。

| # | README 說 | 遊戲裡看得到／程式碼對應 | 怎麼驗收 |
| --- | --- | --- | --- |
| 1 | 一個小地方，浮在純白上，四周沒有介面 | 背景純白；選單與分數是頁面上的小字，不在畫布裡 | 整張畫面只有一個場景，沒有按鈕邊框 |
| 2 | 低解析度畫布 360×280，最近鄰放大，整數倍 | 實測 canvas 360×280；engine.js `W = 360, H = 280` | 放大看，每個點是方塊、沒有糊邊 |
| 3 | 固定 30 度正交鏡頭；邊緣是參差的點狀紋路，溶進白底 | engine.js 檔頭：orthographic camera；沒有近大遠小 | 鏡頭不轉、不縮放；同樣大小的東西不會因遠近變小 |
| 4 | 即時繪圖：深度緩衝、四階明暗、Bayer 抖點 | engine.js 有 BAYER 表與色階陣列（5 個顏色） | 大片平面是規律的點狀漸層 |
| 5 | 又長又硬的影子往畫面右邊倒；每個遊戲只有一個看得懂的物理動作 | engine.js `SHADOW_X` 預設 1.3（正值往右） | 所有影子方向一致、邊緣不糊 |
| 6 | 色階偏色：暗處偏紫，亮處偏暖黃 | engine.js `COOL=[46,38,84]`、`WARM=[255,232,180]`，註解 hue-shifted ramp | 把暗處與亮處放大，暗的偏紫、亮的偏黃 |
| 7 | 深度緩衝邊線：前面的物體邊緣較暗，左上緣有暖色輪廓光 | engine.js 註解：物體在地面上會把自己的輪廓壓暗 | 疊在一起的物體之間有一圈邊線 |
| 8 | 大片平面用相近色調；色階差太大，抖點會變棋盤格 | —（本站沒有逐項驗證） | 平面不要出現棋盤格 |
| 9 | 人物保持清楚：色塊平整、不加抖點、有深色輪廓、花紋至少 3 像素寬 | engine.js 註解：人物用平整色階、不加抖點 | 人物邊緣比地面清楚 |
| 10 | 只有植物會動：草與葉每秒 8 格、整像素擺動；使用者設定減少動態時停止 | 實測樹葉與草在動；8 fps 本站沒有量 | 靜止畫面時只有植物在動 |

![README 的 Visual rules 十條英文原文](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/pixelplays_gh_rules.jpg)

圖：pixel-plays README「Visual rules」（英文原文；AI郵報只裁切；2026/11/10 擷取）

### 把規則寫成給 AI 的指令（本站整理的範例）

作者的專案與貼文都沒有附提示詞，所以下面不是作者的提示詞，是我們依 README 這十條規則整理的寫法。原則是只寫「可以驗收的項目」：尺寸、放大方式、鏡頭角度、影子方向、色階偏色、什麼會動。

```
做一個瀏覽器小遊戲：單一 HTML 檔、純 JavaScript，不使用任何圖片檔，不載入外部程式庫。
畫面規則：
1. 一個小場景浮在純白底上，四周沒有任何介面。
2. 畫布 360×280，用最近鄰放大到整數倍，不要平滑縮放。
3. 固定 30 度的正交鏡頭，不旋轉、不縮放。
4. 影子又長又硬，全部往畫面右邊。
5. 色階要偏色：暗處偏紫，亮處偏暖黃，不是單純變暗變亮。
6. 只有植物會動（每秒 8 格、整像素擺動）；人物保持清楚、不加點狀紋路。
玩法：只有一個看得懂的動作（例如按住蓄力、放開投出），一頁就能玩。
做完後逐條檢查上面 6 條，列出哪一條沒做到。
```

拿到結果後，用上表最右欄逐條驗收；沒有達到的條目再單獨叫 AI 改。

## 「Claude 做的」有證據嗎？

先說我們能確認的與不能確認的。能確認的有四件，每一件都回 GitHub 看過原頁：

| 檢查項目 | 結果 | 依據 |
| --- | --- | --- |
| 作者怎麼說 | 貼文標題寫「用 Claude Opus 5.5 做的」 | [X 貼文](https://x.com/AmirMushich/status/2116509069477621819?ref=aiposthub.com) （作者本人） |
| 專案檔案裡有沒有 Claude、提示詞 | 逐檔搜尋 14 個文字檔：Claude 0 次、prompt 0 次；沒有提示詞或對話紀錄檔 | 本站逐檔搜尋（GitHub 原始檔） |
| 提交說明 | 兩筆提交（ [770640d](https://github.com/amirmushichge/pixel-plays/commit/770640d8f92b553c8d063ee29feec401e2438462?ref=aiposthub.com) 10/31 20:09、 [0c2a70f](https://github.com/amirmushichge/pixel-plays/commit/0c2a70f6809478772ac0b3976496a5147e60a43e?ref=aiposthub.com) 20:12，台北）最後一行都是 Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com> | 作者本人的 GitHub 專案 |
| 貢獻者名單 | GitHub 專案頁列出 2 位：claude 與 amirmushichge | 作者本人的 GitHub 專案 |
| 有沒有簽章 | 兩筆提交都沒有簽章（GitHub API 的 verified 為 false，網頁沒有 Verified 標記） | 本站查 GitHub |

![第一筆提交的說明，最後一行是 Co-Authored-By Claude Opus 5.5](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/pixelplays_gh_commit1.jpg)

圖：第一筆提交（770640d）的說明（英文介面，AI郵報只裁切；2026/11/10 擷取）

![第二筆提交的說明，最後一行同樣是 Co-Authored-By Claude Opus 5.5](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/pixelplays_gh_commit2.jpg)

圖：第二筆提交（0c2a70f）的說明（英文介面，AI郵報只裁切）

Co-Authored-By 是寫在提交說明最後的一行文字，GitHub 會據此在提交旁列出共同作者。它是提交的人寫進去的文字，沒有簽章就不能由第三方驗證；它也沒有記錄 Claude 寫了多少行、改了幾輪、用了什麼指令。

**所以目前能說的是：「Claude 做的」是作者的自述，加上作者自己倉庫提交紀錄裡的共同作者標記。** 我們沒有第二個獨立來源（Anthropic 與 GitHub 都沒有說明這個專案），也沒有證據說它不是。沒有提示詞，所以讀者無法重現作者怎麼對 Claude 下指令；README 的視覺規則是目前唯一接近「指令」的東西。

## 授權：MIT，但音樂不在內

- **程式碼：MIT。** README 寫 “The code is under the MIT License.”，LICENSE 檔寫 Copyright (c) 2026 Amir Mushich。MIT 的條件是使用或散布時保留版權與授權聲明。
- **音樂：不在 MIT 內。** README 接著寫 “The music is not”（音樂不是）。MUSIC.md 說 5 個.mp3 是用 Suno 為這些遊戲生成的，放在專案裡是為了讓遊戲照原樣播放，“not licensed for reuse outside this project”（AI郵報自譯：不授權在這個專案之外重複使用）。
- **沒有音樂遊戲也能跑。** MUSIC.md 寫：A game runs without its track: only the background music is missing（缺了音樂檔，只是沒有背景音樂）；並建議要用這份程式碼就換成自己的曲子。本站這篇的畫面截圖沒有聲音，也沒有使用這些音樂。
![MUSIC.md 的內容：mp3 不在 MIT 授權內，用 Suno 生成](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/pixelplays_gh_music.jpg)

圖：pixel-plays 的 MUSIC.md（英文原文；AI郵報只裁切；2026/11/10 擷取）

### 專案頁面上的兩個公開 PR（第三方，都還沒合併）

| PR | 標題（AI郵報自譯） | 狀態 |
| --- | --- | --- |
| [#1](https://github.com/amirmushichge/pixel-plays/pull/1?ref=aiposthub.com) | 進行中：Fresh Paint——重新粉刷停車場褪色的標線（一個新遊戲） | Open |
| [#2](https://github.com/amirmushichge/pixel-plays/pull/2?ref=aiposthub.com) | 新增合成音樂：遊戲可以把自己的音樂寫成程式碼（內文說明：MUSIC.md 要求 fork 換掉 Suno 的曲子，合成音樂是程式的一部分，所以 MIT 涵蓋） | Open |

這兩個 PR 都是專案作者以外的人提出的，作者還沒有合併，也沒有回覆。這裡只當作「社群想補上音樂授權缺口」的動向，不代表專案會採用。

## 怎麼在自己的電腦跑？怎麼加新遊戲？

README 的說明：需要 Node.js，沒有任何套件要安裝；任何靜態檔案伺服器也可以，網站就是一組靜態檔。

```
node serve.mjs
# 然後打開 http://127.0.0.1:5300/
```

要加新遊戲，README 的步驟是（AI郵報自譯）：

1. 建一個資料夾，裡面放 `index.html`。
2. 在頁面載入 `../shell.js`，呼叫 `PixelPlays.mount({ id, rules })`。
3. 設定 `window.PP_SCENE`，載入 `../engine.js`。
4. 把遊戲加進 `shell.js` 裡的 `GAMES` 清單。

另外，頁面需要有一個 #mute 按鈕；背景音樂放在 `<id>/<id>.mp3`，沒有也能跑。提醒：執行別人的程式前先讀過檔案；本站只在瀏覽器打開了公開網頁，沒有在自己的電腦下載或執行這個專案。

## 這篇沒有查到、也沒有驗證的

- **Claude 實際寫了多少、有沒有人工修改、用了哪些指令** ：查不到，專案與貼文都沒寫。
- **共同作者標記是誰加的** ：不知道（有些工具提交時會自動加），我們沒有驗證。
- **README 說「沒有複製任何素材或主題」** ：本站沒有看概念與畫風參考的原作品，沒有比對。
- **草和樹葉每秒 8 格** ：README 的說法，本站只看到它們在動，沒有量。
- **貼文說「3D」** ：README 的說法是等角像素立體模型（isometric pixel dioramas），畫面看起來像 3D，實際是 360×280 的 2D 畫布加深度緩衝。
- **觀看數與星數會變** ：18.58 萬次觀看與 449 顆星是 11/10 晚上的數字。

## 常見問題

### Pixel Plays 真的一張圖片都沒有嗎？

19 個檔案裡沒有任何圖片檔，網路請求也沒有圖片。有 5 個.mp3 音樂檔，那不是圖片。

### 可以拿這份程式碼做自己的遊戲嗎？

程式碼是 MIT 授權，使用時保留版權與授權聲明即可；音樂不在授權內，要換成自己的。商用前請自己再確認授權條件。

### 可以拿遊戲畫面做影片或文章嗎？

本篇是這樣做的：用瀏覽器打開公開網頁截圖，標明作者與出處，沒有使用音樂。這不是法律意見；作者沒有另外寫畫面的使用規則。

### 作者有公開提示詞嗎？

沒有。專案與 X 貼文都沒附。本文的指令範例是本站依 README 規則整理的。

### 為什麼本站不直接說「Claude 做的」？

因為能確認的只有作者的貼文標題和提交說明裡的共同作者標記；兩者都出自作者自己，沒有獨立來源，也沒有簽章。

## 資料來源

- 作者 AmirMushich 的 X 貼文（2026/10/31）： [https://x.com/AmirMushich/status/2116509069477621819](https://x.com/AmirMushich/status/2116509069477621819?ref=aiposthub.com)
- GitHub 專案 amirmushichge/pixel-plays： [https://github.com/amirmushichge/pixel-plays](https://github.com/amirmushichge/pixel-plays?ref=aiposthub.com) （ [README](https://github.com/amirmushichge/pixel-plays?ref=aiposthub.com#readme) 、 [LICENSE](https://github.com/amirmushichge/pixel-plays/blob/main/LICENSE?ref=aiposthub.com) 、 [MUSIC.md](https://github.com/amirmushichge/pixel-plays/blob/main/MUSIC.md?ref=aiposthub.com) ）
- 提交紀錄： [https://github.com/amirmushichge/pixel-plays/commits/main](https://github.com/amirmushichge/pixel-plays/commits/main?ref=aiposthub.com) （ [770640d](https://github.com/amirmushichge/pixel-plays/commit/770640d8f92b553c8d063ee29feec401e2438462?ref=aiposthub.com) 、 [0c2a70f](https://github.com/amirmushichge/pixel-plays/commit/0c2a70f6809478772ac0b3976496a5147e60a43e?ref=aiposthub.com) ）
- 公開 PR： [#1](https://github.com/amirmushichge/pixel-plays/pull/1?ref=aiposthub.com) 、 [#2](https://github.com/amirmushichge/pixel-plays/pull/2?ref=aiposthub.com)
- 可玩的網頁： [https://pixel-plays-ruby.vercel.app/](https://pixel-plays-ruby.vercel.app/?ref=aiposthub.com)
- IG 帳號 your.engineer.ben 的整理貼文（約 2026/11/8；本文只用來決定查什麼，內容都回一手來源重查）： [https://www.instagram.com/p/DfeZxJXgc7f/](https://www.instagram.com/p/DfeZxJXgc7f/?ref=aiposthub.com)
- 本站實測：2026/11/10 用瀏覽器打開公開網頁看網路請求與 canvas 大小；用 GitHub 原始檔逐檔數行數並搜尋關鍵字（只讀、沒有存檔或執行）。

版權：遊戲程式碼與畫面屬作者 Amir Mushich（程式碼 MIT 授權）；GitHub 截圖屬各自頁面；本文文字與整理屬 AI郵報。