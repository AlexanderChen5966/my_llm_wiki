---
title: "Day 24｜從學習到協作：Supervisor × Multi-Agent 讓 Agent 組成智慧團隊 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10395086"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言 在 昨天 Day 23，我們讓單一 Agent 學會了「反思」與「記憶」——它能在任務結束後回顧、修正、並保留經驗。 但在真實應用中，任務往往不止一個面向。就像規劃旅行，不只是要考慮天氣，還需要..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 24

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 24 篇

## Day 24｜從學習到協作：Supervisor × Multi-Agent 讓 Agent 組成智慧團隊

- 分享至

## 前言

在 **[昨天 Day 23](https://ithelp.ithome.com.tw/articles/10394706)** ，  
我們讓單一 Agent 學會了「反思」與「記憶」——  
它能在任務結束後回顧、修正、並保留經驗。

但在真實應用中，任務往往不止一個面向。  
就像規劃旅行，不只是要考慮天氣，還需要同時處理交通、景點、預算、飲食偏好等資訊。  
若交給單一 Agent 負責所有任務，常會出現：

- 工具太多導致選擇困難；
- 記憶上下文過於龐大；
- 缺乏領域專精與任務分工。

為了解決這些問題，就需要 **分工合作** 。

其實在 **[基礎篇 Day 11](https://ithelp.ithome.com.tw/articles/10387823)** ，  
我們曾介紹過最基本的 Multi-Agent 與 **Supervisor（統籌型）** 模式：  
由中央的主控 Agent 負責協調多位專家 Agent 各司其職，  
如同指揮與樂手，共同完成複雜任務。

今天，我們將延續維也納旅遊助理的情境，  
透過 **LangChain v1.0** 來實現 **Multi-Agent 架構** ，  
打造一個能「協作」的系統：  
由一位 **統籌 Supervisor** 協調三位專家 Agent，  
共同完成任務決策。

---

## 為什麼需要 Multi-Agent？

即使具備記憶與反思，單一 Agent 仍容易陷入「多工超負荷」的狀況：  
它要同時推理、查詢、分析與規劃，結果效率下降、品質不穩。

Multi-Agent 的關鍵理念是：

> **讓每個 Agent 只專注一件事，並透過協作完成複雜任務。**

| 問題情境 | 單一 Agent 的限制 | Multi-Agent 解法 |
| --- | --- | --- |
| 工具過多、選擇混亂 | 無法決定何時使用何種工具 | 由 Supervisor 控制呼叫流程 |
| 記憶過大 | 所有資訊堆疊在同一模型中 | 每位 Agent 僅保留必要上下文 |
| 缺乏專業化 | 同一模型需同時處理不同領域 | 以角色分工（氣象 / 規劃 / 顧問） |

---

## Multi-Agent 架構概念

LangChain v1.0 將 Multi-Agent 的實作歸納為兩種主要模式（ [官方文件說明](https://docs.langchain.com/oss/python/langchain/multi-agent) ）：

1. **Tool Calling Pattern** ：由中央控制的 **Supervisor** 呼叫其他 Agent 當作工具使用。
2. **Handoffs Pattern** ：各 Agent 可自行「轉交控制權」，進行對話式交接。

今天我們聚焦第一種—— **Supervisor × Tool Calling** ，  
透過一個多角色協作的 **維也納旅遊助理** ，展示如何讓多位 Agent 分工合作，產出整合性決策。

## 系統架構

今天採用 **Tool Calling Pattern** ，我們將系統分為四個角色，  
由 **Supervisor（統籌 Agent）** 控制整個任務流程，  
依需求呼叫三位專家：

1. **WeatherAgent** —— 查詢並解讀天氣資訊
2. **PlannerAgent** —— 根據天氣與景點設計行程
3. **AdvisorAgent** —— 整合各方結果並提出建議

  
*圖：Supervisor 採用集中式協作模式，負責統籌三位專家 Agent。每位子 Agent 具備不同任務與工具，最終由 Supervisor 整合所有回應形成完整決策。*

---

## Demo：Supervisor × Specialist 協作完成維也納旅遊規劃

接下來的程式示範，  
展示如何在 LangChain v1.0 中，以 Supervisor 控制多位專家 Agent 的協作。

```python
import asyncio
from langchain.chat_models import init_chat_model
from langchain.agents import create_agent
from langchain.tools import tool
from langgraph.checkpoint.memory import InMemorySaver
from langchain_mcp_adapters.client import MultiServerMCPClient
import os

# 初始化 MCP 工具
async def init_tools():
    accuweather_api_key = os.getenv("ACCUWEATHER_API_KEY")
    if not accuweather_api_key:
        raise ValueError("請設定 ACCUWEATHER_API_KEY 環境變數")
    
    client = MultiServerMCPClient(
        {
            "accuweather": {
                "transport": "stdio",
                "command": "uv",
                "args": [
                    "--directory",
                    "{MCP 程式路徑}",
                    "run",
                    "weather_mcp_server.py"
                ],
                "env": {"ACCUWEATHER_API_KEY": accuweather_api_key}
            },
            "attractions": {
                "transport": "stdio",
                "command": "uv",
                "args": [
                    "--directory",
                    "{MCP 程式路徑}",
                    "run",
                    "attractions_mcp_server.py"
                ]
            }
        }
    )
    return await client.get_tools()

# 建立三位專家 Agent
async def build_subagents(tools):
    model = init_chat_model("gemini-2.5-flash", model_provider="google_genai")
    checkpointer = InMemorySaver()

    weather_agent = create_agent(
        model=model,
        tools=[t for t in tools if "weather" in t.name.lower()],
        system_prompt="你是氣象專家，負責分析目的地天氣狀況與對行程的影響。",
        checkpointer=checkpointer,
    )

    planner_agent = create_agent(
        model=model,
        tools=[t for t in tools if "attractions" in t.name.lower()],
        system_prompt="你是旅遊規劃師，根據天氣與景點資料設計一日行程。",
        checkpointer=checkpointer,
    )

    advisor_agent = create_agent(
        model=model,
        tools=[],
        system_prompt="你是旅遊顧問，負責整合天氣與行程結果，提出最終建議。",
        checkpointer=checkpointer,
    )

    return weather_agent, planner_agent, advisor_agent

# Supervisor 呼叫子 Agent
def make_supervisor(weather_agent, planner_agent, advisor_agent):
    @tool("WeatherAgent", description="查詢目的地天氣並分析其對行程的影響")
    async def call_weather(query: str):
        result = await weather_agent.ainvoke(
            {"messages": [{"role": "user", "content": query}]},
            {"configurable": {"thread_id": "1"}}
        )
        print("\n[WeatherAgent] 輸出：")
        print(result["messages"][-1].content)
        return result["messages"][-1].content

    @tool("PlannerAgent", description="規劃維也納一日行程，考慮天氣與景點")
    async def call_planner(query: str):
        result = await planner_agent.ainvoke(
            {"messages": [{"role": "user", "content": query}]},
            {"configurable": {"thread_id": "1"}}
        )
        print("\n[PlannerAgent] 輸出：")
        print(result["messages"][-1].content)
        return result["messages"][-1].content

    @tool("AdvisorAgent", description="整合各 Agent 輸出，提供最終建議")
    async def call_advisor(query: str):
        result = await advisor_agent.ainvoke(
            {"messages": [{"role": "user", "content": query}]},
            {"configurable": {"thread_id": "1"}}
        )
        print("\n[AdvisorAgent] 輸出：")
        print(result["messages"][-1].content)
        return result["messages"][-1].content

    model = init_chat_model("gemini-2.5-flash", model_provider="google_genai")
    supervisor = create_agent(
        model=model,
        tools=[call_weather, call_planner, call_advisor],
        system_prompt=(
            "你是旅行統籌助理。根據使用者需求，自行決定何時呼叫氣象專家、旅遊規劃師與顧問。"
            "最後整合所有子 Agent 結果，輸出完整建議。"
        ),
        checkpointer=InMemorySaver(),
    )
    return supervisor

# 主程式
async def main():
    tools = await init_tools()
    weather_agent, planner_agent, advisor_agent = await build_subagents(tools)
    supervisor = make_supervisor(weather_agent, planner_agent, advisor_agent)

    result = await supervisor.ainvoke(
        {
            "messages": [{
                "role": "user",
                "content": "幫我規劃維也納一日遊，請考慮天氣與適合的行程安排。"
            }]
        },
        {"configurable": {"thread_id": "1"}}
    )

    print("\n=== 最終整合輸出 ===")
    print(result["messages"][-1].content)

if __name__ == "__main__":
    asyncio.run(main())
```

---

## 執行結果

在這個範例中，我們可以清楚觀察到 **Supervisor × Multi-Agent 協作流程** 的實際運作方式。  
整個系統的決策分為三個層次：

1. **分工階段** ：各專家 Agent 執行任務。
2. **統籌階段** ：Supervisor 收集並整合資訊。
3. **決策階段** ：生成最終的行程建議。

---

首先，三位專家 Agent 各自根據職能完成任務：  
**WeatherAgent** 負責解析天氣狀況並提供氣候建議；  
**PlannerAgent** 根據天氣與景點資料規劃路線；  
**AdvisorAgent** 則整合前兩者輸出，產出具體建議。

Supervisor 並不直接行動，而是像導演般掌握流程，  
自動決定應先諮詢哪位專家、何時串聯結果，  
讓整個決策過程更有條理與透明度。

![https://ithelp.ithome.com.tw/upload/images/20251008/20165544YLUFNvw3oi.png](https://ithelp.ithome.com.tw/upload/images/20251008/20165544YLUFNvw3oi.png)  
*圖：三位專家 Agent 並行協作的執行過程。Supervisor 根據使用者請求動態呼叫 WeatherAgent、PlannerAgent 與 AdvisorAgent，形成分工明確的多角色決策流程。*

接著，Supervisor 將三位專家的回覆整合後，  
生成一份完整且具彈性的「維也納一日遊行程建議」。  
內容不僅包含天氣分析與景點順序，  
還提出替代方案與行程優化原則——  
顯示模型具備「理解 → 分析 → 整合 → 建議」的多層推理能力。

![https://ithelp.ithome.com.tw/upload/images/20251008/20165544CSVBf8p45O.png](https://ithelp.ithome.com.tw/upload/images/20251008/20165544CSVBf8p45O.png)  
*圖：Supervisor 整合各專家輸出後生成最終行程建議。模型綜合天氣、開放時間與景點資訊，輸出具備彈性與可操作性的完整決策結果。*

透過這樣的分工協作機制，  
整個系統就像一支具備「氣象顧問 × 行程設計師 × 旅遊顧問」的智慧團隊：  
專家各司其職，Supervisor 負責決策統籌，  
最終輸出更合理、更具上下文一致性的結果。

---

## 從「學習」到「協作」

回顧前幾天的進化脈絡：

Reflection 讓 Agent 學會「自我檢討」，  
Memory 讓它「記得經驗」，  
而 Multi-Agent 則讓它「學會與他人協作」。

這是智慧體從「單一思考者」進化為「協作系統」的關鍵一步。

---

## 小結

今天，我們讓多位 Agent 學會了「合作」。

透過 **Supervisor × Multi-Agent 架構** ，  
模型不再單打獨鬥，而是能：

- **分工協作** ：每個 Agent 聚焦專業任務，降低錯誤與資訊負擔；
- **集中整合** ：Supervisor 統籌決策，確保輸出一致與完整；
- **結構擴充** ：可彈性新增其他領域專家（交通、美食、預算分析師等），  
	形成可成長的智慧團隊。

這不僅是技術架構的升級，更是 Agent 思維模式的轉變：  
從「單一思考的 AI」邁向「協作運作的團隊式 AI」，  
讓模型不只是能思考、能學習，更能 **協同工作、共同決策** 。

---

![維也納金色大廳](https://ithelp.ithome.com.tw/upload/images/20251008/20165544wHFrkZH6Oo.jpg)  
*圖：維也納金色大廳（Großer Musikvereinssaal）。金碧輝煌的天花與懸掛的水晶吊燈，構成協奏的視覺節奏。每一場演出都由不同樂手在同一份樂譜下協力完成，如同 Multi-Agent 架構中各個專家協作於同一任務，由 Supervisor 統籌指揮，最終奏出和諧一致的智慧交響。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10394706)

[

Day 23｜從推理到學習：Reflection × Memory 讓 Agent 從經驗中進化

](https://ithelp.ithome.com.tw/articles/10394706)[下一篇](https://ithelp.ithome.com.tw/articles/10395412)

[

Day 25｜從文字到感知：多模態 Multimodal 讓 Agent 看懂世界

](https://ithelp.ithome.com.tw/articles/10395412)

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