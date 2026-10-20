---
title: "RetroPie：將您的Raspberry Pi變成複古遊戲機"
source: "https://zh-tw.hwlibre.com/retropie/#google_vignette"
author:
  - "[[Isaac]]"
published: 2020-07-07
created: 2026-08-18
description: "借助RetroPie，您可以將Raspberry Pi板變成真正的Retrogaming機器，享受前所未有的經典視頻遊戲"
tags:
  - "clippings"
status: "processed"
---
![RetroPie徽標](https://www.hwlibre.com/wp-content/uploads/2020/06/retropie.jpg.webp)

如果您對複古視頻遊戲充滿熱情，那些永不過時的經典遊戲，那麼您肯定會關注Raspberry Pi周圍出現的所有有趣的仿真器和項目。 享受復古遊戲的另一個項目是 **RetroPie** ，我將展示其中的所有鍵。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="850" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-2740134681475878&amp;output=html&amp;h=280&amp;slotname=6930539239&amp;adk=793879173&amp;adf=3788857716&amp;pi=t.ma~as.6930539239&amp;abgtt=6&amp;w=850&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787037871&amp;rafmt=1&amp;format=850x280&amp;url=https%3A%2F%2Fzh-tw.hwlibre.com%2Fretropie%2F&amp;fwr=0&amp;fwrattr=true&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1787037871609&amp;bpp=7&amp;bdt=1820&amp;idt=8&amp;shv=r20260817&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie_enabled=1&amp;eoidce=1&amp;prev_fmts=0x0&amp;nras=1&amp;correlator=8569949648447&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=215&amp;ady=906&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95397678%2C122880368%2C122880398&amp;oid=2&amp;pvsid=5272199044067737&amp;tmod=786277670&amp;uas=0&amp;nvt=6&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7CpEe%7C&amp;abl=CS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;plas=412x510_r&amp;bz=1.15&amp;pgls=CAEaBTcuMC40~CAA.&amp;ifi=2&amp;uci=a!2&amp;fsb=1&amp;dtd=11" title="Advertisement" aria-label="Advertisement"></iframe>

事實是，由於 **對這些視頻遊戲充滿熱情的用戶社區** 從過去的平台不會停止增長。 實際上，即使是像SEGA或Atari之類的某些製造商，也決定給他們過去的一些機器以第二次機會來滿足這一巨大的需求...

您可能也有興趣了解 [最好的模擬器](https://zh-tw.hwlibre.com/%E6%A8%B9%E8%8E%93%E6%B4%BE%E6%A8%A1%E6%93%AC%E5%99%A8/) Raspberry Pi以及其他類似項目 [調用框](https://zh-tw.hwlibre.com/%E8%A8%98%E9%8C%84%E6%A1%86/) y [巴托塞拉](https://zh-tw.hwlibre.com/%E8%9D%A0oc%E5%B1%AC/) 。 還有一些小工具，供控制器創建自己的小工具 [街機](https://zh-tw.hwlibre.com/%E8%A1%97%E6%A9%9F%E9%81%8A%E6%88%B2%E6%A1%BF/).

## 什麼是RetroPie？

**RetroPie** 是一個項目 [開源](https://github.com/RetroPie) 經過專門設計，可以將您的SBC變成複古的視頻遊戲中心，也就是真正的複古遊戲機。 此外，它與各種版本的Raspberry Pi等板兼容，還與其他類似的板（如ODroid C1和C2）兼容，甚至與PC兼容。

電腦硬體

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="300" height="600" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-2740134681475878&amp;output=html&amp;h=600&amp;slotname=6502785288&amp;adk=3640309342&amp;adf=2513489626&amp;pi=t.ma~as.6502785288&amp;abgtt=6&amp;w=300&amp;lmt=1787037871&amp;format=300x600&amp;url=https%3A%2F%2Fzh-tw.hwlibre.com%2Fretropie%2F&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1787037871626&amp;bpp=2&amp;bdt=1837&amp;idt=2&amp;shv=r20260817&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie_enabled=1&amp;eoidce=1&amp;prev_fmts=0x0%2C850x280&amp;nras=1&amp;correlator=8569949648447&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=490&amp;ady=1745&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95397678%2C122880368%2C122880398&amp;oid=2&amp;pvsid=5272199044067737&amp;tmod=786277670&amp;uas=0&amp;nvt=6&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7CpeEbr%7C&amp;abl=CS&amp;pfx=0&amp;fu=0&amp;bc=31&amp;plas=412x510_r&amp;bz=1.15&amp;pgls=CAEaBTcuMC40~CAA.&amp;ifi=3&amp;uci=a!3&amp;btvi=1&amp;fsb=1&amp;dtd=4" title="Advertisement" aria-label="Advertisement"></iframe>

從RetroPie 4.6版本開始，還包括對Raspberry Pi 4的支持。

該項目建立在其他眾所周知的現有項目的基礎上，例如 **Raspbian，EmulationStation，RetroArch，Kodi等** 現有的很多。 所有這些都集中在一個集中的項目中，為您提供了一個完整而簡單的平台，因此您只需要擔心自己喜歡的Arcade遊戲。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="850" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-2740134681475878&amp;output=html&amp;h=280&amp;slotname=1636638088&amp;adk=535995927&amp;adf=3405032148&amp;pi=t.ma~as.1636638088&amp;abgtt=6&amp;w=850&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787037871&amp;rafmt=3&amp;format=850x280&amp;url=https%3A%2F%2Fzh-tw.hwlibre.com%2Fretropie%2F&amp;fwr=0&amp;fwrattr=true&amp;rpe=1&amp;resp_fmts=1&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1787037871633&amp;bpp=1&amp;bdt=1843&amp;idt=0&amp;shv=r20260817&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie_enabled=1&amp;eoidce=1&amp;prev_fmts=0x0%2C850x280%2C300x600&amp;nras=1&amp;correlator=8569949648447&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=215&amp;ady=2726&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95397678%2C122880368%2C122880398&amp;oid=2&amp;pvsid=5272199044067737&amp;tmod=786277670&amp;uas=0&amp;nvt=6&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7CpEebr%7C&amp;abl=CS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;plas=412x510_r&amp;bz=1.15&amp;pgls=CAEaBTcuMC40~CAA.&amp;ifi=4&amp;uci=a!4&amp;btvi=2&amp;fsb=1&amp;dtd=3" title="Advertisement" aria-label="Advertisement"></iframe>

但是，如果您是高級用戶，它還包括 **多種配置工具** 因此您幾乎可以根據需要修改和自定義系統。

### 仿真平台

![Atari控制台](https://www.hwlibre.com/wp-content/uploads/2020/06/atari-consola.jpg.webp)

索尼DSC

RetroPie可以效仿 **超過50個視頻遊戲平台** 因此您可以使用他們遊戲的ROM來恢復它們的活力。 最著名的是：

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="300" height="600" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-2740134681475878&amp;output=html&amp;h=600&amp;slotname=2777385541&amp;adk=432872887&amp;adf=2892139693&amp;pi=t.ma~as.2777385541&amp;abgtt=6&amp;w=300&amp;lmt=1787037872&amp;format=300x600&amp;url=https%3A%2F%2Fzh-tw.hwlibre.com%2Fretropie%2F&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1787037871641&amp;bpp=1&amp;bdt=1852&amp;idt=1&amp;shv=r20260817&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D238c6bab5bbfe9c7%3AT%3D1787037871%3ART%3D1787037871%3AS%3DALNI_MZStQ9yiSwEqmwdS6BaasKEmrkmuQ&amp;gpic=UID%3D000015227bf16b6a%3AT%3D1787037871%3ART%3D1787037871%3AS%3DALNI_MZ5Dh5sQipGeNTjebhv4Oy5zMHt5Q&amp;eo_id_str=ID%3D09ba79a3a7ca95ec%3AT%3D1787037871%3ART%3D1787037871%3AS%3DAA-AfjZpOK7mubTr8XTrS-h6k-Q3&amp;prev_fmts=0x0%2C850x280%2C300x600%2C850x280%2C298x600%2C1634x908%2C200x510%2C1005x124&amp;nras=4&amp;correlator=8569949648447&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=490&amp;ady=3970&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=420&amp;eid=95397678%2C122880368%2C122880398&amp;oid=2&amp;psts=AOrYGsmPRNzsKHt8VyCcPfPoEhIgXBDq9PLrBwruOvYo_ByeSpWftFXa-eaSiK3MwJVs6ToPPEG9HC2Qee0evqeOU86LIR6tkjekvj8nGg%2CAOrYGsmBnProSB8jl4KeBrEjhCNkiTmMMpCOljUfZj-BZK4CrkazNlyi78Uxg9KRx2KP13pRfbAht3NWtvCeZbRXz_hRmwQt6wmxfKY9ZmTFc-Y20lOkY4QNjDuq5qg%2CAOrYGsnynMy_ZyBQ802V2Gyp8iEQMtWohsbRVT9xTPiSjUcaYnNvq5FdFdv25CZ7ZQqPa78Ge60zshKNbnNku1p0iJB3s8LA6NGrA1QMCmH37uThdq23wSeXdjL9sF_B%2CAOrYGslG27zk4IFGOkIjaZzc1KwsDY-4qEAsiMiS5bSvKHBzYI1nDYb2bjAsGTBiAAxk3ms--MfhI6U2xD1xpsbjXBXSS0CsR1ivOL5Bdg%2CAOrYGslwIjV6lr_kq93mHqQZ1ABh1DwcZU3NnN7Wbkqdx5x19IdXX5x1i8qSBj0sfCyFR-Xu8bsplh6KV75dAPzh8SUlgjKKlawPQwdw&amp;pvsid=5272199044067737&amp;tmod=786277670&amp;uas=0&amp;nvt=6&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7CpeEbr%7C&amp;abl=CS&amp;pfx=0&amp;fu=0&amp;bc=31&amp;plas=412x510_r&amp;bz=1.15&amp;pgls=CAEaBTcuMC40~CAA.&amp;ifi=5&amp;uci=a!5&amp;btvi=5&amp;fsb=1&amp;dtd=592" title="Advertisement" aria-label="Advertisement"></iframe>

- 任天堂NES
- 超級任天堂
- 塞斯特姆大師
- 1的PlayStation
- 創世紀
- 遊戲男孩
- GameBoy進階
- Atari 7800
- 遊戲男孩的顏色
- Atari 2600
- 世嘉SG1000
- 任天堂64
- 世嘉32X
- 世嘉CD
- 雅達利山貓
- 新紀元
- NeoGeo口袋顏色
- 阿馬斯特拉德CPC
- 辛克萊ZX81
- 雅達利ST
- 辛克萊ZX光譜
- 夢播
- PSP
- Commodore 64
- 還有更多...

## 我怎樣才能擁有RetroPie？

您 **下載RetroPie** 完全免費 [從官方網站](https://retropie.org.uk/download/) 該項目。 但是在著手研究之前，您應該記住，RetroPie可以通過多種方式工作：

電腦硬體

- 將其安裝在現有的操作系統上，例如Raspbian。 有關的更多信息 [拉什比安](https://retropie.org.uk/docs/Manual-Installation/) y [於Debian / Ubuntu](https://retropie.org.uk/docs/Debian/).
- 從頭開始使用RetroPie映像，並添加其他軟件。

 ![巴倫塔蝕刻機](https://www.hwlibre.com/wp-content/uploads/2019/12/etcher.png) ![balentaEtcher](https://www.hwlibre.com/wp-content/uploads/2019/12/etcher.png)

除了這種多功能性之外， **遵循的步驟** 從頭開始在SD上安裝RetroPie的方法如下：

1. [下載圖片](https://retropie.org.uk/download/) de **RetroPie** 對應於您的Pi版本。
2. 現在，您必須在.gz中提取壓縮的圖像。 您可以使用來自 [Linux](#) 的命令或7Zip之類的程序來執行此操作。 結果應為包含以下內容的文件： **.img擴展名**.
3. 然後用一些程序就能 **格式化SD並傳遞圖像** 由RetroPie。 你可以做到 [刻蝕機](https://zh-tw.hwlibre.com/%E8%9D%95%E5%88%BB%E6%A9%9F/) ，它也與Windows，macOS和Linux兼容。 這對所有人來說都是相同的過程。
4. 現在將SD卡插入 **樹莓派** 並開始。
5. 啟動後，轉到“配置”菜單中的“ **WiFi** 將您的SBC連接到網絡。 配置相對應的網絡適配器，因為您的主板可能較舊，帶有USB WiFi適配器，或者您的Pi具有集成的WiFi，或者您可能已通過RJ-45（以太網）電纜進行了連接。 您必須選擇選項並連接到常規網絡。
如果您願意，儘管在大多數情況下不是必需的，但是您可以安裝其他軟件或更多仿真器。

Linux與 Unix

![控制項](https://www.hwlibre.com/wp-content/uploads/2018/04/mandos.png)

一旦實現，以下是 **配置您的控件** 或遊戲控制器（如果有）。 為此，步驟如下：

1. **連接USB控制器** 你有。 亞馬遜上有許多RetroPie兼容控制器。 例如 找不到產品。 或 找不到產品。。 您甚至可以使用一些較新的控制器。
2. 插入後，RetroPie應該會自動啟動 **界面來配置它們** 。 在其中，它將要求您在助手中必須執行的一系列操作。 如果輸入有誤，請放心，您可以稍後通過按“開始”或使用F4並重新啟動來訪問菜單以修改設置。

之後，您可以做的是 **通過ROM** 使您最喜歡的視頻遊戲可以從Raspberry Pi運行。 您可以通過幾種方式來完成此操作，一種是通過SFTP（稍微複雜一些），通過Samba（也有些麻煩），另一種是通過USB（大多數情況下更簡單，更可取）。 對於USB選項：

1. 使用先前以FAT32或NTFS格式格式化的Pendrive或USB存儲器。 兩者都服務。
2. 在裡面你必須創建一個 **名為“ retropie”的文件夾** 不帶引號。
3. 現在安全地拔下USB並將其插入 **USB端口** Raspberry Pi的。 保留它直到LED停止閃爍。
4. 現在，再次從樹莓派上斷開USB連接，然後將其放在PC上 **通過ROM** 在retropie / roms目錄中。 如果ROM被壓縮，則需要解壓縮它們才能工作。 您還可以在roms內創建文件夾，以按平台分類ROM，例如，您可以為Nintendo NES遊戲創建名為nes的文件夾，等等。
5. 將USB重新插入您的Pi，等待LED停止閃爍。
6. 現在 **刷新EmulationStation** 通過從主菜單中選擇“重新啟動”。

現在只有 **開始遊戲** …順便說一句，要退出您沉浸的遊戲，您可以使用遊戲控制器上同時按下的“開始”和“選擇”按鈕，它將返回到RetroPie主菜單…

電腦硬體

### 輕鬆得多（新手用戶）

Si **你不想讓你的生活變得過於復雜** 與ROM或RetroPie的安裝一起，您應該知道，除了已經包括的數千個ROM以外，他們已經在銷售安裝了該系統的SD卡。

例如，在 找不到產品。 賣一個 **128GB microSD卡** 三星品牌的容量，其中已經包括RetroPie，以及已經包括的18000多個視頻遊戲ROM。

### 查找ROM

 ![波斯王子](https://www.hwlibre.com/wp-content/uploads/2020/06/prince-of-persia-roms.jpg) ![Prince Of Persia](https://www.hwlibre.com/wp-content/uploads/2020/06/prince-of-persia-roms.jpg)

請記住，Internet上有許多網頁允許 **下載ROM** 非法，因為它們是專有視頻遊戲。 因此，在知道自己可能犯有侵犯知識產權罪的情況下，必須自擔風險。

此外，在 [互聯網檔案館](https://archive.org/) 您還可以找到一些非常古老的視頻遊戲ROM。 當然你也有 **完全免費的ROM** 和合法的，如果你想要他們，像那些 [MAME](https://www.mamedev.org/roms/).

## 可用的附加組件

![街機](https://www.hwlibre.com/wp-content/uploads/2018/04/m%C3%A1quina-arcade.jpg)

您應該知道，有大量的 **DIY項目** 使用Raspberry Pi創建您自己的便宜且微型的Arcade機器，並以簡單的方式重新創建過去的許多其他遊戲機。 為此，RetroPie還為您提供了一些有趣的文檔：

但這不是您觸手可及的唯一東西，它們也存在 **非常有趣的工具包** 您可以通過簡單的方法購買組裝復古控制台：

- 找不到產品。 模仿SuperCOM的複古控制台殼
- 找不到產品。 這是另一個模仿傳奇的任天堂NES的案例
- 找不到產品。 Raspberry Pi Zero的類似GameBoy的保護套