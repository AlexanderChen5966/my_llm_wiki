---
title: "Day 09 - 改 code 為什麼用字串替換 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402235"
author:
  - "[[iThome]]"
published:
created: 2026-08-10
description: "文章同步發表在我的個人 Blog 前幾篇文章把「看」的能力補齊了，現在 KeSi 會讀檔、列目錄、找檔名、搜尋內容，但 coding agent 的正職不是只回答 code 在哪裡而是動手修改，這篇文..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 9

0

[Software Development](https://ithelp.ithome.com.tw/2026ironman/software-development)

### 為你自己手刻 Claude Code系列 第 9 篇

## Day 09 - 改 code 為什麼用字串替換

- 分享至

> 文章同步發表在 [我的個人 Blog](https://kaochenlong.com/)

前幾篇文章把「看」的能力補齊了，現在 KeSi 會讀檔、列目錄、找檔名、搜尋內容，但 coding agent 的正職不是只回答 code 在哪裡而是動手修改，這篇文章要來做的是可以「編輯」檔案的工具。

前面四個工具雖然不會改檔，仍可能讀到不該讀的檔案，不算是零風險，但從這篇文章開始會再多一種更刺激的風險，模型一旦判斷錯誤，檔案真的會被改掉。

先說結論，「字串替換」是 Claude Code 之類的 coding agent 常見的編輯手段之一，指定舊文字然後把它換成新文字，不過實務上也有人用整檔重寫或是從語法下手的 AST 或 LSP。

GitHub Repo： [https://github.com/kaochenlong/KeSi](https://github.com/kaochenlong/KeSi)

## 整檔重寫不行嗎？

最直覺的做法是整檔重寫，也就是把檔案內容給模型，然後讓模型吐出修改後的完整版本再整個覆蓋舊檔。不過這樣即使只改幾行模型仍要回傳整份檔案，成本有點太高，而且速度也比較慢。以 KeSi 使用的 Haiku 4.5 來說， [Claude API 的基本費率](https://platform.claude.com/docs/en/about-claude/pricing) 是每百萬 input token 1 美元、output token 5 美元。重點在 output 比 input 貴五倍，整檔重寫表示就算沒有變動的部份也會用比較貴的價格算錢，但事實上局部替換通常只需輸出新內容就好。不過實際花費還是要看檔案大小、 `old_string` 帶了多少上下文、tokenizer 與工具 schema，不能光憑行數喊出固定倍數。但基本上方向很明確，檔案越大、改動越小，這種整個檔案重寫的成本就越高。

整檔重寫除了錢錢比較貴之外還有別的問題，因為重寫的過程是讓每個字元都重新經過模型生成，而模型可能不小心漏掉 `import` 或是順手整理註解，甚至用「其餘不變」代替沒有抄完的內容。這些問題不一定會發生，萬一發生也能靠 diff、測試與版本控制抓到，只是局部替換一開始就先縮小了可能被改動的範圍。

還有一條路是丟 diff 給模型，這是對工程師來說應該不陌生，但這對模型不太友善。標準的 unified diff 每一段變更前面都有一行像 `@@ -12,7 +12,9 @@` 的標頭，意思是「從第 12 行開始，原本 7 行，改完之後變 9 行」。這行標頭排在變更內容的前面，模型得先把改動算完，才寫得對這幾個數字，算錯了這份 diff 就可能套不上去。 [Anthropic 的 agent 工具設計文章](https://www.anthropic.com/engineering/building-effective-agents#appendix-2-prompt-engineering-your-tools) 有提到這件事，也建議工具的格式不要讓模型還得花力氣算行數。真的要走 diff 這條路也不是不行，只是那行標頭最好由工具自己算完填上去，模型專心寫改動的內容就好。不然就是換一種根本不用算行數的格式，例如直接給舊文字跟新文字，也就是今天要做的這種。

從「語法」下手的 AST 或 LSP 也是不錯，抽象語法樹 AST（Abstract Syntax Tree）是把程式碼交給對應語言的 parser 解析之後會得到的一棵樹，函式、變數、判斷式各自是樹上的一個「節點」。工具動的是節點而不是文字，所以分得出 `count` 這幾個字什麼時候是變數，什麼時候只是註解裡剛好出現的一個詞。語言伺服器協定 LSP（Language Server Protocol）講的是編輯器要怎麼跟語言分析工具溝通。當你在 VS Code 裡按著 Ctrl 或 Cmd 點一下函式名稱跳到它的定義，或是重新命名變數然後讓好幾個有用到它的地方一起改，這些都是語言功能。改一個名字、把 method 搬到別的類別、做這類重構的時候，這種懂語法的工具往往比一個字一個字比對可靠，代價是得先有對應語言的 parser、language server 與操作定義。

成熟的 coding agent 可以混用多種編輯方式，KeSi 目前則先拿跨語言的字串替換打底，之後有機會再把這些加進來。

## old\_string 與 new\_string

替換式編輯的介面只有三個參數：檔案、舊字串、新字串。參數就這麼少，模型送什麼進來就得管得嚴一點，所以 KeSi 會給 `old_string` 訂兩條規矩，首先是它得跟檔案裡的內容一字不差，另外是它在整份檔案裡只能出現一次。

先講第一條，Anthropic 的 `str_replace_based_edit_tool` 文件規定， [`old_str` 必須連空白與縮排都完全一致](https://platform.claude.com/docs/en/agents-and-tools/tool-use/text-editor-tool#str_replace) 。KeSi 也採同一條規矩，四個空白不等於 tab， `\n` 不等於 `\r\n` ，大小寫當然也不能混。

「換行」有點麻煩，因為 Python 自己就會偷偷動它。 `Path.read_text()` 跟文字模式的 `open()` 預設值是 `newline=None` ，在這個模式下讀進來的 `\r` 跟 `\r\n` 都會被一律轉成 `\n` ，寫出去的時候 `\n` 又可能被轉成當前系統慣用的行尾。

也就是說，我們才剛跟模型說好換行要一字不差，結果光是把檔案讀進來這一步，換行就已經被換掉了。所以順手把第 6 天寫的那個 `read_file` 也一起改掉，讓它先把檔案讀成 bytes，再明確用 UTF-8 解碼：

```python
return target.read_bytes().decode("utf-8"), False
```

`edit_file` 讀檔寫檔也都改用同樣的方式。舉個例子，先寫一個行尾是 `\r\n` 的檔案，再用兩種方式讀回來：

```markdown
>>> from pathlib import Path
>>> Path("demo.txt").write_bytes(b"x = 1\r\ny = 2\r\n")
14
>>> Path("demo.txt").read_text()
'x = 1\ny = 2\n'
>>> Path("demo.txt").read_bytes().decode("utf-8")
'x = 1\r\ny = 2\r\n'
```

同一個檔案 `read_text()` 讀回來的 `\r\n` 已經被換成 `\n` 了，但 `read_bytes().decode()` 才是檔案裡本來的樣子。

讀進來的字串不一樣，這件事會在寫回去的時候變成問題。假設 KeSi 用 `read_text()` 讀進來然後改完再用文字模式寫回去，明明只動了 `x = 1` 那一行，整個檔案的行尾卻會從 `\r\n` 變成執行 KeSi 這台電腦慣用的行尾， `y = 2` 那行根本沒被碰到也一起變了。如果專案沒有透過 `.gitattributes` 或 Git 設定統一行尾，這種改動送進 Git 之後，可能會看到整份檔案都有異動，真正改哪一行反而找不出來。

改用 bytes 讀寫之後，這個問題就不會發生了。 `read_bytes()` 不會去管檔案內容是什麼意思，磁碟上是哪些 byte 就原封不動給你哪些 byte， `\r` 就是 `\r` ，不會被當成換行順手改掉；寫回去用的 `write_bytes()` 也一樣，你給它什麼 byte 它就寫什麼 byte。上面那段 REPL 就看得到了，同一個檔案用它讀出來是 `'x = 1\r\ny = 2\r\n'` ，跟磁碟上一個 byte 都不差。

不過換行的差異並沒有因此消失， `old_string` 的換行要是帶錯一樣會找不到而失敗，只是失敗歸失敗，檔案裡沒被換掉的地方會保持原樣。

這樣是不是太嚴格了？縮排差一個空白，幫模型猜一下不行嗎？也可以啦，但只要開始猜就得再回答兩個問題，一個是萬一有兩個地方都很像，要猜改哪一個？還有，要不要在真的寫下去之前，先把它打算怎麼改印出來給人看一眼？這兩件事現在的 KeSi 都沒做，所以乾脆嚴格到底，差一個字元就報錯，讓模型自己重讀一次、多帶一點前後文再送一次。

至於「修改前應先讀」這句話，它寫在工具的說明書裡，是講給模型聽的建議， `edit_file` 這個函式本身並不會去檢查模型到底讀過沒有。

舉個例子，模型如果剛才讀過 `app.py` ，接著在同一段對話裡想再改一個地方，這時候檔案內容還留在對話紀錄裡，模型不必再讀一次，直接送 `old_string` 過來也會成功。反之如果你在這中間自己開編輯器把那一段改掉了（我還滿常幹這件事的），模型手上那段文字就過期了， `edit_file` 會找不到，替換就失敗。

所以這裡並沒有真的「強制先讀」，只是內容對不上就過不了關而已。真的想要求模型每次動手之前都先讀一次，程式得另外記住「這個檔案上次讀到的時候長什麼樣」，動手前再比對一次才行。

## 為什麼要唯一？

第二條規矩是唯一匹配， `old_string` 在檔案裡只能找得到一個開頭的位置。

找不到表示模型手上那段文字跟現在的檔案對不上。但如果找到很多個也有點危險，例如檔案裡有三個地方都寫著 `count += 1` ，工具根本沒辦法知道模型想改的是哪一個，這時候萬一模型自作主張挑第一個，等於是猜一個答案然後回報成功。

所以 KeSi 的規矩是找不到就報錯，找到不只一個也報錯，只有剛好一個才動手。Anthropic 的 [文字編輯工具實作建議](https://platform.claude.com/docs/en/agents-and-tools/tool-use/text-editor-tool#implement-the-text-editor-tool) 裡，實作時要記得的四件事，最後一件就是這個：

> Unique matching: Make sure replacements match exactly one location to avoid unintended edits.

注意最後那個 `unintended edits` ，它擔心的不是改不到，是改到了但改的不是模型原本想改的那一處。一次找到好幾處符合的時候，回給模型的錯誤訊息會順便把解法講出來，「請加長 `old_string` 、納入更多前後文，使它在檔案中唯一。」，意思就是把函式名稱或前後幾行一起放進去，放到那段文字在檔案裡只剩一處為止。同一份文件示範的訊息也是同一個方向，「Error: Found 3 matches for replacement text. Please provide more context to make a unique match.」，找到幾處講清楚，下一步該做什麼也講清楚。

這裡還有個容易忽略的地方，Python 的 `str.count()` 計算的是「不重疊」的次數， `"aaa".count("aa")` 會回答 1，可是 `aa` 從第 0 個字元開始能算一次，從第 1 個字元開始又能算一次，明明有兩個地方對得上。要決定改哪裡，這就是兩個候選位置，所以實作不能只靠 `count()` 。找到第一處之後，得從下一個字元繼續往後找第二處。

另外兩種不合理的輸入也先擋掉：

- `old_string` 是空字串，因為空字串在哪裡都對得上，根本沒辦法拿來指位置。
- `old_string` 跟 `new_string` 一模一樣，這種替換不會讓檔案有任何改變但卻會被回報成功，模型還以為事情辦好了。

這些檢查都會寫在工具程式裡而不是只放在 description 交代一句就算了。會造成實際損害、或是讓模型誤以為成功的問題就該由程式強制擋下來，不能指望模型每次都記得。

## 動手寫 edit\_file

底下是 `edit_file` 工具的實作：

```python
def edit_file(file_path, old_string, new_string):
    if not all(
        isinstance(value, str)
        for value in (file_path, old_string, new_string)
    ):
        return "錯誤：file_path、old_string、new_string 都必須是字串。", True
    if not old_string:
        return "錯誤：old_string 不可為空。", True
    if old_string == new_string:
        return "錯誤：old_string 與 new_string 相同，沒有內容需要修改。", True

    target = safe_path(file_path)
    if target is None:
        return f"錯誤：找不到或不允許存取檔案 {file_path}", True
    if is_ignored(target):
        return "錯誤：這個檔案不開放修改。", True
    if not target.is_file():
        return f"錯誤：不是可以修改的文字檔 {file_path}", True

    try:
        content = target.read_bytes().decode("utf-8")
    except UnicodeDecodeError:
        return f"錯誤：檔案不是 UTF-8 文字檔 {file_path}", True
    except OSError as exc:
        return f"錯誤：無法讀取檔案 {file_path}：{exc}", True

    first = content.find(old_string)
    if first == -1:
        return (
            "錯誤：在檔案裡找不到要替換的文字。old_string 必須與檔案內容"
            "完全一致（包含空白、縮排與換行），請先用 read_file 確認目前內容。",
            True,
        )
    if content.find(old_string, first + 1) != -1:
        return (
            "錯誤：要替換的文字在檔案裡不只出現一次，無法確定要改哪一處。"
            "請加長 old_string、納入更多前後文，使它在檔案中唯一。",
            True,
        )

    prefix = content[:first]
    line_no = (
        prefix.count("\n")
        + prefix.count("\r")
        - prefix.count("\r\n")
        + 1
    )
    new_content = (
        content[:first] + new_string + content[first + len(old_string):]
    )
    try:
        encoded = new_content.encode("utf-8")
        target.write_bytes(encoded)
    except UnicodeEncodeError:
        return "錯誤：new_string 含有無法寫成 UTF-8 的內容。", True
    except OSError as exc:
        return f"錯誤：無法寫入檔案 {file_path}：{exc}", True
    return f"替換完成（從第 {line_no} 行開始）。", False
```

不管走到哪一個 `return` ，回傳的都是 `(output, is_error)` ，這樣才能接得上之前寫好的 `run_agent()` 。如果成功的時候只回一個字串， `run_agent()` 裡的 `output, is_error = func(...)` 就拆不出正確的兩個值，結果就會變成工具明明寫入成功但最後卻被當成失敗處理。

路徑檢查沿用前兩天的 `safe_path()` 與 `is_ignored(target)` 。黑名單一定要拿解開之後的真實路徑去比對，不能只看模型送進來的那個名字，否則工作目錄裡要是有一個符號連結指向 `.env` ，名字看起來沒踩到黑名單但實際上寫進去的還是禁區。

不過這只能擋下不小心的錯誤操作，擋不住存心找漏洞的人，這終究只是 KeSi 自己在程式裡做的判斷，不是作業系統層級的隔離。像是硬連結（hard link，同一份檔案內容同時掛在兩個不同的路徑底下），或是趁著檢查通過、還沒寫進去的那一小段空檔把路徑換掉，這些都要更底層的手段才防得住。

替換時不用 `replace()` ，而是拿已經確認唯一的位置把內容切成三段再接起來。拿一小段內容做一次就知道了：

```markdown
>>> content = "total = 0\ncount += 1\nprint(total)\n"
>>> first = content.find("count += 1")
>>> first
10
>>> content[:first]
'total = 0\n'
>>> content[first + len("count += 1"):]
'\nprint(total)\n'
>>> content[:first] + "count += 2" + content[first + len("count += 1"):]
'total = 0\ncount += 2\nprint(total)\n'
```

`first` 是 10，表示 `count += 1` 從第 10 個字元開始。前面那一段留著，中間那一段丟掉，後面那一段接回來，中間補上 `new_string` ，這就是上面程式碼裡 `content[:first] + new_string + content[first + len(old_string):]` 做的事。

那為什麼不乾脆用 `replace()` ？因為位置早就算出來了。前面檢查唯一性的時候就已經拿到 `first` ，等一下回報行號也要用它，直接拿來切最省事， `replace()` 反而是再從頭把整份內容掃一遍，找的還是同一段文字。

`new_string` 給空字串的話，中間那一段就直接消失，這樣就變成刪除的效果。剛才的例子把 `"count += 2"` 換成 `""` ，結果是這樣：

```markdown
>>> content[:first] + "" + content[first + len("count += 1"):]
'total = 0\n\nprint(total)\n'
```

`count += 1` 這幾個字不見了，但那一行的換行還在，所以留下一個空行。想連空行一起清掉， `old_string` 就要把後面那個 `\n` 也一起帶進來。 `old_string` 跟 `new_string` 也都可以跨好幾行，所以同一個函式也拿得來換掉一整個區塊，不是只能換一行。

成功訊息回報的是「從第幾行開始」，上面那段內容存成 `counter.py` 再交給 `edit_file` 得到的回應會是：

```markdown
替換完成（從第 2 行開始）。
```

這個行號指的是 `old_string` 的起點，如果這次替換跨了十行，它也不會假裝只動到一行。算行號的時候 LF、CR 跟 CRLF 三種行尾都認得，其中 CRLF 只算一個換行。

這個工具還是有它的限制，這只確認舊文字在哪裡，不會檢查新內容語法對不對、邏輯通不通。 `write_bytes()` 也不是那種寫到一半可以整個退回去的寫法，中途出錯就留下一個改到一半的檔案，目前還沒有自動備份的設計。還有，如果在 KeSi 讀完檔案、還沒寫回去的這段空檔裡有別的程式也動了同一個檔案，那些修改可能會被 KeSi 寫回去的內容蓋掉。重要的檔案還是先放進版本控制或自己留一份備份，之後可以再補上先寫暫存檔再整個換過去的做法、檔案版本檢查，以及改完自動跑測試。唯一匹配這條規則只降低了改錯位置的機會，不等於寫檔案的風險都處理完了。

## 把工具接進迴圈

工具 schema 跟前幾天一樣的寫法：

```python
{
    "name": "edit_file",
    "description": "修改既有 UTF-8 文字檔：把 old_string 替換成 new_string。"
    "old_string 必須與檔案現有內容完全一致（含空白、縮排與換行），"
    "而且在檔案中只出現一次；不唯一時請加長前後文。"
    "修改前應先用 read_file 讀取目前內容。",
    "strict": True,
    "input_schema": {
        "type": "object",
        "properties": {
            "file_path": {
                "type": "string",
                "description": "相對於工作目錄的既有檔案路徑",
            },
            "old_string": {
                "type": "string",
                "description": "要被替換的原文，需完全一致、非空且唯一",
            },
            "new_string": {
                "type": "string",
                "description": "替換後的新內容，可為空字串以刪除原文",
            },
        },
        "required": ["file_path", "old_string", "new_string"],
        "additionalProperties": False,
    },
},
```

最後 `TOOL_FUNCS` 這張對照表也記得加一條：

```python
TOOL_FUNCS = {
    "read_file": read_file,
    "list_files": list_files,
    "glob": glob_files,
    "grep": grep,
    "edit_file": edit_file,
}
```

今天完整的 `kesi.py` 一樣可以在 GitHub Repo 取得。

## 驗收工具！

等等，先別急著跟模型聊天，先確認工具能正常運作再說。 `edit_file()` 裡面有型別檢查、路徑檢查、唯一性判斷好幾層，先確認它自己沒問題，等一下模型出狀況的時候才分得清是誰的責任。何況模型會不會先讀、 `old_string` 會帶幾行或是遇到錯誤之後怎麼重試，每次跑都不見得一樣，工具這一層倒是每次都該有一樣的反應。

做法跟第 8 天一樣，在 `history = []` 前面暫時插幾行，測完再刪掉。要餵給它的情況至少有這些：

- `old_string` 在檔案裡剛好對上一處，替換要成功， `is_error` 是 `False` ，而且只有那一段被換掉，檔案其他地方原封不動。
- `old_string` 找不到，或是一次找到好幾個，這都要算錯誤，而且檔案不能有任何變動。
- `old_string` 給空字串，或是跟 `new_string` 一模一樣，這個也不行。
- `old_string` 跨好幾行的時候要能換掉一整塊， `new_string` 給空字串則要變成刪除。
- 不是 UTF-8 的檔案、目錄、工作目錄以外的路徑，還有名字看起來正常、符號連結解開卻落進禁區的那種，全都要拒絕。
- LF、CRLF 跟單獨一個 CR 這三種行尾，回報的起始行號都要對。CRLF 的檔案裡 `old_string` 要帶 CRLF 才對得上，誤用 LF 要失敗，而且沒改到的地方行尾不能被動。

其中兩個特別值得注意，因為它們就算寫錯了，表面上也看不出來。先是重疊匹配。前面提過 `str.count()` 只算不重疊的次數：

```markdown
>>> "aaa".count("aa")
1
```

Python 說只有一處，但 `aa` 從第 0 個字元能開始一次，從第 1 個字元又能開始一次，其實有兩個地方對得上。開一個裝著 `aaa` 的檔案，把 `aa` 換成 `X` 試試看：

```python
Path("work.txt").write_text("aaa")
output, is_error = edit_file("work.txt", "aa", "X")
print(is_error)
print(output)
print(Path("work.txt").read_text())
```

`Path` 在 `kesi.py` 開頭就 import 過了，所以這裡直接拿來用。跑出來是這樣：

```markdown
True
錯誤：要替換的文字在檔案裡不只出現一次，無法確定要改哪一處。請加長 old_string、納入更多前後文，使它在檔案中唯一。
aaa
```

擋下來了，最後那行 `aaa` 也確認檔案沒被動過。這是因為我們的實作是找完第一處之後，從下一個字元繼續找第二處，沒有靠 `count()` 數數字。再來是 CRLF，一樣插在同一個地方。開一個行尾是 `\r\n` 的檔案，把 `x = 1` 那一行改成 `x = 9` ， `old_string` 用兩種寫法分別送進去：

```python
Path("crlf.txt").write_bytes(b"x = 1\r\ny = 2\r\n")
output, is_error = edit_file("crlf.txt", "x = 1\n", "x = 9\n")
print(is_error)
output, is_error = edit_file("crlf.txt", "x = 1\r\n", "x = 9\r\n")
print(is_error)
print(Path("crlf.txt").read_bytes())
```
```markdown
True
False
b'x = 9\r\ny = 2\r\n'
```

`old_string` 誤用 LF 就是找不到（第一個 `True` 是錯誤），帶對 CRLF 才成功。注意最後那串 bytes，沒被改到的 `y = 2\r\n` 行尾原封不動，這就是前面把 `read_text()` 換成 `read_bytes().decode()` 的理由。

這幾行測完記得刪掉，順手也把 `work.txt` 跟 `crlf.txt` 這幾個檔案刪掉。工具層過關後，我再準備一個 `greet.py` 做個實驗：

```python
print("helo")
```

啟動 KeSi，請它「把 greet.py 的 helo 改成 hello」。如果模型照說明先讀再改，工具的呼叫過程會像這樣：

```markdown
你 > 把 greet.py 的 helo 改成 hello
  [執行工具] read_file({'file_path': 'greet.py'})
  [執行工具] edit_file({'file_path': 'greet.py', 'old_string': 'print("helo")', 'new_string': 'print("hello")'})
```

這條走法是先讀再改，old\_string 帶了一整行，唯一性沒問題。不過前面說過，KeSi 並沒有規定非得先讀不可，模型只要拿得出精確又唯一的原文，KeSi 一樣會放行。

如果有多個一樣的呢？準備一個有兩個 `count += 1` 的檔案然後請它只改最後那一個，其中一種合理的走法會像這樣：

```markdown
你 > counter.py 裡有兩個 count += 1，只把最後那一個改成 count += 2
  [執行工具] read_file({'file_path': 'counter.py'})
  [執行工具] edit_file({'file_path': 'counter.py', 'old_string': 'total = 0\ncount += 1\nprint(total)\ncount += 1', 'new_string': 'total = 0\ncount += 1\nprint(total)\ncount += 2'})
```

這個示意裡，模型沒有先送一個 `count += 1` 過去試，而是第一次就把整整四行包進 `old_string` ，只動最後一行。前後文一次帶足，就沒被擋下來。至於模型為什麼可能這樣做？不知道，光看工具呼叫紀錄只看得到它送了什麼過來，但不知道它心裡在想什麼。

我原本想得到的走法只有兩種，一種是先送一個短的被拒絕、看到錯誤訊息再加長重送，另一種是乖乖先重讀一遍再送。結果它走了第三條，第一次就繞過去了。不過也不是每次都這麼聰明，工具該負責的是把錯誤講清楚，至於模型下一步會不會自己修正那就不是工具能保證的事了。

## 讀完之後檔案被改了？

精確匹配確實能發現內容過期了，但管得到的範圍只有要換掉的那段文字。假設模型讀過：

```python
title = "Hello"
count = 1
```

接著我開編輯器把 `title` 改成 `"Hi"` ，但模型要換的 `count = 1` 仍然存在而且唯一。KeSi 會用目前的檔案內容來進行替換， `title = "Hi"` 也留著。這是局部替換本來就該有的行為，不相干的變動沒必要擋。

但如果我改的剛好就是要替代的部份，例如我把它改成 `count = 2` ，模型拿舊的 `count = 1` 來換就會失敗。所以 `old_string` 比較像是「這一小塊內容還長這樣」的證據，不是「整份檔案從上次讀完到現在都沒被動過」的憑證。讀取跟寫入之間還有一小段空檔，萬一別的程式真的剛好在這時候動了檔案 KeSi 是不知道的。要把這件事做滿，得先幫檔案內容算一個指紋（雜湊）或另外存一個版本，真的要寫下去的摩門特再比對一次，而且比對跟寫入這兩個動作中間可能得要鎖起來，不能讓別人插隊。

這裡有兩個很容易混在一起的東西，一個是前面提過的 `str_replace_based_edit_tool` ，那是 Anthropic 替 API 使用者準備好的工具規格，除了替換還能看檔案、建新檔、插入內容，但實際行為得自己寫，跟我們今天做 `edit_file` 是同一回事。另一個是 Claude Code 這個產品內建的 Edit，你打開 Claude Code 叫它改程式碼，實際動手的就是它。

Claude Code 這邊的規矩比較細。截至 2026 年 8 月 3 日， [Claude Code 工具文件](https://code.claude.com/docs/en/tools-reference#edit-tool-behavior) 是這樣寫的：

> A file that changed on disk after Claude last read it can still be edited when `old_string` matches the current content exactly and unambiguously and Claude Code can read the file without prompting.

意思是檔案在讀完之後被改過並不會直接被擋掉，還要看兩件事， `old_string` 跟現在的內容對不對得上，以及 Claude Code 讀這個檔案需不需要另外跳出權限確認。同一份文件也寫了這是新版才有的行為：

> The relaxed handling of unread and changed files requires Claude Code v2.1.208 or later; before that, Claude Code refused any edit to a file it hadn't read in the conversation or that changed on disk after the read.

`refused any edit` 管得很硬，在 v2.1.208 以前只要沒在這段對話裡讀過、或讀完之後檔案被動過，一律不給改。KeSi 這兩套都沒照抄，只做今天講的這幾條簡單的。

Claude Code 目前的 Edit 同樣做精確、唯一匹配，不用 regex 也不做模糊比對，真的需要把所有匹配到的地方全改掉時，還能明確送一個 `replace_all: true` 。我們現在的 KeSi 沒有這個選項，不能只送 `old_string="count"` 就把所有相符位置一次換掉。它可以逐一修改，每次帶足前後文讓舊文字唯一；如果 10 個位置都落在一段可以唯一辨識的較大區塊，也可以一次替換整個區塊。是說像這種同一個名字如果散在好幾個檔案裡，那可能更適合交給前面提過的 LSP 或各語言自己的重構工具，而不是無條件的全域字串替換。

## 四個工具串起來

把這幾天做的工具排起來， `glob` 找出可能的檔案， `grep` 縮小到哪幾行， `read_file` 把目前的內容拿回來， `edit_file` 再用精確的原文把改動限制在那一小塊整個一條龍。模型不一定每次都會用到這四個，但前一個工具的結果都在替下一步鋪路。

編輯成功也不代表任務成功，KeSi 只知道檔案已經寫下去了，它不知道新的程式碼跑不跑得起來、測試會不會過，也不會替你審查需求。等之後接上執行指令，才會有「修改、測試、讀錯誤、再修改」的循環。而且就算測試都是全綠燈，還是得有人看一眼更大範圍的需求有沒有被誤解。

最後補一件第 5 天文章沒講到的事。Messages API 一次回應確實可以帶好幾個 `tool_use` 區塊，模型可以一口氣說「我要改 A 檔，也要改 B 檔」。但 KeSi 目前在處理這塊是用 `for block in resp.content` 迴圈寫的，這會跑完一個才跑下一個，沒有「同時」進行這回事。

而執行的順序是有差的，假設模型在同一份回應裡對同一個檔案提了兩個 edit，第二個 `edit_file` 讀到的已經是第一個改完之後的檔案。如果第一個 edit 讓第二個 `old_string` 消失或不再唯一，第二次替換就會失敗；光是兩段內容重疊，還不能斷定一定會失敗。就算改的是不同檔案，能一個一個改完，也不代表它們可以分開看，像是改了 schema，呼叫它的地方通常也要跟著改，只改一邊，跑起來大概就不對了。

將來真要讓這些工具同時跑，要處理的事比想像中多。例如同一個檔案不能讓兩邊同時寫，得有辦法讓它們排隊，而且每一次寫下去之前要再確認一次內容沒被別人動過。最麻煩的是一組修改如果要跨三個檔案，改到第二個失敗了怎麼辦，已經改好的那一個要不要退回去？這些都想清楚了，才輪得到把 `for` 換成 `asyncio.gather` 。

## 小結

今天 KeSi 拿到第一個真的會動到檔案的工具。它的重點不在字串替換，而是三個會被擋下來的情況，找不到不猜、找到好幾個不挑、輸入不合理不假裝成功。精確的 `old_string` 限制了改動範圍，解析過符號連結的路徑檢查限制了能寫的地方， `(output, is_error)` 則讓結果能正確回到 agent 迴圈裡。

不過這三條擋的都是「改錯地方」，不是「改錯內容」。新內容可能有錯，做一半出錯目前不會自動復原， `old_string` 也只表示 `edit_file` 讀到的那一版內容裡，這一小塊精確而且唯一，不能證明它從上次讀完之後都沒被動過。即使工具回報成功，這些事情之後仍然要靠測試或其他檢查驗證。

第一段工具還剩最後一個：建立新檔案。聽起來只是開檔、寫入、關檔幾個簡單動作，但要做的事可能會比你想像中的多一點。

咱們下集見:)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402109)

[

Day 08 - 在專案裡搜尋

](https://ithelp.ithome.com.tw/articles/10402109)[下一篇](https://ithelp.ithome.com.tw/articles/10402250)

[

Day 10 - write\_file 的防呆機制

](https://ithelp.ithome.com.tw/articles/10402250)

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