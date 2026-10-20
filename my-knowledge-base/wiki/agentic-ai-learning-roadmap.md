---
title: AI Agent 7 階段學習地圖：從 LLM 基礎到多 Agent 系統
date: 2026-05-14
tags: [ai/agent, pkm/workflow]
source: https://github.com/WenyuChiou/awesome-agentic-ai-zh
type: reference
status: processed
asset_value: 62
---

awesome-agentic-ai-zh 是一份繁體中文 AI Agent 結構化學習路徑，7 個階段從「LLM 是什麼、token 怎麼算」一路到 multi-agent 編排，每階段有必做動手練習和精選 project。共用基礎（Stage 0-2）後分兩條路徑：Track A（CLI Power User）和 Track B（Agent Builder）。

## T（核心論點）
學 AI Agent 不是看完文章就算學會——每個 stage 的動手練習都是「不動手就學不會」的東西，`starter.py` 是完整解答不是 skeleton，正確做法是自己重寫、卡住才參考。主幹最少 14-19 週，現實需 5-6 個月（每週 5-8 hr）。

## B（適用場景）
**適用：** 有基本 Python 能力、想系統性學習 AI Agent 建構的開發者和自學者
**不適用：** 完全沒有程式基礎的入門者（先補 Stage 0）；只想快速使用現成工具（Track A 就夠）

## R（兩條學習路徑）

### 共用基礎（Stage 0-2）
| Stage | 主題 | 時程 |
|-------|------|------|
| 0 | 基礎準備（Python/CLI/git/API） | 1-2 週 |
| 1 | LLM 入門（token/API/各家比較） | 1 週 |
| 2 | Prompt 設計（System prompt/few-shot/CoT） | 1-2 週 |

### Track A — CLI Power User（想用 CLI 把事情做完）
| Stage | 主題 | 時程 |
|-------|------|------|
| A1 | CLI Agent 入門（6 主流 CLI 比較、安裝） | 1 週 |
| A2 | CLI Workflow（CLAUDE.md、slash command、多步驟） | 1-2 週 |
| A3 | Integration & Production（MCP/CI/cost） | 1-2 週 |

**Track A 總時程：** 3-5 週（含 Stage 0-2 約 6-8 週）

### Track B — Agent Builder（從零打造 Agent）
| Stage | 主題 | 時程 |
|-------|------|------|
| 3 ⭐ | Tool Use & Agent 入門（ReAct、function calling） | 2-3 週 |
| 4 | Agent 框架（LangGraph/AutoGen/CrewAI） | 2-3 週 |
| 5 ⭐⭐ | Claude Code 生態（MCP/Skills/Plugins） | 3-4 週 |
| 6 | Memory & RAG（vector DB/long-term memory） | 2 週 |
| 7 | 進階 Multi-Agent（orchestration/eval/observability） | 2-4 週 |

**Track B 總時程：** 主幹 14-19 週，現實 5-6 個月

### 三層概念演進
```
Prompt Engineering（Stage 2）→ Context Engineering（Stage 3+）→ Harness Engineering（Stage 7）
「怎麼說」           →  「管理模型能看到什麼」  →  「設計 agent 工作系統」
```

## C（關鍵行動）
- [ ] 先決定走哪條路：想**用** CLI → Track A；想**建** Agent → Track B；兩條不互斥
- [ ] 動手練習做法：`mv starter.py starter_reference.py`，看 signature 不看 body，自己重寫
- [ ] 卡住處理流程見 `docs/HOW_TO_USE.md`

## 相關連結
- [[harness-engineering]] — Stage 7 的核心概念：Guides × Sensors 設計系統
- [[claude-code-glossary]] — Stage 0-2 需要的術語基礎
- [[claude-skills-guide]] — Stage 5 Claude Code 生態的核心
- [[agents-md-guide]] — AI-Ready repo 設計，Stage 5 的實踐

## 補充資料

### 等級評估：Agentic Engineering 的 8 個等級

來源：《Agentic Engineering 的 8 個等級》（DeTools 工具翼零，2026-03-20，翻譯自 Bassim Eledath）

這套等級框架提供「評估自身位置」的工具，與學習路徑互補：路徑說「要學什麼」，等級說「我現在在哪裡」。

**8 個等級概覽**

| 等級 | 名稱 | 核心特徵 |
|------|------|---------|
| 1 | Tab Complete | Copilot 式補全，資深工程師優勢 |
| 2 | Agent IDE | Cursor 等 AI-core IDE，跨多文件修改 |
| 3 | Context Engineering | CLAUDE.md / AGENTS.md，每 token 必須「為自己存在而戰」|
| 4 | Compounding Engineering | Plan→Delegate→Assess→**Codify** 循環，讓下一次 session 更好 |
| 5 | MCP and Skills | 能力擴充：存取 DB、API、CI、設計系統，PR Review Skill 自動化 |
| 6 | Harness Engineering | 自動回饋迴圈（backpressure）：測試/lint/pre-commit hooks 讓 Agent 自我修正 |
| 7 | Background Agents | 背景並行 Agent，從「多開分頁」到「工作在我不在時持續進行」|
| 8 | Autonomous Agent Teams | Agent 彼此直接協作（目前仍困難，Level 7 才是現在的槓桿點）|

**關鍵洞察**

- Level 4 是轉折點：**Codify（寫入規則）**這一步創造複利效果——把每次 session 學到的東西固化到 CLAUDE.md，讓每次 session 都比上次更好
- Level 6 核心原則：「**限制大於指令**」——定義邊界比提供清單更有效；「**為吞吐量設計，而非完美**」——允許小型非阻塞錯誤，發布前做最終品質檢查
- **多模型效應**：Level 7 的最佳實踐是不同工作用不同模型（Opus 實作、Gemini 探索、Codex review）；**實作者與審查者必須分離**——同一模型同時實作和評估會產生偏誤

**你現在在哪個等級？為什麼要關心隊友的等級？**

> 假設你是等級 7，在你睡覺時透過背景代理提交了多個優質 PR。如果你的 repo 需要同事審核，而那位同事是等級 2，這會壓制你的產出。把你的團隊整體往上拉，對你來說是符合自身利益的。

### 補充資源：Hello-Agents 中文學習教程（Datawhale 開源）

來源：datawhalechina/hello-agents（GitHub，2025-2026）

**定位：** 系統性的 AI Agent 中文實踐教程，目標是讓學習者從 LLM「使用者」蛻變為智能體系統「建構者」。聚焦 AI Native Agent（真正以 AI 驅動），而非 Dify/Coze/n8n 這類流程驅動的低代碼工具。

**5 部分 16 章內容架構：**

| 部分 | 主要章節 | 核心技術 |
|------|---------|---------|
| 第一部分：基礎 | 第 1-3 章 | 智能體定義、發展史、LLM 基礎（Transformer/提示）|
| 第二部分：建構 | 第 4-7 章 | ReAct/Plan-and-Solve/Reflection、Coze/Dify/n8n、LangGraph/AutoGen、自研框架 |
| 第三部分：高級 | 第 8-12 章 | Memory 與 RAG、上下文工程、MCP/A2A/ANP 協議、Agentic-RL（SFT→GRPO）、性能評估 |
| 第四部分：案例 | 第 13-15 章 | 智能旅行助手、自動化深度研究 Agent、賽博小鎮 |
| 第五部分：畢業 | 第 16 章 | 多 Agent 應用畢業設計 |

**與本學習路徑的對應：**
- 第 4 章（ReAct/Reflection）→ Track B Stage 3（Tool Use & Agent 入門）
- 第 6 章（LangGraph/AutoGen）→ Track B Stage 4（Agent 框架）
- 第 9-10 章（上下文工程 + MCP 協議）→ Track B Stage 5（Claude Code 生態）
- 第 8 章（Memory + RAG）→ Track B Stage 6（Memory & RAG）
- 第 13-15 章（綜合案例）→ Track B Stage 7（進階 Multi-Agent）

**社區精選補充（Extra-Chapter）值得關注：**
- Extra05：Agent Skills 與 MCP 對比解讀
- Extra08：如何寫出好的 Skill
- Extra09：Agent 應用開發踩坑與經驗
- Extra10：Agent Self-Evolution 智能體自進化四類閉環

### 工具選擇：5 大 Agentic Coding 工具比較（2026）

來源：《Agentic Coding 完全指南》（WenHao Yu，2026-02-14）

| 工具 | 主要模型 | 月費起價 | 特色 |
|------|---------|---------|------|
| **Cursor** | Claude、GPT、Gemini | 20 美元/月 | 多模型彈性切換，完整 IDE |
| **Antigravity** (Google) | Gemini 3 Pro、Claude | 免費（Preview）| Google 生態整合 |
| **GitHub Copilot** | Anthropic、OpenAI、Google | 免費起 | VS Code + GitHub 最無痛 |
| **Claude Code** | Claude Opus/Sonnet | 20 美元/月 | Anthropic 自家，企業選 Claude 比率 2.3x |
| **Codex CLI** | GPT 系列 | 20 美元/月 | OpenAI 生態，開源 CLI |

**選擇原則**：先選模型，再選工具。重度使用 Anthropic 模型 → Claude Code 或 Cursor（都支援 Claude）。

**Vibe Coding vs Agentic Coding 的適用階段**（產品成熟度框架）：

| 產品階段 | 適用方法 | 類比 |
|---------|---------|------|
| Demo / POC | Vibe Coding | 3D printer mockup |
| MVP | Vibe → Agentic 過渡 | A/B sample |
| Production | Agentic Coding | PVT 量產驗證 |
| Scale | Agentic Coding | MP 正式量產 |

> 經驗法則：**如果這個東西壞了會有人打電話給你，用 Agentic Coding。**
