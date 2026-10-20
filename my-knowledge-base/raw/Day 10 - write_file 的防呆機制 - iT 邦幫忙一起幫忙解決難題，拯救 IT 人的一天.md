---
title: "Day 10 - write_file 的防呆機制 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402250"
author:
  - "[[iThome]]"
published:
created: 2026-08-10
description: "文章同步發表在我的個人 Blog 昨天的 edit_file 可以改檔案裡的一小段內容，但檔案不存在的話它會直接拒絕。所以想請 KeSi 生一份 README.md、一組測試資料或是設定檔範本，現在都..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 10

0

[Software Development](https://ithelp.ithome.com.tw/2026ironman/software-development)

### 為你自己手刻 Claude Code系列 第 10 篇

## Day 10 - write\_file 的防呆機制

- 分享至

> 文章同步發表在 [我的個人 Blog](https://kaochenlong.com/)

昨天的 `edit_file` 可以改檔案裡的一小段內容，但檔案不存在的話它會直接拒絕。所以想請 KeSi 生一份 `README.md` 、一組測試資料或是設定檔範本，現在都還做不到，今天來補上 `write_file` 。

寫檔案本身不難，建新檔跟蓋掉舊檔在 Python 眼裡都是 `open(..., "w")` ，一行就解決。麻煩的是這兩件事的後果差很多，尤其對一個會自己動手的 agent 來說：建新檔頂多是目錄裡多一個檔案，蓋掉舊檔可能把你寫了三小時的東西清成空白。

GitHub Repo： [https://github.com/kaochenlong/KeSi](https://github.com/kaochenlong/KeSi)

## 三行清空舊檔

先看最短的版本：

```python
def write_file(file_path, content):
    with open(file_path, "w", encoding="utf-8") as file:
        file.write(content)
    return f"已寫入 {file_path}"
```

這段程式可以建立新檔，遇到同名的舊檔也會直接蓋掉。補個大家可能比較不知道的事，Python 的 `open()` 函式的那個參數 `"w"` ，會在開檔的那一瞬間就把舊內容清光了，根本不必等到 `write()` 喔。不信的話跑跑看下面這個範例，會發現一個字都沒寫進去：

```markdown
>>> from pathlib import Path
>>> Path("victim.txt").write_text("這是本來就有的重要內容\n第二行\n第三行\n")
20
>>> Path("victim.txt").read_text()
'這是本來就有的重要內容\n第二行\n第三行\n'
>>> with open("victim.txt", "w", encoding="utf-8"):
...     pass
...
>>> Path("victim.txt").read_text()
''
```

給個 `pass` 什麼事都不做，但原本的三行內容沒了，檔案變成 0 bytes。這不是 Python 的什麼奇怪設定， [官方文件](https://docs.python.org/3/library/functions.html#open) 就有講到 `'w'` 模式的行為：

> open for writing, truncating the file first

`truncating the file first` 這個 first 就是重點，先清空然後才輪到你寫。同款還有一個 `'x'` 參數：

> open for exclusive creation, failing if the file already exists

只負責建立新檔，目標已經存在就直接失敗，丟出 `FileExistsError` 。等一下的 `write_file` 就是靠它把「建新檔」跟「蓋舊檔」分成兩條路。

報錯至少會讓 agent 知道這件事沒做成，蓋掉檔案卻是回報「已寫入」，等到有人發現原本的內容不見了，通常已經不知道是多久之後的事了。先來把 `write_file` 的規矩訂出來：

- 如果檔案還不存在，用 `'x'` 建立，萬一就在這個空檔突然從哪裡冒出一個同名的檔案，寧可建立失敗也不要蓋掉它。
- 如果檔案已經存在，模型得先讀過現在的內容才准整份蓋掉。
- 如果讀完之後檔案又被別的程式改過，不准覆寫，請它重讀一次再來一次。
- 如果路徑跑到工作目錄外面、在 ignore 名單裡或是指向一個目錄，直接拒絕。
- 回傳的格式跟之前一樣那組 `(output, is_error)` 。

只改幾行的話還是用 `edit_file` 。 `write_file` 收的是一整份內容，適合拿來開新檔，或是真的要把某個檔案整份換掉的時候。

## 新檔的路徑要檢查

在檢查路徑的 `safe_path()` 函式有這一行：

```python
target = (BASE_DIR / path).resolve(strict=True)
```

`strict=True` 參數的設定效果是如果檔案不存在就直接失敗，這拿來用在讀取跟編輯剛剛好，但要寫的新檔在寫之前本來就不存在，直接拿這個去做 `write_file` 的話正常的新檔路徑也都會拿到 `None` 。所以寫入這裡我另外寫一個 `safe_write_path()` 函式，這個函式只收相對路徑而且改用 `strict=False` ，讓路徑最後那一段還沒出現也能解析，解析完再確認位置仍然在工作目錄裡面，沒有越獄：

```python
def safe_write_path(path):
    if not isinstance(path, str) or not path.strip():
        return None

    path_obj = Path(path)
    windows_path = PureWindowsPath(path)
    if (
        path.startswith("~")
        or path_obj.is_absolute()
        or windows_path.drive
        or windows_path.root
    ):
        return None

    try:
        target = (BASE_DIR / path_obj).resolve(strict=False)
    except (OSError, RuntimeError, TypeError, ValueError):
        return None
    if target == BASE_DIR or not target.is_relative_to(BASE_DIR):
        return None
    return target
```

ignore 的檢查也要拿解析完的 `target` 去問 `is_ignored()` ，不能只看模型傳進來的那一串字。不然像符號連結、 `sub/../.env` 或是 `.env.local` 這些寫法只看字面很容易漏掉。另外，建好上層目錄之後程式會再解析、再檢查一次路徑，讓「檢查完」跟「真的動手寫」中間的空檔小一點。

這還不算是那種可以擋惡意程式的完整 sandbox，更嚴格的隔離之後有專門的一天來處理。

## 「讀過」還不夠

先讀後寫最直覺的做法是準備一個 `set` 把讀過的路徑記起來：

```python
READ_FILES = set()
```

不過只記得路徑還不夠，看看這個順序：

1. KeSi 讀到 `config.py` 的第一版。
2. 我開編輯器、或是別的程式把它改成第二版。
3. KeSi 還以為手上是最新的，整份蓋過去。

set 只記得「讀過」的話，第三步還是會放行然後就被蓋掉了。在第 9 天的 `old_string` 只能保證那一小塊沒被動過，現在要整份覆寫就得知道整個檔案還是不是原來那一份。

所以 KeSi 這裡我改用一個 dict（字典），把執行 `read_file` 時候讀到的原始 bytes 算出一組 SHA-256 然後存進去：

```python
READ_VERSIONS = {}

def file_digest(data):
    return hashlib.sha256(data).digest()
```

這裡的雜湊不是簽章，也不是要證明這個檔案可不可信，它就只是一段固定長度的內容指紋，一樣的內容一定算出一樣的結果，改掉其中一個 byte 就完全不同了。 `read_file` 要成功解出 UTF-8 才會登記，失敗的話連舊的紀錄也要一起清掉：

```python
def read_file(file_path):
    recorded_target = safe_write_path(file_path)
    if recorded_target is not None:
        READ_VERSIONS.pop(recorded_target, None)

    target = safe_path(file_path)
    if target is None:
        return f"錯誤：找不到或不允許存取檔案 {file_path}", True
    if is_ignored(target):
        return "錯誤：這個檔案不開放讀取。", True
    if not target.is_file():
        return f"錯誤：不是可以讀取的文字檔 {file_path}", True

    try:
        data = target.read_bytes()
        content = data.decode("utf-8")
    except UnicodeDecodeError:
        return f"錯誤：檔案不是 UTF-8 文字檔 {file_path}", True
    except OSError as exc:
        return f"錯誤：無法讀取檔案 {file_path}：{exc}", True
    READ_VERSIONS[target] = file_digest(data)
    return content, False
```

`edit_file` 改成功之後這筆紀錄要拿掉，因為之前讀到的那一整份已經不是現在的內容了：

```python
READ_VERSIONS.pop(target, None)
return f"替換完成（從第 {line_no} 行開始）。", False
```

`/reset` 也要清除版本表：

```python
if user == "/reset":
    history = []
    READ_VERSIONS.clear()
    print("（日記與檔案版本紀錄已清空，我們重新開始）")
    continue
```

`/reset` 之後模型已經不記得檔案內容了，工具這邊當然也不能再拿上一段對話讀到的版本當證據。以後 KeSi 如果要同時服務好幾個 session，這張表也不能再擺在全域變數，得跟著每個 session 分開放。

## 新檔跟舊檔是不同行為

覆寫既有檔案的時候，我會先把新內容寫進同一個目錄下的暫存檔，寫完再用 `fsync()` 要求作業系統把資料真的送到磁碟上，不要只是先擺在記憶體的快取裡。接著回頭確認一次目標檔案的版本，還是同一版才呼叫 `os.replace()` 換過去：

```python
def replace_existing_file(target, data, expected_digest):
    temp_path = None
    try:
        fd, temp_name = tempfile.mkstemp(
            dir=target.parent,
            prefix=f".{target.name}.",
            suffix=".tmp",
        )
        temp_path = Path(temp_name)
        with os.fdopen(fd, "wb") as file:
            file.write(data)
            file.flush()
            os.fsync(file.fileno())

        latest = target.read_bytes()
        if file_digest(latest) != expected_digest:
            return "錯誤：檔案在讀取後又有變更，請重新 read_file 後再覆寫。"

        mode = stat.S_IMODE(target.stat().st_mode)
        os.chmod(temp_path, mode)
        os.replace(temp_path, target)
        temp_path = None
        return None
    except OSError as exc:
        return f"錯誤：無法覆寫檔案：{exc}"
    finally:
        if temp_path is not None:
            try:
                temp_path.unlink(missing_ok=True)
            except OSError:
                pass
```

暫存檔為什麼跟目標檔案放在同一個目錄？這跟 Python 的 [`os.replace()` 文件](https://docs.python.org/3/library/os.html#os.replace) 裡的這兩句話有關：

> The operation may fail if *src* and *dst* are on different filesystems. If successful, the renaming will be an atomic operation (this is a POSIX requirement).

前面那句是說，來源跟目標如果落在不同的檔案系統上，這個動作有可能直接失敗。我自己之前的個人習慣是會丟到 `/tmp` 目錄，但這裡所以跟目標放在同一個目錄比較保險。

後面那句的 atomic operation 中文叫「原子操作」，意思是這個動作沒有中間狀態，不是還沒換就是已經換好了，沒有在換與沒換之間的狀態。換句話說，別的程式在這期間跑去讀那個路徑，讀到的不是完整的舊檔就是完整的新檔，不會讀到寫了一半的東西。括號裡還補了一句 this is a POSIX requirement，這是 POSIX（Unix 系統的標準規範）的要求，不是 Python 自己額外提供的保證。

但建立新檔就不能這樣做了，因為先寫暫存檔再換過去的話，萬一這中間剛好有人建了一個同名的檔案，一樣會被蓋掉。所以新檔這條路直接用 `"xb"` 開檔，讓檔案系統來保證「目標不存在才建立得起來」。完整的 `write_file` 長這樣：

```python
def write_file(file_path, content):
    if not isinstance(file_path, str) or not isinstance(content, str):
        return "錯誤：file_path 與 content 都必須是字串。", True

    try:
        data = content.encode("utf-8")
    except UnicodeEncodeError:
        return "錯誤：content 含有無法寫成 UTF-8 的內容。", True

    target = safe_write_path(file_path)
    if target is None:
        return f"錯誤：不允許寫入路徑 {file_path}", True
    if is_ignored(target):
        return "錯誤：這個路徑不開放寫入。", True

    try:
        target.parent.mkdir(parents=True, exist_ok=True)
    except OSError as exc:
        return f"錯誤：無法建立上層目錄 {file_path}：{exc}", True

    target = safe_write_path(file_path)
    if target is None or is_ignored(target):
        return f"錯誤：不允許寫入路徑 {file_path}", True

    if target.exists():
        if not target.is_file():
            return f"錯誤：不是可以覆寫的文字檔 {file_path}", True

        expected_digest = READ_VERSIONS.get(target)
        if expected_digest is None:
            return (
                "錯誤：覆寫既有檔案前，必須先用 read_file 讀取目前內容。",
                True,
            )

        try:
            current = target.read_bytes()
        except OSError as exc:
            READ_VERSIONS.pop(target, None)
            return f"錯誤：無法確認檔案目前內容 {file_path}：{exc}", True

        if file_digest(current) != expected_digest:
            READ_VERSIONS.pop(target, None)
            return (
                "錯誤：檔案在讀取後又有變更，請重新 read_file 後再覆寫。",
                True,
            )
        if current == data:
            return "檔案內容相同，不需要覆寫。", False

        error = replace_existing_file(target, data, expected_digest)
        READ_VERSIONS.pop(target, None)
        if error is not None:
            return error, True
        return f"已覆寫 {file_path}（{len(data)} bytes）。", False

    created_identity = None
    try:
        with target.open("xb") as file:
            info = os.fstat(file.fileno())
            created_identity = (info.st_dev, info.st_ino)
            file.write(data)
            file.flush()
            os.fsync(file.fileno())
    except FileExistsError:
        return (
            "錯誤：檔案剛被其他程式建立，請先用 read_file 讀取後再決定。",
            True,
        )
    except OSError as exc:
        if created_identity is not None:
            try:
                info = target.stat()
                if (info.st_dev, info.st_ino) == created_identity:
                    target.unlink()
            except OSError:
                pass
        return f"錯誤：無法建立檔案 {file_path}：{exc}", True

    READ_VERSIONS.pop(target, None)
    return f"已建立 {file_path}（{len(data)} bytes）。", False
```

不管走哪一條路，回報的都是轉成 UTF-8 之後的 byte 數，而不是用 Python 的字串長度冒充檔案大小。 `len()` 算的是這串字有幾個字元，但檔案在磁碟上佔多少空間是看 byte 數，UTF-8 底下一個英文字母算 1 個 byte，一個中文字通常要 3 個，有中文的時候這兩個數字就對不起來了：

```markdown
>>> content = "# 待辦\n"
>>> len(content)
5
>>> len(content.encode("utf-8"))
9
```

等一下驗收的時候會看到 `已建立 docs/TODO.md（9 bytes）。` ，那個 9 就是這樣來的。另外，建檔成功不會順便算成讀過一次，下一步如果又要整份覆寫，還是得先 `read_file` 。

## 工具定義也要有限制

description 只是「說明書」，本身雖然擋不住任何事但它會影響模型挑 `edit_file` 還是 `write_file` 。把什麼時候該用、什麼情況會失敗寫清楚，真正的檢查還是留在 Python 函式裡：

```python
{
    "name": "write_file",
    "description": "建立新的 UTF-8 文字檔，或用完整內容覆寫既有檔案。"
    "修改既有檔案的一小部分時優先使用 edit_file。"
    "若確定要覆寫既有檔案，必須先用 read_file 讀取目前版本；"
    "檔案在讀取後若又有變更，write_file 會拒絕覆寫。",
    "strict": True,
    "input_schema": {
        "type": "object",
        "properties": {
            "file_path": {
                "type": "string",
                "description": "相對於工作目錄的檔案路徑",
            },
            "content": {
                "type": "string",
                "description": "要寫入檔案的完整 UTF-8 文字內容",
            },
        },
        "required": ["file_path", "content"],
        "additionalProperties": False,
    },
},
```

`TOOL_FUNCS` 這張對照表再補一筆：

```python
TOOL_FUNCS = {
    "read_file": read_file,
    "list_files": list_files,
    "glob": glob_files,
    "grep": grep,
    "edit_file": edit_file,
    "write_file": write_file,
}
```

今天完整的 `kesi.py` 一樣可以在 GitHub Repo 取得。另外有個小改動要提一下，我把 `client` 移進 `main()` 裡面了，檔案最後也加上 `if __name__ == "__main__"` 。這樣等一下要測工具比較方便。

## 模型話說到一半被切斷？

整份內容都塞在 `tool_use.input.content` 這個欄位裡，還有一種情況要處理，模型內容可能還沒寫完，回應就先到了 `max_tokens` 或是 context window 的上限，這怎麼處理？

每一次回應裡都有一個 `stop_reason` 欄位，寫著模型這一輪為什麼停下來。KeSi 只有在它是 `tool_use` 的時候才會去執行工具，所以 `max_tokens` 跟 `model_context_window_exceeded` 這兩種本來就不會有任何工具被跑起來。

麻煩的是其他情況一律被當成「模型話講完了」，程式把文字回傳就換你打下一句。這一輪明明是因為 token 上限而被切斷的，畫面上卻只看得到模型講到一半的那段話，沒有任何地方告訴你後面還有東西沒寫完。在把回應寫進 history 之前加上這個分支的處理：

```python
if resp.stop_reason in ("max_tokens", "model_context_window_exceeded"):
    reason = (
        "輸出達到 max_tokens"
        if resp.stop_reason == "max_tokens"
        else "回應填滿 context window"
    )
    message = f"錯誤：模型{reason}，本輪沒有執行工具。"
    history.append({"role": "assistant", "content": message})
    return message
```

這是保守的做法，只要回應是因為長度上限停下來的，這一輪的工具一個都不執行，也不把可能只寫了一半的內容記進日記。關於這兩個名字的差別，Anthropic 的 [stop reason 處理文件](https://platform.claude.com/docs/en/build-with-claude/handling-stop-reasons#max_tokens) 有分開說明：

> `max_tokens` ：Claude stopped because it reached the `max_tokens` limit specified in your request.
> 
> `model_context_window_exceeded` ：Claude stopped because it reached the model's context window limit.

一個是超過你自己在請求裡設的上限，一個是把模型本身的 context window 塞滿了。同一份文件對後者的處理建議只有一句 `Treat the response as truncated.`，意思是把它當成被切斷的回應處理。至於前者，文件講得更具體：

> If Claude's response is cut off because it hit the `max_tokens` limit, and the truncated response contains an incomplete tool use block, you'll need to retry the request with a higher `max_tokens` value to get the full tool use.

也就是把 `max_tokens` 調大之後再重送一次。現在的 KeSi 沒有自動重送的機制，所以先把錯誤回報出來，因為多送一次就是多給它一次建立或覆寫檔案的機會。而且現在也還沒有 undo 可以用，寫壞了就是寫壞了，要不要再送一次還是交給人自己決定比較好。

## 驗收工具！

一樣先別急著找模型，今天的 `kesi.py` 有 `if __name__ == "__main__"` 擋著，測試不必再插進主程式裡面，直接另外開一個檔案把工具函式 import 進來就好：

```python
from kesi import read_file, write_file
from pathlib import Path

Path("config.py").write_text("DEBUG = False\n")
output, is_error = write_file("config.py", "DEBUG = True\n")
print(is_error)
print(output)
print(Path("config.py").read_text())
```

`config.py` 已經存在，而且這一輪沒有人讀過它：

```markdown
True
錯誤：覆寫既有檔案前，必須先用 read_file 讀取目前內容。
DEBUG = False
```

擋下來了，最後一行也確認舊內容還在。中間補一次 `read_file` 再試一遍：

```python
read_file("config.py")
output, is_error = write_file("config.py", "DEBUG = True\n")
```
```markdown
False
已覆寫 config.py（13 bytes）。
DEBUG = True
```

這次過了。那如果讀完之後、還沒寫回去之前，檔案被別人動過了呢？

```python
read_file("config.py")
Path("config.py").write_text("DEBUG = False\nEXTRA = 1\n")   # 假裝是別人改的
output, is_error = write_file("config.py", "DEBUG = True\n")
```
```markdown
True
錯誤：檔案在讀取後又有變更，請重新 read_file 後再覆寫。
DEBUG = False
EXTRA = 1
```

模型讀過了，照規矩有資格覆寫，但它手上那份已經不是磁碟上的那份了。 `EXTRA = 1` 這行沒有被蓋掉，這就是那張雜湊表在做的事。至於新檔就單純多了，連子目錄都會順手建起來：

```python
output, is_error = write_file("docs/TODO.md", "# 待辦\n")
```
```markdown
False
已建立 docs/TODO.md（9 bytes）。
```

除了這四種，還有幾個情況值得自己補跑： `edit_file` 成功之後版本紀錄要失效、讀取失敗也要失效、`.env.local` 跟工作目錄外面的路徑要拒絕、指向外面的符號連結要拒絕、既有檔案的權限設定覆寫後要保留。

工具這層過關之後才輪到模型。先看新檔：

```markdown
你 > 建立一個 TODO.md，寫三件事：買牛奶、繳電話費、回信給客戶
  [執行工具] write_file({'file_path': 'TODO.md', 'content': '# TODO 清單\n\n- [ ] 買牛奶\n- [ ] 繳電話費\n- [ ] 回信給客戶\n'})
```

新檔沒有舊內容要保護，不必先讀，直接建立就好，這條路很單純。

第二題先放一份現成的 `config.py` ，再叫它整份重寫。照 description 寫的，希望看到的順序是先讀再寫：

```markdown
你 > 把 config.py 的內容整份換成 DEBUG = True
  [執行工具] read_file({'file_path': 'config.py'})
  [執行工具] write_file({'file_path': 'config.py', 'content': 'DEBUG = True'})
```

模型如果沒先讀就直接呼叫 `write_file` ，工具還是會用「必須先用 read\_file」把它擋下來，就跟剛才手動測的那次一樣。

切記 description 的作用只是提高模型「先讀後寫」的機率，它不是防線。這次照做，不代表下次也會照做，換一顆模型更不一定，萬一遇到有心人士想繞過去就更不用說了。擋住「沒讀就覆寫」的還是靠 `READ_VERSIONS` 那張表，description 只是讓模型少走一次冤枉路。

## 跟 Claude Code 的規則差異

截至 2026 年 8 月 3 日， [Claude Code 的 Write 工具文件](https://code.claude.com/docs/en/tools-reference#write-tool-behavior) 開頭是這樣寫的：

> The Write tool creates or overwrites files. It doesn't perform partial edits the way Edit does, and Claude should prefer Edit for changes to existing files.

跟今天的分工一樣，改既有檔案優先用 Edit，Write 是拿來建新檔或整份換掉的。至於覆寫前要不要先讀，同一份文件把它列成第一項檢查：

> Read-before-write: Claude reads the file in the current conversation before overwriting it, and a read cut short with a `PARTIAL view` notice doesn't count.

注意後半句寫的「讀到一半」不算讀過，在第 6 天做 `read_file` 的時候曾經提過，正版 Claude Code 在讀大檔會先回一段，然後附上 `PARTIAL view` 的提示，這種只讀了半份的紀錄，Write 這一關不承認。KeSi 目前功能還很陽春沒有這條規則，因為我們的 `read_file` 還不會把內容截斷，之後如果有機會加上去了，這裡也要跟著補。

陽春歸陽春，但 KeSi 走的是也同一個方向，不過只做了自己有實作的那一部分，多存一份內容的雜湊，檔案讀完之後只要有一個 byte 變了，就拒絕整份覆寫。而事前擋下來也不是唯一的做法，前面引過的那份 [文字編輯工具文件](https://platform.claude.com/docs/en/agents-and-tools/tool-use/text-editor-tool#implementation-best-practices) ，實作建議那節有一條就叫「改之前先備份」：

> Implement a backup system in your application that creates copies of files before allowing Claude to edit them, especially for important or production code.

另一條路是事後還原， [Claude Agent SDK 的 file checkpointing](https://code.claude.com/docs/en/agent-sdk/file-checkpointing) 就是在做這件事，開啟之後它會記錄 Write 跟 Edit 動過哪些檔案，需要的時候可以倒回去。

這三樣 KeSi 一個都沒有，沒有備份、沒有 undo，也沒有還原點。先讀後寫擋的是「模型拿著過期的版本亂寫」，它讓覆寫這件事比較不容易出錯，但檔案真的被蓋掉之後，KeSi 手上沒有任何東西可以把舊內容變回來。 `READ_VERSIONS` 存的只是一段雜湊，拿它可以比對出「這一份被動過了」，卻沒辦法從一段雜湊倒推回原本的檔案內容。

## 還是不夠萬無一失

`os.replace()` 保證的是別的程式不會從目標路徑讀到寫了一半的內容，但這不等於「不管發生什麼事，留下來的一定是完整的舊版或新版」。有幾件事它做不到：

- 最後那次比對雜湊跟 `os.replace()` 中間還是有一小段空檔，這兩個動作沒辦法併成一個不能被插隊的步驟。
- 覆寫的時候只把 Unix 的權限設定複製過去，檔案的擁有者、ACL（誰可以讀、誰可以寫的細部清單）這些都沒有跟著保留。
- 暫存檔有用 `fsync()` 把內容真的送到磁碟，但它所在的那個目錄沒有。例如運氣不好遇到突然斷電，光靠這段程式不能保證一定救得回來。
- 新檔用 `"xb"` 是先把名字佔起來再寫內容。寫到一半失敗、而且錯誤攔得到的話，程式會先確認那個檔案還是自己剛建的那一個，再把它刪掉。整支程式因為某些原因被強制中止的時候，還是可能留下一個殘缺的新檔。
- 路徑檢查適合用在自己信得過的工作目錄。目前它不是作業系統等級的 sandbox，擋不住有心人士在旁邊一直換符號連結。

不過上面這些不會推翻今天做的事，只是先讓大家知道正版的工具需要更嚴格的交易保證、要完整保留檔案的各種屬性，或是有好幾支程式同時在動同一批檔案，要做到這樣就得靠平台專用的 API、檔案鎖、版本控制，或是更完整的還原機制了。

## 佔多少 context？

`write_file` 每次都要交出一整份內容，這樣會不會很燒 context？會，而且不是只花那一次。模型交出來的 `content` 會變成 `assistant` 回應裡的一塊 `tool_use` 留在日記裡，之後你每問一句，這一整份內容都要跟著再送一次給模型看，一直到 `/reset` ，或是之後做了壓縮機制為止。

有沒有感覺就看檔案多大。剛才那個 `TODO.md` 只有 9 bytes，重送幾十輪也沒什麼差別，但如果請它生一份幾百行的設定檔，那幾百行從此每一輪都要再跑一趟。

所以昨天說「為了改一小段就把整份檔案重新生成一次」很浪費，講的是修改既有檔案的情況，不是說完整內容一律不能寫。新檔沒有舊內容要保留，直接給整份 `content` 通常最省事。真的很大的檔案，可以考慮先讓模型寫一份骨架，之後再用 `edit_file` 一段一段補，或是另外做一個只負責往檔案後面接內容的工具。

`READ_VERSIONS` 這張表不會進到日記裡，是 KeSi 這支程式自己在旁邊記的，所以不會佔 context，模型也看不到這張表，它只能從工具說明跟錯誤訊息知道有這條規矩。

## 小結

今天多的不只是在 `open()` 外面包一層。新檔走 `'x'` 這條路，同名的檔案冒出來就失敗。舊檔要先讀過，而且讀到的還得是同一版才准覆寫。禁區跟跑到工作目錄外面的路徑照樣拒絕。真的要覆寫的時候，先寫暫存檔再換過去。模型話講到一半被切斷，這一輪的工具一個都不執行。

KeSi 手上的工具越來越多了，現在有 `read_file` 、 `list_files` 、 `glob` 、 `grep` 、 `edit_file` 跟 `write_file` 六個檔案工具。KeSi 知道 bytes 已經寫進去了，但還是不知道新的程式碼跑不跑得起來、測試會不會過。

下一集要來補上 `run_command` 工具，KeSi 就可以自己執行測試、自己讀結果。不過能做的事變多，能闖的禍也跟著變多，後面我們再看看怎麼收拾。

咱們下集見:)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402235)

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