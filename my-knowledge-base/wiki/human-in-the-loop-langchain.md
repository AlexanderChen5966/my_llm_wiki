---
title: Human-in-the-Loop：讓 Agent 在關鍵決策點暫停等待人類確認
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10396238
type: article
status: processed
asset_value: 58
---

全自動的 Agent 效率高但風險大——尤其當模型能自主呼叫工具、訂房、改資料時，缺乏人工把關可能造成不可逆的錯誤。Human-in-the-Loop（HITL）讓 Agent 在執行關鍵工具前自動暫停，等待人類審核、修改或拒絕。

## T（核心論點）

LangChain v1.0 的 `HumanInTheLoopMiddleware` 能在 Agent 即將呼叫特定工具時自動觸發中斷（interrupt），暫停流程並保存狀態到 LangGraph Checkpointer，等待人類給出 `approve`（同意）、`edit`（修改）或 `reject`（拒絕）三種決策後再恢復執行。這讓「完全自動」與「完全人工」之間找到平衡點。

## B（適用場景）

**適用：**
- 任務涉及不可逆或高風險操作（訂房、付款、修改正式資料）
- 使用者偏好可能臨時改變，需要在執行前補充資訊（飲食限制、預算調整）
- 倫理或判斷模糊的任務類別需要人工裁決

**不適用：**
- 低風險、可逆、高頻率的任務（暫停反而拖慢效率）
- 不需要人類即時介入的純後台批次任務

## R（實作模式）

### 三種決策類型

| 決策類型 | 行為 | 範例 |
|---------|------|------|
| approve | 同意執行原動作 | 確認餐廳推薦後繼續 |
| edit | 修改動作內容 | 加上「不吃牛肉」偏好 |
| reject | 拒絕執行並回覆理由 | 拒絕預訂不合適餐廳 |

### 設定 HITL Middleware

```python
from langchain.agents.middleware import HumanInTheLoopMiddleware
from langgraph.types import Command

hitl = HumanInTheLoopMiddleware(
    interrupt_on={"PlannerAgent": True},
    description_prefix="需要人工補充資訊："
)

supervisor = create_agent(
    model=model,
    tools=[call_planner],
    middleware=[hitl],
    system_prompt="你是旅行統籌助理...",
    checkpointer=InMemorySaver()
)
```

### 中斷與恢復流程

```python
result = await supervisor.ainvoke(
    {"messages": [{"role": "user", "content": "幫我規劃維也納一日遊，包含歌劇院與當地美食體驗"}]},
    {"configurable": {"thread_id": "day27_demo"}}
)

if "__interrupt__" in result:
    interrupt = result["__interrupt__"][0].value
    user_feedback = input("請輸入你的飲食偏好（例如：我不吃牛肉）→ ")

    original_query = interrupt["action_requests"][0]["arguments"]["query"]
    edited_query = f"{original_query}。請注意，使用者的飲食偏好是：{user_feedback}"

    decision = Command(
        resume={"decisions": [{
            "type": "edit",
            "edited_action": {"name": "PlannerAgent", "arguments": {"query": edited_query}}
        }]}
    )

    result = await supervisor.ainvoke(decision, {"configurable": {"thread_id": "day27_demo"}})
```

**關鍵設計：恢復執行時必須使用「相同的 thread_id」**，這樣 Checkpointer 才能正確還原中斷前的狀態，把人類補充的資訊正確接續進原本的工具呼叫參數中。

### 三大效益

1. **可控的自動化**：模型自主行動，但關鍵決策交由人掌握
2. **可追蹤的流程**：每次中斷與人工決策都被記錄，方便審核
3. **可持續的信任**：決策過程透明化，使用者能信任結果

## C（關鍵行動）

- [ ] 識別任務中「不可逆」或「高風險」的工具呼叫，優先在這些節點掛 HITL 中斷
- [ ] 持久化場景（跨 session 暫停）改用 `AsyncPostgresSaver` 等持久化 checkpointer，而非 `InMemorySaver`
- [ ] 恢復執行時務必使用與中斷時相同的 thread_id，否則狀態無法正確還原

## 相關連結

- [[langchain-guardrails-hooks]] — Day 26：另一種安全機制（事前/事後過濾），與 HITL 互補（HITL 處理「需要人判斷」，Guardrails 處理「自動拒絕」）
- [[supervisor-multi-agent-langchain]] — Day 24：本篇加入 HITL 的 Supervisor 架構基礎
