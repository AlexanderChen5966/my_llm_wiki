---
title: "Day 20｜智慧的工具協同：ReAct × Ｍulti-MCP 讓 Agent 展開跨工具對話 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10393533"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言 在 昨天 Day 19，我們示範了 LangChain × MCP 的基礎整合：Agent 能透過 MCP 呼叫單一工具（天氣查詢），並將 API 結果轉換成自然語言回覆。 但真實的應用往往不只..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 20

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 20 篇

## Day 20｜智慧的工具協同：ReAct × Ｍulti-MCP 讓 Agent 展開跨工具對話

- 分享至

## 前言

在 **[昨天 Day 19](https://ithelp.ithome.com.tw/articles/10392878)** ，我們示範了 LangChain × MCP 的基礎整合：  
Agent 能透過 MCP 呼叫單一工具（天氣查詢），並將 API 結果轉換成自然語言回覆。

但真實的應用往往不只需要一個工具。  
例如要規劃旅行，Agent 可能同時需要「天氣資訊」與「景點推薦」：

- 如果天氣好，推薦戶外景點；
- 如果下雨，則建議室內景點。

這就需要 **多工具協同** 。  
今天，我們要讓 Agent 學會「自己思考」該用哪些工具、何時使用，  
並透過 **ReAct 策略（Reason + Act）** ，讓多個 MCP 工具展開「智慧的對話」。

---

## 為什麼需要多工具協同？

單一工具像是一把螺絲起子，能解決特定問題；  
但要完成一個完整專案，往往得靠螺絲起子、鉗子、錘子…協作完成。

這時：

- **MCP** 提供標準化的「 **USB 插槽** 」，讓不同工具能隨插即用；
- **LangChain Agent + ReAct 策略** 則像「 **工地現場的工頭** 」，能根據情境調度合適的工具，  
	先思考、再行動、再整合。

---

### ReAct 策略的 TAO 循環

在昨天的範例中，Agent 是單次呼叫 MCP 工具，屬於「一次推理 → 一次行動」的單輪結構。  
但當進入多工具情境時，我們需要一個更靈活的思考框架——這就是 **ReAct 策略** 。

ReAct 的運作循環可以簡化為三個步驟：

> **T → A → O：Think → Act → Observe**

1. **Think（思考）** ：  
	模型根據使用者問題進行推理，思考需要哪些資訊或工具來回答。
2. **Act（行動）** ：  
	選擇並呼叫合適的工具（例如： `get_weather` 、 `get_attractions` ）。
3. **Observe（觀察）** ：  
	讀取工具的回傳結果，並更新推理，進入下一輪思考。

這樣的循環可以重複多次，直到模型確認掌握足夠資訊，才進行最終回答。

舉例來說，在今天的「旅行規劃助理」中：

1. Think：維也納天氣如何？適合室內還是室外？
2. Act：呼叫 `get_weather("維也納")`
3. Observe：天氣涼爽多雲 → 適合室內活動
4. Think：那有沒有適合的室內景點？
5. Act：呼叫 `get_attractions("維也納")`
6. Observe：取得皇宮、博物館清單 → 整理出推薦回覆

這就是 ReAct 在多工具情境下的「思考－行動－觀察」循環。  
更詳細的運作原理、Prompt 設計與內部狀態管理，可參考 **[基礎篇 Day 8 (ReAct)](https://ithelp.ithome.com.tw/articles/10385814)** 。

---

## Demo：旅行規劃助手（Weather MCP + Attractions MCP）

今天我們要打造一個「旅行規劃助手」：

- 使用 **Weather MCP** 查詢目的地的天氣狀況
- 使用 **Wikivoyage MCP** 查詢當地主要景點資訊
- Agent 透過 **ReAct 策略** 自行判斷先後順序，並整合最終答案

---

### MCP Server：Attractions MCP（以 Wikivoyage API 實作）

繼昨天的天氣 MCP，我們再新增一個 MCP Server，這次改用 **維基導遊 (Wikivoyage)** 作為資料來源。  
這個 API 能直接抓取城市條目的 HTML 內容，我們再透過 BeautifulSoup 解析「主要景點」章節。

```bash
pip install -q httpx beautifulsoup4
```
```python
import httpx
from bs4 import BeautifulSoup
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("attractions-mcp")

WIKIVOYAGE_API = "https://zh.wikivoyage.org/w/api.php"

@mcp.tool()
async def get_attractions(city: str) -> dict:
    """查詢指定城市的熱門景點（使用 Wikivoyage 中文 API）。
    
    Args:
        city: 城市名稱
    Returns:
        景點列表（中文名稱）
    """
    # Step 1. 取得城市的 Wikivoyage 頁面標題
    search_params = {
        "action": "parse",
        "page": city,
        "prop": "text",
        "uselang": "zh-tw",
        "format": "json"
    }

    async with httpx.AsyncClient(timeout=10.0) as client:
        res = await client.get(WIKIVOYAGE_API, params=search_params)
        parsed = res.json()

    html = parsed.get("parse", {}).get("text", {}).get("*", "")
    if not html:
        return {"error": "無法取得頁面內容"}

    # Step 3. 用 BeautifulSoup 抽取景點章節
    soup = BeautifulSoup(html, "html.parser")
    attractions = []

    see_section = soup.find(id="主要景點")
    if see_section:
        for li in see_section.find_all_next("li", limit=20):  # 抓前 20 筆
            # 1) 名稱：span.listing-name
            name_span = li.find("span", class_="listing-name")
            name = name_span.get_text(strip=True) if name_span else li.get_text(strip=True)

            # 2) 說明：蒐集 span.note（時間/價格/簡介等）
            note_texts = [n.get_text(" ", strip=True) for n in li.select("span.note")]
            info = " ".join(note_texts).strip()

            # 3) 後備：若沒有 note，就用整段文字扣掉名稱
            if not info:
                whole = li.get_text(" ", strip=True)
                info = whole.replace(name, "", 1).strip(" ，。；：-— ")

            attractions.append({"name": name, "info": info})

    return {"city": city, "attractions": attractions or ["暫時查不到景點"]}

if __name__ == "__main__":
    mcp.run()
```

這次我們使用 **httpx** 而非傳統的 `requests` ，原因在於：

- 它支援 **async / await** ，能與 LangChain 的非同步架構更好整合。
- 效能更高，可同時請求多個頁面。
- 提供更細緻的逾時與錯誤處理控制。

---

### 在 Claude Desktop 測試 MCP Server

完成 Wikivoyage MCP 後，我們也可以直接在 **Claude Desktop** 中測試這個工具。

開啟設定選單，點選左下角的 **Manage connectors** ，就能看到可用的 MCP 工具。  
將 `attractions` 工具開啟後，Claude 就能直接呼叫這個 MCP Server。

  
*圖：在 Claude Desktop 中啟用 `attractions` MCP 工具後，Claude 可直接呼叫本地 MCP Server。*

接著輸入像「維也納有哪些景點？」這樣的問題，就能看到 Claude 自動觸發 MCP 工具的請求與回傳結果。

![Claude Desktop 呼叫 Wikivoyage MCP 查詢維也納景點](https://ithelp.ithome.com.tw/upload/images/20251005/20165544KUA37HX1aR.png)

*圖：Claude Desktop 呼叫 Wikivoyage MCP 工具查詢維也納景點，MCP Server 回傳景點清單（例如美泉宮、皇宮與史蒂芬大教堂），Claude 將其轉換為自然語言回覆。*

---

### 架構流程

![Agent 流程](https://ithelp.ithome.com.tw/upload/images/20251004/20165544BP8k6aAdxG.png)  
*圖：Agent 先呼叫 Weather MCP 查天氣，再呼叫 Attractions MCP 查景點，最後整合成推薦行程。*

---

### MCP Client 與 Agent 建立

同樣先安裝必要套件與設定環境變數：

```bash
pip install -q --pre -U langchain
pip install -q -U langchain-google-genai
pip install -q langchain-mcp-adapters

export GOOGLE_API_KEY={你的 Google API Key}
export ACCUWEATHER_API_KEY={你的 AccuWeather API Key}
```

接著在 LangChain 建立多工具 Agent：

```python
from langchain_mcp_adapters.client import MultiServerMCPClient
from langchain.chat_models import init_chat_model
from langchain.agents import create_agent
import asyncio
import os

async def run_agent_with_multi_mcp():
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
            },
            "attractions": {
                "transport": "stdio",
                "command": "uv",
                "args": [
                    "--directory",
                    "{MCP 程式路徑}",
                    "run",
                    "attractions_mcp_server.py"
                ]
            }
        }
    )

    # 取得 MCP 工具
    tools = await client.get_tools()
    
    # 從環境變數讀取 Google API key
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
        system_prompt="你是一位旅遊助理，可以根據天氣與景點資料，推薦最合適的維也納旅遊行程。"
    )

    # 與 Agent 對話，詢問維也納的推薦景點
    response = await agent.ainvoke({
        "messages": [{"role": "user", "content": "幫我依今天天氣推薦維也納景點"}]
    })

    print("\n=== 對話歷程 ===")
    for m in response["messages"]:
        role = m.__class__.__name__
        print(f"[{role}] {getattr(m, 'content', '') or getattr(m, 'tool_calls', '')}")

    print("\n=== 模型最終回答 ===")
    print(response["messages"][-1].content)

if __name__ == "__main__":
    asyncio.run(run_agent_with_multi_mcp())
```

---

### 執行結果

執行後，Agent 會自動決定查詢順序並整合回答：

![Agent 多工具整合輸出結果](https://ithelp.ithome.com.tw/upload/images/20251005/201655440YwugBU5G8.png)  
*圖：旅行助理接收「幫我推薦維也納旅遊景點」後，先透過 Weather MCP 查得「多雲涼爽」，再用 Wikivoyage MCP 抽取主要景點。模型依據天氣，推薦皇宮與博物館等室內行程，也建議在多雲氣候下輕鬆散步的戶外選項，展現 ReAct 策略下的多工具協作。*

#### 智慧的多工具對話：從天氣到景點的思考鏈

這段輸出展示了 Agent 如何「先思考、再行動」：

1. **查天氣** —— Weather MCP 回傳「多雲、12.1°C、涼爽無雨」。
2. **查景點** —— Wikivoyage MCP 抽取出維也納主要景點，包括皇宮、美泉宮、藝術史博物館與史蒂芬大教堂。
3. **整合回答** —— 模型根據氣候條件判斷：  
	室內建議 → 皇宮、博物館；  
	戶外建議 → 美泉宮花園與教堂登塔。

這種「思考 → 工具呼叫 → 再思考 → 回覆」的互動過程，就是 **ReAct 策略** 的精髓。  
Agent 不再只是被動查資料，而是真正具備「思考 + 行動 + 整合」的能力，展現出接近人類導遊的智慧。

---

## 小結

今天，我們完成了從「單工具」到「多工具協同」的跨越：

- **多工具 MCP 整合** —— Agent 同時調度 Weather MCP 與 Attractions MCP。
- **ReAct 策略** —— 模型能先思考，再決定是否與哪個工具互動。
- **智慧的對話流程** —— 工具間的輸出輸入形成「協作循環」。

這樣的設計讓 AI Agent 不再只是「回答問題」，而是能根據情境自主規劃、整合多來源資訊。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10392878)

[

Day 19｜工具跨框架共享：LangChain × MCP 讓 Tool Use 成為可攜服務

](https://ithelp.ithome.com.tw/articles/10392878)[下一篇](https://ithelp.ithome.com.tw/articles/10393899)

[

Day 21｜從 ReAct 到 Planning：讓 Agent 學會動態調整維也納行程

](https://ithelp.ithome.com.tw/articles/10393899)

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