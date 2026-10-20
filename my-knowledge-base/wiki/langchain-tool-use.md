---
title: LangChain Tool Use：@tool / ToolNode / create_agent 完整模式
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10391965
type: article
status: processed
asset_value: 60
---

LangChain v1.0 的 Tool Use 三件套：`@tool` 定義能力、`ToolNode` 管理執行、`create_agent` 組合決策。三者分工讓 AI 從「回答問題」進化為「完成任務」。

## T（核心論點）

Tool Use 的本質不是「讓 AI 呼叫 API」，而是讓 AI 能自主判斷「何時需要使用工具」及「使用哪個工具」。LangChain v1.0 的三件套把「定義能力 / 管理執行 / 決策組合」分離，讓工具系統可擴展且錯誤可恢復。

## B（適用場景）

**適用：**
- 需要串接外部 API（天氣、資料庫、搜尋引擎等）
- 任務需要多個工具組合才能完成
- 需要錯誤處理與平行工具呼叫

**不適用：**
- 純文字生成（不需要外部資料）
- 工具呼叫邏輯固定不需要 AI 判斷的場景（直接寫程式即可）

## R（三件套詳解）

### 1. `@tool`：把函式變成 AI 可呼叫的能力

```python
from langchain_core.tools import tool

@tool
def accuweather_search_city(city: str, country_code: str = "AT") -> dict:
    """用 AccuWeather 搜尋城市並回傳 Location Key。
    
    Args:
        city: 城市名稱（中文或英文）
        country_code: 國家代碼（預設 AT 奧地利）
    """
    url = "https://dataservice.accuweather.com/locations/v1/cities/search"
    params = {"apikey": API_KEY, "q": city}
    r = requests.get(url, params=params, timeout=10)
    r.raise_for_status()
    return r.json()[0] if r.json() else {"error": "找不到城市"}

@tool
def accuweather_current_conditions(location_key: str) -> dict:
    """查詢指定 Location Key 的即時天氣。
    
    Args:
        location_key: AccuWeather Location Key
    """
    url = f"https://dataservice.accuweather.com/currentconditions/v1/{location_key}"
    params = {"apikey": API_KEY, "details": "true"}
    r = requests.get(url, params=params, timeout=10)
    return r.json()[0] if r.json() else {"error": "查不到天氣"}
```

**關鍵設計原則：**
- **型別註解** → 自動生成 JSON Schema，讓 LLM 知道如何傳參數
- **docstring** → 成為工具說明，幫助 LLM 理解「什麼時候用這個工具」
- **Args 區塊** → 每個參數有說明，提升 LLM 正確傳參機率

### 2. `ToolNode`：統一管理工具執行

```python
from langchain.agents import ToolNode

tools = [accuweather_search_city, accuweather_current_conditions]

tool_node = ToolNode(
    tools=tools,
    handle_tool_errors=True  # 工具出錯不中斷 Agent，轉為錯誤訊息繼續
)
```

**`handle_tool_errors=True` 的效果：**
- 工具拋出異常 → Agent 收到錯誤訊息 → Agent 可以嘗試其他工具或修正參數
- 支援平行工具呼叫：Agent 同時呼叫多個工具時，ToolNode 並行執行

### 3. `create_agent`：組合 LLM + 工具 + 系統提示

```python
from langchain.chat_models import init_chat_model
from langchain.agents import create_agent

llm = init_chat_model("gemini-2.5-flash", model_provider="google_genai", temperature=0)

agent = create_agent(
    model=llm,
    tools=tool_node,
    system_prompt="你是專業氣象助理。查詢天氣時，先用 accuweather_search_city 找 Location Key，再用 accuweather_current_conditions。"
)

# 執行：思考 → 工具呼叫 → 工具結果 → 最終回答
resp = agent.invoke({
    "messages": [{"role": "user", "content": "幫我查維也納的即時天氣"}]
})
```

### 完整執行循環

```
用戶輸入
  ↓
Agent 思考（LLM 推理）
  ↓
工具呼叫判斷（需要工具？要哪個？傳什麼參數？）
  ↓
ToolNode 執行（呼叫 accuweather_search_city → 取得 Location Key）
  ↓
Agent 再思考（拿到 Location Key，下一步呼叫 current_conditions）
  ↓
ToolNode 執行（呼叫 accuweather_current_conditions → 取得天氣）
  ↓
Agent 最終回答（整合兩次工具結果成自然語言）
```

## C（關鍵行動）

- [ ] `@tool` 的 docstring 是最重要的工具文件——寫給 LLM 看，說明「何時用」而非「怎麼實作」
- [ ] `handle_tool_errors=True` 在 Production 必開，避免單個工具失敗中斷整個 Agent
- [ ] system_prompt 中明確說明工具使用順序（尤其多步驟依賴時）

## 相關連結

- [[langchain-langgraph-basics]] — LangChain v1.0 六大模組基礎
- [[mcp-fastmcp-server]] — 把工具升級為跨框架 MCP 服務
- [[langchain-mcp-integration]] — LangChain Agent 呼叫 MCP 工具
- [[react-multi-mcp-agent]] — ReAct 策略協調多個工具
- [[langchain-rag-architectures]] — 把 RAG retriever 包裝成 @tool
