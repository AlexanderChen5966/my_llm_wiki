---
title: "Day 05 - 模型怎麼使用工具 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401655"
author:
  - "[[iThome]]"
published:
created: 2026-08-06
description: "文章同步發表在我的個人 Blog 昨天的結尾我們做過一個小測試，我跟 KeSi 說「幫我讀一下 config.py」，它給個尷尬又不失禮貌的微笑並表示自己碰不到你電腦裡的檔案，要我把內容貼上來給它看。..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 5

0

[Software Development](https://ithelp.ithome.com.tw/2026ironman/software-development)

### 為你自己手刻 Claude Code系列 第 5 篇

## Day 05 - 模型怎麼使用工具

- 分享至

> 文章同步發表在 [我的個人 Blog](https://kaochenlong.com/)

昨天的結尾我們做過一個小測試，我跟 KeSi 說「幫我讀一下 `config.py` 」，它給個尷尬又不失禮貌的微笑並表示自己碰不到你電腦裡的檔案，要我把內容貼上來給它看。嘴巴講的什麼都懂但卻什麼都做不了，這就是 chatbot 跟 agent 的差別。今天我要來幫 KeSi 裝上手腳，在裝之前先來學習一下怎麼安裝以及是怎麼接收指令的。

GitHub Repo： [https://github.com/kaochenlong/KeSi](https://github.com/kaochenlong/KeSi)

## 模型只會許願

在第 1 天的文章就提過，模型是在 Anthropic 的伺服器上執行，不會直接碰到你本機的檔案、終端機或整台電腦。Messages API 不只收文字，也能收圖片或文件，不過要讀本機檔案或執行指令，還是得透過工具交給別的程式動手。

那 Claude Code 是怎麼讀到檔案的？答案在第 2 天攔下來的那封信裡其實已經看到了：我在那次側錄裡看見請求的 `tools` 欄位裝著 83 個工具定義。工具是我們這邊定義然後隨著請求送過去的，模型看了這份清單之後，它能做的事情只有一件事，就是在回應裡輸出一段固定格式的資料，說「我想使用某個工具，而且參數是這些」。

就這樣而已。模型許願但我們自己寫的程式會決定要不要幫它實現。這個機制在業界通稱 tool calling 或 function calling，在 Anthropic 的文件叫它 [tool use](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview) ，講的都是同一件事。

為什麼模型會乖乖輸出這種固定格式？因為它在訓練階段就被教過這套規矩。你可以想像訓練資料裡塞了大量「看到工具清單、輸出一段格式固定的工具呼叫、拿到結果、接著回答」的示範對話，模型把這個行為模式學了起來。請求裡只要帶著工具清單，模型就知道自己多了一個選項，不急著回答，先輸出一段資料，寫明「我要用哪把工具、參數填什麼」，而且這段資料長什麼樣子是固定的，不是讓模型自由發揮。這不是我們用嘴巴拜託它「請用 JSON 回答」那種不太可靠的約定，是刻在模型裡的行為。

## 第一個工具 get\_time

我另外開一個檔案 `probe.py` 做實驗，原因後面再說明。工具我先挑了一個無聊回報現在時間的功能。在上一篇文章提到日記沒有日期欄，這麼厲害、偉大又無所不知的模型竟然連今天是幾月幾號都答不出來。模型沒有時鐘，訓練資料也停在過去，所以「今天幾月幾號」這個問題它非求助不可，這是模型答不出來的問題（就算是硬給一個答案大概也是錯的），這裡剛好可以觀察模型會怎麼開口求助：

```python
# /// script
# requires-python = ">=3.14"
# dependencies = ["anthropic"]
# ///

import anthropic

client = anthropic.Anthropic()

tools = [
    {
        "name": "get_time",
        "description": "取得現在時間。當使用者問現在幾點時呼叫。",
        "input_schema": {"type": "object", "properties": {}, "required": []},
    }
]

resp = client.messages.create(
    model="claude-haiku-4-5",
    max_tokens=1024,
    tools=tools,
    messages=[{"role": "user", "content": "現在幾點？"}],
)

print("stop_reason:", resp.stop_reason)
for block in resp.content:
    if block.type == "text":
        print("模型說：", block.text)
    elif block.type == "tool_use":
        print(f"模型想呼叫工具：{block.name}，參數：{block.input}")
```

這裡的新同學就是 `tools` 參數。工具就是一包三個欄位的資料，第 2 天拆 Claude Code 的 `Read` 工具時看過同樣的形狀，現在換我們自己寫。其中：

`name` 是工具的名字，模型之後就是用這個名字點名。 `description` 是寫給模型看的說明書。我這裡不是只寫「取得現在時間」，還多交代了一句「當使用者問現在幾點時呼叫」，這是寫給模型看的。

要不要用工具或是什麼時候用這是模型自己判斷的，而模型判斷的依據就是這段描述。這不是我自己歸納的， [官方文件](https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools) 在定義 `description` 欄位的時候就有提到，它要交代的是「這個工具做什麼、什麼時候該用它或是它怎麼運作」，不是只寫一句名詞解釋。看看第 2 天那次側錄裡的 Claude Code，光是一個讀檔工具就寫了一千七百多個字元的使用說明書。

最後的 `input_schema` 是指使用這個工具的時候需要哪些參數。這裡用的格式叫 [JSON Schema](https://json-schema.org/) ，這是業界標準不是 Anthropic 自己發明的，第 2 天攔到的 `Read` 工具裡那行 `$schema` 宣告的就是這東西。 `get_time` 不需要任何參數，所以 `properties` 可以是空的，但這三個欄位一個都不能少。

這三個欄位裡，哪個最值得花時間寫？在官方文件有寫到建議把 description 寫得詳細一點，因為這會是影響工具表現最重要的因素，原文甚至直接用 by far the most important factor 來表示這個欄位的重要性。文件裡還有給一些建議，每個工具的說明至少三、四句起跳，工具越複雜寫越多。問題是要寫哪些東西？例如：

- 這工具做什麼
- 什麼時候該用（以及什麼時候不該用）
- 每個參數是什麼意思
- 有什麼限制跟不會回傳的東西。

舉個例子，一樣是要用來查詢股價的工具，比較普通的寫法就是一句「查股價」就草草了事，比較好的版本會把「只支援美股上市公司、回傳美元計價的最新成交價、使用者問股價時才用、不提供公司的其他資訊」都交代清楚。差別在哪？模型拿到普通版本的說明書就像新人拿到一份只寫著「處理庶務」的工作說明，每次要不要出手或是要怎麼做全靠通靈或自己腦補。

回到我們自己的工具，普通的寫法就是 `"description": "時間工具"` ，四個字，模型得自己猜這是查時間、設鬧鐘還是報時差。我在 `probe.py` 裡寫的版本多加了用途跟時機，之後 KeSi 的每個工具，讀檔、改檔或執行指令，說明書也都會照這個標準來寫。

在 `input_schema` 這裡也有要注意的，如果某個參數只能填固定那幾種值，可以用 `enum` 把選項列出來，再搭配 `strict: true` 把格式收緊。例如溫度單位只給 celsius 跟 fahrenheit 兩個，正常完成的工具呼叫就只能從這份清單裡挑：

```python
{
    "name": "get_weather",
    "description": "查詢指定城市目前的天氣。使用者問天氣時呼叫。",
    "strict": True,
    "input_schema": {
        "type": "object",
        "additionalProperties": False,
        "properties": {
            "city": {
                "type": "string",
                "description": "城市名稱，例如 Taipei",
            },
            "unit": {
                "type": "string",
                "enum": ["celsius", "fahrenheit"],
                "description": "溫度單位",
            },
        },
        "required": ["city"],
    },
}
```

在這裡 `city` 是自由填的字串，模型想填什麼就填什麼，但 `unit` 用 `enum` 列出兩個選項，再開 strict 模式後單位只能有華氏或攝氏兩種，不會冒出第三種單位。另外 `required` 這次只列了 `city` ，意思是 `unit` 選填，模型可以整個不填，這樣收單的那邊（也就是我們的工具）就要自己準備一個預設值。

關於工具的名稱有個規定就是工具不能用中文名稱，官方規定 `name` 欄位必須符合 `^[a-zA-Z0-9_-]{1,64}$` 這條 regex 規則，只能用英文字母、數字、底線跟減號的組合，最多 64 個字元，這就是為什麼你看到的工具都叫 `get_time` 、 `read_file` 這種長相。

還有兩件建議，現在可能還好但等工具變多一點、開始有選擇障礙的時候才比較有感覺。

首先，可以把相關的操作合併成一個工具。舉個例子，同樣是在操作 Git 的 PR（Pull Request），不要 `create_pr` 、 `review_pr` 、 `merge_pr` 寫成 3 個工具，而是直接寫成 1 個然後帶個例如 `action` 參數就好。因為模型每次出手都得先從清單裡挑一個可能適合的工具，選項越少而且工具之間彼此差異越大，挑錯的機會就越小。

第二個是可以在工具的名字加上服務名稱，例如 `github_list_prs` 、 `slack_send_message` 這樣。工具少的時候沒差，但等哪天你同時接了 GitHub、Slack 跟 Notion 而且這三個都想要一個叫 `search` 的工具名字就打架了。加上額外的前綴或後綴，模型一眼就知道這是誰家的工具。

選配欄位除了 `input_examples` ，還有前面沒提到的 `strict` 、 `cache_control` 、 `defer_loading` 這幾個，這些分別跟格式保證、省錢、大量工具的管理有關，等一下會碰到第一個，其他的之後遇到再說。

想深入工具設計的話 Anthropic 的工程團隊有一篇 [Writing tools for agents](https://www.anthropic.com/engineering/writing-tools-for-agents) 專門在講這件事，值得放進待讀清單。

## 說明書也是要錢的

這包 `tools` 不是設定一次就永久生效的東西，它跟 `messages` 一樣，每一次請求都要整包重送，而且照 [官方的計費說明](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview#pricing) ， `tools` 裡的每個字都算在 input token，也就是說這些都是要錢的。

在文件的計費說明裡還有一行容易被略過的話：

> When you use `tools`, the API also automatically includes a special system prompt for the model which enables tool use

意思是只要請求裡帶了工具，API 就會自動幫模型加上一段啟用工具用的特殊 system prompt。昨天才看過 API 往內容裡偷塞 budget 標籤，這裡又抓到一筆加料，而且這次官方大方得多，在 [定義工具那份文件](https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools#tool-use-system-prompt) 裡直接把這段隱形 prompt 寫出來了：

```markdown
In this environment you have access to a set of tools
you can use to answer the user's question.
{{ FORMATTING INSTRUCTIONS }}
String and scalar parameters should be specified as is, ...
Here are the functions available in JSONSchema format:
{{ TOOL DEFINITIONS IN JSON SCHEMA }}
{{ USER SYSTEM PROMPT }}
{{ TOOL CONFIGURATION }}
```

你寫的工具定義，就是被填進 TOOL DEFINITIONS 那一格，跟你自己的 system prompt 組裝在一起送給模型。這段內容在我們自己發出的請求裡看不到但這會算在你的 input token 裡，以 Haiku 4.5 來說是 496 個 token 而且每顆模型的數字不一樣，官方在計費那節有一張完整的列表。也就是說，光是「掛上工具」這個動作本身就有基本費，還沒開始許願就要先付錢了。

496 這個數字不用背，你可以用現成的工具自己算，就是上一篇文章裡學到的免費量體重服務。同一句話量兩次，一次不帶工具、一次帶著 `get_time` ，兩個數字相減，差值就是「隱形 prompt 加上 get\_time 本身」的大小：

```python
bare = client.messages.count_tokens(
    model="claude-haiku-4-5",
    messages=[{"role": "user", "content": "現在幾點？"}],
)
armed = client.messages.count_tokens(
    model="claude-haiku-4-5",
    tools=tools,
    messages=[{"role": "user", "content": "現在幾點？"}],
)
print(bare.input_tokens, armed.input_tokens, armed.input_tokens - bare.input_tokens)
```
```markdown
14 634 620
```

以這次實跑來說，不帶工具 14 個 token，掛上 `get_time` 之後變成 634，差了 620。這個 620 裡面有兩份東西：官方表格上那 496 個 token 的隱形 prompt，加上 `get_time` 自己的名字、說明書跟 schema。也就是說，我寫的那個工具本身大約佔一百二十幾個 token，而在它之前，光是「這個請求帶了工具」這件事就先扣掉 496 個。

現在再回頭想一下第 2 天文章的側錄過程，在正版 Claude Code 發出的那封信裡，83 個工具、加起來十五萬字元出頭而且每一輪對話都重送一次。工具越多、說明書寫得越詳細，模型就越會在正確的時機使用正確的工具，我們可能就會感覺這個 AI 很聰明。不過這也不難想像每一輪的固定開銷會越來越重，這筆帳之後講省錢的時候我再算給大家看。要是好奇那 83 個工具到底吃掉多少 token，你可以試著把第 2 天攔到的 `tools` 陣列從 JSON 檔裡挖出來，餵給 `count_tokens` 算算看，算完你就會知道 Claude Code 為什麼那麼在意快取。

先劇透一下，說明書還只是工具帳單的前菜。等明天工具真的開始運作之後，日記變厚的主力就會是是我跟模型的對話而是工具們的執行結果。例如執行一次讀檔如果讀了個大檔案，幾千個 token 就進了日記而且還會跟著每一輪重送，這樣不太行，所以之後會有一篇文章專門來寫怎麼幫這些肥大的資料減肥（我也需要減肥）。

## 看模型許願

```markdown
$ uv run --env-file .env probe.py
stop_reason: tool_use
模型想呼叫工具：get_time，參數：{}
```

先看 `stop_reason` ，第 3 天介紹過回應裡有這個欄位，記錄模型為什麼「停下來」，如果是一般的聊天它的值是 `end_turn` ，講完想講的話就收工。今天你會看到一個新的值 `tool_use` ，意思是「我話還沒說完，我在等一件事，幫我執行這個工具」。

到這裡不知道有沒有發現，一般人以為我們在用 AI 的時候是我們跟 AI 許願，結果 AI 在過程中也會跟我們許願，原來許願是互相的。

再來看那張許願單本人，也就是 `tool_use` 積木。第 3 天的文章有提到模型的回覆是一疊積木，當時我們只見過文字積木，現在有機會看到第二種，它的結構大概長這樣：

```json
{
  "type": "tool_use",
  "id": "toolu_01...",
  "name": "get_time",
  "input": {}
}
```

`name` 是模型點名要用的工具； `input` 是照著 `input_schema` 填好的參數表， `get_time` 不用參數，所以交回來一張白卷 `{}` ，但白卷也得交卷，這表示模型有看懂了表格的格式。

這裡的 `id` 是重要資訊，每一張許願單都有一個獨一無二的編號，先記得它的存在就好，明天的文章會再詳細介紹。簡單地說，在執行完工具、要回報結果的時候，就是靠這個 `id` 編號告訴模型「你上次那張 9527 號的單子辦好了，結果在這」。在跟 AI 對話的過程中可能同時有好幾張單子在飛，編號就是讓每張單子對得上號的憑證。這次回應裡，許願單的 `id` 是 `toolu_` 開頭，第 3 天執行 `print(resp)` 時看到的 message id 則是 `msg_` 開頭。這些前綴很方便人眼辨認，不過官方契約只要求每張 `tool_use` 有自己的 `id` ，沒有一定要什麼前綴開頭，所以我們在我們之後要寫的工具程式不要把前綴當成判斷依據， `id` 只要用在回報結果時原封不動拿來對號就好。

另外，在執行工具的時候你可能會看到 `tool_use` 前面還躺著一塊文字積木，模型先說了句類似「我來看一下時間」的話才開單。不一定每次都有，不過如果有的話你就會發現我在第 3 天提到的把回覆做成「一疊積木」的設計，就是為了讓文字和工具請求可以混在同一個回應裡。官方文件也提醒，這些開單前的口頭說明沒有固定格式，把它當一般文字處理就好，別依賴它的措辭。

呼應前一篇文章，存日記的時候要存完整的 `resp.content` ，如果只留文字的話接在後面的那張許願單就這樣被丟掉了。

## 一次許好幾個願

今天的實驗比較簡單，執行一次只會拿到一張許願單，但模型是可以一口氣開好幾張的。要是它手上剛好有時鐘跟氣象兩種工具，你問「現在幾點，順便查一下台北的天氣」，回應裡可能會同時躺著兩個 `tool_use` 積木，一個 `get_time` 、一個 `get_weather` ，這叫 parallel tool use，預設就是開著的。道理不難懂，兩件事互不相干，一次開兩張單、結果一起回，比一來一回跑兩趟省時間也省 token。

更常見的可能是另一種，就同一個工具開好幾張許願單，只是參數不同。在我那次實驗裡，同樣只掛一個 `get_weather` 工具，問「台北跟高雄的天氣如何」時它開了兩張， `city` 分別是 Taipei 跟 Kaohsiung；問「台灣六都的天氣」時它一口氣開了六張，六個都市一個不漏，而且它還知道是哪六個（我自己台灣人都不一定背得出來），每張都有自己的 `toolu_` 編號。所以「一次開好幾張」不是什麼罕見的特技，只要一句話裡包了好幾件同類的事，模型認為該開就可能會開。

是說不要把「開 6 張單」想成「跑 6 趟」。6 張是在同一個回應裡一次吐出來的，然後我們寫的工具實作程式會執行 6 次查詢，再把 6 次執行的結果裝進同一則訊息送回去，API 從頭到尾只往返兩趟。

執行工具的時候，模型開了 n 張許願單就要有 n 份執行結果，官方文件還有特別交代所有結果要裝在同一則 user 訊息裡一次回，不要拆成好幾則慢慢回。這條規矩 [排在官方排錯清單的第一位](https://platform.claude.com/docs/en/agents-and-tools/tool-use/parallel-tool-use#troubleshooting) ，用字很有意思：

> The most common issue is formatting tool results incorrectly in the conversation history. This "teaches" Claude to avoid parallel calls.

那個 teaches 還特別加了引號，意思是如果把結果拆開來回，等於默默訓練模型「下次別再並行了」，它會越來越不敢一次開多張單。當然這不是真的在訓練模型，模型的權重不會因為這樣而變化，再開個新對話就沒事了。模型是會看著你的行為調整自己的，這個規矩先記起來，下篇文章我會用「收一疊、回一疊」的方式來處理這件事。

## 模型的許願單

先講模型回來的 `input` 欄位，拆許願單的時候說 `input` 是模型照著 schema 填的表，但其實它填的內容不一定是百分百正確。

第一種狀況是模型自己腦補。官方文件有一段專門講「必填參數漏填時會怎樣」，假設天氣工具一定要填地點，但使用者只問了一句「天氣如何？」沒講在哪，比較謹慎的模型會反問你在哪裡，但有些模型可能直接推測一個合理的值填進去，官方自己舉的例子裡，模型就擅自填了 New York。文件自己也承認這種行為沒有保證，問題越模糊、模型越小顆，越容易發生。表格是它填的，值是它猜的，而我的程式收到的是一張看起來完整無缺的許願單，這個嘛...。

第二種狀況是格式偏差，預設情況下 API 並沒有保證 `input` 一定嚴格符合你的 schema，大多數時候沒問題但偶爾可能出現型別不對之類小狀況。官方為此提供了 strict 模式，在工具定義加上 `strict: true` 後，API 會用受限解碼（constrained decoding）讓模型只能產生符合 schema 的內容，不是等它亂填完才驗證放行。不過這保證的是結構，不保證城市名稱真的存在或使用者真的想查那裡；遇到輸出被截斷或拒答等例外也還是得處理。

這兩件事先知道就好， `get_time` 這個工具沒有參數所以暫時還碰不到這些問題，不過下篇文章的 `read_file` 就有一個 `file_path` 參數要填了，模型會把什麼路徑填進來、填錯要怎麼處理之類的防呆機制都得處理。說到底，許願單也是模型「生成」出來的東西，收單的人還是得驗證值跟業務規則，這很重要！

模型有工具不代表它就一定要用，你可試著把 `probe.py` 裡的問題換成一句用不到時間的話再跑一次就知道了，例如「跟我打個招呼」：

```markdown
$ uv run --env-file .env probe.py   # 問題改成「跟我打個招呼」
stop_reason: end_turn
模型說： 你好！很高興認識你！

有什麼我可以幫助你的嗎？無論是回答問題、提供資訊，還是只是聊天，我都很樂意為你效勞！
```

`stop_reason` 是 `end_turn` ，模型直接回話，一張單子都沒開。這句我跑了好幾次都沒開單，倒是有一、兩次它會在回答裡多補一句「如果你想知道現在幾點，或者有其他問題，我很樂意為你服務」，意思是它知道手上有時鐘，只是這次暫時用不上。

換一句更硬的：

```markdown
$ uv run --env-file .env probe.py   # 問題改成「1 + 1 等於多少？」
stop_reason: end_turn
模型說： 1 + 1 = **2**

這是基本的算術運算。一加一等於二。
```

一樣是 `end_turn` 。我另外還試了寫短詩、問 Python 的 list 跟 tuple 差在哪、解釋什麼是遞迴，也都沒開單。看起來很乖對吧？但我把問題換成「今天過得好嗎？」，同樣是一句寒暄問候也一樣沒提到時間，它就伸手了：

```markdown
$ uv run --env-file .env probe.py   # 問題改成「今天過得好嗎？」
stop_reason: tool_use
模型說： 我是一個 AI 助手，沒有真實的「一天」可以經歷，所以我沒有個人的感受。不過我很樂意幫助你！

讓我先看看現在幾點了：
模型想呼叫工具：get_time，參數：{}
```

看它話講到一半那句「讓我先看看現在幾點了」就知道發生什麼事了。模型想接著跟你聊，剛好手邊有時鐘，就順手拿起來用了。這句我跑了三次，三次都開單。

所以「有工具不代表非用不可」這句話是成立的，只是這個「界線」比想像中的模糊。要不要用工具是模型自己判斷的，關於這件事 [官方文件](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview#when-claude-uses-tools) 是這樣寫的：

> It calls a tool when the request maps to that tool's described capability and the answer isn't already in context. It responds directly for stable knowledge, creative tasks, and conversational turns.

意思是問題對得上某個工具描述的能力、而答案又不在眼前的對話裡，它才會開單；反之，穩定的知識、創作類的要求或純聊天，模型就會直接回答。回頭看剛才那幾個實驗，1 + 1 跟 Python 的 list、tuple 的差別是穩定的知識，寫短詩是創作，打招呼是純聊天全都沒開單。麻煩的是，「對不對得上」是由模型主觀認定，一句「今天過得好嗎」，在它看來就算對得上。所以前面才說 description 欄位是很重要的，這是模型判斷的依據，不過也就是「依據」而已，寫得再清楚都只是「影響」它，不是「控制」它。

這條判斷界線還可以用講的去移動。覺得模型太少用工具，在 system prompt 加一句「回答之前先用工具查證」模型就會積極起來，反之如果想保守一點，就寫「自行判斷要不要用工具」。我們現在連 system prompt 都還沒開始送（那是之後的主題），先知道就好。

到今天為止，系列裡已經碰到三個 `stop_reason` ： `end_turn` 是講完了、 `max_tokens` 是話太長被上限切斷、 `tool_use` 是在等你執行工具。這不是完整清單， [官方文件](https://platform.claude.com/docs/en/build-with-claude/handling-stop-reasons) 還列了其他狀況，例如 server tool 可能回 `pause_turn` 。以今天的流程來說，看到 `end_turn` 就是打收工，看到 `tool_use` 就去幹活。

## 強迫開單！

剛剛講的都是預設的 auto 模式，用不用工具由模型自己決定，不過還有個 `tool_choice` 參數可以控制：

- `auto` ：模型自己決定用不用，這是如果有帶工具時候的預設值
- `any` ：強制它一定要用工具，但用哪個它自己決定
- `tool` ：強制它用你指定的那一個
- `none` ：禁止它用任何工具，這是沒帶工具時的預設值

不管哪個參數都可以再掛一個 `disable_parallel_tool_use: true` ，限制模型一次最多開一張單，前面說的「一次許好幾個願」的機制就被關掉了。 `none` 看起來感覺很沒用其實有它的用途，例如一樣掛著工具，但這一輪就是想要跟模型純聊天，或是收尾請它總結的時候，就用得上。

但 `any` 跟 `tool` 這兩個是怎麼「強制」的？ [官方文件](https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools#forcing-tool-use) 寫的東西你看了可能會笑出來：

> Note that when you have `tool_choice` as `any` or `tool`, the API prefills the assistant message to force a tool to be used. This means that the models will not emit a natural language response or explanation before `tool_use` content blocks, even if explicitly asked to do so.

API 會「預填」一段 assistant 的開頭，讓模型只能從開單的格式接下去。有覺得面熟嗎？對，這就是我在上一篇文章裡用到的 prefill 把戲，同樣的招式只是這次換官方自己在用。因為開頭被塞死了，這兩檔模式下模型不會在單子前面講話，而且原文最後那句 even if explicitly asked to do so 講得很絕，你明著要它先解釋也沒用。

另外我在看模型價目表時有個小發現，就是「強迫」會比較貴一點點。剛有說到掛工具的時候會被自動加一段隱形的 system prompt，以 Haiku 4.5 來說 auto 模式是 496 個 token，但切到 any 或 tool 模式會變成 588 個。不信的話把剛才那段 `count_tokens` 加上 `tool_choice` 再量一次就好：

```markdown
tool_choice=auto  -> 634  (比不帶工具多 620)
tool_choice=any   -> 726  (比不帶工具多 712)
tool_choice=tool  -> 731  (比不帶工具多 717)
```

`auto` 跟 `any` 中間差 92 個 token，跟官方表格上 496 到 588 的落差剛好對得起來，多出來的那些大概就是「你必須用工具」的額外交代。再指定到某一個工具又多 5 個，那是工具名字自己的重量。其實是沒差多少啦，但就是有一些差別。

我在 KeSi 用預設的 `auto` 就好，agent 的精神本來就是讓模型自己安排工作，之後如果想逼模型一定用某種結構回覆的場合再說。

今天講的工具有個共同點，執行的人都是我自己寫的程式，官方文件把這類叫 client 工具，KeSi 就是一個 client。既然有 client 應該就有 server，官方的確有養一批 server 工具，負責執行像是網頁搜尋、網頁抓取、程式碼執行這些，模型許願之後由 Anthropic 自己的機房執行，多數結果會直接出現在回應裡，不用你自己動手。不過碰到 `pause_turn` ，或是 server tool 跟 client tool 混在同一批時，程式仍得照協定把回應送回去繼續。當然，這類工具的用量通常也另外計費。

client 的工具其實又有細分兩種，一種是我們今天寫的，名字、說明書、schema 全部自己寫，另一種是 Anthropic 預先定義、模型認得的工具規格。例如 [官方的 bash 工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/bash-tool) ，宣告時給 `{"type": "bash_20250124", "name": "bash"}` 兩個欄位就結束，連 `input_schema` 都不用自己附。不過這不代表它不佔 token，官方目前列的 `bash_20250124` 在 Claude 4.6 及更早的模型上會增加 244 個 token，另外還有啟用工具的 system prompt。省下來的是自己送 schema 的那一段，不是整把工具都免費。

KeSi 的工具全部會走自訂這條路，畢竟我們的重點就是把每個零件親手做一遍。之後有機會翻別人的 agent 原始碼，如果看到有工具沒寫 schema 別以為是偷懶，說不定是 Anthropic 預先定義的工具。

## 別人家的許願單

這套機制不是 Anthropic 的專利，業界更常聽到的 function calling 或 tool calling 是 OpenAI 那邊的稱呼，Google 的 Gemini 也有定位相同的功能。

如果你去翻 [Gemini 的文件](https://ai.google.dev/gemini-api/docs/function-calling) 就會發現它的 function declaration 看起來很眼熟， `name` 、 `description` 、 `parameters` 同一組三件套，參數一樣用 schema 描述。

天下文章一大抄... 啊，其實也不算抄，這是系出同源，大家都沒有自己發明一套語言來描述參數，Claude Code 用的是 JSON Schema，Gemini 文件上寫著支援 OpenAPI schema 的一個子集，而 OpenAPI 拿來描述資料結構的那套，本來就是從 JSON Schema 長出來的。本是同根生，所以長得像也很正常，哪天你要換接別家的模型，工具定義的心智模型幾乎都通用，要改的只有欄位名跟一些細節。因為大家在做的事都有點像，這也是為什麼後來會出現 MCP 這種東西，把「工具怎麼定義、怎麼提供」標準化成一個協定（protocol），讓同一套工具到處插拔，MCP 也是這個系列後段會接上的東西，先記得這個名字就好。

## 自訂工具，動手的是你的程式

講到這裡，最關鍵的觀念要再講一次。對 KeSi 這種 client tool 來說，模型負責輸出一張寫著願望的資料，不會直接在你的電腦上執行 `get_time` 或 `read_file` 。真正動手的是 client 程式；換成 server tool，動手的則是 Anthropic 的服務。

如果模型許願要刪掉某個資料夾呢？工具是你自己寫的話，程式可以看一眼路徑，覺得不對就拒絕，或是停下來問一句「確定嗎」；如果工具是 Claude Code 內建的，或是別人寫的，判斷的還是那支程式，它可以直接擋下來，也可以跳出通知讓你決定。願望要不要實現、怎麼實現、實現到什麼程度，決定權都在收單的人，也就是你手上。

拒絕之後呢？直接已讀不回嗎？也不是。在下一篇文章看到回報結果的格式就知道，「執行失敗」跟「我拒絕執行」都是用同一個管道回覆給模型的，它收到之後可能會換條路走、回頭再跟你聊聊，也可能直接說做不到。攔下來之後怎麼好好收尾，是之後做權限控制時候的重要議題。

第 1 天說過模型只會許願、你的程式握有最後的否決權。之後做權限控制、攔截危險指令，靠的全是這個結構。不是因為我做的 KeSi 多厲害才攔得住模型，是這個運作機制把「想」跟「做」拆開，模型負責想，真正負責做的那隻手從頭到尾都是我們的。

許願單拿到了，執行不就是一行 `datetime.now()` 的事嗎？對，執行很簡單，不過我先把進度停在這裡。真要挑的話連這一行都有陷阱，例如跑程式的機器跟你本人不一定在同一個時區，這種細節留給下一篇文章再來寫。

一來是想讓你把「許願單」的運作機制看清楚，這是整個 agent 最核心的關鍵；二來，接著要處理的不只是執行工具，還要把執行結果用正確格式包回去，憑工具編號對上那張許願單。模型收到結果之後還會接著說話，這一整圈就是第 1 天畫的那個 `while` 迴圈，下一篇文章再來詳細介紹。

工具的使用機制是內建在模型裡的，只要對話裡出現了帶著 `tool_use` 的模型回覆，下一則 user 訊息就必須帶著對應的回報結果，不然 API 會直接拒收整個請求。也就是說，一張沒有下文的許願單躺在日記裡，這本日記就再也送不出去了。今天我們只看不回，這種殘缺的對話不能進 `history` ，這就是我一開始另外開 `probe.py` 的原因。

那模型現在是不是在某個地方痴痴等著結果？沒有。第 3 天講過 Messages API 是無狀態的，伺服器那邊沒有什麼「等待中」的狀態，你覺得「它在等」只是你手上這包對話停在半空而已。要不要把結果送回去、什麼時候送都是你決定，如果不送出去的話這段對話就到此為止了。

這同時也代表 Messages API 不會替這張單子維持一個正在倒數的 session。晚一點才把完整歷史和對應的 `tool_result` 送回去，本質上只是之後再送一次請求。不過這不等於官方承諾許願單永遠有效，模型可能停用，API 契約也可能改。能確定的是，在你送出下一個請求前，伺服器上沒有一個 Claude 還坐在那裡苦等。

## 小結

今天這篇文章沒有做什麼真正有用的功能，KeSi 還是昨天那個 KeSi，但這裡我們把 agent 最核心的運作機制拿出來看，工具是隨請求送過去的說明書，模型被訓練成看得懂說明書而且會用固定格式開單許願。對 KeSi 的自訂工具來說，最後動手的是我們自己寫的程式；server tool 則由 Anthropic 執行。程式？別急，程式還沒寫，明天寫。

大家也看到了這張許願單的各種面貌，它可能一次來好幾張、填的內容可能是正確的也可能是自己腦補的，或是可以用 `tool_choice` 強迫或禁止，強迫的實作還是昨天玩過的 prefill。工具本身也有分身世，自己定義的、Anthropic 預先定義的、還有 Anthropic 代跑的 server 工具。對自訂工具來說，說明書是官方認證最值得花力氣的地方。

現在應該稍微比較有感覺這套 API 其實沒有藏什麼魔法，每個看起來新的功能，都是用基本招組合出來的。基本招式越熟，之後推出的新功能用猜的都能猜個八成。

在第一天的文章裡曾提到 KeSi 的四個零件：模型、迴圈、工具、context，工具這部份今天做了一半，上半場是模型許願使用工具，下半場就是我們自己要把工作實作出來了。官方文件裡有一篇 [從單一工具呼叫一路蓋到完整迴圈的教學](https://platform.claude.com/docs/en/agents-and-tools/tool-use/build-a-tool-using-agent) ，這篇我認為寫得滿好的，路線跟我們接下來要做的差不多，建議大家有空可以去看一下。

慢慢來，咱們下集見:)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401399)

[

Day 04 - 讓模型假裝有記憶

](https://ithelp.ithome.com.tw/articles/10401399)[下一篇](https://ithelp.ithome.com.tw/articles/10401693)

[

Day 06 - 第一個工具 read\_file

](https://ithelp.ithome.com.tw/articles/10401693)

系列文

[為你自己手刻 Claude Code](https://ithelp.ithome.com.tw/users/20065770/ironman/9031) 共 6 篇

目錄

1. 2
	[Day 02 - 側錄 Claude Code！](https://ithelp.ithome.com.tw/articles/10401028)
2. 3
	[Day 03 - 第一個 API 請求](https://ithelp.ithome.com.tw/articles/10401069)
3. 4
	[Day 04 - 讓模型假裝有記憶](https://ithelp.ithome.com.tw/articles/10401399)
4. 5
	[Day 05 - 模型怎麼使用工具](https://ithelp.ithome.com.tw/articles/10401655)
5. 6
	[Day 06 - 第一個工具 read\_file](https://ithelp.ithome.com.tw/articles/10401693)
[完整目錄](https://ithelp.ithome.com.tw/users/20065770/ironman/9031)

[![.](https://itadstatic.ithome.com.tw/B9/1784885238_6a632ff62c5c0.jpg)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9004)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/4/16/e04c7b02-307c-46c8-969b-4954d20f6dcc.jpg)

[Angular 17 全新檢測變更機制 - Signal](https://itplus.ithome.com.tw/webinar-page/219?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/5/22/bb663f88-7187-4983-af77-03696caf4fdc_small.jpg)

[Flowmon NDR 7x24 的人工智慧網路安全分析師如何快速掌握網路偵測與回應](https://itplus.ithome.com.tw/vod-page/834?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/16/2cdb4e33-8515-4513-8cf8-8d2f45a2065f_small.png)

[帳密悄悄洩，杜浦解危新戰略](https://itplus.ithome.com.tw/vod-page/936?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

27 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/20/aa29c768-8b39-4505-992c-f031b971a0ed_small.png)

[Angular Signals 快速上手：從入門到實戰](https://itplus.ithome.com.tw/vod-page/1400?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

90 分