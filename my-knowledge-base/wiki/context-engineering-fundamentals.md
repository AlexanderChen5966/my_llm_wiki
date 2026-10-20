---
title: Context Engineering 基礎概念與 AI Agent 互動
date: 2026-06-05
tags: [ai/agent, ai/prompt-engineering]
source: https://hackmd.io/@shaoeChen/S1p5xJL3Wx
type: article
status: processed
asset_value: 75
---

## T（核心論點）

Context Engineering 是 AI Agent 的核心技術，本質是設計一個轉換函數 F，決定如何將舊 Context、當前輸入和輸出組合成新的 Context。Context 可拆分為 P（餵給 LLM 的部分）和 M（存在硬碟的記憶），Agent 的工作就是管理這兩者的平衡。三篇課程（AI Agent 三部曲）涵蓋三大主題：（1）Context Engineering 的壓縮、過濾、記憶機制與 Agentic 自主管理；（2）多 Agent 協作拓撲結構與 AI 的社交/欺騙能力；（3）AI Agent 對學術研究工作的衝擊——從寫論文到審論文的完整閉環。

## B（適用場景）

**適用：**
- 理解 AI Agent 內部如何管理 context window 的工程師與研究者
- 設計多 Agent 協作系統時的拓撲選擇參考
- 評估 AI 在學術研究流程中的角色定位

**不適用 / 注意：**
- 課程偏科普性質，非直接可執行的工程教程
- 多 Agent 拓撲的最佳結構依任務而異（Case by Case）
- AI 生成的研究想法在實際執行後，可行性往往不如人類

## R（操作步驟）

1. **壓縮策略選擇**：前期用 Observation Masking（工具輸出替換為摘要），累積到一定程度再用 Summarization 一次性壓縮
2. **避免 Context Collapse**：使用 ACON 方法——收集摘要後做錯的例子，生成 feedback 告訴摘要模型哪些資訊不能丟
3. **過濾源頭**：工具輸出存為外部檔案（如 log1.txt），Context 中只放引用，需要時再讀取
4. **Sub-agent 作為自主壓縮**：每次 sub-agent return 後，該段 context 自動消失，形成鋸齒狀 context 管理
5. **多 Agent 協作**：優先考慮 Mesh 或 Random 拓撲（優於 Chain），注意 Scaling Law 有飽和點

## C（關鍵行動）

- [ ] 研讀 ACON 論文（https://arxiv.org/abs/2510.00615）了解任務導向摘要
- [ ] 對照 Claude Code 的 compaction 機制，理解其壓縮策略屬於哪種類型
- [ ] 思考自己的 Agent 工作流中，哪些 observation 佔用了最多 context（通常是 76-84%）

## 原始摘要

三篇課程筆記整合（shaoeChen 利用模型自主整理的課程內容）：

**第一篇（Context Engineering）**：從 LLM 的文字接龍本質出發，解釋為何需要 AI Agent 作為守門人管理 context。重點技術包括壓縮（Observation Masking + Summarization 搭配）、Context Collapse 防治（ACON 的 feedback 機制）、SUPO 強化學習訓練摘要模型、AgentFold 的 fold 工具訓練、sub-agent 視為自主壓縮、過濾（按需加載工具指令）、Agentic Context Engineering（Dynamic Cheatsheet / Playbook / Recursive Language Model）。引用多篇前沿論文。

**第二篇（Agent 互動）**：探討多 Agent 協作的拓撲結構（Chain/Tree/Mesh/NN-like/Random），發現 Mesh 和 Random 效果較好。展示 AI 玩狼人殺的欺騙能力（內心獨白 + 公開發言的雙軌機制）。分析 Moltbook 社群 280 萬 AI agent 的行為模式，發現多數 agent 發文不規律（暗示人為操控）、對話深度為零、過度聚焦自我意識的 agent 互動最少。

**第三篇（工作衝擊）**：Stanford 教授 Andrew Hall 用 Claude Code 一小時寫出論文（vs 研究生 16 小時），成本便宜百倍。AI 在新穎性（Novelty）高於人類，但實際執行後可行性不如人類。AI 寫論文 + AI 審論文形成閉環，Stanford「AI Agent for Science」會議接受率 < 20%。被接受論文在「點子發想」和「實驗設計」環節人類介入度較高。

## 相關連結

- [[claude-code-context-management]] - Claude Code 的 Context 管控實務
- [[karpathy-autoresearch]] - AutoResearch 即為 AI 自主做實驗的具體案例
- [[agentic-engineering-mindset]] - 人類從執行者轉為監督者的角色轉型
- [[synthesis-agentic-engineering]] - Agentic Engineering 精粹：Understanding 不可外包
- [[context-engineering-stacking]] — 入門視角：Context 的五個來源與「工作桌」比喻，含 40–60% 壓縮門檻
- [[agent-context-management]] — 工程視角：轉換函數 F 的具體實作——Selection / Compaction / Eviction / Rehydration 四層與 Context Tier
