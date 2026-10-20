---
title: "Day 07 - 開眼！看見整個專案 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401983"
author:
  - "[[iThome]]"
published:
created: 2026-08-10
description: "文章同步發表在我的個人 Blog 「你是我的眼，帶我閱讀浩瀚的書海」我很喜歡這首歌詞。 昨天的 KeSi 有一個可以讀檔的工具但眼睛是看不見的，我得先告訴它「去讀 kesi.py」它才有辦法動手，要是..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 7

0

[Software Development](https://ithelp.ithome.com.tw/2026ironman/software-development)

### 為你自己手刻 Claude Code系列 第 7 篇

## Day 07 - 開眼！看見整個專案

- 分享至

> 文章同步發表在 [我的個人 Blog](https://kaochenlong.com/)

「你是我的眼，帶我閱讀浩瀚的書海」我很喜歡這首歌詞。

昨天的 KeSi 有一個可以讀檔的工具但眼睛是看不見的，我得先告訴它「去讀 `kesi.py` 」它才有辦法動手，要是你只說「幫我看看這個專案」，它連專案裡有什麼檔案都不知道：

```markdown
你 > 幫我看看這個專案
KeSi > 我很樂意幫你看看這個專案！不過我需要知道具體的檔案路徑。
```

這樣有點太弱，我希望這個 agent 可以自己看著辦，看看現在手邊有哪些檔案或目錄，而不是站在原地等我一個一個報檔名給它。要一個 agent 摸索陌生專案，它要有能力回答三個問題：

1. 這裡有什麼？
2. 我要的那類東西在哪？
3. 哪個檔案裡寫了我要找的內容？

今天這篇文章我先解決前兩個問題，我會做一個 `list_files` 的工具用來環顧四周，解決第一個問題，然後再做一個 `glob` 工具可以指定找某些類型的檔案，這是第二個問題，第三個問題讓我富樫一下，下一篇文章再來處理。

GitHub Repo： [https://github.com/kaochenlong/KeSi](https://github.com/kaochenlong/KeSi)

## 第一隻眼睛 list\_files

先處理昨天文章裡的一筆技術債，在檢查路徑的時候我是直接寫在 `read_file` 工具裡面的，但今天要加的這兩個都需要檢查路徑的工具，所以這裡我把它抽成一個共用函式 `safe_path()` ：

```python
BASE_DIR = Path.cwd().resolve()

def safe_path(path):
    try:
        target = (BASE_DIR / path).resolve(strict=True)
    except (OSError, RuntimeError, TypeError, ValueError):
        return None
    return target if target.is_relative_to(BASE_DIR) else None
```

這裡沿用昨天的 `resolve(strict=True)` ，先把 `..` 與符號連結（symbolic link，簡稱 symlink）解析成真正指向的位置，再用 `is_relative_to()` 確認結果仍在啟動 KeSi 時的工作目錄裡。檔案不存在、路徑格式不合法、符號連結繞到專案外面，或解析過程出錯都先回 `None` ，由呼叫端決定怎麼回報。

這裡不能只用字串形式的絕對路徑判斷。舉個例子，假設 KeSi 是在 `/Users/kaochenlong/toy/KeSi` 這個目錄啟動，而目錄裡有一個這樣的符號連結：

```markdown
/Users/kaochenlong/toy/KeSi/settings -> /Users/kaochenlong/.ssh/id_rsa
```

箭頭左邊是連結本身，住在工作目錄裡面；箭頭右邊才是這條連結真正指向的東西，在工作目錄外面。

模型給了 `settings` 這個看起來很正常也合規定的路徑，但符號連結是檔案系統層的轉址，真的 `open()` 下去的時候作業系統會照著箭頭走出去，在這個例子裡讀回來的就是我的個人 SSH 私鑰，這東西不應該隨便被誰拿到，就算是模型也不行。而 `resolve()` 做的就是先去問作業系統「這條路徑一路轉下去，最後到底落在哪」，拿到真正的目的地再比對。路徑字串長什麼樣，跟它最後通到哪裡，是兩回事。

好，現在來做第一個眼睛吧，先做可以環顧四周的 `list_files` ：

```python
def list_files(path="."):
    target = safe_path(path)
    if target is None:
        return f"錯誤：找不到或不允許存取目錄 {path}", True
    if is_ignored(target):
        return "錯誤：這個目錄不開放讀取。", True
    if not target.is_dir():
        return f"錯誤：{path} 不是一個目錄", True

    try:
        entries = []
        for entry in target.iterdir():
            resolved = safe_path(entry)
            if resolved is None or is_ignored(resolved):
                continue
            entries.append((entry.name, resolved.is_dir()))
    except OSError as exc:
        return f"錯誤：無法列出目錄 {path}：{exc}", True

    entries.sort(key=lambda item: item[0])
    lines = [name + "/" if is_dir else name for name, is_dir in entries]
    if not lines:
        return "（這個目錄沒有可列出的項目）", False
    if len(lines) > MAX_LIST_FILES:
        head = "\n".join(lines[:MAX_LIST_FILES])
        return (
            f"{head}\n（共有 {len(lines)} 筆，只顯示前 {MAX_LIST_FILES} 筆）",
            False,
        )
    return "\n".join(lines), False
```

主體是 `Path.iterdir()` ，其他同樣都是防護跟整理。列出的目錄本身要先過 `safe_path()` 、 `is_ignored()` 與 `is_dir()` ，而且列出來的每一個項目再解析一次，避免指向專案外面或禁區的符號連結混進來。

那個 `IGNORE` 與 `is_ignored()` 晚點再細講，回傳值延續昨天的 `(文字, is_error)` 格式，成功就帶 `False` ，被拒絕或執行失敗就帶 `True` 。這裡我想先跟大家說明輸出格式的幾個決定：

第一，排序。檔案系統列出的順序不保證固定，同一個目錄兩次列出來可能不同。對人來說無所謂但對 KeSi 就有所謂了，如果模型每次看到的世界長得不一樣，行為就更難重現。同樣的內容但不同順序在 API 眼中是不同的文字，這在之後做快取的時候會吃虧的。

第二，目錄名結尾加斜線。模型拿到一份名單，需要知道哪些是可以再往下走的「目錄」或是哪些是可以直接讀的「檔案」，只要加一個斜線就把這個資訊帶到了，比另外寫一欄「type: directory」省字。

第三，沒有東西也要講一句話。目錄是空的或是裡面的東西被規則濾掉，這時候回傳空字串模型會搞不清楚這個工具到底是沒有執行還是執行失敗。應該是要回一句「這個目錄沒有可列出的項目」再配上 `is_error: false` ，模型就知道工具跑完了只是沒有東西而已。工具執行完沒有結果，跟根本沒有執行是兩回事，這件事之後每個工具都要留意。

第四，給足夠的資訊就好。以這個工具來說可以只給名字，不用給檔案大小、日期這些 metadata。這不是做不到而是這些欄位的資訊如果再乘以檔案數量都會吃 token，而大部份的探索場景只需要知道「有什麼」就夠了。如果之後有需要按檔案大小排序，可以再追加欄位或加參數，目前先夠用就好。同樣在處理目錄的時候，我這裡會一次只列一層而不是列出整棵樹。做成像 `tree` 指令那樣可以一口氣把整個目錄樹倒出來一眼看到全貌好像不錯，但這些資訊也一樣都算 token 的，大部份的問題根本用不到這麼多資訊。這裡我選擇一層一層走，讓模型看到哪裡有興趣再自己往下鑽，缺點就是要多轉幾圈迴圈。但真的遇到需要全貌的場合，模型自己多走幾步也應該都到得了。

不只目錄的「深度」，如果在單層目錄裡有很多檔案的話，一口氣全部倒出來也可能大得離譜，所以這裡的清單跟待會的 `glob` 共用 200 筆上限。注意我在這次的 schema 刻意讓 `path` 參數是選填的：

```python
{
    "name": "list_files",
    "description": "列出指定目錄底下的檔案與子目錄，目錄名稱結尾會帶斜線。"
    "想知道專案裡有什麼、或不確定檔案位置時，先用這個環顧四周。",
    "strict": True,
    "input_schema": {
        "type": "object",
        "properties": {
            "path": {
                "type": "string",
                "description": "相對於工作目錄的路徑，省略時代表工作目錄本身",
            }
        },
        "required": [],
        "additionalProperties": False,
    },
},
```

`required` 是空的代表模型可以交白卷，這時 Python 端 `list_files(path=".")` 的預設值接手，列的就是目前這個工作目錄本身。模型想列根目錄就什麼都不要填、想往子目錄走就填個路徑，等於一個工具可以有兩種用法，兩個願望一次滿足。 `strict: True` 與 `additionalProperties: False` 表示 API 端不接受說明書以外的欄位，不過本機函式還是要處理實際的檔案系統錯誤。

## 第二隻眼睛 glob

`list_files` 工具解決了「這裡有什麼」，另一種常見的需求可能是「幫我找出所有的 Excel 檔」。這種按照「特徵」找東西如果一層一層用 `list_files` 走下去太慢了，所以我直接做一個可以「過濾」的眼睛 `glob_files` ：

```python
def glob_files(pattern):
    if not isinstance(pattern, str) or not pattern.strip():
        return "錯誤：pattern 不可為空。", True

    pattern_path = Path(pattern)
    windows_pattern = PureWindowsPath(pattern)
    if pattern_path == Path("."):
        return "錯誤：pattern 必須指定要找的檔名樣式。", True
    if (
        pattern.startswith("~")
        or pattern_path.is_absolute()
        or windows_pattern.drive
        or windows_pattern.root
        or ".." in pattern_path.parts
        or ".." in windows_pattern.parts
    ):
        return "錯誤：pattern 只能用工作目錄內的相對樣式。", True

    try:
        candidates = sorted(
            BASE_DIR.glob(pattern, recurse_symlinks=False),
            key=lambda candidate: candidate.as_posix(),
        )
    except (OSError, ValueError, NotImplementedError) as exc:
        return f"錯誤：無法解析 glob 樣式：{exc}", True

    matches = []
    for candidate in candidates:
        if candidate == BASE_DIR:
            continue
        target = safe_path(candidate)
        if target is None or is_ignored(target):
            continue
        relative = candidate.relative_to(BASE_DIR).as_posix()
        matches.append(relative + "/" if target.is_dir() else relative)

    if not matches:
        return f"找不到可存取且符合 {pattern} 的檔案。", False
    if len(matches) > MAX_LIST_FILES:
        head = "\n".join(matches[:MAX_LIST_FILES])
        return (
            f"{head}\n（共有 {len(matches)} 筆，只顯示前 {MAX_LIST_FILES} 筆）",
            False,
        )
    return "\n".join(matches), False
```

`glob` 是個歷史悠久的檔名比對語法， `*` 表示匹配任意名稱， `*.py` 意思就是「這一層所有的 Python 檔」， `test_*.py` 是「這一層所有 test\_ 開頭的 Python 檔」，而 `**` 可以匹配任意層目錄，所以 `**/*.py` 就是「不管幾層深，把所有 Python 檔都找出來」。實作直接用 Python 的 `pathlib` 的 `Path.glob()` ，該有的語法它都有了。

講到 `glob` 有一個很不明顯但容易踩到的坑，在 Python 標準庫裡有兩套 glob 但效果不太一樣。 `glob` 模組把小數點開頭的檔案當隱藏檔，預設不匹配，而 `pathlib` 對小數點開頭的檔案在 `pathlib` 裡卻照樣找的到。我這裡用的是 `pathlib` ，也就是說 `.git` 、`.env` 這些東西它是會照實吐出來的，因此過濾的責任會完全落在我們自己身上，這也是晚點會講到那份 `IGNORE` 黑名單存在的理由之一。

前半段先擋不合法或可能跑出工作目錄的 pattern。空字串跟 `.` 表示沒有要過濾什麼，POSIX 絕對路徑、Windows 的磁碟代號或根路徑、 `~` 開頭與路徑中完整的一節 `..` 都會拒絕，不過這幾類擋掉的理由不太一樣。

`..` 這個算是比較危險的，兩個小數點是「上層目錄」的意思， `Path.glob()` 對 `..` 照走不誤，給個 `../../**/*` 就會爬上去掃外面的目錄了。雖然後面寫的 `safe_path()` 會把結果逐筆濾掉，但那已經是掃完之後的事，該翻的硬碟都翻過了。pattern 這一關是唯一能在開掃之前踩煞車的地方，這跟等一下要講的 `**` 效能警告是同一個問題。

絕對路徑跟 `~` 開頭反而不危險，因為 `pathlib` 根本不吃這兩種。絕對路徑會直接丟例外，而 `~/.ssh/*` 這樣的路徑則是因為 `pathlib` 不會幫我們展開家目錄（那是 `expanduser()` 的事），變成在工作目錄裡找一個叫 `~` 的目錄，最後只會得到一份空清單。

這樣為什麼還要擋？為了給模型一句聽得懂的話。丟給後面的例外處理，模型收到的是一句夾著英文的「無法解析 glob 樣式」，而回一份空清單，模型又會以為家目錄真的沒東西，兩種都不如直接說清楚只能用工作目錄裡的相對樣式。

至於 Windows 那兩條，是因為 macOS 跟 Linux 上的 Python 根本看不懂 Windows 的路徑寫法：

```markdown
>>> Path("..\\..\\etc\\*").parts
('..\\..\\etc\\*',)
```

反斜線在這裡只是個普通的檔名字元，所以整條路徑被當成一個檔名，如果 `..` 藏在裡面根本抓不到。同一份 `kesi.py` 換到 Windows 上跑就不一樣了， `Path` 會把反斜線當成分隔符，`..` 就不再是檔名的一部分，而是真的要往上一層走。多一組 `PureWindowsPath` 就是把同一個 pattern 再用 Windows 的規則讀一次，兩套規則都要過得了關，我希望程式不管在哪個平台跑行為都一樣。

判斷的是路徑分段而不是看到字串裡有兩個小數點就擋掉，所以 `version..txt` 仍是合法檔名。即使 pattern 通過， `Path.glob()` 仍可能遇到不支援或格式錯誤的樣式，例外要轉成 `is_error: true` ，不能把例外拋給 agent 讓整個 agent loop 炸掉。

前半段擋的是 pattern 本身，不過 pattern 合法不代表撈回來的東西就能給。像 `*` 這個樣式也很正常，沒有絕對路徑、沒有 `..`、也沒有 `~` ，但 `Path.glob("*")` 會把 `.env` 、`.git` 照樣撈出來，前面那條指向 `~/.ssh/id_rsa` 的 `settings` 也會出現在名單上。

所以 `Path.glob()` 跑完之後還有一輪，每一筆結果都要再走一次 `safe_path()` 跟 `is_ignored()` ，解析後跑到專案外面的或是名字在黑名單裡的在這裡全部拿掉。recurse\_symlinks=False `也明確要求` \*\* `不沿符號連結往下鑽，不過` pathlib\` 在其他 glob 元件還是可能會跟著符號連結走，所以最後的逐筆解析不能省，這些防護動作都是避免結果離開工作目錄。

結尾是這個系列第一次自己實作「截斷」，超過 200 筆就砍，不然直接丟一個 `**/*` 進到大專案可能可以炸出幾萬筆結果，不截斷的話那份清單會直接寫進日記然後跟著每一輪重送。

注意我這裡截斷訊息的寫法，我不是默默砍掉，是明講「共有幾筆、只顯示前 200 筆」，這樣模型知道自己看到的是我在省節成本，樣式下得太寬就可以收斂，例如把 `**/*` 改成 `**/*.py` 再找一次。我這裡是告知截斷而不是隱瞞，這跟昨天錯誤訊息要有指導性是同一條原則。

搜尋正常完成但沒有結果時， `is_error` 是 `False` ，「沒找到」跟「工具壞了」仍是兩回事。

順帶一提， `pathlib` 的 [官方文件](https://docs.python.org/3.14/library/pathlib.html#pattern-language) 對兩顆星星的 `**` 有個效能警告：

> Globbing with the `**` wildcard visits every directory in the tree. Large directory trees may take a long time to search.

也就是說用了 `**` 就會把整棵目錄樹裡的每個目錄都走一遍，如果遇到大樹就會慢。現在的 `is_ignored()` 是結果出來後才過濾，所以可以省掉送給模型的 token，但這卻不會阻止 `Path.glob()` 先走進 `node_modules` ，也不會省下產生完整候選清單的記憶體。要連掃描成本一起省，得改用能在走訪途中剪枝的實作，在下篇文章換上 ripgrep 才會補上這一塊。

## 有些東西不能讓它看

兩隻眼睛都有了，現在來看看黑名單，這是今天的重頭戲：

```python
IGNORE = {
    ".git",
    "node_modules",
    "__pycache__",
    ".venv",
    "venv",
    ".env",
    ".DS_Store",
    ".pytest_cache",
}
MAX_LIST_FILES = 200

def is_ignored(target):
    try:
        parts = target.relative_to(BASE_DIR).parts
    except ValueError:
        return True
    return any(
        part in IGNORE or part.startswith(".env.")
        for part in parts
    )
```

這裡檢查的是 `safe_path()` 解析後的真正路徑，不是模型交來的原始字串。這樣一條叫 `settings` 、實際指向 `.env` 的符號連結也過不了；`.env.local` 、`.env.production` 這類常見變形則由 `.env.` 前綴一起擋。這份名單只是教學用的範例，我沒辦法列舉每個專案的機密命名方式。但為什麼要有這份名單？

`node_modules` 、`.venv` 、 `venv` 是依賴套件的倉庫，隨便一個前端專案的 `node_modules` 就是幾萬個檔案，全列出來那份清單本身就能撐爆一次回應。

`.git` 是版本庫的內部資料，一堆物件跟索引檔，探索專案時通常不需要直接讀。想查版本歷史該用 git 指令問（那是之後 shell 工具的事），不是把內部儲存格式塞進日記。

`__pycache__` 、`.pytest_cache` 、`.DS_Store` 是各種工具留下的快取跟雜物，純噪音。

最後是 `.env` ，這跟前面的都不一樣，它不是雜訊問題而是安全問題，我的 Anthropic 的 API 金鑰就放在這個檔案裡。如果 agent 看得到它也讀得到它，會發生什麼事？

金鑰的內容會被裝進 `tool_result` 、寫進日記，然後隨著之後的每一輪請求，一遍又一遍送到 Anthropic 的伺服器。第二天的文章裡有提醒過大家，攔下來的檔案裡有你的對話內容，要當心外流。進到日記的東西，就是待會會送去給模型的東西。一個會讀檔的 agent 加一個沒設防的 `.env` ，等於你把保險箱密碼抄在便條紙上貼在門口。所以 `.env` 直接進黑名單，KeSi 的探索與讀取工具從今天起不回傳它的內容。

但 `.env` 不是之前就加進 `.gitignore` 了嗎？對，但那是另一件事。`.gitignore` 幫 Git 略過刻意不追蹤的 `.env` ，降低它不小心被加進版控的機會，它管的是 git 的世界。今天這份 `IGNORE` 防的是金鑰跟著 context 流出去，管的是模型的世界。

跟著這個系列做的人還有一個自家特產要留意： `captured/` 資料夾。第 2 天側錄存下來的那些 JSON，裡面躺著你跟模型的完整對話，還有正版的整份 system prompt，要不要讓 KeSi 看見它們，你自己決定。我是覺得讓它讀到同類的內心世界是滿好玩的，但你要是拿 KeSi 做過什麼不想再進日記的對話，把 `captured` 也放進黑名單就是了。

這份寫死的名單有點陽春，真實世界的專案常用 [`.gitignore`](https://git-scm.com/docs/gitignore) 告訴 Git 哪些「刻意不追蹤」的檔案要忽略，而那個格式比表面上複雜，有否定規則（`!` 開頭代表例外放行）、有目錄限定（結尾斜線）、還有自己的 `**` 方言，正經解析起來是一整個小工程，Python 也有現成的套件可以做這件事，不過 KeSi 先不急著做，先用寫死的名單擋住就好。

## 看不見但手還是摸得到

寫到這裡，警覺一點的人可能發現另一個洞， `IGNORE` 只裝在 `list_files` 跟 `glob` 上， `read_file` 沒有。也就是說，KeSi 的眼睛看不見 `.env` ，但如果模型基於任何理由直接點名讀它， `read_file` 一樣會把金鑰整包捧出來。看不見，不等於摸不到。防護只做在「發現」上，忘了做在「存取」上。要碰一個東西不需要先看見它，檔名用猜的就行，何況 `.env` 這種名字根本不用猜。

所以順手把昨天寫的 `read_file` 工具也補上檢查，而且不能只檢查模型交來的字串，要檢查 `safe_path()` 解析後的目標：

```python
def read_file(file_path):
    target = safe_path(file_path)
    if target is None:
        return f"錯誤：找不到或不允許存取檔案 {file_path}", True
    if is_ignored(target):
        return "錯誤：這個檔案不開放讀取。", True
    if not target.is_file():
        return f"錯誤：不是可以讀取的文字檔 {file_path}", True

    try:
        return target.read_text(encoding="utf-8"), False
    except UnicodeDecodeError:
        return f"錯誤：檔案不是 UTF-8 文字檔 {file_path}", True
    except OSError as exc:
        return f"錯誤：無法讀取檔案 {file_path}：{exc}", True
```

連 `node_modules` 裡的檔案被點名讀也一樣擋，目錄與非 UTF-8 檔案則回清楚的錯誤，不讓例外撞斷迴圈。

符號連結那條也要補一下，在工作目錄裡放一條指向外部檔案的連結， `read_file` 、 `list_files` 、 `glob` 三個入口分別餵一次，都不該回傳外面的內容或清單。昨天越獄測的是會不會「跑出工作目錄」，今天測的是「讀目錄裡的禁區」，兩道柵欄現在都有了，爾後每次加一條新規矩記得要把所有入口再巡一遍。（其實這是自動化測試出場的地方，但這又是另一個主題了）

## 雜訊不只是浪費錢

直覺上，餵給模型的資料越多越好，反正目前使用的 Haiku 4.5 有 20 萬 token context window，其它模型甚至可到一百萬，乾脆把整個專案塞進去連工具都省了？模型與上限會變，實際使用前仍要查當時的 [官方 context window 文件](https://platform.claude.com/docs/en/build-with-claude/context-windows) 。這個「全塞派」的想法不是不行，小專案上它真的可行而且也真的有人這樣用。

不過這個帳要算一下，來上個數學公式。固定大小為 R 的整包專案重送 T 輪，輸入量是 T × R，看起來會跟送的次數呈線性正比。不過日記每輪增加大約 h 個 token，而每次請求又把之前累積的日記全部重送，第 1 輪送 h、第 2 輪送 2h，一路加到第 T 輪，總量約是 h × T × (T + 1) / 2。

無論是哪一種，對多數問題來說專案裡都有大量跟問題無關的內容，等於是付了輸入成本卻換來讓模型在垃圾山裡找鑰匙。agent 走的是另一條路，按需探索，用幾圈便宜的迴圈換取「盡量只把相關內容放進日記」。

而且就算錢錢不是問題（對我來說是！），塞好塞滿的效果也不保證更好。剛才那份 context window 文件裡就有提到：

> A larger context window allows the model to handle more complex and lengthy prompts, but more context isn't automatically better. As token count grows, accuracy and recall degrade, a phenomenon known as context rot. This makes curating what's in context just as important as how much space is available.

翻成白話就是更多 context 不會自動換來更好的結果，隨著 token 數成長，準確度跟回想能力可能退化，這個現象叫做 context rot，所以篩選 context 裡放什麼，跟有多少空間一樣重要。

但模型這麼聰明，它自己會避開垃圾吧？有時候還真的滿聰明的，看到 `node_modules` 多半不會傻傻鑽進去。但你想清楚順序，清單是先進了日記、先付了錢，模型才看到最後才判斷不要理它的。靠模型自律省下的只有它後續的動作，但省不下已經送出去的 token，所以過濾要做在工具層。

也就是說，把 `node_modules` 的檔案清單塞給模型這會造成雙重傷害。第一重是錢錢，那些 token 進了日記就每輪重送，第二重是「注意力」，要模型在一萬行雜訊裡找三行重點，就跟要你在被灌爆的群組裡找一則重要訊息一樣，可能找得到但更容易恍神、更容易漏。

這個「餵什麼、不餵什麼」的取捨，就是 context 工程的核心，Anthropic 自己有一篇 [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) 專講這個主題。至於 context rot 為什麼會發生以及 agent 用久了為什麼會變笨，之後會有一篇文章來跟它算帳，今天先這樣就好。

## 工具分派表

現在有三個工具了，昨天 `run_agent` 裡那個 `if block.name == "read_file"` 的寫法，再用 `elif` 疊下去會越來越醜。稍微改一下：

```python
TOOL_FUNCS = {
    "read_file": read_file,
    "list_files": list_files,
    "glob": glob_files,
}
```

迴圈裡的分派改成查表：

```python
func = TOOL_FUNCS.get(block.name)
if func is None:
    output = f"錯誤：沒有 {block.name} 這個工具。"
    is_error = True
else:
    try:
        output, is_error = func(**block.input)
    except TypeError as exc:
        output = f"錯誤：{block.name} 的參數不正確：{exc}"
        is_error = True
    except Exception as exc:
        output = f"錯誤：{block.name} 執行失敗：{type(exc).__name__}"
        is_error = True
```

Python 的函式是一等公民，可以直接當值放進 dict，查到就呼叫，查不到就回一句錯誤訊息。查到的話一樣照昨天的約定，把回傳值拆成 `output` 跟 `is_error` 兩個值。

錯誤處理有兩層， `TypeError` 那層接的是說明書跟函式對不起來的情況，比如說明書上的參數寫 `path` ，函式那邊卻叫 `dir_path` ；再外面的 `except Exception` 收其他漏網的例外，不讓某一個工具壞掉就把整個迴圈拖垮。

以後要加工具，只要在 `TOOLS` 加一份說明書然後在 `TOOL_FUNCS` 加一個對照就好， `run_agent` 本身完全不用動。這張表還順手解決一個命名的小尷尬：工具對模型叫 `glob` ，跟正版同名好記，Python 函式那邊叫 `glob_files` ，避免跟 `pathlib` 的方法撞名，對外跟對內的名字本來就不必綁死。

第五天的文章看過官方文件建議說相關的操作可以合併成一個工具，那 `list_files` 跟 `glob` 為什麼不合成一個？因為那條原則防的是把同一件事切太碎，這兩個工具一個吃路徑、一個吃樣式，回答的是不同問題，硬揉成一個說明書反而更難寫。

工具變多表示基本費也在漲。之前曾經量過掛一個工具的固定開銷，現在掛了三個，說明書全文加上工具使用相關的系統內容，每一輪都會算進輸入。量法跟第 5 天一樣，同一組 model 與 messages 連續呼叫 `count_tokens` 三次，只改 `tools` 那一欄：

```markdown
不帶 tools        24
只帶 read_file   863   (+839)
帶完整三個工具  1597   (+1573)
```

你自己跑的第一欄不會剛好是 24，那個數字跟你送什麼訊息有關，我這裡送的是一句短問題。要看的是括號裡的增量，那部分才是 `tools` 的重量，換一句話問也不會變。

掛第一個工具就跳了 839，那裡面有第 5 天講過的隱形 system prompt（Haiku 4.5 是 496 個 token），剩下的才是 `read_file` 說明書自己的重量。從一個工具加到三個又多了 734。

換句話說，今天之後每按一次 Enter，還沒開始講話就先付一千五百多個 token，內圈每轉一圈就要再付一次。工具的說明書寫得越詳細模型越會用但帳單也越厚，這個取捨之後幫 KeSi 裝上電表就會看得更清楚。

今天的 `kesi.py` 含空白行 258 行，整份貼上來感覺太灌水了，完整檔案可在 GitHub Repo 取得。前面幾節其實已經把該講的段落都拆開貼過了，剩下的就是把它們組起來。之後的文章也照這個作法，只貼動到的部分。

GitHub Repo： [https://github.com/kaochenlong/KeSi](https://github.com/kaochenlong/KeSi)

## 跑起來，讓它自己逛

先來招牌題。這次直接在 KeSi 自己的目錄裡跑，問「這個專案裡有哪些 Python 檔？」：

```markdown
你 > 這個專案裡有哪些 Python 檔？
  [執行工具] glob({'pattern': '**/*.py'})
KeSi > 這個專案裡有以下 **5 個 Python 檔**：

       1. **kesi.py**
       2. **prefill.py**
       3. **probe.py**
       4. **proxy.py**
       5. **tool_tokens.py**

       需要我查看這些檔案的內容或了解它們的用途嗎？
```

它選了 `glob` ，而且 pattern 直接下 `**/*.py` 連子目錄一起撈，不是只找當層的 `*.py` ，說明書裡給的那兩個例子看來有派上用場。

這條路線不是程式保證的，模型也可能先 `list_files` 環顧一圈再決定，或是根據眼前已有的資訊直接回答。你要驗的是它送出的參數、工具回傳跟檔案系統三邊能不能對上，不是它有沒有照你想的順序走。

而且這題也可以看出來黑名單 `IGNORE` 有在工作，因為 `.git` 裡沒有 Python 檔、 `__pycache__` 裡放的是 `.pyc` ，樣式本身就撈不到那些東西。換個問法才看得出來：

```markdown
你 > 列出目前這個目錄有什麼
  [執行工具] list_files({})
KeSi > 目前工作目錄中有以下內容：

       **檔案：**
       - \`.gitignore\` - Git 忽略檔案
       - \`.python-version\` - Python 版本設定檔
       - \`LICENSE\` - 許可證檔案
       - \`README.md\` - 說明文件
       - \`kesi.py\` - Python 程式檔案
       ...

       **目錄：**
       - \`.ruff_cache/\` - Ruff 快取目錄
       - \`captured/\` - 捕獲檔案目錄
       ...
```

目錄實際上有 16 個項目，KeSi 只看到 12 個。少掉的四個是 `.env` 、`.env.example` 、`.git/` 跟 `__pycache__/` ，它們在工具裡就被 `IGNORE` 攔下來了，模型從頭到尾不知道有這些東西存在，日記裡當然也不會有。

倒是 `.ruff_cache/` 留在清單上了，那是 ruff 這個工具的快取目錄，論性質跟 `__pycache__` 是同一類雜訊，只是我沒把它寫進名單裡。再來一題組合技：「幫我看看這個資料夾裡有哪些東西，每個檔案大概是做什麼用的？」這題沒有一個檔名是你給的，它得先環顧、自己挑幾個檔案讀、再彙整成一段話：

```markdown
你 > 幫我看看這個資料夾裡有哪些東西，每個檔案大概是做什麼用的？
  [執行工具] list_files({})
  [執行工具] read_file({'file_path': 'README.md'})
  [執行工具] read_file({'file_path': 'kesi.py'})
  [執行工具] read_file({'file_path': 'prefill.py'})
  [執行工具] read_file({'file_path': 'probe.py'})
  [執行工具] read_file({'file_path': 'proxy.py'})
  [執行工具] read_file({'file_path': 'tool_tokens.py'})
  [執行工具] list_files({'path': 'examples'})
  [執行工具] list_files({'path': 'captured'})
KeSi > 完美！讓我為你總結一下這個資料夾的內容：
       ...
```

九行 `[執行工具]` ，先環顧一圈，再把五個 Python 檔逐一讀過，最後往兩個子目錄各看一眼才開口。這些全發生在你按下 Enter 之後、看到回答之前，內圈就這樣默默轉了九圈。

記得在上一篇文章裡的連環開單，還得我出一道「a.txt 跟 b.txt 有什麼關聯」把檔名餵給它，現在 KeSi 手上的工具已經足夠自己找路。柵欄也照昨天的慣例驗收一輪，這次測相對路徑往上走，問「列出上一層目錄有什麼」：

```markdown
你 > 列出上一層目錄有什麼
  [執行工具] list_files({'path': '..'})
  [執行工具] list_files({'path': '</antml antml:parameter>
'})
  [執行工具] list_files({})
KeSi > 這是目前工作目錄的內容。根據系統限制，無法直接存取上一層目錄 (\`..\`)。

       目前工作目錄包含以下檔案和資料夾：
       ...
```

那三行 `[執行工具]` 才是重點。第一行的 `..` 被柵欄擋下來，第二行是模型收到錯誤之後亂試的一發，那串參數是它自己吐出來的內部標記，一樣被擋，第三行才回到「列當前目錄」這條走得通的路。

它沒有放棄也沒有卡住，連撞兩次之後照樣給了你一份清單，還順口解釋為什麼只能給這些。昨天說錯誤是回給模型的，這就是那句話替你工作的樣子。中間那行不是每次都會出現，我連跑十幾次大概三分之二會遇到，剩下的直接兩行就搞定，你自己跑看到兩行或三行都算正常。最後再測一下禁區，直接問「`.env` 這個檔案裡寫了什麼？」：

```markdown
你 > .env 這個檔案裡寫了什麼？
  [執行工具] read_file({'file_path': '.env'})
KeSi > 很抱歉，系統不開放 .env 檔案的讀取權限。這通常是基於安全考慮，
       因為 .env 檔案通常包含敏感資訊，如：

       - API 金鑰
       - 資料庫密碼
       ...
```

模型確實點名要讀， `read_file` 的第二道檢查擋下來了，回答裡沒有半個字的檔案內容。不過注意它「知道」有 `.env` 這回事，被擋之後還接著解釋 `.env` 通常裝些什麼。這不是柵欄漏了，`.env` 本來就是人盡皆知的檔名，模型不需要看到它也猜得到。我們能保證的是工具不把檔名列出來、也不回傳內容而不是抹掉模型的常識。

## 正版怎麼看世界

正版 Claude Code 的 83 個工具，裡面就有列檔案跟找檔案這類工具，你可以自己在 JSON 裡搜搜看，讀一下它們的說明書是怎麼寫的，跟我們今天寫的比一比，方向是一樣的，但正版的但書跟細節多很多。

特別可以講的是 ignore 這件事，許多 coding agent 在探索專案時會參考 `.gitignore` 來減少不必主動掃描的內容，等於繼承開發者「這些檔案刻意不交給 Git 追蹤」的設定，這比完全不看專案規則更務實。

另外預告一下，明天要做的搜尋工具會直接站在巨人肩膀上。依照 [ripgrep 的官方指南](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md) ，它做遞迴搜尋時，預設會參考 ignore 規則，並跳過隱藏檔與二進位檔，確實替探索場景處理掉不少雜訊。不過這還不是權限系統，如果把被忽略或隱藏的檔案明確當成位置參數交給 `rg` 它仍可能照樣搜尋，這個我們就下集再來處理。

## 小結

KeSi 加了兩隻眼睛，現在已經可以環顧四周或是搜尋檔案，然後再根據使用者的提問決定要不要讀。探索清單先濾掉常見雜訊，讀取入口再拒絕已知禁區，而且每一條候選路徑都要解析後重查，避免符號連結繞過規則。

開場的三個問題現在已經能回答前兩個，這裡有什麼用 `list_files` 工具，那類東西在哪用 `glob` 工具。現在它看得見檔案的「名字」，但看不見「內容」在哪，問它「哪個檔案裡有用到 requests 這個套件」，它只能一個一個檔案讀過去找，這又慢又燒錢。這樣不行，下一集我要幫 KeSi 裝一個快一點的搜尋工具，而且還能一次解決第三個問題，咱們下集見:)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401693)

[

Day 06 - 第一個工具 read\_file

](https://ithelp.ithome.com.tw/articles/10401693)[下一篇](https://ithelp.ithome.com.tw/articles/10402109)

[

Day 08 - 在專案裡搜尋

](https://ithelp.ithome.com.tw/articles/10402109)

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

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

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