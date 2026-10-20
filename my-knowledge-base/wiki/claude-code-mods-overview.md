---
title: Claude Code Mods：改的是 App 本身，不是跟模型講話（總覽、安裝與安全邊界）
date: 2026-10-08
tags: [ai/claude, ai/workflow, type/concept]
source: https://www.aiposthub.com/claude-code-mods-custom-workflow/（Philo，aiposthub，2026-10-03）／https://www.bnext.com.tw/article/92550/claude-code-mods-tutorial（李先泰，數位時代，2026-10-08；文末註明「本文初稿為AI編撰」）；官方來源：https://claude.com/blog/claude-code-mods 、https://code.claude.com/docs/en/plugins/mods/overview
type: article
status: processed
asset_value: 64
---

> 📅 **日期並列（不擇一）**：**2026-10-01 官方公告**（aiposthub、數位時代兩篇記載）；**2026-10-02 隨 Claude Code 2.1.287 發布**（[[mcp-server-vs-mod-rewrite]] 作者 MikeWang 記載）。前者是公告日，後者是版本發布日。需要 Claude Code **2.1.287 以上**，支援終端機與桌面版。

## T（核心論點）

> **Prompt、`CLAUDE.md`、Skills 都是在跟模型溝通；Mods 改的是 Claude Code 這個 App 本身。**

把 Claude Code 拆成兩層：**模型**（例如 Opus 5.5）負責想下一步；**App** 負責顯示對話、真正到電腦上執行指令，也就是大腦的「窗口與手腳」。過去 App 這一層只能用狀態列、設定檔 Hooks 與權限規則做有限的攔截與放行，介面怎麼畫、內建功能怎麼運作，都由 Anthropic 決定。

Mods 打開了這一層：Claude Code 每做一個動作（呼叫工具、請求權限、畫出畫面的一部分）都會發出「事件」，Mod 就是掛在事件上的小程式，可以在動作之前、之後執行，或乾脆取而代之。所以判斷「指令放哪個載體」時（見 [[steering-claude-seven-carriers]]），除了跟模型講的那幾種之外，多了一格 **App 層**。

## B（適用場景）

**適用：**
- 有固定流程、每天反覆提醒同一件事，而且文字指引解決不了「要在哪裡看資訊、何時介入操作」的問題
- 想讓資訊常駐在畫面上（例如 context 用量），或在高風險指令前先看影響範圍
- 團隊要把同一套工具行為分發給成員（Mods 用 plugin 包裝，見 [[claude-code-plugins-guide]]）

**不適用 / 注意：**
- 🔴 **自己做的 Mod 會被自動清除**：Claude 寫的 Mod 存在該對話的專屬資料夾，過一段時間會被自動清除（官方文件，數位時代轉述）。剛做好的 Mod 只在當次對話有效；要保留就得自己複製到目錄，再用 `claude --plugin-dir ~/mods/xxx` 載入。
- 🔴 **Mod 沒有沙箱，以你的身分執行**：它能以使用者權限讀寫檔案、啟動程式、連線，也能接觸對話與工具呼叫，可能讀到含金鑰的設定（官方 admin 文件，aiposthub 轉述）。即使開了 Claude Code 的指令沙箱，Mods 本身也沒有完整隔離；Mod 直接啟動的程序與 Claude 的工具呼叫是不同的管控路徑。這和 [[agent-permission-and-os-sandbox]] 談的是同一類問題；Mod 可讀環境變數與 API key，和 [[agent-api-key-handoff-six-methods-risk]] 屬同一威脅面。
- ⚠️ 只是重複貼同一段指示的話，先做成 Skill 就夠；把所有規則都改寫成 Mod，維護成本較高。
- ⚠️ **自訂面板只在終端機與 Desktop 的 Code 分頁畫得出來**，其他介面可能只跑事件處理、不顯示面板。
- ⚠️ **命名混淆**：官方也把 Mods 底層的事件處理函式叫 **hooks**，跟設定檔裡的 Hooks（[[claude-code-hooks-memory]]）不是同一個東西。
- ⚠️ **撰寫語言來源說法不一**：數位時代轉述官方定義為「改變 Claude Code 運作方式的 **TypeScript** 小函式」；aiposthub 依官方 Mods 文件寫 **JavaScript 或 TypeScript** 皆可，MikeWang 也寫 JS／TS 皆可（範例檔是 `register.tsx`）。本庫照錄，不自行統一。

## R（操作步驟）

### 1. 三種載體的分工

| 載體 | 本質 | 什麼時候用 |
|---|---|---|
| Skills（含 `CLAUDE.md`、Prompt） | 給**模型**讀的指引，告訴 Claude 怎麼想、照什麼規範做 | 讓 AI 遵循固定操作方法（例如照團隊規則審查程式碼） |
| 設定檔 Hooks | App **外部**的腳本，在特定事件跑一段指令，跑完就結束 | 在特定事件執行既有程序（例如改完檔案跑格式檢查、跑測試） |
| Mods | App **內部**常駐整段對話，能記住狀態、畫出介面、新增指令與按鈕 | 加入互動介面、記住數據、跟你來回互動，或直接接手事件 |

數位時代的比喻：Hook 像外包廠商，接到任務去外面跑個腳本就結案；Mod 像正式員工，進得去 App 的內部系統。兩者可以和 Skills 並用：Skill 提供任務方法，Mod 處理執行過程與介面。

### 2. Mod 能做的三件事

**在旁邊記錄**（看著它過去）、**把內容改寫後再放行**、**直接替 Claude Code 回應**（例如拒絕執行某個指令）。

據官方入門指南（數位時代轉述；MikeWang 也提到 `/diff`），Claude Code 自己的 `/diff` 差異面板與 `AGENTS.md` 支援（見 [[claude-code-agents-md-loading-rules]]）本身就是用 Mods 做的。

### 3. 三個官方示範

| 示範 | 做什麼 | 注意 |
|---|---|---|
| **Token Weather** | 在輸入框上方用天氣圖示與百分比顯示 context window 用量 | 呈現的是容量資訊，哪些內容值得留仍要自己判斷 |
| **Blast Radius** | 攔下 `rm -rf`、`git reset --hard`、強制推送等高風險指令，列出會動到哪些檔案，按 Proceed 才執行 | 🔴 官方坦言它**只讀指令文字**，透過別名或腳本間接執行的會漏掉；嚴格禁止仍要靠權限規則 |
| **Replay Theater** | 記錄一輪工作中的檔案編輯，用 `/replay` 逐步回看前後差異 | — |

官方公告另提到企業用法：在對話旁顯示 CI/CD 狀態、動到正式環境設定前要求確認、記錄其他 Mod 的每一次呼叫做稽核。

### 4. 安裝別人寫好的 Mod（4 步驟）

```bash
claude --version                          # 1. 確認 2.1.287 以上
/plugin marketplace add 作者帳號/儲存庫名稱   # 2. 加入市集
/plugin install Mod名稱@市集名稱              # 3. 安裝
/reload-plugins                           # 4. 重新載入
```

也可以把 Mod 連結貼給 Claude 請它代裝。要查看或關閉已裝的 Mod：輸入 `/plugin` → 切到 Installed 分頁。

### 5. 自製與保存

1. 直接用自然語言描述需求（需求越具體越好驗收，例如「每輪工作結束後，顯示對話容量使用比例」）。
2. Claude 寫檔時會詢問是否開啟本次對話的**熱重載**（hot reload）；開啟後這輪回覆結束時就生效，之後再補一句即可當場調整。
3. 🔴 改到滿意後，請 Claude 把 Mod 資料夾複製到自己的位置（例如 `~/mods/token-weather`），之後開新對話用 `claude --plugin-dir ~/mods/token-weather` 載入。不做這步，Mod 會隨對話專屬資料夾被清除。

工程師要手寫的話，官方入門指南與開發文件（`code.claude.com/docs/en/plugins/mods/create`）有完整教學，含驗證與測試指令。API 形狀、時間預算、能力清單的實測細節見 [[mcp-server-vs-mod-rewrite]]。

### 6. 安全邊界

- 官方公告寫明：Mods 擁有與 Claude Code 本身相同的電腦存取權限，**沒有 sandbox**；入門指南也提醒 Mod 的作者是**發布者而非 Anthropic**。
- **`sec-default` 兩種說法並陳**（兩篇都註明僅限 **Team／Enterprise／套用 managed settings 的電腦**，**個人電腦沒有這層防護**）：
  - aiposthub：「保護組織管理的指引與部分權限規則，但**沒有把所有 Mod 的檔案與網路操作都封住**」；企業要限制可載入的 Mods，仍應使用對應管理政策。
  - 數位時代：「**阻止使用者自裝的 Mod 推翻權限拒絕規則**等高風險動作」。
- **兩種立場並陳**：數位時代說做 Mod「不用寫程式」；aiposthub 說「AI 協助建立功能，與完成程式審查是兩件不同的工作」，功能涉及檔案、連線或批准操作時仍需要有人理解它會做什麼。本庫以後者的保守立場作為行動準則（見 C 層）。
- `claude plugin validate` 可以檢查外掛處理哪些事件、呼叫哪些功能，但**通過檢查不代表取得安全認證**。

### 7. 社群生態（轉述）

- **Next Steps**（Claude Code 團隊成員 Thariq Shihipar）：每輪對話後在輸入框上方列出最多 3 個下一步提示詞，按 1／2／3 填入輸入框讓你修改、按 0 關閉，不會自己送出。
- **awesome-claude-code-mods**：截至 2026-10-07 收錄 70 個 Mod（數位時代轉述）；Anthropic 的 Claude 目錄也開始收錄含 Mods 的外掛。

## C（關鍵行動）

- [ ] 先決定要改的是「模型的行為」還是「App 的行為」：前者寫 Skill／`CLAUDE.md`，後者才考慮 Mod
- [ ] 第一個 Mod 選**只顯示狀態、用途單純**的（例如 Token Weather），等需要讀寫檔案或自動批准操作時，再進一步讀懂程式內容
- [ ] 自己做的 Mod 改到滿意，**立刻複製出來**並改用 `claude --plugin-dir` 載入，否則會被清除
- [ ] 裝第三方 Mod 前先讀原始碼、跑 `claude plugin validate` 看它掛哪些事件與能力；記住**通過 ≠ 安全認證**
- [ ] 個人電腦沒有 `sec-default`；要嚴格禁止的指令仍寫成權限規則（deny），不要靠 Blast Radius 這類文字比對的 Mod

## 原始摘要

兩篇同源（官方公告＋官方入門指南）的入門文合併：aiposthub（Philo，2026-10-03）偏重 Mods 與 Skills／設定檔 Hooks 的分工、三個官方示範與權限範圍；數位時代（李先泰，2026-10-08，初稿 AI 編撰）提出「模型層 vs App 層」框架，附 4 步驟安裝、自製與保存教學（含 Mod 會被自動清除、`--plugin-dir`）、`/diff` 與 `AGENTS.md` 是 Mod、社群 Mod 清單與安全提醒。數位時代文內夾帶的「圓剛科技」業配長文、訂閱區與熱門文章不收。

## 相關連結

- [[mcp-server-vs-mod-rewrite]] — 什麼該做成 Mod、什麼該留在 MCP：同一需求寫兩次的實測對照（API 形狀、validate 能力清單、能力最小化拆分）
- [[claude-code-plugins-guide]] — Mods 用 plugin 包裝分發；安裝、市集與外掛啟用層次
- [[steering-claude-seven-carriers]] — 「指令放哪個載體」框架；Mods 是 App 層的新一格
- [[claude-code-hooks-memory]] — 設定檔 Hooks 的機制（與 Mods 的 hooks 同名不同物）
- [[claude-code-mechanism-division-series]] — CLAUDE.md／Skills／Agents／settings+Hooks 四機制分工
- [[claude-code-five-layer-architecture]] — Claude Code 的五層架構，plugin 是分發層
- [[ai-skills-mcp-plugins-agents-comparison]] — Skills／MCP／Plugins／Agents 的定位比較
- [[agent-permission-and-os-sandbox]] — 同為「無沙箱、以你的身分執行」的權限問題
- [[agent-api-key-handoff-six-methods-risk]] — Mod 可讀環境變數與 API key，屬同一威脅面
- [[claude-code-agents-md-loading-rules]] — Claude Code 的 `AGENTS.md` 支援（據官方入門指南本身就是 Mod）
