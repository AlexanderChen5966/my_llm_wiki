---
title: 手刻 agent 的停止機制：停在日記完整的位置，而且要留話
date: 2026-08-13
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10402619（《為你自己手刻 Claude Code》Day 12，作者高見龍，專案 KeSi）
type: article
status: processed
asset_value: 62
---

同系列 Day 02–04 見 [[claude-code-api-request-reverse-engineering]]、Day 05–10（六個檔案工具）見 [[hand-coding-agent-file-tools]]、Day 11（`run_command`）見 [[handcraft-agent-shell-tool]]。前一天給了 agent 執行任意指令的能力，這一天補的是地基：**沒有上限的迴圈，加上能跑任何指令的工具，等於沒有一行程式會攔它。**

## T（核心論點）

`while True` 換成有盡頭的 `for` 只是最表面的一步。真正的重點是三條規矩：

> **① 停下來的位置要挑在「日記完整」的時候——停錯位置等於弄壞整段對話。**
> **② 停下來要留話——讓人跟模型都知道發生了什麼事。**
> **③ 錯誤訊息要先問「這句話是寫給誰看的」——模型跟人要分開對待。**

## B（適用場景）

**適用：**
- 自建 agent 的停止條件、中斷處理與 API 錯誤處理完整檢查表
- 對照理解 Claude Code / Gemini CLI 為什麼要做迴圈偵測
- 用 mock server 造假回應來測「真實環境等不到」的失敗情境（429／529／無限開單）

**不適用 / 注意：**
- 實作為 Python + `anthropic` SDK（文中版本 0.120.2）
- ⚠️ 本篇的兩道網**只會數數**，不看模型這幾圈做的是同一件事還是不同的事——換參數試三條不同的路一樣被當連續失敗攔下；一直重複呼叫同一個每次都成功的工具，得等到第 20 圈才停

## R（四道防護與實測數字）

### 0. 先製造失控（可複製的測試手法）

用 `ANTHROPIC_BASE_URL` 把請求導到自寫 proxy，但這次 proxy 不轉發，而是**自己造假一份回應**丟回去（`MODE=loop` 一律回同一張 `list_files` 許願單）。實測結果：

| 指標 | 無上限 | 加 `MAX_TURNS=20` |
|---|---|---|
| mock server 收到請求 | 164 次（8 秒後人工 Ctrl-C） | 20 次 |
| 日記則數 | 329 則 | 42 則 |
| 最後一次送出 body | 48,395 bytes（首次 3,949） | 9,155 bytes |

🔑 **每一圈都得把整本日記重送一次**，所以 body 從 3.9 KB 長到 47 KB。光是七份工具說明書就有 4,130 token，164 圈等於重送 164 次，六十幾萬 input token（日記還沒算進去）。

### 1. 圈數上限 `MAX_TURNS = 20`

三個細節：

- **管的是內圈不是外圈**——內圈＝按一次 Enter 後模型最多開幾輪工具單；外圈＝等你打字的迴圈，它沒有上限也不該有
- **檢查位置必須在「把日記送給模型」之前**，也就是上一圈的「把工具結果記進日記」做完、日記完整的時候。若圖方便在「記下許願單」與「執行工具」之間就 `return`，日記裡會留下一張沒有結果的許願單——**這本日記就再也送不出去了**
- **停下來要當成 `assistant` 訊息記進日記**，不能只印在畫面上。否則日記最後一則停在工具結果，看起來像模型正做到一半，下一輪很可能把剛才那件事再做一次

⚠️ 20 這個數字作者自己說「沒什麼理論根據就只是喊個大概」：太小會砍斷正常多步任務（除錯十幾圈很正常），太大則跟沒設一樣。

### 2. 連續失敗 `MAX_TOOL_FAILURES = 3`

```python
if all(result["is_error"] for result in results):
    failures += 1
    if failures >= MAX_TOOL_FAILURES: ...停下來
else:
    failures = 0
```

- 用 `all()` 不用 `any()`：模型可以在同一個回應裡開好幾張單，三張裡有一張成功就不算「全部失敗」
- **歸零很重要**：錯二、成功一、再錯二，不會在第 4 次錯時被攔——中間那次成功已經把計數器歸零

實測：3 圈就停，不是 20 圈，省下 17 圈的請求與費用。

### 3. Ctrl-C 中斷的三個位置（一個都不能漏）

因為 `run_command` 一道指令可跑滿 120 秒且用了 `start_new_session=True`（Ctrl-C 不傳給子行程），「工具跑到一半按 Ctrl-C」從罕見變日常。

規矩：**日記裡每一張許願單，後面都要有一則對應的結果緊跟著**（API 硬性要求，錯誤訊息即 `tool_use ids were found without tool_result blocks immediately after`）。

1. **工具執行到一半** — 把工具分派抽成 `run_tools()`，讓中斷變成一個普通的工具結果：被中斷的那張單拿到錯誤結果，同一批還沒輪到的單也**各自補一張**（不是直接跳出）
2. **中斷被 `except` 誤攔** — `KeyboardInterrupt` 不算在 `Exception` 裡本來就穿得過去，但仍明寫 `except KeyboardInterrupt: raise` 比較不會誤會
3. **許願單已記下、結果還沒寫回去** — 用 `seal_dangling_tool_use()` 補上結果，外圈接住中斷後呼叫它，然後回去等下一句話而不是讓程式死掉

🔴 第 3 種情況**光看日記無法判斷工具到底做了沒**，所以補的訊息不能武斷說「沒有執行」，要把不確定講出來：「使用者中斷，這個工具的執行狀態不明；重試前請先檢查現況。」碰到 `edit_file` / `write_file` / `run_command` 這種會真的動到東西的工具，下一步應先讀回現況，不要直接重做。

### 4. API 錯誤：SDK 已經做掉一大半

直接翻 SDK 原始碼（`anthropic/_constants.py`）：

```python
DEFAULT_MAX_RETRIES = 2
INITIAL_RETRY_DELAY = 0.5
MAX_RETRY_DELAY = 8.0
```

- 429 與 5xx（含 529 過載）自動重試；**400 不重試**（再送一次還是錯）
- 0.5 秒起跳、每次加倍、最多 8 秒，並乘一個隨機係數（免得一堆人同時被擋又同時回頭問）；伺服器給 `retry-after` 就照它說的等
- 要調就 `anthropic.Anthropic(max_retries=5)`

重試用完會拋例外，全部收斂在 `AnthropicError`。⚠️ **判斷順序範圍小的要放前面**：`APIStatusError` 收所有帶狀態碼的錯誤，若寫在 `RateLimitError` 前面，被限流時只會看到籠統的「API 回應 429」。

🔑 **這幾行錯誤訊息的讀者是人，不是模型**——因為請求根本沒送到模型手上。這與「工具的錯誤訊息要寫得有指導性（給模型看）」剛好相反。

## C（關鍵行動）

- [ ] 檢查停止判斷的**位置**：是不是落在「日記完整」的時刻？停錯位置比不停更糟
- [ ] 停下來有沒有記一則 `assistant` 訊息進歷史？沒有的話模型下一輪會重做
- [ ] API 呼叫失敗時日記**不動**（那一輪的 user 訊息還在，assistant 什麼都沒補），才能換個問法接下去

## 補充資料

### 兩種被切斷的回應也要停

- 輸出達 `max_tokens`（KeSi 設 1024）
- 整段對話塞滿 context window

被切斷的回應可能停在一張參數只填一半的許願單上，這種東西不能執行——一樣停在這一輪、補一則訊息並講清楚是哪一種。400 裡的 `prompt is too long` 特別討厭（日記已太長，再問只會更長），所以提示要直接寫出路：可以用 `/reset` 清空重來。（真正的解法是壓縮長對話，見 [[agent-context-management]]）

### 正版怎麼防打轉：Gemini CLI 的 LoopDetectionService

（作者查的是 2026-08-08 的 commit `cf22ac7`，781 行）

它的硬上限 `model.maxSessionTurns` 預設 `-1`（不限制），真正做事的是三道檢查：

1. **工具呼叫** — 工具名稱＋參數算 SHA-256 當識別碼，檢查**週期長度 1 到 5 的循環**（`A→A→A→A→A` 與 `A→B→A→B→A→B` 都抓得到）。參數算進識別碼，所以換參數不算重複（那是在試不同的路）
2. **模型輸出文字**有沒有一直重複
3. **請另一顆模型判斷** — 把最近 20 輪送過去問「是不是在原地打轉」，信心值 ≥0.9 才算數

它的 system prompt 強調兩件事**必須同時成立**才算 unproductive：

> ① 至少連續 5 個 model action 呈現重複模式；② 該重複對使用者目標**沒有任何淨進展**

並明列兩種不算迴圈的情況：改完 code 再跑 build 驗證（正常流程）；每次改的 code 不同、錯誤也不同（在除錯不是打轉）。

🔑 **這正是「只會數數」缺的東西**——簡單、便宜、該停的都停得下來，代價是有時誤判。作者建議的中間路線：學它把工具名稱＋參數算成識別碼、記錄最近幾次，連續打到同一個就提早停，幾行就能擋掉最常見的原地打轉。

### 17 條驗收全走本機 mock server

不花 API 錢也不用等真的被限流：無限開單被攔 / 用滿上限後日記結尾是 assistant / 連續失敗提早停 / 429 自動重試 / 重試用盡回錯誤訊息而非 traceback / API 失敗不留半截訊息 / `retry-after` 被照做 / 529 重試 / 400 不重試 / 400 帶 `x-should-retry: true` 仍重試 / 500 重試兩次後成功使用者無感 / max_tokens 與 context window 各自回報 / 工具被中斷後每張許願單都有結果 / 懸空許願單被補上 / 沒有懸空時不會亂補 / `run_command` 被中斷後子行程群組會停止。

腳本在 repo 的 `examples/day12/check.py`，mock server 在 `examples/day12/mock.py`（`MOCK_MODE=429` 可單獨試某情境）。🔑 **真實的 429 等不到、真實的 529 叫不來，但這幾種剛好是 agent 最容易出事的地方——自己造一台會壞的伺服器，想測隨時可以測。**

## 相關連結

- [[synthesis/synthesis-agent-engineering-architecture]] — **本篇為其來源之一**（2026-09-03 重跑，8→16 篇）：Agent 工程架構精粹，含 Capability scaffolding vs Control plane 二分法

- [[handcraft-agent-shell-tool]] — 同系列 Day 11，`run_command` 與 `finally` 收行程群組
- [[hand-coding-agent-file-tools]] — 同系列 Day 05–10，六個檔案工具與四道防呆
- [[claude-code-api-request-reverse-engineering]] — 同系列 Day 02–04，payload 與 fake memory
- [[ai-agent-core-loop]] — 約 80 行迴圈的核心結構
- [[agent-loop-explained]] — Loop 概念與四層階梯
- [[agent-context-management]] — 長對話壓縮（本篇提到的「真正解法」）
- [[gemini-cli-subagent]] — Gemini CLI 的其他機制
