---
title: "iOS 裝置越獄新工具 Relaxin Lite：只要打開網頁就能 Jailbreak"
source: "https://www.kocpc.com.tw/archives/665237"
author:
  - "[[達小編]]"
published: 2026-08-16
created: 2026-08-17
description: "近日 iOS 越獄開發者 82Flex 在 X 上展示了一段最新的 iPhone 越獄（Jailbreak）影片：一部 iPhone 15 Pro Max 打開 Safari 瀏覽器、進入一個網頁，幾秒後裝置重啟，越獄完成。不需要電腦，也不需要 sideload IPA 或任何額外操作。這款實驗性的工具叫做"
tags:
  - "clippings"
status: "processed"
---
近日 iOS 越獄開發者 82Flex 在 X 上展示了一段最新的 iPhone 越獄（Jailbreak）影片：一部 iPhone 15 Pro Max 打開 Safari 瀏覽器、進入一個網頁，幾秒後裝置重啟，越獄完成。不需要電腦，也不需要 sideload IPA 或任何額外操作。這款實驗性的工具叫做 Relaxin Lite，是 iOS 17 越獄工具 Relaxin 的瀏覽器版本，它的出現讓越獄圈回想起 15 年前那段「開網頁就能越獄」的年代。

![](https://x.com/i/status/2087571122502119555)

### 什麼是 JailbreakMe？2010 年時的越獄年代

對於年輕的果粉來說可能不知道「越獄」是做什麼的，早期的 iOS 系統相當封閉，許多功能只能以「陽春」形容，所以許多現在理所當然的功能在當時都必須透過越獄才能實現（當時用最多的就是電話錄音），但隨著 iOS 版本更新與蘋果都會「借鑑」Android 與越獄的獨特功能，現在幾乎已經沒有越獄的必要了（現在幾乎都是玩虛擬機才會使用到越獄功能），而且現在 iOS 系統漏洞也越來越少，導致現在也幾乎沒什麼人在玩越獄（編按：現在我們也不建議一般人去越獄，因為有資安風險）。  
![ Cydia ](https://www.kocpc.com.tw/wp-content/uploads/2018/12/1545017611-b0b1d3bf4540b80b122c2ec981ac2f1d.jpg "iOS 裝置越獄新工具 Relaxin Lite：只要打開網頁就能 Jailbreak - 電腦王阿達")

不過在 2010 到 2011 年是 iOS 越獄的全盛時期，在當時一位化名 Comex（本名 Nicholas Allegra）的天才駭客開發了 JailbreakMe 系列工具，讓使用者只需要在 Safari 瀏覽器輸入一個網址，點幾下就能完成越獄。JailbreakMe 2.0 利用了 FreeType 字體解析器的 PDF 漏洞，而 3.0 版則再次利用 PDF 字體漏洞，累計超過 200 萬台裝置透過這個方式越獄。

![IMG_0090.JPG](https://www.kocpc.com.tw/wp-content/uploads/images/pic.pimg.tw/kocpc/4447c476d1211b5a81cfb5bd2fc42570.jpg "iOS 裝置越獄新工具 Relaxin Lite：只要打開網頁就能 Jailbreak - 電腦王阿達")

Comex 後來被蘋果以實習生身份招安，JailbreakMe 從此成為歷史。隨著蘋果持續強化 iOS 安全機制（代碼簽名、沙盒機制、Pointer Authentication Code（PAC）、Page Protection Layer（PPL）），瀏覽器越獄在過去十幾年幾乎成為不可能的任務。越獄工具逐漸走向需要 sideload IPA、透過電腦安裝的路線，最知名的包括 Dopamine（由 opa334 開發）和 palera1n。直到 Relaxin Lite 的出現。

### Relaxin：iOS 17 越獄的破口

Relaxin 是由 OwnGoal Studio 開發、82Flex 主導的 iOS 越獄工具，採用 roothide 架構（與 Dopamine 類似的半不繫結越獄），正式版支援 iOS 16.5.1 至 17.3.1，涵蓋 A12 至 A17 以及 M1、M2 晶片的裝置。  
![iOS 裝置越獄新工具 Relaxin Lite：只要打開網頁就能 Jailbreak - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260816175916_0_ddb02f.jpg "iOS 裝置越獄新工具 Relaxin Lite：只要打開網頁就能 Jailbreak - 電腦王阿達")

close

kocpc dynamic layout demo

arrow\_forward\_ios

閱讀文章

<video title="Advertisement" src="https://rr3---sn-ipoxu-un5y.googlevideo.com/videoplayback?expire=1786958553&amp;ei=WWKCaqSzM7WT1d8Pj4OmsQk&amp;ip=60.250.84.232&amp;id=a2dbcd24701cfe65&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=196&amp;met=1786929753,&amp;mh=N_&amp;mm=31&amp;mn=sn-ipoxu-un5y&amp;ms=au&amp;mv=m&amp;mvi=3&amp;pcm2cms=yes&amp;pl=21&amp;rms=au,au&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;vprv=1&amp;rqh=1&amp;dur=15.046&amp;lmt=1768374276648015&amp;mt=1786929161&amp;cpn=J81Hqt51-tFqZ3nd&amp;txp=6218224&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,vprv,rqh,dur,lmt&amp;sig=AE0s2JYwRQIgYOpXxq9XCYL9kyZQP5UPfgdoYXwsRlldStdy2Qp5T6ECIQCy-zfLfZy5H_PSH9aoE8Qxbz74j5dwhHFDpXknArJOyw==&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pcm2cms,pl,rms&amp;lsig=APaTxxMwRQIgERuOVwzz_muoQKOmMJKaJ6JfFkWPL0FnMfN5sB_pIB4CIQD8ci40hEc76rXqq3n55dufQHATw_Uo09MxnQDqrpO1Rg==" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.784.0_zh_tw.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_1583815326%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B962325777%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C956056724%2Cnull%2C%5Bnull%2C128%5D%5D%2C%5Bnull%2C745150931%2Cnull%2C%5Bnull%2C1%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45824086%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2Cnull%2C45792627%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45661356%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B1%2C%5B%5B31089630%5D%2C%5B31089631%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B50%2C%5B%5B95393784%5D%2C%5B95393785%2C%5B%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95396036%5D%2C%5B95396037%2C%5B%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95396834%5D%2C%5B95396835%2C%5B%5B45816264%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398187%5D%2C%5B95398188%2C%5B%5B945081832%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95398415%5D%2C%5B95398416%2C%5B%5B45824086%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398837%5D%2C%5B95398838%2C%5B%5Bnull%2C960449109%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%5D%5D%2C%5B100%2C%5B%5B122880242%5D%2C%5B122880243%2C%5B%5Bnull%2C956056724%2Cnull%2C%5B%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B44801479%2C95322027%2C95331589%2C95332046%2C95393785%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=1796223726234945&amp;pvsid=5476150350398861&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.kocpc.com.tw%2Farchives%2F665237%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="168.75"></iframe>

pause

volume\_mute

AD

00:00

00:00

00:59

 <video controls=""><source src="https://gnetwork.gliastudios.com/gnetwork/www.kocpc.com.tw/www.kocpc.com.tw-1776849532.388493.mp4?verify=1786928880-9f1ZUud8JVRC5e7lTwKQ9ZYZ06QZKvntSPPHCXd2dPg%3D"> <source src="https://gnetwork.b-cdn.net/studio_backend/www.kocpc.com.tw/www.kocpc.com.tw-1776849532.388493.mp4?token=EHV4Bd14GQY9oa1mxB-L9C9B-m9DvsUXFEI3O1Rw2L0&amp;expires=1787015280"></video>

Relaxin 的發展時間線如下：

- **2026 年 7 月 23 日** ：82Flex 首次展示在 iPhone 15 Pro Max（iOS 17.1.1）上 10 秒完成越獄，使用 Gruber KRW + backboardd GPU reset + Rocket 漏洞組合，成功率接近 100%
- **2026 年 7 月 27 日** ：進入封閉測試階段
- **2026 年 7 月 28 日** ：AI 工具在 3 小時內恢復了 Coruna 對 A14/M1 裝置的支援，擴大了相容範圍
- **2026 年 8 月 2 日** ：opa334 的 Crane 插件在 Relaxin 上正常運作，多容器推播通知問題獲得修復
- **2026 年 8 月 3 日** ：正式宣布支援 A12-A17、M1-M2 全系列裝置
- **2026 年 8 月 5 日起** ：公開測試版釋出，社群開始大規模安裝測試
- **2026 年 8 月 12 日** ：Relaxin Lite 瀏覽器越獄版本首次展示

### Relaxin Lite 的祕密武器：Coruna 漏洞鏈

Relaxin Lite 能從 Safari 直接執行越獄的關鍵，在於它使用了 Coruna 瀏覽器漏洞鏈。而 Coruna 的故事本身，就是 2026 年資安圈最戲劇性的事件之一。Coruna 是一個 iOS 漏洞利用工具包，包含 5 條完整的攻擊鏈與 23 個獨立漏洞，覆蓋 iOS 13.0 至 17.2.1。它最初是商業間諜軟體的一部分，由監控公司開發，用於對特定目標進行監控。2026 年 3 月，Google 威脅情報小組（GTIG）公開揭露了這個工具包的存在。

<iframe sandbox="allow-scripts" title="政府級 iOS 漏洞工具「Coruna」外洩，逾 4.2 萬部舊 iPhone 遭入侵：Apple 緊急釋出安全性更新 — 電腦王阿達" src="https://www.kocpc.com.tw/archives/634464/embed#?secret=DBIZbu5hSi#?secret=5B0V3Bt5m0" width="500" height="284" frameborder="0"></iframe>

根據 Google 的調查，Coruna 自 2025 年 2 月開始在多個威脅行為者之間流轉：從商業監控公司流出，被疑似俄羅斯駭客組織用於攻擊烏克蘭網站的訪客，最後落入以中國為基地的犯罪集團手中，用於竊取加密貨幣錢包。iVerify 的研究人員形容這是「我們觀察到的最顯著案例之一：精密的間諜軟體能力從商業監控廠商擴散到國家級行為者，最終落入大規模犯罪營運的手中」。

2026 年 3 月，Coruna 漏洞工具包的程式碼被洩漏並上傳至 GitHub，安全研究人員與越獄社群開始分析並改造這些漏洞。這些原本用於監控與攻擊的工具，被轉化為越獄開發的資源。82Flex 團隊利用 Coruna 中的 WebKit RCE（遠端程式碼執行）漏洞鏈，搭配 PAC 繞過技術，打造出了可以從 Safari 直接觸發的越獄流程。

Coruna 工具包中部分漏洞與 2023 年卡巴斯基揭露的 Operation Triangulation 行動使用了相同的零日漏洞。Operation Triangulation 被俄羅斯政府指控為美國國家安全局（NSA）所為，用於監控數千台俄羅斯境內的 iPhone。

### 從間諜工具到解放工具：越獄圈的黑色幽默

Coruna 原本是被設計用來監控手機使用者的間諜工具，在被揭露、洩漏之後，反而成為了讓使用者奪回裝置控制權的越獄基礎設施。82Flex 在 X 上表示「我們使用經過實戰驗證的 AI 工具來驗證越獄後的穩定性」，並強調「沒有 opa334 多年對 Dopamine 的奉獻，這項工作就不會存在」。

![](https://x.com/i/status/2079988220306288825)

社群對 Relaxin 的反應相當正面。Crane（opa334 開發的多帳號容器插件）已經在 Relaxin 上完美運作，Dotto++、Touch-Viz、MilkyWayReborn 等熱門插件也陸續宣布支援。越獄圈資安帳號 @onejailbreak\_ 持續追蹤報導各項相容性進展，AppEnumGuard 甚至修補了 Coruna 漏洞工具包中 CVE-2025-31207 的安全問題。

### Relaxin Lite 限制與現狀

Relaxin Lite 目前仍是實驗性質，尚未公開釋出。幾個重要的限制：

- **版本限制** ：Coruna 瀏覽器漏洞鏈目前只覆蓋 arm64e 裝置的 iOS 17.0 至 17.2.x，17.3 以上需要使用 DarkSword（支援到 17.3.1，但速度較慢）
- **穩定性** ：WebKit 階段可能需要多次嘗試才能成功
- **半不繫結** ：重開機後需要重新執行越獄程序
- **原始碼** ：預計 8 月底公開

82Flex 也展示了在 iPhone 15 Pro Max 上 10 秒完成傳統 IPA 安裝式越獄的流程，使用的漏洞組合為 Gruber KRW + backboardd GPU reset + Rocket，成功率接近 100%。

![](https://x.com/i/status/2080265673524277468)

目前 Relaxin 正式版（IPA 安裝方式）已進入公開測試，支援 iOS 16.5.1 至 17.3.1、A12 至 A17 及 M1/M2 裝置。至於 Relaxin Lite 何時能公開提供 Safari 瀏覽器越獄，82Flex 尚未給出明確時間表。