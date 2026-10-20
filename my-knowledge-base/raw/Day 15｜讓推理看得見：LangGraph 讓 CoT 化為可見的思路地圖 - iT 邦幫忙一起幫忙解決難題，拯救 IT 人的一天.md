---
title: "Day 15｜讓推理看得見：LangGraph 讓 CoT 化為可見的思路地圖 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10390982"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言 在 基礎篇 Day 4 (CoT) 我們曾介紹過 Chain-of-Thought (CoT) —— 讓 LLM 把思考過程逐步寫出來，而不是直接給結論。這樣做的好處是：答案更透明、過程能檢驗、..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 15

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 15 篇

## Day 15｜讓推理看得見：LangGraph 讓 CoT 化為可見的思路地圖

- 分享至

## 前言

在 [基礎篇 Day 4 (CoT)](https://ithelp.ithome.com.tw/articles/10382734) 我們曾介紹過 **Chain-of-Thought (CoT)** —— 讓 LLM 把思考過程逐步寫出來，而不是直接給結論。  
這樣做的好處是：答案更透明、過程能檢驗、錯誤更容易被發現。

這次我們要把這個概念搬到 **LangChain / LangGraph** ，讓 CoT 不只是文字，而是結構化的推理流程。  
透過 LangGraph，我們能清楚看到模型「一步一步」怎麼推到最終結論，就像看著一張推理地圖。

---

## 為什麼要用 LangGraph 來做 CoT？

在 LangChain v1.0， **Agents 已經能自動處理工具呼叫與輸出** ，多數應用已經足夠。  
但如果我們希望：

- 讓 **每個推理步驟顯性化** ，而不是一行文字帶過
- 可以追蹤「模型是怎麼走到答案的」
- 在過程中 **插入輔助查詢（例如天氣、路線）** 來支撐判斷

這時候， **LangGraph 的狀態機** 就特別有價值。  
它能讓「逐步推理」不只是模型的內心戲，而是一個可見、可檢查的推理流程。

---

## Demo：維也納午後行程決策

### 任務需求

- 早上參觀聖史蒂芬大教堂
- 中午在納許市場用餐
- 下午安排景點：
	- 若下雨 → 室內的「藝術史博物館」
		- 若天氣好 → 戶外的「美泉宮花園」
- 並檢查從納許市場出發是否順路

這是一個典型需要 **逐步判斷 → 查詢資訊 → 整合結論** 的情境，非常適合用來展示 CoT。

---

### 程式實作

```shell
pip install -q --pre -U langchain
pip install -q --pre -U langgraph
pip install -q -U langchain-google-genai
```
```python
import os
from langchain_core.tools import tool
from langchain.chat_models import init_chat_model
from langchain_core.messages import SystemMessage, HumanMessage, BaseMessage, ToolCall
from langgraph.graph import add_messages
from langgraph.func import entrypoint, task

# =============== Step 0: 初始化模型（Gemini） ===============
llm = init_chat_model(
    "google_genai:gemini-2.5-flash",
    temperature=0,
    api_key=os.environ["GOOGLE_API_KEY"]
)

# =============== Step 1: 定義工具：查天氣、查距離（順路） ===============
@tool
def get_weather(location: str, period: str) -> str:
    """查詢指定地點在某時段的天氣摘要"""
    if "維也納" in location and "下午" in period:
        return "維也納下午：局部陣雨，建議改室內。"
    return "晴時多雲"

@tool
def check_distance(start: str, end: str) -> str:
    """查詢兩個景點是否順路"""
    if start == "納許市場" and end == "藝術史博物館":
        return "步行 15 分鐘，路線合理。"
    if start == "納許市場" and end == "美泉宮花園":
        return "電車約 12 分鐘，路線合理。"
    return "需轉乘，不太順路。"

tools = [get_weather, check_distance]
tools_by_name = {t.name: t for t in tools}
llm_with_tools = llm.bind_tools(tools)

# =============== Step 2: One-shot CoT 提示 ===============
SYSTEM_PROMPT = """
你是一位維也納旅行助理，請以「逐步思考 → 查詢資訊 → 最終結論」的格式回答。
"""

# =============== Step 3: LLM 節點（推理） ===============
@task
def call_llm(messages: list[BaseMessage]):
    resp = llm_with_tools.invoke([SystemMessage(content=SYSTEM_PROMPT)] + messages)
    print("\n[LLM 推理輸出]")
    print(resp.pretty_repr())  # 顯示推理步驟與可能的 tool call
    return resp

# =============== Step 4: 工具節點（執行查詢） ===============
@task
def call_tool(tool_call: ToolCall):
    tool = tools_by_name[tool_call["name"]]
    print(f"\n[執行查詢] {tool_call['name']} with args {tool_call['args']}")
    result = tool.invoke(tool_call)
    print(f"[查詢結果] {result.content}")
    return result

# =============== Step 5: Orchestrator（一次推理即可） ===============
@entrypoint()
def cot_agent(messages: list[BaseMessage]):
    llm_response = call_llm(messages).result()
    while llm_response.tool_calls:
        tool_results = [call_tool(tc).result() for tc in llm_response.tool_calls]
        messages = add_messages(messages, [llm_response, *tool_results])
        llm_response = call_llm(messages).result()
    return add_messages(messages, llm_response)

# =============== 測試 ===============
question = (
    "早上參觀聖史蒂芬大教堂，中午在納許市場用餐；"
    "請依下午天氣決定：若下雨改去藝術史博物館，否則去美泉宮花園。"
    "並確認從納許市場出發是否順路，請用 Step 說明。"
)

final_messages = cot_agent.invoke([HumanMessage(content=question)])

print("\n[最終答案]")
print(final_messages[-1].content)
```

---

### 執行結果

  
*圖：LangGraph 實作的 One-shot CoT 推理過程。模型逐步展開推理，先確認天氣（藍框），再查詢路程（藍框），最後整合為最終結論（紅框），建議下午行程改為「藝術史博物館」。*

在這個執行過程中，我們可以觀察到：

1. **逐步思考** （Step by Step）  
	模型不是直接回答，而是清楚寫出思路：
	- Step 1：先查詢下午天氣
		- Step 2：依據天氣判斷行程（下雨 → 室內；晴天 → 戶外）
		- Step 3：檢查路線是否順路
2. **工具查詢的介入**  
	模型主動呼叫 `get_weather` 與 `check_distance` 工具，獲得即時資訊，而不是憑記憶假設。
3. **透明的決策過程**  
	每一個步驟都能追蹤與檢查：天氣 → 景點 → 路程 → 結論，讓推理邏輯不再是「黑盒子」。
4. **最終結論收斂**  
	在完整步驟後，LLM 清楚給出結論：「由於維也納下午有局部陣雨，因此改去藝術史博物館，且從納許市場出發順路，步行約 15 分鐘。」

---

### 用到的 LangGraph 元件

這段程式背後，其實是用 LangGraph 搭建了一個「迷你狀態機」。幾個重點元件如下：

### 1\. @task —— 定義節點（Node）

在 LangGraph 裡，每個 **節點** 都代表一段邏輯。  
我們用 `@task` 把函式標註起來，讓它能在圖中被執行：

- `call_llm` ：負責呼叫模型，輸出推理步驟或工具需求
- `call_tool` ：負責執行工具，回傳查詢結果

### 2\. @entrypoint —— 定義流程的入口

整個推理流程的「起點」就是 `cot_agent` 。  
它就像 **狀態機的 entrypoint** ，負責統籌流程：

1. 先呼叫 LLM
2. 若有工具需求 → 執行工具 → 把結果補回訊息
3. 再讓 LLM 收斂出最終答案

### 3\. add\_messages —— 狀態共享（State）

LangGraph 的核心是 **狀態（State）** 。  
我們用 `add_messages` 把 LLM 回覆與工具結果累積起來，當作新的上下文傳到下一輪。  
這樣，推理過程中的「每一步」都會被保留下來。

![LangGraph 中各元件的結構關係](https://ithelp.ithome.com.tw/upload/images/20250930/201655440t76yJmWwt.png)  
*圖：LangGraph 中各元件的結構關係。@task 節點負責執行單一步驟，@entrypoint 是整體流程的入口，而 add\_messages 則負責狀態更新，讓推理路徑能不斷累積與收斂。*

---

## 小結

這次的練習展示了：

- **CoT 的價值** ：逐步拆解推理，讓答案透明、可檢查。
- **LangGraph 的助力** ：
	- `@task` 把邏輯拆成節點
		- `@entrypoint` 定義流程起點
		- `add_messages` 維持狀態流轉  
		讓「思考的路徑」被完整記錄。
- **更好的體驗** ：不是否定模型的直覺，而是把直覺變得清楚，讓使用者安心。

在基礎篇，我們已經看到 CoT 如何讓答案更可靠。

而這次透過 LangGraph，我們更進一步，讓推理過程不只是文字，而是 **可觀察、可追蹤的流程** 。

接下來，我們會繼續探索如何把這樣的能力擴展到 **檢索（RAG）、反思（Reflection）** 等核心能力，讓 Agent 的推理更全面、更穩健。

---

![下奧地利](https://ithelp.ithome.com.tw/upload/images/20250930/20165544g4zWEh0nkx.jpg)  
*圖：下奧地利（Niederösterreich），從維也納往布拉格的列車上望出去，眼前是一望無際的草原與遠方的風車。這樣的景色既遼闊又清晰，就像透過 LangGraph 來實作 Chain-of-Thought：讓推理的每一步都能展開在眼前，不再只是隱晦的內心計算，而是一條清楚可追溯的思路軌跡。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10389376)

[

Day 14｜搭建地圖與指南針：LangChain × LangGraph（最新 v1.0 版）打造 Agent 架構骨架

](https://ithelp.ithome.com.tw/articles/10389376)[下一篇](https://ithelp.ithome.com.tw/articles/10391500)

[

Day 16｜讓答案有憑有據：LangChain 讓 RAG 化為可擴充的標準管線

](https://ithelp.ithome.com.tw/articles/10391500)

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