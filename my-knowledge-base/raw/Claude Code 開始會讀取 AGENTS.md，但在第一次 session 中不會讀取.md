---
title: "Claude Code 開始會讀取 AGENTS.md，但在第一次 session 中不會讀取"
source: "https://codelove.tw/@tony/post/aARpDq#google_vignette"
author:
published:
created: 2026-09-23
description: "Claude Code 2.1.277（2026-09-18）的更新紀錄寫著：> Added AGENTS.md support: in a project with no CLAUDE.md, Claude Code reads AGENTS.md instead;> change it u..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月20日](https://codelove.tw/@tony/post/aARpDq "2026-09-20 17:08:39")

## Claude Code 開始會讀取 AGENTS.md，但在第一次 session 中不會讀取

Claude Code 2.1.277（2026-09-18）的更新紀錄寫著：

探索AI 工具

> Added [AGENTS.md](http://agents.md/) support: in a project with no [CLAUDE.md](http://claude.md/), Claude Code reads [AGENTS.md](http://agents.md/) instead;  
> change it under "Project instructions" in `/config` (not yet on Bedrock, Vertex or Foundry)

Hacker News 上有 **501pt** （截至 2026-09-19）。 **搜尋出來的日文解說寫著「不會讀」** 。  
在 2.1.277 之前，這樣的說法是正確的。

我放入關鍵字，重新測了 6 種組合與剛建立設定後的狀況。

```
只放 AGENTS.md              有讀
也放 CLAUDE.md              AGENTS.md 不讀
放 1 個 CLAUDE.local.md      AGENTS.md 不再讀
有 ~/.claude/CLAUDE.md       AGENTS.md 仍會讀（不算在內）

而且，第一次 session 不會讀
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790125241&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaARpDq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuNTMiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuNTMiXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjUzIl1dLDBd&amp;dt=1790125241658&amp;bpp=3&amp;bdt=1419&amp;idt=3&amp;shv=r20260922&amp;mjsv=m202609170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790125180%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790125180%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790125180%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C1680x969&amp;nras=3&amp;correlator=6467910807078&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=1148&amp;biw=1680&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95403099&amp;oid=2&amp;pvsid=3006384898065259&amp;tmod=339021061&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C1050%2C1680%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=3&amp;uci=a!3&amp;btvi=1&amp;fsb=1&amp;dtd=129" title="Advertisement" aria-label="Advertisement"></iframe>

> 驗證環境：Windows 10 / 隔離安裝 Claude Code 2.1.277 / `claude -p` / 測試時間：2026-09-19

---

## 測法：在檔案中放關鍵字

在各檔案中放入各自獨一無二的字串，然後請它 **「列出指示中所有的關鍵字」** 。

```
AGENTS.md         The AGENTS passphrase is AGENTS-ORANGE-7731.
CLAUDE.md         The CLAUDE passphrase is CLAUDE-VIOLET-4420.
```

**我把所有會讀檔的工具都禁止了。** 不然即使指示裡沒寫，它也能自己把檔案打開。

```
--disallowedTools "Read,Write,Edit,NotebookEdit,Bash,PowerShell,Glob,Grep,Task,WebFetch,WebSearch,Skill,ToolSearch"
--strict-mcp-config --mcp-config <空的MCP設定>
```

我也在 `~/.claude/CLAUDE.md` 放了關鍵字，並在 **空白設定目錄** 中反覆測試。

探索AI 工具

---

## 結果：6 種組合都符合官方表格

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790125241&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaARpDq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuNTMiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuNTMiXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjUzIl1dLDBd&amp;dt=1790125241667&amp;bpp=1&amp;bdt=1428&amp;idt=1&amp;shv=r20260922&amp;mjsv=m202609170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790125180%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790125180%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790125180%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C1680x969%2C952x280&amp;nras=4&amp;correlator=6467910807078&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=2043&amp;biw=1680&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95403099&amp;oid=2&amp;pvsid=3006384898065259&amp;tmod=339021061&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C1050%2C1680%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=4&amp;uci=a!4&amp;btvi=2&amp;fsb=1&amp;dtd=124" title="Advertisement" aria-label="Advertisement"></iframe>

各測了 2～3 次。回傳的關鍵字如下。

| 放置的檔案 | 讀到的內容 |
| --- | --- |
| 只有 `AGENTS.md` | **AGENTS** + 使用者層級 |
| `AGENTS.md` + `CLAUDE.md` | **CLAUDE** + 使用者層級（ **AGENTS 沒有** ） |
| `AGENTS.md` + `CLAUDE.local.md` | **[CLAUDE.local](http://claude.local/)** + 使用者層級（ **AGENTS 沒有** ） |
| `AGENTS.md` + `.claude/rules/` | **AGENTS** + rules + 使用者層級 |
| `AGENTS.md` + `AGENTS.local.md` | **AGENTS** + 使用者層級（ **[AGENTS.local](http://agents.local/) 沒有** ） |
| 上方設定 `claude-md-and-agents-md` | **AGENTS + CLAUDE** + 使用者層級 |

**`~/.claude/CLAUDE.md` 不論任何組合都會一起讀到，而且即使有它， `AGENTS.md` 仍然會被讀取。**

---

## 第一次 session 不會讀到

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790125241&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaARpDq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuNTMiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuNTMiXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjUzIl1dLDBd&amp;dt=1790125241678&amp;bpp=1&amp;bdt=1439&amp;idt=1&amp;shv=r20260922&amp;mjsv=m202609170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790125180%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790125180%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790125180%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C1680x969%2C952x280%2C952x280&amp;nras=5&amp;correlator=6467910807078&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=2921&amp;biw=1680&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95403099&amp;oid=2&amp;pvsid=3006384898065259&amp;tmod=339021061&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C1050%2C1680%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=5&amp;uci=a!5&amp;btvi=3&amp;fsb=1&amp;dtd=117" title="Advertisement" aria-label="Advertisement"></iframe>

**這裡最容易踩坑。**  
在新的設定目錄只放 `AGENTS.md` ，接著連續測 4 次。

探索AI 工具

```
Session 1   沒有讀到
Session 2   讀到 AGENTS
Session 3   讀到 AGENTS
Session 4   讀到 AGENTS
```

**官方也有明確寫出來。**

> It's your first session after you install or upgrade to a version with `AGENTS.md` support.  
> Claude reads `AGENTS.md` from your next session on

**剛升級完就先測一次、然後判斷「沒效」的話，會判錯。**

---

## CLAUDE.local.md 加一個，就會讓團隊的 AGENTS.md 消失

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790125241&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaARpDq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuNTMiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuNTMiXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjUzIl1dLDBd&amp;dt=1790125241683&amp;bpp=1&amp;bdt=1444&amp;idt=1&amp;shv=r20260922&amp;mjsv=m202609170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790125180%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790125180%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790125180%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C1680x969%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=6467910807078&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=3735&amp;biw=1680&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95403099&amp;oid=2&amp;pvsid=3006384898065259&amp;tmod=339021061&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C1050%2C1680%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=6&amp;uci=a!6&amp;btvi=4&amp;fsb=1&amp;dtd=115" title="Advertisement" aria-label="Advertisement"></iframe>

很多人會用 `CLAUDE.local.md` 放個人備忘。  
**一旦放了， `AGENTS.md` 就不會再被讀取。** 官方也有提醒。

> Because `CLAUDE.local.md` counts, adding one to keep your own uncommitted instructions in a  
> project that relies on `AGENTS.md` stops Claude from reading `AGENTS.md` for you.

**`claude -p` 的紀錄裡沒有警告也沒有錯誤。** 如果要讓兩者都被讀到，必須改設定。

```
{
  "pluginConfigs": {
    "agents-md@builtin": {
      "options": { "instructionFiles": "claude-md-and-agents-md" }
    }
  }
}
```

**這段要寫在 `~/.claude/settings.json` 。** 官方說明是， **專案或本機設定都會被忽略。**

---

## 沒能確認的事情

- 只測了 **Claude Code 2.1.277** 。尚未與 2.1.276 以前版本比較
- 只做了 **`claude -p` 的一次往返。** 尚未確認互動式 session 的顯示
- 尚未測 **子目錄中的 `AGENTS.md` 、 `@path` 匯入、傳播到子代理**
- 「第一次不會讀」是 **在新的設定目錄中測得** 。是否與官方所說的「安裝後第一次」是同一機制， **尚未確認**
- 官方寫明 **停用遙測、Bedrock / Vertex / Foundry 不可用** 。 **尚未驗證**
- 官方也寫明 **`AGENTS.md` 不會出現在 `/memory` 或 `/context`** 。確認只能靠關鍵字
- 每個條件只測了 2～3 次。 **次數不多**
- 測試中曾因 **自己的腳本 bug 導致某個條件出現假結果** ，後來重做並重新測試

---

## 總結

- **從 Claude Code 2.1.277 開始，如果沒有 `CLAUDE.md` ，就會讀 `AGENTS.md`**
- **第一次 session 不會讀。** 從第二次開始才會讀
- **`~/.claude/CLAUDE.md` 和 `.claude/rules/` 不算在「有 [CLAUDE.md](http://claude.md/) 」的判定內**
- **放一個 `CLAUDE.local.md` ， `AGENTS.md` 就會默默不被讀取**
- **`AGENTS.local.md` 不會被讀取**
- 如果要兩者都讀，使用 `claude-md-and-agents-md` 。 **寫在 `~/.claude/settings.json`**

**既有的日文文章不是寫錯，而是前提改變了。**  
**自己的環境到底是哪一種，只要放一個關鍵字就能知道。**

---

**參考**

- [Claude Code changelog](https://code.claude.com/docs/en/changelog) — 2.1.277 新增 [AGENTS.md](http://agents.md/)
- [How Claude remembers your project](https://code.claude.com/docs/en/memory) — 判定表與例外條件

## 相關文章

- [Claude Code 的子代理也會拿到全部 CLAUDE.md。現在可以設定排除](https://qiita.com/suwa_nobu/items/b465ef863f8d8608f497) — 測試 [CLAUDE.md](http://claude.md/) 會傳到哪裡的一篇
- [CLAUDE.md 是 21 個區段，還是 8 行 — 用官方文件做出結論的一篇](https://qiita.com/suwa_nobu/items/cea574550613de33a114) — 依官方文件決定 [CLAUDE.md](http://claude.md/) 的份量的一篇

---

JQIT 的工程師有 95% 以上都是從無經驗錄取。  
如果有興趣，也歡迎到公司網站逛逛。

▶ [公司網站](https://www.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=corporate)

我們也在招募工程師。有興趣的話，歡迎看看。

▶ [招募網站](https://recruitment.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=recruit)

---

原文出處： [https://qiita.com/suwa\_nobu/items/c795cf89d0fd4091c9cb](https://qiita.com/suwa_nobu/items/c795cf89d0fd4091c9cb)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aARpDq) [66瀏覽](https://codelove.tw/@tony/post/aARpDq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aARpDq)

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

855

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1790125240&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaARpDq&amp;pra=5&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1My4wLjgwMTAuNTMiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1My4wLjgwMTAuNTMiXSxbIk5vdF9BIEJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTMuMC44MDEwLjUzIl1dLDBd&amp;dt=1790125240334&amp;bpp=2&amp;bdt=95&amp;idt=28&amp;shv=r20260922&amp;mjsv=m202609170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790125180%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790125180%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790125180%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=6467910807078&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95403099&amp;oid=2&amp;pvsid=3006384898065259&amp;tmod=339021061&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C1050%2C1680%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=38" title="Advertisement" aria-label="Advertisement"></iframe>