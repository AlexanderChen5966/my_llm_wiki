---
title: Agent Memory 三層管理：Recent Conversation / Summary / Tool Result 與 Reuse vs Re-query 判準
date: 2026-09-03
tags: [ai/agent, type/concept]
source: https://ithelp.ithome.com.tw/articles/10406864（Data Machi 30 天學習系列 Day 21）
type: article
status: processed
asset_value: 66
---

## T（核心論點）

企業 Agent 的 Memory 真正要解決的問題不是「如何把所有東西記住」，而是：**現在這一輪工作真正需要哪些上下文？哪些資訊可以安全沿用？哪些資訊雖然以前查過，但現在必須重新確認？** Memory 不應該只是一個「聊天紀錄」欄位，而應依用途分三層管理：Recent Conversation（理解追問）、Summary Memory（壓縮長對話背景）、Tool Result Memory（保存可驗證的原始查詢事實）。最關鍵的判準是：**記住條件，不代表一定沿用結果**——修改查詢條件（時間、地點、狀態）須重新查詢，只有純粹改變呈現方式（換語言、整理三點）才可安全沿用舊結果。

## B（適用場景）

**適用：** 設計多輪對話 Agent 的記憶架構；判斷 Agent 何時該用 Cache（沿用舊結果）、何時必須 Re-query；避免 Agent 把「舊資料」當「現在的答案」回答使用者；LangGraph State 設計的前置概念（本篇欄位會直接對應到 State schema）。

**不適用 / 注意：** 本篇是實作層機制設計，非記憶系統的生命週期架構（Selection/Extraction/Recall/Consolidation 見 [[agent-memory-lifecycle]]）；不涉及跨 Session 的長期記憶治理，聚焦單一 Conversation 內的多輪管理。

## R（操作步驟）

**Memory 三層分類：**

| Memory 類型 | 主要用途 | 例子 |
|---|---|---|
| Recent Conversation | 理解近期追問、代名詞與上下文 | 「那去年呢？」中的「那」 |
| Summary Memory | 壓縮較長對話，保留核心任務背景 | 目前正在分析工單趨勢 |
| Tool Result Memory | 保存真正查詢回來的事實 | Google Sheets 查到的數字 |

**Conversation ID 隔離**：每段對話都要有明確 Conversation ID，避免 A 使用者查詢結果被 B 使用者沿用——這不只是回答錯誤，可能直接變成資料權限問題。Memory Key 至少需包含 User ID + Conversation ID。

**原始 Tool Result 是事實層，模型整理後的回答只是 Presentation Layer**：若只保存模型說的「約 330」而非原始 `328`，後續計算會失去精度；更危險的是，若模型第一輪錯誤回答「截止日期是 9 月 30 日」，下一輪把 Assistant Message 當 Memory，就會把錯誤答案當成既定事實繼續使用。因此 Conversation Message 與 Tool Result 必須分開儲存，一旦不一致，優先相信可驗證的 Source。

**Reuse vs Re-query 判斷表：**

| 使用者要求 | 建議行為 |
|---|---|
| 換成英文／整理成表格／縮短成三點／用簡單方式解釋 | 沿用結果（Presentation Change） |
| 「那去年呢？」（修改時間/地點/狀態條件） | Re-query，但沿用問題結構與其他條件 |
| 「那它的定義呢？」（代名詞+跨 Tool） | Memory 解代名詞，Coordinator 決定切換到哪個 Tool |
| 「現在專案進度呢？」「今天銷售多少？」 | Re-query（資料本身會隨時間變動） |
| 「你確定嗎？我剛更新資料」 | 強制 `force_refresh = true`，跳過 Cache |

**Freshness / TTL 概念**：資料新鮮度分級決定 Reuse 優先級——政策文件相對穩定，專案狀態/庫存/今日銷售高度動態。判斷流程：有舊 Result？→ 是否仍在有效期限？→ 使用者要求最新？→ 是則重新查詢，否則安全沿用。

**四輪驗收測試**：Round 1 完整問題（建立 Query Conditions + Result）→ Round 2 省略條件追問（應正確補全並 Re-query）→ Round 3 資訊不足的模糊問題（應 Clarify 而非亂猜）→ Round 4 明確要求最新資料（即使舊 Result 仍在，也應 `force_refresh`）。

## C（關鍵行動）

- [ ] Memory 欄位設計至少分開 Recent Conversation / Summary / Tool Result 三類，不要全部塞進單一「聊天紀錄」欄位
- [ ] Tool Result 保存時附帶 Query、Raw Result、Source、Queried At，不要只留模型整理後的自然語言句子
- [ ] Memory Key 至少包含 User ID + Conversation ID，避免不同對話互相污染
- [ ] 使用者說「換個方式呈現」（換語言/整理格式/縮短）時沿用舊結果；使用者修改查詢條件或使用「最新/現在/剛更新」等詞時強制 Re-query
- [ ] 為不同資料類型設定合理的新鮮度分級（TTL 概念），不是所有資料都用同一套保存期限

## 原始摘要

「Data Machi 30 天學習系列」Day 21：主張 Agent Memory 應依用途分三層管理，並強調「記住條件不代表沿用結果」——只有純粹改變呈現方式時才可安全沿用舊結果，修改查詢條件仍需 Re-query。提出 TTL（保存期限）概念與 Conversation ID 隔離避免跨對話污染，並說明 Memory 欄位設計將直接對應到後續 LangGraph State 架構。下一篇（Day 22）處理資訊不足時該追問、重查還是拒答的判斷邏輯。

## 相關連結

- [[agent-memory-lifecycle]] — **雙向分工**：該條目是記憶的四層生命週期架構（Selection/Extraction/Recall/Consolidation，聚焦跨 Session 的長期記憶治理）；本條目是單一 Conversation 內「該不該記、怎麼記、何時該重查」的實作層判準，兩者已核對無 TTL/Reuse-vs-Requery/Memory-Key 內容重疊，非重複條目
- [[agentmemory]] — 把跨 session 記憶做成 MCP server 的具體實作，可與本篇的 Memory Key 設計對照
- [[agent-clarify-requery-verify-decision]] — **系列下一篇（Day 22）**：承接本篇 Memory 主題，討論資訊不足時該 Clarify、Re-query 還是 Verification/拒答
- [[langchain-langgraph-basics]] — 本篇 Memory 欄位設計（recent_context/task_summary/query_conditions/tool_results 等）預告了後續 LangGraph State 的結構
- [[agent-multi-source-query-parallel-sequential-scheduling]] — **系列上一篇（Day 20）**：本篇承接該篇多工具流程「哪些資訊該記、哪些該重查」的銜接問題
