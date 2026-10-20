---
title: "Sprite Image 是什麼？從遊戲角色動畫到網頁應用，一次認識 Sprite 圖片 | 教學與資訊"
source: "https://tools.wingzero.tw/articles/4040"
author:
  - "[[赤紅翼零]]"
published: 2026-08-28
created: 2026-09-01
description: "Sprite Image 是 2D 遊戲與網頁動畫常見的圖像元素，透過多個連續影格組成 Sprite Sheet，即可製作走路、跑步、攻擊等逐格動畫。本文介紹 Sprite、Sprite Sheet 與 Sprite Animation 的差異、FPS、透明背景、Anchor Point，以及使用 AI 生成 Sprite 時需要注意的角色一致性與動作連續性。"
tags:
  - "clippings"
status: "processed"
---
在遊戲開發、網頁設計或動畫製作中，經常會看到 **Sprite、Sprite Image、Sprite Sheet** 等名詞。尤其是 2D 遊戲中的角色走路、跑步、攻擊、跳躍等動作，看起來像是一段動畫，實際上很多時候並不是播放影片，而是快速切換一張張靜態圖片所形成的效果。

而這些圖片，就是 Sprite 動畫最重要的基礎。

## 什麼是 Sprite Image？

Sprite Image（精靈圖像）通常指可以獨立顯示、移動或產生動畫效果的 2D 圖像元素。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=4116832479&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788224759&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4040&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788224744798&amp;bpp=3&amp;bdt=929&amp;idt=3&amp;shv=r20260825&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788224542%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788224542%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788224542%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280&amp;nras=3&amp;correlator=8712188835943&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=3114&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400779%2C42533293%2C122880743%2C122880333&amp;oid=2&amp;pvsid=1501614659937333&amp;tmod=684280329&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=0&amp;ifi=12&amp;uci=a!c&amp;btvi=2&amp;fsb=1&amp;dtd=14487" title="Advertisement" aria-label="Advertisement"></iframe>

在遊戲中，一個角色、怪物、道具、特效甚至 UI 圖示，都可以是一個 Sprite。例如一個遊戲角色站在畫面上，程式可以控制 Sprite 的位置、大小、旋轉角度與透明度，讓角色在遊戲世界中移動。

如果角色需要產生動作，則可以準備多張不同姿勢的 Sprite，再按照一定速度依序播放。

例如一個簡單的跑步動畫可能包含：

**右腳向前 → 身體下降 → 左腳向前 → 身體上升 → 回到右腳向前**

當這幾張圖片以每秒數張甚至十幾張的速度連續切換，人眼就會將它們視為連續的跑步動作。

這個概念與傳統動畫中的「 **逐格動畫（Frame-by-Frame Animation）** 」非常接近。

## Sprite 與 Sprite Sheet 有什麼不同？

Sprite 與 Sprite Sheet 經常被混在一起使用，但兩者實際上代表不同的概念。

### Sprite

**Sprite** 通常指單一的 2D 圖像元素。

例如角色跑步過程中的其中一個姿勢，就可以是一個 Sprite。

除了人物之外，遊戲中的怪物、武器、寶箱、按鈕、圖示、火焰與魔法特效，也都可能使用 Sprite。

### Sprite Sheet

\*\*Sprite Sheet（精靈圖表）\*\*則是把多個 Sprite 或動畫影格排列在同一張大型圖片裡。

例如一個角色具有 8 個跑步影格，可以將它們按照順序排列：

`Frame 1 → Frame 2 → Frame 3 → Frame 4 → Frame 5 → Frame 6 → Frame 7 → Frame 8`

最後輸出成一張包含 8 個格子的圖片，這張圖片就是 Sprite Sheet。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=1429272977&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788224763&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4040&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788224744813&amp;bpp=3&amp;bdt=943&amp;idt=3&amp;shv=r20260825&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788224542%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788224542%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788224542%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280%2C832x280&amp;nras=4&amp;correlator=8712188835943&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=4712&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1103&amp;eid=95399629%2C95400779%2C42533293%2C122880743%2C122880333&amp;oid=2&amp;psts=AOrYGsmhkxU323fdGQMTf39SOlDa53w4jBJjPNScy2lJF2gsrMK0jOysTJdlI-V7lsYOXANhUAwvwHT_GAMiO6CtP2oj78vS98nfahF9jCm6S0s-T4o%2CAOrYGslZY4S7x385JNO4sZtzLfv1AuIc5VlziHfp0aDLtYE-gN6--KP0-PL4ONBiNex7F5KprlwkgcK4O9_TFetiD-v6yM_rVqFLo0IUIqCIVGLDjV8&amp;pvsid=1501614659937333&amp;tmod=684280329&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=1.15&amp;ifi=13&amp;uci=a!d&amp;btvi=3&amp;fsb=1&amp;dtd=18264" title="Advertisement" aria-label="Advertisement"></iframe>

程式播放動畫時，不需要一次顯示整張 Sprite Sheet，而是透過座標裁切出其中一個區域，再依照時間快速切換到下一個區域。

如此一來，就能產生角色持續跑步的動畫效果。

## 為什麼要把多張圖片合成 Sprite Sheet？

乍看之下，把每一個動作分別儲存成獨立 PNG 圖片似乎更加直覺，但 Sprite Sheet 在遊戲與網頁開發中仍然具有不少優點。

### 減少圖片載入請求

Sprite Sheet 最傳統的用途之一，就是減少大量小圖片造成的載入請求。

尤其在早期網頁環境中，如果一個頁面需要載入數十甚至數百張小圖片，每張圖片都需要分別向伺服器提出請求，可能影響整體載入效率。

將圖片整合成 Sprite Sheet 後，就可以一次載入完整圖片，再透過程式或 CSS 顯示其中需要的部分。

### 集中管理角色動畫

另一個優點則是方便整理動畫素材。

例如一張完整的角色 Sprite Sheet 可以安排成：

- 第一排：待機（Idle）
- 第二排：走路（Walk）
- 第三排：跑步（Run）
- 第四排：跳躍（Jump）
- 第五排：攻擊（Attack）
- 第六排：受傷（Hit）

如此一來，一張 Sprite Sheet 就可以包含角色的大部分基本動畫。

現代遊戲引擎已經可以有效管理大量獨立圖片，因此 Sprite Sheet 不再單純只是為了減少 HTTP Request，但它仍然是 2D 遊戲、Pixel Art 與逐格動畫非常常見的素材格式。

## Sprite Animation 是怎麼動起來的？

Sprite 本身其實不會動。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=3975518427&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788224773&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4040&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788224744822&amp;bpp=1&amp;bdt=953&amp;idt=2&amp;shv=r20260825&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788224542%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788224542%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788224542%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280%2C832x280%2C832x280&amp;nras=5&amp;correlator=8712188835943&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=6192&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2574&amp;eid=95399629%2C95400779%2C42533293%2C122880743%2C122880333&amp;oid=2&amp;psts=AOrYGsmhkxU323fdGQMTf39SOlDa53w4jBJjPNScy2lJF2gsrMK0jOysTJdlI-V7lsYOXANhUAwvwHT_GAMiO6CtP2oj78vS98nfahF9jCm6S0s-T4o%2CAOrYGslZY4S7x385JNO4sZtzLfv1AuIc5VlziHfp0aDLtYE-gN6--KP0-PL4ONBiNex7F5KprlwkgcK4O9_TFetiD-v6yM_rVqFLo0IUIqCIVGLDjV8%2CAOrYGsnwwX-JkcaFI_nfmPbMUwtdoyDMnXAY2VU3FYQzlrCnnMkZAnuzkSZsovzaeEjBjlCy9KI_uxfrvHeF84M-qkpUq2PWa2Sp7g2e4v53UXPqHQ&amp;pvsid=1501614659937333&amp;tmod=684280329&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=1.15&amp;ifi=14&amp;uci=a!e&amp;btvi=4&amp;fsb=1&amp;dtd=28418" title="Advertisement" aria-label="Advertisement"></iframe>

真正產生動畫效果的是「 **依序切換影格** 」。

假設一個跑步動畫共有 8 格：

`1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 回到 1`

程式只要按照固定時間切換顯示的 Sprite，就可以形成循環動畫。

### FPS 會影響 Sprite 動畫速度

其中一個重要參數就是 **FPS（Frames Per Second，每秒影格數）** 。

例如：

**6 FPS**

每秒播放 6 個影格，會具有比較明顯的逐格動畫感。

**12 FPS**

動作會更加自然，也是許多 2D 動畫常見的速度。

**24 FPS**

視覺效果可以更加流暢，但通常也需要準備更多動畫影格。

不過，FPS 越高並不代表動畫一定越好。

如果只有 6 張不同姿勢，即使設定成很高的 FPS，也只是更快速地重複播放這些圖片。

真正影響 Sprite Animation 流暢程度的關鍵，仍然是每個影格之間的 **動作連續性** 。

## 製作 Sprite Sheet 時要注意什麼？

製作 Sprite Sheet 時，很重要的一點就是每一格必須具有一致的規格。

例如製作一組角色跑步動畫時，最好讓每個 Frame 都保持：

- 相同圖片尺寸
- 相同角色比例
- 相同角色中心位置
- 相同視角
- 相同服裝與外觀
- 一致的光影方向
- 一致的背景設定

如果角色在每一格的位置不一致，播放動畫時就可能出現角色上下左右抖動的現象。

例如第一格角色中心位於 X=250，第二格突然變成 X=280，第三格又變成 X=240，即使角色本身的姿勢畫得很好，播放後仍然會產生明顯的跳動。

因此製作 Sprite Animation 時，除了角色本身的動作之外， **Anchor Point（錨點）的一致性** 也非常重要。

## Sprite 為什麼經常使用透明背景？

遊戲中的角色通常需要出現在不同場景，因此 Sprite 圖片很少直接包含完整背景。

比較常見的方式，是使用具有 \*\*Alpha Channel（透明通道）\*\*的 PNG、WebP 或其他支援透明度的圖片格式。

如此一來，同一個角色 Sprite 就可以直接放到不同場景，例如：

- 森林
- 城市
- 室內
- 沙漠
- 雪地
- 地下城
- 網頁介面

而不需要為每一個背景重新製作角色圖片。

這也是為什麼製作角色 Sprite Sheet 時，通常會要求使用 **透明背景** 。

## Sprite Image 有哪些常見應用？

Sprite 最容易讓人想到 2D 遊戲，但實際上的應用範圍相當廣泛。

### 遊戲角色動畫

這是 Sprite Animation 最常見的用途之一。

角色的走路、跑步、跳躍、攻擊、跌倒、揮手、跳舞等動作，都可以利用多個 Sprite Frame 組成動畫。

### 遊戲特效

火焰、爆炸、煙霧、水花、魔法、閃電、光線等視覺效果，同樣可以使用 Sprite Animation 製作。

例如一個爆炸效果可能只存在不到一秒，但實際上是由十幾張不同階段的爆炸圖片快速播放而成。

### UI 與 Icon

Sprite 也曾經大量應用在網頁 UI。

早期網站經常把許多小圖示放在同一張 **CSS Sprite** 中，再透過 CSS 的 `background-position` 顯示指定區域。

這樣就可以使用一張圖片提供大量不同的 UI 圖示。

### 網頁角色動畫

網站上的吉祥物、互動角色、Loading Animation、小型動畫或遊戲化效果，也可以透過 Sprite Sheet 實作。

### Pixel Art 像素藝術

Sprite 與 Pixel Art 的關係尤其密切。

許多像素風格遊戲中的角色、敵人、NPC、道具與環境動畫，基本上都是由大量 Sprite 所組成。

## AI 生成圖片也可以拿來製作 Sprite

生成式 AI 的發展，也讓 Sprite 動畫出現了新的製作方式。

過去如果要製作一組角色跑步動畫，通常需要由繪師逐格繪製：

`Pose 1 → Pose 2 → Pose 3 → Pose 4 → Pose 5……`

現在則可以利用 AI 圖像生成模型，先提供角色參考圖片，再要求 AI 生成同一個角色不同階段的連續動作。

例如可以要求 AI：

> 以參考角色為基礎，製作 8 格完整跑步循環動畫，每格角色外觀、服裝、比例與視角保持一致，手腳按照跑步週期自然前後擺動，每格尺寸一致，角色固定在畫面中央，使用透明背景。

生成完成後，再將各個影格切割、整理與校正，就可以進一步製作成 Sprite Sheet。

## 使用 AI 製作 Sprite 最大的難題

使用 AI 製作 Sprite 時，最大的問題通常不是「圖片畫得好不好」，而是 **動作是否具有連續性** 。

AI 很容易生成「8 張正在跑步的人」，但這並不代表它們就是「8 個連續的跑步動畫影格」。

兩者有很大的差異。

### Sprite 動畫需要時間連續性

真正的 Sprite Animation 必須考慮動作週期。

例如跑步動畫通常會經過：

**Contact → Down → Passing → Up → Contact**

接著換另一隻腳重複相同週期。

因此，如果 AI 生成的每一格只是隨機改變手腳姿勢，播放後可能出現：

- 左右腳突然交換
- 手臂瞬間改變位置
- 身體高度不連續
- 頭部大小發生變化
- 角色位置左右跳動
- 衣服或配件突然改變
- 人體動作不符合正常運動邏輯

結果就是每一張圖片單獨看都很好看，但串起來後完全不像動畫。

所以使用 AI 製作 Sprite 時，Prompt 最重要的不只是描述「角色正在跑步」，而是要讓模型理解：

**這是一組具有明確時間先後關係的連續動畫影格，而不是數張獨立插圖。**

## 一張好的 Sprite Sheet 應該具備哪些條件？

一組適合實際拿來製作動畫的 Sprite Sheet，通常應該具備以下幾個基本條件。

### 角色外觀一致

每格人物的臉、髮型、服裝、身材與整體角色設計都應該保持一致。

### 每格尺寸一致

所有 Frame 應使用相同的寬度與高度，例如每格固定為 512 × 512 px。

這也能讓程式更加容易計算每個 Sprite 的裁切位置。

### 角色位置一致

角色的中心點或腳底位置最好固定，避免動畫播放時產生不必要的位移。

### 動作具有連續性

每一個 Frame 都應該是上一個 Frame 的自然延續，而不是完全獨立的姿勢。

### 影格排列順序明確

Sprite Sheet 最好按照固定方向排列，例如：

**由左至右 → 換行 → 再由左至右**

這樣程式與遊戲引擎就能更容易解析動畫順序。

### 使用透明背景

如果 Sprite 是人物、角色、物件或特效素材，通常建議使用透明背景，方便套用到不同遊戲或網頁場景。

### 首尾影格能自然銜接

如果製作的是循環動畫，最後一格必須能自然回到第一格。

例如：

`1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 1`

如果第 8 格與第 1 格的姿勢差異太大，每次動畫重新開始時就會產生明顯跳動。

這一點對 **Walk Cycle、Run Cycle、Idle Animation** 等循環動畫尤其重要。

## Sprite Image 在現代開發中還重要嗎？

即使現在已經有 WebGL、3D 遊戲引擎、骨架動畫、影片素材與各種即時渲染技術，Sprite 仍然沒有消失。

從復古 Pixel Art 遊戲、獨立遊戲、手機遊戲、網頁互動，到大型遊戲中的 2D UI、粒子效果與視覺特效素材，都仍然可以看到 Sprite 的應用。

而生成式 AI 的出現，也讓 Sprite Image 有了新的製作方式。

過去需要逐格手繪的角色動畫，現在可以先利用 AI 建立角色與動作草稿，再進一步進行切割、校正、去背、補幀與整理，最後製作成能夠實際使用的 Sprite Sheet。

因此，Sprite 並不是一種已經被淘汰的舊技術，而是一種非常基礎、簡單且至今仍相當實用的 2D 圖像與動畫表現方式。

只要理解：

**一張 Sprite 是一個畫面元素，多個具有時間連續性的 Sprite Frame 按照順序播放，就可以形成 Sprite Animation。**

掌握這個基本概念之後，也就掌握了 Sprite 動畫最核心的運作原理。

<iframe frameborder="0" src="https://9297db3b0a3317e214423b682aed3f1c.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="250" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe><iframe frameborder="0" src="https://9297db3b0a3317e214423b682aed3f1c.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="250" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>