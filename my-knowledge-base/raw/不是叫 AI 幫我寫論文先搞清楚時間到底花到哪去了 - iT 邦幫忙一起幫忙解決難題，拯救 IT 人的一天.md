---
title: "不是叫 AI 幫我寫論文:先搞清楚時間到底花到哪去了 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10407323"
author:
  - "[[iThome]]"
published:
created: 2026-09-18
description: "口試前一週,我在改參考文獻。不是寫,是改。三十幾筆一筆一筆對:期刊名有沒有斜體、卷數要不要斜、DOI 該用新格式還是舊的、中文作者用頓號還是逗號。改到第二十筆才發現前面十筆用的是另一套規則,全部重來。..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 1

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 研究生自救指南:30 天用 Claude Code 打造我的論文工具箱系列 第 1 篇

## 不是叫 AI 幫我寫論文:先搞清楚時間到底花到哪去了

- 分享至

口試前一週,我在改參考文獻。不是寫,是改。三十幾筆一筆一筆對:期刊名有沒有斜體、卷數要不要斜、DOI 該用新格式還是舊的、中文作者用頓號還是逗號。改到第二十筆才發現前面十筆用的是另一套規則,全部重來。

那晚我花了四個小時。四個小時裡,跟我的研究有關的時間是零。

隔天早上我想的不是「終於改完了」,而是一個更難受的問題:過去兩年,我有多少時間花在這種事情上?

誠實盤點

我把碩班做過的事分成四類,憑印象估了佔比:

1. 文獻搜尋、下載、讀、做筆記、整理成表 約 35%
2. 資料清理、反向計分、轉檔、跑分析、做表 約 25%
3. 格式APA、論文模板、圖表樣式、目錄頁碼 約 20%
4. 行政進度報告、meeting、口試簡報、委員意見追蹤 約 20%

然後問自己:哪些是只有我能做的?

答案很殘忍。只有我能做的,是「讀完文獻之後決定怎麼用它」和「看到統計結果之後決定它代表什麼」。這兩件加起來可能不到 15%。

剩下那 85% 不是不重要——格式錯了會被退回,資料清理錯了整篇論文都是錯的——而是它們有明確規則、可以驗證對錯、做一百次的做法都一樣。

這種事,正是工具該吃掉的。

這 30 天,以及我不做的三件事

接下來三十天,我會用 Claude Code 一天做一個研究生真的用得到的工具,並把它怎麼從一次性腳本長成能重複使用的工作流寫下來:需求怎麼定義、AI 會在哪裡出錯、驗證機制怎麼補。

但有三件事我不做。

不讓 AI 寫論文內容。 不是做不到,是做了之後論文就不是我的了。研究問題為什麼是這個、結果為什麼讓你意外,這些是你之所以是研究者的全部理由。我要外包的是雜事,不是思考。

不生成沒查證過的引用。 語言模型「生成一筆很像真的參考文獻」的能力,遠強過它「確認這筆文獻真的存在」的能力。這兩件事在輸出上長得一模一樣,後果差很多。Day 6 用一整篇講。

未去識別化的研究資料不離開我的電腦。 這個系列裡所有資料範例都會用假資料重建,截圖也是。

今天就做一件事

在你的論文資料夾裡開一個 CLAUDE.md,Claude Code 每次都會先讀它。寫三件「你每次都要重講一遍」的事就好:

## 這個資料夾是什麼

我的碩士論文專案,護理相關的量性研究。

## 回話方式

一律台灣繁體中文。不要問「要不要我幫你做」,直接做,做完告訴我改了什麼。

## 絕對不要

- 不生成我沒提供來源的參考文獻,引用只能用 refs/ 裡已存在的文獻。
- 不要動 data/raw/,那是原始資料。

不到十行,但以後開新對話不用再重講一遍。最後那條是我某次差點出事之後才加上去的,故事留到 Day 18。

路線圖

第一週地基,第二週文獻(檢索、摘要、引用查證、APA 校對),第三週資料(清理、匯出、表格生成),第四週寫作與整合,最後四天談隱私倫理、失敗檢討與總結。

三十天後,我希望留下的不只是三十篇文章,而是一套你知道它會在哪裡壞掉的工具箱。會壞掉這件事很重要——一個你不知道它何時會出錯的工具,比沒有工具更危險,因為你會相信它。

明天談:同樣都是 AI,為什麼我最後選了 Claude Code?三個判準,以及我一開始選錯過什麼。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[下一篇](https://ithelp.ithome.com.tw/articles/10407629)

[

\# Day 2｜為什麼是 Claude Code，不是聊天視窗：研究生選工具的三個判準

](https://ithelp.ithome.com.tw/articles/10407629)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/20/e32b71d0-e17d-4e0b-a8e3-6127a53a1c44_small.png)

[國泰人壽的可觀測性實踐](https://itplus.ithome.com.tw/vod-page/771?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/8/8/a3324e03-d6b4-46bd-bec1-1d776537bcd8.jpg)

[深入淺出 RAG：使用 Postgres 打造 RAG 系統](https://itplus.ithome.com.tw/webinar-page/273?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

27 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/709f477b-3e59-49d0-9c99-80cdb72c0d67_small.png)

[運用多維策略，打造企業級設計系統](https://itplus.ithome.com.tw/vod-page/1339?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

46 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/d3a17d75-d926-4ae7-aa8d-837e7dd463c7_small.png)

[大會主題演講 III【打造韌性數位政府：從資安威脅中突圍的零信任策略】](https://itplus.ithome.com.tw/vod-page/1235?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

28 分