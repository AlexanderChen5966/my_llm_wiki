---
title: "Day 21｜從 ReAct 到 Planning：讓 Agent 學會動態調整維也納行程 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10393899"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言 在 昨天 Day 20，我們讓 Agent 能同時協調多個 MCP 工具（天氣查詢與景點推薦），完成一次性推理與整合的「智慧旅遊助理」。 但現實世界的行程不會一成不變。真正的智慧助理，應該能 先..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 21

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 21 篇

## Day 21｜從 ReAct 到 Planning：讓 Agent 學會動態調整維也納行程

- 分享至

## 前言

在 **昨天 Day 20** ，  
我們讓 Agent 能同時協調多個 MCP 工具（天氣查詢與景點推薦），  
完成一次性推理與整合的「智慧旅遊助理」。

但現實世界的行程不會一成不變。  
真正的智慧助理，應該能 **先規劃，再根據狀況靈活調整** ——  
這就是 **Planning × ReAct** 的結合：  
讓模型先定方向，再根據工具回傳結果做出即時反應。

---

## 為什麼要結合 Planning 與 ReAct？

單靠 ReAct，模型會不斷重複「思考 → 行動 → 觀察」的循環，  
雖然能逐步推理，但沒有長遠目標。

單靠 Planning，雖然能一次規劃完整路線，  
卻無法面對突發狀況（例如：天氣變化、景點關閉）。

結合兩者後，Agent 就能像真正的導遊一樣：

> 「先排好行程，再隨時根據現場狀況調整。」

---

## MCP Server：Attractions MCP（加上營業檢查工具）

在昨天的 `Attractions MCP` 基礎上，  
我們加上一個名為 `check_open_status` 的工具，  
可模擬景點在「上午、下午、晚上」是否營業。

```python
# attractions_mcp_server.py
import httpx, random
from bs4 import BeautifulSoup
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("attractions-mcp")
WIKIVOYAGE_API = "https://zh.wikivoyage.org/w/api.php"

@mcp.tool()
async def get_attractions(city: str) -> dict:
    """查詢指定城市的熱門景點（使用 Wikivoyage 中文 API）"""
    params = {
        "action": "parse",
        "page": city,
        "prop": "text",
        "format": "json",
        "uselang": "zh-tw"
    }
    async with httpx.AsyncClient(timeout=10.0) as client:
        res = await client.get(WIKIVOYAGE_API, params=params)
        parsed = res.json()

    html = parsed.get("parse", {}).get("text", {}).get("*", "")
    if not html:
        return {"error": "無法取得頁面內容"}

    soup = BeautifulSoup(html, "html.parser")
    see_section = soup.find(id="主要景點")
    attractions = []

    if see_section:
        for li in see_section.find_all_next("li", limit=10):
            name_span = li.find("span", class_="listing-name")
            name = name_span.get_text(strip=True) if name_span else li.get_text(strip=True)
            note_texts = [n.get_text(" ", strip=True) for n in li.select("span.note")]
            info = " ".join(note_texts).strip()
            if not info:
                whole = li.get_text(" ", strip=True)
                info = whole.replace(name, "", 1).strip(" ，。；：-— ")
            attractions.append({"name": name, "info": info})

    return {"city": city, "attractions": attractions or ["暫時查不到景點"]}

@mcp.tool()
def check_open_status(place: str, period: str) -> dict:
    """檢查景點在上午、下午或晚上是否營業（模擬用）"""
    open_status = random.choice([True, False])
    return {
        "place": place,
        "period": period,
        "open": open_status,
        "note": "營業中" if open_status else "暫時關閉（維修或週休）"
    }

if __name__ == "__main__":
    mcp.run()
```

---

## LangChain Agent：計畫＋即時調整

使用最新的 **LangChain v1.0 alpha** （以 `create_agent()` 為主）：

```python
from langchain.chat_models import init_chat_model
from langchain.agents import create_agent
from langchain_mcp_adapters.client import MultiServerMCPClient
import asyncio, os

async def run_trip_planner():
    client = MultiServerMCPClient(
        {
            "weather": {
                "transport": "stdio",
                "command": "uv",
                "args": ["--directory", "./weather", "run", "weather_mcp_server.py"],
                "env": {"ACCUWEATHER_API_KEY": os.getenv("ACCUWEATHER_API_KEY")}
            },
            "attractions": {
                "transport": "stdio",
                "command": "uv",
                "args": ["--directory", "./attractions", "run", "attractions_mcp_server.py"]
            }
        }
    )
    tools = await client.get_tools()

    llm = init_chat_model(
        "gemini-2.5-flash",
        model_provider="google_genai",
        api_key=os.getenv("GOOGLE_API_KEY")
    )

    agent = create_agent(
        model=llm,
        tools=tools,
        system_prompt=(
            "你是一位智慧旅遊助理，要先規劃維也納一日行程（早上、下午、晚上）。"
            "每個時段都需檢查景點是否營業（使用 check_open_status 工具）。"
            "若景點關閉，請使用 get_attractions 重新挑選並更新行程。"
            "請在過程中輸出使用的工具與輸入參數。"
        )
    )

    response = await agent.ainvoke({
        "messages": [{"role": "user", "content": "幫我規劃今天的維也納一日遊"}]
    })

    print("\n=== Agent 推理與規劃歷程 ===")
    for m in response["messages"]:
        if hasattr(m, "tool_calls") and m.tool_calls:
            print(f"🧰 {m.tool_calls}")
        else:
            print(f"💭 {getattr(m, 'content', '')}")

    print("\n=== 最終建議 ===")
    print(response["messages"][-1].content)

if __name__ == "__main__":
    asyncio.run(run_trip_planner())
```

---

### 範例輸出

```markdown
先安排上午：美泉宮、下午：皇宮、晚上：音樂會。 
[TOOL: check_open_status, INPUT: {'place': '美泉宮', 'period': '上午'}]
 美泉宮暫時關閉，重新查找其他景點。
 [TOOL: get_attractions, INPUT: {'city': '維也納'}]
 改為上午參觀阿爾貝蒂娜博物館。
 [TOOL: check_open_status, INPUT: {'place': '皇宮', 'period': '下午'}]
 皇宮營業中。
 [TOOL: check_open_status, INPUT: {'place': '音樂會', 'period': '晚上'}]
 音樂會暫停演出，改建議去史蒂芬大教堂登塔觀夜景。

 最終建議行程：
- 上午：阿爾貝蒂娜博物館  
- 下午：皇宮  
- 晚上：史蒂芬大教堂登塔
```

---

## 小結

今天，我們讓 Agent 從「單次多工具整合」進化為「動態規劃 × 智慧調整」：

- **多工具協同** ：Weather + Attractions + 營業查詢
- **Planning × ReAct** ：先全局規劃，再逐步調整
- **動態決策能力** ：根據工具結果即時修正

這樣的模式，是 Agent 從「反應型」邁向「主動型」智慧的關鍵一步。

---

下一篇（Day 22），我們將讓這套推理流程更直觀、可追蹤：  
透過 **LangGraph** 將整個規劃與調整過程「可視化」，  
讓 Agent 的思考過程不再是黑箱，而成為可觀察、可優化的決策圖譜。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10393533)

[

Day 20｜智慧的工具協同：ReAct × Ｍulti-MCP 讓 Agent 展開跨工具對話

](https://ithelp.ithome.com.tw/articles/10393533)[下一篇](https://ithelp.ithome.com.tw/articles/10394337)

[

Day 22｜讓思考具象化：LangGraph 讓 ReAct × Planning 的決策流程一目了然

](https://ithelp.ithome.com.tw/articles/10394337)

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