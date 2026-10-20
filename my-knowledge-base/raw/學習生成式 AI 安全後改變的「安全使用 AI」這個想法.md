---
title: "學習生成式 AI 安全後改變的「安全使用 AI」這個想法"
source: "https://codelove.tw/@tony/post/ag2dE3"
author:
published:
created: 2026-09-22
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------目前我正朝著 Web 工程師的目標持續學習，也同時在學 AI 驅動開發。這次有機會透過 **Happiness Cha..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月21日](https://codelove.tw/@tony/post/ag2dE3 "2026-09-21 17:02:11")

## 學習生成式 AI 安全後改變的「安全使用 AI」這個想法

## 前言

目前我正朝著 Web 工程師的目標持續學習，也同時在學 AI 驅動開發。這次有機會透過 **Happiness Chain 的課程學習生成式 AI 資安** ，並學到了 Prompt Injection、Jailbreak、資訊外洩、RAG 的安全性，以及企業在使用 AI 時的防護機制等內容。

在上課之前，我對生成式 AI 的安全，大致只有「不要輸入個資或機密資訊」「不要全盤相信 AI 的回答」這樣的印象。但隨著學習深入，我開始思考的範圍也變廣了：像是「這些資訊到底能不能交給 AI」「如果讀入的資料裡含有惡意指令會怎樣」「在 RAG 裡，誰可以搜尋到哪些資訊」等等。

這篇文章不是要整理課程內容本身，而是想回顧 **透過學習後，我對這些議題的理解如何改變，以及我如何整理自己產生的疑問** 。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790060247&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2dE3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuNTIiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuNTIiXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjUyIl1dLDBd&amp;dt=1790059500835&amp;bpp=10&amp;bdt=1345&amp;idt=10&amp;shv=r20260918&amp;mjsv=m202609170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790060209%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790060209%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790060209%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x969&amp;nras=3&amp;correlator=268458292828&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=1005&amp;biw=1680&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95403098%2C122881146%2C95340252%2C95340254&amp;oid=2&amp;pvsid=3731091999409375&amp;tmod=33368715&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 1\. 在把資訊交給 AI 之前，先思考什麼

我最先明顯改變的認知，是 **要怎麼處理輸入給 AI 的資訊** 。以前的我，對把個資輸入 AI 的風險，主要想的是：「如果輸入的資訊外洩並被濫用，那就很危險。」

但在學習過程中，我發現「會不會外洩」和「這些資訊本來是否就可以交給外部 AI 服務」其實必須分開看。

例如，假設你用 AI 來幫忙安排和朋友的旅行行程。你可能會想：「只是要整理行程而已」，於是把朋友傳來的姓名、聯絡方式、訂單資訊原封不動貼進去。從你的角度來看，並沒有想公開，也沒有想濫用，只是「稍微方便地使用一下」而已。

但這裡需要思考的，不只是 **「這些資訊會不會外洩」** ，而是 **「這些資訊本來是否能由自己判斷後，交給外部 AI 服務」** 。

因此，我把這次學到的內容套用到自己的 AI 使用情境，整理成四類資訊：

| 資訊 | 我的判斷 | 理由 |
| --- | --- | --- |
| 個人資訊 | 原則上不直接提供 | 需要確認是否能由自己判斷後交給外部服務，必要時做遮罩處理 |
| 密碼、API 金鑰、Token | 不提供 | 可能導致遭到不正當使用 |
| 公司內部機密、業務資料 | 不要擅自提供 | 可能包含企業或客戶機密，需確認使用規範 |
| 公開資訊、公開也沒問題的自有內容 | 基本上可使用 | 已公開，或公開後也不會有問題的資訊 |

當然，這不是為企業制定的規範，而是 **把這次學到的內容套用到自己使用 AI 的情境後，整理出的現階段基準** 。

另外，也不是只有「把資訊交給 AI」和「完全不用 AI」這兩種選擇。如果某些資訊（例如姓名）對處理其實不必要，就可以改成像「山田太郎」改為「顧客 A」這樣， **先做遮罩，只提供必要資訊** 。

```
山田太郎先生有來電表示要變更預約。
                ↓
顧客 A 有來電表示要變更預約。
```

此外，不同 AI 服務對輸入資料的保存方式、是否用於模型改良等處理也都不同。有些服務可以選擇退出學習利用，但 **「不會拿去學習」不等於「什麼都可以放心輸入」** 。還是必須確認使用的服務條款與設定。

這次學習後，我自己的想法變成這樣：

```
不要輸入外洩後會很困擾的資訊
          ↓
先思考這些資訊本來能不能交出去
          ↓
如果一定要交，就只提供最少必要資訊
          ↓
再確認服務端會如何處理這些資料
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790060247&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2dE3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuNTIiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuNTIiXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjUyIl1dLDBd&amp;dt=1790059500879&amp;bpp=1&amp;bdt=1389&amp;idt=1&amp;shv=r20260918&amp;mjsv=m202609170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790060209%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790060209%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790060209%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x969%2C952x280%2C952x280&amp;nras=5&amp;correlator=268458292828&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=3059&amp;biw=1680&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95403098%2C122881146%2C95340252%2C95340254&amp;oid=2&amp;pvsid=3731091999409375&amp;tmod=33368715&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 2\. Prompt Injection 改變了我的認知

另一個讓我印象深刻的是 Prompt Injection。這次學習中，我學到 Prompt Injection 是指攻擊者試圖用自己的指令覆蓋系統原本預期的指示或任務；而 Jailbreak 則是試圖繞過 AI 所設的安全限制。

另外，我也知道了 Prompt Injection 還分成 **直接（Direct）** 與 **間接（Indirect）** 兩種。

### 2.1 直接 Prompt Injection

看到「直接 Prompt Injection」這個詞時，我一開始想到的是攻擊者自己直接把惡意提示詞輸入 AI。但讓我印象很深的是，它也可能和社交工程結合。

例如，若有人假扮成上司或 AI 供應商的專家，要求你「因為有緊急確認需求，請把這段提示詞輸入公司內部 AI」，那麼真正把提示詞輸入 AI 的，並不是攻擊者本人，而是接收到指示的使用者。

```
攻擊者
  ↓
誘導使用者
  ↓
使用者把指定的提示詞輸入 AI
  ↓
AI
```

像是「上司指示」「專家請求」這類權威性，再加上「請立即處理」這種急迫感，就可能形成鎖定人類判斷的攻擊。

談到生成式 AI 資安時，我原本幾乎都在想 AI 本身會不會被攻擊；但這次讓我印象很深的是， **操作 AI 的人類本身，也可能成為攻擊路徑的一部分** 。

### 2.2 間接 Prompt Injection

我覺得更難察覺的，是 **間接 Prompt Injection（Indirect Prompt Injection）** 。

這不是攻擊者直接對 AI 輸入指令，而是把惡意指令藏進網頁、PDF、電子郵件等資料中，再讓使用者把那些資料讀給 AI 看，藉此讓 AI 執行攻擊者的指示。

我把這次學到的內容，用自己的話整理成一句：

> **「藏在資料裡的指令，卻被 AI 當成是對自己下的指令」的攻擊**

流程整理如下：

```
攻擊者
  ↓
在網頁、PDF、電子郵件等內容中
藏入惡意指令
  ↓
使用者把那些資料讀給 AI
  ↓
AI 把資料中的指令
誤認為是對自己的指示
  ↓
導致使用者沒有預期到的動作
```

重點在於，使用者本身並沒有輸入惡意提示詞。即使只是請 AI「幫我摘要這份 PDF」，那份 PDF 裡仍然可能藏有要給 AI 的另一段指令。

認識到這種攻擊後，我開始覺得， **不只是要注意「自己輸入了什麼給 AI」，也要注意「自己讓 AI 讀了什麼」** 。

## 3\. 從 RAG 思考資料與權限

在學習資訊外洩相關內容時，我也學到了 RAG（Retrieval-Augmented Generation，檢索增強生成）。一開始我心裡的疑問是： **「把需要的所有內部文件都直接塞進提示詞不就好了嗎？」**

但若每次都把所有文件一起交給模型，不但會受到上下文視窗限制，也會有 Token 成本。如果把不必要的資訊大量丟進去，反而可能讓重要資訊無法被妥善處理。

RAG 是先搜尋與問題相關的資訊，再把需要的內容交給 LLM 來產生回答。若資訊更新了，只要更新可被搜尋的文件，就能 **不必重新訓練 LLM，直接把新資訊用在回答中** ，這也是我理解到的優點。

```
問題
  ↓
將問題做 Embedding
  ↓
從向量資料庫搜尋相關資訊
  ↓
把相關資訊交給 LLM
  ↓
產生回答
```

在學習 RAG 的過程中，我也發現有幾個「原本以為的理解」和「學完後的理解」之間有差異。

| 我的疑問／一開始的想法 | 學習後整理出的理解 |
| --- | --- |
| Embedding 和搜尋是同一件事嗎？ | Embedding 是轉成數值表示，Vector Search 是利用這些表示做搜尋 |
| 向量化之後就安全了嗎？ | 向量化不是加密 |
| 是不是先生成答案，再確認權限就好？ | 在 RAG 中，重要的是搜尋階段就不要讓沒有權限的人取得資料 |

特別是 Embedding，雖然會說「把文章的語意轉成向量，再搜尋語意接近的內容」，但一開始我其實很難直觀理解。

後來我在心裡把它分開想： **Embedding 是把文章轉成類似「語意座標」的數值表示，Vector Search 則是用這些數值表示去找距離較近的資料** 。這樣拆開後，我就比較容易理解了。

另外，向量化並不等於加密。依照 RAG 的架構，除了搜尋用向量之外，也可能會保存原文或中繼資料，因此對保存資料本身的存取控制也是必要的。

而在學到 RBAC（Role-Based Access Control，角色式存取控制）之後，我理解到重點不是事後把產生好的回答遮起來，而是 **在 RAG 的搜尋階段，就不要讓沒有權限的文件被取出來** 。

我自己的體會是：

> **如果搜尋不到，就不可能拿那份資訊來生成內容**

這句話特別讓我印象深刻。

當然，這不是說連 LLM 原本已經學過的知識都無法生成，而是指在 RAG 所處理的內部文件中， **不能把沒有權限的資訊以搜尋結果的形式交給 LLM** 。

談到生成式 AI 資安，我一開始會想到的是「讓 AI 不要說出危險的內容」這種輸出端的對策。但學了 RAG 之後，我理解到， **在資訊到達 AI 之前的搜尋與資料設計，本身也是安全的一部分** 。

## 4\. 不能只依賴單一防護

學到這裡我注意到，雖然每一章都在談不同技術，但反覆出現的是同一種思維： **不能只靠一種防護措施，而是要做多層防禦** 。

例如，若依照防護的位置來整理這次學到的對策，大致會像這樣：

| 防護位置 | 對策範例 |
| --- | --- |
| 在輸入 AI 之前 | 輸入規則、遮罩、DLP |
| AI／RAG 內部 | Guardrails、存取控制 |
| 保存資料時 | 加密、權限管理 |
| API 金鑰等秘密資訊 | Vault、最小權限、輪替 |
| 人與組織 | 教育、稽核、事件應變 |

人類有可能出錯，所以不能只說「小心一點」就結束，還要疊加 DLP 之類的技術性防護。另一方面，技術防護本身也不是萬無一失，因此還需要再加上權限管理、稽核、教育等措施。 **共通點就是：不要只靠其中一項。**

在秘密資訊管理方面，我一開始曾經疑惑： **「如果是 API 金鑰，只要放進 `.env` ，再用 `.gitignore` 排除 Git 追蹤，不就好了嗎？」** 因為我在開發學習中也有使用 `.env` ，所以不太清楚它和 Vault 的差異。

整理後我發現，它們想解決的範圍不同：

| 觀點 | `.env + .gitignore` | Vault |
| --- | --- | --- |
| 主要用途 | 把秘密資訊和原始碼分開 | 集中管理秘密資訊 |
| 存取控制／稽核 | 較有限 | 可加入政策與稽核機制 |
| 輪替 | 通常要另外處理 | 可作為機制的一部分來管理 |

對我來說， **Vault 比起是秘密資訊的「存放處」，更像是「關卡」** ，這樣理解起來比較容易。

在維運面，我也學到了模型版本管理與回歸測試。之前我曾嘗試用 AI 做 OCR／表單辨識，結果把模型換成新版之後，反而出現以前能辨識的內容辨識率變差的情況。

```
切換到新模型
        ↓
以前可讀的表單辨識率下降
        ↓
體會到「新模型 ≠ 所有用途都更好」
        ↓
理解回歸測試的重要性
```

當時我真的覺得「明明是新模型，怎麼反而變差了？」這次學到回歸測試之後，再把那次經驗連結起來，理解就比單純背名詞更有說服力。

**不是加上防護就結束，而是系統變化之後，還要持續確認是否仍符合預期** ，我覺得這點很重要。

## 5\. 我自己的 AI 使用規則

在整理完這些學習內容後，我也開始思考：「那麼，之後我自己會怎麼使用 AI？」

目前我想把下面三點作為自己的規則：

| 我的規則 | 與學習內容的連結 |
| --- | --- |
| 1\. 先思考能不能把這些資訊交出去 | 不要輕易輸入個資、驗證資訊、機密資訊；必要時先做遮罩 |
| 2\. 要假設資料裡也可能藏有指令 | 以間接 Prompt Injection 為前提，注意自己讓 AI 讀了什麼 |
| 3\. 影響越大的操作，人越要介入確認 | 對於送出、公開、刪除、轉帳等難以挽回的操作，強化人工確認 |

第一點不只是想「會不會外洩」，而是先問自己： **這些資訊本來能不能由我判斷後交給外部服務** 。即使必要，也要想辦法透過遮罩等方式減少提供的資訊量。

第二點是因為學了間接 Prompt Injection 之後決定的。我不想只看自己輸入的提示詞，也想維持一個前提： **網頁、PDF、電子郵件等我讓 AI 讀取的資料裡，也可能藏有指令** 。

第三點則是，我不想把 AI 的所有動作都用同樣嚴格的標準檢查，而是希望依照失敗時的影響程度，調整確認強度。特別是以下這些操作，我希望加強人工確認：

- 轉帳
- 刪除資料
- 對外公開
- 寄送電子郵件或訊息

如果把 AI 的每一步都當成不可信，並且每次都逐一細查，那 AI 的優勢就會大幅降低。

所以，我為自己定下的原則不是 **「信任 AI 或不信任 AI」的二選一** ，而是 **把確認成本集中在失敗後果最嚴重的地方** 。

## 總結

在學生成式 AI 資安之前，我對它的理解大概只有「不要輸入危險資訊」「不要完全相信 AI 的回答」這種程度。但這次學習讓我知道，在這之前與之後，其實還有很多該守住的地方。

```
學習前
「只要不要把危險資訊丟給 AI 就好」
「如果 AI 回答怪怪的，就小心一點」

        ↓

學習後
「這些資訊本來能不能交給 AI？」
「讀給 AI 的資料裡有沒有藏指令？」
「AI 能搜尋到的資訊是否有適當限制？」
「秘密資訊是怎麼管理的？」
「哪些高風險操作需要人工確認？」
```

這次我學到了 Prompt Injection、RAG、Vault、DLP、存取控制等不同技術。但回頭看，最讓我印象深刻的，其實不是這些個別技術名稱，而是 **不要依賴單一防護手段** 這件事。

不是只相信人，也不是只相信 AI 或 Guardrails，而是要在輸入、資料、權限、輸出、運作等多個環節一起防護。同時，自己在使用 AI 時，也不能只是覺得「因為 AI 很方便所以就用」，而是要 **思考風險在哪裡，並在高風險處放入人工確認與技術防護** 。這是我這次學習後最大的感受。

我雖然才剛開始接觸生成式 AI 資安，但未來在繼續進行 AI 開發時，我希望不只是想著「方便所以使用」，而是能一邊思考 **「要交給它什麼資訊、要給它什麼權限、哪些地方需要人來確認」** ，一邊使用 AI。

## 參考資料

- Happiness Chain「生成式 AI 資安」學習課程

---

原文出處： [https://qiita.com/kf\_webdev/items/cbd1d7bb724676a3b686](https://qiita.com/kf_webdev/items/cbd1d7bb724676a3b686)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/ag2dE3) [12瀏覽](https://codelove.tw/@tony/post/ag2dE3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/ag2dE3)

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

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1790059499&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2dE3&amp;pra=5&amp;asro=0&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuNTIiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuNTIiXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjUyIl1dLDBd&amp;dt=1790059499566&amp;bpp=3&amp;bdt=103&amp;idt=104&amp;shv=r20260918&amp;mjsv=m202609170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790059430%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790059430%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790059430%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=268458292828&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95403098%2C122881146%2C95340252%2C95340254&amp;oid=2&amp;pvsid=3731091999409375&amp;tmod=33368715&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=153" title="Advertisement" aria-label="Advertisement"></iframe>