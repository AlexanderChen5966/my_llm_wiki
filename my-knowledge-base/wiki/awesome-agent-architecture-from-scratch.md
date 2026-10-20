---
title: Awesome Agent Architecture：從零拆解 AI Agent Harness 的 22 章教材
date: 2026-07-24
updated: 2026-08-03
tags: [ai/agent, ai/prompt-engineering]
source: https://github.com/hardness1020/awesome-agent-architecture/blob/main/README.zh-TW.md
type: article
status: processed
asset_value: 100
---

一份把「AI agent 是如何圍繞 LLM 打造出來」拆成 22 個章節、循序漸進的教材。核心主張：**模型負責推理，harness（外層架構）負責行動、狀態與界線**——工具怎麼跑、多次呼叫間狀態怎麼留、副作用怎麼控管、多個 loop 怎麼協調，這些都不是單一次模型呼叫能完成的。

## T（核心論點）

理解 agent 的關鍵不在模型本身，而在 harness 的設計選擇。寫程式的工具、聊天助理、自動執行器彼此的差異，大多只是 harness 層級的取捨不同。這份 repo 用同一套四面向（開場問題／機制／各系統做法／哪裡會出錯）逐章拆解，從最小的 agent loop 一路堆疊到能自我運轉的多 agent 系統，每章都對照 Claude Code、Hermes Agent、mini-swe-agent 三個真實系統的實際做法，並附可執行程式碼。

## B（適用場景）

**適用：**
- 想系統性理解 AI agent harness 全貌，而不是東拼西湊零散知識
- 評估或設計自己的 agent 框架時，需要一份完整的模組清單對照
- 想知道 Claude Code / Hermes Agent 這類真實系統各自在哪些層做了什麼設計取捨
- 需要可執行程式碼佐證機制（而非純理論）的學習材料

**不適用 / 注意：**
- 內容以英文原始 repo 為準，繁中版是社群翻譯，用詞可能與其他中文資料略有出入
- 需要自備 `ANTHROPIC_API_KEY` 才能跑 `demo.py`；`test.py` 則可離線驗證
- 是「地圖」而非「操作手冊」——實際落地仍需對照自己專案的框架文件

## R（22 章結構與核心機制）

**核心主張**：大多數 agent 共用同一套極小的控制流程——呼叫模型 → 執行工具 → 結果接回 → 再呼叫模型。這個 loop 本身很小，工程量都在它周圍：派發工具、控管副作用、管理情境、保存狀態、協調其他 loop。

八層、22 章節架構：

| 層級 | 章節 | 核心問題 | 關鍵機制 |
|------|------|---------|---------|
| 0 基礎 | Harness thesis | agency 從哪裡來？ | Model vs harness、actions、observations、permissions |
| 1 核心 loop | Agent loop / Tool runtime / Permission & sandbox / Hooks | agent 怎麼持續運作、工具怎麼被路由、副作用怎麼控管、擴充怎麼掛進 loop | `messages[]`、Registry+dispatch、Permission modes、PreToolUse/PostToolUse |
| 2 複雜工作 | Planning & todos / Subagents / Skills / Context management | 大工作怎麼拆解、子問題怎麼隔離、能力怎麼隨需載入、長對話怎麼塞進視窗 | Plan mode、Fresh messages[]、SKILL.md 漸進式揭露、Budgeting+compaction |
| 3 知識與韌性 | Memory / System prompt assembly / Error recovery | 跨執行怎麼記住、每輪 prompt 怎麼組出、長任務怎麼在失敗中存活 | Selection/recall/extraction/consolidation、Cache boundaries、Fallback model |
| 4 長時間與非同步 | Task system / Background execution / Scheduling / Worktree isolation | 工作怎麼跨輪次存在、怎麼在主 loop 外執行、怎麼延後執行、平行工作怎麼避免衝突 | Task records+locks、Handles+notification queue、Cron/sleep、Git worktrees |
| 5 多 Agent | Coordination / Protocols / Autonomy | 多 agent 怎麼溝通、怎麼達成共識收尾、怎麼自我組織 | Inboxes+broadcasts、Plan approval+shutdown handshakes、Idle cycle+task claiming |
| 6 擴充整合 | MCP/plugins/channels / Observability | harness 怎麼連外部世界、怎麼知道系統有效 | Transports+tool pool、Tracing/metrics/evals |
| 7 組合 | Loop engineering | loop 怎麼疊成能自運轉的系統 | Verification loop、triggers、budgets、maturity levels |

**三個對照系統**：
- **Claude Code**：完整 harness，覆蓋全部 22 章，建議從這裡讀起
- **Hermes Agent**：長期助理特化，重點在 Memory/Skills/always-on channels（第 7、9、14、16、19、21 章）
- **mini-swe-agent**：極簡研究基準（約 150 行 bash tool），示範最小完整 loop

**學習方法**：照順序讀、可執行章節先讀 `src/loop.py` 再跑 `demo.py`、把某章 `src/` 與前一章 diff 看出新增了什麼機制。

## C（關鍵行動）

- [ ] 對照自己使用的 agent 工具（Claude Code / Cursor / 自建 harness），逐層檢查 22 章模組是否都有對應設計，找出缺口
- [ ] 想深入某個機制時，優先讀該章節的「哪裡會出錯」小節，比讀機制本身更快抓到實務地雷
- [ ] 若要自建 harness，参考本篇的分層順序（loop → tool → permission → hooks → planning → subagents → skills → context → memory → …）決定實作優先級，而非一次求全
- [ ] 想理解 Skills 在 agent 架構中的定位，對照第 7 章與既有 [[claude-skills-deep-dive]]、[[ai-agent-architecture-10-modules]] 的 Skills 模組說明

## 補充資料：Capability scaffolding vs Control plane（2026-08-03 併入）

來源：同作者 iThome 文章〈Agent 不是越複雜越好：哪些機制該留，哪些可以拿掉？〉（https://ithelp.ithome.com.tw/articles/10400653 ，2026-08-03）。**本節補上本 repo 22 章機制清單缺的那一層：如何判斷某個機制該不該留。**

### ① 二分法：不是所有機制都用同一套標準判斷

作者把 Agent Harness 分成兩類，**兩者的判斷方式根本不同**：

| 類型 | 目的 | 常見機制 | 判斷方式 |
|---|---|---|---|
| **Capability scaffolding** | 幫助模型完成任務 | Task decomposition、Retry、Evaluator、Memory、Subagent | ✅ **用測試案例測**——沒帶來可衡量改善就可以移除 |
| **Control plane** | 管理權限、狀態、成本與副作用 | Permissions、Sandbox、Audit log、Budget limits、Human approval | 🔴 **不可用測試結果決定去留**——它管的是政策、究責與不可逆風險，屬於系統邊界 |

🔑 **關鍵論證**：模型變強會**降低犯錯機率，但不會降低一次嚴重錯誤造成的影響**。

> 「一個更聰明的 Agent，也不代表它應該擁有更大的資料庫刪除權限。」
> 即使測試顯示 Agent 很少需要人工核准，也不代表可以取消高風險操作前的核准流程——那只說明模型表現變好，不代表不可逆操作的風險消失了。

**對本 repo 22 章的實務用法**：讀完機制清單後，逐項標記它落在哪一類。落在 capability scaffolding 的（第 5 章 Planning & todos、第 6 章 Subagents、第 11 章 Memory、第 12 章 Error recovery…）都該用 A/B 測試檢驗；落在 control plane 的（第 3 章 Permission & sandbox、第 22 章 Observability 的 audit 面）不論模型多強都留著。

### ② Anthropic 工程師點名該移除的機制

作者在 San Francisco 一場 AI Agent 講座上，一位 **Anthropic 工程師分享的投影片**：隨著模型能力提升，他們**主動移除**了部分原本用來維持長任務穩定性的機制——

- ❌ **拆分多個 sprint**（過度拆解任務）
- ❌ **限制單次 session 長度**
- ❌ 多次重試
- ❌ 每個步驟都呼叫 Evaluator
- ❌ 為了避免 Context 過長而強制切斷流程

理由：這些設計主要在補足模型當時做不到的事；模型升級後，某些補償設計不再帶來足夠幫助，**甚至可能限制模型發揮**。

> ⚠️ **「限制單次 session 長度」這條與 [[claude-code-context-management]] 的「世代反轉注記」互為佐證**——那裡記載 OpenAI 開發者 Jason Liu 也主張「對話太長要開新的」已過時（他有運作五週、累積約 400 個子代理的對話串）。**兩個獨立來源、不同陣營、同一結論**。但那則注記同時保留了代價（壓縮品質不穩、細節可能遺失、帶 `paths:` 的 rules 在 compaction 時會先遺失），判準應是「**依賴什麼而定**」，不是無條件養長對話。

### ③ 移除機制的驗證方法：同一組測試案例跑兩個版本

不要因為 Multi-Agent、Memory 或 Loop Engineering 看起來厲害就直接加進系統。合理做法是**先從真實任務、執行紀錄與過去發生的問題建立測試案例**，再用同一組案例比較「保留該機制」與「移除該機制」兩個版本。

Agent 任務多半沒有唯一標準答案（研究、軟體開發、資料分析、複雜決策），所以測試維度不只對錯：

- 任務完成度與結果品質
- Grounding 與限制遵守
- 工具使用是否合理
- 錯誤恢復與**人工接管率**
- 延遲、token 消耗與**成功任務成本**

### ④ 設計 Agent 架構的三個依序問題

1. 這個機制要解決哪一種**實際**問題？
2. 加入後，能否在**真實任務的測試**中帶來明顯改善？
3. 它是在**幫助模型完成任務**，還是在**限制權限與風險**？

> 好的 Agent 架構不是功能最多的架構，而是用**最少的必要機制**穩定完成任務，同時把權限、成本與副作用控制在可接受範圍內。
> 真正要問的不是「這個功能要不要加」，而是「**它正在解決哪個可以被觀察與驗證的問題？**」

## 相關連結

- [[synthesis/synthesis-agent-engineering-architecture]] — **本篇為其來源之一**（2026-09-03 重跑，8→16 篇）：Agent 工程架構精粹，含 Capability scaffolding vs Control plane 二分法

- [[ai-agent-architecture-10-modules]] — 另一種 10 模組拆解視角（LLM/ReAct/Tools/Skills/Session/Context/Memory/Permission/Hook/SubAgent），可與本篇 22 章對照互補
- [[agent-loop-explained]] — Agent Loop 概念總覽，補充本篇「核心 loop」章節的理論脈絡
- [[ai-agent-core-loop]] — 動手實作視角的 80 行核心迴圈，對照本篇第 1 章 Agent loop 的最小實作
- [[hermes-agent-architecture-deep-dive]] — Hermes Agent 完整架構深度解析，補充本篇對照系統之一的細節
- [[harness-engineering]] — Guides×Sensors 框架，呼應本篇「harness 給模型行動、狀態、界線」的核心主張
- [[production-agent-architecture]] — 生產級 20 模組工程清單，可作為本篇理論分層的落地對照
- [[claude-code-context-management]] — **與本頁補充②互為佐證**：Anthropic 移除「限制 session 長度」建議 ↔ 該頁「世代反轉注記」的長對話取捨
- [[claude-cookbook-official-resources]] — Anthropic 官方 Cookbook 範例索引；**與本篇互補**：本篇是機制地圖，那篇是依問題查的可執行食譜

## 原始摘要

hardness1020/awesome-agent-architecture GitHub README（繁中版）。一份從零拆解 AI agent harness 架構的教材，以 loop、tool、memory、permission、context、task、interface 等 22 個章節循序建立，並以 Claude Code、Hermes Agent、mini-swe-agent 三個真實系統作為對照範例，每章附可執行程式碼。核心主張：agency 來自 harness 設計，而非模型本身。
