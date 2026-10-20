---
title: "Day 4|病歷不能塗改,原始資料也是:論文專案的資料夾結構 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10408143"
author:
  - "[[iThome]]"
published:
created: 2026-09-18
description: "護理紀錄寫錯了,我們不是擦掉重寫。 是劃一條線、簽名、註記時間,然後在旁邊寫正確的。因為那份紀錄本身就是證據——它要能證明「當時發生了什麼、以及我後來怎麼修正」。塗掉的紀錄不只是不完整,它是不可信的。..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 4

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 研究生自救指南:30 天用 Claude Code 打造我的論文工具箱系列 第 4 篇

## Day 4|病歷不能塗改,原始資料也是:論文專案的資料夾結構

- 分享至

護理紀錄寫錯了,我們不是擦掉重寫。

是劃一條線、簽名、註記時間,然後在旁邊寫正確的。因為那份紀錄本身就是證據——它要能證明「當時發生了什麼、以及我後來怎麼修正」。塗掉的紀錄不只是不完整,它是不可信的。

研究資料完全一樣,而且更嚴重:你的原始資料是你整篇論文結論的唯一證據。

一旦你在原始檔上直接修改,那個證據就沒了。而你通常不會在當下發現,你會在三個月後才發現。

我後來才懂的事  
我曾經在原始的問卷檔案上直接動手:刪掉幾筆看起來明顯亂填的資料、順手把反向題算好、把幾個欄位重新命名。存檔,繼續往下做。

三個月後我發現其中一條清理規則訂錯了。想回頭改,才意識到我沒有「回頭」這個選項——原始檔已經被我改成清理後的樣子了。

那次的完整經過我留到 Day 18 講,今天只講從那件事長出來的東西:一個資料夾結構。

結構長這樣  
thesis/  
├── CLAUDE.md # 交班單  
├── refs/ # 文獻 PDF 與書目檔  
├── data/  
│ ├── raw/ # 原始資料,唯讀,只進不出  
│ └── clean/ # 清理後的資料,可以整個刪掉重生  
├── analysis/ # 清理與統計的程式碼  
├── output/ # 跑出來的表格與圖  
└── draft/ # 論文各章  
看起來沒什麼,但它背後有三個原則。

原則一:raw 只進不出  
data/raw/ 裡的東西只能新增,不能修改、不能刪除。

不管是你自己、還是 AI,都不行。

實際做法建議三層,概念上就是三讀:第一層在 CLAUDE.md 裡明文寫「不要動 data/raw/」;第二層把資料夾設成唯讀(Windows 用 attrib +R,Mac 或 Linux 用 chmod 444);第三層另外備份一份到別的地方。

三層都做,是因為單一層都會失效。規則會被忽略、唯讀會被解除、備份會忘記做。

原則二:clean 必須是「可以重新生出來的」  
這一條是整篇最重要的。

data/clean/ 裡的檔案,不應該是你手動改出來的,而應該是 analysis/ 裡的程式碼跑出來的。

判斷標準很簡單:把 clean 整個刪掉,你能不能一鍵重生?

如果可以,那你的清理過程是可重現的——規則錯了就改程式碼、重跑一次,三分鐘的事。如果不行,那你手上的資料就跟三個月後的我一樣,只能重來一次。

順帶一提,這也是論文方法學要求你交代的東西。你的清理過程本來就該講得清楚到別人可以複製,只是多數人是事後憑回憶寫進論文裡,而不是當初就記錄下來。

原則三:每個資料夾都要能回答兩個問題  
「這裡的東西是誰產生的?」和「能不能重來?」

raw:受試者產生的,不能重來。所以要用命的守。  
clean 和 output:程式碼產生的,可以重來。所以壞了不用慌。  
draft:你產生的,不能重來。所以要用版本控制,這個之後會講。  
分清楚這件事之後,你面對每個檔案的緊張程度就會不一樣。該緊張的緊張,不該緊張的就放手讓工具去做。

為什麼在 AI 進來之後,這件事變得更重要  
以前你手動改檔案,一次改一個,錯了還有機會發現。

AI 不一樣。它動得很快,而且一次動很多個。你請它「幫我把資料整理一下」,它可能在三秒內改了十二個檔案——如果邊界沒有先畫好,你連要從哪裡開始檢查都不知道。

所以邊界要在出事之前畫,不是出事之後。這跟我們在臨床做的事是同一件:流程不是為了正常的日子設計的,是為了最忙、最累、最容易出錯的那一天設計的。

明天  
Day 5:Skill 是什麼?把「你每次都要交代的事」變成一次設定——交班單的進階版。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10407883)

[

Day 3|十分鐘裝好,然後寫一份永遠不會遺失的交班單

](https://ithelp.ithome.com.tw/articles/10407883)[下一篇](https://ithelp.ithome.com.tw/articles/10408287)

[

Day 5｜Skill 是什麼：把「每次都要重講一遍的事」變成一次設定

](https://ithelp.ithome.com.tw/articles/10408287)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/6/26/268e4c01-2ca0-4fbb-8c68-27865353b041_small.png)

[錦囊妙招！WiFi 6 網速效能大解放](https://itplus.ithome.com.tw/vod-page/98?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Zyxel兆勤科技- 網路通訊專家 |

35 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/20/9d319579-71e5-4ad3-9cc6-57594bd4e2da_small.jpg)

[敏捷是解藥？還是毒藥？](https://itplus.ithome.com.tw/vod-page/147?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/16/8320d656-6b31-4bd4-9e3e-5421b391c04d_small.png)

[智慧機場誕生：桃園機場數位整合平台開發與挑戰](https://itplus.ithome.com.tw/vod-page/939?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

32 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/ade86bea-340b-45e2-8675-305768ccf15c_small.png)

[Shipping Infrastructure Safely with Modern IaC and CI/CD Patterns](https://itplus.ithome.com.tw/vod-page/1500?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

35 分