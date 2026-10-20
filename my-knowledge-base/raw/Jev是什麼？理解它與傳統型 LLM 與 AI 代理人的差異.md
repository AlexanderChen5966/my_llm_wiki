---
title: "Jev是什麼？理解它與傳統型 LLM 與 AI 代理人的差異"
source: "https://codelove.tw/@tony/post/3DRvQq"
author:
published:
created: 2026-09-18
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言=============================================**Jev 是一種 AI 模型，會讀取文章與業務資料，並將分類、評分、條件判定的結果以附帶機率的形式回傳。**模型..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月18日](https://codelove.tw/@tony/post/3DRvQq "2026-09-18 08:02:13")

## Jev是什麼？理解它與傳統型 LLM 與 AI 代理人的差異

## 前言

**Jev 是一種 AI 模型，會讀取文章與業務資料，並將分類、評分、條件判定的結果以附帶機率的形式回傳。**

模型回傳的結果，預期會用於應用程式需要分岔處理流程的場景。程式可以明確判定條件，並應用於複雜的控制流程。

在 Jev 發表之後，市場上出現了彷彿 Jev 會取代傳統 LLM 以及周邊 AI Agent 技術的熱潮；那麼，Jev 與傳統技術究竟有什麼差異呢？本文將冷靜地拆解其中的不同之處。本文以 AWS 的 Amazon Bedrock AgentCore 作為傳統技術的例子。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789694845&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRvQq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789694264578&amp;bpp=3&amp;bdt=1264&amp;idt=3&amp;shv=r20260917&amp;mjsv=m202609140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789694263%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1592x819&amp;nras=3&amp;correlator=2575331058738&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=987&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=145&amp;ady=877&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=0&amp;eid=95401792%2C95403098&amp;oid=2&amp;pvsid=115747884175273&amp;tmod=1012600094&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1592%2C819&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## Jev 的首次公開

TypeSafe AI 於 2026 年 9 月 15 日發表了 Jev，作為該公司稱為「 **System One Model** 」的 AI 模型系列中的第一個公開模型。

System One 這個名稱源自丹尼爾・康納曼所描述、快速且直覺地做判斷的「系統 1」；而 Jev 的名稱則是取自以效率提升反而導致資源使用擴大現象聞名的經濟學家威廉・史丹利・傑文斯。

## 傳統 LLM 與 Jev 的差異

TypeSafe AI 在發表 Jev 時，強調的是 **應用程式容易使用的 AI 輸出** 。

在業務系統中，有許多判斷需要做，例如分類詢問內容、確認文件是否符合條件、選擇下一步處理等等。 **Jev 的設計，就是將這類判斷以帶型別的值回傳，讓程式可以直接使用。**

光看這段說明可能還是有點抽象，我們用一個更容易理解的例子來說明。假設顧客寄來了以下電子郵件：

> **關於剛才的訂單，我被重複請款了。請退還重複請款的部分。上週我也有詢問，但還沒有收到回覆。**

那麼，系統應該如何決定要執行什麼處理呢？我們來比較一下：一個是將傳統 LLM 與 AI Agent 結合實作的系統，另一個是將 Jev 與非 AI Agent 的應用程式結合實作的系統。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789694845&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRvQq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789694264588&amp;bpp=1&amp;bdt=1274&amp;idt=1&amp;shv=r20260917&amp;mjsv=m202609140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789694263%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1592x819%2C952x280&amp;nras=4&amp;correlator=2575331058738&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=987&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=145&amp;ady=1825&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=0&amp;eid=95401792%2C95403098&amp;oid=2&amp;pvsid=115747884175273&amp;tmod=1012600094&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1592%2C819&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 傳統 LLM + AI Agent 的情況

請想像一個使用 Amazon Bedrock AgentCore 等方式實作 AI Agent 並運營系統的情境。

AI Agent 會將顧客的電子郵件、處理歷程、可用工具、退款規則等資訊作為輸入交給 LLM。LLM 讀取內容後，可能會決定依照以下流程處理，並以字串形式回傳相當於指令文的內容：

```
* 選擇取得訂單資訊與請款紀錄的工具
* 讀取取得的結果，判斷是否為重複請款
* 確認退款規則，並選擇退款或送出核准申請的工具
* 根據執行結果撰寫回覆內容
```

在上面的例子中，LLM 會依照情境選擇下一個工具與處理順序。開發者預先串接好工具、設定權限與核准條件，LLM 則在這些範圍內進行判斷。

## Jev + 非 AI Agent 的情況

相對地，Jev 會針對給定的狀態，回傳對預先定義問題的選擇結果與機率。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789694845&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRvQq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789694264593&amp;bpp=1&amp;bdt=1279&amp;idt=1&amp;shv=r20260917&amp;mjsv=m202609140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789694263%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1592x819%2C952x280%2C952x280&amp;nras=5&amp;correlator=2575331058738&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=987&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=145&amp;ady=2662&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=0&amp;eid=95401792%2C95403098&amp;oid=2&amp;pvsid=115747884175273&amp;tmod=1012600094&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1592%2C819&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

首先，事先定義好「詢問類型」的選項；接著再定義「是否有退款要求」與「是否為再次詢問」這些問題。

在這種情況下，當送出評估前述電子郵件的請求時，預期 Jev 會回傳如下結果。

以下回應是為了幫助理解 Jev 的概念，參考官方文件後，基於方便說明所做的模式化呈現。

```
{
  "詢問類型": {
    "選擇結果": "重複請款",
    "機率": {
      "重複請款": 0.98,
      "配送": 0.01,
      "其他": 0.01
    }
  },
  "有退款要求的機率": 0.99,
  "是再次詢問的機率": 0.97
}
```

表面上看起來，這似乎只是把傳統 LLM 原本就能做到的事情重新發明一次，但 **Jev 所扮演的角色其實只有「處理路由」** 。Jev 回傳的結果是以靜態數值呈現，而程式只要評估這個數值，然後執行數值最高對應的處理即可。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789694855&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRvQq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789694264600&amp;bpp=2&amp;bdt=1286&amp;idt=2&amp;shv=r20260917&amp;mjsv=m202609140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789694263%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1592x819%2C952x280%2C952x280%2C952x280%2C350x280%2C1005x124%2C334x250&amp;nras=8&amp;correlator=2575331058738&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=987&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=145&amp;ady=3602&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=340&amp;eid=95401792%2C95403098&amp;oid=2&amp;psts=AOrYGsmqpgoaoAJlkwLSQMEelxx9WWS8yuuMG4vJF4R5_A6LQ7H250xU6tjf0pMWOEFq2aKmo0l4qfR0G22AbrzKbHP2SARR0ee1s2qkmhANp88GW-MGeVfzztJcZHTEax2o%2CAOrYGsnivjlL5HFkDFJ-dm3pMqV4XnqGvPheHcAr1_GT1z3WeKE8dYmsLqIUpz4JGM3U8-E8chQu3gQVrdCFHA2y26k82MBQZYXVLcbOAly06-dgyw%2CAOrYGskgrERSalS20WcJNfJMEsBwZb0f36YC9QFxsnmjSiSyx_YcAkXmHORbnFWYlyzkbdB_oCuAKLH-dLln_-5SfEM90c38CaSiLL5HmHJFQ0PbSw%2CAOrYGslbIlcV2A4RR5n4k3ftfDU25qlIZShFGlyXmRNj5U8alPvUNUaM_3oBFJXvhqYgZd1-0aCCRsm5jz6uWbGUW7snb4WoZely3Nm7niM9BeCt-JE%2CAOrYGsnlZzYmmnnWqYfvcU-PLZA_IslsXg4RUFgWgrmYguM_KU6AUhPYXJwMOMK6cAa7oeEjmuXe1mBjlOikY3kFVWC67FdixKetFyRmg9GPAnSkVg&amp;pvsid=115747884175273&amp;tmod=1012600094&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=2008%2C82%2C2008%2C82%2C1920%2C30%2C1832%2C900%2C1592%2C819&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

當然，Jev 回傳的數值每次都可能不同。不論回傳什麼值，程式只要評估這些值，透過傳統比較運算決定要呼叫哪個工具即可。

在傳統 AI Agent 中，是由 LLM 決定要呼叫哪個工具，但其判斷邏輯非常像黑盒子。Jev 的回應雖然也是以機率決定，但判斷邏輯可以在程式碼中清楚定義。

不過，由於 LLM 也可以輸出包含機率的相同格式，因此若只看可生成的資訊，兩者其實沒有決定性的差異。

Jev 所主張與傳統 LLM 的不同之處在於， **模型是針對這類分類、機率輸出而特化訓練與設計** ，因此可以更快速、低成本地執行。若只是根據模型分類的結果來處理，程式只需要呼叫 Jev 模型，並依據結果進行條件分支即可，不需要像 Amazon Bedrock AgentCore 這類 AI Agent 基礎架構。

## 「LLM + AI Agent」vs「Jev + 非 AI Agent」

雖然前面將 Jev 描述得像是可取代傳統技術，但傳統 LLM 搭配 AI Agent 的方案當然也有更適合的情境；其差異可如下表所示。

| 觀點 | 結合 Jev 的業務應用程式 | 以 LLM 為核心的 AI Agent |
| --- | --- | --- |
| 交由 AI 處理的內容 | 分類、條件是否符合的判定、評分 | 情境解讀、下一步行動或工具的選擇 |
| 交由程式定義的內容 | 對應判定結果的處理，以及其執行條件與順序 | 可用工具、權限、核准條件、執行上的限制 |
| 下一步處理的決定方式 | 接收 AI 的判定結果後，透過寫在程式中的條件分支決定 | 由 LLM 根據指令、對話歷史、工具執行結果來選擇 |
| 客戶應對範例 | 若判定為退款要求，就進入預先定義好的請款確認與核准流程 | 視情況選擇請款確認、追加提問、交接給客服人員等 |

在結合 Jev 的應用程式中，AI 會對輸入內容進行分類與評估，接著程式依據結果執行預先定義好的處理。應用程式程式碼本身並沒有代理式的處理流程。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789694869&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRvQq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789694264619&amp;bpp=2&amp;bdt=1306&amp;idt=2&amp;shv=r20260917&amp;mjsv=m202609140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789694263%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1592x819%2C952x280%2C952x280%2C952x280%2C350x280%2C1005x124%2C334x250%2C952x280&amp;nras=9&amp;correlator=2575331058738&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=987&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=145&amp;ady=4908&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=1639&amp;eid=95401792%2C95403098&amp;oid=2&amp;psts=AOrYGsmqpgoaoAJlkwLSQMEelxx9WWS8yuuMG4vJF4R5_A6LQ7H250xU6tjf0pMWOEFq2aKmo0l4qfR0G22AbrzKbHP2SARR0ee1s2qkmhANp88GW-MGeVfzztJcZHTEax2o%2CAOrYGsnivjlL5HFkDFJ-dm3pMqV4XnqGvPheHcAr1_GT1z3WeKE8dYmsLqIUpz4JGM3U8-E8chQu3gQVrdCFHA2y26k82MBQZYXVLcbOAly06-dgyw%2CAOrYGskgrERSalS20WcJNfJMEsBwZb0f36YC9QFxsnmjSiSyx_YcAkXmHORbnFWYlyzkbdB_oCuAKLH-dLln_-5SfEM90c38CaSiLL5HmHJFQ0PbSw%2CAOrYGslbIlcV2A4RR5n4k3ftfDU25qlIZShFGlyXmRNj5U8alPvUNUaM_3oBFJXvhqYgZd1-0aCCRsm5jz6uWbGUW7snb4WoZely3Nm7niM9BeCt-JE%2CAOrYGsnlZzYmmnnWqYfvcU-PLZA_IslsXg4RUFgWgrmYguM_KU6AUhPYXJwMOMK6cAa7oeEjmuXe1mBjlOikY3kFVWC67FdixKetFyRmg9GPAnSkVg%2CAOrYGskkMYmW3NaSh_L5uw3AGyoUYS0XfNJMyMXo27ov6m2mmxociC3DYtDyQ4ckUuTyvkP402eYbehlJlQ5Kzs73mF7h3bPZI_ZJqiYZhWMICZJuw&amp;pvsid=115747884175273&amp;tmod=1012600094&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=2008%2C82%2C2008%2C82%2C1920%2C30%2C1832%2C900%2C1592%2C819&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

以 LLM 為核心的 AI Agent，則是在給定權限與限制的範圍內，由 AI 根據上下文選擇下一個要使用的工具與處理順序。

**簡單來說，差別就在於：由程式還是由 AI 來負責決定處理流程。**

## 總結

**Jev 是一種能讀取輸入語意，並將分類與條件判定結果以附帶機率的形式回傳的 AI 模型** 。當考慮將其整合進業務流程時，重點在於程式所要執行的處理必須明確。以客服應對為例，Jev 會判定詢問內容與是否有退款要求（以機率輸出），因此需要在程式碼中具備判定邏輯，並事先定義好請款確認、核准等流程。

另一方面，在以 LLM 為核心的 AI Agent 中，會根據對話歷史與工具執行結果，由 AI 自行決定接下來要確認什麼、要使用哪個工具。雖然非常彈性，但即使執行相同的處理，每次所需的思考時間與上下文消耗都可能不同，進而影響成本。

使用 Jev，可望在維持所需品質的前提下，於應用程式端以低延遲、低成本的方式處理反覆發生的判斷與流程。只要讓 AI 輸出數值，接下來就交給固定的既定處理即可。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=4060917935&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789694871&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRvQq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789694264626&amp;bpp=1&amp;bdt=1312&amp;idt=1&amp;shv=r20260917&amp;mjsv=m202609140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789694263%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1592x819%2C952x280%2C952x280%2C952x280%2C350x280%2C1005x124%2C334x250%2C952x280%2C952x280&amp;nras=10&amp;correlator=2575331058738&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=987&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=145&amp;ady=5736&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=2478&amp;eid=95401792%2C95403098&amp;oid=2&amp;psts=AOrYGsmqpgoaoAJlkwLSQMEelxx9WWS8yuuMG4vJF4R5_A6LQ7H250xU6tjf0pMWOEFq2aKmo0l4qfR0G22AbrzKbHP2SARR0ee1s2qkmhANp88GW-MGeVfzztJcZHTEax2o%2CAOrYGsnivjlL5HFkDFJ-dm3pMqV4XnqGvPheHcAr1_GT1z3WeKE8dYmsLqIUpz4JGM3U8-E8chQu3gQVrdCFHA2y26k82MBQZYXVLcbOAly06-dgyw%2CAOrYGskgrERSalS20WcJNfJMEsBwZb0f36YC9QFxsnmjSiSyx_YcAkXmHORbnFWYlyzkbdB_oCuAKLH-dLln_-5SfEM90c38CaSiLL5HmHJFQ0PbSw%2CAOrYGslbIlcV2A4RR5n4k3ftfDU25qlIZShFGlyXmRNj5U8alPvUNUaM_3oBFJXvhqYgZd1-0aCCRsm5jz6uWbGUW7snb4WoZely3Nm7niM9BeCt-JE%2CAOrYGsnlZzYmmnnWqYfvcU-PLZA_IslsXg4RUFgWgrmYguM_KU6AUhPYXJwMOMK6cAa7oeEjmuXe1mBjlOikY3kFVWC67FdixKetFyRmg9GPAnSkVg%2CAOrYGskkMYmW3NaSh_L5uw3AGyoUYS0XfNJMyMXo27ov6m2mmxociC3DYtDyQ4ckUuTyvkP402eYbehlJlQ5Kzs73mF7h3bPZI_ZJqiYZhWMICZJuw%2CAOrYGskAuh4v-7dGrh6xVscnzvWk5iQFFqvB7FDI7Ygy4iYk9dcXYfbZoBdrwzTdJpRFreV6aYZn9dblUs9GftH9lepN0YMQQnuSR8jr-LXyVK9rXA&amp;pvsid=115747884175273&amp;tmod=1012600094&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=2008%2C82%2C2008%2C82%2C1920%2C30%2C1832%2C900%2C1592%2C819&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

如本文所介紹，將「LLM + AI Agent」與「Jev + 非 AI Agent」進行比較，會更容易理解。了解各自的特點並妥善分工使用，是很重要的。

## 參考資料

- [TypeSafe AI — Introducing System One Models & Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev)
- [TypeSafe AI — Introduction](https://docs.typesafe.ai/introduction)
- [TypeSafe AI — Primitives (Questions)](https://docs.typesafe.ai/primitives)
- [TypeSafe AI — AI primer](https://docs.typesafe.ai/introduction/machine-learning-primer)
- [TypeSafe AI — Confidence](https://docs.typesafe.ai/confidence)
- [TypeSafe AI — Workflow evals](https://evals.typesafe.ai/)

---

原文出處： [https://qiita.com/nasuvitz/items/44f20ed515b9734afb26](https://qiita.com/nasuvitz/items/44f20ed515b9734afb26)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3DRvQq) [5瀏覽](https://codelove.tw/@tony/post/3DRvQq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3DRvQq)

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

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [Codex 怎麼買、怎麼充值？先把這兩套計費搞清楚](https://codelove.tw/@tony/post/q44PLq)

🔥 [Codex 新手入門：從安裝到外掛、MCP、Skills，一篇把設定講清楚](https://codelove.tw/@tony/post/3ojwXx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝42 💬1

603

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1200" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=3809598800&amp;adf=3132389021&amp;pi=t.aa~a.1416458249~rp.1&amp;abgtt=6&amp;w=1200&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789694879&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=1200x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRvQq&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789694264660&amp;bpp=1&amp;bdt=1347&amp;idt=0&amp;shv=r20260917&amp;mjsv=m202609140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789694263%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789694263%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1592x819%2C952x280%2C952x280%2C952x280%2C350x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=2575331058738&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=987&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=196&amp;ady=7229&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=3955&amp;eid=95401792%2C95403098&amp;oid=2&amp;psts=AOrYGsmqpgoaoAJlkwLSQMEelxx9WWS8yuuMG4vJF4R5_A6LQ7H250xU6tjf0pMWOEFq2aKmo0l4qfR0G22AbrzKbHP2SARR0ee1s2qkmhANp88GW-MGeVfzztJcZHTEax2o%2CAOrYGsnivjlL5HFkDFJ-dm3pMqV4XnqGvPheHcAr1_GT1z3WeKE8dYmsLqIUpz4JGM3U8-E8chQu3gQVrdCFHA2y26k82MBQZYXVLcbOAly06-dgyw%2CAOrYGskgrERSalS20WcJNfJMEsBwZb0f36YC9QFxsnmjSiSyx_YcAkXmHORbnFWYlyzkbdB_oCuAKLH-dLln_-5SfEM90c38CaSiLL5HmHJFQ0PbSw%2CAOrYGslbIlcV2A4RR5n4k3ftfDU25qlIZShFGlyXmRNj5U8alPvUNUaM_3oBFJXvhqYgZd1-0aCCRsm5jz6uWbGUW7snb4WoZely3Nm7niM9BeCt-JE%2CAOrYGsnlZzYmmnnWqYfvcU-PLZA_IslsXg4RUFgWgrmYguM_KU6AUhPYXJwMOMK6cAa7oeEjmuXe1mBjlOikY3kFVWC67FdixKetFyRmg9GPAnSkVg%2CAOrYGskkMYmW3NaSh_L5uw3AGyoUYS0XfNJMyMXo27ov6m2mmxociC3DYtDyQ4ckUuTyvkP402eYbehlJlQ5Kzs73mF7h3bPZI_ZJqiYZhWMICZJuw%2CAOrYGskAuh4v-7dGrh6xVscnzvWk5iQFFqvB7FDI7Ygy4iYk9dcXYfbZoBdrwzTdJpRFreV6aYZn9dblUs9GftH9lepN0YMQQnuSR8jr-LXyVK9rXA%2CAOrYGskhjx74Rw2qYzZ3FWMlEahIv_2zXWVfi-ALC2Yez-pxtvS-nxCEAxs-PJ4KGWjFeIhcucUDE-Pp3ExIwTbNdzh8phc8T2OIEWaxoROunDUo&amp;pvsid=115747884175273&amp;tmod=1012600094&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=2008%2C82%2C2008%2C82%2C1920%2C30%2C1832%2C900%2C1592%2C819&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1789694263&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRvQq&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789694263490&amp;bpp=8&amp;bdt=176&amp;idt=82&amp;shv=r20260917&amp;mjsv=m202609140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789616931%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789616931%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789616931%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=2575331058738&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=987&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=0&amp;eid=95401792%2C95403098&amp;oid=2&amp;pvsid=115747884175273&amp;tmod=1012600094&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1592%2C819&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=145" title="Advertisement" aria-label="Advertisement"></iframe>