---
title: Error Recovery 不是 Retry：八種 Error Type 對應八種策略
date: 2026-08-14
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10402025（《30 天從零拆解 AI Agent》Day 12）
type: article
status: processed
asset_value: 80
---

同系列前作：Day 08 見 [[agent-skills-vs-mcp]]、Day 09 見 [[agent-context-management]]、Day 10 見 [[agent-memory-lifecycle]]、**Day 11 見 [[system-prompt-assembly-pipeline]]**。前一天把資訊組裝進模型，今天處理「組裝好之後執行失敗了怎麼辦」。

## T（核心論點）

> **Retry 是 Recovery 的一種，不是全部。**
> **Retry 解決暫時失敗；Recovery 解決的是「下一步應該怎麼改」。**

如果所有錯誤都用 Retry 解決，系統看起來很有韌性，但**它沒有真正理解失敗，只是把同一個錯誤多執行幾次**。所以 Recovery 的第一步不是執行，而是**分類**。

## B（適用場景）

**適用：**
- 自建 Agent 的錯誤處理設計（本篇末有可直接照抄的第一版設計清單）
- 手上 Agent 出現「一直 Retry 卻救不回來」「跑了 12 輪最後失敗但不知道在做什麼」時的診斷框架
- 設計 Tool 的錯誤回傳格式（結構化 Observation 而非 Stack Trace）

**不適用 / 注意：**
- 🔴 **Recovery 不是免費的**：原始任務 1 次模型呼叫 + 2 次 Tool Call，Recovery 後可能變成 5 次 Retry + 3 次 Replan + 2 次 Fallback + 1 次 Judge。**一個成功率只提升 1% 的 Recovery Mechanism，卻讓平均成本增加 50%，不一定值得**
- ⚠️ 本篇是設計原則層，非特定框架實作

## R（四層失敗與對應策略）

八種可能的 Recovery：**Retry / Repair / Fallback / Replan / Degrade / Ask User / Human Handoff / Stop**。

### 第一層：Transient Failure → Retry

暫時性錯誤（429、503、Network Timeout、Rate Limit、短暫資源不足）的特性是「**同樣的操作稍後再做一次，成功機率會提高**」——這才是 Retry 唯一適合的地方。

必須配 **Backoff**（Fixed／Linear／Exponential／Exponential + Jitter）。Jitter 用來避免大量 Agent 同時重試造成 **Thundering Herd**。

必須配 **Budget**，每個 Recovery Policy 至少定義：Max Retries／Max Total Time／Max Token Cost／Max Tool Cost／Backoff Strategy／Stop Condition。

🔑 **限制應由 Harness 控制，不要只在 Prompt 裡寫「如果一直失敗就不要再試了」**——可確定的限制應該存在程式碼（與 [[agent-loop-stop-conditions]] 的 Turn Budget 同一原則）。

🔴 **Retry 前必須先確認操作是不是 Idempotent。** `read_file` 沒問題，但 `send_email`／`charge_card`／`create_order`／`deploy_service` 的第一次操作**可能其實已經成功，只是回應在網路途中丟失**。看到 Timeout 就直接 Retry 會造成重複寄信、重複扣款、重複建單、重複部署。

解法是 **Idempotency Key**（`operation_id = order_123_refund_v1`）。外部 API 不支援時，由 Harness 保存 **Execution Record**（operation_id／tool_name／arguments_hash／status／external_reference），Retry 前先查「**這個操作是不是其實已經成功？**」

### 第二層：Repairable Failure → Repair

Tool Argument 缺欄位、JSON 格式錯、Path 不存在、Schema 不符——**等一下再試不會消失**。

關鍵在於**回傳結構化 Observation 而不是要求「再試一次」**（否則模型會再產生一次相同錯誤）：

```json
{ "ok": false, "error_type": "validation_error",
  "message": "Missing required field: path",
  "expected_schema": { "path": "string" } }
```

流程是 `Failure → Useful Observation → Repair → Retry corrected action`，而非盲目 Retry。

🔑 **Error Message 也是 Agent 的 Observation。** `FileNotFound` 對 Agent 而言是新的環境資訊（Path 猜錯／File 被移動／Repository 結構不同）。好的 Error Result 含 Error Type、Short Message、Relevant Field、Retryable、Suggested Next Action、Remaining Retry Budget——**比完整 Stack Trace 更有用**，也不會洩漏不必要資訊。

### 第三層：Strategy Failure → Replan

工具本身沒壞，**每一次 Tool Call 都成功，但策略錯了**：假設問題來自 Redis → 查 Redis Log → 沒有相關錯誤 → 查更多 Redis Log → 查更久的 Redis Log。

> **Loop 正在移動，但任務沒有前進。**

所以 Error Recovery 不只要偵測 Exception，也要偵測 **No Progress**。五個可用訊號：

- **Repeated Action** — 一直呼叫同一個 Tool + 類似 Arguments？
- **Repeated Observation** — Tool Result 高度重複？
- **Failed Verification** — 多次修改但驗證一直不過？
- **Plan Stagnation** — Current Step 長時間沒變？
- **Evidence Gain** — 每一輪是否真的取得新證據？

（與 [[agent-loop-stop-conditions]] 收錄的 Gemini CLI LoopDetectionService「重複」＋「無淨進展」須同時成立是同一組判準）

### 第四層：Fallback → Degrade

`Preferred Path → Fallback Path → Degraded Path → Human Handoff`。Fallback 的目的不是追求同等品質，而是「主要能力失敗時，能否用較弱但可接受的方法繼續」。

🔴 **Fallback 最大的風險是系統悄悄變差而使用者不知道**——原本 Grounded Retrieval，失敗後偷偷改成模型憑記憶回答，表面上任務完成了，可靠度完全不同。

所以必須標記：用了哪個替代方案／品質下降在哪／哪些結果不再可驗證／是否需要使用者確認／是否仍符合最低成功標準。

```
Primary source unavailable.
Continuing with cached data from 2 hours ago.
Freshness cannot be guaranteed.
```

🔑 **這叫 Graceful Degradation，不是 Silent Degradation。**

**Degradation ≠ Failure**：PDF Renderer 壞掉時，Agent 仍可完成分析、產生 Markdown、保存資料、告訴使用者 PDF 失敗——比整個 Task 直接 Fail 更好。**Recovery 不一定要恢復成完整能力，有時合理目標是「保留最大可用價值」。**

## C（關鍵行動）

- [ ] 先分類再處理：手上這個錯誤是 Transient／Validation／Strategy 哪一種？
- [ ] 每個有 Side Effect 的 Tool 都標記 idempotent 與否，Retry 前先查 Execution Record
- [ ] Fallback 一律顯式揭露品質下降，禁止 Silent Degradation

## 補充資料

### Error Taxonomy（八種）與預設 Recovery 對照表

八種 Error Type：`transient` / `validation` / `permission` / `dependency` / `strategy` / `verification` / `budget` / `ambiguity`（另加 `side effect uncertain`）。

| Error Type | Default Recovery |
|---|---|
| Rate Limit | Backoff + Retry |
| Timeout | Retry once, then Fallback |
| Invalid Arguments | Repair |
| Unknown Tool | Replan |
| Permission Denied | Alternative Path or Stop |
| Verification Failed | Replan |
| Missing User Input | Ask User |
| Budget Exhausted | Degrade or Stop |
| Side Effect Unknown | **Verify State before Retry** |

🔑 **這張表本身就可以變成 Harness Policy。**

### Error Policy 不應散落在 Tool 裡

若每個 Tool 各自決定 Retry 幾次、要不要 Fallback、何時 Stop，整個 Agent 很難有一致策略。正確分層：

```
Tool → 回傳結構化 Failure
Recovery Policy → 決定下一步
Agent Loop → 執行 Recovery Decision
```

**把 Error Detection 與 Recovery Decision 分開，更容易測試。**

### 什麼時候 Ask User、什麼時候 Human Handoff

**Ask User**（「請幫我寄信給 Alex」但有三個 Alex）——這不是 Tool Failure 也不是模型能力不足，而是**缺少必要資訊**。適用：必要輸入缺失／多選項無法安全推斷／高風險 Scope 不清／使用者意圖衝突／Permission 升級需確認／Fallback 會改變輸出品質。🔑 **這不是失敗，是一種正確的控制流程。**

**Human Handoff**：多次 Recovery 都失敗／高風險 Tool 狀態不確定／外部系統可能已執行但無法確認／安全規則衝突／Budget 即將耗盡／需要業務判斷。好的 Handoff 不能只說 "Something went wrong."，而應含 **Goal／Progress／Failure／Attempts／Current State／Risk／Recommended Action**——這樣人類才能直接接手，而不是重新調查。（與 [[context-isolation-and-compaction]] 的交接摘要格式同源）

### Stop 也是一種成功的 Recovery

Agent 系統常有的錯誤偏見是「一定要繼續做，直到完成」。但 Permission Denied／Budget Exhausted／Required Verification Impossible／Dangerous State Unknown／User Constraint Conflict／Repeated Non-progress／Side Effect Status Uncertain 時，**停止不是 Agent 不夠聰明，而是 Harness 正確限制了風險**。

### Tool 的 Side Effect 標記決定 Recovery 積極程度

`read_only` / `reversible` / `irreversible` / `idempotent` / `external_side_effect`

- Read-only + Idempotent → 可較積極 Retry
- Write + Reversible → Retry 前先確認 State
- Irreversible → 需更嚴格 Verification
- External Side Effect → 需 Operation ID、Audit、Approval

⚠️ **Tool Runtime 若完全不知道 Tool 的 Side Effect 類型，就很難做安全 Recovery。**（見 [[agent-tool-runtime-seven-layers]]）

### 簡化的 Recovery Pipeline

```
Failure → Classify → Is retryable?
  ├ Yes → Retry with Budget + Backoff
  └ No → Can repair?
      ├ Yes → Repair
      └ No → Alternative path?
          ├ Yes → Fallback or Replan
          └ No → Can degrade?
              ├ Yes → Degrade with disclosure
              └ No → Need user or human?
                  ├ Yes → Ask / Handoff
                  └ No → Stop
```

### 八個常見錯誤設計

所有 Failure 都 Retry／Retry 沒有 Budget／**Side Effect Tool 直接 Retry**／Error Message 只回完整 Stack Trace／沒有 No-progress Detection／**Fallback 靜默降級**／Permission Failure 也一直 Retry（不會因為多試幾次就突然有權限）／Recovery 沒有 Trace。

### Recovery Trace 與評估指標

失敗時要能回答：原始錯誤是什麼／被分類成哪個 Error Type／Retry 幾次／每次 Backoff 多久／是否換過 Tool／是否 Replan／是否 Fallback／是否 Degradation／最後為什麼 Stop。否則你只會看到 `Agent failed after 12 turns.`——**但不知道那 12 輪到底在做什麼**。

評估不應只看「最後有沒有成功」，還要看 Recovery Success Rate／Average Retries／Recovery Token Cost／Recovery Latency／Fallback Rate／Human Handoff Rate／**Cost per Successful Recovery**。

### 怎麼測：建立 Failure Dataset

Tool Failure（429／500／Timeout／Invalid JSON／Missing File／Schema Error）、Environment Failure（Network／DB unavailable／Disk full／Permission denied）、Agent Failure（Wrong tool／Repeated action／**Premature completion**／No progress）、Side Effect Failure（write 後 timeout／重複操作風險／部分成功）。

測項：是否正確分類／是否用對 Recovery／是否超過 Budget／**是否產生 Duplicate Side Effect**／該 Stop 時是否停止／是否正確 Handoff。🔑 **這比只測 Happy Path 更接近 Production。**

### 模型與 Harness 的分工

模型適合判斷「失敗代表什麼、哪個替代策略可能有效、是否需要重新規劃」；但 Retry Count／Timeout／Backoff／Allowed Fallback／Permission／Budget／Side Effect Safety／Maximum Replans **應由程式控制**——模型決定如何調整策略，Harness 決定允許調整多少次。

### 系列後續

Day 13 進入 Task System：任務不再是一次 Session 能完成時，怎麼保存 State、等待 Approval、暫停、恢復、數小時後繼續執行。（主題與 [[loopx-stateful-control-plane]] 高度重疊，屆時可互相對照）

## 相關連結
- [[synthesis/synthesis-agent-framework-philosophy]] — **本篇為其來源之一**（2026-09-03 重跑，4→5 篇）：🔑 本篇把「crash 後恢復」這個穩定性維度從一個勾選項拆成八種策略
- [[synthesis/synthesis-agent-engineering-architecture]] — **本篇為其來源之一**（2026-09-03 重跑，8→16 篇）：Agent 工程架構精粹，含 Capability scaffolding vs Control plane 二分法

- [[agent-reliability-timeout-retry-fallback-verification]] — 🔑 **姊妹條目（同為 asset_value 80，主題相鄰但切面不同，不是重複條目）**：本篇是**錯誤分類學**——錯誤怎麼分類、八種 Recovery 中下一步該怎麼改；該篇是**可靠性機制實作層**——Timeout / Retry / Fallback / Verification 四機制怎麼配置、Read vs Action Tool 的 Idempotency 差異、Circuit Breaker 與 Cache Fallback 的倫理界線
- [[system-prompt-assembly-pipeline]] — 同系列 Day 11，六層來源與 Priority
- [[agent-loop-stop-conditions]] — 最小實作版：`MAX_TURNS`／連續失敗／SDK 重試常數；本篇是它的策略分類完整版
- [[agent-tool-runtime-seven-layers]] — Tool Runtime 七層責任（Side Effect 標記屬其一）
- [[agent-control-layer-permission-hooks-planning]] — Permission Denied 為何不該 Retry
- [[context-isolation-and-compaction]] — Handoff 摘要該包含什麼
- [[loopx-stateful-control-plane]] — Gate／Evidence 與 Human Handoff 的制度化
- [[production-agent-architecture]] — 生產級 Agent 的 Guardrails 與冪等設計
- [[ai-code-review-safeguards]] — 「格式越完美越要提高警覺」的另一種失敗偵測
- [[ai-autonomy-by-rule-clarity-verifiability-reversibility]] — reversible 分類被拿來當授權判準的一軸（規則明確 × 可驗證 × 可逆）（2026-09-29 新增）
