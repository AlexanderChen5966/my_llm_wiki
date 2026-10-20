---
title: "Day 8｜讀不完怎麼辦：單篇論文的結構化摘要工具 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10409202"
author:
  - "[[iThome]]"
published:
created: 2026-09-23
description: "昨天檢索完，literature/pdf/ 裡躺了（需作者補充：幾篇）篇 PDF。我認真讀了前五篇，讀到第六篇的時候，想不起第二篇的樣本數是多少、用的是哪一版量表。翻回去找，找到了，下一篇讀完又忘了第..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 8

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 研究生自救指南:30 天用 Claude Code 打造我的論文工具箱系列 第 8 篇

## Day 8｜讀不完怎麼辦：單篇論文的結構化摘要工具

- 分享至

昨天檢索完，literature/pdf/ 裡躺了（需作者補充：幾篇）篇 PDF。我認真讀了前五篇，讀到第六篇的時候，想不起第二篇的樣本數是多少、用的是哪一版量表。翻回去找，找到了，下一篇讀完又忘了第三篇的結論。這種事我在臨床也遇過：一個人顧太多床，不靠紀錄，光靠腦袋一定漏。

問題不在讀得慢，在記得亂  
我以前的讀書筆記是 Word 檔，一篇一段，想到什麼寫什麼。有的記結論，有的記量表，有的只記「這篇可以引」。十篇筆記十種長相，要比較的時候完全對不起來。護理紀錄為什麼要固定欄位？因為交班的人要能在三十秒內找到「這床今天最重要的事」。文獻筆記也一樣，欄位固定了，比較才可能發生。

所以我要的工具很單純：丟進去一篇 PDF，回來一張欄位固定的卡。這件事 Day 5 學的 skill 剛好合用，因為它是一個「每次都一樣的流程」，寫一次，之後每篇 PDF 都照做。

摘要卡的欄位  
欄位是我跟老師討論後定的（需作者補充：老師建議增減的欄位）。定的原則是「這張卡要能回答我寫文獻回顧時會問的問題」：

研究設計：橫斷、縱貫、實驗、質性  
樣本：對象、人數、地點、抽樣方式  
測量工具：量表名稱、版本、信度  
主要結果：跟我研究問題有關的數字，照抄不換算  
限制：作者自己承認的  
我的用途：這篇要放進我論文的哪一段，這欄留給我填  
每一欄後面都要附頁碼  
最後一點是這張卡的命脈。Day 6 被 AI 捏造的引用騙過之後，我定了一條規則：AI 給的東西沒有可查證欄位就不算數。摘要卡的可查證欄位就是頁碼。它說樣本 312 人，我要能翻到第 47 頁看到 312 這個數字。

SKILL.md 全文  
存在.claude/skills/paper-card/SKILL.md：

---

## name: paper-carddescription: 讀一篇論文 PDF，輸出固定格式的摘要卡到 literature/cards/。使用者說「做摘要卡」「摘要這篇」「paper card」時使用。

## 論文摘要卡

## 輸入

使用者用 @ 指定一個 PDF 檔。

## 規則

1. 只能使用這份 PDF 裡的內容。不得引入任何 PDF 以外的知識，也不得補充作者沒寫的資訊。
2. 每一欄的每一句話結尾都要附頁碼，格式「（p. 12）」，用期刊印在頁面上的頁碼，不用 PDF 閱讀器的頁數。同一欄引自多頁就分別標。
3. PDF 裡找不到的欄位，寫「〔未在原文找到〕」，不得推測、不得留空。
4. 數字照抄，不換算、不四捨五入、不合併。
5. 「我的用途」欄固定留空，由使用者自己填。
6. 若 PDF 沒有文字層（讀不出文字），停下來告訴使用者，不要猜內容。

## 輸出

檔名：literature/cards/<第一作者姓>\_<年份>.md  
內容依下列格式，欄位順序不可變：

## <篇名>

- 引用：<作者（年份）。篇名。期刊，卷（期），頁碼。DOI>
- 研究設計：
- 樣本：
- 測量工具：
- 主要結果：
- 限制：
- 我的用途：
- 製卡日期：<今天日期>
- 核對狀態：〔未核對〕  
	寫好之後，在專案資料夾啟動 claude，一句話：「@literature/pdf/某篇.pdf 做摘要卡」。它讀完 PDF，把卡寫進 literature/cards/。description 那一行是觸發依據，我把自己平常會講的幾種說法都放進去，這樣不必記指令名稱；真的想指名呼叫，打 /paper-card 也可以。

一張卡長什麼樣  
這是一張示範卡，論文是虛構的，只看格式：

## 〔範例〕護理人員睡眠品質與職業倦怠之相關研究

- 引用：〔範例〕林某某、王某某（2023）。護理人員睡眠品質與職業倦怠之相關研究。虛構護理期刊，10（2），45–58。DOI：〔範例，無真實 DOI〕
- 研究設計：橫斷式相關性研究（p. 46）
- 樣本：北部某醫學中心護理人員 312 人，便利取樣（p. 47）
- 測量工具：中文版匹茲堡睡眠品質量表（PSQI），Cronbach's α =.81（p. 48）；MBI 職業倦怠量表，α =.87（p. 48）
- 主要結果：PSQI 總分與情緒耗竭呈正相關，r =.42，p <.001（p. 51）
- 限制：單一機構、橫斷設計無法推論因果（p. 55）
- 我的用途：
- 製卡日期：<日期>
- 核對狀態：〔未核對〕  
	怎麼核對，哪裡會錯  
	卡片出來的那一刻，狀態是〔未核對〕。我的核對流程是抽兩欄，翻回 PDF 對頁碼。通常抽「樣本」和「主要結果」，因為這兩欄的數字最容易錯。對過了，把狀態改成〔已核對：日期〕。沒改狀態的卡，明天的矩陣工具就不該收。

錯過的地方有幾種。表格裡的數字最危險，AI 曾把標準差抄成人數（需作者補充：是哪一篇、怎麼發現的）。掃描版 PDF 沒有文字層，它讀不到，規則六就是為此加的：第一版沒有這條時，它硬是「摘要」了一篇它根本讀不到的論文，內容全是從篇名推出來的，讀起來還很像那麼一回事。頁碼也出過問題，PDF 閱讀器的頁數和期刊印的頁碼常常差好幾頁，它兩種都用過，所以規則二後來補了「用期刊印在頁面上的頁碼」。

還有一種錯比較隱微：它會把「主要結果」寫得比原文更肯定。原文說「可能與睡眠品質有關」，卡上變成「與睡眠品質相關」。這種錯頁碼抓不到，因為頁碼是對的，只是語氣被改了。目前我只能靠抽查時多留意結果欄的用字，還沒找到更好的辦法。

「我的用途」那一欄我堅持自己填。它可以告訴我這篇研究做了什麼，但「這篇對我的論文有什麼用」是我的判斷，這條線我不打算讓 AI 跨過去。

先做卡，還是先讀？  
這個問題我試過兩種順序。一開始是先讀完再做卡，卡變成讀完的整理，核對很快，因為內容我剛看過。後來 PDF 太多，我改成先做卡再決定讀不讀：卡上的設計、樣本、工具三欄，足夠讓我判斷這篇跟我的題目距離多遠，距離太遠的就不讀全文，卡片留著但「我的用途」寫「不納入，原因⋯」。

第二種順序省時間，但有一個風險：我可能因為一張沒核對過的卡就把一篇論文排除掉。所以我給自己一條規矩，被排除的論文至少要打開 PDF 看摘要那一頁，確認卡上的三欄沒抄錯。這樣的成本我能接受，比整篇讀完才發現不相關便宜得多。至於這條規矩會不會漏掉重要文獻，我還沒有答案，Day 12 週回顧時再誠實看一次。

明天  
Day 9：卡片累積到十張之後，把它們合併成一張文獻比較矩陣。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10408927)

[

Day 7｜文獻檢索：把搜尋策略寫成可重現的腳本（PubMed MCP）

](https://ithelp.ithome.com.tw/articles/10408927)[下一篇](https://ithelp.ithome.com.tw/articles/10409494)

[

Day 9｜十張卡攤在桌上還是看不出來：文獻比較矩陣

](https://ithelp.ithome.com.tw/articles/10409494)

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

[![.](https://itadstatic.ithome.com.tw/B9/1789958815_6ab09a9f3bf4f.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9003)

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