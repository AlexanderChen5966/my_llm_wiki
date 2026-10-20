---
title: "Codex 提示詞怎麼下才聽話？4 要素框架＋AGENTS.md，讓它不再說改了卻沒改"
source: "https://maplefeather.com/article/codex-prompt-tips-agents-md-2026"
author:
  - "[[夜羽凌]]"
published: 2026-07-10
created: 2026-07-17
description: "Codex 提示詞老是講不聽、還說改了其實沒改？我每天用它寫程式，靠官方 4 要素框架＋一份 AGENTS.md，把它調到一次就做對，附可直接複製的繁中提示詞範本。"
tags:
  - "clippings"
status: processed
---
<iframe width="1200" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-7675521343679636&amp;output=html&amp;h=280&amp;slotname=9512328437&amp;adk=588573262&amp;adf=2457949142&amp;pi=t.ma~as.9512328437&amp;abgtt=6&amp;w=1200&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1784277639&amp;rafmt=1&amp;format=1200x280&amp;url=https%3A%2F%2Fmaplefeather.com%2Farticle%2Fcodex-prompt-tips-agents-md-2026&amp;fwr=0&amp;fwrattr=true&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTE1IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjExNSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTE1Il1dLDBd&amp;dt=1784277639590&amp;bpp=1&amp;bdt=1059&amp;idt=213&amp;shv=r20260716&amp;mjsv=m202607140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie_enabled=1&amp;eoidce=1&amp;prev_fmts=0x0&amp;nras=1&amp;correlator=3316231182529&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=167&amp;ady=265&amp;biw=1533&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95396567%2C95394425&amp;oid=2&amp;pvsid=5590589403723527&amp;tmod=577100013&amp;uas=0&amp;nvt=6&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1533%2C1003&amp;vis=2&amp;rsz=%7C%7CEer%7C&amp;abl=CS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;bz=0&amp;pgls=CAA.&amp;ifi=2&amp;uci=a!2&amp;fsb=1&amp;dtd=216" title="Advertisement" aria-label="Advertisement"></iframe>

> **💡 核心結論速覽 (TL;DR)**
> 
> - **Codex 提示詞的核心心法是「給脈絡＞改措辭」** ：它會照「它以為的」而不是「你說的」做，最傷的是那種「流暢地犯錯」——說改了其實沒改。你缺的通常不是更兇的語氣，是更完整的脈絡。
> - **官方四要素框架一次補齊** ：目標＋脈絡＋限制＋完成定義（Done when），一個 prompt 塞好這四塊，命中率立刻不一樣，比反覆重講三次省事。
> - **投報率最高的一次性投資＝寫一份 AGENTS.md** ：三層作用域、合併上限 32 KiB，把「每次都要重複糾正它的地方」寫成一條長期規則，之後每個任務自動帶進場。
> - **行動建議** ：複雜任務先用
> 	```
> 	/plan
> 	```
> 	讓它出書面計畫、你核可再動手；並給它能自我驗證的測試，別讓弱驗證把 agent 變成「錯誤放大器」。

老實說，我一開始也覺得「AI 寫程式就是把需求打進去、等它吐 code」這麼簡單。直到我每天用 Codex 跑好幾個小時，才發現真正卡住我的從來不是它不會寫，而是它很敢寫——我一句模糊的話，它就自信滿滿地一路衝，改了十幾個檔案，結果全歪。更氣的是它還跟你說「已經改好了」，你一跑才發現根本沒動到重點。

用久了我才想通一件事： **Codex 提示詞下不好，八成不是你講得不夠用力，是你給的脈絡不夠。** 它照的是「它以為你要的」，不是「你真正要的」。這篇我不打算丟一張你背不起來的指令表給你，而是把我每天在用、真正讓它「一次做對」的溝通方式跟工作流拆給你看：官方的四要素提示框架、怎麼叫它先規劃再動手、怎麼逼它做到底別半途停下來問，還有那個我覺得投報率最高的東西——一份 AGENTS.md。

開發工具

如果你也常對著螢幕想「我明明講了啊，它怎麼又自作主張」，這篇就是寫給你的。準備好把 Codex 從「很敢亂做的實習生」調教成「聽得懂人話的隊友」了嗎？

---

## 為什麼 Codex 老是「做錯」？先搞懂它照的是它以為的，不是你說的

先給結論： **Codex 做錯，最常見的原因不是它笨，是它在「猜」——你沒給夠脈絡，它就自己腦補一個版本去做。** 所以修好提示詞的第一步，不是把語氣加重，是把它需要知道的東西補齊。

我自己最有感的坑有三種。第一種是「它照它以為的做」：你說「優化這個函式」，你心裡想的是可讀性，它以為你要效能，於是把一段好懂的程式改成一堆位元運算，跑是跑得動，但沒人看得懂。第二種更陰險，我叫它「流暢地犯錯」——它會用非常有條理、非常有自信的口吻，把一件錯的事講得像對的，你一不小心就信了。第三種最傷： **它跟你說「我已經把 X 改好了」，你興高采烈去跑，發現它根本沒動那個檔，或只改了一半。** 這種「說改了其實沒改」，是我踩過最多次、也最想提醒你的雷。

這三種坑的共同解法，其實是同一個心態：把 [OpenAI Codex](https://openai.com/codex/) （還有它的 [開源 CLI](https://github.com/openai/codex) ）當成一個「能力很強、但需要你給結構跟邊界」的資淺隊友，而不是一個丟願望就會變出完美結果的許願池。它強在執行，弱在「讀心」。你越把脈絡講清楚——相關的檔案、實際的錯誤訊息、要遵守的規範——它猜錯的空間就越小。想更完整理解這類會自己動手做事的 AI 到底能做到哪一步、又有哪些天生限制，可以先看我整理的 [AI agent 到底是什麼、能自己扛下哪些工作](https://maplefeather.com/article/ai-agent-what-is-2026-tools-guide) ，回來看技巧會更有感。

也別太意外它會出包——官方自己都還把 Codex 標成 experimental，issue 區也累積了數千筆待處理的回報。這不是要你別用，而是提醒你：它是強大但還在長大的工具，你給它的引導越好，它的表現落差就越小。

**下一步** ：下次它做錯，先別急著罵它「你怎麼又錯」，改問自己「我剛剛有沒有給它足夠的脈絡」。這個視角一換，你會發現八成的錯都能靠補脈絡解掉。

---

## Codex 提示詞的官方四要素框架：目標＋脈絡＋限制＋完成定義

直接給你可以照抄的結構： **一個好的 Codex 提示詞，把「目標、脈絡、限制、完成定義」這四塊補齊就對了。** 這不是我發明的，是 [Codex 官方 best practices 文件](https://learn.chatgpt.com/guides/best-practices) 整理的框架，我實測下來，光是養成補齊這四塊的習慣，重講第二次的機率就掉一大半。

開發工具

四塊分別在回答四個問題，我用一張表幫你對齊：

| 要素 | 它在回答什麼 | 你該塞進去的東西 |
| --- | --- | --- |
| 目標（Goal） | 你到底要改什麼、做什麼 | 一句話講清楚要的結果，不要含糊 |
| 脈絡（Context） | 哪些檔案、資料夾、文件、錯誤訊息相關 | 檔案路徑、貼上實際報錯、參考範例 |
| 限制（Constraints） | 有什麼規範、架構、安全要求要遵守 | 不能碰哪些檔、要沿用哪個慣例 |
| 完成定義（Done when） | 怎樣才算做完 | 要過哪些測試、要驗證什麼、要列出動過哪些檔 |

光看表可能無感，我給你三個不同情境、可以直接複製去改的繁中範本。你會發現它們長得很像，因為四要素的骨架是共用的：

> **範例①｜修 bug（最常用）**
> 
> 目標：修好登入頁在手機上點「登入」沒反應的問題。  
> 脈絡：相關檔案是
> 
> ```
> src/pages/Login.tsx
> ```
> 跟
> ```
> src/api/auth.ts
> ```
> ，我把 console 的錯誤訊息貼在下面：（貼上報錯）。  
> 限制：不要動
> ```
> auth.ts
> ```
> 對外的函式介面，沿用現有的表單驗證工具，不要引入新套件。  
> 完成定義：手機實機點得動、
> ```
> npm run test
> ```
> 的登入相關測試要綠，改完請列出你實際動過哪些檔。

> **範例②｜加功能**
> 
> 目標：在文章頁加一個「複製連結」按鈕，點了把當前網址複製到剪貼簿並顯示提示。  
> 脈絡：頁面元件在
> 
> ```
> components/ArticleHeader.tsx
> ```
> ，UI 風格請參考同資料夾裡現成的
> ```
> ShareButton
> ```
> 。  
> 限制：沿用專案現有的 toast 提示元件，不要自己刻一個；樣式走 Tailwind，不要寫 inline style。  
> 完成定義：桌面跟手機都能複製成功、有成功提示、你補一個對應的測試並跑過。

> **範例③｜重構**
> 
> 目標：把
> 
> ```
> utils/format.ts
> ```
> 裡那個又臭又長的
> ```
> formatData
> ```
> 拆成幾個小函式，維持行為不變。  
> 脈絡：這個函式被
> ```
> Dashboard.tsx
> ```
> 跟
> ```
> Report.tsx
> ```
> 用到，先把呼叫點列給我確認。  
> 限制： **對外行為與回傳格式完全不能變** ，只動內部結構，不改公開介面。  
> 完成定義：現有測試全數通過（別改測試來配合你的改動），並附上你怎麼驗證行為沒變。

你有沒有注意到，這三個範本我都在「限制」裡明講「不要碰什麼」、在「完成定義」裡要求「列出動過哪些檔」？這兩招是我對付「說改了其實沒改」的護身符——你事先要它交代動過哪些檔，它就很難含糊帶過。還沒決定要用哪套 AI 寫程式工具的話，我把幾家攤開比過， [2026 年幾款主流 AI 寫程式工具到底怎麼選](https://maplefeather.com/article/best-ai-tools-for-coding-2026) 那篇可以先幫你定位自己是哪種使用者。

機器學習與人工智慧

**下一步** ：把上面任一個範本存成你的罐頭範本，下次下指令前花 30 秒把四塊填一填。這 30 秒，通常能省掉你後面來回三輪的時間。

---

<iframe width="1200" height="309" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?us_privacy=1---&amp;gpp_sid=-1&amp;client=ca-pub-7675521343679636&amp;output=html&amp;h=280&amp;slotname=4119281396&amp;adk=3389954870&amp;adf=330984058&amp;pi=t.ma~as.4119281396&amp;abgtt=6&amp;w=1200&amp;fwrn=4&amp;fwrnh=0&amp;lmt=1784277885&amp;rafmt=1&amp;armr=3&amp;format=1200x280&amp;url=https%3A%2F%2Fmaplefeather.com%2Farticle%2Fcodex-prompt-tips-agents-md-2026&amp;fwr=0&amp;fwrattr=true&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTE1IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjExNSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTE1Il1dLDBd&amp;dt=1784277885948&amp;bpp=3&amp;bdt=247419&amp;idt=3&amp;shv=r20260716&amp;mjsv=m202607140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dc4d08d560e45f0e7%3AT%3D1784277639%3ART%3D1784277639%3AS%3DALNI_MbS2pkL7RdSrSb3RQeZfCOP_l93nQ&amp;gpic=UID%3D000014c4f34074bf%3AT%3D1784277639%3ART%3D1784277639%3AS%3DALNI_ManE6qDRnxOniLtWq82rFDEeLQ00w&amp;eo_id_str=ID%3Dfd185ca4b91874ce%3AT%3D1784277639%3ART%3D1784277639%3AS%3DAA-AfjaDY3E2AZGzENSXE3Y8IrbY&amp;prev_fmts=0x0%2C1200x280%2C1533x1003%2C1200x280&amp;nras=2&amp;correlator=3316231182529&amp;frm=20&amp;pv=1&amp;rplot=4&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=167&amp;ady=3977&amp;biw=1533&amp;bih=1003&amp;scr_x=0&amp;scr_y=2481&amp;eid=95396567%2C95394425&amp;oid=2&amp;psts=AOrYGskEH_WFxzGgH7hRsxRqLknFzt1o8P4nGA1a4Rbf7b8OPhn70sWKU4agfYzmJ9Svg7MMbT13M0hLAMAHNGr6BYO7VJtph0egVL8b&amp;pvsid=5590589403723527&amp;tmod=577100013&amp;uas=0&amp;nvt=6&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=2070%2C30%2C2070%2C30%2C1920%2C30%2C1773%2C1050%2C1533%2C1003&amp;vis=1&amp;rsz=%7C%7CEebr%7C&amp;abl=CS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;pgls=CAA.&amp;ifi=5&amp;uci=a!5&amp;btvi=2&amp;fsb=1&amp;dtd=10" title="Advertisement" aria-label="Advertisement"></iframe>

## 叫 Codex 先規劃再動手：用 /plan 讓它先給計畫，你核可再執行

先講判斷： **任務越大、越不確定，就越該先讓 Codex「只出計畫、先不要動手」。** 與其讓它埋頭做錯方向、你事後收爛攤子，不如花兩分鐘先看它打算怎麼做。

Codex 有一個計畫模式（Plan mode），官方文件也直接推薦它是多數人最省事的用法——按 **Shift+Tab** 切到

```
/plan
```
，它會先去讀檔、收集脈絡、必要時反問你幾個問題，然後產出一份「我打算改哪些檔、用什麼做法、邊界在哪、要過哪些測試」的書面計畫， **這階段它不會真的動你的原始碼** ，等你點頭才開工。我面對複雜任務幾乎都先走這步，流程大概是這樣：

**❶ 切計畫模式** ：按 Shift+Tab 切到 plan，把四要素需求丟給它。  
**❷ 讀它的計畫** ：重點看它有沒有誤解目標、有沒有要去動你不想讓它碰的檔。  
**❸ 打回去修** ：計畫不對就直接講「第 2 步不要動資料庫 schema，改用既有 migration」，讓它改計畫，而不是改到一半才喊停。  
**❹ 核可再執行** ：計畫對了再讓它動手，這時它已經有一份你認可的地圖，跑歪的機率低很多。

電腦科學

反過來也有個好用招數：如果你自己的需求還很模糊，可以直接叫它「 **先別動手，先訪談我，把需求問清楚再開始** 」。讓它反問你，往往會逼出你自己都沒想清楚的細節。這招跟計畫模式是一體兩面——一個是它先寫計畫給你看，一個是它先問你把需求挖乾淨。 [Claude Code](https://code.claude.com/docs) 也有很像的計畫模式跟一堆隱藏用法，我整理在 [Claude Code 我每天在用的隱藏功能](https://maplefeather.com/article/claude-code-hidden-features-tips-2026) ，兩邊對照看會更懂這類工具的共通邏輯。

**下一步** ：下次遇到「會動到很多檔」的任務，先按 Shift+Tab 進計畫模式，別讓它一聲不吭就開改。光這個習慣，就能擋掉大半的「方向做錯」。

---

## 要 Codex 做到底、別停在分析：可直接貼的「一路做完」指令

先講痛點： **Codex 很愛「做一半就停下來問你」，或是只給你一份分析、卻不真的把 code 寫出來。** 如果你要的是「一路把事情做完」，就得在提示詞裡明講，因為它預設會偏保守。

這件事官方的 [Codex prompting 指南](https://developers.openai.com/cookbook/examples/gpt-5/codex_prompting_guide) 講得很直接，我把它最實用的幾句英文原文貼給你，這種指令我建議保留英文照貼，辨識度最高（後面附中文說明）：

> **要它做到底、別停在分析：**  
> 
> ```
> Persist until the task is fully handled end-to-end within the current turn whenever feasible: do not stop at analysis or partial fixes.
> ```
>   
> （中文意思：這一輪盡量把任務從頭做到尾，不要停在「只做分析」或「只修一半」。）
> 
> **要它預設直接實作、別動不動就回來問：**  
> 
> ```
> Bias to action: default to implementing with reasonable assumptions; do not end your turn with clarifications unless truly blocked.
> ```
>   
> （中文意思：以行動為預設，用合理假設直接做；除非真的卡死，否則不要用「請你再確認」來結束。）
> 
> **要它交出能跑的成品、不是一份計畫：**  
> 
> ```
> Default expectation: deliver working code, not just a plan. If some details are missing, make reasonable assumptions and complete a working version.
> ```
>   
> （中文意思：預設要交出能動的程式，不是只給計畫；細節缺就用合理假設補上，做出一個能跑的版本。）

你可能會想：這跟上一段「先規劃再動手」不是矛盾嗎？其實不會，這是兩個不同階段的開關。 **需求不明、風險高的時候用計畫模式先談清楚；方向已經定了、你只想它一路做完的時候，就掛上這幾句「做到底」指令。** 我自己的用法是——探索期讓它先規劃，執行期讓它別囉嗦。搞清楚什麼時候該用哪個，比死背指令重要太多。

不過提醒一個現實面： **叫它「一路做到底」很爽，但這種放手跑法很吃 token，帳單會比你想的漲得快。** 並行跑、高強度推理都是燒錢大戶。怕被帳單嚇到的話，我把砍半省 token 的實戰心法整理在 [AI 寫程式代理帳單暴增、怎麼省 token](https://maplefeather.com/article/ai-coding-agent-token-cost-save-2026) 那篇，放手之前先看一眼。

**下一步** ：把上面三句存成一組「做到底」罐頭，方向確定的任務就貼上去；但記得配合成本意識，別無腦讓它在背景燒錢。

---

<iframe width="1200" height="309" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?us_privacy=1---&amp;gpp_sid=-1&amp;client=ca-pub-7675521343679636&amp;output=html&amp;h=280&amp;slotname=4119281396&amp;adk=3389954870&amp;adf=2227865734&amp;pi=t.ma~as.4119281396&amp;abgtt=6&amp;w=1200&amp;fwrn=4&amp;fwrnh=0&amp;lmt=1784277893&amp;rafmt=1&amp;armr=3&amp;format=1200x280&amp;url=https%3A%2F%2Fmaplefeather.com%2Farticle%2Fcodex-prompt-tips-agents-md-2026&amp;fwr=0&amp;fwrattr=true&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTE1IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjExNSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTE1Il1dLDBd&amp;dt=1784277893721&amp;bpp=1&amp;bdt=255192&amp;idt=2&amp;shv=r20260716&amp;mjsv=m202607140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dc4d08d560e45f0e7%3AT%3D1784277639%3ART%3D1784277639%3AS%3DALNI_MbS2pkL7RdSrSb3RQeZfCOP_l93nQ&amp;gpic=UID%3D000014c4f34074bf%3AT%3D1784277639%3ART%3D1784277639%3AS%3DALNI_ManE6qDRnxOniLtWq82rFDEeLQ00w&amp;eo_id_str=ID%3Dfd185ca4b91874ce%3AT%3D1784277639%3ART%3D1784277639%3AS%3DAA-AfjaDY3E2AZGzENSXE3Y8IrbY&amp;prev_fmts=0x0%2C1200x280%2C1533x1003%2C1200x280%2C1200x280&amp;nras=2&amp;correlator=3316231182529&amp;frm=20&amp;pv=1&amp;rplot=4&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=167&amp;ady=5739&amp;biw=1533&amp;bih=1003&amp;scr_x=0&amp;scr_y=4267&amp;eid=95396567%2C95394425&amp;oid=2&amp;psts=AOrYGskEH_WFxzGgH7hRsxRqLknFzt1o8P4nGA1a4Rbf7b8OPhn70sWKU4agfYzmJ9Svg7MMbT13M0hLAMAHNGr6BYO7VJtph0egVL8b%2CAOrYGsmyEZoTS4pXw-weLZmT3FY8sqGxKiNTRrp6Yacx3Y2zSt-U39tiWpwSxc-IEk5XYB2hiHz6L19ci5d_dncMxIHoEUDnhPBXrawZ0Xz8FK3X7kM&amp;pvsid=5590589403723527&amp;tmod=577100013&amp;uas=0&amp;nvt=6&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=2070%2C30%2C2070%2C30%2C1920%2C30%2C1773%2C1050%2C1533%2C1003&amp;vis=1&amp;rsz=%7C%7CEebr%7C&amp;abl=CS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;pgls=CAA.&amp;ifi=6&amp;uci=a!6&amp;btvi=3&amp;fsb=1&amp;dtd=6" title="Advertisement" aria-label="Advertisement"></iframe>

## AGENTS.md：Codex 投報率最高的一次性投資，把重複糾正變成規則

如果這篇你只帶走一個東西，我希望是這個： **寫一份 AGENTS.md，是我認為 Codex 使用者投報率最高的一次性投資。** 它是一個「會跟著專案存在的長期指令檔」，Codex 每次開工前會自動讀它，等於你把「每次都要重複交代的話」一次寫好，之後每個任務都自動帶進場。

機器學習與人工智慧

它最迷人的地方在於 **三層作用域** ，根據 [Codex 官方的 AGENTS.md 規格文件](https://learn.chatgpt.com/docs/agent-configuration/agents-md) ，Codex 會從三個地方由外往內把指令串起來：

**❶ 全域層** ：放在

```
~/.codex/AGENTS.md
```
，寫你跨所有專案都想遵守的個人習慣（例如「回答用繁體中文」「改完先列動過哪些檔」）。  
**❷ 專案根層** ：放在專案根目錄，寫這個專案的 build/測試指令、技術棧、慣例。  
**❸ 子目錄層** ：放在特定資料夾裡，寫只適用那塊的規則（例如前端資料夾的樣式規範）。

關鍵規則是： **越靠近你當下工作目錄的檔案，優先級越高、會覆蓋外層** 。所以子目錄能推翻專案根、專案根能推翻全域。另外有兩個實用細節值得記：一是合併後的指令檔 **預設上限 32 KiB** （超過可以調參數或拆到子目錄），這也呼應官方的原則——短而準勝過又長又空泛，別把它當 README 塞兩千行；二是有個

```
AGENTS.override.md
```
，當你想「臨時覆蓋、但不想刪掉原本的規則」時用它，用完移除就回復原狀，很適合短期實驗。

你不用從零手寫——最省事的做法是直接叫 Codex 掃過整個專案、幫你生一份 AGENTS.md 初稿，你再手動補上「這專案的雷區」。那該寫什麼？官方建議放這幾類：

> **📌 一份精簡 AGENTS.md 該有的骨架** （重點是準，不是長）：
> 
> ・ **建置與測試指令** ：跑測試用
> 
> ```
> pnpm test
> ```
> 、lint 用
> ```
> pnpm lint
> ```
> 、build 用
> ```
> pnpm build
> ```
> 。  
> ・ **技術棧與慣例** ：TypeScript + Next.js，樣式一律 Tailwind，不要寫 inline style。  
> ・ **邊界規則** ：不要動
> ```
> .env
> ```
> 、不要碰
> ```
> migrations/
> ```
> 、不要自己加新套件，要加先問。  
> ・ **交付要求** ：改完列出實際動過哪些檔，並說明你怎麼驗證。

而 AGENTS.md 真正的殺手級用法，我覺得是這句話： **每一個你重複糾正它的地方，就寫成一條規則。** 你發現它老是自作主張加套件？寫進去。老是忘記跑 lint？寫進去。老是用簡體字回你？寫進去。糾正它一次是消耗，把糾正變成一條規則是投資——寫一次，之後永遠不用再講。這跟 Claude Code 用 [一份 CLAUDE.md 當專案記憶](https://maplefeather.com/article/claude-code-hidden-features-tips-2026) 是同一套邏輯，只是檔名不同、各認各的。

至於 AGENTS.md 以外的細部設定——像

```
config.toml
```
裡的模型、權限、沙箱那些逐項參數——不在這篇範圍，那是另一個主題，想調到順手可以直接看我寫的 [Codex 設定檔逐項調教指南](https://maplefeather.com/article/codex-settings-config-toml-guide-2026) ，這篇我們專心把「怎麼跟它溝通」講透。

**下一步** ：今天就在你最常用的專案根目錄開一個 AGENTS.md，先寫三行——build 指令、一個邊界規則、一個交付要求。之後每次糾正它，就順手多加一條。

---

## Codex 進階工作流：worktree 平行 agent、/review 審查、丟截圖除錯

這一段是把 Codex 從「單線小助手」變成「小型工程隊」的關鍵。 **真正的 power user 差別，不在他記得多少指令，而在他會不會用工作流把一個 agent 變成很多個、還能自己審自己的 code。** 幾個我天天在用的：

開發工具

**Git worktree 開平行 agent。** 這是我效率真正翻倍的一招。用 git worktree 從同一個 repo 切出 3-4 個工作目錄、各自在不同分支、各開一個 Codex session，它們就能同時各做各的、互不覆蓋檔案——等於把一個助手變成一支小隊。但這裡有個多數教學沒講清楚的雷我一定要提醒你： **worktree 只隔離「檔案」，不隔離「port、資料庫、服務」** ，所以平行跑前端預覽時它們還是會搶同一個 port。這個坑跟怎麼解，我在 [Codex 雲端環境 vs 本機環境怎麼選](https://maplefeather.com/article/codex-cloud-vs-local-environment-2026) 那篇講得很細，要玩平行 agent 之前建議先看。

```
/review
```
**內建 code review。** Codex 可以幫你審自己（或別人）寫的 code，而且能挑對象——針對還沒提交的變更、某一個 commit、或是拿整條分支跟 base branch 比（很像 PR review）。我習慣讓它做完一段就先自審一輪，很多低級錯它自己就抓出來了，省得我逐行讀。

其他幾個順手的功能整理成一張表：

| 功能 | 作用 |
| --- | --- |
| ``` --image ``` ``` -i ``` | 把截圖丟進 prompt，跑版、報錯畫面直接讓它看，比打字描述快十倍 |
| ``` @ ``` | ``` @ ``` 一鍵把專案內的檔案帶進對話，省去複製貼上 |
| worktree 平行 | 多分支多 session 同時跑，各自隔離檔案，不互相覆蓋 |
| ``` /review ``` | 針對未提交變更／某 commit／base branch 做內建審查 |
| 壓縮長對話（compact） | 對話變鈍時把它壓成摘要、釋放上下文，讓它別越聊越笨 |

其中 **丟截圖除錯** 我特別想推：遇到畫面跑版或看不懂的報錯，與其用文字描述半天，直接把截圖用

```
--image
```
丟進去，它往往一秒就懂問題在哪。這種多模態能力不只用在寫 code，把文件、報表截圖丟給 AI 讀也很好用，我在 [丟 PDF、文件給 AI 幫你讀跟分析](https://maplefeather.com/article/claude-analyze-documents-pdf-data-2026) 那篇也玩過類似的路子。

**🚨 提醒** ：工作流越自動、放得越開，越要先把邊界畫好。我看過太多「喊了很多次不要動，檔案還是被 agent 刪光」的慘劇——放手讓它平行跑之前，務必先設好不能碰的路徑，這條血淚防線我整理在 [AI 寫程式代理刪錯檔的安全防線](https://maplefeather.com/article/ai-coding-agent-delete-files-safety-2026) ，看完你就懂為什麼邊界不能省。

**下一步** ：下次有「好幾件不相關的事」要做，試著用 worktree 開兩個平行 session，而不是排隊一件件來；但開跑前先花十分鐘設好 deny 邊界。

---

## 強驗證才是關鍵：別讓弱驗證把 Codex 變成錯誤放大器

這段是所有技巧的地基： **Codex 表現好不好，很大一部分取決於你有沒有給它「能自我驗證的方法」。** 官方 best practices 講得很白——別停在「叫它改一下」，要它寫測試、跑相關檢查、確認結果、審過再交給你。

開發工具

為什麼這麼重要？因為 AI 代理是一個會「自我循環」的東西。 **如果你給它強驗證（跑得起來的測試、lint、build），它每做錯一步都會被擋下來、逼它自己修正；如果你給的是弱驗證（或根本沒驗證），它就會把一個小錯一路放大成一場災難** ——這就是我說的「錯誤放大器」。同樣一個 agent，配強驗證是隊友，配弱驗證是地雷。

落到實務，我會做三件事： **❶ 給它明確的測試指令** （在提示詞或 AGENTS.md 裡寫死「改完要跑

```
npm run test
```
且要綠」）； **❷ 要它「別改測試來配合改動」** （不然它會偷偷把測試改鬆讓自己過關）； **❸ 要它交代怎麼驗證的** （列出跑了什麼、結果如何）。這三招合起來，才能讓「說改了其實沒改」無所遁形。如果你已經被各種 Codex、Claude Code 的疑難雜症搞到頭大，我把最常見的卡關跟解法整理在 [Codex 與 Claude Code 常見問題總整理](https://maplefeather.com/article/codex-claude-code-common-problems-guide-2026) ，可以搭著看。

**下一步** ：回頭檢查你最近一個專案有沒有「跑得起來的測試」。如果沒有，先補一組最基本的，這比學任何花俏提示詞都更能提升 Codex 的可靠度。

---

## Codex 適合誰、不適合誰？跟 Claude Code 溝通面差在哪

先給判斷： **Codex 最適合「任務明確、可以丟著非同步跑」的場景；如果你要的是秒級來回、邊聊邊改的高互動體驗，它不見得是最順手的那個。** 用對場景，比糾結它強不強重要。

先講適合誰、不適合誰，幫你快速定位：

**✅ Codex 適合你，如果你** ：每天要寫程式、跑重複任務；願意花一次時間寫 AGENTS.md 把規則立好；有能丟著跑的長任務、想同時並行推進好幾件事；重視「把慣例寫成檔案、讓工具自動遵守」這種工程化的協作方式。

**❌ Codex 先別急著重壓，如果你** ：只是偶爾用 AI 問問題、改幾行字（那裝個聊天介面就夠）；完全不想碰設定檔、只想開箱即用；做的東西高度依賴一直看畫面即時微調。

那它跟 Claude Code 在「溝通面」到底差在哪？兩者能做的事很像，但它們各認各的長期指令檔，這點務必分清楚：

<table><thead><tr><th><p>面向</p></th><th><p>Codex</p></th><th><p>Claude Code</p></th></tr></thead><tbody><tr><td><p>長期指令檔</p></td><td><p>讀 AGENTS.md</p></td><td><p>讀 CLAUDE.md</p></td></tr><tr><td><p>溝通調性</p></td><td><p>偏「明確任務、非同步交辦」</p></td><td><p>偏「高互動、邊聊邊改」</p></td></tr><tr><td><p>心法共通點</p></td><td colspan="2"><p>都吃「給脈絡＞改措辭」＋把重複糾正寫成規則檔</p></td></tr></tbody></table>

我自己是兩個都養著、按任務型態分工，而不是選一個贏家——這套「讓對的工具做對的事」的思路，我在 [用 Codex、Claude、Gemini 一起蓋一個網站](https://maplefeather.com/article/ai-agent-build-website-codex-claude-cowork-gemini-antigravity) 那篇實作對照裡講得更完整，想看多個 agent 怎麼協作可以接著讀。至於「值不值得為 Codex 付費」，我的判斷很簡單：如果你每天寫 code 的時間夠多，它幫你省下的時間成本，通常遠大於訂閱費；但如果你一週只用兩三次，先用免費額度摸清楚它的脾氣再說。真正的成本從來不只是月費，而是你有沒有花時間把它調教成聽話的隊友——而這，正是這整篇在講的事。

開發工具

**下一步** ：花十分鐘想清楚你的主力場景是「明確任務丟著跑」還是「高互動即時改」，再決定 Codex 該當你的主力還是配角。想連設定檔一起調到順手，接著看 [設定檔逐項調教那篇](https://maplefeather.com/article/codex-settings-config-toml-guide-2026) 。

---

## FAQ 常見問題

### Codex 提示詞用中文寫可以嗎？還是一定要用英文？

中文完全可以，我自己大量用繁體中文下指令，命中率不會比較差——關鍵從來不是語言，是脈絡夠不夠完整。唯一我會建議保留英文的，是那種「做到底、別停下來問」的行為指令（像

```
Persist until the task is fully handled…
```
），因為那是官方文件的原文寫法、辨識度最高，照貼最穩。其餘的目標、脈絡、限制，放心用中文講清楚就好。

### AGENTS.md 跟 Claude Code 的 CLAUDE.md 是同一個東西嗎？能共用嗎？

概念幾乎一樣——都是「跟著專案走的長期指令檔」，但檔名不同、各認各的：Codex 只讀 AGENTS.md、Claude Code 只讀 CLAUDE.md，不會互相讀對方的。如果你兩個工具都用，實務上可以把共通規則寫一份、兩邊各放一個對應檔名，或用符號連結指到同一份內容。重點是別假設它們會共用——你在 CLAUDE.md 寫的規則，Codex 是看不到的。

### Codex 一直說「改好了」但其實沒改，怎麼補救？

這題我踩最多次。三個補救動作：第一，在提示詞裡要求它「改完列出實際動過哪些檔」，讓它無法含糊帶過；第二，給它能跑的測試並要求「測試要綠才算完成」，用驗證逼出真相；第三，別全信它的口頭回報，自己跑一次或用

```
/review
```
讓它自審。把這三招寫進 AGENTS.md，之後就不用每次重講。

### 只有一個小改動，還需要搞四要素、寫 AGENTS.md 這麼麻煩嗎？

不用。一次性的小改動（改個文字、調個顏色），直接一句話講清楚就好，硬套框架反而是浪費。四要素框架是給「會動到多個檔、容易做歪」的任務用的；AGENTS.md 則是給「同一個專案你會反覆回來做很多次」的情境——它是一次性投資，用得越久越划算。判斷原則：任務越大、專案你會回來越多次，越值得先把規則立好。

開發工具

### 把 Codex 放手讓它自動做到底，安全嗎？

安全與否，取決於你有沒有先畫好邊界。放手跑之前，一定要先在 AGENTS.md 或權限設定裡把「不能碰的路徑、不能跑的危險指令」擋掉，再開自動化。我看過太多 agent 刪錯檔的慘劇，關鍵都是邊界沒設就放手。邊界設好，做到底才是省力；邊界沒設，做到底可能是災難。

---

## 結論：把它當隊友調教，比記住多少指令都重要

寫到這裡，回頭看我一開始那個「它怎麼又自作主張」的挫折，其實答案很簡單——我一開始就把它當許願池了。 **Codex 提示詞下得好不好，從來不是誰的措辭比較兇，而是誰願意花時間給它脈絡、幫它立規則、給它能自我驗證的方法。**

如果今天你只帶走三件事，我會說：下指令時把「目標、脈絡、限制、完成定義」四塊補齊；花一次時間寫一份 AGENTS.md，把每次重複糾正的地方變成規則；還有，永遠先給它強驗證再放手。這三件事一旦養成習慣，你會發現它「一次做對」的比例，比你去背任何冷門指令都提升得多。

工具會一直進化，目前的 Codex 模型也還在長大，但「怎麼跟它溝通」這套心法不太會過期。如果這種「幫你篩掉雜訊、只留真正有用的」風格對你有幫助，歡迎追蹤 [夜羽凌的部落格](https://maplefeather.com/page/about-yeyuling) ，我會不定期把每天踩出來的 AI 實戰心得整理給你。願你的 Codex，從今天起少一點自作主張、多一點一次做對。

---