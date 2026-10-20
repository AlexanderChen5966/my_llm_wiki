---
title: "DeepSeek Harness 是抄 pi 的嗎？我 grep 完 194 個套件，然後看懂了它的架構"
source: "https://hackmd.io/@BASHCAT/ByB4Fv6UGl"
author:
published:
created: 2026-08-17
description: "DeepSeek Harness 是抄 pi 的嗎？我 grep 完 194 個套件，然後看懂了它的架構"
tags:
  - "clippings"
status: "processed"
---
!\[\[DeepSeekHarness-架構拆解主視覺.jpg\]\]

DeepSeek Harness 發布不到 24 小時，Hacker News 上那串 \[662 分、276 則留言的討論\]([https://news.ycombinator.com/item?id=49285244)](https://news.ycombinator.com/item?id=49285244%29) 裡就開始有人把它跟 Pi、Claude Code、Codex、Letta 排在一起比。比一比很正常，但「排在一起」很快就變成另一種說法：這東西是不是照著 pi 抄的？

畢竟證據看起來很硬——你 `npm ls` 下去，會看到 DeepSeek Harness 的依賴樹裡明明白白躺著一個 `@earendil-works/pi-ai` ，那正是 Mario Zechner（badlogic）的 [pi harness](https://github.com/badlogic/pi-mono) 家族套件。名字都印在那了，還需要吵嗎？

需要。因為「依賴了某個套件」跟「原始碼相近」是兩件完全不同的事，而這兩件事之間的距離，剛好可以用三個指令量出來。

我把整棵安裝樹翻了一遍。結論可能跟你想的不一樣，而翻的過程順便讓我把 DeepSeek Harness 的架構看懂了——那才是這篇真正想講的東西。

> \[!info\] 環境與前置條件
> 
> - 實測版本： `@deepseek-ai/dsh@0.1.0-rc.6` （透過 npm 全域安裝於 `/opt/homebrew/lib/node_modules/` ）
> - 實測日期：2026-08-14，DeepSeek Harness v0.1 developer preview 發布次日
> - 對照套件： `@earendil-works/pi-ai@0.82.1`
> - 需要的前置知識：npm 依賴解析、基本的 agent harness 概念（工具呼叫、session、system prompt）
> - 官方明講「THERE WILL BE COMPATIBILITY-BREAKING CHANGES」，本文所有細節以你查閱時的版本為準

---

## 先把血緣鑑定做完：三個指令的事

!\[\[DeepSeekHarness-單點依賴血緣鑑定.jpg\]\]

判斷一個開源專案有沒有「抄」另一個，看依賴清單是最沒用的做法。依賴清單只告訴你「用了」，不告訴你「用得多深」。真正該問的問題是： **這個依賴的擴散半徑有多大？**

### 證據一：擴散半徑是一個檔案

先看有幾個套件宣告了這個依賴：

```bash
DSH=/opt/homebrew/lib/node_modules/@deepseek-ai/dsh
grep -Rl 'earendil-works/pi-ai' $DSH --include=package.json
```

輸出：

```
node_modules/@earendil-works/pi-ai/package.json          ← pi-ai 自己
node_modules/@deepseek-ai/dsh-llm-pi-ai/package.json     ← 唯一的 DSH 消費者
```

再看有幾個檔案真的 import 了它：

```bash
grep -Rl 'earendil' $DSH/node_modules/@deepseek-ai --include='*.js'
```

輸出：

```
dsh-llm-pi-ai/lib/index.js
```

一個。整棵安裝樹裡有 **194 個 `@deepseek-ai/*` 套件** （其中 185 個是 `dsh-*` 開頭的外掛），提到 pi 的只有一個檔案。

順帶一提， `@earendil-works` 這個 scope 底下也只裝了 `pi-ai` 一個套件。pi 的 agent runtime（ `pi-agent-core` ）、CLI（ `pi-coding-agent` ）、TUI 函式庫（ `pi-tui` ）、telemetry 契約（ `pi-telemetry` ）—— [官方 README 表列的其他四個核心套件](https://github.com/badlogic/pi-mono/blob/main/README.md) ——在 DeepSeek Harness 裡一個都沒有。

### 證據二：官方揭露文件自己把話講完了

DeepSeek Harness 的 [THIRD\_PARTY\_NOTICES.md](https://github.com/deepseek-ai/deepseek-harness/blob/master/THIRD_PARTY_NOTICES.md) 把第三方分成兩類，這個分類本身就是答案：

| 類別 | 專案 | 官方原文描述 |
| --- | --- | --- |
| **Runtime Dependencies** | `@earendil-works/pi-ai` 、 `@anthropic-ai/claude-agent-sdk` 、 `@modelcontextprotocol/sdk` 、 `@agentclientprotocol/sdk` | "External packages that a workspace package resolves at runtime" |
| **Vendored Source** | `@deepseek-ai/cordis` （上游 [cordiverse/cordis](https://github.com/cordiverse/cordis) ）＋ 六個 cordis plugin 套件 | **"source-vendored into this repository rather than consumed from npm"** |

看清楚了嗎。DeepSeek 真正「把別人的原始碼搬進自己 repo」的對象是 **Cordis** ，不是 pi。pi-ai 跟 MCP SDK、ACP SDK、甚至 Anthropic 自家的 Claude Agent SDK 躺在同一格——就是個 npm 依賴。

如果 DSH 抄了 pi，這份文件是他們必須說謊的地方。他們沒有。

### 證據三：那個 adapter 是隔手翻譯，不是共用內臟

上面兩條都是「數依賴」，還可以再往下鑽一層——直接看那個唯一的消費者到底怎麼用 pi-ai。

[`packages/llm/llm-pi-ai/src/`](https://github.com/deepseek-ai/deepseek-harness/tree/master/packages/llm/llm-pi-ai) 在 GitHub 上是公開的，10 個 TypeScript 檔、合計約 2,700 行（npm 上打包後的 `lib/index.js` 是 1,868 行的建置產物，你 `npm i` 之後才看得到）。這 2,700 行裡，實際會在執行期真的呼叫到 pi-ai 的東西，只有這些：

```ts
// src/adapter.ts
import { createModels, getSupportedThinkingLevels } from '@earendil-works/pi-ai'
// src/provider.ts
import { createProvider } from '@earendil-works/pi-ai'
import { anthropicMessagesApi } from '@earendil-works/pi-ai/api/anthropic-messages.lazy'
import { openAICompletionsApi } from '@earendil-works/pi-ai/api/openai-completions.lazy'
import { openAIResponsesApi } from '@earendil-works/pi-ai/api/openai-responses.lazy'
// src/catalog.ts
import { builtinProviders, getBuiltinModels, getBuiltinProviders } from '@earendil-works/pi-ai/providers/all'
// src/stream.ts
import { isContextOverflow } from '@earendil-works/pi-ai'
```

八個具名函式和三個 lazy API 物件，就這樣。其餘出現在 `config.ts` 、 `context.ts` 、 `replay.ts` 裡的 pi-ai 引用 **全部是 `import type`** ——純型別，編譯完就消失，執行期一行程式碼都不剩。

剩下那兩千多行在幹嘛？看函式名就知道了：

```
toPiReplayState()   toPiAssistant()    toPiContext()    toPiContextWithImages()
mapUsage()          mapStopReason()    classifyPiAiError()
replayedAssistant() foreignAssistant() readReplayState()
```

全部都是「把 DSH 自己的型別翻成 pi 的型別，再翻回來」。而這個套件 export 出去的東西是 `{ Config, PiAiAdapter, apply, inject, name, supportedProtocols }` —— `apply` / `inject` / `name` 是 Cordis 的 plugin 契約， `PiAiAdapter` 實作的是 [`@deepseek-ai/dsh-llm`](https://github.com/deepseek-ai/deepseek-harness/tree/master/packages/llm/llm) 定義的 `LlmAdapter` 介面。

換句話說，DSH 的 LLM 語彙（ `LlmAdapter` 、 `LlmError` 、 `GenerateOptions` 、 `StreamChunk` 、 `BlockAssembler` ） **是它自己定義的** ，pi-ai 只是躲在邊界後面當「一疊 provider 的線路實作」。而且 DSH 還另外準備了 `dsh-llm-deepseek` （原生 chat-completions adapter）， `ctx.llm.registerAdapter()` 甚至支援 `replace()` 熱抽換——pi-ai 那條線隨時可以整條拔掉。

這叫用了同一個 npm 套件，不叫同源。等同於「兩個 app 都用了 axios」。

### 這套方法你可以直接拿去用

血緣鑑定其實有固定套路，下次看到「A 是不是抄 B」的爭論，照著跑就好：

1. **量擴散半徑** ： `grep -Rl '<套件名>' --include=package.json` 找宣告方， `grep -Rl --include='*.js'` 找實際引用方。半徑是一個檔案跟半徑是半個 repo，結論完全不同
2. **讀第三方揭露文件的分類** ： `runtime dependency` 跟 `vendored source` 是兩種法律與工程意義都不同的關係，正經專案會分開列
3. **看邊界函式** ：如果消費端充滿 `toX()` / `fromX()` / `mapX()` 這類翻譯函式，那是 arm's-length adapter；如果直接共用對方的內部型別滿場跑，那才是真的黏在一起
4. **分開數 `import` 與 `import type`** ：TypeScript 專案裡這兩者差很多。 `import type` 編譯後會被整個抹掉，執行期毫無關係；只有值 import 才代表真的在跑對方的程式碼。把兩者混在一起數，會把「借了幾個型別定義」誇大成「共用了整套實作」

> [!tip] Tip
> DeepSeek Harness 不是 pi 的 fork。它把 pi 的 LLM 函式庫包在一個外掛裡用，擴散半徑一個檔案；真正被 source-vendored 進 repo 的是 Cordis。至於架構哲學，兩者幾乎是反的——pi 是 4 個工具的極簡主義，DSH 是 185 個外掛的組合主義。

---

## 那 DSH 到底是什麼：「Everything is a Plugin」不是行銷詞

!\[\[DeepSeekHarness-萬物皆插件.jpg\]\]

翻完血緣，真正有意思的事情才開始。

DeepSeek Harness 的 [官方 repo 描述](https://github.com/deepseek-ai/deepseek-harness) 只有一句話：「Everything is a Plugin.」這種標語通常要打七折——大部分號稱「全都可擴充」的框架，核心那一塊還是碰不得的。

但 DSH 的 [架構文件](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md) 開宗明義寫的是這個：

> "Every part of the product is a plugin, including the model adapter, the tool registry, the session log, and **the agent loop itself**, so every part is replaceable from configuration. **There is no privileged core to patch**."

連 agent loop 本身都是可替換的外掛。這句話值不值七折？我去翻了它的預設組合。

[官方網站](https://deepseek.com/harness/en/) 列了四種執行模式，而我在本機 npm 套件的 `config/agent-presets/` 底下，剛好找到四個目錄， `preset.yml` 裡的中文名稱完全對得上：

| 官方英文模式 | 本機 preset 目錄 | `preset.yml` 中文名 | 內容 |
| --- | --- | --- | --- |
| Standard mode | `standard/` | 标准模式 | 完整編碼 Agent：檔案編輯、Shell、檔案與網頁檢索、Skills、計畫、目標、子代理、工作流 |
| Code mode | `code/` | PTC 模式 | 標準模式全部能力，但工具透過 Code Mode SDK 呈現，讓模型用一個 TypeScript 程式組合多步操作 |
| Minimal mode | `minimal/` | 极简模式 | 只有持久 bash 與 `str_replace_editor` 的雙工具 Agent |
| Creator mode | `cordis/` | 创造模式 | 標準模式全部能力，外加執行期檢查、外掛實驗、preset 創作指導 |

有趣的是 Creator mode 在 repo 裡的目錄名是 `cordis` ，而且是四個 preset 中唯一附帶 `skills/` 的（裡面放 `cordis-plugin-development` 和 `editing-cordis-compositions` 兩個技能）。也就是說，「創造模式」的本質是 **教 agent 怎麼改造 agent 自己** 。

### 一個 40 行 YAML 就是一整隻 agent

Minimal preset 的 [`agent.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/master/apps/cli/config/agent-presets/minimal/agent.cordis.yml) 是我看過最能說明「everything is a plugin」的東西。整隻雙工具 coding agent，長這樣：

```yaml
- id: persona
  name: '@deepseek-ai/dsh-persona'
  config:
    text: You are a helpful software engineer assistant.
    complete: true
    includeRuntimeContext: false

- id: persistent-shell
  name: cordis:group
  group: true
  isolate:
    terminals: true
  config:
    - id: pty
      name: '@deepseek-ai/dsh-terminal'
    - id: terminal-bash
      name: '@deepseek-ai/dsh-terminal-bash'
      config:
        timeoutMs: 300000
    - id: persistent-bash
      name: '@deepseek-ai/dsh-tool-bash-persistent'

- id: filesystem
  name: cordis:group
  group: true
  isolate:
    fs: true
  config:
    - id: fs-local
      name: '@deepseek-ai/dsh-fs-local'
      config:
        cwd: !!js process.env.DSH_CWD ?? process.cwd()
    - id: str-replace-editor
      name: '@deepseek-ai/dsh-tool-str-replace-editor'
      config:
        maxOutputChars: 16000
```

沒有程式碼。整隻 agent 是一份組態，而且每一行都在替換某個原本存在的東西： `persona` 加上 `complete: true` 直接 **吃掉整份 system prompt** （全域身分、Web 導覽、工具指引、後續組裝 listener 全部塞不進來）； `fs-local` 在自己的 realm 裡 **遮蔽掉宿主原本的沙箱化檔案系統** ；context compaction 直接不掛，所以這個 preset 根本沒有壓縮。

這才是「沒有特權核心」的意思——不是「開了很多擴充點」，而是 **預設行為本身就是一層可以被蓋掉的組態** 。

---

## 開機是一場層疊：profile、bundle、patch

!\[\[DeepSeekHarness-啟動層疊組合.jpg\]\]

理解 DSH 最大的門檻在這裡：一個跑起來的 `dsh` 不是一支程式，是 **開機時由有序層疊組合出來的一棵 plugin tree** 。

我把本機的 `~/.dsh/profiles/web/` 打開，裡面只有四個檔案，加起來不到 30 行：

```
package.json        → dsh.profile.bundles: ['@deepseek-ai/dsh-base', '@deepseek-ai/dsh-web-app']
cordis.yml          → []
cordis.patch.yml    → []
pnpm-workspace.yaml → packages: [.] / nodeLinker: hoisted
```

`cordis.yml` 是空陣列，而且檔案裡的註解直接告訴你為什麼：

> `# dsh profile root — an empty entry list. The tree is composed as patches`  
> `# Edit cordis.patch.yml, not this file.`

整棵樹是這樣疊出來的：

```
#mermaid-1786956110083-j5sdwn9i6{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#333;}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#mermaid-1786956110083-j5sdwn9i6 .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#mermaid-1786956110083-j5sdwn9i6 .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#mermaid-1786956110083-j5sdwn9i6 .error-icon{fill:#552222;}#mermaid-1786956110083-j5sdwn9i6 .error-text{fill:#552222;stroke:#552222;}#mermaid-1786956110083-j5sdwn9i6 .edge-thickness-normal{stroke-width:1px;}#mermaid-1786956110083-j5sdwn9i6 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1786956110083-j5sdwn9i6 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1786956110083-j5sdwn9i6 .edge-thickness-invisible{stroke-width:0;fill:none;}#mermaid-1786956110083-j5sdwn9i6 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1786956110083-j5sdwn9i6 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1786956110083-j5sdwn9i6 .marker{fill:#333333;stroke:#333333;}#mermaid-1786956110083-j5sdwn9i6 .marker.cross{stroke:#333333;}#mermaid-1786956110083-j5sdwn9i6 svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-1786956110083-j5sdwn9i6 p{margin:0;}#mermaid-1786956110083-j5sdwn9i6 .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#333;}#mermaid-1786956110083-j5sdwn9i6 .cluster-label text{fill:#333;}#mermaid-1786956110083-j5sdwn9i6 .cluster-label span{color:#333;}#mermaid-1786956110083-j5sdwn9i6 .cluster-label span p{background-color:transparent;}#mermaid-1786956110083-j5sdwn9i6 .label text,#mermaid-1786956110083-j5sdwn9i6 span{fill:#333;color:#333;}#mermaid-1786956110083-j5sdwn9i6 .node rect,#mermaid-1786956110083-j5sdwn9i6 .node circle,#mermaid-1786956110083-j5sdwn9i6 .node ellipse,#mermaid-1786956110083-j5sdwn9i6 .node polygon,#mermaid-1786956110083-j5sdwn9i6 .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#mermaid-1786956110083-j5sdwn9i6 .rough-node .label text,#mermaid-1786956110083-j5sdwn9i6 .node .label text,#mermaid-1786956110083-j5sdwn9i6 .image-shape .label,#mermaid-1786956110083-j5sdwn9i6 .icon-shape .label{text-anchor:middle;}#mermaid-1786956110083-j5sdwn9i6 .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#mermaid-1786956110083-j5sdwn9i6 .rough-node .label,#mermaid-1786956110083-j5sdwn9i6 .node .label,#mermaid-1786956110083-j5sdwn9i6 .image-shape .label,#mermaid-1786956110083-j5sdwn9i6 .icon-shape .label{text-align:center;}#mermaid-1786956110083-j5sdwn9i6 .node.clickable{cursor:pointer;}#mermaid-1786956110083-j5sdwn9i6 .root .anchor path{fill:#333333!important;stroke-width:0;stroke:#333333;}#mermaid-1786956110083-j5sdwn9i6 .arrowheadPath{fill:#333333;}#mermaid-1786956110083-j5sdwn9i6 .edgePath .path{stroke:#333333;stroke-width:2.0px;}#mermaid-1786956110083-j5sdwn9i6 .flowchart-link{stroke:#333333;fill:none;}#mermaid-1786956110083-j5sdwn9i6 .edgeLabel{background-color:rgba(232,232,232, 0.8);text-align:center;}#mermaid-1786956110083-j5sdwn9i6 .edgeLabel p{background-color:rgba(232,232,232, 0.8);}#mermaid-1786956110083-j5sdwn9i6 .edgeLabel rect{opacity:0.5;background-color:rgba(232,232,232, 0.8);fill:rgba(232,232,232, 0.8);}#mermaid-1786956110083-j5sdwn9i6 .labelBkg{background-color:rgba(232, 232, 232, 0.5);}#mermaid-1786956110083-j5sdwn9i6 .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#mermaid-1786956110083-j5sdwn9i6 .cluster text{fill:#333;}#mermaid-1786956110083-j5sdwn9i6 .cluster span{color:#333;}#mermaid-1786956110083-j5sdwn9i6 div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(80, 100%, 96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-1786956110083-j5sdwn9i6 .flowchartTitleText{text-anchor:middle;font-size:18px;fill:#333;}#mermaid-1786956110083-j5sdwn9i6 rect.text{fill:none;stroke-width:0;}#mermaid-1786956110083-j5sdwn9i6 .icon-shape,#mermaid-1786956110083-j5sdwn9i6 .image-shape{background-color:rgba(232,232,232, 0.8);text-align:center;}#mermaid-1786956110083-j5sdwn9i6 .icon-shape p,#mermaid-1786956110083-j5sdwn9i6 .image-shape p{background-color:rgba(232,232,232, 0.8);padding:2px;}#mermaid-1786956110083-j5sdwn9i6 .icon-shape rect,#mermaid-1786956110083-j5sdwn9i6 .image-shape rect{opacity:0.5;background-color:rgba(232,232,232, 0.8);fill:rgba(232,232,232, 0.8);}#mermaid-1786956110083-j5sdwn9i6 :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}dsh --profile web讀 profile 的 package.json
取出 dsh.profile.bundles第 1 層 @deepseek-ai/dsh-base
model adapters / tools / persistence
sandbox & approval / settings / credentials第 2 層 @deepseek-ai/dsh-web-app
Web server + 瀏覽器端 client plugins第 3 層 profile 的 cordis.patch.yml
這個 profile 專屬覆寫第 4 層 DSH_HOME/cordis.patch.yml
整台機器層級覆寫第 5 層 --patch overlay
單次啟動覆寫最終 Cordis plugin treedsh --dump-config 可完整印出
```

**bundle** 是「Cordis config rows ＋ 它們掛載的程式碼」的發行格式，每一層都能被上層 patch。 `dsh-base` 是每個 profile 的第一層； `dsh-web-app` 疊上瀏覽器應用； `dsh-headless` 則疊上無伺服器的一次性 runner。而 `dsh` 這個 CLI 本身只是個 launcher，負責解析 profile、疊 patch、載入選中的 runner。

底下撐住這一切的是 [Cordis](https://github.com/cordiverse/cordis) ，一個獨立的 MIT 開源框架，自稱「Meta-Framework of Spatiotemporal Composability」。 [官方 primer](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/cordis-primer.md) 把它拆成五個機制，其中兩個是理解全局的關鍵：

- **Context 是 service 倉庫** ：每個 service 佔住一個穩定的 `ctx.<key>` （ `ctx.tools` 、 `ctx.llm` 、 `ctx.sessions` 、 `ctx.sandbox` …），其他 plugin **用 key 找 service，永遠不 import 具體實作** 。這就是「可換」的全部來源
- **註冊都是可逆的 effect** ：透過 `ctx.effect()` / `ctx.on()` 安裝，plugin 卸載時自動解開。所以熱重載不是特異功能，是架構的自然結果

還有一個 waterfall 事件機制—— `emit` / `waterfall` / `parallel` / `serial` 四種 dispatch mode，其中 waterfall 的 listener 必須呼叫 `next()` ，因此可以攔截並改寫。等一下你會看到 DSH 把所有關鍵路徑都做成 waterfall。

---

## Session 是唯一真相：Model-visible means logged

!\[\[DeepSeekHarness-事件溯源會話日誌.jpg\]\]

如果只能記住 DeepSeek Harness 的一件事，我會投給這個。

大部分 agent 框架的做法是：維護一個 message array 餵給模型，順便寫個 log 給人看。DSH 把這個關係 **倒過來** 。 [`dsh-session` 的 README](https://github.com/deepseek-ai/deepseek-harness/tree/master/packages/core/session) 寫得毫不含糊：

> "A `Session` is the **append-only source of truth** for an agent's whole interaction history — the LLM message history is **derived** from it."

配套的 runtime invariant 更狠， [架構文件](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md) 只有五個字：

> **"Model-visible means logged."**

任何進入 model request 的內容，都 **必須** 能從 log 重建。fork、resume、transcript、telemetry、持久化，全部是這條事件流的衍生物，沒有旁路。而且 DSH 還附了一個可選的 invariant companion，會去檢查單調遞增的 sequence number、turn/step 的包覆關係、以及同一個 step 內 tool call 與 result 的配對——載入時直接重播既有 session 驗一遍。

這在 HN 上是被誇最多的一點：模型看到的一切都攤在那，不像某些商業 agent 產品的 trace 是加密的。

有了這層之後，執行流程就很清楚了。DSH 定義 **step = 一次 model request 加上它呼叫的所有工具** ， **turn = 零或多個 step** ：

```
ctx.toolsctx.llmctx.systemPromptctx.sessionsctx.agentLoop使用者ctx.toolsctx.llmctx.systemPromptctx.sessionsctx.agentLoop使用者#mermaid-1786956110290-fe1gv8nc8{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#333;}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#mermaid-1786956110290-fe1gv8nc8 .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#mermaid-1786956110290-fe1gv8nc8 .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#mermaid-1786956110290-fe1gv8nc8 .error-icon{fill:#552222;}#mermaid-1786956110290-fe1gv8nc8 .error-text{fill:#552222;stroke:#552222;}#mermaid-1786956110290-fe1gv8nc8 .edge-thickness-normal{stroke-width:1px;}#mermaid-1786956110290-fe1gv8nc8 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1786956110290-fe1gv8nc8 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1786956110290-fe1gv8nc8 .edge-thickness-invisible{stroke-width:0;fill:none;}#mermaid-1786956110290-fe1gv8nc8 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1786956110290-fe1gv8nc8 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1786956110290-fe1gv8nc8 .marker{fill:#333333;stroke:#333333;}#mermaid-1786956110290-fe1gv8nc8 .marker.cross{stroke:#333333;}#mermaid-1786956110290-fe1gv8nc8 svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-1786956110290-fe1gv8nc8 p{margin:0;}#mermaid-1786956110290-fe1gv8nc8 .actor{stroke:hsl(259.6261682243, 59.7765363128%, 87.9019607843%);fill:#ECECFF;}#mermaid-1786956110290-fe1gv8nc8 text.actor>tspan{fill:black;stroke:none;}#mermaid-1786956110290-fe1gv8nc8 .actor-line{stroke:hsl(259.6261682243, 59.7765363128%, 87.9019607843%);}#mermaid-1786956110290-fe1gv8nc8 .messageLine0{stroke-width:1.5;stroke-dasharray:none;stroke:#333;}#mermaid-1786956110290-fe1gv8nc8 .messageLine1{stroke-width:1.5;stroke-dasharray:2,2;stroke:#333;}#mermaid-1786956110290-fe1gv8nc8 #arrowhead path{fill:#333;stroke:#333;}#mermaid-1786956110290-fe1gv8nc8 .sequenceNumber{fill:white;}#mermaid-1786956110290-fe1gv8nc8 #sequencenumber{fill:#333;}#mermaid-1786956110290-fe1gv8nc8 #crosshead path{fill:#333;stroke:#333;}#mermaid-1786956110290-fe1gv8nc8 .messageText{fill:#333;stroke:none;}#mermaid-1786956110290-fe1gv8nc8 .labelBox{stroke:hsl(259.6261682243, 59.7765363128%, 87.9019607843%);fill:#ECECFF;}#mermaid-1786956110290-fe1gv8nc8 .labelText,#mermaid-1786956110290-fe1gv8nc8 .labelText>tspan{fill:black;stroke:none;}#mermaid-1786956110290-fe1gv8nc8 .loopText,#mermaid-1786956110290-fe1gv8nc8 .loopText>tspan{fill:black;stroke:none;}#mermaid-1786956110290-fe1gv8nc8 .loopLine{stroke-width:2px;stroke-dasharray:2,2;stroke:hsl(259.6261682243, 59.7765363128%, 87.9019607843%);fill:hsl(259.6261682243, 59.7765363128%, 87.9019607843%);}#mermaid-1786956110290-fe1gv8nc8 .note{stroke:#aaaa33;fill:#fff5ad;}#mermaid-1786956110290-fe1gv8nc8 .noteText,#mermaid-1786956110290-fe1gv8nc8 .noteText>tspan{fill:black;stroke:none;}#mermaid-1786956110290-fe1gv8nc8 .activation0{fill:#f4f4f4;stroke:#666;}#mermaid-1786956110290-fe1gv8nc8 .activation1{fill:#f4f4f4;stroke:#666;}#mermaid-1786956110290-fe1gv8nc8 .activation2{fill:#f4f4f4;stroke:#666;}#mermaid-1786956110290-fe1gv8nc8 .actorPopupMenu{position:absolute;}#mermaid-1786956110290-fe1gv8nc8 .actorPopupMenuPanel{position:absolute;fill:#ECECFF;box-shadow:0px 8px 16px 0px rgba(0,0,0,0.2);filter:drop-shadow(3px 5px 2px rgb(0 0 0 / 0.4));}#mermaid-1786956110290-fe1gv8nc8 .actor-man line{stroke:hsl(259.6261682243, 59.7765363128%, 87.9019607843%);fill:#ECECFF;}#mermaid-1786956110290-fe1gv8nc8 .actor-man circle,#mermaid-1786956110290-fe1gv8nc8 line{stroke:hsl(259.6261682243, 59.7765363128%, 87.9019607843%);fill:#ECECFF;stroke-width:2px;}#mermaid-1786956110290-fe1gv8nc8 :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}送出 promptappend user/messageagent/pre-step（waterfall，可攔截）組裝 system prompt（sections + tool schemas + 變數）agent/request（waterfall，最後改寫點）ctx.llm.stream(options)StreamChunk 串流append assistant/*工具呼叫進入六段管線tool/resultappend tool/*、step/end
```

注意 `agent/pre-step` 、 `agent/request` 、 `llm/stream` 全是 waterfall。這代表你可以寫一個外掛，在模型看到請求之前把它整個改掉，而不用碰任何核心程式碼。

system prompt 也不是一個字串常數，而是 [一個註冊表](https://github.com/deepseek-ai/deepseek-harness/tree/master/packages/core/system-prompt) ：各個 plugin 貢獻 **有序 section** 、tool schema、具名變數，每個 step 重新組裝一次。 `includeHarnessIdentity` 控制那句 order −100 的固定開場， `persona` 是 order 0， `toolOrder` 甚至能明確指定模型看到工具的排列順序（含一個 `'<unlisted-tools>'` 的 rest 位置）。

順帶一提， [`dsh-agent-instructions`](https://github.com/deepseek-ai/deepseek-harness/tree/master/packages/context/agent-instructions) 這個外掛在載入 `AGENTS.md` / `CLAUDE.md` 時，用的是「the familiar system-reminder pattern」——README 逐字這樣寫。它還會透過觀察成功的 `read` / `write` / `edit` 的 `tools/result` 來發現巢狀目錄的指示檔變動， **而不是靠解析 shell 的 `cd`** ，理由寫得很實在：每次本地 bash 呼叫都是全新的 shell，解析任意 shell 語法不可靠。

這種細節看多了會發現一件事：DSH 在對齊誰，它自己講得很清楚——MCP 工具命名「與 Claude Code 和 Codex 相同」、system-reminder 樣式、 `AGENTS.md` 慣例。從頭到尾 **沒有一個地方提到 pi** 。

---

## 六段管線與兩道 fail-closed 閘

!\[\[DeepSeekHarness-工具執行管線.jpg\]\]

工具執行在 DSH 裡不是「呼叫一個函式」，是走一條有六段的管線。 [`dsh-tools`](https://github.com/deepseek-ai/deepseek-harness/tree/master/packages/core/tools) 定義的順序是：

```
tools/pre-execute   ← 可擴充的 allow/deny 閘門
      ↓
monotonic registered guards
      ↓
tools/execute       ← around-dispatch 包裝（timeout / retry / metrics 插這裡）
      ↓
tools/post-execute  ← 檢查或替換結果、附加 context
      ↓
finalizeContent     ← 工具定義自己擁有的最終邊界
      ↓
tools/result        ← 唯讀通知，不可改
```

註冊的分層規則很漂亮：一般 plugin context 註冊 = 全域工具；用 `agent.ctx` 註冊 = **只給那一個 agent，並且遮蔽同名的全域工具** 。所以「某個 agent 有自己特化的 bash」不需要任何 if-else，只是註冊在不同層。

!\[\[DeepSeekHarness-沙箱與審批.jpg\]\]

安全那一側則是兩道獨立的閘，兩道都是 fail-closed。

[沙箱](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/subsystems/sandbox.md) 是 `ctx.sandbox` seam，把 subprocess 的 argv 包進檔案效果政策。三個後端分別是 Linux 的 bwrap/Landlock、macOS 的 Seatbelt、Windows 的 ACL restricted-token；三種模式是 `read-only` 、 `workspace-write` （只有 workspace root 加後端承諾的暫存區可寫）、 `danger-full-access` 。它甚至會誠實回報執行強度是 `full` 還是 `partial` ——舊版 Landlock ABI 和 Windows ACL 的 Everyone/hard-link 邊界目前就是 partial。

最關鍵的是這句官方原文：

> **"silent unconfined passthrough is never legal"**

沙箱不可用時直接丟 `SandboxUnavailableError` ，絕不默默放行。這在 agent 領域是很值得抄的態度，乾。

[審批](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/subsystems/approval.md) 則是 `ctx.approval` ，回答「這個特定動作能不能做」。結果只有四種且封閉： `allowed-once` （注意， **唯一的授權形式就是一次性** ）、 `rejected` 、 `cancelled` 、 `unavailable` 。每個 session 的政策要嘛是 `ask` （委派給組合進來的回答者，沒人回答就 `unavailable` ），要嘛是 `never` （一律拒絕，給 headless / CI 用）。每次詢問與決定都會 append 一對稽核事件—— **只進 log，不進模型的 transcript** 。

---

## 代價：HN 上 276 則留言在吵什麼

!\[\[DeepSeekHarness-插件疲勞的代價.jpg\]\]

講到這裡都太漂亮了，該講代價。 [Hacker News 那串討論](https://news.ycombinator.com/item?id=49285244) 分成很清楚的兩派，反對意見主要是兩條：

**第一條是「插件疲勞」。** 有人擔心初期熱度過去之後，這會變成「一堆互不相容、被棄坑、毫無一致性的外掛的惡夢」。這個擔憂不是空穴來風——任何以擴充性為賣點的生態都走過這條路，而 DSH 的插件治理故事現在還在成形中。

**第二條更尖銳：值不值得。** 有評論直接質疑 Cordis 那套代數（service / effect / waterfall / realm）到底帶來多少實質價值，還指出「90% 的外掛根本不會有交叉依賴」。翻成白話就是：你為了讓 agent loop 可替換，付出了讓所有人都得先讀懂一套組合框架的代價，這筆帳划算嗎？

老實說，我讀 DSH 那些 README 的時候有同感。它的文件密度高到有點反人類——單一個 `dsh-llm` 的 README 就在講 all-or-nothing 註冊、 `replace()` 的原子性、 `INVALID_DIRECTORY` 與 `DUPLICATE_DIRECTORY` 的區別。第一次讀完只想說是在哈囉，但讀第二次會發現，那些囉嗦其實都在描述同一件事： **每個註冊動作在失敗時要怎麼保持系統不壞** 。

專案作者 Tianyi Cui 自己在串裡也回得很直接：「It's just an early developer preview version we're presenting in MIT license currently. Expect lots of rough edges and compatibility-breaking changes.」官方 [社群公告](https://github.com/deepseek-ai/deepseek-harness/discussions/12) 也承認「许多问题还没有标准答案」。

所以現在的狀態很清楚：架構有想法，但插件 API 不穩、文件落後於程式碼、治理還沒定案。

> [!warning] Warning
> **一、preset 裡的 service row 忘記包 `isolate` realm。** [官方 preset 文件](https://github.com/deepseek-ai/deepseek-harness/tree/master/packages/preset/agent-presets) 明講：preset 內的 service row 必須放在帶 `isolate` realm 的 group 裡，否則會 publish 進 root realm 變成 **process-global** 。你以為只改了一個 agent，結果整個 process 的所有 agent 都被影響——這種 bug 極難從症狀反推。
> 
> **二、Web server 沒有 TLS 也沒有 auth。** `dsh web` 只有 `127.0.0.1` 和 `0.0.0.0` 兩種 host 選項，官方明言綁到非 loopback 位址就等於暴露在那個網路上。別手癢改成 `0.0.0.0` 然後接上公司內網。
> 
> **三、 `run_code` 這個名字是保留字。** 不管你設成哪種工具呈現模式，這個名稱都不能註冊、遮蔽、限制或移除，因為任何 agent 都可能選擇 Code Mode。撞名的話會在註冊階段就炸給你看。

---

## 什麼時候該用它，什麼時候不該

!\[\[DeepSeekHarness-選型決策.jpg\]\]

把三個當事人放在一起比，差異其實非常好分辨：

| 面向 | **pi** | **DeepSeek Harness** | **Claude Code** |
| --- | --- | --- | --- |
| 核心工具數 | 4 個（ `read` / `write` / `edit` / `bash` ） | 數十個（含 `cordis_*` 、 `workflow` 、 `subagent` 、 `schedule_*` 、 `lsp` …） | 中等，意見鮮明的預設集 |
| 擴充方式 | TypeScript Extensions、Skills、Prompt Templates、Themes | Cordis plugin tree；agent 可在執行期自己定義並運行外掛 | Hooks、Skills、MCP、Subagents |
| 權限模型 | 官方明言 **沒有** 內建權限系統，靠容器化 | `ctx.sandbox` ＋ `ctx.approval` 雙重 fail-closed | 內建審批流程 |
| 介面 | TUI | Web GUI ＋ headless CLI ＋ Python SDK ＋ ACP | CLI ＋ 桌面 ＋ IDE |
| 套件規模 | 官方 README 表列 5 個核心套件 | 194 個（185 個 `dsh-*` ） | 閉源 |
| 授權 | MIT | MIT | 商業 |

**該用 DeepSeek Harness 的情況** ：你要做的是 **別人的 agent 產品** ，不是用 agent。你需要換掉 agent loop、換掉 session 儲存、把工具管線塞進自己的審批系統，而且需要完整的執行軌跡可以稽核。它內建的 subagent providers 甚至能把任務委派給 **真正的 Codex CLI 與 Claude Code CLI** （透過 `codex app-server --stdio` 和官方 `@anthropic-ai/claude-agent-sdk` ），這代表它把自己定位成「可以編排其他 agent 產品的 harness」，而不只是又一隻 agent。

**不該用的情況** ：你只是想要一個好用的 coding agent 來寫扣。那 DSH 目前的學習曲線完全不划算——先讀懂 Cordis、profile 層疊、realm 隔離，才能開始客製化。這個階段的它是給 harness 開發者的，不是給 harness 使用者的。而且 v0.1 developer preview 加上官方自己的破壞性變更警告，現在拿去架生產環境等於自己挖坑給自己跳。

想玩的話，門檻倒是低到不行：

```bash
npx @deepseek-ai/dsh web
# 預設服務於 http://127.0.0.1:3080

# 或者從原始碼建
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```

---

## 最後

回到一開始的問題。DeepSeek Harness 抄 pi 了嗎？沒有。三個 grep 指令、一份第三方揭露文件、一個 adapter 檔案就能把這件事定案，而且方法可以複用到下一次爭論。

但我覺得更值得帶走的是另一件事： **「用了同一個套件」跟「原始碼相近」之間的距離，是可以量化的** 。在 AI 生態這種「大家都在互相參考」的時期，能量化這個距離的人，比能大聲主張的人有用得多。

至於 DSH 本身——把 agent loop 做成可替換的外掛、把 session 做成唯一真相來源、把沙箱和審批都做成 fail-closed，這些是有想法的設計。它現在的問題不是想法不好，是想法太多而文件跟不上。v0.1 就是 v0.1，別急著上生產。

如果你也想自己驗一次，把上面那三個 grep 指令跑一遍就好，五分鐘的事。跑出來的結果如果跟我不一樣，歡迎告訴我——說不定你裝到的版本已經變了，畢竟這專案昨天才發布。

---

## 相關筆記

- \[\[深度研究\_DeepSeekHarness與pi血緣鑑定\_20260814\]\] — 完整的一手研究報告，含逐項來源與本機實測指令

## 參考資料

**DeepSeek Harness 官方**

- [deepseek-ai/deepseek-harness GitHub repo](https://github.com/deepseek-ai/deepseek-harness) — MIT，TypeScript
- [DeepSeek Harness 官方網站](https://deepseek.com/harness/en/) — 四種執行模式說明
- [架構文件 docs/architecture.md](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [Cordis primer docs/cordis-primer.md](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/cordis-primer.md)
- [工具目錄 docs/tool-catalog.md](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/tool-catalog.md)
- [沙箱子系統 docs/subsystems/sandbox.md](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/subsystems/sandbox.md)
- [審批子系統 docs/subsystems/approval.md](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/subsystems/approval.md)
- [子代理子系統 docs/subsystems/subagent.md](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/subsystems/subagent.md)
- [THIRD\_PARTY\_NOTICES.md](https://github.com/deepseek-ai/deepseek-harness/blob/master/THIRD_PARTY_NOTICES.md)
- [官方社群公告 Discussion #12](https://github.com/deepseek-ai/deepseek-harness/discussions/12)

**相關套件原始碼**

**pi harness**

**其他**

- [Hacker News: DeepSeek Harness developer preview](https://news.ycombinator.com/item?id=49285244) — 662 分、276 則留言
- [cordiverse/cordis](https://github.com/cordiverse/cordis) — Cordis 上游框架
- [TechNode 報導](https://technode.com/2026/08/14/against-claude-cowork-deepseek-opens-its-open-source-harness-to-developers/)