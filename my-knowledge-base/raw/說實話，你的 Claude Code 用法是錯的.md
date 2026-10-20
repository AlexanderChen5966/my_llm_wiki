---
title: "說實話，你的 Claude Code 用法是錯的"
source: "https://codelove.tw/@tony/post/37ZZ9x"
author:
published:
created: 2026-06-29
description: "如果你覺得「Claude Code 沒有比我想像中快」，原因不是模型，也不是方案。**是你的用法。**這種話說得很重，抱歉。但我花了 3 個月，每天都跟 Claude Code 硬碰硬，一邊個人開發 Expo + Supabase + Swift 原生的通話 App，最後我非常確定了。> 所..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月27日](https://codelove.tw/@tony/post/37ZZ9x "2026-06-27 17:11:53")

如果你覺得「Claude Code 沒有比我想像中快」，原因不是模型，也不是方案。 **是你的用法。**

程式設計

這種話說得很重，抱歉。但我花了 3 個月，每天都跟 Claude Code 硬碰硬，一邊個人開發 Expo + Supabase + Swift 原生的通話 App，最後我非常確定了。

> 所有覺得自己變不快的人，都犯著同樣 7 個錯誤。

這篇不是抒情文，也不是業配。 **只寫明天就能生效的設定** ，不拐彎抹角。只要有一點戳中你，就代表你到今天為止都在浪費時間。

---

## 錯誤1. 把 CLAUDE.md 寫成「全包式聖經」

命名規則、歷史脈絡、目錄結構、情緒宣言——是不是全都塞進 `CLAUDE.md` ，做成 2000 行大作了？

**那會在每一輪都整段進到上下文裡。** 就算只是問 `git status` 的那一輪，你也得每次都為那 2000 行付 token。這不會讓它更聰明，只會燒掉你的錢包和注意力。

**正解是，保持最小化。**

```
# CLAUDE.md（這樣就夠了）
- 套件管理器是 pnpm
- Edge Functions 的部署指令是 supabase functions deploy <name>
- 多語系一定要同時更新 lib/i18n/ 裡的 8 個檔案
```

「只有特定工作才需要的知識」請丟到 **Agent Skills（`.claude/skills/` ）** 。Skills 只會在需要時才讀。不要永遠都讓它讀，讓它被觸發。

> [CLAUDE.md](http://claude.md/) 是憲法。Skills 是六法全書。分不清楚的話，先從這裡開始。

---

## 錯誤2. 把所有工作都塞進同一個指令裡

> 「幫我修測試、重構、更新文件，順便再做 PR」

你一這樣做，Claude 就會 **搞不清楚自己剛剛在做什麼。** 長上下文的中段跟人一樣，注意力會變薄（lost in the middle）。你等於是在對一個天才一次大喊所有待辦，怎麼可能不亂？

機器學習與人工智慧

**正解是，把責任切給子代理。**

```
---
name: reviewer
description: 只冷靜指出差分中的 bug，不做實作。
tools: Read, Grep, Bash
---
你是專職審查。不要給修正方案，只回報「哪裡壞了、為什麼會壞」。
```

審查、執行測試、i18n 的 8 種語言檢查—— **每次都一樣的請求，一旦子代理化，整個世界就會不一樣。** 主對話要保留給指揮官。不要自己下場幹活，要下命令。

---

## 錯誤3. 實作到一半就按 /compact

上下文一膨脹，你是不是就反射性地 `/compact` ？ **那很容易引發事故。**

在壓縮過程中，「接下來原本想做什麼」會被摘要吃掉。重開後的 Claude 會忘記自己剛剛建立的設計，然後在只完成一半的基礎上，若無其事地疊上另一套實作。 **你不會察覺，因為它看起來有在動。**

- `/compact` 只能在 **工作的分界點（提交之後）** 使用
- 長任務一開始就先讓它把 **計畫寫進 `PLAN.md`** 。就算上下文丟了，計畫還在磁碟上

> 實作途中手動 compact 就像「危險的手術」。別隨便上刀。

---

## 錯誤4. MCP 伺服器一次全部接上，還一直開著

Slack、GitHub、Notion、瀏覽器、檔案系統——覺得方便就全部一直接著。是不是很有感？

**已連線的 MCP 工具定義本身就會佔用上下文。** 那些你今天根本不會用到的工具群，每一輪都在削你的 token 和判斷力。

機器學習與人工智慧

- 只接 **今天任務需要的部分** 。不碰 GitHub 的日子，就不需要 GitHub MCP
- 如果要接很多個，請使用支援 **Deferred Tool Loading** 的做法。它會延遲載入 schema，啟動時的負擔就會消失

「先全部裝進去再說」是 2024 年的思維。 **2026 年講的是做減法。**

---

## 錯誤5. 連設計都丟給 AI

> 「幫我做成一個感覺很好的 App」

結果通常會得到的是， **看起來很不錯的地雷。**

作者真的踩過。Swift 原生的 PushKit 註冊器被寫成用 `let` 的區域變數接住，結果做出一個 **收不到來電的通話 App** 。 `PKPushRegistry` 被釋放後，推播就進不來了。AI 非常擅長寫出「看起來能跑」的程式，因此在生命週期、所有權、執行緒這些問題上常常直接漏掉。

**設計是人做的。實作才交給 AI。不要讓這條界線被拿走。**

- 「要做什麼」和「架構怎麼設計」由你決定；AI 只是負責「怎麼寫」的快速手腳
- 用所謂的 **迭代式工程（loop engineering）** 。小步丟出去→驗證→修正→再丟。不要幻想一次完成
- 只要是牽涉到原生層、並行處理、生命週期的地方， **產生後一定要由人類親眼確認所有權和執行緒**

---

## 錯誤6. 評審這件事也由人偷懶

「反正是 AI 寫的，沒問題吧」—— **剛好相反。**

因為產生速度變快了，審查的重要性也跟著上升。不是 bug 變少了，而是流入速度變快了。你拿到的不是品質保證，而是一台高速製造 bug 的機器。

- 讓在錯誤1、2裡建立的 **專職審查子代理** 先做初審，人類只負責最後確認
- 保持 diff 很小。 **1000 行的 PR，人跟 AI 都沒辦法好好審**
- 「測試有過」不等於「正確」。每次都要問： **這些測試沒有保證到什麼**

---

## 錯誤7. 同一個對話從早用到晚

一個 session 從早上一路用到晚上。上下文無止境地變長，回應開始慢慢偏掉，token 悄悄被吃光。 **你卻把這誤以為是「和 AI 深度對話」。**

- **任務一變就換 session。** 「修 Supabase function」和「加 i18n」請用不同的腦袋處理
- 長期脈絡不要放在對話裡，要 **永續化到檔案（ [CLAUDE.md](http://claude.md/) / Skills / [PLAN.md](http://plan.md/) ）** 。對話會消失，檔案不會
- 不要「養對話」，要「 **養機制** 」。這是我 3 個月裡最有效的一次思維轉換

---

## 結論

> **從「寫程式」轉成「委派任務」。**  
> 把委派對象當成很優秀、但 3 分鐘就會失憶的天才來交辦。
> 
> 軟體

常見錯誤正解 [CLAUDE.md](http://claude.md/) 寫成全包最小化＋Agent Skills1 個指令塞全部工作用子代理分責任實作中途 `/compact` 只在分界點用，計畫寫入檔案MCP 全部接上只接需要的＋延遲載入設計也丟給 AI人類負責設計，AI 負責實作不做審查專職審查代理＋小 diff同一段對話一路用到底任務切換就換 session，脈絡存檔案

這 7 項裡，有幾項戳中你？如果有 3 項以上，表示你到今天為止確實一直在浪費時間。今晚就從把 `CLAUDE.md` 砍半開始吧。

作者現在也還在遵守這 7 項原則，一邊個人開發 Expo + Supabase + Swift 的通話 App。如果你也踩過這些坑，或有自己的迴避方式，歡迎在留言區回敬我。 **那些失敗經驗，才是最值得被 LGTM 的知識。**

如果有戳中你，請給個 LGTM 和收藏。下一篇我會寫「使用 Claude Code 實際除錯原生模組的完整紀錄」。

---

原文出處： [https://qiita.com/tehito/items/356e5f1dba112a075be1](https://qiita.com/tehito/items/356e5f1dba112a075be1)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/37ZZ9x) [267瀏覽](https://codelove.tw/@tony/post/37ZZ9x)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/37ZZ9x)

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

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝13 💬2 ❤️1

571

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

71

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782697749&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F37ZZ9x&amp;pra=5&amp;asro=0&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782697749335&amp;bpp=1&amp;bdt=96&amp;idt=36&amp;shv=r20260625&amp;mjsv=m202606240101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782697603%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782697603%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782697603%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=8775335888201&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1605&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099346%2C31099360&amp;oid=2&amp;pvsid=3857575101898207&amp;tmod=636781205&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1605%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=56" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260625/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>

語言資源