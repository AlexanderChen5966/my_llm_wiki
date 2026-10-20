---
title: LangGraph 讓 CoT 可視化：@task / @entrypoint / add_messages
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10390932
type: article
status: processed
asset_value: 55
---

LangGraph 透過 `@task`、`@entrypoint`、`add_messages` 三個裝飾器，讓 Chain-of-Thought 從黑盒文字變成可觀察、可追蹤的執行步驟流程圖。

## T（核心論點）

CoT（Chain-of-Thought）原本只是 LLM 輸出中的一段文字，難以驗證每步是否正確執行。LangGraph 把每個推理步驟包成 `@task`，讓「思考過程」變成有結構、可追蹤的工作流節點，把隱形的推理路徑轉為顯式的狀態圖。

## B（適用場景）

**適用：**
- 需要驗證 Agent 推理每個步驟是否如預期執行
- 多工具協作（需要追蹤哪個工具在哪個步驟被呼叫）
- Production 環境需要 Debug/Observability

**不適用：**
- 簡單單輪對話（成本過高）
- 不需要知道中間步驟的任務

## R（三個核心裝飾器）

### `@task`：定義可追蹤的步驟

```python
from langgraph.func import task

@task
def check_weather(city: str) -> str:
    """Step 1：查詢天氣"""
    # ... 呼叫 API
    return weather_result

@task
def select_venue(weather: str, preferences: list) -> str:
    """Step 2：依天氣選擇場地"""
    # ... 推理邏輯
    return venue_recommendation
```

每個 `@task` 函式成為圖中的獨立節點，LangGraph 自動追蹤其輸入輸出與執行順序。

### `@entrypoint`：定義工作流入口與 CoT 串連

```python
from langgraph.func import entrypoint

@entrypoint()
def plan_vienna_trip(state):
    city = state["city"]
    preferences = state["preferences"]
    
    # Step 1：查天氣（可追蹤節點）
    weather = check_weather(city).result()
    
    # Step 2：依天氣選場地（依賴前步結果）
    venue = select_venue(weather, preferences).result()
    
    return {"plan": venue, "weather": weather}
```

`@entrypoint` 定義整條 CoT 鏈，`.result()` 是 task 的等待點，LangGraph 在這裡記錄狀態快照。

### `add_messages`：跨步驟累積對話歷史

```python
from langgraph.graph import MessagesState
from langgraph.graph.message import add_messages
from typing import Annotated

class TripState(MessagesState):
    messages: Annotated[list, add_messages]  # 自動 merge，不覆蓋
    weather: str
    venue: str
```

`add_messages` 確保多步驟執行中的所有工具呼叫與回應都累積在 `messages` 中，而不是每步覆蓋，讓最後可以看到完整的推理歷程。

### CoT 可視化效果

```
執行前（黑盒）：
User: "幫我規劃維也納行程" → LLM 輸出 → 答案

執行後（可視化）：
User: "幫我規劃維也納行程"
  ↓ @task check_weather("維也納")    → "晴天、15°C"
  ↓ @task select_venue("晴天", [...]) → "美泉宮花園"
  ↓ 最終回覆
```

每步的輸入、輸出、執行時間都有記錄，可用 LangSmith 或自訂 callback 追蹤。

## C（關鍵行動）

- [ ] 任何需要多步驟推理的 Agent，改用 `@task` 包每個步驟
- [ ] 使用 `add_messages` 而非直接 list append，確保多步驟歷史完整累積
- [ ] Production 環境搭配 LangSmith 讓 CoT 可在 UI 中追蹤與 Debug

## 相關連結

- [[langchain-langgraph-basics]] — LangGraph StateGraph 基礎概念
- [[langchain-tool-use]] — @tool 工具定義（@task 的工具版本）
- [[react-multi-mcp-agent]] — ReAct TAO 循環（更完整的多步驟推理）
- [[ai-agent-architecture-patterns]] — Plan&Execute 架構（與 CoT 可視化互補）
