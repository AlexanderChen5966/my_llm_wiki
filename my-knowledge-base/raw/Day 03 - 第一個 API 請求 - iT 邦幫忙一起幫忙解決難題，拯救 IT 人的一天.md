---
title: "Day 03 - 第一個 API 請求 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401069"
author:
  - "[[iThome]]"
published:
created: 2026-08-03
description: "文章同步發表在我的個人 Blog 今天換我們自己當那個「組信的人」，親手組一封陽春版的信寄給模型。前一篇文章裡攔下來的那封信有兩萬八千字元的守則以及 83 個工具我全部都先不要，只先留最核心的三樣：m..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 3

0

[Software Development](https://ithelp.ithome.com.tw/2026ironman/software-development)

### 為你自己手刻 Claude Code系列 第 3 篇

## Day 03 - 第一個 API 請求

- 分享至

> 文章同步發表在 [我的個人 Blog](https://kaochenlong.com/)

今天換我們自己當那個「組信的人」，親手組一封陽春版的信寄給模型。前一篇文章裡攔下來的那封信有兩萬八千字元的守則以及 83 個工具我全部都先不要，只先留最核心的三樣： `model` 、 `max_tokens` ，還有裝著一句話的 `messages` 。寄出去，然後看看模型會回什麼。

## 先辦一把自己的 API Key

側錄的時候我們借了 Claude Code 的登入，不用自己的金鑰，但今天不行了。因為今天開始是用我們自己寫的程式直接去跟模型講話，不是借誰的手。伺服器不認得我們所以得自己出示證件，也就是 API 金鑰。

金鑰要去 Anthropic 的 [Claude Console](https://platform.claude.com/) 申請：

![https://ithelp.ithome.com.tw/upload/images/20260802/20065770KituWcdWAd.png](https://ithelp.ithome.com.tw/upload/images/20260802/20065770KituWcdWAd.png)

流程不複雜，大概就是登入帳號、建立一把新的金鑰然後複製起來。但是...等等，我不是有付 Claude 訂閱了嗎？而且還是 200 元美金的 Max 訂閱耶！不是的，這是兩個錢包，分開算的。

訂閱付的是「人去用官方的介面」的錢，像是 claude.ai 跟 Claude Code。API 付的是「你的程式去打 API」的錢，走的是 Claude Console 底下的 API 帳戶。同一個 email 可以同時有這兩邊的帳號，但它們是各算各的，訂閱不會附贈任何 API 額度，就算我現在是 Anthropic 的社群大使也一樣要乖乖付錢。

你第一次去辦金鑰的時候大概會被要求綁一張卡或先儲值一小筆金額才能開始用，我就先儲值個 5 美金。在上一篇文章裡可以不設定金鑰就攔到請求是因為那時候是 Claude Code 拿它登入後的憑證去打，用的是訂閱的錢包。今天換我們自己的程式上場，就得用 API 那個錢包了。

## 重要提醒

首先，這串東西等於你的錢錢，記得小心保管。隨著 Vibe Coding 大流行，看過不少案例是把金鑰 commit 進公開的 git repo，結果就被機器人掃到然後拿去狂打，帳單直接爆掉。所以請記得不要把金鑰貼到公開的地方，也別 commit 進 git 或是寫死在程式碼裡。

其次，雖然使用 API 是另外花錢的，但我都會用最便宜的模型，一次請求大概就零點幾分錢的等級，要故意燒超過一塊美金可能都有點難度。真正會讓帳單變可觀的是對話變長之後的事，那個我們會再另外專門處理。

重要的事要講三次：

- 別把金鑰寫進程式碼！
- 別把金鑰寫進程式碼！
- 別把金鑰寫進程式碼！

寫進去它就跟著程式碼，你一 commit 就等於金鑰外流了。正確的做法是讓它待在程式碼外面，程式再從環境變數去拿。簡單的做法是在終端機下這一行：

```markdown
$ export ANTHROPIC_API_KEY="你剛剛複製的那串金鑰"
```

這樣金鑰只活在目前這個終端機的環境裡，關掉就沒了。不過你剛剛打的整行指令還是可能被 Shell 記進歷史檔，所以「環境變數會消失」不代表金鑰完全不會被寫到磁碟。如果不想每開一個新的終端機視窗都要重設一次，可以放到你的 Shell 的設定檔（例如 `~/.zshrc` 之類的檔案），或是另外在專案裡放一個 `.env` ，把金鑰寫在裡面：

```markdown
ANTHROPIC_API_KEY=你剛剛複製的那串金鑰
```

然後第一件事就是把它加進 `.gitignore` ，這樣它只會留在你自己的電腦上：

```markdown
.env
```

`.env` 不是什麼魔法檔案，你把金鑰寫進去程式並不會自動就讀得到，那就只是一個純文字檔，得有人把它讀進環境變數才算數，Python 的 `os.environ` 不會自己去翻它。這件事交給 uv 做就好，它內建有支援：

```markdown
$ uv run --env-file .env kesi.py
```

`--env-file` 會先把 `.env` 裡的東西塞進環境變數，再去跑你的程式，不用多裝任何套件。如果你嫌每次都要打這個參數很煩，也可以裝 `python-dotenv` 套件來做類似的事，但代價是多一個相依套件。

最後我會放一個 `.env.example` ，這個檔案跟 `.env` 剛好相反，這個檔案是要進版控的：

```markdown
ANTHROPIC_API_KEY=
```

在這個檔案裡不會放真的金鑰，只放欄位名字或是最多再放個假的範例金鑰格式。它的作用是告訴其他人或是三個月後的自己，這個專案需要哪些秘密。別人把 repo 抓下來，把 `.env.example` 檔案複製一份成 `.env` 再把值填進去就能跑，不用回頭問你需要什麼設定，這是個很小但體貼且常見的習慣。

## 使用 Anthropic 官方 SDK

寫 proxy 那次，我們是自己用 Python 標準庫硬幹 HTTP。今天開始跟模型正式往來，我改用 Anthropic 官方的 Python SDK，也就是 `anthropic` 這個套件。不是說「自己做」嗎，怎麼這裡又用人家的套件了？

這個系列一開始就提過，我們要自己寫的是構成 agent 的骨架。至於那些跟 agent 本身無關、重造輪子只會浪費時間跟 AI 算力的東西，就交給現成套件處理，HTTP 就屬於這一類。你以為「送一個請求出去、把回應收回來」很單純，其實細節多得很。網路斷了要不要重試？重試幾次？遇到伺服器忙碌回你「稍後再試」該等多久？回應是一個字一個字串流回來的，你要怎麼把它們接起來？金鑰要放在哪個標頭、版本號要怎麼帶？這些又雜又容易出錯，而且跟「agent 之所以是 agent」一點關係都沒有。

官方的 SDK 把這些髒活都處理好了，它幫我們處理連線、自動重試、逾時，還把那包 JSON 變成好用的 Python 物件，讓我們不用自己去解析一堆字串。我們站在它上面，專心處理真正是重點的迴圈跟工具就好。

## 第一次 HTTP 請求

好，鑰匙有了，來寫 KeSi 的第一段程式碼。我存成 `kesi.py` ：

```python
# /// script
# requires-python = ">=3.14"
# dependencies = ["anthropic"]
# ///

import anthropic

client = anthropic.Anthropic()

resp = client.messages.create(
    model="claude-haiku-4-5",
    max_tokens=1024,
    messages=[{"role": "user", "content": "用一句話介紹誰是高見龍"}],
)

for block in resp.content:
    if block.type == "text":
        print(block.text)
```

最上面的幾行跟上一篇的 proxy 是一樣的東西，差別是 `dependencies` 這次填了 `anthropic` 。這一行你可以自己打，也可以叫 uv 幫你寫：

```markdown
$ uv add --script kesi.py anthropic
```

這個指令會直接改你的 `kesi.py` 的內容，把套件名字補進那個區塊裡。之後執行 `uv run` 的時候，該裝的它自己會裝。

接著 `import anthropic` ，把官方套件請進來。再來 `client = anthropic.Anthropic()` ，建立一個「客戶端」，這是我們跟模型往來的窗口。這裡面是空的也不需要把金鑰寫在這裡，因為這個客戶端夠聰明會自己去環境變數 `ANTHROPIC_API_KEY` 裡找。這就是為什麼剛剛我們用環境變數設金鑰，程式碼裡才能乾乾淨淨，一個字的金鑰都不用出現。

中間那段 `client.messages.create(...)` ，是整段的核心，我們在這裡「組裝信件」。你看它要填的三個參數是不是很眼熟？

`model` ，我選了最便宜的 haiku。你會發現我這裡寫的是 `claude-haiku-4-5` ，但上一篇攔到的是 `claude-haiku-4-5-20251001` ，後面多了一串日期。那串日期是這顆模型的確切版本，寫短的是別名，官方會幫你對到當下的版本，如果你在意「今天跑跟三個月後跑會不會不一樣」也可以把完整的模型型號寫死。

`max_tokens` 是這次最多讓它講多長。我填 1024，上一篇文章裡我們攔到的正版是 32,000，因為它要應付的是「寫一大段程式碼給你」那種大場面，我這裡只要一句話，暫時先不用開那麼大。這裡順便解釋一下 token 的定義，token 不完全等於「字數」，你可以先粗略地把它想成模型計算長度的單位，一個中文字大概是一到兩個 token。填 1024 代表「最多讓你回這麼長」，一句話綽綽有餘。

`messages` ，這是我們要跟它說的話。這是一個陣列，裡面放一個物件， `role` 是 `user` ，代表這句話是使用者說的， `content` 是我們的問題。這就是那封信裡的 `messages` 。

我們現在做的就是在正版身上看到的那封信，只是把守則跟工具都拿掉，剩下最精簡的骨架。就這一筆 `/v1/messages` 請求來看，正版跟我們的差別只是它塞的資料比較完整；Claude Code 在請求外面怎麼跑迴圈、執行工具，則是另一層的事。最後那個 `for` 迴圈，是在處理模型的回覆。有個觀念要先建立一下，很多人以為呼叫模型得到的就是一段文字，其實沒那麼單純...

## 模型回來的是什麼？

從模型拿回來的東西，是 `resp.content` ，這是一個陣列，裡面裝著一塊一塊的「內容塊」。如果我們的問題很單純，它就回一塊文字塊給我們。等之後把工具定義放進請求，模型認為要使用工具處理事情時，除了文字還可能回一塊 `tool_use` ，內容是「我想用 `Read` 這個工具」。所以回覆就被設計成「一疊積木」，好讓不同種類的東西可以混在一起傳回來。

這也是為什麼我不能直接 `print(resp)` 就了事，得用那個 `for` 迴圈，一塊一塊檢查。 `block.type == "text"` 的意思是「如果這塊是文字塊」才把它印出來。不過今天的範例應該都是文字所以看起來有點多此一舉，但這個判斷之後會變成整個迴圈的分岔路口，模型回文字我們就印、回工具請求我們就去執行。

上一篇我們只拆了請求那一半，說好回應的格式等自己收到再看，現在就是那個時候了。你可以在程式裡加一行 `print(resp)` ，把整包印出來：有 `id` 、有 `model` 告訴你它實際用了哪顆腦、有 `usage` 告訴你這次花了多少 token、還有 `stop_reason` 告訴你它為什麼停下來。這些欄位我們之後都會一個一個用到，尤其那個 `usage` ，之後幫 KeSi 裝電表的時候，數字就是從這裡來的。

## 跑跑看

我在程式碼裡問了，然後執行這行指令：

```markdown
$ uv run --env-file .env kesi.py
```

第一次跑會稍微久一點，因為 uv 要先把 `anthropic` 套件抓下來安裝。我在程式碼裡寫死我的問題「用一句話介紹誰是高見龍」，我的模型這樣回了一句話：

> 高見龍是台灣知名的程式設計師、技術作家和線上教育工作者，以《為你自己學 Ruby on Rails》等技術書籍和多個線上課程平台的教學內容而聞名。

沒意外的話，這裡就會冒出一句它對我本人的介紹。恭喜，KeSi 第一次跟遠端那顆腦袋講上話了。

## 用 proxy 看自己送了什麼

前面有提到我們自己組裝的跟正版是一樣的東西，只是把某些東西拿掉而已。空口無憑，我也開 proxy 來驗一下。前一篇文章裡的 `proxy.py` 不只可以攔 Claude Code，我們自己寫的程式也一樣攔得到，因為官方 SDK 跟 Claude Code 一樣會先看 `ANTHROPIC_BASE_URL` 。一邊讓 proxy 跑著，另一邊把我們的程式導過去：

```markdown
$ ANTHROPIC_BASE_URL=http://127.0.0.1:9527 uv run --env-file .env kesi.py
```

翻開 `captured/` 裡新出現的那個檔案，信封長這樣：

```markdown
POST /v1/messages
User-Agent: Anthropic/Python 0.120.2
anthropic-version: 2023-06-01
X-Api-Key: <已遮蔽>
X-Stainless-Lang: python
X-Stainless-Runtime: CPython
X-Stainless-Runtime-Version: 3.14.2
Content-Length: 121
```

信的內容長這樣：

```json
{
  "max_tokens": 1024,
  "messages": [{ "role": "user", "content": "用一句話介紹誰是高見龍" }],
  "model": "claude-haiku-4-5"
}
```

API 打過去的地址完全一樣，都是 `POST /v1/messages` ，只是我們沒帶 `?beta=true` ，因為我們沒用到那些測試中的功能。 `anthropic-version` 也一樣，都是 `2023-06-01` ，因為同一套 API 就得照同一套規矩報上版本。

`User-Agent` 換人了，變成官方 Python SDK 的名字跟版本。那幾個 `X-Stainless-` 開頭的，上一篇在正版身上也看得到，只是那邊寫的是 `js` 、 `node` ，我們這邊是 `python` 、 `CPython` ，連 Python 版本多少都老實招了。

上一篇 Claude Code 帶的是 `Authorization` ，因為它走的是登入拿到的憑證，而我們自己寫的帶的是 `X-Api-Key` ，因為我們走的是自己花錢錢買的金鑰。同一個 API、兩種認證方式，兩個錢包的差別在這裡就看得出來了。

真正的差距在信的內容，正版那包 `Content-Length` 是 199251，我們這包 121。9 個欄位 vs 3 個欄位，雖然欄位數量不同但一樣寄得出去也收得到回覆。接下來這個系列要做的事，就是把其他缺的欄位一個一個加回來。

## 其實大模型的記性很差

程式跑過了，你可能會很開心想再跟它多聊幾句。但對用習慣 AI 聊天的人可能會發現一個有點反直覺的事：

> 模型不會自己記住上一次 API 呼叫。

我知道這有點奇怪，你平常用 Claude Code 它明明記得你們剛剛在聊什麼啊？事實上，就 API 對話來說，模型都是金魚腦。你每呼叫它一次，對它來說都是全新的、獨立的一次，它不記得三秒前你問了什麼，甚至不知道「三秒前」這回事存不存在。

不信的話，你可以做個小實驗。把上面那段程式碼跑兩次，先跟它說「我是菜市場阿龍」，然後接著問它「我叫什麼名字」：

![https://ithelp.ithome.com.tw/upload/images/20260802/20065770ty9WC0u4uY.png](https://ithelp.ithome.com.tw/upload/images/20260802/20065770ty9WC0u4uY.png)

它會兩手一攤，跟你說它不知道你叫什麼。因為在第二次呼叫裡，我們的 `messages` 陣列只裝了「我叫什麼名字」這一句，前面那句「我是菜市場阿龍」根本沒有跟著送過去。模型收到的，就只有一句沒頭沒尾的問話，它當然答不出來。

## Messages API 是「無狀態」的

我們這裡說的「無狀態（stateless）」，不是指 Anthropic 的伺服器什麼資料都不留，而是 Messages API 不會自動替下一次呼叫接上前一次的對話。每次送出請求，模型能看到的就只有這次請求裡的內容；沒有放進 `messages` 的，它就看不到。

這聽起來有點不方便，為什麼要這樣設計？好處是每一筆推論請求都可以獨立處理，不必先去伺服器找回上一輪的對話。要延續哪些內容、拿掉哪些內容，都由發出請求的程式自己決定。

事實上你每天用瀏覽器逛網頁時走的 HTTP 協定也是 stateless 的，每個請求都得帶著處理這次事情所需的資訊。不過這不代表網站後端不能保存狀態，網站通常還是會把登入狀態存在資料庫或快取，再讓瀏覽器每次帶著 Cookie 或 token 過來；也有些 token 本身就裝著需要的狀態。

那為什麼我登入網站之後，它好像記得我是誰？

好問題，因為每次你的瀏覽器都會自動帶著 Cookie 或 token 過去，伺服器收到之後就可以找回對應的 session，或是直接從 token 裡讀出需要的資訊。HTTP 本身不會替你把兩次請求接起來，但網站可以另外做這一層。

Messages API 用的是更直接的做法，模型不記得你們剛剛聊過什麼，所以就每次都把「剛剛聊了什麼」整包帶過去。那個 `messages` 陣列，就是你這次決定帶給模型看的對話紀錄。

講到這裡順便把兩件很容易混在一起的事分開看，「伺服器那邊有沒有留紀錄」跟「模型下一次記不記得」是兩回事。就算伺服器把每一筆請求都存起來，下一次呼叫時模型看到的還是只有你這次送過去的內容，它不會自己回頭翻上次講過什麼。無狀態講的是後者。

至於前者，以我們今天這種一般的 Messages API 請求來說， [官方文件](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention) 寫的是對話內容預設不留存，留下來的資料未經你同意也不會拿去訓練模型。不過特定模型、需要保存資料才能運作的功能、法律保留或被安全系統標記的內容，都有另外的留存規則。需要更明確承諾的企業，也可以另外談 zero data retention。

想通這個設計你就會發現「模型沒有記憶」好像也不是什麼問題，這是一個刻意而且很聰明的設計選擇，不過代價是記憶這件事得由我們自己來張羅。

## 記憶、帳單，還有聰不聰明

模型沒有記憶這件事，會連帶影響後面三件事：

第一，既然模型自己不記得，那「記憶」這個功能，就得由我們的程式來假裝。做法就是自己維護一個陣列，把每一輪對話都存進去，每次呼叫都整包送出。這就是下一篇文章的重點，我們讓 KeSi 從「講一句忘一句」，變成「記得住整段對話」。

第二，你想想，既然每一輪都要把「從頭到現在」重送一遍，那對話越長，你每次要送的東西就越多，輸入 token 的費用也會跟著增加；模型回給你的輸出 token 則另外計費。這就是為什麼第一篇我就一直碎念成本，因為 stateless 這個設計，本質上就是個會隨對話長度膨脹的收費機制。這條線會一路延伸到後面那幾篇省錢的內容。

最後也是最重要的，這會決定模型跟我們對話的時候聰不聰明。念一本很厚的日記給一個金魚腦聽，念到後面，它反而容易抓不到重點。所以「怎麼安排這本日記、哪些該留哪些該濃縮」，就成了一門真功夫，這是之後幾篇要處理的事。

## 角色與系統訊息

以我們今天用的 Haiku 4.5 來說， `messages` 裡常用的角色有兩種。 `user` 就是使用者，也就是你本人； `assistant` 是模型，它回你的話就是這個角色，下一篇文章在維護日記的時候就會用到。

那 `system` 呢？它不是 `messages` 裡的第三種角色，而是跟 `model` 、 `messages` 同一層的 `system` 欄位。那封信裡長長的守則就在這裡，它是「給模型的行為設定」，地位比較特殊，之後會專門講。有些比較新的模型已經支援在對話中間插入 `system` 訊息，不過那有另外的使用條件，先不混在這裡。

你現在只要記得 `messages` 這本日記裡，會交錯躺著 `user` 跟 `assistant` 的發言，一來一往，這是我們明天要動手處理的東西。

## 小結

是說，在這個系列裡大部分時間是在驗證「這個機制通不通」，不是在解什麼世界難題，殺雞不用牛刀，所以大多我會先選用 Haiku 就夠了。等到後面遇到真的需要比較強推理能力的場合，我們再換上比較好的模型來對照，讓你親眼看到「同一段程式、不同的腦」，效果跟花費差多少。

今天我們沒做什麼花俏的東西，就是讓程式跟模型講上了第一句話，但這二十行證明了那封看起來很嚇人的信我們自己也組得出來。它也讓你認識了 stateless 這個會影響後面一大半內容的關鍵事實，還有那疊「積木」形式的回覆。

下一集，我們要來解決今天發現的那個「金魚腦」問題。我們要做一個能一直對話下去的介面，並且自己動手維護那本「日記」，讓 KeSi 記得住你們聊過什麼。

不過先提醒你，就算明天讓它記得住對話了，它也還只是一個會聊天的機器人，不是 agent。因為它光會講，還不會動手。要讓它從一張嘴進化成長出手腳，那是後天才會開始的事。

一步一步來，咱們下集見:)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401028)

[

Day 02 - 側錄 Claude Code！

](https://ithelp.ithome.com.tw/articles/10401028)

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

[![.](https://itadstatic.ithome.com.tw/B9/1784885238_6a632ff62c5c0.jpg)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9004)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/c6612942-9848-4e40-9cad-5c4173b1f7da_small.jpg)

[潛入核心：惡意程式與驅動程式的狼狽為奸](https://itplus.ithome.com.tw/vod-page/341?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

23 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/22/abb5eb18-d42b-452e-ae40-ff0e52b7d480_small.png)

[Zero Trust 讀書會：必讀 NIST 800-207標準解析 Part 1](https://itplus.ithome.com.tw/vod-page/619?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

零信任資安講堂 |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/13/f39c5726-9674-4c04-a41d-b503c194386e_small.jpg)

[資安長系列：給 1000 位新手資安主管的上手建議](https://itplus.ithome.com.tw/vod-page/358?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome |

48 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/f8b85e24-77ee-423b-b2c9-b82991b70803_small.png)

[城市級主權式 AI，邁向虛實整合的高雄數位孿生平臺](https://itplus.ithome.com.tw/vod-page/1248?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

29 分