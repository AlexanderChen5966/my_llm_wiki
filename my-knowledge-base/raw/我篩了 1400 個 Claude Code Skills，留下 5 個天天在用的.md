---
title: "我篩了 1400 個 Claude Code Skills，留下 5 個天天在用的"
source: "https://codelove.tw/@tony/post/3V2O43"
author:
published:
created: 2026-07-07
description: "我篩了 1400 個 Claude Code Skills，留下 5 個天天在用的  ============================================    先潑個冷水：大部分 Skills 是換了殼的 Prompt--------------------------..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月06日](https://codelove.tw/@tony/post/3V2O43 "2026-07-06 16:30:10")

## 我篩了 1400 個 Claude Code Skills，留下 5 個天天在用的

## 我篩了 1400 個 Claude Code Skills，留下 5 個天天在用的

```
先潑個冷水：大部分 Skills 是換了殼的 Prompt
```

---

這是我裝了幾十個之後最大的感受。很多所謂的 Skill，打開 `SKILL.md` 一看，核心就是一段 Prompt——「你是一個 XX 專家，請按照以下步驟……」。這種東西你直接對 AI 說一句話就能達到同樣效果，根本不需要裝一個 Skill。

**真正有價值的 Skill，一定是改變了 AI 的工作流程，而不只是改變了一段 Prompt。**

下面 5 個全部符合這個標準。

---

## 第 1 個：Superpowers — 245K Star，改變 AI 寫程式的方式

**GitHub：** obra/superpowers（245K ⭐，21.7K Fork）

```
bash 代碼解讀複製代碼npx skills add obra/superpowers
```

Superpowers 不是一個 Skill，是一套包含 20+ 子模組的開發框架。裝了之後 AI 寫程式的行為模式會發生根本變化。

**裝之前 vs 裝之後：**

場景沒裝裝了 Superpowers你說「幫我實作一個搜尋功能」AI 直接開始寫程式，寫完交給你先問你幾個問題（模糊搜尋還是精確？要不要防抖？資料量多大？），然後才動手遇到 bug 說「這裡報錯了」直接改報錯那一行，經常引出新 bug先復現 → 定位根因 → 驗證假設 → 修復 → 回歸測試，整條鏈走完你說「幫我重構這個檔案」一口氣改完整個檔案，改動太大沒法 Review先列出重構計畫讓你確認，然後分步執行，每步可以單獨驗證實作新功能只寫功能程式碼先寫測試案例，再寫實作，跑通測試才算完 **核心變化：AI 從「聽話的打字員」變成「會思考的同事」。**

有人在 GitHub Issue 裡發了對照實驗資料：12 個相同複雜度的任務，裝了 Superpowers 的會話比沒裝的 Token 用量平均低 14%。原因很反直覺——因為它會先花時間問清楚需求，避免後面返工。

Superpowers 裡我日常用到的子模組：

子模組什麼時候觸發解決什麼問題 **systematic-debugging** 「報錯了」/「不工作了」不再頭痛醫頭，按流程排查 **tdd** 「實作一個功能」先寫測試再寫程式 **brainstorming** 「我想做一個 XX」先發散 5 個方案再選最優 **code-review** `/review` 提 PR 前自動審查---

## 第 2 個：Taste Skill — 51.8K Star，解決「AI 做的頁面一股模板味」

**GitHub：** Leonxlnx/taste-skill（51.8K ⭐）

```
bash 代碼解讀複製代碼npx skills add Leonxlnx/taste-skill
```

如果你用 AI 生成過前端頁面，大概率遇到過這種情況：功能全對，但看起來就是「不對勁」——間距不均勻、顏色太跳、按鈕長得像 Bootstrap 預設樣式、排版像沒設計師參與過的內部工具。

Taste Skill 不提供任何元件程式碼。它只做一件事： **在 AI 生成 UI 時注入一套設計美感規則。**

**裝之前 vs 裝之後：**

維度沒裝裝了 Taste Skill間距大小隨機，頁面像沒對齊遵循 4/8/16/24px 的倍數系統配色經常出現高飽和度撞色主色+輔色+中性色，有層次排版標題和正文大小差不多明確的字級層級，視覺重心清晰按鈕/卡片圓角大小不統一全域一致的圓角和陰影 **最適合的場景：** 落地頁、產品官網、作品集、Demo——這些頁面「好不好看」直接影響使用者第一印象的地方。

這個 Skill 在掘金 6 月 GitHub 熱榜排名前十，51.8K Star 不是白來的。前端開發者如果經常用 AI 出頁面，我覺得必裝。

---

## 第 3 個：Graphify — 41.8K Star，接手陌生專案的第一件事

**GitHub：** safishamsi/graphify（41.8K ⭐）

```
bash 代碼解讀複製代碼npx skills add safishamsi/graphify
```

**解決的問題：** 接手一個跑了兩三年的老專案，幾萬行程式碼，README 已經過時了，文件和程式碼對不上。以前理清整個專案結構至少要一兩週——挨個讀檔案、畫關係圖、問同事「這個模組是幹嘛的」。

Graphify 做的事情：掃描整個程式碼庫，生成一張 **知識圖譜** ——模組間的依賴關係、核心呼叫鏈、共用工具函式的引用網路，全部視覺化展示。

**裝之前 vs 裝之後：**

場景沒裝裝了 Graphify接手新專案讀一兩週程式碼才敢動手改 3 分鐘看到全域結構，當天就能定位要改哪裡重構前評估影響手動 grep 引用，很可能漏掉一眼看到「改了這個檔案會影響哪些模組」給新人介紹專案口頭講半天，聽完還是不懂直接給他看圖譜，5 分鐘搞懂 **不只是 Claude Code 能用。** Graphify 相容 Codex、Cursor、Gemini CLI 等主流 AI 程式設計工具。

---

## 第 4 個：Deep Research — 調研型任務的質變

**觸發方式：** `/deep-research "你的問題"`

這不是第三方 Skill，是 Claude Code 內建功能，但很多人不知道。

**和直接讓 AI 搜尋的區別：**

直接說「幫我搜一下 XX」 `/deep-research` 搜尋方式一個角度搜一輪多角度扇形搜尋，自動擴展關鍵字資訊來源可能只搜到 1-2 個抓取多個來源並交叉校驗驗證不驗證，搜到什麼就給你什麼對矛盾的資訊做投票，標註置信度輸出一段文字結構化報告，帶引用來源 **實際使用場景：**

- 技術選型：「React Server Components vs Next.js App Router 哪個更適合我們的專案？」
- 方案評估：「前端監控用 Sentry 還是自建？成本對比是什麼？」
- 新技術了解：「Bun 1.2 和 Node.js 22 在效能上到底差多少？有沒有真實的基準測試？」

每次調研下來大概要消耗不少 Token，不適合頻繁用。但在需要做決策的時候，它給出的資訊密度和準確度遠超自己東搜西搜。

---

## 第 5 個：find-skills — 別在 GitHub 裡大海撈針了

1400+ 個 Skills，不可能一個個去翻 GitHub。用 find-skills 直接按關鍵字搜。

```
arduino 代碼解讀複製代碼/find-skills "前端 UI 設計"
/find-skills "測試 自動化"
/find-skills "效能優化"
```

Skills 生態還在高速成長，每週都有新專案冒出來。有這個入口才能及時發現好用的新 Skill，不至於資訊落後。

---

## 速查對照表

SkillStar核心價值安裝 **Superpowers** 245KAI 從「打字員」變成「會思考的同事」 `npx skills add obra/superpowers` **Taste Skill** 51.8K治好 AI 生成 UI 的「模板味」 `npx skills add Leonxlnx/taste-skill` **Graphify** 41.8K陌生程式碼庫 3 分鐘看懂 `npx skills add safishamsi/graphify` **Deep Research** 內建調研型任務的質變 `/deep-research` **find-skills** 內建Skills 搜尋入口 `/find-skills` ---

## 我卸掉的那些 Skill 有什麼共性

裝了幾十個卸掉大部分之後，我總結出一個判斷標準：

**打開它的 [SKILL.md](http://skill.md/) ，如果核心內容你可以用一句話對 AI 說出來——那就不需要這個 Skill。**

比如「你是一個程式碼優化專家，請幫我優化以下程式碼」——這種 Prompt 你自己說就行了，不需要裝一個 Skill 來幫你說這句話。

**真正好用的 Skill，改變的是 AI 的工作流程和思維模式，不只是換一段 Prompt。** Superpowers 讓 AI 先想再寫，Taste Skill 讓 AI 在生成每一個 UI 元素時都過一遍設計規則，Graphify 讓 AI 先理解專案全局再回答問題——這些是「換一段 Prompt」做不到的。

---

## 最後

Skills 生態現在很像 2020 年的 VS Code 外掛——數量爆炸但品質參差不齊。看到「必裝 Top 30」別急著全裝。先裝一個 Superpowers 用一週，感受一下「AI 行為模式的變化」。如果覺得確實不一樣，再按需加其他的。

而且別忘了，這些 Skills 不只是 Claude Code 能用——Cursor、Codex、Gemini CLI 都支援。不管你用哪個工具，都值得試試。

**你裝了哪些 Skills？有沒有發現什麼小眾但特別好用的？** 評論區說說，有好的我加到列表裡。

---

原文出處： [https://juejin.cn/post/7657936630132883492](https://juejin.cn/post/7657936630132883492)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3V2O43) [20瀏覽](https://codelove.tw/@tony/post/3V2O43)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3V2O43)

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

📝17 💬3 ❤️4

689

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬5 ❤️3

137

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783385923&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2O43&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783385923713&amp;bpp=2&amp;bdt=170&amp;idt=38&amp;shv=r20260706&amp;mjsv=m202606300101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783049968%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783049968%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783049968%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3052335166421&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1599&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95395662%2C42533293%2C95393485&amp;oid=2&amp;pvsid=7785885799423281&amp;tmod=1849337775&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1599%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=63" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260706/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>