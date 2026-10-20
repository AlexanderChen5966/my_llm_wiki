---
title: LangChain × MCP 整合：MultiServerMCPClient 讓工具跨框架共享
date: 2026-06-16
tags: [ai/mcp, ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10392878
type: article
status: processed
asset_value: 58
---

`langchain-mcp-adapters` 讓 LangChain Agent 直接呼叫 MCP Server，實現「工具一次實作，Claude + LangChain 兩邊共用」的跨平台架構。

## T（核心論點）

LangChain 本身有完整工具系統，MCP 的額外價值在於「解耦」：工具以獨立服務存在，不依賴任何框架。結合 `langchain-mcp-adapters`，LangChain Agent 可以動態載入 MCP 工具清單，讓同一個 MCP Server 被 Claude Desktop、LangChain、VSCode 插件同時使用而不需重寫。

## B（適用場景）

**適用：**
- 已有 MCP Server，想讓 LangChain Agent 也能使用相同工具
- 工具需要跨多個 AI 框架共用
- 工具邏輯複雜，想集中維護在 MCP Server 中

**不適用：**
- 工具只有 LangChain 會用（直接用 `@tool` 更簡單）
- 不需要跨平台可攜性

## R（整合實作）

### 安裝依賴

```bash
pip install -q --pre -U langchain
pip install -q -U langchain-google-genai
pip install -q langchain-mcp-adapters
```

### `MultiServerMCPClient`：連線多個 MCP Server

```python
from langchain_mcp_adapters.client import MultiServerMCPClient
from langchain.chat_models import init_chat_model
from langchain.agents import create_agent
import asyncio, os

async def run_agent_with_mcp():
    # 設定 MCP Server 連線（可同時設定多個）
    client = MultiServerMCPClient({
        "accuweather": {
            "transport": "stdio",
            "command": "uv",
            "args": ["--directory", "/path/to/weather", "run", "weather_mcp_server.py"],
            "env": {"ACCUWEATHER_API_KEY": os.getenv("ACCUWEATHER_API_KEY")}
        }
    })

    # 動態取得 MCP 工具清單（自動從 MCP Server 抓 schema）
    tools = await client.get_tools()
    
    llm = init_chat_model("gemini-2.5-flash", model_provider="google_genai")

    # Agent 透明地呼叫 MCP 工具（與呼叫普通 @tool 相同介面）
    agent = create_agent(
        model=llm,
        tools=tools,
        system_prompt="你是一位天氣助理，可以透過工具查詢城市的即時天氣。"
    )

    # ainvoke（非同步）
    response = await agent.ainvoke({
        "messages": [{"role": "user", "content": "維也納的天氣如何？"}]
    })
    
    print(response["messages"][-1].content)

if __name__ == "__main__":
    asyncio.run(run_agent_with_mcp())
```

### 架構分工

```
使用者
  ↓
LangChain Agent（推理 + 回答）
  ↓ 呼叫 MCP 工具
MCP Server（API 互動 + 資料處理）
  ↓ 呼叫外部 API
AccuWeather / 其他外部服務
```

**關鍵價值：Agent 不需要知道 API 細節，只需會呼叫 MCP 工具**

### 與 Claude Desktop 的比較

| | Claude Desktop × MCP | LangChain × MCP |
|--|---|---|
| 設定方式 | `claude_desktop_config.json` | `MultiServerMCPClient` 程式碼設定 |
| 使用場景 | 互動式對話 | 自動化流程 / 程式整合 |
| 工具來源 | 相同 MCP Server | 相同 MCP Server |
| 擴展方式 | 增加 `mcpServers` 設定 | 增加 `MultiServerMCPClient` 鍵值 |

**同一個 MCP Server，兩種客戶端都能用，工具邏輯零重複。**

### `transport: "stdio"` 說明

- `stdio`：LangChain 透過標準輸入/輸出與 MCP Server 通訊（本地進程）
- 啟動時會在背景 fork 一個 MCP Server 進程
- `command` + `args` 定義如何啟動該進程

## C（關鍵行動）

- [ ] 有多個 MCP Server 時，在 `MultiServerMCPClient` 中一次設定，`get_tools()` 會合併所有工具清單
- [ ] MCP Server 有更新時，LangChain 這邊不需要改程式碼（`get_tools()` 動態取最新 schema）
- [ ] 非同步環境：記得用 `ainvoke` 而非 `invoke`

## 相關連結
- [[context7-mcp-server-guide]] — Context7 MCP：用「use context7」注入最新版官方文件，避免 AI 依賴過時 API
- [[mcp-fastmcp-server]] — 如何建立 FastMCP Server（工具服務端）
- [[react-multi-mcp-agent]] — 多個 MCP Server 同時協作（Multi-MCP 模式）
- [[langchain-tool-use]] — LangChain 框架內 @tool 工具（不需要 MCP 時使用）
- [[ai-skills-mcp-plugins-agents-comparison]] — MCP 在整個 AI 工具生態的定位
