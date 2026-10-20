---
title: "不是模型太貴，是沒有人幫你選模型：OpenSquilla 讓大事叫大模型、小事叫小模型"
source: "https://vocus.cc/article/6a1a4ce9fd89780001eb241b"
author:
  - "[[Slow Signals]]"
published: 2026-06-02
created: 2026-06-05
description: "你的 AI agent 每次回答問題，不管是「查一個 function 文件」還是「重構 2000 行的 class」，付的都是 Claude Opus 的價錢。OpenSquilla 是五件套的最後一塊：一個「模型路由器」，每個 turn 自動判斷問題難不難。"
tags:
  - "clippings"
status: processed
---
> 你的 AI Agent 月費 $200，但有多少是花在「查天氣」上？——OpenSquilla 幫你把錢花在刀口上

![zoomable](https://images.vocus.cc/8dabc92a-7b2d-4794-9c01-563ae9e0181f.png)

**圖一：OpenSquilla 的切入點不是少用 AI，而是把每次模型選擇變成成本決策。**

---

你上次用 Claude Code 的時候，有沒有叫它幫你查過某個 function 的文件？搜過某個 error message？問過某個套件的版本號？

我有。而且常常。

這些問題，用最便宜的模型就能回答。

但你付的是 Claude Opus 的價格：每百萬 output token，約 NT$785。

查一個 function 文件大概花掉 200 token。NT$0.16。好像還好？對，一次 NT$0.16。每天二十次呢？一個月呢？加上你團隊裡另外五個人呢？

我第一次認真算這個數字的時候，真的愣了一下。

AI 工具最陰險的地方不是它貴。是它每一筆都小到你很容易當作沒看到。

---

## 不是模型太貴，是沒有人幫你選模型

我們先回頭看一下五件套到目前為止做了什麼。

deepclaude 幫你把 Claude Code 的後端從 Anthropic 換成比較便宜的選項。ds4 讓你完全離線跑模型，零 API 費。agentmemory 幫你省掉重複解釋背景的 token。headroom 在 agent 送東西進模型前先壓縮。

這四件事的共同點是：它們都在「用更少的錢，做一樣的事」。

但 OpenSquilla 想的事情不太一樣。

它不是叫你換工具、換後端、或壓 token。它是問一個更根本的問題： ****我們每一次叫模型，用的力氣，真的都是對的嗎？****

問現在幾點，跟重構一個 2000 行的 class，你付的錢是一樣的。你的 Claude Code 不會說「這個我三分力就夠了，不用叫 Opus 來」。它每一次都把最強的大腦叫出來——不管你是問天氣還是解 bug。

這不是 Claude Code 的錯。它是工具，不是會計師。它不懂省錢。

OpenSquilla 懂。

---

## 模型路由器：每個問題用對的力氣

OpenSquilla 的核心概念是「模型路由器」。每個 turn，它會自動判斷你問的問題難不難：

- 「幫我查這個 error message」 → GPT-5 Nano，幾乎免費
- 「幫我修這個 bug」→ Claude Sonnet，中等花費
- 「幫我重新設計這個模組的架構」→ Claude Opus，值得花

你不需要手動切換。不需要自己判斷「這個問題值不值得叫 Opus」。路由器幫你做決定。

打個比方。如果你家裡水電壞了，你不會叫建築師來修。你叫水電工。三十分鐘搞定，收費合理。但如果你的房子要整棟改建，你不會叫水電工來畫設計圖。你會叫建築師，付他的專業費用。

你的 AI agent 目前的行為比較像是：不管水電還是改建，全部叫建築師。然後你每個月底看到帳單，愣了一下：明明也沒做什麼大事，怎麼會用掉這麼多？

OpenSquilla 想當那個幫你叫對人的人。

為什麼這很關鍵？因為我們現在用 AI agent 的方式，跟拿法拉利去買菜差不多——車很好，但你不需要每次都踩滿油門。路由器的核心不是幫你省錢（雖然它會），是幫你把力氣用在對的地方。

---

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F02f90920-128b-442e-b47f-3b89c52c7334.png&width=740&sign=xyPZMUYV-f3GhPCu14lbyTDJKPyqMpesChS7bMh4QhM) ![zoomable](https://images.vocus.cc/02f90920-128b-442e-b47f-3b89c52c7334.png)

**圖二：從 Nano、Sonnet 到 Opus，路由器把任務難度轉成模型升級階梯。**

## 四種 Agent，四種哲學

寫到第五篇，我覺得是時候把鏡頭拉遠一點了。

為什麼這很關鍵？因為 AI coding agent 這個領域，不是只有「換一個更便宜的模型」這一種解法。不同的工具背後的設計思路完全不一樣，你如果只看功能列表，很容易選錯。

接下來這段，就算你今天一個工具都不打算換，我也希望你讀完會覺得賺到。因為它不是在推薦你裝什麼，而是一張地圖：AI coding agent 這個領域，目前其實分成四種很不一樣的設計思路。看懂這四種，你之後再看到任何一個新工具冒出來，大概都能一眼認出它站在哪一邊、它在替你省什麼、又在賭什麼。這張地圖放在判斷邊上，比任何單一工具都耐用。

這四種，不是誰好誰壞的問題；是哪一種比較貼近你現在的處境。

****第一種：重量級 loop，一模型到底****

Claude Code、Codex CLI、Cursor 都屬於這一派。一個強大的 agent loop，從讀 codebase、搜尋、改檔、跑測試、修 bug 全部包辦。背後的模型通常是固定的——Claude Opus 或 GPT-5。好處是你不用想，開下去就用。壞處是帳單也不會想，直接跳。

****第二種：劫持式換後端****

deepclaude 是這一派的代表。不重寫 agent loop，只是在你啟動前偷偷把環境變數換掉，讓 Claude Code 連到比較便宜的後端。好處是你不用學新工具。壞處是它只能換後端，無法決定「什麼任務用什麼模型」。

****第三種：完全本地，零邊際成本****

ds4 是這一派的旗艦。不依賴雲端，不按 token 計費。在你的 MacBook 或 GPU 上直接跑模型。好處是隱私 + 零月費。代價是它把成本從「每個月的 API 帳單」換成「一次性的硬體」——今天就想動手，你需要一張夠力的 GPU，或一台記憶體大一點的 Apple Silicon。手邊還沒有也沒關係，先知道這條路存在，等哪天裝備到位再走也不遲。

****第四種：微核心 + 智慧路由****

OpenSquilla 走的是這條路。它不是另一個 agent loop，而是一個輕量核心，上面可以接 20+ 種不同的模型後端。Web UI、CLI、Chat 三種入口共用同一個 turn loop——不管你從哪裡用，行為完全一致。

這四種不是競爭關係，比較像同一條路線上的四個不同站點：

```javascript
省錢入門             隱私進階             架構升維
deepclaude ────→ ds4 ────→ OpenSquilla
(換便宜後端)    (完全離線)    (自動選模型)
```

你不需要一次坐到終點。可以先從 deepclaude 開始，把帳單降下來。如果有一天你需要離線工作，再考慮 ds4。如果你管的是團隊，希望有一套完整的系統來做智慧路由——那 OpenSquilla 就是給你準備的。

---

## 它還有一些別的東西

除了路由器之外，OpenSquilla 的微核心架構還有幾個有意思的地方：

****20+ provider 支援。**** OpenRouter、OpenAI、Anthropic、Ollama、DeepSeek、Gemini、Qwen——不是「我們以後會支援」，是已經接上了。對於不想被鎖在任何一家模型公司的人來說，這個靈活度滿重要的。

****三個入口，同一套邏輯。**** Web UI、CLI、Chat 共用同一個 turn loop。tool dispatch、retry、decision logging 行為完全一樣。不會出現「Web 版可以這樣、CLI 版不行」的混亂。

****內建持久記憶體。**** 這部分跟 agentmemory 想解決的是同一個問題——不要每個 session 都重新跟 agent 解釋一次你的專案背景。

****安裝門檻很低。****

```
pip install opensquilla
```
。Windows 甚至有 portable zip，連 Python 都不用裝。

---

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F89cbeb78-2ebd-48f4-80ab-f8bf8aa04592.png&width=740&sign=qyyIo82RBWDv7SI0RswvvU65Wbp5gmncBNCKEnpzySM) ![zoomable](https://images.vocus.cc/89cbeb78-2ebd-48f4-80ab-f8bf8aa04592.png)

**圖三：OpenSquilla 的硬規格不只在多 provider，也在 decision logging 與持久記憶體。**

## 路由器有風險，使用前請詳閱公開說明書

誠實時間。

模型路由器有一個很根本的問題：如果它判斷錯了怎麼辦？

把難題當成簡單題 → 答案品質不夠，agent 可能會往錯的方向走。把簡單題當成難題 → 只是多花一點 token，還好。

但第一個情況是真的會出事的。如果你讓它幫你 debug 一個複雜的 race condition，結果路由器把它送到小模型，給了一個「看起來合理但其實不對」的解釋——你可能會在錯的方向上花更多時間。

OpenSquilla 目前有 decision logging，你可以回頭看路由器為什麼做了某個決定。但它還沒有公開的 routing accuracy benchmark。

翻譯成白話就是：路由器很聰明，但我們還不知道它有多聰明。

這不表示你不能用。這只是表示你用的時候要保持一點健康的懷疑。如果你在做高風險的工作——金融交易系統、安全漏洞分析、醫療相關程式碼——先不要讓路由器幫你做決定。人工指定模型比較安全。

對於日常 coding、重構、寫測試、查文件這種場景，路由器的「偶爾誤判」的代價很低，值得為了省下來的 token 去承受。

---

## 五件套的最後一塊拼圖

寫到這裡，五件套的五個工具全部到齊了。

| 順序 | 工具 | 做什麼 | 一句話 |
| --- | --- | --- | --- |
| 一 | deepclaude | 換後端 | 不改工作流，先把最貴的 API 呼叫換掉 |
| 二 | agentmemory | 記背景 | 不要每個 session 重複燒 token |
| 三 | headroom | 壓 token | 送進模型前先瘦身 |
| 四 | ds4 | 本地跑 | 完全離線，零邊際成本 |
| 五 | OpenSquilla | 自動路由 | 系統幫你選對的力氣做對的事 |

這五個工具是從不同層次解決「AI 成本管理」這件事。從最簡單的換後端，到最根本的架構思維改變。

我一開始寫這個系列，其實只是因為自己看到 API 帳單的時候深呼吸了好幾次。後來發現我不是唯一一個。很多人都在用 AI coding 工具，但很少人認真想過自己到底在為什麼付錢。

這個系列不是要說「AI 工具太貴了不要用」。剛好相反——我想說的是：這些工具真的很好用，所以我們更應該學會聰明地用。

省下來的錢能買咖啡。省下來的 token 能讓你在真正需要 Opus 的時候，放心地叫它出來。

---

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F90578de7-ed41-4c66-bfec-692939fd31ad.png&width=740&sign=knxfzzSe-E4Erv9cHihDX_83SGwpgKrfAt8XhSXrQgk) ![zoomable](https://images.vocus.cc/90578de7-ed41-4c66-bfec-692939fd31ad.png)

**圖四：五件套從換後端、記背景、壓 token、本機跑，最後走到自動路由。**

## 下一步

如果你只打算從五件套裡挑一件事來做，我的建議還是跟第一篇一樣：從 deepclaude 開始。它門檻最低，回報最直接。

如果你已經在 deepclaude 的省錢之路上走了一陣子，開始好奇「還有沒有更根本的方法」——那 ds4 和 OpenSquilla 就是你的下一站。

如果五件套全部看完，你只有一個 takeaway，我希望是這個： ****AI 成本管理的關鍵，不是找最便宜的模型。是讓模型做它該做的事，不做它不該做的事。****

---

## 寫到這裡，想問你一件事

你應該也有過月底打開 API 帳單、默默深呼吸一下的那個瞬間吧。

我也是過來人。而且我猜，那筆錢裡面，有不少 token 其實是連 GPT-5 Nano 都能回答的問題。

如果你方便的話，留言跟我說個數字也好、一句感想也好——我自己很好奇大家的 agent 平常都在忙些什麼，這對我之後在這裡寫東西的方向也很有幫助。沒空也完全沒關係，光是你讀到這裡，我就很感謝了。

這篇如果剛好幫你省下了一點什麼，輕輕點個愛心我就很開心了；想收藏起來慢慢看、或哪天順手轉給也在燒 API 的同事，那更是我意料之外的禮物。

五件套到這裡就走完了。如果你也覺得「把 AI 用得聰明一點」是件值得繼續聊下去的事，之後這裡還會有新的東西——想回來看看的話，我在。沒回來也沒關係，這裡會一直在。

---

**文中數字截至 2026 年 5 月，以官方最新公布為準。**