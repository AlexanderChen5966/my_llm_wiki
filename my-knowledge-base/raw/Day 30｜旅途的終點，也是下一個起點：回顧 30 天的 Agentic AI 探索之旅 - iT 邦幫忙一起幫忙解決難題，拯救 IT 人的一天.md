---
title: "Day 30｜旅途的終點，也是下一個起點：回顧 30 天的 Agentic AI 探索之旅 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10397208"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：從一個念頭開始的長途旅程 這趟長達 30 天的探索之旅，如 Day 1 前言 所述，其實起源於我在今年七月前往 維也納 參加 ACL 2025 研討會 的那段經歷。那時心裡萌生了一個念頭——如果..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 30

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 30 篇

## Day 30｜旅途的終點，也是下一個起點：回顧 30 天的 Agentic AI 探索之旅

- 分享至

## 前言：從一個念頭開始的長途旅程

這趟長達 30 天的探索之旅，如 [**Day 1 前言**](https://ithelp.ithome.com.tw/articles/10379981) 所述，其實起源於我在今年七月前往 **維也納** 參加 **[ACL 2025 研討會](https://2025.aclweb.org/)** 的那段經歷。那時心裡萌生了一個念頭——  
如果能有一個「AI 替身」，能在我忙著準備口試、趕論文的同時，替我規劃旅程、查資料、安排行程，該有多好。

在那場大會上，我見識到許多頂尖的研究、議題與觀念，以及多場與 **Agentic AI** 、 **Human-AI Collaboration（人機協作）** 相關的議程與工作坊，其中以 [**人機協作的 Tutorial**](http://acl25-hai-team.github.io/) 與 Agent 為主軸的 [**REALM Workshop**](https://realm-workshop.github.io/) 尤其令人印象深刻，內容相當豐富。這些交流讓我更深刻地體會到 **Agentic AI** 的未來樣貌與潛力——近來的突破一再超乎想像，也一步步接近過去我們所描繪的「想像中的 AI」。  
AI 不再只是回答問題，而是能主動理解任務、規劃步驟，並與人協作完成目標。  
受到這些啟發，我決定更深入探索 **Agentic AI** ，親手打造出自己的 AI 隊友——一個能與人一起思考、一起行動、一起成長的 **AI Agent** 。

這是我第一次參加 **iThome 鐵人賽** 。在報名截止前，我抱著忐忑的心按下「參賽」鈕，正式展開這場從概念到實作的「長途徒步旅行」。  
因為幾乎是一個臨時的念頭就決定參賽，沒有預先準備或囤稿，就這樣展開被緊湊 Deadline 追著跑的 30 天。  
但也正因如此，讓自己每天都能全心投入——探索一個新的概念、寫一段新的程式，並把當天的收穫整理成文章分享出來。  
這樣的節奏雖然緊張，卻讓每一天都格外充實；就像每天走完一小段山路，雖然疲累，卻能清楚感受到自己正一步一步往前邁進。

---

## 30 天的旅程回顧

這 30 天的內容，從理論出發、深入技術、最終落地應用，串起了一條完整的 **Agentic AI 探索之旅** 。  
整體架構不只是技術的堆疊，而是一條從概念、能力、實戰到應用的成長路徑，逐步形塑出一個能「思考、行動、反思、協作」的智慧系統。

在進入逐日回顧前，我先整理了這段旅程的全貌。  
以下這張心智圖，呈現了本系列從 **基礎概念、核心能力、技術實戰、進階應用，到人機共創** 的完整發展脈絡，能一眼看出整個系列如何在 **技術、應用與人機協作** 三個層面，完整探索 **Agentic AI 的發展全景** 。

  
*圖：本系列 30 天的結構全覽——從理論基礎到應用實作、再到人機共創。每一階段對應不同層面的 Agentic AI 能力，串連出從 LLM 推理、記憶、規劃到多 Agent 協作的完整旅程，構築出一個可行動、可反思、能協作的智慧隊友藍圖。*

### 1\. 基礎概念（Day 1–3）

從「為什麼需要 Agentic AI」開始，重新思考 LLM 的角色。  
不只是被動回答，而是具備行動與推進任務的能力，  
並建立以 **LLM 為基底的 Agent 架構觀念** ，作為後續開發的出發點。

- 為什麼需要 Agentic AI： [Day 1｜啟程！踏上 Agentic AI 的探索之旅](https://ithelp.ithome.com.tw/articles/10379981)
- Agentic 特性與核心能力： [Day 2｜從工具到夥伴：Agentic AI 的核心能力與設計模式](https://ithelp.ithome.com.tw/articles/10381480)
- LLM 為基礎驅動 Agent： [Day 3｜為什麼是 LLM？從強化學習到語言模型的轉折](https://ithelp.ithome.com.tw/articles/10382280)

### 2\. 核心能力（Day 4–13）

讓 LLM 學會推理（CoT）、檢索（RAG）、工具使用（Tool Use）、規劃（Planning）、記憶（Memory）與反思（Reflection）。  
這階段像是替 AI 打基礎，把零散的功能組成一套完整的「行動迴路」。  
最終進入 **Multi-Agent 協作** ，讓模型能分工合作、互相修正，展現初步的「集體智慧」。

- 推理（CoT）： [Day 4｜推理是第一步：Chain-of-Thought (CoT) 讓 LLM 從直覺走向可靠](https://ithelp.ithome.com.tw/articles/10382734)
- 檢索（RAG）： [Day 5｜接上真實世界：RAG 讓 LLM 有憑有據](https://ithelp.ithome.com.tw/articles/10383804)
- 工具使用（Tool Use）： [Day 6｜行動是關鍵：Tool Use 讓 LLM 突破回答的侷限](https://ithelp.ithome.com.tw/articles/10384461)
- 規劃（Planning）： [Day 7｜有計劃才有行動：Planning 讓 LLM 安排更周全](https://ithelp.ithome.com.tw/articles/10385063)
- ReAct（邊想邊做）： [Day 8｜邊想邊做更靈活：ReAct 讓 LLM 學會臨機應變](https://ithelp.ithome.com.tw/articles/10385814)
- 反思（Reflection）： [Day 9｜做中學才能進化：Reflection 讓 LLM 從經驗中改進](https://ithelp.ithome.com.tw/articles/10386571)
- 記憶（Memory）： [Day 10｜有了記憶才懂成長：Memory 讓 LLM 不再重蹈覆轍](https://ithelp.ithome.com.tw/articles/10387323)
- Multi-Agent（統籌型）： [Day 11｜合奏的樂曲更精彩：Multi-Agent（統籌型）讓 LLM 學會分工合作](https://ithelp.ithome.com.tw/articles/10387823)
- Multi-Agent（討論型）： [Day 12｜思想碰撞才有火花：Multi-Agent（討論型）讓 LLM 學會共識決策](https://ithelp.ithome.com.tw/articles/10388549)
- 基礎篇回顧： [Day 13｜旅途中繼站：回顧 Agentic 覺醒足跡，邁向實戰新篇章](https://ithelp.ithome.com.tw/articles/10388740)

### 3\. 技術實戰（Day 14–24）

導入 **LangChain v1.0 / LangGraph / MCP** 等核心技術。  
新版 LangChain 結構與語法全面更新，線上資源極少，這段期間幾乎都在閱讀原始碼、查文件、試錯中前進。  
但正因為摸索艱難，也讓我更深入理解新版的設計理念。

這階段運用這些核心技術一步一步更穩健、更有效率地實現 Agentic AI 的核心能力。

- [Day 14｜搭建地圖與指南針：LangChain × LangGraph（最新 v1.0 版）打造 Agent 架構骨架](https://ithelp.ithome.com.tw/articles/10389376)
- [Day 15｜讓推理看得見：LangGraph 讓 CoT 化為可見的思路地圖](https://ithelp.ithome.com.tw/articles/10390982)
- [Day 16｜讓答案有憑有據：LangChain 讓 RAG 化為可擴充的標準管線](https://ithelp.ithome.com.tw/articles/10391500)
- [Day 17｜行動力升級：LangChain 讓 Tool Use 接上真實世界](https://ithelp.ithome.com.tw/articles/10391965)
- [Day 18｜工具的高效協奏：MCP (Model Context Protocol) 讓 Tool Use 更好調度外部能力](https://ithelp.ithome.com.tw/articles/10392577)
- [Day 19｜工具跨框架共享：LangChain × MCP 讓 Tool Use 成為可攜服務](https://ithelp.ithome.com.tw/articles/10392878)
- [Day 20｜智慧的工具協同：ReAct × Ｍulti-MCP 讓 Agent 展開跨工具對話](https://ithelp.ithome.com.tw/articles/10393533)
- [Day 21｜從 ReAct 到 Planning：讓 Agent 學會動態調整維也納行程](https://ithelp.ithome.com.tw/articles/10393899)
- [Day 22｜讓思考具象化：LangGraph 讓 ReAct × Planning 的決策流程一目了然](https://ithelp.ithome.com.tw/articles/10394337)
- [Day 23｜從推理到學習：Reflection × Memory 讓 Agent 從經驗中進化](https://ithelp.ithome.com.tw/articles/10394706)
- [Day 24｜從學習到協作：Supervisor × Multi-Agent 讓 Agent 組成智慧團隊](https://ithelp.ithome.com.tw/articles/10395086)

### 4\. 進階應用（Day 25–28）

技術逐漸成熟後，我開始關注更貼近實務的議題：

- **Multimodal（多模態）** ：讓 AI 理解圖像、文件、甚至情境訊息。
- **Guardrails（安全防護）** ：設計 AI 行為邊界，避免不當輸出。
- **Human-in-the-Loop** ：在決策關鍵點引入人類審核與反饋。
- **n8n 可視化工作流** ：把整個 Agent 運作邏輯化成圖像化流程，觀察 AI 的行動脈絡。

這部分不只是技術延伸，更是讓 AI 能在真實世界「被信任地運作」的重要關鍵。

- [Day 25｜從文字到感知：多模態 Multimodal 讓 Agent 看懂世界](https://ithelp.ithome.com.tw/articles/10395412)
- [Day 26｜AI 的守護靈獸：Guardrails 讓 Agent 劃出安全邊界](https://ithelp.ithome.com.tw/articles/10395588)
- [Day 27｜讓 AI 暫停思考：Human-in-the-Loop 讓 Agent 與人類成為協作夥伴](https://ithelp.ithome.com.tw/articles/10396238)
- [Day 28｜讓 AI 用流程思考：n8n 讓 Agent 以工作流驅動協作](https://ithelp.ithome.com.tw/articles/10396549)

### 5\. 集大成：與 AI 並肩共創（Day 29）

受到人機協作的概念啟發，AI 的終點不是取代人類，而是共創協作。

以「旅遊助理」為核心範例，整合所有能力。  
AI 能自動生成行程、寄出審核郵件、根據回饋反思修正、並更新 Google Calendar。  
整體流程形成閉環： **人機協作 × 自我反思 × 持續優化** 。  
這不再只是自動化，而是能「共同創作」的 AI 隊友。

- [Day 29｜旅途最後一哩：人機協作讓 Agent 成為並肩共創的隊友，不再獨自升級！](https://ithelp.ithome.com.tw/articles/10396972)

---

## 實作與學習的收穫

### 1\. 新版 LangChain v1.0 超前部署

整個系列採用即將在十月份發布並取代舊版的 v1.0 新架構。  
新版本寫法跟觀念都與目前有不小的差異，無法直接沿用現有的學習資源。  
雖然網路上幾乎沒有教學，但邊研究官方文件邊試錯，反而讓我對底層邏輯更熟悉。  
這是一場提前的實戰演練，也算是超前部署。

### 2\. 從想法變成可運作系統

我們一步一步探索 Agentic AI 的特性與核心能力，並運用關鍵技術 LangChain / LangGraph / MCP / n8n 實現這些能力。  
每一篇 Demo 都是實際執行、截圖、驗證過的結果。  
在反覆修正中，我學到如何設計一個「可觀察、可維護」的 AI 架構。

### 3\. 每天都是新的探索

這 30 天沒有預先囤稿。  
每天都面對新的問題，有時遇到意外卡住、有時找到新解法。  
依實際的狀況一步一步走，也能讓每天的內容跟脈絡更好地銜接。  
雖然節奏緊湊，但這樣的過程讓每天都充滿動能。

### 4\. 寫作讓理解更扎實

為了把概念寫清楚，我必須重新整理邏輯、對照官方文件、實際驗證結果。  
就算過了 Deadline 或已經發佈一陣子，有想到任何點子或問題也還是會盡可能持續調整優化到自己可以接受的程度。  
每篇文章都像一次小型報告，也逼自己確保內容正確。  
長期下來，發現寫作本身就是學習的一部分。

### 5\. ACL 會議學習分享

[**Day 26 的 Guardrails**](https://ithelp.ithome.com.tw/articles/10395588) 與 [**Day 29 的人機協作**](https://ithelp.ithome.com.tw/articles/10396972) 主題，  
都延伸自我在 ACL 研討會中聽到的內容分享出來。  
那些研究強調 AI 的安全性與人機合作機制，也讓我找到把理論轉成實作的方式。

### 6\. 每天收尾的療癒時刻

每篇文章最後都放上一張這次維也納旅行的照片。  
在挑照片的過程中，也回顧了旅行當下的美好，是個療癒的時刻。

---

## 這段挑戰帶來的改變

- 發現自己真的喜歡研究、實作、整理並分享出來。
- 學會在有限時間中聚焦與持續產出。
- 對 Agentic AI 的整體觀念與架構有更完整的理解。
- 眼界更開闊，也更確定自己接下來要探索的方向。

老實說，當最後一篇完成時，心情有點複雜。  
雖然終於可以鬆一口氣，但也覺得可惜——  
還有太多主題想寫、太多想測試的想法。  
這趟旅程結束的同時，也開啟了下一個階段。

---

## 結語：從啟程到同行

回頭看，這 30 天從一個旅行靈感出發，  
走到了真正能與人協作的 AI 系統。  
過程中學到的，不只是工具與框架，  
還有如何讓 AI 在現實世界中「與人並肩」。

> **Agentic AI 的終點，不是取代人類，而是與人類一起進步。**

感謝這趟挑戰讓認識到自己 **熱愛學習並透過寫作分享** 這件事。  
也感謝一起升級的各位，希望這系列文章也能幫助你認識 Agentic AI。  
今天雖是總結篇，但對我來說，也是下一個旅途的起點。

---

![回望維也納美泉宮](https://ithelp.ithome.com.tw/upload/images/20251014/20165544HKMyHQWNYT.jpg)  
*圖：登上維也納美泉宮凱旋門（Gloriette, Schönbrunn Palace）至高點，回望美泉宮與整座城市的壯麗景色。池畔前的父子並肩而立，如同這趟與 AI 並肩前行的旅程——從探索出發，一路陪伴、彼此學習，最終在回望中看見壯麗的風景，也找到新的方向。（攝影：作者自攝）*

- [留言2](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10396972)

[

Day 29｜旅途最後一哩：人機協作讓 Agent 成為並肩共創的隊友，不再獨自升級！

](https://ithelp.ithome.com.tw/articles/10396972)

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

[![.](https://itadstatic.ithome.com.tw/B9/1779447171_6a10358375783.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2605B9004)

### 2 則留言

1

謝謝您，很完整的教學文章! 想請問說，各文章中相關的程式碼，會以Github形式統整成一包分享出來嗎? 謝謝!

- 回應 1
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[tedlctai](https://ithelp.ithome.com.tw/users/20165544/profile) iT邦新手 5 級 ‧ 2025-12-16 09:48:33 [檢舉](https://ithelp.ithome.com.tw/users/login)

謝謝您，會整理提供哦

[登入發表回應](https://ithelp.ithome.com.tw/users/login)