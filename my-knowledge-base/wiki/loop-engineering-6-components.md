---
title: Loop Engineering：迴圈工程完整框架（六大組件 + 架構模式 + 陷阱防護）
date: 2026-06-22
tags: [ai/agent, ai/claude, pkm/workflow]
source: https://www.koc.com.tw/archives/loop-engineering
type: article
status: processed
asset_value: 88
---

Loop Engineering（迴圈工程）是一種**設計讓 AI 自動觸發、而非人工觸發 prompt 的自動化系統**的方法論。核心轉向：從「我來寫 prompt」到「設計一個系統讓 AI 自動跑」。

> Boris Cherny（Anthropic，Claude Code 負責人）：「我已經不再 Prompt Claude 了。我有一些 Loop 在跑，它們負責 Prompt Claude。我的工作，是寫這些 Loop。」
> Addy Osmani（Google）：「Loop engineering = 用系統設計，取代你本人充當 Agent 的 Prompt 操作員。」

三層抽象演進：**Prompt → Harness → Loop**（往上疊，前層不消失）

## T（核心論點）

關鍵指標不是 tokens burned，而是**每個被採納的修改花了多少成本**（cost per accepted change）。Loop 的成敗取決於寫進去的**評分標準和驗收條件**，跟模型強弱關係不大。

**Worker + Verifier 必須分離**：寫的人不能自己打分數。Worker 用 context A，Verifier 用 context B，兩個獨立 API call，不共享歷史——共用歷史的 Verifier 會繼承 Worker 的盲點。

## B（適用場景）

**適用——任務通過三個檢查：**
1. **重複性**：這件事頻繁發生，設計系統的成本可被攤平
2. **可驗收**：「做完了」可以被寫成機器可跑的檢查條件（最常被忽略）
3. **有價值**：產出值得那些 token，有明確的 ROI

**不適用：**
- 一次性任務、無法客觀驗收、或產出價值低於成本的任務
- 尚未有可重複步驟的探索性工作（用 open loop 探索，確認後再建 closed loop）

## R（六大組件詳解）

| 組件 | 用途 | Claude Code 對應 |
|------|------|------|
| **Automations** | 定時/事件觸發，讓 Loop 自己去找活幹 | `/loop`、cron、GitHub Action |
| **Worktree** | 並行 Agent 各自隔離，不踩腳 | `git worktree` / `--worktree` flag |
| **Skills** | 把專案知識外置（SKILL.md），每輪可重用 | `SKILL.md`、`CLAUDE.md` |
| **Connectors** | 連接真實世界（issue tracker、DB、Slack、API）| MCP servers |
| **Sub-agents** | Maker 和 Checker 分離；Writer 和 Reviewer 分離 | `.claude/agents/` |
| **Memory** | 跨 session 持久化進度和規則 | Markdown 清單、AGENTS.md |

### 建立 Loop 的前提條件

1. **可重複性**：任務有清晰、可重複的步驟
2. **可評估性**：有明確的「成功」標準可自動判斷
3. **低風險性**：失敗的代價可接受（或有自動回滾）
4. **成本划算**：設計 loop 的成本 < 手動重複的累積成本

### Worker + Verifier 分離架構

```
LOOP CONTROLLER → GOAL + CONTEXT
                      │
               DISCOVER + PLAN
                      │
               WORKER (context A)
                      │  output
               VERIFIER (context B)  ← 獨立 API call，不共享歷史
                      │  clean judgment
                     GATE
                    /    \
                 PASS    FAIL
                  │        │
                DONE    DIAGNOSE → 提取規則 → 新策略 → WORKER
```

### 5 個 Agent 死因

1. **Context Collapse**：長任務偏離主線 → 解法：拆 sub-loop，每個有自己的 context + verifier
2. **No Self-Correction**：遇錯盲目重試 → 解法：capture error → 分析 root cause → 換策略
3. **No Verifier**：做完不等於做對 → 解法：Worker 和 Verifier 必須獨立 API call
4. **No Guardrails**：亂刪檔、亂花錢 → 解法：RULES.md、budget cap、read-only 原則
5. **No Memory**：重複犯錯 → 解法：失敗時萃取通用規則存檔，下次先載入

### 分級路由（不要把每個任務都用最強模型）

| 任務類型 | 模型層級 |
|---------|---------|
| 資料萃取、格式化、樣板生成 | 便宜（Haiku / MiniMax）|
| 中等 code gen、review | 中階（Sonnet / DeepSeek V4 Flash）|
| 架構決策、困難 bug 診斷、最終驗證 | 最強（Fable 5 / Opus）|

大部分 iteration 便宜，真正該花錢的是**驗證環節**。

### Memory as Rules, Not Logs

失敗時不存 raw log，而是讓模型萃取**一條通用規則**（`RULE: concise principle`），存成可讀檔案，下次 Loop 啟動時先載入。三個月後你會有幾十條規則，比任何 vector memory 機制都實用。

### 開放 vs 封閉迴圈

| 面向 | 開放迴圈 | 封閉迴圈 |
|-----|---------|---------|
| 契約 | 給目標和護欄，路線自己選 | 先畫好路線、每步驟驗收、明確停止條件 |
| 適合 | 探索、未知地形 | 正式環境、可預測成本 |
| 成本 | 不可預測，容易爆 | 可控，越跑越好 |

正式環境預設走封閉迴圈。

### 三道必要護欄（缺一不可）

1. **次數上限**：硬性天花板，防卡住空轉
2. **diff 檢查**：最近幾輪產出不再變化就停止
3. **成本上限**：以 token 或金額計，達到上限就收手

Uber 在四個月燒完整年 AI 預算後，把每位工程師每月上限設在 ~1,500 USD。

### 「理解債」（Comprehension Debt）警示

迴圈跑得越好，repo 裡的程式碼和你真正理解的落差就越大。必須定期親自讀 diff，不然等系統出包時面對的是陌生人留下的爛攤子。

### 生產環境 Loop 三層架構

| 層級 | 代表工具 | 適用場景 |
|------|---------|---------|
| **Terminal harness loops** | Claude Code, Codex | 單人/小團隊，本機，有人在旁 |
| **Platform runtime loops** | LangChain/LangSmith Sandboxes | 生產環境、需要 24/7 不間斷、跨機器 |
| **Editor lightweight loops** | Cursor, Windsurf + cron | 鎖定在 editor 環境，輕量調度 |

LangSmith Sandboxes：硬體隔離的 microVM（每個 agent 有獨立 filesystem/shell/network），支援 durable execution（狀態 checkpoint，可在 crash 後恢復）和 Human-in-the-loop 暫停。

## C（關鍵行動）

- [ ] 盤點目前有哪些重複性工作流，用「重複/可驗收/有價值」三關過濾
- [ ] 起步：建立封閉迴圈，先裝三道護欄（次數/diff/成本），不要跳過
- [ ] Worker 和 Verifier 分開：不能讓寫程式的 agent 自己判斷是否合格
- [ ] 定期讀 diff，不要讓理解債默默累積
- [ ] 參考 [[loop-engineering-implementation-patterns]] 看 Claude Code 具體實作

## 相關連結

- [[loop-engineering-implementation-patterns]] — Claude Code 具體實作（CLAUDE.md 協議 + Stop hook + fixer agent + Mastra Goals）
- [[loop-engineering-playbook-cli-tools]] — Cobus Greyling 實戰 Playbook：Tier A/B/C runtime 選型 + 7 個生產模式 + loop-audit/init/cost CLI（本篇五塊積木的 runtime 落地層）
- [[agent-loop-explained]] — Loop 概念總覽（本系列的入門總覽篇）
- [[claude-code-advanced-workflow]] — Boris Cherny 15 個進階技巧（/loop 排程細節）
- [[claude-code-goal-command]] — /goal 指令：loop 的停止條件設定
- [[skill-md-writing-techniques]] — Skills 組件的撰寫方法
- [[synthesis-agentic-engineering]] — Agentic Engineering 的角色轉型哲學
- [[agentic-coding-expertise-research]] — Anthropic 研究：領域知識比 coding 技能更重要

## 補充資料（來源：〈万字长文谈谈 Loop Engineering〉周末程序猿）

本篇與 Loop Engineering 系列高度重疊（六大構件、七大實戰模式、L0-L3 自主分級、Worker/Verifier 驗證獨立性均已見於本條目與 [[loop-engineering-playbook-cli-tools]]），唯一補充的新角度：

**LangChain 四層 Loop 模型**：該文以 LangChain 的視角把生產環境的 Loop 分成四層抽象（對應本條目「生產環境 Loop 三層架構」的另一種切法，強調從單機 harness 到平台 runtime 的漸進），可與 [[loop-engineering-playbook-cli-tools]] 的 Tier A/B/C runtime 選型交叉參照。其餘構件與模式內容與既有條目一致，此處不重複。

## 補充資料（來源：iThome ithelp〈Loop Engineering 實際上到底要怎麼做？〉hardness1020，2026-07）

該文以工程師視角把 Agent 系統拆成**四層 Loop**（Agent Loop 執行 / Verification Loop 驗證 / Event Loop 觸發 / Improvement Loop 改善），並強調 Worker 不自評、Rubric 固定在 Loop 外、Feedback 是下一輪資料、Budget 由 Harness 強制、L1→L2→L3 漸進自治——這些與本條目及 [[loop-engineering-human-roles]]、[[loop-engineering-playbook-cli-tools]] 已涵蓋內容一致，此處不重複。

**唯一新料——開源學習資源**：作者開源專案 [awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture) 把現代 Agent Harness 拆成 **22 個循序漸進的 Sections**，從最小 Agent Loop 開始，逐步加入 Tool Runtime、Permission、Hooks、Planning、Subagent、Skills、Context Management、Memory、Task System、Scheduling、Multi-Agent Coordination、Evaluation，最後在 Section 21 組合成 Loop Engineering。Sections 1–21 都有可執行的 Python 範例，可比較相鄰章節觀察每層新增了什麼機制——適合已會呼叫 LLM/Tools、但想搞懂如何組成長時運作 Agent System 的人按序學習。
