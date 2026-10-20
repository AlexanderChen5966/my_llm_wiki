---
title: "Native、Web、PWA、Hybrid App 有什麼不同？一次搞懂四種 App 開發方式 | 教學與資訊"
source: "https://tools.wingzero.tw/articles/4054"
author:
  - "[[赤紅翼零]]"
published: 2026-09-07
created: 2026-09-08
description: "Native App、Web App、PWA、Hybrid App 有什麼差別？本文比較四種 App 開發方式的執行架構、跨平台能力、原生功能與適用情境，並介紹如何透過 Capacitor 將 Web App 打包成 iOS、Android App，幫助你依照效能、開發成本與產品需求選擇合適的 App 技術。"
tags:
  - "clippings"
status: processed
---
在開始開發 App 之前，經常會遇到幾個名詞： **Native App、Web App、PWA 與 Hybrid App** 。

它們看起來都能製作「應用程式」，但實際上在執行方式、安裝方式、跨平台能力，以及能否使用手機原生功能等方面，都有很大的差異。

如果簡單從與手機作業系統的整合程度來看，可以先建立這樣的概念：

**Web App → PWA → Hybrid App → Native App**

越往右，通常代表與 iOS、Android 等手機作業系統的整合程度越高。不過這並不是單純的優劣排名，而是不同的開發架構，各自適合不同的需求。

## Native App：直接使用手機原生技術開發

Native App（原生 App）是針對特定作業系統，直接使用該平台提供的技術與開發工具製作的應用程式。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="837" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=4270348415&amp;adf=199514577&amp;abgtt=6&amp;w=837&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788829673&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=837x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4054&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=836&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788829622163&amp;bpp=1&amp;bdt=6878&amp;idt=1&amp;shv=r20260902&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788829582%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788829582%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788829582%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1522x969%2C789x280&amp;nras=3&amp;correlator=3530933270953&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=169&amp;ady=2725&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400775%2C122880834%2C95400618%2C122880333%2C95340253%2C95340255&amp;oid=2&amp;pvsid=289262645472135&amp;tmod=1169943733&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=157x763_l%7C157x763_r&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=2&amp;fsb=1&amp;dtd=50850" title="Advertisement" aria-label="Advertisement"></iframe>

例如開發 iPhone App，通常會使用 Swift、SwiftUI 與 Xcode；Android 則可以使用 Kotlin、Jetpack Compose 與 Android Studio。

Native App 最大的特色，就是可以直接與作業系統整合，因此 Camera、GPS、Bluetooth、Push Notification、Haptics、Face ID 等手機功能，都能獲得完整的支援。

在效能、動畫流暢度與系統整合方面，Native App 通常也是最完整的方案。

但它的缺點是開發成本較高。如果一個產品同時要推出 iOS 與 Android 版本，通常需要針對不同平台進行開發與維護。

**Swift / Kotlin → Native UI → iOS / Android**

因此，需要高效能、大量使用手機硬體，或對原生使用體驗要求較高的 App，通常比較適合採用 Native App。

## Web App：在瀏覽器裡執行的應用程式

Web App（網頁應用程式）本質上仍然是一個網站，只是介面與操作方式可以設計得非常接近一般 App。

它通常使用 HTML、CSS、JavaScript，以及 React、Vue、Svelte 等前端技術開發。

使用者只需要透過 Safari、Chrome 等瀏覽器開啟網址即可使用，不需要先從 App Store 或 Google Play 安裝。

**HTML / CSS / JavaScript → Browser → Web App**

Web App 最大的優點就是跨平台。

只要裝置擁有現代瀏覽器，同一套 Web App 通常就能在 Windows、macOS、iPhone、Android 等不同環境執行。

而且開發者更新網站之後，使用者下一次開啟就能直接使用新版，不需要重新下載 App。

不過 Web App 主要受到瀏覽器環境限制，因此能夠使用的手機原生功能，通常沒有 Native App 那麼完整。

## PWA：可以安裝的 Web App

PWA（Progressive Web App，漸進式網頁應用程式）可以簡單理解成「進一步強化的 Web App」。

它仍然使用 HTML、CSS、JavaScript 等 Web 技術開發，但透過 Web App Manifest、Service Worker、Cache 等技術，可以提供更多接近一般 App 的使用體驗。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="837" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=4270348415&amp;adf=4116832479&amp;abgtt=6&amp;w=837&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788829691&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=837x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4054&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=836&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788829622169&amp;bpp=1&amp;bdt=6885&amp;idt=1&amp;shv=r20260902&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788829582%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788829582%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788829582%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1522x969%2C789x280%2C837x280&amp;nras=4&amp;correlator=3530933270953&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=169&amp;ady=4085&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=244&amp;eid=95399630%2C95400775%2C122880834%2C95400618%2C122880333%2C95340253%2C95340255&amp;oid=2&amp;psts=AOrYGslRLvMTE7HSSdBc6aYqXNYU3RPE_Juyhhq4jHemrtP7RJC9-ZQZHMURu5_AYUabItvZe3Mor-ZhNX4I1WQ7gV-3KZuswfIjeYj0dlO7wnYgniQ%2CAOrYGsnYso2m2Wi95QAkAo2JR2bWM-VTCHOFnL8qlzrva__zce2zMiuDb64d-PhWIJk2KRxPp0Xb4SMN9MZ3fgaOAdV5VSOJFdiXiVoLIybpkKPWOA&amp;pvsid=289262645472135&amp;tmod=1169943733&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1762%2C1050%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=157x763_l%7C157x763_r&amp;bz=1.16&amp;ifi=6&amp;uci=a!6&amp;btvi=3&amp;fsb=1&amp;dtd=69460" title="Advertisement" aria-label="Advertisement"></iframe>

例如 PWA 可以加入裝置主畫面、以接近獨立 App 的方式啟動，也可以透過快取機制支援部分離線功能。

因此可以簡化成：

**Web App + App-like 能力 → PWA**

PWA 最大的優勢，是保留 Web App 跨平台與容易更新的特色，同時又能提供更接近 App 的操作體驗。

不過需要注意的是， **PWA 本質上仍然屬於 Web 技術** 。

它能使用哪些相機、通知、檔案、背景執行等裝置功能，仍然取決於作業系統與瀏覽器對相關 Web API 的支援程度。

所以 PWA 並不等於 Native App。

## Hybrid App：把 Web App 放進真正的 App

\*\*Hybrid App（混合式 App）\*\*則是另一種非常重要的架構。

它的核心概念是：

**使用 Web 技術開發主要介面與程式邏輯，再透過 Native 容器與 Bridge 取得手機原生能力。**

例如使用 Capacitor，就可以先使用 HTML、CSS、JavaScript、Vue 或 React 開發 Web App，再將它整合進 iOS 與 Android 原生專案。

App 的主要內容可以透過 WebView 執行，而當程式需要使用相機、震動、分享、檔案系統等原生能力時，就可以透過 Capacitor Plugin 與 Native Bridge 呼叫手機提供的功能。

整個流程可以理解成：

**Web UI → App Logic → Capacitor → Native Bridge → iOS / Android**

例如使用者在 Web UI 按下「拍照」按鈕之後，JavaScript 可以呼叫 Capacitor Camera Plugin，再透過 Native Bridge 使用 iPhone 的相機功能。

因此 Hybrid App 同時擁有兩個世界的特色：主要程式可以繼續使用熟悉的 Web 技術開發，又可以取得許多 Native App 才具備的手機功能。

更重要的是，它最後可以建立成真正的 iOS 或 Android App，並提交至 App Store 或 Google Play。

## 四種方式該怎麼選？

![](https://live.staticflickr.com/65535/55512978685_2a91597d51_b.jpg)

這四種技術並不存在絕對的「哪一種最好」，真正需要考慮的是產品需求。

如果只是提供資訊、會員功能、查詢工具或一般網路服務， **Web App** 通常已經足夠，而且開發與維護成本最低。

如果希望網站可以加入主畫面、支援部分離線功能，並提供更接近 App 的體驗，可以考慮 **PWA** 。

如果已經熟悉 HTML、CSS、JavaScript、Vue 或 React，希望用同一套 Web 技術開發手機 App，同時又需要 Camera、Haptics、Share 等手機功能， **Hybrid App** 會是相當實用的選擇。

而如果產品非常重視效能、複雜動畫、大量硬體整合，或需要深度使用 iOS、Android 平台提供的能力，則可以考慮直接開發 **Native App** 。