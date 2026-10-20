---
title: ReAct × Multi-MCP：多工具協同的 TAO 推理循環
date: 2026-06-16
tags: [ai/agent, ai/mcp, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10393533
type: article
status: processed
asset_value: 68
---

ReAct 策略（Reason + Act）讓 Agent 在多工具環境中「先思考、再行動」；Multi-MCP 讓多個獨立 MCP Server 同時接入同一 Agent，形成真正的跨工具智慧協作。

## T（核心論點）

單一工具解決單一問題；真實應用需要多工具協同——天氣 + 景點 + 行程合計在一次對話中完成。ReAct 的 TAO 循環（Think→Act→Observe）讓 Agent 不再線性執行，而是根據每個工具的觀察結果動態調整下一步行動。

## B（適用場景）

**適用：**
- 需要多個外部資料來源才能回答的問題
- 工具間有條件依賴（「如果天氣好 → 推薦戶外」的邏輯）
- 複雜任務需要多輪工具呼叫

**不適用：**
- 單一工具可以解決的問題（ReAct 開銷不必要）
- 對延遲極度敏感的場景

## R（TAO 循環 + Multi-MCP 實作）

### ReAct TAO 循環

```
T（Think）→ A（Act）→ O（Observe）→ T（Think）→ ...
```

**維也納旅遊助理的 TAO 執行範例：**

```
[T] 用戶問「依今天天氣推薦維也納景點」
    → 我需要先知道天氣，再知道景點，才能推薦

[A] 呼叫 Weather MCP → get_weather("維也納")

[O] 回傳：多雲、12°C、涼爽無雨

[T] 天氣偏涼且多雲，適合室內。我需要景點清單

[A] 呼叫 Attractions MCP → get_attractions("維也納")

[O] 回傳：皇宮、美泉宮、藝術史博物館、史蒂芬大教堂

[T] 根據多雲天氣 → 主推室內（皇宮/博物館），
    帶備選戶外（美泉宮花園、教堂登塔）

→ 整合最終回答
```

每個 [O] 觀察都可能改變下一步的 [T] 思考，這是 ReAct 比固定流程更靈活的關鍵。

### Multi-MCP Server 設定

```python
# 同時接入兩個 MCP Server
client = MultiServerMCPClient({
    "accuweather": {
        "transport": "stdio",
        "command": "uv",
        "args": ["--directory", "/path/to/weather", "run", "weather_mcp_server.py"],
        "env": {"ACCUWEATHER_API_KEY": accuweather_api_key}
    },
    "attractions": {
        "transport": "stdio",
        "command": "uv",
        "args": ["--directory", "/path/to/attractions", "run", "attractions_mcp_server.py"]
        # 不需要 API Key（用 Wikivoyage 公開 API）
    }
})

# get_tools() 自動合併兩個 MCP Server 的所有工具
tools = await client.get_tools()
# tools 現在包含：get_weather + get_attractions
```

### Attractions MCP Server（Wikivoyage API）

```python
import httpx
from bs4 import BeautifulSoup
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("attractions-mcp")

@mcp.tool()
async def get_attractions(city: str) -> dict:
    """查詢指定城市的熱門景點（Wikivoyage 中文 API）。
    
    Args:
        city: 城市名稱（中文）
    """
    params = {
        "action": "parse",
        "page": city,
        "prop": "text",
        "format": "json"
    }
    async with httpx.AsyncClient(timeout=10.0) as client:
        res = await client.get("https://zh.wikivoyage.org/w/api.php", params=params)
    
    html = res.json().get("parse", {}).get("text", {}).get("*", "")
    soup = BeautifulSoup(html, "html.parser")
    
    # 解析「主要景點」章節
    attractions = []
    see_section = soup.find(id="主要景點")
    if see_section:
        for li in see_section.find_all_next("li", limit=20):
            name_span = li.find("span", class_="listing-name")
            name = name_span.get_text(strip=True) if name_span else li.get_text(strip=True)
            attractions.append({"name": name})
    
    return {"city": city, "attractions": attractions}

if __name__ == "__main__":
    mcp.run()
```

**用 `httpx` 而非 `requests` 的原因：** 支援 async/await，與 LangChain 非同步架構整合更順暢，且支援同時請求多個頁面。

### Multi-MCP Agent 執行

```python
agent = create_agent(
    model=llm,
    tools=tools,  # 包含 get_weather + get_attractions
    system_prompt="你是旅遊助理，根據天氣與景點資料推薦最合適的行程。"
)

response = await agent.ainvoke({
    "messages": [{"role": "user", "content": "幫我依今天天氣推薦維也納景點"}]
})
```

### ReAct vs 固定流程比較

| 面向 | 固定流程 | ReAct |
|------|----------|-------|
| 工具呼叫順序 | 事先定義 | 動態決定 |
| 條件邏輯 | 硬編碼 | Agent 自行推理 |
| 工具間依賴 | 顯式定義 | 隱式推導 |
| 靈活度 | 低 | 高 |
| 可預測性 | 高 | 低 |

## C（關鍵行動）

- [ ] 多工具 Agent 必須在 system_prompt 說明工具組合的意圖（「天氣 + 景點 = 行程推薦」）
- [ ] 每個 MCP Server 的 docstring 要讓 Agent 知道「何時該呼叫這個工具」
- [ ] 工具間有條件依賴時（先查天氣再推薦景點），讓 TAO 循環自然驅動，不需手動定義順序

## 相關連結

- [[mcp-fastmcp-server]] — FastMCP Server 建立方式
- [[langchain-mcp-integration]] — MultiServerMCPClient 基礎設定
- [[langchain-tool-use]] — @tool / ToolNode 框架內工具模式
- [[langchain-langgraph-basics]] — LangGraph StateGraph 控制更複雜的多步流程
- [[ai-agent-architecture-patterns]] — ReAct 架構深度解析
