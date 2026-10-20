---
title: "Day 2｜從工具到夥伴：Agentic AI 的核心能力與設計模式 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10381480"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：旅行中的「分工角色」 還記得 昨天 Day 1 提到的「旅行規劃 Agent」嗎？ 如果把一次完整的旅行想像成一個專案，就會發現光靠一個人其實很難處理好所有細節。通常我們會分工：有人專門安排行程..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 2

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 2 篇

## Day 2｜從工具到夥伴：Agentic AI 的核心能力與設計模式

- 分享至

## 前言：旅行中的「分工角色」

還記得 **[昨天 Day 1](https://ithelp.ithome.com.tw/articles/10379981)** 提到的「旅行規劃 Agent」嗎？  
如果把一次完整的旅行想像成一個專案，就會發現光靠一個人其實很難處理好所有細節。通常我們會分工：有人專門安排行程（Planner）、有人研究當地美食（Researcher）、有人專門記錄交通方式（Navigator）、有人會在突發狀況時做臨場調整（Problem Solver）。

這些角色各有特色，但共同構成了一個能完成複雜任務的團隊。 **Agentic AI 也是一樣** ：它不是一個單一的「問答機器」，而是透過不同能力模組與設計模式（Design Patterns）的組合，變成能在動態環境下持續「做事」的智慧夥伴。

---

## 從被動到主動：LLM 與 Agentic AI 的分水嶺

傳統的 LLM 多半是「一問一答」：你問它，它答你；你改條件，它再重答。這種互動雖然方便，但在面對多步驟、需要即時外部資訊、或需要持續追蹤狀態的任務時，往往顯得不足。

而 **Agentic AI** 的價值在於「把 **目標** 丟給系統，讓系統 **自己想辦法完成** 」。它會主動查資料、呼叫工具、拆解任務、反思修正，並在必要時與其他 Agent 協作或重新規劃，直到達成目標為止。

---

## Agentic AI 的核心能力

要讓 AI 從「工具」走向「夥伴」，它必須具備一系列核心能力，這些能力像是一棵逐層展開的能力樹，從基礎的感知環境，到高階的規劃、反思與協作，彼此環環相扣。

- **Autonomy（自主性）**  
	不必等人類逐條下指令，能依既定目標自行推進。交代「三天行程、預算 300 歐、偏好藝術與咖啡館」後，會主動展開安排，而不是只回一串清單。
- **Perception（感知）**  
	能察覺環境狀態與變化：天氣轉雨、景點臨時休館、交通延誤、餐廳營業時段、票價浮動等都在視野內。
- **Reactivity（反應力）**  
	在偵測到變化時立即採取合適對策：下雨就把戶外換成室內動線；火車誤點則改用其他路線或調整到站時間，避免硬走既定計畫。
- **Planning（規劃力）**  
	把大目標拆成可執行步驟並安排順序，必要時預先準備備案。實務上常結合逐步思考（如 CoT / ToT）與地理動線、開放時段、移動成本等約束來調整。
- **Tool Use（工具使用）**  
	不只「會說」，還能「會查、會算、會驗證」：調用天氣與票價 API、用地圖估算移動時間、檢視餐廳菜單或訂位可用性，讓建議從想像落到可執行。
- **Memory（記憶）**  
	把偏好與限制寫入並在後續沿用：不吃牛肉、偏好咖啡館、對走路距離的耐受度、曾挑過的餐廳／景點等，後續規劃會自動避開不合適選項並優先符合偏好。
- **Reflection（反思）**  
	產出不是終點；會自檢與修正。若發現同日移動過遠、預算超標、或排程過於緊湊，能主動重排或提供替代方案。
- **Learning & Adaptivity（學習與適應）**  
	不必重訓模型，也能隨互動調整決策傾向：觀察你常拒絕早場行程，下次就延後起點；看到你偏好步行多於轉車，規劃會逐步貼近這種節奏。
	> 與 Memory 的差異在於：Memory 記的是「事實／偏好」，Learning 調的是「策略／行為傾向」。
- **Collaboration（協作）**  
	能與其他 Agent 或模組分工並整合：有人專責動線、有人看預算、有人審查可行性；協調者把結果彙整成一致方案，讓 1 + 1 > 2。
- **Goal-oriented（目標導向）**  
	始終圍繞任務目標行動，而非生成漂亮但不落地的文字；當目標是「三天 300 歐、偏好藝術、避免牛肉」，整體決策都圍繞這個框架收斂。

---

## Agentic AI 的運作

這些能力如何協同？可以用一個循環來理解：

1. **Input** ：人類給定目標（例如三天行程、300 歐預算）。
2. **Observation** ：Agent 持續觀察環境（天氣、交通、景點營業資訊）。
3. **Action** ：依據觀察與規劃做出行動（呼叫工具、生成建議）。
4. **Environment** ：行動會改變環境（例如預訂完成、選擇的景點已被標記為已訪）。
5. **Loop** ：環境回饋再被觀察，觸發下一輪決策，直到達成目標。

這種「有刺激就有反應」的循環，讓 Agentic AI 能夠在動態情境下持續推進，而不是停留在靜態的一次性回答。

![Agentic AI 的運作流程](https://ithelp.ithome.com.tw/upload/images/20250918/20165544ZlJWBYSU20.png)  
圖：Agentic AI 的運作流程。人類給定目標後，Agent 會透過規劃、觀察與行動與環境互動，並藉由反思與記憶修正策略，形成持續推進的閉環。

---

## Agentic Design Patterns

在實務上，要讓這些能力真正落地，不會只靠模型「天生就會」，而是需要一些結構化的設計方式。這些 **Design Patterns（設計模式）** 就像是常用的積木，讓我們能把不同的能力模組組合起來，搭建出更可靠、可擴展的 Agent 系統：

- **Tool Use Pattern**  
	主動呼叫外部工具（天氣 / 地圖 / 搜尋 / 票價 API、資料庫、OCR 等），並能判斷「何時用、該用哪個」。
- **Planning Pattern**  
	把多步驟任務拆解為子任務並安排順序，必要時還會設計備案，確保在環境變動下依舊能前進。
- **Reflection Pattern**  
	產出不是結束，而是開始。會自動檢查輸出，若發現錯誤或不合理，就重試或修正。這可由單一 Agent 完成，也能交給專門的 Evaluator Agent。
- **ReAct Pattern（Reason + Act）**  
	在推理與行動之間來回：思考 → 執行 → 觀察 → 再思考，直到找到可行的解法。這特別適合動態、不確定的情境。
- **Multi-Agent Pattern**  
	由多個專責 Agent 分工協作：有人負責規劃（Planner）、有人掌控預算（Budget）、有人檢查飲食限制（Diet）、有人做整體評估（Evaluator）。最終結果由協調者整合，通常比單一 Agent 更完整、更可靠。

---

## Demo：LLM（被動） vs Agentic AI（主動）

以下用同一個任務，分別在 **關閉工具** （單純 LLM）與 **啟用工具／思考模式** （Agentic 傾向）兩種設定下進行，直觀比較差異。

**任務 Prompt（相同）**

> 幫我規劃三天維也納行程，限制：不吃牛肉、偏好藝術與咖啡館、預算 300 歐；請考慮下週天氣。

---

### 設定 A：僅 LLM（關閉瀏覽／工具）

- 直接輸出一份三天行程清單：列出熱門景點、幾家咖啡館，提供簡要動線。
- **不會** 查即時天氣與票價、也 **不會** 驗證餐廳是否符合飲食限制，預算常為估計值。

![LLM 靜態輸出（關閉思考與工具）](https://ithelp.ithome.com.tw/upload/images/20250916/20165544mWOw5QbeFt.png)  
*圖：單純 LLM 的一次性回覆，提供概括建議但缺少即時資料與驗證。*

---

### 設定 B：Agentic（啟用工具／思考模式）

- 系統會先 **查天氣** 與 **票價/營業資訊** ，再拆解任務規劃行程。
- 依天氣把戶外/室內活動做 **動態調整** ，並 **檢查預算與飲食偏好** （不吃牛肉）。
- 產出後進行 **自我檢查** （Reflection），必要時 **重排** 並附上備案。

![Agentic 規劃輸出（啟用工具 / 思考模式）](https://ithelp.ithome.com.tw/upload/images/20250916/20165544MPawwBm6my.png)  
*圖：啟用工具後，系統會查天氣與票價並動態規劃，輸出可執行且含備案的行程。*

---

### 觀察重點

- **資料來源** ：A 只靠內部知識；B 會主動查詢官方/即時資料。
- **任務推進** ：A 停在「回答」；B 能持續「行動」到達成目標（查→規劃→驗證→修正）。
- **使用者條件** ：B 會把「不吃牛肉」「預算 300 歐」等條件落到餐廳與門票選擇上。
- **穩健性** ：B 具備備案與自檢，遇到天氣變動或預算超標時能自動調整。

> 你也可以用同一個 Prompt 在 ChatGPT / Gemini 做比較： **一次關閉瀏覽/工具** 、 **一次啟用瀏覽/工具** 。會很直觀地看到從「靜態建議」到「主動規劃與驗證」的差異。

---

## 小結

- **LLM 很有用，但偏向知識與生成；Agentic AI 更像一個會「做事」的隊友。**
- 要讓 AI 主動起來，需要一系列核心能力：自主、感知、反應、規劃、工具使用、記憶、反思、學習與協作。
- 這些能力透過 **設計模式（Design Patterns）** 組合落地，例如 Tool Use、Planning、Reflection、ReAct、Multi-Agent。
- 善用設計模式，就能從單次問答的體驗，進一步打造一個能「持續推進」的 Agentic 系統。

---

![維也納美景宮（Belvedere Palace）上花園](https://ithelp.ithome.com.tw/upload/images/20250916/20165544kGKVZv57ut.jpg)  
*圖：從維也納美景宮（Belvedere Palace）上花園遠眺市區，能看見史蒂芬大教堂與市中心天際線。正如 Agentic AI 的角色，不只專注於局部任務，更能拉高視野，協調各方，推動整體目標前進。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10379981)

[

Day 1｜啟程！踏上 Agentic AI 的探索之旅

](https://ithelp.ithome.com.tw/articles/10379981)[下一篇](https://ithelp.ithome.com.tw/articles/10382280)

[

Day 3｜為什麼是 LLM？從強化學習到語言模型的轉折

](https://ithelp.ithome.com.tw/articles/10382280)

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