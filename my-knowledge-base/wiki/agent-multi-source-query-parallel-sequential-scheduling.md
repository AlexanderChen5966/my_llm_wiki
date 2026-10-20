---
title: 多來源查詢安排：Agentic Search、Dynamic Fan-out 與 Agent 停止條件
date: 2026-09-03
tags: [ai/agent, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10405767（Data Machi 30 天學習系列 Day 20）
type: article
status: processed
asset_value: 64
---

## T（核心論點）

判斷多工具流程時，第一個問題不是「有幾個 Tool」，而是「**下一個步驟的輸入是否依賴前一個步驟的輸出？**」——無依賴可 Parallel Execution 降低總等待時間，有依賴則須 Sequential Execution，否則即使每個 Tool 都成功，整段流程仍是錯的。複雜問題要先做 Decomposition（拆解）並判斷資料依賴，才能決定 Parallel/Sequential 安排，這稱為 **Agentic Search**：每次新搜尋都建立在前一次已取得資訊之上，可動態補查，而非單次搜尋。Agent 必須知道何時停止查詢（已取得關鍵證據、額外查詢不再降低不確定性、已達執行上限、必要資訊只能由使用者提供時應 Clarify），且當工作量只有執行後才知道時（如按章節動態產生摘要任務），需要 **Dynamic Fan-out** 搭配 Fan-in 彙整，同時還要考慮 API Rate Limit 與並行寫入衝突。

## B（適用場景）

**適用：** 判斷多來源查詢該平行還是循序執行；把模糊的複合問題拆解成子問題並標出資料依賴；設計會議錄音→行動項目這類多節點 Workflow（音訊處理與 LLM 結構化整理應分開測試）；定義 Agent 的決策邊界（可用 Tool、問題拆解上限、Parallel/Sequential 規則、Maximum Steps、Clarification 條件、停止條件）。

**不適用 / 注意：** 本篇的 Agentic Search 概念承接 Day 15 的資料依賴判斷，但加入了「觀察→補查→重新拆題」的動態能力；不涉及跨輪對話 State 管理（見既有條目 [[agent-memory-three-layers-reuse-vs-requery]]）。

## R（操作步驟）

1. **判斷 Parallel vs Sequential 的核心原則**：下一步輸入是否依賴前一步輸出？獨立則 Parallel（如「今年總工單數」與「差旅報銷規範」可同時查詢）；有依賴則 Sequential（如「哪類需求成長最高」必須先知道結果才能查該類定義）。
2. **先做 Decomposition 再判斷依賴**：把「最近三個月核心指標為什麼下降？正式定義是什麼？改善專案做到哪裡？」拆成子問題表（子問題／需要的能力／可能來源），再檢查依賴關係決定執行安排。
3. **Agentic Search 循環**：拆問題 → 搜尋 → 觀察結果 → 資訊足夠嗎 → 否則重新拆題或補查，是則整合回答；每次新搜尋建立在前一次已取得資訊之上（如查無專案 → 改查文件確認正式名稱 → 再查專案）。
4. **會議錄音案例：Speech-to-Text 與 LLM 結構化整理分開測試**：先確認 Transcript 是否正確，再檢查 Summary/Action Items 是否合理，避免把 Audio→AI→結果 包成一步，錯誤時無法定位是哪個節點出問題。
5. **結構化輸出允許空值**：Assignee、Due Date 查無資料時應回傳 `null`，而非讓 AI 自行補一個看起來合理的值。
6. **Action Item 保留來源與 Traceability**：Source（會議日期）、Transcript Reference（時間戳）、Created At，讓使用者日後能追問「為什麼有這個任務」。
7. **定義四種停止條件**：已取得關鍵證據；額外查詢不會明顯降低不確定性；已達執行限制（Maximum Steps/Tool Calls/Timeout）；必要資訊只能由使用者提供時應 Clarify 而非繼續搜尋。
8. **Dynamic Fan-out 搭配 Fan-in**：只有執行後才知道下游工作量時（如按章節動態產生摘要任務），需動態產生工作並等待全部完成後彙整；同時考慮 API Rate Limit 與同一份資料的並行寫入衝突，判斷不是「能不能平行」而是「這些工作彼此獨立嗎？外部系統允許平行執行嗎？」。

## C（關鍵行動）

- [ ] 複合問題先做 Decomposition，建立子問題與資料依賴表，再決定 Parallel 或 Sequential 執行
- [ ] 多節點 Workflow（如語音轉文字→結構化整理）的每個節點分開測試，不要包成一步
- [ ] 結構化輸出允許空值（null），不讓 AI 自行補一個看似合理的答案
- [ ] Action Item 保留 Source／Transcript Reference／Created At，確保可追溯
- [ ] 為 Agent 標記四類停止條件（關鍵證據已足夠／額外查詢無效益／已達執行上限／需使用者澄清），並在 Dynamic Fan-out 情境額外檢查 API Rate Limit 與並行寫入衝突

## 原始摘要

「Data Machi 30 天學習系列」Day 20：處理多工具 Agent 如何安排 Parallel/Sequential 執行順序，核心問題是「下一步輸入是否依賴前一步輸出」。介紹 Agentic Search（拆題→搜尋→觀察→視資訊足夠與否決定補查或整合）概念，及 Agent 停止條件。以會議錄音案例示範多節點 Workflow 需分層測試、結構化輸出允許空值、Action Item 需保留 Traceability。文末延伸至 Dynamic Fan-out 需搭配 Fan-in 彙整。本篇是 Data Machi Day15→30 系列鏈末端銜接篇，下一篇（Day 21）進入 Agent Memory 主題。

## 相關連結

- [[agent-coordinator-tool-routing-accuracy-test]] — **系列上一篇（Day 19）**：本篇把該篇的 Coordinator 放進更複雜的多來源任務，處理 Parallel/Sequential 安排
- [[agent-memory-three-layers-reuse-vs-requery]] — **系列下一篇（Day 21，既有條目）**：本篇的多工具流程接下來要面對「哪些資訊該記、哪些該重查」的 Memory 與 State 管理問題，正是該篇的起點
