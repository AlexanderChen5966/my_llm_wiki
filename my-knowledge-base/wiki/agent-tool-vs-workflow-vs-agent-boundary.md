---
title: Tool、Workflow、Agent 三層判斷法：何時該讓系統動態決策
date: 2026-09-03
tags: [ai/agent, type/concept]
source: https://ithelp.ithome.com.tw/articles/10405442（Data Machi 30 天學習系列 Day 16）
type: article
status: processed
asset_value: 64
---

## T（核心論點）

Tool Use 不等於 Agent：即使系統擁有很多工具，只要每次執行順序都由程式事先決定，仍然只是固定 Workflow。真正的 Agent 特性是**能根據前一步 Tool Result 重新判斷下一步該做什麼**（Reason → Act → Observe 循環）。可用三個問題快速判斷一個功能該放在哪一層：只需完成一件明確的事→ Tool；步驟能事先列清楚→ Workflow；下一步必須看中間結果才知道→ Agent。Agent 不是 Workflow 的升級版，而是**適合處理另一種問題類型的設計方式**——每多一次模型判斷就多一次 Token、等待時間與走錯路機會，固定規則能完成的不必為了 Agentic 而增加複雜度；且企業 Agent 一定要有邊界（可用 Tool 有限、資料範圍有限、嘗試次數有限、高風險動作需人工確認）。

## B（適用場景）

**適用：** 判斷新功能該做成 Tool、Workflow 還是 Agent；設計企業 Agent 的自主性邊界（Allowed Tools、Maximum Tool Calls、需要 Clarify 的情境、高風險操作確認機制）；避免把所有東西都設計成 Agent 造成不必要的成本與不穩定性。

**不適用 / 注意：** 本篇聚焦 Tool/Workflow/Agent 的分界判斷，尚未展開 Agent 內部的 Reason-Act-Observe 具體機制與停止條件（見 [[agent-react-reason-act-observe-stop-conditions]]）。

## R（操作步驟）

1. **三層判斷法**：① 只需完成一件事（查表、搜文件）→ Tool；② 步驟能事先列清楚且每次流程大致相同 → Workflow；③ 下一步必須依賴中間結果或需要語意判斷 → Agent。
2. **CDP 縮寫案例示範 Agent 特性**：查 `get_project_status("CDP")` 回傳 No project found，Agent 觀察後重新判斷「CDP 可能是縮寫」，改查 `search_documents("CDP")` 找到正式名稱，再重新查專案——**前一次 Tool Result 會改變下一次行動**，這是 Agent 與單次 Tool Calling 最關鍵的差異。
3. **比較表判斷適用性**：步驟固定/條件明確/每次流程相同 → Workflow 更合適；問題類型多/下一步依賴中間結果/需語意判斷 → Agent 更合適。
4. **設定企業 Agent 邊界四要素**：可用 Tool 有限（明確 Allowlist）、資料範圍有限（Tool 仍需遵守權限邊界）、嘗試次數有限（Maximum Tool Calls）、高風險操作（delete/send_email）需明確確認機制。
5. **兩種情境必須詢問使用者**：找到多個名稱相近的候選（如兩個 CDP 專案）時不應自行挑選，應回問使用者。

## C（關鍵行動）

- [ ] 新功能上線前先用三層判斷法確認該做成 Tool、Workflow 還是 Agent，避免不必要的 Agentic 化
- [ ] 為 Agent 設定 Allowed Tools、資料範圍、Maximum Tool Calls 三項明確邊界
- [ ] 高風險操作（刪除、寄信、修改狀態）必須加上人工確認機制，不能讓 Agent 單獨決定執行
- [ ] 找到多個相似候選（專案名稱、文件版本）時應回問使用者，而非自行猜測
- [ ] 定期檢視：是否有原本用固定規則就能完成、卻被改成 Agent 而增加不必要成本與不穩定性的功能

## 原始摘要

「Data Machi 30 天學習系列」Day 16：釐清 Tool Use、Workflow、Agent 三者分界，強調 Agent 關鍵特性是「前一次 Tool Result 會改變下一次行動」，並用 CDP 縮寫案例示範。反對什麼都做成 Agent，因為每多一次模型判斷代表更多 Token、等待時間與走錯路機會；企業 Agent 必須有明確邊界（可用 Tool、資料範圍、嘗試次數有限，高風險動作需確認）。系列後續重點將從「怎麼讓 AI 做更多事」轉向「怎麼讓 AI 決策過程可被觀察、驗證與控制」。下一篇（Day 17）介紹 ReAct，把 Agent 決策循環拆得更清楚。

## 相關連結

- [[agent-cross-source-workflow-partial-success-traceability]] — **系列上一篇（Day 15）**：本篇承接該篇固定 Deterministic Workflow 的限制，說明何時真正需要 Agent
- [[agent-react-reason-act-observe-stop-conditions]] — **系列下一篇（Day 17）**：把本篇提到的 Reason→Act→Observe 循環拆解成具體機制與停止條件
- [[agent-loop-to-graph-workflow-signals]] — 既有條目：討論 Loop 何時該升級為 Graph/Workflow 的五個訊號，與本篇 Tool/Workflow/Agent 三層判斷法層級相鄰但分界不同（該篇聚焦「何時從單一 Loop 走向多節點 Graph」，本篇聚焦「何時從固定流程走向動態決策」），可互相對照
