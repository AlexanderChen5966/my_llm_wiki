---
title: Agent Loop 何時該走向 Graph Workflow：五個訊號與可控自主性
date: 2026-09-04
tags: [ai/agent, type/concept]
source: https://ithelp.ithome.com.tw/articles/10407401（Data Machi 30 天學習系列 Day 23）
type: article
status: processed
asset_value: 68
---

## T（核心論點）

當 Agent 加入 Tool、Memory、Verification、Clarification、Retry、Human Approval 後，若這些判斷全部藏在同一個自由的 Agent Loop 裡，行為會變得難以預測與除錯——不是模型變差，而是控制邏輯變多了卻仍全部塞在同一個循環裡。核心原則：**需要語意判斷的地方交給模型；不能被跳過的規則交給程式。** 這不代表 Agent Loop 不好——任務簡單（2-3 個 Tool、沒有高風險 Action、沒有複雜 Retry）時它仍然是最合適的選擇。真正的問題不是「Agent Loop 好不好」，而是**這個任務是否已經複雜到需要更明確的流程控制（Graph Workflow）**。

## B（適用場景）

**適用：** 判斷現有 Agent Loop 架構何時該演進為 LangGraph 等 Graph Workflow；設計高風險 Action（寄信、寫資料庫、審批）前的強制驗證關卡；需要 Pause/Resume 的 Human-in-the-loop 流程；除錯時需要精確定位「哪個節點失敗」。

**不適用 / 注意：** 不是所有任務都要走到 Graph Workflow 這一層——若只是 User→Agent→2 個 Tool→Answer 且無分支、無 Human Approval、無複雜 Retry，簡單 Agent Loop 仍最合適，把單純 RAG 硬拆成十個 Node 只是把簡單系統複雜化；架構應該跟著問題長大，而非先選框架再讓問題配合框架。

## R（操作步驟）

**判斷「該從 Agent Loop 走向 Graph Workflow」的五個訊號：**

1. **Prompt 開始大量出現「一定要先 A 再 B」的規則**——這些規則若越來越多，通常代表它們該離開 Prompt 進入 Workflow
2. **Agent 偶爾會跳過必要步驟**——例如十次裡九次先做 Verification，但有一次直接跳到 Action；對高風險場景「90% 會遵守」通常不夠
3. **開始需要 Pause / Resume**，即 Human-in-the-loop（例如寄信前需人工確認，可能幾分鐘到隔天才收到 Approve）
4. **Retry 路徑變多，且不同錯誤需要不同處理方式**（Timeout 有限重試、Permission Denied 直接停止回報、Missing Parameter 走 Clarification、No Relevant Document 改寫查詢重搜）
5. **除錯時已經很難回答「這個 Request 到底經過了哪些節點」**（Observability 問題）

**Prompt 規則 vs 系統保證的本質差異：**

Prompt 說「請務必先驗證資料，再建立任務」——模型大多數時候會照做，但企業系統需要的是「這個步驟在技術上就不能被跳過」。Workflow 則是在系統裡直接設定：`沒有 approval = true → send_email() 無法被執行`，這才是真正的流程控制。

**架構演進路徑（不需每個任務都走到最後一層）：**

```
簡單問題 → Direct LLM
需要外部能力 → Tool Use
需要多步驟 → Workflow
需要動態決策 → Agent
需要可控的動態流程 → Graph-based Agent Workflow
```

**決策責任分工表：**

| 問題 | 比較適合誰決定 |
|---|---|
| 使用者真正想問什麼？ | LLM |
| 哪個 Tool 語意上最適合？ | LLM |
| 找不到資料後應改怎麼查？ | LLM / Policy |
| Verification 失敗能不能進下一步？ | 程式 |
| 最多 Retry 幾次？ | 程式 |
| Action 前是否一定需要 Approval？ | 程式 |
| 某個 User 是否有權限？ | 權限系統 |
| Workflow 接下來允許走哪些路？ | Graph |

**混合設計範例**：選 Tool 讓 Agent 自主決定，但「Verification Failed → 不允許 Execute」由 Workflow 強制保證——不是把 Agent 關起來，而是讓它在需要彈性的地方自主、在不能出錯的地方受到明確控制。

**Human-in-the-loop 需要 State Persistence**：Pause 代表系統必須保存 workflow_id、current_node、pending_action、task_payload、verification_status 等狀態，使用者半小時後按下確認時，系統才知道當時停在哪一步。

## C（關鍵行動）

- [ ] System Prompt 若已出現大量「先做 A，如果失敗做 B，如果還失敗就 C」的條件規則，視為訊號：這些內容該搬進 Workflow，而非繼續留在 Prompt
- [ ] 高風險 Action（寄信、寫資料庫、審批）前，用程式 Edge 強制要求 Verification/Approval 通過，不要只在 Prompt 裡提醒模型
- [ ] 為每種錯誤類型設計不同的下一步（Timeout 有限重試/Permission Denied 停止回報/Missing Parameter 進 Clarification/No Relevant Document 改寫查詢），不要全部用同一套「再試一次」處理
- [ ] 需要 Human-in-the-loop 時，設計明確的 State Persistence（workflow_id/current_node/pending_action），支援 Pause→保存→Resume
- [ ] 任務簡單（沒有高風險 Action、沒有複雜 Retry、2-3 個 Tool）時，不要為了流行而硬套 Graph Workflow，簡單 Agent Loop 仍是最合適選擇

## 原始摘要

「Data Machi 30 天學習系列」Day 23：說明當 Agent Loop 加入 Tool、Memory、Verification、Clarification、Retry、Human Approval 後行為變得難以預測與除錯，提出五個訊號（流程順序需固定、Agent 偶爾跳步驟、需要 Pause/Resume、Retry 路徑增多、除錯難以追蹤決策路徑）判斷該不該從 Agent Loop 走向 Graph Workflow（如 LangGraph）。核心原則是「需要語意判斷的地方交給模型；不能被跳過的規則交給程式」。下一篇（Day 24）正式進入 LangGraph 的 State/Node/Edge 概念。

## 相關連結

- [[agent-clarify-requery-verify-decision]] — **系列上一篇（Day 22）**：本篇正是承接該篇 Clarify/Re-query/Verification/Retry 全部塞進同一 Agent Loop 後難以控制的問題
- [[langchain-langgraph-basics]] — **系列下一篇（Day 24，既有條目已回連本篇）**：正式進入 LangGraph 的 State/Node/Edge 概念，把本篇提出的訊號具體化為架構實作
- [[graph-engineering-node-edge-state]] — Node/Edge/State 的通用編排框架視角，可作為本篇「決策責任分工表」的延伸架構參考
- [[langgraph-coordinator-refactor-seven-nodes]] — LangGraph 實作版：Coordinator 改造成七 Node 可控 Workflow，是本篇理論訊號的具體落地案例
- [[ai-agent-architecture-patterns]] — Agent 四大架構模式總覽，可對照本篇「架構演進路徑」的定位
