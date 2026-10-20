---
title: "Claude Code 的新稽核指令，預設情況下不會看到我的 49 個技能"
source: "https://codelove.tw/@tony/post/xN6DGx"
author:
published:
created: 2026-09-29
description: "Claude Code 2.1.283（2026-09-25）的 changelog 裡這樣寫著：> Added `/doctor prompt-audit` (also `/checkup prompt-audit`) to audit your CLAUDE.md files,> skil..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月28日](https://codelove.tw/@tony/post/xN6DGx "2026-09-28 17:02:16")

## Claude Code 的新稽核指令，預設情況下不會看到我的 49 個技能

Claude Code 2.1.283（2026-09-25）的 changelog 裡這樣寫著：

> Added `/doctor prompt-audit` (also `/checkup prompt-audit`) to audit your [CLAUDE.md](http://claude.md/) files,  
> skills, agents and commands for prompting patterns written for older models

**可以理解成它會指出我的 [CLAUDE.md](http://claude.md/) 和技能是不是用了舊式寫法。**  
因為我有 49 個技能、286 個子代理，所以立刻拿來試了。

```
第一次　在專案內執行　21.7 分鐘 / $10.69 / 66 筆
      → ~/.claude 沒有取得權限，技能和子代理也都不在範圍內

第二次　傳入 --add-dir　12.7 分鐘 / $9.62 / 12 筆
      → 辨識出 45 個技能、286 個子代理

合計約 34 分鐘、$20.31。出現的並不是「舊模型用的寫法」
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790671147&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxN6DGx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669063476&amp;bpp=2&amp;bdt=1289&amp;idt=2&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790650089%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873&amp;nras=3&amp;correlator=1131722881137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=1083&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95403098%2C95403356%2C42533294%2C122881195%2C95340253%2C95340255&amp;oid=2&amp;pvsid=4199539382628444&amp;tmod=933744953&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

> 驗證環境：Windows 10 / 隔離安裝 Claude Code 2.1.283 / `claude -p` / 測量日期 2026-09-28

---

## 第一次：一個技能都沒看到

我在實際專案（ `CLAUDE.md` 167 行）中執行了 `/doctor prompt-audit` 。

**大約花了 22 分鐘，回來 66 條指摘。** 下面是指摘表格中的一行  
（擷取了對應位置與根據欄位）。

> `docs/git-branch-strategy.md` に従い、 `feater/LABEL-XXX_作業内容`  
> ／ 參照先は 2025-09-09（4da04ff）に削除、この行は 2026-06-22 記述

**問題出在範圍。** 報告開頭這樣寫著：

> **未監査のもの。** Next, the permission prompt couldn't be approved in a non-interactive session, so even the listings couldn't be read:  
> `~/.claude/CLAUDE.md` 、 `~/.claude/{rules,skills,commands,agents,output-styles}` 、 `~/.claude/plugins`

記錄裡有 22 個錯誤。 **其中 15 個是權限被拒，5 個是嘗試讀取 `~/.claude` 時被擋下來。**

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790671147&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxN6DGx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669063481&amp;bpp=1&amp;bdt=1293&amp;idt=1&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790650089%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873%2C952x280&amp;nras=4&amp;correlator=1131722881137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=1976&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95403098%2C95403356%2C42533294%2C122881195%2C95340253%2C95340255&amp;oid=2&amp;pvsid=4199539382628444&amp;tmod=933744953&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

```
Claude requested permissions to read from C:\Users\...\.claude,
but you haven't granted it yet.
```

**49 個技能和 286 個子代理，一個都沒看。**  
實際上只審查了 **專案內的 18 個檔案** 。

**如果用 `-p` 或在 CI 裡跑，這裡會默默漏掉一半。**

---

## 第二次：加上 --add-dir 就會去看

加上 `--add-dir ~/.claude` ，在幾乎空白的專案裡重新跑了一次。

1 次（只含專案）2 次（含 `--add-dir` ）所需時間（API 時間） **21.7 分鐘** 12.7 分鐘成本 **$10.69** $9.62 輸出 token105,58953,868 工具呼叫75 次56 次審查的對象專案 18 個檔案 **45 個技能 / 286 個子代理** 指摘 66 筆 **12 筆\*\*** 這次有看到了。 **不過** 讀法還是掃過而已\*\*。針對 `~/.claude` 的 49 次操作內訳如下。

工具次數做了什麼Grep19 次批次掃描Glob16 次取得清單Bash6 次一起 grep **Read\*\*** 8 ****個別開啟**** 完整讀過的，只有 286 個子代理中的 2 個，以及 3 個技能檔案\*\*（ `systematic-debugging` 與  
`review-python-backend` 這 2 個技能）。另外還有 2 個只讀了一部分。  
報告本身也這樣說：

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790671147&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxN6DGx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669063486&amp;bpp=1&amp;bdt=1299&amp;idt=1&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790650089%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873%2C952x280%2C952x280&amp;nras=5&amp;correlator=1131722881137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=2936&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95403098%2C95403356%2C42533294%2C122881195%2C95340253%2C95340255&amp;oid=2&amp;pvsid=4199539382628444&amp;tmod=933744953&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

> 沒有逐一完整閱讀 45 個技能（中訊號區間以 grep 為主）

---

## 出現的不是「舊模型用的寫法」

changelog 寫的是「prompting patterns written for older models」。  
從各分類的件數來看，那部分其實很少。

分類1 次目2 次目舊模型用的語句14 **2\*\*** 設定陳舊／矛盾 ****49**** 8\*\*工具說明00結構322 次目的報告裡明確寫著：

> 壓力語密度很低， `budget_tokens` 、prefill、以及「think step by step」等 API 化石是 **零**

**反而出現的是環境裡的化石。**

- **有 5 處是別的使用者的 macOS 絕對路徑** （ `/Users/takahiromiyamoto/.claude/skills/...`）
- \* *有 4 個技能在引用不存在的外掛名稱 \`document-skills:*\`\*\*
- \* *有技能在引用未安裝的 \`superpowers:*\`\*\*

第一個問題是我自己也知道的缺陷，這次是 **獨立找出來的。**

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790671149&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxN6DGx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669063490&amp;bpp=1&amp;bdt=1303&amp;idt=1&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790671147%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790671147%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790671147%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873%2C952x280%2C952x280%2C952x280%2C350x280%2C334x250&amp;nras=7&amp;correlator=1131722881137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=3793&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=333&amp;eid=95396000%2C95403098%2C95403356%2C42533294%2C122881195%2C95340253%2C95340255&amp;oid=2&amp;psts=AOrYGskhKraEBp41tLI0jictuTYnqHmdr2g5Dm47XGLUouEhd_ZA-hYKlI2CmxocwKVDJ_H3kkZrpJD4pNqqxVjF_0XuXNb3WSFfPBshNQoImP74jA%2CAOrYGsn6TYByeJhBc-OHHC3Qn8RZwkh-a-UJo_alHTWAUtbmb7sVSODtgitFxtvnP6F3XMDo0zAlxXPWW4hhZ9c94glIxkugw6t7YFCHnwhBkQc%2CAOrYGske9nAJMbkhpZBU9cRBn2rPR78J7QkvpD7pMBAHXYKjiawB-rFkKDgqfFHB5DhY42H9Ojc-WlLLqs6GhGDW0nUTf7PgVSPFYrti0scWt5Q&amp;pvsid=4199539382628444&amp;tmod=933744953&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1933%2C30%2C1933%2C30%2C1920%2C30%2C1920%2C954%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=5&amp;uci=a!5&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

在第一次結果裡，還有更有感的指摘：

> **6 個專案用代理中，有 5 個在本次工作階段無法使用。**  
> 清單上只有 `playwright-tester` ，而 `code-reviewer` 則被解析到使用者側的另一個代理  
> （TDD Gate 3 用）

---

## 說不出來的事

- **只測了 2026-09-28 這一天，而且每種情境各 1 次。** 結果可能每次都不一樣
- 我只數了 **指摘件數與分類。** 沒有逐條驗證每個指摘是否正確
- **評分標準沒有公開。** 也不清楚它是依據什麼判定為「舊」
- 我的環境有 49 個技能、286 個子代理。 **前提比一般素環境更多**
- **技能數量對不起來。** 目錄是 49 個， `SKILL.md` 有 48 個，報告顯示 45 個。 **3 個差異的原因我沒有確認**
- **尚未在對話式工作階段驗證。** 還沒確認在對話中核准權限時會怎樣
- 成本是 `--output-format` 回傳的 `total_cost_usd` 。 **實際請款會依方案不同而有差異**

---

## 總結

- **`/doctor prompt-audit` 預設不會看使用者層級。** 在 `-p` 下權限不會下來，  
	**49 個技能與 286 個子代理都不在範圍內**
- **傳入 `--add-dir ~/.claude` 就會去看。** 但實際完整打開的只有 **2 個子代理、2 個技能**
- **很貴。** 1 次 $10.69 和 $9.62，加起來 **$20.31 / 約 34 分鐘**
- **幾乎不會出現「舊模型用的寫法」。** API 化石是零，  
	出現的是 **已刪除檔案、別台機器的絕對路徑、以及不存在的外掛名稱**
- **指摘品質很高。** 甚至能對上 git 歷史，寫出「什麼時候被刪掉」都查得到

**雖然名字叫 prompt-audit，但實際上有效的是「設定有沒有和現況脫節」的檢查。**  
設定用得越久，越容易被抓出來。 **要跑的話別忘了加上 `--add-dir` 。**

---

**參考**

- [Claude Code changelog](https://code.claude.com/docs/en/changelog) — 2.1.283 新增 `/doctor prompt-audit`

## 相關文章

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=4060917935&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790671162&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxN6DGx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669063500&amp;bpp=1&amp;bdt=1313&amp;idt=0&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790671147%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790671147%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790671147%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873%2C952x280%2C952x280%2C952x280%2C350x280%2C334x250%2C952x280%2C952x280&amp;nras=9&amp;correlator=1131722881137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=5301&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=1871&amp;eid=95396000%2C95403098%2C95403356%2C42533294%2C122881195%2C95340253%2C95340255&amp;oid=2&amp;psts=AOrYGskhKraEBp41tLI0jictuTYnqHmdr2g5Dm47XGLUouEhd_ZA-hYKlI2CmxocwKVDJ_H3kkZrpJD4pNqqxVjF_0XuXNb3WSFfPBshNQoImP74jA%2CAOrYGsn6TYByeJhBc-OHHC3Qn8RZwkh-a-UJo_alHTWAUtbmb7sVSODtgitFxtvnP6F3XMDo0zAlxXPWW4hhZ9c94glIxkugw6t7YFCHnwhBkQc%2CAOrYGske9nAJMbkhpZBU9cRBn2rPR78J7QkvpD7pMBAHXYKjiawB-rFkKDgqfFHB5DhY42H9Ojc-WlLLqs6GhGDW0nUTf7PgVSPFYrti0scWt5Q%2CAOrYGsnpSkyquLFi_JLJK953gVhXmcqIsZWUPrF9GsFnokC5xI_ihT6etf6yRYnk87BSws5Xvp5ggDWiVudWpgx72hxg7nNkkdsrz3AjcBfl2WFQ&amp;pvsid=4199539382628444&amp;tmod=933744953&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1933%2C30%2C1933%2C30%2C1920%2C30%2C1920%2C954%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

- [Claude Code 的技能以 47 個運用並測試後得到的 7 件事](https://qiita.com/suwa_nobu/items/08e3bd535dbc79e0c738) — 測試增加技能後會發生什麼事
- [Claude Code 開始讀取 AGENTS.md 了。不過第一個工作階段不會讀到](https://qiita.com/suwa_nobu/items/c795cf89d0fd4091c9cb) — 測試指示檔如何被讀取的回合

---

JQIT 的工程師有 95% 以上是從零經驗招募進來的。  
如果可以的話，也歡迎到企業網站逛逛。

▶ [企業網站](https://www.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=corporate)

我們也在招募工程師。如果有興趣，歡迎看看。

▶ [招募網站](https://recruitment.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=recruit)

---

原文出處： [https://qiita.com/suwa\_nobu/items/ae5a8c1609c794a74a25](https://qiita.com/suwa_nobu/items/ae5a8c1609c794a74a25)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xN6DGx) [130瀏覽](https://codelove.tw/@tony/post/xN6DGx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xN6DGx)

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

🔥 [Codex 怎麼買、怎麼充值？先把這兩套計費搞清楚](https://codelove.tw/@tony/post/q44PLq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝59 💬2

850

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="350" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=1414751727&amp;adf=2365317702&amp;pi=t.aa~a.4029027559~rp.3&amp;abgtt=6&amp;w=350&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790671147&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=350x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxN6DGx&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669063521&amp;bpp=1&amp;bdt=1334&amp;idt=0&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790650089%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=1131722881137&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=1190&amp;ady=961&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95403098%2C95403356%2C42533294%2C122881195%2C95340253%2C95340255&amp;oid=2&amp;pvsid=4199539382628444&amp;tmod=933744953&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=8&amp;uci=a!8&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1790669062&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxN6DGx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669062318&amp;bpp=8&amp;bdt=147&amp;idt=90&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790650089%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=1131722881137&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95403098%2C95403356%2C42533294%2C122881195%2C95340253%2C95340255&amp;oid=2&amp;pvsid=4199539382628444&amp;tmod=933744953&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C873&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=141" title="Advertisement" aria-label="Advertisement"></iframe>