---
title: ReAct 機制層：Observation 可能改變下一輪查詢，Tool 失敗也是一種 Observation
date: 2026-09-03
tags: [ai/agent, type/concept]
source: https://ithelp.ithome.com.tw/articles/10405480（Data Machi 30 天學習系列 Day 17）
type: article
status: processed
asset_value: 64
---

## T（核心論點）

ReAct（Reason → Act → Observe）的核心價值不是「模型會思考」，而是**下一個 Tool Call 的輸入來自前一個 Tool Result，而不是一開始全部猜好**。本篇聚焦兩個機制層重點：① **Observation 不只是判斷「有沒有資料」，還可能改變下一輪查詢本身**——原始問題會隨著取得的事實逐漸變得更具體（例如從「Top Category 是什麼」演變成「Delivery Issue 的正式定義」再到「Last Mile Delivery Issue 的改善專案」）；② **Tool 失敗（Timeout、Permission Denied）本身也是一種 Observation**，且 Permission Denied 應解讀為「不知道是否存在」而非「沒有相關專案」，兩者狀態完全不同。ReAct 不代表可以無限循環，必須搭配程式層邊界（Maximum Steps、Tool Calls、Timeout、Cost Budget、Stop Conditions）。

## B（適用場景）

**適用：** 設計 Agent 的 Reason-Act-Observe 決策循環；判斷 Tool 回傳異常結果（查無資料、權限不足）時該如何重新推理而非直接下結論；設定 Agent 停止條件；保留可除錯的執行紀錄（Tool、Status、Observation、Input）。

**不適用 / 注意：** 本篇不涉及 ReAct 循環如何用 Graph/State/Node 視覺化管理（後續 LangGraph 篇章處理）；不是所有問題都需要 ReAct，高風險固定流程仍適合明確 Workflow。

## R（操作步驟）

1. **Reason 階段形成可執行決策**：不是輸出一長串內部思考文字，而是判斷「目前缺少什麼、需要哪個 Tool」，讓模糊需求轉成明確執行動作。
2. **Act 階段事實必須來自 Tool**：模型判斷「應該比較各市場需求量」，但真正的數字（1,280 / 1,640 / 320）必須來自 Tool 與程式計算，不能由模型自己補。
3. **Observe 階段重新比較任務是否完成**：取得 Hong Kong 需求最多後，仍要檢查原本任務（「相關改善專案有沒有在進行」）是否完成，未完成則決定下一步查什麼。
4. **CDP 縮寫案例示範 Observation 改變下一輪查詢**：查無 CDP 專案 → Reason「CDP 可能是縮寫」→ 查文件找到正式名稱 → Reason「現在可以重新查專案」→ 查到正確進度。
5. **Tool 失敗也是 Observation**：Permission Denied 不等於「沒有相關專案」，應誠實回答「專案系統回傳權限不足，暫時無法確認」，而非把權限問題解讀成資料不存在。
6. **設定程式層停止條件**：Maximum Steps、Maximum Tool Calls、Timeout、Allowed Tools、Cost Budget、Stop Conditions 六類限制，避免模型無限循環查詢。
7. **四種停止時機**：已取得足夠證據；缺少必要資訊需使用者確認（如兩個相似專案名稱）；無權限或來源不可用；已超過合理成本。
8. **保留執行紀錄而非模型內部推理**：記錄 Tool、Status、Observation、Input，供除錯、成本追蹤與企業稽核使用，比一段模糊的「Agent 思考過程」更實用。

## C（關鍵行動）

- [ ] 為 Agent 設定 Maximum Steps / Tool Calls / Timeout / Cost Budget 四項程式層邊界，不只寫在 Prompt 裡
- [ ] Tool 回傳查無資料時，先判斷是否可能是縮寫或名稱不一致，再決定是否改查其他來源確認正式名稱
- [ ] Permission Denied 與「沒有相關資料」必須用不同狀態回應使用者，不可混為一談
- [ ] 找到多個相似候選時應停止並詢問使用者，而非自行猜測
- [ ] 保留每一步的 Tool / Status / Observation / Input 紀錄，而非只保存模型內部推理文字

## 原始摘要

「Data Machi 30 天學習系列」Day 17：介紹 ReAct（Reason→Act→Observe 循環），強調 Observation 不只是判斷有無資料，也可能改變下一輪查詢本身（問題逐步變具體），且 Tool 失敗本身也是一種 Observation。ReAct 須搭配程式層邊界避免無限循環，四類停止條件為已取得足夠證據、缺必要資訊需確認、無權限或來源不可用、已超過合理成本。下一篇（Day 18）比較 Router 與 Coordinator 兩個角色。

## 相關連結

- [[agent-tool-vs-workflow-vs-agent-boundary]] — **系列上一篇（Day 16）**：本篇承接該篇 Tool/Workflow/Agent 三層判斷法，把 Agent 的決策循環拆解成具體機制
- [[agent-router-vs-coordinator-state-tracking]] — **系列下一篇（Day 18）**：ReAct 循環延伸到 Coordinator 角色如何持續追蹤任務狀態
- [[react-planning-dynamic-adjustment]] — 既有條目：ReAct × Planning 結合處理突發狀況（旅遊行程案例），屬應用層；已核對兩篇全文非重複——本篇是 ReAct 迴圈機制與停止條件本身（Observation 改變下一輪查詢、Tool 失敗即 Observation），該篇是把 ReAct 結合 Planning 用於動態調整既定計畫的應用案例
- [[langgraph-react-planning-visualization]] — 既有條目：把 ReAct × Planning 循環視覺化成 LangGraph 節點，屬視覺化/實作層；已核對非重複，可與本篇機制層互相對照
