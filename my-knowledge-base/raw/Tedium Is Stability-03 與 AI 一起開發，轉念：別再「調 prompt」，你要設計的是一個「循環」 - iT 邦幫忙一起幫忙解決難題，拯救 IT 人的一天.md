---
title: "[Tedium Is Stability-03] 與 AI 一起開發，轉念：別再「調 prompt」，你要設計的是一個「循環」 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10400615"
author:
  - "[[iThome]]"
published:
created: 2026-07-30
description: "轉念：別再「調 prompt」，你要設計的是一個「循環」 Design the Loop, Not the Prompt 系列文｜繁瑣即安定：與 AI 一起開發，把「沒說出口的開發規矩」也當成程式碼。..."
tags:
  - "clippings"
status: processed
---
2

## \[Tedium Is Stability-03\] 與 AI 一起開發，轉念：別再「調 prompt」，你要設計的是一個「循環」

- 分享至

## 轉念：別再「調 prompt」，你要設計的是一個「循環」

**Design the Loop, Not the Prompt**

> 系列文｜繁瑣即安定：與 AI 一起開發，把「沒說出口的開發規矩」也當成程式碼。

上一篇我們說，一個功能背後有一組固定、可清點的隱形義務，值得把它寫成清單。但清單只是「知道要檢查什麼」，還沒回答「怎麼做才不會漏」。而多數人真正卡住的地方，是一個更底層的習慣。

我們太習慣把跟 AI 的協作，想成「寫一句夠好的 prompt」，彷彿只要咒語夠精準，AI 就會一次把事情做對、做全。這是這個時代最貴的錯覺之一。

## prompt 不是咒語，你在設計的是一個迴圈

真相是：AI 幫你做一件真正的工作時，它從來不是「一次射出」，而是在一個 **循環** 裡跑，蒐集脈絡、動手、看結果、再調整。所以你真正該花心思設計的，不是那句話，而是這個圈。

這個圈其實很樸素： **蒐集 → 動手 → 驗證 → 再來一輪** 。蒐集，是先讀懂這個任務點亮了哪些義務（就是上一篇那份清單）；動手，是實作；驗證，是對每一條義務，跑一個「能判定對錯」的檢查；然後看結果，紅燈就回到蒐集、重來，綠燈才收工。

這裡有個關鍵洞見：這個循環的骨架，在任何任務裡都一樣。唯一會隨領域改變的，是\*\*「怎麼算對」，也就是驗證那一步\*\*。寫程式的驗證可能是跑測試，接資料的驗證可能是比對筆數，改流程的驗證可能是走一遍端到端。骨架不變，只有 verifier 隨題目換。

一旦你這樣看，身分就變了：你不再是「操作」一個聊天機器人的人，而是「設計」一個迴圈的工程師。這跟 POG 講的「別再手動在對話框裡輸入、把它結構化成可版控的東西」，其實是同一種心態。

## 咒語思維 vs 循環思維

把兩種思維並排，差別就很清楚了。

![咒語思維與循環思維的對照](https://ithelp.ithome.com.tw/upload/images/20260727/20181364RKVQEqElcr.png)

咒語思維裡，prompt 射出去，對就對、錯就錯；漏了什麼，你當下不會知道，要等三個月後帳單來了才發現。循環思維裡，每跑一輪，都有一道「驗證」把關；漏做的事會在驗證那一步被攔下來，逼你回頭補，趁它還在你手上、還沒上線的時候。

換句話說，循環思維把「會不會漏」從一種運氣，變成一個可以設計的機制。

## 循環的好壞，取決於你的 verifier

但這裡有個陷阱。循環聽起來很美，它卻有一個致命前提： **驗證那一步，必須是「可判定」的。**

如果驗證只是你自己回頭看一眼、覺得「嗯，應該有做吧」，那這個循環是空轉的，它跟咒語思維沒兩樣，只是多繞了一圈。真正有力量的循環，是把上一篇那份義務清單，一條條接上一個「機器能判定」的檢查。

![每條義務接一個可判定的檢查](https://ithelp.ithome.com.tw/upload/images/20260727/20181364xQ99XRm1ty.png)

「有沒有記稽核」這條，接上一句 grep：搜那個統一寫入口的呼叫點，看你的新路徑在不在裡面。「端點有沒有過授權」這條，接上一個負向測試：拿一個沒權限的身分去打，期望它被擋在門外。「改 A 有沒有壞 B」這條，接上一輪回歸測試，而且記得斷言「最終狀態」，而不是「這一次跑出來的差異」。「欄位跟資料庫對不對得上」這條，接上一個結構檢查，讓它在啟動時就紅燈。

你會發現，這正好把前兩篇接了起來。上一篇我們把隱形義務「寫下來」，這一篇我們替每一條配上一個「可判定的檢查」，前者讓 AI 知道要顧什麼，後者讓 AI（和你）知道到底顧到了沒有。清單，加上驗證，循環才真正轉得起來。

那麼，這個循環要跑得穩，光有清單和驗證還不夠：AI 動手前得先「懂這個產品」，你也得替它裝上該有的護欄，它才不會憑空亂猜。

下一篇，我們談那個把循環包起來、決定它跑得好不好的東西： **骨架（harness）** ，你替 AI 準備的「引導」與「感測器」。

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