---
title: "不想閱讀生成式 AI 撰寫的文件"
source: "https://codelove.tw/@tony/post/qJRMWq"
author:
published:
created: 2026-09-15
description: "[BrainPad股份有限公司](https://www.brainpad.co.jp/)產品開發部中負責開發 [Rtoaster GenAI](https://www.brainpad.co.jp/rtoaster/genai/) 的依田。讓 Claude Code 撰寫 Design doc..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月15日](https://codelove.tw/@tony/post/qJRMWq "2026-09-15 08:30:09")

[BrainPad股份有限公司](https://www.brainpad.co.jp/) 產品開發部中負責開發 [Rtoaster GenAI](https://www.brainpad.co.jp/rtoaster/genai/) 的依田。

讓 Claude Code 撰寫 Design doc 或 PR 時，你是否曾覺得「資訊量很多，卻很難看出最後決定了什麼」？我有很多次這種感受。只要讀的時候稍微分心，就會漏掉最想知道的結論。那種獨特的疲勞感究竟從何而來，我實際用 GitHub PR 來驗證了一下。

## 對象讀者

- 讓 Claude Code 等 AI agent 撰寫設計書或 PR 的人
- 想重新檢視 Skill 或 prompt 設計的人
- 曾經疲於審查生成式 AI 所寫文件的人

## 為什麼讀 AI 的文章會疲憊

內容並不是錯的。相反地，大多數時候都寫得沒問題。即便如此還是會累，我認為原因可歸納為以下 4 點。

**不區分完整性與重要度** 。人類寫 Design doc 時，會在腦中不自覺地做取捨：「這個應該寫」「這個可以省略」。生成式 AI 不太會做這種篩選，會把想到的觀點全部用同樣的權重列出來。結果就是關鍵的判斷與理所當然的前提被用同等篇幅說明，讀者得自己負責分辨重要程度。

**結論放在最後** 。人類作者常會先直接說「結論來說～」，但生成式 AI 往往會先依序重現檢討過程，最後才在總結裡寫出結論。讀者在抵達結論之前，必須把所有資訊一直留在腦中。

**老實填滿模板標題** 。像是「背景」「目的」「用語定義」「未來擴充性」這類固定標題，一旦使用過，即使內容很薄也會很認真地補滿。標題越多，讀者就越有「非讀不可」的壓力。

**連顯而易見的項目也要正反並列** 。一旦採用「優點／缺點」這種格式，對於根本沒被採用的方案，也會機械式套用同樣格式。即使是從需求一看就知道不成立的方案，也會附上很完整的說明。

光用文字說明不太容易有感，所以我實際動手確認了一下。

## 以 EC 網站購物車折扣優惠券來確認

題材是 EC 網站購物車的折扣優惠券套用邏輯。需求如下：

- 購物車中有多個商品（分類、單價、數量）
- 優惠券有 3 種：固定金額折扣、比例折扣、免運費
- 每張優惠券都有最低消費金額、有效期限、適用分類等套用條件
- 只要包含任一不可併用的優惠券，就只採用折扣金額最大的 1 張
- 過期等不合法的優惠券要忽略，並繼續用剩下的優惠券計算

只指定要以 DDD（領域驅動設計）／OOP（物件導向）的方式實作，並沒有指定 Design doc 的寫法。

我會讓同一個需求分別以「沒有特別優化的 Skill」與「有調整 Design doc 寫法的 Skill」來實作，比對實際的 PR。

### 沒有特別優化的 Skill 寫出的 Before

Skill 的內容只是把「讀 Issue、寫 Design doc、實作、建立 PR」這個流程排起來而已。

```
請在 Design doc 中詳細記載你所檢討過的設計方針及其背景、優點與缺點。
請寫得即使不是實作者的人也能看懂設計意圖。
```

這個 Skill 產生的 Design doc、實作與 PR 如下。

連適用條件採用 Specification pattern（將條件表現為物件的設計模式）這一個判斷，都會以這種方式把優點與缺點並列。

```
### 以 Specification pattern 表現適用條件

將優惠券的適用條件（最低消費金額、有效期限、適用分類）分別實作成獨立的 Specification
類別，並以 \`AndSpecification\` 組合。

**優點**:
- 每個條件都是獨立的類別，因此可以遵守單一職責原則
- 新增新的適用條件時，不需要修改既有條件類別，因此可以遵守開放封閉原則
- 可以彈性表現條件組合（AND/OR）

**缺點**:
- 會因條件數量增加而導致類別變多，檔案數與程式碼量也會增加
- 相較於單純的 if 敘述串接，間接參照變多，可能提高追蹤程式碼的成本
```

甚至還老老實實列出 4 個沒被採用的替代方案，其中一個如下：

```
### 替代方案 4：在可否併用的判定中導入優先順序欄位

讓每張優惠券帶有優先順序（priority）欄位，在不可併用時採用優先順序
最高的優惠券。

**優點**:
- 不論折扣金額大小，營運端都能明確指定希望優先套用的優惠券

**缺點**:
- 本 Issue 的需求明確寫著要「採用折扣金額最大的優惠券」，導入優先順序欄位會與需求產生落差
- 會產生管理優先順序這項新的營運成本

這個替代方案因為不符合需求中明訂的「採用折扣金額最大的優惠券」規則，所以不採用。
```

連從需求一看就知道不成立的方案，也分配了和採用方案同樣篇幅的說明。這正是前面提到的「對顯而易見的項目也要正反並列」的直接體現。就這樣，「用語定義」「未來擴充性」等章節也一路排下去，Design doc 膨脹到 284 行、8521 字、32 個標題。PR 說明文也有 1232 字，還很老實地列出了實作檔案清單。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789457076&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRMWq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789457076031&amp;bpp=1&amp;bdt=1670&amp;idt=0&amp;shv=r20260911&amp;mjsv=m202609110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789457068%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789457068%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789457068%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C1592x764%2C1005x229&amp;nras=7&amp;correlator=6512288963438&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=145&amp;ady=3273&amp;biw=1592&amp;bih=763&amp;scr_x=0&amp;scr_y=402&amp;eid=95400775%2C95401792%2C122881021%2C95400620%2C122880334&amp;oid=2&amp;psts=AOrYGskWUXPb2cO6ILwXxTvxM_7KjiXjyffoo5Ek0Nbq1kG7y0Ht-09uvUBLAcUWv7SR1C2bBEgv4lSuOqm9igYUAdjnbi_KbppxfQfMusMSKTN7MY1tXLwdoKno9B6ixEtyMNU&amp;pvsid=6408570324184185&amp;tmod=232333558&amp;uas=3&amp;nvt=3&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1984%2C43%2C1984%2C43%2C1920%2C30%2C1832%2C900%2C1592%2C763&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=6&amp;fsb=1&amp;dtd=813" title="Advertisement" aria-label="Advertisement"></iframe>

程式碼本身是正確運作的。實際上，生成的程式碼透過了 pytest 的 24 個測試，全數 PASS，功能上沒有問題。這次真正想討論的是，讓人類去追理解設計判斷的文件分量。

讀起來會累，原因不是資訊錯誤，而是「到底採用了哪個方案、為什麼」這個最想知道的資訊，被沒採用的選項說明，以及顯而易見的優缺點列表給埋掉了。即使讀完，也還是得花時間才能撿出結論。

### 指定 Design doc 寫法的 Skill 寫出的 After

我在同一個 Skill 裡，加入了關於 Design doc 與 PR 寫法的指南。對應前面提到的 4 個原因，逐一處理。

```
## Design doc 的寫法

- 在開頭 3 行內先寫結論（採用的設計與理由），細節之後再補
- 將檢討過的替代方案用表格比較（方案、淘汰理由兩欄就夠了，不要每次都用文章把優點／缺點寫一遍）
- 沒採用的選項，只保留能傳達淘汰理由的最小必要長度
- 寫出「不做什麼（Non-goals）」，不要羅列範圍外的一般論或未來擴充可能性
- 只有在讀者可能不知道該技術名詞時，才寫用語定義、背景、目的、對象讀者這類固定章節；若屬理所當然則省略
- 只在有意義的單位建立標題，不要用多個標題重複相同內容
- 不確定時就刪減。寫完後先問自己：「即使沒有這個段落，審查者是否仍能理解決策？」若可以，就刪掉
```

「Non-goals」是為了避免把完整性與重要度混為一談；「結論先寫」是對應結論放最後的問題；「只在必要時才寫用語定義等固定章節」是為了抑制老實填滿模板標題的傾向；「替代方案用表格」則是為了避免對顯而易見的項目也正反並列。

同一個 Issue、同一個實作內容，重做之後的結果如下（實作與 #5 完全相同，方便只比較 Design doc 和 PR 的寫法）。

原本 4 個替代方案，被整理成 1 個表格。

```
## 檢討過的替代方案

| 方案 | 淘汰理由 |
|---|---|
| 直接用 if-else 判定適用條件 | 條件越多，單一函式越容易膨脹，而且難以對每個條件做單元測試 |
| 驗證函式集合（不使用 Specification） | 無法持有條件的狀態（例如最低金額的數值），需要 closure，反而更難閱讀 |
| 規則引擎（用外部 DSL 定義條件） | 雖然非工程人員可以修改條件，但對這次的需求規模來說過度設計 |
| 透過優先順序欄位判定可否併用 | 需求明確寫著要「採用折扣金額最大的優惠券」，優先順序欄位與需求不符 |
```

只要讀「結論」，3 行內就能知道採用了什麼設計與理由；看替代方案表格，也能一眼看出淘汰理由。像用語定義、對象讀者這些固定章節則省略了。雖然有用到 Specification pattern、Strategy pattern 這些詞，但因為判斷這篇文章的讀者應該不用額外解釋也看得懂，所以就省略了說明。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=4060917935&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789457137&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRMWq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789457076034&amp;bpp=1&amp;bdt=1673&amp;idt=0&amp;shv=r20260911&amp;mjsv=m202609110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789457068%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789457068%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789457068%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C1592x764%2C1005x229%2C952x280%2C952x280&amp;nras=9&amp;correlator=6512288963438&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=990&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=145&amp;ady=4668&amp;biw=1592&amp;bih=763&amp;scr_x=0&amp;scr_y=1634&amp;eid=95400775%2C95401792%2C122881021%2C95400620%2C122880334&amp;oid=2&amp;psts=AOrYGskWUXPb2cO6ILwXxTvxM_7KjiXjyffoo5Ek0Nbq1kG7y0Ht-09uvUBLAcUWv7SR1C2bBEgv4lSuOqm9igYUAdjnbi_KbppxfQfMusMSKTN7MY1tXLwdoKno9B6ixEtyMNU%2CAOrYGsmebWY2v6ZO95fSI7_CRERIO6K_fzCd21mjTpWqSFlZpyRWevrFTi2U_H8JCICeiDqhVg9jkwBTbmS61N1d_BeHPjbAggIU55DJjOlDXFccWbw&amp;pvsid=6408570324184185&amp;tmod=232333558&amp;uas=1&amp;nvt=3&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=294%2C30%2C294%2C30%2C1920%2C30%2C1832%2C900%2C1592%2C763&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=8&amp;fsb=1&amp;dtd=61127" title="Advertisement" aria-label="Advertisement"></iframe>

### 定量比較

以下整理在相同需求、相同實作內容下，Design doc 與 PR 說明文有多大的變化。

項目Before（#5）After（#6）Design doc 行數284 行41 行Design doc 字數8521 字1239 字Design doc 標題數32 個6 個PR description 字數1232 字396 字Design doc 在行數與字數上都變成了約 7 倍，PR 說明文也大約差了 3 倍。實作內容一字不差，因此這些差異全部來自「文件寫法」。

## Skill 設計的重點

根據這次比較，我整理出在讓 AI 撰寫 Design doc／PR 的 Skill 裡，值得加入的內容。若依照前面提到的「疲憊原因」來重新整理，會變成以下幾項：

- 明確寫出 Non-goals（不做什麼），不要讓它羅列範圍外的一般論（避免把完整性與重要度混為一談）
- 要求先寫結論，細節放在後面補強結論（對應結論放最後的問題）
- 只有在真的需要時才寫用語定義或背景說明這類固定章節（抑制老實填滿標題的傾向）
- 替代方案比較改成表格。若用文章逐一寫優點／缺點，只會不斷增加篇幅（避免對顯而易見的項目正反並列）
- 給 AI 的判斷標準是：「即使沒有這個章節，審查者還能理解決策嗎？」

這些都不是特別高明的技巧。只是，如果不明確寫進 Skill 裡，生成式 AI 就會傾向往「盡量完整」的方向寫。這是這次實驗得到的感受。

## 總結

讀生成式 AI 的文章會疲憊，原因不在於內容正確與否，而是它有 4 個習慣：不篩選重要度、把結論往後放、老實填滿模板、以及對顯而易見的項目也要正反並列。即使是同一個需求，只要在 Skill 中明確指定如何處理這些問題，Design doc 的篇幅就能變成將近 7 倍的差距。當你覺得不想再讀 AI 的輸出時，與其先懷疑 AI 的內容，不如先檢視 Skill 或 prompt 是否有指定「不要寫什麼」。

這次使用的 Issue 和兩個 PR 都保留在原始倉庫中。有興趣的人可以實際比對 Design doc 與 PR 說明文。

- Issue: [https://github.com/Taketo-Yoda/Qiita/issues/4](https://github.com/Taketo-Yoda/Qiita/issues/4)
- Before PR: [https://github.com/Taketo-Yoda/Qiita/pull/5](https://github.com/Taketo-Yoda/Qiita/pull/5)
- After PR: [https://github.com/Taketo-Yoda/Qiita/pull/6](https://github.com/Taketo-Yoda/Qiita/pull/6)

## 【附錄】實際使用的 SKILL.md

這裡是 Before／After 各自實際使用的 [SKILL.md](http://skill.md/) 全文。

- Before（未優化版）
- After（改善版）

---

原文出處： [https://qiita.com/take-yoda/items/e5d9ce6618523af1ffc5](https://qiita.com/take-yoda/items/e5d9ce6618523af1ffc5)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qJRMWq) [23瀏覽](https://codelove.tw/@tony/post/qJRMWq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qJRMWq)

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

🔥 [Codex 新手入門：從安裝到外掛、MCP、Skills，一篇把設定講清楚](https://codelove.tw/@tony/post/3ojwXx)

🔥 [Codex 怎麼買、怎麼充值？先把這兩套計費搞清楚](https://codelove.tw/@tony/post/q44PLq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝31 💬1

462

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1789457074&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRMWq&amp;pra=5&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789457074448&amp;bpp=5&amp;bdt=87&amp;idt=31&amp;shv=r20260911&amp;mjsv=m202609110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789457068%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789457068%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789457068%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=6512288963438&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1592&amp;bih=764&amp;scr_x=0&amp;scr_y=0&amp;eid=95400775%2C95401792%2C122881021%2C95400620%2C122880334&amp;oid=2&amp;pvsid=6408570324184185&amp;tmod=232333558&amp;uas=0&amp;nvt=3&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1984%2C43%2C1984%2C43%2C1920%2C30%2C1832%2C900%2C1592%2C764&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=40" title="Advertisement" aria-label="Advertisement"></iframe>