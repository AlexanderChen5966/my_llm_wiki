---
title: "別再每次都把資料貼給 AI！善用檔案、SKILL 與 AGENTS.md，Token 計費最高可減少 6 成？實測 6 種方法"
source: "https://codelove.tw/@tony/post/3KK503"
author:
published:
created: 2026-10-29
description: "![6種資料與指示的傳遞方式的輸入費用比較表，以 1b（只在第一輪貼上並參照）為基準，分成 4 個實驗（測量、短文 10 輪、長文 5 輪）。Claude Code・Claude Sonnet 5.5・短文：1a 兩次貼上 $0.2401（較 1b +136%）、1b 參照指示 $0.1017（基準..."
tags:
  - "clippings"
status: "inbox"
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [10月26日](https://codelove.tw/@tony/post/3KK503 "2026-10-26 17:02:03")

## 別再每次都把資料貼給 AI！善用檔案、SKILL 與 AGENTS.md，Token 計費最高可減少 6 成？實測 6 種方法

[![6種資料與指示的傳遞方式的輸入費用比較表，以 1b（只在第一輪貼上並參照）為基準，分成 4 個實驗（測量、短文 10 輪、長文 5 輪）。Claude Code・Claude Sonnet 5.5・短文：1a 兩次貼上 $0.2401（較 1b +136%）、1b 參照指示 $0.1017（基準）、2 外部檔案 $0.1286（較 1b +26%）、3 SKILL 指示 $0.1404（較 1b +38%）、4 CLAUDE.md $0.1024（較 1b +1%）、5 檔案＋SKILL $0.2180（較 1b +114%）。Antigravity・Claude Sonnet 5.5・短文：1a 兩次貼上 $0.1361（較 1b +83%）、1b 參照指示 $0.0745（基準）、2 外部檔案 $0.1409（較 1b +89%）、3 SKILL 指示 $0.0748（較 1b ±0%）、4 AGENTS.md $0.0706（較 1b -5%）、5 檔案＋SKILL $0.0959（較 1b +29%）。Antigravity・Gemini 3.8 Flash・短文：1a 兩次貼上 $0.0603（較 1b -41%）、1b 參照指示 $0.1017（基準）、2 外部檔案 $0.1313（較 1b +29%）、3 SKILL 指示 $0.1136（較 1b +12%）、4 AGENTS.md $0.1082（較 1b +6%）、5 檔案＋SKILL $0.1529（較 1b +50%）。Antigravity・Gemini 3.8 Flash・長文：1a 兩次貼上 $0.1119（較 1b +173%）、1b 參照指示 $0.0410（基準）、2 外部檔案 $0.0754（較 1b +84%）、3 SKILL 指示 $0.0442（較 1b +8%）、4 AGENTS.md $0.0402（較 1b -2%）、5 檔案＋SKILL $0.0491（較 1b +20%）。不含輸出。](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-summary.bf1dfc63.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2e9dc3a5945f58fee901df8e6560b31b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-summary.bf1dfc63.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2e9dc3a5945f58fee901df8e6560b31b)

我們營運的 IT 連接地圖技術指南（例如 [什麼是提示快取](https://renkeimap.jp/guide/learn/prompt-caching) ）中，一直在解釋「前一次對話中開頭相同的部分會從快取讀取，因此費用大幅下降」這個機制。提示快取會由 AI 代理與 AI 服務端（如 Anthropic、Google 等）自動使用。不過，對於資料與指示的不同傳遞方式——例如「直接貼到使用者提示」「外部檔案（ [article.md](http://article.md/) ）」「 [SKILL](https://renkeimap.jp/guide/learn/agent-skills) 」「 [AGENTS.md](https://renkeimap.jp/guide/learn/agents-md) 」等——其 [token](https://renkeimap.jp/guide/term/llm-token) 消耗究竟如何變化，始終是一個疑問。

學習AI 課程

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1793265955&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK503&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguNDgiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguNDgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjQ4Il1dLDBd&amp;dt=1793265955789&amp;bpp=1&amp;bdt=2355&amp;idt=-M&amp;shv=r20261028&amp;mjsv=m202610260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793265954%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793265954%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793265954%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250&amp;nras=2&amp;correlator=3530513410619&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=1594&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95404667%2C95405492%2C95405781%2C95406274%2C122881730%2C95405500%2C31061691%2C31061693&amp;oid=2&amp;pvsid=1271033611352165&amp;tmod=708190788&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1944%2C30%2C1944%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=36" title="Advertisement" aria-label="Advertisement"></iframe>

因此，IT 連接地圖編輯部以 [Claude Code](https://renkeimap.jp/tool/claude-code) （訂閱版）與 [Antigravity](https://renkeimap.jp/tool/antigravity) CLI，測試了 6 種資料與指示的傳遞方式。模型為 [Claude](https://renkeimap.jp/tool/claude) Sonnet 5.5 與 [Gemini](https://renkeimap.jp/tool/gemini) 3.8 Flash。上方圖表是將 4 個實驗的輸入費用，以 1b（只在第一輪貼上並參照）為基準，按各方式排列而成。對話以短文進行 10 輪、長文進行 5 輪，測量所有回合的 token 數，並依官方費率換算輸入費用。

學習AI 課程

## 先得結論

1. **不同情況該用哪一種** ：每次都要遵守的共通規則放在 [AGENTS.md](http://agents.md/) （Claude Code 則是 [CLAUDE.md](http://claude.md/) ），特定任務的流程放在 SKILL，長篇且會反覆閱讀、即使摘要後也要回頭看的資料放在檔案，而只會用一次的資料則在第一輪貼進對話，之後只做參照。關鍵不是費用，而是這些資訊要用多久、用到什麼範圍。
2. **費用差異最大的，是是否重貼同一份資料** ：每輪都重貼的 1a，相較於參照的 1b，輸入費用在 Claude Code 中高出 136%，在 Antigravity 的 Claude 短文中高 83%，在 Gemini 的長文中高 173%。換句話說，只要不重貼，輸入費用就能在 Claude Code 降低 58%，在 Gemini 長文降低 63%。
3. **[AGENTS.md](http://agents.md/) ・SKILL・檔案並不會大幅降低費用** ： [AGENTS.md](http://agents.md/) （4）與 1b 幾乎相同（-5～+6%）。SKILL 與檔案（2・3・5）則因代理程式要來回讀取，費用與 1b 相同或更高。
4. **不過在 Gemini 的短對話中，1a（在對話中重貼兩次）反而較便宜** ：這是因為 1b 等方式中，單次輸入未達本次快取可用的最小量（19,492 token），因此無法使用快取；而在重貼資料後輸入量變大的 1a 中，反而能使用快取。這只是偶然結果，並不代表重貼兩次比較好（見第 6 節）。

> 📘 **完整輪次的測量資料與分析（IT 連接地圖指南）**
> 
> 探索數據管理
> 
> - [資料與指示的 6 種傳遞方式 — 如何取捨與費用差異](https://renkeimap.jp/guide/learn/agent-input-strategy)
> - [資料與指示的 6 種傳遞方式的 token 測量與費用（測量）](https://renkeimap.jp/guide/learn/agent-input-experiment)

## 1\. 6 種方法分別適合什麼情況？

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1793265955&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK503&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguNDgiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguNDgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjQ4Il1dLDBd&amp;dt=1793265955789&amp;bpp=1&amp;bdt=2355&amp;idt=-M&amp;shv=r20261028&amp;mjsv=m202610260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793265954%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793265954%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793265954%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C952x280&amp;nras=3&amp;correlator=3530513410619&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=2735&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95404667%2C95405492%2C95405781%2C95406274%2C122881730%2C95405500%2C31061691%2C31061693&amp;oid=2&amp;pvsid=1271033611352165&amp;tmod=708190788&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1944%2C30%2C1944%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=36" title="Advertisement" aria-label="Advertisement"></iframe>[![資料與指示的 6 種傳遞方式的實務聊天 UI 比較圖。對話框 {…} 與事前準備檔案📄的組合所構成的 2 輪指示流程。1a 對話中重貼兩次：Turn 1 傳送[資料本文]與[指示本文]，Turn 2 再次貼上資料本文並下修指示（把已貼資料在快取之外再付一次）。1b 對話貼上與參照指示：Turn 1 傳送[資料本文]與[指示本文]，Turn 2 只送出「前面的文章」與參照指示。2 資料放入外部檔案：事前準備📄article.md，Turn 1 指示讀取 article.md 並依指示撰寫，Turn 2 也指定 article.md。3 指示放入 SKILL：事前準備📄SKILL，Turn 1 指示依[資料本文]與 SKILL 撰寫，Turn 2 指示依前面的文章與 SKILL 精簡。4 指示放入 AGENTS.md：事前準備📄AGENTS.md（常駐），Turn 1 指示依[資料本文]撰寫，Turn 2 只送出前面的文章與參照指示。5 資料檔案＋SKILL：事前準備📄article.md 與📄SKILL，Turn 1 指示依 article.md 與 SKILL 撰寫，Turn 2 也依 article.md 與 SKILL 精簡。](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-ui-methods.87cebb13.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6107e277472bf90a494b8b5c290ee90d)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-ui-methods.87cebb13.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6107e277472bf90a494b8b5c290ee90d)

取捨方式不是依費用，而是依資訊要用多久、用到哪裡來決定。費用欄位是將 10 輪（長文為 5 輪）的輸入費用，與 1b（只在第一輪貼上並參照）相比較的結果。

查閱線上詞典

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1793265958&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK503&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguNDgiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguNDgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjQ4Il1dLDBd&amp;dt=1793265955771&amp;bpp=1&amp;bdt=2337&amp;idt=0&amp;shv=r20261028&amp;mjsv=m202610260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793265954%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793265954%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793265954%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C1654x873%2C1005x93&amp;nras=6&amp;correlator=3530513410619&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=3929&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=533&amp;eid=95404667%2C95405492%2C95405781%2C95406274%2C122881730%2C95405500%2C31061691%2C31061693&amp;oid=2&amp;psts=AOrYGsnsracAApRFmi08Y_9i9sKpdZWQXQY1aWHJ15VVod4pHwk4g_vMmnwdB81_CDQX6B32BAiFVgGQGxqnJJQrqJfNkDW9xHhZMMiu-WNJOq7S3XrBK-z-orzMdg%2CAOrYGsk1hSt75mXYwxKfhfTIjOn1SXU5_DVaStLcmSVwHZUICxSRUp1RJL7DE0FoaqAWs9ZkW1BrM9PbnN7d9Y50GNcviSHuW2PZy3jVn9FPpKv1%2CAOrYGsn6bDHd6kk2YGkRc10_66vbJiwxOk0GhrgbOi7VJkho3GChW-SoNlAKRJsM688lNP1DtzAljliiuveFpY0UHslLUb0fz2L6DmXmztlFO_nx&amp;pvsid=1271033611352165&amp;tmod=708190788&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1944%2C30%2C1944%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=5&amp;fsb=1&amp;dtd=2315" title="Advertisement" aria-label="Advertisement"></iframe>

方式放置位置適合的情況與 1b 相比的輸入費用（Claude Code・Claude 短文／Antigravity・Claude 短文／Antigravity・Gemini 短文／Antigravity・Gemini 長文） **1a 對話中重貼兩次** 每輪都貼在對話中不用+136%／+83%／−41%／+173% **1b 對話貼上與參照指示** 只在第一輪貼入對話一次性資料・短篇任務基準 **2 資料放入外部檔案** 檔案（例如 `article.md` ）反覆閱讀的長篇資料・摘要後仍需回看的資料+26%／+89%／+29%／+84% **3 指示放入 SKILL** SKILL（Claude Code 為 `.claude/skills/` ）僅在特定任務使用的流程+38%／±0%／+12%／+8% **4 指示放入 [AGENTS.md](http://agents.md/)** [AGENTS.md](http://agents.md/) （Claude Code 為 [CLAUDE.md](http://claude.md/) ）每次都要遵守的共通規則+1%／−5%／+6%／−2% **5 資料檔案＋SKILL** 檔案＋SKILL以相同流程重複處理長篇資料的任務+114%／+29%／+50%／+20%當第二輪之後要參照時，最好不要用「前面的文章」，而是寫出對象的標題。因為對話一長或資料一多，「前面的文章」會變得不夠明確。本次實驗最後一輪也沒有寫「前面的文章」，而是直接點名「重寫版」（因為長文中「前面的文章」可能會指向 4 萬字的原文）。

學習AI 課程

- ❌ 再次貼上資料全文並說「修改一下」（資料的費用會重新支付，且會累積到歷史中）
- △ 「把前面文章的第 3 段縮短一點」（對話變長時，目標容易變得模糊）
- ◎ 「把剛才寫的『總務省白書 令和 5 年版摘要』第 3 段縮短一點」
- ◎ 「把 `article.md` 的第 3 段縮短一點」

那麼，費用差異到底有多大呢？

## 2\. 費用差異有多大？

[![4 個實驗中，以平滑曲線呈現各輪輸入費用累計的圖（測量）。Claude Code・Claude 短文（10 輪）最便宜的是 1b 參照指示 $0.1017，最貴的是 1a 兩次貼上 $0.2401，1a 為 1b 的 2.4 倍。Antigravity・Claude 短文（10 輪）最便宜的是 4 AGENTS.md $0.0706、最貴的是 2 外部檔案 $0.1409，1a 為 1b 的 1.8 倍。Antigravity・Gemini 短文（10 輪）最便宜的是 1a 兩次貼上 $0.0603、最貴的是 5 檔案＋SKILL $0.1529，1a 為 1b 的 0.6 倍。Antigravity・Gemini 長文（5 輪）最便宜的是 4 AGENTS.md $0.0402、最貴的是 1a 兩次貼上 $0.1119，1a 為 1b 的 2.7 倍。](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-turns.9b5d55a2.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1104b4cd73b1733a6e2b1619e6dfc47a)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-turns.9b5d55a2.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1104b4cd73b1733a6e2b1619e6dfc47a)

方式名稱Claude Code・Claude 短文（10 輪）Antigravity・Claude 短文（10 輪）Antigravity・Gemini 短文（10 輪）Antigravity・Gemini 長文（5 輪） **1a 對話中重貼兩次** $0.2401（+136%）$0.1361（+83%）$0.0603（−41%）$0.1119（+173%） **1b 對話貼上與參照指示** $0.1017（基準）$0.0745（基準）$0.1017（基準）$0.0410（基準） **2 資料放入外部檔案** $0.1286（+26%）$0.1409（+89%）$0.1313（+29%）$0.0754（+84%） **3 指示放入 SKILL** $0.1404（+38%）$0.0748（±0%）$0.1136（+12%）$0.0442（+8%） **4 指示放入 [AGENTS.md](http://agents.md/) （Claude Code 為 [CLAUDE.md](http://claude.md/) ）** $0.1024（+1%）$0.0706（−5%）$0.1082（+6%）$0.0402（−2%） **5 資料檔案＋SKILL** $0.2180（+114%）$0.0959（+29%）$0.1529（+50%）$0.0491（+20%）（ ）內為與 1b 相比的增減。上圖為各輪輸入費用的累計，線條斜率即該輪所支付的輸入費用。

- **Claude Code・Claude 短文** ：1b 與 4（CLAUDE.md）最便宜，1a 是 1b 的 2.4 倍。Claude Code 也會回傳寫入快取的次數，因此得以測量；不過輸入費用的大多數來自快取寫入（1a 佔 84%，1b 佔 75%）。
- **Antigravity・Claude 短文** ：4（AGENTS.md）、1b、3（SKILL）幾乎同樣便宜，而把資料放進檔案的 2 最貴。
- **Antigravity・Gemini 短文** ：重貼的 1a 反而最便宜（見第 4 節）。
- **Antigravity・Gemini 長文** ：4 與 1b 最便宜，1a 是 1b 的 2.7 倍。

那麼，為什麼重貼會變貴，而檔案與 SKILL 不會變便宜呢？

## 3\. 為什麼重貼會變高，而檔案與 SKILL 不會變便宜？

[![重貼資料的 1a 與參照資料的 1b 比較圖（Claude Sonnet 5.5・短文測量）。第 2 輪時，1a 在快取外的輸入為 8,573（比 1b 多 2,461），1a 重新寫入了全文 3,123 字；1b 的回覆只有 127 字。10 輪輸入費用中，1a 為 $0.1361、1b 為 $0.0745（1a 比較下為 -45%）。在 Gemini 長文（5 輪）中，1b 便宜 63%；而在 Gemini 短文中，1b 因未命中快取而高出 69%。](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-why-not-repaste.2574d415.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4900d132b67676b78d2fa51b17903d7f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-why-not-repaste.2574d415.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4900d132b67676b78d2fa51b17903d7f)

提示快取只會低價讀取與前次查詢從開頭起一致的部分。到了第 2 輪若把資料重新貼一次，貼上的那段會被當成新的輸入並以一般費率計算，而且還會留在歷史（ [context](https://renkeimap.jp/guide/learn/agent-context) ）裡，因此從第 3 輪起每一輪都得持續送出。Antigravity 的 Claude 短文中，單輪輸入量，1a 每輪增加 3,457 token，而參照的 1b 只有 929 token。

SKILL 和檔案之所以沒有變便宜，是因為代理程式每次讀取 SKILL 或檔案時，都得再次送出先前的內容。把資料放在檔案中的 2，代表代理程式每輪都重新讀取 `article.md` 並覆寫，Antigravity 的 Claude 甚至還會寫出確認用腳本。在 Antigravity 的 3 個實驗中，2 的單輪輸入量是 1b 的 1.3～3.4 倍。檔案的用途不是為了降低費用，而是為了在摘要後仍可重讀。

那麼，指示與資料應該依什麼順序決定放在哪裡呢？

## 4\. 指示與資料該依什麼順序決定放哪裡？（建議順序）

[![指示與資料的放置位置建議順序。Step 1 全域共通規則放在 AGENTS.md 或 GEMINI.md（會在對話開始時被讀取並固定在前面）。Step 2 專門任務流程放在 SKILL（平常只讀名稱與說明文字，使用時才讀正文）。Step 3 工作對象資料放在外部檔案（摘要後仍可重讀，但因讀寫往返輸入會增加）。Step 4 對話提示只保留命令與參照，第二輪以後不再重貼。](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-priority.b7613002.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=747f74034aa000170bb1db7603dd010f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-priority.b7613002.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=747f74034aa000170bb1db7603dd010f)

放置位置請依上圖 Step 1 起，根據資訊需要使用多久、用到多廣來決定。

1. **Step 1：全體共通規則 → [AGENTS.md](https://renkeimap.jp/guide/learn/agents-md) （Claude Code 為 [CLAUDE.md](http://claude.md/) ，Gemini 為 [GEMINI.md](http://gemini.md/) ）**
	- 理由：會在對話開始時讀取並固定在開頭，因此每輪查詢都會從快取中讀到。因為沒有來回讀取，Antigravity 的 Claude 短文（$0.0706）與 Gemini 長文（$0.0402）都最便宜，Claude Code（CLAUDE.md）也幾乎與 1b 相同（+1%）。
2. **Step 2：特定任務流程 → [SKILL](https://renkeimap.jp/guide/learn/agent-skills) （Antigravity 為 `.agents/skills/` ，Claude Code 為 `.claude/skills/` ）**
	- 理由：平常只會載入名稱與說明，只有執行該任務時才讀正文。由於第一輪有讀取往返，在 Antigravity 中相較 1b 約為 ±0～+12%，而在 Claude Code 中 10 輪後比 1b 高 38%。
3. **Step 3：工作的對象資料 → 外部檔案（例如 `article.md` ）**
	- 理由：摘要後仍能重讀，可反覆使用。但代理程式讀寫檔案的往返，使單輪輸入變成 1b 的 1.3～3.4 倍。這不是為了省費用，而是為了能重讀（ [長篇資料是貼上，還是用檔案傳遞](https://renkeimap.jp/guide/learn/agent-files) ）。
4. **Step 4：本次指示 → 使用者提示（對話）**
	- 理由：第二輪以後不重貼資料，而是只加上標題的參照指示，讓前面的內容從快取讀取。

放置位置的決定方式，也已在指南 [指示該寫在哪裡 — 對話・AGENTS.md・SKILL](https://renkeimap.jp/guide/learn/where-to-put-rules) 與 [資料與指示的 6 種傳遞方式](https://renkeimap.jp/guide/learn/agent-input-strategy#recommended-order) 中說明。那麼，在持續對話時，該注意什麼呢？

## 5\. 在對話中不要白白用掉快取的 4 個鐵則是什麼？

[![在對話中避免浪費快取的 4 個鐵則。鐵則 1：不要重貼，改用標題參照。鐵則 2：如果修改了指示檔，請開新對話。鐵則 3：了解快取可用的最小量，以及何時不會命中快取（官方：Claude Sonnet 5.5 為 512，Gemini 3.8 Flash 為 4,096。本次 Gemini 在單次輸入小於 17,459 時都未命中快取）。鐵則 4：超過保留時間再恢復時，整段歷史都要重新支付（Claude Code 官方為訂閱版 1 小時、API 金鑰 5 分鐘。實測 Claude Code 為 61 分鐘、Antigravity 的 Claude 為 6 分鐘、Gemini 為 12 分鐘後失效；失效後的單輪輸入費用是快取可讀時的 11 倍、9.4 倍、6.1 倍。若要長時間中斷，請在工作告一段落時使用 /compact）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-rules.73ec1b24.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=42347b5d0c4cf8fac1b3dcd7c775fe61)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-rules.73ec1b24.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=42347b5d0c4cf8fac1b3dcd7c775fe61)

### 鐵則 1：不要重貼，改用標題參照

一旦送出的資料會留在對話歷史中，下一次查詢時會從快取讀取。要修改時，不要把同樣的文字再貼一次，而是像「把剛才寫的『總務省白書 令和 5 年版摘要』第 3 段縮短一點」這樣，直接點名對象標題或標頭來參照（例子見第 1 節）。只說「前面的文章」並不足夠，因為對話一長或資料一多，指涉就會變得模糊。

### 鐵則 2：修改指示檔後，請開新對話

Claude Code 的官方說明中，專案的 `CLAUDE.md` （與 `AGENTS.md` ）只會在對話開始時讀取一次。即使在對話中途修改，也不會讓快取失效，但修改後的內容也不會傳到目前這段對話中。修正後的指示會在 `/clear` 、 `/compact` 或新對話開始後才會被讀入。若修改了指示檔，請重新開啟對話。

### 鐵則 3：知道快取可用的最小量，以及「不會命中快取」的情況

Claude Sonnet 5.5 從 512 token、Gemini 3.8 Flash 的隱式快取從 4,096 token 起可用（官方）。但本次 Gemini 中，單次輸入在 17,459 token 以下的回合，從未命中快取（見第 6 節）。費用請以使用後的 usage（從快取讀取的 token 數）確認（ [什麼是提示快取](https://renkeimap.jp/guide/learn/prompt-caching) ）。

### 鐵則 4：超過保留時間再重開，就得把整段歷史重新付費

快取的保留時間，Claude Code 訂閱版為 1 小時，API 金鑰（API 是 Application Programming Interface 的縮寫）與雲端經由方式為 5 分鐘（官方）。實測顯示，Claude Code 訂閱版在相隔 58 分鐘前的回合仍可從快取讀取，從 61 分鐘起失效；Antigravity 中 Claude 在相隔 6 分鐘後失效，Gemini 則在 12 分鐘後失效（ [快取保留時間](https://renkeimap.jp/guide/learn/agent-cache-ttl) ）。一旦超過這個時間，繼續同一段對話時，就會把歷史重新寫入快取。若長對話要中斷超過 1 小時，請在工作告一段落時使用 `/compact` ，或把摘要保存在檔案後開啟新對話（ [什麼是 compact](https://renkeimap.jp/guide/learn/compaction) ）。在恢復工作後才做 compact，幾乎不會便宜多少。

最後，在 Gemini 的短對話中，為什麼會反過來呢？

## 6\. Gemini 的短對話為何會出現反轉？

[![從機制對照到實驗結果的對應圖。①只有從開頭起一致的部分會被重用：1a 的單輪輸入在 Claude 短文中每輪增加 3,457、長文中增加 22,705（1b 為 929、256）。1a 與 1b 的差異，可僅用資料項的公式解釋 Claude 短文的 89% 與長文的 109%。②使用工具時，單輪內會多次查詢：方式 2 每輪的輸入約為 1b 平均的 2.59 倍，同樣的請求中，第一次輸入為 52,246 與 96,212。③快取可用的最小量：官方為 Claude Sonnet 5.5 的 512、Gemini 3.8 Flash 的 4,096。Gemini 中，單次輸入在 17,459 以下的 20 次都未命中快取，而 19,492 以上的 19 次則都命中快取。](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-mechanism.6e052f26.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=f582484aff4115e739ffa6c608639c0f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fq14e-mechanism.6e052f26.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=f582484aff4115e739ffa6c608639c0f)

Gemini 短對話中 1a 之所以最便宜，是因為快取是否被使用，會依方式而分開。如下表所示，1b・3・4 在 10 輪內一次都沒有命中快取，因此每輪都以一般費率支付全部輸入。重貼的 1a 因為單次輸入變大，從第 4 輪開始命中快取，因此成為最便宜的方式。

方式單輪輸入（token・最小～最大）命中快取的輪次10 輪輸入費用（與 1b 相比） **1a 對話中重貼兩次** 10,978～31,488第 4 輪起（7 次）$0.0603（−41%） **1b 對話貼上與參照指示** 10,985～15,272一次都沒有$0.1017（基準） **2 資料放入外部檔案** 20,572～73,954第 5 輪起（6 次）$0.1313（+29%） **3 指示放入 SKILL** 12,985～22,262一次都沒有$0.1136（+12%） **4 指示放入 [AGENTS.md](http://agents.md/)** 11,063～16,266一次都沒有$0.1082（+6%） **5 資料檔案＋SKILL** 19,658～64,274第 7 輪起（4 次）$0.1529（+50%）Gemini 3.8 Flash 的隱式快取，官方說明為從 4,096 token 起可用，但不保證一定會命中。本次測量中，單次查詢輸入在 17,459 token 以下的 20 次都沒有命中快取，而 19,492 token 以上的 19 次則全部命中。本次實測可用的最小量為 19,492 token，而參照的 1b 在到第 10 輪時，單次輸入仍未達到這個最小量。2 與 5 也在途中開始命中快取，但因為讀取檔案與 SKILL 的往返使輸入變多，所以仍高於 1b。

這只是偶然結果，不代表「重貼比較省錢」。同樣是 Gemini 3.8 Flash，在資料較長的長文對話中，所有方式都在第 2 輪前就開始命中快取，而參照的 1b 比重貼的 1a 便宜 63%。是否命中快取，請以使用後的 usage（從快取讀取的 token 數）確認。

## 7\. 怎麼測的？

項目內容 **產品與模型** Claude Code 2.1.310（訂閱版）・Claude Sonnet 5.5（實驗 4）。Antigravity CLI 1.4.6・Claude Sonnet 5.5（low）・Gemini 3.8 Flash（low）（實驗 1～3） **資料與需求** 將總務省資訊通信白皮書的短文（2,893 字）或長文（39,875 字），依 8 種寫法規則重寫。從第 2 輪開始，每輪只請對方修改一處 **輪次・次數** 短文 10 輪・長文 5 輪，各方式各 1 次。每次實驗都建立新的工作資料夾 **費用** 將輸入費用依官方費率換算（Claude Sonnet 5.5：輸入 $2、1 小時保留的快取寫入 $4、從快取讀取 $0.10；Gemini 3.8 Flash：輸入 $0.75、從快取讀取 $0.075，以每 100 萬 token 計）。由於輸出費用每次波動很大，因此不列入比較 **注意** Antigravity 不會回傳快取寫入數，因此那部分會視為快取外的一般輸入費用。Claude Code（實驗 4）會回傳快取寫入數，所以可測得包含加成的數值。

對話間隔時的快取保留時間，以及摘要（ `/compact` ）的費用，已整理在另一篇文章與指南中（ [快取保留時間](https://renkeimap.jp/guide/learn/agent-cache-ttl) ・ [compact 是什麼](https://renkeimap.jp/guide/learn/compaction) ）。

## 最後

「 [AGENTS.md](http://agents.md/) 」「SKILL」「外部檔案」的取捨，對專案可維護性與指示遵守精度都很重要。另一方面，從 token 計費的角度來看，影響最大的，是不要重貼資料，以及確認是否真的命中快取。請把「共通規則放 [AGENTS.md](http://agents.md/) 、專門流程放 SKILL、長資料放檔案、對話只用帶標題的參照」這個原則，納入日常 AI 代理的運作中。

完整輪次的測量資料與分析，已公開於以下指南：

- [資料與指示的 6 種傳遞方式 — 如何取捨與費用差異](https://renkeimap.jp/guide/learn/agent-input-strategy)
- [資料與指示的 6 種傳遞方式的 token 測量與費用（測量）](https://renkeimap.jp/guide/learn/agent-input-experiment)
- [什麼是提示快取 — 機制・各家條件・控制方法](https://renkeimap.jp/guide/learn/prompt-caching)

各種放置位置的詳細說明與費用確認方式，整理在以下指南：

- [什麼是 AGENTS.md — AI 會最先讀取的指示檔](https://renkeimap.jp/guide/learn/agents-md)
- [什麼是 SKILL — 只在使用時載入的 AI 指示](https://renkeimap.jp/guide/learn/agent-skills)
- [指示要寫在哪裡 — 對話・AGENTS.md・SKILL](https://renkeimap.jp/guide/learn/where-to-put-rules)
- [長篇資料是貼上還是用檔案傳遞 — 已測量 token](https://renkeimap.jp/guide/learn/agent-files)
- [AI 的 context 是什麼 — 每次都會送出的內容與量](https://renkeimap.jp/guide/learn/agent-context)
- [AI 代理 token 很快減少時該確認的 4 件事](https://renkeimap.jp/guide/fix/agent-token-cost)
- [快取保留時間 — 幾分鐘會失效、會變幾倍（測量）](https://renkeimap.jp/guide/learn/agent-cache-ttl)
- [什麼是 compact — 節省量・頻率・自動 compact・缺點](https://renkeimap.jp/guide/learn/compaction)
- [AI 代理 14 種產品怎麼選 — 8 個指標與導入 7 步驟](https://renkeimap.jp/guide/choose/choose-ai-agent)

本次使用的產品與模型頁面： [Claude Code](https://renkeimap.jp/tool/claude-code) ・ [Antigravity](https://renkeimap.jp/tool/antigravity) ・ [Claude](https://renkeimap.jp/tool/claude) ・ [Gemini](https://renkeimap.jp/tool/gemini)

## 相關文章・參考

- [How Claude Code uses prompt caching（Claude Code 官方）](https://code.claude.com/docs/en/prompt-caching)  
	― 快取保留時間（訂閱版 1 小時・API 金鑰 5 分鐘）以及在途中修改 [CLAUDE.md](http://claude.md/) 也不會傳到對話中的出處。
- [Context caching（Gemini API 官方）](https://ai.google.dev/gemini-api/docs/generate-content/caching?hl=en)  
	― Gemini 隱式快取可用的最小量（4,096 token）與不保證會命中快取的出處。

**讀者問卷進行中**  
如果你想更深入了解這個主題，請在文章按「讚」。我們會依按讚數較多的主題優先追加調查，結果會在新文章中通知你（若有追蹤將收到通知）。

**【可轉載】關於本文轉載**

本文的文字與圖表皆可轉載。圖表請 **勿加工** 直接使用。轉載時，請註明出處並連結至本文完整版 [〈資料與指示的 6 種傳遞方式〉](https://renkeimap.jp/guide/learn/agent-input-strategy) （ [renkeimap.jp](http://renkeimap.jp/) ）。不需事前聯絡。

---

原文出處： [https://qiita.com/songchong/items/235bed0f8c72f268477e](https://qiita.com/songchong/items/235bed0f8c72f268477e)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3KK503) [75瀏覽](https://codelove.tw/@tony/post/3KK503)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3KK503)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [REA：讓你的 AI 代理反向工程你希望擁有的應用程式功能](https://codelove.tw/@tony/post/qenz7x)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝7

259

🥈

[boluoji](https://codelove.tw/@boluoji123)

📝3

39

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商事務所：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1793265954&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KK503&amp;pra=5&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguNDgiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguNDgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjQ4Il1dLDBd&amp;dt=1793265953968&amp;bpp=3&amp;bdt=534&amp;idt=27&amp;shv=r20261028&amp;mjsv=m202610260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793177084%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793177084%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793177084%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=3530513410619&amp;rume=1&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95404667%2C95405492%2C95405781%2C95406274%2C122881730%2C95405500%2C31061691%2C31061693&amp;oid=2&amp;pvsid=1271033611352165&amp;tmod=708190788&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1944%2C30%2C1944%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=38" title="Advertisement" aria-label="Advertisement"></iframe>