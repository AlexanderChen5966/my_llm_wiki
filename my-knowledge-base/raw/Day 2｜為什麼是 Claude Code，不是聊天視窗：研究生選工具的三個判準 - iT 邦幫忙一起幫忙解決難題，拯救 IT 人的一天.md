---
title: "# Day 2｜為什麼是 Claude Code，不是聊天視窗：研究生選工具的三個判準 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10407629"
author:
  - "[[iThome]]"
published:
created: 2026-09-18
description: "系列：研究生自救指南：30 天用 Claude Code 打造我的論文工具箱進度：第一週「地基」（Day 1–6）第 2 篇 先自我介紹：一個不會寫程式的護理系研究生 昨天盤點完時間都花到哪去了，今天..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 2

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 研究生自救指南:30 天用 Claude Code 打造我的論文工具箱系列 第 2 篇

## \# Day 2｜為什麼是 Claude Code，不是聊天視窗：研究生選工具的三個判準

- 分享至

> 系列：研究生自救指南：30 天用 Claude Code 打造我的論文工具箱  
> 進度：第一週「地基」（Day 1–6）第 2 篇

## 先自我介紹：一個不會寫程式的護理系研究生

昨天盤點完時間都花到哪去了，今天先讓我把自己的底交代清楚，因為接下來 28 天所有的工具，都是從這個背景長出來的。

我是護理系畢業，在病房待過幾年之後回學校念研究所。我的論文是問卷型研究：發問卷、收回來、清資料、跑統計、寫成 APA 第七版格式的文章，最後站上口試台。這條路每個護理研究生都走過，我沒有比較特別。

比較特別的可能是： **我不會寫程式** 。大學沒修過，臨床上更用不到。我會的是 Excel、SPSS 點選單、Word 排版，還有在 PubMed 上一篇篇找文獻。所以當我說「用 Claude Code 打造論文工具箱」時，我很清楚讀者心裡的第一個問題：

> 「一個護理系的，幹嘛去用工程師的終端機工具？ChatGPT 或 Claude 的聊天視窗不夠用嗎？」

這篇就是回答這個問題。

## 我在聊天視窗裡撞到的牆

老實說，我一開始就是用聊天視窗的。它很好用，好用到我第一個月幾乎每天開著。但大概從第二個月開始，我發現自己一直在做三件很像病房裡「白工」的事。

**第一，每次都要重新交班。**  
每開一個新對話，我就得重講一遍：我的論文題目是什麼、研究對象是誰、量表有幾個構面、引用要用 APA 第七版、中文文獻要放在英文前面。講完這些，才能開始問真正的問題。護理人員都知道，交班講不清楚，後面的照護一定出事。而聊天視窗的問題是，它每天都失憶，我每天都得重新交班。

**第二，做過的事留不下紀錄。**  
我請 AI 幫我把一份參考文獻格式修好，它修好了，我複製貼上。兩週後老師說第 12 筆有問題，我想回頭看當初是怎麼改的，找不到。對話被埋在幾十個「New chat」裡面。病房有一句話：「沒寫下來的，就等於沒做過。」聊天視窗做過的事，幾乎都等於沒做過。

**第三，資料進不去、結果出不來。**  
我的問卷原始檔是 Google 表單匯出的 CSV，三百多筆、六十幾欄。我不可能把它整包貼進聊天視窗，一來太大，二來裡面有受試者資料，我根本不該貼。所以我只能貼「前五筆」請 AI 示範，然後自己回 Excel 手動做完剩下的三百筆。AI 幫我想了方法，但活還是我自己幹。

這三面牆撞多了，我開始懷疑：問題不在 AI 不夠聰明，而在 **我把它放錯位置了** 。

## 三個判準：我後來怎麼選工具

撞牆之後我逼自己想清楚，一個研究生選 AI 工具，到底該看什麼。最後收斂成三條，剛好對應上面三面牆。

### 判準一：它能不能「累積」？

好的工具應該像病房的標準作業流程（SOP）：寫一次，之後每個人、每一次都照著做，不必重講。

聊天視窗的記憶是以「一次對話」為單位，Claude Code 的記憶則是以「一個資料夾」為單位。我在論文專案的根目錄放一個 `CLAUDE.md` ，把研究題目、量表構面、引用格式、我的禁忌（例如「不准捏造參考文獻」）全部寫進去。之後每次開啟，它都先讀這份檔案。這就是一份永遠不會遺失的交班單。Day 3 會帶大家實際寫一份。

### 判準二：它做的事能不能「被稽核」？

護理紀錄之所以要求「即時、正確、完整」，是因為事後一定有人要查。論文也一樣：指導老師會問、口試委員會問、期刊審查會問。

Claude Code 不是把答案顯示在視窗裡讓我複製，而是直接在我的資料夾裡動手：修改檔案、產生腳本、留下輸出。每一個動作都會變成一個「檔案的變化」，我可以看差異、可以退回、可以在兩週後打開來看它到底改了什麼。對我來說，這才是能拿去給老師看的東西。

### 判準三：它能不能「碰到我的資料」？

這是最實際的一條。我的論文不是一段文字，而是一整個資料夾：PDF 文獻、CSV 問卷、SPSS 輸出、Word 草稿。工具如果碰不到這些檔案，它就只能給建議，不能真正幫我做事。

Claude Code 在我的電腦上跑，讀的是我硬碟裡的檔案。清理三百筆問卷，它不是教我怎麼做，而是寫一支腳本跑完，然後把「哪些筆被判定為遺漏值、為什麼」列成一張表給我核對。資料沒有離開我的電腦去到某個網頁的輸入框，這件事對護理研究特別重要，Day 27 會專門談隱私與倫理。

## 同一件事，兩種做法

用一個我真的做過的例子收尾：「檢查參考文獻清單裡每一篇是否真的存在」。

|  | 聊天視窗 | Claude Code |
| --- | --- | --- |
| 怎麼開始 | 把 25 筆文獻貼進對話框 | 說一句「檢查 references.md 裡的每一筆」 |
| 它做什麼 | 逐筆「看起來」判斷，常常說「這篇應該存在」 | 寫一支腳本，逐筆去 PubMed 或 DOI 查，回傳有／沒有 |
| 留下什麼 | 一段對話，兩週後找不到 | 一支可以重跑的腳本，加一份查證結果表 |
| 我要做什麼 | 自己再去一篇篇確認 | 只看「查不到」的那幾筆 |

第二種做法會在 Day 10 完整展開。但光看這張表就能理解，我選的不是「比較聰明的 AI」，而是 **比較像研究流程的工作方式** 。

## 那聊天視窗還有用嗎？

有，而且我還是天天用。問統計概念、把一段英文摘要講給我聽、口試前腦力激盪可能被問的問題，這些「問一句、答一句」的事，聊天視窗更快、更沒負擔。

我的分法很簡單： **要留下來的、要重複做的、要碰到資料的，進 Claude Code；問完就算的，留在聊天視窗。**

## 今天的重點

1. 我是不會寫程式的護理系研究生，工具箱裡的每一件工具都是在這個前提下做的。
2. 聊天視窗的三面牆：不能累積、不能稽核、碰不到資料。
3. 選工具的三個判準，剛好是三面牆的反面：能累積、能被稽核、能碰到資料。
4. 聊天視窗沒有被淘汰，只是被放回它適合的位置。

明天 Day 3：十分鐘裝好研究生版的最小設定，以及那份「永遠不會遺失的交班單」 `CLAUDE.md` 到底該寫什麼。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10407323)

[

不是叫 AI 幫我寫論文:先搞清楚時間到底花到哪去了

](https://ithelp.ithome.com.tw/articles/10407323)[下一篇](https://ithelp.ithome.com.tw/articles/10407883)

[

Day 3|十分鐘裝好,然後寫一份永遠不會遺失的交班單

](https://ithelp.ithome.com.tw/articles/10407883)

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