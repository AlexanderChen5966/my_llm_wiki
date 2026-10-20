---
title: "像素畫怎麼畫？Pixel Art 入門：從紅白機到 HD-2D，學會調色盤、抖色、反鋸齒與色彩循環"
source: "https://rar.design/posts/pixel-art-beginner-guide"
author:
  - "[[設計師 Riven]]"
published: 2026-09-29
created: 2026-09-30
description: "像素藝術（Pixel Art）是在有限的格子與顏色裡刻意決定每一格的畫法。本文從 1978 年街機、紅白機講到 2018 年 HD-2D，拆解色相偏移、抖色、反鋸齒、色彩循環四個技法，並整理 Aseprite、Pixelorama、Photoshop 的上手方式與 16×16、4 色的練習法。"
tags:
  - "clippings"
status: "inbox"
---
從一支 100 秒像素動畫的幕後講起，拆解像素藝術四十年的歷史、四個核心技法與新手起步練法

![設計師 Riven](https://s.teachifycdn.com/image/width=128,quality=80,format=webp/lecturer/avatar/fd0b78a9-03c9-4ad3-ae0a-a8965aa676ea/d822e5e9-519b-4f2e-b049-807272104edd.png)

[設計師 Riven](https://rar.design/@riven)

2026年9月29日 下午 8:46

平面設計

PIXEL ART · 像素藝術 · 2026

![像素藝術 Pixel Art 教學封面：吉祥物角色站在黃昏懸崖上，山谷、湖面倒影與瀑布全以像素繪製，標題「一格一格，畫出整個世界」](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/bbb4be07-64a5-4bc3-aefd-23eb268724d3/190b6708-7301-4bc7-8c82-b3843b1976ba.jpg)

像素藝術 Pixel Art 教學封面：吉祥物角色站在黃昏懸崖上，山谷、湖面倒影與瀑布全以像素繪製，標題「一格一格，畫出整個世界」

▲ 像素藝術（Pixel Art）：一格一格，畫出整個世界。這是我最近完成的像素動畫短片開場畫面，本篇的定格圖都來自這支片。

QUICK ANSWER

像素藝術（Pixel Art）是在有限的格子與顏色裡，刻意決定每個像素的畫法。它在記憶體昂貴的早期電腦與遊戲機上發展起來，2010 年代以來越來越流行。新手從 16×16、4 色開始，用 Aseprite、免費的 Pixelorama 或 Photoshop 鉛筆工具都能畫，放大只用整數倍加最近鄰。

我最近做的一支像素動畫短片，第一個畫面只有一個像素。

全黑的畫面正中間，一格淡青色的光，像文字游標一樣閃著。那是放大到 6400% 的一個點。接著鉛筆游標在它周圍一格一格補上顏色，一隻眼睛、一張臉慢慢成形；鏡頭再一階一階往後退——3200%、1600%、800%、400%——最後退成一整座黃昏的山谷：懸崖、瀑布、湖面倒影，還有我的吉祥物站在崖邊。

這個開場想說的只有一件事：像素藝術的每一張畫，都是這樣一格一格決定出來的。這支片的主調色盤只有 32 色，做完它，我把過程中查證過的歷史、實際用上的技法，整理成這篇。完整的 100 秒短片就在下面。

![](https://www.youtube.com/watch?v=1pWt4ppjJ3s)

▲ 像素藝術 100 秒入門：從 1978 年的街機講到 2018 年的 HD-2D，再拆解色相偏移、抖色、反鋸齒、色彩循環四個技法。

做設計、教設計快十年，我一直覺得限制是設計最好的老師，像素藝術大概是這句話最極端的版本。

CONTENTS

*01*

[像素藝術：每一格都有理由](#ch01)

*02*

[從街機到 HD-2D：四十年](#ch02)

*03*

[調色盤與色相偏移](#ch03)

*04*

[抖色：兩個顏色，假裝成第三個](#ch04)

*05*

[線條與反鋸齒](#ch05)

*06*

[動起來：逐格動畫與色彩循環](#ch06)

*07*

[工具與上手：16×16、4 色起步](#ch07)

*08*

[重點整理](#ch08)

CHAPTER 01 · WHAT IS IT

## 什麼是像素藝術：每一格都有理由

Pixel 由 pix（pictures）和 el（element）組成，也就是 picture element，意思是「畫面元素」。這個字在 1965 年首次見於出版品，NASA 噴射推進實驗室的 Frederic C. Billingsley 用它來描述太空探測器掃描回來的月球與火星影像。今天每一張照片、每一張截圖都由像素組成，所以「由像素組成」這件事，定義不了像素藝術。

片子裡我做了一個對照。同一顆櫻桃，左邊把一張平滑的高解析圖自動縮成 16×16，右邊一格一格手工擺成 16×16。尺寸完全一樣。左邊擠出了 94 種混濁的中間色，糊、雜、沒有重點；右邊只用 9 色，輪廓、亮面、果實上的高光，每一格都在它該在的位置。

![像素藝術是什麼：同一顆 16×16 櫻桃，自動縮小後糊成 94 色，手工擺放只用 9 色、每一格都有理由](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/2f337cee-4ef5-4f8a-bf8c-91c930e8e645/a10fe701-e5b5-4f3b-98ce-28a8fb91ba70.jpg)

像素藝術是什麼：同一顆 16×16 櫻桃，自動縮小後糊成 94 色，手工擺放只用 9 色、每一格都有理由

▲ 同樣是 16×16 的櫻桃：左邊自動縮小，擠出 94 種混濁的中間色；右邊手工擺放只用 9 色。解析度一模一樣，右邊清楚得多，因為每一格都經過決定。

自動縮小的那顆用了十倍以上的顏色，看起來反而比較差。這是我覺得最反直覺、也最能說明像素藝術的一點。它的核心是在有限的格子與顏色裡，刻意決定每一格；解析度低只是外觀。

這也是它跟「像素效果」的分界。把照片一鍵轉成像素顆粒，我在〈 [Photoshop 經典懷舊像素顆粒效果](https://rar.design/posts/dithered) 〉教過那條路：減色、抖色、放大，全交給軟體算。那是很好用的效果，只是每一格放什麼顏色由演算法決定。像素藝術做的事，是把這個決定權拿回自己手上。

CHAPTER 02 · HISTORY

## 從街機到 HD-2D：像素藝術的四十年

像素藝術一開始沒打算當一種風格。早期的機台與家用主機解析度低、顏色少，很大一部分原因是記憶體很貴——畫面上能放多少格、每格能選幾種顏色，全被預算卡死。接下來，限制一個一個鬆綁，風格卻留了下來。

TIMELINE · 1978–2018

**1978**

**Space Invaders**

黑白螢幕＋彩色玻璃紙

**1982**

**Pixel Art 一詞見刊**

目前已知最早的見刊紀錄

**1983**

**紅白機（日本）**

8×8 圖塊 · 每個圖塊 3 色＋透明

**1984**

**初代 Macintosh**

Susan Kare 用方格紙畫圖示

**1989–1990**

**Game Boy · 超任**

4 階灰，到可選 32,768 色

**1994–1996**

**3D 主機登場**

主流轉向 3D，像素沒有消失

**2010 年代以來**

**像素風越來越流行**

很多作品來自獨立工作室

**2018**

**HD-2D**

《歧路旅人》：像素角色＋3D 場景＋光影景深

### 1978–1984：玻璃紙與方格紙

1978 年 7 月，Taito 在日本推出《Space Invaders》（中文常稱《太空侵略者》）。它的畫面其實是黑白的，螢幕上某幾段貼了橘色和綠色的透明玻璃紙，那幾塊區域看起來就成了彩色。硬體給不了顏色，就在玻璃上想辦法。

英文 Pixel Art 這個詞，目前已知最早的見刊紀錄是 1982 年 12 月：Xerox PARC 的 Adele Goldberg 與 Robert Flegal 在《Communications of the ACM》發表的一封信，標題就叫〈ACM President's Letter: Pixel Art〉。做法本身更早，1973 年，同樣在 Xerox PARC 的繪圖系統 SuperPaint 就畫出了第一張穩定的影像。

對設計師來說，最有感的名字應該是 Susan Kare。她為 1984 年 1 月推出的初代 Macintosh 設計了圖示和最早的幾套字體，那些 32×32 像素的圖示，是先在方格紙上一格一格塗出來的；2015 年，紐約現代藝術博物館（MoMA）收藏了她的手稿本。小尺寸還要一眼認得出來，這套紀律到今天畫 icon 還是一樣，網格怎麼抓，我在〈 [Figma 圖解設計系統｜繪製 ICON 圖標](https://rar.design/posts/iconkeylines) 〉拆過。

### 1983：8×8 圖塊與換色

1983 年 7 月，任天堂在日本推出紅白機（Famicom）。主機輸出的畫面是 256×240 像素，所有圖都由 8×8 像素的圖塊拼成，角色用的每個圖塊只能從 3 色＋透明裡挑。整台主機可選約 54 色，同一個畫面最多只能出現 25 色。

限制這麼緊，換色（palette swap）就成了常見做法：同一組圖塊換一組調色盤，就多一種敵人、多一個 2P 角色，不用重畫，也不用多佔記憶體。

![紅白機 8-bit 像素角色換色示範：由 2×2 個 8×8 圖塊組成的角色只用 3 色＋透明，換一組調色盤就從 1P 變成 2P](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/ede004c4-1058-4681-82e3-36d130a73698/c9b7d0cf-1d51-45e2-889a-8abce03ca4ae.jpg)

紅白機 8-bit 像素角色換色示範：由 2×2 個 8×8 圖塊組成的角色只用 3 色＋透明，換一組調色盤就從 1P 變成 2P

▲ 紅白機的圖以 8×8 圖塊組成，每個圖塊 3 色＋透明。片中這個示範角色由 2×2 個圖塊拼成，換一組調色盤就從 1P 變成 2P，不用重畫。電視裡是原創的示意畫面。

常聽到的 8-bit、16-bit，大致對得上主機 CPU 的位元數：紅白機用 8 位元 CPU，超任用 16 位元 CPU。它同時也是當年的行銷標籤，像北美的 TurboGrafx-16（日本叫 PC Engine）名字掛著 16，CPU 其實是 8 位元，只是搭了 16 位元的繪圖晶片，這個命名也被批評有誤導之嫌。把它當成時代的暱稱，會比當成規格準確。

### 1989–1990：從 4 階灰到可選 32,768 色

1989 年的 Game Boy 走向另一個極端：160×144 的螢幕，只有 4 階偏綠的灰。隔年 11 月，超任（Super Famicom）在日本上市，調色盤一口氣放大到可選 32,768 色，同畫面最多 256 色，最常見的解析度是 256×224。

![像素藝術調色盤演進：同一角色在 1-bit（2 色）、Game Boy（4 階）、紅白機（可選約 54 色）、超任（可選 32,768 色）四個時代的樣子](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/80904081-fa4f-4207-9b21-90cb3eef9f82/d8724ff7-682d-4a2b-829f-5963b9e04ed5.jpg)

像素藝術調色盤演進：同一角色在 1-bit（2 色）、Game Boy（4 階）、紅白機（可選約 54 色）、超任（可選 32,768 色）四個時代的樣子

▲ 同一個角色，四種「可選顏色」：1-bit 的 2 色、Game Boy 的 4 階、紅白機可選約 54 色、超任可選 32,768 色（同畫面最多 256 色）。

主機

**解析度**

**可選顏色**

**同畫面**

紅白機（1983）

256×240

約 54

最多 25

Game Boy（1989）

160×144

4 階

4 階

超任（1990）

256×224

**32,768**

**最多 256**

表格裡請分開看「可選」和「同畫面」兩個數字。超任的 32,768 色是調色盤的大小，同一時間畫面上最多 256 色；只寫 32,768 色，會讓人以為畫面上同時有那麼多顏色。從 25 色到 256 色，選擇變多了，挑色這件事卻一直都在。

### 90 年代中期之後：3D 來了，像素沒走

1994 到 1996 年間，Sega Saturn、PlayStation、Nintendo 64 相繼在日本上市，主流一口氣轉向 3D 多邊形。像素藝術沒有消失，它在一部分主機遊戲和掌機上繼續活著。

2010 年代以來，像素風越來越流行，很多作品來自獨立工作室：Shovel Knight（2014）、Stardew Valley（2016）、Hyper Light Drifter（2016）、Celeste（2018），一路到 Eastward（2021）、Sea of Stars（2023）。這些遊戲跑在不缺顏色的硬體上，像素在它們手上是刻意選擇的風格。

2018 年 7 月，Square Enix 與 Acquire 開發的《歧路旅人》（OCTOPATH TRAVELER）在 Switch 上推出，是 Square Enix 稱為「HD-2D」風格的第一款作品：像素角色放進 3D 場景，再加上動態光影、景深和移軸效果，整個畫面像一座微縮模型。之後 Triangle Strategy（2022）和 Dragon Quest III 的 HD-2D 重製版（2024）也用了這個風格。

![《歧路旅人》的 HD-2D 畫面：像素角色站在立體的墓園場景裡，前後景有明顯景深模糊，看起來像一座微縮模型](https://www.nintendo.com/tw/switch/octopath_traveler/img/4.jpg)

《歧路旅人》的 HD-2D 畫面：像素角色站在立體的墓園場景裡，前後景有明顯景深模糊，看起來像一座微縮模型

▲ 2018 年的《歧路旅人》（OCTOPATH TRAVELER）是 Square Enix 稱為「HD-2D」風格的第一款作品：像素角色放進 3D 場景，再加上光影與景深。圖片來源：任天堂官方網站（台灣）

限制就這樣一個個被拿掉：可選顏色從約 54 色變成 32,768 色，主流畫面從 2D 走向 3D。像素藝術還在，而且越來越多人主動選它。大家留戀的，除了懷舊，還有那份取捨帶來的清楚。

CHAPTER 03 · PALETTE

## 調色盤與色相偏移：顏色越少，越要精挑細選

進入技法。第一個技法，也是後面幾個的前提：先限制顏色，再開始畫。

一組小而刻意挑選的調色盤，是像素藝術的核心紀律之一。新手教學通常建議從 4 色開始；大家共用的調色盤從幾色到幾十色都有，像 PICO-8 的 16 色，或是很多 32 色的調色盤。PICO-8 本身是一台「幻想主機」，刻意把自己限制在 128×128 的畫面、16 色——當年的限制是硬體給的，現在的創作者是自己把它戴回去。

顏色少，一條色階就得撐起整個物體的明暗。直覺的做法是只調明暗：同一個紅，往暗就加黑、往亮就加白。這種色階很容易顯得髒，暗部灰灰的，亮部像粉筆。

解法叫色相偏移（hue shifting）：沿著色階往暗、往亮走的時候，連色相一起轉。常見做法是暗部偏冷、偏藍，飽和度也常降一點；亮部偏暖、偏黃。

![色相偏移 hue shifting 對比：左邊像素球只調明暗顯得髒，右邊色階從暗紫紅偏移到亮黃，暗部偏冷、亮部偏暖](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/1ce4239d-29f7-45d3-b341-0bdaf2c4121c/c75e89b5-7e6f-42c6-8bec-03129a1f6cc3.jpg)

色相偏移 hue shifting 對比：左邊像素球只調明暗顯得髒，右邊色階從暗紫紅偏移到亮黃，暗部偏冷、亮部偏暖

▲ 兩顆球都只用 5 色。左邊只調明暗，看起來髒；右邊讓色相跟著明暗一起移動。暗部偏冷、亮部偏暖是常見做法之一，可以依畫面調整。

HUE SHIFT · 同樣 5 色，兩種走法

只調明暗：色相幾乎不動

#530e0f

H 359°

#892928

H 1°

#ca4542

H 1°

#e77e77

H 4°

#fbafa8

H 5°

色相偏移：暗部偏紫紅，亮部轉向橘黃

#4a1433

H 326°

#8c1f3d

H 343°

#d13b3b

H 0°

#f27a3d

H 20°

#ffb54a

H 35°

上排色相只在 0° 附近打轉，下排從 326° 轉到 35°，跨了將近 70°。H 值為 HSL 色相，數值四捨五入。

用 HSL 來想最直覺：色階往暗走時，L 往下，H 也跟著轉。HSL 這套思考方式，我在〈 [配色公式怎麼用？](https://rar.design/posts/color-palette-formulas) 〉有完整拆解。

冷暗暖亮只是常見做法之一。這支片的主調色盤是 32 色、6 條色階，每一條都做了色相偏移，方向卻不全是從冷到暖：膚色那條是從紅走到橘。先懂為什麼要轉，再決定往哪裡轉。想找現成的調色盤，可以去 Lospec，上面有 4,000 多組免費的像素調色盤，照色數篩選就好。

CHAPTER 04 · DITHERING

## 抖色：兩個顏色，假裝成第三個

抖色（dithering）是讓兩個顏色按圖樣交錯排列，例如 50% 的棋盤格，或 Bayer 這類有序抖色，遠看就像中間色、漸層或質感。顏色預算只有 4 色的時候，它等於幫你憑空多變出幾階。

![像素抖色 dithering 教學：夕陽天空的色帶交界用棋盤格抖色過渡，放大 1600% 可見兩色交錯，旁邊是 25%、50%、75% 三種抖色圖樣](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/84880373-866a-473b-a0b2-8045924fe72f/4189f36a-d835-40f7-8adf-b519f775d593.jpg)

像素抖色 dithering 教學：夕陽天空的色帶交界用棋盤格抖色過渡，放大 1600% 可見兩色交錯，旁邊是 25%、50%、75% 三種抖色圖樣

▲ 抖色就是讓兩個顏色交錯排列，遠看像第三色。放大到 1600% 才看得出棋盤格；縮回 100%，25%／50%／75% 三種圖樣就混成三種中間色。

片子裡的夕陽天空就是這樣過渡的，每條色帶的交界都是抖色。它有一個代價：用太多，畫面很快就變成雜訊。Derek Yu 在他的像素教學裡也提到，自己用抖色用得很節制。

當年的電視讓抖色多了一層意思。接 RF 或 AV 端子的訊號本身就會糊，相鄰像素常常微微暈開，部分遊戲就利用這一點：例如 Mega Drive 上，直條的抖色圖樣經過 AV 訊號，會混成看起來像半透明或中間色的效果。把照片轉成 GIF 時軟體自動加上的顆粒圖樣，也是抖色，差別在每一格交給演算法決定。

CHAPTER 05 · LINES

## 線條與反鋸齒：一條線也有節奏

像素世界裡沒有真正的斜線和曲線，只有一段一段水平、垂直的線段。鋸齒（jaggies）會出現，是因為線段長度變化得不規律。

乾淨的直線要保持固定的步伐，例如 1 比 1、2 比 1；乾淨的曲線，線段長度要穩定地遞增或遞減。同一段圓弧，6-3-2-1 看起來順，3-1-3 忽長忽短，就會出現鋸齒。

![像素線條鋸齒對比：左邊圓弧線段 3-1-3 忽長忽短產生鋸齒，右邊線段 6-3-2-1 規律遞減，曲線較順](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/6c0d1193-9e28-4f4a-9e06-76f50bd470c7/fdc0ae61-58ef-4b87-b2cd-b142b656249b.jpg)

像素線條鋸齒對比：左邊圓弧線段 3-1-3 忽長忽短產生鋸齒，右邊線段 6-3-2-1 規律遞減，曲線較順

▲ 同一段圓弧，線段長度 3-1-3 忽長忽短就會出現鋸齒（jaggies）；6-3-2-1 規律遞減，看起來就順。

第二個常見問題是 L 型轉角：用 1 像素的鉛筆畫曲線，轉彎處常會多出一格，線就變粗、變髒。Aseprite 這類編輯器有「Pixel-perfect」選項，勾起來，畫的時候就自動把那一格拿掉。

![Aseprite 官方示範動畫：同樣畫一條 1 像素的曲線，關閉「Pixel-perfect」時轉角有多餘像素，勾選後轉角變乾淨](https://www.aseprite.org/assets/images/pixel-perfect.gif)

Aseprite 官方示範動畫：同樣畫一條 1 像素的曲線，關閉「Pixel-perfect」時轉角有多餘像素，勾選後轉角變乾淨

▲ Aseprite 官網的示範：鉛筆勾選「Pixel-perfect」後，1 像素線條轉角多出來的 L 型像素會被自動拿掉。圖片來源：aseprite.org

線條乾淨了，才輪到反鋸齒（anti-aliasing）。手動反鋸齒是在線段的階梯轉角補上中間色，縮小來看，階梯就被磨圓。它要挑著用。遊戲角色的外輪廓通常不補，因為角色會站在什麼顏色的背景上並不固定，替淺色背景調好的中間色，一到深色背景就變成一圈突兀的邊。

![像素反鋸齒 anti-aliasing 教學：放大 1600% 的圓形線稿在階梯轉角補上兩個灰色中間色，並以 1×、2×、4× 預覽比較補色前後](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/b16dac6e-3153-4eb4-9a12-750c1c4ede3c/9296bb7d-a36c-4c52-a338-e0f5d3ca3265.jpg)

像素反鋸齒 anti-aliasing 教學：放大 1600% 的圓形線稿在階梯轉角補上兩個灰色中間色，並以 1×、2×、4× 預覽比較補色前後

▲ 手動反鋸齒：在線段的階梯轉角補上中間色，縮小來看就更圓滑。外輪廓通常不補，因為角色會放在什麼顏色的背景上並不固定。

CHAPTER 06 · MOTION

## 讓像素動起來：逐格動畫與色彩循環

像素動畫的張數通常很少，每個姿勢在畫面上停一段時間。教學裡的走路循環有 2、4、6、8 張的做法，8-bit 時代的日式 RPG 常常重複用同一組 2 張的走路；跑步循環多在 3 到 8 張之間。

Raymond Schlitter（Slynyrd）的動畫教學給了很實用的節奏參考：8 張的跑步循環，每張停 80 毫秒；4 張的，每張停 160 毫秒。片子裡的 16×16 小角色只畫了 4 張，輪流播放就走起來了。

![像素動畫走路循環：16×16 角色只畫 4 張圖，像底片一樣排成一列，輪流播放就在草地上走起來](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/4a1fd019-3b21-4b6f-aeb3-789f0758da56/f94a37cf-c949-4132-8583-aebd58a18a5c.jpg)

像素動畫走路循環：16×16 角色只畫 4 張圖，像底片一樣排成一列，輪流播放就在草地上走起來

▲ 這個 16×16 的小角色只畫了 4 張圖，輪流播放就能走路。走路、跑步循環常用 4～8 張圖。

還有一種更細的動法：只改邊緣和反鋸齒像素的顏色，讓動作看起來比 1 像素還細，例如角色待機時的呼吸起伏。這叫次像素動畫（sub-pixel animation）。

我最喜歡的是色彩循環（color cycling）。像素一格都不動，只把調色盤裡的幾個顏色一圈圈輪轉，像跑馬燈的燈泡，水就開始流、火就開始燒。1985 年的 Deluxe Paint 把它做成內建功能；以這門手藝最為人所知的，是美術家 Mark Ferrari。片頭那道瀑布，就是用這個方法流起來的。

![色彩循環 color cycling 示範：像素瀑布旁是一條循環轉動的調色盤，像素本身不動，只靠轉動調色盤讓水流動](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/ab42e8cf-bf8b-46b1-b590-527b39bf1865/2ccfb164-0e08-4862-bbe6-03caa64fd9fa.jpg)

色彩循環 color cycling 示範：像素瀑布旁是一條循環轉動的調色盤，像素本身不動，只靠轉動調色盤讓水流動

▲ 色彩循環：瀑布的像素一格都沒動，只是把旁邊那條調色盤一圈圈輪轉，水就流起來了。美術家 Mark Ferrari 就以這種技法聞名。

色彩循環在當年的價值很實際：不用多畫一張圖，不用多佔記憶體，一張靜止的畫就有了動態。在我看來，它是整個像素藝術最漂亮的一招，因為它把限制本身變成了動畫的材料。

“

*像素一格都沒動，水卻流起來了。  
把限制想透，限制就會變成畫面。*

想看本人示範，Mark Ferrari 在 GDC 2016 有一場演講，講他的 8-bit 畫法和色彩循環。英文、約 85 分鐘，可以開自動字幕。

![](https://www.youtube.com/watch?v=aMcJ1Jvtef0)

▲ 以色彩循環聞名的美術家 Mark Ferrari，在 GDC 2016 示範他的 8-bit 畫法與色彩循環（英文演講，約 85 分鐘，可開自動字幕）。影片來源：GDC 官方 YouTube 頻道

CHAPTER 07 · GETTING STARTED

## 工具與上手：從 16×16、4 色開始

工具不用多，四選一就能開始。

工具

**費用與授權**

**適合誰**

**Aseprite**

付費，一次買斷；原始碼可自行編譯，但不算開源

認真做像素繪圖＋動畫的人

**Pixelorama**

免費 · 開源（MIT）

零預算入門；Windows、macOS、Linux，瀏覽器也能用

**LibreSprite**

免費 · 開源（GPLv2）

想要免費開源版本的人；它分支自 2016 年的 Aseprite

**Photoshop**

付費訂閱

已經會 Ps、不想換軟體的人：鉛筆工具＋最接近像素

Aseprite 是專門為像素繪圖和動畫做的編輯器，付費、一次買斷（2026 年 9 月官網標價 US$19.99）。它從 2016 年 8 月起改用自有授權，原始碼還能下載、自行編譯，但已經不算開源。LibreSprite 就是從它改授權前最後一版 GPLv2 程式碼分出來的免費開源版本。

預算是零的話，我會先推 Pixelorama：免費、開源（MIT），Windows、macOS、Linux 都能裝，也有瀏覽器版，打開網頁就能畫。

![Pixelorama 介面截圖：深色介面中央是一張像素風夕陽雲朵畫，右側是色輪與圖層面板，下方是調色盤](https://img.itch.zone/aW1nLzE3MTIwMjIxLnBuZw==/original/NTuV%2FB.png)

Pixelorama 介面截圖：深色介面中央是一張像素風夕陽雲朵畫，右側是色輪與圖層面板，下方是調色盤

▲ Pixelorama 的介面：左邊工具列、中間畫布、右邊色輪與圖層，下方是調色盤。圖片來源：Orama Interactive 官方 itch.io 頁面

已經在用 Photoshop 的人不必換軟體。鉛筆工具設成 1 像素，畫出來就是硬邊、沒有反鋸齒；放大超過 500% 時，像素格點預設會出現，也可以從「檢視 → 顯示 → 像素格點」開關。

放大作品時，一律用整數倍（200%、300%、400%），重新取樣選最近鄰。在 Photoshop 裡，它是「影像尺寸」對話框「重新取樣」選單裡的「最接近像素（硬邊）」，Adobe 的說明寫得很直接：最適用於「像素藝術或銳利邊緣圖形」。用 107% 這種非整數倍，像素會變得大小不一；用環迴增值法（雙立方）這類內插，邊緣會糊掉。

像素圖是點陣圖，放大時軟體得替多出來的像素補顏色，選了會混色的補法就會糊；向量圖沒有這個問題。點陣和向量的差別，〈 [設計新手該學哪個軟體？](https://rar.design/posts/which-design-software-for-beginners) 〉有一段講得很清楚。

如果你只想要一個有像素感的標題字，不想一格一格點，Illustrator 也有捷徑：〈 [設計可編輯的像素化文字](https://rar.design/posts/pixelatedtext) 〉用點陣化效果做，字還能改。那是像素風的效果，跟手工擺放的像素藝術是兩條路，看需求選。

工具選好，練法比工具重要。Pedro Medeiros（saint11）的新手教學給了一條很清楚的路：

START HERE · 新手起步的畫布與色數

STEP 01

16×16

4 色 · ARQ4

先畫一顆水果、一個道具。格子少到每一格都得想清楚。

STEP 02

32×32

12 色 · AAP-Micro12

畫布放大一倍，顏色放寬到 12 色，開始練色相偏移。

STEP 03

48×48

16 色 · 慢慢放寬

這 16 色留給你自己挑，可以從 Lospec 找起。每次只放寬一點點。

分享前放大：只用整數倍（200%、300%、400%）＋最近鄰，Photoshop 選「最接近像素（硬邊）」。

16×16 聽起來小得可憐，它會逼你只畫最重要的東西。剛開始很容易犯的錯，就是畫布開太大、顏色放太多，最後又畫回那顆 94 色的櫻桃。

CHAPTER 08 · TAKEAWAYS

## 重點整理：一格一格，畫出整個世界

一分鐘懶人包

→

像素藝術的核心是決定：在有限的格子與顏色裡，刻意決定每一格。自動縮小的 16×16 櫻桃用了 94 色，手工擺放只要 9 色。

→

它起源於記憶體很貴的年代：8×8 圖塊、每個圖塊 3 色＋透明、換色，都是在限制下想出的解法。

→

2010 年代以來像素風越來越流行；2018 年的 HD-2D 把像素角色放進 3D 場景。

→

色相偏移：色階往暗、往亮時連色相一起轉。常見做法是暗部偏冷、亮部偏暖。

→

抖色讓兩色看起來像第三色，省著用；線段長度要規律變化，反鋸齒挑著補，外輪廓通常不補。

→

動畫用少少幾張就夠；色彩循環讓像素不動，只轉調色盤。

→

新手從 16×16、4 色開始；放大只用整數倍＋最近鄰。

回頭看這四十年，每一個技法背後都站著一個限制：顏色不夠，就抖色；記憶體不夠，就換色、轉調色盤；解析度不夠，就一格一格算線段的節奏。今天隨手打開一個繪圖軟體，顏色多到用不完，像素藝術還是請你先挑 4 色。

那個「挑」的動作，就是設計。

打開一張 16×16 的畫布，挑 4 個顏色，放下第一格。那支片裡的整個世界，也是從一格開始的。

RELATED READS · 延伸閱讀

[**Illustrator 點陣圖懷舊造型文字**](https://rar.design/posts/dounthole)

位移複製＋變形＋點陣化，在 Illustrator 裡做出 80 年代電腦畫面那種顆粒感標題字。

[閱讀文章 →](https://rar.design/posts/dounthole)

[**Photoshop 教學像素故障錯位效果**](https://rar.design/posts/valrant)

用風動效果＋遮色片做出像素故障、錯位的畫面，像素美學的另一種玩法。

[閱讀文章 →](https://rar.design/posts/valrant)

[**Photoshop 點陣漸變文字怎麼做？彩色網屏＋錯位效果的東京霓虹感教學**](https://rar.design/posts/photoshop-halftone-gradient-text-tutorial)

景色模糊、彩色網屏、顏色覆蓋三個內建功能，讓文字邊緣化成網點，帶出街機般的復古氛圍。

[閱讀文章 →](https://rar.design/posts/photoshop-halftone-gradient-text-tutorial)

[**配色法則完全指南：6 種色輪配色技巧，讓你的設計不再「怪怪的」**](https://rar.design/posts/color-harmony-guide)

互補、類比、三角、分裂互補、矩形與 60-30-10，挑一組小色盤之前，先懂色輪上的關係。

[閱讀文章 →](https://rar.design/posts/color-harmony-guide)

[**Figma Vectorize：一鍵把點陣圖變向量圖**](https://rar.design/posts/figma-vectorize)

點陣圖和向量圖的另一個方向：Figma 把 PNG、JPG 一鍵轉成可編輯的向量路徑。

[閱讀文章 →](https://rar.design/posts/figma-vectorize)

[**網頁設計的發展歷史：從第一個網頁到 AI 生成的三十五年演變**](https://rar.design/posts/history-of-web-design)

從 1991 年的純文字網頁一路走到今天，用「版面控制權」一條線串起網頁設計三十五年。

[閱讀文章 →](https://rar.design/posts/history-of-web-design)

EXTRA · LINKS · 官方資源

[Aseprite 官網](https://www.aseprite.org/) 、 [常見問題](https://www.aseprite.org/faq/) ：功能介紹、授權方式與價格

[Pixelorama（GitHub）](https://github.com/Orama-Interactive/Pixelorama) 、 [瀏覽器版](https://www.oramainteractive.com/Pixelorama/) ：免費開源，打開網頁就能畫

[LibreSprite 官網](https://libresprite.github.io/) ：GPLv2 授權的免費開源像素編輯器

[Lospec 調色盤列表](https://lospec.com/palette-list) ：4,000 多組免費像素調色盤，可依色數篩選

[Adobe 說明：Photoshop 中的重新取樣選項](https://helpx.adobe.com/tw/photoshop/desktop/crop-resize-transform/resize-adjust-resolution/resampling-options.html) 、 [在 Photoshop 中檢視影像](https://helpx.adobe.com/tw/photoshop/using/viewing-images.html) ：最接近像素與像素格點

[saint11：An Absolute Beginner's Guide](https://saint11.art/pixel_art_articles/article1/) 、 [Basic Color Theory](https://saint11.art/pixel_art_articles/article6/) ：從 16×16、4 色開始的新手練法與色相偏移

[Derek Yu：Pixel Art Tutorial: Basics](https://www.derekyu.com/makegames/pixelart.html) ：線段節奏、抖色、反鋸齒的經典教學

[Slynyrd：Pixelblog 8 – Intro to Animation](https://www.slynyrd.com/blog/2018/8/19/pixelblog-8-intro-to-animation) ：走路、跑步循環的張數與節奏

[Canvas Cycle](https://www.effectgames.com/demos/canvascycle/) ：在瀏覽器裡看 Mark Ferrari 的色彩循環場景

[Goldberg & Flegal〈ACM President's Letter: Pixel Art〉](https://doi.org/10.1145/358728.358731) ：1982 年《Communications of the ACM》原始出處

[Wikipedia：Pixel art](https://en.wikipedia.org/wiki/Pixel_art) 、 [HD-2D](https://en.wikipedia.org/wiki/HD-2D) ：歷史與名詞整理

[任天堂台灣：《歧路旅人》官方頁](https://www.nintendo.com/tw/switch/octopath_traveler/) ：HD-2D 的第一款作品

![100 種 Photoshop 設計的風格](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/91f5ba43-3814-4e87-8074-bb77eecbea1e/679a2bca-5ca5-42f2-bba3-e4901552ca5a.png)

100 種 Photoshop 設計的風格

**COURSE**

[100 種 Photoshop 設計的風格](https://rar.design/courses/ps)

1 像素鉛筆、像素格點、最接近像素放大，Photoshop 也能一格一格畫。像素顆粒、故障錯位這些像素風效果，課程裡都有完整工序。

[**了解課程 →**](https://rar.design/courses/ps)

![AI 覺醒設計應用攻略](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/63e96ac9-97ce-4dfc-be50-154f731f1cc9/e5ca45f0-206a-478f-af1a-96960be47d6a.jpg)

AI 覺醒設計應用攻略

**SUBSCRIPTION**

[AI 覺醒設計應用攻略](https://rar.design/courses/aiwaken)

AI 工具每週都在更新，我幫你過濾雜訊，只留對設計師真正有用的應用。

[**了解訂閱 →**](https://rar.design/courses/aiwaken)

## 常見問題

### Q：像素藝術跟把照片縮小、加馬賽克有什麼不同？

A：差在誰決定每一格。照片自動縮小或轉成像素顆粒，每一格的顏色由演算法算出來，常會擠出一堆混濁的中間色；像素藝術是在有限的格子與顏色裡，由畫的人刻意決定每一格。同樣 16×16 的櫻桃，自動縮小用了 94 色，手工擺放只用 9 色，看起來反而更清楚。

### Q：新手畫像素要從多大的畫布、幾個顏色開始？

A：常見的建議是 16×16、4 色左右，例如 Lospec 上的 ARQ4 調色盤。畫熟了再放大到 32×32、換成 12 色的 AAP-Micro12，之後再慢慢往 48×48、16 色走。畫布小、顏色少，才會逼你只畫最重要的東西。

### Q：像素圖放大後變糊，怎麼辦？

A：只用整數倍放大（200%、300%、400%），重新取樣選最近鄰。Photoshop 在「影像尺寸」的「重新取樣」選單選「最接近像素（硬邊）」。用 107% 這種非整數倍，像素會大小不一；用環迴增值法（雙立方）這類內插，邊緣會糊掉。

### Q：有免費的像素繪圖軟體嗎？

A：有。Pixelorama 免費、開源（MIT），Windows、macOS、Linux 都能裝，也有瀏覽器版；LibreSprite 也是免費開源（GPLv2）。常見的 Aseprite 是付費、一次買斷，原始碼可以自行編譯，但並非開源軟體。

### Q：Photoshop 可以畫像素藝術嗎？

A：可以。鉛筆工具設成 1 像素，畫出來就是硬邊、沒有反鋸齒；放大超過 500% 時像素格點預設會出現，也能從「檢視 → 顯示 → 像素格點」開關。畫完用「最接近像素（硬邊）」以整數倍放大。

### Q：8-bit、16-bit 到底是什麼意思？

A：大致對得上主機 CPU 的位元數：紅白機用 8 位元 CPU，超任用 16 位元 CPU。但這個對應並不嚴格，它也是當年的行銷標籤，例如 TurboGrafx-16 的 CPU 其實是 8 位元，名字卻掛 16。把它當成時代的暱稱比較準確。

### Q：HD-2D 是什麼？

A：HD-2D 是 Square Enix 從 2018 年《歧路旅人》開始使用的風格名稱：像素角色放進 3D 場景，加上動態光影、景深和移軸效果，看起來像微縮模型。之後的 Triangle Strategy（2022）、Dragon Quest III 的 HD-2D 重製版（2024）也用了這個風格。