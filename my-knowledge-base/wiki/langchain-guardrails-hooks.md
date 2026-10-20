---
title: Guardrails：用 LangChain Hook 機制為 Agent 劃出安全邊界
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10395588
type: article
status: processed
asset_value: 60
---

Multi-Agent 系統能力越強，風險也越高——使用者可能輸入敏感內容，模型也可能誤解意圖生成不當建議。Guardrails 不是限制模型的創造力，而是在輸入前與輸出後加上「行為邊界」，讓自由推理仍維持在安全範圍內。

## T（核心論點）

LangChain v1.0 提供 `pre_model_hook` 與 `post_model_hook` 兩種掛勾，讓開發者能在模型呼叫前後插入自訂安全檢查邏輯（意圖分類、內容過濾）。NVIDIA 在 ACL 2025 Tutorial 中將 LLM Agent Safety 歸納為風險地圖、評估測試、防護監督、訓練強化、開放課題五層架構，核心精神是「讓模型自由思考但不越界行動」。

## B（適用場景）

**適用：**
- Multi-Agent 系統需要防止使用者輸入或模型輸出涉及敏感/違法主題
- 需要限定 Agent 只處理特定領域請求（如僅處理旅遊相關查詢）
- 合規或信任需求較高的應用場景

**不適用：**
- 封閉、受控的內部工具（風險本身就低，過度設防增加延遲與成本）

## R（實作模式）

### Hook 類型與時機

| 類型 | 檢查時機 | 典型用途 |
|------|---------|---------|
| pre_model_hook | 模型呼叫前 | 檢查使用者輸入是否合法、安全 |
| post_model_hook | 模型輸出後 | 審核模型生成內容是否合理或違規 |

### 雙重判斷：意圖分類 + 安全審查

```python
BANNED_LIST = ["drugs", "weapon", "gambling", "黑市", "毒品"]

intent_agent = create_agent(
    model=model, tools=[],
    system_prompt=(
        "你是一個意圖分類 Agent。判斷使用者意圖是否與「旅遊/觀光/行程規劃」有關。"
        "若屬於旅遊相關請輸出 True；否則輸出 False"
    )
)

safety_agent = create_agent(
    model=model, tools=[],
    system_prompt=(
        f"你是一個安全審查 Agent。判斷文字是否涉及不良或禁止主題: {BANNED_LIST}。"
        "若屬於上述禁止類別請輸出 False；否則輸出 True"
    )
)

class SafetyViolationError(Exception):
    def __init__(self, message: str):
        self.message = message
        self.ai_message = AIMessage(content=message)
        super().__init__(message)

async def safety_hook(state):
    last_message = state["messages"][-1].content
    if not await is_travel_intent(last_message):
        raise SafetyViolationError("抱歉，我只能協助旅遊/行程相關的請求。")
    if not await is_safe_text(last_message):
        raise SafetyViolationError("抱歉，我無法提供涉及不良或禁止場所的旅遊建議。")
    return state
```

### 掛入 Supervisor

```python
supervisor = create_agent(
    model=model,
    tools=[call_weather, call_planner, call_advisor],
    system_prompt="你是旅行統籌助理...",
    checkpointer=InMemorySaver(),
    pre_model_hook=safety_hook,
    post_model_hook=safety_hook
)

try:
    result = await supervisor.ainvoke(
        {"messages": [{"role": "user", "content": "幫我規劃維也納「不良場所」一日遊"}]},
        {"configurable": {"thread_id": "1"}}
    )
except SafetyViolationError as e:
    result = {"messages": [e.ai_message]}
```

**設計優點：** Hook 是獨立函式，可在不同 Agent 間重複使用；不改變模型核心能力的前提下建立可維護的安全框架；邏輯清晰，日後加新規則只需擴充 `safety_hook()` 內部判斷。

### NVIDIA ACL Tutorial：LLM Agent Safety 五層架構

1. **風險地圖**：工具使用風險 / 互動風險 / 多 Agent 責任邊界不明
2. **評估測試**：建立可擴充沙盒環境、具體化行動路徑
3. **防護監督**：生成者+驗證者、紅隊/藍隊機制、可擴充監督（AI 辯論/人機共審）
4. **防護導向訓練**：攻擊者 vs 守門員對抗訓練、自我對弈紅隊測試
5. **開放課題**：實際應用與長期風險的平衡、可重現評估指標

## C（關鍵行動）

- [ ] Multi-Agent 系統上線前，至少加入意圖邊界（主題限定）與內容過濾（禁用詞/類別）兩層基本防護
- [ ] 用 pre/post_model_hook 雙閘設計，輸入輸出都要審查，不能只防一邊
- [ ] Hook 函式設計成獨立可重用模組，方便在多個 Agent 間共享同一套安全規則
- [ ] 安全檢查失敗時用自訂例外（如 SafetyViolationError）中止流程，而非讓模型自由發揮回應

## 相關連結

- [[supervisor-multi-agent-langchain]] — Day 24：本篇加上安全防護的 Supervisor 架構基礎
- [[human-in-the-loop-langchain]] — Day 27：另一種「人類介入」的安全機制，與 Guardrails 互補
- [[ai-agent-architecture-patterns]] — 含 Prompt Injection 防禦相關內容
