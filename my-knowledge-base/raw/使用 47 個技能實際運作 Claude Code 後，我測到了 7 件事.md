---
title: "使用 47 個技能實際運作 Claude Code 後，我測到了 7 件事"
source: "https://codelove.tw/@tony/post/3V2Pk3"
author:
published:
created: 2026-09-17
description: "Claude Code 的 skill，**光是加入的話 1 分鐘**就能完成。公開的內容也很多。既然如此，要做的事情就很明確了。**不管三七二十一全都裝進去**。在運用自製 47 個 skill 的同時，量測了約 1 個月後，我發現了**加越多，反而越不會被叫到**。原因不是變重了。**..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月15日](https://codelove.tw/@tony/post/3V2Pk3 "2026-09-15 17:03:04")

## 使用 47 個技能實際運作 Claude Code 後，我測到了 7 件事

Claude Code 的 skill， **光是加入的話 1 分鐘** 就能完成。公開的內容也很多。

職業資源與生涯規劃

既然如此，要做的事情就很明確了。 **不管三七二十一全都裝進去** 。

在運用自製 47 個 skill 的同時，量測了約 1 個月後，我發現了 **加越多，反而越不會被叫到** 。  
原因不是變重了。 **而是已加入 skill 的說明，會默默消失。**

這篇文章是把至今公開的 6 篇驗證文章整合成一條脈絡，  
並把 **本週（2026-09-11）推出的官方評估指令** 加上去，成為第 7 項。

```
1. 增加也不會變重          加 80 個只多 +185 token
2. 超過的部分會消失說明    自己的 2 個 skill 388 字→0、350 字→0
3. 哪些會消失每次都不同    同一環境每次執行都不一樣
4. 有效的 skill 確實有效    輸出 −43%（9 次中 9 次）。但資訊也變少
5. 指示書型與實作型不同    啟動時 token 約差 28 倍
6. 限制模式下全部消失      工具 42→23、skill 47 個→0 個
7. 官方的評估指令登場      但「量」無法測量
```

> 驗證環境：Windows 10 / Claude Code 2.1.247〜2.1.270 / 量測時間 2026-08-19〜09-14

---

## 1\. 增加也不會變重

先從最讓人擔心的地方確認： **增加 skill 會不會壓迫 context。**

職業資源與生涯規劃

在自製 47 個 skill 的環境中，再加 80 個，變成 127 個。

skill 數輸入 token 47 個82,179 **127 個\*\*** 82,364 **\*\*增加了 185 token。** 加 80 個，只多這些。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789610041&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2Pk3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789609652228&amp;bpp=1&amp;bdt=940&amp;idt=1&amp;shv=r20260915&amp;mjsv=m202609150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789609620%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789609620%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789609620%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1592x819%2C952x280&amp;nras=4&amp;correlator=442050386009&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=145&amp;ady=1706&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95401791%2C95403098%2C122880937%2C122881070&amp;oid=2&amp;pvsid=1103035008319093&amp;tmod=792537935&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1592%2C819&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

原因是， **skill 清單有字數預算** 。會在預算範圍內放入各個 skill 的說明文字，  
超過的部分就不放。也因此，即使增加數量，清單大小也幾乎不變。

預算可以透過設定調整。把 `skillListingBudgetFraction` 提高到 100,000 之後，  
**多回來了 1,285 token 的說明。**

### /skill-doctor 的預告會有落差

`/skill-doctor` 會告訴你「有 19 個未使用 skill 每次都會被載入」。  
照它說的刪除後， **少了 3,710 token。** 但 **預告大約是 2,160** 。

**預告和實測差了 1.7 倍。** 可以作為刪除判斷的參考，但 **數字還是自己重新量一次比較安全。**

另外， **刪掉 1 個也沒有讓 prompt cache 失效。** 這部分看來可以不用太在意。

→ 詳細： [在 Claude Code 加 80 個 skill，增加的 token 只有 185](https://qiita.com/suwa_nobu/items/e02595193c2ec2e733a3)

AI工具、聊天機器人與虛擬助理

---

## 2\. 超過的部分，說明會消失

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789610041&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2Pk3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789609652249&amp;bpp=1&amp;bdt=961&amp;idt=2&amp;shv=r20260915&amp;mjsv=m202609150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789609620%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789609620%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789609620%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1592x819%2C952x280%2C952x280&amp;nras=5&amp;correlator=442050386009&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=145&amp;ady=2560&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95401791%2C95403098%2C122880937%2C122881070&amp;oid=2&amp;pvsid=1103035008319093&amp;tmod=792537935&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1592%2C819&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

重點從這裡開始。 **一旦超出預算，會發生什麼事。**

把 GitHub 上正在趨勢中的 37 個 skill 集合（ `mattpocock/skills` ，255,313 stars）加到  
自製 47 個 skill 上。

輸入 token 自製 47 個82,255 **\+ skill 集合 37 個（共 84 個）\*\*** 81,971 **\*\*少了 284 token。** 明明是加上去，結果反而變少了。

追查減少的內容後發現， **原本已加入的自製 skill 中有 2 個的說明，從 388 字→0、350 字→0** 。

說明字數呼叫次數常用的 2 個 **1 個字都沒少** 42 次 / 25 次 **被消掉的 2 個\*\*** 388→0 / 350→0 ****0 次**** 被刪掉的是呼叫次數較少的那些。\*\*

新加進來的 skill 當然是 0 次，所以 **會最先被踢掉** 。  
當你覺得「新裝的 skill 沒有被叫到」時， **有可能不是沒用，而是說明根本沒被載入。**

職業資源與生涯規劃

### 不會出錯誤

最麻煩的就是這裡。 **即使說明消失，名稱一定還在。**

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1789610125&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2Pk3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789609652267&amp;bpp=1&amp;bdt=980&amp;idt=1&amp;shv=r20260915&amp;mjsv=m202609150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789610043%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789610043%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789610043%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1592x819%2C952x280%2C952x280%2C952x280%2C728x90%2C334x250&amp;nras=7&amp;correlator=442050386009&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=145&amp;ady=3521&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=284&amp;eid=95396000%2C95401791%2C95403098%2C122880937%2C122881070&amp;oid=2&amp;psts=AOrYGslLgUpx8DGoeT3PMfefd2dDzagRQqScc-sSM_xZ9wp3Z7474W6baU0CpV3tHZ_vzZdRL8XVV6myPESb-c6Ru4LfGv-T6r0fURs0CrpnlRSBWAM%2CAOrYGslURLMtgN6C-_w03c1YU9tLmTLjhhxFLtFJb9GN4h-GQU-rc5ZDEMEpsv4taqLg9jnqQbKK5vdTVwp2eV-qQTd5chFEXdI0rNWAup0PA5DUoqo%2CAOrYGsmqrAAQl5Uhv7of0ul4l2wvnDxrSOIXLYVpMs49CktYPIICY_nQ9ZSOhfIhAAMYEhyAsJto4APHD8iMlpcVDaFHa4mnC4H6bLjqfj6ug7dcyw&amp;pvsid=1103035008319093&amp;tmod=792537935&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=2008%2C82%2C2008%2C82%2C1920%2C30%2C1832%2C900%2C1592%2C819&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

看 skill 清單時，47 個名稱都會照樣列出來。 **畫面上看起來是正常的。**  
沒有警告，也沒有 log。

**「明明裝了卻沒被用到」有可能不是設定錯，而是預算不夠。**

### 如果預算沒滿，就會正常增加

**我的環境在加入前就已經滿預算了。** 如果本來就比較少，  
加進去的部分理論上會正常增加 token。 **我沒有在預算有餘裕的環境測過，  
所以這點沒有確認。**

→ 詳細： [加入 GitHub 趨勢 skill 集合後，自己原本的 2 個 skill 說明消失了](https://qiita.com/suwa_nobu/items/0424c506333a31da0d04)

---

## 3\. 哪些會消失，每次執行都不同

把第 2 項實驗重複 4 次後，還發現了另一件事。

**即使環境相同、skill 組成相同，被刪掉的對象每次都不一樣。**

```
某一次   decompose 的說明 240 字（完整保留）
另一回   decompose 的說明  58 字（被刪掉）
```

**不能把某個 skill 當成固定安全。**

呼叫次數高的那些 4 次都安然無恙，但 **呼叫次數中等的那些會隨執行次數而擺動。**

**從運用角度可以得出的結論只有一個。**

**真的希望它被叫到的 skill，要先累積被呼叫的實績。** 只放著不管，  
等你再加別的 skill 時，它就會最先被踢掉。

---

## 4\. 有效的 skill 確實有效，但資訊也會減少

前面一直在講「不要亂加」，不過 **有效的東西確實有效。**

我們量了在 Hacker News 獲得 269 分的 `ayghri/i-have-adhd` （30,233 stars、MIT、 `SKILL.md` 只有 1 張）。  
這是一個 **讓 agent 的回答去掉前言，把行動放在開頭** 的 skill。

同樣的 3 個問題，在有 skill 和沒有 skill 的情況下，各自 **重複 3 次** 。

對照組有 skill 差 Q1 git643 字 **348 字\*\*** −46% **Q2 CSV2,163 字** 1,593 字 **\*\*−26%** Q3 Docker2,847 字 **1,256 字\*\*** −56% ****平均**** 1,884 字 ****1,066 字**** −43% **\*\*9 次中 9 次，有 skill 的回答都比較短。** 範圍也沒有重疊。

```
Q1  對照 620, 643, 744    skill 238, 348, 350
Q2  對照 2119, 2163, 2367 skill 1488, 1593, 1715
Q3  對照 2361, 2847, 2860 skill 1178, 1256, 1617
```

### 但資訊也變少了

我數了每個回答中出現多少技術關鍵字。

對照組有 skill 差 Q1 4.3 個 3.0 個 −30% Q2 6.0 個 5.0 個 −17% Q3 9.0 個 8.3 個 **−8%\*\*** 「變短了但資訊一樣」並不是這樣。\*\*

不過 **字數減少得更多** ，所以密度反而提高了。Q3 每 1,000 字中， **3.2 個 → 6.6 個** 。

而且 **內容會被替換掉。** 在 Q1 中，「 `-m` 會移除 trailer」這個注意事項變多了，  
但相對地 `git commit --amend --only` 消失了。 **不是單純被刪掉而已。**

→ 詳細： [裝了 3 萬 stars 的 Claude Code skill 後，輸出短了 43%](https://qiita.com/suwa_nobu/items/cec37ce5a6141bb3eefc)

---

## 5\. 只有指示書的 skill，和帶著實作的 skill，是不同東西

在決定要放什麼時， **依內容型態不同，成本完全不一樣。**

我們把 stars 60,480 的 Agent Skill 裝進來，並和自製的同類 skill 比較。

自製導入的東西實作 `SKILL.md` 1 個 **Python 308 個檔案 / 32MB** 常駐 token—約 103 **啟動時 token\*\*** 3,225 **\*\*約 90,000（約 28 倍）** 標準 Windows 可運作 **不能運作** （必須 Python 3.12+，我的環境是 3.11.9） **約 28 倍的差距** 。而且在我的環境中無法運作。

### 在加入前就能看到成本

```
claude plugin details <外掛名稱>
```

**常駐多少、啟動時多少 token** ，在導入前就會顯示。  
**是不是後者，看這個就知道。**

另外，執行 `uninstall` 後， **32MB 的 cache 還留著。**

### 環境確認的坑

看到「必須 Python 3.12+」後， **我誤以為自己符合需求了。**  
因為我看到的是安裝在別處的 Python。

**確認的不是「有沒有裝」，而是「這個工具看不看得到」。** 這是我自己的失誤。

→ 詳細： [裝了 stars 6 萬的 Agent Skill 後，每次呼叫都需要 9 萬 token](https://qiita.com/suwa_nobu/items/817a26e02cd1f08c7edd)

---

## 6\. 限制模式下，skill 會全部消失

2.1.248 加入了 `--restricted` （ `CLAUDE_CODE_RESTRICTED=1` 也一樣）。

通常 `--restricted` 工具42 **23（減少 45%）\*\*** 自製 skill ****47 個**** 0 個 **消失的不只有 `Bash` / `PowerShell` / `WebFetch` 。  
**官方說明裡沒提到的 `Workflow` / `Monitor` / `LSP` 與 8 個 MCP 工具也都會消失。\*\*

相對地 `Write` / `Edit` / `Agent` / `WebSearch` 會 **保留** 。這不是唯讀模式。

**`~/.claude/` 底下整個都會成為對象。** 應該是因為這是讀不到使用者設定的模式，  
才會這樣，但 **官方並沒有寫明理由。**  
`Skill` 工具本身會保留，但 **沒有地方可以呼叫。**

如果用 `--tools` 把名稱列出來，就會恢復。 **`default` preset 不會恢復** （與官方說明一致）。

**越是堆很多 skill 的人，在這個模式下就越是什麼都不剩。**  
這本來就是這個模式的目的，所以行為是正確的，但 **與其先理解成「更安全」，  
不如先理解成「手上的資產全部消失」** ，比較不容易出事。

→ 詳細： [Claude Code 加入限制模式後，自製 skill 47 個變成 0 個](https://qiita.com/suwa_nobu/items/4183a8e97738ba45f435)

---

## 7\. skill 的評分工具，只看結構

有可以自動評分 skill 好壞的工具。它會用 9 維 rubric 打分。

它測的是 **結構** 。步驟是否清楚、是否具體、是否有失敗時的分支、  
是否有檢查點。

**內容本身作為事實是否正確，並不在任何一個維度裡。**

而且 **越是寫得具體的錯誤，越會在「具體性」這個維度拿到高分。**  
把規格條號寫錯的 skill， **即使錯了還是把分數拉高了。**

### 另外問「有沒有哪裡怪怪的」，而不是只看分數

同一個模型，如果不使用 rubric，而是直接問 **「如果有問題請指出來」** ，它就會注意到矛盾。  
**分數不會顯示出來。**

不過 **這種指摘也不能全信。** 兩個模型都一致指出的項目中，  
**有 1 件其實是錯的。**

→ 詳細： [skill 評分工具並沒有在看內容是否正確](https://qiita.com/suwa_nobu/items/9cf8152a99b1d178a4fd)

---

## 8\. 官方的評估指令登場了，但無法測量「量」

這一項是本週的新消息。 **2.1.269（2026-09-11）加入了 `claude plugin eval` 。**

這是把外掛套到評估案例上並輸出分數的官方指令。 **實驗設計很仔細。**

> 一次非決定性 agent 的執行只能告訴你很少，因此 **每個 case 預設會執行三次。**
> 
> 每個 case 會在有外掛與沒有外掛的情況下各執行三次， **所以一個 case 總共六次執行。**

**1 個 case 6 次執行。** 然後 `WITH` 和 `W/OUT` 的差 **`Δ` 就是外掛的貢獻** 。  
**即使分數高，若 Δ 是 0，就代表那個 skill 沒有發揮作用。**

case 和 grader 會由 `claude plugin eval init` 自動生成。

### grader 有 6 種，全部都是 pass/fail

種類看什麼收費 `regex` 回答或檔案是否符合正規表達式免費 `tool_used` 該工具被呼叫的次數是否在範圍內免費 `tool_order` 兩個工具的呼叫順序免費 `file_exists` 產生的檔案是否存在免費 `llm` 判定模型是否依照基準做出通過/失敗 **收費** `baseline` 與參考逐字稿相比是否同等以上 **收費\*\*** 而且官方文件寫著這句話。\*\*

> There are no custom-code graders.

**不能插入自訂檢查。**

### 所以，這篇文章裡的數字不能用 eval 算出來

回想一下前面出現過的數字。

```
185 token 增加        284 token 減少
388 字 → 0           輸出 1,884 字 → 1,066 字（−43%）
42 個工具 → 23       啟動時 token 3,225 → 90,000
```

**這些全部都是「量」** 。6 種 grader 全都是通過/失敗，不會回傳數量。  
能處理數字的只有 `regex` 的 `match: "count:N"` （ **剛好 N 次** ）與  
`tool_used` 的 `min` / `max` （ **是否在範圍內** ），但都會被壓成通過/失敗。

**`claude plugin eval` 能說的只有「是否判定為短了」而已。**  
**沒辦法得出「短了 43%」。**

彙整方式也不同。官方是 `the case's score is the mean across its runs` （ **平均值** ）。  
我一直都是看 **中位數** 。只要碰到一次離群值，平均就會被拉動。

**我認為正確的做法是分開使用。**

- **要看行為是否穩定可重現** → `claude plugin eval` 。可以上 CI，也能自動化
- **要看到底變了多少** → 手動量。數 `--output-format json` 的 token 數

---

## 實際運用時，結論到底是什麼

把 7 項整理成實務流程，會變成這樣。

**加入前**

1. **用 `claude plugin details` 看成本。** 有些東西啟動時就要 9 萬 token
2. **確認是 1 張指示書，還是帶實作一起帶進來。** 後者會一併承擔依賴、容量與 token 成本
3. **在自己的環境中，確認是否符合需求，並且要從那個工具本身去看。** 不是「有沒有裝」，而是「看不看得到」

**加入後**

1. **比較前後的清單 token 數。** 如果沒有增加，代表 **某些說明已經消失**
2. **真的希望被叫到的 skill，要先做出被呼叫的實績。** 0 次的會先被刪
3. **`/skill-doctor` 的預告有 1.7 倍落差。** 刪除前後要自己量

**評估時**

1. **用 `claude plugin eval` 量行為是否可重現。** 但不會給出量化數字，需要時還是要手動數
2. **除了評分工具的分數外，再問一次「有沒有怪怪的地方」**

---

## 不能說的事

- **全部都是在我自己的單一環境中量測** 。有 47 個 skill、MCP server、Windows 10。  
	**前提比乾淨環境更多** 的狀態下測的
- \*\*預算超出時的行為，是因為「我自己本來就滿預算」\*\*才觀測到。  
	在 token 較少的環境，短時間內不會發生
- 第 4 項的 `i-have-adhd` 是 **3 題 × 3 次** 。換題材後比例會改變
- 第 6 項的限制模式是 **2.1.248 時點** 的量測。之後的版本沒有再確認
- 第 8 項的 `claude plugin eval` 是在 **讀完文件並執行 `--help` 的階段** ，  
	**還沒有真的套到自製 skill 上**
- 量測期間 Claude Code 從 2.1.247 跑到 2.1.270。  
	**各項目的量測時點都寫在連結文章裡**

---

## 總結

- **skill 增加也不會變重。** 加 80 個只多 +185 token。清單有字數預算
- **但說明會默默消失。** 依照呼叫次數少的先被刪， **只留下名稱**
- **不會報錯。** 畫面上看起來正常，實務上很難察覺
- **哪些會消失每次都不同。** 不能固定認定某個 skill 絕對安全
- **有效的 skill 確實有效** （輸出 −43%、9 次中 9 次）。 **但資訊也變少** （−8〜−30%）
- **指示書型與實作型是不同東西。** 啟動時 token 差約 28 倍。可先用 `claude plugin details` 看
- **限制模式下 47 個 skill 會變成 0 個。** 工具也會從 42 變 23
- **評分工具只看結構。** 越具體的錯誤，反而越容易拿高分
- **官方的評估指令出現了。** 1 個 case 6 次執行，Δ 才是重點。 **但不能測量數量**

**skill 是資產，但不是放著就會變成資產。**

**與其增加安裝數，不如把真正有在被呼叫的留下來，效果更好。**

---

**參考**

- [Extend Claude with skills](https://code.claude.com/docs/en/skills) — 清單字數預算與 `/skill-doctor`
- [Settings reference](https://code.claude.com/docs/en/settings-reference) — `skillListingBudgetFraction` / `skillListingMaxDescChars`
- [Test plugins with evals](https://code.claude.com/docs/en/plugin-evals) — `claude plugin eval` 的 grader 與 ablation
- [CLI reference](https://code.claude.com/docs/en/cli-reference) — `--restricted` 與 `--output-format json`

---

JQIT 的工程師有 95% 以上是從零經驗錄用的。  
如果有興趣，也歡迎來公司網站看看。

▶ [公司網站](https://www.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=corporate)

我們也有工程師招募。如果有興趣的話，歡迎看看。

▶ [招募網站](https://recruitment.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=recruit)

---

原文出處： [https://qiita.com/suwa\_nobu/items/08e3bd535dbc79e0c738](https://qiita.com/suwa_nobu/items/08e3bd535dbc79e0c738)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3V2Pk3) [75瀏覽](https://codelove.tw/@tony/post/3V2Pk3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3V2Pk3)

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

📝32 💬1

492

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1789609651&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2Pk3&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuMzciLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuMzciXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjM3Il1dLDBd&amp;dt=1789609651370&amp;bpp=2&amp;bdt=81&amp;idt=130&amp;shv=r20260915&amp;mjsv=m202609150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1789609620%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1789609620%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1789609620%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=442050386009&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1592&amp;bih=819&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95401791%2C95403098%2C122880937%2C122881070&amp;oid=2&amp;pvsid=1103035008319093&amp;tmod=792537935&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1592%2C819&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=211" title="Advertisement" aria-label="Advertisement"></iframe>