---
title: "Day 22｜讓思考具象化：LangGraph 讓 ReAct × Planning 的決策流程一目了然 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10394337"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言 在 昨天 Day 21，我們讓 Agent 學會「規劃再行動」——先制定旅遊行程，再根據實際狀況（例如景點是否營業）靈活調整。這讓 Agent 從被動執行工具，進化成能夠「推理、規劃、再反應」的..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 22

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 22 篇

## Day 22｜讓思考具象化：LangGraph 讓 ReAct × Planning 的決策流程一目了然

- 分享至

## 前言

在 **[昨天 Day 21](https://ithelp.ithome.com.tw/articles/10393899)** ，我們讓 Agent 學會「規劃再行動」——  
先制定旅遊行程，再根據實際狀況（例如景點是否營業）靈活調整。  
這讓 Agent 從被動執行工具，進化成能夠「推理、規劃、再反應」的智慧助理。

但還有一個問題：

> 我們雖然知道它「做對了事」，卻看不到它 **怎麼想** 。

今天，我們要讓 Agent 的思考變得可見。  
透過 **LangGraph** ，將整個 **ReAct × Planning** 的推理過程「圖形化」，  
讓你能清楚看見模型的每一步決策邏輯、每次工具呼叫、與每個狀態轉換。

---

## 為什麼需要 LangGraph？

在多工具 ReAct 或 Planning Agent 中，模型往往會經歷多輪：

- Think（思考）
- Act（行動）
- Observe（觀察）

但這一切都發生在「黑箱」裡。  
你只能從最終答案，去猜模型在中間做了哪些推理。

LangGraph 的出現，正是為了打開這個黑箱。  
它能讓整個思考過程「節點化、狀態化、可視化」。

| 功能 | 說明 |
| --- | --- |
| **節點化推理** | 將每個步驟視為可組合節點（如 Plan → Check → Adjust） |
| **追蹤狀態變化** | 觀察每一步的輸入、輸出與決策分支 |
| **可視化流程** | 自動生成節點圖，顯示執行路徑 |
| **重現推理路徑** | 可回放與分析整個 Agent 的思考過程 |

---

## 範例：智慧旅遊助理的「可視化思考過程」

我們延續維也納旅遊助理的例子，  
這次讓 Agent 自行思考：

1. **Plan** — 規劃早、中、晚的行程
2. **CheckWeather** — 透過 Weather MCP 檢查天氣
3. **CheckOpen** — 查詢景點是否營業（用 MCP 工具）
4. **Adjust** — 若關閉則重新安排行程
5. **Summarize** — 統整最終結果

---

## LangGraph × LangChain × MCP 實作

```python
import asyncio
from langchain.chat_models import init_chat_model
from langchain.agents import create_agent
from langchain_mcp_adapters.client import MultiServerMCPClient
from langgraph.graph import StateGraph, END, MessagesState
import os

class State(MessagesState):
    plan: str = ""
    check: str = ""
    adjusted: str = ""
    output: str = ""

# Step 1: 初始化 MCP 工具
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
    tools = await client.get_tools()
    return tools

# Step 2: 建立 LangChain Agent
async def build_agent(tools):
    llm = init_chat_model("gemini-2.5-flash-lite", model_provider="google_genai")
    agent = create_agent(
        model=llm,
        tools=tools,
        system_prompt=(
            "你是一位智慧旅遊助理，根據天氣與景點資料，"
            "推薦最合適的維也納一日行程。必要時可查天氣、查景點或檢查營業狀況。"
        )
    )
    return agent

# Step 3: 定義 LangGraph 流程節點
async def main():
    tools = await init_tools()
    agent = await build_agent(tools)

    graph = StateGraph(State)

    async def step_plan(state):
        print("\n[PLAN] 模型規劃中...")
        result = await agent.ainvoke({"messages": [{"role": "user", "content": "請幫我規劃維也納今天的行程"}]})
        return {"plan": result["messages"][-1].content}

    async def step_check(state):
        print("\n[CHECK] 模型確認是否需查營業狀況...")
        result = await agent.ainvoke({"messages": [{"role": "user", "content": "請確認行程中的景點是否開放"}]})
        return {"check": result["messages"][-1].content}

    async def step_adjust(state):
        print("\n[ADJUST] 若有休館，重新規劃...")
        result = await agent.ainvoke({"messages": [{"role": "user", "content": "若有景點關閉，請重新安排替代行程"}]})
        return {"adjusted": result["messages"][-1].content}

    async def summarize(state):
        print("\n[SUMMARY] 最終結果整合...")
        result = await agent.ainvoke({"messages": [{"role": "user", "content": "請整理最終行程"}]})
        print("\nDone.")
        print(result["messages"][-1].content)
        return {"output": result["messages"][-1].content}

    # LangGraph 節點設計
    graph.add_node("plan", step_plan)
    graph.add_node("check", step_check)
    graph.add_node("adjust", step_adjust)
    graph.add_node("summary", summarize)

    graph.set_entry_point("plan")
    graph.add_edge("plan", "check")
    graph.add_edge("check", "adjust")
    graph.add_edge("adjust", "summary")
    graph.add_edge("summary", END)

    app = graph.compile()
    result = await app.ainvoke({})
    print("\n最終輸出：", result["output"])

if __name__ == "__main__":
    asyncio.run(main())
```

---

### 執行結果

```csharp
[PLAN] 模型規劃中...
→ Agent 呼叫 get_weather({"city": "維也納"})
→ 回傳：多雲 14°C
→ 呼叫 get_attractions({"city": "維也納"})
→ 回傳：美泉宮、皇宮、史蒂芬大教堂
→ 初步規劃完成。

[WEATHER] 查詢天氣中...
→ 決定優先推薦室內景點。

[CHECK] 檢查景點是否開放...
→ 呼叫 check_open({"place": "皇宮"}) → 開放
→ 呼叫 check_open({"place": "美泉宮"}) → 休館

[ADJUST] 若有關閉景點，請重新安排行程...
→ 呼叫 get_attractions({"city": "維也納"})
→ 改推薦 阿爾貝蒂娜博物館 替代美泉宮。

[SUMMARY] 統整最終行程...
維也納今日行程：
上午：皇宮（開放）
下午：阿爾貝蒂娜博物館（替代美泉宮）
晚上：音樂會
```

---

## 延伸思考：LangGraph 在 ReAct × Planning 的角色

| 階段 | 模型行為 | 技術核心 |
| --- | --- | --- |
| **Day 20** | 一次推理 → 一次工具 | ReAct 單輪 |
| **Day 21** | 多步規劃 → 動態反應 | Planning × ReAct |
| **Day 22** | 可視化決策 → 可追蹤推理 | LangGraph 狀態圖 |

LangGraph 讓「思考過程」從黑箱變白箱：  
你能明確看到每一個節點、狀態轉換與條件分支，  
這是進入「可監控的 AI 工作流」的重要一步。

---

## 小結

今天，我們完成了從「會思考」到「看得見它在思考」的進化：

- **可視化決策流程** —— 觀察每一步推理與工具使用。
- **可重現與調試** —— 清楚追蹤狀態轉換與分支。
- **ReAct × Planning × LangGraph** —— 打造能「思考又透明」的 AI Agent。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10393899)

[

Day 21｜從 ReAct 到 Planning：讓 Agent 學會動態調整維也納行程

](https://ithelp.ithome.com.tw/articles/10393899)[下一篇](https://ithelp.ithome.com.tw/articles/10394706)

[

Day 23｜從推理到學習：Reflection × Memory 讓 Agent 從經驗中進化

](https://ithelp.ithome.com.tw/articles/10394706)

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