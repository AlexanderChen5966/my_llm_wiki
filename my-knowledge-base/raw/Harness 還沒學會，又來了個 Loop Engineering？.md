---
title: "Harness 還沒學會，又來了個 Loop Engineering？"
source: "https://codelove.tw/@tony/post/aWYP4x"
author:
published:
created: 2026-06-22
description: "Harness 還沒學會，又來了個 Loop Engineering ？=======================================    六月初，AI 圈被一句話點燃了---------------2026 年 6 月 7 日，OpenClaw 作者 Peter St..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月18日](https://codelove.tw/@tony/post/aWYP4x "2026-06-18 16:13:34")

## Harness 還沒學會，又來了個 Loop Engineering ？

```
六月初，AI 圈被一句話點燃了
```

---

2026 年 6 月 7 日，OpenClaw 作者 Peter Steinberger 在 X 上發了一篇貼文：

> **You shouldn't be prompting coding agents anymore. You should be designing loops that prompt your agents.**  
> （你不該再親自 Prompt 編碼 Agent 了。你應該設計一套 Loop，由它去 Prompt Agent。）
> 
> 程式設計

[![](https://i.imgur.com/y8tvU8f.jpeg)](https://i.imgur.com/y8tvU8f.jpeg)

短短一句話，瀏覽量迅速突破到 **820 萬** 。Tech Twitter 上，有人把它稱為「六月最短的爭議句」——支持和質疑的貼文刷了一整屏又一整屏。Firecrawl 的部落格寫得很直白： *A six-word sentence has tech Twitter in a chokehold this month.*

爭議還沒降溫，Google 工程師 **Addy Osmani** 第二天就發表了長文 [Loop Engineering](https://link.juejin.cn/?target=https%3A%2F%2Faddyosmani.com%2Fblog%2Floop-engineering%2F) ，把這個正在發酵的概念正式命名、拆解成可落地的框架。

隨後一週裡，MindStudio、AlphaMatch、Firecrawl 等團隊相繼發文解讀——一個新術語，幾乎在一夜之間從「大佬語錄」變成了「2026 年 AI 編程的方法論」。

而 Steinberger 那句話，並非空穴來風。

早在這之前的 6 月 2 日，Anthropic **Claude Code 負責人 Boris Cherny** 在 WorkOS 的 Unplugged 活動上說了幾乎一模一樣的話——這段發言隨後被剪成短影片，在 X 上廣泛傳播：

> **I don't prompt Claude anymore. I have loops that are running. They're the ones that are prompting Claude and figuring out what to do. My job is to write loops.**  
> （我已經不再 Prompt Claude 了。我有一些 Loop 在跑，它們負責 Prompt Claude、決定下一步該做什麼。我的工作，是寫這些 Loop。）
> 
> AI工具、聊天機器人與虛擬助理

[![](https://i.imgur.com/exkh2Vf.jpeg)](https://i.imgur.com/exkh2Vf.jpeg)

到 2026 年 3 月，Claude Code 專案本身已 **100% 由 Claude Code 自主維護** 。據他披露，當時已有約 **4% 的公開 GitHub commit** 來自 Claude Code。這樣的工作流，從「手寫程式碼」→「Prompt Agent 寫程式碼」→「寫 Loop 讓 Agent 自己 Prompt 自己」—— **三次抽象層級躍遷，全部發生在不到一年的時間裡。**

Osmani 在部落格裡引用了上述兩位的話，並給出了自己的定義：

> **Loop Engineering = 用系統設計，取代你本人充當 Agent 的 Prompt 操作員。**

Loop 是一個 **遞迴目標** ：你定義目的和停止條件，系統自動迭代，直到完成為止。你設計的是「發現工作 → 分發 → 執行 → 檢查 → 記錄 → 決定下一輪」——而不是坐在 Chat 視窗裡，一個 turn 接一個 turn 地打字。

Cherny 說得很清楚： **工作並沒有變輕鬆，變的是槓桿點。**

過去比的是誰 Prompt 寫得好；現在比的是誰 Loop 設計得巧。Osmani 也提醒：這還早，Token 成本可以差幾個數量級，Verification 比以往任何時候都更依賴工程師本人——但 Codex 的 Automations、 `/goal` ，Claude Code 的 `/loop` 、hooks、Sub-agents， **骨架已經長進產品裡了** 。

AI工具、聊天機器人與虛擬助理

一旦看清這個形狀，爭論該用哪個工具反而次要；關鍵是你的 Loop 能不能轉起來。

[![](https://i.imgur.com/PPCBbWi.jpeg)](https://i.imgur.com/PPCBbWi.jpeg)

---

## 什麼是 Loop Engineering？

如果你剛在社群裡刷過 **Harness Engineering** ——給 Agent 搭環境、定規矩、建回饋——先別慌：很多團隊還沒完全吃透，Loop 的概念又火了。

Osmani 有個比喻很貼切： `Harness 是跑道；Loop 是跑道上的調度系統。` 前者管單個 Agent 怎麼安全地跑；後者管誰去找活、誰去幹、誰去驗收、什麼條件下收工。

Osmani 把一套能轉起來的 Loop 拆成五塊積木，加一塊外部記憶。不必記工具名，先理解分工。

`Automations——Loop 的心跳。` 沒有定時觸發，Loop 就只是你手動跑過一次的腳本。Automations 負責按節奏自動發現工作：CI 昨晚紅了、Issue 堆了、某個模組上週剛改過。Codex 有 Automations 面板，Claude Code 有 `/loop` 和 cron；本質一樣—— **讓 Loop 自己去找活幹** ，而不是你每天早上打開 IDE 想「今天讓 Agent 幹啥」。

AI工具、聊天機器人與虛擬助理

`Worktrees——並行時不踩腳。` 兩個 Agent 同時改同一個檔案，和兩個人沒溝通就 commit 同一行一樣麻煩。Git worktree 給每個任務獨立的 checkout 和分支；Codex 內建了 per-thread worktree，Claude Code 也支援 `--worktree` 和 subagent 級隔離。 **並行是 Loop 的乘數，worktree 是並行的前提。**

`Skills——專案知識外置。` Agent 每次 Session 都是冷啟動，Context 裡的空洞會被它用自信的錯誤填滿。Skill（ `SKILL.md` ）把約定、建置步驟、踩坑記錄寫進磁碟，Loop 每一輪都能讀到。 **Intent 寫一次，Loop 每一輪複用** ，而不是反覆從頭解釋專案。

`Connectors——Loop 碰到真實世界。` 只會讀檔案系統的 Loop 很小。MCP 和各類 Connector 讓 Loop 能查 Issue、讀 CI 日誌、發 Slack、調 staging API。 **差一步開 PR、差一步更新 ticket，Loop 就還是半成品。**

`Sub-agents——寫的人不要自己判卷。` 讓同一個模型寫完程式碼再宣稱「沒問題」，它幾乎一定會偏袒自己。Loop 裡常見的拆法是：一個 Agent 探索、一個實作、一個審查。Claude Code 的 Task subagents、Codex 的 `.codex/agents/` 都是這個思路。 **Maker 和 Checker 分離，是 Loop 無人值守時還能信得過結果的前提。**

`State / Memory——Agent 會忘，磁碟不會。` 進度、試過的方案、哪些過了哪些沒過，必須落在外部——Markdown 檔案、Linear board、AGENTS.md，任何形式都行。模型跨 Session 失憶； **Loop 能接力，靠的是 State，不是聊天記錄。**

### 拼起來：一個晨間 Triage（分流）Loop

Osmani 給了一個很具體的例子。

每天早上，Automation 自動跑一輪：讀昨天的 CI 失敗、新開的 Issue、近期 commit，把值得處理的事項分流出來。

每一項開獨立 worktree，Sub-agent A 起草修復，Sub-agent B 對照 Skill 和現有測試做審查。

Connector 負責開 PR、更新 ticket。Loop 處理不了的，進 Triage inbox 等人看。

State 檔案記下「試過什麼、過了什麼、還剩什麼」——明天 Automation 醒來，從同一頁接著幹。

到這一步，Loop 已經能 **發現工作、寫程式、審程式、開 PR、記狀態** 。

聽起來閉環了。

但仔細想一個問題： **它怎麼知道「做完了」？**

---

## Loop 的「完成條件」，通常停在哪裡

Codex 和 Claude Code 裡的 **`/goal`** ，是最接近「Loop 自己收工」的機制：寫一條機器能檢查的停止條件，Loop 一輪輪跑，直到成立——常見是 lint 乾淨、單測全綠、型別檢查通過，Sub-agent 再補一層 diff 審查。

AI工具、聊天機器人與虛擬助理

**對後端、CLI、純函式庫，這往往夠用。**

但對 App、Web、跨端 UI，「命令 exit 0」不等於「產品沒問題」： `畫面對不對、真機能不能跑、流程通不通` ， **都不在原始碼和終端機日誌裡。** 於是多數 Coding Loop 實際停在 **程式碼可合併** ；驗產品這一步，還是人編譯、安裝、點點點。

Loop 名義上在轉， **驗證這條支路往往仍是開環的。**

[![](https://i.imgur.com/kp4VxS1.jpeg)](https://i.imgur.com/kp4VxS1.jpeg)

`產品驗沒驗過，誰來幹？` 做 App、做 Web 的團隊，會先撞上這個問題。

---

## Coding Agent 看不見螢幕

團隊通常會試三條路，但都很難形成 **驗證子 Loop** ——Coding Agent 旁邊那條能自己轉、能判定、能把證據送回去的自動化支路：

程式設計

1. `人肉測試。` 程式碼 Loop 轉得飛起，產品還是你一個個驗。人不是 Connector，沒法被 `/goal` 調度， **瓶頸只是從 Prompt 挪到了點螢幕。**
2. `讓 Coding Agent 寫 Playwright、XPath。` 腳本能進 CI，但 UI 一改就掛，維護成本不低。更致命的是：寫測試和判測試往往是同一個 Agent——前面強調「寫的人不要自己判卷」，這裡恰恰是 **自己考自己** ，進了 CI 也不等於 Loop 可信。
3. `上雲端視覺大模型逐幀看。` 理論上說得通，一次完整回歸的截圖量，就足以讓 Token 帳單在 Loop 轉完之前叫停—— **驗證太貴，Loop 轉不起** ，又回到 Cherny 那條規律。

三條路要麼把你嵌回 Loop，要麼讓 Agent 同源判卷，要麼貴到無法日常運轉。

`我們缺的不是一個簡單的測試框架，而是驗證子 Loop。` 它至少要具備三件事——也就是前面講的 Connector、Sub-agents、State，在「驗產品」時各自該幹什麼：

- **Connector** ——連得上真機/瀏覽器，驗完自動把結果送回 Coding Agent，不用人截圖貼上去
- **Sub-agents** ——點的和判的不是同一個 Agent，不能自己說自己過了
- **State** ——驗過什麼、哪一步掛了，寫進檔案；別只留在聊天記錄裡

換句話說， `要實現 AI 研發的 Loop 閉環，就必須讓 AI 實現 E2E 測試` 。這一點在行動端領域尤為重要。

這也是我做 [Munk AI](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fchaxiu%2Fmunk-ai) 這個開源專案的原因。

---

## 拼起來：一條帶「驗產品」的功能交付 Loop

我從幾個月前開始做 **Munk AI** ，它的功能很簡單，那就是： `讓 AI 控制 Android / iOS，去做真機 E2E 測試。`

它能接到 Cursor、Claude Code、Trae 的日常開發流程，跑在你本機。

[![output-4fps.gif](https://p6-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/191f686696b541d1ab26b921f18c534c~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5pyx5rab55qE6Ieq5Lmg5a6k:q75.awebp?rk3s=f64ab15b&x-expires=1782089018&x-signature=ApZ337HGPJM9Xxy83yaYrtb5ux4%3D)](https://p6-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/191f686696b541d1ab26b921f18c534c~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5pyx5rab55qE6Ieq5Lmg5a6k:q75.awebp?rk3s=f64ab15b&x-expires=1782089018&x-signature=ApZ337HGPJM9Xxy83yaYrtb5ux4%3D)

還是用一個例子——開發「刪除帳號」功能， **lint、單測、真機驗證都過，才開 PR。**

---

1. `你先用自然語言把需求說清楚` ，比如：「使用者可以在設定頁刪除帳號，確認後回到登入頁，再次登入時舊資料不可見。」
2. `Coding Agent 據此寫程式碼` ，把 App 裝到手機或瀏覽器上——這一步和平時一樣。
3. `接下來交給 Munk。` 它先看需求，再看程式碼 Diff 摘要，再在裝置上按使用者路徑操作一遍：設定 → 帳號 → 刪除 → 確認。然後將操作紀錄交給另一個 Agent 來判定： `測試是否通過` 。
4. `第一次可能過不了` 。比如確認彈窗被鍵盤擋住，點不到「確認」。這時候，判定的 Agent 不會只丟一句「失敗了」：它把 `卡在哪一步、當時畫面長什麼樣` 打包送回 Coding Agent。Agent 改程式碼、重新安裝，自動接著驗—— `條件沒滿足，就不停` 。
5. `第二次通過了` ，Coding Agent 再去開 PR。

整條鏈路就是： **你說清楚要什麼 → 系統自動跑 → 搞不定的帶著證據回來 → 下一輪接著幹。** 差別在於，收工條件裡多了一條—— **產品在真機上真的驗過了。**

[![](https://i.imgur.com/4o1uvt8.jpeg)](https://i.imgur.com/4o1uvt8.jpeg)

---

## 寫在最後

不管是 `Harness Engineering` 還是 `Loop Engineering` ，它們最終的目標，都是為了：

- 讓 AI 工作表現更 `穩定` ；
- 讓 AI 工作時間更 `持久` ；
- 讓 AI 工作能更 `自主` ；

Munk AI 的目標，就是： `打通行動端 AI 真機測試這條鏈路` 。

機器學習與人工智慧

Loop Engineering 要在行動端更穩地落地，就離不開 AI 真機測試。

Munk AI 是我們做「行動端 AI 測試」這條支路的開源嘗試，還在早期。

如果你也在用 AI 寫程式、苦於總要自己點點點，歡迎一起來試：

- 👀 **安裝體驗、文件與更新** ： [munk.sh](https://link.juejin.cn/?target=https%3A%2F%2Fwww.munk.sh%2Fzh)
- ⭐ **Star 開源倉庫** ： [github.com/chaxiu/munk…](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fchaxiu%2Fmunk-ai)
- 🐦 **關注 X / Twitter，看後續實踐與更新** ： [x.com/iBoyCoder](https://link.juejin.cn/?target=https%3A%2F%2Fx.com%2FiBoyCoder)
- 📮 **關注公眾號「朱濤的自習室」** ：Loop Engineering、AI 測試與 Munk 的落地筆記會陸續更新 · \*

---

原文出處： [https://juejin.cn/post/7651153611318460425](https://juejin.cn/post/7651153611318460425)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aWYP4x) [107瀏覽](https://codelove.tw/@tony/post/aWYP4x)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aWYP4x)

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

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝16 💬1 ❤️1

507

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

30

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782108504&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaWYP4x&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782108504068&amp;bpp=1&amp;bdt=290&amp;idt=231&amp;shv=r20260617&amp;mjsv=m202606170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782108370%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782108370%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782108370%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=7923853542672&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099218&amp;oid=2&amp;pvsid=8078091810123139&amp;tmod=1256732551&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D2&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=243" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260617/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>

機器人工學