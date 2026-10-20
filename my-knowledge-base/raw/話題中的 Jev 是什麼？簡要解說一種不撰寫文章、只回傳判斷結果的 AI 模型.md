---
title: "話題中的 Jev 是什麼？簡要解說一種不撰寫文章、只回傳判斷結果的 AI 模型"
source: "https://codelove.tw/@tony/post/anbj0a"
author:
published:
created: 2026-09-22
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------Jev 最近很有話題呢。  雖然被介紹成「不寫文章的 AI」，那麼它到底會回傳什麼呢？這篇文章是為了提高對 Je..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月19日](https://codelove.tw/@tony/post/anbj0a "2026-09-19 17:02:06")

## 話題中的 Jev 是什麼？簡要解說一種不撰寫文章、只回傳判斷結果的 AI 模型

## 前言

Jev 最近很有話題呢。  
雖然被介紹成「不寫文章的 AI」，那麼它到底會回傳什麼呢？

這篇文章是為了提高對 Jev 是做什麼的理解，而我自己整理出的內容。

## 簡要整理

Jev 是一種會讀取以文字描述的狀況，並針對預先準備好的選項判斷屬於哪一類，還會附上機率的模型。和會生成文章的 LLM 相比，它能接手的工作範圍較窄。相對地，由於不需要把答案寫成文章，回應速度更快，價格也更低。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790060230&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbj0a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuNTIiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuNTIiXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjUyIl1dLDBd&amp;dt=1790059437409&amp;bpp=4&amp;bdt=1396&amp;idt=4&amp;shv=r20260918&amp;mjsv=m202609170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790060209%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790060209%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790060209%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x969&amp;nras=3&amp;correlator=8438667299386&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=1037&amp;biw=1680&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95403098%2C95390278%2C122881146&amp;oid=2&amp;pvsid=6999550150450131&amp;tmod=33368715&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

- **發布方與時間** — 舊金山的 TypeSafe AI 於 2026 年 9 月 15 日以早期存取形式公開。這是該公司稱為「System One Model」的分類模型的第一個產品 [1](#fn-1)
- **輸入輸出** — 傳入描述情境的文字或 JSON（state）以及帶有型別的問題（questions），會回傳各選項的機率與信心度。不會生成文章
- **問題型別** — Choice（從選項中選 1 個）、Score（分級評分）、Noul（Yes/No 的機率）三種。可在一次呼叫中並行評估多個問題
- **標稱速度與價格** — 回應時間 70～500 毫秒，每 100 萬 token 輸入 0.042 美元，輸出免費（官方部落格數值）
- **適合用途** — 分類、分流、評分、驗證。官方明確寫著不擅長文章生成、計算、日期比較

## 記述式與劃卡式的比喻

我們用考卷的形式來看 Jev 和傳統 LLM 有什麼不同。

例如，假設把 EC 網站收到的商品評論交給 LLM，並要求它回答：「這則貼文應該先交給哪個團隊處理？請用 JSON 回答。」  
這比較接近讓模型寫出 **記述式** 答案的方式。  
模型會一個 token 一個 token 地依序寫出答案。  
接收的程式再讀取它，先確認格式有沒有壞掉，再把內容取出來。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790060230&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbj0a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuNTIiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuNTIiXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjUyIl1dLDBd&amp;dt=1790059437420&amp;bpp=2&amp;bdt=1408&amp;idt=2&amp;shv=r20260918&amp;mjsv=m202609170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790060209%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790060209%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790060209%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x969%2C952x280&amp;nras=4&amp;correlator=8438667299386&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=1962&amp;biw=1680&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95403098%2C95390278%2C122881146&amp;oid=2&amp;pvsid=6999550150450131&amp;tmod=33368715&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Jev 則比較像 **劃卡式** 答案。  
出題的程式先準備好選項，Jev 只回傳每個欄位應該塗多滿，以及各自有多高的把握。  
因為不需要把答案寫出來，而且能一次算出所有欄位的可能性，所以回應更快。  
也不會冒出沒事先準備的選項。

[![jev-fig1-essay-vs-marksheet.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F37b01e5d-c0d7-462d-9b49-951b42058b0c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=379b60541273b2ba9c13c515ebc6d515)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2F37b01e5d-c0d7-462d-9b49-951b42058b0c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=379b60541273b2ba9c13c515ebc6d515)

這張圖並排展示同一個問題用兩種方式回答的情況。  
左邊是 LLM 從頭到尾依序寫出 `{"team": "shipping"}` 這份答案，程式則按順序處理格式檢查與內容擷取。  
右邊則是 Jev 對程式準備好的 `shipping` 、 `product` 、 `payment` 、 `other` 四個欄位各自回傳機率。  
和紙本劃卡不同，並不是只塗一個欄位，而是所有欄位都有對應的把握程度。  
欄位塗色的深淺與旁邊的長條，都代表相同的機率。  
程式端的工作就只剩下讀取這些機率。

（圖中的機率只是為了說明而放的範例，token 的切分也簡化了。）

另一方面，即使是劃卡，也可能塗錯欄。  
官方部落格說型別錯誤是 0%，這指的是答案不會超出預先準備好的選項範圍。  
但在選項之中選錯，這種誤差仍然可能存在。

接下來把這個比喻換成實際的輸入輸出形式來看。

## state 與 questions 的輸入輸出

Jev 的請求由 **state** 和 **questions** 兩部分組成。

**state** 是希望模型判斷的情境。  
可以傳入字串，也可以傳入像 JSON 這樣的結構化資料。  
例如詢問內容、聊天紀錄、應用程式狀態等都可以放進去。

**questions** 是針對 state 的問題。  
每個問題都有型別，可從以下三種選擇：

- **Choice** — 從預先準備的選項中選 1 個。回傳的是被選中的選項、所有選項的機率、信心度
- **Score** — 依照由低到高排列的標準進行評分。至少要有 2 個等級。回傳的是機率加權後的值、各等級的機率、信心度
- **Noul** — 可用 Yes/No 回答的問題。回傳的是 Yes 的機率（0～1）

以下以剛才的商品評論為例，從是否為垃圾訊息、不滿程度、以及應該先交給哪個團隊處理這三個角度來看。  
（僅為示例。）

請求會像下面這樣：

```
{
  "model": "jev-latest",
  "state": "收到的箱子被壓扁了，裡面的馬克杯也有缺角。請幫我更換。",
  "questions": {
    "is_spam": {
      "type": "noul",
      "instructions": "這則貼文是不是宣傳或無關內容的垃圾訊息"
    },
    "dissatisfaction": {
      "type": "score",
      "instructions": "發文者的不滿程度",
      "criteria": ["沒有不滿", "有點不滿", "非常不滿"]
    },
    "team": {
      "type": "choice",
      "instructions": "這則貼文應該先交給哪個團隊處理",
      "criteria": {
        "shipping": "運送途中損壞、延遲、錯誤配送",
        "product": "商品本身的瑕疵或品質問題",
        "payment": "付款、退款、請款",
        "other": "以上皆非"
      }
    }
  }
}
```

回應時，會依照問題所命名的欄位傳回答案。

```
{
  "model": "jev-latest",
  "answers": {
    "is_spam": { "type": "noul", "noul": 0.02 },
    "dissatisfaction": {
      "type": "score",
      "score": 1.3,
      "legend": { "0": "沒有不滿", "1": "有點不滿", "2": "非常不滿" },
      "probabilities": { "0": 0.05, "1": 0.60, "2": 0.35 },
      "confidence": 0.74
    },
    "team": {
      "type": "choice",
      "choice": "shipping",
      "probabilities": { "shipping": 0.52, "product": 0.44, "payment": 0.01, "other": 0.03 },
      "confidence": 0.41
    }
  }
}
```

（請求與回應的格式是依照官方 API 參考文件整理的。數值只是為了說明而放的例子，並不是實際呼叫得到的結果。）

這 3 個問題會在一次呼叫中並行評估。  
增加問題數，也不會增加往返次數。

[![jev-fig2-state-questions.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fcbfb7448-0d40-4170-b92a-8ed24d1efe31.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=23570eeeaf153decb42f8f0006cc00e8)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fcbfb7448-0d40-4170-b92a-8ed24d1efe31.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=23570eeeaf153decb42f8f0006cc00e8)

上圖對應這個範例的請求與回應。  
一個 state 和三個不同型別的問題，在一次請求中送到 Jev。  
`is_spam` 回傳 Yes 機率 0.02， `dissatisfaction` 回傳 1.3， `team` 回傳 `shipping` 。  
`dissatisfaction` 的 1.3 是依照各等級機率加權後的值，表示它介於「有點不滿」（1）與「非常不滿」（2）之間。

在這個例子裡，從文字無法完全判斷箱子損壞到底是配送問題還是商品問題，所以 `team` 的機率在 `shipping` 和 `product` 之間分散了。  
這種分散情況，可以拿來作為程式中的分支條件。

## 依信心度在程式端分流

接下來看看 Jev 回傳的機率與信心度，要怎麼在程式端使用。

**信心度（confidence）** 是把各選項的機率分布濃縮成 0～1 的單一數值。  
如果機率集中在單一選項，信心度就高；如果分散到多個選項，信心度就低。

[![jev-fig3-confidence.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fd0e5f5d6-cf0d-4c1b-8ea9-da563cdbc217.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2f830183181d0b4f3e91a72a9a01c7b7)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fd0e5f5d6-cf0d-4c1b-8ea9-da563cdbc217.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2f830183181d0b4f3e91a72a9a01c7b7)

這裡並列了兩則關於 `team` 問題的評論答案。  
左邊是只抱怨物流延遲的評論範例。  
機率幾乎集中在 `shipping` 的 0.96，信心度是 0.94。  
右邊是前面提到的那則評論， `shipping` 和 `product` 的機率分散，信心度只到 0.41。  
（左邊的數值也只是說明用的範例。）

官方指南介紹了依照信心度分成三種處理方式的寫法。

```
team = answers["team"]

# 閾值依業務決定。這裡只是範例
if team["confidence"] >= 0.9:
    assign_to_team(team["choice"])          # 自動指派給對應團隊
elif team["confidence"] >= 0.5:
    show_candidates(team["probabilities"])  # 顯示候選項目，詢問承辦人員
else:
    send_to_triage_queue()                  # 交給人工分流
```

前面圖中的左側評論因為信心度是 0.94，所以會自動指派給對應團隊。  
信心度介於 0.5 到 0.9 之間的評論，則會把機率較高的候選項目提示給承辦人員。  
前面那則評論的信心度是 0.41，因此會被送到人工分流流程。  
這樣就能在一次分流大量詢問或貼文時，只把真的需要人工查看的內容交給人。

[![jev-fig4-decision-flow.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fb9b93159-4d96-4f06-b4ed-0c9b3fce2869.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fdb624f4cc398cd226bb03a3c8f32aea)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fb9b93159-4d96-4f06-b4ed-0c9b3fce2869.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fdb624f4cc398cd226bb03a3c8f32aea)

流程由程式決定，Jev 只負責回傳對問題的判斷。

能夠用閾值分流的前提是，官方說這些機率已經經過 **校準（calibration）** [2](#fn-2) 。  
校準過的機率，指的是當模型回答「80%」的判斷被收集起來時，大約有 8 成真的正確。

## 與 Structured Output、分類模型的差異

從選項中作答的方法，在 Jev 之前就已經存在。  
這裡把 LLM 的 Structured Output 與經過標籤訓練的專用分類模型並列，比較它們的差異。

方法出力答案的作法事前訓練信賴程度 **LLM 的自由文章** 文章 token 依序生成不需要以文中的詞彙表達 **LLM 的 Structured Output** 指定格式的 JSON token 依序生成不需要把輸出項目寫出來 **專用分類模型** 學過的標籤一次計算評分候選以帶標籤資料訓練各標籤的機率 **Jev** 各選項的機率一次計算評分候選不需要。問題可用自然語言撰寫機率與信心度

一次計算就對候選項目評分的作法，和分類模型的想法相同。  
也有人指出，既有的 LLM 也可以透過讀取下一個 token 的機率，做出類似的機制。  
而 Jev 的特色，主要在於以下幾點：

- **可以當場寫問題** — 以自然語言傳入問題與選項。不需要為每個任務準備訓練資料
- **可以一次評估多個問題** — 把同一個 state 的多個問題一起傳入，並行取得答案
- **預設就是拿機率來做分流** — 官方說其訓練方法以機率校準為目標

## 適合的用途與注意事項

從官方的使用案例清單中，挑一些比較容易聯想到實務情境的例子。

- **詢問分流** — 當電子郵件、聊天訊息、表單投稿進來時，決定負責人與緊急程度
- **貼文審核** — 針對每則貼文同時檢查是否為垃圾訊息、是否有攻擊性表達、是否帶有宣傳目的
- **搜尋結果重排** — 依照與問題的相關度，重新排序搜尋或 RAG 取出的候選結果
- **LLM 輸出驗證** — 評分另一個 LLM 的回答是否符合根據、是否偏離指示
- **代理流程分支** — 在不生成文章的情況下，決定工具選擇、繼續、重試或停止

官方文件中有一頁整理目前模型（jev-1.13）的弱點。  
這裡擷取與設計相關的部分：

**注意事項：**

- 會照字面理解問題，不會自行體會潛台詞。條件要在各選項中寫得具體
- 不可靠於計數與計算。計算請交給程式，Jev 只負責抽取值
- 不能把日期當成有前後關係的量來處理。日期請抽出各部件，比較交給程式
- state 中若有太多無關資訊，會降低準確度。只傳入問題需要的部分
- 不會生成文章。若想取出值，就把候選項目做成選項來讓它選

考量這些注意事項後，系統通常不會只讓 Jev 單獨完成所有處理，而是會和程式或 LLM 分工。  
官方文件也建議，像「超過 30 天就轉給催收部門」這種固定規則要寫在程式裡，只有像文字解讀這類部分才交給 Jev。

[![jev-fig5-division-of-roles.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fb3e08e9d-fd6a-48cf-a9c3-6b01df01beae.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b4abcf514dfc59a936f19b512151b72c)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3963468%2Fb3e08e9d-fd6a-48cf-a9c3-6b01df01beae.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b4abcf514dfc59a936f19b512151b72c)

這張圖中，程式位於核心位置，分別呼叫 Jev 與 LLM。  
與 Jev 的互動如前所述，傳入 state 與 questions，接收 answers。  
只有需要文章的部分，才交給 LLM。

對於標稱數值，也要先理解其前提。  
「快 193.6 倍、便宜 444.6 倍」這些數字，是 TypeSafe AI 以自家建立的工作流程評估所得，對照基準使用的是 GPT-6 Astra 與 Fable 5.1。  
在自己手上的題材上能差多少，還是需要實際測試。

## 試用方法

截至 2026 年 9 月 18 日，試用 Jev 的主要途徑有以下 3 種：

- **官方 API** — 到 [typesafe.ai](https://typesafe.ai/) 註冊候補名單，等待邀請。收到邀請後，可在控制台的 Playground 試用 state 與 questions。已公開的 SDK 有 Python（ `typesafe-sdk` ）與 JavaScript（ `@typesafe-ai/sdk` ）
- **Vercel AI Gateway** — 自 9 月 16 日起以 `typesafe-ai/jev` 提供。9 月 17 日時不包含在免費額度內，儲值後即可不用等邀請直接使用
- **Cloudflare Workers AI** — 以 `typesafe/jev` 提供。上下文長度為 32,000 token

## 總結

Jev 是一種不寫文章、而是回傳預先準備好之各選項機率的模型。  
因為答案附帶機率與信心度，所以可以由程式決定要自動進行到哪一步、從哪裡開始交給人處理。

官方明確說它不擅長文章生成、計算與日期比較，而是把分類、分流、評分、驗證這類小型判斷交給它。  
那些部分交給 LLM 或程式，Jev 則負責分類、分流、評分、驗證這類小型判斷。

由於才剛開始早期存取，標稱數值也還停留在自家評估階段。  
如果這篇文章能帶來哪怕一點參考價值，我會很高興。

## 參考資料

官方資訊

- [Introducing System One Models & Jev - TypeSafe AI Blog](https://typesafe.ai/blog/introducing-system-one-models-and-jev)
- [TypeSafe AI 文件](https://docs.typesafe.ai/introduction)
- [API reference](https://docs.typesafe.ai/api)
- [Confidence guide](https://docs.typesafe.ai/confidence)
- [How to build with System One](https://docs.typesafe.ai/concepts/how-to-build-with-system-one)
- [Use-case map](https://docs.typesafe.ai/concepts/use-case-map)
- [Jev 1.13 jaggedness](https://docs.typesafe.ai/model-jaggedness/jev-1.13)
- [TypeSafe AI's Jev now available on AI Gateway - Vercel](https://vercel.com/changelog/typesafe-ai-jev-now-available-on-ai-gateway)
- [Jev (typesafe) - Cloudflare AI docs](https://developers.cloudflare.com/ai/models/typesafe/jev/)

參考過的文章與資料

- [現在很紅的 AI「Jev」是什麼？宇宙最快上手會（みのるんさん的投影片）](https://speakerdeck.com/minorun365/konwadai-no-ai-jev-tte-nani-uchuu-saisoku-de-manabu-kai)
- [用 Jev 做 harness engineering（watany 先生）](https://zenn.dev/watany/articles/36e11a20ce3743)
- [備忘：System One Model / Jev（kun432 先生）](https://zenn.dev/kun432/scraps/7d699847974237)
- [TypeSafe AI「Jev」是什麼？（caen 先生）](https://zenn.dev/caen/articles/a629b80e76e193)
- [丟掉字串的模型。Jev 為什麼能快好幾個數量級（1amageek 先生）](https://zenn.dev/1amageek/articles/typesafe-jev-system-one-model)
- [nwn 先生的 Zenn 文章](https://zenn.dev/nwn/articles/824026c76116e0)
- [Jev means structured output is interesting again（Sean Goedecke）](https://www.seangoedecke.com/jev-means-structured-output-is-interesting-again/)
1. 依官方部落格所述，System One 這個分類名稱來自心理學家 Daniel Kahneman 的著作《快思慢想》中，快速直覺思考的「系統 1」。Jev 這個名字則源自經濟學家 William Stanley Jevons。這是把蒸汽機效率提升後帶動煤炭需求增加的現象，類比到機器智慧也會走上同樣道路的看法。
2. 官方部落格將這種學習方法稱為 Reinforcement Learning for Calibrated Decisions（RLCD）。截至 2026 年 9 月 18 日，尚未公開詳細說明的論文。

---

原文出處： [https://qiita.com/yushibats/items/472325ce548da370ed5d](https://qiita.com/yushibats/items/472325ce548da370ed5d)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/anbj0a) [526瀏覽](https://codelove.tw/@tony/post/anbj0a)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/anbj0a)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Jev是什麼？理解它與傳統型 LLM 與 AI 代理人的差異](https://codelove.tw/@tony/post/3DRvQq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [話題中的 Jev 是什麼？簡要解說一種不撰寫文章、只回傳判斷結果的 AI 模型](https://codelove.tw/@tony/post/anbj0a)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝64 💬1

857

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1790059436&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbj0a&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuNTIiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuNTIiXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjUyIl1dLDBd&amp;dt=1790059436360&amp;bpp=13&amp;bdt=375&amp;idt=86&amp;shv=r20260918&amp;mjsv=m202609170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790059430%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790059430%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790059430%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=8438667299386&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95403098%2C95390278%2C122881146&amp;oid=2&amp;pvsid=6999550150450131&amp;tmod=33368715&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=120" title="Advertisement" aria-label="Advertisement"></iframe>