---
title: "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本"
source: "https://www.kocpc.com.tw/archives/661684"
author:
  - "[[Rocky]]"
published: 2026-08-04
created: 2026-08-05
description: "隨著 AI 操作電腦的能力越來越強，很多人一定會想把每天重複都工作，都交給 AI 處理，這時就會需要 Skill 技能檔。為了讓大家能更快獲得自己需要的 Skill，微軟最近就開源一款名為 Skill Recorder 的工具，你只要親自示範一次，AI 就會嘗試理解你想完成的任務，並將過程整理成能重複使用的 Skill 或 Automation。其實就跟 Codex 的「錄製 Skill」和 Claude 的「Record a skill」功能一樣。"
tags:
  - "clippings"
status: processed
---
隨著 AI 操作電腦的能力越來越強，很多人一定會想把每天重複都工作，都交給 AI 處理，這時就會需要 Skill 技能檔。

為了讓大家能更快獲得自己需要的 Skill，微軟最近就開源一款名為 Skill Recorder 的工具，你只要親自示範一次，AI 就會嘗試理解你想完成的任務，並將過程整理成能重複使用的 Skill 或 Automation。其實就跟 Codex 的「錄製 Skill」和 Claude 的「Record a skill」功能一樣。

Windows 和 Mac 都能裝，下面就示範給你看怎麼操作。

## 微軟 Skill Recorder 開源工具，把操作畫面錄下來，就能讓 AI 學會整套工作流程

Skill Recorder 是一款以 Electron 製作的桌面工具，整套流程可以分成 Record、Analyze 和 Create 三個階段。

錄製期間，Skill Recorder 會收集目前使用的 App、視窗與文件標題、視窗切換、瀏覽器網址、螢幕畫面，以及剪貼簿中複製文字的短預覽。此外，如果開啟 Narrate 功能，你也能一邊操作一邊用語音說明「為什麼現在要做這件事」，幫助 AI 理解那些光看畫面不容易判斷的操作。

語音不是直接在雲端轉錄，而是透過本機的 Whisper 模型處理。支援 99 種語言（包括中文），第一次使用時要由使用者同意下載約 252MB 的模型，後續轉錄則能在本機完成。

錄製好後，即可讓 AI 開始分析內容，完成時即可選擇建立 Skill 或 Automation，操作非常簡單。當然，如果有 AI 判斷錯誤或有誤的地方，後續你也能手動修改。

有一點要提醒的是，用這工具製作出來的 Skill，目前會依照微軟 Scout 或 Cowork 的能力來生成內容，因此其他 AI 代理不一定能直接使用，通常還需要修改。

[取得 Skill Recorder](https://github.com/microsoft/skill-recorder/releases)

Skill Recorder 目前沒有提供安裝檔，需透過終端機或 PowerShell 的指令來安裝。當然，也能直接叫 AI 幫你安裝。下方是安裝指令（也能在 Release 頁面找到）

Windows 安裝指令（PowerShell）

`$commit="32fd0b57e02c3ea1e016cca0d64e59052e93a9b9"; $env:SKILL_RECORDER_COMMIT=$commit; irm "https://raw.githubusercontent.com/microsoft/skill-recorder/$commit/install.ps1" | iex`

Mac 安裝指令

`commit="32fd0b57e02c3ea1e016cca0d64e59052e93a9b9"; curl -fsSL "https://raw.githubusercontent.com/microsoft/skill-recorder/$commit/install.sh" | SKILL_RECORDER_COMMIT="$commit" bash`

![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804145501_0_8424fa.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

我的是 Mac 版，因此在終端機輸入 Mac 的安裝指令：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804145534_0_41f72e.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

close

kocpc dynamic layout demo

<video title="Advertisement" src="https://rr2---sn-ipoxu-un5e7.googlevideo.com/videoplayback?expire=1785921916&amp;ei=_JByavKFI8K21d8PzMnV4Ao&amp;ip=60.250.84.232&amp;id=e2de62400acb04b4&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=177&amp;met=1785893116,&amp;mh=8L&amp;mm=31&amp;mn=sn-ipoxu-un5e7&amp;ms=au&amp;mv=m&amp;mvi=2&amp;pl=21&amp;rms=au,au&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;rqh=1&amp;dur=11.424&amp;lmt=1780896010518504&amp;mt=1785892636&amp;cpn=z_HOWjshDJNqkqZM&amp;txp=6318224&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,rqh,dur,lmt&amp;sig=AE0s2JYwRAIgO48QMGyIUYkp4oZZrpfRzMh7yKzsioZ6WcuuWmKYsXgCIEJvl1LudTtSgvg2qO4VfCMVbusfQKST7PMfqzadx89D&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pl,rms&amp;lsig=APaTxxMwRQIhAPZhqM8c7xiClr8ord5w3lJZbOV8MHfbniAHS832fBVzAiBUUatfs7oFG9NwS3kZB4hvGIymgyGWRksmoj0EaGoc4Q==" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.780.3_zh_tw.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_257259880%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C956056724%2Cnull%2C%5Bnull%2C128%5D%5D%2C%5Bnull%2C745150931%2Cnull%2C%5Bnull%2C1%5D%5D%2C%5Bnull%2C749060184%2Cnull%2C%5Bnull%2C128%5D%5D%2C%5B841585769%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B846355750%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B905081831%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45824086%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45774999%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2Cnull%2C45792627%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45661356%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761216%2Cnull%2Cnull%2C%5B1%5D%5D%5D%2C%5B%5B16%2C%5B%5B1%2C%5B%5B31089630%5D%2C%5B31089631%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B10%2C%5B%5B95391980%5D%2C%5B95391981%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fpagead%2Fmanaged%2Fjs%2Factiveview%2Fcurrent%2Frx_omid_video.js%5C%22%5D%5D%5D%5D%2C%5B95391982%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fomsdk%2Freleases%2Flive%2Fomid_monitoring_bin.js%5C%22%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95393784%5D%2C%5B95393785%2C%5B%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95396036%5D%2C%5B95396037%2C%5B%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95396834%5D%2C%5B95396835%2C%5B%5B45816264%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398187%5D%2C%5B95398188%2C%5B%5B945081832%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95398415%5D%2C%5B95398416%2C%5B%5B45824086%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=3561258037996057&amp;pvsid=776954996868166&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.kocpc.com.tw%2Farchives%2F661684%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="168.75"></iframe>

pause

volume\_mute

AD

00:00

00:00

00:59

 <video controls=""><source src="https://gnetwork.gliastudios.com/gnetwork/www.kocpc.com.tw/www.kocpc.com.tw-1776849532.388493.mp4?verify=1785892736-CM%2BVyFu8GQDRpWSC2KIzFiQ0%2FQ8VC%2By1cjF8TiI%2FXks%3D"> <source src="https://gnetwork.b-cdn.net/studio_backend/www.kocpc.com.tw/www.kocpc.com.tw-1776849532.388493.mp4?token=bozTo1eLLaehoyp1FqqQo8W1x9FpunoFKG2Q7XsIe3o&amp;expires=1785979136"></video>

就會開始安裝，等待它跑完：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804145553_0_005c53.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

Mac 安裝完成後，系統會在 \`~/Applications\` 建立「Skill Recorder（Source）」App，往後可透過 Spotlight、Launchpad 或 Dock 再次開啟。如果你希望關閉終端機後程式還會繼續執行，微軟還有提供加入 \`SKILL\_RECORDER\_DETACHED=1\` 的背景啟動指令。

Windows 安裝完成後，則會在桌面和開始功能表看到「Skill Recorder（Source）」捷徑。

下圖就是 Skill Recorder 的介面，按上方的錄製按鈕，即可開始錄製你的操作。Narrate 是下語音指令用的，如果你有需要，記得要先下載模型，按下方 voice tanscription 右邊的 download。另外也需要串接你的 GitHub 帳號才能使用，沒有的人就免費註冊一下：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804145816_0_5b45e3.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

按 Narrate 右邊的設定圖示，可調整語言和麥克風設定，語言部分有中文選項（Chinese）。如果你不需要語音，Narrate 保持關閉就好，可以直接開始錄製：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804150202_0_5abc6c.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

開始錄製後，首次會要求授予螢幕錄影權限。如果你有使用語音解說，還需要允許麥克風權限。接著就按照你平常工作流進行操作即可：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804150251_0_2a6835.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

錄製好後，下方會出現「1 ready to analyze」的提示：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804150439_0_1475e4.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

點入後會看到分析錄影「Analyze recording」的功能：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804150544_0_4b7eee.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

AI 就會開始分析：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804150700_0_077d78.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

分析完成後，畫面會顯示 AI 判斷出的任務意圖與有順序的步驟。建議不要看到內容通順就直接按下方生成（Create），最好逐項確認固定網址、檔案路徑、資料來源、送出動作與例外條件是否正確。如果是會寄信、送出表單、建立資料或刪除內容的流程，更要確認 AI 沒有把原本的判斷條件省略：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804150719_0_e505ec.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

你也能自行添加更多步驟：

![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804150758_0_6ffd2b.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

接著就選擇你是要在哪個平台使用，有 Scout skill、Scout automation、Cowork skill：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804150850_0_8341a0.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

我選擇 Scout skill，再按 Plan the skill：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804151607_0_ac1b63.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

你可以直接加到 Scout，或是輸出成 SKILL.md 檔（Export）：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804151635_0_61e4a4.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")

輸出後，你就能在目的地資料夾中找到這個 SKILL.md：  
![Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260804151723_0_8e4917.jpg "Skill Recorder 微軟最新開源 AI 工具！錄製一次螢幕操作，自動生成工作流的 Skill 技能腳本 - 電腦王阿達")