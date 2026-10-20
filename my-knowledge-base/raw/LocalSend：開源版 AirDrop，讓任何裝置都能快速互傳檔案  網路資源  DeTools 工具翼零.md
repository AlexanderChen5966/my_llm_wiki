---
title: "LocalSend：開源版 AirDrop，讓任何裝置都能快速互傳檔案 | 網路資源 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3956"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-06-22
created: 2026-06-23
description: "LocalSend 是開源跨平台檔案傳輸工具，支援 Windows、macOS、Linux、Android 與 iPhone，透過區域網路快速分享檔案，免註冊、免雲端，打造類似 AirDrop 的傳輸體驗。"
tags:
  - "clippings"
status: processed
---
LocalSend 是一款開源、跨平台的區域網路檔案傳輸工具，核心用途是在不經過雲端伺服器的情況下，直接於同一網路環境中的裝置之間傳送檔案。它最大的特色在於不需要註冊帳號、不需要建立配對碼，也不必依賴第三方服務，只要裝置連上同一個 Wi-Fi 網路，就能快速完成檔案分享。

對許多人來說，在手機、平板與電腦之間傳送檔案一直是個麻煩問題。不同品牌設備往往使用不同生態系，例如 Apple 有 AirDrop、Android 有 Quick Share，但跨平台時體驗通常不夠理想。LocalSend 的目標就是打破這種限制，提供一套不分作業系統的檔案傳輸方案。

<iframe width="825" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=3666733072&amp;adf=828129217&amp;pi=t.aa~a.2909957178~i.5~rp.4&amp;abgtt=13&amp;w=825&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1782177025&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=825x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticle%2Fsn%2F3956&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=825&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782177025099&amp;bpp=1&amp;bdt=2749&amp;idt=1&amp;shv=r20260622&amp;mjsv=m202606170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1782177019%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1782177019%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1782177019%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=825x280%2C0x0%2C255x600%2C255x600&amp;nras=2&amp;correlator=6497496153523&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=285&amp;ady=1382&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099218%2C95393485&amp;oid=2&amp;pvsid=6094029927605518&amp;tmod=1196950045&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Fweb%2Fpage%2F1&amp;fc=384&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=247x742_l&amp;bz=1.14&amp;pgls=CAs.&amp;ifi=16&amp;uci=a!g&amp;btvi=3&amp;fsb=1&amp;dtd=4" title="Advertisement" aria-label="Advertisement"></iframe>

從產品定位來看，LocalSend 可以視為開源版 AirDrop，但支援的平台範圍更廣。它採用區域網路（LAN）直接傳輸技術，不需經過外部伺服器，因此速度通常只受限於本地網路環境。

目前支援的平台包括：

這意味著使用者可以在不同品牌與系統之間自由交換檔案，例如 Android 傳到 Mac、Windows 傳到 iPhone，或 Linux 傳到 Android。

其中最受到使用者喜愛的是零設定體驗。

一般情況下：

1. 開啟 LocalSend
2. 裝置自動出現在列表中
3. 選擇接收裝置
4. 傳送檔案

即可完成。

不需要：

因此對一般使用者來說相當直覺。

從技術角度來看，LocalSend 採用本地網路直接通訊模式。

檔案不會上傳至第三方伺服器，而是在裝置之間直接傳送。

這帶來幾個重要優勢：

- 傳輸速度快
- 不受雲端容量限制
- 不需網際網路連線
- 隱私性較高
- 不產生額外儲存成本

例如傳送數 GB 的影片檔時，不需要先上傳到雲端再下載，而是直接在本地網路中完成。

對於重視隱私的人而言，LocalSend 的另一項優勢是開源。

專案程式碼完全公開，任何人都可以檢查其運作方式。

相較於某些封閉式傳輸服務，使用者更容易確認：

- 是否有資料蒐集
- 是否存在追蹤機制
- 是否依賴外部伺服器

這也是它在開源社群快速累積大量使用者的重要原因。

整體而言，LocalSend 是近年最成功的開源生產力工具之一。它透過簡單易用的介面與跨平台支援，讓不同裝置之間的檔案分享變得像 AirDrop 一樣方便，同時保有開源、隱私與本地傳輸的優勢。對於經常在手機、平板與電腦之間交換檔案的人來說，LocalSend 提供了一個免費、快速且不受平台限制的解決方案。