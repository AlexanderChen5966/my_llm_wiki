---
title: "Claude API成本怎麼降？Anthropic公開3招省錢術，成本降至1/5、準確率反升"
source: "https://www.bnext.com.tw/article/92211/claude-api-cost-optimization-prompt-cache-effort-guide"
author:
  - "[[李先泰]]"
published: 2026-09-11
created: 2026-09-11
description: "Anthropic工程師拆出Claude API三種常見浪費：重複讀同樣的前綴、背著舊模型的提示詞包袱、模型想太多，每種都有對應開關，帳單可差數倍。"
tags:
  - "clippings"
status: processed
---
Claude API成本怎麼降？Anthropic近期 [公開](https://claude.com/blog/reducing-cost-and-improving-performance-with-claude-platform%29) 一套API成本優化方法，從Prompt Caching（提示快取）、提示詞審查到effort設定，目標都是減少模型不必要的token與推理成本。

Anthropic在一組客服基準測試中，把同一個應用從Opus 4.8搭配預設effort，一路調整到Sonnet 5搭配低effort，再加入任務分流規則。最後在驗收測試題上，\*\*準確率從78.6%升到90.5%，成本只剩原本約五分之一。\*\*這是Anthropic工程師Lance Martin在2026年9月8日公布的實測案例。

Anthropic把常見的成本浪費歸納成三種：

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

> 1. 每次API請求都重複處理相同內容
> 2. Prompt裡保留大量為舊模型撰寫的多餘指令
> 3. 簡單任務也使用過高的effort

對應的解法分別是 **Prompt Caching（提示快取）、Prompt Audit（提示詞審查）與Effort設定** 。以下逐一說明怎麼設定，以及各自適合什麼情境。

## Claude Prompt Caching怎麼用？用提示快取降低重複Token成本

先講什麼情況用得到。只要你的應用每次呼叫API時，開頭都帶著同一份說明或工具定義，例如一段幾千字的系統提示，這段每次都相同的內容就叫「前綴」。

舉例來說，一個客服機器人每次都會先送上同一份產品說明與退換貨規則，再接上使用者的問題，前面那份說明就是前綴。沒有快取時，模型每次都要重新處理一遍，也每次都照全價收費。

提示快取做的事，是把模型處理過這段前綴的中間結果暫存起來，下一次請求 **只要前綴完全一致，就直接沿用** ，這叫「快取命中」。它存的不是答案，而是處理過程的中間狀態，不會改變回答內容。

要注意的是「完全一致」就是字面上的意思： **前綴依tools、system、messages的順序建立，中間只要有一個字不同，後面全部得重算。**

要做的只有兩件事。

> 第一，把不會變的內容排在最前面，會隨對話增長的部分放最後。  
> 第二，在API請求頂層加一個cache\_control設定，系統會自動在最後一個可快取的位置設下斷點。

下面是示意寫法，不能直接執行。括號裡的中文說明代表你原本的內容，實際請求要換成自己的資料：

```
{
  "model": "claude-sonnet-5",
  "max_tokens": 4096,
  "cache_control": {"type": "ephemeral"},
  "tools": [ "（工具清單，例如查訂單、查庫存的功能定義，每次相同）" ],
  "system": "（系統提示，例如產品說明與退換貨規則，每次相同）",
  "messages": [ "（這次的對話內容，含使用者剛問的問題，每次不同）" ]
  }
```

對照前面說的排序原則：tools與system每次相同，就是會被快取的前綴；messages每次不同，放在最後。你要加的只有cache\_control那一行，其他欄位都是原本請求裡就有的，內容不用改。

價格方面，依 [Anthropic提示快取文件](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) 截至9月11日的規格，寫入快取時按基本輸入價的1.25倍計費，之後每次命中只收0.1倍，也就是原價的一折。token是模型計算文字用量的單位，快取有最低門檻，以Sonnet 5為例，前綴至少要1,024個token才會被快取，太短的提示詞用不到這項功能。

快取預設保留5分鐘，這段保留時間叫TTL，5分鐘內沒有新請求就會清掉；可以付2倍寫入價延長到1小時，但划不划算要看重複內容有多長、會被重用幾次、請求之間隔多久，不是由對話長短決定。同一段對話裡，前面已處理過的歷史內容也算前綴，一樣可以被重用。

最常見的失敗原因是系統提示裡放了時間戳或每次不同的編號，這等於在前綴最前面插了一個永遠對不上的內容，後面全部白算。設好之後不用自己猜有沒有命中，Claude Console的 [快取診斷](https://platform.claude.com/docs/en/build-with-claude/cache-diagnostics) 會顯示命中率與沒命中的原因。

## Claude Prompt怎麼優化？升級模型後先刪掉3種多餘指令

換新模型時，很多人只把模型名稱改掉，提示詞原封不動。問題是舊提示詞裡常有很多叮嚀，像「回答前再檢查一次」「一定要非常仔細」「請照這幾個步驟想」，那是以前模型不夠聰明時加的保險。

新模型自己就會推理，這些叮嚀變成多餘的工作。Lance Martin舉例，「再檢查一次」會讓模型把同一筆訂單查兩遍；兩條互相矛盾的退款規則，會讓它不敢退本來該退的錢。Anthropic實測，把這類叮嚀刪掉後，同一個新模型平均成本降14.6%、準確率升5.3%。

做法是升級前把提示詞從頭讀一遍。跟業務有關的規則要留，例如退款前要確認訂單狀態；純粹叫模型「再想一次」「再確認一次」的可以刪。用Claude Code的人，可以直接跑 **/claude-api prompt-audit** ，它會把這類叮嚀找出來。

## Claude Effort怎麼設定？簡單任務不用每次都開High

**Claude API的effort設定，用來控制模型在任務上投入多少推理資源。** 等級越高，模型通常會投入更多推理與工具使用，因此也可能帶來更高成本。

不設定的話，預設是high，也就是幾乎每個任務都會投入較多推理。如果只是分類、摘要、格式轉換等簡單任務，使用high就可能花掉不必要的成本。

要調整，只要在API請求裡加一個output\_config欄位，填low、medium、high、xhigh、max其中一級。示意如下，省略號代表你原本的內容：

```json
{
  "model": "claude-opus-5",
  "max_tokens": 4096,
  "messages": [ ... ],
  "output_config": {"effort": "medium"}
}
```

要改的只有effort那個值。要留意的是，effort調低不等於回答變短，它影響的是模型思考和查資料的多寡，不是字數。

該調到哪一級，沒有公式，只能試。拿一組固定的題目，每一級都跑一遍，看準確率從哪一級開始明顯掉下來，就停在它的前一級。Anthropic自己測出一個有趣的結果：新模型Fable 5.1用low，成績和前一代Fable 5用high差不多，費用卻只要三分之一。

也就是說，換新模型時不用急著開高effort，先從低的試起可能更划算。

兩個提醒。第一，調低之後要確認答案品質有沒有掉，尤其是需要多想幾步的任務，太低會讓模型還沒查完就下結論。第二，同一段對話中途改effort，前面存好的快取會失效，所以一開始就選定，不要邊聊邊改。

## Claude API成本怎麼評估？用固定測試題比較準確率與費用

三招做完，要拿同一組固定的題目再跑一次，看費用有沒有降、答案有沒有變差。Anthropic的做法是把題目分兩份，一份拿來反覆調設定，另一份留到最後驗收，避免設定只對練習題有效。

用Claude Code的人可以直接跑 **/claude-api hillclimb** ，它會自動反覆試設定； **/claude-api cost-optimize** 則是先幫你盤點錢花在哪，再逐項試省錢做法。

### Claude Message Batches：非即時任務還能再省50%

還有一招適合不急著拿結果的工作，例如凌晨把一整天的客服對話統一做摘要。改用Message Batches API把請求打包送出，多數1小時內完成，最慢24小時，費用直接打五折，還能和快取折扣疊加。

最後提醒，文中的省錢幅度都是Anthropic自己測的，你的情況不一定一樣。建議順序是先看錢花在哪，再清提示詞，接著試effort，最後才用批次，每改一項就驗一次。

> 延伸閱讀： [Claude官方課程有中文版了！22門全免費，涵蓋入門到API開發，完課還能拿證書](https://www.bnext.com.tw/article/91963/claude-academy-chinese-courses)

## Claude API省錢3招怎麼選？

如果每次API請求都有大量重複內容，先使用Prompt Caching；如果是從舊模型升級，先檢查Prompt裡是否留下多餘指令；如果大量任務只是分類、摘要或格式轉換，則可以優先測試降低effort。

三種方法不需要一次全部套用。比較穩妥的方式，是先建立一組固定測試題，每改一項設定，就重新比較成本與準確率，確認省下來的token沒有換來明顯的品質下降。

> 延伸閱讀：  
> [Gemini Notebook別只拿來摘要！8組讀書指令，讓AI幫你出題、抓重點、做模擬考](https://www.bnext.com.tw/article/92111/notebooklm-study-prompts-exam-prep)  
> [Gemini Notebook教學地圖！AI筆記本、簡報製作、財報解讀27篇精華用法一次看](https://www.bnext.com.tw/article/91193/notebooklm-complete-guide-and-use-cases)

資料來源： [Anthropic官方部落格](https://claude.com/blog/reducing-cost-and-improving-performance-with-claude-platform) 、 [Anthropic提示快取文件](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) 、 [Anthropic快取診斷文件](https://platform.claude.com/docs/en/build-with-claude/cache-diagnostics) 、 [Anthropic effort文件](https://platform.claude.com/docs/en/build-with-claude/effort) 、 [Anthropic批次處理文件](https://platform.claude.com/docs/en/build-with-claude/batch-processing)

本文初稿為AI編撰，整理．編輯/ 李先泰