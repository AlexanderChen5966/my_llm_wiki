---
title: Supervisor × Multi-Agent：用 LangChain v1.0 組成 Agent 團隊
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10395086
type: article
status: processed
asset_value: 68
---

單一 Agent 身兼多職（查天氣、排行程、給建議）容易角色混亂、prompt 臃腫。LangChain v1.0 官方提出兩種多 Agent 協作模式，本篇實作「Tool Calling Pattern」：讓 Supervisor 把專家 Agent 包裝成工具來呼叫，組成一支智慧團隊。

## T（核心論點）

LangChain v1.0 官方文件（docs.langchain.com/oss/python/langchain/multi-agent）定義兩種多 Agent 協作模式：(1) **Tool Calling Pattern** — Supervisor 把其他 Agent 包裝成工具呼叫，控制權始終在 Supervisor；(2) **Handoffs Pattern** — Agent 之間直接對話式轉移控制權。本篇示範 Tool Calling Pattern，搭配共享 checkpointer 讓子 Agent 間保有記憶。

## B（適用場景）

**Tool Calling Pattern 適用：**
- 子任務之間相對獨立，由中央協調者統一發派與整合結果
- 需要明確的職責分工（天氣專家、行程專家、建議專家各司其職）
- 想保留單一入口點與決策中心，方便追蹤與除錯

**Handoffs Pattern 適用（本篇未實作，但需知道差異）：**
- 子 Agent 之間需要連續對話、互相補充，而非單向呼叫回傳

## R（實作模式）

### 建立共享 Checkpointer 的子 Agent

```python
from langgraph.checkpoint.memory import InMemorySaver

checkpointer = InMemorySaver()

def build_subagents():
    weather_agent = create_agent(
        model=llm, tools=[get_weather],
        system_prompt="你是天氣專家，只回答天氣相關問題。",
        checkpointer=checkpointer
    )
    planner_agent = create_agent(
        model=llm, tools=[get_attractions],
        system_prompt="你是行程規劃專家。",
        checkpointer=checkpointer
    )
    advisor_agent = create_agent(
        model=llm, tools=[],
        system_prompt="你是旅遊建議專家，根據天氣與行程給出綜合建議。",
        checkpointer=checkpointer
    )
    return weather_agent, planner_agent, advisor_agent
```

### 把子 Agent 包裝成工具

每個子 Agent 用 `@tool` 裝飾器包成 Supervisor 可呼叫的工具，內部用共享的 `thread_id` 串連記憶：

```python
@tool("WeatherAgent", description="查詢城市天氣，回傳天氣摘要")
async def call_weather_agent(city: str) -> str:
    result = await weather_agent.ainvoke(
        {"messages": [{"role": "user", "content": f"{city}天氣如何？"}]},
        {"configurable": {"thread_id": "1"}}
    )
    return result["messages"][-1].content

@tool("PlannerAgent", description="規劃城市景點行程")
async def call_planner_agent(city: str) -> str:
    result = await planner_agent.ainvoke(
        {"messages": [{"role": "user", "content": f"規劃{city}的一日行程"}]},
        {"configurable": {"thread_id": "1"}}
    )
    return result["messages"][-1].content

@tool("AdvisorAgent", description="根據天氣與行程給出綜合旅遊建議")
async def call_advisor_agent(query: str) -> str:
    result = await advisor_agent.ainvoke(
        {"messages": [{"role": "user", "content": query}]},
        {"configurable": {"thread_id": "1"}}
    )
    return result["messages"][-1].content
```

### Supervisor 統籌

```python
def make_supervisor():
    return create_agent(
        model=llm,
        tools=[call_weather_agent, call_planner_agent, call_advisor_agent],
        system_prompt=(
            "你是旅遊規劃 Supervisor，依需求呼叫 WeatherAgent、PlannerAgent、"
            "AdvisorAgent 取得資訊，再整合成完整建議回覆使用者。"
        )
    )
```

**共享 `thread_id="1"` 的 checkpointer 是關鍵**：讓 WeatherAgent 查到的天氣資訊，AdvisorAgent 呼叫時也能存取到同一份對話記憶，不需要 Supervisor 手動把結果塞進 prompt。

## C（關鍵行動）

- [ ] 子任務職責清晰且由中央協調時，用 Tool Calling Pattern（包裝成 @tool）
- [ ] 子 Agent 間需要共享上下文記憶時，用同一個 checkpointer + 固定 thread_id
- [ ] 每個子 Agent 的 system_prompt 要嚴格限定職責範圍，避免角色越界、互相搶答
- [ ] 若子任務需要連續對話交接（而非單向問答），改用 Handoffs Pattern（官方文件另一模式）

## 相關連結

- [[langchain-langgraph-basics]] — create_agent() 與 LangGraph 基礎
- [[multimodal-multi-agent-extension]] — Day 25：在此架構上擴充 CuisineAgent
- [[n8n-multi-agent-workflow]] — Day 28：同一架構在 n8n 視覺化工作流中的重現
- [[react-planning-dynamic-adjustment]] — 單一 Agent 的規劃調整模式（對照組）
- [[synthesis-multi-agent]] — Multi-Agent 系統知識精粹（本主題 7 篇整合）
