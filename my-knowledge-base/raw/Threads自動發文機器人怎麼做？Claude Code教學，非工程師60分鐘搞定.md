---
title: "Threads自動發文機器人怎麼做？Claude Code教學，非工程師60分鐘搞定"
source: "https://www.bnext.com.tw/article/91812/claude-code-threads-auto-poster"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-08-11
created: 2026-08-11
description: "Threads發文機器人怎麼做？我用Claude Code做了一個機器人，萃取voice profile、設排程⋯⋯整個過程不用工程師背景也做得到！"
tags:
  - "clippings"
status: processed
---
> 先短答給趕時間的人：每天自動發 Threads 的小機器人，難的不是技術，而是寫出來像不像你。先萃取一份 voice profile，也就是寫給 AI 看的「我平常怎麼講話」說明書：把自己過去 8 到 15 篇真實貼文交給 Claude 分析，第一版大概有 70 分，再自己校對兩三輪。之後每次寫稿前，都先讓 AI 讀這份文件。  
> 素材來源要選每天都有新內容、又能取得公開資料的；我選的是 GitHub 當天的熱門專案。整套流程大約 30 到 60 分鐘可以完成。

我每天都想在 Threads 分享一點東西，把做東西的過程公開記下來。

但「想分享」和「想得到分享文要怎麼寫」，是兩回事。很多時候，我打開 App 想了 10 分鐘，最後又關掉，因為腦袋空空，不知道要講什麼。

後來，我用 Claude Code 做了一個小機器人，每天晚上 8 點自動執行。它會挑選 GitHub 當天的熱門專案，用我的口吻寫成一段介紹，再發布到 Threads。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

上線至今，它沒有出過狀況；發出去的貼文，也很少有人看出是機器人寫的。

接下來，我會從 voice profile、素材來源到定時排程，一步一步拆解這個機器人的做法。

這篇是一步一步的教學，讀完你會做出三樣東西：

- 一份自己的 voice profile 文件，作用是讓 AI 寫得像你，而不是像一台機器
- 一個會自動抓素材、生稿、發文的小機器人
- 一個到點觸發的排程

範例是 Threads 加 GitHub，但任何「自動發文機器人」都套得上，像是 X、LinkedIn、IG 文案、Bluesky。30-60 分鐘可以全部跑完。

### 目錄（點擊可直接前往該段落）：

- [自動發文機器人為什麼對 build-in-public 這麼重要？](#sec-1)
- [怎麼讓 AI 寫出來的東西像你，而不是像機器人？](#sec-2)
- [Threads 自動發文機器人怎麼從 0 做到 100？（5 步做完）](#sec-3)
- [中文在 Windows PowerShell 變成亂碼怎麼解？](#sec-4)
- [怎麼讓機器人每天 8 點自己起床跑？](#sec-5)

## 自動發文機器人為什麼對 build-in-public 這麼重要？

持續公開記錄最難的地方，其實是每天晚上累了之後，不想再花腦力選題、寫稿、貼出去。這篇自動發文機器人教學，想解決的就是把「每天要分享什麼」，從你的待辦清單上拿掉。

先看選素材來源該看什麼？我自己抓 3 個原則。

- 每天都有新內容，不會重複，像是 GitHub Trending、Hacker News、各家更新紀錄、PapersWithCode。
	- 跟你的品牌軸線對齊。我做 AI workflow，所以選 GitHub Trending；交易者可以選 Finviz 或 TradingView，設計師可以選 Awwwards。
	- 有公開 API 或 RSS，不用寫爬蟲去對抗反爬機制。

我選 GitHub Trending，三條原則剛好都中。每天都有新的熱門專案，跟我的 AI workflow 品牌對齊，而且有公開 API 抓得到。

其他軸線的素材，可以用 RSS，或自己寫一個簡單的爬蟲，下面的步驟一樣都能套用。

## 怎麼讓 AI 寫出來的東西像你，而不是像機器人？

做自動發文機器人，最重要的其實是「寫出來像不像你」，技術反而是其次。如果貼文長得像 ChatGPT 那種工商風格，大家一看就知道是機器人，發了還不如不發。

要讓 AI 寫得像你，得先餵一份「voice profile（你的寫作口吻說明書）」當教材。

voice profile 的萃取，分成下面 4 個步驟：

### 步驟 1：收集樣本

把過去 8-15 篇自己寫的真實貼文整理成一個檔案，Threads、X、LinkedIn 都可以。8 篇是最低門檻，15 篇最理想，少於 8 篇 AI 抓不出規律。

### 步驟 2：把樣本丟給 Claude 分析

指令範本：

> 請從下面 N 篇我寫的貼文，萃取出我的 voice profile，包含開頭習慣、句法特徵、用詞偏好、情緒詞彙、結尾習慣，還有我絕對不會寫的東西，也就是反指紋。輸出格式是條列，每條附 1-2 個範例。

### 步驟 3：校對加補強

Claude 萃出來的第一版通常只有 70 分，要自己看一遍，補上哪些招牌詞沒抓到，刪掉哪些是 Claude 瞎掰的。改個 3-5 輪會出 85 分的版本。

### 步驟 4：存成 voice\_yourname.md

之後每次叫 AI 寫東西，就先把這份文件丟給 AI 讀。AI 會照你的招牌寫，不再是那種 ChatGPT 工商口吻。

我自己的 voice profile 裡面有這些：場景直入的開頭、段落中間用空格停頓、招牌的「(是有屁用」，以及招牌的「鍋」字。

還有不放 emoji 的進度更新型貼文，以及「免費仔」「普通聰明人」這種自我貼標籤。

完整版本放在 Lab 的記憶層。

AI 拿到這份文件之後寫出來的東西，多數場合家人朋友都看不出是 AI 寫的。

## Threads 自動發文機器人怎麼從 0 做到 100？（5 步做完）

下面用「GitHub Trending 到 Threads」當範例。

兩端都可以換。把 GitHub Trending 換成你的素材來源，把 Threads 換成 X 或 LinkedIn，整套架構完全不變。

### 步驟 1：跟 Claude Code 講需求

你可以輸入：

> 我想做一個每天晚上 8 點自動發 Threads 的小機器人。流程是這樣：1) 抓 GitHub trending 當天前 5 個專案，2) 隨機挑 1 個，3) 讀我的 voice\_coolkid.md，用我的口吻寫一段 200 字內的介紹，4) 發到我的 Threads 帳號。請建一個 Python 專案，requirements.txt 寫清楚要裝什麼。不要規劃，直接建檔。

### 步驟 2：設定 Threads API 帳號

Threads 走 Meta Graph API，去 developers.facebook.com 建 App，加 Threads 產品，再拿 access token。細節看官方文件，AI 也能一邊帶你做。第一次設定大概 15 分鐘。

### 步驟 3：在自己電腦試跑

在資料夾打開 PowerShell 跑 python bot.py，看會不會抓到 GitHub 專案、寫出貼文、發到 Threads。先用測試帳號發 1-2 則確認流程通，再切到正式帳號。

### 步驟 4：處理 Windows 中文編碼這個雷，做法看下一節

中文發出去變成問號，是 Windows 預設 cp950 編碼造成的。先把這個雷處理掉，自動發文才會真的發成中文。

### 步驟 5：設定每天 8 點自動跑

用 Windows 工作排程器，或是 Vercel 加上 cron-job.org，比較看下下節。設完就不用每天手動跑，機器人自己會起床發文。

跑完這 5 步，機器人就開始每天自動發文了。

發出去 1-2 週，你會發現一件事。壓力真的小很多。「每天要分享什麼」不再壓在頭上，剩下的就是有靈感的時候，自己出手寫一篇。

## 中文在 Windows PowerShell 變成亂碼怎麼解？

這是我自己卡了 2 小時的雷。

Python 在 Windows PowerShell 執行時，印出中文或透過 API 傳送中文，可能會變成「???」加上一堆奇怪符號。

原因是 Windows PowerShell 預設使用 cp950，也就是 Big5；Python 預設使用 UTF-8，兩邊編碼對不上就會變成亂碼。

解法有 3 個，下面從最輕的排到最重的：

### 解法 1（推薦）：強制 Python 使用 UTF-8 輸出

在 Python 檔案最上面加入 `import sys; sys.stdout.reconfigure(encoding='utf-8')` 這一行讓 Python 強制用 UTF-8 輸出，不管 PowerShell 預設是什麼編碼。

### 解法 2：先把貼文寫進文字檔案

把貼文先寫進一個.txt 檔案，開檔的時候指定 encoding='utf-8'，程式再去讀那個檔案發送，不要把中文字直接放在 print 或 subprocess 裡傳。

### 解法 3（一勞永逸）：將 PowerShell 終端機切換成 UTF-8

在 PowerShell 跑 chcp 65001，把終端機改成 UTF-8。但這條只對當下那個視窗有效，重開 PowerShell 又會變回 cp950。

我自己使用解法 1 加上解法 2 作為雙重保險。中文透過 API 送出之後，Threads 可以正常顯示，不會變成問號。

Mac 使用者不會遇到這個問題，因為 macOS 預設使用 UTF-8。

這個雷在其他自動化工具上也會碰到，例如 IG 機器人、X 機器人和 LINE 機器人。學會一次之後，就能一直用得到。

## 怎麼讓機器人每天 8 點自己起床跑？

讓機器人每天定時執行有 3 種方式，下面從免費排到付費：

### 方式 1（最簡單）：Windows 工作排程器（Task Scheduler）

這是 Windows 本來就有的功能，開始選單搜「工作排程器」，建立工作，設定每天 20:00 觸發，動作填執行 python C:\\path\\to\\bot.py。免費。缺點是電腦關機機器人就不會跑。

### 方式 2（推薦）：cron-job.org 免費 cron 加上 Vercel Function

把機器人包成一個 API 端點部署到 Vercel（免費），再讓 cron-job.org 每天 8 點 GET 一次那個網址觸發。永久免費，電腦關機照跑。

### 方式 3（進階）：GitHub Actions cron

在.github/workflows/ 寫一個 cron schedule，每天 8 點觸發 GitHub Action 跑機器人。完全免費，而且跟程式碼放在一起好管理。缺點是要懂 GitHub Actions 的設定。

製作第一個機器人時，建議先使用方式 1，也就是 Windows 工作排程器。15 分鐘可以設定完成，幾乎沒有學習成本，單機就能執行。後續真的需要 24 小時不間斷運作，再升級到方式 2 或方式 3。

我自己現在用方式 2，cron-job.org 加 Vercel Function 永久免費，電腦關機機器人照樣跑。

> 原文出處：Coolkid AI Lab [〈我用 AI 做了一個每天 8 點自動發 Threads 的小機器人〉](https://www.coolkidlab.com/newbie-pitfalls/ai-daily-thread-auto-poster.html)