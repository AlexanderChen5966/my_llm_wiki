---
title: 用 LangGraph 把 ReAct × Planning 決策流程節點化
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10394337
type: article
status: processed
asset_value: 55
---

[[react-planning-dynamic-adjustment]] 的決策過程藏在 LLM 的「黑箱」推理裡，外部只看到最終輸出。用 LangGraph `StateGraph` 把 plan→check→adjust→summary 拆成顯式節點，讓整個決策流程變成可觀察、可除錯的圖。

## T（核心論點）

把隱性的 ReAct × Planning 推理循環，轉換成 LangGraph 的顯式狀態圖（State Graph），每個推理階段（規劃、檢查、調整、總結）都是一個獨立節點，狀態在節點間流轉，使決策過程透明化、可視覺化。

## B（適用場景）

**適用：**
- 需要除錯或解釋 Agent 決策過程的場景（合規、教學、團隊協作）
- 流程步驟固定但每步邏輯複雜，想拆解管理
- 需要在特定節點插入人工確認（Human-in-the-Loop）的擴充需求

**不適用：**
- 流程簡單、一次性、不需要中途檢視狀態的任務（直接用 create_agent 即可）

## R（實作模式）

### 自訂 State 類別

延伸 `MessagesState`，加入流程專屬欄位：

```python
from langgraph.graph import StateGraph, MessagesState, END

class State(MessagesState):
    plan: str
    check: str
    adjusted: str
    output: str
```

### 定義節點函式

```python
async def step_plan(state: State):
    plan = await llm.ainvoke(f"請規劃維也納一日行程：{state['messages']}")
    return {"plan": plan.content}

async def step_check(state: State):
    result = check_open_status(state["plan"])
    return {"check": result}

async def step_adjust(state: State):
    if "關閉" in state["check"]:
        new_plan = await llm.ainvoke(f"原計畫：{state['plan']}，部分景點關閉，請調整")
        return {"adjusted": new_plan.content}
    return {"adjusted": state["plan"]}

async def summarize(state: State):
    return {"output": f"最終行程：{state['adjusted']}"}
```

### 組裝圖

```python
graph = StateGraph(State)
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
```

呼叫 `app.invoke({"messages": [...]})` 後，可逐節點檢視 `plan`、`check`、`adjusted`、`output` 的中間狀態，而非只拿到最終回覆。

### 三階段演進對照

| 階段 | 特性 |
|------|------|
| Day 20 純 ReAct | 單輪推理+行動，無顯式規劃 |
| Day 21 Planning × ReAct | 有規劃+動態調整，但邏輯藏在 system_prompt 裡 |
| Day 22 LangGraph 視覺化 | 同樣邏輯拆成節點圖，狀態與流程外顯 |

## C（關鍵行動）

- [ ] 當需要除錯 Agent 決策路徑或插入人工審核點時，把邏輯從 system_prompt 移植到 LangGraph 節點
- [ ] State 類別繼承 MessagesState 並按流程階段加欄位，比塞進單一字串更易讀
- [ ] 圖節點命名直接對應業務階段（plan/check/adjust/summary），讓圖本身具備文件價值

## 相關連結

- [[react-planning-dynamic-adjustment]] — Day 21：本篇視覺化的決策邏輯來源
- [[langgraph-cot-visible]] — Day 15：LangGraph 可視化 CoT 的最初應用
- [[langchain-langgraph-basics]] — LangGraph 基礎概念
- [[supervisor-multi-agent-langchain]] — Day 24：多 Agent 協作架構
