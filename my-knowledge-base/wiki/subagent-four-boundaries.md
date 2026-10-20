---
title: Subagent 不等於平行處理：先切 Context、責任與權限的四層邊界
date: 2026-08-07
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw（《30 天從零拆解 AI Agent》Day 7）
type: article
status: processed
asset_value: 82
---

## T（核心論點）

「多一個 Agent = 多一份產能」是錯的——多開 Agent 實際帶來的是**更多 Token、更多延遲、更多協調、更多重複工作、更多不一致結果、更多權限風險**。所以該問的不是「可以多開幾個 Agent 嗎」，而是：

> **哪一段 Context、責任與工具範圍，值得被獨立切出去？**

Subagent 的定義是「由另一個 Agent 或控制器啟動、負責完成一個更小任務的**獨立 Agent Loop**」——它有自己的 Prompt / Context / Tool Set / Permission / Budget / State / Stop Condition。**切出去的理由是邊界，不是並行。**

## B（適用場景）

**適用：** 決定某段工作該做成 Tool 還是 Subagent；評估要不要引入 Multi-Agent；診斷「開了多個 agent 但成本暴增品質沒變好」

**不適用 / 注意：** 本篇是通用架構層拆解；Claude Code 的具體實作差異（fresh context vs 持久 cached context）見 [[fable-5-delegation-cost-benchmark]] 的架構對照節

## R（四層邊界）

### 第一層：Context Boundary

錯誤做法是「Parent 有什麼，就全部傳給 Child」。Parent 的 context 混雜了使用者需求、程式碼、文件、Tool Results、Plan、Error Logs、Memory、Previous Attempts——**不該下放的包括**：與子任務無關的歷史、使用者敏感資訊、其他工具輸出、其他 Agent 的中間結果、不必要的 Prompt、不應下放的 Secret。

**該傳的只有五樣**：子任務目標、必要背景、可使用資源、成功條件、回傳格式。

> 核心原則：**把局部問題放進局部 Context。**

### 第二層：Responsibility Boundary

「幫我研究一下這個問題」這種模糊交辦會讓 subagent：重複 Parent 已做的事、擴大範圍、修改不該修改的東西、回傳大量無法整合的內容、**自己重新定義問題**。

**交辦必須明確四件事**：
| 欄位 | 問題 |
|---|---|
| **Goal** | 要完成什麼？ |
| **Scope** | 只處理哪一部分？ |
| **Expected Output** | 要回傳什麼？ |
| **Completion Condition** | 什麼情況算完成？ |

### 第三層：Capability and Permission Boundary

> **子 Agent 應該獲得完成子任務需要的最小能力，而不是繼承 Parent 的完整權限。**

（Capability 與 Permission 的區別見 [[agent-control-layer-permission-hooks-planning]]）

### 第四層：Result Contract

Subagent 內部可能搜尋 20 個檔案、執行 8 次工具、產生大量中間假設、遇到 3 次錯誤——**但回傳的應該只是結論**。範例：要求「找出目前測試失敗的前三個主要原因」，回傳 Root Cause A/B/C ＋ 對應證據 ＋ 建議下一步，而不是整段探索過程。

---

## 🔑 Subagent 與 Tool 怎麼選

| 適合做 **Tool** | 適合做 **Subagent** |
|---|---|
| 讀檔、搜尋、查詢資料庫、執行測試、呼叫 API | 多步驟探索、多次工具呼叫、**中途修正策略**、需要局部 Context、需要獨立 Budget、需要獨立 Completion Condition |
| — | 例：分析一個模組的架構／研究三種方案／Debug 一個局部問題／Review 一組大型變更 |

判準：**單一確定動作 → Tool；需要自己跑一輪迴圈才知道答案 → Subagent。**

## 🔴 Subagent ≠ Parallelism

適合平行的是**互不依賴**的工作：搜尋三個不同資料來源／Review 三個獨立模組／評估三個候選方案／分析三組互不依賴的 Log。

**平行前必須逐項檢查五個問題**：
- 是否讀取相同資源？
- 是否修改相同資源？
- 是否存在前後依賴？
- 是否需要共享最新狀態？
- 是否會產生互相衝突的結果？

任一為「是」，就不該平行。

## ⚠️ 多 Agent 最容易出現的成本：Context Duplication

同一份背景被多個 agent 各讀一次。五種緩解手段：

| 手段 | 作用 |
|---|---|
| **Shared Summary** | 共用一份摘要而非各自重讀原文 |
| **Scoped Retrieval** | 各自只取自己範圍內的資料 |
| **Artifact Reference** | 傳檔案位置／索引而非內容本身 |
| **Result Compression** | 回傳前先壓縮 |
| **Deduplicated Search** | 避免多個 agent 重複搜同樣東西 |

另有 **Coordination Cost**：多個結果回來後，Parent 還要判斷「哪個結果比較可信、是否可以合併」——這筆成本常被低估。

## C（關鍵行動）

- [ ] 🔴 想開 subagent 時先問「**哪段 context/責任/權限值得切出去**」，而不是「能不能多開幾個」
- [ ] 交辦一律寫齊 Goal / Scope / Expected Output / Completion Condition，別用「幫我研究一下」
- [ ] 子 agent 只給完成該任務的**最小權限**，不要繼承 Parent 全部
- [ ] 要求回傳**結論而非過程**（Result Contract）
- [ ] 平行前跑一次五問檢查（同資源／同修改／前後依賴／共享狀態／衝突結果）
- [ ] 單一確定動作做成 Tool，需要跑迴圈才知道答案的做成 Subagent

## 相關連結
- [[synthesis/synthesis-multi-agent]] — **本篇為其來源之一**（2026-09-03 重跑，7→9 篇）：🔑 本篇正面填上該精粹原本的首要空白「值不值得建」
- [[synthesis/synthesis-agent-engineering-architecture]] — **本篇為其來源之一**（2026-09-03 重跑，8→16 篇）：Agent 工程架構精粹，含 Capability scaffolding vs Control plane 二分法
- [[context-isolation-and-compaction]] — **同一命題的入門視角**：Subagent 的價值是 Context Isolation 而非「多一個 AI 比較快」；該篇的四行判斷表（要不要派）比本篇的五問檢查更輕量，兩篇都引 Anthropic 自家「高度互相依賴時硬拆反而增加溝通成本」
- [[agent-tool-runtime-seven-layers]] — 同系列 Day 3：Tool Call 只是工作單、Tool Runtime 七層責任；本篇的 Result Contract 對應其 Output Normalization
- [[agent-control-layer-permission-hooks-planning]] — 同系列 Day 4–6：Capability vs Permission、Hooks 生命週期、Planning 的執行狀態；本篇第三層邊界的細節在該篇
- [[fable-5-delegation-cost-benchmark]] — ⚠️ **實作差異對照**：Devin Fusion 的 sidekick 是「常駐並行 + 持久 cached context」，Claude Code subagent 是「按需生成 + fresh context」——本篇的 Context Duplication 成本在後者結構性更高
- [[synthesis-multi-agent]] — Multi-Agent 精粹；本篇補上「切邊界優先於談平行」的判準
- [[claude-code-cross-session-messaging]] — Claude Code 各層分工（subagent／agent teams／agent view／cross-session）的官方定位對照
- [[multi-agent-papers-three-elements]] — 40 篇論文的迴圈/提示/集約三要素
- [[agent-os-workstation-division]] — 工位分工原則（核查者不能是提煉者自己）
- [[ideas/multi-agent-handoff-acceptance-workbench-agency-agents]] — 📎 2026-10-19 新增：Workbench 看板＋Agency Agents 角色模板，「產出＋檢查」兩角色起步（aiposthub 同作者兩篇，未實測）
- [[ideas/claude-managed-agents-dynamic-workflows-every-2026-10]] — 📎 2026-10-19 新增：Managed Agents 動態流程上限 1,000 是規模上限非建議值；子代理 vs 動態流程的官方適用情境
