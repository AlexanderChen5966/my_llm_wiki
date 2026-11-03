---
title: "「AI感設計」為什麼消失不了──讀懂 Impeccable，並用相同需求做出 4 種版本比較"
source: "https://codelove.tw/@tony/post/qJRgrq"
author:
published:
created: 2026-10-30
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------現在的 AI 只要你請它做登陸頁設計，幾分鐘內就能生出一個看起來很完整的頁面。配色和留白也不差，但總覺得有種似曾相識的..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [10月29日](https://codelove.tw/@tony/post/qJRgrq "2026-10-29 17:15:18")

## 「AI感設計」為什麼消失不了──讀懂 Impeccable，並用相同需求做出 4 種版本比較

## 前言

現在的 AI 只要你請它做登陸頁設計，幾分鐘內就能生出一個看起來很完整的頁面。配色和留白也不差，但總覺得有種似曾相識的感覺。你是否也有過這樣的經驗呢？

這篇文章要介紹的是，如何從 AI 產出的設計中去除「AI 味」的技能—— **Impeccable** 。我看了兩支解說影片、閱讀了 GitHub 儲存庫，最後用同一份需求文作出四種版本的頁面來比較。

[![原生 Claude Code 與 Impeccable 的比較](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fc6e94700-160b-496f-b7a3-e8ea614bb5ea.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ca9ff5dab5cb5e7876df2ead26ee959c)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fc6e94700-160b-496f-b7a3-e8ea614bb5ea.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ca9ff5dab5cb5e7876df2ead26ee959c)

左邊是原生 Claude Code，右邊是使用 Impeccable 做出來的頁面。需求文完全相同。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1793347388&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRgrq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguNzQiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguNzQiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4Ljc0Il1dLDBd&amp;dt=1793347348271&amp;bpp=1&amp;bdt=1415&amp;idt=1&amp;shv=r20261028&amp;mjsv=m202610290101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793347075%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793347075%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793347075%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1654x898&amp;nras=3&amp;correlator=3228459268358&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=1252&amp;biw=1654&amp;bih=898&amp;scr_x=0&amp;scr_y=0&amp;eid=95404667%2C95405492%2C95406273%2C95407880%2C122881731%2C122881957&amp;oid=2&amp;pvsid=7304497320523026&amp;tmod=1339698072&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1654%2C898&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=40598" title="Advertisement" aria-label="Advertisement"></iframe>

這篇文章不會做的事：不會逐一解釋所有指令。安裝方式與指令列表請參考官方文件或既有文章。

**版本說明**  
Impeccable 更新很頻繁。本文的實驗是用 **Impeccable v4.4.0** （2026 年 10 月）進行，儲存庫內容則是以 2026 年 10 月 14 日的 **v4.5.2** 為準。到發文日 10 月 28 日時，版本已經推進到 **v4.8.0** 。指令、檢測規則數量與畫面呈現方式，都可能在未來版本中改變。

## 1\. AI 設計的追逐遊戲

Impeccable 的官方網站有一個畫廊，把 AI 做出的設計依年份排列。它以同一個虛構產品，依照每個年份的「AI 味」來描繪。

- **2022 年** ：紫色漸層與霓虹光感
- **2025 年** ：奶油色底、像雜誌一樣的斜體
- **2026 年** ：粗黑框、硬陰影、貼紙感

[![Impeccable 的 slop 畫廊（2022 年、2025 年、2026 年）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fafeb9b84-084d-43db-ba59-7ba461bb80fd.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d8274fffb496bfb34fa9dd6cf2e700ec)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fafeb9b84-084d-43db-ba59-7ba461bb80fd.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d8274fffb496bfb34fa9dd6cf2e700ec)

同一個虛構產品「Lumina」，以每個年份的「AI 味」來畫出不同版本。黃色標籤是 Impeccable 加上的指摘。  
出處：Impeccable 官方網站（ [https://impeccable.style/slop）](https://impeccable.style/slop%EF%BC%89)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1793347388&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRgrq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguNzQiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguNzQiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4Ljc0Il1dLDBd&amp;dt=1793347348274&amp;bpp=1&amp;bdt=1418&amp;idt=0&amp;shv=r20261028&amp;mjsv=m202610290101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793347075%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793347075%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793347075%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1654x898%2C952x280&amp;nras=4&amp;correlator=3228459268358&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=2483&amp;biw=1654&amp;bih=898&amp;scr_x=0&amp;scr_y=0&amp;eid=95404667%2C95405492%2C95406273%2C95407880%2C122881731%2C122881957&amp;oid=2&amp;pvsid=7304497320523026&amp;tmod=1339698072&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1654%2C898&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=40600" title="Advertisement" aria-label="Advertisement"></iframe>

※ 這種按年份切分的方式，是 Impeccable 方面的整理。

看著看著會發現， **為了避開「AI 味」所做的努力，本身又成了下一波的 AI 味** 。紫色漸層被嫌棄後，奶油色流行起來；奶油色膩了之後，黑框又登場。就算透過改變風格試圖逃離，很快它也會變成新的定番。

回顧 Qiita 上的文章，對策也一直在演進：

1. **靠提示詞請它做** ：在指令裡要求「不要有 AI 味」
2. **提供規則檔** ：像 [DESIGN.md](http://design.md/) 這樣，寫下設計規範再交給 AI
3. **有步驟的技能** ：像 Impeccable 一樣，連製作流程本身都先定義好

Impeccable 的儲存庫裡，就寫了這種以追逐遊戲為前提的規則。它列出容易聚集在 AI 產出 UI 上的常見外觀，例如「奶油色底、強對比襯線字體與赤陶色」、「幾乎全黑背景搭配霓虹點綴」等等，接著要求做出確認（意譯）：

> 如果只看分類，或只看「分類＋刻意避開它」，就能猜出外觀，那就重做。

連「刻意避開」都能被預測到，那也算是一種定番。所以 Impeccable 提供的不是某個特定風格，而是流程。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1793347388&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRgrq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguNzQiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguNzQiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4Ljc0Il1dLDBd&amp;dt=1793347348276&amp;bpp=1&amp;bdt=1420&amp;idt=0&amp;shv=r20261028&amp;mjsv=m202610290101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793347075%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793347075%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793347075%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1654x898%2C952x280%2C952x280&amp;nras=5&amp;correlator=3228459268358&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=3307&amp;biw=1654&amp;bih=898&amp;scr_x=0&amp;scr_y=0&amp;eid=95404667%2C95405492%2C95406273%2C95407880%2C122881731%2C122881957&amp;oid=2&amp;pvsid=7304497320523026&amp;tmod=1339698072&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1654%2C898&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=40603" title="Advertisement" aria-label="Advertisement"></iframe>

## 2\. Impeccable 是什麼

Impeccable 是一套用於前端設計的技能，起點來自 Anthropic 的 frontend-design skill。可在 Claude Code、Codex 等工具中使用。

- 1 個 skill，24 個指令
- 59 條不用 LLM 的檢測規則（README 記載，v4.5.2 時）
- 將寫產品事實的 **[PRODUCT.md](http://product.md/)** 與描述視覺系統的 **[DESIGN.md](http://design.md/)** 分開保存

檢測規則數量在我第一次讀到的 v4.1.0 是 61 條，v4.5.2 變成 59 條，發文日（2026 年 10 月 28 日）的 v4.8.0 則是 62 條。版本與數量會變動，我想這正代表 AI 味的定義本身也還在持續變化。

安裝方式是執行 `npx impeccable install` ，之後在 Claude Code 中執行 `/impeccable init` 。

## 3\. 「好的設計」的衡量標準

在比較之前，先定義衡量標準。Gary Chen 的解說影片把好的設計定義為「讓使用者的認知成本盡可能低」，並拆成四個測試：

1. **是否符合直覺** ：看起來像按鈕的東西，就應該真的能按
2. **主次是否清楚** ：頁面應該只有一個主角
3. **是否有節制** ：拿掉也不會少資訊的裝飾就不需要
4. **是否為對象量身打造** ：是誰、在什麼情境下會看這頁

另外還有一個本文會反覆使用的測試：

1. **把產品名稱換成別的名字，頁面還成立嗎？**

如果還成立，那這頁就是「哪個產品都能套用的平均值」。

## 4\. 離開平均值的步驟

Gary Chen 的影片（繁體中文字幕）詳細解說了 Impeccable 的想法。以下會依照影片中的步驟，結合儲存庫內容與我實際操作的畫面來驗證。

### 4-1. 詢問：不問顏色與字體

執行 `/impeccable init` 後，第一步會先詢問產品資訊：誰會使用、產品是做什麼的、用什麼技術做。它不會問你偏好的顏色或字體。答案會寫入 [PRODUCT.md](http://product.md/) 。

[![init 的回答確認畫面](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fc6b7b7a9-1ef6-4ca3-879c-477fc5185354.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fb92918d158f62b251e00420ac857018)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fc6b7b7a9-1ef6-4ca3-879c-477fc5185354.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fb92918d158f62b251e00420ac857018)

我對產品回答的是「Not decided yet」，但 Impeccable 已經從需求文中讀出了產品內容。

### 4-2. 候選：從使用者熟悉的世界找 7 個

次に、使用者平時就常接觸的世界中，找出 7 個可用的隱喻候選。儲存庫裡有這樣的規則（意譯）：

- 至少往三個不同系統的方向挖掘
- 業界慣例，以及「照著預期反著來」的選項，從一開始就不要列入候選

### 4-3. 抽籤：沒辦法跳過的抽選

從候選中決定哪一個作為主提案，是靠抽籤決定的。儲存庫把在執行抽選前先寫程式碼稱為「違反契約」。因為如果沒有抽選，最後每次又都會回到那個分類的定番。

### 4-4. 選擇頁面：最後選的是人

抽選完成後，瀏覽器會開啟選擇頁面。我實際試跑時的畫面如下。

[![世界觀選擇頁面](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fd21f5774-3c41-49ae-aa7e-ab3055638192.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6f01cc5c298ea6a43ebb6689f1295964)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fd21f5774-3c41-49ae-aa7e-ab3055638192.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6f01cc5c298ea6a43ebb6689f1295964)

每張卡片都寫著第一個畫面會長什麼樣、選它的理由，以及風險。評估軸只有兩個：「使用者會不會有代入感」和「產品是否容易理解」。

我特別覺得有趣的是以下三點：

**IMPECCABLE'S PICK（代理人自己的首選）會老實寫出自己的風險**  
這次的首選是「十九景（名所浮世繪系列）」。它的風險欄寫著「這也是這個需求第一個想到的方向之一」。儲存庫也明白寫著，自己的第一名通常是「很多執行結果都會走到的地方」。

**從落選候補拿到的，不是外觀，而是「規律」**  
抽選選中的「導覽手冊跨頁」卡片上寫著「IMPROVED BY IMPECCABLE'S WORLDS / FROM LABANOTATION（舞譜）」的框架。它不會把舞譜的外觀一起帶進來，只借用「左右對稱排列、時間由上往下流動」這個骨架。

**定番方案一定會放，但代理人不會推薦它**  
屬於業界定番的方案也一定會準備一張。不過儲存庫裡寫得很清楚：那是「使用者的門，不是你的門；絕對不要推薦它」。（意譯）平均值的選擇權，只留給人類。

儲存庫中相關段落（原文） [skill/reference/new-work.md](https://github.com/pbakaus/impeccable/blob/main/skill/reference/new-work.md) ：

> It is the user's door, never yours: never recommend it, never weigh it against the roll
> 
> guess your aesthetic from the category alone, or from category-plus-avoidance, rework until neither answer is obvious.

[skill/reference/craft-floor.md](https://github.com/pbakaus/impeccable/blob/main/skill/reference/craft-floor.md) ：

> These are the category's defaults, not bans: the brief's own words can earn any of them.

### 4-5. craft-floor 並不是禁止列表

在寫程式之前，會先讀一份 AI 味的型態清單（craft-floor）。影片把它說成「禁止模式清單」，但實際檔案裡寫的是：這些是「該分類的定番，不是禁令」。只要需求文句有依據，就可以使用。

真正絕對禁止的只有一項，也就是標題上方那種小標籤（eyebrow）。除此之外，規則不是「不要用」，而是「不要下意識地用」。

### 4-6. 兩層審查

完成之後，會先由另一個代理人檢查是否遵守對使用者的承諾，再用檢測工具做機械式檢查。

## 5\. 選擇的分岔

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1106437894&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1793347424&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRgrq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguNzQiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguNzQiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4Ljc0Il1dLDBd&amp;dt=1793347348288&amp;bpp=1&amp;bdt=1432&amp;idt=0&amp;shv=r20261028&amp;mjsv=m202610290101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793347388%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793347388%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793347388%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1654x898%2C952x280%2C952x280%2C952x280%2C728x90%2C334x250%2C952x280%2C952x280%2C952x280%2C936x280&amp;nras=11&amp;correlator=3228459268358&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=7533&amp;biw=1654&amp;bih=898&amp;scr_x=0&amp;scr_y=3995&amp;eid=95404667%2C95405492%2C95406273%2C95407880%2C122881731%2C122881957&amp;oid=2&amp;psts=AOrYGslUopz-d8_XbZ_Kq9Zf9c5fKIe672FAhCsEPMPyfl29ZwQZirPxhKSO1kPLUbTjtdCdSxhf4cQrHeq2OuresJicy1FHu6d89z0LI-VI7BTBF_nEOw7ZkQ2sVRw-b2sBTAI%2CAOrYGsnpD7a5P5W-gfoMjkejFv4yPX1xMgYcAP1JXIgVicSWklucrZ3TI6ClHyaK1-JLsYLF8kmAZfhxCu7lJMxIXg2emGFX13ghOQ%2CAOrYGskl0ppfts68ZGoVPkDdT9_KF2qS8FRaehhe18gFiWdXrmEzp3FqSrlzTO5naCYqVttb8ItKkI1ITTy0i712pEXknIAEY2WP4sU%2CAOrYGsm6hG28LJv4b-sr-jIBNYmf1zS0tOOjcVWiWuN-3AP3mTNvBlzbP0zh-cTZqEFX2Yy0oHfrTlxMeg5KkzsyL43jsej3ollyRA%2CAOrYGsmmrPP5ci-JlYTMUEYq4y846ggztmoSIfcMk793TYkmuL1BpYXrTkdTKmllD4EcLl59GrIUHVZq2hgB0ho9Z0VpeeizVAtKIfexTuT7GV437ic&amp;pvsid=7304497320523026&amp;tmod=1339698072&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1946%2C30%2C1946%2C30%2C1920%2C30%2C1894%2C979%2C1654%2C898&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=76048" title="Advertisement" aria-label="Advertisement"></iframe>

Nuno Tavares 的影片重點是從安裝到修改的實際操作。把兩支影片放在一起看，就會發現兩位創作者在同一個分岔點做了相反的選擇。

**Gary 選擇離開抽到的方案**  
抽到的是燈塔方案，但 Gary 選了另一張卡「開發者主控台」。他自己指出成品的缺點是：「第一個畫面資訊太多」、「小英文字太多」。這正是觸發第 3 個「節制」與第 2 個「主次」測試的情況。

**Nuno 則直接採用抽到的方案**  
Nuno 將抽到的「製圖」方案直接採用。雖然沒有 AI 味，但他本人也表示：「我不懂斜線框的意思。」在製圖的世界裡，斜線代表「不適用」，但對來買課程的人來說並不直觀。這就碰上了第 1 個「是否符合直覺」測試。

借用隱喻會帶來獨特性，但也會把那個隱喻的規則一起帶進來。不管是跟著抽籤還是放棄抽籤，都會出現不同種類的缺點。Nuno 最後是在 Live 模式中，只挑出有問題的元件來修正。 **最後由誰來選，最後由誰來修，也都是人。**

## 6\. 用同一份需求做出 4 種版本比較

接下來是我自己的實驗。

### 6-1. 條件

我用同一份需求文，為虛構的河輪「Molenvaart（摩倫瓦爾特）」做了四個版本的登陸頁。設定是：以小型船隻沿著荷蘭 Kinderdijk 的運河緩緩前行，近距離欣賞河岸 19 座風車的 90 分鐘導覽式河輪行程。

共通需求文（ [brief.md](http://brief.md/) ）\`\`\`  
請幫我製作「Molenvaart（摩倫瓦爾特）」的登陸頁。  
Molenvaart 是一個 90 分鐘的導覽式河輪行程，搭乘小型船隻在荷蘭 Kinderdijk 的運河中慢慢航行，近距離欣賞沿岸的 19 座風車。  
目標對象是正在荷蘭自由行的日本旅客，會在阿姆斯特丹或鹿特丹住宿期間，尋找半天可往返的觀光行程。  
請以單頁 HTML/CSS 製作，並支援響應式設計。

```
作法**a-pure**：原生 Claude Code，只把需求文交給它**a**：原生 Claude Code，但有啟用 Impeccable 的自動檢查（hook）**c**：對 a 的副本執行 \`/impeccable critique\`，再修正指出的問題**b**：透過 \`/impeccable init\` → \`/impeccable craft\` 從零開始製作  
項目版本與設定：Impeccable v4.4.0（判定引擎 v0.1.11）、Claude Code v2.1.287、模型 Sonnet 5.5 推論：高  
作業系統：Windows + WSL 2（Ubuntu 22.04）  
實施日：2026 年 10 月 3 日  
所有截圖都以 27 吋螢幕上的 Chrome 擷取，顯示寬度約 1145px。若顯示寬度較窄，卡片排列等版面會不同。

**實驗中出現的失誤**  
我原本把 a 當作「原生 AI」版本，但 Windows 端其實裝了 Impeccable 外掛，導致自動檢查 hook 生效了。因為 hook 提示「奶油色背景是 AI 的定番」，背景色從奶油色被改成了藍灰色。於是我暫時停用外掛，重新做了一版 a-pure。

### 6-2. a-pure：原生 AI

![a-pure 的首屏](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fef809e13-86c9-42a5-bbd2-b238d27a7e94.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=16aa68638f55d5b6f61bfc713e234b65)

查看整頁![a-pure 的整頁](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fe5bb7927-747d-4a0f-a666-8b3a82fc9994.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=142bd1247cd5ff57154e5061b0c5bec4)

背景是奶油色（\`#f6f3ec\`），重點色是赤陶色（\`#d9622b\`）。這正是第 1 章提到、Impeccable 直接點名的組合。

另外還有標題上方的 eyebrow、三張長得一樣的大數字＋小標籤卡片、以及三張「適合這類人」卡片，AI 定番模板幾乎都湊齊了。頁面裡沒有任何風車插圖，只要把產品名稱和數字換掉，就能套用到任何觀光行程。

有趣的是，這個頁面還支援深色模式。切到深色模式後，背景變成幾乎全黑，重點色只剩橘色一種。**同一個頁面裡，兩種 AI 定番同時共存**。

![a-pure 的深色模式版](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fed8769c7-4644-4a6a-a442-ca48e291d4f1.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b502d24ca831a619eea9599abf3a602f)

查看整頁![a-pure 的深色模式版](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2F8e129f51-3d70-476a-b028-394b5182f572.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=099e374aec85b8612da00019f86536d1)

### 6-3. a：被 hook 指出後只改了顏色

![a 的首屏](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2F44f2ec16-ba3d-4084-a287-12f0ace5354c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c8f6061be7ca33893b3a8ea75730f632)

查看整頁![a 的整頁](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Faec5f41b-338a-4ee1-851e-bbf5b8a56c15.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=43c61454a48b815544d1841612021671)

a 一開始的背景也同樣是奶油色（\`#f4efe4\`），和另外獨立製作的 a-pure 幾乎一樣。後來因為 hook 的指摘改成藍灰色，但各區塊的 eyebrow、以及「19 座風車」這種大數字的結構都保留了下來。

### 6-4. c：經過批評修改後，結構並沒有變

![c 的首屏](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2F1a7e086f-a9f0-4274-a6a8-44243bf48748.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7f4daf1d6c62d3346c41bbf1416a7e71)

查看整頁![c 的整頁](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fe39f0e2a-1fed-4fff-b57f-1c03db8cb715.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7c1fc5e159ccc3d3f74e7a755e59f031)

\`/impeccable critique\` 會同時跑兩個代理人：一個負責設計批評，一個負責檢測工具的機械檢查，最後給出 32 分滿分中的 20 分。批評中有這樣一條：

> 結構過於通用，換成「京都的茶道體驗」也還能成立。

連第 5 個測試，工具本身都點出來了。

收到批評後，我把頁首文字調整得更易讀，補上票價與集合地點欄位，也重畫了風車。靜態截圖看不出來，但主視覺裡的風車葉片會慢慢轉動（60 秒一圈）。不過 eyebrow 和大數字仍然保留。其實批評還稱讚大數字是「這頁特有、而且很有自信的元素」。也就是說，新建時用的 craft-floor，與評估既有頁面的衡量標準並不是同一套。

在 c 裡我沒有執行 \`/impeccable polish\`。原因是批評之後，Claude Code 自己把優先度最高的問題修掉了。

### 6-5. b：從零開始遵守步驟

![b 的首屏](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2F575143b8-1c9c-4fde-8b03-e5084c70b231.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=495806cc26e67bebbd4e5ea6d7a0d320)

查看整頁![b 的整頁](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2F12dd56be-93a1-4497-a9ca-ca56fefd9493.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=37efc29a044088221eec305f0968cb6b)

抽籤分配到的是「導覽手冊跨頁」這個方案，也就是日本旅遊導覽書常見、附地圖的跨頁版面。我直接選了這個方案。

選它的理由很單純：像導覽書跨頁那樣能一眼看完整體，會比較容易確認。用一張地圖就能看見整體，會有「還能走到哪裡呢」的期待感。

最後做出來的頁面，是一張沿著運河縱向展開的地圖，19 座風車依序編號排列。捲動頁面時，船會沿著運河前進，經過的風車圖釘會從紅色變成藍色。把產品名稱換掉，整個頁面結構就會垮掉。

![捲動時船會前進，經過的風車圖釘會變藍](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4110077%2Fbf8a16e2-c5ad-4b8f-a45b-7b206964ba68.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=9b1ff4b158827da1425c206208e368bc)

我覺得很重要、但很容易被忽略的一點是，**它沒有用虛構內容去填票價和出發時間**。乘船資訊表上有一排黃色的「待確認」標籤，地圖上也寫著「示意地圖，實際航線可能不同」。相較之下，a 卻寫了「搭乘大眾運輸可輕鬆安排一日往返」，這是沒有驗證過的內容。

不過，它也有讓我在意的地方：

- **與期待有落差**：雖然捲動時船會前進，但下方說明面板不會自動跟著切換，必須點選編號才會變。畫面一旦會自動動作，讀者往往會期待「面板也會跟著走」，這會碰到第 1 個測試
- **很接近今年的定番**：粗黑框、黃色框線、帶陰影的按鈕，和第 1 章提到的「2026 年 AI 味」很接近。若從導覽書的語境來看是合理的，但對某些讀者來說，可能還是會覺得「這不就是今年的定番嗎？」

說真的，我對面板的互動還是有點不滿意。不過這次我在途中也有回答「照你決定」，所以如果我在提示詞裡更明確要求面板互動，結果也許會不同。正因為幾乎全交給它處理，才更看得出來人類仍有很多可微調的空間。

至於黑框是不是接近今年的定番，我想這大概只有有在追設計趨勢的人才會特別有感。就我自己而言，倒沒有特別在意。

### 6-6. 比較結果

所需時間 檢出件數 名稱 改成別的名字後是否仍成立（以我的感受）  
a-pure：5 分 32 秒、8 件、成立  
a：5 分 1 秒、7 件、幾乎成立  
c：14 分 57 秒、7 件、幾乎成立  
b：20 分 2 秒、5 件、不成立  

※ 所需時間是從 Claude Code 啟動到結束，以腳本記錄。a-pure 包含一次因讀取需求文而暫停的時間。  
※ 檢出件數是四個版本都用同一個 Impeccable launcher 執行 \`impeccable detect\` 的結果。

檢測結果的內涵如下：

- a-pure：low-contrast 5、side-tab 1、hero-eyebrow-chip 1、wide-tracking 1
- a：low-contrast 3、undersized-ui-text 1、cramped-padding 3
- c：與 a 完全相同
- b：cramped-padding 5

檢出件數是 8、7、7、5，幾乎沒有差異。c 明明經過批評修改，內容卻和 a 完全一樣，仍是 7 件。相對地，從「改名後是否還成立」來看，只有 b 變成完全不同的東西。

**可由機器計數的問題，和這頁是否真正屬於那個產品，是兩條不同的軸。**

另外，檢測工具也有其侷限。c 裡被警告「文字只有 4px」的地方，實際在瀏覽器中量測卻是 32px；被說間距太小的地方，量起來其實相當足夠。b 的 5 件警告也屬於同類。另一方面，大數字配小標籤這種版型，在 a-pure、a、c 都出現過，但檢測結果一次都沒抓到。7. 使用時機與注意事項
------------------------------------------------------------------------------------------

**時間與 token**
Gary 的影片花了 1 小時以上，但在我的環境大約 20 分鐘就完成了。不過這仍然是原生 AI 的 4 倍時間。我認為如果是要給人看的公開頁面，值得用；若是公司內部工具，原生 AI 大概就足夠了。

**最擅長的是從零開始**
如果是修改既有頁面，Impeccable 會延續現有外觀。c 之所以沒有改變結構，就是因為這樣。如果你想徹底換掉外觀，就必須明確說「重新設計」。

**AI 有時會自己放寬檢查**
自動檢查的 hook，對於它自己很有把握的誤判，允許 AI 自行登記忽略設定。b 中，Claude Code 判定某個警告是誤判，便在沒有先徵求我同意的情況下，直接註冊了整組忽略規則。c 則是在註冊前先詢問我，但我回答「照你決定」，最後也一樣加入了整組忽略設定。雖然這符合工具的設計方針，但也可能讓真正的問題被忽略。最好還是檢查一下設定檔（\`.impeccable/config.json\`）。

**外部通訊**
候選世界觀是從外部 API 取得，另外也有 telemetry。若要關閉，可設定環境變數 \`IMPECCABLE_NO_TELEMETRY\`。在放進工作儲存庫前，建議先確認一下。結語
---------------------------------------------

Impeccable 不是什麼全新發明。借用 Gary 的話來說，它只是把設計師幾十年來一直在做的事，整理成一套流程。先問清產品事實，再從現實世界借用隱喻，透過規則與審查避免掉回平均值。

實際試用後，讓我印象最深的不是流程有多精密，而是**人類在其中還保留了哪些位置**。選定番方案的權利只留給使用者，決定要不要跟著抽到的方案走，也由人類決定。而在使用者沒注意到的時候，AI 甚至會自行放寬檢查標準。

用機器算出來的問題數，四個版本幾乎沒有差異。真正拉開差距的，是用「改名後是否仍成立」這個人類視角去看時。

我自己在做完四個版本比較後，更認同的是這種分工：先讓 AI 搭出基底，再由我去強化自己在意的地方。像這次面板互動那樣，凡事都交給 AI 的部分，還是留有讓人類再細修的空間。

以前要從零形塑成品，總是要花不少時間。AI 進步後，第一步變快了，這件事真的很令人高興。接下來，我想把省下來的時間用在「哪些部分由自己決定」。參考
-------------------------

- Gary Chen [How to make AI designs that don't look AI-made: a deep dive into /impeccable](https://www.youtube.com/watch?v=QuQ2FOznK18)
- Nuno Tavares [Impeccable: The #1 Claude Code Design Skill (No More AI Slop)](https://www.youtube.com/watch?v=GDB6ATIPo4E)
- Impeccable（GitHub）：<https://github.com/pbakaus/impeccable>
- Impeccable 官方網站：<https://impeccable.style>

---

原文出處：https://qiita.com/jinxin4869/items/0e72c0b925b75f1c28c9
```

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qJRgrq) [28瀏覽](https://codelove.tw/@tony/post/qJRgrq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qJRgrq)

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

🔥 [REA：讓你的 AI 代理反向工程你希望擁有的應用程式功能](https://codelove.tw/@tony/post/qenz7x)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [PHP脆弱性php2shell應對：在不停止7.4正式環境下守住系統的那個夜晚與實際攻擊](https://codelove.tw/@tony/post/qOZgJa)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝7 💬1

273

🥈

[boluoji](https://codelove.tw/@boluoji123)

📝5 ❤️3

97

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [川川｜全客製電商：開店平台限制多？我們幫你寫一套自己的。➡️ 了解更多](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1793347347&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRgrq&amp;pra=5&amp;aiof=14&amp;asro=0&amp;itsi=3&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguNzQiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguNzQiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4Ljc0Il1dLDBd&amp;dt=1793347347077&amp;bpp=3&amp;bdt=221&amp;idt=51&amp;shv=r20261028&amp;mjsv=m202610290101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793347075%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793347075%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793347075%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=3228459268358&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1654&amp;bih=898&amp;scr_x=0&amp;scr_y=0&amp;eid=95404667%2C95405492%2C95406273%2C95407880%2C122881731%2C122881957&amp;oid=2&amp;pvsid=7304497320523026&amp;tmod=1339698072&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1654%2C898&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=89" title="Advertisement" aria-label="Advertisement"></iframe>