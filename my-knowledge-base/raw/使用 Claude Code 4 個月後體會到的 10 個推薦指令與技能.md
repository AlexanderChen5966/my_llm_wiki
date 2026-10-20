---
title: "使用 Claude Code 4 個月後體會到的 10 個推薦指令與技能"
source: "https://codelove.tw/@tony/post/37Z0jx#google_vignette"
author:
published:
created: 2026-06-30
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言=============================================以前我寫過這樣一篇文章。從那之後實際用了超過 4 個月，終於有了「開始用得順手了」的實感。使用下來我覺得，寫程..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月29日](https://codelove.tw/@tony/post/37Z0jx "2026-06-29 17:12:14")

## 使用 Claude Code 4 個月後體會到的 10 個推薦指令與技能

## 前言

以前我寫過這樣一篇文章。

從那之後實際用了超過 4 個月，終於有了「開始用得順手了」的實感。

使用下來我覺得，寫程式、呼叫工具或 MCP、把各種工作交給 Claude 去做固然很方便，但 **Claude Code 本身的操作與控制，還是得自己來** 。像是切換 session、在方向走偏時倒回去、調整設定與技能……你有沒有掌握這些，體驗會差很多。

這篇文章要介紹我日常在用的 10 個指令與技能。

## Session 管理

在 Claude Code 裡，一整段對話歷史會被稱為一個「session」。實際使用時通常會依照任務切換 session，所以這一類指令幾乎是必備。

## /clear

結束目前的 session，並以空白 context 開始新的 session。

重點是， **先前的 session 不會消失，隨時都能從 `/resume` 回去** 。所以可以放心地 `/clear` 。如果加上名稱，例如 `/clear 認證 bug 調查` ，剛結束的 session 就會被加上標籤，之後在 `/resume` 清單裡更容易找到。

也有在同一個 session 裡節省 context 的 `/compact` ，但我個人不太常用。因為 compact 若沒處理好，文脈可能會部分缺失，後續回應也比較容易飄掉；比起來， **產出交接提示詞再轉到新的 session** ，目前感覺更穩定。

## /resume

從過去的 session 清單中選擇並重新開始。

常用在這些情境：

- 想接著昨天的進度繼續做
- 已經 `/clear` 了，但發現還是需要剛剛那段文脈
- 更新設定後需要重啟 Claude Code，重啟後想回到剛才的工作

這類情況很常用。當需要重啟時，結束時通常會顯示一個用 `claude --resume <session-id>` 重新開始的指令，可以直接用它回去。和 `/clear` 搭配記住會很方便。

## /rewind

把對話和程式碼倒回到過去的狀態。每次送出 prompt 時都會自動建立 checkpoint，從選單中挑選想倒回的 prompt 後，可以從以下 5 種操作中選擇：

- **對話與程式碼都倒回去**
- **只倒回對話** （程式碼維持現狀）
- **只倒回程式碼** （對話維持現狀）
- **將之後的內容摘要化** ：把所選位置之後的對話改寫成摘要。當討論已經偏題，但又想保留前段文脈時很好用
- **將之前的內容摘要化** ：把所選位置之前的對話改寫成摘要。當前面鋪陳很長，但想保留最近互動時很好用

我個人最常用的是 **對話與程式碼都倒回去** ，或是 **只倒回對話** 。像是「指示方式不好，導致往奇怪方向跑掉」這種情況，只把對話倒回去再重寫 prompt，就很方便。

在輸入欄空白時按兩次 `Esc` 也能開啟。

兩個摘要類操作可以視為 `/compact` 的局部版。前面我說「 `/compact` 不太常用」，但這兩個可以自己選擇要壓縮哪一段，所以比較不容易出事，也會有派上用場的時候。

注意，像 `rm` 、 `mv` 這類 bash 指令造成的變更不會被追蹤。能回復的只有 Claude 的編輯工具所做的檔案修改。

## Claude Code 的客製化

接下來介紹的是，為了符合自己的使用方式而逐步養成 Claude Code 的指令與技能。就我個人而言，這一類是最推薦的。

## /insights

會將自己的 Claude Code session 分析後，產生一份 HTML 報告（儲存在 `~/.claude/usage-data/report-YYYY-MM-DD-HHMMSS.html` ，並以瀏覽器開啟）。

內容看起來會依產生報告時的版本而有所不同；我實際試用時，大致包含以下幾種資訊：

- 期間內的使用狀況彙整（訊息數、編輯行數、常碰的專案區域等）
- 表現順暢的使用方式
- **容易卡住的地方（friction points）** 。會區分是 Claude 端的問題（例如憑推測就斷言、無視 [CLAUDE.md](http://claude.md/) 規則等），還是使用者端的問題（例如一開始沒有提供前提）
- 尚未使用過的功能與模式建議
- 當模型能力再提升後，可能可行的自動化工作流程建議

建議定期根據這份報告來改善 [CLAUDE.md](http://claude.md/) 或技能，持續循環優化。

不過， **這份報告固定是英文** （大概不會本地化成日文以外的語言）。如果覺得難讀，可以請 Claude Code 幫忙：「讀取 `~/.claude/usage-data/report-...html` ，用中文只說重點。」它會透過 Read 讀取 HTML，並幫你摘要成中文。也可以直接從報告延伸對話，例如「那我們把這條規則加進 [CLAUDE.md](http://claude.md/) 吧」，這樣反而更容易把改善循環做起來。

## /update-config

讓 Claude Code 自己修改 settings.json（permissions、環境變數、hooks 等）的技能。

Claude Code 的設定因為要區分 scope（user / project / local），再加上 hooks 的格式等等，說實話還算複雜。與其自己手動寫，不如直接用日文或中文下指令，例如「讓 `gh pr view` 可以不經確認直接執行」、「在回應結束時加上桌面通知 hook」，會更快也更準。 **Claude Code 的設定，就交給 Claude Code 自己處理。**

不過， `permissions.allow` 的內容有時候 Claude 會「順手擴大」掉（例如你要求 `Bash(gh pr view *)` ，它卻提議成 `Bash(gh *)` ）。套用之前的 diff 一定要 **自己讀過並確認** ，避免混入過度寬鬆的 allow。

有時候你提出設定變更需求後，系統會自動啟動這個流程；但也有可能沒有啟動，而是 Claude 直接去改 settings.json。若你想確保一定走 skill 流程，直接明確輸入 `/update-config` 會比較安全。

## /skill-creator

這是 Anthropic 官方提供、支援建立與改善技能的外掛。預設沒有安裝，需要先從 `/plugin` 加入官方 marketplace 再安裝。安裝方式這篇文章有更詳細說明。

它不只可以從零開始建立技能，也能拿來改善既有技能，或評估技能是否會如預期觸發。比起自己隨手寫 `SKILL.md` ，它能幫你整理成更符合最佳實踐的形式，這點很不錯。

特別有效的是 **description 的調校** 。description 是 Claude 判斷「現在該不該使用這個技能」的唯一依據；如果這裡寫得太模糊，就不會 auto-trigger。把既有 skill 丟給 `/skill-creator` ，並要求「讓它更容易觸發」，它通常會幫你把可能的觸發日文／中文片語整理進 description 裡。

## /fewer-permission-prompts

這個技能會掃描過去的 session 日誌，找出常執行的 **唯讀** Bash 與 MCP 工具呼叫，並把它們加到專案的 `.claude/settings.json` 當作允許清單。

它剛好解決了「全部放行很可怕，但每次都按 Enter 又很煩」的痛點。

它有幾個重點：

- **只限唯讀** ：只有像 `gh pr view` 、 `gh issue list` 、 `aws ... describe-*` 這類沒有副作用的指令才會被列入候選。像 `rm` 、 `gh secret set` 這種寫入型指令會被排除，所以即使直接同意也比較不容易出事
- **依頻率排序建議** ：會附上呼叫次數並排序，方便你從「每天都會用的」開始放行
- **可以逐一採用** ：不是一次全部套用，而是每個候選項都能單獨選 OK/NG，因此比較不容易出現前面 `/update-config` 提到的「allow 太寬」問題
- **預設是專案層級，指定也可用 user scope** ：如果不特別說，會寫進專案的 `.claude/settings.json` 。若你想跨 repository 共用放行規則，跟它說要寫到 user scope，它就會改寫到 `~/.claude/settings.json`

把 `/insights` 拿來回顧 session，接著用 `/fewer-permission-prompts` 整理權限，再用 `/update-config` 佈好 hook，這樣一組搭配下來，Claude Code 會越用越像為你量身打造。

## 其他

## /copy

把最近一則回應複製到剪貼簿。

在終端機裡用滑鼠拖曳選取複製時，常常會連同行首縮排（2 個空白）一起帶進去，雖然不嚴重但很惱人； `/copy` 就沒有這個問題。

如果回應裡包含程式碼區塊，還會出現一個 **可選擇整段或特定程式碼區塊的挑選器** ，這點也很方便。像 `/copy 2` 這樣傳入數字，就能複製倒數第 2 則回應。

另外，如果你啟用了 `/tui fullscreen` 的 fullscreen rendering 模式（依版本不同，更新後第一次啟動時可能會跳出對話框詢問；如果當時選了 OK，通常就已經啟用了），那麼用滑鼠點選拖曳的範圍，放開滑鼠瞬間就會自動進入剪貼簿。因為這時使用的是 Claude Code 內部的選取，而不是終端機自己的選取，所以不會把行首縮排一起帶進去。若只想複製回應的一部分，這樣會更快，所以我會視情況和 `/copy` 交替使用。

## /loop 與 /schedule

這兩個其實不是指令，而是技能（只是可以用 `/` 呼叫，所以使用感很像）。

`/loop` 會在保持 session 開啟的狀態下，重複執行同一個 prompt。

```
/loop 5m CI 跑過了再告訴我
```

如果省略間隔，Claude 會根據情況自行決定執行節奏。

`/schedule` 則是可互動建立會在 Anthropic 雲端定期執行的任務（Routine）的技能。和需要開著本地 session 的 `/loop` 不同， **就算把電腦關掉也能運作** ，因此很適合每天早上的資訊蒐集或定期檢查。

最近的 Claude Code 已經能把指令當成背景任務執行並等待完成，所以像 CI 監控這類用途， `/loop` 的出場機會稍微少了一些；但如果是想定期去查看外部狀態，它還是很實用。

## 結語

把這兩個月來用得最順手的指令整理起來後，發現重點幾乎都集中在 session 切換和 Claude Code 本身的客製化。實際工作的部分——例如寫程式、調查、發 PR——其實多半只是透過 prompt 交給 Claude，自己真正去敲指令的機會不多。感覺上，現在已經變成「程式由 Claude Code 寫，而把 Claude Code 調整好則是人類的工作」這樣的分工了。

之後如果又累積到新的心得，我再寫成文章。

## 附記：本次介紹的指令分類

雖然都可以從 `/` 呼叫、使用感相同，但內部其實分成內建指令、內建技能與外掛。

分類指令內建指令 `/clear` `/resume` `/rewind` `/copy` `/insights` 內建技能 `/loop` `/schedule` `/update-config` `/fewer-permission-prompts` 外掛（需安裝） `/skill-creator`

---

原文出處： [https://qiita.com/wataru86/items/b859f1578191a1e15808](https://qiita.com/wataru86/items/b859f1578191a1e15808)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/37Z0jx) [12瀏覽](https://codelove.tw/@tony/post/37Z0jx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/37Z0jx)

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

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝13 💬2 ❤️1

574

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

72

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782780882&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F37Z0jx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782780882421&amp;bpp=1&amp;bdt=82&amp;idt=47&amp;shv=r20260625&amp;mjsv=m202606250101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782780865%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782780865%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782780865%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6311776515460&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;oid=2&amp;pvsid=2090187516874031&amp;tmod=1184711186&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=59" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260625/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>