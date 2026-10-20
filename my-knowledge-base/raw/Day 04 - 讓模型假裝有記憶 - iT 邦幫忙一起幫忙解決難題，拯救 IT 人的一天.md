---
title: "Day 04 - 讓模型假裝有記憶 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401399"
author:
  - "[[iThome]]"
published:
created: 2026-08-04
description: "文章同步發表在我的個人 Blog 在上一篇文章中提到 Messages API 不會自動把上一輪的對話傳給下一次，所以模型看起來就像個金魚腦，講一句忘一句。接下來我要用一個簡單的方法讓它假裝記得，就是..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 4

0

[Software Development](https://ithelp.ithome.com.tw/2026ironman/software-development)

### 為你自己手刻 Claude Code系列 第 4 篇

## Day 04 - 讓模型假裝有記憶

- 分享至

> 文章同步發表在 [我的個人 Blog](https://kaochenlong.com/)

在上一篇文章中提到 Messages API 不會自動把上一輪的對話傳給下一次，所以模型看起來就像個金魚腦，講一句忘一句。接下來我要用一個簡單的方法讓它假裝記得，就是準備一本「日記」，每聊一句就記一筆，然後在每次發出 HTTP 請求的時候都把整本日記的內容從頭念一遍給它聽。

這聽起來很笨，但正版的 Claude Code 也是從這裡開始的，只是現在正版的已經不會傻傻地一路重送到底（因為這太浪費錢了）。隨著對話越來越多就會準備清掉不重要的東西，把舊內容整理並濃縮成摘要，這些之後都會做，今天先把地基打好就好。

## 讓模型可以一直聊下去

現在的 `kesi.py` 有個不太方便的地方，就是它現在只會問一句就結束了，而且還只會問固定的問題，想再問其它問題就得改程式再重跑一次程式。我要的是一個能一直聊下去的介面，我問一句、它回一句、我再問一句，直到我不想聊為止。

這種「一直等著輸入、處理、再回頭等下一次輸入」的迴圈，其實大家每天有在用只是可能沒意識到而已。這東西叫 REPL，這是 Read、Eval、Print 以及 Loop 這四個英文單字的縮寫。讀取你的輸入、處理、印出結果、然後回頭再來一次。你在終端機打 `python3` 進到那個 `>>>` 的環境，或是在瀏覽器按 F12 打開的開發者工具，那個可以打 JavaScript 程式碼的 console，這些都是 REPL。甚至是你每天在用的 Claude Code，那個等你輸入問題的互動介面，也可以把它看成一個 REPL。基本上它們就是個不會停的迴圈，每一圈服務你一次。

我們的 KeSi 本質上就是要做一個這樣的東西，只是每一圈的過程多了「把問題送去給模型、把答案拿回來」的來回過程。骨架長這樣：

```python
while True:
    user = input("你 > ")   # 讀取你的輸入
    # ... 把你的話送去給模型，拿回答案 ...
    print("KeSi >", 答案)    # 印出來
    # 然後回到迴圈開頭，繼續等你下一句
```

這裡的 `while True` 會讓程式一直愛的魔力轉圈圈，每一圈就是一次「我問、它答」。但這樣還不夠，這個迴圈每一圈都是獨立的，一樣有金魚腦的問題，下一圈根本不知道上一圈聊了什麼，每次都是全新的一次呼叫（是說這其實很讓人羨慕啊，都不會有煩惱）。要治療這個失憶症，我得幫它準備一本「日記」。

## 日記只是個陣列

所謂的「日記」也沒什麼玄機，其實就是一個 Python 的 List 罷了。科普一下關於 Python 的 List 結構，其實在 Python 裡 List 不是陣列（Array），在 Python 裡陣列有另外的資料結構，有興趣可參閱《為你自己學 Python》這本書裡關於 [串列](https://pythonbook.cc/chapters/basic/list#%E5%86%B7%E7%9F%A5%E8%AD%98python-%E7%9A%84%E9%99%A3%E5%88%97) 的介紹。不過因為在其它程式語言常會把這種序列型的資料結構稱之為陣列，所以以下我也都會使用陣列的講法。

是說，記憶這麼重要的東西用一個普通的陣列就好了嗎？是的，這樣就行了，因為這裡需要的只是「按照順序把每一句話存起來」，陣列剛好就是個夠用的資料結構。一筆接著一筆，要從頭念到尾也很方便。

上一篇提過 `messages` 陣列裡放的是 `{"role": "user", "content": "..."}` 這種東西，日記要記的就是這個。規則只有兩條：

- 我講一句，就用 `user` 的身分記一筆
- 模型回一句，就用 `assistant` 的身分記一筆

一來一往，日記裡就會交錯躺著 user、assistant、user、assistant，完整記錄我跟模型的對話。

然後每次呼叫模型，我們就把整本日記當成 `messages` 送出去。所以之後模型收到的不再是簡單的一句話而是從頭到現在的完整來龍去脈。它還是金魚腦，但因為我們每次都把整本日記念給它聽，模型就能根據上下文回答了。

## 把 kesi.py 改成有記憶功能

`kesi.py` 改完長這樣：

```python
# /// script
# requires-python = ">=3.14"
# dependencies = ["anthropic"]
# ///

import anthropic

client = anthropic.Anthropic()
history = []

print("KeSi。輸入 /exit 離開、/reset 清空對話。")
while True:
    try:
        user = input("你 > ").strip()
    except (EOFError, KeyboardInterrupt):
        break
    if user in ("/exit", "/quit"):
        break
    if user == "/reset":
        history = []
        print("（日記已清空，我們重新開始）")
        continue
    if not user:
        continue

    history.append({"role": "user", "content": user})
    resp = client.messages.create(
        model="claude-haiku-4-5",
        max_tokens=1024,
        messages=history,
    )
    history.append({"role": "assistant", "content": resp.content})

    text = "".join(b.text for b in resp.content if b.type == "text")
    print("KeSi >", text)
```

比上個版本稍微多了幾行程式碼，不過應該不算太難懂。 `history = []` 這個陣列就是那本日記，一開始是空的。

進到 `while` 迴圈後 `input("你 > ")` 會停下來等你打字，後面接的 `.strip()` 是把不小心多打的前後空白去掉。包住 `input` 的那個 `try` 是處理當我按 Ctrl-D 或 Ctrl-C 想離開的情況，不然程式會噴一個難看的錯誤； `/exit` 跟 `/quit` 是離開的指令， `/reset` 晚點再另外講。 `if not user` 是你什麼都沒打就按 Enter，那就跳過這一圈，不用浪費一次呼叫也不用花錢。

真正的重點是下面那三步。

1. 先看 `history.append({"role": "user", "content": user})` ，這是日記的第一條規則，這就把我剛打的話用 `user` 的身分先記一筆。
2. 接著 `client.messages.create(...)` 跟上一版幾乎一樣，唯一的差別是 `messages` 那一欄，原本塞一句話，現在塞的是 `history` ，也就是整本日記。這個改動就是「有記憶」跟「金魚腦」的差別，其實這整篇文章濃縮起來的重點就只有這一行！
3. 最後 `history.append(...)` 是日記的第二條規則，把模型的回覆也用 `assistant` 的身分記一筆進去。

其它像是 `/reset` 要做的事也很簡單，就是把 `history` 清掉重來，KeSi 又變回一張白紙。當你發現對話越聊越歪，或是越回越慢、越來越燒錢的時候， `/reset` 一下就能把對話打掉重練。正版的 Claude Code 有個 `/clear` 指令，對模型來說效果跟這幾行差不多，都會清空目前的 context。差別是 Claude Code 仍會把舊的 session 存在本機，有需要的話之後還能接回來，但我這裡寫的 `/reset` 比較陽春，就只是直接把記憶體裡的 `history` 陣列清掉而已。

## 疊積木？

不知道你有沒發現一個小細節，在把東西印出來的時候我先把回覆整理成純文字 `text` ，但存進日記的時候存的卻是 `resp.content` 而不是只存 `text` 的值。

為什麼這樣做？這是先幫後面會介紹到的「工具呼叫」章節鋪的路。在前面的文章裡有說過模型的回覆是一疊積木，現在都還只是文字積木，但等之後把工具也放進來，模型就可能回一塊 `tool_use` 積木，意思是「我想用某個工具」，到時候日記裡也必須完整保存這些積木模型才會知道「我剛剛決定要用哪個工具」。

還好這件事不用我們操心，Anthropic 的 SDK 收到那些積木物件會自己序列化成正確的 JSON 送出去，直接把 `resp.content` 塞回 `history` 就好。

## 它記得住了！

跑下去，先報上名字再問它我是誰，看它接不接得住：

```markdown
$ uv run --env-file .env kesi.py
```

實際回答的句子每次可能不同，這裡只看一件事：第二次問「我是誰？」的時候，它能不能回答「菜市場阿龍」。只要接得上，就表示日記生效了：

![https://ithelp.ithome.com.tw/upload/images/20260804/20065770xtyexM6hhC.png](https://ithelp.ithome.com.tw/upload/images/20260804/20065770xtyexM6hhC.png)

同樣是問它我是誰，上一篇文章的時候它還兩手一攤說不知道，因為那次 `messages` 裡只有簡單一句話。現在它答得出來是因為 `messages` 裡除了這句，還加上前面那句「我是菜市場阿龍」。同一顆金魚腦，差別只在這次把整本日記念給它聽。

你以為的「AI 的記憶」，其實只是自己每次不厭其煩地再跟模型把講過的話重新報告一次。

### 不重要的小技巧：上下鍵

我是個終端機的重度使用者（重度到現在終端機都自己做一個的那種重度），我很習慣按上下鍵可以切換剛才輸入的訊息，不過如果在我剛才寫的對話機器人按上鍵，畫面只會跳出 `^[[A` 這種怪東西。這不是 bug，這是因為 Python 的 `input()` 函數就只負責讀我打給它的字，像是歷史紀錄這些貼心功能目前還沒有。

沒有怎麼辦？做就有啦。在 macOS 跟 Linux 上，解法只要一行，就在檔案開頭多加：

```python
import readline
```

就這樣，後面什麼都不用寫。Python 標準庫的 [readline 模組](https://docs.python.org/3/library/readline.html) 有個特性，光是把它 import 進來， `input()` 就會自動獲得上下鍵翻歷史、左右鍵移游標這些能力，官方文件寫明這個模組的設定會同時影響直譯器的互動提示和內建 `input()` 函數的行為。

這個功能在 Windows 上沒有支援。

## 用 Proxy 偷看日記

把之前寫的那個 `proxy.py` 叫醒，然後同樣把 KeSi 的流量導過去：

```markdown
$ ANTHROPIC_BASE_URL=http://127.0.0.1:9527 uv run --env-file .env kesi.py
```

然後跟它聊個兩三輪，再去翻 `captured/` 資料夾。這次你會看到不只一個檔案，聊了幾輪就有幾個 `request-N.json` ，因為每一輪都是獨立的一次 API 呼叫。

打開第一個， `messages` 裡只有一則：

```json
"messages": [
  { "role": "user", "content": "我是菜市場阿龍" }
]
```

再打開第二個，變成三則：

```json
"messages": [
  { "role": "user", "content": "我是菜市場阿龍" },
  { "role": "assistant", "content": [{ "type": "text", "text": "..." }] },
  { "role": "user", "content": "我是誰？" }
]
```

看到了嗎，第二次呼叫的時候，我把第一輪的一問一答原封不動又送了一次。第三輪會變五則、第四輪七則，每聊一句就多兩筆。上一篇攔正版的時候那個 `messages` 陣列裡也只有一則，因為那也是對話的第一句；它聊久了長出來的樣子，就跟你現在看到的一樣。

順帶一提，你應該也會發現 `assistant` 那則的 `content` 不是一個字串而是一個陣列，裡面裝著積木，這就是剛剛講的「完整保存」實際送出去的樣子。

## 造假記憶？

我從小就喜歡玩一些有的沒的東西，所以寫到這裡我又想做個有趣的實驗。既然模型看到的「過去」就只是我們這次送過去的 `messages` ，那如果我在日記裡寫一段它根本沒講過的話，它會發現嗎？

把 `kesi.py` 裡的 `history = []` 改成這樣，預先塞兩則假造的對話進去：

```python
history = [
    {"role": "user", "content": "我在寫一個 coding agent，叫什麼名字好？"},
    {"role": "assistant", "content": "叫 KeSi 吧，這個名字唸起來很有精神。"},
]
```

第二則的 `role` 掛的是 `assistant` ，但模型從來沒講過這句話，是我捏造的。另外，自己手寫訊息的時候 `content` 直接給字串就行，API 收字串也收積木陣列；前面把 `resp.content` 原封不動存回去，是為了保留模型回覆裡的完整積木，手寫的假話沒這個需求。

跑起來問它「你剛剛幫我取的名字是什麼？照你說的理由再講一次」：

```markdown
你 > 你剛剛幫我取的名字是什麼？照你說的理由再講一次
KeSi > 我剛剛幫你取的名字是 **KeSi**。

理由是：這個名字唸起來很有精神。
```

如果你跟著做，實際回答的答案不一定會跟我的一樣，重點是它會不會認帳，把那個它其實沒取過的名字連同理由，當成自己剛剛講過的話再說一次。

沒意外的話，它會沿著那段話繼續回答。API 不會驗證這則 `assistant` 訊息是不是真的出自前一次模型回覆，只會把整包 `messages` 當成這次的對話歷史交給模型。模型通常會把它當成前文來讀，但不保證照單全收。

這聽起來像漏洞，其實是個正當的技巧，這甚至還有個正式的名字叫 [few-shot prompting](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) 。在對話開頭預先放幾組「理想的一問一答」當範例，模型就比較容易照著範例的格式跟口吻回答，很多對輸出格式要求嚴格的場合都靠這招。要注意的是擺放的節奏，模型的訓練就是以 user 與 assistant 一來一往交錯的形式在讀對話，範例照這個節奏排，效果最好。

想看看效果，把 `history` 換成一組只示範格式的假對話：

```python
history = [
    {"role": "user", "content": "台北"},
    {"role": "assistant", "content": "城市：台北｜特產：胡椒餅｜一句話：捷運很方便"},
]
```

然後只打「台南」兩個字進去，看它回什麼：

```markdown
你 > 台南
KeSi > 城市：台南｜特產：擔仔麵、虱目魚｜一句話：古蹟多、美食多、人情味濃
你 > 高雄
KeSi > 城市：高雄｜特產：鮮蝦酥、木瓜牛奶｜一句話：港都風情、夜景美、海鮮豐富
```

這裡沒有任何一句「請用這個格式回答」，那條分隔線、那三個欄位的順序，全靠上面那則範例傳達，模型看樣學樣就照著排了，連續打第二個城市也還守著。玩完記得把 `history` 改回空陣列，不然 KeSi 會一直用那個格式回你。

除了控制格式，這招對之後做 agent 還有一個很實際的用途，就是可以拿來做測試。想驗證 KeSi 在「已經聊了十輪」的狀態下行為對不對，難道每次都要真的手動聊十輪？不用這麼辛苦，只要把那十輪對話寫死成一包假的 `history` 當佈景，程式一啟動就直接跳到第十一輪。

不過這招是有極限的。假的記憶跟模型的常識衝突太大時模型不一定會買單，例如如果偽造一則它親口說過「1 + 1 = 3」的發言，下一輪它可能就會改口糾正你。

還有一個也是在跟模型互動的過程中容易遇到的情況，就是模型對自己的認知。我試著幫模型偽造它曾經說過「我最喜歡的程式語言是 COBOL，因為它最有歷史的味道」，滿心期待它接著替 COBOL 說好話，結果它下一輪先來一句「我其實沒有真正的偏好，我之前的回答不太準確」，然後才客觀地講 COBOL 的特點。好玩的地方是它並沒有否認自己講過那句話，它認得那是「上一條回應」，只是不認同那個內容。

也就是說，日記可以決定它「記得」什麼，但不一定能改變模型的判斷力，有興趣你可以自己多塞幾種假資訊，跟模型玩造謠的遊戲看會發生什麼事。

發出請求的人可以編排這一次要讓模型看到的對話歷史，你決定送什麼資料給模型就等於決定它這次能從日記裡讀到什麼。之後不管是把舊對話濃縮成摘要，還是把專案的慣例寫進檔案再餵回去，做的其實都是同一件事。

## 話只講一半？

造假還有一個更進階的玩法。剛剛偽造的是「它講過的話」，這次要造的是「它正在講的話」。不過這個實驗沒辦法在聊天迴圈裡做，因為你一打字，你的話就會被排到日記最後面去。我這裡另外寫一個 Python 程式 `prefill.py` ，跟之前寫的單次對話差不多：

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
    messages=[
        {"role": "user", "content": "用一句話評論 Python 這個程式語言"},
        {"role": "assistant", "content": "我對 Python 的評價是：這語言最大的特色在於"},
    ],
)

for block in resp.content:
    if block.type == "text":
        print(block.text)
```

重點在 `messages` 的最後一則，role 掛的是 `assistant` ，而且話只講到一半。這包送出去，模型不會重新開始回答，它會從那半句話的斷點直接接下去講，彷彿那個開頭真的是它自己起的。這招叫 prefill（預填），是 [官方文件明載的功能](https://platform.claude.com/docs/en/api/messages) ：在支援 prefill 的模型上，只要最後一則訊息是 assistant，回應就會從那則訊息的內容接續下去：

![https://ithelp.ithome.com.tw/upload/images/20260804/20065770PWeiKz9oBa.png](https://ithelp.ithome.com.tw/upload/images/20260804/20065770PWeiKz9oBa.png)

實際接續的文字每次可能不同，觀察重點是回應會直接接在「這語言最大的特色在於」後面，而不是重新從頭回答。程式只印出模型新生成的部分，所以那半句預填的開頭不會再出現一次，你得自己在腦袋裡把它接回去讀。

為什麼有這種功能？這是控制輸出格式的老牌手法。想引導模型直接輸出 JSON，不用囉嗦講一堆前情提要，直接把 `{"` 塞進它嘴裡，它就可能從 JSON 的第一個欄位接著寫，但這不保證最後一定是合法的 JSON。想讓它在選擇題裡只回答選項？預填「答案是（」。在還沒有更好工具的年代，這招也是可以用來控制模型輸出的手法。

我這裡用的 Haiku 4.5 還支援這個玩法，不過 Opus 跟 Sonnet 從 4.6 版開始就把它拿掉了，最後一則放 assistant 會直接吃一個 400 錯誤，訊息是 `This model does not support assistant message prefill` ，取而代之的是更正式的結構化輸出功能。Haiku 這條線目前停在 4.5 還沒往上走所以還玩得到，換成 Opus 或 Sonnet 的新版本就會被打回來。

## 寫日記沒什麼規矩

連玩了兩輪造假，你可能會以為 `messages` 有一套嚴格的格式檢查在把關，但其實它的規矩比想像中的鬆。例如，你可能以為 user 和 assistant 必須交錯，一來一往不能亂。官方文件說得很清楚，模型的訓練習慣確實是交錯的對話，但如果你連續塞了兩則同角色的訊息，API 不會報錯，它會被併成同一輪。不信的話把一句話拆成兩則連續的 user 訊息送出去：

```python
messages=[
    {"role": "user", "content": "我最喜歡的食物"},
    {"role": "user", "content": "是滷肉飯。請問我最喜歡的食物是什麼？"},
]
```
```markdown
根據你的陳述，你最喜歡的食物是 **滷肉飯**。
```

API 會把兩則連續的 user 訊息合併成同一個 user turn 而不是多出一輪新的對話，模型會把它們當成同一輪來讀。

另外，你可能也以為日記必須「從頭開始」，第一則就是對話真正的起點。其實也不必，你可以只送最後三輪，前面聊過的十輪統統不給。這是合法的請求，模型就只知道那三輪，對它來說世界就是從那裡開始的。聽起來像缺陷？之後對話長到快撞上限的時候，「砍掉舊的、只送新的」就是最簡單的手段，後面處理長對話的時候會再詳細介紹。

所以真正的規矩不多，例如規定 `messages` 不能是空陣列，至少要給一則訊息，不然 API 會直接退件。其他很多你以為的規矩，多半只是慣例。

## 日記沒有日期

翻回去看我們的 `history` ，每則訊息就兩個欄位， `role` 跟 `content` ，沒有時間戳記，API 的訊息格式裡也沒有這個欄位。

這代表模型對時間的流逝毫無感覺。就算兩句話中間隔了一個月，只要在日記裡是連著的，對模型來說它們就是無縫接軌的。就算你把這包 `history` 存起來，下週再讀回來繼續聊，它也渾然不覺，不會跟你說「好久不見」，因為在模型的世界觀裡，你們的對話從來沒有中斷過。

模型連「現在」是什麼時候都不知道。雖然訓練資料有截止日，但日記裡又沒有日期，如果沒特別跟它講的話，模型會連今天是幾月幾號都只能用猜的。Claude Code 的解法很樸素，就是把今天的日期直接寫進每次送出去的內容裡。好奇的話，你可以拿我們之前寫的 proxy 自己攔一包下來找找看就會知道了。

還有個容易誤會的地方。模型能看到的對話歷史由送進去的 `messages` 決定，但如果把同一本日記一字不差地送兩次，答案也不會完全一樣。模型生成文字的過程帶有隨機性，每一步都是從下一個 token 的候選分布裡「抽」出來的，同樣的輸入，兩次抽出來的路線可以不一樣。不過這還好，都已經 2026 年了這也不是什麼新鮮事。API 有個 `temperature` 參數可以控制隨機的程度，調低會讓輸出比較收斂，但就算調到最低，官方也從來沒有保證過每次輸出完全相同。這是 Haiku 4.5 還支援的做法， [Opus 從 4.7 版、Sonnet 從 5 開始](https://platform.claude.com/docs/en/about-claude/models/migration-guide) 就不再接受調整這個參數了，硬填同樣會直接吃一個 400 回應，錯誤訊息是 `temperature is deprecated for this model` 。

換句話說，日記決定模型這次看得到哪些前文但不會決定模型「怎麼說」。這也表示同一個 bug 丟給它修兩次，它可能走兩條不同的路，一次先看測試、一次先翻程式碼。也許這兩次都能把 bug 修好，但過程不一樣。你沒辦法靠重跑一次得到完全相同的重播，這跟許多決定性的（deterministic）傳統程式很不一樣，那些程式同樣的輸入就是同樣的輸出，但模型卻是個每次都即興發揮的傢伙。

這也讓「怎麼測試一個 agent」變成一門學問，剛剛才說偽造歷史可以準備出相同的純文字對話輸入，現在又說同一份輸入跑兩次結果可能不同，這兩件事加起來，你大概就能感覺到這裡的水有多深了。在之後的實作就會再親身體會到。

## 日記是要付錢的

每聊一句 `history` 就多兩筆，而每一次呼叫我們都把整本 `history` 送出去。聊得越久，每次送出去的東西就越厚，input token 也就越多。一開始可能只有幾十個 token，聊到第五十句，每次的 Enter 鍵送出去的都是前面累積的整本日記再加上現在的這句。

同樣一句「嗯，然後呢」，在對話剛開始跟在聊很久之後送出去的價錢是不一樣的。前者送出去的只有這幾個字，後者送出去的是這幾個字加上前面一大疊日記。

而且這筆帳越後面越傷。粗略抓個數字，假設你跟它每輪講的話各佔 100 個 token，聊 10 輪，你們實際產生的內容不過 2,000 個 token 上下，但因為每一輪都要重送前面的全部，10 輪加起來實際送出去的 input 總量會逼近 10,000 個 token，是內容本身的五倍。聊到 100 輪，這個倍率會滾到 50 倍左右。內容只是線性地變多，送出去的總量卻是平方在長，這就是「每次整本重念」這個天真做法的代價。

看到這裡你可能會替 Anthropic 的客人捏一把冷汗，Claude Code 隨便就聊上幾百輪，每輪重送豈不是燒到破產？還記得第 2 天的文章裡提到，在正版的 `system` 上看到的那個 `cache_control` 標記嗎？那就是解法之一。重複的部分命中快取時，只算標準 input token 價格的 10%，也就是打一折啦！不過第一次建立快取仍會另外計價，之後會有一整篇文章專門講它。

不用光聽我說，上一篇提過回應裡有個 `usage` 欄位，加一行就看得到：

```python
print(f"（input {resp.usage.input_tokens}、output {resp.usage.output_tokens}）")
```

把這行加在印出回覆的後面，每聊一輪就會看到數字。output 會在你設的 `max_tokens` 底下浮動，但 input 會一路往上爬：

```markdown
你 > 我是菜市場阿龍
KeSi > ...
（input 18、output 148）
你 > 我是誰？
KeSi > ...
（input 175、output 124）
你 > 那你猜猜看我可能在菜市場賣什麼？
KeSi > ...
（input 324、output 288）
```

回覆的內容跟這裡要看的事無關，所以我用 `...` 代替了。實際數字會隨著對話內容變動，觀察重點不是某一輪有多少，而是 `input_tokens` 會隨著日記變厚一輪一輪往上爬。你也可以順手驗一個關係：第二輪的 input 差不多是第一輪的 input 加 output 再加上你新打的那句。

不過這個問題今天先放著，先心裡有數就好，知道有一顆會越滾越大的雪球。之後我們會幫 KeSi 裝上正式的電表，把每一輪的花費算出來，到那時候再來想辦法讓它別燒這麼兇。這也是我一直說的，context 的管理才是這個系列真正的重頭戲，而它的源頭就是今天這本越寫越厚的日記。

## 免費算 Token

講到算錢，補個比較少人知道的功能。官方有一個專門的 [token 計數端點](https://platform.claude.com/docs/en/build-with-claude/token-counting) ，把跟正式請求同樣格式的 `messages` 丟給它，它回你一個數字，而且這個 API 還是免費的，只有每分鐘請求次數的限制：

```python
count = client.messages.count_tokens(
    model="claude-haiku-4-5",
    messages=history,
)
print(count.input_tokens)
```

拿它來做個小實驗。前面曾經提到中文一個字粗抓差不多是 1 到 2 個 token，現在可以驗證了，例如「我是菜市場阿龍」這七個字是幾個 token：

```markdown
18
```

端點回來的 `input_tokens` 是這包請求在這顆模型上的 input token 估計值。這個數字跟中文字數之間沒有簡單的倍數關係，token 也不是照中文字逐字切開的。

咦？18？「我是菜市場阿龍」全部也才 7 個中文字，就算一個字 2 個 token 也不過 14。這不是中文特別貴，而是 `count_tokens` 量的不是那串文字本身，而是整包請求的 input token 估計值。換幾組長度不同的內容量量看：

| 內容 | `input_tokens` |
| --- | --- |
| `a` | 8 |
| `hello` | 8 |
| 我 | 9 |
| 我是 | 10 |
| 我是菜市場阿龍 | 18 |
| 我是菜市場阿龍我是菜市場阿龍 | 28 |

先看最上面兩行，一個字母 `a` 是 8，五個字母的 `hello` 也是 8。這表示兩包請求的估計總量相同，但光靠這組數字，還不能倒推出文字本身各佔幾個 token，也不能把剩下的差額精確算成固定的「包裝費」。

能確定的是，API 計算的「輸入」不只看畫面上那幾個字，而是衡量整包請求。不過官方也提醒，估計值可能包含 Anthropic 為了系統最佳化自動加入的 token，而且這些系統加入的 token 不會計費，所以別把表格裡的差額直接當成帳單上的固定成本。

還有一個小地方提醒：這個端點只估 input，不會幫你預測模型要回多長，output 的部分要等正式回應的 `usage` 才知道。所以之後在幫 KeSi 裝電表的時候，這個端點跟 `usage` 欄位就是我們的兩件量測工具，一個秤寄出前的，一個看寄出後的。

## 日記總有寫滿的一天

錢的雪球是慢慢滾的，但這本日記還有另一個更硬的限制，就是模型一次能讀進去的量是有上限的，這個上限就是所謂的 [context window](https://platform.claude.com/docs/en/build-with-claude/context-windows) 。以我在範例裡用的 Haiku 4.5 來說是 20 萬個 token，而且不是只算日記，包括 system 守則、之後會掛上的工具清單連同模型這次要產生的回應，全部都算在同一個額度裡。不同模型的上限不同，有的開到一百萬個 token，但就算再大都還是有寫滿的一天。

20 萬聽起來很多，但等 KeSi 開始動手做事就知道了，讀幾個大檔案、跑幾次測試把輸出塞回對話，幾萬個 token 一下就沒了。

寫滿了會怎樣？也許你會認為模型會自動忘掉最舊的內容，繼續聊下去。不會。就 Messages API 來說，如果 input 本身已經超過 context window，API 會直接回一個 400 錯誤，訊息是 `prompt is too long` ，這次請求就是失敗，一個字都不會回。以 Claude 4.5 以及之後的模型來說，如果只是 input 加上你設定的 `max_tokens` 可能超過上限，請求仍會被接受；真的在生成途中撞牆時，會以 `stop_reason: "model_context_window_exceeded"` 停下來。無論哪一種，它都不會偷偷把你的日記撕掉幾頁。

這個設計是正確的。要是 API 自作主張丟掉內容，模型看到的世界就跟你以為的不一樣，答錯了你還查不出原因。哪些該丟、什麼時候丟或是丟掉之前要不要先濃縮成摘要，這些決定 API 統統留給我們自己做。先知道有這個上限就好，怎麼在撞到上限之前出手，之後會花一整篇文章來介紹。

另外，20 萬這個數字也不用死背。官方有個 models 端點，可以用程式查每顆模型的規格：

```python
m = client.models.retrieve("claude-haiku-4-5")
print(m.max_input_tokens)   # context window 的大小
print(m.max_tokens)         # 單次輸出的上限
```
```markdown
200000
64000
```

第一個數字是這顆模型目前的 input context 上限，剛好就是前面說的 20 萬；第二個是單次輸出可設定的 `max_tokens` 上限，六萬四，我在程式裡填的 1024 離天花板還很遠。這兩個值都以執行當下的模型規格為準，之後 KeSi 要是想在快撞牆之前自己踩剎車，就不用把數字寫死在程式裡，跟 API 問就有。

## 模型知道日記剩幾頁

講到上限，補一個不太重要的冷知識，模型其實看得到自己還剩多少空間。

[官方文件](https://platform.claude.com/docs/en/build-with-claude/context-windows#context-awareness) 把這個功能叫 context awareness。在每一次請求裡，API 會自動往給模型的內容裡塞一個標籤，告訴它這次的總額度有多大，長得像這樣：

```xml
<budget:token_budget>200000</budget:token_budget>
```

等之後 KeSi 會用工具了，每次工具執行完 API 還會再補一行用量回報，告訴模型現在用掉多少、還剩多少。這一切全自動，你什麼都不用做，官方還特別交代這些標籤是 API 自己注入的，你不要自己送。

這可以解釋一個你可能曾經遇過的現象，有時模型會主動說「考量篇幅我長話短說」，那不一定只是在客套，因為它可能真的看得到那個數字，知道自己快把空間用完了。不是每顆模型都有這個功能，我這裡用的 Haiku 4.5 剛好在支援清單上。

這件事還有一個好玩的地方，在前面我用 proxy 攔下送出去的請求，我們以為看到了模型讀到的全部，其實不然，API 在伺服器那端還會往裡面再加料，像是這個 `budget` 標籤就是一例。你攔到的是你寄出的信，模型最後讀到的是加工過的版本，中間的差距，就是 API 替你打點的那些事。

問題是，這些加的料算誰的錢？官方在 token 計數的文件裡有交代，系統為了最佳化自動加入的 token 不會向你收費，帳單只反映你自己送的內容。嗯，合理。

## 記憶有三層

我們今天做的是「這一次對話之內」的記憶。你把程式關掉重開，那本 `history` 又是空的，KeSi 就把你忘得一乾二淨，輸入 `/reset` 也是一樣的效果。這種記憶活在這一次的對話裡，程式一關就沒了。

不過「程式一關就沒了」這件事是可以補救的，Claude Code 就有做到這件事。你可以去翻自己電腦上的 `~/.claude/projects/` 資料夾，裡面躺著一堆副檔名是 `.jsonl` 的檔案，一場對話一個檔。每一行可能是一則訊息、一次工具呼叫、工具結果或其它中繼資料，格式是 Claude Code 的內部實作，版本更新時也可能會變。

當你輸入 `claude --continue` 的時候，Claude Code 會從這些紀錄恢復最近那場對話以及相關狀態。短對話可以接回完整歷史，太長的對話也可以先濃縮成摘要再繼續。不過概念還是一樣，把日記存起來，需要的時候再把該讓模型知道的內容放回 context。

另一種記憶就不太一樣了。Claude Code 能記得你這個專案的一些事情，像是慣例、指令怎麼下，跨了好幾次開關、甚至開了全新的對話都還記得。這靠的不是模型，也不是把哪本舊日記讀回來，而是透過 `CLAUDE.md` 或 auto memory 把該記的事另外寫進檔案，每場新對話再讀進來、當成 context 的一部分送出去。今天做的是對話裡的記憶，像這種跨對話的專案記憶我們之後再來做。

還有一種在更底下，不過老實說把它叫「記憶」有點勉強，因為那根本就是模型本身。在第 3 天的文章裡，我直接問「用一句話介紹誰是高見龍」，它一下子就答出來，那可不是誰塞進日記裡的，是訓練的時候就固化在權重裡的東西，論知識量它遠比另外兩層大得多。權重不是模型拿來放記憶的地方，權重就是模型，訓練跑完之後那堆數字就是它的全部。

之所以把它放進來一起講，是因為「模型為什麼會知道這件事」的答案就只有這三個來源：

1. 你在這次的對話直接告訴它，也就是這篇文章的重點
2. 或是你從檔案餵給它
3. 它本來就會（在模型裡）

前面兩種你能決定要送什麼進去，但第三種你動不了。你跟它聊再多，那些對話都不會回頭改寫權重，它不會因為跟你聊過就變得比較懂你。下一場全新的對話如果沒把舊 context 帶回來，它不會只因為上次聊過就自動認得你。

## 小結

今天 KeSi 從「講一句忘一句」進步到「記得住整段對話」了。做法一點都不神奇，就是自己維護一本日記，每次都整本念給金魚腦聽，核心濃縮起來就是 `messages=history` 那一行。我們可以順著 proxy 看那本日記怎麼從一則變三則、五則，看著 input token 跟著往上爬，也可以偽造一段它沒講過的話、把半句話塞進它嘴裡讓它接著講。這些實驗指向同一件事，模型這次能接著哪些前文回答，取決於你送出去的那包 `messages` 。

不過目前的 KeSi 還只是一張嘴。如果我跟它說「幫我讀一下 `config.py` 這個檔案，看看裡面寫了什麼」，它會很有禮貌地回你說沒辦法直接存取你的檔案，但如果你把內容貼上來它很樂意幫你看。

它聽得懂我要它做什麼、知道「讀檔案」是什麼意思、也知道我想幹嘛，但它就是做不到，因為它碰不到我電腦裡的檔案。這就是 chatbot 跟 agent 之間那道最關鍵的差別之一，一個只能跟你動嘴皮子，一個能真的伸手到你的電腦裡讀檔、改 code、跑測試。

要做到這件事靠的是「工具」，也是我們下一篇文章要做的東西。咱們下集見:)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401069)

[

Day 03 - 第一個 API 請求

](https://ithelp.ithome.com.tw/articles/10401069)

系列文

[為你自己手刻 Claude Code](https://ithelp.ithome.com.tw/users/20065770/ironman/9031) 共 4 篇

目錄

1. 1
	[Day 01 - Claude Code 其實就只是一個 while 迴圈](https://ithelp.ithome.com.tw/articles/10400753)
2. 2
	[Day 02 - 側錄 Claude Code！](https://ithelp.ithome.com.tw/articles/10401028)
3. 3
	[Day 03 - 第一個 API 請求](https://ithelp.ithome.com.tw/articles/10401069)
4. 4
	[Day 04 - 讓模型假裝有記憶](https://ithelp.ithome.com.tw/articles/10401399)
[完整目錄](https://ithelp.ithome.com.tw/users/20065770/ironman/9031)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/c2c3a6ed-a8d7-4216-9cb4-6c80562bc647_small.jpg)

[人工智慧安全與隱私](https://itplus.ithome.com.tw/vod-page/346?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

33 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/54069ccc-c179-470c-8452-a88658852f04_small.png)

[多雲資料保護解決方案](https://itplus.ithome.com.tw/vod-page/555?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/6/4/6708bf9f-c054-4d87-9f6c-0c8ae3ac6170_small.jpg)

[進階PHP網路爬蟲開發經驗談](https://itplus.ithome.com.tw/webinar-page/229?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

41 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/7c1c79a4-e624-4fbc-90cf-9d7a01d99221_small.png)

[從觀察到實踐：打造符合公司需求的GitLab DevOps流水線](https://itplus.ithome.com.tw/vod-page/1093?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

38 分