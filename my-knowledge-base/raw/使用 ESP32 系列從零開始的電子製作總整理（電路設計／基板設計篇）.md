---
title: "使用 ESP32 系列從零開始的電子製作總整理（電路設計／基板設計篇）"
source: "https://codelove.tw/@tony/post/a9yvBa"
author:
published:
created: 2026-09-01
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言=============================================最近，我迷上了用 ESP32 做電子製作，所以想根據自己做過幾個雜七雜八作品的經驗，介紹一下使用 ESP32 進行大致..."
tags:
  - "clippings"
status: "processed"
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月31日](https://codelove.tw/@tony/post/a9yvBa "2026-08-31 08:06:10")

## 使用 ESP32 系列從零開始的電子製作總整理（電路設計／基板設計篇）

## 前言

最近，我迷上了用 ESP32 做電子製作，所以想根據自己做過幾個雜七雜八作品的經驗，介紹一下使用 ESP32 進行大致上的電子製作方法。

## 什麼是 ESP32

ESP32 系列是由中國公司 Espressif Systems 開發的微控制器系列。很多機型都內建 Wi-Fi 或藍牙模組，利用這些功能可以很容易做出能連接周邊裝置並進行某些操作的電子作品。  
以前大多數機型只能透過 UART 進行程式燒錄（大概是這樣），但最近能直接透過 USB 連接電腦等裝置並燒錄程式的機型已經大幅增加。  
至少筆者認為，大多數 ESP32 系列不但 Flash 容量不像 Arduino UNO 那麼少，還支援 UART、SPI、I2C 等主要通訊協定，也內建 PWM、ADC 等功能，算是相當好用。（看起來沒有 DAC）

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228687&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa9yvBa&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227619514&amp;bpp=3&amp;bdt=2638&amp;idt=3&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788227622%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905&amp;nras=3&amp;correlator=6012514477548&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1087&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774&amp;oid=2&amp;pvsid=1874362402046663&amp;tmod=496159329&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

前言就先說到這裡。。。

## 硬體設計

## 微控制器選型

ESP32 有幾個不同的型號，各自有不同特點。（以下是筆者主觀感受）

- ESP32-S：高效能，適合想接攝影機模組或做邊緣運算的人
- ESP32-C：效能比 S 系列低一些，但兼具低功耗與 Wi-Fi、藍牙
- ESP32-H：無線功能只有藍牙，但據說比 C 系列更省電
- ESP32：從流行初期就存在的型號，大概是設計資源最多、社群也最大

筆者主要會使用 ESP32-C 系列與 H 系列較新的型號（例如 ESP32-C3、ESP32-H2），原因是可以直接透過 USB 燒錄程式。（若是 UART 方式，則需要另外準備 USB-UART 轉換電路之類的東西，覺得很麻煩）

以下這類電子作品，大多用 C 系列或 H 系列就足夠了。筆者也大致只做過這種程度的作品而已。

- LED 閃爍
- 簡單的模組控制（液晶模組、感測器模組等）

## 電路設計

這裡主要介紹適用於 C 系列與 H 系列的 ESP 周邊電路設計。  
（如果你覺得這些都不需要，反正有現成開發板就好，那建議直接上一頁）

### 腳位配置

C 系列與 H 系列中有不少型號內建了晶體振盪器（不確定是不是全部，需自行確認），基本上只要至少正確接好以下幾項，應該就能先運作。（筆者的環境下是能動的）

- 3.3V
- GND
- EN 腳
- USB（用於燒錄程式）
- Boot Select 腳

[![Screenshot 2026-08-24 at 22.46.05.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Ff2f0513c-a1bc-4f3f-b93e-74b7dbc923b4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=01c05630e1a519a8546d0b4ad4061114)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Ff2f0513c-a1bc-4f3f-b93e-74b7dbc923b4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=01c05630e1a519a8546d0b4ad4061114)

下面是 **ESP32-H2-MINI-1** 的腳位配置，但不同系列與型號的腳位分配可能會有些微差異，要注意。  
另外，這個型號是類似 BGA 的封裝，沒有錫膏與熱板的話很難實裝。

[![Screenshot 2026-08-24 at 22.55.28.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fe6cf88e3-12c7-4ca8-ab20-b98feb062068.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=91316ab6a5864607dc1dfbf28602ee08)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fe6cf88e3-12c7-4ca8-ab20-b98feb062068.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=91316ab6a5864607dc1dfbf28602ee08)

這裡是 **ESP32-C3-WROOM-02** 這個型號，由於腳位焊盤是從封裝側面露出來的類型，也不是不能用烙鐵來實裝。

[![Screenshot 2026-08-24 at 23.04.11.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F2decd284-b95a-44ed-a05a-74b243b85e86.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=988cbd6959f8f0958e7aa3d55cbf0433)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F2decd284-b95a-44ed-a05a-74b243b85e86.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=988cbd6959f8f0958e7aa3d55cbf0433)

回到正題， **ESP32-H2-MINI-1** 的腳位配置詳細如下。↓  
特別是 SPI 的標示不太容易看懂，但若換成一般 4 線式 SPI，對應如下。  
（大概是 Quad SPI 的標示？）

| 周邊標示 | 4 線式 SPI |
| --- | --- |
| FSPICLK | SPI CLK |
| FSPID | SPI MOSI |
| FSPIQ | SPI MISO |
| FSPICSx | SPI CS |

[![Screenshot 2026-08-24 at 23.07.22.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F89a78e83-56d1-4660-8238-43f2e3d949dd.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fc375c39226de3f64ed443dcd8c066e8)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F89a78e83-56d1-4660-8238-43f2e3d949dd.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fc375c39226de3f64ed443dcd8c066e8)

其他像 I2C、PWM、GPIO 等周邊，似乎可以在一定程度上自由分配腳位。不過 ADC、32.768kHz 晶體振盪器、UART、SPI、USB 等可分配的腳位好像是固定或有限的，要注意。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228718&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa9yvBa&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227619550&amp;bpp=1&amp;bdt=2675&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788227622%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=8&amp;correlator=6012514477548&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6892&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=3344&amp;eid=95399629%2C95400774&amp;oid=2&amp;psts=AOrYGsl1BhDmg3gWPnRhfOnDlG_3mMzKNO3PA8NJezaORbVGHKdcQ0lICnbh2iGhAesMz0erw2TzWOOMcEBtZVasg64bCBxu8DzCo91xxVvyDGd7qOxTdR0d87tkQ15So_wWQyo%2CAOrYGsnY0upA2eM3IMYq7GUjzKh2nDKVgEHukDUO7ajE-d06cMZHy9OHRm2coBn9Ic4d3tv-14hkAAnbj32Lm5w92qi22WpeuNkgk2KLDlM%2CAOrYGsl_Kzg4eJH3ISrmRXX84asuMqavjuo15jfO1uqxKr8fNkB4Q-WnYTJyQ6WnyUI4ml1n_j1tm7RikjWZNUlF-AUwy6GBk1NKo3FFb399srY&amp;pvsid=1874362402046663&amp;tmod=496159329&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

### EN 腳（重置腳）

跟其他微控制器一樣，EN 腳（重置腳）周邊也必須妥善設計。（雖然不需要搞得非常複雜，但如果太隨便，可能會無法正常啟動）  
ESP32 的 EN 腳需要從外部以電源電壓等方式做 pull-up，而且必須設計成符合以下的上電時序。（邏輯當然是低有效）

[![Screenshot 2026-08-25 at 23.16.20.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F205b0770-3455-4e5f-be62-a3122dd12b4d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=9fb987b02c9c0fe94e00012c6072f2e7)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F205b0770-3455-4e5f-be62-a3122dd12b4d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=9fb987b02c9c0fe94e00012c6072f2e7)

不同型號可能會有些微差異，但以 **ESP32-H2-MINI-1** 來說，電源電壓上升後至少需要等待 tSTBL = 50us。  
如果將上拉電阻設為 10kΩ，並在 GND 間接上 0.1uF～1uF 左右的電容，應該就足以滿足這個條件。  
下面是電路示意。這個例子雖然標示為不實裝，但也有放一顆重置 IC（❌ 標記）。  
我覺得這算是鐵則，EN 與 GND 間最好再加一個小按鈕，這樣也能手動重置，很方便。

[![Screenshot 2026-08-25 at 23.33.00.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fe11a998e-0314-4634-b4d7-c6432a580261.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5e7f4ff70598992279024e2b90c04363)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fe11a998e-0314-4634-b4d7-c6432a580261.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5e7f4ff70598992279024e2b90c04363)

### Boot Select 腳

ESP32 有兩個 boot select 腳。  
在 ESP32-H 系列中，GPI08 與 GPIO9 被分配為 Boot Select 腳。（不同系列可能不同，需確認）  
如下表所示，透過控制啟動時 GPIO8 與 9 的狀態，可以用兩種 Boot mode 開機。  
**SPI Boot** 模式會啟動寫入 ESP32 的程式（更精確地說，是從內部透過 SPI 連接的快閃記憶體讀出程式後啟動）。  
相對地， **Joint Download Boot** 模式則是從 USB 或 UART 下載程式後再啟動的模式。

[![Screenshot 2026-08-29 at 16.49.23.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F07de3322-3671-4fe5-b7e1-ae409b02bee4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=218bc0be93fae43c058d17d7f1c9682e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F07de3322-3671-4fe5-b7e1-ae409b02bee4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=218bc0be93fae43c058d17d7f1c9682e)

先看 GPIO8，其應有的邏輯為 **Any value** 或 **1** 。因此，GPIO8 用 10kΩ 左右上拉是合理的。  
另一方面，GPIO9 基本上維持在 1（上拉）應該也沒問題。你可能會以為正常開機與燒錄程式時必須把 GPIO9 在 1 ↔ 0 間切換，但至少依照原廠建議的燒錄方式，內建的 USB-Serial-JTAG 控制器會在內部自動把燒錄模式切換成 **Joint Download Boot** 。（燒錄方式等內容會在別的文章介紹）不過，由於各種因素，有時這個燒錄模式切換可能不順利，所以最好還是準備一個可手動拉低的開關。  
電路大致如下：

[![Screenshot 2026-08-29 at 17.40.33.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fce6662cf-14c4-450f-808f-80cec327539d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5699d5ae5b86e1a76131723b634f8722)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fce6662cf-14c4-450f-808f-80cec327539d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5699d5ae5b86e1a76131723b634f8722)

### 電源腳

跟其他廠牌的微控制器一樣，電源腳除了正常的 3.3V 之外，還有一個看起來是供電池使用的 VBAT 腳。不過至少在 ESP32-H 系列與 C 系列中，這兩個電源腳在金屬屏蔽罩內部是用 0Ω 跳線電阻直接相連的。雖然不知道 VBAT 腳存在的意義是什麼，但 3.3V 的供電只要接到 3.3V 腳即可。  
電路如下。查了一下據說 ESP 模組內部也已經有 3.3V - GND 間的去耦電容，不過為了穩定運作，再額外接一些去耦電容也沒問題。

[![Screenshot 2026-08-29 at 18.04.28.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F7f05b77d-cb05-4e62-a2c4-90ea05a2f738.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=9893e421f2d1b2453ee743f62dee778e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F7f05b77d-cb05-4e62-a2c4-90ea05a2f738.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=9893e421f2d1b2453ee743f62dee778e)

### USB

USB 支援到 2.0 Full-Speed（12Mbps）。ESP 端的 USB 腳位是固定對應到特定腳位，只要注意不要把 +/− 接反即可。

[![Screenshot 2026-08-29 at 18.09.54.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fd325a501-77fd-4cca-aacc-856e69c4ff19.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b676693db21e3f7f9fec7a04f7b51a90)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fd325a501-77fd-4cca-aacc-856e69c4ff19.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b676693db21e3f7f9fec7a04f7b51a90)

USB 端的設計我認為還是要稍微認真做一下比較好。雖然隨便設計有時候也可能能用。筆者通常會注意以下幾點：

- 在 USB+/- 與 GND 間接上用於 ESD 保護的壓敏電阻（RV1、RV2）
- 在電路 GND 與 USB 連接器的機殼地（UGND）之間，透過磁珠（FB1）與 1MΩ（R35）相連  
	→ 在連接機殼與 GND 的同時，抑制高頻雜訊
- 在 VBUS 路徑上配置保險絲開關（F1）（選用約 500mA 會動作的型號）
- 在 CC1、CC2 腳接上 5.1kΩ（R28、R30）  
	→ 這是做 PD 協商用的腳位，如果不接 5.1kΩ，有時無法從 VBUS 正確供電
- 在 USB+/- 路徑上配置 22Ω 的阻尼電阻（R29、R31）

[![Screenshot 2026-08-29 at 18.19.11.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fd371357e-943d-4ad7-ac96-23ea221113f7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=042aba1933fe9939ba7d9d3e388db42a)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fd371357e-943d-4ad7-ac96-23ea221113f7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=042aba1933fe9939ba7d9d3e388db42a)

另外，也可以將 VBUS 接到 5V → 3.3V 轉換電路，作為 ESP32 的電源。  
下面這個例子用了比較貴的切換式穩壓器，不過市面上也有很多 100～300 日圓左右就能買到的切換式穩壓器，因此用那些來設計電路也可以。

[![Screenshot 2026-08-29 at 19.46.20.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F0fb0e50f-f27a-4b6c-853d-855022b51178.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3f0bc86f9821ce9e93714013fa6d4d81)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F0fb0e50f-f27a-4b6c-853d-855022b51178.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3f0bc86f9821ce9e93714013fa6d4d81)

到這裡為止，只要把電路設計好，就能做出可用 USB 供電驅動 ESP，並且可從 USB 燒錄程式的電路圖。

### 其他

如果要使用 SPI、I2C、PWM 等功能，就要邊參考資料表邊進行設計。  
上述說明與圖片大多引用自以下文件。

↓ESP32-H2-MINI-1 資料表

## 基板設計

這裡介紹在基板設計上比較值得特別注意的地方。

### ESP32 周邊

例如使用 ESP32-H2-MINI-1 時，基板焊盤大致如下。  
標有 **Antenna Area** 的部分與內建天線區域重疊，因此應避免走線與鋪銅（包含 GND 大面積銅箔）。KiCad 提供的 footprint 會把該區域設成禁佈區，因此不容易誤放。  
另外，這個型號是類似 BGA 的 SMD 封裝，所有端子都配置在晶片背面。要實裝幾乎必須有 **錫膏** 與 **熱板** 或 **熱風槍** 。向 JLCPCB 等廠商訂製基板時，也建議一併訂購用來精準塗佈錫膏的 **鋼網** 。  
端子尺寸與 pitch 都很小，約 0.2～0.3mm 寬的走線剛剛好。以下範例為了省基板費，硬塞在雙層板上；但如果想做得比較正式，我認為 4 層以上會比較好。

[![Screenshot 2026-08-29 at 19.54.39.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F7262f085-45fc-4580-9dcc-6f847f43233b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=02316b9bc236040d9edb42de0f37a312)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2F7262f085-45fc-4580-9dcc-6f847f43233b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=02316b9bc236040d9edb42de0f37a312)

### USB 周邊

USB 周邊大致如下。這個例子中，黃色標示的 USB+/- 走線盡量做得短。若很難縮短，通常就讓 USB+/- 兩端被 GND 大面積銅箔包圍，這是常見做法。（雖然因為只有 Full Speed 12Mbps，可能不必太過神經質。）  
另外 VBUS 與 GND 則可依耗電電流設計成適當的線寬。

[![Screenshot 2026-08-29 at 20.10.52.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fac0357f9-7b12-4b13-a69f-f845167e57e9.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=edc5a4e64550b2410870b7c7267cb474)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Fac0357f9-7b12-4b13-a69f-f845167e57e9.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=edc5a4e64550b2410870b7c7267cb474)

### 其他

Boot Select 與 EN 腳周邊的電路（開關等）沒有什麼特別需要注意的地方，所以這裡省略。。。

## 最後

前面寫了這麼多，其實只要去買像秋月電子也有賣的這類開發板，就不需要自己搞這些事情，全部都能解決。  
↓ESP32-H2-MINI-1 開發板

[![Screenshot 2026-08-29 at 21.44.32.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Faf3d179a-5749-4cef-94b6-4d194bda5e41.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=80ee02fe8bb0071321f3a271f678dc07)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F513034%2Faf3d179a-5749-4cef-94b6-4d194bda5e41.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=80ee02fe8bb0071321f3a271f678dc07)

如果你想連基板尺寸、概念與各種細節都完全自行自由設計，那麼不妨試試看。

另外，這篇的續篇（開發環境建置、實機運作）我也整理成後篇，放在以下。

---

原文出處： [https://qiita.com/\_shin\_/items/47bc69006f6467d79e91](https://qiita.com/_shin_/items/47bc69006f6467d79e91)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/a9yvBa) [12瀏覽](https://codelove.tw/@tony/post/a9yvBa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/a9yvBa)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝15 💬4

206

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788227618&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa9yvBa&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227617595&amp;bpp=3&amp;bdt=690&amp;idt=812&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787904025%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787904025%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787904025%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6012514477548&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774&amp;oid=2&amp;pvsid=1874362402046663&amp;tmod=496159329&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=863" title="Advertisement" aria-label="Advertisement"></iframe>