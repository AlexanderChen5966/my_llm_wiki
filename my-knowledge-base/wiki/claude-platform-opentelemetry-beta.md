---
title: Claude Platform OpenTelemetry（2026-10 Beta）：追蹤 Messages API 與 Managed Agents 的呼叫過程
date: 2026-10-19
tags: [ai/claude, ai/agent, type/how-to]
source: https://www.aiposthub.com/claude-opentelemetry-api-tracing-beta-guide/
official_refs:
  - https://platform.claude.com/docs/en/release-notes/overview
  - https://platform.claude.com/docs/en/manage-claude/opentelemetry
  - https://platform.claude.com/docs/en/manage-claude/opentelemetry/api
type: reference
status: processed
asset_value: 51
---

Claude Platform 於 2026-10-14 在 release notes 列入 OpenTelemetry（OTel）Beta 能力，涵蓋 Messages API 與 Claude Managed Agents，讓團隊用 trace／span 追查一次 AI 工作的等待與失敗位置。

> ⚠️ 官方連結未經本次核對（2026-10-19）；內容經 aiposthub（Philo，2026-10-15）單一媒體轉述，原始出處為 Claude Platform 官方文件。Beta 規格可能變動。原文**未給 span 欄位名與 SDK 設定程式碼**，本條也不自行補寫，接線時以官方文件為準。作者未實測接線。
>
> 📎 2026-10-19 Team Lead 核對：已讀官方〈OpenTelemetry for the Claude API〉（status: beta），**預設 `metadata_only`、完整內容模式、best-effort 傳送、對帳改用 Usage API、API tracing 需 Admin** 皆與本條一致。官方另載（補第 32 行「原文未列細節」）：① 組織有 HIPAA／FERPA／K-12 資料處理限制時，**API 目的地一律只輸出中繼資料**（SDK spans 不受影響）；② ZDR 下仍可用完整內容，Messages API 的輸出內容只暫存記憶體直到送達；③ **Claude Managed Agents 不適用 ZDR**，開 OTel 也不改變其資料保留；④ 完整內容每個 span 預設上限 0.75 MB，超過會截斷並設 `anthropic.content.truncated=true`；⑤ API tracing 支援 Claude Platform 與 Claude Platform on AWS；⑥ Claude Code／Agent SDK／Cowork 各有獨立監控文件，不走本機制。API 接入頁與 release notes 未核對。

## T（核心論點）

OTel 補上的是**呼叫過程的可見性**，不是任務品質保證：它讓團隊分辨「慢」是發生在網路、模型還是外部工具（例如模型回覆很快、但查訂單佔了大部分時間，改提示詞就無效）。三條邊界要先記住：① 預設 `metadata_only`，開完整內容模式會把 prompt 與回應送到目的地；② 輸送是 **best-effort**，缺紀錄不等於沒發生；③ **帳務對帳用 Usage API**，不要拿 trace 加總當帳單。這是 [[production-agent-architecture]] Observability 模組在 Claude 平台上的官方接法。

## B（背景）

**適用：**
- 工作包含多次模型與工具呼叫、常難以定位等待或失敗的團隊
- 已有監控平台、想讓 Claude 呼叫出現在既有 trace 畫面

**不適用 / 注意：**
- 單純想知道整月花費 → 先用 Usage API 做好用量核對（見 [[claude-code-billing-usage]]）
- 🔴 完整路徑順利 ≠ 內容正確：每個呼叫都成功，答案仍可能錯，品質驗收要另做
- Messages API 與 Managed Agents 的**資料處理資格不完全相同**（官方），有資料保留要求要逐項核對，不能用同一承諾概括（原文未列細節）

## R（關鍵細節）

### 規格（官方，經轉述）

| 項目 | 內容 |
|---|---|
| 發布 | 2026-10-14 release notes，Beta |
| 涵蓋 | Messages API、Claude Managed Agents |
| 追蹤來源 | **SDK 端**與 **API 服務端**提供不同觀察範圍，兩者互補（不是兩份獨立工作，不能直接加總） |
| 預設模式 | `metadata_only`：識別資訊、時間、狀態、token 計數、工具名稱等 |
| 完整內容模式 | prompt 與回應也會送到設定的目的地 |
| 權限 | 設定目的地需**管理員權限** |
| 傳輸 | OTLP over HTTP、protobuf |
| 目的地 | 須為**可公開連線的 HTTPS**、**443 埠**；自架 Collector 的常見預設埠不符，需調整對外接入方式 |
| 測試 | Console 測試會送出 `anthropic.test` 測試 span（只證明基本路徑可用） |
| 送達 | best-effort，不保證每段紀錄送達 |
| 對帳 | 完整用量核對應使用官方 **Usage API** |

### 名詞

- **trace**：同一次工作的整條路徑；**span**：路徑中某一段工作的紀錄；兩者靠共同識別資訊串接
- **Collector**：接收與轉送觀察資料的程式

## 作者觀點（aiposthub／Philo）

- 先用 `metadata_only`；工具名稱與工作名稱本身也可能透露業務資訊，中繼資料模式不能取代欄位、權限與保存時間的檢查；接收端存取權限也要管
- 從**單一專用測試工作區**、用不含真實個資的資料開始；健康狀態只是起點，要自己送一筆可辨識的測試請求（記下請求代號）並包含一個可預期的失敗情境來驗收
- 驗收後記下當時的模式、工作區與目的地設定
- 🔑 為什麼 trace 的 token 總和不能當帳單：同一條 trace 的**上下層 span 可能都帶用量**，直接相加會重複計算；重試可能是重複紀錄、也可能真的重新執行，對費用意義不同
- 分工：用 trace 找出哪條工作流用量突增，再用完整用量資料確認整體變化
- 先定義追蹤要回答的問題（哪一段最慢、哪種工具常失敗），再決定要收哪些紀錄

## C（連結與應用）

- [ ] 若接入：先列出要回答的 2–3 個問題，選一條最常出事的工作流
- [ ] 目的地確認 HTTPS／443／OTLP HTTP＋protobuf，自架 Collector 先核對對外埠
- [ ] 預設維持 `metadata_only`；要開完整內容前先盤點會帶出的欄位與接收端權限
- [ ] 統計用量時固定採哪一層 span，帳務一律以 Usage API 為準
- [ ] 待追蹤：Beta 結束後的規格與欄位定義；兩產品資料處理資格差異的官方細節

## 原始摘要

- **aiposthub〈Claude OpenTelemetry 上線 Beta：追蹤 AI 呼叫、資料隱私與費用核對指南〉（Philo，2026-10-15）**：依官方 release notes、OTel 總覽與 API 接入文件，整理 trace／span、SDK vs API 追蹤、metadata_only 與完整內容模式、接線驗收、token 對帳。無付費推廣；同站延伸閱讀未收。

## 相關連結

- [[production-agent-architecture]] — Observability 模組（trace_id／token／latency／error／retry）的抽象設計；本條是 Claude 平台官方接法
- [[agent-reliability-timeout-retry-fallback-verification]] — 重試與可靠性機制；trace 能區分「重複紀錄」與「真的重試」
- [[ideas/claude-managed-agents-dynamic-workflows-every-2026-10]] — Managed Agents 動態工作流程（同作者、同站，不構成獨立佐證）
- [[claude-code-billing-usage]] — 用量與帳務；對帳另用 Usage API
- [[llm-cost-optimization-strategies]] — 成本觀測→優化
- [[claude-dashboards-beta-2026-10]] — 同批 2026-10 Claude beta 功能
