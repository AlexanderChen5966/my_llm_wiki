---
title: "提示詞總是寫不好？Anthropic工程師「盲點巡查」prompt，讓AI動工前先問你10個問題"
source: "https://www.bnext.com.tw/article/91490/anthropic-engineer-blind-spot-prompt"
author:
  - "[[韋惟珊]]"
published: 2026-07-13
created: 2026-07-13
description: "提示詞寫不好，往往是你不知道自己漏了什麼。Claude Code 工程師這段「盲點巡查」提示詞，可以讓 AI 動工前先幫你揪出盲點。"
tags:
  - "clippings"
status: processed
---
前幾天看到 [數位時代的一篇報導](https://www.bnext.com.tw/article/91455/anthropic-engineer-claude-harness) ，整理了 Anthropic Claude Code 工程師 Thariq Shihipar 在今年 AI Engineer World's Fair 的演講。

他說，提示詞寫不好，根本原因不是技術問題，是我們「不知道自己不知道什麼」。

登愣！對啊，因為我不知道，所以我提示詞沒寫，AI 要怎麼幫我想清楚？

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

他提出的解法叫做「盲點巡查」，在動工前挑出「我不知道」的事情，問完我再執行。我覺得挺不錯，分享給你。

如果你也有請 AI 改了很多遍還是不對的朋友，把這封信傳給他！也記得 [點文末大拇指 👍](https://bnext.surveycake.biz/s/kyG4L) ，我都會看。

## 你的每個提示，可分解成 4 種維度

你給的提示通常代表涵蓋著，：你已知自己知道的、你知道有問題但沒答案的。  
但其實還有： **你知道但沒說出來的（像是公司規範）** 、以及 **根本不知道（知識盲點）** 。後兩者，可以先讓 AI 幫你做巡查。

![#1 一天一AI](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/5ait-1783918947.jpg?w=1200&output=webp)

圖／ 數位時代

## 讓 AI 先問你問題，再開始動工

請 AI：

> 針對這份需求，問我 20 個問題，優先問那些會改變最終結果的問題。

模型會把你心裡沒說出口的假設，一條一條逼出來，尤其是會影響整體架構的那種問題，最有用。

![#2 一天一AI](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/7iwr-1783918947.jpg?w=1200&output=webp)

圖／ 數位時代

## 丟一份「範例」給 AI，讓它找你漏了什麼

手邊有類似的文件嗎？一份別人的提案、同類型的報告、甚至競品的網頁，都可以丟給 AI，說： **「對照這份，看看我的提示還漏掉了什麼？** 」它能看出你沒講清楚的地方。

![#3 一天一AI](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/1913-1783918948.jpg?w=1200&output=webp)

圖／ 數位時代

## 讓 AI 把它猜的事情，即時記下來

交辦任務時加一句： **「過程中遇到我沒交代清楚的事情，即時寫在備忘欄裡。** 」AI 做完後，你請 AI 給你備忘欄，就知道它到底猜了什麼，也是你下次寫指令時，改善的依據。

![#4 一天一AI](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/52ze-1783918948.jpg?w=1200&output=webp)

圖／ 數位時代

## 完整「盲點巡查」prompt，直接貼

以下是請AI完整「盲點巡查」的提示詞：

```
這是我想做的任務。動工前，先做盲點巡查：把我的提示當藍圖，以最終成果為依歸，找出我說清楚的（已知已知）、我知道有問題但沒回答的（已知未知）、我知道但忘了說的（未知已知）、以及我沒想到的風險（未知未知）。問我 10 個最可能改變結果的問題，動工後，把實作過程、你自己判斷的事記在備忘。
```

![#5 一天一AI](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/c3f1-1783918948.jpg?w=1200&output=webp)

圖／ 數位時代

[訂閱《一天一AI》日報](https://account.bnextmedia.com.tw/guest/subscribe?new_ids=44&utm_source=web_bn&utm_medium=post&tr_medium=website&tr_site_name=1day1ai&tr_space=post) ，每天跟你分享超實用的AI小技巧！

> 延伸閱讀：  
> [在公司推AI，同事一周就打回原形？他用「3層知識庫」把流程變成按鈕](https://www.bnext.com.tw/article/91473/ai-adoption-knowledge-base-skill-library)
> 
> [不是AI不強，是你的表格它看不懂！上傳Excel前先做這幾件事，AI才不會亂猜](https://www.bnext.com.tw/article/91460/ai-excel-table-prep)