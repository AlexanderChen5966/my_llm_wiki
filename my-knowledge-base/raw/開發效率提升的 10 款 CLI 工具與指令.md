---
title: "開發效率提升的 10 款 CLI 工具與指令"
source: "https://codelove.tw/@tony/post/aYWROx"
author:
published:
created: 2026-07-27
description: "「再做一次同樣的工作……」一邊盯著畫面看著，那時候我真的被錯誤折磨了很多次。每次追著 Git 的提交紀錄回頭找時，我都會覺得「這個每次都得手動輸入也太麻煩了吧」。而當我終於遇見 CLI 工具的那一刻，我感受到開發速度發生了劇烈的變化。如果你也正想著「有沒有什麼好用的工具呢」，那麼現在正是時候，讓我為..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月27日](https://codelove.tw/@tony/post/aYWROx "2026-07-27 08:09:43")

「再做一次同樣的工作……」一邊盯著畫面看著，那時候我真的被錯誤折磨了很多次。每次追著 Git 的提交紀錄回頭找時，我都會覺得「這個每次都得手動輸入也太麻煩了吧」。而當我終於遇見 CLI 工具的那一刻，我感受到開發速度發生了劇烈的變化。如果你也正想著「有沒有什麼好用的工具呢」，那麼現在正是時候，讓我為各位剛起步的工程師介紹我在現場學到的 10 款 CLI 工具。

## 把作業粗略加倍的「fzf」——頓悟般的搜尋體驗

我那時幾乎每天都在用 find 和 grep 的組合找程式碼。每次要在大量檔案中搜尋特定函式名稱或變數時，我都得在終端機重複輸入同樣的指令。「git grep」、「ack」、「ripgrep」……我全都試過了，但一直缺少的，就是「可互動式縮小範圍的搜尋」。

這就是「fzf」的登場。它是一款所謂的 fuzzy finder（模糊搜尋器），可以一邊用方向鍵選擇候選項，一邊逐步縮小範圍，那種感覺簡直正中開發者的心。實際用過之後，連指令補完都能做到，只要在 `git checkout` 後面接上 fzf，就能不用手打分支名稱直接選取。

具體來說，只要在 `.bashrc` 或 `.zshrc` 裡設定別名就可以了。我是這樣設定的。

```
# fzf 的設定
export FZF_DEFAULT_COMMAND='fd --type f --hidden --follow --exclude .git'
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"

# git branch + fzf 的組合
alias gco='git checkout $(git branch | fzf)'
```

光是這一行設定，就讓切換分支的作業幾乎瞬間完成。一開始我也懷疑「這真的能用嗎？」但連續用了幾天後，就真的感受到它的方便。開發中那種「什麼都能用點選來選」的感覺，大幅降低了心理負擔。

## 終於解決！用「peco」玩出生活效率的秘密搜尋術

除了 fzf 之外，在我的工具箱裡絕對不能少的就是「peco」。peco 是另一款用 Ruby 製作的 fuzzy finder，特色是設定彈性很高。特別吸引我的，是它能「從 Pull Request 或 Issue 編號無縫跳到相關程式碼」。

例如，當我想搜尋 GitHub 的 Issue 編號，並跳到對應內容的程式碼時，我會這樣用：

```
# 設定於 .zshrc
alias issue='gh issue list | peco --prompt="ISSUES> " | cut -d" " -f1'

# 使用範例
cd $(git rev-parse --show-toplevel)
open "$(issue)"
```

這個設定會讓終端機中的 `gh issue list` 結果交給 peco 顯示，接著只要用方向鍵選取，就能取得 Issue 的 URL。之後不會自動在瀏覽器打開，而是透過 `open` 指令來開啟 URL，但光是這樣就已經讓日常工作輕鬆很多了。

## 早點用就好了！「ripgrep」的地獄級除錯體驗

在我的環境裡，一開始我是用「ag」或「grep」。但是一旦到了大型專案，搜尋速度就變慢，偶爾還會遇到「不知為何變成 0 筆」這種奇怪行為。尤其是使用正規表示式搜尋時，常常因為 `.`（點）或 `*` （星號）造成和預期不同的結果，除錯的時間白白浪費掉了。

這就是「ripgrep（rg）」登場的時候。它以超高速搜尋引擎著稱，而我最深刻感受到的是「預設就會排除隱藏檔與.git 目錄」。以前得輸入 `find . -name "*.js"` 的工作，現在只要 `rg ○○` 就能完成。

實際上我會這樣使用：

```
# 搜尋整個專案
rg "useState"

# 只搜尋特定檔案類型
rg "function hoge" -t js

# 輸出行號（方便在 IDE 中跳轉）
rg -n "const [a-z]" src/
```

光是這樣，搜尋所需的時間就從數秒縮短到數毫秒。特別是在切換分支前後，曾經很多次都有「搜尋太慢很煩」的經驗，但自從開始使用 ripgrep 之後，這個問題也完全解決了。

## 給工程師的「tmux」——把終端機發揮到極致的生活方式

「又忘了再開一個終端機……」我曾經反覆犯同樣的錯。當你想同時推進多個任務時，很容易讓視窗越開越多，最後陷入「我剛剛到底在哪個視窗做什麼」的狀態。

這時我找到的是「tmux」。它是一款終端機多工器（把終端機分割並管理的工具），可以在一個視窗裡操作多個 pane。我是這樣使用的：

1. 左邊編輯程式碼（vim 或 code）
2. 右上執行測試（npm test）
3. 右下操作 Git（git status、git diff）

這樣的配置可以儲存成一個「session」，隔天進來只要 `tmux attach` ，同樣的畫面就會被還原。一開始我也想過「這真的有必要嗎？」但實際開始用之後，立刻感受到「切換視窗瞬間完成」、「工作很容易延續下去」的舒適感。

設定很簡單，只要在 `.tmux.conf` 寫入以下設定即可。

```
# 快捷鍵設定
unbind C-b
set -g prefix C-t
bind C-t send-prefix

# 啟用滑鼠操作
set -g mouse on
```

## 每天 5 分鐘就好！用「task」自動化工作流程

我最不擅長的就是「環境建置」了。「npm install」、「webpack 設定」、「啟動伺服器」……每個都有複雜的指令，只要一出錯，我就會不安地想著「這要怎麼修啊」。

就在這時我找到了「task」。這是一個用 Go 寫成的工具，可以作為 Makefile 的替代方案使用。你可以用 YAML 格式定義任務，並透過一個指令執行。

例如，像這樣設定後，專案初始化就會變得非常輕鬆。

```
# Taskfile.yml
version: '3'

tasks:
  setup:
    desc: 安裝相依套件
    cmds:
      - npm install
      - npm run build

  dev:
    desc: 啟動開發伺服器
    cmds:
      - npm run dev

  test:
    desc: 執行測試
    cmds:
      - npm test
```

這樣只要輸入 `task dev` ，開發伺服器就會啟動。起初我也疑惑「這跟 shell script 有什麼不同？」但它在錯誤處理和相依關係管理上更方便，結果讓我對「環境建置的不安」大幅降低了。

## 方便到不行的「httpie」——API 開發的救世主

「用 curl 發 POST 時，每次 JSON 都會有語法錯誤……」我在測試 API 時經常卡關。尤其是包含驗證權杖和標頭的請求，只要手動輸入，幾乎一定會出錯。

這就是「httpie」登場的時候。它最大的魅力，在於可讀性高的輸出，以及能用簡單語法撰寫 HTTP 請求。我是這樣活用它的：

```
# GET 請求
http GET https://api.example.com/users

# POST 請求（自動設定 JSON）
http POST https://api.example.com/users name=太郎 age:=30

# 附帶驗證標頭
http GET https://api.example.com/protected Authorization:"Bearer $TOKEN"
```

JSON 語法錯誤從此消失，輸出也變成彩色，更容易閱讀。特別是當回應內容變複雜時，它會幫忙做 pretty print，這讓 API 開發者的壓力大幅減少。

## 方便到不行的「direnv」——環境變數管理變輕鬆了

「我想要在每個目錄自動載入.env 檔」這個願望，終於透過「direnv」實現了。以前我每次都要手動輸入 `source .env` ，但只要一切換目錄就很容易忘記，結果老是遇到「環境不一致」的錯誤。

開始使用 direnv 之後，只要在專案根目錄建立 `.envrc` ，它就會自動幫你載入環境變數。

```
# .envrc 範例
layout node
export NODE_ENV=development
export DEBUG=true
```

只要做這樣的設定，進入目錄的瞬間環境就會自動完成設定。一開始我也覺得「真的會自動處理嗎？」有點半信半疑，但實際持續使用後，就明顯感受到「再也不會忘記」了。

## 大幅提升生產力的「gh」CLI——從終端機操作 GitHub

「先用瀏覽器確認儲存庫、建立提交、再開 Pull Request……」這種流程實在很麻煩。我一開始覺得 GitHub CLI（gh）「應該沒必要吧」，但實際用過後，我真的感受到它很方便。

特別是「建立 PR」和「搜尋 issue」能直接在終端機完成，這點非常吸引人。

```
# 建立 Pull Request
gh pr create --title "新增功能" --body "已新增 XX 功能"

# 顯示自己的 PR 清單
gh pr list --author me

# 對特定 issue 留言
gh issue comment 123 --body "驗證完成了"
```

開始使用這些指令後，前往瀏覽器的時間大幅減少。尤其是「Pull Request 的審查請求」與「合併作業」變得明顯更輕鬆，結果讓我實際感受到「程式碼審查的週期變快了」。

## 方便到不行的「delta」——讓 git diff 更好讀的魔法

你有沒有過「git diff 的輸出太單調，根本看不出改了什麼……」的經驗？我一開始覺得有顏色的 diff 工具「應該不太重要」；但多虧了「delta」，我的想法完全改變了。

delta 會把 git 的 diff 輸出按行上色，並自動高亮差異。設定也很簡單，只要加到 `.gitconfig` 即可。

```
[core]
    pager = delta
[delta]
    syntax-theme = Monokai Extended
[git]
    difftool = delta
```

這樣執行 `git diff` 時，修改處會以綠色和紅色高亮顯示，一眼就能看出改了哪裡。特別是面對「變動很多的檔案」時，以前常常因為追不到差異而「看不出到底改了什麼」，而這個問題也因此解決了。

## 讓工作變得大幅輕鬆的「http-server」——簡單的靜態伺服器

「想在本機確認 HTML 檔，但 npm 外掛怎樣都不順……」以前我通常會用 `python -m http.server` 。不過我有希望能在純 npm 環境中完成的需求，因此遇見了名為「http-server」的套件。

安裝只要 `npm i -g http-server` ，啟動也很單純，只要輸入 `http-server` 就行。特別是它很容易指定「連接埠」和「主機」，非常適合在開發中做確認。

```
# 以預設連接埠（8080）啟動
http-server

# 指定連接埠
http-server -p 3000

# 指定特定目錄
http-server ./dist --cors
```

這種簡單易用，讓我更常做「順手確認一下」，結果也讓我感受到「開發流程變快了」。

## 總結

這次介紹的 10 款 CLI 工具，都是我在感受到「作業太慢」、「錯誤太多」、「不安太多」的瞬間，一個一個導入的。起初我也曾困惑「這真的有必要嗎？」但在實際持續使用後，確實感受到「開發品質提升了」、「更有自信了」、「壓力減少了」。

剛起步的工程師們，也請務必一個一個試試看。即使一開始只是覺得「這個很方便」，日常工作也會一點一點變輕鬆。而那些累積，將會成為走向「屬於自己的開發者」的第一步。

CLI 工具帶給你的不只是「方便」，還有「能以自己的步調成長」的感覺。不要被「應該還有更好的方法」這種常識綁住，持續找出「讓自己更舒適的方法」，最終就會通往更專業的道路。

---

原文出處： [https://qiita.com/NekoByte/items/efa81aaa8a61d3478568](https://qiita.com/NekoByte/items/efa81aaa8a61d3478568)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aYWROx) [8瀏覽](https://codelove.tw/@tony/post/aYWROx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aYWROx)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝10 💬1 ❤️6

301

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬6 ❤️3

113

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1785117592&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaYWROx&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTg0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjE4NCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTg0Il1dLDBd&amp;dt=1785117592650&amp;bpp=2&amp;bdt=131&amp;idt=64&amp;shv=r20260721&amp;mjsv=m202607210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1784854184%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1784854184%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1784854184%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5840283775756&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1619&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95395796%2C95396139%2C95396923%2C31099899%2C95391233%2C95396252&amp;oid=2&amp;pvsid=6801173844527385&amp;tmod=975530735&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=96" title="Advertisement" aria-label="Advertisement"></iframe><iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="320" height="50" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260721/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>