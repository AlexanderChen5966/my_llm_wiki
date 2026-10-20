---
title: Claude Code 命令速查：三層體系完整參考
date: 2026-05-14
tags: [ai/claude]
source: https://tools.wingzero.tw/article/sn/3824
type: reference
status: processed
asset_value: 69
---

Claude Code 命令體系分三層：終端主命令（開啟程式）→ 終端子命令（管理功能）→ Slash 命令（控制會話）。掌握結構後，不需要背命令，知道去哪層找即可。2026 版已大幅擴充至 13 大類 Slash 指令。

## T（核心論點）
用對話能做的事，用指令通常更快、更確定。`/compact` 一秒執行，對話要先解釋意圖；`/rewind` 確定倒帶，對話可能誤解「剛才」的時間點；有些指令（如 `/tui`、`/terminal-setup`）根本無法用對話替代。

## B（適用場景）
**適用：** 需要快速查找指令的速查表用途
**不適用：** 了解新命令設計理念（見 [[claude-code-commands-2026]]）

## R（三層命令體系）

### 第一層：終端主命令（開啟程式）
```bash
claude                    # 進入互動式會話
claude -p "任務"          # 非互動模式，執行後退出（適合腳本）
claude -c                 # 繼續最近一次會話
```

### 第二層：終端子命令（管理功能）
```bash
claude auth               # 帳號認證管理
claude mcp                # MCP server 管理
claude plugins            # Plugin 安裝/更新管理
claude config             # 設定（editor mode、theme 等）
```

### 第三層：Slash 命令（13 大類）

**對話與 Context 管理**
| 指令 | 說明 |
|------|------|
| `/btw <問題>` | 側邊問題，不寫入對話歷史 |
| `/compact [焦點]` | 壓縮對話歷史釋放 context，可指定保留重點 |
| `/context [all]` | 視覺化 context 用量 |
| `/clear [名稱]` | 清空開新對話（別名：`/reset`、`/new`）|
| `/recap` | 生成當前工作階段單行摘要 |

**程式碼變更**
| 指令 | 說明 |
|------|------|
| `/diff` | 互動式 diff 檢視（左右鍵切換）|
| `/rewind` | 倒帶到上一個 checkpoint（別名：`/undo`）|
| `/branch [名稱]` | 建立對話分支實驗不同方向（別名：`/fork`）|
| `/security-review` | 偵測注入、驗證、資料洩露風險 |

**任務規劃與自動化**
| 指令 | 說明 |
|------|------|
| `/plan [描述]` | 進入 Plan Mode |
| `/ultraplan` | 瀏覽器檢視草稿再決定執行 |
| `/goal [條件]` | 跨回合持續工作直到條件達成 |
| `/schedule` | 建立雲端排程例行工作 |
| `/autofix-pr` | 監視 PR，CI 失敗自動修復 |

**並行執行**
| 指令 | 說明 |
|------|------|
| `/batch <instruction>` | 自動拆解並行執行，每個開 PR |
| `/agents` | Agent 管理員入口 |
| `/tasks` | 管理背景工作（別名：`/bashes`）|

**模型與設定**
| 指令 | 說明 |
|------|------|
| `/model` | 切換模型 |
| `/config` | 修改設定（取代 `/vim`）|
| `/effort` | 調整努力程度滑塊 |

**Skills 管理**
| 指令 | 說明 |
|------|------|
| `/plugins` | 瀏覽/管理 Plugin |
| `/plugin marketplace add` | 加入 marketplace |
| `/plugin install name@marketplace` | 安裝 plugin |

## C（快捷鍵）
- `Shift+Tab × 2`：切換 Plan Mode（先規劃再動手）
- `Ctrl+C`：中斷當前操作
- `Ctrl+R`：搜尋歷史指令
- `Ctrl+U`：清空整個輸入緩衝區（注意：2026 版行為變更）
- `Ctrl+O`：切換詳細日誌（2026 版行為變更）

## 補充視角：官方完整命令清單（2026-05 版）

來源：數位時代完整版（bnext.com.tw），補充原速查表未收錄的指令：

**補充命令（原速查表未收錄）：**

| 類別 | 指令 | 說明 |
|------|------|------|
| 對話管理 | `/export [檔名]` | 匯出對話為純文字 |
| 對話管理 | `/copy [N]` | 複製最後 N 個回覆；`w` 鍵寫入檔案 |
| 程式碼變更 | `/add-dir <路徑>` | 新增額外工作目錄（僅本次 session）|
| 程式碼變更 | `/review [PR]` | 本地 PR 審閱（深度審閱用 `/ultrareview`）|
| 並行執行 | `/background [提示]` | 分離為背景 agent，別名 `/bg` |
| 並行執行 | `/stop` | 停止附加的背景 session |
| Session 管理 | `/rename [名稱]` | 重命名當前 session |
| Session 管理 | `/resume [session]` | 按 ID 或名稱繼續對話 |
| Session 管理 | `/desktop` | 移至 Claude Code 桌面應用（macOS/Windows）|
| Session 管理 | `/teleport` / `/tp` | 把網頁版 session 拉入終端機 |
| Session 管理 | `/remote-control` / `/rc` | 允許從 claude.ai 遠端控制 |
| Session 管理 | `/sandbox` | 切換 sandbox 模式 |
| 模型設定 | `/fast [on\|off]` | 切換快速模式 |
| 介面 | `/theme` | 更換色彩主題（含色盲無障礙）|
| 介面 | `/color [顏色]` | 設定本次 session 提示列顏色 |
| 介面 | `/tui [fullscreen]` | 切換 UI 渲染器（fullscreen 無閃爍）|
| 介面 | `/focus` | 焦點檢視（僅最後提示 + 最終回應）|
| 介面 | `/statusline` | 配置狀態列內容 |
| 記憶體與權限 | `/memory` | 編輯 CLAUDE.md + 自動記憶體管理 |
| 記憶體與權限 | `/permissions` | 管理允許/詢問/拒絕規則 |
| 外部整合 | `/ide` | 管理 IDE 整合 |
| 外部整合 | `/chrome` | 配置 Claude in Chrome |
| 外部整合 | `/voice [hold\|tap\|off]` | 語音聽寫模式 |
| 外部整合 | `/setup-bedrock` | Amazon Bedrock 互動式設定精靈 |
| 外部整合 | `/setup-vertex` | Google Vertex AI 互動式設定精靈 |
| Skills | `/skills` | 列出可用 skills；`t` 依 token 排序 |
| Skills | `/reload-plugins` | 重新載入所有 plugins |
| 診斷 | `/doctor` | 診斷安裝；`f` 自動修復 |
| 診斷 | `/heapdump` | 堆快照寫入桌面（診斷高記憶體使用）|
| 診斷 | `/usage` | session 成本 + 使用限制 + 統計 |
| 診斷 | `/insights` | 分析工作階段的互動模式與摩擦點 |
| 團隊 | `/team-onboarding` | 從 30 天使用歷史產生入職 Markdown 指南 |
| 其他 | `/powerup` | 互動式課程探索 Claude Code 功能 |

**補充快捷鍵（原速查表未收錄）：**
- `Ctrl+D`：結束 Claude Code（無法重新綁定）
- `Ctrl+T`：顯示/隱藏工作清單
- `Ctrl+J`：插入換行（不送出）
- `Ctrl+L`：強制全螢幕重繪；兩秒內按兩次執行 `/clear`
- `Ctrl+G` / `Ctrl+X Ctrl+E`：在外部編輯器開啟輸入內容
- `Ctrl+S`：暫存當前提示
- `Meta+P`：開啟模型選擇器
- `Meta+O`：切換快速模式
- `Meta+T`：切換延伸思考模式
- `Ctrl+X Ctrl+K`：終止本 session 所有背景子 agent

## 相關連結
- [[claude-code-commands-2026]] — 2026 新增命令設計理念與 Skills 架構
- [[claude-code-context-management]] — `/compact`、`/clear` 的使用策略
- [[claude-pilot-suite]] — 三模式執行手冊
- [[ideas/claude-code-stuck-triage-claude-status]] — ⚠️ `/status` 看的是帳號與計費，不是服務狀態；Claude Code 卡住時的錯誤字樣分流與 status.claude.com

## 補充資料（來源：Claude Code快捷鍵+指令大全！13大類速查不用背，從Ctrl+C到多Agent協作一次整理.md）

### 為什麼要學指令（不只靠對話）

**效率**：`/compact` 一秒執行，對話需要先理解意圖再確認再執行

**確定性**：`/rewind` 就是倒帶，對話說「回到剛才那個版本」可能誤解「剛才」

**無法替代**：`/tui fullscreen`、`/terminal-setup`、`/setup-bedrock` 這類改變 CLI 本身設定的指令，必須由使用者明確輸入，無法靠口語觸發

快捷鍵更是「對話做不到的事」——介面層操作（選取、送到外部編輯器）Claude 根本無法透過回覆執行。

### 13 大類完整指令（數位時代整理版）

**一、對話與上下文管理**：`/btw`（側邊問題不汙染主線）、`/compact`（壓縮釋放）、`/context`（視覺化用量）、`/clear`（清空開新對話）、`/recap`（單行摘要）、`/export`（匯出純文字）、`/copy`（複製回覆，按 `w` 寫入檔案）

**二、程式碼變更與版本追蹤**：`/diff`（互動式差異，左右鍵切換）、`/rewind`（倒帶到上一個 checkpoint）、`/branch`（建立對話分支）、`/add-dir`（新增工作目錄）、`/security-review`（偵測安全風險）、`/review`（本地 PR 審閱）

**三、任務規劃與自動化**：`/plan`（Plan Mode）、`/ultraplan`（瀏覽器草稿）、`/goal`（跨回合持續目標）、`/schedule`（雲端例行工作）、`/autofix-pr`（CI 失敗自動修復）

**四、並行執行與 Agent 管理**：`/agents`（Agent 管理員）、`/tasks`（背景工作管理）、`/background`（分離為背景 Agent）、`/stop`（停止背景工作階段）

**五、工作階段管理**：`/rename`、`/resume`、`/desktop`（移至桌面 App）、`/teleport`（網頁版拉進終端機）、`/remote-control`（允許遠端控制）、`/sandbox`

**六、模型與效能設定**：`/model`（切換模型）、`/effort`（努力程度 low/medium/high/xhigh/max/auto）、`/fast`

**七、介面與個人化**：`/config`、`/theme`、`/color`、`/tui`（渲染器切換）、`/focus`（焦點檢視）、`/statusline`、`/keybindings`

**八、記憶體與權限管理**：`/memory`（CLAUDE.md + Auto Memory）、`/permissions`（工具白名單）、`/init`（初始化專案）

**九、外部整合**：`/mcp`、`/ide`、`/chrome`、`/voice`（語音聽寫）、`/setup-bedrock`、`/setup-vertex`

**十、Skills 與 Plugins**：`/skills`（列出可用 Skills，按 `t` 依 Token 排序）、`/plugin`、`/reload-plugins`

**十一、診斷、費用與帳戶**：`/doctor`（診斷，按 `f` 自動修復）、`/usage`（成本 + 使用限制）、`/insights`（互動模式分析）、`/heapdump`（記憶體診斷）

**十二、團隊協作**：`/team-onboarding`（從 30 天歷史生成入職文件）

**十三、其他**：`/help`、`/powerup`（互動式課程）、`/release-notes`、`/feedback`

---

## 補充資料（來源：Claude Code 命令大全 2.1.74 版｜DeTools 工具翼零）

### 三層命令體系核心說明

**三層的本質**：
- **第一層**（終端主命令 `claude`）：**開啟程式** 的入口
- **第二層**（終端子命令 `claude mcp`、`claude plugins` 等）：**管理功能** 的工具
- **第三層**（Slash 命令 `/help`、`/model` 等）：**控制會話** 的操作

記住這個結構，就不會在「要打 claude mcp add 還是 /plugin install」之間搞混。

### 第二層：終端子命令（v2.1.74 完整版）

```bash
claude auth                # 帳號認證管理（login/logout/status）
claude mcp                 # MCP server 管理（add/remove/list）
claude plugins             # Plugin 安裝/更新管理
claude config              # 設定（editor mode、theme 等）
claude update              # 更新 Claude Code 版本
claude doctor              # 診斷安裝問題

# 非互動模式（適合 CI/CD 腳本）
claude -p "任務描述"       # 執行後退出
claude -c                  # 繼續最近一次會話
claude --dangerously-skip-permissions  # 跳過確認（謹慎使用）
```

### 常見混淆釐清

| 困惑 | 正確理解 |
|------|---------|
| 為什麼有些功能要在外面打，有些要在裡面打？| 外面（子命令）管理整個環境設定，裡面（Slash）控制當前會話 |
| `/plugins` 和 `claude plugins` 差在哪？| `/plugins` 是會話內瀏覽 UI，`claude plugins` 是終端管理工具 |
| `/vim` 不見了？| v2.1.92 移除，改用 `/config` → Editor mode 設定 |
| `/pr-comments` 不見了？| v2.1.91 移除，直接對話「幫我看 PR #1234 的 comments」|

### 新手必記 5 個 Slash 命令

| 指令 | 用途 | 說明 |
|------|------|------|
| `/help` | 查看所有可用命令 | 必須定期打，命令一直在增加 |
| `/clear` | 開新對話（alias: `/reset`、`/new`）| Context 滿了就用這個 |
| `/compact` | 壓縮對話歷史 | 不確定要不要 clear 時用這個 |
| `/model` | 切換模型 | 複雜任務換 Opus，快速任務換 Haiku |
| `/memory` | 管理 Auto Memory | 查看/開關/瀏覽記憶檔案 |

## 補充資料：一段提示詞批次重新命名歷史對話（2026-08-04 併入）

來源：〈Claude Code 對話批次重新命名〉。**僅限 Claude Code**（讀取 session 管理工具），Claude.ai 網頁版不支援。

**批次改名提示詞**（貼給 Claude Code）：
> 檢查你目前可以管理的對話，根據每則對話的開始日期、主要討論對象與實際內容，重新命名對話標題。標題必須方便日後搜尋、辨識與分類。有明確對象：YYYYMMDD｜相關對象｜主題名稱。無明確對象：YYYYMMDD｜主題名稱。日期必須使用該對話「建立」的日期。若對話內容不足以判斷主題、日期，在標題加上「？」，方便識別。

**執行流程**：Claude 取得對話清單（session ID/現有標題/建立時間/內容）→ 讀內容判定主題與對象（優先順序：品牌/系統/公司/組織/課程/專案）→ 主題名稱用具體動作、控制在 8–12 個中文字 → 避免與其他標題重複 → 執行改名並回報成功/失敗清單。

**設定每週自動排程**：直接說「幫我設每週一自動整理對話標題」，用 `/schedule` 建立；新開的對話會在下次排程時一併重新命名，不需手動記得跑。

## 補充資料：五個既有清單漏掉的操作細節（2026-08-07 併入）

來源：〈實用的 12 個 Claude Code 技巧〉（thisweb.dev）。該篇 12 項中有 11 項本頁已涵蓋，此處只收**本頁原本沒有的五個細節**。

### 1. `/compact` 可以指定優先保留什麼

```
/compact 保留所有已決定的資料庫 schema 設計
```
指令後加自訂說明，告訴模型哪些資訊需要優先保留。**對長達數小時的大型重構任務特別關鍵**——預設壓縮可能丟掉你最在意的那段決策。

### 2. `Shift + Tab` 三種權限模式的完整行為對照

| 模式 | 行為 |
|---|---|
| `default` | **只能讀取**，每個操作都需詢問確認 |
| `acceptEdits` | 可自動讀取與編輯檔案，減少確認中斷 |
| `plan` | 只能讀取（如 default），但**不會嘗試執行任何動作** |

→ Plan mode 下 Claude 只讀取與分析 codebase、不寫任何程式碼，可安全探索現有架構、確認改動範圍，來回調整計畫後再切執行模式。

### 3. `Ctrl+G` 搭配一個容易漏掉的設定

`Ctrl+G` 開啟外部編輯器下 prompt。🔑 **搭配 `/config` → 搜尋 `Show last response in external editor` 勾選後**，用 `Ctrl+G` 開編輯器時 Claude 會**把上一輪的對話也放進編輯器**，方便對照它的回覆來改 prompt。

### 4. `/recap` 可設為自動觸發

`/recap` 掃描目前對話歷史，生成精簡的狀態描述——回到暫停的 session 時快速想起「上次做到哪裡」，不必從頭捲回對話。
🔑 **可在 `/config` 開啟自動 recap**，回到 session 時自動執行，不必每次手動輸入。

### 5. `/simplify`（本頁原本未收錄）

品質導向的清理指令——審查已變更的程式碼，找出可重用、可簡化、效率與抽象層級的問題並套用修正。**只做品質，不獵 bug**（獵 bug 用 `/code-review`）。

### 原文的分層總結（值得記的框架）

> 這 12 個指令代表 Claude Code 在**不同層次的控制能力**：
> - **管理對話狀態**：`/btw`、`/rewind`、`/fork`
> - **調配模型資源**：`Option+P`、`/model`、`/effort`
> - **管理 context**：`/compact`、`/memory`
> - **提升使用效率**：`!` 前綴、`Shift+Tab`、`/simplify`、`/remote-control`、`Ctrl+G`

## 補充資料：🔴 兩個環境變數的組合陷阱，會讓任務清單完全消失（2026-08-20 併入）

來源：https://codelove.tw/@tony/post/amZKgq （Windows 10／Claude Code 2.1.234 實測，`--model opus` 解析為 `claude-opus-5`）

症狀：交給 Claude Code 的多步驟工作**不再顯示檢查清單**，而且是**按工具別整個消失**。

### 官方 changelog 只講了一半

2.1.233 的說明是：

> Todo/task-tracking tools (TaskCreate/Get/Update/List, TodoWrite) are no longer available on **Opus 4.8、Sonnet 5、Fable 5、Mythos 5 及更新的模型**；
> set `CLAUDE_CODE_ENABLE_TODO_TOOLS=1` to bring them back

⚠️ **看起來像「因為是新模型」就結束了，但實測顯示同樣是 Opus 5，只要環境變數不同還是能用**——真正關掉它的是 `CLAUDE_CODE_ENABLE_TASKS=0`。

### 四行實測表（`claude -p` 非互動模式下詢問可用工具）

| 模型 | `ENABLE_TASKS` | `ENABLE_TODO_TOOLS` | 可用工具 |
|---|---|---|---|
| Opus 5 | 未設定 | 未設定 | `TodoWrite` |
| Haiku 4.5 | 未設定 | 未設定 | `TaskCreate / Get / List / Update` |
| **Opus 5** | **0** | 未設定 | 🔴 **無**（兩邊都消失）|
| Opus 5 | 未設定 | **1** | `TaskCreate / Get / List / Update` |

### 🔑 為什麼會兩邊都消失

官方文件對這個變數的定義是：

> On other models, Claude Code provides the Task tools by default and `TodoWrite` **only when you set `CLAUDE_CODE_ENABLE_TASKS=0`**.

所以 `ENABLE_TASKS=0` 的原意是「**把 Task 工具關掉，改成用 TodoWrite**」。但——

> 🔴 **新模型預設就沒有 Task 工具。這時再加上「把 Task 工具關掉」這個設定，等於對一個本來就不存在的東西做切換，結果 TodoWrite 也不會出現——最後兩邊都消失。**

⚠️ **排查提醒**：該變數**不在 `settings.json` 裡**，是**從父行程繼承**的（所以查設定檔會找不到原因）。
解法是設 `CLAUDE_CODE_ENABLE_TODO_TOOLS=1`。

🔑 **這是一個典型的「兩個各自合理的設定組合出非預期結果」案例**——與 [[agent-permission-and-os-sandbox]] 的
`['python']` 事件（同意一句指令變成放行整個直譯器）同屬「設定的語義沒有隨模型世代更新」這類陷阱。

