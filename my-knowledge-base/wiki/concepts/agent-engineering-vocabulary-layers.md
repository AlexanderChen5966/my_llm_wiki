---
title: Agent 工程詞彙的四層抽象：#101-120 圖卡與本庫條目對齊地圖
date: 2026-09-01
tags: [ai/agent, dev/architecture]
type: concept-synthesis
sources:
  - [[llm-concepts-visual-cards-61-120]]
  - [[synthesis-agent-engineering-architecture]]
  - [[synthesis-multi-agent]]
  - [[harness-engineering]]
  - [[agent-loop-explained]]
status: processed
---

## 概念定義

[[llm-concepts-visual-cards-61-120]] 的 #101-120 共 20 張卡是一份 2026 年 Agent 工程詞彙表，但它**把四個不同抽象層的東西平鋪成一條編號序列**——範式（Harness Engineering）、機制（Agent Loop）、軟體實體（Agent Scaffold）、成品型態（Coding Agent）並列，彼此沒有標示層級關係。

這正是中文圈 Agent 用詞混亂的來源：**「Harness」有時指方法論、有時指那包程式碼；「Loop」有時指那個 while 迴圈、有時指設計該迴圈的整套範式。** 本條目把 20 張卡歸位到四層，並逐一標出本庫的覆蓋深度。

這篇是導航地圖，不重複各條目內容；每格連到既有 wiki 條目。

## 四層抽象總表

覆蓋標記：🟢 本庫深度**超過**圖卡（引用時以本庫為準）／🟡 有覆蓋但**分散、無專篇**（圖卡定義可當骨架）／🔴 **真正缺口**（圖卡是本庫目前唯一來源）

### ① 範式層——「人該用什麼方式面對模型」

| # | 概念 | 覆蓋 | 本庫條目 |
|---|------|------|---------|
| 104 | Loop Engineering | 🟢 | [[loop-engineering-6-components]]、[[loop-engineering-human-roles]]、[[loop-engineering-implementation-patterns]]、[[loop-engineering-playbook-cli-tools]] |
| 102 | Harness Engineering | 🟢 | [[harness-engineering]]、[[claude-code-harness-internals]]、[[harness-engineering-vue3-admin-practice]]、[[unhobbling-harness-capability-overhang]]、[[claude-code-review-harness-principles]] |
| 105 | Context Engineering | 🟢 | [[context-engineering-fundamentals]]、[[context-engineering-stacking]]、[[claude-5-context-engineering-rules]]、[[agent-context-management]] |

🔴 **這三張卡最大的問題是沒有順序**。本庫已上網交叉比對 Addy Osmani／codecentric／O'Reilly 三個權威來源，確認堆疊順序為 **Prompt → Context → Harness → Loop**（每層包住前一層、也繼承前一層的弱點），並記錄了知識庫自身曾沿用錯誤順序的修正過程（見 [[harness-engineering]] 與 [[agent-loop-explained]]）。**用這三張卡校準用詞時必須自行補上這個排序**，否則會退回「三種平行的 Engineering」的錯誤心智模型。

### ② 機制層——「單一能力或循環怎麼運作」

| # | 概念 | 覆蓋 | 本庫條目 |
|---|------|------|---------|
| 103 | Agent Loop | 🟢 | [[agent-loop-explained]]（概念）、[[ai-agent-core-loop]]（約 80 行實作視角）、[[agent-loop-stop-conditions]] |
| 108 | Native Tool Use | 🟢 | [[agent-tool-runtime-seven-layers]]、[[agent-tool-authorization-gatekeeper]]、[[langchain-tool-use]]、[[handcraft-agent-shell-tool]] |
| 107 | Computer Use Agent | 🟢 | [[fara-computer-use-agent]]、[[codex-computer-use-modes]]、[[puppeteer-browser-automation]] |
| 119 | World Models for Agents | 🔴 | **無任何條目**——圖卡是本庫目前唯一來源 |

🔑 **#108 是圖卡與本庫落差最大的一張**。卡片把 Native Tool Use 講成「把工具當一等動作調用」的能力宣傳，而 [[agent-tool-runtime-seven-layers]] 的核心論點恰恰相反：**Tool Call 只是一張工作單**，不是執行結果、不是安全邊界；模型只參與第一步，Registry／Schema 驗證／Permission Gate／執行／Timeout／Output Normalization／Audit 七層全屬 Harness。引用 #108 時務必補上這層，否則會低估工具調用的工程量。

### ③ 實體層——「支架裡實際有哪些元件」

| # | 概念 | 覆蓋 | 本庫條目 |
|---|------|------|---------|
| 109 | Agent Scaffold | 🟢 | [[awesome-agent-architecture-from-scratch]]（22 章八層架構）、[[production-agent-architecture]]（20 模組）、[[ai-agent-architecture-10-modules]]、[[claude-code-five-layer-architecture]] |
| 106 | Agentic Memory | 🟢 | [[agent-memory-lifecycle]]、[[llm-memory-pattern]]、[[memharness-memory-reconstruction]]、[[concepts/agent-handoff-memory-patterns]]、[[agentmemory]] |
| 117 | Memory Hierarchy | 🟢 | [[agent-memory-lifecycle]]、[[agent-context-management]]、[[claude-code-handoff-memory-strategy]] |
| 118 | Dynamic Context Compression | 🟢 | [[context-isolation-and-compaction]]、[[headroom-context-compression]]、[[claude-code-context-management]]、[[agent-context-management]] |
| 115 | Workflow Orchestration | 🟢 | [[n8n-multi-agent-workflow]]、[[google-adk-2-graph-workflow]]、[[claude-code-dynamic-workflows]]、[[graph-engineering-node-edge-state]]、[[enterprise-ai-four-layers-prompt-policy-tool-workflow]] |
| 111 | Eval Harness | 🟡 | 無「Eval Harness」專篇，但拆散在 [[search-eval-set-design]]（評測集設計）、[[agent-skill-evaluation-tools-comparison]]、[[agent-bug-fix-eval-green-is-not-correct]]、[[testing-your-ai-workflow-config-ci]] |
| 116 | Agent Observability | 🟡 | 無專篇，散見 [[production-agent-architecture]]、[[awesome-agent-architecture-from-scratch]]、[[agent-tool-runtime-seven-layers]]（Audit&Trace 層）、[[ai-agent-dev-concepts-practitioner]] |

🔴 **#106/#117 的分層記憶敘事本庫有強力反面證據**：[[memharness-memory-reconstruction]] 實測「加記憶反而變差」（ALFWorld 76.4%→70.1%，Mem0/SimpleMem 更低到 52%），問題出在**靜態回放整個範式**。兩張卡都把分層存取講得理所當然，卻沒談「該不該回放」。校準用詞可以用卡片，但論證記憶價值時不能只引卡片。

### ④ 應用層——「組裝出來的成品長什麼樣」

| # | 概念 | 覆蓋 | 本庫條目 |
|---|------|------|---------|
| 113 | Coding Agent | 🟢 | [[hand-coding-agent-file-tools]]、[[pi-coding-agent-minimal-terminal]]、[[codegraph-ai-coding-agent]]、[[ai-coding-agent-governance-dotbabel]]、[[vibe-vs-agentic-coding-maturity]] |
| 110 | Multi-Agent Collaboration | 🟢 | [[synthesis-multi-agent]]、[[multi-agent-papers-three-elements]]、[[multi-agent-group-chat-pattern]]、[[supervisor-multi-agent-langchain]]、[[subagent-four-boundaries]] |
| 120 | Native Multimodal Agents | 🟡 | [[multimodal-multi-agent-extension]]、[[multimodal-document-processing-three-layers]]（偏文件處理，非「原生多模態 Agent」本身） |
| 114 | Long-Horizon Agent | 🔴 | **近乎缺口**——僅 [[claude-model-selection-guide]] 提及「長時程」一詞；相鄰能力散在 [[agent-session-management]]、[[agent-error-recovery-taxonomy]]、[[agent-task-system-background-scheduling]] |
| 112 | RACG（Retrieval-Augmented Code Generation） | 🔴 | **無條目**——相鄰的是 [[rag-retrieval-augmented-generation]]（通用 RAG）與 [[codegraph-token-optimization]]／[[graphify-codebase-knowledge-graph]]（程式庫索引），但「檢索前置於補丁生成」這條線沒有 |
| 101 | OpenClaw | 🟢 | ⚠️ **本庫覆蓋方式比卡片好**：卡片是產品簡介，本庫把它當**比較對象**——[[agent-framework-stability-comparison]] 以它為四框架之一分析「對穩定性的不同理解」，[[hermes-agent]] 記錄從它遷移的路徑 |

## 關鍵區分

這 20 張卡最容易被混為一談的六組，本質不同：

| 對比 | 差別 |
|------|------|
| **#102 Harness Engineering vs #109 Agent Scaffold** | **方法論 vs 軟體實體**。前者是「怎麼做」的工程主張，後者是那包實際跑起來的程式碼。說「我們在做 Harness Engineering」與「我們的 Scaffold 有七層」是兩件事 |
| **#103 Agent Loop vs #104 Loop Engineering** | **機制 vs 設計該機制的範式**。Agent Loop 是 Observe→Plan→Act→Evaluate→Reflect 這個閉環本身；Loop Engineering 是「人從寫 Prompt 的操作者變成設計 Loop 的系統設計者」這個角色轉移 |
| **#105 Context Engineering vs #118 Dynamic Context Compression** | **全局 vs 其中一種手段**。壓縮只是 Context Engineering 六類輸入處理裡的一招（過濾/排序/摘要），不是同級概念 |
| **#106 Agentic Memory vs #117 Memory Hierarchy** | **記憶系統 vs 其分層存取策略**。前者答「存什麼」（偏好/事實/決策），後者答「放哪一層、怎麼命中與下探」 |
| **#111 Eval Harness vs #116 Agent Observability** | **事前量測 vs 事後追溯**。Eval Harness 固定資料與種子求可複現的分數；Observability 追的是這一次到底發生了什麼。兩者都叫「看得見」，但一個對比版本、一個定位單次失敗 |
| **#115 Workflow Orchestration vs #110 Multi-Agent** | **控制流 vs 角色分工**。DAG／條件分支／人工確認點是流程問題；誰擅長什麼、誰審誰是分工問題。兩者可疊加，也常被誤當替代方案（見 [[concepts/agent-workflow-taxonomy]] ③ vs ① 之分） |

## 共同含意

1. **圖卡的價值是詞彙表，不是知識來源**。20 張裡 13 張本庫深度已超過卡片（🟢），4 張分散無專篇（🟡），3 張是真缺口（🔴）。**該引用卡片的場合只有一種：需要一句話標準定義、或需要向外部說明概念間的層級關係。** 論證任何主張都應回到本庫條目。

2. **層級混平是這份卡的結構性缺陷，不是排版問題**。#101 是產品、#102 是方法論、#109 是實體、#113 是成品——編號相鄰不代表同級。任何照編號順序讀完就以為掌握 Agent 工程的人，會得到一張沒有縱深的平面圖。

3. **三個真缺口指向同一個方向：本庫強在「單次任務的可靠性」，弱在「跨時程的自主性」**。World Models（行動前模擬）、Long-Horizon Agent（多里程碑＋檢查點＋失敗恢復）、RACG（大型倉庫的檢索前置）都屬於**任務規模拉長後才出現的問題**。這與本庫既有偏向（Claude Code 單 session 的權限、記憶、驗證）恰好互補，是下一輪選材時值得優先補的方向。

4. **本庫的反面證據比卡片多**——#108 的工具調用七層、#106/#117 的記憶反效果、#101 的框架穩定性比較，三處都是卡片樂觀敘事的對照。這正是 🟢 標記的實質含意：不只是「也有寫」，而是**多一層失敗模式的認識**。

## 相關概念

- [[concepts/agent-workflow-taxonomy]] — Agent 開發工具的六大類型與選型決策樹；本條目談概念層級，該條目談工具層級，兩者互補
- [[concepts/meta-ai-engineering-methodology]] — AI 工程方法論的元層框架
- [[concepts/environment-over-model-principle]] — 為什麼環境（②③層的總和）比模型選擇更關鍵
- [[concepts/agent-handoff-memory-patterns]] — #106/#117 記憶層的跨 Agent 交接深入
- [[synthesis-agent-engineering-architecture]] — Agent 工程架構精粹
- [[llm-concepts-visual-cards-61-120]] — 本地圖對齊的圖卡來源（#61-100 的訓練側與安全治理不在本地圖範圍）
- [[ai-engineering-terms-genealogy]] — AI 工程術語的世代演進脈絡，與本條目的層級切法互為經緯
