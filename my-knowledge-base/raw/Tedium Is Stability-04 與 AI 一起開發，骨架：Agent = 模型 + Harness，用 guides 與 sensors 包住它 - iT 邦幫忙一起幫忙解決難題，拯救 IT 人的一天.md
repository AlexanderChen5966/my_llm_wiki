---
title: "[Tedium Is Stability-04] 與 AI 一起開發，骨架：Agent = 模型 + Harness，用 guides 與 sensors 包住它 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401444"
author:
  - "[[iThome]]"
published:
created: 2026-08-10
description: "骨架：Agent = 模型 + Harness，用 guides 與 sensors 包住它 Agent = Model + Harness 系列文｜繁瑣即安定：與 AI 一起開發，把「沒說出口的開發..."
tags:
  - "clippings"
status: processed
---
4

## \[Tedium Is Stability-04\] 與 AI 一起開發，骨架：Agent = 模型 + Harness，用 guides 與 sensors 包住它

- 分享至

## 骨架：Agent = 模型 + Harness，用 guides 與 sensors 包住它

**Agent = Model + Harness**

> 系列文｜繁瑣即安定：與 AI 一起開發，把「沒說出口的開發規矩」也當成程式碼。

上一篇我們說，你該設計的是一個循環，而循環的好壞取決於驗證那一步。但循環不會憑空跑得穩，它需要一個東西把它包起來。這個東西，業界有個很貼切的名字： **骨架（harness）** 。

有個說法我很喜歡： **Agent = 模型 + 骨架** 。同一個模型，你給它不同的骨架，產出的品質可以天差地遠。換句話說，決定成敗的，往往不是模型多聰明，而是你替它搭了什麼樣的架子。

## 骨架有兩半：前饋的 guides，回饋的 sensors

骨架拆開來看，是兩半。

一半是 **guides（前饋控制）** ：在 AI 動手之前，就把該知道的規矩餵進去，引導它做對的事。給 AI 讀的規約檔（專案慣例、邊界、不能碰的地方）、第二篇那份義務清單的機器可讀版、可以一鍵觸發的標準流程、過去踩過坑留下的決策紀錄，都是 guides。它們讓 AI 一開始就知道「這一類任務，該顧哪些面向」。

另一半是 **sensors（回饋控制）** ：在 AI 做完之後，用機器判定結果，讓它發現做錯、然後自己修。這就是上一篇那些 verifier，一句 grep 探針、一個負向測試、一輪回歸、一個啟動時的結構檢查、一次 code 或 security review。它們讓「算不算對」有個客觀的答案，而不是靠感覺。

一句話總結： **把隱性知識寫成 guides，把「算不算對」寫成 sensors。** 這個系列前面幾篇，其實一直在準備這兩半：第二篇把義務寫下來，那是 guides；第三篇替每條義務配一個可判定的檢查，那是 sensors。

## 一條義務，配一個 guide 加一個 sensor

骨架真正落地的方式，是很具體的： **你希望 AI 顧到的每一條義務，最好同時給它一個 guide 和一個 sensor。** guide 告訴它去做，sensor 檢查它做了沒，兩個都有，這條義務才算真的被包住。

![每條義務配一個 guide 加一個 sensor](https://ithelp.ithome.com.tw/upload/images/20260804/201813648BeWxSrGug.png)

拿稽核來說：guide 是寫在規約檔裡的一句「會改狀態的動作都要記一筆稽核」，讓 AI 動手前就讀到；sensor 是一句 grep，搜那個統一寫入口的呼叫點，看新路徑在不在裡面。權限那條：guide 是「端點都要掛授權閘、綁對案件類型」，sensor 是一個負向測試，拿沒權限的身分去打、期望被擋。資料庫那條：guide 是「改欄位要同步遷移腳本、四層一起改」，sensor 是啟動時的結構檢查，對不上就紅燈。

你會發現，只給 guide 不給 sensor，AI 可能忘了做、你也不知道；只給 sensor 不給 guide，它每次都得撞牆才學會。兩個一起，循環才真的轉得穩。

## 模型只是引擎

如果要用一個畫面記住這件事，我會用開車來比。

![沒有骨架的模型像蒙眼開車](https://ithelp.ithome.com.tw/upload/images/20260804/20181364mvMvPKZYp5.png)

一個沒有骨架的模型，就像一台蒙眼開的車：引擎再強，它不知道要往哪開（沒有地圖），也不知道自己開歪了（沒有回饋），結果引擎越強、撞得越用力。「射一句 prompt 就交差」，開的就是這種車。

替它裝上骨架，就像給車配了 guides（事先的高精地圖、導航、護欄）和 sensors（即時的雷達、攝影機，一偏就修正）。同一台車、同一具引擎，卻能開得又快又穩，而且不必你全程死盯著方向盤。 **guides 讓它知道往哪開，sensors 讓它知道開歪了；模型，只是引擎。**

值得一提的是，如果你已經在用 POG，你其實已經搭好了骨架的一半。POG 那份定義任務長相的規約、那套 AI 動手前必讀的協定，本質上就是 guides，它們讓 AI 的行為穩定、不亂猜。這個系列要補的，是把「義務」也寫成 guides，再替每條義務配上 sensors，把另一半也補齊。

不過，骨架要有效，還卡著一個前提：你餵給 AI 的那些 guides，它得先看得懂、接得上。如果它連這個產品的長相、慣例、地雷都還沒摸清，你給它再漂亮的規約，它也接不住。

所以下一步，我們得先讓 AI 認識這個產品。下一篇談前導：接手一個陌生系統，怎麼先畫好地圖，再上路。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1786008610_6a745422552c3.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9001)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/20/7659574e-4e9e-4ad3-bfcf-7843d2bb0936_small.jpg)

[透過Headless CMS讓前端與行銷團隊高效合作](https://itplus.ithome.com.tw/vod-page/151?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

37 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/06b37579-92a2-4be3-a8ea-9ca641c74caa_small.png)

[別人的 K8s 死袂了 - Kubernetes Failure Stories](https://itplus.ithome.com.tw/vod-page/562?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

22 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/dbfdba4a-8f61-4ea5-be4a-2c9e66e5471e_small.png)

[基於 BPMN 的微服務設計方法](https://itplus.ithome.com.tw/vod-page/886?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/17/d985abc8-a53e-4f64-91b6-a93d33a03af6_small.png)

[90 秒就裝進你的 CI——動手攔下一次真實的 npm 供應鏈攻擊（JFrog Curation 實作）](https://itplus.ithome.com.tw/vod-page/1460?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

85 分