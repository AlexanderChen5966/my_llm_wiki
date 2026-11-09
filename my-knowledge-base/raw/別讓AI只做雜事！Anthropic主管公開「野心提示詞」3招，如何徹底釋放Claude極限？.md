---
title: "別讓AI只做雜事！Anthropic主管公開「野心提示詞」3招，如何徹底釋放Claude極限？"
source: "https://www.bnext.com.tw/article/92896/claude-ambition-altitude-depth-range"
author:
  - "[[李先泰]]"
published: 2026-11-03
created: 2026-11-05
description: "AI不只能幫你改信。Anthropic工程主管拆解高度、深度、廣度3招，附可直接複製的提示詞，教你把交給Claude的題目想得更遠、更深、更廣。"
tags:
  - "clippings"
status: "inbox"
---
2026.11.03 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

AI不只能幫你改信。Anthropic工程主管拆解高度、深度、廣度3招，附可直接複製的提示詞，教你把交給Claude的題目想得更遠、更深、更廣。

[＃Anthropic](https://www.bnext.com.tw/tags/Anthropic) [＃Claude](https://www.bnext.com.tw/tags/Claude)

---

> 一名沒有天文專業背景的產品經理，靠著 [Claude Code](https://claude.com/product/claude-code) 等AI工具分析太空望遠鏡資料， [可能發現一顆新的系外行星](https://gizmodo.com/did-claude-help-an-amateur-astronomer-discover-a-new-planet-2000824393) （仍待後續觀測確認）。
> 
> 當有人已經用AI挑戰專業研究，有人卻還停留在問問題、改Email。 **同樣使用AI，為什麼成果差這麼多？如何讓AI不只幫你省時間，還能做到原本做不到的事？**

Anthropic工程主管Felix Rieseberg在10月13日發表的 [部落格文章](https://felixrieseberg.com/how-to-be-more-ambitious-a-tutorial-for-humans-with-models/) ，正是想回答這個問題。他曾帶領 [Claude.ai](https://claude.ai/) 、Cowork與Claude Code桌面版的工程團隊，也是開源框架 [Electron](https://www.electronjs.org/) 的共同維護者。

Rieseberg指出，AI產業經常鼓勵使用者「用AI要更有野心」，但這句話說起來容易，實際上卻很難知道該從何做起。因此，他整理出一套具體方法，教使用者如何改變向AI提問的方式。

他以企業常見的季度業務檢討（QBR）簡報為例，如果只是要求AI把數據填進既有範本，AI確實能快速完成任務；但 **更有野心的做法，是讓AI重新思考：公司真的需要這份簡報嗎？有沒有更好的管理方式，甚至能利用過去的資料，提前發現原本被忽略的問題？**

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

Rieseberg將AI應用分成四個方向： **速度、高度、深度與廣度** 。其中，「速度」是讓AI協助更快完成既有工作，也是多數人最熟悉的用法。

但如果想讓AI創造更大的價值，就不能只追求效率。以下整理Rieseberg提出的另外3種AI使用技巧，並附上可以直接複製使用的提示詞，幫助讀者從單純提高工作效率，進一步運用AI解決更複雜的問題。

## Rieseberg為什麼認為，AI用不出效果是人先限制了自己？

Rieseberg認為，卡住的往往是人對「可能」的想像。他引用心理學的「自我效能」（self-efficacy）概念：愈相信自己做得到的人，實際成就也愈高。

目標設定理論的代表學者Locke與Latham在2002年的論文中回顧35年研究，指出拿到具體、困難目標的人，表現優於只被要求「盡力就好」的人。

➜ [時間都花在整理，重要的工作總是加班做?跟著AI高手提升工作效率](https://biz-online.bnextmedia.com.tw/courses/aiworksystem?utm_source=web_bn&utm_medium=middle_text&utm_campaign=aiworksystem&utm_content=177903&utm_term=channel_1 "時間都花在整理，重要的工作總是加班做?跟著AI高手提升工作效率")

他舉的例子之一是數學家丹齊格（George Dantzig）。1939年，丹齊格在加州大學柏克萊分校上課遲到，把黑板上的兩道題抄回去當作業寫完交出，後來才知道那是統計學界還沒解開的難題。他不知道題目有多難，自然也沒替自己設限。

**Rieseberg認為，AI讓我們能做到的事每隔幾周就往前推進一次，人更需要一些方法，逼自己去做原本不覺得可能的事。**

## 第1招拉高度：為什麼要交代結果，而不是交代任務？

拉高度，就是把交給AI的事從「一個任務」換成「你真正想要的結果」。動手前先問自己： **這件事做完之後，我希望接下來發生什麼？**

Rieseberg自己的例子是，2025年初他還在請Claude逐一修App的當機問題，後來直接請Claude「讓我們的App保持不當機」。

延續季度檢討的例子，他給了三個由淺到深的版本。

**第一階：別只更新數字。** 盡可能把拿得到的資料都交給AI，讓它理解你的生意，而不只是換掉範本裡的數字。

```
告訴我這一季有什麼變化、為什麼會變，以及我們該怎麼因應。
```

**第二階：讓會議產出決策。** 好的會議最後要有決定。

```
根據你從這些資料了解到的一切，找出最急迫的三個決策，並開始為每個可能的方向整理正反論點。
```

**第三階：重新設計季度檢討本身。** 這套機制誕生在需要人工篩資料、判讀數字的年代。

```
季度檢討存在的目的，是讓高層及早發現問題，把錢和人調到對的地方。請設計一個比季度簡報更好的做法，用我們過去的資料證明，如果當初採用這個做法，本來就能更早抓到問題，並和現行做法並行跑一季。
```

怎麼判斷一件事適不適合拉高度？Rieseberg給的建議是， **如果你的工作成果需要開會討論** ，就值得試試。

**拉高度通常得先改造工作流程。** 為了讓Claude負責「App保持不當機」這個結果，他先讓Claude能自己抓取當機報告、查出每個App版本對應哪一次程式碼提交（git commit），還給它Windows、macOS和Linux電腦，讓它能獨立重現問題、驗證修正。

做到這一步，人才不必一直跟在後面派任務。

## 第2招挖深度：怎麼讓AI用專家的方法做事，不只換上專家的口吻？

挖深度，是要AI拿出業界頂尖專家用的工具和方法，一路做到連專家都可能從中學到新東西。Rieseberg點出一個常見陷阱： **叫模型「扮演專業人士」，它往往只換了用詞，做法沒變。**

以下是他常用的一段提示詞，使用時可以把原本的提示詞接在這段後面：

```
我想要這個任務更深的版本，而不是更大的版本。在改寫之前，請先做四件事：
（1）說出這個領域的頂尖專業者在這裡會做、但我沒有要求的事；
（2）指出他們會從哪份最原始的材料著手，而不是已經替我消化過的東西；
（3）列出他們會使用的具體方法與工具；
（4）說明除了你和我之外，還有什麼能證明結果是對的。
接著，改寫我的提示詞，讓它要求的正是這些。
```

回到季度檢討的例子，假設公司正為客戶流失所苦，他會搭配以下五個技巧：

> 1. **堅持用業界頂尖的工具** ：AI想用Markdown表格整理數據時，要求它改用資料庫和統計模型。你不必知道專家用什麼，Claude通常知道。
> 2. **訂一個不合理的目標，並告訴AI這很合理** ：如果你需要網站載入時間從10秒降到5秒，直接要求200毫秒。AI覺得做不到時，重申這題解得開，請它繼續做。
> 3. **挑戰你以為的事實** ：商業上很多「常識」只是粗略的近似值。把原始資料交給AI，要它反過來檢驗假設，例如：「我們認為客戶流失是新手引導（onboarding）的問題，請試著從原始資料重現這個結論。」
> 4. **產出新知識** ：AI已經深入資料之後，再請它找盲點，例如：「用我們的系統紀錄建一個客戶模擬，重現過去一年的狀況。接著找出我們還不知道、最早出現的流失訊號，並證明它在沒看過的資料上也能預測。」
> 5. **找外部裁判** ：不要讓AI自己評自己。Rieseberg認為模型更聰明之後這點可能會改善，但在2026年10月，他堅持要有外部驗證，最好是AI能自己執行、立即拿到回饋的工具。

## 第3招拓廣度：為什麼要把名詞換成動詞，再找一組跨領域團隊？

拓廣度，是讓AI從其他領域的角度處理同一件事。過去找一群不同專業的人一起看一件事並不划算，如今AI產品愈來愈容易同時跑多個代理（multi-agent），門檻低了很多。

Rieseberg坦言這招最需要創意，也是他看到最多驚喜的地方。直到看見同事Alex Albert用3D軟體 [Blender](https://www.blender.org/) 渲染室內設計，他才想到可以請Claude替自己的個人網站做3D場景。

**先把名詞換成動詞。** 這是Tom Kelley與Jonathan Littman在《The Art of Innovation》書中提過的老技巧。季度檢討表面上是「一份簡報」，換成動詞，它要做的是「在30分鐘內，讓一群忙碌的人相信一個因果故事，並在不確定中做出決定」。寫成動詞之後，人和AI都比較容易看出該找哪些領域來幫忙。

**再組一支團隊。** 假設這份簡報做不好，外星人就會炸掉地球，人類會派誰上場？Rieseberg的名單是：分析師、記者和編輯負責整體製作品質；如果要上台報告，再加一名動態與音效設計師。他也建議視情況找做空投資人與鑑識會計師組成「紅隊」，專門挑毛病。

不過這招很容易變成角色扮演。Rieseberg提醒，要AI用做空投資人研究一檔股票的方法去查，光模仿口吻只會讓對話變怪，成果不會變好。

原文沒有給這一招的完整提示詞，但依他的方法可整理出參考提示詞如下：

```
我要處理的任務是：［貼上任務］

1. 先不要動手。請用「動詞」重新描述這個任務真正要達成的事，不要用「簡報」「報告」這類名詞。

2. 根據這個動詞描述，列出5個能帶來獨特觀點的專業領域，其中至少1個負責挑毛病（例如做空投資人、鑑識會計師）。

3. 每個領域寫出：這類專家實際會做哪些研究或檢查步驟、會用什麼工具或資料。只寫做法，不要模仿他們的說話口吻。

4. 依照這些做法實際執行，整合成一份成果，並標出各領域的檢查發現了哪些問題。
```

## 這3招在哪些情況不適用？

Rieseberg指出，三招都有一個共同前提：AI要有一台能自行安裝軟體的電腦，並接得到各種相關資料，例如透過 [MCP](https://modelcontextprotocol.io/) （讓AI連上外部系統與資料的開放標準）連接器。

要提醒的是，如果只在對話框貼幾段文字，AI碰不到原始資料，深度那一招要求的資料庫與統計模型就很難做到。

Rieseberg認為，這些工作方法最適合真正追求某個最終目標的工作，創作類比較不適用；他也坦言，在2026年，單靠一句「Claude，去賺錢」，對多數企業似乎還行不通。不過他觀察到，模型能力愈強，往往就能接手愈高層次的工作。

> 延伸閱讀： [美國期中選舉懶人包：開票時間、結果何時出爐？川普關稅與台灣影響一次看](https://www.bnext.com.tw/article/92878/us-midterm-elections-2026-result-taiwan-tariff)

資料來源： [Felix Rieseberg〈How to be more ambitious: A tutorial for humans with models〉](https://felixrieseberg.com/how-to-be-more-ambitious-a-tutorial-for-humans-with-models/) 、 [Felix Rieseberg〈Hunger beats tenure: Advice for junior engineers〉](https://felixrieseberg.com/hunger-beats-tenure-advice-for-junior-engineers/) 、 [《Gizmodo》](https://gizmodo.com/did-claude-help-an-amateur-astronomer-discover-a-new-planet-2000824393) 、 [Locke與Latham目標設定研究回顧](https://med.stanford.edu/content/dam/sm/s-spire/documents/PD.locke-and-latham-retrospective_Paper.pdf)

本文初稿為AI編撰，整理．編輯/ 李先泰

[![](https://cdn.bnextmedia.com.tw/img/16x9.png "Open the Next Door 企業開門，解鎖共創下一關 2026創育共創發展交流會")](https://eventgo.bnextmedia.com.tw/event/detail/dcdmntx4qmpmec7rkf3t?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "Open the Next Door 企業開門，解鎖共創下一關 2026創育共創發展交流會")

[Open the Next Door 企業開門，解鎖共創下一關 2026創育共創發展交流會](https://eventgo.bnextmedia.com.tw/event/detail/dcdmntx4qmpmec7rkf3t?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "Open the Next Door 企業開門，解鎖共創下一關 2026創育共創發展交流會")

[

活動詳情

](https://eventgo.bnextmedia.com.tw/event/detail/dcdmntx4qmpmec7rkf3t?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "Open the Next Door 企業開門，解鎖共創下一關 2026創育共創發展交流會")

關鍵字： [＃Anthropic](https://www.bnext.com.tw/tags/Anthropic) [＃Claude](https://www.bnext.com.tw/tags/Claude)

往下滑看下一篇文章

<video controls=""></video>

Current Time 04:59

Duration 05:31

006208擬1拆8！一張24萬變3萬、0050一張逾10萬，買哪檔更划算？｜20261023

<iframe src="https://imasdk.googleapis.com/js/core/bridge3.797.1_zh_tw.html#compression_dictionary_version=%22m202611030101%22&amp;deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_1839063026%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45848619%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45869267%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B957164676%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45803173%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45843251%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45870731%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45870732%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45830425%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B1009155880%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45868504%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45867420%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45851551%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2Cnull%2Cnull%2C%5B%5D%2Cnull%2Cnull%2Cnull%2C45823412%5D%5D%2C%5B%5B16%2C%5B%5Bnull%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B100%2C%5B%5B83323870%5D%2C%5B83323871%2C%5B%5B45830425%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95404887%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B100%2C%5B%5B95394594%5D%2C%5B95394595%2C%5B%5B45803173%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398837%5D%2C%5B95398838%2C%5B%5Bnull%2C960449109%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95401453%5D%2C%5B95401454%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95403043%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45843251%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95405338%5D%2C%5B95405339%2C%5B%5B45816264%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95407374%5D%2C%5B95407375%2C%5B%5B45867420%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95407513%5D%2C%5B95407514%2C%5B%5B45868504%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95408328%5D%2C%5B95408329%2C%5B%5B45870731%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95408330%2C%5B%5B45870731%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45870732%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%2C%5B34%2C%5B%5B10%2C%5B%5B95404718%2C%5B%5Bnull%2Cnull%2Cnull%2C%5Bnull%2Cnull%2Cnull%2C%5B%5C%22CgUIsIW%2FLQ%3D%3D%5C%22%5D%5D%2Cnull%2Cnull%2Cnull%2C45823412%5D%5D%5D%2C%5B95404719%2C%5B%5Bnull%2Cnull%2Cnull%2C%5Bnull%2Cnull%2Cnull%2C%5B%5C%22CgUIsYW%2FLRIJCJ%2FH7hUSAggB%5C%22%5D%5D%2Cnull%2Cnull%2Cnull%2C45823412%5D%5D%5D%5D%2C%5B4%2Cnull%2C114%2Cnull%2Cnull%2Cnull%2Cnull%2C%5B%5C%2245851551%5C%22%5D%5D%5D%5D%5D%2C%5B35%2C%5B%5Bnull%2C%5B%5B95404720%5D%2C%5B95404721%2C%5B%5B45851551%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%2C%5B4%2Cnull%2C114%2Cnull%2Cnull%2Cnull%2Cnull%2C%5B%5C%2245851551%5C%22%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95331589%2C95332046%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=1509659975410068&amp;pvsid=8254140409471931&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F92896%2Fclaude-ambition-altitude-depth-range%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="500" height="281"></iframe>

開啟音效

<iframe frameborder="0" width="0" height="0" src="https://player.svc.litv.tv/v3/ex.html?id=ppn-Ym5leHRfd2Vi:60CD98306BA776E1F6.-0"></iframe><video controls=""></video>

00:18/09:34

閱讀全文

即時熱門文章

[1 ChatGPT生活風指令大全！LINE貼圖、月曆、旅遊行程表…30組實用提示詞一次看](https://www.bnext.com.tw/article/92865/chatgpt-image-prompts-life-creative) [2 永豐銀用戶遭「盜轉」10萬！官方認程式異常、款項全數退還，完整回應一次看](https://www.bnext.com.tw/article/92895/sinopac-bank-unauthorized-transfer-100000) [3 我被駭了，差點一夕破產！Google Cloud遭盜刷3萬元，追查才發現Mac早被植入木馬](https://www.bnext.com.tw/article/92902/google-cloud-hacked-fake-claude-code-site-mac-trojan) [4 別讓AI只做雜事！Anthropic主管公開「野心提示詞」3招，如何徹底釋放Claude極限？](https://www.bnext.com.tw/article/92896/claude-ambition-altitude-depth-range) [5 力晶投控告「揪團賣股」發起人！林緯程是誰？每股50元收購方案為何惹議？](https://www.bnext.com.tw/article/92877/powerchip-pihc-lawsuit-stake-sale-control-dispute) [6 寶可夢郵局周邊線上預購2小時搶光！郵差包、存錢筒全缺貨，沒買到還有機會？](https://www.bnext.com.tw/article/92903/pokemon-stamp-post-office-preorder-2026)

![業界首創！元大證券攜手Google Cloud，新開戶就有機會抽「Google企業級AI助理」](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/7tus-1793269555.jpg?w=900&output=webp)

2026.11.03 |

業界首創！元大證券攜手Google Cloud，新開戶就有機會抽「Google企業級AI助理」

元大證券「AI理財神隊友」活動首開國內證券業先例，投資人也能體驗Google企業級的AI助理—Gemini Enterprise，活動辦法一次看

---

分享

重點一：Gartner預估2026年底40%企業應用軟體將內建AI Agent，企業大量導入AI，員工也得學會與AI協作，會用AI正成為上班族的基本功。

重點二：消費端使用AI的比例同步攀升，Mastercard Taiwan「2026台灣消費者AI使用習慣調查」顯示，近七成台灣民眾每週使用AI助理，18至30歲使用最頻繁，31至40歲族群也有二成訂閱付費AI服務。

重點三：元大證券攜手Google Cloud推出「AI理財神隊友」活動，首開國內證券業先例，讓一般投資人也有機會拿到Google企業級的AI助理服務—Gemini Enterprise，活動自2026年11月2日至12月31日限時限量登場。

## 企業加速導入AI Agent，上班族的工作方式也跟著變了

AI走進職場的速度，比多數人想像得快。市場研究機構Gartner於2025年8月預估，到2026年底，將有4成企業應用軟體內建能處理特定任務的AI Agent。

這代表什麼？多數人對職場AI的想像，還停留在「多開一個聊天視窗問問題」。但AI Agent不會等人來問，而是直接嵌進員工每天使用的軟體裡，接手整理資料、彙整報告這類重複性工作。

換句話說，當AI陸續走進日常工作的每個環節，上班族的做事方式也跟著改變：從凡事親力親為，變成懂得把任務交給AI，再由自己把關成果。會不會用AI工具，正從履歷上的加分項，變成上班族的基本功。

## 從辦公室到個人生活，AI助理成了日常工具

有意思的是，這股改變並沒有停在辦公室門口。Mastercard Taiwan「2026台灣消費者AI使用習慣調查」顯示，近七成台灣民眾每週使用AI助理，Gemini是受訪者最常用的AI助理之一；18至30歲是使用最頻繁的族群，31至40歲族群中也有兩成已訂閱付費AI助理服務。實際掏錢訂閱，代表AI助理對這群人來說已經不是嘗鮮玩具，而是值得花錢的日常工具。

元大證券表示，AI助理正快速改變投資人取得與整理資訊的方式，從解讀財經新聞、消化法說會內容到掌握財報脈絡。這也是元大證券與Google Cloud這次跨界合作的出發點。

## 元大證券攜手Google Cloud，讓一般投資人也能體驗企業級AI助理服務

元大證券宣布攜手Google Cloud推出「AI理財神隊友」新戶活動，首開國內證券業先例，把原本主要提供給企業客戶的Gemini Enterprise開放給一般投資人體驗。只要新開立元大證券戶，就有機會獲得使用權益。

Gemini Enterprise可以做什麼呢？  
**●Token吃到飽**  
日常辦公使用無限額度，查資料不怕被降速  
**●長文件智慧摘要**  
一鍵上傳法說會、新聞、財報等，自動翻譯並抓出白話重點  
**●市場數據分析**  
快速整理市場資訊，提供分析觀點  
**●多檔標的比較**  
多檔台股、美股、ETF一次比較更輕鬆

**「AI理財神隊友」活動辦法**

● 活動期間：2026年11月2日至12月31日  
● 參加資格：活動期間內至活動網頁完成登錄及開戶  
● 抽獎機制：共抽出20名「Gemini Enterprise 6個月方案」得主  
● 加碼門檻：累計買進台股或美股任3筆，前200名優先達標者可直接獲得領取資格，不必抽獎。  
(買進標的含：台股｜整張、零股、定期定額、ETF；美股｜整股、定期定額、ETF)  
● 詳細辦法：以元大證券官網公告為準: [https://yuantasec.pse.is/9qztjd](https://yuantasec.pse.is/9qztjd)

[![元大證券](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/2u8p-1793269631.jpg?w=2000&output=webp)](https://yuantasec.pse.is/9qztjd)

圖／ 元大證券

## 元大證券投資先生APP 台美股投資輕鬆上手

身為市場龍頭券商的元大證券，旗下擁有超過500萬人次下載的「投資先生 APP」，整合了台股、美股、ETF、定期定額、零股交易等多元商品服務，因其簡單上手且系統穩定的特性，無論是投資台股或佈局美股，都是投資人的熱門選擇。除此之外，投資先生今年更全新推出「簡易模式」，介面直覺、下單便利，提供市場概況、熱門個股、產業強弱等重點資訊；讓第一次投資的新手，或想體驗簡易介面的投資人，都能更輕鬆踏出理財第一步，隨著投資經驗提升，還能一鍵切換至專業模式，兼顧簡單易上手與進階看盤需求；投資先生APP接下來也會持續加碼更多數位金融與智慧應用場景，為AI浪潮下的投資人打造更完整的數位投資體驗。

## 常見問題 FAQ

**Q1.只要開戶就一定能拿到Gemini Enterprise嗎?**  
A1.活動期間完成登錄並成功開戶，再累計買進台股或美股任3筆，前200名優先達標者可直接獲得領取資格，若未獲得早鳥資格，也可以參加抽獎(共20名)。詳細規則以官網公告為準。

**Q2.Gemini Enterprise可以幫我操作股票或建議該買什麼嗎?**  
A2.Gemini Enterprise是通用的資訊整理與增進工作效率工具，能協助使用者更快消化新聞、報告等大量文字資訊，但不提供選股、投資建議或決策判斷，AI僅為輔助工具，實際投資決策仍須由使用者自行評估。

**Q3.怎麼參加活動?**  
A3.活動期間至「AI理財神隊友」活動網頁完成登錄及新開戶即可，詳細辦法請見元大證券官網： [https://yuantasec.pse.is/9qztjd](https://yuantasec.pse.is/9qztjd)

即時熱門文章

[1 ChatGPT生活風指令大全！LINE貼圖、月曆、旅遊行程表…30組實用提示詞一次看](https://www.bnext.com.tw/article/92865/chatgpt-image-prompts-life-creative) [2 永豐銀用戶遭「盜轉」10萬！官方認程式異常、款項全數退還，完整回應一次看](https://www.bnext.com.tw/article/92895/sinopac-bank-unauthorized-transfer-100000) [3 我被駭了，差點一夕破產！Google Cloud遭盜刷3萬元，追查才發現Mac早被植入木馬](https://www.bnext.com.tw/article/92902/google-cloud-hacked-fake-claude-code-site-mac-trojan) [4 別讓AI只做雜事！Anthropic主管公開「野心提示詞」3招，如何徹底釋放Claude極限？](https://www.bnext.com.tw/article/92896/claude-ambition-altitude-depth-range) [5 力晶投控告「揪團賣股」發起人！林緯程是誰？每股50元收購方案為何惹議？](https://www.bnext.com.tw/article/92877/powerchip-pihc-lawsuit-stake-sale-control-dispute) [6 寶可夢郵局周邊線上預購2小時搶光！郵差包、存錢筒全缺貨，沒買到還有機會？](https://www.bnext.com.tw/article/92903/pokemon-stamp-post-office-preorder-2026)

[![一次搞懂工業AI](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/b4gn-1790235909.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130276)