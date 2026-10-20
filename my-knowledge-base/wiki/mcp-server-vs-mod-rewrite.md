---
title: 同一需求寫兩次：MCP server 與 Claude Code Mod 的差別在「站在哪裡」，不在省 token
date: 2026-10-08
tags: [ai/mcp, ai/claude, type/comparison]
source: https://medium.com/@n913239/一個月前我做了一個-mcp-server-這週我用-claude-mods-把它重寫了一遍-8d7c87f341d7（MikeWang，Medium，2026-10-05；Claude Code 系列，前作見相關連結）
type: article
status: processed
asset_value: 75
---

> 📅 **版本與日期**：作者記載 Mods「**2026-10-02 隨 Claude Code 2.1.287 落地、預設開啟**」；[[claude-code-mods-overview]] 記為「**2026-10-01 官方公告**」。前者是發布日，後者是公告日，兩者並列。官方明說 Mods API 是 **early access、會隨版本變**；本篇全部跑在 **2.1.289**，以你機器上的宣告檔為準。

作者一個月前把發文前檢查清單做成 MCP server（見 [[self-built-mcp-server-237-lines]]；該 server 後來增至 **307 行、九項檢查**，是後續增加，不是矛盾），這週把同一件事改寫成 Mod：**277 行**，之後拆成兩支（257 行＋126 行）。

## T（核心論點）

> **MCP server 給 Claude 工具，要被叫才會動；Mod 改 Claude Code 本身，常駐，而且不必先驚動模型。兩者不是同一類東西，差別在「站在哪裡」，不在省多少 token。**

- **MCP server**：給 Claude 工具。要被叫、會花 token，但跨 app 通用、任何語言都能寫。
- **Mod**：改 Claude Code 本身。能畫畫面、能常駐、能註冊直接跑函式的指令，但只在這一個產品裡。

作者原本打算拿「省 token」當賣點，實測後自己推翻：

- MCP 工具定義的常駐成本**只有 174 tokens**，`/context` 顯示 **0.0%**（`list_articles` 是 deferred，用到才載入）。
- 回傳格式：MCP 的 JSON 每項 **104 字元**（2,083 字元／20 項），Mod 的格式化文字每項 **53 字元**（737 字元／14 項）。一個月跑約 30 次檢查，差距約 2 萬 token；對照單一 session 就用掉 45 萬，**這是雜訊**。
- 真正的差別只有兩個：**「一直在」**（band 常駐畫面，不進 transcript）＋**「不必先驚動模型」**（省的是一輪推論，不是幾個 token）。用作者的話說：省 token 不是理由，**不必開口**才是。

> 🔴 **原文總結段與正文矛盾，本庫採正文。** 文末總結寫「mod … 不用被叫，**不花 token**」，但正文已自我更正：
> - `ui.render` 畫的 band **不進 transcript**、模型看不到，這部分才是真正零 token；
> - `command.run` 回傳的 `text` 依宣告檔是「the command's output as a transcript line」，模型讀得到，所以 **mod 指令不是零 token**。
>
> ⚠️ 正文另有一句「MCP 的工具 schema 每一輪都在 context 裡」，也和作者自己量到的 174 tokens／deferred 載入有落差；本庫以實測數字為準。

## B（適用場景）

**適用：**
- 已經有一支 MCP server 或腳本在做「隨時想問一句現在合格了嗎」的檢查，想判斷要不要改成 Mod
- 想讓狀態常駐在畫面上，不用每次開口叫模型
- 要安裝別人的 Mod，想先看懂它的能力範圍

**不適用 / 注意：**
- ⚠️ **樣本是單一作者、單一用途**（發文前檢查），174 tokens 與每項字元數是他的工具與他的文章。
- ⚠️ 要跨 app 共用、或要用 Python 等其他語言寫，MCP 仍是對的選擇；Mod 只活在 Claude Code 裡。撰寫語言：作者寫 JS／TS 皆可（範例檔 `register.tsx`），和數位時代轉述的官方定義「TypeScript 小函式」說法不同，見 [[claude-code-mods-overview]]。
- ⚠️ **畫面只在終端機與桌面版畫得出來**；hook 幾乎到處都跑，但 UI 元素由 `$.ui.resolve(e)` 依當下介面提供。
- 🔴 **Mod 沒有 sandbox**：官方文件自己列，它能讀寫你帳號能碰的任何檔案、讀環境變數與設定檔（含 API key）、看見並改寫每個 prompt 與 tool call、**替你核准 tool call**、花你的額度叫模型。

## R（操作步驟）

### 1. 最小 mod＝3 個檔

```text
medium-band/
├── .claude-plugin/plugin.json     # 外掛 manifest
└── hooks/
    ├── hooks.json                 # { "modules": ["./register.tsx"] }
    └── register.tsx               # 真正的程式
```

`register.tsx` 匯出 `register(on)`，以 `on(事件, 條件?, hook)` 註冊，hook 簽章是 `($, e, next)`：

- `$`：引擎介面，要做自己程式碼以外的任何事都得經過它（`$.fs.read`、`$.ui.resolve`、`$.process.run`…）
- `e`：事件內容，凍結的純資料
- `next(e)`：往下跑其他 mod，再跑引擎本身

所以 hook 只有三種選擇：**放行**（`return next(e)`）、**改寫後放行**（`next({ ...e, ... })`）、**自己答**（不呼叫 `next`）。

### 2. 熱重載的開關只有人能按（作者實測）

寫下第一個檔案時畫面會問要不要熱重載，並註明「They run with your permissions.」。這個開關**不吃 permission mode、不吃 settings 規則、也不吃任何 hook**，只有坐在鍵盤前的人能答。作者按 `Not now` 後，引擎回報模型「Do not say the change is running.」，主動防止模型謊報已生效。

### 3. 兩個 MCP 做不到的能力

| 能力 | 做法 | token 成本 |
|---|---|---|
| **一直在** | `on('ui.render', { component: 'AbovePrompt' }, …)` 在輸入框上方畫一條檢查結果，每次寫完檔重算 | **零**（band 不是 transcript row） |
| **不必先驚動模型** | `session.start` 時 `$.command.register` 註冊 `/check`，`command.run` 直接跑函式；Claude 正在忙也能按 | 輸出**會**進 transcript，但省掉「模型判斷要叫哪個工具」那一輪推論 |

### 4. 慢的地端模型塞得進 hook

hook 的時間預算是 10 秒，但宣告檔寫明：等 `next(e)` 或任何 `$` 呼叫時，**時鐘是停的**（作者讀宣告檔得出，宣告檔自己舉的例子）。所以 `$.process.run` 跑 LM Studio（5–20 秒／次）不會超時。作者用它做 `/check-drift`：逐節把中英版交給本機模型比對語意，一次 1–3 分鐘；用地端的理由是「草稿不該離開這台機器」。

### 5. 出錯時引擎怎麼處理

- hook 拋錯**不會弄垮 session**：引擎跳過該 hook，在 transcript 留一行指名哪個 mod、哪個事件、什麼原因。
- 作者踩到的坑：把 async 函式直接交給 `update`，存進 `$.state` 的是 Promise 而不是資料。
- **介面安靜就等於在說「沒事」**：「兩邊都有才比對、沒有就不說」和「不認得的參數默默忽略」都是同一個毛病，band 與指令是介面，要一律回報原因（例如「跳過，還沒有英文版」「不認得的參數：deift」）。

### 6. `claude plugin validate`：不執行就列出能力清單

```text
$ claude plugin validate ./medium-band
❯ hooks: session.start, command.run{command=check}, tool.call, ui.render{component=AbovePrompt}
  ❯ calls: $.command.register, $.fs.list (via currentArticle), $.fs.read (via readLines), …
✔ Validation passed
```

**為什麼這張清單可信**：引擎強制 `$` 只能流進**檔案最上層宣告的函式**（closure 寫法會被拒收），所以「這個 mod 會呼叫什麼」可以靜態窮舉。這是拿寫法的自由換來的保證。

### 7. 🔑 能力最小化拆分

作者用 validate 抓到自己：原本一支 mod 掛在 `tool.call`（每次寫檔都跑），又握有 `$.process.run`。於是拆成兩支：

- `medium-band`（257 行）：常駐，掛 `tool.call` 與 `ui.render`，能力只有讀檔、讀 state、畫畫面
- `medium-drift`（126 行）：只掛一個 `command.run`，握有 `$.process.run`，但只在按 `/check-drift` 時醒來

> **常駐的不具開 process 的能力，能開 process 的不常駐。**

代價是兩支各有一份約 40 行的 markdown 解析；作者刻意不用 `dependencies` 共用（那是設計來共用 `$` 能力的）。

### 8. 參考數字與未驗證入口

- 宣告檔約 100 個事件；2.1.289 的 `types/claude-code.d.ts` 為 20,354 行（在 session 裡載入 `plugin-authoring` skill 時由引擎產生）。
- 官方 sample：`token-weather` 122 行、`replay-theater` 249 行、`blast-radius` 528 行；`/diff` 本身就是一個 mod。
- **作者未驗證**、但宣告檔裡存在的入口：`$.tool.register`（不寫 MCP 也能給 Claude 新工具）、`prompt.compose`（動態改 system prompt）、`session.append`、`$.model.fork`。

## C（關鍵行動）

- [ ] 決定 MCP 還是 Mod 時，問「它需要一直在嗎？需要先叫模型嗎？要跨 app 嗎？」，不要用「省 token」當理由
- [ ] 想知道 MCP 工具定義實際吃多少，先跑 `/context` 看逐工具數字，再下結論
- [ ] 寫或裝 Mod 前跑 `claude plugin validate`，檢查**常駐的 hook 有沒有握 `$.process.run` 這類高權限能力**；有就拆
- [ ] Mod 的 band 與指令一律回報「為什麼沒跑」，不要讓介面安靜
- [ ] 記下自己跑的 Claude Code 版本；Mods API 仍是 early access

## 原始摘要

MikeWang 把發文前檢查從 MCP server（307 行、九項檢查）改寫成 Claude Code Mod（277 行），結論是兩者不是同一類東西：MCP 給 Claude 工具、要被叫；Mod 改 Claude Code 本身、常駐且不必驚動模型。實測推翻「省 token」賣點（工具定義 174 tokens、0.0%；每項 104 vs 53 字元；一個月約差 2 萬 token），並示範 `claude plugin validate` 靜態列出能力清單、據此把一支 mod 拆成「常駐但不能開 process」與「能開 process 但不常駐」兩支。Medium 訂閱框不收。

## 相關連結

- [[self-built-mcp-server-237-lines]] — 前作：237 行 MCP server 與「脈絡／能力／強制」判準；本篇在其外補上「改 App 本身」這一格
- [[claude-code-mods-overview]] — Mods 是什麼、怎麼裝、安全邊界（日期、語言說法、sec-default 並陳）
- [[claude-code-mechanism-division-series]] — 同作者系列：四機制分工
- [[claude-code-hooks-memory]] — 設定檔 Hooks（同作者系列；與 Mods 的 hooks 不同物）
- [[agent-skills-vs-mcp]] — Skills vs MCP 的選擇
- [[ai-skills-mcp-plugins-agents-comparison]] — Skills／MCP／Plugins／Agents 定位比較
- [[claude-code-context-management]] — `/context` 與 context 成本（本篇 174 tokens 實測）
- [[agent-permission-and-os-sandbox]] — Mod 無沙箱、以你的身分執行；能力最小化的同類思路
