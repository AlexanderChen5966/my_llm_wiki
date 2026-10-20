---
title: "Day 01 - LLM Wiki：知識庫不要用 RAG - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10409229"
author:
  - "[[iThome]]"
published:
created: 2026-09-17
description: "前言 今年 Andrej Karpathy 寫了一則 gist，標題叫 LLM Wiki。 裡面有一個主張，直接否定了現在做知識庫最主流的做法：不要用 RAG。 這個系列的三十天就是從那則 gist..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 1

0

[佛心分享-SideProject30](https://ithelp.ithome.com.tw/2026ironman/side-project-30)

### 為你自己蓋一座會複利的知識庫——WikiBrain系列 第 1 篇

## Day 01 - LLM Wiki：知識庫不要用 RAG

- 分享至

## 前言

今年 Andrej Karpathy 寫了一則 gist，標題叫 [**LLM Wiki**](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 。

裡面有一個主張，直接否定了現在做知識庫最主流的做法： **不要用 RAG。**

這個系列的三十天就是從那則 gist 開始的。我照著它的想法從零做了一套實作。過程中所有的設計、取捨與踩坑都會寫出來。但今天先講清楚他對 LLM Wiki 的構想。

## 他反對 RAG 的理由

先說 RAG 是什麼：檢索增強生成。你問問題，系統去資料堆裡撈出最相關的幾段，塞進提示詞，讓模型根據那幾段回答。這是現在做知識庫問答的標準答案。

Karpathy 的反對意見不是「它不準」，而是更根本的一點： **每一次查詢都在從零重新發現一次知識** 。

模型每次都要重新找出相關片段、重新把它們拼起來、重新理解它們之間的關係。他的原話是「Nothing is built up」——什麼都沒有被建立起來。

這句話的重點在於： **你昨天那次查詢所產生的理解，今天完全不存在** 。系統不記得上次它發現這兩份文件互相矛盾，不記得上次花了多少 token 才理清某個概念的脈絡。每一次提問，它都是第一次讀這些資料。

如果你的情境是偶爾查一次，這沒什麼問題。但如果你在 **長期經營一個主題** ——研究一個領域、追蹤一個產業、累積一個專案的脈絡——那你等於每天都在付一次同樣的理解成本，而且永遠不會變便宜。

順帶一提，另外兩種常見做法都有相同的缺點。 **筆記軟體** 把整理的工作留給你，你收藏了三百篇文章，那三百篇還是三百篇，除非你真的坐下來讀完、寫成自己的話；收藏本身就給了一種「我已經處理過了」的錯覺。 **AI 的記憶功能** 記的是你講過的話，不是你的資料——它知道你上週提過在研究某個主題，不代表它讀過那三百篇文章。

三種做法都缺同一道工序： **沒有人在做編纂** 。因為維護的成本會隨著資料增加而上升到無法維護的狀態。想想看你的 Notion、Obsidian……

## 他提出的替代方案

Karpathy 的主張是：與其在查詢時去翻原始資料，不如讓 LLM **持續維護一份 wiki** 。

關鍵在「持續」兩個字。新的來源進來時，系統做的不是把它索引起來，而是 **讀完它、抽出重點、整合進既有的 wiki** ——更新相關頁面、補上交叉引用、把跟現有內容矛盾的地方標記出來。

他用了一個詞形容這份 wiki：a persistent, compounding artifact，一個持續存在而且會複利的產物。

差別就在這裡。RAG 的成果是一次性的答案，答完就消失；編纂的成果是一頁會留下來的內容，而且下一次有新資料進來時，它是被加在這一頁上面，不是重新來過。

## 三層

他把這件事切成三層。這就是標題說的「三個資料夾」。

```graphql
raw/      原始來源，唯讀
wiki/     LLM 寫的頁面
schema/   規則，等於這座知識庫的 CLAUDE.md
```

**raw/ 是唯讀的** ，因為它是所有結論的憑據。你三個月後看到一個結論、想確認它是不是被 AI 幻想出來的，能查的就是這一層。它一旦可以被改寫，整條追溯鏈就斷了。

**wiki/ 是 LLM 的工作區** ，可以隨時改寫、合併、重組。他建議的結構長這樣：

```lua
wiki/
  index.md      目錄，整座知識庫的地圖
  log.md        變更紀錄，照時間排
  overview.md   總覽
  sources/      一份來源一頁的摘要
  entities/     人、組織
  concepts/     理論、方法、概念
```

**schema/ 是規則層** ，寫給 AI 看的：這座知識庫要怎麼寫、術語怎麼統一、什麼時候該開新頁。他直接類比成 CLAUDE.md，也就是你給編碼 agent 的那份專案說明。

*來源進 `raw/` 之後不再改動；agent 動筆前先讀 `schema/` 的規則，產出的頁面落在 `wiki/` 底下並互相連結。這張圖是本專案的實作版本，Karpathy 原文只有文字描述。*

## 三個操作

他定義了三個動作，這三個詞我在整個系列都會用同一組譯法。

**編纂（Ingest）** ：處理一份新來源。注意他講的規模—— **一份來源進來，通常會更新十到十五頁** 。這個數字很重要，它說明編纂不是「幫這份資料寫一篇摘要」，而是把它的內容散佈到整座知識庫該更新的每一個角落。

**對話（Query）** ：問這座 wiki 問題。搜尋相關頁面、綜合出答案，而且 **有價值的分析要存回 wiki 變成新的一頁** 。問答本身也是知識。

**健檢（Lint）** ：定期體檢。找出互相矛盾的說法、過時的主張、沒有人連到的孤兒頁、該有卻缺少的交叉引用。

另外還有一個容易跟編纂混淆的動作叫 **匯入** （import），那只是把檔案或網址放進 `raw/` ，還沒有經過任何理解。匯入是搬運，編纂才是消化。

## 他認為這樣比較好的四個理由

1. **省掉煩人的簿記工作。** 更新交叉引用、維護目錄、確認術語一致，這些事人做會累、會漏，LLM 不會。
2. **綜合是漸進累積的。** 理解被寫下來留著，而不是每次重新產生。
3. **一致性維持得住。** 交叉引用會被持續更新，不會越長越亂。
4. **複雜的問題變得可回答。** 需要綜合五份文件的問題，那五份的關係早就整合好了，不必在查詢當下重新發現。

最後他講了分工： **人負責挑選來源、提出問題；LLM 負責那些組織性的苦工。**

我認為這是整則 gist 最實際的一句話。它沒有說 AI 會幫你想出洞見，它說的是 AI 幫你做那些你明知道該做、但永遠不會抽空去做的整理工作。

## 那我為什麼要把它做成產品

看完之後我第一個念頭不是「這個概念很有趣」，而是「這個我現在就想用」。

但真的要用起來，中間隔著很多事。gist 描述的是一種工作方式，現在也有很多衍生的 Skills 或是程式碼。但是有一群人不想或不會自己建。還有一個更現實的動機：我想知道 **一個人到底能不能把這種東西做到可以收錢的程度** 。不是做一個能動的原型，是做一個別人願意把自己的資料放進去、而且可以收錢的服務。這兩者之間差的東西，正好就是這三十天要寫的內容。

## 這個模式不適合誰

我不想把它講得像萬靈丹，所以先說它不適合的情況。

**只查一次的東西不要用。** 你臨時要看一份規格書上的某個數字，直接丟給模型問就好。編纂是一種投資，投資只有在會重複使用的時候才划算。

**資料變動極快的領域效果打折。** 如果你的來源每週都被推翻，編纂出來的頁面會一直過時，維護成本大於收益。

**完全不想碰規則的話，成效有限。** 這個模式的品質上限，很大一部分取決於你有沒有花時間告訴它「這座知識庫要怎麼寫」。什麼都不設定也能用，但它就只是一個比較整齊的摘要工具。

適合的是相反的情況： **同一批資料你會反覆回頭查，而且它會持續長大** 。研究主題、專案脈絡、產業追蹤、讀書筆記，都屬於這一類。

## 這三十天要寫什麼

| 天數 | 主題 |
| --- | --- |
| 1–3 | 動機、產品長什麼樣、三層架構 |
| 4–9 | 為什麼選 MCP、六個工具怎麼設計、資料模型與樂觀鎖 |
| 10–13 | 為什麼不用 Next.js、編輯體驗、知識圖譜、深色模式與多語 |
| 14–18 | 匯入引擎：網址、無頭瀏覽器、SSRF 防護、PDF 與書目、學術引用 |
| 19–22 | 伺服器端的 agent：自己寫工具迴圈、Ingest、Query、Lint |
| 23–29 | 上線：OAuth 2.1、SEO、容器化、部署、網域與寄信、備份監控、壓力測試 |
| 30 | 收費、開源與心得 |

最後那七天是我自己最想寫的部分。網路上教你把 side project 寫出來的文章很多，教你把它 **放上線並且撐住** 的很少。容器怎麼包、平台怎麼選、網域和寄信的 DNS 怎麼設、備份怎麼確認真的還原得回來、上線後壓測發現的瓶頸跟你以為的完全不一樣——這些都會寫。

這個實作叫 **WikiBrain** ，服務在 [wikibrain.app](https://wikibrain.app/?utm_source=ironman&utm_campaign=day01) ，程式碼開源在 [GitHub](https://github.com/wikibrain-app/wikibrain) ，AGPL-3.0。文章裡提到的每一段都可以去翻完整的程式碼，包括我寫錯又改掉的那些，git 歷史都還在。有問題歡迎在留言區問，我會盡量回。

## 小結

Karpathy 反對用 RAG 做知識庫，理由是每一次查詢都在從零重新發現一次知識，什麼都沒有累積起來。他的替代方案是讓 LLM 持續維護一份會複利的 wiki，分成來源、wiki、規則三層，靠編纂、對話、健檢三個操作運轉。

說穿了，這座 LLM Wiki 就是三個資料夾。接下來三十天要回答的是： **當你把這三個資料夾交給 AI 維護，工作流會變成什麼樣子，以及要付出什麼代價，才能讓它從一個資料夾變成一個真的有人付錢用的產品。**

明天實際跑一遍給你看：丟一個網址進去，按下編纂，看它產生哪些頁面、怎麼更新目錄，以及第二份來源進來時會發生什麼事。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[下一篇](https://ithelp.ithome.com.tw/articles/10409505)

[

Day 02 - 深入 Karpathy 的 LLM wiki 架構

](https://ithelp.ithome.com.tw/articles/10409505)

系列文

[為你自己蓋一座會複利的知識庫——WikiBrain](https://ithelp.ithome.com.tw/users/20184129/ironman/9590) 共 7 篇

目錄

1. 3
	[Day 03 - 把 Karpathy 的六條準則寫成程式](https://ithelp.ithome.com.tw/articles/10409772)
2. 4
	[Day 04 - 用 LLM 執行 Karpathy 的六條準則](https://ithelp.ithome.com.tw/articles/10409915)
3. 5
	[Day 05 - Karpathy 的準則，在 WikiBrain 實現會自我強化的知識庫](https://ithelp.ithome.com.tw/articles/10410555)
4. 6
	[Day 06 - 為什麼選 MCP，以及怎麼用無狀態的 Streamable HTTP 實作](https://ithelp.ithome.com.tw/articles/10411041)
5. 7
	[Day 07 - 把提示詞寫進工具描述與錯誤訊息](https://ithelp.ithome.com.tw/articles/10411905)
[完整目錄](https://ithelp.ithome.com.tw/users/20184129/ironman/9590)

[![.](https://itadstatic.ithome.com.tw/B9/1789119962_6aa3cdda3af6d.png)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9001)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/10/12/1f29d019-5d7a-4699-8fae-f90b58f7c1b3_small.png)

[打造 MLOps 平台改善 AI 模型開發流程](https://itplus.ithome.com.tw/webinar-page/175?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/21/8965c726-8e9e-4798-94d6-7fa6296d5c66_small.png)

[識別出遠距團隊的 bad smell](https://itplus.ithome.com.tw/vod-page/746?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

34 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/14/3002f288-e430-41e7-8867-695f3b75510b_small.png)

[審計機關因應數位轉型發展趨勢之作為](https://itplus.ithome.com.tw/vod-page/650?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/a09fa9f5-f703-4d2f-9bbf-4a1144fa59c3_small.png)

[DataOps + ChatOps: 打造跨團隊協作的控制面](https://itplus.ithome.com.tw/vod-page/1473?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

25 分