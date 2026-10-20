---
title: LangChain v1.0 × LangGraph 入門：框架選擇與 StateGraph 基礎
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10389765
type: article
status: processed
asset_value: 65
---

LangChain v1.0 定義了 6 個核心模組；LangGraph 在其上加入狀態機（StateGraph）讓複雜多分支流程成為可能。兩者的選擇標準清晰：任務複雜度決定用哪個。

## T（核心論點）

LangChain v1.0 把「讓 LLM 完成任務」的所有必要元件統一成 6 個模組；LangGraph 則把 Agent 流程從線性對話升級為有狀態的圖形結構，讓 CoT、human-in-loop、多 Agent 協作在同一框架中可控。

## B（適用場景）

| 適用 LangChain | 適用 LangGraph |
|----------------|----------------|
| FAQ 問答 / 聊天機器人 | 多分支條件流程 |
| 基礎工具呼叫 | 長跑任務（long-running） |
| 單輪 RAG | Human-in-the-Loop |
| — | 多 Agent 協作 |
| — | Production 級別需求 |

## R（核心元件）

### LangChain v1.0 六大模組

| 模組 | 作用 |
|------|------|
| **Agents** | 決策核心，判斷呼叫哪個工具 |
| **Models** | LLM 介面（`init_chat_model`，統一不同供應商）|
| **Messages** | 對話歷史管理，`content_blocks` 統一輸出格式 |
| **Tools** | `@tool` 裝飾器定義 AI 可呼叫的能力 |
| **Short-term memory** | 對話內記憶（Message history）|
| **Streaming** | 逐字輸出，提升體感速度 |

`content_blocks` 是 v1.0 關鍵變更：文字、工具呼叫、工具結果統一成 Block 陣列，讓 Agent 不再需要分別處理不同輸出格式。

### LangGraph 四個基本概念

```python
from langgraph.graph import StateGraph, MessagesState

# State：圖中所有節點共享的資料結構
class MyState(MessagesState):
    pass  # MessagesState 已內建 messages

# Node：執行函式（輸入 State → 返回 State 更新）
def my_node(state: MyState):
    return {"messages": [...]}

# Edge：節點之間的連接（含條件邊）
# Compile：把圖編譯成可執行 App
graph = StateGraph(MyState)
graph.add_node("my_node", my_node)
graph.set_entry_point("my_node")
app = graph.compile()
```

### 最小 Agent 範例（LangChain）

```python
from langchain.chat_models import init_chat_model
from langchain_core.tools import tool
from langchain.agents import create_agent

@tool
def get_info(query: str) -> str:
    """查詢資訊"""
    return f"Result for: {query}"

llm = init_chat_model("gemini-2.5-flash", model_provider="google_genai")
agent = create_agent(model=llm, tools=[get_info], system_prompt="你是助理")
result = agent.invoke({"messages": [{"role": "user", "content": "..."}]})
```

## C（關鍵行動）

- [ ] 單次查詢 / 簡單工具呼叫 → 直接用 LangChain `create_agent`
- [ ] 多分支 / 長跑 / 需要 checkpointing → 用 LangGraph StateGraph
- [ ] `init_chat_model` 是切換 LLM 供應商的關鍵：改 model 字串即可，其他邏輯不變

## 補充資料｜State / Node / Edge 的語意定義與「多 Node ≠ Multi-Agent」判準（2026-09-09 併入）

來源：〈Day 24｜LangGraph 是什麼？用狀態（State）、節點（Node）、連線（Edge）把代理變成工作流程〉，
Data Machi 30 天系列 Day 24，iThome，2026-09-08，<https://ithelp.ithome.com.tw/articles/10407728>。
（上方「LangGraph 四個基本概念」已給出語法層定義，本節只補語意層的判準，不重複程式碼。）

### 三元素各回答一個問題

| 元素 | 回答的問題 | 意義 |
|------|-----------|------|
| **State** | 現在知道什麼？ | 在 Workflow 裡往下傳的「接力棒」：每個 Node 讀取所需欄位、寫回結果。價值在於**把 Workflow 已經知道的事情，從模糊的 Prompt Context 變成明確的資料欄位**（`sheets_result` / `verification_status` / `retry_count` / `missing_information`…），Node 不必重讀整段對話再猜前一步發生什麼 |
| **Node** | 現在要做什麼？ | 一個責任明確的工作單元。責任切乾淨後**除錯可以定位**：答案數字錯 → 先查 `query_sheets`；資料對但判定錯 → 查 `verify_results`；State 全對只有文字錯 → 才是 `generate_answer` |
| **Edge** | 接下來去哪裡？ | 固定邊定義不可改的順序；**Conditional Edge 的關鍵是「哪些路可以走」由 Workflow 事先定義，「實際走哪條」才由當前 State 決定**——自主性被限制在允許範圍內，而非由模型自由決定下一步 |

由此得到與 Prompt Instruction 的本質差異：「Verification 沒過不能寄信」不再是模型**需要記得**的提醒，而是圖上**沒有這條路**；「最多 Retry 2 次」不再是提醒，而是 State 的 counter ＋ Conditional Edge。控制邏輯從「最好這樣做」變成「只能按允許的流程執行」。

### 兩個容易混淆的邊界

- **State ≠ Memory**：Memory 關心「哪些過去資訊值得保留」，State 關心「這一次執行到這裡手上有什麼」。
  兩者是串接關係——Memory（過去保存）→ Coordinator 判斷是否仍適用、挑選需要的 Context → 載入 State。
  若支援 Human-in-the-loop / Pause-Resume，再把當下 State 持久化。
- **Node ≠ LLM 呼叫**：同一張 Graph 可混用 **LLM Node / Tool Node / Rule Node / Human Approval Node**。
  `route_request` 需語意理解才用 LLM，`query_sheets` 是查詢程式、`calculate_growth_rate` 是 Python/SQL、
  `retry_counter` 只是加一。原則是**需要語意判斷的交給模型，不能出錯的流程與計算交給程式**。

### 🔑 多 Node ≠ Multi-Agent

七個 Node 不等於七個 Agent。**只有當某個 Node 自身同時具備 Goal、Context、Tools、Decision Loop 四者，
並能反覆 Reason → Act → Observe，才接近一個獨立 Agent。** Tool Node 只是執行查詢、Rule Node 只是檢查條件，
即使是 LLM Summary Node，若只做一次摘要也不是 Agent。

- 實用原則：**先用「單一 Coordinator ＋ 多個 Tools」**；只有當 Context 必須隔離、權限差異明顯、
  Tool Selection 已難管理，或角色交接本身就是業務流程時，才考慮 Multi-Agent。
  把 6 個 Tool 拆成 6 個 Agent 只會多出模型呼叫、Prompt、Context 傳遞、State 同步、權限與除錯成本
- 反向也成立：1 個 Coordinator ＋ 5 個 Tool ＋ 10 個 Node ＋ 0 個額外 Agent，仍非常適合 LangGraph。
  因此 LangGraph 應理解為**建立 stateful workflow 的方式**，而非「建立 Multi-Agent 的框架」

### 該用／不該用 LangGraph 的訊號

出現以下任一項才值得上 Graph：流程需要回到前面的 Node、不同 Failure 需要不同處理、Conditional Routing、
Parallel Branch、Human Approval、Pause / Resume、State 開始變複雜、需要清楚 Trace 每條執行路徑。
反之 `Input → SQL → Calculate → Output` 或「Agent ＋ 2 個 Tool 且無分支」用一般程式流程或簡單 Agent Loop
更簡單；把一個單純 RAG 硬拆成十個 Node 只是把簡單系統複雜化——**架構不是越進階越好，剛好能承受問題的複雜度最好**。

### 先畫 Blueprint 再寫程式

導入前先問四個問題：(1) State：整條流程需要保存哪些資訊？(2) Node：哪些工作該拆成明確步驟？
(3) Edge：哪些順序一定不能改？(4) Conditional Edge：什麼條件會讓流程走向不同分支？
四題答完就已是一份 Workflow Blueprint——**先讓人看得懂，再讓程式執行**。

## 相關連結

- [[agent-loop-to-graph-workflow-signals]] — Data Machi 系列 Day 23（前篇）：本篇（Day 24）承接該篇提出的五個「該從 Agent Loop 走向 Graph Workflow」訊號，正式把訊號具體化為 State/Node/Edge 架構
- [[langgraph-coordinator-refactor-seven-nodes]] — 本篇概念的實作版：Coordinator 改造成七 Node 可控 Workflow
- [[graph-engineering-node-edge-state]] — Node/Edge/State 的通用編排框架視角
- [[langgraph-cot-visible]] — LangGraph @task/@entrypoint 實作可視化 CoT
- [[langchain-rag-architectures]] — LangChain 的三種 RAG 架構
- [[langchain-tool-use]] — @tool / ToolNode / create_agent 完整工具呼叫模式
- [[react-multi-mcp-agent]] — ReAct 策略 + Multi-MCP 多工具協同
- [[ai-agent-architecture-patterns]] — Agent 四大架構模式（含 ReAct）
- [[langchain-agent-interview-guide]] — LangChain/A2A/MCP/Transformer 面試全景
