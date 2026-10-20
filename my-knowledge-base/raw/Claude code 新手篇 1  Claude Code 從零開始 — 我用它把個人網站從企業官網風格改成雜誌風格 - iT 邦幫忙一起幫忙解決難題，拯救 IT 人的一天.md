---
title: "Claude code 新手篇 1 : Claude Code 從零開始 — 我用它把個人網站從企業官網風格改成雜誌風格 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10400090"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "這是「Claude Code 實戰手冊」系列的第一篇(G1)。如果你完全沒用過 Claude Code、甚至連 terminal 都不太熟，這篇就是給你看的。 1. 「我的個人網站感覺很無聊」 去年我..."
tags:
  - "clippings"
status: processed
---
2

## Claude code 新手篇 1: Claude Code 從零開始 — 我用它把個人網站從企業官網風格改成雜誌風格

- 分享至

這是「Claude Code 實戰手冊」系列的第一篇(G1)。如果你完全沒用過 Claude Code、甚至連 terminal 都不太熟，這篇就是給你看的。

## 1\. 「我的個人網站感覺很無聊」

去年我做了一個個人作品集網站。

Bootstrap 模板、深藍配亮黃、首頁放一張大頭照、底下用打字機效果輪播四個職稱:AI/ML Algorithm Engineer → Data Scientist → RAG System Architect → Full-Stack Developer。

技術上沒問題，但越看越覺得 —— 它沒突顯我做的事。

> 「無聊、普通、不好看、沒突顯我的特點。」

這是我自己給它的評語。

想重做但卡在三件事:**不知道從哪開始** 、 **設計能力不夠** 、 **寫 code 的速度跟不上想法** 。Vue 我會寫，但要從頭設計、修改成一個「不像企業官網」的東西，我每次坐下來就放棄。

這篇想記錄那個下午發生的事，以及我用的工具:Claude Code。

![https://ithelp.ithome.com.tw/upload/images/20260601/20182796rgq2Hxx5E2.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796rgq2Hxx5E2.png)  
![https://ithelp.ithome.com.tw/upload/images/20260601/20182796ACEFj3iXCI.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796ACEFj3iXCI.png)

---

## 2\. Claude.ai、Claude Desktop、Claude Code 差在哪

Anthropic 現在有四個主要的入口共用同一批模型(Opus / Sonnet / Haiku)，但介面跟能力差很多:

- **Claude.ai(網頁版)** —— 你最熟悉的那個，瀏覽器打開就用
- **Claude Desktop** —— 桌面 app，可以掛 MCP servers，但本質還是「對話 + 上傳檔案」
- **Claude Code** —— terminal 裡的 CLI，直接動你電腦上的檔案
- **Anthropic API** —— 不是給人聊天用的，是給你自己的 app 呼叫的(後端串接、自動化流程、自己包工具)

用網頁版 / Desktop 寫 code 的痛點，你大概都熟悉:

- 你要把 code 複製貼進對話框，它看不到你 **整個專案** 的結構
- 它給你的 code 你要自己複製貼回 editor、自己決定貼到哪個檔案
- 它沒辦法跑你的 code，所以也不知道有沒有錯
- 你改完一輪，下一次對話它什麼都忘了

Claude Code 解決的就是這四件事:

1. **它讀得到你的整個 repo** —— 不是只看你貼過去的一段 code，是整個資料夾
2. **它可以直接改檔案** —— 不用你複製貼上，它有 Read / Edit / Write 工具
3. **它跑得了 Bash** —— `npm install` 、 `git diff` 、跑測試，都會
4. **它會記得你的專案** —— 透過一個叫 `CLAUDE.md` 的檔案(下一篇會講)

**Claude.ai 跟你聊 code，Claude Code 幫你動 code。** 同樣的模型，工具集差很多。

跟 Cursor / VS Code Copilot 也不一樣 —— 那些是 editor 內建的 AI，Claude Code 本質上是 terminal 工具(雖然也有 VS Code extension 可掛)。  
![https://ithelp.ithome.com.tw/upload/images/20260601/20182796FX5mJYo3f9.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796FX5mJYo3f9.png)

---

## 3\. 安裝(假設你完全不熟 CLI)

如果你看到「Terminal」這些英文字就緊張，這節是幫助你的。

### Mac / Linux

1. 按 `⌘ + Space` 開 Spotlight，輸入 `Terminal` ，按 Enter，跳出一個黑色視窗就對了
2. 把這行貼進去，按下 Enter:
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

這是 Anthropic 官方推薦的安裝方式 —— **一行安裝完成、不需要 Node.js** 。等它跑完。

3. 啟動 Claude:
```bash
claude
```

### Windows

開 PowerShell(按 Windows 鍵搜尋 `PowerShell`)，貼這行:

```powershell
irm https://claude.ai/install.ps1 | iex
```

裝完一樣輸入 `claude` 啟動。

**注意 Windows 一個小陷阱**:Claude Code 在 Windows 上預設用 PowerShell 跑指令，如果你要跑 `npm run build` 之類的 Bash 指令，需要先裝 [Git for Windows](https://git-scm.com/download/win) (會附帶 Git Bash)。

### 其他安裝方式

如果你已經熟悉這些工具，也可以用:

- Homebrew(Mac):`brew install --cask claude-code`
- npm(舊版，但還能用):`npm install -g @anthropic-ai/claude-code`
- WinGet(Windows):`winget install Anthropic.ClaudeCode`

### 登入

第一次跑 `claude` ，它會要你登入。 **這裡有個重點:Claude Code 沒有免費試用** 。你需要:

- **方案 A — 訂閱方案**:Pro($20/月)、Max、Team、Enterprise 任一即可
- **方案 B — API key**:在 [console.anthropic.com](https://console.anthropic.com/) 申請，按 token 用量計費(沒有月費)

免費的 Claude.ai 帳號 **不能** 用 Claude Code，這是很多人沒有注意到的。

選哪個看你用量:每天都用 → Pro 划算。偶爾才用、寫單一專案 → API key 比較有彈性。

### 檢查裝好了沒

在 terminal(**不是 Claude Code session 裡面**)跑:

```bash
claude doctor
```

它會檢查環境、登入狀態、有沒有問題。出狀況時也是先跑這個。

### 你會看到什麼

第一次跑 `claude` ，你的 terminal 會變成這樣 —— 一個對話框，等你打字。

跟它打招呼:

```bash
hi, what can you do?
```

它會解釋自己的能力。 **這就是你的第一次對話** 。

![https://ithelp.ithome.com.tw/upload/images/20260601/20182796zJUWkVzYlo.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796zJUWkVzYlo.png)

順帶一提:第一次進入 **新的專案資料夾** 跑 `claude` ，它會先問你「這個資料夾你信不信任?」 —— 這是 Claude Code 的安全機制，避免它在你不熟的 repo 上亂跑指令。選 `1. Yes, I trust this folder` 就好。  
![https://ithelp.ithome.com.tw/upload/images/20260601/20182796o2GnfJHM0m.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796o2GnfJHM0m.png)

---

## 4\. 第一次任務:讓它看我的舊 portfolio

我的舊 portfolio repo 在 `~/Projects/個人/timwei0801/portfolio` 。

我先 `cd` 進去:

```bash
cd ~/Projects/個人/timwei0801/portfolio
claude
```

(`cd` 是 change directory，把 terminal 切換到那個資料夾。Claude Code 會用你 **當前位置** 當「專案根目錄」。)

進去之後我打第一句:

```ruby
這是什麼專案?風格上你覺得如何?
```

它回我(以下是 Claude Code 的回覆):

---

剛 Read 了一下 `Hero.vue` 、 `tailwind.config.js` 、 `package.json` 跟 `About.vue` ，大致掌握了。

這是個 Vue 3 + Vite + Tailwind 的個人作品集，9 個 sections，有 zh-TW / en 雙語 i18n，基本架構算紮實。

但風格上講老實話 —— 第一眼有點「 **企業內部簡報** 」的味道。 `tailwind.config.js` 那組深藍 `#1B2845` + 亮黃 `#FFC72C` 是很 corporate 的 palette，如果你想強調個人品牌、區隔出個人特質，這套色彩反而綁住你。

Hero.vue 裡幾個點想直接講:

1. **typewriter** 那段 30+ 行邏輯在跑職稱輪播 —— 這幾年在 dev portfolio 已經有點 cliché 了
2. 右上角 **"1+ Years Exp." badge** —— 你還在唸碩二，這個 framing 反而把你縮小了，你的賣點不是年資，是 research-to-production
3. 整體像 Bootstrap template 改色，card + section 的節奏很標準，不太 match 你寫 Medium 長文 + 在台大教課的 identity

要不要先告訴我:**這個站主要想打動誰?** HR、技術 hiring manager、還是潛在合作對象?方向會差很多。

---

—— 它整個打臉上一版的設計。 **但它確實提出了一些具體的問題點** 。

中間它沒有複製貼上、沒有要我給它看 code，它 **自己** 去讀我的檔案。這就是 Claude Code 的差別。

![https://ithelp.ithome.com.tw/upload/images/20260601/20182796AOTzLxqp7z.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796AOTzLxqp7z.png)

---

## 5\. 真正的重做:不是一個 prompt，是一輪對話

下面看起來像「一個 prompt 搞定」，其實 **我跟它聊了好幾輪** —— 風格參考、配色取捨、字體選型。下面是濃縮版，完整對話的進階用法會留到後面幾篇拆開講。

我開頭的 prompt 是這樣下的:

```markdown
我需要你先去幫我看一下我目前的個人網頁設計，主要是我覺得目前的設計
有一點太無趣。所以我需要你幫我去深入的看下，並且你可以去搜尋一下網路
上的一些大老或是專業人士，他們在做個人網頁的時候的一些好的想法，我們
可以一些討論。其中如果你有一些問題的話，也可以一起提出來討論。
```

注意我沒有給它「答案」，我給的是 **方向 + 對話權** —— 「你深入看」「你去搜」「我們討論」「你有問題也提」。Claude Code 拿到這種 prompt，不會直接動手改檔案，它會先 Read 一遍 repo、跑 Web Search，然後回來跟我對話。

(接續第 4 節它問「想打動誰」，我先回它:**主要想打 AI/ML hiring manager** 。)

然後它回我:

---

OK 我又翻了一遍 `Hero.vue` / `About.vue` / `Projects.vue` ，然後 WebSearch 了「AI engineer portfolio 2026」、「developer portfolio editorial design」這幾組 keyword。

看了幾個常被拿來當 reference 的站:`brittanychiang.com` (終端機風、單頁 scroll)、 `leerob.com` (Vercel design lead，極簡 + monospace + 文章為核心)、 `rauno.me` (editorial + 微動畫，設計感最強)、Josh Comeau(最有玩心、教學型)、還有 Stripe Press 跟 NYT tech 的版面。

把你的條件擺進去看 —— AI/ML + 寫 Medium 長文 + 教學經驗 —— 我覺得最 match 你的方向其實是 **leerob + rauno 的混合**:editorial 排版(serif 標題 + 大量留白 + 文章列在首頁)當骨架，加一點 rauno 那種克制的微動畫當質感。brittanychiang 那種 terminal 風太「前端工程師」了，跟你 research-to-production 的定位不對盤。

不過要繼續往下推之前，有幾件事想先問你:

1. 你願不願意把 Medium 那 4 篇直接 **embed 在首頁當主軸**?如果願意，整個 IA 會圍繞「寫作 + 專案」重組
2. 現在這組深藍 + 亮黃要不要留?還是直接換一套屬於你自己的 palette(我傾向後者)
3. 中英文版本要繼續同等地位，還是中文為主、英文當 fallback?這會影響字體選擇跟排版節奏

---

接下來是 **配色** 。這邊我學到一件事:

> 不要讓 AI 自己決定配色。

原因很單純，業界給它取了個名字叫 **「the purple problem」** —— Bolt、Lovable 這類 AI 網頁產生工具生出來的東西，十有八九是 **深藍紫漸層 + 螢光紫色 accent** 。原因是它們的訓練資料大量基於 Tailwind 預設 palette，加上 2023-2024 那波 Vercel / Linear / Anthropic 風的網頁，讓 AI 把「深藍紫」=「現代科技感」刻進神經了。

v0 自己在 2025-2026 演化過(現在預設偏黑底 minimalist)，但 Bolt、Lovable、其他多數 vibe coding 工具還沒擺脫紫色 default。Claude Code 沒固定 default，但如果你沒給 reference，它一樣會偏「sci-fi tech」那種方向。

![https://ithelp.ithome.com.tw/upload/images/20260601/20182796dVbV2Sp1lD.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796dVbV2Sp1lD.png)

所以我下配色 prompt 的時候，沒有問「你建議什麼?」 —— 我先去 [coolors.co](https://coolors.co/) 、 [Realtime Colors](https://www.realtimecolors.com/) 、Mobbin 翻了一輪，腦中有了一個喜歡的方向，然後用自己的話描述給 Claude Code:

```markdown
配色我想走深色底配一個暖色當重點，有點像在讀紙本雜誌或 editorial
部落格的那種感覺，不要走 v0 / Bolt 那種紫漸層的科技風。可以先做一版
簡單的初版讓我看效果嗎?
```

注意我下 prompt 的方式 —— **給方向、給比喻、給反例，但讓它先出一版我看** ，不是讓它自己決定也不是我自己挑色號。它直接動手改 `tailwind.config.js` ，跑 build，然後給我看結果:

---

初版我這樣配:

- 主底走「冷一點的深 + 一點青」—— 比一般 dark theme 的純黑或藍黑更有印刷紙的厚度
- 重點色用一個偏「鐵橘 / 燒紅」的暖橘 —— 像在看 AI 訓練 GPU 的熱力圖，跟你做 AI 的調性接
- 另外配一個 mint 綠當第二強調，用在「safe / governance」狀態，跟橘形成 warm/cool tension
- 文字色用米白(不是純白)，減少對比的刺眼感，看起來更像紙

已經改進 `tailwind.config.js` ，Hero.vue 也用新的 token 套了一個示範，你 reload localhost 看看。

---

我打開 dev server，看了一眼 —— **對，就是這個感覺** 。後面我才回頭去問它具體 hex code(`ink-800 = #0E1A1F` 、 `ember-500 = #FF6B35` 、 `mint-500 = #00D9A3` 、 `bone-100 = #F5F2EA`)，它寫進 `tailwind.config.js` 。

這個流程我學到一件事:**跟 AI 協作做設計，你要當 art director，不是 designer。** 給方向、給比喻、給反例，讓它先出一版，你用眼睛判斷。把「美感判斷」留在你這邊，把「色票生成 + 系統實作」交給它。

**字體** 它一次性拋三個出來:Fraunces italic 做 display(雜誌感)、Inter 做 body(中性、可讀)、JetBrains Mono 做 code / 標籤(技術感)。我看了 Fraunces italic 樣本一眼就同意，沒再爭。

整輪討論大概 30 分鐘，我只下方向性的問題，具體判斷大部分是它提的。然後它做了三件事，讓我覺得「 **這真的不是聊天工具能做到的** 」:

### a. 它先做 design system，再做頁面

我會直接動手改 Hero。但它先告訴我:

> 我建議先在 `tailwind.config.js` 定義一套新的色彩系統跟字型階層，後面所有 sections 都從這套規範出來。我會這樣做:
> 
> - 深色系列:ink-800 (#0E1A1F) 主底、ink-700 (#122229) surface、ink-900 (#0A1418) 最深
> - 重點色:ember-500 (#FF6B35) 橘
> - Display:Fraunces 襯線斜體
> - Body:Inter
> - Mono:JetBrains Mono

雜誌風常用襯線體做標題， **這個建議我自己想不到** 。

### b. 它建議用 italic serif 當主視覺

Hero 標題它建議用 Fraunces italic 做重點字:

```html
<h1 class="h-display text-display-xl">
  把 AI 從研究做到 <em>產品</em>
</h1>
```

那個 `<em>產品</em>` 在新的 design system 裡會渲染成大號的 Fraunces 斜體橘字。第一次看到 build 出來的成果，我才理解「editorial」是什麼感覺。

### c. 它跑 build 失敗，自己 debug 修好

中間有一段 i18n 字串它寫成 `chwei9181@gmail.com` ，vue-i18n 把 `@` 當特殊語法，build 直接炸。它跑了 `npm run build` ，看到錯誤，自己改成 `chwei9181{'@'}gmail.com` ，再跑一次 build，通過。

整個過程 **我沒插手** 。  
![https://ithelp.ithome.com.tw/upload/images/20260601/20182796i6Qmkv2NfI.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796i6Qmkv2NfI.png)

---

## 6\. 結果:一個 commit、27 個檔案

最後這個重做變成一個 commit:

```sql
commit db88a9c
feat: redesign portfolio v2 with Editorial Tech style
27 files changed, +1,834 -2,912
```

刪掉的比加的多，因為 v1 寫太囉嗦了。具體拆下來:

- 整段 Certifications section 折進 Skills(刪掉 171 行)
- Hero 從 30+ 行 typewriter 邏輯變成簡潔的 stats(數字 + label)
- 新增 6 個 Vue viz components(每個專案配一個互動視覺化)
- i18n(中英文)同步更新 800+ 行

對比一下 Hero 的改變:

> ⚠️ 圖片佔位 ⑥ — 上傳 `svg-4-v1-vs-v2-table.png` (v1 vs v2 對比圖)  
> 圖說:v1 EY 企業官網 vs v2 Editorial Tech — Hero 結構、Code 複雜度、視覺風格、個人化方式四個維度

|  | v1 舊版 | v2 新版 |
| --- | --- | --- |
| Hero 結構 | 打字機輪播 4 個 role + 大頭照 | 雜誌風 italic 標題 + 4 個量化 stats |
| Code 複雜度 | 30+ 行 typewriter JS 邏輯 | 簡潔的 computed stats |
| 視覺風格 | EY 深藍黃、企業官網感 | ink-800 深底 + ember 橘、雜誌風 |
| 個人化方式 | 「展示我做什麼」(role 列表) | 「展示我的成績」(quantified stats) |

對比 commit 訊息底下 Claude Code 自己寫的:

> QA review (recruiter + industry director personas) lifted the site from 79/100 to ~90/100.

(它跑了兩個 subagent persona 做 QA，但那是另一篇的故事 —— 後面 subagent 那篇會講。)

![https://ithelp.ithome.com.tw/upload/images/20260601/20182796dNA2u2Pae9.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796dNA2u2Pae9.png)

![https://ithelp.ithome.com.tw/upload/images/20260601/20182796qTsjW8sDXF.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796qTsjW8sDXF.png)

成果:[timwei0801.github.io/timwei0801](https://timwei0801.github.io/timwei0801/)

---

## 7\. 本課單字:四個重要且常用的指令

每篇文章結尾我會固定收錄幾個指令，就像國小英文課的「本課單字」 —— 看完整篇之後，這些是你可以 **立刻打進 terminal 試試看** 的。本篇是你的第一課，挑四個最常用的:

### /help

看所有指令清單。記不住、不確定有什麼功能，就 `/help` 。

### /model

切換模型。Opus 4.7 最強但最貴、Sonnet 4.6 平衡、Haiku 4.5 快又便宜。

我的習慣:

- 寫複雜架構決策 → Opus
- 日常 coding → Sonnet
- 簡單修字、改 typo → Haiku

(下一篇 G2 會深入講怎麼省 token，因為一不小心就會燒錢。)

![https://ithelp.ithome.com.tw/upload/images/20260601/20182796mnlTgav7nF.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796mnlTgav7nF.png)

### /clear

長對話跑久了會「鬼打牆」 — 它會被太多 context 卡住。 `/clear` 清掉重來，通常比繼續糾結省時間。

### /usage

看你這段時間用了多少 token、花了多少錢。第一週幾乎每天會打一次 —— 因為一不小心對話跑長就會嚇到。 `/usage` 是 **對成本有感的第一步** ，下一篇 G2 整篇都會圍繞它跟其他省 token 指令展開。

(順帶提醒:`claude doctor` 在第 3 節已經介紹過，那是 terminal 指令，不是 slash command。)

---

## 8\. 給純新手的三個建議

寫完 portfolio v2 之後，我給自己整理了三條給「剛開始用 Claude Code」的人的建議:

### a. 不要怕對話 —— 你不用一次寫完美 prompt

prompt engineering 教大家 **一次寫完美** 。但 Claude Code 是 **對話** ，不是 **一次性查詢** 。

第一個 prompt 寫得粗一點沒關係，看它怎麼理解，再補充。我重做 portfolio 那次，我的第一句只有兩行字。後面所有調整都是聊出來的。

### b. 看它寫的 code 你看不懂 —— 就直接問「為什麼」

它寫了 `class="reveal"` 加上一個 IntersectionObserver 我看不懂，我就問:

```markdown
為什麼用 IntersectionObserver 不用 AOS library?
```

它回了 5 行解釋:AOS 多一個 dependency、bundle 變大、效能差，native API 已經支援得很好。

**每次「我看不懂」都是一次免費的學習機會** ，不要錯過。

### c. 先設定預算上限，免得帳單嚇到你

如果你選 API key，記得到 [console.anthropic.com](https://console.anthropic.com/) 設一個 **monthly spending limit** 。預設沒有上限，跑長對話 / 大專案會無感燒錢。

我自己第一週就燒掉 $30 才意識到要設上限。下一篇 G2 會深入講怎麼省 token。

---

## 結語

Portfolio v2 從決定重做到上線，大概是 **一個下午** 。

不是因為我進步了，是因為 **我把不擅長的部分外包給 Claude Code**:設計系統、字型搭配、debug、跑 build、寫 i18n 重複的字串 —— 它做這些比我快十倍。我留下來做的是 **判斷**:這個方向對不對、這個對話要往哪走、什麼時候該停。

這就是 2026 寫 code 的樣子。不是 AI 取代你，是你跟 AI 分工。  
![https://ithelp.ithome.com.tw/upload/images/20260601/20182796Dk7ug11xtV.png](https://ithelp.ithome.com.tw/upload/images/20260601/20182796Dk7ug11xtV.png)

---

下一篇 **G2 — 模型選擇與省 token** ，因為你寫一寫會發現「咦，我這個月帳單怎麼這麼多?」。會教你 `/model` 、 `/effort` 、 `/usage` 、 `/context` 、 `/compact` 五個指令的搭配，以及我自己的省 token 工作流。

---

## 系列文章

- **G1(本篇)** Claude Code 從零開始 — Portfolio v2 重做
- G2 模型選擇與省 token(即將發布)
- G3 CLAUDE.md — 讓 Claude 真的「懂」你的專案
- G4 Memory 系統 — 讓 Claude 跨 session 記得你

> 完整 20 篇地圖會放在系列首頁。本系列同步發佈於 Medium。

---

**關於我**

Tim Wei / AI/ML Engineer @ EY Taiwan，專注於 Responsible AI、金融業 ML 落地。也在台大擔任企業導師。

歡迎在留言區跟我討論，或在 [LinkedIn](https://www.linkedin.com/in/timwei0801) 上找我。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1779447171_6a10358375783.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2605B9004)