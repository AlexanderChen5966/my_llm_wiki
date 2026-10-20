---
title: "Day 7｜文獻檢索：把搜尋策略寫成可重現的腳本（PubMed MCP） - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10408927"
author:
  - "[[iThome]]"
published:
created: 2026-09-18
description: "上一次 meeting,老師看著我的文獻回顧初稿,問我這些文獻是用什麼條件找的、哪一天查的、總共跑出幾筆。我打開 PubMed 的搜尋歷史,只剩最近幾次,其中一次還是我半夜隨手打的關鍵字。我答不出來。..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 7

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 研究生自救指南:30 天用 Claude Code 打造我的論文工具箱系列 第 7 篇

## Day 7｜文獻檢索：把搜尋策略寫成可重現的腳本（PubMed MCP）

- 分享至

上一次 meeting,老師看著我的文獻回顧初稿,問我這些文獻是用什麼條件找的、哪一天查的、總共跑出幾筆。我打開 PubMed 的搜尋歷史,只剩最近幾次,其中一次還是我半夜隨手打的關鍵字。我答不出來。

那一刻才發現,論文裡每一個數字都有出處,唯獨「這些文獻是怎麼來的」這件事,我完全沒有紀錄。

第二週開始談文獻。今天先處理最前面、也最容易被忽略的一步:檢索。

檢索是整條文獻流程裡最不可重現的一步  
問卷資料我會留原始檔,統計我會留輸出,但檢索式我從來沒留過。

原因很簡單:PubMed 的網頁就是拿來「試」的。打幾個字,看結果不對,加一個 AND,再拿掉一個,改成 MeSH 詞,來來回回幾十次。最後留下的是一堆 PDF,中間走過的路全部消失。

這在寫文獻回顧那一章時會出問題。口試委員問「你怎麼確定沒漏掉重要文獻」,我只能說我查得很認真。如果做的是系統性文獻回顧,檢索式、日期、筆數更是必須寫進論文的東西。就算是我這種問卷型研究,能拿出一份「我在哪一天、用什麼條件、找到幾筆」的紀錄,跟拿不出來,在老師眼裡是兩種學生。

聊天視窗幫不上這個忙。它可以幫我想關鍵字,但它碰不到 PubMed,它說的筆數是猜的。這正是 Day 2 說的第三面牆:碰不到資料。

MCP:讓 Claude Code 自己去查 PubMed  
Claude Code 本來只認得我電腦裡的檔案。要讓它連到 PubMed,需要一個叫 MCP(Model Context Protocol)的東西。

我第一次看到這三個字母也很怕,後來想通了,它就像會診單的格式:任何外部服務只要照這個格式接上來,Claude Code 就知道怎麼跟它講話。PubMed 有社群維護的 MCP 伺服器,接上之後,Claude Code 就能直接送出檢索式、拿回結果。

設定只有一行指令。在論文專案的資料夾打開終端機:

claude mcp add pubmed -- \<PubMed MCP 伺服器的啟動指令>  
尖括號裡的啟動指令依你選的伺服器而定,它的說明頁會寫。加完之後用 claude mcp list 確認有沒有出現 pubmed 這一項。如果想讓實驗室同學也直接用,把設定寫進專案根目錄的.mcp.json,整個資料夾複製過去就能用,這件事 Day 26 會再談。

接上之後,在 claude 裡打一句「用 PubMed 查 nurse burnout AND sleep quality,回傳筆數與前十筆的 PMID」,它會真的去查。回來的筆數是 PubMed 說的,這一點跟聊天視窗完全不同。Day 6 那些被標成〔未查證〕的引用,從今天起也有辦法真的查到 PMID,變成〔已查證〕。

交付物一:檢索策略檔  
工具接上了,接下來的重點反而是把「策略」從我腦袋裡拿出來寫成檔案。這是 Day 4 資料夾結構裡 literature/ 底下的第一份檔案:literature/search-strategy.md。

## 檢索策略

## 研究問題(PICO)

- P(對象):(需作者補充:研究對象)
- I／E(暴露或介入):(需作者補充:主要變項)
- C(比較):無
- O(結果):(需作者補充:結果變項)

## 關鍵字與 MeSH

| 概念 | 自由詞 | MeSH |
| --- | --- | --- |
| 對象 | nurses, nursing staff | Nurses |
| 主要變項 | burnout, occupational burnout | Burnout, Professional |
| 結果變項 | sleep quality, insomnia | Sleep Quality |

## 檢索式(版本 v1,建立日期:<日期>)

- S1:("Nurses"\[MeSH\] OR nurse\*\[tiab\])
- S2:("Burnout, Professional"\[MeSH\] OR burnout\[tiab\])
- S3:("Sleep Quality"\[MeSH\] OR "sleep quality"\[tiab\] OR insomnia\[tiab\])
- S4:S1 AND S2 AND S3

## 限制條件

- 日期範圍:<起訖年>
- 語言:English, Chinese
- 文獻類型:排除 letter、editorial

## 版本紀錄

- v1:初版  
	上面的關鍵字是我這個題目方向的示範,你的題目請整份換掉。

重點是結構:PICO 放最上面提醒自己為什麼要找,關鍵字表讓自己看得到有沒有漏同義詞,檢索式編號讓之後的紀錄可以指名道姓,版本紀錄讓每一次改動都被看見。

交付物二:每一次檢索都留一行  
策略檔是「打算怎麼查」,還需要另一份檔案記「實際查了什麼」。

Day 4 的樹狀圖裡我只畫了一個 search-strategy.md,打算策略和紀錄放同一份,做了才發現策略會改版、紀錄只會一直往下加,兩種東西混在一起讀不了。所以拆成兩份,紀錄用 literature/search-log.md,格式固定:

| 日期 | 策略版本 | 檢索式編號 | 實際送出的檢索式 | 筆數 | 結果檔 | 備註 |
| --- | --- | --- | --- | --- | --- | --- |
| <日期> | v1 | S4 | ("Nurses"\[MeSH\] OR nurse\*\[tiab\]) AND... | <筆數> | results/<日期>\_S4.csv | 首次檢索 |
| 然後在 Claude Code 裡這樣下指令: |  |  |  |  |  |  |

讀 @literature/search-strategy.md,用 PubMed MCP 依序執行 S1 到 S4。  
不要改動任何檢索式。  
每一條執行完,把日期、版本、編號、實際送出的檢索式、筆數追加到  
@literature/search-log.md。  
S4 的結果(PMID、標題、年份、期刊)存成 literature/results/<今天日期>\_S4.csv。  
它做完之後,換我做事。

哪裡會出錯  
筆數要跟網頁對一次。 我自己去 PubMed 網頁把 S4 貼進去跑一遍,筆數應該一樣。第一次跑就不一樣,差了(需作者補充:幾筆)。查了半天,是 MCP 伺服器帶的日期篩選和網頁介面的預設不同。這種差異沒有誰對誰錯,但一定要寫進備註欄,不然三個月後我會以為自己記錯。

AI 會「好心」改檢索式。 我第一次沒寫「不要改動任何檢索式」,它幫我在 S3 加了兩個同義詞,還說這樣比較完整。它可能是對的,但檢索式一旦跟策略檔不一致,紀錄就沒有意義了。所以指令裡那一句「不要改動」是必要的,而且我要求它把「實際送出的檢索式」原封不動寫進紀錄,我用眼睛比對過才算數。想改,回策略檔改,版本號加一。

結果檔要存下來。 筆數只是個數字,真正要回溯的是「那天找到的是哪幾篇」。PubMed 的內容會變,同一條檢索式半年後跑出來的筆數不同很正常。存了 PMID 清單,之後才有辦法說「這篇是後來才出現的,所以初稿沒有納入」。

我還不確定的地方  
結果檔會越存越多。一個題目、四條檢索式、改三次版本,資料夾裡就是十幾個 CSV。我目前的做法是全部留著,只在紀錄檔裡標哪一份是「正式採用」的。這是不是最好的做法,我還沒想清楚,等 Day 12 週回顧時再看。

明天  
Day 8:檢索回來的 PDF 讀不完怎麼辦,做一個丟一篇 PDF 就回一張固定格式摘要卡的 skill。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10408556)

[

Day 6｜我踩的第一個坑：AI 捏造引用，以及我怎麼建驗證機制

](https://ithelp.ithome.com.tw/articles/10408556)[下一篇](https://ithelp.ithome.com.tw/articles/10409202)

[

Day 8｜讀不完怎麼辦：單篇論文的結構化摘要工具

](https://ithelp.ithome.com.tw/articles/10409202)

系列文

[研究生自救指南:30 天用 Claude Code 打造我的論文工具箱](https://ithelp.ithome.com.tw/users/20183988/ironman/9487) 共 15 篇

目錄

1. 11
	[Day 11｜APA 第七版校對：中英文混排的地獄，和一份學校規定優先的規則檔](https://ithelp.ithome.com.tw/articles/10409963)
2. 12
	[Day 12｜第二週回顧：五個文獻工具用了一週，老實算帳](https://ithelp.ithome.com.tw/articles/10410477)
3. 13
	[Day 13｜問卷資料清理：從 Google 表單匯出的那份原始檔開始](https://ithelp.ithome.com.tw/articles/10411094)
4. 14
	[Day 14｜反向題、遺漏值、離群值：清理規則不是 prompt，是程式碼](https://ithelp.ithome.com.tw/articles/10412049)
5. 15
	[Day 15｜匯出給 SPSS：變數標籤與值標籤的自動化，以及一個會讓平均數爆掉的數字](https://ithelp.ithome.com.tw/articles/10412671)
[完整目錄](https://ithelp.ithome.com.tw/users/20183988/ironman/9487)

[![.](https://itadstatic.ithome.com.tw/B9/1789123100_6aa3da1cbf61d.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/4/30/9ad6b34a-4a8d-4f4c-9196-3024178103b9_small.jpg)

[使用 MongoDB Relational Migrator 實現應用程式現代化](https://itplus.ithome.com.tw/vod-page/806?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

43 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/14/a27ba4f3-6361-4f8f-ac88-5459361ae17f_small.png)

[生成式 AI 對政府部門的挑戰、機會與潛藏的資安風險](https://itplus.ithome.com.tw/vod-page/648?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

36 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/25/4fc60419-2101-4dff-9d0c-aa8ec1053505_small.png)

[從制品管理到供應鏈防護：用 JFrog 打造現代 DevSecOps 流程](https://itplus.ithome.com.tw/vod-page/1157?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

37 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/16/98b18f45-ab69-47eb-8c50-7e1309f899ff_small.png)

[人工智能與 NetApp 數據效能的完美結合](https://itplus.ithome.com.tw/vod-page/931?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

28 分