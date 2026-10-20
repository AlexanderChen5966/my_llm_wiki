---
title: Claude Code Plugins 完整指南：六元件系統與 Marketplace 生態
date: 2026-05-19
tags: [ai/claude, ai/agent, pkm/automation]
source: https://frankknow.com/claude-code-plugins/
type: article
status: processed
asset_value: 70
---

Claude Code Plugins 是一個打包系統，把 Skills、Agents、Hooks、MCP Servers、LSP Servers、Monitors 六種元件整合成一鍵安裝的能力包。Plugin 比單獨的 Skill 涵蓋更廣——Skill 是「工作手冊」，Plugin 是「整個裝備齊全的工具箱」。

## T（核心論點）

Plugin 解決的是「跨專案複用整套能力」的問題。不是只有 Skill 一種元件，而是六種協作元件打包分發。安裝 Plugin 越多，每次啟動的 Token 消耗越高，應只安裝真正會用到的。

## B（適用場景）
**適用：** 需要一次安裝多種能力（MCP + Skills + Hooks 組合）；想把工作流分享給團隊；非工程師想快速啟用專業工具
**不適用 / 注意：** Plugin 數量過多會拖慢啟動速度；require remote MCP server 的 Plugin 等於把程式碼權限交給第三方，應避免

## R（操作步驟）

### 六大元件

| 元件 | 用途 | 特性 |
|------|------|------|
| **Skills** | 封裝 SOP 的可呼叫指令 | 手動 `/指令名稱` 或 Claude 自動觸發 |
| **Agents** | 獨立子代理人，處理超出範圍的任務 | 在自己的 context window 執行，回傳結果 |
| **Hooks** | 事件觸發器（存檔後自動修格式、commit 前自動檢查）| 設定後自動執行，不需手動觸發 |
| **MCP Servers** | 打通 Claude 與外部 API（GitHub、Slack、Figma）| 設定打包好，啟用 Plugin 即啟動 |
| **LSP Servers** | 語言智慧伺服器（自動補全、型別檢查、跳轉定義）| 語言伺服器本身需另外安裝 |
| **Monitors** | 背景監視器，觀察系統狀態並即時回傳給 Claude | 本地端運作，節省 Token |

### Plugin 目錄結構

必要資料夾是 `.claude-plugin/`（記錄 Plugin 基本資訊，Claude Code 安裝時優先讀取）：

```
my-plugin/
├── .claude-plugin/   ← 唯一必要資料夾（含 plugin.json）
├── skills/
├── agents/
├── hooks/
└── CLAUDE.md
```

### 安裝流程

```bash
# 官方 Marketplace 瀏覽
/plugins   # 進入 Manage Plugins 視窗

# 從 Marketplace 安裝
/plugin marketplace add owner/repo-name
/plugin install name@marketplace

# 官方目錄：claude.com/plugins
```

### 官方推薦 Plugin 速覽

| 領域 | Plugin | 用途 |
|------|--------|------|
| 前端開發 | Frontend Design | 生成有設計感的前端程式碼 |
| 前端開發 | Figma | 讀取設計稿轉換為程式碼 |
| Code Review | Code Review | 信心分數過濾 + 多代理審查 |
| Code Review | Security Guidance | 即時警告 Command Injection / XSS |
| 文件管理 | Claude.md Management | 管理 CLAUDE.md 確保 AI 記憶最新 |
| 文件管理 | Notion | 直接搜尋/建立/更新 Notion 頁面 |
| 行銷 | Postiz | 28+ 平台跨平台排程發文 |
| 專案管理 | Asana | 30+ 工具整合任務追蹤 |
| 資料擷取 | Firecrawl | 網站轉 Markdown / 整站爬取 |

### 自訂 Plugin 兩種方式

- **Standalone**：直接在專案目錄加 Skills/Agents/Hooks，適合單一專案
- **Plugins 打包**：整理成完整資料夾結構，適合跨專案重用或團隊分享

## C（關鍵行動）
- [ ] 只安裝真正會用到的 Plugin，每個 Plugin 都會佔用啟動 Token
- [ ] 選 Plugin 原則：先看官方目錄 → GitHub star > 200 → 絕對避免 require remote MCP server
- [ ] 自製 Plugin 可投稿至官方 Marketplace（`claude.ai/settings/plugins/submit`）；提交路徑與審核流程見補充資料

## 相關連結
- [[claude-code-five-layer-architecture]] — Plugin 是五層架構的第五層（分發層）
- [[claude-skills-guide]] — Skills 元件的設計原理與 Progressive Disclosure
- [[claude-skills-ecosystem]] — GitHub Skills 生態全景：16 個高 star Repo 分類
- [[claude-code-commands-2026]] — Plugin Marketplace 生態爆發與 Skills 統一架構
- [[agent-skills-vs-mcp]] — Skills vs MCP 的選擇判斷
- [[agent-plugins-open-standard]] — 外掛開放標準（plugin.json＋MCP servers＋Agent Skills），與 Plugin Bundle 的組成一致
- [[claude-marketplace-enterprise-spend-commitment-drawdown]] — 商務 Claude Marketplace（企業採購、承諾額度抵扣），與本篇的 `/plugin marketplace` 是不同層次
- [[claude-code-mods-overview]] — 2026-10 起 plugin 也用來包裝分發 Mods（改 Claude Code App 本身的擴充層）
- [[custom-gpt-gems-to-skills-migration]] — 自訂 GPT／Gems 遷移到 Skills 的跨廠商對照（2026-10-08 快照）

## 補充資料（來源：oh-my-claudecode GitHub）

### oh-my-claudecode（OMC）：Teams-first 多代理編排框架

**定位**：「Don't learn Claude Code. Just use OMC.」—— 韓國開發者 Yeachan-Heo 開源，以零學習曲線為目標，把多代理編排複雜度封裝在內。

**核心特性**：
- Teams-first 設計：預設假設多代理協作場景，而非單一 Agent 模式
- 零學習曲線：不需要深入了解 Claude Code 底層機制，直接使用封裝好的工作流
- 多代理編排：處理 Agent 間通訊、任務分配、結果回傳的複雜度

**與 Claude Code Plugins 的關係**：OMC 是建立在 Claude Code Agents 元件之上的上層封裝，把多代理協作的設定複雜度抽象化，類似 Plugin 的「一鍵啟用整套能力」理念，但更專注在代理編排而非個別工具整合。

**適用場景**：熟悉多代理概念但不想手動設定每個 Sub-agent 的開發者；希望快速啟用 Teams 模式的場景。

### 補充資料（來源：Moksa〈Claude Code Plugins 7 大元件與 6 個實戰範例〉）

**Namespace 命名衝突處理**：Plugin 用 Namespace 區隔各元件，避免不同 Plugin 的同名 Skill/Agent/指令互相覆蓋——這是 standalone `.claude/` 設定難以跨團隊共享的痛點之一，Plugin 打包後可版本控制、可透過 Marketplace 一鍵安裝。

**生態系現況（早期）**：目前 Plugin 生態尚早，`settings` 支援有限，跨 Plugin 的設定管理仍在演進。

**⚠️ 安全風險（重要）**：第三方 Plugin 的 hooks 與 bin 可執行任意指令——安裝等於把本機的命令執行權限交給 Plugin 作者。呼應本條目 C 層「絕對避免 require remote MCP server 的 Plugin」原則，安裝前應確認來源可信（官方目錄 / GitHub star 門檻 / 檢視 hooks 與 bin 內容）。

**6 個實戰範例類別**：前端開發、Code Review、文件管理、行銷排程、專案管理、資料擷取——與本條目「官方推薦 Plugin 速覽」表對應，說明各領域 Plugin 的實際串接方式。

### 補充資料（來源：aiposthub〈Claude 開放外掛提交入口〉，2026-09-27）

> ⚠️ 原文**沒有官方公告連結、沒有寫開放日期**；網址中的「110x」在正文中找不到說明；附圖標為示意。以下為作者轉述的官方說法，可信度要打折。

- **提交資格**：付費 Claude 方案的開發者
- **兩種形式**：① 單一遠端 MCP 連接器；② Plugin Bundle（MCP 伺服器＋Agent Skills，託管在 GitHub repo）——組成與 [[agent-plugins-open-standard]] 的定義一致
- **流程**：自動驗證＋安全掃描 → 可看審核狀態、回饋與修正建議 → 通過後由開發者決定上架時間 → 分析看板（各產品介面的安裝分布、版本分布、目錄頁瀏覽、搜尋詞）
- Claude Code 外掛還可以包含指令、Hooks、Agents、LSP，但不是每個 Claude 介面都支援
- 作者建議：最小權限；用乾淨帳戶測試安裝；區分「提出建議」與「實際執行」；Bundle 裡的 Skills 要當成產品一起維護

**三種「marketplace」名詞釐清：**

| 名稱 | 是什麼 | 見 |
|------|------|------|
| 商務 Claude Marketplace | 企業採購入口，合作夥伴支出抵扣 Anthropic 承諾額度 | [[claude-marketplace-enterprise-spend-commitment-drawdown]] |
| Claude 外掛目錄與提交入口 | 開發者提交 MCP 連接器／Plugin Bundle，審核後上架 | 本段 |
| Claude Code `/plugin marketplace`、組織自建的內部外掛 marketplace | 外掛分發與安裝機制 | 本條目 R 層「安裝流程」 |

### 補充資料（來源：aiposthub〈Claude Code You Should Know 是什麼？官方外掛幫你抓住長任務裡的重要訊息〉，Philo，2026-10-03）

- **You Should Know**：Anthropic 官方內建外掛，會掃描 Claude 的輸出，把長任務裡容易漏看的重要訊息提出來。2026-10-03（台灣時間）由 ClaudeDevs 官方 X 公布。
- **啟用**：`/plugin enable cc-plugin-you-should-know@builtin`。要保留 `@builtin`（內建來源），避免裝到同名的第三方套件。
- **定位是閱讀輔助**：官方沒有提供提醒準確率，也沒宣稱能抓到所有錯誤，所以**提醒 ≠ 驗收、沒提醒 ≠ 沒問題**（呼應 [[agent-ux-status-transparency-maturity]]「狀態要可驗證」）。
- **外掛的三個層次**：「設定中啟用／電腦上已安裝／目前工作階段已載入」是三件不同的事，打完指令還要確認真的載入；啟用範圍分使用者、專案、本機（官方 plugins 文件，作者轉述）。
- 原文**沒說**它是用 Mods 做的，本庫不把它寫成 Mod。同為官方外掛的另一例見 [[claude-code-security-guidance-plugin]]。

> ⚠️ 2026-10-08：2026-10 起 plugin 也用來包裝分發 Mods（App 層擴充：常駐在 App 內、能記狀態、能畫介面），見 [[claude-code-mods-overview]]。
