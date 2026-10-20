---
title: "[Tedium Is Stability-01] 結構性的不安：AI 幫你把功能寫完了，你為什麼還是不放心？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10400402"
author:
  - "[[iThome]]"
published:
created: 2026-07-30
description: "結構性的不安：AI 幫你把功能寫完了，你為什麼還是不放心？ The Structural Unease 系列文｜繁瑣即安定：與 AI 一起開發，把「沒說出口的開發規矩」也當成程式碼。 你可能有過這種感..."
tags:
  - "clippings"
status: processed
---
0

## \[Tedium Is Stability-01\] 結構性的不安：AI 幫你把功能寫完了，你為什麼還是不放心？

- 分享至

## 結構性的不安：AI 幫你把功能寫完了，你為什麼還是不放心？

## The Structural Unease

> 系列文｜繁瑣即安定：與 AI 一起開發，把「沒說出口的開發規矩」也當成程式碼。

你可能有過這種感覺。

你把一個需求交給 AI，它很快回你一段會動的程式碼。編譯過了、測試綠了、demo 也順利跑完。照理說該鬆一口氣，你心裡卻有個聲音一直沒消失，總覺得，好像還少了什麼。

這不是因為 AI 不夠聰明。現今的模型，幾秒鐘就能生出一整個模組，品質好得驚人。這種不安更深一層，它不是「做得好不好」的問題，而是「做得全不全」的問題。我把它叫做 **結構性的不安（Structural Unease）** ：你隱約知道還有些東西沒被照顧到，卻指不出它們是什麼，於是也無從檢查起。

這個系列，就是想把這份不安講清楚，然後一步步把它拆掉。而它會走向一個乍看反直覺的結論，也是整個系列的主線：那些讓你不安、又繁瑣得要命的檢查，恰恰是你和 AI 一起開發時，最大的安定來源。 **繁瑣，即安定（Tedium is Stability）。** 先記住這句，我們慢慢說。

先講清楚範圍：這裡談的不是週末寫的 toy app，而是那種規矩多到只有資深工程師才記得住的成熟系統。功能越簡單，這份不安越不明顯；系統越成熟，它越致命。

## 一個「會動的功能」，其實只是水面上的一角

我們先看清楚，問題長什麼樣子。

假設你請 AI「幫我加一個把訂單匯出成報表的 API」。它給你的，是水面上那塊你看得見的東西：一個端點、一段服務邏輯、一組會綠的測試。乾淨俐落，令人安心。

但一個功能，從來就不只是那段會動的 code。水面下，還牽著一長串沒人明講、卻一定得做的事。

有沒有記下「誰在什麼時候匯出了什麼」，好讓日後查得到帳？這個動作要不要先經過審核？誰有權限用它、這個方案的客戶到底有沒有買這個功能？匯出的報表裡，會不會夾帶身分證號這類個資、需不需要先遮罩？如果過程中要寄一封通知信出去，那通對外呼叫有沒有留下痕跡？正式環境同時跑兩台機器時，它會不會重複執行？還有，你改了這裡，會不會不小心弄壞了別的地方？

你看到的是一個功能，水下藏著的卻是八件事。它們多半沒寫在需求單裡，只安靜地活在資深工程師的直覺，和程式碼某個不起眼的角落。你的不安，正是來自這裡：你感覺得到它們在，卻叫不出名字。

## 不是 AI 不夠聰明，而是沒人告訴它「還得檢查這些」

這裡有個很殘酷的不對稱。

AI 會把「看得見的」做到滿分，端點、測試、乾淨的命名，甚至連執行紀錄都寫得漂漂亮亮。但對於「看不見的」那一串，它會安靜地跳過。不是因為它偷懶，而是因為從頭到尾，沒有人告訴它：這一類任務，除了會動，還得檢查這些。

![AI 把看得見的做到滿分，把看不見的靜默略過，帳單三個月後才來](https://ithelp.ithome.com.tw/upload/images/20260711/20181364eKngKwC7Vj.png)

最麻煩的是，這種「漏做」不會當場報錯。功能照樣上線、照樣運轉，一切看起來風平浪靜。真正的帳單，往往三個月後才寄到：客戶問「這批資料是誰匯出去的？」，你翻遍系統卻查不到任何紀錄，因為當初那筆稽核根本沒被記下；或者某天才發現，報表把個資原封不動地送了出去；又或者，正式環境那兩台機器，把同一封通知信各自寄了一遍。這些坑，在你本機單台跑的時候，一個都測不出來。

## POG 補上一層，這個系列再往上補一層

如果你熟悉 POG，你會知道它的主張： **把任務與提示詞，當成程式碼一樣對待** 。別再讓那段反覆打磨的 prompt、那份 AI 拆解任務的邏輯，隨著對話視窗一關就石沉大海；把它們寫進 Git、留下執行紀錄，讓經驗可以累積、可以回頭追。這補上了很關鍵的一層，它回答的是「AI 到底做過什麼」。

而這個系列想談的，是再往上一點的那一層。

![POG 讓任務可追溯，這個系列讓任務可驗收](https://ithelp.ithome.com.tw/upload/images/20260711/20181364NLRHNrIUbQ.png)

它要回答的不是「AI 做過什麼」，而是「 **AI 有沒有做全** 」。用的是同一種精神：既然任務能當成程式碼，那麼「一個任務背後該滿足哪些義務」，當然也能。我們可以把水面下那串隱形的規矩，一條條清點出來、寫下來，變成每個任務都自動長得出來的檢查清單，讓 AI 在動手之前，就知道這一類任務該顧到哪些面向；也讓你在收尾時，手上有一份實實在在、能逐項打勾的東西，而不再只是一種說不出口的直覺。

把這份不安好好命名，是整趟旅程的起點。

下一篇，我們就把水面下那座冰山整個翻上來看：一個功能，究竟是由哪些「隱形契約」組成的？你會發現，它們遠比你以為的更有跡可循。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/11/7a3e83f3-714c-4860-917a-a58a8f683d8a_small.png)

[2021 Q4 - Progress MFT 安全檔案傳輸管理軟體 - MOVEit Transfer 培訓課程 (1)](https://itplus.ithome.com.tw/vod-page/511?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

EC NETWORKER |

128 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/14/bfb5bc28-183a-4440-a1ef-aee3c6a7ea5b_small.png)

[Aruba 安全網路架構保護公共資料](https://itplus.ithome.com.tw/vod-page/649?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/20/a63ccd8a-d1d0-4921-8edc-c94636c6899e_small.png)

[開發者的匠心](https://itplus.ithome.com.tw/vod-page/1393?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/10/31/82880d3a-c666-452d-93c6-c8d4c22ae9ca_small.png)

[從容應對量子時代：NetApp AI 驅動的現代化資料中心安全藍圖](https://itplus.ithome.com.tw/vod-page/1281?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣醫院資訊主管會議 |

36 分