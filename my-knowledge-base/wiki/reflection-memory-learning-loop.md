---
title: Reflection × Memory：讓 Agent 從經驗中學習的循環
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10394706
type: article
status: processed
asset_value: 62
---

[[langgraph-react-planning-visualization]] 讓 Agent 的推理過程可視化，但 Agent 依然「每次都從零開始」，不會記取教訓。加入 Reflection（反思）與 Memory（記憶）兩個元件，Agent 才能真正從經驗中學習，而非單回合的暫時調整。

## T（核心論點）

ReAct 與 Planning 型 Agent 缺乏長期記憶，每次互動都從零推理，無法累積與改善。LangChain v1.0 把記憶整合進 Agent 狀態本身（而非外部物件），透過 `checkpointer` + `thread_id` 讓 Agent 在對話線程中持續保存上下文；搭配 Reflection 節點將反思結果提煉為「可重用決策準則」，形成「推理→反思→記憶→改進」的完整學習循環。

## B（適用場景）

**適用：**
- 任務需要跨多輪互動累積經驗（同一使用者重複諮詢、長期專案協作）
- 需要將「事後修正」轉化為「下次不再犯」的系統設計

**不適用：**
- 單次性、無需累積上下文的任務
- 已知所有規則且不需要從互動中學習的場景

## R（實作模式）

### LangChain v1.0 記憶架構三元件

| 元件 | 功能 |
|------|------|
| AgentState | 管理狀態與訊息，取代傳統記憶物件 |
| checkpointer | 控制記憶儲存方式（`InMemorySaver` 或資料庫）|
| thread_id | 識別對話線程，保留上下文連續性 |

### 五階段學習循環（LangGraph 節點）

```python
class TripState(dict):
    plan: str | None = None
    weather: str | None = None
    reflection: str | None = None
    memory_summary: str | None = None
    output: str | None = None

async def step_plan(state: TripState):
    res = await agent.ainvoke({"messages": [...]}, {"configurable": {"thread_id": "1"}})
    state["plan"] = res["messages"][-1].content
    return state

async def step_reflect(state: TripState):
    res = await agent.ainvoke({
        "messages": [{"role": "user", "content":
            "請基於行程與天氣等可得資訊，指出不合理之處並提出改進準則。"
            "輸出請包含：問題點、原因、改進原則三項。"
        }]
    }, {"configurable": {"thread_id": "1"}})
    state["reflection"] = res["messages"][-1].content
    return state

async def step_memory_update(state: TripState):
    res = await agent.ainvoke({
        "messages": [{"role": "user", "content":
            f"請將以下反思整理成可重用的決策準則摘要：\n{state['reflection']}"
        }]
    }, {"configurable": {"thread_id": "1"}})
    state["memory_summary"] = res["messages"][-1].content
    return state
```

### 串接圖：Plan → Weather → Reflect → Memory → Summary

```python
graph = StateGraph(TripState)
graph.add_node("plan", step_plan)
graph.add_node("weather", step_weather)
graph.add_node("reflect", step_reflect)
graph.add_node("memory", step_memory_update)
graph.add_node("summary", summarize)

graph.set_entry_point("plan")
graph.add_edge("plan", "weather")
graph.add_edge("weather", "reflect")
graph.add_edge("reflect", "memory")
graph.add_edge("memory", "summary")
graph.add_edge("summary", END)
```

**關鍵設計：所有節點共用同一 `agent`（同一 checkpointer + thread_id），讓 Reflect 階段能存取 Plan/Weather 階段的完整上下文，Memory 階段的摘要又能在 Summary 階段被引用——記憶不是外部資料庫查詢，而是「同一對話線程」自然延續的結果。**

### 實測範例：雨天行程自我修正

1. **Plan**：規劃含美泉宮、皇宮、美景宮的行程
2. **Weather**：查得持續下雨，建議減少戶外停留
3. **Reflect**：歸納「午後行程過於緊湊」「雨天體驗下降」等問題，提出「雨天應優先選室內活動」原則
4. **Memory**：將反思整理成可重用準則摘要
5. **Summary**：依準則重新輸出「雨天室內精華版行程」

### 演進脈絡

| Day | 能力 | 技術核心 |
|-----|------|---------|
| Day 20 | 多工具任務 | ReAct |
| Day 21 | 多步規劃+動態反應 | Planning × ReAct |
| Day 22 | 可視化決策過程 | LangGraph |
| Day 23 | 反思與記憶 | Reflection × Memory |

## C（關鍵行動）

- [ ] 需要跨輪次累積經驗時，把記憶設計成 Agent State 的一部分，而非外部資料庫查詢層
- [ ] Reflection 節點的輸出要結構化（問題點/原因/改進原則），才能被 Memory 階段穩定提煉成可重用準則
- [ ] 所有學習循環節點共用同一 checkpointer + thread_id，確保上下文在反思與記憶間正確延續

## 相關連結

- [[langgraph-react-planning-visualization]] — Day 22：本篇延伸的 LangGraph 節點化基礎
- [[llm-memory-pattern]] — LLM Memory Pattern 通用三步循環（Write/Retrieve/Apply）
- [[supervisor-multi-agent-langchain]] — Day 24：Reflection × Memory 之後的多 Agent 協作延伸
- [[agent-trajectory-distillation-harness-zero-perplexity]] — 對照：訓練時期改權重的做法（本篇是推論時期、不改權重）（2026-09-29 新增）
