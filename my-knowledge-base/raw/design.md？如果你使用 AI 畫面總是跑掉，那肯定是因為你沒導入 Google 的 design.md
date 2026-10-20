---
title: "design.md？如果你使用 AI 畫面總是跑掉，那肯定是因為你沒導入 Google 的 design.md"
source: "https://israynotarray.com/ai/20260627/1947382651/"
author:
  - "[[Ray]]"
published: 2026-06-27
created: 2026-06-29
description: "design.md 是 Google 從 AI 設計工具 Stitch 孵化、由 Google Labs 開源出來的格式規範，目的是讓 Claude Code、Cursor、GitHub Copilot 這類 AI Coding 工具能讀懂你的設計系統。這篇會聊一下 design.md 是什麼、它跟 CLAUDE.md / AGENTS.md 這類規則文件的差別、YAML Token + Markdown 設計理由的混合結構長怎樣、實際的範例（Heritage），以及內建 CLI 工具能做什麼（lint 含 WCAG 對比度檢查、diff、export 到 Tailwind v3/v4 或 W3C DTCG、spec），最後也會聊一下怎麼搭配 AI Coding 工具一起使用。"
tags:
  - "clippings"
status: processed
---
![design.md 是什麼？Google Labs 釋出讓 AI 看懂設計系統的格式](https://images.israynotarray.com/blog/ai/ai-desgin-md/ChatGPT%20Image%202026%E5%B9%B46%E6%9C%8825%E6%97%A5%20%E4%B8%8A%E5%8D%8810_42_14.png)

design.md 是什麼？Google Labs 釋出讓 AI 看懂設計系統的格式

> **Note**  
> design.md 是 Google Labs 釋出的格式規範，把設計系統拆成「YAML 設計 Token」與「Markdown 設計理由」兩層，讓 Claude Code、Cursor 這類 AI Coding 工具不只看到色碼、字級數字，還能讀懂背後的設計意圖。內建 CLI 可以做 lint（含 WCAG 對比度檢查）、diff、export 到 Tailwind 或 W3C DTCG 格式。目前版本是 alpha。

## 前言

你是不是常常遇到 Vibe Coding 之後，AI 版面總是歪一邊，然後你一直說「這邊間距跑掉、這邊顏色不一致」之後，AI 依然跑了一個不正確的畫面給你？如果有的話，那麼就是你沒有正確導入 Design System 的規則給 AI。

為了解決這件事情，所以這一篇就會來介紹 Google Labs 釋出的 design.md。這是一款 AI 設計系統規範，目的是讓 Claude Code、Cursor、GitHub Copilot 這類 AI Coding 工具能夠讀懂你的設計系統，產出一致的 UI，讓你不用一直說「這邊顏色不對、這邊字級不對」這種白癡問題。

> **Note**  
> 如果你還不熟悉 CLAUDE.md / AGENTS.md 這類給 AI 看的規則文件，可以先去看 [CLAUDE.md 與 Rules for AI 撰寫技巧](https://israynotarray.com/ai/20260310/3737444063/) 跟 [比 MCP 更省 Token 的 Agent Skills](https://israynotarray.com/ai/20260120/4189755271/) ，會更容易理解 design.md 為什麼會出現。

## design.md 的來歷：從 Stitch 孵化出來

那依照慣例，我們還是稍微了解一下 design.md 的歷史故事吧！

其實 design.md 是源自於 Google 旗下的 Stitch 的 AI 設計工具（2025 年首次在 Google I/O 大會亮相）。這是一個使用 Google Gemini 所驅動的 UI 設計工具，你可以使用一句文字描述（Prompt）或上傳草稿的方式來快速生成 UI 畫面（還包含前端）。

所以剛推出的時候，是真的滿猛的。

然後時間線大約到了 2026 年左右 Google 就把這個 design.md 格式以 Apache 2.0 方式釋出到 GitHub（ [google-labs-code/design.md](https://github.com/google-labs-code/design.md) ）。

什麼意思呢？其實這有點想要把 Stitch 的設計系統規則「標準化」的意思。

畢竟 Vibe Coding 開發者最容易遇到的痛點就是畫面不統一、風格不統一，但如果透過開源這一份設計規範的話，那麼在商業價值上就會非常大，因為你不論是使用 Claude Code、Cursor、GitHub Copilot，還是其他 AI Coding 工具，都可以透過這份規範來產出一致的 UI。

恩？你好奇開源這麼能帶來什麼商業價值？

舉例來講：

- 變成業界標準等於拿到定義權：你可以想像成當年的 OpenAPI 或 Kubernetes，剛開源的時候看起來人畜無害，可是當大家都開始使用之後，未來規範要往哪走、要新增什麼欄位、要支援什麼 Token 類型，Google 的聲音就會變成最大，畢竟它是老大。
- 讓其他 AI 工具自願變盟友：當 Claude Code、Cursor、Copilot 把「我支援 design.md」當成自己的賣點時，等於這些競爭工具都在替 Google 推這套格式，Google 連行銷費都不用花，超級省錢。
- 訓練資料的 flywheel：開源之後，大家會把自家專案的 design.md 放在 GitHub 公開的 repo 裡，這些檔案就可以讓 Google 拿來訓練「AI 生 UI」，這些結構化的設計 Token 加上人寫的設計理由剛剛好就是 AI 訓練資料的黃金組合。未來 Gemini 就可以拿這些公開資料來變得更強 ~~更有理由賣更貴的 Token~~ 。
- Stitch 的免費行銷漏斗：當開發者習慣使用 design.md 之後，當他要找一個「懂 design.md 的設計工具」時，第一個會想到的就是 Stitch，畢竟它就是 design.md 源頭。

所以某些大公司的開源看起來是虧錢，但其實是拿未來（時間）去換生態（生態系統）。Chromium、Kubernetes、TensorFlow 都是這種打法，表面上是貢獻給社群，實際上是用免費把標準先卡下來。

## design.md 想解決什麼問題？

所以…design.md 到底想解決什麼問題呢？其實就是…

> **讓 AI 產出來的 UI，每次顏色、字級、圓角都是一致的。**

這件事情非常非常重要，我相信大家在 Vibe Coding 應該都遇過這些狀況，請 AI 製作一個 Button，然後…

- 第一次它給你 `#3B82F6`
- 第二次給你 `#2563EB`
- 第三次直接給你 `bg-blue-500`

畫面看起來很正常也是藍色，但實質上他們根本不是同一個藍色，變成一個專案有各種藍色，搞得跟設計師手上的水彩調色盤一樣超級多種顏色。就連文字的字級也是差不多狀況，一下子使用 `1.25rem` 、一下子使用 `20px` 、一下子使用 `text-xl` 。

那這些問題就會導致畫面「不統一」，而畫面不統一就會導致使用者體驗不一致，進而影響品牌形象。

那到底為什麼會發生這件事情呢？最主要原因是出在你沒有給 AI 一份「設計系統規則」的文件。

也許會有人說：「那我寫成 CLAUDE.md 不就好了？」

確實是可以這樣做，但是你必須要知道 CLAUDE.md（AGENTS.md 也是一樣）是給 AI 看的「自然語言規則」，它沒有結構化的色票、Token 系統。AI 讀完還是要自己猜「Primary 是哪個 hex」「按鈕該配哪個圓角」，最終還是使用「猜」的方式。

所以 design.md 就是把這份「設計系統規則」變成一份 **機器能精準讀取 + 人類能讀懂理由** 的檔案。

## 所以 design.md 長什麼樣子呢？

前面都提到了「機器能精準讀取 + 人類能讀懂理由」的檔案，那這份檔案長什麼樣子呢？其實很簡單，design.md 就是一個 Markdown 檔案。

而結構部分可以區分成兩塊：

- **YAML Front Matter** ：放精確的設計 Token（顏色、字體、間距、圓角、元件）。
- **Markdown 內文** ：放設計理由 — 為什麼選這個顏色、什麼時候用、什麼時候不要用。
![YAML Front Matter(上) ](https://images.israynotarray.com/blog/ai/ai-desgin-md/20260625102119.png)

YAML Front Matter(上)

![+ Markdown 內文(下) 的結構](https://images.israynotarray.com/blog/ai/ai-desgin-md/20260625102151.png)

\+ Markdown 內文(下) 的結構

透過這個檔案內容的拆分，AI 不只知道「Primary 是 `#1A1C1E` 」，它還知道「Primary 是用來放在標題跟核心文字上的深墨色」。這樣 AI 在產生出 UI 時，就比較不容易出現「這個按鈕用錯顏色、這個文字用錯字級」的狀況。

### YAML Token 區塊

YAML 區塊大致長這樣：

```yaml
---
name: Heritage
colors:
  primary: "#1A1C1E"
  secondary: "#6C7278"
  tertiary: "#B8422E"
  neutral: "#F7F5F2"
typography:
  h1:
    fontFamily: Public Sans
    fontSize: 3rem
  body-md:
    fontFamily: Public Sans
    fontSize: 1rem
rounded:
  sm: 4px
  md: 8px
spacing:
  sm: 8px
  md: 16px
components:
  button-primary:
    backgroundColor: "{colors.tertiary}"
    textColor: "{colors.on-tertiary}"
    rounded: "{rounded.sm}"
---
```

挑幾個重點講：

- `colors` 、 `typography` 、 `rounded` 、 `spacing` 、 `components` 是五個主要的 Token 類別。
- Token 之間可以互相引用，例如 `button-primary` 的 `backgroundColor` 寫成 `"{colors.tertiary}"` ，這樣以後改 `tertiary` 顏色，所有用到它的元件都會跟著變。
- 顏色支援所有合法的 CSS 顏色字串：hex、rgb、hsl、oklch 都可以。

如果你用過 Tailwind 的 `tailwind.config.js` ，或 W3C 的 Design Tokens Format Module（DTCG），那你應該會特別熟悉。

### Markdown 內文區塊

YAML 結束的 `---` 後面，就是給人（跟 AI）讀的設計原因。

底下是官方推薦的章節順序：

- Overview（概觀）
- Colors（顏色）
- Typography（字級）
- Layout
- Elevation & Depth
- Shapes
- Components
- Do’s and Don’ts

只有 `Colors` 跟其中的 primary 顏色是 **必填** 的，其他章節要不要寫都看你的設計系統有沒有那塊。

舉個實際的例子，Colors 區塊可能會這樣寫：

```markdown
## Colors

Primary (#1A1C1E): Deep ink for headlines and core text.
Use on light surfaces to maximize legibility.

Tertiary (#B8422E): Warm rust accent reserved for primary actions
and brand moments. Avoid using as body text colour due to contrast.
```

這段文字就是再跟 AI 說「這個顏色叫 Tertiary、它是要拿來放在主要動作按鈕、不要拿去當內文顏色」。

## 竟然有 CLI ！？

沒錯，這個 design.md 還有一些超方便的工具，主要是幫你做四件事情：

- `lint`
- `diff`
- `export`
- `spec`

安裝後的使用方式也非常簡單：

```bash
npm install @google/design.md
npx @google/design.md lint DESIGN.md
```

ps.如果你是 Windows 系統，那 `.md` 可能會被檔案關聯吃掉，這時候需要搭配使用 `designmd` 這個 alias：

```bash
npx -p @google/design.md designmd lint DESIGN.md
```

那麼後面就來介紹每一個工具的用途吧。

### lint：驗證 DESIGN.md 結構

這個 `lint` 指令我相信身為一名開發者，應該會非常熟悉，畢竟 ESLint、Stylelint、Prettier 都是這種概念。

簡單來講， `lint` 會幫你檢查以下幾件事情：

- YAML schema 對不對
- Token 引用有沒有壞掉（例如你寫了 `{colors.tertiery}` ，少了個 a？！）
- 有沒有 primary 顏色
- WCAG AA 對比度有沒有過（這個我認為是最猛的）
- 有沒有定義了卻沒人用的 orphan token
- 章節順序符不符合推薦

像是 WCAG AA 那條規則，其實就是直接告訴你「 `on-secondary` 配在 `secondary` 上面對比度只夠 X.X:1，沒過 AA」，這就等於把無障礙檢查整合進設計系統文件裡了。

### diff：比較兩個版本的差異

這個其實滿有趣的，就跟 git diff 一樣，你可以比較兩個版本的 DESIGN.md 差異，使用方式也很簡單：

```bash
npx @google/design.md diff old/DESIGN.md new/DESIGN.md
```

這個工具會告訴你哪些 Token 改了、哪些被刪了、哪些是新增的。

這個主要用於，你替設計系統做版本升級，把 `primary` 從 `#1A1C1E` 換成 `#0A0A0A` ，然後想知道「所以整個專案有多少個元件會跟著變色」，那這個 diff 工具就會幫你列出所有受影響的 Token，讓你可以快速知道哪些元件要改。

### export：轉成其他格式

`export` 這個工具我個人認為滿猛的，尤其是你想把 design.md 當成「上游」，然後再轉成 Tailwind / CSS / DTCG 這些前端會用的格式。

| 格式 | 說明 |
| --- | --- |
| `json-tailwind` | 轉成 Tailwind v3 的 config 物件 |
| `css-tailwind` | 轉成 Tailwind v4 的 `@theme` CSS 區塊 |
| `dtcg` | 轉成 W3C Design Tokens Format Module |

使用方式：

```bash
npx @google/design.md export DESIGN.md --format json-tailwind > tailwind.config.js
npx @google/design.md export DESIGN.md --format css-tailwind > tailwind.css
npx @google/design.md export DESIGN.md --format dtcg > design-tokens.json
```

這好處在於你只需要專注於 design.md，然後 export 成 Tailwind / CSS / DTCG，這樣就可以讓前端、設計師、AI Coding 工具都用同一份 source of truth。

（這真的超猛的）

### spec：把規範丟給 AI

好，最後一個就是 `spec` 了，使用方式也很簡單：

```bash
npx @google/design.md spec
```

這個指令會輸出 design.md 自己的格式規範。

輸出完畢後，你可以把這一份 spec 塞給你的 AI Coding（如：Claude Code、Cursor、Copilot），告訴它「請按照這個規範來讀我的 DESIGN.md」，這樣 AI 就會知道 YAML 的 schema 該怎麼解、Token 引用語法是 `{path.to.token}` 、什麼是必填、什麼是選填。

加上 `--rules` 還可以一併把 linting 規則也輸出唷！

## 那要怎麼取得 DESIGN.md 呢？

如果你會好奇要怎麼取得 DESIGN.md，其實有幾種方式：

1. 手動建立，照著上面 YAML + Markdown 結構自己寫一份。
2. 從 [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) 挑一個自己喜歡的風格，把它的 `DESIGN.md` 直接丟到專案根目錄，再跟 AI 說：

> `請你依照 DESIGN.md 的規範來產生 UI 畫面`

這樣就可以了，是不是超級簡單？

## 怎麼搭配 AI Coding 工具使用？

那認識完了 design.md 的結構、CLI 工具之後，接下來就要聊一下怎麼搭配 AI Coding 工具使用吧？

畢竟 design.md 本身的設計目標就是「UI 設計系統變成可以持續理解一個設計系統」，所以使用方式也不複雜：

1. 把 `DESIGN.md` 放在專案根目錄。
2. 在 `CLAUDE.md` / `AGENTS.md` / `.cursor/rules/*.mdc` 裡面加一句：「請在做 UI 相關任務時優先讀 `DESIGN.md` ，並嚴格遵守裡面的 Token 與設計理由。」

對，就這樣，超簡單的吧。

但最後這邊有一個小提醒要注意，其實也算是老生常談了，也就是…

> **design.md 不要塞太多無關的東西**

因為它的設計理念是給 AI 一份「精煉過的設計知識」，這跟之前分享過的 [Agent Skills](https://israynotarray.com/ai/20260120/4189755271/) 是同一個方向，給 AI 重點不是給 AI 全部唷！

> **Note**  
> 目前 design.md 並沒有提供 Claude Code / Cursor / Copilot 的官方整合範例，只有 CLI 跟 TypeScript API。所以需要靠你在 CLAUDE.md / AGENTS.md / rules 裡面引導 AI 去讀 design.md。

## 結語

最後來總結一下吧。

所以 design.md 到底是什麼？

- 一份給 AI Coding 工具看的設計系統文件，格式是「YAML Token + Markdown 設計理由」。
- 解決的痛點是 AI 產 UI 時顏色、字級、間距前後不一致問題。
- 內建 CLI 可以做 lint（含 WCAG 對比度檢查）、diff、export 到 Tailwind / DTCG、輸出 spec 給 AI。

希望這一篇能幫助你解決「AI 產 UI 時顏色、字級、間距前後不一致」的問題，讓你在使用 AI Coding 工具時可以更順利～

你的支持會直接轉換成更多技術筆記

如果我的筆記讓你少踩一個坑、節省 Debug 的時間，  
也許你可以請我喝杯咖啡，讓我繼續當個不是 Array 的 Ray ☕

 [![buymeacoffee](https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png)](https://www.buymeacoffee.com/israynotarray)| [![line](https://israynotarray.com/images/line-images.png) ](https://store.line.me/search/zh-Hant?q=israynotarray)| [![portaly](https://israynotarray.com/images/logo.b91a6dbf.webp)](https://portaly.cc/israynotarray/support)

Terminal

ray — -zsh — 80x24

Welcome to Ray Terminal v1.0.0

Type "help" to see available commands.

\------------------------------------

➜ ~