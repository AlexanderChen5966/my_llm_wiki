---
title: "Day 08 - 在專案裡搜尋 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402109"
author:
  - "[[iThome]]"
published:
created: 2026-08-10
description: "文章同步發表在我的個人 Blog 上一篇文章三個問題只解了兩個，還剩最後一個，哪個檔案裡有我想要找的內容？ 目前幫 KeSi 裝的眼睛可以看到檔案的名字但還看不到內容。如果問它「safe_path 這..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 8

0

[Software Development](https://ithelp.ithome.com.tw/2026ironman/software-development)

### 為你自己手刻 Claude Code系列 第 8 篇

## Day 08 - 在專案裡搜尋

- 分享至

> 文章同步發表在 [我的個人 Blog](https://kaochenlong.com/)

上一篇文章三個問題只解了兩個，還剩最後一個，哪個檔案裡有我想要找的內容？

目前幫 KeSi 裝的眼睛可以看到檔案的名字但還看不到內容。如果問它「 `safe_path` 這個函式被誰用到？」，現在大概會用比較笨的方式，先 `glob` 出所有 Python 檔，然後一個一個 `read_file` 把檔案讀進來自己找。小專案還行，大一點的專案幾十個檔案的內容會陸續寫進日記，現在大家也知道把這些不必要的內容寫進日記要付出的代價了。

這篇文章就來幫 KeSi 裝上可以搜尋檔案內容的新功能！

GitHub Repo： [https://github.com/kaochenlong/KeSi](https://github.com/kaochenlong/KeSi)

## 自己寫搜尋功能？

這個系列一開始就有提到自己動手做的邊界，agent 的骨架會自己寫，但跟骨架無關的我們就不造輪子了，「搜尋」就是最標準的已有成熟輪子的範例。

Python 有個 `re` 模組，搭配 `os.walk` 走一遍檔案自己比對，大概也就幾十行程式還算好寫，小專案不會有什麼問題，但像是目錄走訪、ignore 規則、隱藏檔、二進位判斷、文字編碼、錯誤回報與輸出上限就都得自己處理，效能問題也要另外驗證，上一篇文章的「作業感」會原封不動再體會一次。

評估了一下目前現成選項裡， [ripgrep](https://github.com/BurntSushi/ripgrep) ，又稱 `rg` ，是個很適合做這個工作的工具，本體用 Rust 寫成。這些工程已經有人長期維護所以我就不再用 Python 重做一套。 `rg` 用的人也不少，例如 Microsoft 維護的 [`vscode-ripgrep`](https://github.com/microsoft/vscode-ripgrep) 專案也是用這個套件做的，並且把各平台的 ripgrep 執行檔一起包進 npm 套件。若你曾經用過 VS Code 做全專案搜尋，這背後就是它。

所以今天的工具策略改成不自己實作搜尋功能，而是把現成工具包進來給 KeSi 用。macOS 可以照 [官方安裝說明](https://github.com/BurntSushi/ripgrep#installation) 執行 `brew install ripgrep` ，其他平台也有各自的套件或預編譯版本。裝完先在終端機執行 `rg --version` ，確認目前 shell 找得到它，再重新啟動 KeSi。

萬一沒有安裝就啟動 KeSi，程式也不會炸掉。模型呼叫 `grep` 的時候會收到一句「錯誤：找不到 `rg` 指令，請先安裝 `ripgrep` 。」，這個工具結果也會被標記成錯誤，送回去提醒模型（還有你）。

我選 ripgrep 的另一個理由，就是它在做遞迴搜尋的時候本身就會先篩掉一批檔案。README 文件有提到 `rg` 預設會參考 ignore 規則直接自動略過隱藏檔案以及二進位內容，這直接可以省掉幾個麻煩：

- 在 Git repo 裡， `rg` 會讀取 `.gitignore` ；另外也會讀 `.ignore` 跟 `.rgignore` ，所以要走訪的檔案就可以少幾個。
- 小數點開頭的路徑遞迴時預設不會進去，例如 `.git` 跟 `.env` 都是。
- 遇到疑似二進位的內容就停下來，不會把它當一般文字進行搜尋。判斷的方式是看內容裡有沒有出現值為 0 的那個 byte，這個 byte 叫做 `NUL` ，一般純文字檔幾乎用不到它，不過像是圖片跟執行檔裡倒是不少。如果某個文字檔裡夾了一個 `NUL` ，rg 也會把它當成二進位，並不是真的去認 PNG、ZIP 這些格式。

是說，`.gitignore` 的用途是這個檔案裡列的內容刻意不交給 Git 追蹤，不是「不重要」，不過對搜尋工具來說這剛好是個好用的「不搜尋」的預設值，KeSi 不必自己實作整套 `.gitignore` 語法，就先繼承了這些規則。

來做個實驗，開一個乾淨的目錄，放一個假的 `.env` 跟一個 `app.py` 進去試試看：

```markdown
$ rg SECRET
（指令沒有輸出）（結束碼 1）

$ rg SECRET .env
API_SECRET=fake-value-for-test
（結束碼 0）
```

同樣的 `SECRET` 、同一個目錄，兩次搜出來的結果卻不一樣。第一次是叫 rg 自己去整個目錄裡翻，它一個字都沒印出來，後面那個結束碼是指令跑完留下的回報，rg 用 1 表示「找過了，但沒找到」。第二次把 `.env` 這個檔案名字直接跟在指令後面，表示這次由我指定要在哪個檔案進行搜尋，不是讓它自己去挑。簡單地說， `rg` 的預設功能可以讓搜尋過程的雜訊更少一點。

## 包一個子行程

而要把 `rg` 包進 KeSi 的方法是把 `rg` 當子行程叫起來。子行程的意思是由 KeSi 這支程式去啟動另一支程式，等它跑完再把它印出來的東西收回來自己處理，Python 內建的 `subprocess` 模組就是專門做這件事的。下面只列今天新增的部分， `BASE_DIR` 、 `IGNORE` 、 `safe_path()` 與 `is_ignored()` 沿用昨天的版本：

```python
import shutil
import subprocess

MAX_SEARCH_LINES = 100
MAX_ERROR_CHARS = 2000
RG_PATH = shutil.which("rg")
RG_EXCLUDES = [
    glob
    for name in sorted(IGNORE)
    for glob in (f"!**/{name}", f"!**/{name}/**")
] + ["!**/.env.*", "!**/.env.*/**"]

def grep(pattern, path="."):
    if not isinstance(pattern, str) or not pattern:
        return "錯誤：pattern 不可為空。", True

    target = safe_path(path)
    if target is None:
        return f"錯誤：找不到或不允許搜尋路徑 {path}", True
    if is_ignored(target):
        return "錯誤：這個路徑不開放搜尋。", True
    if not (target.is_file() or target.is_dir()):
        return f"錯誤：{path} 不是可以搜尋的檔案或目錄", True
    if RG_PATH is None:
        return "錯誤：找不到 rg 指令，請先安裝 ripgrep。", True

    relative_path = target.relative_to(BASE_DIR).as_posix() or "."
    args = [
        RG_PATH,
        "--no-config",
        "--no-follow",
        "--line-number",
        "--with-filename",
        "--no-heading",
        "--color",
        "never",
        "--max-columns",
        "200",
        "--max-columns-preview",
    ]
    for excluded in RG_EXCLUDES:
        args.extend(["--glob", excluded])
    args.extend(["--", pattern, relative_path])

    try:
        proc = subprocess.run(
            args,
            cwd=BASE_DIR,
            stdin=subprocess.DEVNULL,
            capture_output=True,
            text=True,
            encoding="utf-8",
            errors="replace",
            timeout=10,
        )
    except FileNotFoundError:
        return "錯誤：找不到 rg 指令，請先安裝 ripgrep。", True
    except subprocess.TimeoutExpired:
        return "錯誤：搜尋超過 10 秒被中止，請縮小範圍後再試。", True
    except OSError as exc:
        return f"錯誤：無法執行搜尋：{exc}", True

    if proc.returncode == 1:
        return f"找不到含有 {pattern} 的內容。", False
    if proc.returncode != 0:
        detail = proc.stderr.strip() or "rg 沒有提供錯誤訊息"
        return f"搜尋失敗：{detail[:MAX_ERROR_CHARS]}", True

    lines = []
    for line in proc.stdout.splitlines():
        if line.startswith(("./", ".\\")):
            line = line[2:]
        lines.append(line)
    if len(lines) > MAX_SEARCH_LINES:
        head = "\n".join(lines[:MAX_SEARCH_LINES])
        return (
            f"{head}\n（共有 {len(lines)} 行，只顯示前 {MAX_SEARCH_LINES} 行）",
            False,
        )
    return "\n".join(lines), False
```

工具定義照舊，記得 `TOOL_FUNCS` 也加一條：

```python
{
    "name": "grep",
    "description": "在工作目錄的檔案內容中搜尋文字，支援正規表達式，"
    "回傳「檔名:行號:該行內容」格式。想知道某個函式、變數或字串"
    "出現在哪些檔案時用這個，不要一個一個檔案讀。",
    "strict": True,
    "input_schema": {
        "type": "object",
        "properties": {
            "pattern": {
                "type": "string",
                "description": "要搜尋的文字或正規表達式",
            },
            "path": {
                "type": "string",
                "description": "限定搜尋的檔案或子目錄，省略時搜整個工作目錄",
            },
        },
        "required": ["pattern"],
        "additionalProperties": False,
    },
},
```

這裡有幾個寫法可以解釋一下。先看 `subprocess.run` 的第一個參數，它是一個 list，指令跟每個參數各佔一格而不是黏成一整條字串。 `subprocess.run()` 預設是 `shell=False` ，所以就算第一個參數寫成一整條字串，也不會自動先經過 shell；在 POSIX 系統上，它反而會把整條字串當成執行檔名稱，通常直接找不到。

真正有風險的是把模型給的內容拼成一整條指令，再設成 `shell=True` 。假設模型想搜的字是 `$(whoami)` ，shell 看到 `$(...)` 的反應是把括號裡的東西抓去執行，再把結果填回原處。於是 `rg` 收到的不是 `$(whoami)` 這幾個字，而是你的使用者名稱。分號更乾脆，`;` 在 shell 眼裡就是「這道指令結束了，換下一道」。

改用 list 並維持預設的 `shell=False` 就沒這回事。程式不會啟動 shell，每個參數的邊界也標得很清楚，那些符號原封不動送到 `rg` 手上。現在叫 KeSi 搜 `$(whoami)` ，它只會老實回一句「找不到含有 `$(whoami)` 的內容。」

不過會解讀那串字的不只 shell，rg 自己也會。所以參數的最後還帶了一個 `--` ，意思是「選項到這裡結束，後面全都是資料」。少了這一道關卡，模型想搜的字如果剛好長得像 `--hidden` 或 `--files` ，rg 會以為那是在對它下指令，而不是要找的字串。

再來把上一篇文章裡講到的柵欄接進來，搜尋起點先過 `safe_path()` ，它會把符號連結解開再確認位置，符號連結（Symbolic Link）是指向別處的捷徑，看起來待在工作目錄裡但內容可能在另一個目錄裡。過關之後再用 `is_ignored()` 擋掉黑名單。

不過整個目錄翻的時候，光看入口是不夠的。模型說要搜整個工作目錄沒問題，但 `rg` 接下來會一路往子目錄裡走然後就會遇到 `node_modules` 跟 `.git` 目錄了。所以同一份黑名單要再告訴 rg 一次，讓它自己邊走邊避開，程式碼裡那串 `RG_EXCLUDES` 就是在做這件事。 `--no-follow` 則是叫它別跟著捷徑走到工作目錄外面去。換句話說， `safe_path()` 跟 `is_ignored()` 看的是「從哪裡開始搜」，交給 rg 的那份名單管的是「走進去以後會經過哪些地方」。

剩下幾個選項處理的是 rg 的執行環境，像 `shutil.which()` 在 KeSi 啟動、載入這個模組時找出 rg 的完整路徑， `cwd=BASE_DIR` 把相對路徑釘在工作目錄， `--no-config` 不讀使用者自己的 rg 設定檔， `stdin=DEVNULL` 不留機會讓它在那邊等輸入。加這些設定的目的是讓不同電腦盡量得到一致的結果。它們沒有鎖定 rg 版本、作業系統或檔案系統，不可能保證完全一樣，不過至少拿掉了使用者設定與目前目錄這兩個變因。

輸出格式這一組則全是為模型服務的。 `--line-number` 、 `--with-filename` 跟 `--no-heading` 湊出「檔名:行號:內容」的三段式，行號是模型的座標，看到 `kesi.py:42:` 才知道下一步該讀哪裡。這是一般文字命中的格式；如果模型明確指定一個疑似二進位檔， `rg` 可能只回 `binary file matches` 之類的提示，不會帶行號，KeSi 這版也沒有用 `--text` 強迫它把二進位內容當文字處理。 `--color never` 關掉高亮，因為那些顏色是文字裡插進去的 `\x1b[31m` 控制碼，畫面上看不到，進日記就是亂碼 token，輸出是給模型看的，第 6 天的老原則。 `--max-columns 200` 配 `--max-columns-preview` 對付超長行，這裡的 200 是 200 個 byte，不是 200 個字元，所以中文通常會更早碰到上限。太長的那行不會整行消失，而是印到上限為止再接一句 `[... omitted end of long line]` 。 `errors="replace"` 則是遇到不合 UTF-8 的 byte 就換成 `�` ，也就是在亂碼網頁上常看到的那個菱形問號，那一行照樣交出來，不會為了一個壞 byte 整個中斷。

`timeout=10` 的這個 10 秒是為了教學隨手訂的，意思是「超過十秒就別等了」，如果專案大這個數字自己往上調就行了。時間到了，Python 會把 `rg` 砍掉再等它真的結束，模型則會收到「錯誤：搜尋超過 10 秒被中止，請縮小範圍後再試。」的訊息。實際花的時間有可能比 10 秒多一些， [Python 官方文件](https://docs.python.org/3/library/subprocess.html#subprocess.run) 裡有這麼一段：

> The initial process creation itself cannot be interrupted on many platform APIs so you are not guaranteed to see a timeout exception until at least after however long process creation takes.

意思是在許多平台的 API 上，啟動一支程式的過程本身沒辦法中途取消，所以逾時最快也要等它啟動完才抓得到。收到這句話之後是要縮小範圍再搜一次，還是改用別的工具，由模型自己決定。

最後把結果限制在 100 行，錯誤訊息最多留 2,000 個字元。搜尋結果會整份寫進日記，不然一次塞幾千行進去之後每問一句都要把那幾千行重送一次、再付一次錢，而且模型也容易在一大堆行號裡看漏重點。

`capture_output=True` 是等 rg 整個跑完、把完整輸出全部收進記憶體之後，才由 Python 動手截斷，但這時候該吃的記憶體早就吃掉了。真的遇到大型專案要連這一段都顧好，要改成邊讀邊處理，數到 100 行就把子行程收掉。

## regex 是雙面刃

工具的說明書裡寫了支援正規表達式，也就是 regex。它是一種描述「要找的文字長什麼樣子」的寫法，比起搜一個固定的詞，它可以表達「開頭是大寫、後面接三個數字」這種條件。

麻煩的是 regex 有「方言」問題，同一種寫法這個工具吃、那個工具不吃。舉個例子，假設模型只想找 `safe_path` 的定義但不想看到那些呼叫它的地方，可能會寫 `(?<=def )safe_path` ，意思是「只找前面跟著 `def ` 的那一個」。這種寫法 rg 預設不支援，跑出來是這樣：

```markdown
搜尋失敗：rg: regex parse error:
    (?:(?<=def )safe_path)
       ^^^^
error: look-around, including look-ahead and look-behind, is not supported

Consider enabling PCRE2 with the --pcre2 flag, which can handle backreferences
and look-around.
```

這段訊息幫了三個忙：說了不支援哪一種寫法，用 `^^^^` 指出是哪幾個字出問題，還告訴你加上 `--pcre2` 就能換一套功能更完整的 regex。這就是第六天文章裡提到的規矩，不要把有用的錯誤吃掉。如果只回一句「搜尋失敗」，模型連自己哪裡寫錯都不知道，更不會知道還有 `--pcre2` 這條路可以走。不過 KeSi 這版的 schema 沒有 `--pcre2` 開關，模型不能自己把它打開；它只能改寫 regex 或換方式找。真的要走 PCRE2，得先改工具實作。

順著這個例子，把三種結果一次分清楚。rg 跑完會留一個結束碼，KeSi 就照這個數字分成三種回應：

- `0` 是有找到，回那幾行「檔名:行號:內容」
- `1` 是沒找到，回「找不到含有 xxx 的內容。」這不算錯誤，所以 `is_error` 是 `False`
- 其他數字是執行出了問題，回「搜尋失敗：」加上 rg 自己的錯誤訊息， `is_error` 標成 `True`

## 為什麼不用 RAG？

講到「在一堆文件裡找相關內容」，你可能聽過另一個做法，就是把檔案切塊、算成 embedding 向量（把一段內容表示成一串數字，供系統比較語意相似度）存進索引，提問時先撈出相關片段再交給模型。這是 RAG（Retrieval Augmented Generation），也就是檢索增強生成。它厲害的地方是能找到字面上沒有共同關鍵字的段落，例如搜「登入」也能撈出寫著「驗證身分」的那一段，這是 grep 做不到的事。

這個系列不會走這條路，因為要多出來的不是只一個函式而已，像是內容要怎麼切塊、拿什麼去算 embedding、算出來的向量存哪裡、檔案改了索引怎麼跟著更新都得考慮，這些沒有一項屬於 agent 的骨架。如果 KeSi 要找的是目前這個小型、持續變動的專案，先用關鍵字搜尋已經夠用。Anthropic 的工程文章 [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) 講另一條路 just-in-time 的時候，舉的例子就是正版 Claude Code 本人，它手上先拿著檔名、路徑這類輕量線索，需要的時候才用工具把內容載進 context，它先載入 `CLAUDE.md` ，接著就用 glob、grep 去探索。

## 跑起來！

先別急著問模型。 `grep()` 是我們自己寫的函式，裡面有路徑檢查、參數組裝、結束碼判斷好幾層，先確認它自己沒問題，等一下模型出狀況的時候才分得清是誰的責任。最快的方法是在 `history = []` 前面暫時插三行，測完再刪掉：

```python
output, is_error = grep("safe_path")
assert not is_error
print(output)
```

`assert` 的意思是「這件事必須成立」，不成立就當場中斷。所以萬一 `is_error` 是 `True` ，程式會停在第二行， `assert` 後面那行 `print` 根本不會執行：

```markdown
kesi.py:36:def safe_path(path):
kesi.py:56:    target = safe_path(file_path)
kesi.py:73:    target = safe_path(path)
kesi.py:84:            resolved = safe_path(entry)
kesi.py:134:        target = safe_path(candidate)
kesi.py:155:    target = safe_path(path)
kesi.py:344:output, is_error = grep("safe_path")
```

「檔名:行號:內容」的三段式出來了，那個行號就是要給模型的座標。

最後一行看起來有點多餘，其實那就是我們剛剛插進去的第一行測試碼。它現在住在 `kesi.py` 的第 344 行，內容裡又剛好有 `safe_path` 這幾個字，而這次搜尋的範圍就是整個工作目錄，所以連它自己也被撈出來了。

工具層沒問題，換模型上場。先問一題直球：「 `safe_path` 這個函式在哪些地方被用到？」

```markdown
你 > safe_path 這個函式在哪些地方被用到？
  [執行工具] grep({'pattern': 'safe_path'})
KeSi > 根據搜尋結果，safe_path 函式在以下地方被用到：
       第 36 行：函式定義
       第 56 行：在某個函式中呼叫
       ...
```

一次命中，連 glob 都沒動用。這條路線不是保證，小專案裡它也可能憑日記裡已經讀過的內容直接回答。但我第二題問「這個專案裡限制檔案存取範圍的邏輯在哪？」，我故意問得模糊一點，看看它怎麼翻成程式碼裡可能出現的字：

```markdown
你 > 這個專案裡限制檔案存取範圍的邏輯在哪？
  [執行工具] list_files({})
  [執行工具] grep({'pattern': 'working.?dir|path|access|restrict|boundary|scope'})
  [執行工具] read_file({'file_path': 'kesi.py'})
  [執行工具] read_file({'file_path': 'README.md'})
KeSi > 這個專案的檔案存取限制邏輯主要在 kesi.py，有三個關鍵函式：...
```

這裡有四次工具執行，不過這份畫面沒有標出 API response 的邊界，所以光看四行日誌算不出內圈究竟轉了幾輪。最值得看的是那個 grep 的 pattern。它沒有一個詞一個詞慢慢試，是一口氣用 `|` 串了六個候選詞 `working.?dir|path|access|restrict|boundary|scope` 。「限制檔案存取範圍」這幾個中文字不會照字面出現在原始碼裡，模型自己猜這個概念寫成程式會長什麼樣。

搜尋之後它還讀了兩個檔案。光看這份日誌不能證明兩個 `read_file` 是看到 grep 結果後才決定的，不過「先拿線索、再讀內容」的整體方向仍符合官方文章所說的 progressive disclosure：

> Letting agents navigate and retrieve data autonomously also enables progressive disclosure—in other words, allows agents to incrementally discover relevant context through exploration.

線索也不只藏在內容裡，檔名跟目錄結構本身就是線索，同一篇文章舉的例子是 `tests/` 底下的 `test_utils.py` ，跟 `src/core_logic/` 底下的同名檔案，用途一看就不一樣。不過那是線索不是事實，看名字只能猜，還是要讀內容才算查證，剛剛那次它讀 `kesi.py` 跟 `README.md` 就是在做這件事。

路線每次不一定相同，模型、專案內容跟日記裡已經有什麼都會影響。

像這次它一口氣把六個詞串起來搜是很快，但運氣不好的時候可能反而會一次只試一個，搜 `restrict` 沒有就換 `limit` ，再換 `permission` 、 `guard` 、 `validate` ，一個一個試下去，每次都收到「找不到含有 xxx 的內容。」，然後日記就這樣一輪一輪變厚。目前沒有圈數上限的是 `run_agent()` 裡處理工具呼叫的內圈；最外層的 REPL 只是等下一次使用者輸入。這個內圈想試幾次就試幾次，幾天後會補上這道煞車。

## 正版是怎麼搜的

翻開我們之前攔到正版 Claude Code 工具清單，找內容搜尋類的工具讀一下說明書，你會看到人家的參數表比我們豐富一大截，輸出模式的切換、結果數量的上限、上下文行數這些選項都做成了參數，讓模型視情況自己調，不過還好，反正 KeSi 的目的是教學不是真的要復刻一個 Claude Code，知道怎麼運作就好，而且知道這些原理之後如果要追上正版功能也是追的上的。

行為層面的對照可以自己做一次，例如拿同一份專案跟同一道問題，分別交給 KeSi 跟你手邊的 coding agent，看它們各用了哪些工具、搜了幾次、什麼時候改讀檔、什麼時候回頭問你。

回頭量一下 KeSi 自己這邊，目前這四個工具的說明書每按一次 Enter 都要整包送給模型，量法跟前幾天一樣，同一組 model 跟 messages 呼叫 `count_tokens` ，只改 `tools` 那一欄：

```markdown
不帶 tools    25
三個工具    1598   (+1573)
四個工具    2157   (+2132)
```

grep 這個工具的說明書加上 schema，多掛這一個就多 559 個 token。昨天三個工具是 1,573，今天變成 2,132，而且這些內容每按一次 Enter、內圈每轉一圈都會重新送進 context。這個數字裡不只有你寫的 schema，也包含平台替 tool use 加上的那段系統提示。不過 `count_tokens` 回傳的是估計值，也可能包含 Anthropic 自動加入但不計費的 system token，實際帳單還是要看 Messages API 回傳的 `usage` 。自己量的時候記得跟前幾天用同一套方法重測，別拿不同日期的舊數字混著比。

## 工具箱盤點

第一段的工具做到這裡算是告個段落，盤點一下現在 KeSi 腰帶上掛了什麼：

- `read_file` ：讀，一次一個檔，黑名單與柵欄雙重防護
- `list_files` ：看，環顧一層目錄
- `glob` ：找檔名，按樣式撈檔案
- `grep` ：找內容，用 ripgrep 的預設過濾，再疊上 KeSi 自己的拒絕規則

四個工具正好對應偵探辦案的基本功，像是環顧現場、按特徵篩選、全文檢索、調閱檔案。昨天開場的三個問題，這裡有什麼、那類東西在哪以及內容寫在哪，現在都有工具可以用了。至於模型會不會選對順序、查完再驗證就要看實際運作才知道，工具齊全也不等於答案自動可靠。

有沒有發現這個工具箱很有 Unix 的味道？每個工具只做好一件事，威力來自「組合」。五十年前的人用 pipe 把小工具串起來，現在換成模型用迴圈串。這也回頭解釋了為什麼四份說明書都要寫清楚「用槍時機」，工具各管各的、功能不重疊，分流的指引就越重要。

目前這四個工具都還是唯讀模式，雖然不會改動到檔案內容但不表示沒有風險，萬一柵欄有洞還是可能把你的憑證讀出來然後送給模型那邊，失控的搜尋可能也會吃掉 CPU、記憶體跟 token。唯讀只代表「不改你的資料」，不代表「做什麼都沒事」。

## 小結

今天 KeSi 多了一個內容搜尋工具。真正新增的不只是呼叫 rg，還包括重接第 7 天的路徑與禁區邊界、固定 rg 的執行環境、區分「沒找到」與「執行失敗」，以及限制送回模型的內容。搜尋引擎借現成的，工具的規範還是我們負責。

然後眼前這個小型、持續變動的本機專案，先用即時文字搜尋最省事；資料量、查詢方式與重複使用需求改變時，混合索引也可能是更好的答案。

目前讀、看、找這些功能都齊了，也差不多準備讓 KeSi 自己動起來了。明天要進入這個系列的深水區，要開始讓模型來改我的程式碼了。而且你還會看到像是改程式碼這種對 AI Agent 很常見的功能竟然可以只用「字串替換」這麼樸素無華且枯燥的手法。

咱們下集見:)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401983)

[

Day 07 - 開眼！看見整個專案

](https://ithelp.ithome.com.tw/articles/10401983)[下一篇](https://ithelp.ithome.com.tw/articles/10402235)

[

Day 09 - 改 code 為什麼用字串替換

](https://ithelp.ithome.com.tw/articles/10402235)

系列文

[為你自己手刻 Claude Code](https://ithelp.ithome.com.tw/users/20065770/ironman/9031) 共 10 篇

目錄

1. 6
	[Day 06 - 第一個工具 read\_file](https://ithelp.ithome.com.tw/articles/10401693)
2. 7
	[Day 07 - 開眼！看見整個專案](https://ithelp.ithome.com.tw/articles/10401983)
3. 8
	[Day 08 - 在專案裡搜尋](https://ithelp.ithome.com.tw/articles/10402109)
4. 9
	[Day 09 - 改 code 為什麼用字串替換](https://ithelp.ithome.com.tw/articles/10402235)
5. 10
	[Day 10 - write\_file 的防呆機制](https://ithelp.ithome.com.tw/articles/10402250)
[完整目錄](https://ithelp.ithome.com.tw/users/20065770/ironman/9031)

[![.](https://itadstatic.ithome.com.tw/B9/1786008610_6a745422552c3.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9001)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/20/7659574e-4e9e-4ad3-bfcf-7843d2bb0936_small.jpg)

[透過Headless CMS讓前端與行銷團隊高效合作](https://itplus.ithome.com.tw/vod-page/151?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

37 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/06b37579-92a2-4be3-a8ea-9ca641c74caa_small.png)

[別人的 K8s 死袂了 - Kubernetes Failure Stories](https://itplus.ithome.com.tw/vod-page/562?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

22 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/dbfdba4a-8f61-4ea5-be4a-2c9e66e5471e_small.png)

[基於 BPMN 的微服務設計方法](https://itplus.ithome.com.tw/vod-page/886?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/17/d985abc8-a53e-4f64-91b6-a93d33a03af6_small.png)

[90 秒就裝進你的 CI——動手攔下一次真實的 npm 供應鏈攻擊（JFrog Curation 實作）](https://itplus.ithome.com.tw/vod-page/1460?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

85 分