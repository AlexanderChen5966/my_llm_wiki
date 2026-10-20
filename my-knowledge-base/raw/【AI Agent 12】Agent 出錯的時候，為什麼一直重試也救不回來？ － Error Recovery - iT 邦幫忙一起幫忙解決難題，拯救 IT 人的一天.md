---
title: "【AI Agent 12】Agent 出錯的時候，為什麼一直重試也救不回來？ － Error Recovery - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402025"
author:
  - "[[iThome]]"
published:
created: 2026-08-14
description: "前一天我們談 System Prompt Assembly。 核心觀念是： System Prompt 不是一份固定文件，而是 Harness 根據目前狀態動態組裝出的執行介面。 當 Agent 開始..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 12

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 12 篇

## 【AI Agent 12】Agent 出錯的時候，為什麼一直重試也救不回來？ － Error Recovery

- 分享至

前一天我們談 System Prompt Assembly。

核心觀念是：

> System Prompt 不是一份固定文件，而是 Harness 根據目前狀態動態組裝出的執行介面。

當 Agent 開始真的執行多輪任務之後，下一個一定會遇到的問題是：

> 出錯了，現在怎麼辦？

最直覺的答案通常是：

> Retry。

Tool 失敗，Retry。

模型輸出格式錯誤，Retry。

API Timeout，Retry。

測試失敗，Retry。

Agent 卡住，還是 Retry。

但如果所有錯誤都用 Retry 解決，系統最後很容易變成：

```markdown
失敗
↓
再試一次
↓
又失敗
↓
換個 Prompt 再試
↓
還是失敗
↓
增加 Retry 次數
```

看起來很有韌性。

但系統沒有真正理解失敗，只是把同一個錯誤多執行幾次。

今天我們要把 Error Recovery 拆成四個問題：

1. 這個錯誤值得 Retry 嗎？
2. 如果 Retry 沒用，什麼時候該換策略？
3. 什麼時候應該降級，而不是硬撐？
4. 哪些錯誤必須直接停止？

---

## Error Recovery 不是 Retry

先建立最重要的區分：

> Retry 是 Recovery 的一種，不是全部。

真正的 Error Recovery 至少可能包含：

- Retry
- Repair
- Fallback
- Replan
- Degrade
- Ask User
- Human Handoff
- Stop

不同 Failure Mode 應該對應不同策略。

例如：

```markdown
API 429
→ Retry

Tool Arguments 格式錯誤
→ Repair

主要搜尋 API 掛掉
→ Fallback

原本假設不存在
→ Replan

高階功能不可用
→ Degrade

需求缺少必要資訊
→ Ask User

高風險操作無法驗證
→ Human Handoff

Permission Denied
→ Stop 或改走其他路徑
```

所以 Recovery 的第一步不是執行。

而是分類。

---

## 第一層：Transient Failure

Transient Failure 是暫時性錯誤。

例如：

- API 429
- API 503
- Network Timeout
- Temporary DNS Failure
- Rate Limit
- 短暫資源不足
- 外部服務瞬間不可用

這類錯誤的特性是：

> 同樣的操作稍後再做一次，成功機率會提高。

這才是 Retry 最適合的地方。

---

## Retry 應該有 Backoff

如果 API 已經 Rate Limit，立即連續 Retry 通常只會讓問題更嚴重。

更合理的策略是：

```markdown
第一次失敗
↓
等待短時間
↓
Retry

第二次失敗
↓
等待更久
↓
Retry

再次失敗
↓
Fallback 或 Stop
```

這就是 Backoff。

常見方式包括：

- Fixed Delay
- Linear Backoff
- Exponential Backoff
- Exponential Backoff + Jitter

例如：

```python
delays = [1, 2, 4, 8]
```

Jitter 則是在等待時間中加入隨機量，避免大量 Agent 同時重試，造成 Thundering Herd。

---

## Retry 必須有 Budget

如果沒有 Retry Budget，Agent 可以把一個小錯誤變成成本黑洞。

每個 Recovery Policy 最少要定義：

- Max Retries
- Max Total Time
- Max Token Cost
- Max Tool Cost
- Backoff Strategy
- Stop Condition

例如：

```markdown
search_api

max_retries: 3
timeout: 10s
backoff: exponential
fallback: browser_search
```

Retry 的限制應該由 Harness 控制。

不要只在 Prompt 裡寫：

> 如果一直失敗，就不要再試了。

這和 Day 2 的 Turn Budget 是同一個原則。

可確定的限制，應該存在程式碼。

---

## Retry 之前先確認操作是不是 Idempotent

這是 Production Agent 很容易忽略的問題。

假設工具是：

```markdown
read_file
```

Retry 通常沒有問題。

但如果工具是：

```markdown
send_email
charge_card
create_order
deploy_service
```

第一次操作可能其實已經成功。

只是回應在網路途中丟失。

如果 Harness 看到 Timeout 就直接 Retry，可能造成：

- 重複寄信
- 重複扣款
- 重複建立訂單
- 重複部署
- 重複修改資料

所以 Side Effect Tool 不能把 Retry 當成預設安全行為。

---

## Idempotency Key

對可重複呼叫的 Side Effect，可以加入 Idempotency Key。

例如：

```markdown
operation_id = order_123_refund_v1
```

即使 Runtime 重送相同請求，外部系統也能辨認：

> 這是同一個操作，不應該執行兩次。

如果外部 API 不支援 Idempotency，也可以由 Harness 保存 Execution Record。

例如：

```markdown
operation_id
tool_name
arguments_hash
status
external_reference
```

Retry 前先查：

> 這個操作是不是其實已經成功？

這比單純重送安全很多。

---

## 第二層：Repairable Failure

有些錯誤不會因為等一下再試就消失。

例如：

- Tool Argument 缺少欄位
- JSON 格式錯誤
- Path 不存在
- Query 語法錯誤
- Schema 不符合
- Command 使用錯誤參數

這類失敗適合：

> Repair，而不是 Retry。

例如模型產生：

```json
{
  "tool": "read_file",
  "arguments": {
    "file": "config.yaml"
  }
}
```

但 Tool Schema 要的是：

```markdown
path
```

如果 Harness 完全不改資訊，只要求：

> 再試一次。

模型可能再次產生相同錯誤。

更好的方式是回傳結構化 Observation：

```json
{
  "ok": false,
  "error_type": "validation_error",
  "message": "Missing required field: path",
  "expected_schema": {
    "path": "string"
  }
}
```

模型得到新的資訊後，再修正 Tool Call。

這是：

```markdown
Failure
↓
Useful Observation
↓
Repair
↓
Retry corrected action
```

而不是盲目 Retry。

---

## Error Message 也是 Agent 的 Observation

Tool Failure 不只是系統例外。

對 Agent 來說，它是新的環境資訊。

例如：

```markdown
FileNotFound
```

可能代表：

- Path 猜錯
- File 被移動
- 使用者資訊過期
- Repository 結構不同

所以 Error Recovery 的一個重要原則是：

> 把失敗轉換成模型可以理解、但不會洩漏不必要資訊的 Observation。

好的 Error Result 可以包含：

- Error Type
- Short Message
- Relevant Field
- Retryable
- Suggested Next Action
- Remaining Retry Budget

例如：

```json
{
  "ok": false,
  "error_type": "file_not_found",
  "retryable": false,
  "message": "src/config.py does not exist",
  "suggestion": "Search for files named config.py before retrying."
}
```

這會比完整 Stack Trace 更有用。

---

## 第三層：Strategy Failure

有時候工具本身沒有壞。

但整個策略錯了。

例如 Agent 想 Debug 一個問題：

```markdown
假設：
問題來自 Redis

執行：
查 Redis Log

結果：
完全沒有相關錯誤

再次：
查更多 Redis Log

再次：
查更久的 Redis Log
```

每一次 Tool Call 都成功。

但 Strategy 沒有進展。

這時再 Retry 同類行動沒有意義。

應該：

> Replan

Strategy Failure 的訊號可能包括：

- 同類 Tool Call 重複很多次
- 新 Observation 沒有增加資訊
- 同一個 Hypothesis 連續被否定
- Progress Metric 長時間沒有改善
- Step 多次完成但 Verification 仍失敗
- Token 大量增加但 Evidence 沒有增加

這些都是：

> Loop 正在移動，但任務沒有前進。

---

## Progress Detection

Error Recovery 不只要偵測 Exception。

也要偵測：

> No Progress。

可以建立簡單訊號：

- Repeated Action: 是否一直呼叫同一個 Tool + 類似 Arguments？
- Repeated Observation: Tool Result 是否高度重複？
- Failed Verification: 是否多次修改，但 Verification 一直不通過？
- Plan Stagnation: Current Step 是否長時間沒有改變？
- Evidence Gain: 每一輪是否真的取得新證據？

如果 Progress 太低，可以觸發：

```markdown
Replan
Fallback
Escalate
Stop
```

這也是 Loop Engineering 很重要的一部分。

---

## 第四層：Fallback

假設主要 Tool 不可用。

不一定要直接失敗。

例如：

```markdown
Primary:
Internal Search API

Fallback:
Public Search
```

或者：

```markdown
Primary:
Structured Parser

Fallback:
Plain-text extraction
```

Fallback 的目的不是追求完全一樣的品質。

而是：

> 主要能力失敗時，能否用較弱但可接受的方法繼續？

可以把 Recovery Level 想成：

```markdown
Preferred Path
↓
Fallback Path
↓
Degraded Path
↓
Human Handoff
```

---

## Fallback 也需要品質邊界

Fallback 最大的風險是：

> 系統悄悄變差，但使用者不知道。

例如原本：

```markdown
Grounded Retrieval
```

失敗後偷偷改成：

```markdown
模型憑記憶回答
```

表面上任務完成了。

但可靠度完全不同。

所以 Fallback 應該標記：

- 使用了哪個替代方案
- 品質下降在哪裡
- 哪些結果不再可驗證
- 是否需要使用者確認
- 是否仍符合最低成功標準

例如：

```markdown
Primary source unavailable.

Continuing with cached data from 2 hours ago.

Freshness cannot be guaranteed.
```

這叫 Graceful Degradation。

不是 Silent Degradation。

---

## Degradation 和 Failure 不一樣

假設 Agent 的原目標是：

> 產生完整 PDF 報告。

PDF Renderer 壞掉。

但 Agent 還可以：

- 完成分析
- 產生 Markdown
- 保存資料
- 告訴使用者 PDF 產生失敗

這可能比整個 Task 直接 Fail 更好。

所以 Recovery 不一定要：

> 恢復成完整能力。

有時候合理目標是：

> 保留最大可用價值。

這就是 Degradation。

---

## 什麼時候應該 Ask User？

不是所有問題都應該讓 Agent 自己修。

例如：

```markdown
請幫我寄信給 Alex。
```

系統發現 Contact 中有三個 Alex。

這不是 Tool Failure。

也不是模型能力不足。

它是：

> 缺少必要資訊。

如果 Agent 自己猜，風險很高。

這時最好的 Recovery 是：

> Ask User。

適合 Ask User 的情況包括：

- 必要輸入缺失
- 多個選項無法安全推斷
- 高風險 Scope 不清楚
- 使用者意圖互相衝突
- Permission 升級需要確認
- Fallback 會改變輸出品質

這不是失敗。

這是一種正確的控制流程。

---

## 什麼時候應該 Human Handoff？

有些 Failure Mode 已經超出 Agent 應該自行處理的範圍。

例如：

- 多次 Recovery 都失敗
- 高風險 Tool 狀態不確定
- 外部系統可能已執行，但無法確認
- 安全規則互相衝突
- Budget 即將耗盡
- Verification 無法完成
- 需要業務判斷而非技術判斷

這時應該把目前狀態整理好，交給人類。

好的 Handoff 不應該只說：

> Something went wrong.

而應該包含：

```markdown
Goal
目前要完成什麼

Progress
已完成什麼

Failure
哪裡失敗

Attempts
已經試過什麼

Current State
目前環境狀態

Risk
有哪些不確定性

Recommended Action
建議人類下一步怎麼做
```

這樣人類才能直接接手，而不是重新調查。

---

## Stop 也是一種成功的 Recovery

Agent 系統常有一個錯誤偏見：

> 一定要繼續做，直到完成。

但有時候最正確的行為是停止。

例如：

- Permission Denied
- Budget Exhausted
- Required Verification Impossible
- Dangerous State Unknown
- User Constraint Conflict
- Repeated Non-progress
- Tool Side Effect Status Uncertain

停止不是 Agent 不夠聰明。

而是 Harness 正確限制了風險。

---

## Error Taxonomy

如果所有錯誤最後都只是：

```markdown
Exception
```

Recovery 很難做。

可以先建立基本 Taxonomy。

例如：

- Transient: 暫時錯誤，可 Retry。
- Validation: 輸入不合法，需要 Repair。
- Permission: 操作不允許。
- Dependency: 外部服務或工具不可用。
- Strategy: 目前路徑沒有進展。
- Verification: 結果未通過檢查。
- Budget: Token、時間或成本不足。
- Ambiguity: 缺少必要資訊。
- Side Effect Uncertain: 不知道操作是否已經真的發生。

不同 Error Type 對應不同 Recovery Policy。

---

## Error Policy 不應該散落在 Tool 裡

如果每個 Tool 自己決定：

- Retry 幾次
- 要不要 Fallback
- 何時 Stop

整個 Agent 很難有一致策略。

更好的方式是：

```markdown
Tool
回傳結構化 Failure

Recovery Policy
決定下一步

Agent Loop
執行 Recovery Decision
```

例如：

```markdown
Tool Result:
type = rate_limit
retryable = true

Recovery Policy:
max retries = 3
backoff = exponential
fallback = alternate provider
```

把 Error Detection 和 Recovery Decision 分開，更容易測試。

---

## Retry Policy 可以依 Error Type 不同

例如：

| Error Type | Default Recovery |
| --- | --- |
| Rate Limit | Backoff + Retry |
| Timeout | Retry once, then Fallback |
| Invalid Arguments | Repair |
| Unknown Tool | Replan |
| Permission Denied | Alternative Path or Stop |
| Verification Failed | Replan |
| Missing User Input | Ask User |
| Budget Exhausted | Degrade or Stop |
| Side Effect Unknown | Verify State before Retry |

這張表本身就可以變成 Harness Policy。

---

## Recovery 需要知道 Side Effect

每個 Tool 最好標記：

```markdown
read_only
reversible
irreversible
idempotent
external_side_effect
```

因為 Recovery Strategy 會不同。

例如：

- Read-only + Idempotent: 可以比較積極 Retry。
- Write + Reversible: Retry 前先確認 State。
- Irreversible: 通常需要更嚴格 Verification。
- External Side Effect: 需要 Operation ID、Audit、Approval。

Tool Runtime 如果完全不知道 Tool 的 Side Effect 類型，就很難做安全 Recovery。

---

## Recovery 也需要 Trace

當 Agent 最後失敗時，我們應該能回答：

- 原始錯誤是什麼？
- 被分類成哪個 Error Type？
- Retry 幾次？
- 每次 Backoff 多久？
- 是否換過 Tool？
- 是否 Replan？
- 是否使用 Fallback？
- 是否發生 Degradation？
- 最後為什麼 Stop？

Recovery Trace 對 Debug 和 Evaluation 都非常重要。

否則你只會看到：

> Agent failed after 12 turns.

但不知道那 12 輪到底在做什麼。

---

## Error Recovery 的成本也需要計算

Recovery 不是免費的。

假設原始任務：

```markdown
1 次模型呼叫
2 次 Tool Calls
```

但 Recovery 後變成：

```markdown
5 次 Retry
3 次 Replan
2 次 Fallback
1 次 Judge
```

即使最後成功，成本可能已經高很多。

所以 Evaluation 不應該只看：

> 最後有沒有成功？

也要看：

- Recovery Success Rate
- Average Retries
- Recovery Token Cost
- Recovery Latency
- Fallback Rate
- Human Handoff Rate
- Cost per Successful Recovery

有時候一個成功率只提升 1% 的 Recovery Mechanism，卻讓平均成本增加 50%。

不一定值得。

---

## 如何測 Error Recovery？

可以建立專門的 Failure Dataset。

例如：

### Tool Failure

- 429
- 500
- Timeout
- Invalid JSON
- Missing File
- Schema Error

### Environment Failure

- Network unavailable
- Database unavailable
- Disk full
- Permission denied

### Agent Failure

- Wrong tool
- Repeated action
- Premature completion
- No progress

### Side Effect Failure

- Request timeout after write
- Duplicate operation risk
- Partial success

然後測：

- 是否正確分類
- 是否使用正確 Recovery
- 是否超過 Budget
- 是否產生 Duplicate Side Effect
- 是否在應該 Stop 時停止
- 是否正確 Handoff

這比只測 Happy Path 更接近 Production。

---

## Recovery 不應該讓模型無限自由決定

模型很適合判斷：

- 失敗代表什麼
- 哪個替代策略可能有效
- 是否需要重新規劃

但一些 Recovery 邊界應該由程式控制：

```markdown
Retry Count
Timeout
Backoff
Allowed Fallback
Permission
Budget
Side Effect Safety
Maximum Replans
```

可以分成：

```markdown
Model
決定如何調整策略

Harness
決定允許調整多少次、哪些操作可以做
```

這和整個系列的主題一致。

---

## 一個簡化的 Recovery Pipeline

可以整理成：

```markdown
Failure
↓
Classify
↓
Is retryable?
├── Yes → Retry with Budget + Backoff
└── No
     ↓
Can repair?
├── Yes → Repair
└── No
     ↓
Alternative path?
├── Yes → Fallback or Replan
└── No
     ↓
Can degrade?
├── Yes → Degrade with disclosure
└── No
     ↓
Need user or human?
├── Yes → Ask / Handoff
└── No → Stop
```

這比：

```markdown
except Exception:
    retry()
```

可靠很多。

---

## 常見的錯誤設計

### 1\. 所有 Failure 都 Retry

Temporary Failure 和 Logic Failure 被混在一起。

### 2\. Retry 沒有 Budget

一個小錯誤變成無限成本。

### 3\. Side Effect Tool 直接 Retry

可能產生重複付款、重複 Email 或重複修改。

### 4\. Error Message 只回完整 Stack Trace

模型很難使用，也可能洩漏敏感資訊。

### 5\. 沒有 No-progress Detection

Agent 一直移動，但任務沒有真正前進。

### 6\. Fallback 靜默降級

使用者不知道可靠度已經下降。

### 7\. Permission Failure 也一直 Retry

不會因為多試幾次就突然有權限。

### 8\. Recovery 沒有 Trace

最後只知道任務失敗，不知道在哪個策略失敗。

---

## 如何設計第一版 Error Recovery？

可以先定義八種 Error Type：

```markdown
transient
validation
permission
dependency
strategy
verification
budget
ambiguity
```

然後為每種 Error 定義：

- Retryable
- Max Retry
- Backoff
- Fallback
- Replan Allowed
- Ask User
- Stop Condition

再為所有有 Side Effect 的 Tool 加入：

- Idempotency
- Operation ID
- Execution Record
- Pre-retry Verification

最後加入兩個 Global Limit：

- Max Total Recovery Attempts
- Max Recovery Cost

這已經能避免很多「一直 Retry」的問題。

---

## 今天新增了什麼能力？

Day 11 我們建立 Prompt Assembly。

今天加入：

- Error Taxonomy
- Retry Policy
- Backoff
- Repair
- Fallback
- Replan
- Graceful Degradation
- No-progress Detection
- Idempotency
- Human Handoff
- Recovery Budget
- Recovery Trace

因此，Agent 遇到失敗時，不再只有：

> 再試一次。

而是可以先回答：

> 這是哪一種 Failure？最安全、最便宜的下一步是什麼？

---

## 今天的結論

Error Recovery 的核心不是增加 Retry 次數。

而是把不同失敗映射到不同處理策略。

真正可靠的 Recovery 至少要知道：

- 哪些錯誤只是暫時的
- 哪些錯誤需要修正輸入
- 哪些錯誤代表策略已經錯了
- 哪些操作不能安全 Retry
- 哪些情況可以 Fallback
- 哪些情況只能降級
- 哪些情況需要 Ask User
- 哪些情況必須 Stop

最重要的原則是：

> Retry 解決暫時失敗，Recovery 解決的是「下一步應該怎麼改」。

下一篇會進入 Task System：

> 當 Agent 任務不再是一次 Session 就能完成，我們要怎麼保存 State、等待 Approval、暫停、恢復，甚至在數小時後繼續執行？

**完整系列與範例收錄於： [https://github.com/hardness1020/awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture)**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401899)

[

【AI Agent 11】每個任務需要的背景都不一樣，System Prompt 怎麼可以寫死？ － System Prompt

](https://ithelp.ithome.com.tw/articles/10401899)

系列文

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121) 共 12 篇

目錄

1. 8
	[【AI Agent 08】Skill 越加越多，要怎麼讓 Agent 只在需要時才載入？ － Skills](https://ithelp.ithome.com.tw/articles/10401893)
2. 9
	[【AI Agent 09】聊到一半 Agent 開始忘東忘西，上下文該留什麼、丟什麼？ － Context Management](https://ithelp.ithome.com.tw/articles/10401895)
3. 10
	[【AI Agent 10】關掉視窗就全部忘光，Agent 要怎麼記得事情？ － Memory](https://ithelp.ithome.com.tw/articles/10401896)
4. 11
	[【AI Agent 11】每個任務需要的背景都不一樣，System Prompt 怎麼可以寫死？ － System Prompt](https://ithelp.ithome.com.tw/articles/10401899)
5. 12
	[【AI Agent 12】Agent 出錯的時候，為什麼一直重試也救不回來？ － Error Recovery](https://ithelp.ithome.com.tw/articles/10402025)
[完整目錄](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/19/e81292fc-aa02-4bfb-9f0c-643ab1cf557d_small.png)

[Elastic Security: 監測 x 告警，揪出潛在威脅！【Webinar：Elastic 系列講座】｜歐立威科技](https://itplus.ithome.com.tw/vod-page/670?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

歐立威科技 |

59 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/18/2323173f-b468-40b5-bcc6-2ae6cdbdd815_small.png)

[舞動雙軸、關鍵資安、企業永續發展](https://itplus.ithome.com.tw/vod-page/947?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位永續高峰會 |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/240a3713-014e-461b-bece-6fba1d7c1cbc_small.png)

[協作與敏捷： 外部廠商如何融入我們的DevOps環境那條漫長道路](https://itplus.ithome.com.tw/vod-page/1095?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

39 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/ad8f4085-3932-4202-a5a6-2478f7f6b61a_small.png)

[Elastic AI 的優勢：從 RAG 到治理 - 智能系統的統一平台](https://itplus.ithome.com.tw/vod-page/1210?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

26 分