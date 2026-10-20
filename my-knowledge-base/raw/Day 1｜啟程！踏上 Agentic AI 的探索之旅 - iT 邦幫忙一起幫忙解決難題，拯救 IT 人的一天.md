---
title: "Day 1｜啟程！踏上 Agentic AI 的探索之旅 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10379981"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：啟程的契機 今年七月，我有幸前往 維也納 參加 NLP 領域的頂級研討會 ACL 2025。但時程非常緊湊：畢業口試一結束，隔天一早就要搭機前往歐洲。當時因全力準備口試而焦頭爛額，完全沒有多餘心..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 1

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 1 篇

## Day 1｜啟程！踏上 Agentic AI 的探索之旅

- 分享至

## 前言：啟程的契機

今年七月，我有幸前往 **維也納** 參加 NLP 領域的頂級研討會 **[ACL 2025](https://2025.aclweb.org/)** 。但時程非常緊湊：畢業口試一結束，隔天一早就要搭機前往歐洲。當時因全力準備口試而焦頭爛額，完全沒有多餘心力安排行程。偏偏目的地既遙遠又陌生，理應要有更充足的準備才安心。那時我心裡確實閃過一個念頭——如果能有「替身能力」，讓另一個分身幫我處理行程，該有多好。

平常我在旅遊規劃上算是蠻講究的人，甚至會整理成完整的行程文件（例如以前規劃日本旅行時的紀錄）。規劃行程時，我通常會先找好想去的景點，再搜尋附近的美食，接著考慮交通方式，最後整合成每日流暢的動線。當時還得面對一些背景因素，例如疫情期間需要填寫 Visit Japan Web 等入境手續，讓準備過程更顯繁瑣。換個角度看，這其實就像一個小型的 **「旅行規劃 Agent」** ——會自己蒐集、整理、再輸出一個最合適的方案。而這也點出了 AI 若要成為真正的智慧協作夥伴，就不只要能處理靜態資訊，還得懂得搜尋並因應當下的情境與限制，做出不同的應對。

  
*圖：先前規劃日本旅行時的行程文件，除了景點、美食、交通等基本要素，也必須因應疫情期間的入境規範（如 [Visit Japan Web](https://services.digital.go.jp/zh-cmn-hant/visit-japan-web/) ）。*

這次 **還好有女友大力相助** ，幫我分擔不少細節，最後才讓行程順利又充實。但也正因如此，我更強烈地想像：要是能有個智慧 **「隊友」** ，能主動幫忙安排行程，那將會減輕許多負擔。

就在這樣的心境下，我參加了 **ACL 大會** 。會場上我見識到許多頂尖研究與應用，深受振奮，也帶回滿滿的能量，更加確信——我們需要的，是能 **「主動幫忙」** 的 AI，而不只是被動回答的工具。帶著這份啟發，我決定參加今年的 **iThome 鐵人賽** ，用 30 天的時間一步步探索 **Agentic AI** 的關鍵能力， **親手打造屬於自己的 AI 替身** 。

---

## 什麼是 Agentic AI？

簡單來說， **Agentic AI** 就是 **「主動型 AI」** 。它的特點是：只要給它一個目標，AI 就能自己想辦法完成，包含找資料、規劃步驟、執行解決方案。相比之下，傳統的 **被動式 AI** 更像一個工具，只能一問一答， **「一個口令一個動作」** ，不會持續幫你推進。

如果要找個比喻，我會想到《JOJO 的奇妙冒險》。承太郎的替身 **「白金之星」** 在覺醒時，會主動替他擋下子彈，而不是等他發號施令才行動。 **Agentic AI** 就像這樣，具備 **主動性** 與 **行動力** 。

![替身白金之星主動擋下子彈](https://ithelp.ithome.com.tw/upload/images/20250915/20165544T8pYgAL0TQ.png)  
*圖：《JOJO 的奇妙冒險》中，承太郎的替身「白金之星」會在危險來臨時主動擋下子彈。這正是 Agentic AI 想像中的樣貌——具備主動性，而非等待指令。*

放到旅遊情境裡來說，一般 AI 助手只能回答：「維也納有哪些景點？」但一個 **旅遊規劃 Agent** 就能更進一步：主動幫你蒐集資訊、依照喜好排出行程，甚至幫你準備備案，就像真正的 **智慧旅伴** 。

---

## 這 30 天我要做什麼？

既然目標是打造 **AI 替身隊友** ，我的策略不是重新訓練模型，而是讓現有的 **LLM（大型語言模型）** 逐步變得更 **Agentic** 。

在這 30 天中，我會依序探索幾項關鍵能力： **推理、使用工具、反思、自我修正、多代理協作** 等等。並且結合 **LangChain** 、 **LangGraph** 、 **MCP（Model Context Protocol）** 、 **n8n** 這些實用框架，把零散的功能串起來，組合成一個更完整的系統。

最終，我希望 AI 不只是 **回答問題的助手** ，而能成為 **「一起解決問題的夥伴」** 。甚至進一步能讓多個 Agent 組隊，各自分工，協作完成更複雜的任務。在這過程中，我也期待發現一些還沒想到的新方法或觀念，讓這趟探索更豐富。

---

## 期許與展望

這 30 天，我會完整記錄學習與實作的過程。希望這些學習經驗，也能幫助對 **Agentic AI** 有興趣的朋友們。透過這些分享，讓學習更有效率，協助讀者更快掌握重點，也幫助後來者更快上手。

我也期許自己能和大家一起成長。在這條探索之路上，我們 **不再是孤軍奮戰** ，而是能與 **AI 夥伴** 並肩前行， **共同升級** 。相信隨著更多人掌握 **主動型 AI** ，每個人都能打造出屬於自己的 **智慧替身** ，讓人與 AI 真正攜手，開展全新的協作篇章。

---

![維也納美景宮大理石廳（The Marble Hall）](https://ithelp.ithome.com.tw/upload/images/20250915/20165544MtZpWcxOjV.jpg)  
*圖：維也納美景宮（Belvedere Palace）的大理石廳 The Marble Hall。抬頭望見滿天壁畫與華麗裝飾，正如這場 30 天的挑戰，也將是一段壯闊而充滿細節的探索旅程。（攝影：作者自攝）*

- [留言1](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[下一篇](https://ithelp.ithome.com.tw/articles/10381480)

[

Day 2｜從工具到夥伴：Agentic AI 的核心能力與設計模式

](https://ithelp.ithome.com.tw/articles/10381480)

系列文

[踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友](https://ithelp.ithome.com.tw/users/20165544/ironman/8801) 共 30 篇

目錄

1. 26
	[Day 26｜AI 的守護靈獸：Guardrails 讓 Agent 劃出安全邊界](https://ithelp.ithome.com.tw/articles/10395588)
2. 27
	[Day 27｜讓 AI 暫停思考：Human-in-the-Loop 讓 Agent 與人類成為協作夥伴](https://ithelp.ithome.com.tw/articles/10396238)
3. 28
	[Day 28｜讓 AI 用流程思考：n8n 讓 Agent 以工作流驅動協作](https://ithelp.ithome.com.tw/articles/10396549)
4. 29
	[Day 29｜旅途最後一哩：人機協作讓 Agent 成為並肩共創的隊友，不再獨自升級！](https://ithelp.ithome.com.tw/articles/10396972)
5. 30
	[Day 30｜旅途的終點，也是下一個起點：回顧 30 天的 Agentic AI 探索之旅](https://ithelp.ithome.com.tw/articles/10397208)
[完整目錄](https://ithelp.ithome.com.tw/users/20165544/ironman/8801)

[![.](https://itadstatic.ithome.com.tw/B9/1780653847_6a229f1744d1f.jpg)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9001)

### 1 則留言

1

感謝 未知作者 的精彩分享！

AI 相關的技術分享總是令人興奮，期待更多深入的內容。

實際的程式碼範例很有幫助，讓理論更容易理解。

遇到的問題和解決方案分享很實用，相信很多人都會遇到類似的情況。

也歡迎版主有空參考我的系列文「南桃AI重生記」：https://ithelp.ithome.com.tw/users/20046160/ironman/8311

如果覺得有幫助的話，也歡迎訂閱支持！

- 回應
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[登入發表回應](https://ithelp.ithome.com.tw/users/login)