---
title: "現實版《哈利波特》魔法日記！開發者改造reMarkable平板，讓「AI佛地魔」與你對話"
source: "https://www.bnext.com.tw/article/91432/xhejnc8rjurch7kzudri"
author:
  - "[[李先泰]]"
published: 2026-07-06
created: 2026-07-07
description: "開發者Maxime Rivest用開源專案Riddle，把reMarkable Paper Pro電子紙平板改造成《哈利波特》裡會回話的魔法日記。"
tags:
  - "clippings"
status: processed
---
[![2026 未來經理人年會‼️重磅講者公開](https://bnextmedia.s3.hicloud.net.tw/pumpkin/image/photo/2026-07/img-1783235221-73379.gif)](https://edm.managertoday.com.tw/future-manager/?utm_campaign=2026FM&utm_source=web_bn&utm_medium=logo_banner&utm_content=164730&utm_term=channel_all)

2026.07.06 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

開發者Maxime Rivest用開源專案Riddle，把reMarkable Paper Pro電子紙平板改造成《哈利波特》裡會回話的魔法日記。

---

> 先說結論： **這不是特效，而是一台市面上真的買得到的電子紙平板，加上一份開源程式碼做出來的。**

開發者 Maxime Rivest（X 帳號 @MaximeRivest）在 X 分享了一段 23 秒的展示影片。畫面裡，一支數位筆在電子紙上寫下一行字，停頓幾秒後，這行字像被紙「喝」進去一樣慢慢淡掉，接著紙面自己浮現一行手寫草書的回覆，寫完後再度淡去。

整個過程沒有傳統 LCD 或 OLED 那種發光的聊天視窗，所有互動都只呈現在電子紙頁面上。

Rivest 長期關注人與 AI 的互動方式，也是開源框架 DSPy 的貢獻者，他把這個專案命名為「Riddle」，靈感直接來自《哈利波特》裡湯姆·瑞斗（Tom Riddle）那本會回話的魔法日記。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

Rivest 表示，他是靠 Anthropic 的 Claude Fable 5 協助開發 Riddle 的。他在原始貼文寫下：「Fable 把我的 reMarkable 變成了《哈利波特》裡湯姆·瑞斗的日記，我的提示逐漸消失，一個大型語言模型回應我，太神奇了。」

## 先搞懂：reMarkable 到底是什麼？

[reMarkab](https://remarkable.com/?utm_source=google&utm_medium=paid&utm_campaign=tt01-co01-dn01%7Cgse%7Cprosp%7Cpurchase%7Cbrand-apac%7C0226&utm_content=tt01-co01-dn01%7Capac%7Ckeyword%7Cbrand-exactsearch%7C0226&utm_term=23630588368,192612982254,799172984385&gad_source=1&gad_campaignid=23630588368&gbraid=0AAAAACTQ8Cyx8by8m9rbAUyst-Au1BM2s&gclid=CjwKCAjwpK3SBhASEiwAtV1SPBiQFVQHceuesuC9xX6E_mBWhg0mgUGUu63uaEgNjbQ-8tpmPYHlWRoCAo0QAvD_BwE) 在台灣知名度相對不高，其為一家挪威新創公司做的電子紙平板。它其實也支援打字輸入、檔案整理、搜尋、雲端同步與外部服務串接，只是刻意不做一般平板的 App 生態，把核心功能集中在閱讀、手寫筆記與文件整理，主打減少通知和娛樂內容的干擾。

這次被改造的是它的高階機型 reMarkable Paper Pro：11.8 吋彩色電子紙螢幕（配有可調式閱讀燈）、筆尖與螢幕墨跡的落差不到 1 毫米（寫起來幾乎跟真筆一樣貼手），機身約 525 公克。美國官網現行售價約 629 美元起、含手寫筆 Marker，換算約新台幣 2 萬元。它本質上就是一塊「會存檔的高級紙」，這也是為什麼把它變成魔法日記，畫面才會那麼像真的在紙上寫字。

![](https://www.youtube.com/watch?v=agsO2jezIOg)

## 它到底怎麼「魔法」？

[Riddle](https://github.com/MaximeRivest/Riddle) 的核心體驗，是把「打字問 AI」這個動作整個拿掉。

運作流程是這樣：你用筆在紙上寫字，停筆後系統會等待約 2.8 秒，確認你寫完了；接著剛寫的字會滲進紙裡淡去，同時整頁內容被轉成一張 PNG 圖片，送給 AI 判讀；判讀完，AI 的回覆再用手寫草書一個字一個字浮現在紙上，讀完後同樣淡去。

至於速度，README 標示的「約 0.9 到 1.1 秒」，指的是「送出內容之後，到裝置開始寫下第一筆回覆」的時間。換句話說，從你停筆到看見回覆，還要加上前面那 2.8 秒的等待，實際約 3.7 到 3.9 秒起跳，再視你選用的模型與網路速度而定。

除了寫字，Riddle 還設計了幾個像「咒語」的手勢：

- **想擦掉內容** ：把筆倒過來當橡皮擦
- **需要操作說明** ：在紙上畫一個大大的問號，就會叫出導引畫面
- **想離開日記** ：五根手指同時點一下螢幕
- **讓它睡覺** ：按電源鍵，日記會留下一句話進入休眠

## 那手漂亮的手寫字，是怎麼生出來的？

先講一個容易混淆的重點： **Riddle 其實有兩種顯示模式，難度差很多。**

- **視窗模式（qtfb）** ：透過 xovi、AppLoad 在原廠介面 xochitl 裡面執行，不會停掉原廠系統，是比較好裝的版本。
- **接管模式（quill）** ：直接停掉 xochitl、自己驅動電子紙顯示引擎，換取最低延遲。展示影片裡那種流暢的即寫即應，用的就是這個模式。

所以影片裡最吸睛的效果，並不是隨便裝一裝就有，而是靠接管模式硬把反應速度壓到極限。有另一位也想做出同樣效果的開發者就在留言區說，他問過的 AI 認為除非完全避開 xochitl，否則辦不到；Rivest 回應，他正是把 xochitl 整個繞了過去。

至於 AI 回覆那手草書，用的是開源字體 Dancing Script。程式會先把字體轉成圖，用一套叫 Zhang-Suen 的演算法把筆畫「瘦身」成單線，再拆成一筆一畫的軌跡，播放時一筆一筆描出來，看起來就像有人正在你眼前寫字。

技術上，Riddle 主體用 Rust 語言寫成，負責筆跡輸入與手寫合成；接管模式那顆顯示驅動叫 quill，用 C/C++ 寫成，專門接手電子紙引擎、換取最低延遲。

## 想自己裝？先看你要哪一種

大多數人比較可能採用的，是作者已經編好的預先編譯版本（跑在前面說的視窗模式）。大致步驟：

> 1. 把 reMarkable Paper Pro 切換到開發者模式、開啟 SSH 連線
> 2. 用作者提供的 remagic 工具裝好 xovi 加 AppLoad 這套第三方啟動器
> 3. 到專案 releases 頁面下載 `riddle-appload-aarch64.zip` ，解壓後用 `scp` 指令複製到平板指定目錄
> 4. 在 `oracle.env` 檔案設定 API 金鑰，或設定 pi 後端
> 5. 在 AppLoad 重新載入、打開「The Diary」就完成了

但要提醒，想重現展示影片那種直接接管電子紙、追求最低延遲的效果，門檻明顯更高。你還得準備 reMarkable SDK 工具鏈、從自己的裝置取出專有函式庫，並自行編譯 quill 與接管版本。別以為下載預先編譯套件，就能完全複製影片裡的底層效果。

AI 後端也不只一種選擇：可以接任何具備圖片辨識能力的「OpenAI 相容」API（OpenAI、OpenRouter、Groq 都行），也可以指向相容的本機模型伺服器；此外作者還做了一個叫 pi 的常駐程序，事先把連線與訂閱驗證載入，讓每一次問答只需付「模型運算」本身的時間。

## 這是玩具，不是產品

要先給想嘗鮮的讀者潑一點冷水。

第一，Riddle 目前只在 reMarkable Paper Pro（處理器架構 aarch64、系統版本 3.26 到 3.27）上測試過，其他機型不保證能動。第二，接管模式會以 root 權限執行、停止原廠介面並直接驅動電子紙，作者明講「風險自負」，務必保留 SSH 連線，萬一出事才救得回來。

還有一筆隱形成本，和一個隱私提醒。AI 回覆要嘛用你自己的 API 金鑰（用多少付多少）、要嘛自己架本機模型，要嘛走作者的 pi 常駐程序（掛在你的訂閱上），都不是一次買斷的功能。

而且既然 Riddle 是把你手寫的整頁內容轉成 PNG 圖片送給模型判讀，若你用的是雲端 API，這些內容就會傳到外部服務。因此建議還是別把敏感日記、密碼或個資寫進去。

授權方面也講清楚：Riddle 主程式採 MIT 授權開源；但接管模式依賴的 `libqsgepaper.so` 與部分 Qt 元件屬原廠專有內容，不包含在專案裡，必須由你自己的裝置或 SDK 取得。

## 好玩，就是重點

Riddle 示範了個人開發者如何借助 AI，把一台市售電子紙平板，改造成原廠沒有提供的互動形式。它有趣的地方，不只在於把《哈利波特》的魔法搬進現實，更在於整條路徑：先用 AI 幫忙把想法寫成程式，再讓成品去呼叫另一個看得懂圖的 AI。

至於實用嗎？大概不太實用。但有時候，好玩本身就是重點。

> 延伸閱讀： [Fable 5怎麼用？官方教你這樣下提示詞，才用得出最強能力](https://www.bnext.com.tw/article/91415/claude-fable-5-guide)

資料來源： [Maxime Rivest 的 X 貼文](https://x.com/MaximeRivest/status/2073544461473169432) 、 [Riddle GitHub 專案](https://github.com/MaximeRivest/Riddle) 、 [reMarkable Paper Pro 官方頁面](https://remarkable.com/products/remarkable-paper/pro) 、 [Anthropic Claude Fable 5](https://www.anthropic.com/claude/fable)

本文初稿為AI編撰，整理．編輯/ 李先泰

關鍵字： [＃AI工具](https://www.bnext.com.tw/tags/AI%E5%B7%A5%E5%85%B7) [＃Anthropic](https://www.bnext.com.tw/tags/Anthropic) [＃Claude](https://www.bnext.com.tw/tags/Claude)

[![](https://edm.bnext.com.tw/epaper/banner/bn_add_line.png)](https://lin.ee/ptiP6Md) [![](https://edm.bnext.com.tw/epaper/banner/fc_new_web.png)](https://fc.bnext.com.tw/?utm_source=web_bn&utm_medium=article_bottom)

<iframe allow="autoplay *; encrypted-media *; fullscreen *; clipboard-write" frameborder="0" height="175" src="https://embed.podcasts.apple.com/us/podcast/ep295-%E6%8A%95%E8%B3%87%E7%BE%8E%E5%9C%8B-%E9%99%A4%E4%BA%86%E4%BA%9E%E6%AD%B7%E6%A1%91%E9%82%A3-%E5%8F%B0%E5%BB%A0%E7%82%BA%E4%BD%95%E9%97%9C%E6%B3%A8%E5%BE%B7%E5%B7%9E-ft-%E5%BE%B7%E5%B7%9E%E5%A5%A7%E6%96%AF%E6%B1%80%E4%BA%9E%E6%B4%B2%E5%95%86%E6%9C%83%E6%9C%83%E9%95%B7mark-duval%E5%92%8C%E5%89%8D%E6%9C%83%E9%95%B7angelos/id1555393924?i=1000775072633"></iframe>

往下滑看下一篇文章

<video title="Advertisement" src="https://rr2---sn-ipoxu-un5ez.googlevideo.com/videoplayback?expire=1783414950&amp;ei=JlBMapK9LaLr7OsPr_WMsAs&amp;ip=60.250.84.232&amp;id=9667a93b37745e12&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=228&amp;met=1783386150,&amp;mh=fW&amp;mm=31&amp;mn=sn-ipoxu-un5ez&amp;ms=au&amp;mv=m&amp;mvi=2&amp;pl=21&amp;rms=au,au&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;vprv=1&amp;rqh=1&amp;dur=73.769&amp;lmt=1700838858208253&amp;mt=1783385839&amp;cpn=uOQp4xNfPhJKar1r&amp;txp=4432434&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,vprv,rqh,dur,lmt&amp;sig=AHEqNM4wRQIgCE0pGl5Jxi6QsORLe9xn63uv6alMV3gv7fDVa_yQ8yMCIQC_Tg3MQM8EULeubFy4D1XXaQyCW5aN4UeBh2DzSzy2jA==&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pl,rms&amp;lsig=APaTxxMwRQIgdk7pc8TDl5MJVSAzixM_PI1OwRd5wAIe44nzJ0z9h-wCIQDDtiy_bfum9uh2-C-zyaOpvJp7baGFd4jh8StwWQjojQ==" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.774.0_en.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_636711574%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C745150931%2Cnull%2C%5Bnull%2C1%5D%5D%2C%5Bnull%2C749060184%2Cnull%2C%5Bnull%2C128%5D%5D%2C%5B841585769%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B846355750%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45774999%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2Cnull%2C45792627%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45765927%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45661356%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C500%5D%5D%2C%5Bnull%2C45767902%2Cnull%2C%5Bnull%2C500%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761216%2Cnull%2Cnull%2C%5B1%5D%5D%5D%2C%5B%5B16%2C%5B%5B1%2C%5B%5B31089630%5D%2C%5B31089631%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95345206%5D%2C%5B95345207%2C%5B%5B45661356%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95382403%2C%5B%5B45794243%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95386528%2C%5B%5Bnull%2C45767902%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95390088%5D%2C%5B95390089%2C%5B%5B45784857%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B10%2C%5B%5B95391980%5D%2C%5B95391981%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fpagead%2Fmanaged%2Fjs%2Factiveview%2Fcurrent%2Frx_omid_video.js%5C%22%5D%5D%5D%5D%2C%5B95391982%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fomsdk%2Freleases%2Flive%2Fomid_monitoring_bin.js%5C%22%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95393784%5D%2C%5B95393785%2C%5B%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=2526268250331601&amp;pvsid=7811107394032255&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F91432%2Fxhejnc8rjurch7kzudri%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="169"></iframe>

即時熱門文章

[1 工程師與PM的價值被AI重寫？Claude Code負責人點名：未來團隊最缺的不是「建造者」，而是這2種人](https://www.bnext.com.tw/article/91395/ai-product-work-archetypes-from-handovers-to-prototypes) [2 AI課程學習地圖！Claude、Google、微軟⋯免費線上課一次收，完成還能拿證書](https://www.bnext.com.tw/article/91411/free-ai-online-courses-certificate-guide) [3 ChatGPT指令大全！80組提示詞一次整理，手工紙雕風、大地色塊插畫風、LINE貼圖全搞定](https://www.bnext.com.tw/article/90908/chatgpt-image-2-prompt-guide-complete) [4 2026下半年這8張信用卡要退場了！換發卡別、停卡日期、注意事項一次看](https://www.bnext.com.tw/article/91419/2026-credit-card-termination-list-8-co-branded-cards) [5 微風使出「焦土戰術」，限北車專櫃7月拆光光！北車將陷2個月服務黑暗期？](https://www.bnext.com.tw/article/91354/breeze-mitsukoshi-taipei-station-lease) [6 AI寫了70%程式碼又如何？微軟副總裁警示人才斷層危機：省下新人職缺，可能賠上企業未來](https://www.bnext.com.tw/article/91396/vxusik79gcnw5fqq1cxb)

<iframe frameborder="0" src="https://08116f5c15966253486fa646da2b576d.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="250" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe><iframe frameborder="0" src="https://08116f5c15966253486fa646da2b576d.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="600" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>![從客服到數位員工：宏華國際如何用 AI Agent 幫企業把握每一筆商機？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/az14-1782805324.png?w=900&output=webp)

2026.07.01 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

從客服到數位員工：宏華國際如何用 AI Agent 幫企業把握每一筆商機？

宏華國際將客服經驗模組化、場景化，打造出適用多元產業的 AI 客服代理人，協助企業降低導入門檻與試錯成本，加速 AI 落地應用。

---

分享

隨著生成式 AI 與 AI Agent 的快速發展，企業導入 AI 應用，早已不是「要不要」的是非題，而是「如何選對解決方案」的決策題。

以 AI 客服為例，市場上雖然有眾多解決方案，但真正能夠拉開差距的，不是技術本身，而是對產業場景、服務流程與顧客需求的理解，這是決定AI 能否改善營運效率、提升顧客體驗，並創造實際商業價值的關鍵。

深耕客服領域多年的宏華國際，憑藉著長期累積的第一線服務經驗，以及對產業應用場景的深刻洞察，打造出適用多元產業的 AI 客服代理人，透過將客服經驗模組化、場景化，協助企業降低 AI 導入門檻與試錯成本，讓 AI 不只是技術工具，而是真正能快速落地、創造營運效益的虛擬助手。

![宏華 AI 客服](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/xeid-1782959119.jpg?w=1200&output=webp)

圖／ 宏華國際

## 有溫度的 AI 服務，成為企業競爭力的新關鍵

宏華國際資深技術顧問李宗遠指出，對企業、尤其是服務業而言，客服的價值早已不只是接聽電話或回覆訊息，而是建立顧客信任、優化服務體驗的重要平台。透過客服，企業不僅能維持對外溝通的一致性與完整性，也能延伸服務時間與服務範圍，進而即時掌握顧客需求，有效承接每一次商機。

然而，過去要打造全天候客服，企業往往需要投入大量人力與管理成本。如今，AI大幅降低了建置與維運成本，24 小時服務不再是大企業的專利。透過 AI 客服代理人，即使是資源有限的中小企業，也能在不額外增加人力的前提下，快速建立客服機制，提供更貼心、更有溫度的服務，而這也將成為未來服務業的重要競爭力。

以餐飲業為例，消費者有聚餐需求時，通常會透過網路訂位系統同時向多家餐廳預約，等到接近用餐日時，再做出最終選擇。此時，餐飲業者若能在網路訂位流程中整合 AI 客服代理人，主動提供完整且貼心的一條龍服務，例如：推薦適合餐點、提醒停車資訊、確認特殊飲食需求，甚至提前規劃包廂與慶生活動等，讓顧客到店前就有好的服務體驗，自然有機會打動顧客的心、成為最終選擇。

李宗遠認為，AI 的價值不只是提升效率，更在於協助企業打造更有溫度的服務，這不僅有助於降低臨時取消率、提升訂單轉換率與顧客黏著度，更能建立差異化競爭優勢，讓 AI 從工具真正成為企業夥伴。

![宏華國際](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/8pci-1782875287.jpg?w=1200&output=webp)

宏華國際資深技術顧問李宗遠

圖／ 數位時代

## AI 客服下一階段，比的不是技術，而是誰更懂服務場景

然而，AI 要打造有溫度的服務，關鍵不在於採用了哪些技術，而是 AI 能否真正理解不同產業的服務場景與顧客需求。為此，宏華將多年來服務不同產業、不同場景所累積的第一線經驗，轉化為開發 AI 客服代理人的重要基礎，讓 AI 不只是回答問題，更能理解企業的服務流程，以及客戶的情緒與潛在需求，提供更貼近人性的互動體驗。

李宗遠認為，宏華 AI 客服代理人可以為企業帶來三項價值。第一項是將原本仰賴人工處理的流程自動化。AI 客服代理人不僅能線上服務客戶，完成訂位、預約、報修等流程，還能自動執行後續作業，例如：生成維修派工單，並通知師傅前往服務。

第二項則是優化顧客體驗。AI 客服代理人以多模態 AI 作為核心，可以進行語音、文字與影像的互動，因此，顧客透過電話、LINE、社群平台、網站或實體門市等留下的互動記錄，都能整合至同一平台，避免在服務過程中產生斷點。

第三項是延長服務時間與擴大服務範圍，承接更多商機。AI 客服代理人支援國語、台語、英語 24 小時應答，無論顧客在任何時間、透過何種管道聯絡，甚至使用外語，都能獲得即時回覆。

## 十多年客服經驗，如何成為宏華 AI 的護城河？

當 AI 客服的技術能力逐漸成熟，真正難以複製的，不是模型，而是背後累積的服務經驗，而這正是宏華最大的競爭力。

宏華國際新事業處協理曾世忠指出，宏華國際深耕客服領域超過十年，累積全台數一數二的服務量能與顧客互動經驗。從客服、門市到到府服務，這些第一線服務經驗，不僅讓團隊更理解真實客服情境，也更熟悉不同產業的服務流程與需求，成為 AI 客服代理人得以持續優化的關鍵。

![宏華國際](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/g3rb-1782723024.jpg?w=1200&output=webp)

宏華國際客服新事業處協理曾世忠

圖／ 數位時代

這些第一線經驗讓宏華非常清楚顧客互動過程中的各種變數，包括臨時修改需求、跳躍式提問、說話帶有情緒等。曾世忠表示，真實世界的客服互動很少按照既定腳本進行，許多AI在面對顧客突然改變想法或偏離原本的對話流程時，容易陷入重複問答或無法理解語意的困境，進而影響使用體驗。  
也正因此，宏華訓練的不只是 AI 的智商（知識理解、順暢對話），更重視服務情商（情緖分析），讓 AI 能理解對話情境、顧客情緒與需求變化，可以「秒問秒答、邊聽邊想」，而非只是依照固定腳本機械式回應。

除了理解真實客服互動情境外，長期服務不同產業客戶的經驗，也讓宏華更了解各產業面臨的服務情境與需求差異，並將這些產業 Know-how 模組化，發展出餐飲、技術服務、專業服務等產品包，企業只要完成需求完成設定，便能快速導入，大幅降低建置時間與試錯成本。

![宏華國際](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/5wxq-1782723024.jpg?w=1200&output=webp)

圖／ 數位時代

「AI Agent 的價值不只是協助企業服務客戶，更將成為企業營運的重要基礎設施。」曾世忠認為，未來，AI Agent 除了協助企業回應外部客戶需求，也將進一步應用於員工服務、人資諮詢等內部場景，成為串聯企業內外部溝通的重要角色。而宏華國際也將持續投入相關技術發展，協助企業在 AI 時代建立更穩定、更具韌性的營運能力。

即時熱門文章

[1 工程師與PM的價值被AI重寫？Claude Code負責人點名：未來團隊最缺的不是「建造者」，而是這2種人](https://www.bnext.com.tw/article/91395/ai-product-work-archetypes-from-handovers-to-prototypes) [2 AI課程學習地圖！Claude、Google、微軟⋯免費線上課一次收，完成還能拿證書](https://www.bnext.com.tw/article/91411/free-ai-online-courses-certificate-guide) [3 ChatGPT指令大全！80組提示詞一次整理，手工紙雕風、大地色塊插畫風、LINE貼圖全搞定](https://www.bnext.com.tw/article/90908/chatgpt-image-2-prompt-guide-complete) [4 2026下半年這8張信用卡要退場了！換發卡別、停卡日期、注意事項一次看](https://www.bnext.com.tw/article/91419/2026-credit-card-termination-list-8-co-branded-cards) [5 微風使出「焦土戰術」，限北車專櫃7月拆光光！北車將陷2個月服務黑暗期？](https://www.bnext.com.tw/article/91354/breeze-mitsukoshi-taipei-station-lease) [6 AI寫了70%程式碼又如何？微軟副總裁警示人才斷層危機：省下新人職缺，可能賠上企業未來](https://www.bnext.com.tw/article/91396/vxusik79gcnw5fqq1cxb)

[![代理式商務連動百兆商機](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/8e6m-1779880883.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130167)