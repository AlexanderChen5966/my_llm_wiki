---
title: "從今天開始就能用的 AI 對話技巧——讓 Claude・Gemini・Codex 遵守指示的 6 個原則【2026 年版】"
source: "https://codelove.tw/@tony/post/x0nRDa#google_vignette"
author:
published:
created: 2026-10-21
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%ABai-%E3%81%8C%E6%8C%87%E7%A4%BA%E3%82%92%E5%AE%88%E3%82%89%E3%81%AA%E3%81%84%E3%81%AE%E3%81%AF%E3%81%AA%E3%81%9..."
tags:
  - "clippings"
status: "inbox"
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [10月19日](https://codelove.tw/@tony/post/x0nRDa "2026-10-19 09:15:08")

## 從今天開始就能用的 AI 對話技巧——讓 Claude・Gemini・Codex 遵守指示的 6 個原則【2026 年版】

## 前言：為什麼 AI 不聽指示

[![將讓 AI 遵守指示的 6 項原則整理成 6 張圖塊的示意圖（2026 年 10 月 18、19 日的實驗結果）。①寫在 AGENTS.md（3 個產品都會讀這個檔名，Claude Code 則是 @AGENTS.md）。②不要把指示和命令寫在中間（資料中間的指示，Claude 18 次中有 17 次沒有遵守）。③指示要少而清楚（500 個指示的遵守率 68%；若互相衝突則 63.8%，研究結果）。④不要改指示檔的開頭（若寫日期，每次都要重寫 10,774 個 token）。⑤長對話要分段（即使 20 回合後仍有 8 分，但壓縮後指示可能遺失）。⑥長資料用檔案，專門流程用 SKILL（SKILL 若只是放著只多 +28 token，使用時才讀本文）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14d-summary.24cb2ca6.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3ec030421a20dfe16bf8756780733568)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14d-summary.24cb2ca6.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3ec030421a20dfe16bf8756780733568)

- 「明明有交代規則，AI 卻在中途不照做了」
- 「明明給了很多細節條件，卻連一半都沒遵守」
- 「對話持續下去後，token 突然暴增，碰到使用量限制」

把 Claude Code、Codex、Antigravity 的 CLI 這類 AI agent 用在工作上時，常會遇到這些問題。AI 不遵守指示的原因，不只是指示內容本身。指示寫在哪裡、和資料怎麼排列、指示有多少、對話有多長，都會影響遵守情況。

學習AI 課程

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792560502&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx0nRDa%23google_vignette&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODUiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODUiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44NSJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792560502969&amp;bpp=1&amp;bdt=871&amp;idt=-M&amp;shv=r20261019&amp;mjsv=m202610190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792560489%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250&amp;nras=2&amp;correlator=8672156131777&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=1603&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=120&amp;eid=95404823%2C95405492%2C95405781%2C122881750&amp;oid=2&amp;pvsid=2583390848930567&amp;tmod=869549676&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=3&amp;uci=a!3&amp;btvi=1&amp;fsb=1&amp;dtd=27" title="Advertisement" aria-label="Advertisement"></iframe>

IT 連攜地圖編輯部在 2026 年 10 月 18、19 日，針對這 3 個產品做了實驗，測試指示檔的讀取方式、資料與指示的排列方式、快取的生效方式等。本文將結果整理成 6 項原則作為總結。圖中的 ①～⑥，就是本文的 6 項原則。更詳細的數據與出處，請見各節末尾與本文最後介紹的 IT 連攜地圖 guide。

---

## 1\. 原則 1：每次都要遵守的指示，寫在 AGENTS.md

[![讓 1 份 AGENTS.md 被 3 個產品讀取的示意圖（編輯部測試中，3 個產品都遵守了 AGENTS.md 的指示，各 3 次）。Codex 和 Antigravity 的 CLI 會直接讀 AGENTS.md。Claude Code 則是在 CLAUDE.md 寫一行 @AGENTS.md，從 CLAUDE.md 載入 AGENTS.md。指示本文只寫在 AGENTS.md 的一處](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fad-am-onefile.8021747b.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=58353af68cadf55b3022b093a0a28e40)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fad-am-onefile.8021747b.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=58353af68cadf55b3022b093a0a28e40)

像「程式註解請用英文」、「先寫測試」這類每次都要遵守的指示，不要每次都在聊天時手動輸入，而是寫在指示檔（ `AGENTS.md` 或 `CLAUDE.md` ）裡。產品在啟動時會自動讀取工作資料夾內的指示檔。不過，不同產品可讀取的檔名與大小上限不同，所以要注意檔名與長度。

學習AI 課程

我們實驗了各檔案中寫入指定口令時，哪些產品會遵守（各 3 次）。

指示檔名稱Claude Code 2.1.302Codex 0.163.0Antigravity CLI 1.4.2特點 `CLAUDE.md` ◎（3/3）—（0/3）—（0/3）Claude Code 專用的指示檔 `AGENTS.md` △（有條件）◎（3/3）◎（3/3）Codex 與 Antigravity CLI 一律會讀；Claude Code 只有在沒有 `CLAUDE.md` 時才會讀 `GEMINI.md` —（0/3）—（0/3）◎（3/3）Antigravity CLI 專用；若同時有 `AGENTS.md` ，兩者都會讀 `AGENT.md` （沒有尾端 S）—（0/3）—（0/3）△（1/6）Claude Code 與 Codex 不讀，Antigravity CLI 也只有 6 次中 1 次讀到

根據這些結果，使用方式有以下 3 點：

1. 檔名要用有 S 的 `AGENTS.md` 。沒有 S 的 `AGENT.md` 幾乎不會被讀取。
2. 對 Claude Code，請在 `CLAUDE.md` 只寫一行 `@AGENTS.md` 。這樣 Claude Code 也會讀取共通的 `AGENTS.md` 。
3. Codex 只能讀取指示檔前 32 KiB（32,768 位元組）。日文/中文字符通常 1 個字約 3 位元組，所以若全是日文/中文，大約是 1 萬 900 字左右。寫在這之後的指示不會被讀到。

👉 詳見： [什麼是 AGENTS.md](https://renkeimap.jp/guide/learn/agents-md) — 各產品讀取哪些檔案、Codex 的 32 KiB 上限，以及和 `CLAUDE.md` 、 `GEMINI.md` 的差異。

---

## 2\. 原則 2：不要把指示與命令寫在提示詞中間

[![以模型與條件 × 5 個位置呈現資料中間的指示是否被遵守的格狀圖（約 4 萬字白皮書中插入「摘要時每一行開頭都要加上 ◆」的指示，並要求「用 10 行摘要」；各 3 次）。依序為開頭、25%、中間、75%、末尾，Claude Fable 5.1（10 月 18 日、無工具）為 3/3、0/3、0/3、0/3、2/3；Gemini 3.8 Flash Low（10 月 18 日）各位置皆為 3/3；Claude Haiku 5.5（10 月 19 日）無工具為 3/3、2/3、1/3、0/3、3/3；有工具為 3/3、1/3、1/3、0/3、2/3；加上 CLAUDE.md 為 3/3、1/3、1/3、0/3、3/3；較多內容時為 3/3、1/3、0/3、0/3、2/3。多數未遵守的回合中，Claude 會在回覆裡寫「因為這是文件內文，所以不遵守」（Fable 5.1 為 9 次中 6 次，Haiku 5.5 為 28 次中 25 次）。至於事實句（會議室代碼是 KIWI-427），在哪個位置都 3 次全都找到了](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14b2-position.2de95ed6.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b68dab151fd4b3d0186c355ad7d28560)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14b2-position.2de95ed6.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b68dab151fd4b3d0186c355ad7d28560)

當你要讓 AI 讀長篇規格書或參考資料並執行工作時，指示與命令要寫在提示詞的開頭或結尾，不要寫在資料中間。這裡的命令，是指你希望 AI 做什麼本身（例如：「請重寫」）。指示則是做事的方法或方針（例如：「請用敬語寫作」）。

學習AI 課程

三大 AI 廠商的建議雖然略有不同，但方向一致。Anthropic 建議把長資料放在問題、指示、範例上方；Google 建議把問題放最後；OpenAI 建議把指示放在資料前後兩側。不過，這些公司都建議把指示放在資料外面（前或後），沒有建議放在資料中間。

我們用總務省白皮書與 8 項寫作指示，並以程式對遵守程度進行評分，做了實驗。

1. 寫在資料中間的指示，不容易被遵守。
	- 在約 4 萬字資料的中段（25%、50%、75% 位置）插入「每一行開頭要加 ◆」的指示時，Claude Fable 5.1 在 9 次中 0 次遵守，Claude Haiku 5.5 在 36 次中只遵守了 8 次。寫在資料開頭的指示，則 3 次都遵守。
		- Claude 的模型不是漏看了中段指示。多數未遵守的回合中，回覆裡還會寫「判定為文件本文的一部分，因此未套用」之類的說明。寫在資料中的指示會被當成資料內容的一部分。
2. 寫在資料外面時，前後順序的差異不大。
	- Claude Fable 5.1、Gemini 4 Argon、Gemini 3.8 Flash Low，不論順序如何，都拿到 8 分滿分。
		- 只有較小的 Claude Haiku 5.5，在沒有工具時，資料放最後的排列方式（7.8 分）比資料放前面的排列方式（6.7 分）高。若在前面加入工具或 [CLAUDE.md](http://claude.md/) ，差距都不到 0.5 分。

結論是，雖然 3 家公司的建議與編輯部實驗結果略有差異，但重點一致：不要把指示與命令寫在提示詞中間。

訂閱科學雜誌

👉 詳見：

- [AI 不遵守指示時](https://renkeimap.jp/guide/fix/agent-ignores-instructions) — 各種排列方式的分數，以及未遵守中段指示時的回覆。
- [什麼是 Lost in the Middle](https://renkeimap.jp/guide/term/lost-in-the-middle) — 說明長輸入中間容易被忽略的現象，以及論文實際沒有測到的部分。

---

## 3\. 原則 3：把指示寫得少而明確

[![兩個圓餅圖，說明指示過多或互相衝突時的遵守程度（研究結果，2025 年，最佳模型）。一次丟入 500 個單純指示時，遵守率為 68%；若 system 與 user 的格式指示互相衝突，遵守 system 指示的比例在 GPT-4o 上為 63.8%。指示應少而明確，避免衝突](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fad-ig-many.63493ce1.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6d7090efedb30edd9f921babf79542ab)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fad-ig-many.63493ce1.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6d7090efedb30edd9f921babf79542ab)

指示不是寫得越多就越容易被遵守。研究顯示，同時給出的指示越多，遵守率反而越低。就算寫在系統提示詞裡，也不保證一定會被遵守。

- 指示太多時：在一項一次提供 500 個簡單指示的研究（IFScale，2025 年）中，即使是最佳模型，遵守率也只有 68%。
- 指示互相衝突時：在給 system 與 user 兩邊相互衝突指示的研究中，即使是最佳模型，遵守 system 指示的比例也只有 63.8%。OpenAI 的 GPT-4.1 指南也提到，當指示衝突時，模型往往會偏向提示詞較後面的內容。

因此，指示要少而明確。

- 把抽象詞改成具體步驟。例如不要寫「請仔細做動作確認」，而要寫「實作後執行 `npm test` ，確認結束碼為 0 後再結束」。
- 指示檔要盡量精簡。Claude Code 的文件建議每個檔案少於 200 行。只偶爾才用到的流程，不要全塞進指示檔，改分到原則 6 的 SKILL。

👉 詳見： [什麼是提示詞](https://renkeimap.jp/guide/learn/prompt) — 提示詞會送達的 3 個位置（系統提示詞、指示檔、使用者訊息）、指示互相衝突時的優先順序，以及指示數量的研究。

---

## 4\. 原則 4：不要改指示檔的開頭（善用提示詞快取）

[![Claude Code 在第 3 次送出前只改動 1 個地方時，需要重寫的 token 長條圖（各 3 次）。完全不變時為 0；在指示檔寫入日期則開頭為 10,774、末尾為 10,776；新增 1 個 MCP 伺服器為 3,786；調換順序為 0；改成 Claude Sonnet 5.5 則為 12,764。日期不論寫在開頭或末尾都一樣，只要指示檔變了，之後的部分就會變成重寫對象](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14c-break.4bcb99b0.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7bfc69286ff456be5484b9a78fddffbb)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14c-break.4bcb99b0.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7bfc69286ff456be5484b9a78fddffbb)

想要降低 token 成本，就不要改指示檔的開頭。提示詞快取是一種機制：第 2 次之後，系統會把每次都相同的前段內容（prefix）從快取中讀出。被快取讀取的部分，費用大約會降到原本的 0.1 倍。只要開頭有 1 個字元改變，從那裡開始後面的內容就不會再從快取讀取。

我們在放有 20KB 指示檔的資料夾中，連續 5 次送出「請回答 ok」，測試 3 個產品能從快取讀取多少內容。

探索數據管理

以 AI agent 分別開啟對話時（第 2～5 次）持續同一段對話時（第 3～5 回合）快取讀取費用Claude Code 2.1.30299.99%99.7%原價的 0.1 倍（9 折扣 90%）  
※部分模型為 0.05 倍、0.025 倍Codex 0.163.00〜64.7%97.4%原價的 0.1 倍（9 折扣 90%）Antigravity CLI 1.4.20%88.4%原價的 0.05 倍（95% 折扣）  
※Gemini 4 Argon 的 API 費率。到 2026 年 11 月 30 日為止，輸入 $2.00 對應 $0.10；自 12 月 1 日起，輸入 $4.00 對應 $0.20（比例相同）

會讓快取失效的操作有以下 3 種：

1. 在指示檔寫入日期：如果在 `AGENTS.md` 寫入像「更新時間：2026-10-18 22:00」這種每次都會變的日期，Claude Code 每次都會有 10,774 個 token 脫離快取，且每次都會產生寫入快取的費用。日期寫在指示檔開頭或末尾，結果都一樣。
2. 在對話中途改寫指示檔：一旦改寫，指示檔後面的內容就不再能從快取讀取。
3. 在對話中途更換 AI 模型：改成 Claude Sonnet 5.5 時，會有 12,764 個 token 被重寫。

👉 詳見：

- [什麼是提示詞快取](https://renkeimap.jp/guide/learn/prompt-caching) — 3 家公司的折扣率、快取生效條件、以及會讓快取失效的操作。
- [AI token 很快就減少時](https://renkeimap.jp/guide/fix/agent-token-cost) — 先檢查指示檔、工具、快取、對話長度這 4 件事的順序。
- [什麼是 token（AI）](https://renkeimap.jp/guide/term/llm-token) — 日文 1,000 字大約是多少 token，以及為什麼費用與上限要用 token 來計算。

---

## 5\. 原則 5：對話變長後，先整理重點再開新對話

[![長對話中 AI 變得不再遵守指示時的 4 種修正方式示意圖。原因包括：對話過長、舊對話被 context 壓縮（compact）、指示只存在於對話中。1 把指示移到指示檔（AGENTS.md、CLAUDE.md），壓縮後仍會重新讀取。2 請 AI 先整理目前為止的指示與決定，再貼到新對話一開始。3 指示 compact 時保留哪些內容（Claude Code 可在 /compact 後寫入重點，CLAUDE.md 也可寫 Compact Instructions 章節）。4 將無關工作拆到其他對話（subagent）。研究中有指示在 8 回合內就開始偏離的例子（2024 年），以及把條件逐一在後面補上時，6 項工作的平均成績下降 39% 的例子（2025 年）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-forgets-fixes.a7ea4cce.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7b48236ed1c222c767d0b520265d05f2)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-forgets-fixes.a7ea4cce.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7b48236ed1c222c767d0b520265d05f2)

在同一段對話中一直來回，AI 有時會開始不遵守指示。原因如圖上方所示：對話太長、舊對話會在 context 壓縮（compact）時被摘要、以及指示只存在於對話內容中。

學習AI 課程

編輯部的實驗中，即使中間插入了 20 回合無關內容（輸入約 3 萬 token），Claude Fable 5.1 仍然遵守了最初的 8 項指示。不過，對話越長，每次送出的輸入也會增加。即使是在空資料夾中，只說「請回答 ok」，送給 AI 模型的內容每次也有 13,084～18,920 token（Claude Code 18,920、Antigravity CLI 14,709、Codex 13,084）。當接近 context window 上限時，舊對話會被 context 壓縮而整理，細節指示可能會消失。研究中也有舊模型（LLaMA2-chat-70B 與 GPT-3.5）在 8 回合內就開始偏離 system prompt 指示的例子（2024 年），以及當條件在每個回合只增加 1 項時，6 項任務的平均成績下降 39% 的例子（2025 年）。

修正方式如圖中的 4 點：

1. 把每次都要遵守的指示移到指示檔（ `AGENTS.md` 、 `CLAUDE.md` ）。Claude Code 在壓縮後仍會從磁碟重新讀取專案根目錄的指示檔。
2. 請 AI 將到目前為止的指示與已決定事項整理好，貼到新對話的開頭。Claude Code 即使開新對話，只要開頭相同，從第 2 次起有 99.99% 的內容會從快取讀取，因此費用不會大幅增加。
3. 指示在壓縮時要保留哪些內容。Claude Code 可以在 `/compact` 後指定希望保留的重點。也可以在 `CLAUDE.md` 裡寫 Compact Instructions 章節。
4. 將無關的工作（例如除錯）拆到其他對話或 subagent。

👉 詳見：

- [長對話中指示不再被遵守時](https://renkeimap.jp/guide/fix/agent-forgets) — 原因與 4 種修正方式，以及插入 20 回合無關內容的實驗。
- [什麼是 AI 的 context](https://renkeimap.jp/guide/learn/agent-context) — 3 個產品每次送給 AI 模型的內容與其容量。
- [什麼是 context window](https://renkeimap.jp/guide/term/context-window) — 為什麼即使有「100 萬 token」，也不代表全部都能同樣使用。

---

## 6\. 原則 6：長資料用檔案，專門手順用 SKILL 傳遞

[![說明長資料、專門手順與指示何時會送達 AI 的圖，依放置位置分成 3 個時點（第一回合、AI 使用時、之後的回合）（根據 Claude Code 的實驗與文件）。指示檔（AGENTS.md 等）在 3 個時點都會全文送達，適合放每次都要遵守的指示。SKILL 在第一回合只送出名稱與說明（+28 token），等 AI 決定要用時才讀本文，之後的回合也會把本文當作對話內容的一部分送達，適合偶爾才用的專門手順。檔案在 AI 讀取前不會送達，讀過後會把內容當作對話的一部分持續送達，適合長資料與長輸出。貼到對話中的文字，從貼上那刻起就會在該對話中持續送達，適合臨時命令](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14d-where.5b711d18.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4f839b8d032ffcbe17dc1e1443cd1de7)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14d-where.5b711d18.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4f839b8d032ffcbe17dc1e1443cd1de7)

長規格書或特定工作的手順書，不要直接貼進聊天，而是把長資料當作檔案傳遞，偶爾才用的專門手順則做成 SKILL。正如圖所示，不同放置位置，AI 讀到的時間點也不同。

學習AI 課程

- 指示檔：從第一回合開始，每次都會全文送達。只寫每次都要遵守的指示。
- SKILL：一開始只會送出名稱與說明（在 Claude Code 中，輸入只增加 28 token）。當 AI 決定要使用時才讀取本文，而之後的回合中，已讀取的本文會作為對話的一部分每次送達。
- 檔案：在 AI 讀取之前不會送達。讀過之後，內容會作為對話的一部分每次送達。
- 貼在對話中：從貼上的那一刻起，在該對話中每次都會送達。

不過，就算是為了減少 token 而改成檔案，token 也不一定會變少。把 2,893 字的資料以檔案方式提供時，光是 AI 呼叫讀檔工具，就讓 2 回合的輸入總量增加了約 2 萬 token。長資料之所以要放成檔案，原因是讓 AI 把長輸出直接寫到檔案，而不是寫進對話，避免對話紀錄被長文撐大。

MCP 的工具也採用同樣的思路來縮減數量。

- Claude Code 支援延後載入工具說明（deferred），就算新增 150 個 MCP 工具，最初輸入的增加也只有 2,482 token。
- Anthropic 的文件指出，當可用工具超過 30～50 個時，選對工具的能力會下降。

👉 詳見：

- [指示要寫在哪裡](https://renkeimap.jp/guide/learn/where-to-put-rules) — 比較對話、 [AGENTS.md](http://agents.md/) 、SKILL 與檔案中，8 項指示的遵守情況與 token 數。
- [什麼是 AI 的技能（SKILL）](https://renkeimap.jp/guide/learn/agent-skills) — SKILL 何時、會讀入多少內容。
- [長資料要貼在對話中，還是用檔案傳遞](https://renkeimap.jp/guide/learn/agent-files) — 用檔案傳遞時 token 的增加方式與原因。

---

## 7\. 3 個產品差異一覽表

我們把 3 個產品的差異，根據實驗結果與各家文件整理成一張表（2026 年 10 月 18 日）。

比較項目Claude Code 2.1.302Codex 0.163.0Antigravity CLI 1.4.2自動讀取的指示檔 `CLAUDE.md`  
（沒有時才讀 `AGENTS.md` ）只讀 `AGENTS.md` 會讀 `AGENTS.md` 與 `GEMINI.md` （兩者都讀）可讀取大小上限550KB 也能全文讀取（文件上可達 4 MiB）前 32 KiB（32,768 位元組）文件上 1 個檔案 24,000 位元組（實驗時依回合而異）沒有尾端 S 的 `AGENT.md` 不讀（0/3）不讀（0/3）6 次中只讀到 1 次（1/6）只放 SKILL 時只讀到說明（+28 token）視情況而定會讀到本文（3/3）MCP 工具的延後載入支援支援（150 個工具時 +2,482 token）本次實驗未測本次實驗未測另一段對話中的快取從第 2 次起 99.99%0～64.7%0%（同一段對話中為 88.4%）查看送入內容的指令 `/context``/status``/context` context 壓縮（compact）約 96.7 萬 token 時自動；也可用 `/compact` 在 context window 的 90% 時自動；也可用 `/compact` 舊回合會被摘要成 checkpoint

學習AI 課程

---

## 8\. 想知道更多——依問題挑選 IT 連攜地圖的 guide

本文的 6 項原則，是從 IT 連攜地圖的 13 篇 guide 中濃縮出的重點。設定方法、實驗數據與出處，請參考以下 guide。

### 我現在就想解決遇到的問題

困擾・想知道的事guide可了解內容指示不被遵守 [AI 不遵守指示時](https://renkeimap.jp/guide/fix/agent-ignores-instructions) 確認步驟，以及 8 種排列方式各自的分數token 減很快 [AI token 很快就減少時](https://renkeimap.jp/guide/fix/agent-token-cost) 先確認指示檔、工具、快取、對話長度的順序長對話中忘記指示 [長對話中指示不再被遵守時](https://renkeimap.jp/guide/fix/agent-forgets) 原因與 4 種修正方式### 了解機制後，想決定放置位置與寫法

困擾・想知道的事guide可了解內容想建立指示檔 [什麼是 AGENTS.md](https://renkeimap.jp/guide/learn/agents-md) 與 `CLAUDE.md` 、 `GEMINI.md` 的差異、各產品讀哪些檔案、Codex 的 32 KiB 上限想了解指示寫法與優先順序 [什麼是提示詞](https://renkeimap.jp/guide/learn/prompt) 提示詞會送達的 3 個位置、互相衝突時的優先順序、排列方式與數量想降低快取費用 [什麼是提示詞快取](https://renkeimap.jp/guide/learn/prompt-caching) 3 家公司的折扣率、快取生效條件與失效操作想決定指示放哪裡 [指示要寫在哪裡](https://renkeimap.jp/guide/learn/where-to-put-rules) 對話、 [AGENTS.md](http://agents.md/) 、SKILL、檔案的遵守方式與 token 想把流程拆成 SKILL [什麼是 AI 的技能（SKILL）](https://renkeimap.jp/guide/learn/agent-skills) SKILL 何時、會讀入多少內容想把長資料交給檔案 [長資料要貼在對話中，還是用檔案傳遞](https://renkeimap.jp/guide/learn/agent-files) 用對話與檔案傳遞時的 token 差異想了解每次送出的內容 [什麼是 AI 的 context](https://renkeimap.jp/guide/learn/agent-context) 空資料夾中仍超過 1 萬 token 的內容內訳### 想正確理解詞語的意思

困擾・想知道的事guide可了解內容什麼是 token [什麼是 token（AI）](https://renkeimap.jp/guide/term/llm-token) 日文 1,000 字大約是多少 token想知道一次可讀多少 [什麼是 context window](https://renkeimap.jp/guide/term/context-window) 為什麼即使有「100 萬 token」，也不代表全部都能同樣使用想知道漏看中間的現象 [什麼是 Lost in the Middle](https://renkeimap.jp/guide/term/lost-in-the-middle) 論文測了什麼，以及現今 AI 的實驗結果

**讀者問卷進行中**  
如果你希望我們更深入挖掘這個主題，請在文章按「讚」。我們會依照按讚數由高到低追加調查，結果會在新文章中公布（追蹤後會收到通知）。

---

原文出處： [https://qiita.com/songchong/items/351a939e2e1b673c2929](https://qiita.com/songchong/items/351a939e2e1b673c2929)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/x0nRDa) [67瀏覽](https://codelove.tw/@tony/post/x0nRDa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/x0nRDa)

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

🔥 [Jev是什麼？理解它與傳統型 LLM 與 AI 代理人的差異](https://codelove.tw/@tony/post/3DRvQq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝11 💬1

341

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商事務所：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1792560502&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx0nRDa%23google_vignette&amp;pra=5&amp;aiof=14&amp;asro=0&amp;itsi=4&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODUiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODUiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44NSJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792560502235&amp;bpp=6&amp;bdt=137&amp;idt=26&amp;shv=r20261019&amp;mjsv=m202610190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792560489%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=8672156131777&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=120&amp;eid=95404823%2C95405492%2C95405781%2C122881750&amp;oid=2&amp;pvsid=2583390848930567&amp;tmod=869549676&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=36" title="Advertisement" aria-label="Advertisement"></iframe>