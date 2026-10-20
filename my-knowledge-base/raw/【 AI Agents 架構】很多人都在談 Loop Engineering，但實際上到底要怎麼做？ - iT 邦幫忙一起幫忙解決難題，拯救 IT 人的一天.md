---
title: "【 AI Agents 架構】很多人都在談 Loop Engineering，但實際上到底要怎麼做？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10400443"
author:
  - "[[iThome]]"
published:
created: 2026-07-20
description: "常見的解釋是： 不要只設計下一個 Prompt，要設計一個能持續執行、取得回饋並改善結果的 Loop。 這個方向沒有錯，但對工程師來說還是不夠具體。 真正開始實作時，問題馬上就會出現： 到底是哪一層在..."
tags:
  - "clippings"
status: processed
---
4

## 【 AI Agents 架構】很多人都在談 Loop Engineering，但實際上到底要怎麼做？

- 分享至

常見的解釋是：

> 不要只設計下一個 Prompt，要設計一個能持續執行、取得回饋並改善結果的 Loop。

這個方向沒有錯，但對工程師來說還是不夠具體。

真正開始實作時，問題馬上就會出現：

- 到底是哪一層在 Loop？
- Agent 說完成，就真的算完成嗎？
- 驗證失敗後，要怎麼把結果送回下一輪？
- Agent 可以重試幾次？
- 誰負責啟動下一次任務？
- 系統執行了一百次之後，怎麼知道它有沒有變好？

先講結論：

> Loop Engineering 不是把 Agent 放進 `while True` ，而是設計 Agent 外面的系統，讓它能自動發現工作、執行、驗證、控制預算、保存結果，並決定下一步。

如果沒有這層系統，人類其實就是那個 Outer Loop。

我們手動輸入 Prompt、閱讀答案、判斷對錯、補充資訊，再要求 Agent 重做。人一離開，工作也就停止了。

Loop Engineering 要做的事情，是把這些原本由人類手動完成的控制流程，變成明確、可驗證的系統。

## 一個 Agent 系統其實有四層 Loop

要理解 Loop Engineering，可以先把系統拆成四個不同層級。

```markdown
Trigger
   ↓
Agent Loop
   ↓
Candidate Output
   ↓
Verifier
   ├── Pass → Deliver
   ├── Fail → Feedback → Retry
   └── Budget exhausted → Escalate to human
                               
Execution Trace
   ↓
Evaluate failures
   ↓
Improve harness, skill or model
```

這裡其實包含四個 Loop。

## 1\. Agent Loop：一個步驟怎麼完成？

這是最基礎的 Agent Loop：

```markdown
messages → LLM → tool call → tool result → messages → LLM
```

模型提出工具呼叫，Harness 執行工具，將結果放回 `messages[]` ，再呼叫模型。

```python
def run_agent(messages, tools):
    while True:
        response = call_model(messages, tools)
        messages.append(response)

        if response.has_tool_call:
            result = execute_tool(response.tool_call)
            messages.append(to_tool_result(result))
            continue

        return response.text
```

這個 Loop 回答的是：

> Agent 如何完成一次工作？

但它有一個很大的問題。

它通常會在 **模型認為自己完成時** 停止。

模型可能說：

- 程式碼已經完成
- Bug 已經修正
- 分析結果正確
- 文件已經更新

但它不一定真的執行過測試，也不一定符合原始需求。

因此，只有 Agent Loop 還不夠。

## 2\. Verification Loop：它真的完成了嗎？

Verification Loop 包在 Agent Loop 外面。

Agent Loop 先產生一個 Candidate Output，再交給獨立的 Checker 驗證。

如果通過，就交付結果。

如果失敗，就把失敗原因變成下一輪的 Feedback。

```python
def verified_run(task, worker, checker, rubric, max_attempts=2):
    feedback = None
    attempts = []

    for attempt in range(1, max_attempts + 1):
        candidate = worker(
            task=task,
            feedback=feedback,
        )

        verdict = checker(
            task=task,
            output=candidate,
            rubric=rubric,
        )

        attempts.append({
            "attempt": attempt,
            "passed": verdict.passed,
            "reason": verdict.reason,
        })

        if verdict.passed:
            return {
                "ok": True,
                "output": candidate,
                "attempts": attempts,
            }

        feedback = {
            "previous_output": candidate,
            "failure_reason": verdict.reason,
        }

    return {
        "ok": False,
        "output": None,
        "attempts": attempts,
        "action": "escalate_to_human",
    }
```

這段程式碼看起來不複雜，但有四個重要的設計決定。

### Worker 不應該驗證自己的輸出

如果同一個 Agent 同時負責產生答案和判斷答案是否正確，它通常很容易接受自己的結果。

比較可靠的方式是使用 Maker-Checker 分工：

- Worker 負責完成任務
- Checker 負責找出不符合條件的地方

Checker 每次驗證時都應該使用新的 Context，不應繼承 Worker 的推理過程。

```python
def check(task, output, rubric):
    messages = [{
        "role": "user",
        "content": build_review_prompt(
            task=task,
            output=output,
            rubric=rubric,
        ),
    }]

    return run_checker_with_fresh_context(messages)
```

這不是為了讓 Checker 比 Worker 更聰明，而是避免它被 Worker 的推理和自我辯護影響。

### Rubric 必須固定在 Loop 外面

Agent 可以努力滿足 Rubric，但不能修改 Rubric。

例如，程式修改任務的驗證條件可以是：

```markdown
1. 所有既有測試必須通過
2. 必須新增對應的回歸測試
3. 不得刪除或跳過失敗測試
4. 修改範圍不得超出指定模組
```

如果 Agent 可以自己改寫完成標準，它可能會透過降低標準來讓自己通過。

所以 Rubric、權限和預算，應該由 Agent 無法修改的外層 Harness 控制。

### Feedback 應該是下一輪的資料

驗證失敗不能只回傳一句：

> 再試一次。

下一輪需要知道：

- 上一次產生了什麼
- 哪一項條件沒有通過
- 有哪些證據支持這個判斷
- 哪些部分不需要重新執行

例如：

```markdown
Previous attempt:
新增了 API endpoint，但沒有新增 authorization test。

Verification result:
FAIL

Reason:
需求要求未授權使用者收到 403，目前測試只覆蓋成功路徑。

Required correction:
新增未授權情境測試，確認 endpoint 回傳 403。
```

這樣第二輪不是重新猜測，而是根據具體失敗證據修正。

### Budget 必須由 Harness 強制執行

每個 Loop 都需要一個模型無法突破的上限。

可以是：

- 最大嘗試次數
- Token 預算
- 執行時間
- 工具呼叫次數
- 最大成本
- 連續幾輪沒有發現新資訊就停止

最簡單的 Budget 就是：

```python
for attempt in range(max_attempts):
    ...
```

當 `max_attempts` 是 2，就不可能出現第三次執行。

這和在 Prompt 裡寫「最多嘗試兩次」不同。

Prompt 是建議，程式碼才是限制。

當預算耗盡時，正確的結果不是繼續祈禱下一輪會成功，而是：

```markdown
ok = false
action = escalate_to_human
```

> 沒有 Verifier 的 Loop 只是在自動化產出；沒有 Budget 的 Loop 則是在自動化帳單。

## 3\. Event Loop：工作什麼時候開始？

前兩層 Loop 解決了「如何執行」和「是否完成」，但任務仍然需要有人手動啟動。

Event Loop 負責決定工作什麼時候開始。

Trigger 可能來自：

- Cron schedule
- GitHub webhook
- Slack 或 Discord 訊息
- 新增的 Issue
- CI 失敗
- 某個背景程序的輸出
- Queue 裡的新任務
- Agent 自己排定的下一次喚醒時間

例如：

```python
def on_event(event):
    task = convert_event_to_task(event)
    result = verified_run(
        task=task,
        worker=worker,
        checker=checker,
        rubric=select_rubric(task),
        max_attempts=2,
    )

    persist_result(task, result)
    deliver_report(result)
```

這一層回答的是：

> Agent 什麼時候應該開始工作？

但 Event Loop 不應該直接把所有 Trigger 都變成具有寫入權限的自主工作。

比較安全的方式是逐步提高自治程度。

## Loop 的三個成熟度

可以將 Loop 分成三個等級。

### L1：Report

Loop 只讀取資料並產生報告。

例如：

- 每天整理失敗的 CI
- 分析最近新增的 Issues
- 找出過期 dependencies
- 回報可能有問題的程式碼

人類閱讀報告後，再決定是否採取行動。

### L2：Assisted

Loop 可以準備修改，但需要人類批准。

例如：

- 建立修正用 Branch
- 產生 Pull Request
- 準備 dependency update
- 草擬 Issue 回覆
- 產生 migration plan

人類確認後才合併或執行。

### L3：Unattended

Loop 可以直接採取行動，人類在事後稽核。

例如：

- 自動修復特定類型的格式問題
- 自動處理已驗證的簡單回歸
- 自動執行低風險 maintenance task
- 自動部署通過完整驗證的變更

不要一開始就給 Loop L3 權限。

比較可靠的升級方式是：

```markdown
L1 長期產生正確報告
        ↓
L2 產生的變更長期可直接批准
        ↓
L3 才取得有限、自動化的寫入權限
```

自治程度本質上是權限設計，不是 Agent 聰明程度。

## 4\. Improvement Loop：系統有沒有變好？

當系統開始持續執行後，會累積大量 Trace：

- 哪些任務成功
- 哪些任務失敗
- 每次使用多少 Token
- 哪些工具最常失敗
- Checker 最常拒絕什麼
- 哪些任務需要人工接管
- 哪些失敗在重複發生

Improvement Loop 使用這些資料改善：

- System Prompt
- Tool schema
- Skills
- Rubric
- Context 組裝方式
- 權限範圍
- Budget
- 模型選擇
- 整個 Harness 的結構

例如，每週可以執行一次失敗分析：

```python
def improve_harness(traces, regression_set):
    repeated_failures = mine_failure_patterns(traces)
    proposal = propose_bounded_change(repeated_failures)

    evaluation = run_regression_test(
        change=proposal,
        dataset=regression_set,
    )

    if evaluation.is_better:
        return submit_for_review(proposal)

    return reject_change(proposal)
```

這裡最重要的仍然是外部限制。

負責改善 Harness 的 Agent，不能同時修改：

- 自己的權限上限
- 自己的 Budget
- 驗證是否通過的程式碼
- Regression set
- 部署批准機制

否則 Improvement Loop 可能透過移除限制，讓自己的評分看起來更好。

## Loop Engineering 並沒有發明新的 Primitive

Loop Engineering 的關鍵不是增加一個新的 Agent 元件，而是把既有元件組合起來：

- Agent Loop 負責執行
- Subagent 提供 Worker 與 Checker 的 Context 隔離
- Permission 控制每個 Loop 可以做什麼
- Scheduler 與 Channel 提供 Trigger
- Memory 與 Task Record 保存跨次執行狀態
- Worktree 隔離平行修改
- Observability 保存 Trace
- Evaluation 將執行結果轉成改善依據

所以 Loop Engineering 比較像是一種 Composition Discipline。

它把單次 Agent Call，組合成一個可以持續運作的系統。

## 常見的失敗方式

### 1\. 沒有真正的停止條件

Agent 不斷收到「繼續改善」，直到 Token 或預算耗盡。

解法是由 Harness 強制限制嘗試次數、時間和成本。

### 2\. Worker 自己評分

同一個 Context 同時產生答案與判斷答案，導致驗證形同虛設。

解法是使用獨立 Checker、全新 Context 和固定 Rubric。

### 3\. Checker 永遠蓋章通過

有 Verifier 不代表有有效驗證。

如果 Rubric 太模糊，Checker 可能每次都回傳通過。

解法是使用可觀察證據、對抗式檢查，以及定期人工抽查。

### 4\. 太早給予自主權限

系統還沒有證明 L1 報告可靠，就直接取得 L3 寫入與部署權限。

解法是逐層提高成熟度，並限制每類任務的權限範圍。

### 5\. 每次都忘記上一次做了什麼

如果狀態沒有持久化，每次 Event 都會重新發現相同問題、重做相同工作。

解法是保存 Task Record、驗證結果、已處理項目和執行 Trace。

### 6\. Improvement Loop 可以移除自己的限制

如果 Agent 可以修改驗證、Budget 或 Permission，它可能繞過原本的安全邊界。

解法是讓關鍵 Gate 存在於 Loop 無法修改的外層系統。

## AI 工程師可以怎麼開始？

不需要一開始就建立完整的 Autonomous Agent System。

可以先挑一個範圍明確、容易驗證的任務。

例如：

> 每天檢查 Repository 中失敗的 CI，找出可能原因並產生修正建議。

第一版只做 L1：

1. Cron 每天啟動一次
2. Agent 讀取失敗 Log
3. 產生原因與建議
4. Checker 根據固定 Rubric 驗證
5. 最多重試兩次
6. 將所有 Attempt 和結果寫入 Trace
7. 無論成功或失敗都產生報告

等它長期穩定後，再升級成 L2：

1. 建立獨立 Worktree
2. 產生修正
3. 執行測試
4. Checker 驗證
5. 建立 Pull Request
6. 等待人工批准

只有當這些 Pull Request 長期都能直接批准時，才考慮讓特定、低風險任務升級成 L3。

## 結論

很多人把 Loop Engineering 理解成：

> 讓 Agent 一直跑，直到完成。

但真正的工程問題不是如何讓它一直跑。

而是：

- 誰啟動工作
- Agent 如何執行
- 誰判斷完成
- 失敗證據如何進入下一輪
- Loop 最多能消耗多少資源
- 何時必須交給人類
- 執行狀態如何保存
- 系統如何從歷史失敗中改善

一個真正可用的 Loop，應該具備：

```markdown
Trigger
+ Agent
+ Independent Verification
+ Feedback
+ Hard Budget
+ Persistent State
+ Escalation
+ Trace
+ Improvement
```

## 資源分享

我最近在開源專案 [**Awesome Agent Architecture**](https://github.com/hardness1020/awesome-agent-architecture) 中，將現代 Agent Harness 拆成 22 個循序漸進的 Sections。

專案從最小 Agent Loop 開始，逐步加入 Tool Runtime、Permission、Hooks、Planning、Subagent、Skills、Context Management、Memory、Task System、Scheduling、Multi-Agent Coordination、Evaluation，最後在 Section 21 將這些機制組合成 Loop Engineering。

Sections 1 到 21 都有可執行的 Python 範例，可以直接比較相鄰章節，觀察每一層新增了什麼機制。  
[https://github.com/hardness1020/awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture)

如果你已經知道怎麼呼叫 LLM 和 Tools，但還不清楚如何把它們組合成可以長時間運作的 Agent System，可以直接從 Section 1 的 Agent Loop 開始，再一路看到 Section 21。

Loop Engineering 的目標不是移除人類。

它真正做的是把人類從每一輪的操作員，移到整個系統的設計者。

- [留言1](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/26/e715ef1b-e94f-4f68-a0db-65851fc9ca66_small.jpg)

[敏捷轉型，工具?流程?哪個先行?](https://itplus.ithome.com.tw/vod-page/436?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Agile Summit 敏捷高峰會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/94a9c545-092c-4054-b09e-0f332abc94a6_small.png)

[產業 IT 新戰略 - 完善數據資料免遭勒索加密](https://itplus.ithome.com.tw/vod-page/529?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

20 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/18/6b3f64e9-7a04-46c2-b176-dc4e5531b2be_small.png)

[瞬間整合網路安全服務，就像一個魔法！](https://itplus.ithome.com.tw/vod-page/607?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/e7adb7fd-6bb8-4533-b703-eaecb45ea74f_small.png)

[Prometheus + AutoML 驅動的 AIOps 革命](https://itplus.ithome.com.tw/vod-page/1363?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

44 分

### 1 則留言

1

這篇對 loop 的用處定義好讚，我現在終於懂 loop 實際的功能了，有點像是 ai 界的 unittest 的感覺

希望版主能夠寫一篇關於 harness 的實例🙏🙏🙏🙏

- 回應 2
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[jarryzeng](https://ithelp.ithome.com.tw/users/20169536/profile) iT邦新手 5 級 ‧ 2026-07-18 08:25:15 [檢舉](https://ithelp.ithome.com.tw/users/login)

拍謝我沒看完文章就急著表達我但感嘆了，我現在才注意到版主分享的資源有 harness 的範例😅😅😅

[hardness1020](https://ithelp.ithome.com.tw/users/20183246/profile) iT邦新手 5 級 ‧ 2026-07-19 06:47:37 [檢舉](https://ithelp.ithome.com.tw/users/login)

感謝回覆！有建議或想看什麼歡迎直接到 repo 發 issue

[登入發表回應](https://ithelp.ithome.com.tw/users/login)