---
title: "讓 96GB MacBook 離線跑 DeepSeek、月費 NT$0：Redis 作者手寫的 ds4 到底是什麼？"
source: "https://vocus.cc/article/6a197c00fd89780001a5aa7f"
author:
  - "[[Slow Signals]]"
published: 2026-05-31
created: 2026-06-05
description: "多數開源專案一輩子到不了 1,000 顆星，Redis 作者 antirez 手寫的 ds4 卻在 23 天衝上 ⭐12.4k。從頭開始，只為 DeepSeek V4 Flash 這個模型打造引擎—讓 96GB RAM 的 MacBook 完全離線跑到 250K context、月費 NT$0。"
tags:
  - "clippings"
status: processed
---
> antirez 不包裝別人的程式碼，從 tokenizer 開始全部自己來。不是因為他比較厲害——是因為這個模型，真的值得有自己的引擎。

![zoomable](https://images.vocus.cc/5710551f-747f-445e-a577-f061f55265f1.png)

**圖1：ds4 解的不是「跑得快不快」，而是「一個值得的模型，該不該有自己的引擎」。**

在上一篇 deepclaude 的文末，我說「五件套的最後兩篇會盡快整理出來」。然後我就打開 ds4 的 GitHub 頁面，看著那行數字愣了一下：

****12,400 顆星。23 天。****

這個數字要怎麼理解呢？這樣說好了：多數開源專案一輩子都到不了 1,000 顆星。12,400 是一個「你確定你沒多看一位數」的數字——而且我寫這篇的當下它還在漲。

在說它做什麼之前，有一個問題更有趣：antirez 是誰，他為什麼要從頭手寫一個引擎，而不是包裝現有工具？把這件事搞懂，就算你手邊的機器今天跑不起來，你也會看懂 AI 工具世界正在發生的事。

---

## antirez 又寫了一個東西

如果你寫過任何跟資料庫有關的程式，你大概用過他的作品。就算你沒直接寫 Redis，你用的某個服務底層一定有 Redis 在跑。

Salvatore Sanfilippo——大家都叫他 antirez——是那種「寫程式寫到變成傳奇」的人。Redis 從 2009 年一個小 side project，變成全世界最流行的 in-memory 資料庫。他對開源世界的影響力，不需要我再多說。

2026 年 5 月 6 日，他在 GitHub 上推了一個新 repo：

```
antirez/ds4
```

名字很低調。DwarfStar 4，簡稱 ds4。沒有誇張的 README 標語，沒有「革命」「顛覆」「下一代」這些關鍵字。他就是寫了一個東西，推上去，然後 23 天後這東西有了一萬兩千多顆星。MIT 授權，全部開源，誰都能拿去用。

這次他寫的不是資料庫。是一個 AI 推理引擎——專門為了 DeepSeek V4 Flash 這個模型。

一個引擎，一個模型。就這樣。

---

## 為什麼要從頭造輪子？

這是大多數人看到 ds4 的第一個問題。

現在已經有 llama.cpp、Ollama、LM Studio 這些工具，都可以在本地跑 AI 模型。為什麼要從頭再寫一個？

antirez 的答案，藏在 Redis 的設計哲學裡。

Redis 從來不是一個「什麼都能存」的資料庫。它是「只存記憶體裡的 key-value，而且要做到極致」。不是 MongoDB 的競爭者，不是 PostgreSQL 的替代品。它就是它自己，只做一件事，做到沒有人比它快。

ds4 採用的是同一種思路。

DeepSeek V4 Flash 有三個特質，讓通用引擎沒辦法真正發揮它：

****第一，MoE 架構。**** 284B 參數，但每次推理只啟動一部分專家。通用引擎的 KV cache 管理是為 dense model 設計的；拿來跑 MoE，就像拿轎車底盤去裝卡車引擎——可以跑，但一定有很多東西浪費掉。

****第二，thinking mode 極短。**** DeepSeek V4 Flash 的 thinking section 只有其他模型的五分之一。而且 thinking 長度會跟問題複雜度成正比——你問簡單的，它就短；你問難的，它就多花一點時間。這在本地 coding agent 場景裡超級關鍵。其他模型 thinking mode 一開，你泡完咖啡回來它還在想。這個模型不會。

****第三，1M token context window。**** 一百萬 token 的上下文，大約等於整本《哈利波特》系列都塞進去。但通用引擎的 KV cache 沒辦法在消費級硬體上處理這麼大的窗口。antirez 做了一件事：把 KV cache 寫到硬碟。不是一次性 dump，是可以跨 session 持續讀寫的持久化設計。

這三個特性加在一起，形成一個「通用引擎做不到」的組合。antirez 沒有在既有工具的基礎上疊加功能；他從 tokenization 開始，全部自己來。

不是造輪子。是發現這台車的輪子形狀跟別人不一樣。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Ff934ed9d-40f3-4f4d-8f5d-ba2edb45c36f.png&width=740&sign=g3nl9f5vLkdDDiLeNzerxYQqnDRMX3PVzzKELVRHAE8) ![zoomable](https://images.vocus.cc/f934ed9d-40f3-4f4d-8f5d-ba2edb45c36f.png)

**圖2：這三件事不是「ds4 做得比較好」，而是「通用引擎根本做不到」——這才是 antirez 從頭寫的理由。**

---

## 一眼看懂 ds4 的硬實力

口號聽起來都很美，我自己看一個工具，習慣先把數字攤開來看。ds4 我覺得最該記住的是這幾個：

| 指標 | 數字 | 為什麼重要 |
| --- | --- | --- |
| GitHub 星數 | ⭐12.4k | 23 天衝上來，antirez 號召力 + 題材本身都站得住（2026-05-29 實測） |
| 模型規模 | 284B MoE | DeepSeek V4 Flash，每次推理只啟動一部分專家——通用引擎的 KV cache 吃不下這個架構 |
| 上下文長度 | 1M token | 約等於整套《哈利波特》塞進去；96GB RAM 本地實測可跑到 250K |
| thinking 長度 | 別人的 1/5 | 本地 coding agent 的關鍵——其他模型 thinking 一開，你泡完咖啡回來它還在想 |
| 月費 | NT$0 | 完全離線、零邊際成本（硬體成本另計） |
| 准入門檻 | 96GB RAM | 真正的結構性門檻——這不是「一般開發者」的工具 |

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fbc1043ca-6432-4d84-8175-1f4de9a6d579.png&width=740&sign=igGcufBOeV2nczKYms-tqr7qLY2eI1vFd2cc3OLWpx4) ![zoomable](https://images.vocus.cc/bc1043ca-6432-4d84-8175-1f4de9a6d579.png)

**圖3：這張表我刻意把「最香的」和「最痛的」擺在一起——零月費很誘人，96GB RAM 門檻很現實。ds4 的全部故事，其實就在這兩行之間的拉扯。**

---

## 它到底能做什麼？

最直接的答案：如果你有一台 96GB 以上 RAM 的 MacBook（或 NVIDIA GPU），ds4 可以讓你在本機跑到 250K context window 的 DeepSeek V4 Flash。

這是什麼概念？

你可以在飛機上、咖啡廳裡、網路斷掉的時候——讓一個接近 frontier 等級的 AI 幫你寫程式、讀 codebase、重構、debug。而且不用付一毛錢 API 費。

沒有月費。沒有 token 計價。沒有月底帳單。

它內建了 OpenAI-compatible server API，意思是你可以把它當成 Claude Code 或其他 coding agent 的後端。也內建了 CLI 聊天介面，甚至可以直接當 coding agent 用。

支援三種硬體後端：Mac 的 Metal、NVIDIA 的 CUDA、AMD 的 ROCm（這個是社群維護的）。

聽起來很美好。但。

---

## 那個「但」

冷靜一下。

96GB RAM 的 MacBook 不是多數人會買的規格。就算你買了，你可能是為了跑 Docker、剪影片、開 50 個 Chrome 分頁——不是為了在本地跑一個 284B 參數的模型。

如果今天就想動手，大概有兩種情境：

****第一群：隱私需求極高的開發者。**** 金融、醫療、軍工領域的工程師，程式碼不能離開本機。不是不想用雲端 API，是合規不允許。對他們來說，ds4 不是省錢工具，是「終於有解法了」的工具。

****第二群：硬體已經到位的 AI 從業者。**** 你的日常工作就是跟模型打交道，手上已經有高規格機器。ds4 讓你榨乾手邊硬體的每一分算力，把原本閒置的資源變成一個隨手可用的 AI 助理。

如果你現在還不在這兩種情境裡，反而更值得讀下去——因為 antirez 押注的邏輯，比他造的工具本身更值錢。

---

## antirez 在賭什麼？

ds4 只支援 DeepSeek V4 Flash。一個模型。

如果 DeepSeek 明年推出 V5，架構跟 V4 完全不同，ds4 的價值就會歸零。antirez 自己是知道的——他在 README 裡寫了「我們期待 DeepSeek 發布更新版本的 V4 Flash」。

這句話很有意思。他不是說「如果 DeepSeek 繼續維護 V4 Flash 線」，他是說「我們期待」。這不像是工程師在寫技術文件，比較像是一個人在公開場合說：「我相信這件事會發生，所以我把時間押上去。」

antirez 押注的不是 ds4 這個產品。他押注的是 DeepSeek V4 Flash 這條模型線的長期價值。

這個賭注值不值得？從開源世界的角度看——值得。不是因為保證成功，而是因為當下值得做。

這也正是開源最迷人的地方。不是計算過的 ROI，不是保證回本的投資。就是一個程式設計師看到一個值得解決的問題，然後開始寫程式。

23 天，12,400 顆星。證明他不是唯一這樣想的人。

---

## 它跟 deepclaude 的關係

如果你有讀上一篇 deepclaude 的文，你可能會想：這兩個東西是不是在做同一件事？

不是。它們是互補的。

|  | deepclaude | ds4 |
| --- | --- | --- |
| 跑在哪 | 雲端 API（OpenRouter / DeepSeek） | 本機（MacBook / GPU） |
| 做什麼 | 沿用 Claude Code 工作流，換便宜後端 | 完全離線跑一個模型 |
| 成本 | 月費約 NT$416 | 月費 NT$0（硬體成本另計） |
| 隱私 | 資料過雲端 | 完全不離開本機 |
| 適合誰 | 所有 Claude Code 使用者 | 高隱私需求 / 硬體到位的開發者 |

我自己會這樣想：白天用 deepclaude 的雲端方案，方便，不用管硬體；晚上或離線時，或處理敏感資料時，用 ds4。兩者加在一起，就是一套從雲端到本地的完整策略。

不是二選一。是看場景切換。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F584e6660-cdeb-4c77-99a3-8717a8e69b92.png&width=740&sign=4QuiLBznmAOaVSTuUTBmxf5pzEz_Mj2rS93rfTW22ks) ![zoomable](https://images.vocus.cc/584e6660-cdeb-4c77-99a3-8717a8e69b92.png)

**圖4：deepclaude 和 ds4 不是對手，是同一條路上的兩段。一段讓你先把帳單砍下來，一段讓你連網路都不依賴。**

---

## 誰適合關注 ds4？

誠實說，這篇寫給所有人——不是所有人今天都能動手，但所有人都能帶走點什麼。

****現在就能動手的人：****

- 你有 96GB+ RAM 的高規格機器，想知道怎麼把它榨乾
- 你的工作有隱私合規需求，資料不能離開本機

****現在動不了手、但讀完賺到的人：****

- 你對 antirez 這個人有興趣，想看一個傳奇程式設計師如何看待 AI 時代
- 你想提前看清楚「本地 AI」這條路的輪廓，等硬體普及那天直接入場
- 你對「通用 vs 專屬」的設計哲學辯論有興趣——ds4 和 Redis 的故事，說的是同一件事

工具選擇永遠是看場景，不是看規格表。

---

## 接下來

ds4 是五件套的第四篇。從 deepclaude 的「換個便宜後端」、agentmemory 的「不要每次重新解釋背景」、headroom 的「送進模型前先壓縮」，一路走到這一篇的「完全離線、零邊際成本」——你會發現我們其實在做一件事：從「租 AI」慢慢走向「擁有 AI」。

這個轉變不是每個人今天都要走——如果你的 API 月費還在 NT$640 以下，雲端方案已經夠用。但知道這條路存在，等哪天想走的時候，你不會是從零開始。

但如果你想的是更遠的事——如果有一天，AI 不是一個你每月付錢租用的服務，而是你筆電裡一個隨時待命、不依賴網路、不按 token 計費的夥伴——那 ds4 代表的，就是這條路上第一個認真的嘗試。

下一篇是五件套的最後一塊拼圖。它不換後端，也不在本地跑，而是回答一個更根本的問題： ****你每一次叫模型，用的力氣都對嗎？**** 查一個函式文件跟重構整個模組，憑什麼付一樣的錢？那篇叫 OpenSquilla——一個會自動幫你「大事叫大模型、小事叫小模型」的路由器。

****留言跟我說**** ：你的開發機器有幾 GB RAM？你願意為了完全離線的 AI 助理，花多少硬體預算？或者，你覺得這整個「本地 AI」的討論，是不是還太早？