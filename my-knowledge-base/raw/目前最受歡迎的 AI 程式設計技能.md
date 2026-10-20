---
title: "目前最受歡迎的 AI 程式設計技能"
source: "https://codelove.tw/@tony/post/aGQ8Jx"
author:
published:
created: 2026-06-23
description: "## 簡介我一直覺得很誇張，有些 GitHub 儲存庫明明才在去年剛建立，星星數卻比外面一些最熱門的程式開發框架還多。以我寫這篇文章時來看，[Superpowers](https://github.com/obra/superpowers) 大約有 226,000 顆星，超前 Vue 的儲存庫，..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月23日](https://codelove.tw/@tony/post/aGQ8Jx "2026-06-23 08:30:10")

## 簡介

我一直覺得很誇張，有些 GitHub 儲存庫明明才在去年剛建立，星星數卻比外面一些最熱門的程式開發框架還多。以我寫這篇文章時來看， [Superpowers](https://github.com/obra/superpowers) 大約有 226,000 顆星，超前 Vue 的儲存庫，甚至遠遠超過 Next.js。另一個基於 Andrej Karpathy 編碼建議的儲存庫也有 174,000 顆星。而且它們甚至不是程式碼，只是一堆 markdown 檔案資料夾，告訴你的 AI 程式開發代理該怎麼把軟體寫得更好。

換句話說，這些就是代理技能（agent skills），而且在過去一年裡爆紅，成為最受歡迎的新一類開發者工具之一。這種格式最早出自 Anthropic：一個 `SKILL.md` 檔案，裡面有簡短描述和一組指令。現在它可以在 Claude Code、 [Kiro](https://kiro.dev/) 以及你其他喜歡的程式開發代理中運作。

這篇文章會帶你看幾個我覺得最有意思、而且大家每天都在用的技能。我這篇大多會用 Kiro 示範，但其實這些都能在你已經在用的任何代理上運作，只是各代理的安裝路徑不同。

本文涵蓋：

1. 代理技能到底是什麼
2. 幾個大型工作流程技能（Superpowers、Karpathy、Matt Pocock、GStack、GSD）
3. 框架技能與「隨套件附帶」模型
4. AWS 的 Agent Toolkit
5. 如何挑選一個並安全地試用

如果你願意，也可以直接看這個主題的 [完整影片](https://youtu.be/enWhBCyU36U?si=wySkthIQYr6592m5) ！另外，如果什麼都不看，至少也請看看 [AWS 的 Agent Toolkit](https://github.com/aws/agent-toolkit-for-aws) ！下面會有更多資訊。 *先坦白說明，我是 AWS 的開發者推廣人員*

## 什麼是代理技能？

技能就是一個目錄，裡面有一個 `SKILL.md` 檔案。這個檔案包含名稱、一行描述，以及指令。這些指令會說明要遵循哪些步驟、要呼叫哪些 API、要避免哪些錯誤，以及如何檢查結果。

技能應該只在需要時執行。你的代理會讀取簡短描述，判斷這個技能是否和你提出的需求相關，然後才載入完整指令。之後參考檔案才會按需載入。這樣你就算安裝了五十個技能，情境視窗也不會亂掉，因為代理只會拉進它需要的那兩個，其他的都忽略掉。 *實務上，我發現我還是得手動用 `/` 指令加上技能名稱，才能叫代理去執行技能。各家情況可能不同*

## 大型工作流程技能

大多數受歡迎、通用型的技能，其實都在修補代理常有的問題：走錯方向。代理如果放著不管，常常會急著動手、碰太多檔案，而且會跳過規劃。以下這五個技能採取了不同方式來讓它慢下來。

### Superpowers（obra/superpowers）

[Superpowers](https://github.com/obra/superpowers) 是這一類裡星星數最多的，也是最有主張的一個。它是一套完整的開發方法論，而且會自動觸發。它真的讓我想到一個超強版的規格驅動開發（Spec-Driven Development，SDD）工作流程。

當代理一看出你正在做某件事，它就會停下來，問你到底想做什麼。它會根據對話整理出規格，先顯示給你看（可選擇在瀏覽器中查看），等你核准之後才會寫實作計畫。接著它會把計畫交給子代理，由它們負責建置與互相審查彼此的工作。要注意，這會吃掉很多 token。

它可透過官方 Claude 外掛市集使用，也支援 Codex、Cursor、Gemini CLI，以及其他一些工具。你也可以透過 [Skills.sh](https://www.skills.sh/obra/superpowers/brainstorming) 讓它在 Kiro 上運作。不過如果你已經在用 Kiro，我大概會跳過它，因為 Kiro 裡面大部分的 SDD 已經有了。

如果你想要一套從想法到審查過的程式碼都包辦的系統，這個就是首選。在 Claude Code 裡，可從官方外掛市集安裝：

```
/plugin install superpowers@claude-plugins-official
```

### Karpathy 準則（multica-ai/andrej-karpathy-skills）

這一個幾乎是完全相反的做法。它是一個很短的行為規則檔，現在已經有 [174,000 顆星](https://github.com/multica-ai/andrej-karpathy-skills) 。雖然名稱是 Andrej Karpathy，但其實不是他本人寫的，這點我覺得真的很好笑。這是一個社群製作的檔案，源自 Karpathy 在一月發表的一篇文章，裡面列出 LLM 程式開發代理常見的失敗方式，後來這個儲存庫轉移到了 multica-ai 組織。

這些規則歸納成四個簡單概念：先思考再寫程式、保持簡單、做外科手術式修改，以及始終聚焦目標。如果你覺得 Superpowers 的流程太重，這就是一個低門檻的起點。它只是一個單獨的 `CLAUDE.md` （或 `AGENTS.md` ，也可以當作 Skill 使用），所以最簡單的安裝方式就是把它放到你的專案裡：

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/multica-ai/andrej-karpathy-skills/main/CLAUDE.md
```

老實說，就我自己的測試而言，我不太確定這東西值不值得。不過把它加進你自己的 `CLAUDE.md` 或 `AGENTS.md` 很容易，所以你不妨試試看，再形成自己的看法。

### Matt Pocock 的技能（mattpocock/skills）

[Matt Pocock 的技能集合](https://github.com/mattpocock/skills) 介於中間，並且明確反對那種一把抓的整套系統。事實上，他常說自己不喜歡 SDD，覺得太繁瑣了。他的觀點是：那種包辦整個流程的方法，也會拿走你的控制權，而且流程中的 bug 很難修。於是他的技能都很小、可組合，也很容易改造。你可以用 Vercel 的 `skills` CLI 安裝你想要的專案：

```bash
npx skills@latest add mattpocock/skills
```

他最受歡迎、也是我最喜歡的一個，是 `grill-me` ，它會在代理寫任何東西前，先像面試一樣問你正在做什麼。還有一個更強的版本 `grill-with-docs` ，除了同樣的訪談之外，還會為你的專案建立一份共享語言文件。這個儲存庫的主張是，這份共享語言文件能在不同 session 之間發揮效果。裡面還有 `tdd` ，用於紅綠重構迴圈； `diagnose` ，用於除錯；以及 `improve-codebase-architecture` ，用來拯救已經變成義大利麵條的專案。

另一個叫 `caveman` 的技能可以幫忙降低 token 成本。我覺得 `caveman` 和 `grill-me` 這兩個最實用。不過，當我只是想趕快把事情做完時， `grill-me` 用久了會有點煩。

### GStack（garrytan/gstack）

[GStack](https://github.com/garrytan/gstack) 是 Garry Tan 的 Claude Code 設定組合，星星數接近 110,000，是目前最受歡迎的技能集合之一。它不是只用一個模式包辦一切，而是給代理不同角色，每個角色都有自己的斜線指令。裡面有產品願景、設計師、工程經理、發布經理、文件工程師、QA，以及上線後回顧。它和 Superpowers 同樣屬於完整工作流程，但組織方式是圍繞軟體團隊中的角色，而不是一連串階段。如果你習慣用「誰來負責這一塊」這種方式思考，GStack 會非常貼近這種心智模型。安裝方式是 clone 下來再跑設定腳本（其他代理可傳入 `--host kiro` 、 `--host cursor` 等參數）：

```bash
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
cd ~/.claude/skills/gstack && ./setup
```

它也很適合在 Kiro 裡使用。

### GSD Core（open-gsd/gsd-core）

[GSD Core](https://github.com/open-gsd/gsd-core) （Git. Ship. Done.）是一套規格驅動系統，主打解決 context rot，也就是當你的情境視窗在長時間 session 中逐漸塞滿時，品質會慢慢下降的問題。它把工作拆成原子化的計畫，並在新的情境視窗中執行，讓主 session 保持輕量，而子代理負責重活。它支援十幾種以上的代理。這個專案從舊的 `gsd-build` 組織移到了現在的 `open-gsd/gsd-core` ，安裝程式會詢問你的 runtime：

```bash
npx @opengsd/gsd-core@latest
```

我在使用時發現它很快就把 token 用完了。我也不喜歡它幾乎接管整個工作流程。不過，還是值得試試看，看看它在你的個人專案上是否真的好用。

## 框架技能與「隨套件附帶」模型

上面的技能都不依賴特定框架。對前端開發者來說，更有意思的發展是：那些了解你特定技術棧的技能。

Vue 社群很早就開始走這條路。 [vuejs-ai/skills](https://github.com/vuejs-ai/skills) 是一個社群版 Vue 3 技能專案（README 裡提到作者可能會提案移交給 Vue 組織），其中的 `vue-best-practices` 技能是一套結構化工作流程，涵蓋 Composition API、響應式系統，以及你希望模型遵循的各種模式。 [Anthony Fu 的技能集合](https://github.com/antfu/skills) 則涵蓋 Vue、Vite、Nuxt、Pinia、VueUse 和 Vitest。這兩個都可透過同一個 CLI 安裝：

```bash
npx skills add vuejs-ai/skills
npx skills add antfu/skills --skill='*'
```

更新的概念則是把技能直接隨套件一起發佈。這裡有三件事值得知道：

- [skilld](https://github.com/skilld-dev/skilld) 會根據你已經安裝的 npm 相依套件，從文件、版本資訊與 GitHub issue 產生具備版本意識的技能。它解決了模型訓練截止日期導致它寫出你函式庫上一個版本的問題。
- [TanStack Intent](https://tanstack.com/intent) （ `@tanstack/intent` ）則是從維護者角度反過來。它讓函式庫作者可以在自己的 npm 套件中生成並發佈 `SKILL.md` 檔案，並隨每次版本更新而一起版本化，讓函式庫更新時，技能也會跟著更新。這是最近才推出的，並直接串接到 TanStack 生態系。
- [Vercel skills CLI](https://github.com/vercel-labs/skills) 是本文一直提到的 `npx skills` 工具，可以把這類技能安裝到 70 多種代理上。如果你使用 Kiro，它會自動從 `.kiro/skills/` 載入技能，不需要任何設定，這讓它成為在螢幕錄影或真實專案中試用新東西的簡單方式。

以下是這三者的指令：

```bash
# 根據你已安裝的 npm 相依套件，產生具備版本意識的技能
npx -y skilld

# 函式庫維護者：在自己的套件中建立 SKILL.md 檔案骨架
npx @tanstack/intent scaffold

# 本文一路使用的通用安裝器 —— 模式是 \`npx skills add <owner/repo>\`
npx skills add vercel-labs/skills
```

## AWS 的 Agent Toolkit

AWS 也有提供技能，而且同樣採用開放格式，所以可以在 Kiro、Claude Code、Cursor 和 Codex 中執行。

[AWS 的 Agent Toolkit](https://docs.aws.amazon.com/agent-toolkit/latest/userguide/skills.html?trk=1ad04439-1c50-4fdd-a845-d07d2655fe7a&sc_channel=el) 已於 2026 年 5 月公布，是把 AWS 專業知識帶進代理中的官方支援方式。它結合了 Claude Code 和 Codex 的外掛、一組精選技能，以及 [AWS MCP 伺服器](https://aws.amazon.com/blogs/aws/the-aws-mcp-server-is-now-generally-available/?trk=1ad04439-1c50-4fdd-a845-d07d2655fe7a&sc_channel=el) ，而後者已於 2026 年 5 月 6 日正式可用。這個工具包涵蓋部署、無伺服器架構，以及資料庫等領域。設定方式依代理而異。在 Kiro 中，你先透過 MCP 設定加入 AWS MCP 伺服器，然後再用 `npx skills` 把技能拉進來：

```bash
# 透過通用 CLI 安裝 AWS 精選技能
npx skills add aws/agent-toolkit-for-aws/skills
```

如果你是在 AWS 上開發，這個你應該要下載。就算你現在沒用 AWS，我也會建議先下載起來，留待未來使用。因為有了它，代理就能載入特定服務的指引，而不是憑記憶去猜一個只記得一半的 API。

## 如何挑一個並安全試用

就我個人而言，我會先用 Matt Pocock 的技能，再試試其他較大型的技能工作流程，例如 Superpowers。看看它是否適合你。然後也可以再搭配一個特定框架的技能。

**最後一件事，而且很重要。技能本質上就是把指令放進你的代理情境中，所以請把它當成 npm 相依套件來看待。安裝前先讀 `SKILL.md` 。一個有 100,000 顆星的儲存庫代表它很受歡迎，但星星只是受歡迎程度的訊號，不是品質審查。先快速看一下檔案，確認它會叫你的代理做什麼，再保留那些符合你實際工作方式的技能。**

## 結論

代理技能從 Anthropic 的一個低調格式，變成了星星數甚至超過我們所依賴框架的儲存庫，而這一切其實很合理。它們把一位好工程師腦中原本就有的模式，在每個任務開始時交給模型。大型工作流程技能提供結構，框架技能提供正確性，而 AWS 外掛則提供原本你得自己手動貼上的特定服務知識。

我的建議是：先安裝一個，讀看看它實際會對代理下什麼指令，然後看看輸出的結果是否和你自己本來會做的一樣。這才是真正的測試，而只有你能做這個測試。如果你想看這些實際怎麼運作，我會在搭配影片裡逐一帶你看。

---

原文出處： [https://dev.to/aws/the-most-popular-ai-coding-skills-right-now-4183](https://dev.to/aws/the-most-popular-ai-coding-skills-right-now-4183)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aGQ8Jx) [3瀏覽](https://codelove.tw/@tony/post/aGQ8Jx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aGQ8Jx)

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

516

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

33

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782175997&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaGQ8Jx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782175997172&amp;bpp=2&amp;bdt=88&amp;idt=37&amp;shv=r20260622&amp;mjsv=m202606150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782109119%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782109119%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782109119%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=8047432088176&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=42533293&amp;oid=2&amp;pvsid=3706457998251484&amp;tmod=1140211766&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=66" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260622/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>