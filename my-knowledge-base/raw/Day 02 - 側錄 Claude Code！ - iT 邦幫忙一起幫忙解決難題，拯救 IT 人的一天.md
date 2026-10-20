---
title: "Day 02 - 側錄 Claude Code！ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401028"
author:
  - "[[iThome]]"
published:
created: 2026-08-03
description: "文章同步發表在我的個人 Blog Claude Code 本身沒有開源，我們一般人應該看不到它的原始碼，那要怎麼知道它到底送了什麼給模型？可以翻文件也可以用猜的，但還有一個更直接的辦法，就是把它丟給模..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 2



[Software Development](https://ithelp.ithome.com.tw/2026ironman/software-development)

### 為你自己手刻 Claude Code系列 第 2 篇

## Day 02 - 側錄 Claude Code！

- 分享至

> 文章同步發表在 [我的個人 Blog](https://kaochenlong.com/)

Claude Code 本身沒有開源，我們一般人應該看不到它的原始碼，那要怎麼知道它到底送了什麼給模型？可以翻文件也可以用猜的，但還有一個更直接的辦法，就是把它丟給模型的那包東西抓下來看是怎麼回事。

## 就只是一個 HTTP 請求

Claude Code 的模型們，像是 Haiku、Sonnet、Opus 以及不久前被關起來又被放出來的 Fable，這些都不在你的電腦裡，這代表 Claude Code 如果要使用這些模型的話就得透過網路把 HTTP 請求送到 Anthropic 的伺服器。這也不是什麼神秘的事情，基本上跟你每天用瀏覽器逛網頁的那個 HTTP 是差不多的概念。

只要是走 HTTP 的東西，理論上就有辦法在半路把它攔下來。這件事聽起來很像駭客在做的事，但其實 Anthropic 有留一條路給我們，我們連駭都不用駭。

Claude Code 以及之後我們會用到的官方 SDK，在決定「要把請求送去哪裡」的時候會先看一個名為 `ANTHROPIC_BASE_URL` 的環境變數。這個環境變數如果沒特別設定的話預設就是送到官方的網址，但如果有另外設定，它就乖乖送到你指定的地方。

這條路本來是設計給企業用的，有些公司基於資安或稽核的需求會要求所有對外的 API 請求都要先經過公司自己的閘道， `ANTHROPIC_BASE_URL` 就是為了這種場景留的。但我們也可以借來用，我要做的是把 `ANTHROPIC_BASE_URL` 指向我自己電腦上的一個小程式。這樣 Claude Code 以為它在跟 Anthropic 講話，但其實它是先把整包東西交到我手上，我看完再幫它轉交出去，順便把人家的回覆也帶回來給它，全程不會發現有人在中間偷看。

這種「站在中間偷看」的小程式，有個名字叫 proxy。不難寫，我們現在就來寫一個。

## 開工前先裝個 uv

這個系列的程式我都用 Python 寫，動手之前先把工具準備好。再賣一下藥，如果對 Python 有興趣，歡迎參閱《 [為你自己學 Python](https://pythonbook.cc/) 》這本書。

我不打算用傳統的虛擬環境，不是不好用，是它每次都要你先想我現在人在哪個環境裡，一不小心套件就裝到系統的 Python 去了，這個坑我踩過很多次。所以現在我的 Python 專案的起手式都是 [uv](https://docs.astral.sh/uv/) ，這是一個用 Rust 寫的 Python 套件與專案管理工具，「用哪個版本的 Python」跟「裝哪些套件」這些事都可以交給它。安裝方式官網 [寫得很清楚](https://docs.astral.sh/uv/getting-started/installation/) ，裝完在終端機打 `uv --version` ，有跳版本號就成了。

## 可以看資料的 proxy

這個 proxy 要做三件事：

1. 收下 Claude Code 送來的請求
2. 把請求的內容存成檔案
3. 最後再原封不動轉交給真正的 Anthropic，把回應送回去

三件事，用 Python 的標準函式庫就能搞定，一個第三方套件都不用裝：

```python
# /// script
# requires-python = ">=3.14"
# dependencies = []
# ///

from http.server import BaseHTTPRequestHandler, ThreadingHTTPServer
import http.client
import json
import os

seq = 0

class Tap(BaseHTTPRequestHandler):
    def do_POST(self):
        global seq

        # 1. 收下請求
        length = int(self.headers.get("Content-Length", 0))
        body = self.rfile.read(length)

        # 2. 存成檔案，這包東西太大，印在畫面上根本看不完
        seq += 1
        name = f"captured/request-{seq}.json"
        secret_headers = {"authorization", "x-api-key", "anthropic-oauth-token"}
        saved_headers = {
            k: "<已遮蔽>" if k.lower() in secret_headers else v
            for k, v in self.headers.items()
        }
        record = {"method": self.command, "path": self.path,
                  "headers": saved_headers, "body": json.loads(body)}
        with open(name, "w", encoding="utf-8") as f:
            json.dump(record, f, ensure_ascii=False, indent=2)
        print("攔到一包，存到", name)

        # 3. 原封不動轉交給真正的 Anthropic
        conn = http.client.HTTPSConnection("api.anthropic.com")
        headers = {k: v for k, v in self.headers.items() if k.lower() != "host"}
        conn.request("POST", self.path, body, headers)
        resp = conn.getresponse()

        # 再把回應送回給 Claude Code
        self.send_response(resp.status)
        for k, v in resp.getheaders():
            if k.lower() not in ("transfer-encoding", "connection"):
                self.send_header(k, v)
        self.end_headers()
        self.wfile.write(resp.read())

os.makedirs("captured", exist_ok=True)
print("proxy 已啟動，監聽 127.0.0.1:9527，按 Ctrl-C 結束")
ThreadingHTTPServer(("127.0.0.1", 9527), Tap).serve_forever()
```

來解釋上面這些程式碼是什麼用途，最上面那個 `seq` 是個計數器，等一下存檔要用它編號。再往下，最下面那行 `ThreadingHTTPServer` ，是在你自己的電腦上開了一個門牌號碼 `9527` 的小服務，讓它一直蹲在那邊等人上門。 `127.0.0.1` 是「你自己這台電腦」的意思，代表這個服務只有你自己連得到。它上面那行 `print` 是印給你自己看的，因為這支程式跑起來之後，在沒人上門之前畫面上完全不會有動靜，少了這行你會不確定它到底活著沒。中間那個 `do_POST` ，是「有人用 POST 方法上門的時候，你要做什麼」，因為 Claude Code 送請求就是用 POST，所以這裡只要處理這個就好。

為什麼是 `9527` 這個號碼？沒什麼原因，就只是個人偏好而已:)

進到 `do_POST` 裡面，第一步先看 `Content-Length` 這個標頭，這會告訴我們這封信的內容有多長，我們才知道要讀多少。讀出來的 `body` ，就是請求的主體，也就是等一下的重頭戲。

第二步是把攔到的東西存起來，這一步是整個 proxy 的目的所在，其他都是配角。

我本來想直接 `print` 到畫面上就好，跑一次就打消念頭了。這一包東西大得嚇人。我攔到的這一筆，光是 `system` 那份行為守則就有兩萬八千多個字元， `tools` 裡面塞了 83 個工具、加起來十五萬字元出頭，整包 JSON 二十萬字元，存成檔案是 237 KB。這種份量印在終端機上，你只會看到它嘩啦啦捲過去，捲完什麼都抓不到。存成檔案，再用編輯器慢慢翻，才有辦法看。

所以我用了一個變數 `seq` 當計數器，每攔到一包就存成 `request-1.json` 、 `request-2.json` 這樣，畫面上只印一行短短的提示，告訴你又進來一筆。

這些檔案我沒有直接丟在工作目錄下，而是集中放進 `captured` 這個資料夾。一來跑久了會累積不少檔案，跟程式碼放在同一個目錄裡有點礙眼；二來這些檔案裡有你自己的對話內容，集中在一個資料夾，要在 `.gitignore` 裡擋掉也方便，一行就解決。最下面那行 `os.makedirs` 是開機時把資料夾準備好， `exist_ok=True` 的意思是「已經有了就別吵」。

存進去的東西有四塊， `method` 跟 `path` 是這封信寄去哪、 `headers` 是信封、 `body` 是信的內容，等一下我們都要看。我在存檔前把 `Authorization` 、 `X-Api-Key` 和 `anthropic-oauth-token` 這三種可能裝著憑證的標頭都換成 `<已遮蔽>` 。HTTP 標頭名稱不分大小寫，所以程式先用 `k.lower()` 轉成小寫再比對，避免只因為大小寫不同就漏掉。

提醒一下，不同的客戶端，可能會把憑證放在不同的標頭裡。例如 Claude Code 走登入拿到的憑證是放在 `Authorization` ，而我們自己寫程式串接官方 SDK 加自己辦的金鑰去打的，它的標題是 `X-Api-Key` 。所以這裡我用了一份清單掃過每個標頭，就不只針對某一個名字去做馬賽克。

我一開始就是只寫了一行「把 `Authorization` 換成已遮蔽」，攔 Claude Code 的時候看起來一切正常，換成自己的程式一跑，金鑰就明文躺在檔案裡了。更陰險的是，那行程式在請求沒有 `Authorization` 的時候，會自己生一個出來，於是檔案裡明明白白寫著 `Authorization: <已遮蔽>` ，讓你以為遮好了，真金鑰卻在上面幾行。

還有一點，`.gitignore` 是要避免「不小心 commit 出去」而不是這個檔案很安全。被 ignored 的檔案還是一份明文躺在你硬碟上的東西，裡面有你的對話內容。要貼給別人看、要丟進 issue、要當作教材截圖之前，自己再看一眼。

最後那個 `ensure_ascii=False` 別漏掉，不然中文會被存成一堆看不懂的跳脫字元。

第三步是轉交。我們開一條線到真正的 `api.anthropic.com` ，把剛剛收到的東西原封不動送過去。這裡有個小地方要注意，就是那行把 `host` 濾掉的程式碼，因為請求裡原本的門牌寫的是我的電腦，我得把它拿掉，讓底層自己填上正確的官方門牌，不然這封信會寄丟。

最後把 Anthropic 回來的東西送回去給 Claude Code。這一段看起來只是照抄，但這裡幾個坑要注意：

首先，是標頭也要跟著抄。Anthropic 傳回來的內容是壓縮過的，如果你只把狀態碼送回去、沒把 `Content-Encoding` 這類標頭一起帶上，Claude Code 收到一包壓縮資料卻不知道它是壓縮的，就會跟你抱怨回應是空的或格式不對。所以我們把上游的標頭抄一份過去。

不過有兩個標頭不能照抄， `Transfer-Encoding` 講的是上游那段連線怎麼切包送，我們這段是另一條連線，得自己決定。另一個標頭是 `Connection` ，上游說「這條連線留著別關」，可是我們既沒告訴 Claude Code 內容有多長、也沒說要分段送，它只能靠連線關閉來判斷「講完了」，你卻叫它繼續等，它就會一直等下去然後就整個卡住。要把這兩個濾掉，其他的照抄就好。

這是一個教學用的精簡版，真正能穩定跑的完整版還要處理串流回應之類的細節。上面的程式碼我放在這個系列的 repo 裡，有興趣可以去翻一下，不過重點就是上面這二十幾行，沒什麼秘密。

GitHub: [https://github.com/kaochenlong/KeSi](https://github.com/kaochenlong/KeSi)

## 咦，HTTPS 不是有加密嗎？

你可能有聽過 HTTPS 是有加密的，而 Anthropic 的連線是走 HTTPS 的，這樣怎麼可能看得到裡面的明文？

你注意看，我等一下設定 `ANTHROPIC_BASE_URL` 的時候，用的是 `http://127.0.0.1:9527` ，開頭是 `http` ，不是 `https` 。也就是說，Claude Code 送到我這個 proxy 的這一段路，是沒有加密的明文。加密這件事，是發生在我的 proxy 轉交給 `api.anthropic.com` 的那一段，也就是程式碼裡 `HTTPSConnection` 負責的地方。

換句話說，我們並沒有破解任何加密，Claude Code 把明文交給我，我看完之後才幫它加密送出去。這就是為什麼中間人這種手法攔的是自己主動導流過來的流量，而不是去破解別人的連線。

## 跑起來，叫它講句話

開個終端機執行：

```markdown
$ uv run proxy.py
proxy 已啟動，監聽 127.0.0.1:9527，按 Ctrl-C 結束
```

如果看到這行就代表它起來了，在有人上門之前畫面不會再有任何動靜。接著，我再開一個終端機視窗，這次的重點是那個環境變數，這裡我要叫 Claude Code 不要把資料送去官方伺服，而是改送到我的 proxy：

```markdown
$ ANTHROPIC_BASE_URL=http://127.0.0.1:9527 claude -p "說一句話就好" --model haiku
```

前面那段 `ANTHROPIC_BASE_URL=...`，是指在單次指令有效的環境變數設定，我不想污染整個系統的設定，所以就寫在指令前面，跑完就沒了。

中間的 `claude -p` ，是不進入互動介面、直接給一句話就跑的模式， `-p` 是 print 的意思，我故意挑了一個最無聊的任務，只要它有反應、有送出請求就夠了。

最後的 `--model haiku` ，是指定用個便宜的模型來做事，畢竟我只是要看它送什麼，沒必要用貴的模型花大錢。執行後 Enter，Claude Code 那邊很快就回了一句話給我：

> 「我準備好了，告訴我需要什麼幫助。」

你不一定會看到跟我一樣的回應。切回 proxy 的視窗，它多了幾行：

```markdown
127.0.0.1 - - [02/Aug/2026 11:44:07] code 501, message Unsupported method ('HEAD')
127.0.0.1 - - [02/Aug/2026 11:44:07] "HEAD /api/hello HTTP/1.1" 501 -
攔到一包，存到 captured/request-1.json
127.0.0.1 - - [02/Aug/2026 11:44:11] "POST /v1/messages?beta=true HTTP/1.1" 200 -
```

東西到手了，就在 `captured/request-1.json` 這個檔案裡。

那行 `code 501, message Unsupported method ('HEAD')` 先別理它，那是 Claude Code 出手之前先用 `HEAD` 探一下路，但我們的 proxy 只寫了處理 `POST` 的部分。但探不到路也會照樣把請求送出來，不影響我們要看的東西，所以我就不多寫程式去接它了（你想處理也是可以）。

如果你第一次在某個資料夾跑 Claude Code，它可能會先問你要不要信任這個資料夾，跟著按確認就好。另外如果它抱怨 9527 這個號碼被占用了，換一個數字就好，比如 8787，記得 proxy 跟指令兩邊都要改成一樣的。

另外提一句，proxy 不只看得到請求，模型的回應也是原路經過它的。你要是好奇模型回的東西長什麼格式，也可以如法炮製，在轉發回去之前把回應也印一份出來。這篇我們先專心看請求這一半，回應的格式等之後我們自己收到的時候再細看。

## 先看信封

我用編輯器打開 `captured/request-1.json` 。二十幾萬字元躺在那邊，不可能一口氣看完，所以我一塊一塊帶大家看。先看寄件資訊跟 `headers` 那幾塊，也就是這封信的信封：

```markdown
POST /v1/messages?beta=true
Authorization: <已遮蔽>
User-Agent: claude-cli/2.1.220 (external, sdk-cli)
anthropic-version: 2023-06-01
anthropic-beta: oauth-2025-04-20,interleaved-thinking-2025-05-14,
    thinking-token-count-2026-05-13,context-management-2025-06-27,
    prompt-caching-scope-2026-01-05,claude-code-20250219,
    advisor-tool-2026-03-01,extended-cache-ttl-2025-04-11
x-app: cli
anthropic-dangerous-direct-browser-access: true
Accept-Encoding: gzip, deflate, br, zstd
Content-Length: 199251
```

這是我挑出來的幾行，實際上還有一票 `X-Stainless-` 開頭的標頭，那是官方 SDK 自己加的，會回報跑在什麼作業系統、什麼語言、SDK 版本幾號，跟我們要看的東西無關，先跳過。

這包東西裡有不少東西可以講，最上面那行 `POST /v1/messages` ，是這封信要寄到的地址。所有跟模型對話的請求，都是寄到 `/v1/messages` 這個地址，記住它，因為明天我們自己的程式也是打到這裡。後面那個 `?beta=true` 是在說「我要用一些測試中的新功能」，等一下信封裡那串 beta 標記會呼應到它。

`Authorization` 那行帶的是一串憑證，證明「我有權使用這個服務」。這裡會看到 `<已遮蔽>` 是因為我的 proxy 存檔前就把它換掉了。這是身分認證，形同你的通行證，外流會被人盜用。我們的 KeSi 之後也得帶上自己的憑證，模型才會理它。

`User-Agent` 這行滿好玩的，它乖乖的說自己是誰。你看， `claude-cli/2.1.220 (external, sdk-cli)` ，它把自己是 Claude Code、版本幾號、走哪條路來的都寫在這裡了，伺服器那邊就是靠這個知道來訪的是哪個程式。你攔到的版本號有可能會比我新一點，因為 Claude Code 更新得很勤。

`anthropic-version` 是 API 的版本，這行是在說「請用這個版本的規則來理解我這封信」，避免哪天官方改了東西，舊程式突然就壞掉。

最後那個落落長的 `anthropic-beta` ，每個用逗號隔開的字串都是一個 Claude Code 正在使用的進階功能，我這次攔到八個，像是 `context-management` 是在管理對話的長度、 `prompt-caching-scope` 跟快取有關、 `extended-cache-ttl` 是延長快取能撐多久、 `thinking-token-count` 跟計算思考用掉多少 token 有關。context 管理跟快取是這系列的重頭戲，這一行等於先幫我把後半段要做的事列出來了。

## 拆開那封信

信封講完，來看裡面那封信，也就是請求的主體。這是一包 JSON，最外層的骨架大概像這樣：

```json
{
  "model": "claude-haiku-4-5-20251001",
  "messages": [ { "role": "user", "content": [...] } ],
  "system": [ ...三個區塊... ],
  "tools": [ ...83 個... ],
  "metadata": { "user_id": "..." },
  "max_tokens": 32000,
  "thinking": { "budget_tokens": 31999, "type": "enabled", "display": "omitted" },
  "context_management": { "edits": [ { "type": "clear_thinking_20251015", "keep": "all" } ] },
  "stream": true
}
```

`model` ，這次要用哪一顆腦，就是我指定的那個 haiku，完整型號寫在這裡。

`max_tokens` ，這次最多讓模型講多長的話。這裡設 32000，是一個上限，不是它一定會講這麼多。這個數字設太小會有風險，它話講到一半就被切斷，這個之後會遇到。

`stream` 設成 `true` ，是要求模型一個字一個字吐出來，而不是整段想完才一次給。這就是為什麼你看 Claude Code 回話是逐字跳出來的，這塊我們後面會專門處理。

`thinking` ，是要不要讓模型「先想再答」。你看它給了一個 `budget_tokens` ，是在說「你最多可以花這麼多力氣去想」。

`context_management` ，就是前面信封裡呼應到的那個功能，管理對話長度用的。它跟 `messages` 是一對，一個裝對話、一個管對話。我這次攔到的內容是一條 `clear_thinking` 的規則，不過它的 `keep` 設成 `all` ，意思是「所有 thinking 都保留」，這次其實沒有清掉任何內容。名字裡雖然有 `clear` ，真正要看的是後面的保留策略。這還是看得出 Claude Code 已經接上 context management，至於什麼時候真的清、怎麼清，我們後面再處理。

`metadata` 裡面放的是裝置識別碼、帳號代號、session 代號這類東西，是拿來做統計跟辨識用的，跟對話內容本身無關，先不管它。

剩下 `system` 、 `tools` 、 `messages` 這三塊比較大，待會再拉出來個別看。是說，有沒有覺得這幾個欄位，有點眼熟？

## 四個詞，全在這封信裡

agent 就四個詞：模型、迴圈、工具、context。這封真實的信裡，四個全都在。

先看模型。第一行 `model` 明明白白寫著它這次要用哪一顆腦。

再看工具，也就是 `tools` 那個陣列，裡面裝了 83 個工具的定義，這一整包就是模型的手腳。順帶一提，這個數量會因為你的設定而不同，我這台機器裝了一些額外的東西，所以攔到的清單比預設的長，你攔自己的 Claude Code 看到的是你自己的工具組。是說你有發現嗎？工具是「連同這次請求一起送過去」的，不是模型自己內建的。這很重要，這代表工具是我們這邊定義而且是由我們這邊送過去的，等一下我把其中一個工具拆開來給你看，你就會知道什麼叫做「跟 AI 許願」了。

接著看 context。你看 `messages` ，這一次因為對話才剛開始，裡面只有一則我剛剛講的話，但你可以想像，如果我跟它聊了二十輪，這個陣列就會塞滿二十輪的來回。那本要重新念給模型聽的日記，就是這個 `messages` 陣列，它會隨著對話越變越長。

最後是迴圈。迴圈比較特別，它不在這一包請求裡，因為迴圈是 Claude Code 這個程式的「行為」，不是送給模型的資料。這一整包東西被送出去然後拿回一個「我想用某個工具」的回應，接著 Claude Code 執行完再把結果塞進 `messages` 陣列、最後把整包再送一次，這個「送出、拿回、再送出」的節奏就是迴圈。我們攔到的，是迴圈轉某一圈的時候，手上正拿著的那包東西。

我們接下來 30 天要親手打造的東西，它的規格就長這個樣子。

## 工具長什麼樣子？

`tools` 是一個陣列，裡面每一個工具，都是一個像這樣的物件。我拿一個最單純的「讀檔」工具當例子，它大概長這樣：

```json
{
  "name": "Read",
  "description": "Reads a file from the local filesystem. You can access any file directly by using this tool.\nAssume this tool is able to read all files on the machine. ...（這段說明書總共 1782 個字元，我截掉後面）",
  "input_schema": {
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "type": "object",
    "properties": {
      "file_path": {
        "description": "The absolute path to the file to read",
        "type": "string"
      },
      "offset": {
        "description": "The line number to start reading from. Only provide if the file is too large to read at once",
        "type": "integer"
      },
      "limit": { "...": "還有 limit 跟 pages 兩個參數，格式差不多" }
    },
    "required": ["file_path"],
    "additionalProperties": false
  }
}
```

這三個欄位，就是一個工具的全部。

`name` 是工具的名字，模型會用這個名字來「點名」它想用哪個工具。

`description` 是用白話寫給模型看的說明書，告訴它這個工具是幹嘛的以及什麼時候該用。這一段很重要，因為寫得好不好會直接影響模型會不會在對的時機想到要用它。你看正版的 Claude Code 多捨得下工夫，光是「讀一個檔案」這種聽起來理所當然的事，說明書就寫了一千七百多個字元，連「路徑要給絕對路徑」「檔案不存在也沒關係，會回一個錯誤給你」這種細節都交代了。

`input_schema` 是這個工具需要哪些參數。以讀檔來說，必填的只有 `file_path` ，型別是字串，也就是要讀哪個檔案的路徑。另外還有 `offset` 、 `limit` 、 `pages` 三個選填的，是給大檔案跟 PDF 用的，注意每個參數自己也帶了一句 `description` ，一樣是寫給模型看的。

所以「跟 AI 許願」這件事，例如模型想讀檔的時候，它不會自己去開檔案，它會照著這張 schema，吐出一段像「我要用 `Read` ， `file_path` 是 `/path/to/project/main.py` 」的資料，真正拿著這個路徑去開檔案的，是我們的程式。模型只是照著我們給的表格，填了一張申請單而已。

## 華麗的 system

`system` 藏了很多好玩的東西，值得單獨拉出來講。

它是我們給模型的「行為守則」，告訴它「你是誰、你該怎麼做事」。Claude Code 送出去的這份守則，大約有兩萬八千個字元。這是什麼概念？它光是「交代模型該怎麼當一個 coding agent」的說明書，內容就比你現在正在讀的這篇文章還長。裡面會有它該遵守的規矩、遇到各種狀況該怎麼處理、講話的風格、什麼事不能做，鉅細靡遺。

agent 的行為好不好，很大一部分是這份守則寫得好不好。另一個更有趣的是，這份 `system` 被拆成三個區塊，而且我發現後面兩個區塊各自帶了一個叫 `cache_control` 的小標記。這個標記是在跟伺服器說：「這一段內容很固定，每次都一樣，你可以先把它快取起來，下次我再送同樣的東西，你就不用重算，算我便宜一點。」

如果每一輪對話都要讓模型重讀一次就要重新付一次錢也太傷本了，所以 Claude Code 的做法是把這種每次都一樣的大塊頭標記起來快取，只有真正變動的部分才付全額。以第一篇提到的費率來算，命中快取的那段 input，價格只有原價的十分之一，守則越長、對話越多輪，省下來的越可觀。

這一個小小的標記之後會有一整篇專門在講關於怎麼省錢，我們現在先在正版身上看到它長什麼樣子，之後再自己動手做。

## 小結

我們今天做的側錄，攔的是自己電腦上、自己登入的 Claude Code，這完全沒問題，就像你拆自己買的玩具一樣。但這套手法別亂用，去攔別人的流量、去偷看不屬於你的憑證，那是另一回事。我們是為了學習拆自己的東西，分寸要拿捏好。

另外，那份 `system` 守則是 Anthropic 花了很多心血寫的內部內容。我們攔下來，看它的結構、學它的設計，這沒問題，但我不會把整份逐字貼出來到處散布，那不太厚道。看門道，不是搬走人家的東西。這個側錄的手法，是靠 `ANTHROPIC_BASE_URL` 的設定。這條路是官方支援的，短時間內應該不會關掉，但 Claude Code 一直在更新，哪天它換了做法，這招有可能就不靈了。我寫這篇的時候實測是通的，如果你照著做卻攔不到東西，別擔心，大概是版本或設定的差異。

萬一真的這條路被關了也沒關係，還有備案。 [Messages API 的官方文件](https://platform.claude.com/docs/en/api/messages) 本身就把請求的格式寫得清清楚楚，我們攔到的那些欄位， `model` 、 `messages` 、 `system` 、 `tools` 、 `max_tokens` 、 `stream` ，在文件上每個都查得到。看不到活的請求，看文件也可以。側錄只是比較有臨場感的一條路，不是唯一的路。

是說，這包東西是 Claude Code 在你按下 Enter 的那一刻，臨時把三樣東西湊在一起然後組成這一包 JSON，裡面有：

- 內建的系統守則，也就是 `system`
- 支援的工具清單，也就是 `tools`
- 還有你剛剛講的那句話，也就是 `messages`

組好，貼上信封，寄出去。其實我們這 30 天要做的就是學會當這個「組信的人」。Claude Code 組的是豪華全配版，長長的守則、幾十個工具；我們明天要組的是最陽春的版本，沒有守則、沒有工具，只有 `model` 、 `max_tokens` 跟一句話的 `messages` 。就算陽春，也是一封合法的、寄得出去、收得到回覆的信。

今天我們還沒開始寫 KeSi 的程式碼，但做了一件更重要的事，我們看到了目標長什麼樣子。

一個地址、一把憑證、還有那包裝著 `model` 、 `max_tokens` 、 `system` 、 `tools` 、 `messages` 的 JSON，這就是我們要親手打造的東西的完整規格。之後每一天，你都可以回頭對照這封信，看看我們又補上了哪一塊。下一集，我們就自己寫程式，打出這封信裡最陽春的一個版本，只有 `model` 、 `max_tokens` 、一句話的 `messages` ，別的都先不管，送出去，然後看模型回話。

咱們下集見:)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10400753)

[

Day 01 - Claude Code 其實就只是一個 while 迴圈

](https://ithelp.ithome.com.tw/articles/10400753)[下一篇](https://ithelp.ithome.com.tw/articles/10401069)

[

Day 03 - 第一個 API 請求

](https://ithelp.ithome.com.tw/articles/10401069)

系列文

[為你自己手刻 Claude Code](https://ithelp.ithome.com.tw/users/20065770/ironman/9031) 共 3 篇

目錄

1. 1
	[Day 01 - Claude Code 其實就只是一個 while 迴圈](https://ithelp.ithome.com.tw/articles/10400753)
2. 2
	[Day 02 - 側錄 Claude Code！](https://ithelp.ithome.com.tw/articles/10401028)
3. 3
	[Day 03 - 第一個 API 請求](https://ithelp.ithome.com.tw/articles/10401069)
[完整目錄](https://ithelp.ithome.com.tw/users/20065770/ironman/9031)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/10/5a8abfe5-156d-4f08-a876-62c23d668038_small.png)

[Flowmon - 結合MITRE 與 Netflow 達成CP值最高的資安事件應變手段](https://itplus.ithome.com.tw/vod-page/489?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/21/2a8be1d3-b145-48c6-9f47-81bd4605bb02.png)

[Implement Engineering Operations Dashboard with Faros CE](https://itplus.ithome.com.tw/vod-page/786?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/ead58431-65f0-494c-b0bd-a95f3d606b84_small.png)

[導入 OPA 在金融業監控 k8s 部署檔的安全性](https://itplus.ithome.com.tw/vod-page/1054?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/25/4fc60419-2101-4dff-9d0c-aa8ec1053505_small.png)

[從制品管理到供應鏈防護：用 JFrog 打造現代 DevSecOps 流程](https://itplus.ithome.com.tw/vod-page/1157?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

37 分