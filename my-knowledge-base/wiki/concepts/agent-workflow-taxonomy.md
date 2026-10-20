---
title: Agent Workflow 分類地圖：六大類型與選型決策
date: 2026-06-22
tags: [ai/agent, dev/architecture]
type: concept-synthesis
sources:
  - [[synthesis-agent-framework-philosophy]]
  - [[synthesis-ai-dev-workflow-frameworks]]
  - [[synthesis-practitioner-skills-design]]
status: auto-generated
---

## 概念定義

「Agent 開發框架」常被當成單一光譜討論，但實際上知識庫中的工具分屬**六個不同抽象層**：有的控制執行流程、有的只是溝通協定、有的管的是工程紀律。把它們攤在同一張表上才看得出——多數情況不是「選哪一個」，而是**疊加哪幾層**（協定 + 框架 + Skills 常常同時用）。

這篇是導航地圖，不重複各條目內容；每格連到既有 wiki 條目。

## 六大類型總表

| 類型 | 控制的是什麼 | 代表工具 | 知識庫條目 |
|------|------------|---------|-----------|
| **① LLM 原生框架** | 執行流程（單/多步驟、狀態） | LangChain / LangGraph / CrewAI / AutoGen / Mastra | [[langchain-langgraph-basics]]、[[langgraph-react-planning-visualization]] |
| **② 協定層** | Agent 之間怎麼溝通（與框架無關） | MCP（裝備工具）/ A2A（呼叫其他 Agent） | [[mcp-fastmcp-server]]、[[a2a-agent-protocol-implementation]] |
| **③ 工作流編排** | 把多 Agent/節點串成可視流程 | n8n / Harness Engineering | [[n8n-multi-agent-workflow]]、[[harness-engineering]] |
| **④ 記憶 / 長效** | Agent 跨 session 記得什麼、如何學 | Hermes / Reflection×Memory / agentmemory | [[hermes-agent]]、[[reflection-memory-learning-loop]]、[[concepts/agent-handoff-memory-patterns]] |
| **⑤ 規格 / 工程流程** | 開工前的規格與 Sprint 紀律 | gstack / Superpowers / ZeroSpec / OpenSpec / SpecKit / BMAD | [[gstack-framework]]、[[superpowers-framework]]、[[ai-sdd-framework-comparison]] |
| **⑥ Skills 工作流** | AI 的**判斷行為**（把隱性工程判斷顯性化） | agent-skills / Addy Osmani / Matt Pocock / Superpowers / GStack | [[synthesis-practitioner-skills-design]]、[[ai-agent-skills-popular-2026]] |

## 補充層：學習式編排（Meta-Orchestration）

六大類型的編排邏輯都是**人寫死的**（流程、角色、誰審誰都固定）。2026 年出現的 **Sakana Fugu** 把這層再往上推一級——**把「編排器本身」變成一個被訓練出來的模型**（learned orchestration）：

- 對外是**單一 OpenAI 相容 API，看起來像一顆 LLM**；內部是動態組裝的多 Agent 系統。
- 由一個 LLM 透過 RL／進化學會「何時委派、派給誰、何時驗證、怎麼合成」——官方稱會發現「人類設計不出來、但更有效率的協作模式」。
- 兩套協調機制：**TRINITY**（Thinker／Worker／Verifier 角色分工）與 **The Conductor**（RL 學自然語言協調策略）。
- 模型池**可熱抽換**，能繞過受限／降級的供應商（緩解 vendor lock-in）。

> 定位：這不是第七類工具，而是**把第③層「工作流編排」從「人為軟編排」升級為「學習式編排」**。代價是失去對流程與 token 帳單的精確控制（黑箱單一 API）。

### 三者對照：人為編排 vs 學習式編排

知識庫裡有三個「多模型分工」案例，差別正好落在「編排邏輯是不是被訓練出來的」：

| 維度 | **Sakana Fugu** | **OpenRouter Fusion**（[[openrouter-fusion]]）| **ai-pair skill**（[[ai-pair-skill]]）|
|------|----------------|------------------|------------------|
| 編排邏輯 | **RL／進化學出來**（學習式）| 寫死：並行答→評審綜合 | 寫死：作者產出→三方審查 |
| 協調者 | 一個**被訓練過的 LLM** | 固定 judge 合成器 | Team Lead（規則腳本＋人）|
| 模型池 | 動態組裝、可熱抽換 | 固定候選 | 固定三角色（GPT／Gemini／Claude）|
| 結合方式 | 學到的非顯而易見協作模式 | 多答案→評審挑／融合 | 共讀單一 REVIEW_FILE 各自審 |
| 對外介面 | **單一 API，像一顆模型** | 一次 API 呼叫 | 一個 skill 流程 |
| 成本控制 | RL 動態決定（黑箱、不可控帳單）| 多模型並行（成本可預估）| **人工最佳化**：分散帳單＋共讀省 token |
| 在本表的位置 | 學習式編排（③ 的進化）| ③ 工作流編排（人為）| ⑥ Skills 工作流（人為軟編排）|

**核心差異**：Fusion／ai-pair 的「誰先誰後、誰審誰、怎麼合成」是人決定的；Fugu 把這些交給 RL。對「節省＋分散 token」這種**需要精確控制帳單**的訴求，ai-pair 的明擺式控制反而比 Fugu 黑箱更可控（見 [[concepts/agent-handoff-memory-patterns]] 並行協作 token 機制）。

## 關鍵區分

這六類最容易被混為一談，但本質不同：

| 對比 | 差別 |
|------|------|
| **框架（①）vs Skills（⑥）** | 框架控制**執行流程**（程式怎麼跑）；Skills 控制**判斷行為**（AI 怎麼想）。兩者可疊加 |
| **框架（①）vs 協定（②）** | 框架是實作；協定是介面標準。MCP/A2A 不綁任何框架，LangGraph 可同時當 A2A Server |
| **編排（③）vs 多 Agent 框架（①）** | n8n 用視覺節點重現 Supervisor 架構，與 LangChain 互補而非競品（見 [[n8n-multi-agent-workflow]]）|
| **規格流程（⑤）vs Skills（⑥）** | 規格框架管「開工前的交接單」；Skills 管「執行中每一步的判斷」。gstack 內部其實是由 Skills 組成 |

## 選型決策樹

來自 [[synthesis-agent-framework-philosophy]] 的核心提問——**先問「Agent 壞掉最壞是什麼」，再選工具**：

```
需要顯式控制流、可 debug、可暫停      → LangGraph（①）
需要多 Agent 角色分工協作            → CrewAI / Supervisor Pattern（①）
需要跨系統工具整合                   → MCP + LangChain（②+①）
需要 Agent 互相呼叫（跨組織/服務）    → A2A 協定（②）
需要 Loop 自動化（CI / 自動 PR）     → Loop Engineering + Mastra（③+①）
要單一 API 自動調度多家 frontier 模型 → Sakana Fugu（學習式編排，緩解 vendor lock-in）
需要 AI 有工程紀律（TDD/審查）        → agent-skills / Superpowers（⑥+⑤）→ 不知從哪選：[[ai-agent-skills-popular-2026]]
需要開工前對齊需求與規格             → ZeroSpec / gstack / SpecKit（⑤）
純 TypeScript 專案                  → Mastra（①）
純 Python 專案                      → LangGraph（①）
```

## 共同含意

1. **多數真實系統是疊加的**：典型組合 = 協定（MCP）+ 框架（LangGraph）+ Skills（工程紀律）+ 規格（開工交接）。不是六選一
2. **抽象層越高越像護城河**：模型（原料）→ 框架（加工廠）→ Skills/規格（工程文化）。越往上越是組織資產、越跨越模型世代（呼應 [[environment-over-model-principle]]）
3. **選型先看失敗模式**：不是看「哪個最強」，而是看「哪一層最容易出錯」——需求錯位選 ⑤/⑥，執行失控選 ① 的顯式控制流，跨 Agent 協作選 ②

## 相關概念

- [[environment-over-model-principle]] — 為什麼環境（這六層的總和）比模型選擇更關鍵
- [[synthesis-agent-framework-philosophy]] — 框架穩定性的四種哲學（① 的深入比較）
- [[synthesis-ai-dev-workflow-frameworks]] — ⑤ 規格/工程流程生態的整合
- [[synthesis-practitioner-skills-design]] — ⑥ Skills 設計的三套件分析
- [[openrouter-fusion]]、[[ai-pair-skill]] — 三者對照中的「人為編排」案例（Fugu 為「學習式編排」對照組）

## 相關連結

- [[concepts/ai-dev-workflow-taxonomy]] — **姊妹導航地圖**：本篇按「工具的抽象層」分類，該篇按「**AI 開發的哪個失敗點**」分類（上下文不完整／無工程紀律／執行迴圈失控／輸出品質無保證／跨 Session 失憶／心智模型錯誤）