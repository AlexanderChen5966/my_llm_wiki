---
title: Agentic Engineering 心智模型：品味、判斷力與 Bounded Context
date: 2026-05-19
tags: [ai/agent, ai/workflow]
source: https://www.bnext.com.tw/article/90837/from-vibe-coding-to-agentic-engineering-andrej-karpathy-s-software-3-0-ghosts-and-new-10x-talent
type: article
status: processed
asset_value: 60
---

Agentic Engineering 是從 Vibe Coding 進化而來的工程哲學：Vibe Coding 把「能寫軟體的樓地板」墊高，Agentic Engineering 則守住「品質天花板」並讓速度提上來。核心競爭力不在工具熟練度，而在 Bounded Context 定義能力、問題拆解品味，以及對 AI 輸出的有見識判斷。

## T（核心論點）

**Karpathy 的觀點（Sequoia AI Ascent 2026）**：
- LLM 不是動物（吼它沒用），而是「鬼魂」——統計召喚出的精靈，能力呈鋸齒狀分布，某些峰值極高，某些低谷匪夷所思。能駕馭鬼魂才是新的工程美學
- 真正擅長代理工程的人，加速幅度「峰值遠超過 10 倍」——十倍人才的分水嶺劃在理解而非思考
- 金句：「You can outsource your thinking, but you can't outsource your understanding.」

**YOLO LAB 的觀點（3 大工程邏輯反轉）**：
- 狀態反轉：Context Engineering 是核心任務，工程師不再管狀態，而是策劃「模型下次該看到什麼」
- 架構反轉：程式碼生成成本趨近零，但錯誤的架構決策代價依然高昂；AI 時代最致命的技術債是任由 Agent 決定系統邊界
- 邊界反轉：傳統工程追求確定性，Agentic Engineering 擁抱機率並建立外部驗證治理層（Harness）

## B（適用場景）
**適用：** 從事 AI 輔助開發、思考自身工程師價值定位、評估 AI 帶來的人才市場變化
**不適用：** 需要具體操作步驟的情況（見 [[agentic-ai-learning-roadmap]] 和 [[spec-driven-ai-workflow]]）

## R（操作步驟）

### Software 3.0 範式理解
- Software 1.0：明確規則程式碼
- Software 2.0：訓練神經網路
- Software 3.0：prompt 即程式，context window 就是指令

### 三大工程邏輯反轉
1. **Context Engineering**：在 AI 每次推理前精準策劃並注入所需背景（CLAUDE.md、AGENTS.md、Memory 設計）
2. **架構決策守護**：人類定義 Bounded Context（使用者身份、帳單、通知等業務邊界）；程式實作可外包，邊界定義不可
3. **治理層（Harness）設計**：Eval + Guardrails + Reflection——確定性來自外部治理層，不來自 LLM 本身

### Karpathy 的實戰洞察
- 不要用 Plan Mode 依賴——更好的做法是與 Agent 一起設計極細的 spec，spec 成為文件，再讓 Agent 執行
- AI 在「填空」上強，在「設計」上弱——判斷力、設計感、品味還無法外包
- 招募新做法：給應徵者大型真實專案，而非演算法題

## C（關鍵行動）
- [ ] 每次使用 Agent 出錯時，先問「是任務在 RL 訓練甜蜜點裡，還是落在資料盲區？」
- [ ] 在啟動任何 Agent 任務前，先定義 Bounded Context（哪些業務邊界不能讓 AI 自行決定）
- [ ] 建立「Context 策劃習慣」：考慮 CLAUDE.md、AGENTS.md 是否給了正確的上下文

## 相關連結
- [[agentic-ai-learning-roadmap]] — Agentic Engineering 的 7 階段學習路徑
- [[agentic-engineering-patterns]] — Simon Willison 的實用工程模式指南
- [[harness-engineering]] — Guides × Sensors 框架，Agentic Engineering 的技術實踐
- [[agents-md-guide]] — Context Engineering 的核心工具
- [[karpathy-knowledge-base-method]] — Karpathy 在知識管理上的另一重要貢獻
- [[harness-engineering]]「補充資料：AI 代理是聰明的猴子」段 — 執行邊界（讓錯誤可發現）與本篇 DDD 業務邊界是字面相同、意思不同的兩件事（2026-09-23 新增）
