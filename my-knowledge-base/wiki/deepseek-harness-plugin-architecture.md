---
title: DeepSeek Harness 架構與依賴血緣鑑定四步法
date: 2026-08-17
tags: [ai/agent, dev/architecture]
source: https://hackmd.io/@BASHCAT/ByB4Fv6UGl
type: article
status: processed
asset_value: 78
---

同作者的 Harness Engineering 見 [[harness-engineering]]。⚠️ 實測版本 `@deepseek-ai/dsh@0.1.0-rc.6`（2026-08-14，developer preview 發布次日），對照 `@earendil-works/pi-ai@0.82.1`；**官方明講「THERE WILL BE COMPATIBILITY-BREAKING CHANGES」**。

## T（核心論點）

兩個獨立可帶走的結論：

> **① 「依賴了某個套件」跟「原始碼相近」是兩件完全不同的事——而這兩件事之間的距離，剛好可以用三個指令量出來。**
> **② DeepSeek Harness 的「Everything is a Plugin」不打七折：連 agent loop 本身都是可替換的外掛——「There is no privileged core to patch」。**

而血緣爭議的答案很乾脆：**DSH 不是 pi 的 fork**，pi-ai 的擴散半徑是**一個檔案**；真正被 source-vendored 進 repo 的是 **Cordis**。

> 🔑 **這叫用了同一個 npm 套件，不叫同源——等同於「兩個 app 都用了 axios」。**

## B（適用場景）

**適用：**
- 判斷「A 專案是不是抄 B」的爭論（四步法可直接套用）
- 評估 plugin 式 agent 架構（與 pi 的極簡主義正好相反的一極）
- 想理解「可替換」在架構上要付什麼代價

**不適用 / 注意：**
- ⚠️ v0.1 developer preview，API 會破壞性變更
- 🔴 三個實際地雷見補充資料（realm 隔離、Web server 無 TLS/auth、`run_code` 是保留字）

## R（🔑 依賴血緣鑑定四步法）

**判斷一個開源專案有沒有「抄」另一個，看依賴清單是最沒用的做法——依賴清單只告訴你「用了」，不告訴你「用得多深」。真正該問的是：這個依賴的擴散半徑有多大？**

1. **量擴散半徑** — `grep -Rl '<套件名>' --include=package.json` 找宣告方，`grep -Rl --include='*.js'` 找實際引用方。**半徑是一個檔案跟半徑是半個 repo，結論完全不同**
2. **讀第三方揭露文件的分類** — `runtime dependency` 與 `vendored source` 是**法律與工程意義都不同**的兩種關係，正經專案會分開列
3. **看邊界函式** — 消費端充滿 `toX()`／`fromX()`／`mapX()` 這類翻譯函式 → **arm's-length adapter**；直接共用對方內部型別滿場跑 → 才是真的黏在一起
4. 🔑 **分開數 `import` 與 `import type`** — TypeScript 專案裡這兩者差很多：**`import type` 編譯後會被整個抹掉，執行期毫無關係**。混在一起數，會把「借了幾個型別定義」誇大成「共用了整套實作」

### 這四步在本案的實測結果

- **擴散半徑**：整棵樹 **194 個 `@deepseek-ai/*` 套件**（185 個 `dsh-*` 外掛），提到 pi 的只有**一個檔案**（`dsh-llm-pi-ai/lib/index.js`）；`@earendil-works` scope 底下也只裝了 `pi-ai` 一個套件——pi 的 agent runtime、CLI、TUI、telemetry **一個都沒有**
- **揭露文件**：pi-ai 與 MCP SDK、ACP SDK、**甚至 Anthropic 自家的 Claude Agent SDK 躺在同一格**（Runtime Dependencies）；Vendored Source 那一格是 Cordis。🔑 **「如果 DSH 抄了 pi，這份文件是他們必須說謊的地方。他們沒有。」**
- **邊界函式**：那個唯一消費者約 2,700 行，實際執行期呼叫 pi-ai 的只有 **8 個具名函式 + 3 個 lazy API 物件**，其餘 `config.ts`／`context.ts`／`replay.ts` 的引用**全部是 `import type`**。剩下兩千多行全是 `toPiReplayState()`／`toPiAssistant()`／`mapUsage()`／`classifyPiAiError()` 這類翻譯函式
- **可抽換性**：DSH 的 LLM 語彙（`LlmAdapter`／`LlmError`／`GenerateOptions`／`StreamChunk`）**是它自己定義的**，且另備 `dsh-llm-deepseek` 原生 adapter，`ctx.llm.registerAdapter()` 支援 `replace()` 熱抽換——**pi-ai 那條線隨時可以整條拔掉**

## R（DSH 的架構：可替換性從哪裡來）

### Context 是 service 倉庫

每個 service 佔住一個穩定的 `ctx.<key>`（`ctx.tools`、`ctx.llm`、`ctx.sessions`、`ctx.sandbox`…），🔑 **其他 plugin 用 key 找 service，永遠不 import 具體實作——這就是「可換」的全部來源。**

### 註冊都是可逆的 effect

透過 `ctx.effect()`／`ctx.on()` 安裝，plugin 卸載時自動解開。🔑 **所以熱重載不是特異功能，是架構的自然結果。**

### Session 是唯一真相

> **「A `Session` is the append-only source of truth for an agent's whole interaction history — the LLM message history is derived from it.」**
> 🔑 **「Model-visible means logged.」**

（這與 [[agent-task-system-background-scheduling]] 的「Task State 不是模型自己說了算」、[[loopx-stateful-control-plane]] 的 Evidence Log 是同一種思路：**狀態的真相在系統這邊，模型看到的只是投影**）

### 四種預設模式（本機 preset 目錄與官方模式對得上）

| 官方模式 | preset 目錄 | 內容 |
|---|---|---|
| Standard | `standard/` | 完整編碼 Agent（檔案編輯、Shell、檔案與網頁檢索、Skills、計畫、目標、子代理、工作流）|
| Code（PTC） | `code/` | 同上，但工具透過 **Code Mode SDK** 呈現，讓模型用一個 TypeScript 程式組合多步操作 |
| Minimal | `minimal/` | **只有持久 bash 與 `str_replace_editor` 的雙工具 Agent** |
| Creator | **`cordis/`** | 標準模式全部能力 ＋ 執行期檢查、外掛實驗、preset 創作指導 |

🔑 **Creator mode 的目錄名就叫 `cordis`，且是四個 preset 中唯一附帶 `skills/` 的（`cordis-plugin-development`、`editing-cordis-compositions`）——也就是說「創造模式」的本質是教 agent 怎麼改造 agent 自己。**

而 Minimal preset 的 `agent.cordis.yml` **約 40 行 YAML 就是一整隻雙工具 coding agent**（persona ＋ persistent-shell group ＋ filesystem group…），這是「everything is a plugin」最有說服力的證據。

⚠️ 開機是一場層疊（profile → bundle → patch），profile root 檔案裡甚至寫著 **`# Edit cordis.patch.yml, not this file.`**

## C（關鍵行動）

- [ ] 下次看到「A 抄 B」的爭論，先跑四步法（擴散半徑／揭露分類／邊界函式／`import` vs `import type`）
- [ ] 評估 plugin 架構時問：service 是用 key 找還是 import 實作？註冊可不可逆？
- [ ] 用 DSH 前先確認三個地雷（見下）

## 補充資料

### 🔴 HN 上 276 則留言在吵的三個實際代價

1. **preset 裡的 service row 忘記包 `isolate` realm** — 官方文件明講 service row 必須放在帶 `isolate` realm 的 group 裡，否則會 publish 進 root realm 變成 **process-global**。🔑 **「你以為只改了一個 agent，結果整個 process 的所有 agent 都被影響——這種 bug 極難從症狀反推。」**
2. **Web server 沒有 TLS 也沒有 auth** — `dsh web` 只有 `127.0.0.1` 與 `0.0.0.0` 兩種 host 選項，**官方明言綁到非 loopback 就等於暴露在那個網路上**。⚠️ 別手癢改成 `0.0.0.0` 然後接公司內網
3. **`run_code` 是保留字** — 不管設成哪種工具呈現模式，這個名稱都不能註冊、遮蔽、限制或移除（因為任何 agent 都可能選擇 Code Mode），撞名會在註冊階段就炸

### 六段管線與兩道 fail-closed 閘

DSH 的工具執行走六段管線，並有兩道 fail-closed 閘（`ctx.sandbox` ＋ `ctx.approval`），官方原則寫得很硬：

> 🔑 **「silent unconfined passthrough is never legal」**

（與 [[agent-tool-authorization-gatekeeper]] 的 fail-closed DD-14、[[agent-permission-and-os-sandbox]] 的「沒有人可以問時拒絕」完全同構——**三個獨立來源都收斂到同一條原則**）

### 🔑 三個 harness 的定位對照（本篇最實用的表）

| 面向 | **pi** | **DeepSeek Harness** | **Claude Code** |
|---|---|---|---|
| 核心工具數 | **4 個**（`read`/`write`/`edit`/`bash`）| 數十個（含 `cordis_*`／`workflow`／`subagent`／`schedule_*`／`lsp`）| 中等，意見鮮明的預設集 |
| 擴充方式 | TS Extensions、Skills、Prompt Templates、Themes | Cordis plugin tree；**agent 可在執行期自己定義並運行外掛** | Hooks、Skills、MCP、Subagents |
| 權限模型 | 🔴 **官方明言沒有內建權限系統，靠容器化** | `ctx.sandbox` ＋ `ctx.approval` 雙重 fail-closed | 內建審批流程 |
| 介面 | TUI | Web GUI ＋ headless CLI ＋ Python SDK ＋ ACP | CLI ＋ 桌面 ＋ IDE |
| 套件規模 | 5 個核心套件 | **194 個**（185 個 `dsh-*`）| 閉源 |
| 授權 | MIT | MIT | 商業 |

> 🔑 **架構哲學幾乎是反的：pi 是 4 個工具的極簡主義，DSH 是 185 個外掛的組合主義。**

## 相關連結

- [[pi-coding-agent-minimal-terminal]] — 光譜另一端的極簡主義（同一份對照表的 pi 欄）
- [[harness-engineering]] — 同作者的 Harness Engineering 主條目
- [[claude-code-harness-internals]] — Claude Code 的內部機制對照
- [[agent-plugins-open-standard]] — 2026-08 五家共發的 plugin 1.0（只定義打包與發現，信任與安全刻意留白）
- [[agent-tool-authorization-gatekeeper]] — fail-closed 與工具授權
- [[agent-permission-and-os-sandbox]] — 權限分級與 OS 沙箱
- [[agent-task-system-background-scheduling]] — Session 作為 append-only 真相來源
- [[ds4-deepseek-local-inference]] — DeepSeek 模型的本機推論
