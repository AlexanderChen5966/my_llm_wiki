---
title: "打造自己的懷舊遊戲機 - RetroPie(樹莓派)(三: 設定篇)"
source: "https://www.mhfohk.com/wordpress/2019/06/07/%E6%89%93%E9%80%A0%E8%87%AA%E5%B7%B1%E7%9A%84%E6%87%B7%E8%88%8A%E9%81%8A%E6%88%B2%E6%A9%9F-retropie%E6%A8%B9%E8%8E%93%E6%B4%BE%E4%B8%89-%E8%A8%AD%E5%AE%9A%E7%AF%87/"
author:
  - "[[bennychi]]"
published: 2019-06-07
created: 2026-08-18
description: "上一篇，我們已經把系統安裝好，也設定完手把。 現在先選Retropie(其實剛剛安裝好就應該只有這一個選項) …"
tags:
  - "clippings"
status: "processed"
---
上一篇，我們已經把系統安裝好，也設定完手把。

現在先選Retropie(其實剛剛安裝好就應該只有這一個選項)  

再選RASP-CONFIG  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/1-3.jpeg)

1\. 這裡我們先設定好WiFi(如果你是插網線可跳過這一步)  
1A. 先選"Network Options"  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/network1.jpg)

1B. 選"N2 WiFi"  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/network2.jpg)

1C. 輸入你的SSID  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/network3.jpg)

1D. 輸入密碼金鑰  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/network4.jpg)

輸入正確的話就會自動連線到你的WiFi，後面我們再看現在的IP是什麼，  
現在先打開SSH，因為很多設定我們透過SSH設定會更加方便。

2\. 打開SSH  
2A. 選"5 Interfacing Options"  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/interface1.jpg)

2B. 選"P2 SSH"  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/interface2.jpg)

連線和SSH都設定好後就可以退出了，選"Finish"離開  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/quit_raspi_config.jpg)

好吧，我們現在先來看看這樹莓的IP是什麼，回到Retropie的選單，選"Show IP"  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/IP1-1.jpeg)

上面就會顯示IP  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/IP2-1.jpeg)

![](https://www.youtube.com/watch?v=6Lwlf4MRz2A)

知道IP後，我們就可以做更多東西了，例如用SSH改設定，加ROMs(遊戲)等等......  
在加遊戲前，我們先看看怎麼增加更多的模擬器，其實預設已經自己把大部份常用的模擬安裝好了  
例如: 紅白機，超任，街機，PC-Engine，世嘉，Game Boy等等......  
但是...人總是貪心的，可以玩Playstation嗎? 可以玩PSP嗎? 可以玩Dreamcast嗎?  
答案是可以的，不過模擬器要先自行安裝。

這次我們選"RETROPIE SETUP"  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/retropie-setup.jpg)

開始安裝其他模擬器前，建議大家先更新一次所有Setup script再安裝(也就是把模擬器版本先更新)  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/1-1.jpg)

然後選"Manage packages"  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/2-1.jpg)

再選"Manage optional pcakages"  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/3.jpg)

在這裡你就可以把想安裝的模擬器都點一次了，左下角會有注解說明是什麼模擬器  
reicast = Dreamcast，ppsspp就是PSP啦  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/4.jpg)

如果你會玩街機類的遊戲，個人建議你把fbalpha2012也安裝，兼容性比預設的好  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/5-1.jpg)

再來就可以開始傳輸遊戲到樹莓啦，剛剛我們已經知道IP，  
現在只要打開檔案總管，輸入"\\\\"+上剛剛的IP 或者 \\\\retropie也可  
這裡會看到4個資料夾，我們主要用到的就是"bios"和"roms"，  
有些模擬需要bios才能運行，例如紅白機的磁碟機遊戲(FDS)，Dreamcast等等  
當然遊戲就是放在roms裡面  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/1.png)

附上bios連結: [Dreamcast](http://www.mhfohk.com/retropie/bios/dc_bios.rar) ， [磁碟機(FDS)](http://www.mhfohk.com/retropie/bios/disksys.rom)

![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/bios.jpg)

roms裡面就更精彩了...  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/roms.png)

對新手來說...可能會傻眼...那麼多資料夾，怎麼知道自己下載回來的遊戲該放在那裡...

這裡附上對照表，算是一目了然吧...

| 模擬器 | Roms位置 | 支援的檔案格式 |
| --- | --- | --- |
| Arcade | /home/pi/RetroPie/roms/arcade | .7z.cue.fba.iso.zip |
| Dreamcast | /home/pi/RetroPie/roms/dreamcast | .cdi.chd.gdi |
| Final Burn Alpha | /home/pi/RetroPie/roms/fba | .7z.cue.fba.iso.zip |
| Famicom Disk System | /home/pi/RetroPie/roms/fds | .7z.nes.fds.zip |
| Sega Gamegear | /home/pi/RetroPie/roms/gamegear | .7z.gg.bin.sms.zip |
| Game Boy | /home/pi/RetroPie/roms/gb | .7z.gb.zip |
| Game Boy Advance | /home/pi/RetroPie/roms/gba | .7z.gba.zip |
| Game Boy Color | /home/pi/RetroPie/roms/gbc | .7z.gbc.zip |
| Multiple Arcade Machine Emulator | /home/pi/RetroPie/roms/mame-libretro | .zip |
| Sega Mega Drive | /home/pi/RetroPie/roms/megadrive | .7z.smd.bin.gen.md.sg.zip |
| Nintendo 64 | /home/pi/RetroPie/roms/n64 | .z64.n64.v64.zip |
| Neo Geo | /home/pi/RetroPie/roms/neogeo | .7z.cue.fba.iso.zip |
| Nintendo Entertainment System | /home/pi/RetroPie/roms/nes | .7z.nes.zip |
| Neo Geo Pocket | /home/pi/RetroPie/roms/ngp | .7z.ngp.zip |
| Neo Geo Pocket Color | /home/pi/RetroPie/roms/ngpc | .7z.ngc.zip |
| PC Engine | /home/pi/RetroPie/roms/pcengine | .7z.pce.ccd.chd.cue.zip |
| PlayStation Portable | /home/pi/RetroPie/roms/psp | .iso.pbp.cso |
| PlayStation | /home/pi/RetroPie/roms/psx | .cue.cbn.img.iso.m3u.mdf.pbp.toc.z.znx |
| Sega SG-1000 | /home/pi/RetroPie/roms/sg-1000 | .7z.sg.bin.zip |
| Super Nintendo | /home/pi/RetroPie/roms/snes | .7z.bin.bs.smc.sfc.fig.swc.mgd.zip |
| Vectrex | /home/pi/RetroPie/roms/vectrex | .7z.vec.gam.bin.zip |
| Virtual Boy | /home/pi/RetroPie/roms/virtualboy | .7z.vb.zip |
| Wonderswan | /home/pi/RetroPie/roms/wonderswan | .7z.ws.zip |
| Wonderswan Color | /home/pi/RetroPie/roms/wonderswancolor | .7z.wsc.zip |

好，馬上抄遊戲去試一下  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/addroms.jpg)

然後回去樹莓裡按"Start"=>"Quit"  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/6.jpg)

"Restart Emulationstation"=>"Yes"  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/7.jpg)

等重啟完成後，去看看"Arcade"裡，就應該可以看到剛剛加入的遊戲  
PS:每次加完新遊戲，都必須重啟一次。  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/8.jpg)

按手把上的A鍵開始遊戲，會出現以下畫面  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/9.jpg)

如果沒問題就會進遊戲，如果模擬器不支持你的遊戲，就會退回遊戲列表，  
退回去也不用怕，再按A開始遊戲，然後在上一步的畫面出現時，  
隨便按手把上的一個按鍵，會進去設定畫面  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/11.jpg)

在這裡我們可以更改用那個模擬開啟遊戲，像Arcade類的遊戲，  
我建議大家把預設模擬器改成Ir-fbalpha2012，兼容性比較高  
沒記錯的話，預設是pifba...很多遊戲都開不了  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/12.jpg)

改完設定，從新再開遊戲看看，  
Yeah~~~成功了!  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/13.jpg)

加遊戲大概就是這樣，很簡單，另外，大家應該會發覺，  
我的遊戲都是英文的，為什麼遊戲列表會顯示中文，還有預覽圖!!!  
其實可以在每個roms資料夾內新增一個gamelist.xml的檔案  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/gamelist1.jpg)

用"記事本"打開就可以編輯，遊戲名稱，預覽圖等等的資料  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/gamelist2.jpg)

檔案的格式如下:  
<?xml version="1.0"?>  
\<gameList>  
\<game>  
\<path>./dino.zip\</path>  
\<name>\[動作\]恐龍新世紀(930201世界版)\</name>  
\<image>./images/dino.jpg\</image>  
\</game>  
\</gameList>

\<path>\</path> <---rom的檔案名稱  
\<name>\</name> <---遊戲列表上顯示的名稱  
\<image>\</image> <---預覽圖的位置(PS:我習慣在每個roms資料夾內再新增一個images資料夾放預覽圖)

如果大家一直跟著這篇教學設定，現在一定發現，  
把遊戲名改成中文後，只有一堆正方形跟亂碼，  
無法正常顯示中文名稱。

現在我們用SSH來解決這個問題，先來下載PUTTY  
官方網址: [https://www.putty.org/](https://www.putty.org/) ，安裝後打開PUTTY  
在Host輸入樹莓派的IP，再按Open  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/SSH.jpg)

輸入使用者名稱: pi 密碼預設是 raspberry  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/SSH2-1.jpg)

登入後可以看到樹莓的各項資料，例如SD剩餘的內量等等  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/SSH3.jpg)

重點來了，複製以下指令，在PUTTY裡面按滑鼠右鍵，  
把複製的指令貼上，再按enter。  
其實這個指令的作用就是為樹莓安裝中文字形  
sudo apt-get install libsdl2-dev libboost-system-dev libboost-filesystem-dev libboost-date-time-dev libboost-locale-dev libfreeimage-dev libfreetype6-dev libeigen3-dev libcurl4-openssl-dev libasound2-dev libgl1-mesa-dev build-essential cmake fonts-droid-fallback  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/font1.jpg)

等指令跑完後再輸入 sudo reboot，樹莓會重新啟動，  
再看看所有中文字應該都能正常顯示了  
![](https://www.mhfohk.com/wordpress/wp-content/uploads/2019/06/font2.jpg)

設定篇就先到這裡了，但是還沒有結束啦，別忘了，  
我還有個退出遊戲的按鍵，還有還有......  
你覺得只可以玩舊遊戲就足夠了嗎? 如果能看即時球賽呢? 看電影呢?  
玩一些舊的電腦遊戲又如何(如DOS版的金庸群俠傳，DOS時代的天使帝國，大富翁等等)?  
如果想瀏覽網頁，看看facebook，IG? 做點簡點的文書處理工作?  
甚至是遠端桌面(RDP)???

還有更多更多的東西，直播你玩遊戲的實況到Youtube，Twitch等等等等......  
後面會慢慢教大家發揮這台小電腦的極限~~~

下一篇: [DIY退出遊戲，回到遊戲列表的按鍵](http://www.mhfohk.com/wordpress/2019/06/10/%e6%89%93%e9%80%a0%e8%87%aa%e5%b7%b1%e7%9a%84%e6%87%b7%e8%88%8a%e9%81%8a%e6%88%b2%e6%a9%9f-retropie%e6%a8%b9%e8%8e%93%e6%b4%be%e5%9b%9b-%e8%87%aa%e5%88%b6%e9%80%80%e5%87%ba%e9%8d%b5/)