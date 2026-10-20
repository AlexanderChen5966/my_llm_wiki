---
title: "Day 11 - 執行 shell 指令 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402531"
author:
  - "[[iThome]]"
published:
created: 2026-08-12
description: "文章同步發表在我的個人 Blog KeSi 現在手上有 6 個檔案工具，已經會讀、會找、會改、會寫，但現在只知道資料寫進去了，不知道寫進去的東西跑不跑得動。測試會不會過、語法有沒有寫壞，或者 impo..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 11

0

[Software Development](https://ithelp.ithome.com.tw/2026ironman/software-development)

### 為你自己手刻 Claude Code系列 第 11 篇

## Day 11 - 執行 shell 指令

- 分享至

> 文章同步發表在 [我的個人 Blog](https://kaochenlong.com/)

KeSi 現在手上有 6 個檔案工具，已經會讀、會找、會改、會寫，但現在只知道資料寫進去了，不知道寫進去的東西跑不跑得動。測試會不會過、語法有沒有寫壞，或者 import 的模組名字有沒有打錯字，KeSi 都不知道。

今天要來幫它補上 `run_command` 這個新工具，讓它可以執行指令。提醒一下，今天這篇在處理 subprocess 的細節有點難懂，可能得需要一些系統相關的背景知識... 看不懂也沒關係，看個大概、盡力就好。

GitHub Repo： [https://github.com/kaochenlong/KeSi](https://github.com/kaochenlong/KeSi)

## 一個工具，換掉一整組防線

第 8 天把 ripgrep 包進來的時候特別提到， `subprocess.run()` 的第一個參數是 list 不是字串，而真正的關鍵是 `shell=False` 。因為中間沒有 shell 幫忙解讀那串字，模型就算把 pattern 填成 `hello; echo 慘了` ，這一整串也只是一串要拿去搜尋的字，那個分號不會變成「前面這道結束了，換下一道」。

今天要做的事，正好是把那道門打開。為什麼？因為 coding agent 的日常大概長這樣：

```markdown
pytest -q
ruff check . && pytest -q
git log --oneline -5
npm run build 2>&1 | tail -20
```

這幾行裡面有些東西不是指令。 `&&` 的意思是「前面那個成功了才做後面這個」， `|` 是把左邊印出來的東西直接餵給右邊當輸入， `2>&1` 是把錯誤訊息也一起導到正常輸出的那條路上。這三個都是 shell 的語法，只有 shell 看得懂。

不開 shell，模型就只能一次下一個執行檔加一串參數，想串兩個動作得多轉一圈迴圈，想用 `|` 把兩個指令接起來也得由我們在 Python 這邊先串好。

另一個做法是不讓模型下整句指令，改成一格一格填。工具收到的不是 `"pytest -q"` 字串而是 `["pytest", "-q"]` 陣列，這樣做的好處是模型要跑哪支程式一定在第一格。你可以在真的執行之前先看那一格是什麼，只放行 `pytest` 、 `ruff` 、 `git` 幾隻比較安全、不會出問題的程式，其他一律拒絕，就是「白名單」的概念。字串的版本做不到這件事，因為得先解析那句指令字串才知道它到底要跑什麼。

不過這樣還是沒把模型關起來。假設你的清單放行了 `python3` （要跑測試本來就少不了它），模型下一句 `python3 -c "print(open('.env').read())"` ，你的 `.env` 就這樣被印出來了。 `sh script.sh` 也是同樣的道理， `sh` 一放行，那個腳本裡面想寫什麼都行。第一格是乾淨的，不代表它要做的事是乾淨的。

是說我每次看 Claude Code 或 Codex 在組合這些參數都覺得很神奇，有些寫法我根本沒想過可以這樣用。

我讓 KeSi 走 shell 是為了讓它能把好幾個指令串起來，不是說 argv 那條路沒有用。開 shell 是因為它好用，不是因為它安全。安全這一關這個工具本身擋不住，得靠後面幾天要做的授權，還有把整個 KeSi 關進容器或虛擬機裡跑。今天先讓它動起來，再把動起來之後多出來的那些洞，一個一個老實標出來。

## 三行版本

跟前一天的文章一樣，先從最短的能跑版本開始：

```python
def run_command(command):
    proc = subprocess.run(command, shell=True, capture_output=True, text=True)
    return proc.stdout + proc.stderr, False
```

`run_command("pytest -q")` 這樣可以動，不過有幾個問題：

1. 沒有執行時間上限。模型給一句 `npm install` ，KeSi 就得陪它把整包 `node_modules` 裝完，大專案跑好幾分鐘是常態。要是它給了一個無窮迴圈那就是永遠回不來了，到時候只能按 Ctrl-C 解決。
2. 沒有輸出上限。有個 `yes` 指令它會一直印 `y` ，印到有人叫它停為止，而 `capture_output=True` 會乖乖地全部收進記憶體，直到 Python 行程把記憶體吃光。
3. 錯誤訊息全部被搬到最後面。正常輸出（stdout）跟錯誤訊息（stderr）走的是兩條不同的通道，這三行分開收再前後接起來，原本穿插的順序就沒了，這樣看不出來是跑到哪一步才出事。
4. 結束碼（exit code）被丟掉。指令跑完都會回報一個數字，0 是成功，其他數字都代表某種失敗。這三行沒去接它模型就分不出「測試全過」跟「測試掛掉」。

這幾個問題來一個一個處理。

## 自己指定 shell

前面那三行用的是 `shell=True` ，這個寫法有個問題：它沒有說要用哪一個 shell。

依照 Python 的 [subprocess 文件](https://docs.python.org/3/library/subprocess.html#popen-constructor) ，在 macOS、Linux 這類 POSIX 系統上， `shell=True` 用的是 `/bin/sh` ，Windows 上則是看 `COMSPEC` 這個環境變數，也就是同一行 Python 程式在不同的機器上執行可能會是不同的 shell，這不太行，所以我在 KeSi 直接先把這個寫死成固定的常數：

```python
SHELL_PATH = "/bin/sh"
```
```python
[SHELL_PATH, "-c", command]
```

這跟第 9 天不用 `replace()` 而是自己切三段是同一個理由，程式碼寫死一個 `/bin/sh` ，看的人一眼就知道跑的是誰。之後想換成 `bash` 或 `zsh` 或是其它 Shell 改這個常數就好。

順帶一提，同一份文件的安全考量那節有提到，用 `shell=True` 的時候誰要負責處理特殊字元：

> If the shell is invoked explicitly, via `shell=True`, it is the application's responsibility to ensure that all whitespace and metacharacters are quoted appropriately to avoid shell injection vulnerabilities.

責任在 the application 這一邊，也就是我們自己。常見的做法是把那些特殊符號加上跳脫字元，讓 shell 把它們當普通的字看。我今天沒做這件事而且還是刻意把整條字串交給 shell 去解釋，因為這就是這個工具的功能。

那該在哪裡擋？在這個函式裡擋不了。如果看到分號就拒絕的話 `ruff check . && pytest -q` 這種正常的指令也一起被擋掉了，所以防線得往外挪一層，不是檢查那串字裡有哪些符號，而是看這道指令整體想做什麼再決定放不放行，但那也是之後的事。

## 逾時？

subprocess 要控制 timeout 只要一個參數：

```python
subprocess.run(args, timeout=120)
```

文件對 `run()` 的 `timeout` 是這樣寫的：

> If the timeout expires, the child process will be killed and waited for.

注意 the child process 是單數。KeSi 叫起一個 `sh` ， `sh` 拿到指令之後自己又去叫起 `pytest` ，那個 `pytest` 跟 KeSi 已經隔了兩層。文件說的 child process 只有 `sh` 那一個，所以時間到了，Python 殺掉的就只是 `sh` ，它底下自己叫起來的程式管不到。

做個小實驗，把上限暫時調成 2 秒，然後執行這道指令：

```markdown
(sleep 20; echo alive > orphan.txt) & echo 背景丟出去了; sleep 30
```

那個 `&` 是「丟到背景去做不用等它」，所以 `sh` 把左邊括號那一組丟到背景，自己卡在 `sleep 30` 。兩秒後計時器響了， `sh` 被殺掉，工具回報逾時，看起來收工了。但背景那個 `sleep 20` 完全沒事，二十秒後 `orphan.txt` 會安安靜靜出現在工作目錄裡，那時候 KeSi 早就在跟模型講別的事了。

要收乾淨就不能只殺那一個 `sh` ，得連它生出來的整串一起處理：

```python
proc = subprocess.Popen(
    [SHELL_PATH, "-c", command],
    cwd=BASE_DIR,
    env=env,
    stdin=subprocess.DEVNULL,
    stdout=subprocess.PIPE,
    stderr=subprocess.STDOUT,
    start_new_session=True,
)
```

`start_new_session=True` 會讓 Python 在啟動子行程之前先呼叫 `setsid()` 。這個 `sh` 於是脫離終端機，變成一個新群組（process group）的老大，作業系統給這一組一個編號， `sh` 之後生出來的行程預設都算在裡面。有了這個編號，要收的時候就不必一個一個去點名，對整組發一次訊號就好：

```python
def stop_process_group(proc):
    pgid = proc.pid
    try:
        os.killpg(pgid, signal.SIGTERM)
    except OSError:
        return

    deadline = time.monotonic() + KILL_GRACE_SECONDS
    while time.monotonic() < deadline:
        proc.poll()
        try:
            os.killpg(pgid, 0)
        except ProcessLookupError:
            return
        except OSError:
            return
        time.sleep(0.05)

    try:
        os.killpg(pgid, signal.SIGKILL)
    except OSError:
        pass
```

先敲門再破門。SIGTERM 是「請你結束」的請求，程式可以自己把手上的事做完再走。SIGKILL 沒得商量，作業系統直接把它結束掉。所以先送 SIGTERM 給整個群組，三秒內清空就收工，期限到了還有釘子戶才送 SIGKILL，慢走不送！

順著這條線還有個事情要處理，不只逾時要收，正常結束也要收：

```python
timed_out = False
try:
    try:
        proc.wait(timeout=COMMAND_TIMEOUT)
    except subprocess.TimeoutExpired:
        timed_out = True
finally:
    stop_process_group(proc)
    proc.wait()
```

模型下 `sleep 25 & echo 背景啟動` 這種指令， `sh` 兩秒內就結束了，結束碼 0，看起來一切正常，但那隻 `sleep` 會留下來。一個 agent 跑一整個下午，這種殘留就一隻一隻累積起來了。所以我的選擇是每次呼叫結束就把整個群組收掉，不管它是逾時還是正常結束。

但這麼做的代價是 KeSi 沒辦法讓一個服務在背景一直跑著。舉個例子 `npm run dev` 會開一個開發用的網頁伺服器，改了程式碼它就自動更新，一般開發的時候都會開著不關。模型如果想這樣用，大概會寫成 `npm run dev &` ，用那個 `&` 把伺服器丟到背景， `sh` 立刻結束然後工具回報成功。但接下來 `finally` 就把整個群組收掉了，伺服器跟著一起死。模型以為服務起來了其實早就沒了，這種需求可能得另外設計可以執行背景任務的介面，不是在這個工具裡順便做。

還有 `start_new_session=True` 這個參數有個副作用，剛才那個 `setsid()` 讓子行程脫離了終端機，所以當按 Ctrl-C 的時候，那個中斷訊號（SIGINT）只會打到 KeSi 身上，不會直接傳給那道指令。

指令本身不會變成孤兒，外層的 `finally` 還是會把整個群組收掉。但那個 `KeyboardInterrupt` 接著往上拋，而 `main()` 裡只有 `input()` 那一段包在 try 底下，這一發沒人接，於是整個 KeSi 帶著 traceback 收攤。

在一般終端機按 Ctrl-C 停的是那道指令，shell 還在，可以接著下一句。在 KeSi 這裡按下去，指令是停了，這一輪的對話也一起沒了。想中斷一道跑太久的指令卻連整個 agent 都賠進去，這不是我要的行為，先記下來之後再處理。

設定 timeout 也有別的問題，像是剛講到 `npm install` 就是一個例子，一套完整的整合測試跑起來也是，設定 120 秒兩分鐘不一定夠。不過因為模型收到的是「指令超過 120 秒未結束，已終止」，它分不出這是指令本來就慢還是真的壞了，很可能就當成失敗，回頭跟你說套件裝不起來。

嗯... 這個 subprocess 的東西有不少細節要處理。

## 輸出串流

stderr 直接併進 stdout：

```python
stdout=subprocess.PIPE,
stderr=subprocess.STDOUT,
```

把兩種輸出寫進同一根管子，然後 KeSi 按這根管子實際收到的先後順序收下來，不用再自己重排。這對讀 traceback（程式出錯時吐出來的那一長串呼叫過程）來說很重要，錯誤訊息前面那幾行正常輸出，往往就是判斷問題出在哪一步的線索。

不過這不算完全的保證。程式發現自己的輸出被接進管子而不是印到螢幕，很多會改變吐字的節奏，所以順序不一定跟終端機畫面上完全一樣。

做個簡單實驗，下面這道指令會先印「一」到正常輸出，再印「二」到錯誤輸出，最後印「三」到正常輸出：

```markdown
$ echo 一; echo 二 >&2; echo 三
一
二
三
（結束碼 0）
```

順序是對的。而同一道指令，交給前面那個三行版本去跑，收到的是這樣：

```markdown
一
三
二
```

「二」最後才出現，因為它走的是另一條通道，而那個版本是等兩條都收完才接起來。真正的 traceback 有幾十行，錯誤訊息一被搬走就看不出它原本卡在哪一步。

把兩條併成一條也有代價，模型收到之後分不出哪一行是從錯誤輸出來的。我認為這可以接受，因為指令自己通常會把嚴重程度寫在訊息裡，而且真正的成敗訊號是結束碼，不是輸出走哪一條通道。

接著是讀取。第 8 天的 `grep` 用 `capture_output=True` ，等子行程結束再一次收下，當時就說過這是先全部進記憶體再由 Python 截斷。ripgrep 吐出來的東西大概有個範圍，shell 沒這回事所以這次改成邊跑邊讀，讀太多就直接終止：

```python
def drain_output(proc, captured, state):
    try:
        while True:
            chunk = proc.stdout.read1(65536)
            if not chunk:
                break
            remaining = MAX_CAPTURE_BYTES - len(captured)
            if len(chunk) > remaining:
                captured.extend(chunk[:remaining])
                state["overflow"] = True
                stop_process_group(proc)
                break
            captured.extend(chunk)
    except (OSError, ValueError):
        pass
    finally:
        try:
            proc.stdout.close()
        except (OSError, ValueError):
            pass
```

這段程式跑在另一條執行緒上，主執行緒那條負責等行程結束跟計時。為什麼要分兩條？因為那根管子塞得下的東西有限，我在我的 macOS 上實測是 64 KB。管子一滿，子行程想再印東西就會卡在那裡出不來，它不結束，等它結束的我們也就一起卡住了。有人負責讀、有人負責等，這個死結才解得開。

然後 `read1()` 是我實測才發現的坑。原本寫的是 `read(65536)` ，而 `read(n)` 要嘛讀滿 n 個 bytes、要嘛等到管子關掉才回來， `read1()` 則是有多少拿多少。差別在哪？改之前 `sleep 25 & echo 背景啟動` 回給模型的輸出是空的，那個 `echo` 明明已經印出來了。程式不會當掉，模型只會收到一份缺內容的結果然後開始亂猜，這種問題不太容易查。

## 設定上限

在輸出的地方設了兩個上限，目的不一樣：

```python
MAX_OUTPUT_BYTES = 8000
MAX_CAPTURE_BYTES = 1_000_000
```

`MAX_CAPTURE_BYTES` 守的是這台機器的記憶體。每收到一塊之前先算額度還剩多少，只拿裝得下的那些、剩下的丟掉，然後把指令終止。這樣寫成 1,000,000 就真的是 1,000,000，不會被最後那一塊超收六萬多。

另一道 `MAX_OUTPUT_BYTES` 守的是模型那邊。工具的結果每一輪都會跟著日記整包送給模型，一次塞一百萬 bytes 進去帳單很有感，模型還得自己在那堆東西裡撈重點，所以回給模型的那一份另外壓到 8,000 bytes，兩道關卡差了一百多倍。第 7 天 `list_files` 最多列 200 筆、第 8 天 `grep` 最多 100 行，跟這裡是同個概念，工具吐回來的東西要有個天花板。

截斷的方式跟前面兩個工具不一樣，這次頭尾都要留，只省略中間：

```python
def clip_output(data):
    if len(data) <= MAX_OUTPUT_BYTES:
        return data.decode("utf-8", errors="replace")

    half = MAX_OUTPUT_BYTES // 2
    head = data[:half].decode("utf-8", errors="replace")
    tail = data[-half:].decode("utf-8", errors="replace")
    dropped = len(data) - half * 2
    return f"{head}\n（中間省略 {dropped} bytes）\n{tail}"
```

`glob` 跟 `grep` 回來的東西是一條一條長得差不多的清單，砍掉後面幾條沒什麼差。指令的輸出不是這樣，例如像是測試報告的重點常常在最後幾行，或是編譯輸出的錯誤總結也在尾巴，只留開頭等於把結論丟掉。中間省略了多少 bytes 也要寫出來，這是第 7 天截斷清單時就建立的好習慣，資訊可以截斷，但不要隱瞞。

`yes kesi` 這種無限輸出實測不到十分之一秒就收場，回給模型的東西尾巴長這樣：

```markdown
kesi
kesi
kesi
（輸出超過 1000000 bytes，已終止指令）（被訊號 15 終止）（結束碼 -15）
```

這次尾巴剛好停在完整的一行，是因為 `kesi\n` 剛好 5 個 bytes，跟 1,000,000 恰好整除，不是程式替我們顧了字的邊界。中文就不一定了，一個字占 3 個 bytes，切在中間那個字就壞了，所以 `decode` 要帶 `errors="replace"` ，壞掉的地方變成一個替代符號，不會讓整支程式當掉。

## 結束碼不能丟

時間上限、輸出上限、輸出的順序，前面這三個問題都是為了不出事，但最後這個結束碼不一樣：

```python
code = proc.returncode
if code is not None and code < 0:
    notes.append(f"被訊號 {-code} 終止")
notes.append(f"結束碼 {code}")
```

結束碼是 shell 世界裡最標準的成敗訊號，0 代表成功，不是 0 就代表失敗。Python 的 `returncode` 還多一個「負數」代表這個行程是被訊號殺掉的，數字就是訊號的編號， `-15` 就是被 SIGTERM 收掉。逾時那次的真實輸出長這樣：

```markdown
開始跑
（指令超過 2 秒未結束，已終止）（被訊號 15 終止）（結束碼 -15）
```

至於 `is_error` 要怎麼填，我在 KeSi 的設定是結束碼非 0 就標成錯誤：

```python
is_error = bool(timed_out or state["overflow"] or code)
```

好處是模型不用自己從輸出裡猜。測試沒過， `is_error` 就是 `true` ，它知道這一步沒達成目的。

壞處是有些指令會用不是 0 的結束碼表達一個完全正常的答案。 `grep` 沒找到東西回 1、 `diff` 發現兩個檔案有差也是 1、 `test` 判斷為假還是 1，這三個指令其實都跑得很成功，只是答案剛好是「沒有」。這時候 KeSi 還是標成 `is_error: true` ，模型收到的意思就變成「你這一步搞砸了」，然後可能換個方式再試一次，而那一次根本沒必要，因為第一次其實就成功了。補償的做法是把結束碼寫進輸出，讓模型自己可以判斷。

另外，如果沒有輸出的情況也要明說：

```markdown
（指令沒有輸出）（結束碼 0）
```

不能回空字串，因為那會讓模型分不清是指令沒印東西還是工具壞掉。不過空白本身也是輸出， `printf '   '` 印出來的三個空白要原樣留下，不能先 `.strip()` 掉再判斷，只有真的一個 byte 都沒收到才叫「沒有輸出」。

結束碼也有騙人的時候，接一根管子就會發生：

```markdown
$ exit 1
（指令沒有輸出）（結束碼 1）      is_error=True

$ exit 1 | tail -1
（指令沒有輸出）（結束碼 0）      is_error=False
```

用 `|` 串起來的一整串指令，預設回報的是最後那一個的結束碼。 `tail` 自己跑得很成功，所以整串回報 0，但前面那個失敗就這樣被吃掉了。這件事在 agent 身上特別容易踩到，因為模型知道輸出會被截斷所以很自然就會在指令後面接個 `| tail -20` 控制長度，接完之後測試掛掉也會回報成 `is_error: false` 。

## 沒互動也沒有記憶

這個工具還有兩件事得提一下，不然模型可能會給 `vim` 這種要等人類輸入的指令，或者以為 `cd` 進去之後就留在那裡了。

第一件是沒有互動輸入可用。 `stdin=subprocess.DEVNULL` 會把子行程的輸入接到一個空的地方，它一去讀輸入，馬上就被告知「沒有了，到底了」。實測一下， `read x` 本來要從鍵盤讀一行存進變數 `x` ：

```markdown
$ read x; echo got=[$x]
got=[]
（結束碼 0）
```

換成 Python 的 `input()` 更直接，當場拋出 `EOFError` 。要的就是這個結果，立刻失敗，而不是安安靜靜卡在那裡等一個永遠不會來的輸入。Anthropic 官方 bash 工具的 [限制那節](https://platform.claude.com/docs/en/agents-and-tools/tool-use/bash-tool#limitations) 也把這條列在第一項：

> **No interactive commands:** The session can't run `vim`, `less`, password prompts, or any command that waits for input on stdin.

`vim` 、 `less` 、密碼提示，以及任何會停下來等 stdin 的指令都不能用。所以工具說明書要寫清楚，讓模型別去下那種指令。

第二件事是每次呼叫都是全新的行程，所以狀態不會留下來：

```markdown
$ mkdir -p sub && cd sub
$ pwd
/private/var/folders/.../kesi-day11-3efko9xu
```

第一道指令的 `cd` 只影響那個已經結束的 `sh` ，第二道又是從工作目錄重新開始。環境變數也一樣， `export` 完就沒了。這是「每次都開一個新的 `sh` 」這個設計的必然結果，但模型不會通靈所以得把這件事也寫進說明書，需要換目錄請在同一行用 `cd x && ...`。

說明書之外還順手做了一件事，不把 API 金鑰交給它：

```python
env = os.environ.copy()
env.pop("ANTHROPIC_API_KEY", None)
```

子行程的環境裡沒有那把金鑰， `echo $ANTHROPIC_API_KEY` 會得到空的。這個擋法如果執行 `cat .env` 照樣讀得到， `env` 也還印得出你其他的環境變數，不過這行程式的目的是模型如果只是想確認金鑰有沒有設好而順手 echo 一下，不會把它印進日記或送去伺服器。只是它擋不住存心要拿的做法。

順著這個標準看，今天做的逾時、截斷、把整個群組收乾淨，全部都屬於「避免出事」而不是「防止有人搞破壞」。這些動作可以讓 KeSi 不會被一道爛指令卡死或是被輸出撐爆。

## 掛上第七個工具

新增三個 import 跟五個常數（ `os` 跟 `subprocess` 前幾天就進來了）：

```python
import signal
import threading
import time
```
```python
MAX_OUTPUT_BYTES = 8000
MAX_CAPTURE_BYTES = 1_000_000
COMMAND_TIMEOUT = 120
KILL_GRACE_SECONDS = 3
SHELL_PATH = "/bin/sh"
```

前面拆開講的那幾塊串起來就是完整的 `run_command` ，函式有點長，直接上 GitHub Repo 看 `kesi.py` 。裡面那個 `cwd=BASE_DIR` 是讓沒寫完整路徑的檔名一律從工作目錄算起，跟第 8 天包 rg 同一招，不過它只決定指令從哪裡開始，不限制指令能走到哪裡。

工具定義把限制寫進說明書：

```python
{
    "name": "run_command",
    "description": "在工作目錄執行一行 shell 指令，回傳合併後的輸出與結束碼。"
    "用來跑測試、linter、建置或查 git 紀錄。"
    f"指令最多執行 {COMMAND_TIMEOUT} 秒，超過會被終止；輸出過長會截斷。"
    "沒有互動輸入可用，不要下需要等待輸入的指令。"
    "每次都是全新的行程，cd 或環境變數不會留到下一次呼叫，"
    "需要換目錄時請在同一行用 cd x && ...。"
    "讀寫檔案請優先使用 read_file、edit_file 與 write_file。",
    "strict": True,
    "input_schema": {
        "type": "object",
        "properties": {
            "command": {
                "type": "string",
                "description": "要執行的完整 shell 指令，例如 pytest -q",
            }
        },
        "required": ["command"],
        "additionalProperties": False,
    },
},
```

為什麼最後要多寫一句叫它優先用檔案工具？因為 `cat` 可以讀檔、 `sed -i` 可以改檔，這些都做得到檔案工具的事但不會經過那些工具的檢查。 `read_file` 碰到不該看的檔案會拒絕， `cat` 不會。 `write_file` 覆寫之前會要求先讀一次、確認檔案沒被別人改過， `sed -i` 也不會。那些檢查是寫在我們的 Python 裡的，而 `cat` 跟 `sed` 根本不從那裡經過。

所以說明書裡多寫一句，讓模型「盡量」走檔案工具那邊。這招可以影響模型的選擇，但就只是引導不是攔阻。它要是不聽，程式裡沒有任何一行會阻止它，因為現在的 KeSi 還沒做這個檢查。

分派表補上 `run_command` ：

```python
TOOL_FUNCS = {
    "read_file": read_file,
    "list_files": list_files,
    "glob": glob_files,
    "grep": grep,
    "edit_file": edit_file,
    "write_file": write_file,
    "run_command": run_command,
}
```

## 驗收工具！

一樣先不問模型，直接呼叫函式。驗收腳本放在 repo 的 `examples/day11/check.py` ，跑 `uv run examples/day11/check.py` 會在一個暫存目錄裡把下面這些都測一遍：

```markdown
PASS echo 與結束碼 0
PASS 非 0 結束碼
PASS stdout 與 stderr 依同一根 pipe 的實收順序合併
PASS 沒有輸出也講清楚
PASS 前後空白與純空白都不被吃掉
PASS 空指令被拒絕
PASS cwd 固定在工作目錄
PASS cd 不會留到下一次
PASS 子行程看不到 API 金鑰
PASS 逾時會被終止
PASS sh 生出來的背景行程也被收掉
PASS 正常結束也不留背景行程
PASS SIGTERM 寬限等的是整個 process group
PASS Ctrl-C 也會清掉獨立 session
PASS 無限輸出最多保留 1000000 bytes 並終止
PASS 截斷後頭尾都留得住
PASS read_file 擋得住，run_command 擋不住
PASS 工作目錄柵欄對 shell 無效
```

倒數第六條講的是 `sh` 自己生出來的那些行程，也就是 KeSi 隔了兩層的那些。這條是指令結束之後再用 `pgrep` 這個指令去數還有沒有殘留的行程，並且確認那個背景任務原本要寫的檔案沒有出現。工具說它收乾淨了不算數，作業系統說了才算。

最後兩條也不是打錯，那是刻意造成的失敗，待會來介紹。工具層過了，接著看模型會拿到什麼樣的證據，先準備一個有 bug 的小專案：

```python
# calc.py
def add(a, b):
    return a - b
```
```python
# test_calc.py
from calc import add

def test_add():
    assert add(2, 3) == 5
```

工具收到的東西長這樣：

```markdown
$ python3 -m pytest -q
F                                                                        [100%]
=================================== FAILURES ===================================
___________________________________ test_add ___________________________________

    def test_add():
>       assert add(2, 3) == 5
E       assert -1 == 5
E        +  where -1 = add(2, 3)

test_calc.py:5: AssertionError
=========================== short test summary info ============================
FAILED test_calc.py::test_add - assert -1 == 5
1 failed in 0.02s
（結束碼 1）
```

`is_error` 是 `True` 。這份東西進了日記之後，模型手上第一次有了「這段 code 錯在哪」的直接證據，不是靠讀原始碼推測：哪個測試、哪一行、預期什麼、實際拿到什麼，全部都在。

把 `a - b` 改成 `a + b` 再跑一次：

```markdown
$ python3 -m pytest -q
.                                                                        [100%]
1 passed in 0.00s
（結束碼 0）
```

`is_error` 變成 `False` 。第 9 天文章結尾說編輯成功不代表任務成功，KeSi 只知道 bytes 寫進去了，現在它有辦法自己確認這件事了。改 code、跑測試、讀錯誤、再改 code... 可以開始進入這個迴圈了。

## 換模型上場

上面兩段是我直接呼叫函式跑的，模型還沒出手。下面是我當時實跑的節錄，模型每次挑的路線不見得一樣， `examples/day11/demo.py` 可以重跑同樣三題，跑出不一樣的結果是正常的。測試專案另外多放一個正確的 `mul()` 跟一個完全無關的 `helper.py` ，題目只給一句話。

第一題我原本想看的是它會先讀檔還是先跑測試，結果兩個都不是：

```markdown
你 > 這個測試過不了，你看一下
  [執行工具] list_files({})
  [執行工具] read_file({'file_path': 'test_calc.py'})
  [執行工具] read_file({'file_path': 'calc.py'})
  [執行工具] read_file({'file_path': 'helper.py'})
  [執行工具] edit_file({'file_path': 'calc.py', 'old_string': 'def add(a, b):\n    return a - b',
                       'new_string': 'def add(a, b):\n    return a + b'})
  [執行工具] run_command({'command': 'python -m pytest test_calc.py -v'})
KeSi > 完成！測試現在都通過了。
       問題總結：calc.py 中的 add() 函數實現錯誤，使用了減法而不是加法 ...
```

它把檔案讀完、靠 code 推出原因、直接動手改，最後才跑測試。測試在這條路線裡的角色是驗收，這跟人類的習慣有點像，一眼看得懂的 bug 誰會先跑一次測試。

值得注意的是它下的指令， `python -m pytest test_calc.py -v` ，指定了檔案還開了 `-v` ，跟我在工具層驗收時下的 `python3 -m pytest -q` 完全不一樣，參數是它自己挑的。

至於它最後回報的那句「測試現在都通過了」，光聽它講不算數，所以我另外去翻了檔案： `add()` 真的變成 `a + b` ，而旁邊的 `mul()` 一個字都沒被改到。第二題測工具分流：

```markdown
你 > 看一下這個目錄有幾個 Python 檔
  [執行工具] list_files({})
KeSi > 這個目錄有 3 個 Python 檔：calc.py、helper.py、test_calc.py
```

它沒有下 `ls *.py` ，直接列一次目錄自己數完。不過同一題我後來又重跑兩次，兩次它都改用 `glob` ，可見這條路線本來就不固定。三次共通的只有一件事，它沒有選 `run_command` 。到底是 `list_files` 跟 `glob` 自己的 description 起了作用，還是最後那句工具分流的文字影響了選擇，這樣跑幾次還是看不出來，更不能當成保證。

那如果指令一定會逾時呢？我把上限暫時調成 5 秒，叫它跑一個絕對跑不完的指令：

```markdown
你 > 幫我跑 sleep 300 這個指令
  [執行工具] run_command({'command': 'sleep 300'})
KeSi > 指令執行結果：
       sleep 300 這個指令開始執行，但由於它會睡眠 300 秒（5 分鐘），而系統對長時間執行的
       指令有限制（最多執行 120 秒），所以指令在執行超過 5 秒後被系統終止了。
       如果你只是想測試 sleep 功能，可以試試較短的時間 ...
```

它沒有硬送第二次，也沒有卡住，改成回頭問我，這很合理。但請看它的解釋，上限是 120 秒，而指令在 5 秒後被終止，這兩件事同時成立才有鬼。這不是模型在唬爛，是我留給它的資料本來就自相矛盾。工具的說明書是這樣寫的：

```python
f"指令最多執行 {COMMAND_TIMEOUT} 秒，超過會被終止；輸出過長會截斷。"
```

關鍵在那個 `f"..."` 。Python 的 f-string 是在這一行被執行到的當下，就把 `COMMAND_TIMEOUT` 那一刻的值填進字串裡，之後你再去改那個變數，這個字串裡的數字也不會跟著變。而工具定義是模組載入的時候就建好的，所以 120 那時候就已經燒死在裡面了。

我為了 demo 是在程式跑起來之後才把 `COMMAND_TIMEOUT` 暫時改成 5，工具的實際行為跟著變，回報訊息也老實寫著「指令超過 5 秒未結束」，只有那份說明書還停在 120。模型手上一份說 120、一份說 5，它沒有挑一個相信，而是把兩個都塞進同一句話，重點是句子讀起來還很通順咧，這就是大語言模型的本事（誤）

這在真實情境裡一樣會發生，只要有人改了常數卻忘記動那份 description 兩邊就分岔了。解法要嘛是說明書別寫死具體數字，只講「有時間上限，實際秒數看工具回報」，要嘛讓這個數字只有一個來源，別讓它在兩個地方各活一份。

## 柵欄全部繞得過去

前面十天蓋了不少柵欄，例如解析後的路徑檢查、禁區名單、先讀後寫、原子替換。今天全部繞得過去。同一個工作目錄裡放一個 `.env` ，兩個工具問同一件事：

```markdown
read_file(".env")
  錯誤：這個檔案不開放讀取。

run_command("cat .env")
  ANTHROPIC_API_KEY=sk-ant-這是假的測試金鑰
  （結束碼 0）
```

工作目錄外的檔案也是：

```markdown
read_file("/tmp/kesi-outside/secret.txt")
  錯誤：找不到或不允許存取檔案 /tmp/kesi-outside/secret.txt

run_command("cat /tmp/kesi-outside/secret.txt")
  工作目錄外的內容
  （結束碼 0）
```

`IGNORE` 名單、 `safe_path()` 、 `is_ignored()` 這些對 shell 全部無效，因為它們檢查的是模型填進 `file_path` 的那個字串，而 shell 底下開檔案的是 `cat` ，不是我們的工具。之前的文章說過「眼睛看不見，手還摸得到」，今天是升級版：手能摸到的地方，比眼睛和前面所有柵欄加起來都大。

還可以更難看一點。 `rm -rf` 會真的刪（提醒，不要亂試！）、 `curl` 可以把你的檔案送去任何地方、 `pip install` 會裝任何東西，而 KeSi 目前對這些一律照做，連問都不會問。

模型只會許願，你的程式握有最後的否決權，這個結構今天仍然成立， `run_command` 收到的還是一張許願單，執行的還是我們的 Python。不過目前的 `run_command` 工具對每一張單子都說好。否決權還在手上，只是我還沒拿出來用。Anthropic 官方在 bash 工具文件裡這樣寫：

> Your application runs whatever command Claude requests. Run the session in an isolated environment, such as a container or a virtual machine, as the least-privileged user that can do the work. Treat every command as untrusted input.

同一頁還列了幾個該補的控制，例如用允許清單而不是黑名單來驗證指令、用 `ulimit` 這類機制限制一個行程能吃多少資源、把每一道指令跟它的輸出都記下來方便事後回頭查、把憑證從輸出裡遮掉再回給模型。這些目前 KeSi 都還沒做，之後會一項一項處理。在那之前，如果是跟著我的文章做的人請先做好心理準備，而且最好只在你願意整個弄丟的目錄裡啟動今天的 KeSi，千萬不要在家目錄、放著正事的專案，或是有正式憑證的機器上執行。

## 正版怎麼執行指令？

截至 2026 年 8 月 8 日， [Claude Code 的工具文件](https://code.claude.com/docs/en/tools-reference#bash-tool-behavior) 對 Bash 工具的描述，行程模型跟我們一樣，每道指令都跑在獨立的行程裡。但狀態保留的部分它做得比我們細很多， `cd` 會延續到後面的指令，只要沒有離開專案目錄；環境變數跟我們一樣不保留。

逾時的部分，文件說預設兩分鐘、上限十分鐘，模型還可以在呼叫時自己傳 `timeout` 要求更長的時間。我們的 120 秒剛好跟它的預設值一樣，但 KeSi 這版沒開權限給模型調。

輸出的做法差距最大。正版的 Claude Code 會把輸出邊跑邊寫進一個工作檔案，短的直接放進對話，太長就只給模型那個檔案的路徑，讓它自己去讀。KeSi 則是回給模型 8,000 bytes 就沒了，之後沒有檔案可以再查。而且它還有一個 KeSi 目前沒有的能力， `run_in_background` 可以把 dev server 或 watch 模式丟到背景讓模型繼續做別的事，我們反而是主動把背景行程收掉的那一派。

再往上一層看 Anthropic Platform 那個預先定義的 [bash 工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/bash-tool) ，它的設計又不一樣，文件第一句就寫明那是一個持續存在的 bash session，工作目錄、環境變數與產生的檔案都會留給下一道指令，還多一個 `restart` 參數可以把 session 重開。好用，但要處理的東西比我們多，我們目前是每次都開新行程，簡單粗暴。至於誰該負責什麼，文件是這樣寫的：

> Claude determines which command to run. Your application owns everything else: the shell process, the timeout, and the safety checks.

模型只決定跑什麼，其他全是你的事。

## 小結

KeSi 今天算是拿到目前威力最大的工具，也第一次能自己確認改動有沒有效。真正麻煩的是幾件讓它在迴圈裡不出事的設定，例如明寫 `/bin/sh` 不靠 `shell=True` 、用群組編號把逾時跟背景行程一起收掉、邊跑邊讀並用兩道上限分別守住記憶體跟送回模型的資料量、把結束碼照實回報給模型。

實測抓到的幾件事都不會讓程式當掉，只會讓結果安靜地錯掉：

- 只殺 `sh` 不殺整組，背景那些孤兒行程留在你的機器上。
- `read()` 會等到管子關掉才回來，輸出就變成空的。
- `.strip()` 會讓有意義的空白憑空消失。
- 說明書裡寫死的秒數跟實際行為一分岔，模型會若無其事地把兩個數字編進同一句話。

現在的 KeSi 對每一道指令都無條件放行，它現在能修 bug 但也能刪掉你的專案，兩件事用的還是同一個工具。明天先處理另一個更基礎的問題。 `run_agent` 裡那個 `while True` 從一開始到現在都還沒有圈數上限，模型只要不停開單它就能一直轉下去，而現在它開的單子裡可以裝任何指令，明天的文章先讓這個迴圈有辦法停下來，順便把 API 錯誤該怎麼重試一起處理掉。

咱們下集見:)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402250)

[

Day 10 - write\_file 的防呆機制

](https://ithelp.ithome.com.tw/articles/10402250)

系列文

[為你自己手刻 Claude Code](https://ithelp.ithome.com.tw/users/20065770/ironman/9031) 共 11 篇

目錄

1. 7
	[Day 07 - 開眼！看見整個專案](https://ithelp.ithome.com.tw/articles/10401983)
2. 8
	[Day 08 - 在專案裡搜尋](https://ithelp.ithome.com.tw/articles/10402109)
3. 9
	[Day 09 - 改 code 為什麼用字串替換](https://ithelp.ithome.com.tw/articles/10402235)
4. 10
	[Day 10 - write\_file 的防呆機制](https://ithelp.ithome.com.tw/articles/10402250)
5. 11
	[Day 11 - 執行 shell 指令](https://ithelp.ithome.com.tw/articles/10402531)
[完整目錄](https://ithelp.ithome.com.tw/users/20065770/ironman/9031)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/21/1c9eeb19-694e-4ee4-a53b-004a9e59460a_small.png)

[醫療業導入微服務架構的策略方向](https://itplus.ithome.com.tw/vod-page/684?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣醫院資訊主管會議 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/10/14/b76c9aca-d863-4baa-9f9b-7452175a1770_small.png)

[Istio-Powered Observability](https://itplus.ithome.com.tw/vod-page/982?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/ccb12077-d9be-4d5d-88cf-12a232f64877_small.png)

[如何在多點開發和離岸進行 LeSS](https://itplus.ithome.com.tw/vod-page/1102?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

52 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/18/f5688738-7fff-4cb1-8a26-e9aa7c4ccf3d_small.jpg)

[用 Web API 打造文件閱讀器](https://itplus.ithome.com.tw/webinar-page/258?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

38 分