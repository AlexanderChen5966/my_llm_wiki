---
title: "🔮 Hermes Agent 🤖 — 深入解析與自建指南 📘"
source: "https://codelove.tw/@tony/post/aw0oyq#google_vignette"
author:
published:
created: 2026-06-24
description: "對 [Nous Research 的 Hermes Agent](https://github.com/nousresearch/hermes-agent) 的一個實用、端到端導覽：它建立在什麼原則之上、讓它運作的架構是什麼，以及如何自己動手打造類似的自我改進型代理的具體清單。> 延伸閱讀：[🏗..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月24日](https://codelove.tw/@tony/post/aw0oyq "2026-06-24 08:45:11")

對 [Nous Research 的 Hermes Agent](https://github.com/nousresearch/hermes-agent) 的一個實用、端到端導覽：它建立在什麼原則之上、讓它運作的架構是什麼，以及如何自己動手打造類似的自我改進型代理的具體清單。

> 延伸閱讀： [🏗️ 建構高品質 AI 代理 🤖 — 一份全面、可執行的實戰指南 📚](https://dev.to/truongpx396/building-high-quality-ai-agents-a-comprehensive-actionable-field-guide-5m1) 、 [📎 Paperclip 深入解析 🤖 — 打造「AI 公司」🏢 Control Plane 的建置指南](https://dev.to/truongpx396/paperclip-deep-dive-a-build-guide-for-an-ai-company-control-plane-dda) 、 [🤖 Multica 深入解析 — 如何建構託管代理平台 🌐](https://dev.to/truongpx396/multica-deep-dive-how-to-build-a-managed-agents-platform-54l2) 。

---

## 📋 目錄

- [🤖 1. Hermes 到底是什麼（用一段話說明）](#1-hermes-到底是什麼用一段話說明)
- [🧭 2. 核心原則](#2-核心原則)
- [🏗️ 3. 高層架構](#3-高層架構)
- [🔄 4. 代理人迴圈（整個系統的核心）](#4-代理人迴圈整個系統的核心)
- [🧩 5. 系統提示詞組裝](#5-系統提示詞組裝)
- [🛠️ 6. 工具系統](#6-工具系統)
	- [6.1 📦 自我註冊式註冊表](#61-自我註冊式註冊表)
		- [6.2 🗂️ 工具集](#62-工具集)
		- [6.3 🖥️ 執行環境](#63-執行環境)
		- [6.4 🤖 代理人層級工具](#64-代理人層級工具)
		- [6.5 🔗 MCP 整合](#65-mcp-整合)
		- [6.6 🛡️ 工具核准與安全性（分層防禦）](#66-工具核准與安全性分層防禦)
- [🧠 7. 技能系統（最關鍵的功能）](#7-技能系統最關鍵的功能)
	- [7.1 📄 什麼是技能](#71-什麼是技能)
		- [7.2 📁 技能存放在哪裡](#72-技能存放在哪裡)
		- [7.3 🔍 漸進式揭露（3 層）](#73-漸進式揭露3-層)
		- [7.4 ⚡ 觸發機制](#74-觸發機制)
		- [7.5 🎛️ 條件式啟用](#75-條件式啟用)
		- [7.6 🔁 自我改進： `skill_manage` 工具](#76-自我改進skill_manage-工具)
		- [7.7 🌐 技能中心與分享](#77-技能中心與分享)
- [💾 8. 記憶系統](#8-記憶系統)
	- [🧊 機制 1 — 凍結快照式持久記憶](#機制-1--凍結快照式持久記憶)
		- [🗃️ 機制 2 — 透過 SessionDB 的跨會話回憶](#機制-2--透過-sessiondb-的跨會話回憶)
		- [🔌 機制 3 — 可插拔供應器（Honcho / mem0 / supermemory）](#機制-3--可插拔供應器honcho--mem0--supermemory)
- [🔌 9. 外掛系統](#9-外掛系統)
- [📋 9b. `COMMAND_REGISTRY` 模式（很值得借鑑）](#9b-command_registry-模式很值得借鑑)
- [🎨 9c. 外觀引擎（把主題視為資料）](#9c-外觀引擎把主題視為資料)
- [📡 9d. 多模態與串流](#9d-多模態與串流)
- [🎓 9e. RL / Atropos 訓練整合（ `environments/` ）](#9e-rl--atropos-訓練整合environments)
- [🖥️ 10. 介面 — 代理人如何接觸使用者](#10-介面--代理人如何接觸使用者)
	- [10.1 💻 CLI（經典）](#101-cli經典)
		- [10.2 🖼️ TUI（ `hermes --tui` ）— 真的很新穎](#102-tuihermes---tui--真的很新穎)
		- [10.3 📨 Gateway（訊息平台）](#103-gateway訊息平台)
		- [10.4 🔗 ACP（Agent Client Protocol）— 給 AI 原生編輯器使用](#104-acpagent-client-protocol--給-ai-原生編輯器使用)
		- [10.5 🌐 Web UI（ `hermes web` ）](#105-web-uihermes-web)
		- [10.6 ⏰ Cron 排程器（ `~/.hermes/cron/` ）](#106-cron-排程器hermescron)
		- [10.7 🏭 批次執行器（訓練資料管線）](#107-批次執行器訓練資料管線)
- [👤 11. 設定檔與多實例](#11-設定檔與多實例)
- [⚙️ 12. 設定與密鑰](#12-設定與密鑰)
- [💰 13. 提示詞快取（成本故事）](#13-提示詞快取成本故事)
- [🗺️ 14. 自己打造 — 具體檢查清單](#14-自己打造--具體檢查清單)
	- [🌱 階段 1 — 迴圈（第 1–2 天）](#階段-1--迴圈第-12-天)
		- [💻 階段 2 — CLI（第 3 天）](#階段-2--cli第-3-天)
		- [🛠️ 階段 3 — 工具註冊表（第 4–5 天）](#階段-3--工具註冊表第-45-天)
		- [💾 階段 4 — 記憶與人格（第 6 天）](#階段-4--記憶與人格第-6-天)
		- [🧠 階段 5 — 技能（第 7–10 天）← 這就是魔法](#階段-5--技能第-710-天--這就是魔法)
		- [💰 階段 6 — 提示詞快取（第 11 天）](#階段-6--提示詞快取第-11-天)
		- [📨 階段 7 — Gateway（第 12 天起）](#階段-7--gateway第-12-天起)
		- [🔗 階段 8 — MCP（第 14 天起）](#階段-8--mcp第-14-天起)
		- [✨ 階段 9 — 設定檔與打磨](#階段-9--設定檔與打磨)
- [⚡ 15. 建議的技術堆疊](#15-建議的技術堆疊)
- [⚠️ 16. 你一定會遇到的坑](#16-你一定會遇到的坑)
- [💡 17. 一句話心智模型](#17-一句話心智模型)
- [📚 18. 參考資料](#18-參考資料)

---

## 🤖 1. Hermes 到底是什麼（用一段話說明）

Hermes 是一個 **與模型無關、可自我改進的對話式代理人** ，可在本機以 CLI/TUI 執行，也可在伺服器上以訊息 Gateway（Telegram/Discord/Slack/WhatsApp/Signal）運作，或作為排程的 cron 工作執行。它的關鍵差異化在於一個 **封閉式學習迴圈** ：在使用工具解決問題時，它會撰寫可重複使用的「技能」文件，並整理一個持久化的記憶檔案，因此代理人跑得越久，能力就越強。所有東西——模型、工具、技能、記憶後端、執行環境、UI——都能插拔替換。

在開始建造之前，先內化兩個概念：

1. **一個代理人，多種介面。** 同一個 `AIAgent` 類別驅動所有介面。各種介面（CLI、Gateway、cron、batch、API）只是薄薄的入口，負責建立代理人並呼叫 `run_conversation()` 。
2. **程序式記憶 > 聰明的提示詞。** 大部分「智慧型代理人」的行為，不是來自提示詞工程，而是來自代理人自己擁有一個 markdown 文件資料夾（技能 + 記憶 + 人格），並能隨時間讀、寫、成長。

---

## 🧭 2. 核心原則

這些是 Hermes 遵循的設計規則。自己實作時也要記住——程式碼庫裡大多數看起來「奇怪」的決策，都可追溯到這些原則之一。

### 2.1 🌐 與平台無關的核心

代理人不知道自己是在終端機、Telegram 對話，還是 cron 工作中執行。所有平台細節都放在 **adapter** 裡，負責把平台事件轉成 `agent.run_conversation(...)` ，再把回應轉回去。如果你發現自己在核心代理人程式裡加 Telegram 專用的 `if` 分支，就代表架構已經偏離了。

### 2.2 🔒 提示詞穩定性（有利於快取）

系統提示詞會在 **會話開始時一次組裝完成，且在對話中不會變動** 。這不只是美觀問題，而是經濟問題。Anthropic 和 OpenAI 的提示詞快取都需要穩定前綴才能命中。對話中途改變工具集、重新載入記憶或切換技能，都會讓快取失效，成本暴增 10 倍。預設應把變更延後到「下一個會話」。

### 2.3 🔍 漸進式揭露

不要把每個技能、每個記憶、每個工具的完整文件都塞進系統提示詞。先載入 **描述** （第 0 層）。只有代理人真的需要某個技能時，才讓它拉進完整內容（第 1 層）。只有技能本身要求時，才載入參考檔案（第 2 層）。這就是 Hermes 能在上下文限制內支援 47 個工具與數十個技能的方法。

### 2.4 📝 自我註冊勝過中央清單

工具與外掛應在 import 時自行註冊（ `registry.register(...)` ），而不是人工維護一份 `__all__` 清單。新增一個工具只需要新增一個檔案，不必到處修改。

### 2.5 🧱 設定檔隔離

多個獨立代理人實例可同時存在，方式是每個實例擁有自己的 `HERMES_HOME` 目錄（預設 `~/.hermes/` ，可用環境變數覆寫）。程式碼庫中的所有檔案路徑都必須經由 `get_hermes_home()` ，絕不能硬寫 `~/.hermes` 。

### 2.6 🎒 代理人自己擁有自己的學習素材

技能不是由人手動編輯原始碼加入的。代理人在解決一個非平凡任務後，會透過 `skill_manage` 工具自己寫入技能。記憶也不是由人手動整理的——代理人會在各回合之間編輯 `MEMORY.md` 和 `USER.md` 。這就是整個迴圈。

---

## 🏗️ 3. 高層架構

```
┌──────────────────────────────────────────────────────────────────┐
│                           入口點                                 │
│   CLI / TUI / Gateway（TG、Discord、Slack）/ Cron / Batch / API  │
└──────────────────┬───────────────────────────────────────────────┘
                   │   每個入口點都建立一個 AIAgent
                   ▼
┌──────────────────────────────────────────────────────────────────┐
│                        AIAgent（核心迴圈）                        │
│  build_system_prompt → 呼叫模型 → 派發工具呼叫 → 重複             │
└─────┬─────────────┬────────────────┬────────────────┬────────────┘
      │             │                │                │
      ▼             ▼                ▼                ▼
┌──────────┐  ┌──────────┐    ┌────────────┐   ┌─────────────┐
│  工具    │  │  技能    │    │   記憶    │   │  供應器     │
│ 註冊表   │  │  載入器  │    │  管理器   │   │（模型 API） │
└──────────┘  └──────────┘    └────────────┘   └─────────────┘
      │
      ▼
┌──────────────────────────────────────────────────────────────────┐
│  執行環境：本機 / Docker / SSH / Modal / Daytona                 │
└──────────────────────────────────────────────────────────────────┘
```

用白話說，分成三層：

- **第 1 層 — 介面（Surfaces）：** 人或系統如何和代理人互動（CLI、聊天平台、cron）。
- **第 2 層 — 代理人核心：** 迴圈加上四個可插拔子系統（工具、技能、記憶、模型）。
- **第 3 層 — 執行後端：** shell / 程式碼工具真正在哪裡執行。今天在本機筆電，明天可以是沙箱化 Docker，正式環境則可能是 Modal 雲端。

---

## 🔄 4. 代理人迴圈（整個系統的核心）

這是最重要的部分。整個 `AIAgent` 類別本質上就是這個迴圈：

```
1. 接收輸入               → 來自 CLI / Gateway / cron / ACP / web
2. 組裝系統提示詞          → 人格 + 記憶 + 技能 + 工具（每個會話只做一次）
3. 解析供應器              → 使用哪個 API key + endpoint
4. 呼叫模型               → 四種 API 模式之一（依 endpoint/model 自動偵測）：
                             chat_completions | codex_responses |
                             anthropic_messages | bedrock_converse
5. 解析回應
   ├─ 如果有工具呼叫       → 透過註冊表逐一派發 → 附加結果 → 回到步驟 4
   └─ 否則                 → 最終助理訊息 → 顯示 → 持久化 → 完成
6. 持久化                 → SQLite SessionDB（WAL 模式 + FTS5 索引）
```

幾個很重要但不直觀的細節：

- **迭代預算比單純計數器更細緻。** 一個執行緒安全的 `IterationBudget` 會同時共享給父代理人與它建立的任何子代理人。 `execute_code` 在完成時會 **退還** 迭代次數，因此程式化工具迴圈不會把預算耗盡。當預算用完時：只注入一則警告訊息（ `_budget_exhausted_injected` ），之後允許 **剛好一次** 最後 API 呼叫（ `_budget_grace_call` ），然後強制摘要。 **不插入中途警告** ——這是刻意設計，避免模型提早放棄。
- **推理內容與可見助理訊息分開儲存。** OpenAI o 系列與 Anthropic extended thinking 都會產生隱藏的「推理」token。要把它們放在自己的欄位中；它們對快取有效性很重要，但不應顯示。回呼包括： `stream_delta_callback` 、 `interim_assistant_callback` 、 `thinking_callback` 、 `reasoning_callback` 。
- **具狀態的串流清理。** `_stream_context_scrubber` 會移除 `<memory-context>` 區段，即使它們被切成多個 chunk 也能處理——當標籤跨越網路邊界時，這類事情其實非常麻煩。
- **壓縮，不是截斷。** 當上下文滿了， `context_compressor` 會摘要中間輪次，而不是直接丟掉。摘要本身會成為一則訊息。可以有資訊損失；不能保留完整內容，否則會 OOM。
- **中斷處理。** 在工具呼叫中按 Ctrl-C，必須能乾淨地取消進行中的工具、在歷史紀錄中附加一個「使用者中斷」的工具結果，並把控制權交還回來。不要把整個迴圈殺掉——讓代理人知道這次中斷，並做出回應。
- **會話續接。** `--continue` / `--resume` 旗標會透過 `SessionDB.get_messages()` 載入先前歷史。SQLite WAL 模式加上自訂重試層（20–150 ms 抖動、 `BEGIN IMMEDIATE` ）可處理多程序寫入競爭。在繼續之前，系統會先向使用者顯示摘要。

---

## 🧩 5. 系統提示詞組裝

`prompt_builder.build_system_prompt()` 會依照以下順序串接內容：

1. **人格** — `SOUL.md` / `DEFAULT_AGENT_IDENTITY` 。身份、語氣、價值觀。
2. **平台提示** — `PLATFORM_HINTS` 。告訴模型目前是在 CLI、Telegram、Slack 等環境執行，這會改變格式規則（例如 CLI 不用 MarkdownV2、Telegram 不要巢狀 code block，等等）。
3. **記憶指引** — `MEMORY_GUIDANCE` 。把 **凍結快照** 的 `MEMORY.md` + `USER.md` 以單一區塊嵌入（用 `§` 分隔）。大小有上限（大約 MEMORY 2200 字元、USER 1375 字元）。
4. **會話搜尋指引** — `SESSION_SEARCH_GUIDANCE` 。告訴代理人可透過 FTS5 搜尋先前會話，並附上一個小範例。
5. **技能指引** — `SKILLS_GUIDANCE` 。第 0 層技能索引，以及在完成困難任務後「建立技能」的提示性文字。
6. **情境檔案** — 工作目錄中的 `AGENTS.md` 與 `.hermes.md` 。
7. **工具使用強制規則** — `TOOL_USE_ENFORCEMENT_GUIDANCE` 。例如平行呼叫、錯誤恢復等硬性規則。
8. **工具結構描述** — 所有已啟用工具的 JSON schema。

接著 `prompt_caching.py` 會插入快取斷點（Anthropic 的 `cache_control: {type: ephemeral}` ；其他供應器也有對應做法）。整個組裝後的前綴就成為可快取區域。

**凍結快照模式（這就是關鍵）。** `MEMORY.md` 和 `USER.md` 會在 **會話開始時只讀一次** ，並在整個會話期間以不可變方式嵌入系統提示詞。代理人仍然可以在會話中寫入這些檔案——但系統提示詞不會改變。結果就是：整個對話期間快取都有效，而新的記憶會在下一次會話生效。跳過這一步就會破壞前綴快取。

**記憶安全掃描。** 在注入前， `MEMORY/USER` 的內容會先掃描提示詞注入模式、利用環境變數的 `curl` / `wget` 外洩嘗試、持久化後門，以及不可見 Unicode。被污染的記憶檔對代理人來說就像朊毒體一樣危險——一定要防禦性掃描。

**關鍵規則：** 第 1–8 區塊在 **整個會話期間都固定不變** 。使用者訊息與工具結果則會附加到歷史紀錄中；它們不會進入系統提示詞。

---

## 🛠️ 6. 工具系統

### 6.1 📦 自我註冊式註冊表

中央的 `tools/registry.py` 提供：

```python
registry.register(
    name="read_file",
    toolset="filesystem",
    schema={...JSON schema...},
    handler=read_file_handler,
    available=lambda ctx: True,   # 可用性判斷
)
```

每個工具檔案都在模組 import 時呼叫這個方法。註冊表負責：

- **收集 schema** 供系統提示詞使用。
- 模型發出工具呼叫時，依名稱進行 **派發** 。
- **可用性過濾** （依使用者、平台、工具集）。
- **錯誤包裝** ——任何 handler 的例外都會轉成工具結果，讓模型看見並能回應。絕不要讓工具把迴圈弄掛。

所有 handler 都回傳 **JSON 字串** ，不是 Python 物件。模型永遠只會看到文字。

### 6.2 🗂️ 工具集

工具會分成邏輯集合（filesystem、web、browser、code、mcp、vision、audio、…）— Hermes 內建約 40+ 個工具（文件某些地方寫「47 個內建」，有些地方寫「40+」；AGENTS.md 說以檔案系統為準，因為數量一直變動——你自己實作時不要硬寫數字）。使用者以工具集為單位啟用/停用，而不是逐一工具設定。被停用的工具集會完全不出現在系統提示詞中——節省 token，也避免模型甚至知道它們存在。

### 6.3 🖥️ 執行環境

執行 shell 命令或程式碼的工具，會透過環境抽象層（ `tools/environments/` ）：

| 後端 | 使用情境 |
| --- | --- |
| `local` | 開發時在筆電上使用。最快，沒有隔離。 |
| `docker` | 共用開發機。每個會話一個容器。 |
| `ssh` | 遠端 VM。把 VM 當成代理人的「電腦」。 |
| `daytona` / `modal` | 用於正式環境的無伺服器沙箱。自動啟動。 |
| `singularity` | HPC 叢集。 |

同一個工具，爆炸半徑不同。代理人不需要知道——只有執行環境會變。

### 6.4 🤖 代理人層級工具

有幾個工具（ `todo_*` 、 `memory_*` 、 `skill_manage` 、 `skills_list` 、 `skill_view` ）會在通用工具派發之前先被攔截，由代理人自己處理，因為它們改的是代理人狀態（記憶、技能、待辦清單），而不是外部世界。這類工具應維持少量且明確。

### 6.5 🔗 MCP 整合

[Model Context Protocol](https://modelcontextprotocol.io/) 伺服器可作為額外工具來源插入。Hermes 將每個 MCP 伺服器視為一個虛擬工具集，讓使用者可以過濾單一工具，並透過同一個註冊表派發呼叫。這就是你能得到大量整合（GitHub、Slack、Linear、…）而不用自己全部重寫的方法。

### 6.6 🛡️ 工具核准與安全性（分層防禦）

Shell 工具很危險。Hermes 使用 **四層** 機制：

1. **Tirith** — 外部 Rust 基礎掃描器，支援自動安裝與 SHA-256 驗證。可偵測同形異義字 URL、終端機注入攻擊（隱藏指令的 ANSI escape）以及已知危險模式。
2. **正則危險指令偵測** — 針對 **正規化** 後的命令字串執行（大小寫不敏感、空白壓縮），避免攻擊者用 `RM  -RF` 之類方式繞過。
3. **智慧核准** — 由 LLM 對每個命令進行風險評分。低風險自動核准；中高風險則需人工核准。
4. **核准範圍** — 人工核准時可選 **一次 / 整個會話 / 永久** 。信任會累積，而不是每次都重問。

當代理人在訊息 Gateway 上執行且需要核准時，會用 `threading.Event` 阻塞，直到人類在聊天中回應。 `/yolo` 指令可在受信任會話中完全略過核准。 **沙箱後端會自動略過核准** （Docker/Modal 沙箱本身就是安全邊界；再問一次只會增加摩擦）。

---

## 🧠 7. 技能系統（最關鍵的功能）

### 7.1 📄 什麼是技能

技能是一份帶有 YAML frontmatter 的 **markdown 文件** ，用來教代理人如何把一件事做好。不是程式碼，不是設定檔，而是一份代理人會閱讀的 *runbook* 。

```yaml
---
name: deploy-staging
description: Push current branch to staging via Vercel and verify health.
version: 1.2.0
platforms: [macos, linux]
requires_toolsets: [shell, web]
fallback_for_toolsets: []
required_environment_variables: [VERCEL_TOKEN]
tags: [deploy, vercel]
category: devops
---

## When to Use
The user asks to "ship", "deploy to staging", or "preview this branch".

## Procedure
1. Run \`git status\` — abort if dirty.
2. Run \`vercel --token=$VERCEL_TOKEN\`.
3. Poll \`/healthz\` until 200 OR 60s timeout.
4. Report the preview URL.

## Pitfalls
- Don't deploy from \`main\` — only feature branches.
- If the build fails, fetch logs via \`vercel logs <deployment>\`.

## Verification
The healthz endpoint returns \`{"status":"ok"}\`.
```

### 7.2 📁 技能存放在哪裡

```
~/.hermes/skills/
├── devops/deploy-staging/
│   ├── SKILL.md              ← 上面的檔案
│   ├── references/           ← 技能可載入的額外文件
│   ├── templates/            ← 檔案樣板
│   ├── scripts/              ← 代理人可執行的輔助腳本
│   └── assets/               ← 圖片等
├── .hub/                     ← 從技能中心安裝
└── .bundled_manifest         ← Hermes 隨附內容
```

### 7.3 🔍 漸進式揭露（3 層）

這就是控制 token 使用量的關鍵：

| 層級 | 載入內容 | 時機 |
| --- | --- | --- |
| **0** | 名稱、描述、類別 | 永遠載入 — 在系統提示詞中 |
| **1** | 完整 `SKILL.md` 內容 | 代理人決定要使用該技能時 |
| **2** | `references/` 、 `scripts/` 中的檔案 | 技能本文要求「請參考 references/ [foo.md](http://foo.md/) 」時 |

代理人會呼叫 `read_skill` （或等效工具），從 L0 升級到 L1，再到 L2。

### 7.4 ⚡ 觸發機制

技能有三種啟動方式：

1. **斜線指令** — 使用者輸入 `/deploy-staging please ship #123` 。
2. **自然語言** — 「把這個部署到 staging」；代理人根據 L0 描述比對並載入 L1。
3. **程式化** — cron 工作明確附加技能。

### 7.5 🎛️ 條件式啟用

frontmatter 欄位用來控制可見性：

- `platforms: [linux]` — 在 macOS 上隱藏。
- `fallback_for_toolsets: [web]` — 只有在 **沒有** 啟用高級 web 工具時才可見（例如 Brave Search 沒設定時，用 DuckDuckGo 技能補位）。
- `requires_toolsets: [shell]` — 若 shell 工具停用則隱藏。

這讓技能目錄會隨部署環境自動調整。

### 7.6 🔁 自我改進：skill\_manage 工具

代理人會使用兩個互補工具：

- **讀取路徑：** `skills_list` （瀏覽第 0 層索引）與 `skill_view` （升級到第 1/2 層內容）。
- **寫入路徑：** `skill_manage` ，一個帶有子操作的 meta-tool：

| 動作 | 效果 |
| --- | --- |
| `create` | 從零建立新技能 |
| `patch` | 精準文字替換（更新時首選） |
| `edit` | 全量重寫 |
| `delete` | 刪除技能（僅限使用者/代理人建立的技能；內建技能不能刪） |

注意：技能內的檔案管理（ `references/` 、 `scripts/` ）會透過一般的 `write_file` / `remove_file` 工具處理，但範圍限制在該技能目錄下。

系統提示詞中的 `SKILLS_GUIDANCE` 區塊會 **明確提醒代理人** 在以下情況建立技能：

- 解決了一個需要 5 次以上工具呼叫的任務。
- 找到了一個不明顯的 workaround。
- 發現了一個可能重複使用的工作流程。

從技能中心安裝技能是 **只能由使用者驅動** 的（安全性考量）。代理人絕不會自己安裝不受信任的技能——它只能基於自己的經驗執行 `skill_manage create` 。

這就是封閉式學習迴圈。代理人在工作時，也在撰寫自己的作業手冊。

### 7.7 🌐 技能中心與分享

技能本質上就是可攜式 markdown——非常容易分享。Hermes 整合多個來源（ `official/` 、 `skills-sh/` 、 `github/` 、 `well-known/` 、 `url` 、 `clawhub` 、 `lobehub` ）。安裝時，每個技能都會先經過 **安全掃描** ，檢查提示詞注入、資料外洩與破壞性命令，然後才會被信任。信任層級： `builtin` > `official` > `community` 。

格式採用開放的 [agentskills.io](https://agentskills.io/) 標準——也就是說，為 Hermes 撰寫的技能也能在其他相容代理人中使用。

---

## 💾 8. 記憶系統

三個獨立機制共同運作（這裡的「3 層」只是教學上的簡化說法——在程式中它們是彼此正交的）：

### 🧊 機制 1 — 凍結快照式持久記憶

兩個 markdown 檔案，都是 **由代理人整理** ：

- **`MEMORY.md`** — 事實。「專案每週二出貨。」「測試資料庫密碼在 1Password vault X。」（約 2200 字元上限）
- **`USER.md`** — 使用者模型。「偏好簡潔回答。」「資深 Go 工程師，剛接觸 React。」（約 1375 字元上限）

`MemoryStore` 會在 **會話開始時只讀一次** ，並把它們以單一不可變區塊（用 `§` 分隔）嵌入系統提示詞。代理人可以在會話中途寫入這些檔案（寫入也會落盤），但系統提示詞中的副本直到下一次會話都不會變。這就是維持前綴快取有效的方式。

### 🗃️ 機制 2 — 透過 SessionDB 的跨會話回憶

`SessionDB` （SQLite、 **WAL 模式** 、FTS5 全文索引）儲存過去每一輪對話。需要時，代理人會使用 `session_search` 工具查詢；LLM 摘要器會把命中的內容濃縮成一段可放入上下文的文字。多程序寫入競爭則透過 `BEGIN IMMEDIATE` 加上自訂重試迴圈（20–150 ms 抖動）處理。

### 🔌 機制 3 — 可插拔供應器（Honcho / mem0 / supermemory）

這不是額外層，而是可直接替換的方案。只有一個 `MemoryProvider` 抽象類別（ `agent/memory_provider.py` ）；由 `agent/memory_manager.py` 負責協調。生命週期鉤子包括： `prefetch()` （呼叫模型前）、 `sync_turn()` （回合後）、 `shutdown()` 。

重要的供應器參數：

- **回憶模式：** `hybrid` / `context` / `tools` 。 `tools` 模式讓模型自己決定何時查詢； `context` 模式則是在每回合直接注入相關記憶。
- **寫入頻率：** `async` / `turn` / `session` / 數字（每 N 回合）。

**Honcho 的「dialectic」值得一提** ，因為聽起來很神祕但其實不是：它會依序執行三個推理階段——Initial Assessment → Self-Audit → Reconciliation——深度由 `dialecticDepth` （1–3）控制。本質上就是把自我批判串接起來，以提升使用者模型品質。

同一時間只能有一個啟用中的供應器。要根據用途選擇合適的抽象（想要深度使用者建模就選 Honcho；想要向量回憶就選 mem0/supermemory；若文件 + FTS5 已經夠用，則不用）。

---

## 🔌 9. 外掛系統

`PluginManager` 會從三個地方發現外掛：

1. `~/.hermes/plugins/` （使用者層級）
2. `./.hermes/plugins/` （專案層級）
3. pip entry points（ `hermes.plugins` ）

每個外掛都定義一個 `register(ctx)` 函式，並可掛接到生命週期事件：

- `pre_tool` / `post_tool`
- `pre_llm` / `post_llm`
- `session_start` / `session_end`

……也可以註冊新工具、新 CLI 指令，或替換記憶供應器。

**鐵則：** 外掛絕對不能修改核心檔案。如果外掛需要框架未暴露的功能，應該是框架新增一個通用 hook，而不是搞特殊 import。這樣才能維持外掛介面的穩定。

---

## 📋 9b. COMMAND\_REGISTRY 模式（很值得借鑑）

`hermes_cli/commands.py` 裡的一個 `COMMAND_REGISTRY` 常數是所有斜線指令的單一真相來源。程式庫會從這個結構自動推導出：

- CLI 派發
- Gateway hooks（讓 Telegram 也能用 `/skill foo` ）
- Telegram inline menu 專案
- Slack slash 子指令
- prompt\_toolkit 自動完成
- `/help` 文字

新增一個斜線指令，只需要新增一筆 `CommandDef` 和一個 handler。 **完全沒有分散式修改。** 這和工具註冊表是同一種模式，只是應用在 UI 指令上。很值得自己學起來——Hermes 就是靠這種方式，在不增加維護成本的前提下擴大表面積。

---

## 🎨 9c. 外觀引擎（把主題視為資料）

`~/.hermes/skins/` 裡的 YAML 檔案（並可從 `default` 繼承）。一份 YAML 控制 18 種命名顏色、spinner 的臉與動詞、代理人名稱與問候/道別、提示符號、工具 emoji、帶 Rich 標記的 ASCII banner。內建十種 skin（default、daylight、mono、poseidon、charizard、…）。Hermes Mod 還提供含即時預覽與圖片轉 ASCII 的 web 編輯器。

重點在架構上： **品牌樣式存在 YAML，而不是程式碼。** 使用者可以在不碰 Python 的情況下改整體視覺。對一個使用者可能連續待上好幾小時的代理人來說，這點比你想像中更重要。

---

## 📡 9d. 多模態與串流

- **視覺：** `vision_analyze` 工具。Anthropic 圖片轉文字的 fallback 快取透過 `_anthropic_image_fallback_cache` 處理（當模型原生看不到圖片時，快取可避免重複描述）。
- **音訊輸出：** `text_to_speech` 工具。
- **音訊輸入：** 輸入端的語音備忘錄轉錄。
- **瀏覽器工具：** 注入多模態上下文（螢幕截圖 + DOM + 擷取文字）。
- **串流：** `_stream_callback` 、 `_current_streamed_assistant_text` ，以及具狀態的 `_stream_context_scrubber` ，即使跨 chunk 邊界也能移除 `<memory-context>` 區段。

---

## 🎓 9e. RL / Atropos 訓練整合（environments/）

這可以說是 Nous Research 這個專案真正的重點，而不是附加功能。 `environments/` 目錄把 Hermes 包裝成強化學習訓練用環境：

- **`HermesAgentBaseEnv`** — 抽象化工具解析與沙箱接線。
- **`HermesAgentLoop`** — 以 RL rollout 可驅動的方式執行工具呼叫迴圈。
- **`ToolContext`** — 將沙箱暴露給 reward function（例如 reward 可以 grep 檔案系統來驗證代理人是否真的做事）。
- **`resize_tool_pool`** — 避免平行 rollout 時的 thread pool deadlock。
- **雙階段訓練管線：**
	- *Phase 1:* VLLM/SGLang 原生工具呼叫解析。
		- *Phase 2:* `ManagedServer` 原始 token 解析——這是為了 Hermes 的 XML 風格工具標記與 DeepSeek 的 Unicode 分隔符。
- **三層工具結果預算：** 每個工具截斷 → 沙箱溢出並附預覽 → 每回合預算。沒有這個機制，一個 `ls /` 就可能把訓練 rollout 的上下文視窗撐爆。
- **預整合基準測試：** TerminalBench 2.0、YC-Bench、WebResearch。

你自己的 v1 代理人通常不需要這些。但如果哪天你要用自己的 traces 微調模型，知道這些 hook 已經存在就很有幫助。

---

## 🖥️ 10. 介面 — 代理人如何接觸使用者

同一個 `AIAgent` 驅動六種不同介面。每一種都是薄薄的 adapter，而不是重新實作一份。

### 10.1 💻 CLI（經典）

`cli.py` （約 11k 行）。基於 Rich 的面板、帶自動完成的 prompt\_toolkit 輸入、動畫 spinner（KawaiiSpinner）、API 呼叫期間的活動 feed。

### 10.2 🖼️ TUI（hermes --tui）— 真的很新穎

**不只是比較漂亮的 CLI。** 架構如下：

- **前端：** Node.js + React Ink。
- **後端：** Python `tui_gateway/server.py` 。
- **傳輸格式：** 透過 stdio 的 newline-delimited **JSON-RPC 2.0** 。

Python 端會把 `print` 重新導向到 stderr，確保 stdout 保持乾淨以供協議使用。持久化的 `_SlashWorker` 子程序負責執行斜線指令，而較慢的 handler 會走 `ThreadPoolExecutor` ，讓中斷保持即時可回應。特色包括：帶有 braille spinner 的串流思考內容、 `ToolTrail` 樹狀視覺化、虛擬歷史視窗（只渲染可見列）、滑鼠選取。

**來自 [AGENTS.md](http://agents.md/) 的設計規則：** *不要在 React 裡重新實作聊天介面。* 轉錄內容、composer 與斜線指令行為都屬於內嵌的 TUI。側邊欄與檢視器可以，但不要做替代視圖。

### 10.3 📨 Gateway（訊息平台）

Telegram、Discord、Slack、WhatsApp、Signal。每個 adapter 都會：

1. 連接平台（websocket / long-poll / webhook）。
2. 收到訊息時：驗證使用者、產生穩定的 `session_key` 、到 SessionDB 查找 session、以該歷史建立 `AIAgent` 。
3. 呼叫 `agent.run_conversation()` 。
4. 將回應格式化並送回（Telegram 的 MarkdownV2、Discord 的格式、Slack 的 mrkdwn——這些都在 adapter 裡處理）。

### 10.4 🔗 ACP（Agent Client Protocol）— 給 AI 原生編輯器使用

ACP 是 Zed 與新興 VS Code 整合用來與代理人溝通的標準協議。Hermes 實作了 `HermesACPAgent` 。ACP 會話綁定到編輯器的 `cwd` ，並保留在同一個共用的 SessionDB 中。Hermes 工具會對應到 ACP 的語意型別（例如 `read_file` → `read` ），而 IDE 也可以註冊 MCP 伺服器，讓代理人把它們視為額外工具集。

### 10.5 🌐 Web UI（hermes web）

`web/` 裡的 React SPA + `hermes_cli/web_server.py` 裡的 FastAPI。分頁有：Status、Sessions（FTS5 搜尋介面）、Config（表單 + 原始 YAML）、Cron、Skills。安全性包括：短效 session token、DNS rebinding 防護、CORS、rate limiting。支援英文 / 中文本地化。

### 10.6 ⏰ Cron 排程器（~/.hermes/cron/）

**不是 APScheduler。** 這是一個自訂排程器，在 Gateway 程式內的背景執行緒上跑 60 秒一輪的 `tick()` 迴圈。工作以 **JSON** 儲存在 `~/.hermes/cron/jobs.json` （不是 SQLite）。輸出會持久化到 `~/.hermes/cron/output/{job_id}/{timestamp}.md` 。

工作定義支援：

- 間隔（ `every 30m` ）、5 欄 cron、一次性持續時間、ISO 時間戳。
- `prompt` 欄位（要送出的使用者訊息）。
- 可選的 `skills` 清單，執行前先附加（例如「review-PRs」cron 工作可以先載入 `pr-review` 技能）。
- 傳送目標： `local` （只寫檔）、 `origin` （回傳到工作建立的地方），或 `platform:chat_id` （送到特定 Telegram/Slack 聊天室）。

每次 tick 都會建立一個 **全新的、沒有歷史紀錄的 AIAgent** ，載入附加的技能，執行 prompt，送出輸出，然後更新工作狀態。

### 10.7 🏭 批次執行器（訓練資料管線）

根目錄下有兩個兄弟檔：

- **`batch_runner.py`** — 以 `multiprocessing.Pool` 執行 `BatchRunner` ，每個 worker 都有一個獨立的 `AIAgent` 。 `toolset_distributions.py` 會根據獨立包含機率，對每個 prompt 抽樣工具集。 `checkpoint.json` 的 checkpoint 以 **prompt 文字** 為鍵，而不是索引（因此 prompt 清單修改不會讓 checkpoint 失效）。輸出軌跡會整理成 HuggingFace 格式；推理內容會透過 `<REASONING_SCRATCHPAD>` 標記或原生 thinking tokens 偵測，沒有推理的軌跡會被丟棄。
- **`mini_swe_runner.py`** — 用於 SWE 風格基準測試的兄弟執行器。

這些就是 Nous 產生真實代理人執行資料的方式。

---

## 👤 11. 設定檔與多實例

如果你想在同一台機器上同時跑「個人」代理人和「工作」代理人，而又不想讓它們的記憶混在一起？用 profiles。

實作其實很簡單，但 **時機非常關鍵** ：

- 每個 profile 都有自己的 `HERMES_HOME` 目錄。
- `hermes_cli/main.py` 裡的 `_apply_profile_override()` 會在 **任何其他模組 import 之前** 設定 `HERMES_HOME` 。如果你在 import 之後才設定，某些在 import 時讀取路徑的模組就會使用錯誤的 home。
- 程式碼庫中的每次路徑查找都必須經由 `get_hermes_home()` 。任何地方硬寫 `~/.hermes` 都會破壞 profile 隔離。

要注意的事項：

- 測試必須同時 mock `Path.home()` 與 `HERMES_HOME` 環境變數——只 mock 其中一個會導致不穩定失敗。
- Gateway adapter 會為每個 profile 取得獨立的 token lock，因此兩個 profile 不會同時嘗試消耗同一個 Telegram bot token。
- Honcho identities（以及其他記憶供應器 ID）都以 profile 為範圍——不要跨 profile 共用，否則會把使用者模型互相污染。

---

## ⚙️ 12. 設定與密鑰

三個調整點，三個地方：

| 內容 | 放哪裡 | 原因 |
| --- | --- | --- |
| 模型、工具集、終端後端、skin | `config.yaml` | 非密鑰，可與 profile 一起版本控管 |
| API key、token | `.env` | 密鑰，絕不記錄 |
| 每個技能的設定 | 各技能自己的 `config.yaml` | 技能本地化 |

之所以存在三個 config loader（ `load_cli_config` 、 `load_config` 、直接 YAML），是因為 CLI / 工具 / Gateway 執行時的需求有微妙差異。不要太早把它們合併——這種重複是刻意的。

---

## 💰 13. 提示詞快取（成本故事）

這是你的代理人在正式環境中會便宜或昂貴的最主要原因。

**要做的事：**

- 每個會話只建立一次系統提示詞。
- 插入供應器專用的快取斷點（Anthropic：在前綴中最後一則靜態訊息上標記 `cache_control: {type: "ephemeral"}` ）。
- 記憶使用 **凍結快照模式** ：在會話開始時只讀一次 MEMORY/USER 檔案，即使之後磁碟上的檔案變了，嵌入內容也保持不變。
- 將設定變更（如「工具集開/關」、「切換模型」）延後到下一個會話。會改變狀態的斜線指令如果真的需要立刻生效，可以接受可選的 `--now` 旗標，但預設應延後。

**不要做的事：**

- 在對話中途重新載入記憶。
- 在對話中途新增或移除工具。
- 因為使用者「換話題」就改變系統提示詞。
- 讓系統提示詞依賴目前時間、隨機 ID，或任何每回合都會變的東西。

可快取的前綴讀取成本大約是寫入成本的 1/10。若前綴穩定，一段 10 輪對話的成本大約只有單輪的 1.5 倍。若前綴不穩定，成本則會暴增 10 倍。

---

## 🗺️ 14. 自己打造 — 具體檢查清單

以下是我會建議的建置順序。每一步都可以獨立上線。

### 🌱 階段 1 — 迴圈（第 1–2 天）

1. 選擇語言（Hermes 用 Python；Go 也可以——可參考你的 repo 裡的 `backend-go/` ）。
2. 實作 `AIAgent.run_conversation(messages) -> messages` ：
	- 呼叫模型。
		- 如果回應有工具呼叫，就逐一派發、附加工具結果訊息、繼續迴圈。
		- 否則回傳最終助理訊息。
3. 加入 `IterationBudget` （預設：每個使用者回合 25 次工具呼叫）。預算耗盡時再給一次寬限回合。
4. 每個工具呼叫都包在 try/except 裡——錯誤要 **作為工具結果** 回傳，絕不要讓例外逃出迴圈。

這樣你就有了一個「會使用工具的聊天機器人」。這已經是任何代理人 80% 的核心了。

### 💻 階段 2 — CLI（第 3 天）

5. 建立一個薄薄的 CLI：讀取一行、呼叫 `run_conversation` 、印出回應、重複。
6. 加入 Ctrl-C 中斷處理，能優雅取消進行中的工具。
7. 把會話持久化到 SQLite。對訊息文字欄位加上 FTS5 virtual table。

### 🛠️ 階段 3 — 工具註冊表（第 4–5 天）

8. 建立 `Registry` 類別，提供 `register(name, toolset, schema, handler, available)` 。
9. 自動 import `tools/` 下每個檔案，讓工具模組自行註冊。
10. 實作 5 個起始工具：
	- `read_file` 、 `write_file` 、 `list_dir`
		- `run_shell` （先從本機限定開始）
		- `web_fetch`
11. 加入 `terminal.backend` 設定，用來在本機 / Docker / SSH 之間切換 `run_shell` 。

### 💾 階段 4 — 記憶與人格（第 6 天）

12. 加入 `~/.youragent/{SOUL.md, MEMORY.md, USER.md}` 檔案。
13. 在 `build_system_prompt()` 中按這個順序嵌入它們。
14. 加入代理人層級工具 `memory_append` 、 `memory_replace` 、 `memory_delete` ，讓代理人可以更新它們。

### 🧠 階段 5 — 技能（第 7–10 天）← 這就是魔法

15. 定義 `SKILL.md` 的 frontmatter 規格（直接沿用 Hermes 的——它已經是開放標準）。
16. 啟動時掃描 `~/.youragent/skills/**/SKILL.md` ，把第 0 層條目（名稱 + 描述）輸出到系統提示詞中。
17. 加入工具：
	- `read_skill(name)` → 回傳完整 `SKILL.md` （第 1 層）。
		- `read_skill_file(name, path)` → 回傳參考檔案（第 2 層）。
		- `skill_manage(action, name, ...)` → `create | patch | edit | delete` 。
18. 在系統提示詞中加入明確提醒：「當你完成一個困難任務後，寫成 skill，這樣下次就不用重新想一次。」就是這一句，會把聊天機器人變成自我改進型代理人。

### 💰 階段 6 — 提示詞快取（第 11 天）

19. 選擇主要供應器（Anthropic 的快取通常最慷慨）。
20. 在系統提示詞結尾標記快取斷點。
21. 稽核所有會碰到 `agent.system_prompt` 的程式路徑——確認沒有任何一個會在對話中途觸發。
22. 加一個 CI 測試，確認第 1 回合與第 10 回合的系統提示詞是位元組級完全一致。

### 📨 階段 7 — Gateways（第 12 天起）

23. 建立一個 adapter（Telegram 最容易—— `python-telegram-bot` 或類似方案）。
24. Adapter 的責任：驗證、session key 推導、附件下載、回應格式化。
25. 驗證：同一個代理人、同一組技能、同一份記憶，從 CLI 和 Telegram 兩邊存取時，看到的是相同的記憶更新。

### 🔗 階段 8 — MCP（第 14 天起）

26. 加入 MCP client。每個 MCP server 會變成註冊表中的一個虛擬工具集。
27. 你就能免費獲得 GitHub、Slack、Linear、Postgres、Notion 等整合。

### ✨ 階段 9 — 設定檔與打磨

28. 把所有檔案系統路徑都導向 `get_home()` helper。加上 `--profile` 旗標，在 import 前先設定 home 目錄。
29. 加入上下文壓縮器（當對話超過 N 個 token 時，用 LLM 摘要中間輪次）。
30. 加一個 cron runner，從 `~/.youragent/cron.yaml` 載入工作，並在沒有歷史紀錄的情況下執行。

整個產品就是這樣。對一位工程師來說，大約 2–3 週的專注工作量。

---

## ⚡ 15. 建議的技術堆疊

Hermes 使用的，以及我會替換的選項。

| 專案 | Hermes 的選擇 | 合理替代方案 |
| --- | --- | --- |
| 語言 | Python 3.11+ | Go（CLI 啟動更快、單一 binary）、TypeScript（Web 原生） |
| CLI 渲染 | `rich` + `prompt_toolkit` | `bubbletea` （Go）、 `ink` （TS） |
| TUI | Node.js + Ink，透過 JSON-RPC 連 Python | 同樣架構，或單一語言堆疊 |
| 儲存 | SQLite + FTS5 | 同樣即可。這裡別花俏。 |
| 向量記憶（可選） | Honcho / mem0 / supermemory | pgvector、Chroma、Qdrant |
| 沙箱 | Docker / Modal / Daytona | Firecracker、gVisor、E2B |
| MCP | Python MCP SDK | Anthropic 官方 SDK |
| 設定 | YAML | 如果你偏好，也可用 TOML |

那些看起來「很無聊」的選擇（SQLite、markdown 檔、JSON schema）不是巧合。請抵抗想把它們「升級」的衝動——Hermes 使用這些平凡方案，是因為它們能和代理人自己的工具無縫整合（代理人可以 `cat MEMORY.md` 並推理其內容）。

---

## ⚠️ 16. 你一定會遇到的坑

按照你最先遇到的順序列出：

1. **工具錯誤讓迴圈崩潰。** 每個 handler 都要包 try/except，錯誤要回傳給模型。
2. **破壞快取的提示詞。** 系統提示詞裡的 `datetime.now()` 會悄悄毀掉你的成本模型。要儘早稽核。
3. **無限工具迴圈。** 沒有迭代預算時，模型會很樂意把 `list_dir` 呼叫 400 次。一定要設硬上限。
4. **無限制的 shell 存取。** 本機後端適合開發；正式環境請用 Docker，根目錄唯讀，另設明確可寫工作區。
5. **說謊的技能。** 代理人可能會寫出引用不存在工具或環境變數的技能。安裝時要嚴格驗證 `requires_toolsets` 與 `required_environment_variables` 。
6. **記憶檔腐化。** 代理人會無止盡地往 `MEMORY.md` 加內容。要在系統提示詞中定期加上壓縮提醒：「如果 [MEMORY.md](http://memory.md/) 超過 500 行，就進行整理。」
7. **測試中的 profile 洩漏。** 測試時因為忘了 mock home 目錄，導致檔案被建立到 `~/.youragent/` 。 `Path.home()` 與 home 環境變數都要 mock。
8. **對話中途切換工具集。** 使用者輸入 `/tools` 改了設定。要告訴他「下次會話生效」——別破壞快取。
9. **跨 Gateway 的競態條件。** 兩則 Telegram 訊息在 100ms 內同時進來。要用 per-session lock。
10. **推理內容遺失。** OpenAI o 系列與 Claude extended thinking 會輸出推理區塊，必須保留在歷史紀錄中（或所有回合一致地丟棄）——不一致會破壞快取。

---

## 💡 17. 一句話心智模型

> 一個 Hermes 風格的代理人，就是 **一個會把自己檔案櫃塞滿的迴圈** ：它從 `skills/` 和 `MEMORY.md` 讀取內容來完成工作，並在學到新東西時把內容寫回同一批檔案——而其他所有系統（工具、Gateway、供應器、外掛、設定檔）都只是為了讓這個迴圈更快、更安全、並且能從更多地方被呼叫。

先把迴圈建好，再把檔案櫃建好，給它一個能修改檔案櫃的工具，並在系統提示詞裡明確告訴它「你可以這麼做」。其他一切都是支架。

---

## 📚 18. 參考資料

- Repo: [github.com/nousresearch/hermes-agent](https://github.com/nousresearch/hermes-agent)
- 文件: [hermes-agent.nousresearch.com/docs](https://hermes-agent.nousresearch.com/docs)
- 架構頁面: [hermes-agent.nousresearch.com/docs/developer-guide/architecture](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture)
- 技能格式規格: [agentskills.io](https://agentskills.io/)
- 技能中心: [agentskills.io](https://agentskills.io/)
- DeepWiki 總覽: [deepwiki.com/NousResearch/hermes-agent](https://deepwiki.com/NousResearch/hermes-agent)

---

延伸閱讀 [🏗️ 建構高品質 AI 代理 🤖 — 一份全面、可執行的實戰指南 📚](https://dev.to/truongpx396/building-high-quality-ai-agents-a-comprehensive-actionable-field-guide-5m1)

---

> 如果你覺得這篇有幫助，歡迎按 👍 或留言告訴我；如果你覺得這篇文章可能對其他人有幫助，也歡迎分享！非常感謝！😃

---

原文出處： [https://dev.to/truongpx396/hermes-agent-deep-dive-build-your-own-guide-1pcc](https://dev.to/truongpx396/hermes-agent-deep-dive-build-your-own-guide-1pcc)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aw0oyq) [3瀏覽](https://codelove.tw/@tony/post/aw0oyq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aw0oyq)

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

530

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

38

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782264292&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Faw0oyq&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782264292076&amp;bpp=3&amp;bdt=171&amp;idt=34&amp;shv=r20260623&amp;mjsv=m202606230101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782205526%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782205526%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782205526%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6940212977073&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099321%2C95394760&amp;oid=2&amp;pvsid=7140813891351313&amp;tmod=207890853&amp;uas=1&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=43" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260623/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=4&amp;uci=a!4&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>