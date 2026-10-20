---
title: MCP FastMCP Server：把工具升級為跨框架可攜服務
date: 2026-06-16
tags: [ai/mcp, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10392577
type: article
status: processed
asset_value: 65
---

MCP（Model Context Protocol）把 AI 工具從「框架內的函式」升級為「獨立可重複使用的服務」，一次實作就能被 Claude Desktop、LangChain、VSCode 插件等不同應用共用。

## T（核心論點）

> 🔴 **過時警語（2026-09-10 加註）：本條目 R 層的 import 路徑已失效。**
> MCP Python SDK **2.0.0 已移除 `mcp.server.fastmcp` 模組**，`FastMCP` 類別改名為 **`MCPServer`**，
> 位置搬到 `mcp.server.mcpserver`。照本條目的程式碼直接跑會拿到
> `ModuleNotFoundError: No module named 'mcp.server.fastmcp'`。
> 同版還把 Python 側物件屬性全面改成 snake_case（`serverInfo` → `server_info`、
> `inputSchema` → `input_schema`），寫測試 client 時會連撞第二次；
> 線上傳輸的 JSON-RPC 仍是 camelCase，改的只有 Python 物件介面。
>
> **正確替代寫法（SDK 2.0.0 起）：**
> ```python
> # SDK 2.0 起 FastMCP 改名為 MCPServer（網路上多數教學仍停在 fastmcp）
> from mcp.server.mcpserver import MCPServer
>
> mcp = MCPServer(
>     "weather-mcp",
>     instructions="給模型看的 server 用途說明。",
>     version="0.1.0",
> )
> ```
> 介面本身幾乎沒變——一樣是 `@mcp.tool()` 裝飾器加 `mcp.run()`，type hint + docstring 仍自動翻成
> JSON Schema，**所以下方 R 層除了第一行 import 與建構子名稱之外的內容仍然成立**，
> 舊寫法保留在下方作為版本對照。
> 現行做法與踩坑實錄見 [[self-built-mcp-server-237-lines]]。

工具的可攜性問題本質上是「耦合度」問題：傳統 `@tool` 函式與框架綁定，換框架就得重寫。MCP 提供標準化協議，讓工具成為獨立服務，不依賴任何特定 AI 框架——就像 USB 插槽讓不同設備相容。

## B（適用場景）

**適用：**
- 同一工具需要在多個環境使用（Claude Desktop + LangChain + VSCode）
- 工具邏輯複雜，值得獨立維護
- 需要工具與 Agent 邏輯解耦（工具更新不影響 Agent 流程）

**不適用：**
- 工具只在單一框架中使用（直接用 `@tool` 更簡單）
- 一次性工具（搭建 MCP Server 的成本超過收益）

## R（FastMCP Server 實作）

### 專案初始化（uv）

```bash
uv init weather
cd weather
uv venv && source .venv/bin/activate
uv add "mcp[cli]" requests
touch weather_mcp_server.py
```

MCP 官方文件推薦 `uv` 作為 Python 環境管理工具（比 pip/venv 更快且整合專案管理）。

### FastMCP Server 完整範例

⚠️ 以下為 SDK 2.0.0 之前的寫法（保留作對照）。第 1 行 import 與 `FastMCP(...)` 建構子
須依 T 層警語改為 `from mcp.server.mcpserver import MCPServer` / `MCPServer(...)`。

```python
import os, requests
from mcp.server.fastmcp import FastMCP

API_KEY = os.getenv("ACCUWEATHER_API_KEY")
if not API_KEY:
    raise ValueError("請先設定 ACCUWEATHER_API_KEY 環境變數")

mcp = FastMCP("weather-mcp")

@mcp.tool()
def get_weather(city: str) -> dict:
    """查詢指定城市的即時天氣（AccuWeather API）。
    
    Args:
        city: 城市名稱（例：維也納、台北）
    Returns:
        天氣資料字典（溫度、濕度、風速等）
    """
    # Step 1. 找城市代碼
    loc_url = "https://dataservice.accuweather.com/locations/v1/cities/search"
    loc_res = requests.get(loc_url, params={"apikey": API_KEY, "q": city}, timeout=10).json()
    if not loc_res:
        return {"error": f"找不到城市：{city}"}
    location_key = loc_res[0]["Key"]

    # Step 2. 查天氣
    weather_url = f"https://dataservice.accuweather.com/currentconditions/v1/{location_key}"
    weather_res = requests.get(weather_url, params={"apikey": API_KEY, "details": "true"}, timeout=10).json()
    return weather_res[0] if weather_res else {"error": "查不到天氣"}

if __name__ == "__main__":
    mcp.run()
```

**與 LangChain `@tool` 的關鍵差異：**

| | LangChain `@tool` | FastMCP `@mcp.tool()` |
|--|---|---|
| 運行方式 | 框架內函式 | 獨立 MCP Server 進程 |
| 可攜性 | 綁定 LangChain | 任何相容 MCP 的應用 |
| 維護方式 | 更新需同步更新 Agent 程式碼 | 獨立版本管理 |
| 呼叫方式 | 直接呼叫 | 透過 MCP 協議 |

### Claude Desktop 整合設定

```json
{
  "mcpServers": {
    "accuweather": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/weather",
        "run",
        "weather_mcp_server.py"
      ],
      "env": {
        "ACCUWEATHER_API_KEY": "你的 API Key"
      }
    }
  }
}
```

設定路徑：Claude Desktop → Settings → Developer → Edit Config → `claude_desktop_config.json`

### MCP 三大價值

1. **統一介面**：資料庫 / API / 內部服務 → 全部包裝成 MCP Tool
2. **跨應用可攜性**：一次實作，Claude Desktop / LangChain / VSCode 全用
3. **治理彈性**：工具獨立維護、控管權限，不綁死特定 Agent 流程

## C（關鍵行動）

- [ ] 工具跨平台使用 → 用 FastMCP；單一框架使用 → 直接 `@tool`
- [ ] API Key 統一透過 `env` 傳入，不寫死在程式碼中
- [ ] MCP Server 的 `@mcp.tool()` docstring 同樣重要，是 LLM 判斷工具用途的依據

## 相關連結
- [[self-built-mcp-server-237-lines]] — 🔴 **現行做法**：SDK 2.0.0 的 `MCPServer` 寫法、snake_case 破壞性變更、
  `claude mcp add --scope project` 與 `.mcp.json` 核准機制；本條目的 `mcp.server.fastmcp` 已不可用
- [[microsoft-mcp-for-beginners-course]] — MCP 四階段學習路徑（六語言範例）；⚠️ 課程對齊舊 spec，注意 2026-07-28 的破壞性變更
- [[context7-mcp-server-guide]] — Context7 MCP Server：在提示詞裡寫「use context7」讓 AI 讀到最新版官方文件，解決訓練資料過時問題
- [[langchain-tool-use]] — LangChain 框架內 @tool 工具模式
- [[langchain-mcp-integration]] — LangChain Agent 透過 MultiServerMCPClient 呼叫 MCP
- [[react-multi-mcp-agent]] — 多個 MCP Server 協同（Multi-MCP）
- [[ai-skills-mcp-plugins-agents-comparison]] — MCP vs Skills vs Plugins vs Agents 定位比較
- [[agent-skills-vs-mcp]] — Skills vs MCP 選擇判斷標準
- [[fastapi-python-api-framework]] — FastAPI（Python API 框架，AI 服務化）
- [[mcp-stateless-architecture]] — MCP 協定演進：新版無狀態架構讓 MCP Server 更易水平擴充、適合企業級部署
