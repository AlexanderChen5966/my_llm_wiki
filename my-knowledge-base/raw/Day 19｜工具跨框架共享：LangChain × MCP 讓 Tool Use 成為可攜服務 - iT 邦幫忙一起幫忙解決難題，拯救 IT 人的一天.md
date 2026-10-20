---
title: "Day 19｜工具跨框架共享：LangChain × MCP 讓 Tool Use 成為可攜服務 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10392878"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言 在 昨天 Day 18，我們看到 MCP (Model Context Protocol) 如何把工具抽象成獨立服務，讓同一個工具能被 Claude Desktop、VSCode 插件，甚至不同..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 19

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 19 篇

## Day 19｜工具跨框架共享：LangChain × MCP 讓 Tool Use 成為可攜服務

- 分享至

## 前言

在 **[昨天 Day 18](https://ithelp.ithome.com.tw/articles/10392577)** ，我們看到 **MCP (Model Context Protocol)** 如何把工具抽象成獨立服務，讓同一個工具能被 **Claude Desktop、VSCode 插件，甚至不同 Agent 框架共用** ，大幅提升可攜性與治理彈性。

昨天我們先嘗試了 **Claude Desktop** 的整合，體驗了 MCP 如何讓外部 API 工具以獨立服務的方式接入應用。

今天，我們要把這個概念延伸到 **[LangChain (最新 v1.0 版)](https://docs.langchain.com/oss/python/releases/langchain-v1)** —— 讓 Agent 也能透過 MCP 來呼叫工具，並把結果整理成自然語言回覆。

這一步，就像在不同框架之間搭起一座橋，讓同一個工具可以跨平台使用。

---

## 為什麼要結合 MCP 與 LangChain？

雖然 LangChain 本身已經有完整的工具系統（例如 `@tool` 、 `ToolNode` 等），  
MCP 的價值在於，它讓工具可以 **脫離框架束縛** ，成為獨立的「跨平台服務」。

這樣的設計有幾個好處：

1. **共用性**  
	一個 MCP 工具，可以同時給 Claude、LangChain、VSCode 用，無需重寫。
2. **維護性**  
	工具與 Agent 流程分離（解耦），更新工具程式碼時，Agent 邏輯不需更動。
3. **一致性**  
	不同應用程式呼叫 MCP 工具，拿到的結果一致，避免版本分歧。

更直觀的比喻是：  
**MCP** 就像一個標準化的「 **USB 插槽** 」，把不同工具變成可以隨插即用的裝置；  
**LangChain Agent** 則像「 **電腦** 」，透過插槽與工具互動，依需求來運用它們。  
電腦不需要理解滑鼠或隨身碟的內部設計，只要透過 USB 插槽，就能順利使用。

---

## Demo：LangChain Agent 呼叫 MCP Weather Tool

我們延續昨天的例子 —— **查詢維也納天氣** 。  
不同的是，這次改由 **LangChain Agent** 透過 MCP 工具來完成。

(以下程式寫法皆使用 [LangChain 最新 v1.0 版](https://docs.langchain.com/oss/python/releases/langchain-v1) ，官方預計十月份（這個月）會全面切換到 v1.0 版本)

### 架構流程

  
*圖：使用者提出問題後，LangChain Agent 判斷需要工具，呼叫 MCP Server 的 `get_weather` 。MCP Server 負責轉接至 AccuWeather API，取得天氣資料，再回傳給 Agent，由 Agent 整理成自然語言回覆給使用者。*

整個流程可以分成 **三個層次** ：

1. **使用者層**
	- 輸入自然語言問題（例：「維也納的天氣如何？」）。
2. **LangChain Agent 層**
	- 推理後判斷需要外部工具。
		- 呼叫 MCP 工具（ `get_weather` ）。
		- 拿到 MCP 回傳結果，整理成自然語言回答。
3. **MCP Server 層**
	- MCP 工具 `get_weather` 負責與 **AccuWeather API** 溝通。
		- API 的結構化資料經由 MCP 回傳給 Agent。

透過這樣的分工， **LangChain 專注在推理與對話** ，而 MCP 則把外部 API 包裝成 **跨框架可重用的服務** ，讓工具在不同應用之間保持一致性。

---

### MCP Server（查 AccuWeather 天氣）

要讓 LangChain Agent 能透過 MCP 呼叫工具，前提是要有一個 **MCP Server** 負責提供工具。  
這部分的建立方式與細節，已經在 **[昨天 Day 18 的 Demo：把 AccuWeather 包裝成 MCP 工具](https://ithelp.ithome.com.tw/articles/10392577)** 中完整說明。

簡單來說，MCP Server 的角色是：

1. 把外部 API（例如 AccuWeather）封裝成 MCP 工具（如 `get_weather` ）。
2. 負責與 API 溝通，處理參數、請求與回傳。
3. 對外只暴露「工具介面」，讓不同框架（Claude、LangChain、VSCode 插件）都能共用。

---

### MCP Client 設定與 Agent 建立

在進入程式之前，先安裝必要套件，並設定環境變數：

```bash
# 安裝套件
pip install -q --pre -U langchain
pip install -q -U langchain-google-genai
pip install -q langchain-mcp-adapters

# 設定環境變數
export GOOGLE_API_KEY={你的 Google API Key}
export ACCUWEATHER_API_KEY={你的 AccuWeather API Key}
```

以下程式示範如何將 MCP 工具注入 LangChain Agent：

```python
from langchain_mcp_adapters.client import MultiServerMCPClient
from langchain.chat_models import init_chat_model
from langchain.agents import create_agent
import asyncio
import os

async def run_agent_with_accuweather_mcp():
    # 從環境變數讀取 AccuWeather API Key
    accuweather_api_key = os.getenv("ACCUWEATHER_API_KEY")
    if not accuweather_api_key:
        raise ValueError("請設定 ACCUWEATHER_API_KEY 環境變數")
    
    # 建立 MultiServerMCPClient，並設定 AccuWeather MCP Server
    client = MultiServerMCPClient(
        {
            "accuweather": {
                "transport": "stdio",
                "command": "uv",
                "args": [
                    "--directory",
                    "{MCP 程式路徑}",
                    "run",
                    "weather_mcp_server.py"
                ],
                "env": {"ACCUWEATHER_API_KEY": accuweather_api_key}
            }
        }
    )

    # 取得 MCP 工具
    tools = await client.get_tools()
    
    # 從環境變數讀取 Google API Key
    google_api_key = os.getenv("GOOGLE_API_KEY")
    if not google_api_key:
        raise ValueError("請設定 GOOGLE_API_KEY 環境變數")
    
    # 建立 Gemini-2.5-FLASH LLM
    llm = init_chat_model(
        "gemini-2.5-flash", 
        model_provider="google_genai",
        api_key=google_api_key
    )

    # 建立 Agent，並加入 MCP 工具
    agent = create_agent(
        model=llm,
        tools=tools,
        system_prompt="你是一位天氣助理，可以透過工具查詢城市的即時天氣。"
    )

    # 與 Agent 對話，詢問維也納的天氣
    response = await agent.ainvoke({
        "messages": [{"role": "user", "content": "維也納的天氣如何？"}]
    })

    print("\n=== 對話歷程 ===")
    for m in response["messages"]:
        role = m.__class__.__name__
        print(f"[{role}] {getattr(m, 'content', '') or getattr(m, 'tool_calls', '')}")

    print("\n=== 模型最終回答 ===")
    print(response["messages"][-1].content)

if __name__ == "__main__":
    asyncio.run(run_agent_with_accuweather_mcp())
```

這段程式主要完成以下幾件事：

1. **讀取 API Key**
	- 從環境變數抓取 `ACCUWEATHER_API_KEY` 與 `GOOGLE_API_KEY` ，確保程式能連線到 AccuWeather API 與 Gemini 模型。
2. **建立 MCP Client**
	- 使用 `MultiServerMCPClient` 連線到 AccuWeather MCP Server，並將 `get_weather` 工具註冊進來。
		- MCP Client 的設定裡包含了執行指令（ `uv run main.py` ）、路徑與環境變數。
3. **取得工具清單**
	- 呼叫 `client.get_tools()` ，動態取得 MCP Server 提供的工具（此例為 `get_weather` ）。
4. **建立 LLM**
	- 初始化 Google Gemini-2.5-Flash 作為語言模型，負責推理與生成自然語言回覆。
5. **建立 Agent 並注入 MCP 工具**
	- 透過 `create_agent` 把 LLM 與 MCP 工具組合在一起。
		- Agent 的提示設定為「你是一位天氣助理」，告訴模型使用工具回答天氣相關問題。
6. **執行對話**
	- 使用 `ainvoke` 模擬一次對話，輸入「維也納的天氣如何？」。
		- Agent 會推理並呼叫 `get_weather` ，最後輸出自然語言答案。

---

### 執行結果

![LangChain Agent 呼叫 MCP 工具與最終回覆](https://ithelp.ithome.com.tw/upload/images/20251003/20165544HSPF6sVqag.png) ![LangChain Agent 最終生成回覆](https://ithelp.ithome.com.tw/upload/images/20251003/20165544bMOu0J2YmG.png)

*圖：使用者詢問「維也納的天氣如何？」後，LangChain Agent 判斷需要呼叫 MCP 工具 `get_weather` ，並由 MCP Server 向 AccuWeather API 取得即時天氣資料。接著 Agent 將 JSON 資料整理成自然語言回答，完整呈現氣溫、體感溫度、濕度、能見度、風速與紫外線指數等資訊，回覆給使用者。*

這裡可以看到：

1. **Agent 呼叫 MCP 工具** —— 帶入 `city="維也納"` 的參數。
2. **MCP Server 負責 API 互動** —— 向 AccuWeather API 取回即時天氣資料。
3. **Agent 整理回覆** —— 將結構化數據轉換成自然語言答案。
4. **分工的關鍵價值** ——
	- Agent **不需要理解 AccuWeather API 的細節** ，只要會呼叫 MCP 工具即可。
		- MCP Server **專責處理 API 的請求與回傳** ，保持獨立與可攜性，確保工具能跨應用共用。

---

## 小結

今天的重點是基礎整合：

- **MCP 工具動態載入** —— 工具以獨立服務形式存在
- **LangChain × MCP 基礎串接** —— Agent 能直接呼叫 MCP 工具
- **跨平台可攜性** —— 同一工具，同時能在 Claude 與 LangChain 中使用
- **分工的價值** —— Agent 專心做推理與回覆，MCP Server 專責 API 互動與維護

這是一個重要的轉折點：  
工具不再依賴某一個框架，而是成為真正的 **「共享資源」** 。

明天，我們將進一步挑戰：  
不只是一個 MCP 工具，而是讓 Agent 能 **同時協調多個 MCP 工具** ，並引入 **ReAct 推理策略** —— 讓工具之間不再只是單獨呼叫，而能展開「智慧的對話」。

---

![布拉格克萊門特學院的天文塔](https://ithelp.ithome.com.tw/upload/images/20251003/20165544B30C2YfOo6.jpg)  
*圖：布拉格克萊門特學院（Klementinum）的「天文塔 (Astronomical Tower)」內部，早期以大型日晷與弧形測角儀測量太陽位置來校準時間。這些精密的觀測工具，讓天文學家能準確掌握宇宙規律。正如 MCP 的角色 —— 提供標準化的「工具介面」，讓不同系統能在同一基準下協同運作，維持知識與應用的一致性。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10392577)

[

Day 18｜工具的高效協奏：MCP (Model Context Protocol) 讓 Tool Use 更好調度外部能力

](https://ithelp.ithome.com.tw/articles/10392577)[下一篇](https://ithelp.ithome.com.tw/articles/10393533)

[

Day 20｜智慧的工具協同：ReAct × Ｍulti-MCP 讓 Agent 展開跨工具對話

](https://ithelp.ithome.com.tw/articles/10393533)

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

[![.](https://itadstatic.ithome.com.tw/B9/1779447171_6a10358375783.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2605B9004)