---
title: "Day 3|十分鐘裝好,然後寫一份永遠不會遺失的交班單 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10407883"
author:
  - "[[iThome]]"
published:
created: 2026-09-18
description: "護理交班有一件事很現實:口頭講的會漏,寫下來的才留得住。 而且不是寫得越多越好。交班單上如果全是流水帳,下一班會直接跳過;真正有用的交班單只寫一件事——會影響下一班判斷的資訊。 CLAUDE.md 就..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 3

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 研究生自救指南:30 天用 Claude Code 打造我的論文工具箱系列 第 3 篇

## Day 3|十分鐘裝好,然後寫一份永遠不會遺失的交班單

- 分享至

護理交班有一件事很現實:口頭講的會漏,寫下來的才留得住。

而且不是寫得越多越好。交班單上如果全是流水帳,下一班會直接跳過;真正有用的交班單只寫一件事——會影響下一班判斷的資訊。

CLAUDE.md 就是這份交班單。差別在於它不會被弄丟、不會因為今天誰值班而變樣,而且每一次開工都一定會被讀到。

今天先花十分鐘把環境裝起來,剩下的時間都花在這份交班單上。

十分鐘裝好  
安裝本身沒什麼好講的,官方文件寫得很清楚,版本號我也不寫死免得過期。流程大致是:先裝 Node.js,再用 npm 全域安裝,然後在終端機輸入 claude 啟動,第一次會要你登入。

npm install -g @anthropic-ai/claude-code  
真正會卡住的是這三個地方,先講在前面:

一、你得先開終端機。 對沒寫過程式的人這是最大的心理門檻。實際上你會用到的指令,第一週不會超過三個。

二、你要先 cd 到你的論文資料夾再啟動。 Claude Code 是「在哪個資料夾啟動,就以那個資料夾為工作範圍」。在桌面啟動它,它就看不到你的論文。這一步錯了後面全部都怪。

三、第一次啟動會問你信不信任這個資料夾。 不要無腦按同意——這個問題的意思是「我要開始讀寫這裡的檔案了」。確認你 cd 對地方了再說好。

裝完之後,先不要急著問它問題。

先寫交班單  
在論文資料夾裡開一個檔案叫 CLAUDE.md。每次啟動,它都會先讀這份。

我的版本分四塊:

這個專案是什麼。 一兩句,不用寫成研究計畫摘要。就像交班時先講「這床是誰、為什麼住院」。

我要你怎麼回話。 語言、口氣、要不要先問過我。這一塊寫得越具體,你之後省下的重講次數越多。

絕對不要做的事。 這是整份交班單最重要的區塊,等同於床頭那張過敏標示。

現在進度到哪。 我在第幾章、這週在處理什麼。這塊需要常常改,但值得。

## 這個專案是什麼

我的護理碩士論文,量性研究,目前在寫第四章結果。

## 回話方式

- 一律台灣繁體中文,不要用中國用語。
- 不要問「要不要我幫你做」,直接做,做完告訴我改了什麼。
- 我沒問的東西不要主動加。

## 絕對不要

- 不生成我沒提供來源的參考文獻。引用只能用 refs/ 裡已存在的文獻。
- 不要動 data/raw/,那是原始資料。
- 統計數字一律從 analysis/ 的輸出檔讀,不要憑印象寫。

## 現在進度

第四章結果撰寫中。表 4-1、4-2 已完成,4-3 待補。  
少即是多  
我第一版的交班單寫了快兩百行,把整份研究計畫都塞進去。結果是它抓不到重點,反而開始在回答裡夾雜一堆我根本沒問的背景說明。

砍到三十行以內之後,效果立刻好轉。

判斷標準跟臨床交班一樣:這句話會不會影響它接下來的動作? 不會的話就別寫。「本研究採橫斷性設計」寫在論文裡就好,寫在交班單上沒有用;「不要動 raw 資料」則一定要寫,因為那會改變它的行為。

什麼時候該更新它  
有一個很好用的訊號:當你發現自己又在重講同一件事的時候。

只要你第二次跟它交代同一個規則,那件事就該進交班單。這個習慣養成之後,你的交班單會自己長大,而且長出來的每一行都是有理由的。

明天  
Day 4:資料夾結構。為什麼 data/raw/ 必須是唯讀的——這件事的道理,跟病歷不能塗改是同一個。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10407629)

[

\# Day 2｜為什麼是 Claude Code，不是聊天視窗：研究生選工具的三個判準

](https://ithelp.ithome.com.tw/articles/10407629)[下一篇](https://ithelp.ithome.com.tw/articles/10408143)

[

Day 4|病歷不能塗改,原始資料也是:論文專案的資料夾結構

](https://ithelp.ithome.com.tw/articles/10408143)

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

[![.](https://itadstatic.ithome.com.tw/B9/1789119962_6aa3cdda3af6d.png)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9001)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/06b37579-92a2-4be3-a8ea-9ca641c74caa_small.png)

[別人的 K8s 死袂了 - Kubernetes Failure Stories](https://itplus.ithome.com.tw/vod-page/562?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

22 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/16/dea9b9ed-1cef-42f6-b23f-52a575af395f.png)

[【知己也要知彼 - 強化資安韌性，做好數位風險防護！】](https://itplus.ithome.com.tw/vod-page/930?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/20/77f95165-5a4e-4a44-add3-684a5a1f2ff2_small.png)

[以 Temporal 實現工作流即程式碼，打造高韌性流程](https://itplus.ithome.com.tw/vod-page/1392?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/bd4e7a30-73ea-48d2-8eee-3530c39a699b_small.png)

[如何使用 kubeflow 在不同硬體上建立 AI 開發及服務平台](https://itplus.ithome.com.tw/vod-page/1198?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

31 分