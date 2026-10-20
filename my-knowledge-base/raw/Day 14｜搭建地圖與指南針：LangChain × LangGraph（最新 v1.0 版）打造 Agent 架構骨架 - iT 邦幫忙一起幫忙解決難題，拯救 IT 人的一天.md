---
title: "Day 14｜搭建地圖與指南針：LangChain × LangGraph（最新 v1.0 版）打造 Agent 架構骨架 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10389376"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言 經過前 13 天，我們一步步讓 LLM 從單純回話工具，逐漸具備了 Agentic 的核心能力。在「旅途中繼站」回顧後，我們要開始進入實戰篇：不只是 Demo，而是要用 架構化方式，把能力組織起..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 14

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 14 篇

## Day 14｜搭建地圖與指南針：LangChain × LangGraph（最新 v1.0 版）打造 Agent 架構骨架

- 分享至

## 前言

經過前 13 天，我們一步步讓 LLM 從單純回話工具，逐漸具備了 **Agentic 的核心能力** 。  
在「旅途中繼站」回顧後，我們要開始進入實戰篇：不只是 Demo，而是要用 **架構化方式** ，把能力組織起來。

值得注意的是， **LangChain 官方已公告在今年 10 月全面切換到 v1.0 版本** ，舊版 API 將逐步淘汰。  
因此，這裡我們直接研究 **最新 v1.0 的設計** ，讓未來的實作與維護更穩健。

如果說前半程像是「徒步旅行」，今天開始，我們要拿出 **地圖（架構）** 與 **指南針（控制流）** ，讓旅程更長遠、更可靠。這就是 **LangChain × LangGraph** 要帶來的力量。

---

## LangChain：工具箱與積木

[LangChain v1.0](https://docs.langchain.com/oss/python/langchain/overview) 把介面大幅簡化，核心功能聚焦在六大模組：

### 核心功能

- **Agents** ：代理人，能判斷是否呼叫工具，並整合輸出。
- **Models** ：核心模型（文字、多模態、推理），支援 `invoke / stream / batch` 。
- **Messages** ：標準訊息格式（含 `.content_blocks` ），讓輸入輸出一致。
- **Tools** ：封裝外部功能（API、函式），可由模型透過 tool calling 呼叫。
- **Short-term memory** ：保存上下文。
- **Streaming** ：即時輸出（chunk 與事件流）。

  
*圖：LangChain v1.0 的積木式架構，Agent 作為核心，結合模型、工具、記憶與串流，處理輸入並輸出結果*

---

### v1.0 的有感改變

今年 10 月要全面上線的 **LangChain v1.0** ，其實帶來了幾個關鍵轉變，對開發者來說「用起來真的有感」。

1. **Agent 抽象更統一**
	- 過去有 ReAct、Plan-and-Solve、ReAct-Retry… 等多種 agent 寫法，新手常常不知道該挑哪一種。
		- 在 v1.0，這些主流做法都整合到一個更成熟的 **Agent 抽象** ，使用上更直覺。
		- 如果是複雜或非典型的情境（例如多分支、多代理人），才建議用更底層、可彈性組裝的 **LangGraph** 。
2. **輸出格式更標準**
	- 各家 LLM API 回傳格式差異很大，以前要自己處理 parsing，非常麻煩。
		- v1.0 引入了統一的 **`content_blocks`** 屬性，把推理過程、工具呼叫、引用來源，甚至多媒體內容，都放進相同結構裡。
		- 好處是：你不需要再寫一堆相容層，直接用同一套 API 解析即可。

簡單來說：

- **Agent 更簡潔統一** ，少了選擇困難。
- **輸出更有一致性** ，少了 parsing 地獄。

這些改動，讓 v1.0 不只是「改版號」，而是真正替開發者省下學習與維護成本。

---

### 最小 Demo：Gemini + Tool Calling Agent

這邊我們拿 [核心篇 Day 6 (Tool Use)](https://ithelp.ithome.com.tw/articles/10384461) 「查今天維也納的天氣」的例子運用 **LangChain** 來重新實作

**1\. 安裝最新 `v1.0` 的 `LangChain` 以及 `Gemini` model 套件**

```shell
pip install -q --pre -U langchain
pip install -q -U langchain-google-genai
```

**2\. 用 `LangChain v1.0` 實作「查今天維也納的天氣」，簡單幾行程式碼就能實現**

```python
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain.agents import create_agent
from langchain_core.tools import tool

# 1. 定義工具（用 @tool 包裝）
@tool
def get_weather(city: str) -> str:
    """查詢指定城市的天氣狀況。"""
    weather_data = {
        "維也納": "大晴天，氣溫 22°C",
        "台北": "多雲，氣溫 30°C",
    }
    return weather_data.get(city, "查無資料")

# 2. 初始化 Gemini 模型
llm = ChatGoogleGenerativeAI(
    model="gemini-2.5-flash",
    temperature=0,
)

# 3. 建立 Agent（支援 tool calling）
agent = create_agent(
    model=llm,
    tools=[get_weather],
)

# 4. 使用者查詢
query = "請告訴我今天維也納的天氣"

# 5. 由 Agent 決定是否呼叫工具
result = agent.invoke({"messages": [{"role": "user", "content": query}]})

# 6. 輸出最終回覆
print(result["messages"][-1].content)
```

**3\. 執行結果**  
![查今天維也納的天氣（LangChain v1.0）](https://ithelp.ithome.com.tw/upload/images/20250929/20165544gkhufH7IcG.png)

這個範例展示了最小的 **LangChain v1.0 Agent** ：使用者輸入經過模型判斷，若需要，會自動呼叫工具，最後整合輸出。

---

## LangGraph：狀態機與有向圖

光有積木還不夠，我們需要一張「路線圖」，能處理分支、迴圈、決策。這就是 **LangGraph** 。

### 核心功能

- **StateGraph** ：以狀態機（state machine）方式構建流程。
- **Node** ：節點，執行一段邏輯（模型推理、工具呼叫…）。
- **Edge** ：節點之間的路徑，靜態或條件式。
- **State** ：共享上下文，保存輸入與輸出。
- **Compile → App** ：編譯成可執行應用，可 `invoke()` 或 `stream()` 。

![LangGraph 的狀態機架構](https://ithelp.ithome.com.tw/upload/images/20250928/20165544f0MRvR1w6M.png)  
*圖：LangGraph 的狀態機流程圖，以 Router 決策分流，經過 Reason 或 Tool 節點，再由 Observe 節點整合輸出*

---

### Demo：最小 StateGraph

```shell
pip install --pre -U langgraph
```
```python
from langchain_google_genai import ChatGoogleGenerativeAI
from langgraph.graph import StateGraph, END
from typing import TypedDict

# 初始化 Gemini
llm = ChatGoogleGenerativeAI(model="gemini-2.5-flash", temperature=0)

# 定義狀態
class State(TypedDict):
    message: str

# 節點
def start_node(state: State):
    resp = llm.invoke(f"Say hello to {state['message']}")
    return {"message": resp.content}

def end_node(state: State):
    print("LangGraph Output:", state["message"])
    return state

# 建立圖
graph = StateGraph(State)
graph.add_node("start", start_node)
graph.add_node("end", end_node)

graph.set_entry_point("start")
graph.add_edge("start", "end")

app = graph.compile()
app.invoke({"message": "LangGraph"})
```

這個範例展示了最小的 **StateGraph** ，只有兩個節點（start → end）。雖然只是打招呼，但能清楚看到 **狀態流動與節點邏輯** 。

---

## LangChain × LangGraph 的互補關係

[官方 (v1.0)](https://docs.langchain.com/oss/python/langgraph/overview) 建議：

> **If you are just getting started with agents, or want a higher level abstraction, it is recommended that you use LangChain’s agents.**

也就是說：

- **LangChain Agents** ：已足夠應付大多數情境（tool calling、記憶、streaming、簡單分支）。
- **LangGraph** ：專注於 **底層 orchestration** （狀態機、長時間任務、人類介入、debug 與視覺化），適合更複雜或生產級應用。

---

### 什麼情境用 LangChain？什麼情境需要 LangGraph？

如果你只是要開發一般的 AI 助理、聊天機器人，或是做簡單的工具串接， **LangChain v1.0 的 Agents 就已經足夠** 。它幫你處理了 Prompt、模型呼叫、工具調用、短期記憶和串流輸出，大部分應用都能直接搞定。

但如果遇到以下情境，就會建議引入 **LangGraph** ：

- **流程需要清楚顯示分支或條件判斷** （像狀態機）。
- **任務可能要跑很久，甚至能恢復執行** （例如長時間的任務）。
- **需要人類在中途介入** ，檢查或修改 Agent 的狀態。
- **要做多代理人協作** ，多個 Agent 之間要互相傳遞訊息。
- **在生產環境中** ，需要可視化、可監控、可 debug 的工作流。

換句話說：

- **LangChain** = 像樂高積木，適合快速組合出常見功能。
- **LangGraph** = 像設計藍圖，適合需要流程透明、耐久執行的大專案。

| 功能需求 | LangChain Agents（就足夠） | LangGraph（必要時才用） |
| --- | --- | --- |
| 單輪對話 | ✅ | — |
| 基本 tool calling | ✅ | — |
| 短期記憶（上下文） | ✅ | — |
| Streaming 即時回覆 | ✅ | — |
| 多步驟推理（簡單 if/else） | ✅ | — |
| 複雜分支 / 條件流 | ⚠️ 難維護 | ✅ |
| 長時間執行 / 可恢復任務 | — | ✅ |
| Human-in-the-loop | — | ✅ |
| Debug / 視覺化執行路徑 | — | ✅ |
| Multi-agent 協作 | — | ✅ |
| 生產級部署（容錯、監控） | — | ✅ |

---

## 小結

今天，我們從「積木」到「地圖」，認識了 **LangChain v1.0 的核心組件** ，也看到了 **LangGraph 的狀態機思維** 。

- 由於今年 10 月起，LangChain 將全面切換到 **v1.0** ，因此我們的實作全部以新版 API 為基準。
- 對於大部分應用： **LangChain Agents 就足夠** 。
- 若要更高可控性（長時間任務、Human-in-the-loop、多代理人），才需要 **LangGraph** 。

接下來，我們將在這個基礎上，逐步把 **CoT、RAG、Reflection、Memory** 等能力工程化落地。

---

![維也納中央車站](https://ithelp.ithome.com.tw/upload/images/20250929/20165544gYuTSypG7z.jpg)  
*圖：維也納中央車站（Wien Hauptbahnhof）。現代感十足的玻璃立面與巨大的時鐘，象徵著跨越城市與國界的交通樞紐。旅人從這裡出發，選擇不同的路線，最終都能抵達目的地。這就像 LangChain 與 LangGraph —— 一者提供積木般的模組，另一者規劃清晰的流程，讓我們能自由組合、靈活轉換，開啟屬於自己的 AI 工程旅程。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10388740)

[

Day 13｜旅途中繼站：回顧 Agentic 覺醒足跡，邁向實戰新篇章

](https://ithelp.ithome.com.tw/articles/10388740)[下一篇](https://ithelp.ithome.com.tw/articles/10390982)

[

Day 15｜讓推理看得見：LangGraph 讓 CoT 化為可見的思路地圖

](https://ithelp.ithome.com.tw/articles/10390982)

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