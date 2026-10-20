---
title: Claude Cowork 設定指南：Local 自動化模式完整教學
date: 2026-06-15
tags: [ai/claude, pkm/workflow]
source: https://codelove.tw/@tony/post/3Qj5Ya
source_2: https://www.bnext.com.tw/article/91158/from-chat-to-cowork（數位時代 10 分鐘設定教學，2026-08-17：Chat vs Cowork 三個本質差異、Capabilities 設定、全域指示與 CLAUDE.md 分工）
type: article
status: processed
asset_value: 72
---

> ⚠️ **2026-09-16 已改版**：本篇整篇前提是 Cowork 為與 Chat/Code 並列的**獨立分頁/模式**，
> 但 Cowork 已併入一般對話 Chat，獨立分頁消失，介面縮減為 Claude 與 Claude Code 兩入口，
> 由 Claude 自行判斷該用哪種能力而非使用者自選分頁。**照本篇步驟找「Cowork 分頁」會找不到入口**，
> 詳見 [[claude-cowork-merged-into-chat-2026-09]]。

Claude Cowork 是 Claude Code 的本地自動化模式：把 AI 從「等你問」變成「主動幫你做」。透過排程、Connectors、記憶等機制，讓 AI 在你指定的時間自動讀取來源、執行任務、產出報告。

## T（核心論點）

> ⚠️ **2026-09-16 已改版**：下方表格「Claude Chat / Claude Code」vs「Claude Cowork」的兩欄對比前提已變——
> Cowork 不再是與 Chat/Code 並列的獨立分頁，能力已併入 Chat，詳見 [[claude-cowork-merged-into-chat-2026-09]]。

Cowork 的核心概念是**把手動 prompting 流程轉成自動化系統**。不是讓 AI 更強，而是讓 AI 不需要你在場也能執行。關鍵差異：

| 面向 | Claude Chat / Claude Code | Claude Cowork |
|------|--------------------------|---------------|
| 觸發方式 | 你問 AI 才動 | 排程自動執行 |
| 素材來源 | 你貼進來 | Connectors 自動拉取 |
| 跨對話記憶 | 無（每次重新開始） | memory.md 持久記憶 |
| 指令優先級 | 對話 > 預設 | Global Instructions > CLAUDE.md > 對話 |

## B（適用場景）

- 每日/每週自動摘要（新聞、競品、論文）
- 定期處理積壓素材（知識庫整理、raw/ 批次處理）
- 自動化報告生成
- 跨工具資料整合

## R（完整設定流程）

### 7 步驟設定

**Step 1：取得並設定 API Key**
- 前往 console.anthropic.com → Profile → API Keys
- 環境變數設定：`export ANTHROPIC_API_KEY=sk-ant-...`
- ⚠️ 有訂閱（Pro/Max）也需要獨立 API Key，兩者帳單分開

**Step 2：安裝 Claude Code**
```bash
npm install -g @anthropic-ai/claude-code
```

**Step 3：建立 Global Instructions（`~/.claude/CLAUDE.md`）**
- 全局規則，所有專案都生效
- 放**通用身份定義**：你是誰、偏好語言、輸出格式
- 範例：「你是我的知識庫管理員，負責整理 Markdown 筆記，語言用繁體中文」

**Step 4：建立專案 CLAUDE.md（每個專案資料夾）**
- 專案特定規則，比 Global Instructions 更具體
- 放**專案背景**：目錄結構、命名規範、任務路由表
- 此知識庫的 CLAUDE.md 即是範例

**分工原則：**
- `~/.claude/CLAUDE.md`：身份 + 全局偏好（對所有 Claude Code 工作都有效）
- `project/CLAUDE.md`：專案目錄結構 + 任務規則 + Skill 路由表

**Step 5：設定 Connectors（外部資料來源）**
Connectors 讓 Cowork 自動拉取外部資料，不需手動貼素材：
- **Telegram**：Telegram → Settings → Devices → Telegram for Desktop → Link
- **其他支援**：RSS、Email、Slack、自定義 webhook

**Step 6：設定 /schedule（任務排程）**
```
/schedule 每天早上 9 點執行 daily-brief-generator
/schedule 每週日晚上整理 raw/ 新素材
```
排程任務在你指定的時間自動觸發，不需要你在場。

**Step 7：建立 memory.md（持久記憶）**
```markdown
# 知識庫狀態
上次整理：2026-06-14
待處理 raw/ 筆記：12 篇
進行中專案：Sanlong App v2.0
```
放在 `~/.claude/memory.md` 或 `project/.claude/memory.md`，讓 AI 跨對話記住狀態。

### 三部分 Prompt 結構

每次給 Cowork 任務時，prompt 應包含三個部分：

```
1. 成品（What）：你要什麼
   「生成今日摘要報告，輸出為 Markdown 檔案到 outputs/」

2. 約束（How）：格式/規則/邊界
   「只處理 raw/ 下 status: inbox 的檔案，不修改 raw/ 內容」

3. 不確定時（When to ask）：遇到什麼情況要先問你
   「遇到無法判斷分類的文章，先列出讓我確認，不要猜」
```

## C（關鍵行動）

- [ ] 建立 `~/.claude/CLAUDE.md` 全局身份定義
- [ ] 在此知識庫設定 Telegram Connector，讓文章可直接傳給 AI 處理
- [ ] 設定週日 /schedule 自動執行 raw-pipeline
- [ ] 建立 `memory.md` 記錄知識庫狀態（待處理數量、上次整理日期）

## 相關連結

- [[loop-engineering-6-components]] — Loop Engineering 六元件（Cowork 的更深層設計原則）
- [[skill-md-9-types-and-gotchas]] — SKILL.md 九種類型（Cowork 裡的 Skills 怎麼寫）
- [[claude-code-billing-usage]] — Claude Code 計費與用量說明（API Key vs 訂閱的差別）
- [[claude-engineering-lead-workflow-principles]] — Claude 工程主導工作流原則

## 補充資料（來源：數位時代〈Claude Cowork 是什麼、怎麼用？新手 10 分鐘設定教學〉，2026-08-17）

補上原條目未涵蓋的**兩層規則分工**與 Capabilities 設定。

### Chat vs Cowork 的三個本質差異

| 差異 | Chat | Cowork |
|---|---|---|
| **處理檔案** | 上傳上限 **20 檔／單檔 500MB** | 直接讀寫本機實體檔案，**不受上傳件數與單檔大小限制** |
| **累積記憶** | 雲端 Memory，**有總量上限** | 寫進本機 `CLAUDE.md`（可隨時打開看／改／備份）|
| **下指令方式** | **一步一步陪它走** | **給結果與約束，走開讓它做** |

> ⚠️ 2026-10-08 更新：「處理檔案」列描述的是 10/6 以前的本機模式。10/6 起 Pro／Max 新任務一律雲端執行，本機檔案只能透過 Desktop App 存取已連結的資料夾，要用時才複製單檔上雲，而且 session 必須由桌面版發起或是排程任務。要完全在本機跑，官方建議改用 Claude Code。見 [[claude-cowork-merged-into-chat-2026-09]]。

🔑 **第三點最關鍵**：Chat 期待「先讀這個、然後抓那個、最後寫成段落」；Cowork 期待「產出 5 段、每段 100 字、存到 outputs 資料夾，不確定的標 TBD」。

🔑 **判斷標準一句話：你需要它「做完一整件事」還是「陪你討論」？** ⚠️ 但 **Cowork 比 Chat 更吃 token**，要注意方案限額。

比喻很傳神：**Chat 像需要頻繁寫入的黑板，Cowork 是可以堆疊資料的辦公桌**。而 100 張收據的例子說明落差——Chat 光上傳就卡住、回的只是對話框裡的文字還要自己複製到 Excel；Cowork 給資料夾路徑後直接產出格式化 Excel，**模糊看不清的還會自動標 VERIFY 提醒複核**。

✅ **既有的雲端 Skill 不需要重做**：Skill 綁 Claude 帳號，Chat／Cowork／Claude Code 三個介面都能呼叫，**差別只在 Cowork 還能呼叫本機檔案**。

### 🔑 兩層規則分工（本篇最實用的一節）

| 名稱 | 位置 | 內容性質 | 範例 |
|---|---|---|---|
| **全域指示**（Global Instructions）| Settings → Cowork → Edit | 跨所有 Cowork 對話的**普世規則** | 不刪檔、動作前列計畫、不要自動補圖、**不要用「視頻」這類中國用語** |
| **`CLAUDE.md`** | 資料夾根目錄 | 跟**這個工作環境**綁定的規則 | 你是誰、資料夾結構、產出該放哪個子資料夾 |

作者的例子很清楚：同一個 PPTX 任務，「蘋果風」資料夾的 `CLAUDE.md` 寫「白底黑字、無襯線、極簡」，「輝達風」寫「綠黑強對比、科技感、強調力量」→ **產出風格完全不同**；而「不要自動補圖」「動檔案前先問我」這類**跨資料夾都該守的規則寫進全域指示，寫一次涵蓋所有資料夾**。

⚠️ **全域指示與 Profile Instructions 是不同的方框**（後者在 Settings → General，管的是 Chat）。

可直接複製的全域指示範本：

```
動任何檔案前先列計畫，等我確認再執行。刪除、覆寫、改名檔案前一律問我。
預設輸出格式：docx 或 xlsx。不確定的地方明說，不要猜。
需要圖的地方留空白給我，不要自動補圖。
```

### Capabilities 設定四件事（Settings → Capabilities）

1. 開啟 **Generate memory from chat history**（Cowork 的雲端記憶，系統自動從對話提取偏好與習慣）
2. **Location metadata 可考慮關閉**（隱私）
3. Tool access 設為 **Load tools when needed**（需要時載入）
4. 其他功能全開；另有 **Import memory from other AI providers** 可繼承其他 AI 產品的記憶

### 進階：本地記憶檔 `memory.md`（不會自動載入）

⚠️ 與 `CLAUDE.md` 不同，**它必須被明確引用才會生效**：在資料夾根目錄建空白 `memory.md`，然後在 `CLAUDE.md` 加一行「**開始任何任務前，先讀 memory.md，套用裡面記錄的偏好與決策**」。

🔑 用法是**每次收工或踩到雷後，請 Cowork 把這次學到的東西記進 `memory.md`**——這正是 [[agent-memory-lifecycle]] 的 Consolidation 在產品層的手工版。

