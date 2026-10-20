---
title: "Day 28｜讓 AI 用流程思考：n8n 讓 Agent 以工作流驅動協作 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10396549"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言 在 昨天 Day 27，我們讓旅遊助理學會在關鍵決策前「停下腳步」。透過 Human-in-the-Loop（HITL），AI 能在行動前等待人類確認與補充。 但當我們的決策已確定、任務明確之後..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 28

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 28 篇

## Day 28｜讓 AI 用流程思考：n8n 讓 Agent 以工作流驅動協作

- 分享至

## 前言

在 **[昨天 Day 27](https://ithelp.ithome.com.tw/articles/10396238)** ，  
我們讓旅遊助理學會在關鍵決策前「停下腳步」。  
透過 **Human-in-the-Loop（HITL）** ，  
AI 能在行動前等待人類確認與補充。

但當我們的決策已確定、任務明確之後，  
下一步自然是——

> 「讓 AI 真正動起來。」

過去幾天，我們透過 LangChain 建立了一套多 Agent 協作架構。  
然而，在某些應用場景中，我們不一定需要完整的程式框架，  
而是希望能快速整合多步驟任務、清楚觀察執行過程、  
並直接串接外部服務。

這樣的情境中， **n8n** 正好能發揮它的長處。

---

## 為什麼用 n8n 來建立 AI 工作流？

**[n8n](https://n8n.io/)** 是一個開源的可視化自動化平台，  
能以「節點（Node）」的方式組合任務，  
支援數百種常見服務（如 Gmail、Google Calendar、Slack、Notion、OpenAI、Gemini 等）。

在 AI 系統設計裡，  
n8n 扮演的是 **流程協作與執行層** 的角色，  
而 LangChain 則專注於 **語意推理與 Agent 管理** 。

這兩者的定位並非取代關係，而是互補：

- 當任務流程明確，例如「接收請求 → 生成內容 → 寄出結果」，  
	可以用 n8n 直覺地完成。
- 當任務邏輯複雜，需要多輪推理或上下文記憶時，  
	LangChain 則更具彈性。

今天，我們就用 n8n 來重現「維也納旅遊助理」的多 Agent 架構，  
並新增最後一步—— **自動寄出 Gmail 行程建議** 。

---

## 架構設計：從多 Agent 到多節點

在前幾篇中，我們運用 **[Day 24](https://ithelp.ithome.com.tw/articles/10395086)** 的「旅遊助理」作為基底，以 LangChain 實作 `Supervisor` 統籌多位 Agent。  
今天，我們以 n8n 節點的方式呈現相同概念：

每一個節點（Node）代表一個 Agent：

- Supervisor Agent 負責分派任務與彙整結果；
- 各子 Agent（Weather、Planner、Advisor）各司其職；
- Gmail Node 則是最終輸出端。

---

## Demo：以 n8n Workflow 實現旅遊助理 Multi-Agent 協作

在 n8n 中，我們以「 **Supervisor Agent** 」作為總控節點，  
負責接收使用者輸入並依內容決定要呼叫哪個子 Agent。

（以下運用到的 Agent 與相關 MCP 工具皆基於 [Day 24 Demo](https://ithelp.ithome.com.tw/articles/10395086) 的實作）

### 安裝 n8n

1. 使用 Docker，執行以下指令設定時區並啟動 n8n Container。詳細請參考 **[官方文件](https://docs.n8n.io/hosting/installation/docker/)**
```bash
docker volume create n8n_data

docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -e GENERIC_TIMEZONE="Asia/Taipei" \
 -e TZ="Asia/Taipei" \
 -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true \
 -e N8N_RUNNERS_ENABLED=true \
 -v n8n_data:/home/node/.n8n \
 docker.n8n.io/n8nio/n8n
```
2. 安裝完啟動後，進入 [http://localhost:5678](http://localhost:5678/)
3. 使用 email 註冊並完成驗證  
	![使用 email 註冊並完成驗證](https://ithelp.ithome.com.tw/upload/images/20251012/20165544CNh09k49gT.png)

---

### Supervisor Agent：任務協調中心

**System Message**

```markdown
你是旅行統籌助理。根據使用者需求，自行決定何時呼叫氣象專家 Weather Agent、旅遊規劃師 Planner Agent 與顧問 Advisor Agent。
最後整合所有子 Agent 結果，輸出完整建議。
輸出行程請整理成易讀的 Html 格式。
並將行程透過 Gmail 寄出。
```

![n8n Supervisor Agent Workflow](https://ithelp.ithome.com.tw/upload/images/20251012/20165544XX91vEJx2s.png)  
*圖：Supervisor Agent 統籌整個流程，依照使用者輸入分別呼叫 Weather、Planner、Advisor 三個 Agent，最後整合結果並透過 Gmail 寄出信件。*

在這個工作流中：

- **When Chat Message Received** ：接收使用者請求（例如「幫我規劃維也納一日遊」）；
- **Supervisor Agent** ：使用 `gemini-2.5-flash` 模型判斷需求並分派任務；
- **Call Weather / Planner / Advisor Agent** ：透過 `Call n8n Workflow Tool` 呼叫對應的子工作流；
- **Send a Message in Gmail** ：彙整最終結果並寄出郵件。

---

### 子 Agent：模組化任務單元

#### 1\. Weather Agent（天氣專家）

**System Message**

```markdown
你是氣象專家，負責分析目的地天氣狀況與對行程的影響。
```

**Tool**  
Weather MCP Client (查詢天氣)

![n8n Weather Agent Workflow](https://ithelp.ithome.com.tw/upload/images/20251012/20165544Of9Pi8jcL2.png)  
*圖：Weather Agent 使用 Gemini 模型與天氣 Weather MCP Client 取得天氣資訊，作為行程規劃參考。*

以 **When Executed by Another Workflow** 為觸發條件，  
由 Supervisor Agent 呼叫並回傳天氣查詢結果。

---

#### 2\. Planner Agent（行程規劃師）

**System Message**

```markdown
你是旅遊規劃師，根據天氣與景點資料設計一日行程。
```

**Tool**  
Attractions MCP Client (查詢景點)

![n8n Planner Agent Workflow](https://ithelp.ithome.com.tw/upload/images/20251012/20165544oy6SZJZCK1.png)  
*圖：Planner Agent 使用 Gemini 模型與景點 Attractions MCP Client 生成行程規劃。*

它是整個系統的核心 Agent，  
根據地點與天氣整合出完整的行程路線。

---

#### 3\. Advisor Agent（旅遊顧問）

**System Message**

```markdown
你是旅遊顧問，負責整合天氣與行程結果，提出最終建議。
```

![n8n Advisor Agent Workflow](https://ithelp.ithome.com.tw/upload/images/20251012/20165544UD6Wjlygt6.png)  
*圖：Advisor Agent 作為旅遊顧問，整合 Weather 與 Planner 的輸出，提出最終建議與提醒。*

Advisor Agent 負責扮演「旅遊顧問」角色，  
會接收來自天氣與行程兩個 Agent 的結果，  
整合後提供更貼近使用者需求的最終建議——  
例如穿著建議、最佳出發時間、交通安排或當地注意事項。

---

### Workflow 運作過程

使用者輸入：

> 「幫我安排維也納一日遊，想包含音樂廳與當地餐廳」

1. **Supervisor Agent** 接收請求並分派任務；
2. **Weather Agent** 查詢天氣；
3. **Planner Agent** 根據氣候與地點生成行程；
4. **Advisor Agent** 綜合天氣與行程資訊，提出整體建議；
5. 所有輸出合併後由 **Gmail Node** 寄出。

整個過程中，n8n 即時顯示每個節點的狀態與輸出，  
讓協作流程透明可觀察。

---

### 執行結果：流程成功、紀錄清晰

![n8n Workflow Execution Result](https://ithelp.ithome.com.tw/upload/images/20251012/20165544ktTIEnMkju.png)  
*圖：完整的任務執行畫面。左側為行程內容，右側 Logs 區清楚顯示每個 Agent 的執行歷程與順序。*

整個工作流順利完成——  
Supervisor Agent 成功協調三個子 Agent，  
自動生成一份考慮當日氣候的維也納行程，  
並以 **HTML 格式** 透過 **Gmail 節點** 寄出。

右側的 **Logs** 清楚記錄整個歷程：

- 每個 Agent 的呼叫次序與狀態；
- 使用的模型與 Token 消耗；
- 執行耗時與回應內容。

這樣的設計，讓 AI 協作流程不再是黑箱，  
而是能被追蹤、調整、解釋的「透明決策過程」。

![寄出 Gmail 的最終結果](https://ithelp.ithome.com.tw/upload/images/20251012/20165544rFo5IyvILm.png)  
*圖：AI 自動寄出的 Gmail 郵件內容，以 HTML 格式排版，整合天氣資訊與完整行程建議。*

當整個工作流執行完成後，  
**Gmail Node** 會將整合後的旅遊建議以 **HTML 格式** 自動寄出。  
郵件主旨會包含天氣摘要與主要景點，  
內文則分為「今日天氣概況」、「上午行程」、「中午餐飲建議」等段落，  
讓內容閱讀起來就像專屬旅遊顧問親手撰寫。

這一步不只是「完成任務」，  
更象徵著從 AI 規劃 → 人機協作 → 自動化執行 → 對外溝通 的完整循環。  
AI 不只是思考，也能 **確實執行，並以人類習慣的形式交付結果** 。

---

## 可視化 AI 工作流的價值

透過這樣的 n8n 架構，我們得到三項重要特性：

1. **模組化（Modularity）**  
	每個 Agent 皆為獨立工作流，可獨立測試與維護。
2. **可觀察性（Observability）**  
	所有節點執行紀錄可視化呈現，  
	清楚顯示資料如何在各 Agent 間傳遞。
3. **可擴充性（Extensibility）**  
	若要加入新 Agent（例如 Booking Agent 或 Translator Agent），  
	只需新增一個 workflow 並連接即可。

---

## 小結

今天，我們讓多 Agent 系統從程式邏輯走入可視化工作流。  
透過 **n8n** ，AI 的決策與行動不再隱藏在程式碼中，  
而以節點與線條的方式具體呈現。

這樣的架構讓 AI 的「思考」與「執行」都能被觀察與管理：

- 每個 Agent 的角色清晰；
- 任務流程一目了然；
- 所有結果皆可追蹤與回溯。

AI 不只是思考的存在，  
而是能透過流程—— **真正行動的智慧隊友。**

---

![美泉宮](https://ithelp.ithome.com.tw/upload/images/20251012/20165544104ZL28LNG.jpg)  
*圖：維也納美泉宮（Schönbrunn Palace）。巴洛克式的宮殿與對稱花園，是哈布斯堡王朝榮光的象徵。從中央軸線向遠方延伸的構圖，如同 n8n 的可視化工作流——每一條路徑都精準對應，每一個節點都自成秩序。AI 系統在這樣的結構中運作，不僅展現智慧的層次，也讓人類的意圖在規劃與行動之間自然銜接。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10396238)

[

Day 27｜讓 AI 暫停思考：Human-in-the-Loop 讓 Agent 與人類成為協作夥伴

](https://ithelp.ithome.com.tw/articles/10396238)[下一篇](https://ithelp.ithome.com.tw/articles/10396972)

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