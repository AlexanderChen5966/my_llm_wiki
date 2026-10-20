---
title: 可靠性四機制：Timeout / Retry / Fallback / Verification 各自解決不同層次
date: 2026-09-08
tags: [ai/agent, dev/architecture, type/concept]
source: https://ithelp.ithome.com.tw/articles/10408173（《Data Machi 30 天》Day 26）
type: article
status: processed
asset_value: 80
---

**系列：《Data Machi 30 天學習系列：從 Chat 到 Product》Day 26**。
前一天的 Workflow 拆解見 [[langgraph-coordinator-refactor-seven-nodes]]，
下一天把後端狀態轉成前端體驗見 [[agent-ux-status-transparency-maturity]]。

## T（核心論點）

⚠️ **先說與 [[agent-error-recovery-taxonomy]] 的分工**（兩篇同為 asset_value 80，主題相鄰但切面不同，不是重複條目）：

| 條目 | 切面 | 回答的問題 |
| --- | --- | --- |
| [[agent-error-recovery-taxonomy]] | **錯誤分類學** | 錯誤怎麼分類？八種 Recovery 中下一步該怎麼改？ |
| 本條目 | **可靠性機制實作層** | 四機制怎麼配置？Idempotency 與熔斷的邊界在哪？ |

在此分工下，本篇的核心論點是：

> **外部服務一定會失敗，所以要把失敗當成 Workflow 的正常分支。**
> 核心問題不是「服務有沒有成功」，而是 **「失敗之後，系統知不知道接下來該怎麼做？」**

而最常被混談的四個機制其實解決不同層次的問題：

| 機制 | 解決的核心問題 |
| --- | --- |
| **Timeout** | 一次外部呼叫最多等多久 |
| **Retry** | 暫時性失敗是否值得重新嘗試 |
| **Fallback** | 主要服務失敗後是否有替代路徑 |
| **Verification** | 成功取得結果後，內容是否真的可靠可用 |

一次呼叫可能同時經過四者：Timeout → Retry ×2 → 切 Fallback Model → 仍須進 Verification。
**可靠性不是「API 回 200 就成功」，而是一連串不同層次的品質控制。**

## B（適用場景）

**適用：**
- Agent 從 Demo 走向正式產品前的可靠性盤點（本篇附 9 項 Reliability Test 可直接照抄）
- 系統已有 Action Tool（寄信、建 Task、寫 DB）而 Retry 策略還是一套通用規則時
- 多來源 Workflow 需要決定「一個來源掛掉時要不要整批失敗」

**不適用 / 注意：**
- ⚠️ **Circuit Breaker 在小型 Demo 還不必要**，使用者與外部服務數量增加後才逐漸重要
- 🔴 Reliability 的目標不是讓成功率變成 100%——只要依賴網路、第三方 API 與模型服務，就不存在永不失敗的產品
- 本篇是設計原則層，非特定框架 API

## R（操作步驟）

### 1. Timeout 依 Node 分化，不要全系統一個 30 秒

Router 屬快任務→短逾時；長文件摘要／最終報告生成→允許較長。依 **任務特性、服務正常延遲、
使用者等待成本** 三項決定。沒有 Timeout 的代價不只是多等，而是整條 Workflow 持續佔用資源，
前端也分不出「還在算」和「已經沒回應」。

### 2. Retry 必須建立在 Error Classification 上

| 錯誤類型 | 適合 Retry | 處理 |
| --- | --- | --- |
| Timeout | 是 | 有限重試 |
| 429 Rate Limit | 是 | 等待後重試 |
| 暫時性 5xx | 視情況 | 有限重試 |
| 401 認證錯誤 | **否** | 停止並檢查 Credential |
| 403 權限不足 | **否** | 停止並說明權限 |
| Invalid Input | **否** | 修正輸入或要求補充 |
| No Result | 通常不是 | 改寫 Query、補查，或回報沒有結果 |

配 **Exponential Backoff（1→2→4 秒）＋ Jitter**（避免大量使用者故障後同時重送形成新一波尖峰），
並且一定要有 **Retry Limit（2–3 次）**，否則 Retry 自己變成另一個無限迴圈。

### 3. 🔑 Read Tool 與 Action Tool 的 Retry 策略不能相同

Sheets 查詢 Timeout 重查無妨（Read Operation）；但 `create_task()` Timeout 時，
**你不知道是完全沒建立，還是已經建立成功只是回應在網路中途遺失**——盲目 Retry 會得到兩筆
一模一樣的 Task。因此 Action Tool 需要 **Idempotency（冪等性）**：每個 Action 帶唯一
Request ID，外部系統若已處理過就不再重複建立。

同理，Send Email／Update Database 這類 Action Node **不應因 Timeout 就直接重跑**。

### 4. Fallback 的倫理界線：可以用 Cache，但必須揭露資料時間

主模型不可用→切替代模型；即時來源不可用→在特定情境用最近一次已驗證結果。
但 🔴 **使用者問「現在庫存多少」，偷偷拿三小時前的 Cache 當即時資料回答，這不叫可靠性，
只是把服務錯誤藏起來**。至少要說明「即時來源無法存取，以下結果來自最近一次成功查詢」。
沒有可信替代時，**停止並清楚說明無法完成哪一部分反而是最可靠的選擇**。

### 5. Verification 處理「技術上成功、業務上失敗」

HTTP 200 但空陣列、來源 Schema 剛好改動、欄位消失、JSON 被截斷（只回了 `{"summary": "..."` ）、
模型只生成一半——全都算失敗。判準只有一句：**這個結果能不能安全地被下一個 Node 使用？**

### 6. Partial Success：State 不能只有 success / failed

三來源中兩成功一 Timeout，不該整批回「系統錯誤」。保留每個 Tool 的 status，
由 Coordinator 判定 **Complete / Partial Success / 完全無法交付**。Tool 越多，
「全部服務永遠同時正常」就越不該被當成前提。

### 7. Log 要同時滿足兩件看似矛盾的事

**記**：Request ID、Node、Tool Name、Error Type、HTTP Status、Retry Count、Duration、是否用 Fallback。
**不記**：API Key、Access Token、Credential、完整 Prompt、使用者敏感資料。
Agent 能存取企業資料後，Observability 本身也受資料治理限制。

### 8. Circuit Breaker：不要讓每個使用者都重撞一次

某來源連續多次 503 時，暫時標記 unavailable 直接走 Fallback 或回傳清楚狀態，
健康檢查確認恢復後再開放。否則每個新使用者都經歷一次 Timeout→Retry→再 Timeout，
還會繼續加重已故障服務的負擔。

### 9. 主動注入故障來測試，不要等正式環境壞掉

| 測試案例 | 預期行為 |
| --- | --- |
| 模型 Timeout | 結束等待 → 有限 Retry |
| 429 Rate Limit | Backoff 後 Retry |
| 401 錯誤 API Key | **不 Retry**，回報認證問題 |
| 403 權限不足 | **不 Retry**，說明權限 |
| 主模型持續失敗 | 切換 Fallback |
| Tool 回空結果 | Verification 判斷是否可接受 |
| JSON 被截斷 | Validation 失敗，不送往下游 |
| 一個來源失敗 | 保留其他結果，回 Partial Success |
| Action Timeout | 確認 Idempotency，不盲目重跑 |

目標不是證明系統不會失敗，而是證明 **每一種已知失敗都有合理而可預期的路徑**。

### 10. 責任分兩層

- **Tool Layer**：Timeout、Response Parsing、Error Classification、格式驗證。
  🔴 **不可把所有異常包成一個 `None`**，否則 Workflow 分不出 Permission Error、Timeout 還是 No Result
- **Workflow Layer**：決定 Retry / Fallback / Verification / Partial Success / 停止，管理 Retry Count 與 Task Status

一句話：**Tool 回答「這一次呼叫發生什麼」，Workflow 回答「接下來該怎麼辦」。**

## C（關鍵行動）

- [ ] 盤點自己 Agent 的所有 Tool，標出哪些是 Read、哪些是 Action；為每個 Action 補上唯一 Request ID
- [ ] 檢查現有 Retry 是否對 401／403／Invalid Input 也在重試（這是最常見的浪費）
- [ ] 若有用 Cache 當 Fallback，補上資料時間的揭露文案
- [ ] 照第 9 節那張表跑一次故障注入測試，記錄每一條實際走到的路徑
- [ ] 檢查 Log 是否寫入了完整 Prompt 或 Token

## 原始摘要

Data Machi 系列 Day 26。主張正式產品要把失敗視為 Workflow 的正常分支，並釐清 Timeout／Retry／
Fallback／Verification 四機制各自解決不同層次。最關鍵的兩個判準：Retry 必須建立在 Error
Classification 上（401/403/Invalid Input 重試一百次也不會好），以及 Read Tool 與 Action Tool
的 Retry 策略不能相同（create_task() 重試會建兩筆，需 Idempotency）。另強調 Fallback 用 Cache
必須揭露資料時間、Reliability Policy 應按 Node 分化、Circuit Breaker 的適用時機，
以及用主動注入故障的方式驗證每種已知失敗都有可預期路徑。

## 相關連結
- [[agent-error-recovery-taxonomy]] — 🔑 **姊妹條目，分工見本篇 T 層**：該篇＝錯誤分類學（八型錯誤對應八種 Recovery 策略），本篇＝可靠性機制實作層（四機制配置、Idempotency、Circuit Breaker、Cache Fallback 的倫理界線）
- [[langgraph-coordinator-refactor-seven-nodes]] — 同系列 Day 25：本篇是它那張「錯誤策略表」的完整展開
- [[agent-ux-status-transparency-maturity]] — 同系列 Day 27：本篇的 Error Classification 在該篇轉成使用者可行動的訊息
- [[enterprise-ai-permission-three-layers-governance]] — 同系列 Day 28：本篇的 Log 規範在該篇歸入資料治理
- [[agent-loop-stop-conditions]] — Retry Limit 與連續失敗的最小實作版
- [[graph-engineering-node-edge-state]] — Reliability Policy 按 Node 分化的編排基礎
- [[production-agent-architecture]] — 生產級 Agent 的 Guardrails 與冪等設計
- 📎 2026-10-19：[[claude-platform-opentelemetry-beta]] — Claude 平台 OTel trace：定位等待與失敗位置；🔴 trace 是 best-effort，缺紀錄不等於沒發生，帳務對帳另用 Usage API
