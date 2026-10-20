---
title: "Day 3｜為什麼是 LLM？從強化學習到語言模型的轉折   - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10382280"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：旅行的「智慧助手」 想像你正在異地旅行。早上原本要去戶外景點，但一出門發現下雨，整個計畫被打亂。如果此時有個智慧助手，它能即時查天氣、重新規劃路線，甚至找到附近的咖啡館避雨——這就是我們對 Ag..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 3

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 3 篇

## Day 3｜為什麼是 LLM？從強化學習到語言模型的轉折

- 分享至

## 前言：旅行的「智慧助手」

想像你正在異地旅行。早上原本要去戶外景點，但一出門發現下雨，整個計畫被打亂。如果此時有個智慧助手，它能即時查天氣、重新規劃路線，甚至找到附近的咖啡館避雨——這就是我們對 **Agentic AI** 的期待：不只是單步回答，而是能隨環境動態調整，持續幫助你達成目標。

然而問題來了：要打造這樣的智慧助手，我們究竟該走哪條路？

- 是像 **AlphaGo** 那樣，用強化學習從零訓練？
- 還是收集大量資料，針對旅遊規劃或客服等場景做微調（SFT/RLHF）？
- 或者，我們能不能善用現有的 **LLM** ，直接透過 Prompt、工具與設計模式，快速把它進化成 Agent？

這正是今天要回答的問題： **為什麼我們選擇 LLM 作為 Agent 的基礎，而不是一開始就重訓或微調模型。**

---

## 過去的主角：強化學習與 AlphaGo

還記得 AlphaGo 橫掃世界冠軍的那一年嗎？  
它靠 **強化學習** 自我對弈、修正策略，最終在圍棋這個明確規則的領域達到超越人類的水準。當時許多人認為，這就是智慧 Agent 的未來。

RL 的流程看似簡單：

1. **觀察** 環境（Observation）
2. **採取** 動作（Action）
3. **獲得** 回饋（Reward）
4. **修正** 策略（Policy）

在棋局裡，獎勵函數很明確：贏就是 +1，輸就是 -1。  
但如果要把 RL 用在旅行規劃呢？獎勵該怎麼設？是預算最省？景點最多？還是舒適度最高？這些標準模糊、多維度，難以量化。

換句話說，RL 很強，但它的強項是 **封閉、單一標準的環境** 。一旦進入複雜、多元的人類世界，設計成本與訓練門檻都變得極高。

  
*圖：強化學習（RL）的 Agent–Environment 架構。此閉環在棋盤等封閉環境效果卓越，但在旅行規劃等開放任務上設計成本極高。*

眼尖的朋友可能會發現，這張圖其實和 [**昨天 (Day 2)**](https://ithelp.ithome.com.tw/articles/10381480) 所講的 **Agentic AI** 的運作概念很相似：同樣是「觀察 → 行動 → 回饋 → 策略修正」的循環。

差別在於，RL 必須透過明確定義的獎勵函數與環境模擬來推進，而 Agentic AI 則能藉由 **LLM 所帶來的語言理解能力** ，並透過工具、記憶與規劃等機制，去處理開放且複雜的真實環境。

---

## RL 的優勢與限制

- **優勢** ：在棋盤遊戲、機器控制等封閉系統中表現卓越。
- **限制** ：每個任務都要重新設計獎勵函數、環境模擬與訓練流程，成本龐大，且無法涵蓋人類細膩的偏好（例如「不吃牛肉」「想悠閒」）。

若要用 RL 訓練一個「維也納旅行 Agent」，幾乎得為每一位旅客重建一個環境並訓練，難以普及。

---

## 轉折點：語言模型的崛起

**大型語言模型（LLM）** 帶來了轉折。  
LLM 的本質是「基於機率分布、不斷進行文字接龍來生成下一個字」的強大預測器；然而，因為訓練語料涵蓋了龐大的知識與語境，它意外地展現出理解與推理的能力，能處理各種開放性問題，而不需要像 RL 那樣事先定義所有狀態與獎勵。

更重要的是，LLM 提供了三個關鍵優勢：

1. **泛用性** ：不用為每個新任務重新訓練，換個 prompt 就能切換情境。
2. **語言介面** ：人類能用自然語言直接與模型溝通，不需要專門的 reward function 設計。
3. **可組合性** ：透過工具、記憶與設計模式，LLM 能逐步演化成更 Agentic 的系統。

---

## 重新訓練、微調，還是善用現成 LLM？

講到這裡，可能有人會問：既然 RL 在現實任務上不易直接落地，那是否可以透過 **重新訓練（Pre-training）** 或 **微調（Fine-tuning, SFT/RLHF）** ，打造一個更專屬的 Agent 模型？

這確實是一條可能的路，只是以「今天要快速打造能用的 Agent」為目標， **重新訓練或微調並不是最務實的起點** ：

- **成本過高** ：從頭訓練一個模型需要龐大算力與資料，對大多數團隊而言幾乎不可行。
- **任務碎片化** ：旅行規劃、專案管理、客服回應……每個場景都不同。若要針對每個應用單獨微調，等於要做無數次「小模型」，很難維護。
- **開放性需求** ：人類任務常常模糊、多面向，很難提前定義清楚要「學什麼」再去訓練。

相比之下， **善用現成的 LLM** （例如 ChatGPT、Gemini、Claude、Llama 等）更有彈性：

- 直接透過 **Prompt 工程** 、 **設計模式** 、 **工具連結** ，就能讓同一個模型在不同場景展現 Agentic 行為。
- 不需要重新準備資料或訓練流程，能快速實驗、快速迭代。
- 保留擴充性，未來若真有需要更精準的模型，再回頭考慮 **SFT** 或 **RLHF** 。

換句話說， **現階段最好的策略不是「一開始就重訓模型」，而是「讓現有的 LLM 逐步學會當 Agent」** 。

等到我們累積了更多經驗與需求，或許才會進一步用 **SFT** 來優化行為，甚至重新設計 **RL 框架** 來提升特定能力。

---

## 小結與展望

前三天我們主要談的是 **「為什麼」** ：

- **[Day 1｜啟程！踏上 Agentic AI 的探索之旅](https://ithelp.ithome.com.tw/articles/10379981)**  
	→ 從動機出發，說明為什麼需要 Agentic AI。
- **[Day 2｜從工具到夥伴：Agentic AI 的核心能力與設計模式](https://ithelp.ithome.com.tw/articles/10381480)**  
	→ 建立能力樹與設計模式的藍圖。
- **[Day 3｜為什麼是 LLM？從強化學習到語言模型的轉折](https://ithelp.ithome.com.tw/articles/10382280)**  
	→ 解釋為什麼選擇以 LLM 為基礎，而不是一開始就走 RL 或重新訓練。

這三天像是完整的「啟程篇」，先釐清方向，再鋪好基礎。  
接下來，從 **Day 4** 開始，我們將正式進入 **「怎麼做」** ：逐步讓 LLM 學會推理、檢索、工具使用、規劃、記憶與反思，並最終具備協作與適應的能力。換句話說，從明天起，我們會把理論真正轉化為實作，一步一步養成能持續進化的 Agent。

---

![維也納史蒂芬大教堂夜景](https://ithelp.ithome.com.tw/upload/images/20250917/20165544khefS2GIj4.jpg)  
*圖：維也納史蒂芬大教堂（St. Stephen's Cathedral）在夜幕中巍然矗立，屋頂繁麗的花紋與尖塔的輪廓在黑暗裡閃耀。正如 LLM 的出現，讓智慧代理從侷限的規則系統，邁向開放而無限的真實世界。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10381480)

[

Day 2｜從工具到夥伴：Agentic AI 的核心能力與設計模式

](https://ithelp.ithome.com.tw/articles/10381480)[下一篇](https://ithelp.ithome.com.tw/articles/10382734)

[

Day 4｜推理是第一步：Chain-of-Thought (CoT) 讓 LLM 從直覺走向可靠

](https://ithelp.ithome.com.tw/articles/10382734)

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