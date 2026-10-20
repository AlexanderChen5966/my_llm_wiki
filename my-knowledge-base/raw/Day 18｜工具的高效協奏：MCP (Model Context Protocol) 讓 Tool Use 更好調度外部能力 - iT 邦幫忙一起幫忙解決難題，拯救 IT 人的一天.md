---
title: "Day 18｜工具的高效協奏：MCP (Model Context Protocol) 讓 Tool Use 更好調度外部能力 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10392577"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言 在 昨天 Day 17，我們看到如何透過 LangChain 的工具系統，把 API 真正接上來，讓 AI 不只是「回答」，而是能 實際完成任務。 不過，當工具越來越多，問題就來了： 如何統一管..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 18

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 18 篇

## Day 18｜工具的高效協奏：MCP (Model Context Protocol) 讓 Tool Use 更好調度外部能力

- 分享至

## 前言

在 **[昨天 Day 17](https://ithelp.ithome.com.tw/articles/10391965)** ，我們看到如何透過 LangChain 的工具系統，把 API 真正接上來，讓 AI 不只是「回答」，而是能 **實際完成任務** 。

不過，當工具越來越多，問題就來了：

- 如何統一管理工具？
- 如何避免每個 Agent 框架都要重寫工具邏輯？
- 如何讓不同應用程式（Claude、LangChain、VSCode 插件…）都能共用同一套工具？

這就是 **MCP (Model Context Protocol)** 的價值。

MCP 把「工具」抽象成一個標準化協議，讓模型可以透過統一的方式存取它們。換句話說，工具不再依賴某個框架，而是變成 **獨立可重複使用的服務** 。

---

## MCP 的角色定位

簡單來說， [MCP](https://modelcontextprotocol.io/docs/getting-started/intro) 提供三件事：

1. **統一的介面** ：不管是資料庫、API，還是內部服務，都可以包裝成 MCP Tool。
2. **跨應用的可攜性** ：一次實作，可以在 Claude Desktop、LangChain、VSCode 等不同環境使用。
3. **更好的治理** ：工具可獨立維護、控管權限與存取，不用綁死在某個應用裡。

簡單來說，MCP 就像 **「USB 介面」** ：  
工具就像滑鼠或鍵盤，MCP 提供了統一的插槽，任何相容的系統都能直接使用。

---

## Demo：把 AccuWeather 包裝成 MCP 工具

我們延續昨天的場景 —— 查詢維也納天氣。  
不同的是，這次我們用 **MCP 服務** 來封裝，讓工具能被 Claude Desktop 或 LangChain **共用** 。

### 運作流程

  
*圖：MCP 把工具獨立成一個服務，Claude Desktop 呼叫 MCP Tool，再由 MCP 串接 AccuWeather API，最後整合結果回覆使用者。*

---

### 建立 MCP Server 專案

在開始前，我們需要先建立一個 **MCP Server 專案** 。  
這裡使用的是 **[uv](https://docs.astral.sh/uv/)** —— 一個現代化的 Python 專案管理工具。

它本身內建了常見的功能：

- **套件安裝** ：具備與 pip 類似的功能，但速度更快，並支援快取與多來源
- **虛擬環境管理** ：提供與 venv 類似的隔離環境，無需額外工具
- **專案依賴管理** ：逐步補齊 poetry/pipenv 的特性，支援 `pyproject.toml` 與 lockfile

這些功能都被整合在同一套流程裡，讓我們能更快、更輕量地建立乾淨的專案環境。  
再加上 MCP 官方文件中也經常搭配 uv 作為範例，因此它是目前 **相當推薦的選擇** 。

以下是初始化步驟：

```bash
# 建立一個新的專案目錄
uv init weather
cd weather

# 建立虛擬環境並啟用
uv venv
source .venv/bin/activate

# 安裝相依套件
uv add "mcp[cli]" requests

# 建立 MCP Server 的主程式檔案
touch weather_mcp_server.py
```

完成後，將下面的程式碼（ `weather_mcp_server.py` ）存入專案中：

---

### 程式實作

```python
import os
import requests
from mcp.server.fastmcp import FastMCP

# 從環境變數讀取 AccuWeather API Key
API_KEY = os.getenv("ACCUWEATHER_API_KEY")
if not API_KEY:
    raise ValueError("請先設定 ACCUWEATHER_API_KEY 環境變數")

# 建立 FastMCP Server
mcp = FastMCP("weather-mcp")

@mcp.tool()
def get_weather(city: str) -> dict:
    """查詢指定城市的即時天氣（使用 AccuWeather API）。
    
    Args:
        city: 城市名稱（例如：維也納、台北）

    Returns:
        天氣資料字典
    """
    # Step 1. 找城市代碼
    loc_url = "https://dataservice.accuweather.com/locations/v1/cities/search"
    loc_params = {"apikey": API_KEY, "q": city, "language": "zh-tw"}
    loc_res = requests.get(loc_url, params=loc_params, timeout=10).json()
    if not loc_res:
        return {"error": f"找不到城市：{city}"}
    location_key = loc_res[0]["Key"]

    # Step 2. 查天氣
    weather_url = f"https://dataservice.accuweather.com/currentconditions/v1/{location_key}"
    weather_params = {"apikey": API_KEY, "language": "zh-tw", "details": "true"}
    weather_res = requests.get(weather_url, params=weather_params, timeout=10).json()

    return weather_res[0] if weather_res else {"error": "查不到天氣"}

if __name__ == "__main__":
    mcp.run()
```

---

### 使用方式

在 **Claude Desktop** 的 `Settings → Developer → Edit Config` 進行 MCP 設定

![Claude Desktop Config](https://ithelp.ithome.com.tw/upload/images/20251003/20165544epDpm7g3Nt.png)

編輯 `claude_desktop_config.json` ：

```json
{
  "mcpServers": {
    "accuweather": {
       "command": "uv",

       "args": [
           "--directory",
           "{MCP 程式路徑}",
           "run",
           "weather_mcp_server.py"
       ],
       "env": {
           "ACCUWEATHER_API_KEY": "{你的 API Key}"
       }
  }
}
```

這裡每個欄位的意義是：

- **`mcpServers`**  
	一個 JSON 物件，裡面可以列出多個 MCP 服務。  
	`accuweather` 是自訂的名稱（你也可以取成 `weather` 或其他名字），Claude 會依照這個名稱來識別 MCP Tool。
- **`command`**  
	啟動 MCP Server 的指令。  
	這裡設定為 `uv` ，代表我們用 `uv` 來執行 Python 程式。
- **`args`**  
	傳給 `command` 的參數。
	- `--directory MCP 程式路徑` ：指定 MCP server 的所在資料夾
		- `run weather_mcp_server.py` ：要執行的檔案
	這樣 Claude 就能知道要執行哪個 Python MCP server。
- **`env`**  
	傳遞給 MCP server 的環境變數。  
	在這裡我們需要把 `ACCUWEATHER_API_KEY` 提供給伺服器，否則工具無法呼叫 API。

這樣設定好之後，Claude Desktop 啟動時會自動依照這個配置去 **開啟一個 MCP Server** ，讓 Claude 在需要時呼叫裡面的工具（例如 `get_weather` ）。

重新啟動 Claude Desktop，直接輸入：

```markdown
維也納的天氣如何？
```

Claude 會自動呼叫 `get_weather(city="維也納")` ，並回覆最新的即時天氣。

---

### 執行結果

![MCP Claude Desktop 執行結果](https://ithelp.ithome.com.tw/upload/images/20251002/20165544m4uAJuEaig.png)  
*圖：Claude Desktop 透過 MCP Tool 呼叫 get\_weather(city="維也納")，並自動將 AccuWeather API 回傳的數據整理成自然語言回答。*

在這個執行結果中，我們可以看到：

1. 使用者輸入「維也納的天氣如何？」。
2. **Claude Desktop 透過 MCP 協議** ，自動呼叫 `get_weather` 工具，並傳入參數 `city="維也納"` 。
3. MCP Server 負責與 AccuWeather API 溝通，回傳即時天氣的原始資料（包含溫度、體感、風速、濕度等）。
4. Claude 再將這些結構化數據整理，生成清楚的自然語言回答，補充了體感描述與建議。

這樣的設計讓工具不只是一段程式碼，而是成為 **可跨應用共享的服務** 。不管是在 Claude Desktop、LangChain，甚至 VSCode 插件裡，都能以相同方式呼叫並取得結果。

---

## 小結

今天我們看到 **MCP (Model Context Protocol)** 的價值：

- 工具不再綁定在某個框架，而是獨立成服務
- 一次實作，能在 **Claude Desktop、LangChain 等不同應用共用**
- 讓工具的管理更清晰，也更容易擴充

透過「維也納天氣查詢」這個例子，我們看到 MCP 如何讓工具從「框架內的 Tools」升級成「跨應用的服務」。

明天，我們將進一步把 MCP 與 **LangChain** 結合，示範如何在 Agent 流程中直接呼叫 MCP 工具 —— 讓工具不只是獨立的服務，而能真正融入 **跨平台、多工具的智慧助理** 。

---

![布拉格查理大橋](https://ithelp.ithome.com.tw/upload/images/20251003/201655444kNmCgqqc9.jpg)  
*圖：布拉格查理大橋（Charles Bridge）上的鍍金浮雕與祈願鐵欄。人們在此掛上象徵心願的鎖頭，將不同的故事匯聚於同一座橋。這正如 MCP 的角色 —— 為各種工具建立共通的「橋樑」，讓它們能跨越框架與系統，讓不同的系統都能連結其上，共同運作，展現跨越邊界的力量。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10391965)

[

Day 17｜行動力升級：LangChain 讓 Tool Use 接上真實世界

](https://ithelp.ithome.com.tw/articles/10391965)[下一篇](https://ithelp.ithome.com.tw/articles/10392878)

[

Day 19｜工具跨框架共享：LangChain × MCP 讓 Tool Use 成為可攜服務

](https://ithelp.ithome.com.tw/articles/10392878)

系列文

[踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友](https://ithelp.ithome.com.tw/users/20165544/ironman/8801) 共 30 篇

目錄

1. 26
	[Day 26｜AI 的守護靈獸：Guardrails 讓 Agent 劃出安全邊界](https://ithelp.ithome.com.tw/articles/10395588)
2. 27
	[Day 27｜讓 AI 暫停思考：Human-in-the-Loop 讓 Agent 與人類成為協作夥伴](https://ithelp.ithome.com.tw/articles/10396238)
3. 28
	[Day 28｜讓 AI 用流程思考：n8n 讓 Agent 以工作流驅動協作](https://ithelp.ithome.com.tw/articles/10396549)
4. 29
	[Day 29｜旅途最後一哩：人機協作讓 Agent 成為並肩共創的隊友，不再獨自升級！](https://ithelp.ithome.com.tw/articles/10396972)
5. 30
	[Day 30｜旅途的終點，也是下一個起點：回顧 30 天的 Agentic AI 探索之旅](https://ithelp.ithome.com.tw/articles/10397208)
[完整目錄](https://ithelp.ithome.com.tw/users/20165544/ironman/8801)

[![.](https://itadstatic.ithome.com.tw/B9/1780653847_6a229f1744d1f.jpg)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9001)