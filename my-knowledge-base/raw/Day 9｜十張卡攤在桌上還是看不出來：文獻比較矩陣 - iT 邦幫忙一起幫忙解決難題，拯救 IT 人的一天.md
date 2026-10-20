---
title: "Day 9｜十張卡攤在桌上還是看不出來：文獻比較矩陣 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10409494"
author:
  - "[[iThome]]"
published:
created: 2026-09-23
description: "摘要卡做到第十張的時候，我把它們全部印出來攤在桌上，想看出「這些研究之間的關係」。看了二十分鐘，看出來的只有桌子不夠大。 臨床上有同樣的問題。每一床的交班單都寫得很清楚，但護理長要知道「今天整個病房哪..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 9

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 研究生自救指南:30 天用 Claude Code 打造我的論文工具箱系列 第 9 篇

## Day 9｜十張卡攤在桌上還是看不出來：文獻比較矩陣

- 分享至

摘要卡做到第十張的時候，我把它們全部印出來攤在桌上，想看出「這些研究之間的關係」。看了二十分鐘，看出來的只有桌子不夠大。

臨床上有同樣的問題。每一床的交班單都寫得很清楚，但護理長要知道「今天整個病房哪幾床有壓傷風險」，不會一張一張翻，她看的是白板上那張總表：一列一床，一欄一個項目，掃一眼就知道。

摘要卡是交班單。今天要做的是白板。

矩陣是什麼，為什麼要另外做  
文獻比較矩陣就是一張表，一列一篇論文，一欄一個欄位。欄位跟 Day 8 摘要卡一模一樣，因為它就是把卡片的內容平鋪出來。

聽起來只是排版。但它讓兩件在卡片上做不到的事變得可能。

第一是橫著看。十張卡各自寫「樣本 312 人」「樣本 87 人」「樣本 1,204 人」，要在腦袋裡排序很難；放進同一欄，一眼就看到誰大誰小、誰是單一機構誰是多中心。

第二是看空格。這件事我做了才發現。矩陣裡「測量工具」那一欄如果有一半寫著〔未在原文找到〕，那不是我的卡做壞了，是這個領域的研究普遍沒有報告信度。這種發現，寫進文獻回顧就是一段——而且是老師會點頭的那種段落。

這個工具只有一條真正的規則  
只收〔已核對〕的卡。

Day 8 說過，卡片出來的那一刻狀態是〔未核對〕，我抽兩欄翻回 PDF 對過頁碼，才改成〔已核對：日期〕。矩陣工具讀卡片的時候，第一件事是看這個狀態，〔未核對〕的一律跳過，而且要在輸出最後告訴我它跳過了哪幾張。

為什麼這麼堅持？因為矩陣是拿來比較的，一格錯，整列的比較都是錯的；而矩陣一旦做出來，我就會開始相信它，不會再回頭翻卡。錯誤要在進矩陣之前攔下來，進去之後就攔不住了。

跟給藥一樣：藥車上的藥都對過了才推出去，不是推到床邊才對。

SKILL.md 全文  
存在.claude/skills/lit-matrix/SKILL.md：

---

## name: lit-matrixdescription: 把 literature/cards/ 裡已核對的摘要卡合併成一張文獻比較矩陣，輸出到 literature/matrix.md 與 matrix.csv。使用者說「做矩陣」「合併卡片」「文獻比較表」時使用。

## 文獻比較矩陣

## 輸入

literature/cards/ 底下所有.md 檔。

## 規則

1. 只收「核對狀態」為〔已核對：日期〕的卡。〔未核對〕的卡一律跳過，並在輸出最後列出被跳過的檔名。
2. 只能搬運，不能改寫。每一格的內容必須與卡片原文逐字相同，包含頁碼。不得縮寫、不得摘要、不得把兩句合成一句。
3. 卡片上寫〔未在原文找到〕的欄位，矩陣裡也寫〔未在原文找到〕，不得留空、不得改成「無」。
4. 欄位順序固定：作者\_年份、研究設計、樣本、測量工具、主要結果、限制、我的用途。
5. 列的順序依年份由舊到新，同年依第一作者姓氏。
6. 「我的用途」欄照卡片搬運，卡片上空的就留空。
7. 不得新增任何卡片上沒有的列，不得補上任何卡片上沒有的資訊。

## 輸出

- literature/matrix.md：markdown 表格
- literature/matrix.csv：同樣內容，UTF-8 with BOM，給 Excel 開
- 表格下方三行：收錄幾張、跳過幾張、跳過的檔名  
	在專案資料夾啟動 claude，打「做矩陣」。它讀完所有卡，寫出兩個檔。

為什麼要輸出兩個版本？markdown 表格在十篇以內還能看，超過十篇、每格又有頁碼，一列會長到螢幕塞不下。CSV 丟進 Excel 可以凍結首欄、調欄寬、篩選，看起來舒服很多。UTF-8 with BOM 那句一定要寫，不然 Windows 的 Excel 打開中文會變亂碼，這是我第一次跑就踩到的。

矩陣長什麼樣  
示範用三篇虛構論文，實際有七欄，這裡只放四欄看格式：

| 作者\_年份 | 研究設計 | 樣本 | 主要結果 |
| --- | --- | --- | --- |
| 林\_2021 | 橫斷式相關性研究（p. 46） | 北部某醫學中心護理人員 312 人，便利取樣（p. 47） | PSQI 總分與情緒耗竭呈正相關，r =.42，p <.001（p. 51） |
| 王\_2022 | 橫斷式相關性研究（p. 103） | 三家區域醫院護理人員 587 人，分層隨機抽樣（p. 104） | 睡眠品質差者倦怠得分顯著較高，t = 4.21，p <.001（p. 108） |
| 陳\_2023 | 縱貫式研究，追蹤六個月（p. 12） | 〔未在原文找到〕 | 基線睡眠品質預測六個月後倦怠，β =.31，p =.002（p. 17） |
| 收錄：3 張 |  |  |  |
| 跳過：1 張 |  |  |  |
| 跳過的檔名：張\_2020.md（核對狀態：〔未核對〕） |  |  |  |
| 陳\_2023 那一列的樣本欄是〔未在原文找到〕，不是漏掉，是卡片上就這樣。這種格子要留著，它會提醒我這篇在寫進文獻回顧之前要回頭確認一次。 |  |  |  |

哪裡會出錯  
它會「幫我」把長格壓短。 第一版沒寫規則二，它把「北部某醫學中心護理人員 312 人，便利取樣（p. 47）」壓成「312 人（p. 47）」，還說這樣表格比較好看。表格是好看了，但抽樣方式不見了，而抽樣方式正是我要比較的東西。所以規則二寫「逐字相同」，而且我做完會抽一列，跟卡片對一次。

它會把〔未核對〕的卡也收進去。 有一次它收了一張沒核對的卡，理由是「內容看起來完整」。看起來完整跟核對過是兩件事。規則一後面那句「列出被跳過的檔名」就是為此加的：我看到跳過清單，數一下，跟我知道還沒核對的卡數對不對得上。對不上就是它偷收了。

排序不是中性的。 依年份排序看起來最客觀，但我寫文獻回顧不是照年份寫，是照概念分組寫。矩陣的排序只是「找得到」，不是「怎麼寫」。這一點我一開始搞混了，對著年份排好的矩陣想順著往下寫，寫出來是流水帳（需作者補充：老師怎麼說的）。

卡片改了，矩陣不會自己跟著改。 這正好回到 Day 4 的原則二：矩陣是 output，可以整個刪掉重生。所以我從不手動改矩陣，要改就回卡片改，改完重跑一次「做矩陣」。手動改矩陣一次，下次重跑就全部蓋掉，白改。

矩陣不是文獻回顧  
這句話老師講過（需作者補充：老師原話），我記得很清楚：表格不會替你論證。

矩陣是原料。它讓我看得到十篇研究之間的異同，但「這些異同代表什麼、我的研究站在哪裡」是我要寫的，不是表格會長出來的。

實際上我怎麼用它：矩陣做出來之後，我回卡片填「我的用途」那一欄，寫這篇是支持我的假設、還是反例、還是方法上可以借。填完重跑矩陣，最後一欄就變成分組依據。文獻回顧的段落結構，是從那一欄長出來的，不是從年份長出來的。

這也是為什麼「我的用途」一定要自己填。它是矩陣裡唯一不是搬運來的東西，也是唯一有我的判斷的地方。

我還不確定的地方  
卡片超過二十張之後，就算用 CSV 也開始難看，一列七欄每格都帶頁碼，橫向要捲很久。目前想到兩個方向：一是拆成兩張矩陣，方法學一張、結果一張；二是在矩陣裡只放摘要句、頁碼留在卡上。第二個方向跟規則二衝突，我還沒決定。Day 12 週回顧再看。

明天  
Day 10：引用查證器。Day 6 標了那麼多〔未查證〕，該一筆一筆去確認它們是不是真的存在了。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10409202)

[

Day 8｜讀不完怎麼辦：單篇論文的結構化摘要工具

](https://ithelp.ithome.com.tw/articles/10409202)[下一篇](https://ithelp.ithome.com.tw/articles/10409750)

[

Day 10｜引用查證器：每一筆參考文獻都要能被打開

](https://ithelp.ithome.com.tw/articles/10409750)

系列文

[研究生自救指南:30 天用 Claude Code 打造我的論文工具箱](https://ithelp.ithome.com.tw/users/20183988/ironman/9487) 共 20 篇

目錄

1. 16
	[Day 16｜表一被退回：不是數字錯，是格式錯，還有一個分母算錯的百分比](https://ithelp.ithome.com.tw/articles/10413273)
2. 17
	[Day 17｜圖表工具：色盲安全色票，印成黑白還是可能失敗](https://ithelp.ithome.com.tw/articles/10413818)
3. 18
	[Day 18｜沒有「回頭」這個選項：我在原始資料上直接動手的那三個月](https://ithelp.ithome.com.tw/articles/10414351)
4. 19
	[Day 19｜第三週回顧：資料這週，錯的代價比文獻週高很多](https://ithelp.ithome.com.tw/articles/10414697)
5. 20
	[Day 20｜文獻回顧段落草稿：它寫的每一句都對，合起來卻什麼都沒說](https://ithelp.ithome.com.tw/articles/10415367)
[完整目錄](https://ithelp.ithome.com.tw/users/20183988/ironman/9487)

[![.](https://itadstatic.ithome.com.tw/B9/1789123100_6aa3da1cbf61d.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/20/198811eb-2540-480e-98d2-e0a8cc3d0784.png)

[2023臺灣企業DORA關鍵數據大公開 (下）](https://itplus.ithome.com.tw/vod-page/768?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

22 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/4/30/1bc870b6-6ca0-4221-a765-01d4ece72ac1_small.jpg)

[新世代數位流程自動化：Control-M突破障礙，有效提升IT服務效能、可用性與降低營運成本](https://itplus.ithome.com.tw/vod-page/822?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/18/00f6500d-47de-48ee-b781-e34fad8426e4_small.png)

[如何利用 Splunk 進行『雲原生』、『容器化』、『微服務』的應用服務效能監控、使用者分析管理](https://itplus.ithome.com.tw/vod-page/611?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/2/f2ec2119-75c0-48de-bea9-63bbf2fbce50_small.png)

[不洩漏原始碼，也能全面防護？Hybrid AI @DevSecOps](https://itplus.ithome.com.tw/vod-page/861?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

29 分