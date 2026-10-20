---
title: 人機協作三層架構：從自動化到增強的範式轉移
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10396972
type: article
status: processed
asset_value: 65
---

30 天系列走到尾聲，最終的關鍵不是「Agent 與 Agent 之間」的合作，而是「人與 AI 之間」的協作。本篇整合 Multimodal、Guardrails、HITL、Reflection、Memory 全部能力，並引入 ACL 2025 Tutorial 的人機協作三層架構，把整套系統落地成一個完整閉環。

## T（核心論點）

人機協作的目標不是讓 AI 取代人（Automation），而是讓 AI 增強人類能力（Augmentation）。ACL 2025《Human-AI Collaboration: How AIs Augment Human Teammates》Tutorial 提出三層協作架構——互動層（Interaction）、流程層（Process）、基礎層（Infrastructure）——並以「Centaur Evaluation」（人機混合評估）取代「AI 是否比人強」的單一標準，改看「人+AI 是否超越個別表現」。

## B（適用場景）

**適用：**
- 系統需要長期、跨輪次與使用者互動並逐漸個人化（記住偏好、累積信任）
- 任務涉及人類價值判斷（飲食禁忌、倫理界線），AI 難以單方面推斷
- 需要完整可追溯、可驗證的決策流程（合規或團隊協作場景）

**不適用：**
- 一次性、無需個人化或人工確認的簡單查詢任務

## R（三層協作架構）

| 層級 | 核心任務 | 對應技術元件 |
|------|---------|-------------|
| Interaction Layer（互動層）| 使用者與 AI 的溝通介面 | Chat、Gmail 審核、n8n 可視化工作流 |
| Process Layer（流程層）| 協作核心、任務分工、回饋機制 | LangChain / LangGraph Agent 流程 |
| Infrastructure Layer（基礎層）| 記憶、個人化、執行模組 | Memory、MCP 工具、n8n 節點 |

### 完整閉環：規劃→審核→反思→修正→再規劃→落地

```
[Chat 輸入] → [Supervisor /plan] → [Markdown→HTML] → [Gmail 審核]
                                                          ├─ Approve → [Google Calendar 建立事件]
                                                          └─ Reject  → [/reflect 反思] → [Memory 寫入] → 回到 Supervisor 重新規劃
```

### FastAPI 服務化（供 n8n 呼叫）

```python
class PlanRequest(BaseModel):
    text: str
    reflection: str | None = None
    thread_id: str = "trip"

class ReflectRequest(BaseModel):
    plan: str
    feedback: str
    thread_id: str = "trip"

@app.post("/plan")
async def api_plan(req: PlanRequest):
    if req.reflection:
        req.text += f" 另外，請參考以下反思建議進行調整：{req.reflection}"
    payload = {"messages": [{"role": "user", "content": req.text}]}
    result = await SUPERVISOR.ainvoke(payload, {"configurable": {"thread_id": req.thread_id}})
    return {"plan": result["messages"][-1].content}

@app.post("/reflect")
async def api_reflect(req: ReflectRequest):
    prompt = f"行程安排：{req.plan}。使用者回饋：{req.feedback}。請分析並提供反思建議。"
    payload = {"messages": [{"role": "user", "content": prompt}]}
    result = await REFLECT_AGENT.ainvoke(payload, {"configurable": {"thread_id": req.thread_id}})
    return {"reflection": result["messages"][-1].content}
```

**關鍵設計：把 LangChain Multi-Agent 系統包成 FastAPI 服務，讓 n8n（互動層）可以透過 HTTP 呼叫，而不需要把 Python Agent 邏輯搬進 n8n 節點本身——n8n 負責人機互動與流程視覺化，FastAPI 背後的 LangChain 負責推理。**

### 實測閉環：「我不吃牛肉」個人化案例

1. 第一版行程含牛肉餐點（Tafelspitz、Goulash）寄出審核
2. 使用者拒絕並回饋「我不吃牛肉」（人類 steering/control 介入點）
3. Reflect Agent 解析回饋，產生結構化反思，寫入 Memory
4. Supervisor 重新規劃，餐食改為非牛肉選項並明顯標示
5. 使用者批准 → 自動建立 Google Calendar 事件（Augmentation 落地）
6. **同一 thread_id 下再次詢問**：系統無需再次審核偏好，直接輸出無牛肉版本——證明反思已寫入 Memory，形成個人化記憶

### Centaur Evaluation：評估標準的轉移

不再問「AI 是否比人強」，而是問「人+AI 是否超越個別表現」。本案例中 AI 擅長資料整合與生成，人類提供價值觀與偏好——AI 難以單獨推測「不吃牛」，人類又耗時比對景點天氣，兩者透過反思循環協作達成互補。

## C（關鍵行動）

- [ ] 設計人機協作系統時，明確區分互動層（介面）、流程層（協調）、基礎層（記憶/執行），避免三層職責混雜在同一段程式碼
- [ ] 把 AI 推理邏輯（LangChain）包裝成獨立 API 服務，讓互動層工具（n8n、Slack Bot 等）透過標準介面呼叫，而非耦合進流程平台
- [ ] 評估系統成效時採用 Centaur Evaluation 思維：衡量「人+AI 協作後」的整體表現，而非單獨評價 AI 輸出品質
- [ ] 同一 thread_id 持續累積反思記憶，讓系統能對同一使用者「越用越懂你」

## 相關連結

- [[reflection-memory-learning-loop]] — Day 23：本篇閉環中 Reflect/Memory 節點的技術基礎
- [[human-in-the-loop-langchain]] — Day 27：Gmail 審核環節背後的 HITL 機制
- [[n8n-multi-agent-workflow]] — Day 28：本篇 n8n 互動層的前置實作
- [[langchain-guardrails-hooks]] — Day 26：系統安全邊界，與本篇人機協作互補
