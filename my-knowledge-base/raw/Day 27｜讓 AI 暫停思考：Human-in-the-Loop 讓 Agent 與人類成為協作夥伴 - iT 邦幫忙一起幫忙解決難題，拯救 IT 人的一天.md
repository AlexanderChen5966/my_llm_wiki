---
title: "Day 27｜讓 AI 暫停思考：Human-in-the-Loop 讓 Agent 與人類成為協作夥伴 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10396238"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言 在 昨天 Day 26，我們替旅遊助理加上了 Guardrails——讓系統懂得在安全邊界內行動，避免出現誤導性或不當的回應。 但現實世界的決策，並非永遠能由模型單方面判斷。有些情境需要「人類判..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 27

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 27 篇

## Day 27｜讓 AI 暫停思考：Human-in-the-Loop 讓 Agent 與人類成為協作夥伴

- 分享至

## 前言

在 **[昨天 Day 26](https://ithelp.ithome.com.tw/articles/10395588)** ，  
我們替旅遊助理加上了 **Guardrails** ——讓系統懂得在安全邊界內行動，  
避免出現誤導性或不當的回應。

但現實世界的決策，並非永遠能由模型單方面判斷。  
有些情境需要「人類判斷力」介入——  
例如是否要預訂高價飯店、修改旅遊路線，  
或處理可能含糊、具風險的任務。  
當模型即將執行關鍵操作時，我們希望能有「人」介入，確認方向是否正確。

這就是 **Human-in-the-Loop（HITL）** 的角色：  
讓模型「在關鍵時刻停下腳步」，  
等人類審核後再繼續執行。  
它讓人類成為 AI 的協作夥伴，  
在關鍵決策點暫停、補充、修正，  
使 AI 不只是自動化，而是讓 **人與 AI 共同協作決策** 。

---

## 為什麼需要 Human-in-the-Loop？

在自動化系統中，「全自動」雖高效，卻也高風險。  
尤其當模型能自主調用工具時，若缺乏人工把關，可能造成不可逆的錯誤。

**Human-in-the-Loop** 解決的正是這一點——負責「引導決策」。  
它在自動化過程中插入「暫停點」，  
讓人類確認、修改或拒絕模型的動作。

在實務應用中，有許多 AI 難以自行判斷的情境：

| 情境類型 | 範例 | 解決方式 |
| --- | --- | --- |
| 偏好變更 | 使用者臨時改口「我不吃牛肉」 | 在工具執行前中斷等待輸入 |
| 關鍵動作 | 模型即將預訂餐廳或修改資料 | 暫停並請人類確認 |
| 模糊判斷 | 不確定的任務類別或倫理界線 | 人類審核後決定是否執行 |

Human-in-the-Loop 讓這些「模糊地帶」能由人類補足，  
在 AI 自主與人工智慧之間，建立安全的中介層。

---

## LangChain v1.0 的 Human-in-the-Loop 機制

在 **LangChain v1.0 (請參考 [官方文件](https://docs.langchain.com/oss/python/langchain/human-in-the-loop))** 中，  
可使用 `HumanInTheLoopMiddleware` 讓 Agent 在執行工具前自動中斷等待人工回應。

它能在模型即將執行某個工具時自動中斷，  
暫停流程並要求人類審核或補充資訊。

這個中斷（interrupt）會被保存到 LangGraph 的 Checkpointer，  
讓整個任務可以安全地「暫停與恢復」。

1. 監聽模型生成的工具呼叫；
2. 比對設定的 `interrupt_on` 規則；
3. 若符合條件，發出中斷（ `interrupt` ）並保存狀態；
4. 等待人類審核後再繼續執行。

| 決策類型 | 行為 | 範例 |
| --- | --- | --- |
| `approve` | 同意執行原動作 | 確認餐廳推薦後繼續 |
| `edit` | 修改動作內容 | 加上「不吃牛肉」偏好 |
| `reject` | 拒絕執行並回覆理由 | 拒絕預訂不合適餐廳 |

---

## Demo：讓人類介入決策的維也納旅遊助理（補充飲食偏好）

延續我們的維也納旅遊助理，  
今天讓它在呼叫 **PlannerAgent** 規劃行程前，  
先停下來詢問使用者是否有飲食偏好。

  
*圖：Human-in-the-Loop 流程。當 Supervisor 呼叫 PlannerAgent 前，HITL Middleware 會檢查是否需人工審核。若條件符合，流程暫停並等待使用者輸入補充資訊，再以修正版重新執行，讓人類與 AI 能夠協作完成任務。*

這樣一來，AI 不再是自顧自地規劃，  
而能主動邀請人類一起參與決策。

### 實作程式（片段）

以下程式與執行結果基於 LangChain v1.0（因旅遊助理的完整程式篇幅較長，故今天聚焦在新增的 Human-in-the-Loop 程式片段，完整程式請參考 [Day 24 的 Demo](https://ithelp.ithome.com.tw/articles/10395086) ）

```python
import asyncio
from langchain.chat_models import init_chat_model
from langchain.agents import create_agent
from langchain.agents.middleware import HumanInTheLoopMiddleware
from langchain.tools import tool
from langgraph.checkpoint.memory import InMemorySaver
from langgraph.types import Command

# 初始化模型
model = init_chat_model("gemini-2.5-flash", model_provider="google_genai")

# 建立 PlannerAgent
planner_agent = create_agent(
    model=model,
    tools=[],
    system_prompt="你是一位旅遊規劃師，擅長設計維也納一日行程，會考慮天氣與美食建議。",
    checkpointer=InMemorySaver()
)

# 封裝成 Tool
@tool("PlannerAgent", description="規劃維也納一日行程，考慮天氣與景點")
async def call_planner(query: str):
    result = await planner_agent.ainvoke(
        {"messages": [{"role": "user", "content": query}]},
        {"configurable": {"thread_id": "1"}}
    )
    print("\n[PlannerAgent] 輸出：")
    print(result["messages"][-1].content)
    return result["messages"][-1].content

# 設定 HITL Middleware
hitl = HumanInTheLoopMiddleware(
    interrupt_on={
        "PlannerAgent": True,  # 啟用人工回饋
    },
    description_prefix="需要人工補充資訊："  # 系統提示
)

# Supervisor 整合多 Agent
supervisor = create_agent(
    model=model,
    tools=[call_planner],
    middleware=[hitl],
    system_prompt=(
        "你是旅行統籌助理。根據使用者需求，自行決定何時呼叫氣象專家、旅遊規劃師與顧問。"
        "最後整合所有子 Agent 結果，輸出完整建議。"
    ),
    checkpointer=InMemorySaver()
)

# === 第一步：AI 嘗試呼叫 PlannerAgent 前被中斷 ===
result = await supervisor.ainvoke(
    {
        "messages": [
            {"role": "user", "content": "幫我規劃維也納一日遊，包含歌劇院與當地美食體驗"}
        ]
    },
    {"configurable": {"thread_id": "day27_demo"}}
)

# === 第二步：等待使用者補充資訊 ===
if "__interrupt__" in result:
    interrupt = result["__interrupt__"][0].value
    print("\n=== HITL 啟動：等待人工補充 ===")
    print("動作請求：", interrupt["action_requests"])

    # 暫停等使用者輸入飲食偏好。Demo 輸入：user_feedback = 我不吃牛肉
    user_feedback = input("請輸入你的飲食偏好（例如：我不吃牛肉）→ ")

    # 將回饋加入原始查詢
    original_query = interrupt["action_requests"][0]["arguments"]["query"]
    edited_query = f"{original_query}。請注意，使用者的飲食偏好是：{user_feedback}"

    # === 第三步：人工修改後繼續執行 ===
    decision = Command(
        resume={
            "decisions": [
                {
                    "type": "edit",
                    "edited_action": {
                        "name": "PlannerAgent",
                        "arguments": {"query": edited_query},
                    },
                }
            ]
        }
    )

    result = await supervisor.ainvoke(
        decision,
        {"configurable": {"thread_id": "day27_demo"}} # 要相同 \`thread_id\`
    )

print("\n=== 最終整合結果 ===")
print(result["messages"][-1].content)
```

這段程式示範如何在 **LangChain v1.0** 中，  
使用 `HumanInTheLoopMiddleware` 為旅遊助理加入「人工審核節點」。

核心做法如下：

1. **中斷點設定（interrupt\_on）**  
	在建立 `supervisor` 時，我們針對 `PlannerAgent` 加上 Human-in-the-Loop 機制，  
	允許人工在執行前「審核」或「補充」輸入。  
	在這個範例中，AI 規劃行程前會詢問使用者是否有飲食偏好，  
	若使用者回覆「我不吃牛肉」，該資訊就會被附加到原本的查詢中。
2. **執行暫停與回覆（interrupt → resume）**  
	當模型觸發中斷（ `__interrupt__` ）時，程式會輸出暫停訊息，  
	並等待人類決策：
	- `approve` ：直接執行原始輸入。
		- `edit` ：修改輸入內容（例如新增「不吃牛肉」）。
		- `reject` ：拒絕執行該動作。  
		修改後的決策透過 `Command(resume={...})` 傳回，  
		模型會以更新後的內容繼續執行。
3. **狀態保存（checkpointer）**  
	為了讓暫停與恢復不會丟失上下文，  
	這裡使用 `InMemorySaver()` 作為暫存的記憶體儲存。  
	若在實際應用中，則可改用 `AsyncPostgresSaver` 等持久化儲存方案，  
	讓 HITL 能跨階段安全運作。

這樣的設計使 AI 具備「暫停→等待→再執行」的能力，  
在人與模型的協作之間建立出一個柔性的中間層。  
它不僅提升決策正確性，也讓使用者能在關鍵時刻介入，  
共同塑造更符合個人化需求的輸出結果。

---

### 執行結果

在這次的互動中，系統在呼叫 PlannerAgent 前暫停執行，等待使用者補充飲食偏好。當輸入「我不吃牛肉」後，模型將回饋整合進查詢內容，重新規劃午餐建議。

![https://ithelp.ithome.com.tw/upload/images/20251012/20165544e7hbGqsX8I.png](https://ithelp.ithome.com.tw/upload/images/20251012/20165544e7hbGqsX8I.png)  
*圖：Human-in-the-Loop（HITL）啟動，系統在呼叫 PlannerAgent 前暫停執行，等待使用者補充飲食偏好。*

最終輸出中，AI 明確標註「炸豬排（Wiener Schnitzel）」的肉類來源，並提供實用德語句「Ist das vom Schwein?（這是豬肉嗎？）」協助現場溝通。

![https://ithelp.ithome.com.tw/upload/images/20251011/20165544ZYMbrLMyuu.png](https://ithelp.ithome.com.tw/upload/images/20251011/20165544ZYMbrLMyuu.png)  
*圖：Human-in-the-Loop（HITL）介入後，AI 根據使用者回饋「我不吃牛肉」重新生成的行程建議。*

這展現出 HITL 的核心價值——讓人類的細膩判斷成為 AI 決策的一部分，  
AI 能主動建議，人類能掌控最後決策。  
使旅遊助理不僅自動化，更貼近真實需求。

---

## 概念重點

Human-in-the-Loop 並非減緩效率，而是 **讓自動化更負責任** 。  
它在「完全自動」與「完全人工」之間取得平衡：

1. **可控的自動化（Controlled Automation）**  
	模型能自主行動，但關鍵決策仍交由人掌握。
2. **可追蹤的流程（Traceable Workflow）**  
	每次中斷與人工決策都被記錄，方便審核與再訓練。
3. **可持續的信任（Human-AI Trust）**  
	HITL 讓 AI 的決策過程透明化，使用者能真正信任結果。

---

## 小結

今天，我們讓旅遊助理不再只是「回答問題」的系統，  
而成為能與人協作的夥伴。

透過 **Human-in-the-Loop (HITL)** ：

- AI 能自動偵測關鍵決策並等待人工確認；
- 人類能即時編輯或拒絕模型行為；
- 系統則在「自動化」與「人性化」之間取得平衡。

這樣的機制，讓多 Agent 系統更接近真實的合作場景：  
AI 的邏輯、人的直覺，共同塑造智慧的決策流程。

---

![維也納國立歌劇院](https://ithelp.ithome.com.tw/upload/images/20251011/2016554490CZo8yNCM.jpg)  
*圖：維也納國立歌劇院（Wiener Staatsoper）的舞台幕後。金碧輝煌的包廂與層層挑高的觀眾席，如同多層次的對話舞台——每一層都有其角色、視角與節奏。Human-in-the-Loop 的精神亦然：在人類與 AI 的協作過程中，模型提供框架與節奏，而人類的回饋則是舞台上不可或缺的指揮手勢，讓整場智慧的演出最終在和諧中落幕。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10395588)

[

Day 26｜AI 的守護靈獸：Guardrails 讓 Agent 劃出安全邊界

](https://ithelp.ithome.com.tw/articles/10395588)[下一篇](https://ithelp.ithome.com.tw/articles/10396549)

[

Day 28｜讓 AI 用流程思考：n8n 讓 Agent 以工作流驅動協作

](https://ithelp.ithome.com.tw/articles/10396549)

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