---
title: Agent 資訊不足時的決策框架：Clarify、Re-query、Verification 或拒答
date: 2026-09-03
tags: [ai/agent, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10407137（Data Machi 30 天學習系列 Day 22）
type: article
status: processed
asset_value: 68
---

## T（核心論點）

當手上資訊不足時，可靠的企業 Agent 不是「永遠都有答案」，而是要先判斷缺的是哪一種資訊，避免走向兩個極端：什麼都問使用者（即使前文已提供條件仍反覆確認），或什麼都不問就用模型常識自己猜。核心判斷可記成三件事：**問題不清楚就 Clarify、資料可能過期就 Re-query、問題清楚但證據不足就 Verification 或補查。** 若完成這些步驟後仍無足夠證據，才應明確說明無法回答，而不是用常識把缺口補滿——企業 Agent 最危險的幻覺，是把推論寫成事實。

## B（適用場景）

**適用：** 設計 Agent 在資訊不足/資料可能過期/證據不足三種情境下的分流邏輯；建立「不能被模型自由決定」的驗證規則（Verification）；避免 Agent 把「沒找到」偽裝成「不存在」；區分 Verified Fact 與 Analytical Hypothesis 的回答格式設計。

**不適用 / 注意：** 本篇聚焦單輪/多輪對話內的資訊不足判斷，屬於承接 Memory（Day 21）之後的下一層決策；不涉及整體 Workflow 控制架構（見 [[agent-loop-to-graph-workflow-signals]]）。

## R（操作步驟）

**三種情境分流：**

| 情況 | 正確行為 |
|---|---|
| 使用者問題缺少必要條件（意圖或查詢條件不明） | Clarification |
| 問題清楚，但要求最新動態資料 | Re-query |
| Tool 有結果，但不足以支持完整結論 | Verification / 補查 |
| 來源互相衝突 | 顯示衝突並驗證 |
| 所有來源都沒有足夠證據 | 拒答該部分 |
| 部分問題有證據、部分沒有 | Partial Answer |
| 模型只有分析推論 | 標示 Analytical Hypothesis |

**Clarification 三原則：** 一次只問真正阻擋下一步的關鍵問題；能提供具體選項就不要只問開放式問題；讓使用者知道為何需要確認。已知的條件不要重問——若前文已知 Market 與 Metric，只缺 Period，就只問 Period，不要要求「請提供市場、指標與時間範圍」。

**Re-query 判斷**：兩個問題——這份資訊本身是否可能變動？使用者是否明確要求最新/現在/重新確認？任一為是即應提高 Re-query 優先級。使用者說「你確定嗎？我剛更新資料」時，即使舊結果剛查一分鐘，也應理解為 `force_refresh = true`，並在回答中比較新舊結果。

**Verification 檢查清單（多數可用 deterministic rule 完成，不必全交給另一層 LLM）：**

| Verification | 要確認什麼 |
|---|---|
| Number Check | 回答中的數字是否存在於 Tool Result |
| Filter Check | 日期、市場與條件是否和 Query 一致 |
| Source Check | 文件敘述是否有來源 |
| Tool Status Check | Tool 失敗時是否假裝成功 |
| Freshness Check | 動態資料是否仍在有效期限 |
| Secret Check | 回答是否包含 Secret 或敏感設定 |
| Error Sanitization | 是否洩露內部 Stack Trace 或系統細節 |

**錯誤狀態需保留明確類型**，不要全部包裝成「沒有資料」：

| 狀態 | 代表意思 | 下一步 |
|---|---|---|
| No Result | 查詢成功但沒有資料 | 改查條件、補查或回報沒有結果 |
| Permission Denied | 無權限取得資料 | 說明權限限制，不是說沒有專案 |
| Timeout | 來源暫時不可用 | 可重試有限次或部分回答 |
| Missing Input | 缺少必要條件 | Clarification |
| Invalid Source | 資料來源設定錯誤 | 回報系統問題，不自行猜答案 |

**Verified Fact 與 Analytical Hypothesis 必須分開標示**：已確認事實必須來自 Tool/RAG/Database/Source；分析推論可由 LLM 產生，但必須清楚標記為「待驗證假設」。例如轉換率下降的原因分析，應先列「已確認事實：轉換率由 4.8% 降至 3.9%，同期 Paid Traffic 占比由 25% 升至 40%」，再另外標示「待驗證假設：流量組合改變可能是原因之一，但需進一步控制其他因素才能確認」。

**Partial Answer**：問題部分有證據、部分沒有時，不需整題拒答，應分別交付「Verified Part 正常交付」與「Missing Evidence Part 清楚標記」。

## C（關鍵行動）

- [ ] Clarification 只問前文真正沒有提供、且會阻擋下一步的條件，不要把已知條件重新問一遍
- [ ] 使用者出現「最新/現在/目前/剛剛/我剛更新/再查一次/重新確認」等語句時，一律提高 Re-query 優先級
- [ ] 把可程式化的驗證（數字比對、Filter 一致性、Tool 狀態、敏感資訊過濾）用 deterministic rule 完成，不必都交給另一個 LLM 判斷
- [ ] 錯誤狀態保留明確類型（No Result / Permission Denied / Timeout / Missing Input / Invalid Source），不要全部說成「沒有資料」
- [ ] 回答中明確區分「已確認事實」（必須有 Tool/RAG 來源）與「待驗證假設」（LLM 推論），不要把邏輯填補寫成事實

## 原始摘要

「Data Machi 30 天學習系列」Day 22：提出企業 Agent 面對資訊不足時應依情境選擇 Clarification（問題不清楚）、Re-query（資料可能過期）、Verification/補查（證據不足以支持結論）或明確拒答，而非用模型常識硬填答案。特別區分「Verified Fact」與「Analytical Hypothesis」，主張許多驗證可用 deterministic rule（程式規則）完成，不必全部交給另一個 LLM。下一篇（Day 23）處理這些能力全部塞進同一 Agent Loop 導致難以控制的問題，正式帶入 LangGraph。

## 相關連結

- [[agent-memory-three-layers-reuse-vs-requery]] — **系列上一篇（Day 21）**：本篇的 Re-query 判準直接承接該篇的 Reuse vs Re-query 框架與 TTL 概念
- [[agent-loop-to-graph-workflow-signals]] — **系列下一篇（Day 23）**：本篇 Clarify/Re-query/Verify/Retry 全部塞進同一 Agent Loop 後難以控制的問題，正是該篇的起點
- [[agent-error-recovery-taxonomy]] — 錯誤處理與重試分類的既有框架，可與本篇「錯誤狀態保留明確類型」表格互相對照
- [[agent-reliability-timeout-retry-fallback-verification]] — Timeout/Retry/Fallback/Verification 的可靠性設計，補充本篇 Verification 檢查清單的實作細節
- [[rag-refusal-evidence-gate-threshold-calibration]] — ⚠️ **同天數（Day 22）不同系列**：該篇是 RAG 檢索分數的量化拒答門檻機制（系統工程/評測方法論層級），本篇是通用 Agent 對話決策框架（Clarify/Re-query/Verification，對話層級行為分類），角度互補、各自獨立收錄，避免誤判重複
