---
title: "Day 29｜旅途最後一哩：人機協作讓 Agent 成為並肩共創的隊友，不再獨自升級！ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10396972"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言 走到第 29 天，這趟旅程也即將進到尾聲。 回顧過去的篇章，我們從零開始，一步步探索 Agentic AI 的核心能力，讓 LLM 學會： 推理（Reasoning） 檢索（Retrieval）..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 29

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 29 篇

## Day 29｜旅途最後一哩：人機協作讓 Agent 成為並肩共創的隊友，不再獨自升級！

- 分享至

## 前言

走到第 29 天，這趟旅程也即將進到尾聲。

回顧過去的篇章，我們從零開始，一步步探索 Agentic AI 的核心能力，讓 LLM 學會：

- 推理（Reasoning）
- 檢索（Retrieval）
- 工具使用（Tool Use）
- 規劃（Planning）
- 記憶（Memory）
- 反思（Reflection）
- 協作（Collaboration）

同時，我們也探討了三個讓 AI「更貼近現實應用」的核心能力：

- **Multimodal（多模態）** ：讓模型能同時理解文字、圖像與聲音，不只閱讀行程，也能分析菜單、辨識景點。
- **Guardrails（安全邊界）** ：建立行為準則與防錯機制，確保模型在生成內容或使用工具時，不偏離倫理與安全範圍。
- **Human-in-the-Loop（人類回饋機制）** ：在人類關鍵決策點介入審核、修改、批准或拒絕，讓 AI 的決策過程納入人類判斷力。

透過 **LangChain / LangGraph / MCP / n8n** 的協同整合，  
我們讓這些能力從概念變成可觀察、可操作的智慧系統。  
如今的 Agent，已不只是回答問題的模型，  
而是真正能與人協作、思考、行動的智慧夥伴。

但故事還沒結束。  
因為最終的關鍵不是 *Agent 與 Agent 之間* 的合作，  
而是——

> **人與 AI 之間的協作。**

---

## 從自動化到增強：人機協作的新範式

今年七月我在維也納參加 **[ACL 研討會（Association for Computational Linguistics）](https://2025.aclweb.org/)** 時，  
還有一場關於人機協作非常精彩的 Tutorial——  
**[《Human-AI Collaboration: How AIs Augment Human Teammates》](https://ithelp.ithome.com.tw/articles/acl25-hai-team.github.io)**  
這場教學完整梳理了目前 Human-AI Collaboration 的研究與實務進展，提出了許多有啟發性的觀點：

> 人機協作的目標，不是讓 AI 取代人，  
> 而是讓 AI **增強人類的能力** （Augmentation），  
> 幫助我們完成更高層次的任務。

這正是人機協作與傳統自動化的根本差異。

- **自動化（Automation）** 追求取代與效率；
- **增強（Augmentation）** 則追求互補與創造力。

若 AI 只追求取代人力，將陷入所謂的 **「Turing Trap」** ：  
人被排除在決策與創造之外，工作品質與價值感反而下降。  
而真正成熟的 AI 系統，應該讓人能「看得見、調得動、學得到」。

這樣的觀點，為我們在旅遊助理案例中實現的「AI × Human Workflow」提供了清晰方向。

---

## 人機協作的三層架構：從互動到基礎

Tutorial 中提出 Wang & Lu (2025) 的 **[三層協作架構（Three-Layer Framework）](https://arxiv.org/abs/2506.11718)** ，  
正與本系列的技術實作高度契合：

| 層級 | 核心任務 | 本系列對應 |
| --- | --- | --- |
| **Interaction Layer（互動層）** | 使用者與 AI 的溝通介面 | Chat、Gmail 審核、可視化工作流 |
| **Process Layer（流程層）** | 協作核心、任務分工、回饋機制 | LangChain / LangGraph Agent 流程 |
| **Infrastructure Layer（基礎層）** | 記憶、個人化、執行模組 | Memory、MCP 工具、n8n 節點 |

這三層構成了現代人機協作系統的「骨架」。

在本系列的 Demo 中：

- 我們以 **n8n** 建立可視化的工作流（互動層），
- 以 **LangChain / LangGraph** 管理多 Agent 推理（流程層），
- 再透過 **Memory 與 MCP 工具** 承接資料與上下文（基礎層）。

這正呼應了 ACL 教學課程所強調的重點：

> 真正有效的人機系統，必須整合「互動、流程、基礎」三個維度，  
> 並在其中保持 **透明性、可控性與學習性** 。

---

## Process Layer：任務分工與共同反思

在協作的核心中，AI 的任務不只是執行，  
而是要讓人 **理解過程、參與決策、驗證結果** 。

這與我們在 **Day 28 的 n8n 工作流** 不謀而合：  
每個節點都清楚標示任務分工，  
而人類能透過 Gmail 審核、意見回饋來調整決策方向。

Tutorial 指出，這種「 **回合式合作設計（Turn-based Collaboration）** 」  
是當代人機協作的核心：

其關鍵特性包括：

- 支援 **動態角色切換** （有時 AI 主導、有時人主導）。
- 促進 **共同反思（Co-reflection）** ，讓系統與人都能成長。
- 強調 **任務委派與再分配** ，AI 可根據回饋重新組織行動策略。

這與我們的 **HITL（Human-in-the-Loop） + Reflection & Memory** 設計對齊。

---

## Demo：讓 Agent 成為你的 AI 隊友

最終，我們把所有 Agent 能力整合為一個循環：  
**行程生成 → 郵件審核 → 反思修正 → 記憶更新 → 再規劃 → 批准後自動上架行事曆** 。

### n8n 流程架構

下圖是完整的人機協作工作流：

  
*圖：AI 與人類共同完成「規劃 → 審核 → 反思 → 修正 → 再規劃」的閉環流程。*

![最終 n8n 工作流示意圖](https://ithelp.ithome.com.tw/upload/images/20251013/20165544l7OXK08tRZ.png)  
*圖：n8n 工作流設計。每個節點皆對應一個角色與任務，流程透明可追蹤。*

在這個流程中：

1. **When Chat Message Received**  
	使用者輸入旅行需求，啟動流程。
2. **Supervisor Agent**  
	呼叫 FastAPI `/plan` 端點，協調 Weather、Planner、Advisor 三位子 Agent，  
	整合天氣、景點與建議。
3. **Markdown → HTML**  
	將輸出內容轉成郵件可讀格式。
4. **Gmail 審核（Human-in-the-loop）**  
	將結果寄給人類審核，等待「Approve」或「Reject」的回覆。
5. **If 節點分流**  
	若批准 → 建立 Google Calendar 事件；  
	若拒絕 → 呼叫 `/reflect` 產生反思。
6. **Reflect Agent**  
	分析使用者回饋，提出修正方向，並透過 Memory 保存反思紀錄。
7. **Loop Over Items**  
	反思內容再回 Supervisor，重新規劃行程，進入新一輪循環。

---

### Multi-Agent 旅遊助理整合成 FastAPI 服務程式實作（片段）

將 [Day 24](https://ithelp.ithome.com.tw/articles/10395086) 的旅遊 Agent 包成 `FastAPI` 服務供 n8n 工作流呼叫使用（以下為節錄示意）

```python
import asyncio
from fastapi import FastAPI
from pydantic import BaseModel
from langchain.chat_models import init_chat_model
from langchain.agents import create_agent
from langchain.tools import tool
from langgraph.checkpoint.memory import InMemorySaver
from langchain_mcp_adapters.client import MultiServerMCPClient
import os
from contextlib import asynccontextmanager

# 輸入模型
class PlanRequest(BaseModel):
    text: str
    reflection: str | None = None
    thread_id: str = "trip"

class ReflectRequest(BaseModel):
    plan: str
    feedback: str
    thread_id: str = "trip"

SUPERVISOR = create_agent(
    model=model,
    tools=[call_weather, call_planner, call_advisor],
    system_prompt="你是旅行統籌助理，整合子 Agent 輸出或反思建議並提供完整建議。",
    checkpointer=MEMORY,
)

# Reflect Agent
REFLECT_AGENT = create_agent(
    model=model,
    tools=[],
    system_prompt="你是反思 Agent，分析使用者回饋並提供修正建議。",
    checkpointer=MEMORY,

# FastAPI 初始化
app = FastAPI(title="Multi-Agent Travel Planner", lifespan=lifespan)

# API endpoint
@app.post("/plan")
async def api_plan(req: PlanRequest):
    if req.reflection:
        req.text += f" 另外，請參考以下反思建議進行調整：{req.reflection}"
    payload = {"messages": [{"role": "user", "content": req.text}]}
    result = await SUPERVISOR.ainvoke(payload, {"configurable": {"thread_id": req.thread_id}})
    return {"plan": result["messages"][-1].content}

@app.post("/reflect")
async def api_reflect(req: ReflectRequest):
    prompt = f"行程安排：{req.plan}。使用者回饋：{req.feedback}。請分析並提供反思建議。"
    payload = {"messages": [{"role": "user", "content": prompt}]}
    result = await REFLECT_AGENT.ainvoke(payload, {"configurable": {"thread_id": req.thread_id}})
    return {"reflection": result["messages"][-1].content}
```

---

### 實際執行結果（逐步）

以下以「我不吃牛肉」為情境，展示 **人機協作 × 反思記憶** 如何循環運作。

#### 1）提出需求（User Input）

使用者於 Chat 發出請求：規劃維也納一日遊，需考量天氣與適合的安排。

![使用者輸入需求](https://ithelp.ithome.com.tw/upload/images/20251013/20165544FiGnLdD2GX.png)

#### 2）第一版行程寄出（含牛肉選項）

*Supervisor 整合 Weather / Planner / Advisor，轉 Markdown→HTML，透過 Gmail 寄出審核信。餐食包含 **Tafelspitz（煮牛肉）** 與 **Goulash（燉牛肉）** 。*

![第一封審核信：含牛肉](https://ithelp.ithome.com.tw/upload/images/20251014/201655448lhSYotYL6.png)

#### 3）人類拒絕並回饋偏好（Human-in-the-loop）

使用者在審核表單勾選 **No** ，並回饋：「我不吃牛肉」。這是人類「導向（steering）」與「控制（control）」的介入點。

![拒絕並回饋：不吃牛肉](https://ithelp.ithome.com.tw/upload/images/20251013/201655440wuUnhB6O1.png)

#### 4）反思 Agent 啟動（Reflection）

Reflect Agent 解析回饋，產生結構化反思：  
— 明確標註含牛肉餐點  
— 提供非牛肉替代  
— 調整文案與提示語句  
反思紀錄寫入 Memory（ **協作記憶** ）。

![模型反思與修正建議](https://ithelp.ithome.com.tw/upload/images/20251013/201655441V4w9g0AM3.png)

#### 5）重新規劃並標示「無牛肉」

Supervisor 接收反思，重產行程；餐食部分改為 **非牛肉選項** ，並在段落標題中 **明顯標示（無牛肉）** 。

![第二封：已改為無牛肉行程](https://ithelp.ithome.com.tw/upload/images/20251014/20165544h5H0YUAzqF.png)

#### 6）人類批准（Approve）

使用者確認調整合理，於審核表單選擇 **Yes** 。

![使用者批准](https://ithelp.ithome.com.tw/upload/images/20251013/20165544rinXaiS85q.png)

#### 7）自動更新 Google Calendar（落地執行）

n8n If 節點分流至 Google Calendar：建立事件並帶入完整 Markdown（內含「不含牛肉」標註）。這是 **Augmentation** 的體現：AI 幫人把決策落地。

![加入 Google Calendar](https://ithelp.ithome.com.tw/upload/images/20251013/201655445jxV4ERFlI.png)

#### 8）再次詢問（長期互動測試）

同一工作階段、同一 thread\_id 下，再次請 AI 安排行程。

![再次提問](https://ithelp.ithome.com.tw/upload/images/20251013/20165544RkSt0lGiYF.png)

#### 9）記憶生效（個人化）

系統無須再次審核偏好， **直接輸出無牛肉版本** 。這證明反思結果已寫入 **Memory** ，形成個人化偏好。

![直接輸出無牛肉版本](https://ithelp.ithome.com.tw/upload/images/20251014/20165544jN6WS4fTXF.png)

#### 10）完整可觀察性（Observability）

n8n 日誌完整呈現每個節點的輸入 / 輸出與時間序，使用者能追蹤「決策為什麼這樣走」。這正是 **Transparency & Alignment** 的要求：可追溯、可驗證、可調整。

![n8n 全歷程 Logs](https://ithelp.ithome.com.tw/upload/images/20251013/201655442RHd7g3p18.png)

這正是 **ACL Tutorial** 所強調的三層協作精神的具體落地：

- **Interaction** ：以 Gmail 表單讓人類可介入並可驗證。
- **Process** ：回合作業（Reject → Reflect → Re-plan）。
- **Infrastructure** ：以 Memory 積累個人化偏好，讓系統越用越懂你。

---

## Evaluation：從 AI 表現到人機整體效能

Tutorial 提出 **Centaur Evaluation（人機混合評估）** ：

- 不再以「AI 是否比人強」為標準，
- 而是看「 **人 + AI 是否超越個別表現** 」。

在這個案例中：  
AI 擅長資料整合與生成，人類提供價值觀與偏好。  
單靠 AI 難以推測「不吃牛」的偏好；單靠人又耗時去比對景點與天氣。  
兩者透過反思循環 **協作** 達成互補，輸出結果既快速又貼心。  
這就是「人機共創」的真正價值所在。

---

## 小結：從第一步，到最後一哩

29 天的旅程，我們從一個簡單的 LLM，  
一路走到具備多 Agent、工具協作、記憶反思與人機共創的智慧系統。

沿途我們看見：

- **Agentic AI** 的核心能力；
- **LangChain** 賦予結構與抽象；
- **LangGraph** 帶來狀態與記憶；
- **MCP** 有效運用外部能力；
- **n8n** 讓整體決策與執行可視化；
- **Multimodal × Guardrails × Human-in-the-Loop**  
	則讓 AI 走出封閉的文字世界，  
	進入能理解世界、尊重安全、與人共創的全新時代。

> **AI 的終點，不是自動化，而是協作化。**  
> 我們要的不是取代，而是共創——  
> 讓 AI 成為真正的隊友，與我們一起思考、一起行動、一起成長。
> 
> 這 29 天的旅程，不只是打造一個智慧系統的過程，  
> 也是一場學習「如何與智慧共處」的旅程。  
> 當 AI 不再只是工具，而成為理解我們、與我們並肩協作的隊友時，  
> 那才是人機協作真正的起點。

---

![維也納美泉宮凱旋門](https://ithelp.ithome.com.tw/upload/images/20251013/20165544S5FDjs7efI.jpg)  
*圖：維也納美泉宮凱旋門（Gloriette, Schönbrunn Palace）。坐落於宮殿花園的最高點，這座巴洛克式建築象徵「勝利與遠望」。初次見到此景色相當震撼。從下方花園望去，整體構圖呈現完美的軸線平衡——就像人機協作系統的設計，AI 在高處統籌資訊、人類則在前線感知世界，兩者相互呼應、成就全景的智慧視野。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10396549)

[

Day 28｜讓 AI 用流程思考：n8n 讓 Agent 以工作流驅動協作

](https://ithelp.ithome.com.tw/articles/10396549)[下一篇](https://ithelp.ithome.com.tw/articles/10397208)

[

Day 30｜旅途的終點，也是下一個起點：回顧 30 天的 Agentic AI 探索之旅

](https://ithelp.ithome.com.tw/articles/10397208)

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