---
title: "AI 時代 Harness 工程在後台管理系統中的實踐 🌈"
source: "https://codelove.tw/@tony/post/anb7Qa"
author:
published:
created: 2026-07-14
description: "AI 時代 Harness 工程在後台管理系統中的實踐 🌈  ================================    Hello，我是 pany，好久不見～我是開源專案 [V3 Admin Vite](https://link.juejin.cn?target=https%..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月13日](https://codelove.tw/@tony/post/anb7Qa "2026-07-13 16:45:09")

## AI 時代 Harness 工程在後台管理系統中的實踐 🌈

## AI 時代 Harness 工程在後台管理系統中的實踐 🌈

```
Hello，我是 pany，好久不見～
```

我是開源專案 [V3 Admin Vite](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fun-pany%2Fv3-admin-vite) / [MobVue](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fun-pany%2Fmobvue) 的作者，這是我為提升它們的 `AI Coding` 體驗而進行的第二期實踐，旨在 **努力成為 AI 友善的 Vue3 模板**

早在一年前，我寫了第一期採用 Rules 的實踐方案 [第一款 AI 友善的開源 Vue3 模板 🌈](https://juejin.cn/post/7526575166383505458) ，但是隨著 AI 的發展，這已經遠遠不夠了，所以需要在此基礎上繼續提升 AI Coding 體驗！

讓我們一起來看看如何實現這個想法

## 背景

程式設計師群體的 AI Coding 覆蓋率已經接近 100%，雖然 AI 愈來愈強大，但是在讓 AI 針對特定專案進行編碼時，依舊容易出現輸出程式碼不理想的情況

為了減少這種情況的發生，V3 Admin Vite 開源專案在既有 Rules 的基礎上，新增專案級約定和 Skills，以確保更可靠的 AI 輸出

## 目標

完成專案級約定和 Skills 內建，讓 AI Agent 更懂我們的專案、輸出理想的程式碼

## 前置知識

> 這部分內容參考自 Claude Code 文件 [features-overview](https://link.juejin.cn/?target=https%3A%2F%2Fcode.claude.com%2Fdocs%2Fzh-CN%2Ffeatures-overview)

你需要先懂得區分它們三者：

**方面\*\*** [CLAUDE.md](http://claude.md/) ****Rules**** Skill **\*\*載入** 每次會話每次會話，或開啟匹配的檔案時按需，被呼叫或相關時 **範圍** 整個專案可以限定在檔案路徑範圍內特定的任務 **最適合** 核心約定和建構命令特定語言或目錄的指南參考材料、可重複的工作流程 **何時使用** 專案約定、「始終執行 X」規則/可重用內容、參考文件、可重複的任務 **範例** 使用 pnpm，而不是 npm；提交前執行測試。/ `/deploy` 執行部署檢查清單；熟悉 API 文件和端點模式 **總結**

- **對於每個會話需要的說明，使用 [CLAUDE.md](http://claude.md/)** ：建構命令、測試約定、專案架構
- **使用 Rules 來保持 [CLAUDE.md](http://claude.md/) 專注：** 帶有 paths frontmatter 的 Rules 只在 Claude 處理匹配的檔案時載入，節省上下文
- **對於 Claude 有時只需要的內容，使用 Skills：** 如 API 文件或您使用 `/<skill-name>` 觸發的部署檢查清單
- **[CLAUDE.md](http://claude.md/) + Skills 的組合：** [CLAUDE.md](http://claude.md/) 保存始終開啟的規則，Skills 保存按需載入的參考材料。例如 [CLAUDE.md](http://claude.md/) 裡說「遵循我們的 API 約定」，Skill 則包含完整的 API 風格指南

**翻譯翻譯**

- **[CLAUDE.md](http://claude.md/) ：** 每次都生效的通用約定
- **Rules：** 每次但局部生效的規則
- **Skills：** 按需並且局部場景固定的執行步驟

## 本期實現

### 新增專案級約定

> Claude Code 文件將 [CLAUDE.md](http://claude.md/) 解釋為專案上下文和約定
> 
> 並規定了將其內容保持為 Claude 應該在每個會話中保持的事實，比如建構命令、約定、專案架構、「總是做 X」規則，如果一個條目是多步驟過程或僅對程式碼庫的一部分重要，需將其移到 Skill 或 Rules 中

為了更好地讓 AI 理解專案、遵守約定，所以新增 [AGENTS.md](http://agents.md/) 、 [CLAUDE.md](http://claude.md/) 兩個根目錄配置檔案，具體內容需包含以下模組（保持內容在 200 行以內）：

- 命令
- 架構
- 約定

在實踐中，我選擇在開源專案 [andrej-karpathy-skills](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fforrestchang%2Fandrej-karpathy-skills%2Fblob%2Fmain%2FCLAUDE.md) 的基礎上完善以上三個模組

*這個開源專案透過簡單的四條原則來有效約束 AI 的輸出和避免 AI 的陷阱，在實踐中非常有效！*

值得注意的是：

- `CLAUDE.md` 檔案作用於 `Claude Code` 工具
- `AGENTS.md` 檔案作用於 `Cursor` 、 `Antigravity` 、 `Trae` 、 `CodeX` 等工具

所以我們優先輸出更加通用的 [AGENTS.md](http://agents.md/) 檔案，然後將 [AGENTS.md](http://agents.md/) 與 [CLAUDE.md](http://claude.md/) 關聯，避免撰寫兩份一模一樣的約定檔案

### 新增專案 Skills

> Claude Code 文件將 Skill 定義為可重複使用的知識和可呼叫的工作流程

通常，你可以選擇 [skill-creator](https://link.juejin.cn/?target=https%3A%2F%2Fskills.sh%2Fanthropics%2Fskills%2Fskill-creator) 這個官方工具來協助你完成你想要的 Skill，它比自己從零開始編寫效率更高

如果你想仔細地學習如何寫好一個 Skill，可以閱讀： [如何寫好一個 Skill](https://link.juejin.cn/?target=https%3A%2F%2Fdocs.trae.cn%2Fide%2Fbest-practice-for-how-to-write-a-good-skill)

在實踐中，我們先將編寫好的 Skill 存放在更加通用的 `.agents/skills/` 目錄下，然後與 `.claude/skills/` 關聯（關聯方案採用軟連結命令： `ln -s` ，目的依舊是避免撰寫兩份一模一樣的檔案）

針對 **V3 Admin Vite** 我們首期新增了以下專案 Skill（並嚴格保持每個 Skill 內容在 500 行以內）：

- `v3-create-crud` ：建立新增、刪除、修改、查詢頁面
- `v3-upsert-store` ：建立或更新全域狀態
- `v3-upsert-route` ：建立或更新路由表
- `v3-use-utils` ：內建工具使用教學
- `v3-use-composables` ：內建 Composition API 函式使用教學

將這些後台管理系統中高頻出現的需求抽象成 Skill 以後，AI Agent 能更加準確地產出你想要的程式碼，並準確地實現你的需求

### 安裝社群 Skills

除了專案 Skill，專案還引入了社群裡對撰寫 Vue 專案非常友善的第三方 Skill，它們包括但不限於：

- `pinia`
- `pnpm`
- `unocss`
- `vite`
- `vitest`
- `vue`
- `vue-best-practices`
- `vue-router-best-practices`
- `vue-testing-best-practices`

它們主要來自於儲存庫： [antfu/skills](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fantfu%2Fskills)

這些第三方 Skill 以 `skills-lock.json` 檔案的形式記錄，Skill 本身並非預設包含在專案中。如果有需要，你可以執行 `npx skills experimental_install` 命令將它們安裝到本地即可。安裝後可在 `.agents/skills/` 目錄下找到它們

## 實踐總結

至此，我們階段性實現了標題提到的 **AI 時代 Harness 工程在後台管理系統中的實踐**

[![AI友好架構圖.png](https://i.imgur.com/KJFMaVA.jpeg)](https://i.imgur.com/KJFMaVA.jpeg)

基於這套方案並配合軟連結的方式，目前已支援了 `Claude Code` 、 `CodeX` 、 `Cursor` 、 `Trae` 、 `Antigravity` 等主流 AI Coding 工具

但，隨著 AI Agent 的快速發展，未來可能還會有更多不一樣的實踐方案，讓我們共同期待～

## 原始碼

- 直達約定檔案： [AGENTS.md](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fun-pany%2Fv3-admin-vite%2Fblob%2Fmain%2FAGENTS.md)
- 直達技能目錄： [Skills](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fun-pany%2Fv3-admin-vite%2Ftree%2Fmain%2F.agents%2Fskills)
- 直達規則目錄： [Rules](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fun-pany%2Fv3-admin-vite%2Ftree%2Fmain%2F.cursor%2Frules)

取得開源專案完整程式碼： [V3 Admin Vite](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fun-pany%2Fv3-admin-vite) / [MobVue](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fun-pany%2Fmobvue)

---

原文出處： [https://juejin.cn/post/7660780385584316454](https://juejin.cn/post/7660780385584316454)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/anb7Qa) [3瀏覽](https://codelove.tw/@tony/post/anb7Qa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/anb7Qa)

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

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Anthropic 為何限制中國大陸使用 Claude？](https://codelove.tw/@tony/post/qO2P8a)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝17 💬3 ❤️5

738

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝3 💬6 ❤️4

167

🥉

[流引色群@jptd847utpp](https://codelove.tw/@sp9863)

1

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1784013748&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanb7Qa&amp;pra=5&amp;asro=0&amp;itsi=6&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1784013748195&amp;bpp=1&amp;bdt=69&amp;idt=28&amp;shv=r20260713&amp;mjsv=m202607070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1784013358%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1784013359%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1784013359%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=7286325398598&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=4&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1676&amp;bih=1003&amp;scr_x=0&amp;scr_y=2087&amp;eid=31099566%2C31099683%2C95396101%2C42533294%2C95396252&amp;oid=2&amp;pvsid=3601393943326173&amp;tmod=609401492&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1916%2C1050%2C1676%2C1003&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=32" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260713/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>