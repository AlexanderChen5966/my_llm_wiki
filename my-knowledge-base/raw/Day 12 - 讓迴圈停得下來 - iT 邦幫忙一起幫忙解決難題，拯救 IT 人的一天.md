---
title: "Day 12 - 讓迴圈停得下來 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402619"
author:
  - "[[iThome]]"
published:
created: 2026-08-13
description: "文章同步發表在我的個人 Blog 第 1 天的標題是「Claude Code 其實就只是一個 while 迴圈」，第 6 天真的把這個迴圈寫出來之後，它一直沒有圈數上限，只能先相信模型總有一天會得到答..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 12

0

[Software Development](https://ithelp.ithome.com.tw/2026ironman/software-development)

### 為你自己手刻 Claude Code系列 第 12 篇

## Day 12 - 讓迴圈停得下來

- 分享至

> 文章同步發表在 [我的個人 Blog](https://kaochenlong.com/)

第 1 天的標題是「Claude Code 其實就只是一個 while 迴圈」，第 6 天真的把這個迴圈寫出來之後，它一直沒有圈數上限，只能先相信模型總有一天會得到答案、自己停下來不再開單。

昨天 `run_command` 上線之後就不能再拖了，因為現在每一圈都可能執行任何一道指令，沒有上限表示它想開幾張單就開幾張，程式裡沒有一行會攔它。模型要是不打算停，就只能人類自己按 Ctrl-C 了。

所以，今天不加新工具，先來把這個問題補起來。

GitHub Repo： [https://github.com/kaochenlong/KeSi](https://github.com/kaochenlong/KeSi)

## 先讓它失控！

要看失控不必等它自然發生，我們可以直接製造意外。在第 2 天文用 `ANTHROPIC_BASE_URL` 環境變數把 Claude Code 導到自己寫的 proxy，同一招今天再用一次，只是這次那支程式收到請求之後不再幫忙轉給 Anthropic 伺服器，而是自己造假一份回應丟回去。

我先準備了好幾種可能發生的情境，像是模型一直開同一張單、工具怎麼呼叫都失敗、伺服器回一句「你問太快了」，後面每一種都會用到。挑哪一種是由 `MODE` 這個變數決定，今天先用 `loop` ，表示不管收到什麼，一律回一張 `list_files` 的許願單：

```python
elif MODE == "loop":
    self.ok(tool_use("list_files", {}, "我再看一次目錄。"))
```

於是 KeSi 每一輪都收到同一句「我再看一次目錄」，執行完把結果送回去，下一輪又是同一張單。把昨天的 `kesi.py` 接上去，畫面就一直是這幾行：

```markdown
[執行工具] list_files({})
[執行工具] list_files({})
[執行工具] list_files({})
...
```

跑了 8 秒之後我自己按下 Ctrl-C，它才停。那時候日記已經 329 則，mock server 收到 164 次請求，每一次送出去的那包資料，從第 1 次的 3,949 bytes 長到第 164 次的 48,395 bytes，也就是 3.9 KB 變成 47 KB。日記越寫越長，而每一圈都得把整本重送一次。

這是我自己機器上跑一次的結果，你自己跑數字不會一樣，重點是它不會自己停。

轉這麼快是因為對面是本機的 mock server，馬上就回，換成真的 API 的話每一圈要等模型回話不會這麼快。那 164 圈每一圈都是真的請求也就是都要算錢的，光是七份工具說明書就有 4,130 個 token，164 圈等於把它重送 164 次，六十幾萬個 input token 就這樣出去了，日記那部分還沒算進去。

## 最簡單的數圈圈

最簡單的做法，就是先設定圈數上限：

```python
MAX_TURNS = 20
```
```python
def run_agent(client, history):
    for turn in range(1, MAX_TURNS + 1):
        ...

    message = (
        f"錯誤：這一輪已經用掉 {MAX_TURNS} 次模型往返還沒有結論，先停下來。"
        "可以換個問法、把任務拆小，或用 /reset 重來。"
    )
    history.append({"role": "assistant", "content": message})
    return message
```

`while True` 換成 `for` ，迴圈自然就有盡頭，這裡有三個細節要注意：

第一，圈數上限管的是內圈，不是外圈。內圈是你按一次 Enter 之後模型最多能開幾輪工具單，外圈是一直等你打字的迴圈，它沒有上限，也不該有。所以用滿 20 圈只代表這一題停在這裡，你可以接著問下一句。

第二，停下來的當下日記必須是完整的。這段程式很容易在這裡寫錯，迴圈裡一圈做的事情照順序是這樣：

```markdown
1. 把整本日記送給模型
2. 把模型回的許願單記進日記
3. 如果它沒開單，這一題就結束了
4. 執行單子上的工具
5. 把工具的結果也記進日記
```

檢查圈數的位置必須在第 1 步之前，也就是上一圈的第 5 步做完、日記完整的時候。如果貪圖方便，在第 2 步跟第 4 步中間就 `return` ，日記裡就會留下一張沒有結果的許願單。第 5 天的文章曾經提過這會發生什麼事：

> 一張沒有下文的許願單躺在日記裡，這本日記就再也送不出去了。

這個停下來會把整段對話弄壞。

第三，停下來要留話，也就是上面那句錯誤訊息不能只印在畫面上，還要當成一則 `assistant` 訊息記進日記。這樣你看得到它為什麼停，模型下一輪讀日記的時候也看得到自己上次是被攔下來的，你如果接著說聲「繼續」，模型會知道上一輪發生什麼事，不會從頭再做一遍。如果沒記進去，日記的最後一則會停在工具結果那裡，看起來就像模型正做到一半，那它下一輪很可能接著把剛才那件事再做一次。

20 圈這個數字怎麼來的？沒什麼理論根據就只是喊個大概而已。太小會把正常的多步任務砍斷。第 8 天問「這個專案裡限制檔案存取範圍的邏輯在哪」，模型先列目錄、再搜一次關鍵字、接著讀了兩個檔案才回答得出來，昨天那題修 bug 更長，列目錄、讀三個檔案、改一次 code，最後還跑了測試驗收。這種做下來就是好幾圈，複雜一點的除錯十幾圈也很正常。但如果設定太大則是跟沒設一樣，反正你早就先按 Ctrl-C 了。就先設定個 20 看看，之後真的常常停在這裡再往上調就好。

同一台 mock server 再跑一次，這次它自己停了：

```markdown
KeSi > 錯誤：這一輪已經用掉 20 次模型往返還沒有結論，先停下來。可以換個問法、把任務拆小，或用 /reset 重來。
```

從按下 Enter 到看到這句話是 0.2 秒，日記 42 則，mock server 收到 20 次請求，最後那次送出去的資料是 9,155 bytes。164 次變 20 次，日記 329 則變 42 則。

## 連續失敗？

圈數上限會停，但它可能停得有點太晚了。想像模型一直呼叫 `read_file` 讀同一個不存在的檔案，第 2 次就該察覺不對了，不用等到第 20 圈。所以再補一張網，這次看的是「結果」：

```python
MAX_TOOL_FAILURES = 3
```
```python
if all(result["is_error"] for result in results):
    failures += 1
    if failures >= MAX_TOOL_FAILURES:
        message = (
            f"錯誤：工具連續 {MAX_TOOL_FAILURES} 輪全部失敗，"
            f"在第 {turn} 圈停下來，請換個方式再試。"
        )
        history.append({"role": "assistant", "content": message})
        return message
else:
    failures = 0
```

為什麼是 `all()` 不是 `any()` ？因為模型可以在同一個回應裡一次開好幾張單，三張裡有一張成功，至少代表這一輪不是「全部失敗」，不該加這個計數器。只要有一輪不是全錯就 `failures = 0` ，這個歸零很重要！錯兩次、成功一次、再錯兩次，這樣不會在第 4 次錯的時候被攔下來，因為中間那次成功已經把計數器歸零，後面那兩次得從頭數起。

實測用一台永遠要求讀不存在檔案的 mock server：

```markdown
[執行工具] read_file({'file_path': '不存在的檔案.txt'})
  [執行工具] read_file({'file_path': '不存在的檔案.txt'})
  [執行工具] read_file({'file_path': '不存在的檔案.txt'})
PASS 連續失敗會提早停  mock 收到 3 次請求，比上限 20 圈早停
```

3 圈停，不是 20 圈，那 17 圈的請求就不用送、也不用付錢了。

不過這兩道防護網擋的只有「迴圈一直不停止」跟「工具連續全錯」，它們只會數數，不看模型這幾圈做的是同一件事還是不同的事。所以模型換著參數試三次不同的路、每次都失敗，一樣會被當成連續失敗攔下來。反過來，一直重複呼叫同一個每次都成功的工具，得等到第 20 圈才會停。

## 按下 Ctrl-C

還有一種情況更容易出事，就是中途按下 Ctrl-C 中斷流程。為什麼？

昨天的 `run_command` 一道指令可以跑滿 120 秒，而且我們用了 `start_new_session=True` ，Ctrl-C 不會傳給子行程。這兩件事加起來，「在工具跑到一半按 Ctrl-C」從罕見變成日常。

按下去會發生什麼？Python 收到 Ctrl-C 的時候會丟出一個叫 `KeyboardInterrupt` 的例外，它從工具函式裡一路往上拋，沿路沒有人接，程式就這樣結束了。

問題不在程式結束，在它結束的那個時間點。你按下去的時候，程式正卡在前面那份清單的第 4 步，許願單已經記進日記了，工具的結果還沒寫回去。所以日記的最後一則會是一張沒有下文的許願單，跟剛才把 `return` 寫錯位置的下場一樣。

所以這裡有個規矩，日記裡的每一張許願單，後面都要有一則對應的結果緊跟著。這是 API 的規定，第 6 天列過官方的錯誤訊息：

> tool\_use ids were found without tool\_result blocks immediately after。

中斷可能發生在三個位置，一個都不能漏掉。

第一個，工具執行到一半。這件事昨天那個 `finally` 已經處理掉了，中斷拋出來的時候它照樣會跑，整個行程群組還是收得掉，指令不會留在背景偷偷跑完。所以今天要補的只剩日記那一半。

做法是把工具分派從 `run_agent` 裡抽出來，讓中斷變成一個普通的工具結果：

```python
def run_tools(blocks):
    results = []
    interrupted = False
    for block in blocks:
        if block.type != "tool_use":
            continue
        if interrupted:
            results.append(
                tool_result(block.id, "錯誤：這一輪已中斷，工具沒有執行。", True)
            )
            continue

        print(f"  [執行工具] {block.name}({block.input})")
        try:
            output, is_error = call_tool(block)
        except KeyboardInterrupt:
            interrupted = True
            output, is_error = "錯誤：使用者中斷了這個工具。", True
        results.append(tool_result(block.id, output, is_error))
    return results, interrupted
```

被中斷的那張單子拿到一個錯誤結果，同一批還沒輪到的單子也各自補一張。不是直接跳出，是把每一張單都補上結果。

第二個，中斷被 `except` 誤攔。第 7 天為了不讓單一工具把整個迴圈弄斷，分派那裡加了一層 `except Exception` 。 `KeyboardInterrupt` 剛好不算在 `Exception` 裡面所以穿得過去，不過既然講好一個都不能漏，還是在它前面明寫一句 `except KeyboardInterrupt: raise` 比較不會誤會。

第三個，許願單已經記下、結果還沒寫回去。在等模型回話的那段時間按下去其實是安全的，因為 `assistant` 的回應還沒進日記，最後一則還是 `user` 訊息。危險的是收到回應之後，中斷可能落在工具開始執行之前，也可能落在工具已經真的把檔案改掉、結果卻還沒寫進日記的那一瞬間。

後面這種情況光看日記沒辦法判斷工具到底做了沒，所以補上去的訊息不能武斷地說「沒有執行」，要把這個不確定講出來：

```python
def seal_dangling_tool_use(history):
    """補上懸空許願單的結果，否則這本日記再也送不出去。"""
    if not history or history[-1].get("role") != "assistant":
        return False

    content = history[-1].get("content")
    if not isinstance(content, list):
        return False

    pending = [b for b in content if block_type(b) == "tool_use"]
    if not pending:
        return False

    history.append({
        "role": "user",
        "content": [
            tool_result(
                block_id(b),
                "錯誤：使用者中斷，這個工具的執行狀態不明；"
                "重試前請先檢查現況。",
                True,
            )
            for b in pending
        ],
    })
    return True
```

外圈接住中斷之後呼叫它，然後回去等你的下一句話，而不是讓程式死掉：

```python
history.append({"role": "user", "content": user})
try:
    print("KeSi >", run_agent(client, history))
except KeyboardInterrupt:
    seal_dangling_tool_use(history)
    print("\n（已中斷，可以接著問下一句）")
```

這個修補只能保證 API 要的日記格式完整，不能把「執行狀態不明」變成「一定沒執行」。碰到 `edit_file` 、 `write_file` 或 `run_command` 這種會真的動到東西的工具，下一步應該先讀回現況再說，不要直接重做一次。

## SDK 自己會重試

網路會斷、伺服器會忙，你也可能因為問得太密集被暫時擋下來，這件事叫做限流（rate limit）。這些不是異常狀況，是長時間跑的 agent 的日常。好消息是官方 SDK 已經幫我們做掉一大半了。這件事與其看文件，不如直接翻 SDK 的原始碼，它是開源的，裝完就躺在你的電腦裡。我用的是 `anthropic` 0.120.2 這一版：

```python
# anthropic/_constants.py
DEFAULT_MAX_RETRIES = 2
INITIAL_RETRY_DELAY = 0.5
MAX_RETRY_DELAY = 8.0
```

碰到 429 限流跟 5xx 開頭的錯誤，SDK 都會自己重試，529「伺服器過載」也算在裡面，400 這種請求本身就填錯的則不重試，因為再送一次還是錯。兩次之間會等一下，從 0.5 秒起跳、每次加倍、最多 8 秒，伺服器要是用 `retry-after` 標頭指定了秒數，那就照它說的等。

不用只憑讀 code 相信，讓 mock server 一律回 429 就看得到：

```markdown
第 1 次請求（retry-count=0，body 3936 bytes）
第 2 次請求（retry-count=1，body 3936 bytes，距離上次 0.48 秒）
第 3 次請求（retry-count=2，body 3936 bytes，距離上次 0.85 秒）
```

一次原始請求加兩次重試，剛好是 `DEFAULT_MAX_RETRIES = 2` ，間隔也跟著加倍。你自己跑秒數不會一樣，因為它還乘了一個隨機係數，免得一堆人同時被擋、又同時回頭問。伺服器要是只是暫時出問題，這兩次重試就把事情擋掉了，你完全不會發現剛剛出過事。

要調的話，建立 client 的時候傳參數就好：

```python
client = anthropic.Anthropic(max_retries=5)
```

KeSi 就用預設值 2。

## 重試之後呢？

SDK 的重試會用完。用完之後它會拋例外，而昨天為止的 KeSi 完全沒接，結果就是畫面上噴出一長串錯誤訊息，程式結束，這場對話也沒了。補一下：

```python
try:
    resp = client.messages.create(...)
except anthropic.AnthropicError as exc:
    return describe_api_error(exc)
```

這一整套 API 錯誤最後都算是 `AnthropicError` ，所以接它一個就收得完。不過它只管 API 錯誤，程式自己寫壞拿到的 `ValueError` 不在裡面。至於怎麼講給人聽，另外寫一個函式來處理：

```python
def describe_api_error(exc):
    """這裡的讀者是人，不是模型；模型根本沒收到這個請求。"""
    if isinstance(exc, anthropic.RateLimitError):
        return "錯誤：重試後仍被限流（429），等一下再試。"
    if isinstance(exc, anthropic.OverloadedError):
        return "錯誤：伺服器忙碌中（529），重試後仍未成功，等一下再試。"
    if isinstance(exc, anthropic.BadRequestError):
        return (
            f"錯誤：請求被拒絕（400）：{exc}。"
            "如果是對話太長，可以用 /reset 清空後重來。"
        )
    if isinstance(exc, anthropic.APITimeoutError):
        return "錯誤：等待模型回應逾時。"
    if isinstance(exc, anthropic.APIConnectionError):
        return f"錯誤：連不上 API：{exc}"
    if isinstance(exc, anthropic.APIStatusError):
        return f"錯誤：API 回應 {exc.status_code}：{exc}"
    return f"錯誤：{type(exc).__name__}：{exc}"
```

之前文章講過官方建議工具的錯誤訊息要寫得有指導性，因為看的人是模型。今天這幾行剛好相反，這些字是寫給坐在終端機前面的人看的，因為請求根本沒送到模型手上。

判斷順序也不能隨便排，範圍小的要放前面。 `APIStatusError` 收的是所有帶狀態碼的錯誤， `RateLimitError` 只收 429，要是把範圍大的那個寫在前面，被限流的時候你就只會看到籠統的「API 回應 429」，專門為 429 寫的那句提示永遠輪不到。

400 這一類裡有一種是第 4 天預告過的 `prompt is too long` ，也就是日記太長，超過模型一次讀得下的量。這種特別討厭，日記已經太長了，你再問下一句只會更長，每次送都失敗。所以那句提示要把出路直接寫出來，讓人知道可以用 `/reset` 清空重來。（真正的解法是把長對話壓縮掉，那是之後的事。）

模型的回應寫到一半被切斷的時候，這一輪也會停。有兩種情況會這樣，一種是輸出達到 `max_tokens` 這個上限，KeSi 設的是 1024；另一種是整段對話塞滿了模型一次讀得下的量，也就是 context window。被切斷的回應可能停在一張還沒寫完的許願單上，參數只填了一半，這種東西不能拿去執行，所以一樣是停在這一輪、補一則訊息進日記，順便講清楚是哪一種：

```markdown
錯誤：模型輸出達到 max_tokens，本輪沒有執行工具。
錯誤：模型回應填滿 context window，本輪沒有執行工具。
```

還有一個沒寫在程式裡、但驗收有測的行為，API 呼叫失敗的時候日記不動。

```markdown
PASS API 失敗不會在日記留下半截訊息  日記 1 則
```

那一輪的 `user` 訊息還在，assistant 什麼都沒補。這樣你換個問法或稍後重試都接得下去，不會在日記裡留下一則假的「我失敗了」讓模型之後讀到。

## 驗收！

一樣先不靠模型，全程走本機 mock server，不花 API 錢也不用等真的被限流：

```markdown
PASS 無限開單會被 MAX_TURNS 攔住
PASS 用滿上限後日記結尾是 assistant
PASS 連續失敗會提早停
PASS 429 會被 SDK 自動重試
PASS 重試用盡回錯誤訊息而不是 traceback
PASS API 失敗不會在日記留下半截訊息
PASS retry-after 指定的秒數會被照做
PASS 529 會重試，訊息說得清楚
PASS 400 不重試，直接回報
PASS 400 有 x-should-retry: true 仍會重試
PASS 500 重試兩次後成功，使用者無感
PASS max_tokens 會回報輸出上限
PASS context window 會回報 context 上限
PASS 工具被中斷後每張許願單都有結果
PASS 懸空的許願單會被補上結果
PASS 沒有懸空時不會亂補
PASS run_command 被中斷後子行程群組會停止
```

上面那 17 條的驗收腳本放在 repo 的 `examples/day12/check.py` ，它會自己把 mock server 叫起來，全部通過會回結束碼 0：

```markdown
uv run examples/day12/check.py
```

這台 mock server 放在 `examples/day12/mock.py` ，平常由驗收腳本自己叫起來，想單獨試某一種情境的話，用 `MOCK_MODE` 這個環境變數指定就好：

```markdown
MOCK_MODE=429 uv run examples/day12/mock.py
```

真實的 429 等不到、真實的 529 叫不來，但這幾種又剛好是 agent 最容易出事的地方，自己造一台會壞的伺服器，想測隨時可以測。

今天完整的 `kesi.py` 一樣可以在 GitHub Repo 取得。

## 正版怎麼防打轉

這次看 Google 的 Gemini CLI，它是開源的，可以直接讀 code。下面講的都是 2026-08-08 那天查到的版本，連結固定在 [`cf22ac7`](https://github.com/google-gemini/gemini-cli/blob/cf22ac7e86f3dcf528e3ae591fec1c03090a49f8/packages/core/src/services/loopDetectionService.ts) 這個 commit 上。

它也有硬上限，叫 `model.maxSessionTurns` ，文件寫的預設值是 `-1` ，也就是不限制。真正在做事的是另一個東西，叫 `LoopDetectionService` ，它裡面有三道檢查，做法比我們今天這兩道網細膩很多。

第一道看工具呼叫。它把工具名稱加上參數算出一組 SHA-256 當成這次呼叫的識別碼，然後檢查最近的呼叫紀錄裡有沒有重複的模式：

```typescript
const R = TOOL_CALL_LOOP_THRESHOLD; // 5

// Check for repeating patterns of cycle length k from 1 to 5
for (let k = 1; k <= 5; k++) {
```

注意它找的不只是「同一個工具連叫五次」，而是週期長度 1 到 5 的循環。 `A→A→A→A→A` 會被抓到， `A→B→A→B→A→B` 這種兩步一循環的也會被抓到。參數也算進識別碼裡，所以換了參數就不算重複，因為那是在試不同的路。

第二道看模型吐出來的文字有沒有一直重複。第三道乾脆請另一顆模型來判斷，把最近 20 輪的紀錄送過去問「這是不是在原地打轉」，信心值 0.9 以上才算數。它給那顆模型的 system prompt 是這樣寫的：

> An unproductive state requires BOTH of the following to be true:
> 
> 1. The assistant has exhibited a repetitive pattern over at least 5 consecutive model actions (tool calls or text responses, counting only model-role turns).
> 2. The repetition produces NO net change or forward progress toward the user's goal.

它強調「重複」跟「沒有進展」要同時成立，後面還特別交代了兩種不算迴圈的情況：一種是改完 code 再跑一次 build 驗證，那是正常流程；另一種是每次改的 code 不同、拿到的錯誤也不同，那是在除錯不是打轉。

這正好是我們今天這兩道網缺的東西。只會數數的好處是簡單、便宜、該停的都停得下來，代價是有時候會誤判。

KeSi 這版先這樣。要再進一步話可以學它把工具名稱加參數算出一個識別碼，記錄最近幾次，連續打到同一個就提早停。人家那份 `loopDetectionService.ts` 是 781 行，我們這個只要幾行，就能擋掉最常見的那種原地打轉。

## 小結

今天沒加工具，補的是地基。 `while True` 換成有盡頭的 `for` ，連續全錯提早喊停，中斷之後日記還是送得出去，API 出事也不會炸掉整場對話。

程式碼之外，有幾件事也一起記著：

- 停下來的位置要挑在日記完整的時候，不然停下來等於弄壞對話。
- 停下來要留話，讓人跟模型都知道發生什麼事。
- 錯誤訊息要先問「這句話是寫給誰看的」，模型跟人要分開對待。

這 12 天累積下來的東西，現在是一個轉得動也停得下來的迴圈、七個工具、一層擋在工作目錄外的路徑檢查、一組「沒讀過就不准覆寫」的規矩，加上今天這幾張網。

明天就拿這些去實戰，我會故意準備一個真的有 bug 的小專案，把題目丟給 KeSi，看它自己讀測試、找原因、改 code、跑測試。過程我會完整記下來，包含它不小心走的彎路以及花掉的錢錢。💸

咱們下集見:)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402531)

[

Day 11 - 執行 shell 指令

](https://ithelp.ithome.com.tw/articles/10402531)

系列文

[為你自己手刻 Claude Code](https://ithelp.ithome.com.tw/users/20065770/ironman/9031) 共 12 篇

目錄

1. 8
	[Day 08 - 在專案裡搜尋](https://ithelp.ithome.com.tw/articles/10402109)
2. 9
	[Day 09 - 改 code 為什麼用字串替換](https://ithelp.ithome.com.tw/articles/10402235)
3. 10
	[Day 10 - write\_file 的防呆機制](https://ithelp.ithome.com.tw/articles/10402250)
4. 11
	[Day 11 - 執行 shell 指令](https://ithelp.ithome.com.tw/articles/10402531)
5. 12
	[Day 12 - 讓迴圈停得下來](https://ithelp.ithome.com.tw/articles/10402619)
[完整目錄](https://ithelp.ithome.com.tw/users/20065770/ironman/9031)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/26/6975ca4f-767f-46c4-a9e3-6c4198204dc5_small.png)

[從混亂到有效治理：Policy as Code 在跨團隊 CI/CD 的實踐與應用](https://itplus.ithome.com.tw/vod-page/1177?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/613c15de-b530-4ab1-96ea-bc0b4be94442_small.jpg)

[解鎖 AI 功能與生產力｜Dell 與 Windows 11 Pro 軟硬搭配](https://itplus.ithome.com.tw/vod-page/1252?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

45 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/b5c33eaa-9086-4e86-9382-a01910a707db_small.png)

[我們與「敏捷」的距離](https://itplus.ithome.com.tw/vod-page/1360?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

48 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/786a51f4-a5d4-4cbe-af56-284e2e9802ad_small.png)

[走出混沌的DevOps時代: 智慧協作](https://itplus.ithome.com.tw/vod-page/1491?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

46 分