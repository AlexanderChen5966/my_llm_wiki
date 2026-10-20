---
title: "D02｜把搜文獻、收論壇、整理引用，包成「幫我查」 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10406915"
author:
  - "[[iThome]]"
published:
created: 2026-09-11
description: "這集的坑是我自己踩的。什麼都還沒建，這是直接對 AI 對話框說「幫我查情緒勒索」，做為對照組。它回我的（全文照錄）： 情緒勒索（Emotional Blackmail）：心理學家蘇珊・佛沃（Susan..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 2

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 怎麼找到新東西？把 200 年的「發現」老方法教給 AI——方法圖鑑 × Claude Skills系列 第 2 篇

## D02｜把搜文獻、收論壇、整理引用，包成「幫我查」

- 分享至

這集的坑是我自己踩的。什麼都還沒建，這是直接對 AI 對話框說「幫我查情緒勒索」，做為對照組。它回我的（全文照錄）：

> **情緒勒索（Emotional Blackmail）** ：心理學家蘇珊・佛沃（Susan Forward）1997 年提出的概念，指一方利用恐懼、義務、罪惡感（合稱 **FOG** ：Fear, Obligation, Guilt）操控另一方，使對方為了避免衝突或維持關係而妥協，常發生於權力不平等的人際關係中，如親子、伴侶、職場、朋友之間。
> 
> **六階段循環** （各家整理略有出入，常見版本）：要求 → 抵抗 → 施壓（引發內疚、恐懼等負面情緒）→ 威脅（強調拒絕後果）→ 妥協 → 重複，進入下一輪循環。
> 
> **常見話術** ：「如果你真的愛我，你就不會讓我傷心」「你要是真的在乎我，就不會去聚會了」——常包裝在「愛」「關心」「重視」之下，讓人不易察覺。
> 
> **台灣脈絡** ：諮商心理師周慕姿的著作《情緒勒索》把這個概念在地化，強調重點不在指責被勒索者，而是覺察自己身上「容易被勒索」的特質，比喻「小偷的目標是好偷的車」，可以探索自己是否有容易被勒索的人格特質，進而學習自我保護。
> 
> 需要我針對某個面向（例如自我保護策略、或某種關係情境）再展開嗎？

整齊、完整、讀起來很有道理——但它像關起門來寫的，前後流暢首尾呼應。但無法回頭追到出處，看不出哪些是 1997 年那本書說的、哪些是後來的研究說的，也看不出這個題目現在還有哪些不同講法。現實中許多概念都有不同的觀點、定義，簡單的回答抹去了真實世界的多元。

問題不在 AI。一句「幫我查」沒告訴它去哪裡搜、引用怎麼寫、東西放哪，它只好交一篇誰讀了都不會反對、也不會多懂什麼的東西。這張底片要留著——後面每疊一支方法，都是拿來跟它對的。

> 出處：無，這集是把自己踩過的坑寫成規矩。這份整理的源頭是 Forward（1997）的普及書，本輪未逐驗學界引用。

## 先立櫃子：作業區

所以今天做兩件事：先立專案規範，再把「去搜學術文獻和論壇、引用寫清楚」包成一支 skill。

我是這樣跟它說的：

> 建立一個資料夾「作業區」，存放每天練習的輸出，格式是 D01～D30。這個專案是一個練習，逐步建構 skill 和研究體系。未來開啟這個專案的時候，先詢問今天是第幾天內容；可能有些天沒有實際操作、會跳號，就如實建立即可。

這段話看起來像在交代檔案位置，其實立了三條後面會一直用到的規矩。

\*\*一天一個位子。\*\*每天的產出是隔天的輸入，翻資料夾就看得到思考過程一天天長出來，不用另外寫進度說明。

\*\*進來先問今天是第幾天。\*\*沒問就動手，它會用昨天的上下文繼續寫，或者自己發明一個「合理的下一集」。問了，才知道該建 D02 還是 D07。

\*\*跳號就跳號。\*\*有些天沒有實際操作，不要補一篇假的交差。櫃子是實錄，不是功課表。

```markdown
flowchart LR
    A[開啟專案] --> B[先問：今天第幾天]
    B --> C[作業區/DN]
    C --> D[如實建立 跳號就空著]
```

立完之後，櫃子長這樣：

```markdown
作業區/
├── D02/
│   ├── 文獻/
│   └── 彙整報告.md
├── D03/
└── ...
```

中間哪天空了，那個編號就空著。

## 一句話讓它自己建 skill

然後是今天的 skill。我沒有先寫骨架，就一句話：

> 建立 D02 的 skill。當我說「幫我查」，就上網搜尋指定主題的學術文獻和相關領域論壇資料，每一筆都整理成可追溯的引用（題名、作者或帳號、年份、出處、連結、來源類型），存檔到 D02 的「文獻」資料夾。然後彙整報告放到 D02 裡，並且摘要所有觀點，存到D02資料夾。

這句話鎖的是搜尋本身：去哪裡找、找哪兩類、引用要寫到能回頭核對。論壇跟論文性質不同，今天先一起收、來源類型標清楚，後面要驗的時候才分得開。裡面長什麼標題、步驟寫幾步，都讓 agent 依據 skill 處理。標準骨架是後面踩坑才長出來的，今天不改它。

## 試試看

把這句話直接丟進 Claude：

> 幫我查「情緒勒索」

跑完去 `作業區/D02/` 裡翻，文獻夾有每一筆引用，彙整報告有來源、概念、能說什麼、還沒資格說什麼。跟剛才 AI 對話框那份比——那份整齊、但不知道依據哪一篇，也不知道哪裡是普及書說的、哪裡是學術文獻說的。這兩份並排，就是 skill 有沒有在做事的差別。

---

**明天** ：用肘點切出弱連結區。今天底片裡那串還沒排序的概念，明天開始要被切開。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10406879)

[

D01｜值得知道的事，多半已經存在，只是散落在兩個地方

](https://ithelp.ithome.com.tw/articles/10406879)[下一篇](https://ithelp.ithome.com.tw/articles/10406925)

[

D03｜「不熟」到底怎麼算？

](https://ithelp.ithome.com.tw/articles/10406925)

系列文

[怎麼找到新東西？把 200 年的「發現」老方法教給 AI——方法圖鑑 × Claude Skills](https://ithelp.ithome.com.tw/users/20183944/ironman/9462) 共 12 篇

目錄

1. 8
	[D07｜把五天的檔案寫成一篇給外人看的文章](https://ithelp.ithome.com.tw/articles/10407437)
2. 9
	[D08｜兩個沒人接過的題目，找一個兩邊都寫過的東西](https://ithelp.ithome.com.tw/articles/10407443)
3. 10
	[D09｜比喻只准比關係，不准比長相](https://ithelp.ithome.com.tw/articles/10407706)
4. 11
	[D10｜把問題的行話放掉，丟給遠方](https://ithelp.ithome.com.tw/articles/10407723)
5. 12
	[D11｜九個skill串成一個，順便說 skill 為什麼長成兩層](https://ithelp.ithome.com.tw/articles/10407727)
[完整目錄](https://ithelp.ithome.com.tw/users/20183944/ironman/9462)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/5bd6ccc9-6e77-4128-829c-572fc2f8fd93_small.jpg)

[一秒鐘破解一個密碼？網站如何應對多變的撞庫攻擊？](https://itplus.ithome.com.tw/vod-page/369?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

32 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/13/af82df28-d4f2-4c2f-8f3f-f0a1ef51469a_small.png)

[如何盡量避免 Throttling 在 K8s 中](https://itplus.ithome.com.tw/vod-page/910?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/5/26/dca44cec-0ad2-4b35-b22b-1d027d5670ae_small.jpg)

[大型語言模型應用開發，No Code 也可以！](https://itplus.ithome.com.tw/webinar-page/249?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/8/11/dd90d83e-35e2-412d-9cae-982c0102264b_small.png)

[AI can generate tests for you, now what?](https://itplus.ithome.com.tw/vod-page/1504?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Agentic Automation Day 企業自動化 x Agent |

31 分