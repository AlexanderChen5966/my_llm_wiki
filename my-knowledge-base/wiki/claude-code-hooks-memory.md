---
title: Claude Code Hooks + Memory：自動觸發與跨對話長期記憶
date: 2026-05-08
updated: 2026-09-17
tags: [ai/claude, ai/workflow, pkm/automation]
source: https://medium.com/@n913239/hooks-memory-讓-claude-code-自動反應-長期記憶-bc9e107116e8
source_2: https://ithelp.ithome.com.tw/articles/10403774（《觀察 AI，也觀察自己》Day 19：規則檔與 Hooks 的分界、讀取回條、OpenClaw 郵件事故）
type: article
status: processed
asset_value: 86
---

## T（核心論點）

CLAUDE.md 的本質限制是**靜態**——寫進去的規則每次都會被看到，但 Claude 不會「主動做事」，也不會「記住上次說了什麼」。Hooks 與 Memory 各解決一件：**Hooks 讓特定動作觸發特定指令（規則被執行）**，**Memory 讓資訊跨對話保留（Claude 了解你）**。

三層設定機制：**CLAUDE.md 告訴 Claude 規則 → Hooks 讓規則被執行 → Memory 讓它了解你。**

## B（適用場景）

**適用：** 有「每次都要提醒 Claude 做同一件事」的重複動作；需要跨對話保留個人偏好與工作背景；想設安全守門員攔截危險指令

**不適用 / 注意：**
- Hooks 是 shell 層強制，**寫錯會影響每次工具呼叫**——建議用自然語言請 Claude 產生設定，別手寫 JSON
- Memory **不是什麼都該存**（見下方判準）；可從程式碼推斷或 git log 查到的都不該存

## R（兩套機制）

### Part 1：Hooks

設定在 `settings.json`（全域 `~/.claude/settings.json` 或專案 `.claude/settings.json`）。

**四種事件**，最常用 `PostToolUse`（等 Claude 做完某件事，再自動執行你的指令）。
`matcher` 對應工具名（`Bash`／`Edit`／`Write`／`Read`／`Glob`／`Grep`），**不填 matcher = 攔截所有工具**。

**資料怎麼傳進來**：Claude Code 透過 **stdin 傳 JSON**，用 `cat` 讀、`jq` 解析：
```bash
FILE=$(cat | jq -r '.tool_input.file_path // empty')
```
PostToolUse + Edit 的 stdin JSON 結構：
```json
{ "hook_event_name": "PostToolUse", "tool_name": "Edit",
  "tool_input": { "file_path": "...", "old_string": "...", "new_string": "..." } }
```
另有環境變數 `CLAUDE_PROJECT_DIR`、`CLAUDE_PLUGIN_ROOT`；**但工具資料本身只走 stdin**。

#### 🔴 PreToolUse 的 exit code 語意（最容易搞錯）

> **只有 exit code 為 `2` 才會中止那個工具呼叫**，並把 hook 的 **stderr 當成錯誤訊息反饋給 Claude**。
> 其他非 0 exit code **只是非阻擋性錯誤，工具仍會繼續執行**。

安全守門員範例（攔截危險指令）：
```bash
CMD=$(cat | jq -r '.tool_input.command // empty')
if echo "$CMD" | grep -qE '(rm -rf|DROP TABLE|DELETE FROM)'; then
  echo '⛔ 危險指令，已阻擋。請確認後手動執行。' >&2; exit 2
fi
```

#### 實用範例四種

| 事件 | 用途 |
|---|---|
| `PostToolUse` + `Edit\|Write` | 改完 `.swift` 自動跑 SwiftLint |
| `PostToolUse` + `Bash` | 記錄所有 Bash 指令到 log |
| `Stop` | macOS 桌面通知（`osascript -e 'display notification ...'`）——可離開螢幕，等通知響再回來 |
| `Notification` | 把通知轉發到 Telegram bot |

#### 補充配方：壓縮後自動重新提醒規矩（2026-09-17 查核後補入）

來源：《Claude Code Hooks 教學：讓 AI 自動守規矩的 5 個實用配方》（brickverse.com.tw，2026-03-12）。
該篇共 5 個配方，逐一比對後：改完自動排版、完成通知你一聲兩項與本表既有的 SwiftLint／桌面通知屬同一模式（僅換了工具與通知管道），保護敏感檔案一項本頁「Hooks 的定位」段落已寫過「Agent 準備改 `.env` 時 Hook 在動作發生前擋下」，皆判定重複，不重貼。

唯一真正的增量是**上下文壓縮後重新提醒規矩**這個具體配方，本頁先前只在「規則檔是提醒不是鐵門」段落談過 compaction 會遺失規則的問題（OpenClaw 事故），未給出可直接照搬的設定寫法：

> 設定：「每次上下文被壓縮後，自動重新提醒以下規矩——本專案用 Bun、port 是 3004、部署前跑測試。」

用途：長對話被自動壓縮後，Claude 可能忘記專案規矩；用一個 hook 在 compaction 後把關鍵規矩重新塞回 context，比事後發現遺漏再手動提醒更可靠。

> 💡 **設定建議**：比起手動編輯 `settings.json`，直接說「幫我加一個 PostToolUse hook，當 Claude 修改 Swift 檔案後自動跑 SwiftLint」——Claude 會正確處理 JSON 格式、路徑、引號跳脫。

### Part 2：Memory

**位置**：`~/.claude/projects/{project-path}/memory/`，每條記憶一個獨立 `.md`，透過 `MEMORY.md` 當索引。

#### 四種記憶類型

| 類型 | 存什麼 |
|---|---|
| **user** | 你是誰——讓 Claude 調整解釋深度與溝通方式（iOS 老手不需要解釋 `@StateObject`）|
| **feedback** | 🔴 **最重要的一種**：每次你糾正 Claude 的行為就存一條，下次不用再說 |
| **project** | 正在進行的事、背景脈絡、時效性資訊（合併凍結、某 bug 的根本原因、下週發版）|
| **reference** | 記住「**資訊放在哪裡**」而非資訊本身（Linear project key、監控 dashboard URL、設計稿位置）|

> `feedback` 與 `project` 特別建議用 **Why** 與 **How to apply** 兩段補脈絡——讓下次讀到時能**判斷邊界情況**，而不只是盲目遵守一條規則。

#### ⚠️ MEMORY.md 索引的載入限制

> `MEMORY.md` 每次 session 開始時自動載入，**前 200 行或 25KB，以先達到者為準**。
> Claude 靠它決定哪些記憶跟當前任務相關，需要時才去讀個別完整檔案。
> → **索引要保持簡短（每行一條，不超過 150 字）；記憶本身的長度影響不大。**

#### 什麼值得存 / 不值得存

| ✅ 值得存 | ❌ 不值得存 |
|---|---|
| 糾正過 Claude 但**與程式碼無關**的行為（「不要在回答結尾加總結」）| **可從程式碼推斷**的事（架構、命名規範 → 放 CLAUDE.md）|
| 現在的工作背景但**git log 裡看不到**（「這個重構是因為 legal 要求，不是技術 debt」）| **Git 歷史裡有**的事（誰改了什麼、為什麼改 → 看 git log）|
| 外部資源的位置（設計稿在 Figma Team Library）| 只在**這次對話**有用的暫時資訊 |

**主動操作**：`記住：這個專案的 API key 都存在 Keychain，不要用 UserDefaults` ／ `忘掉關於合併凍結的記憶，已經解凍了`（會自動找到檔案刪除並更新 MEMORY.md）

#### 🆕 Sub-agent 的獨立 Memory

在 agent frontmatter 設 `memory` 欄位即啟用，**三種 scope**；**官方建議預設用 `project`**，讓 agent 學到的東西能透過 git 與團隊共用。

```yaml
---
name: code-reviewer
description: 審查程式碼品質與 best practices
memory: user
---
```
- **名字不同的 agent，memory 目錄完全隔離**——`code-reviewer` 累積的知識不會跟 `ios-auditor` 混在一起
- 主 Claude 的 auto memory 是**另一套系統**，與 agent memory 互不相關
- 可在 agent system prompt 寫進記憶指令：「每次完成審查後，把發現的 patterns、架構決策、常見問題記錄進你的 memory」

### Memory vs CLAUDE.md 的分工

> **CLAUDE.md 是寫給所有人的規則，Memory 是屬於你個人的記憶。**

## C（關鍵行動）

- [ ] 找出「每次都要提醒 Claude」的重複動作，改成 `PostToolUse` hook
- [ ] 設一個 `PreToolUse` 安全守門員（記得 **exit code 必須是 2** 才會阻擋）
- [ ] 用自然語言請 Claude 產生 hook 設定，不要手寫 JSON
- [ ] 每次糾正 Claude 後存一條 **feedback 記憶**，並補上 **Why / How to apply**
- [ ] 保持 `MEMORY.md` 索引精簡（每行 < 150 字，總量 < 200 行／25KB）
- [ ] 用 sub-agent 時考慮開 `memory: project`，讓累積知識能與團隊共用

## 補充資料：官方對「三層機制」的定性（2026-08-03 併入）

來源：TechNews（徐知誼，2026-07-29）〈我們知道 Claude Opus 5 很厲害了，怎麼用才划算？〉（https://technews.tw/2026/07/29/claude-opus-5/ ），整理自官方 memory / hooks 文件。
⚠️ 該篇的自動記憶儲存位置、200 行／25KB 載入限制、`/memory` 管理方式等內容，與本頁及 [[claude-code-auto-memory]]**已飽和不重複**。只收一句話定性——它直接證成本頁的核心論點：

> **CLAUDE.md 與自動記憶都在每次對話開始時載入，但兩者都被視為「上下文參考」而非「強制設定」。要在任何情況下硬性擋下某個動作，得改用 [PreToolUse hook](https://code.claude.com/docs/en/hooks-guide)。**

這是官方文件層級對「advisory vs mandatory」的明確劃線，補強本頁「Hooks 讓規則被執行」的定位。配套的**排查順序**（它沒照做時）：

1. 先跑 `/context`（或 `/memory`）確認檔案**到底有沒有被載入**
2. 有載入 → 再檢查指示是否**夠具體**、有無**跨檔衝突**
3. 若是「必須在特定時機執行」的規則 → **本來就不該寫在 CLAUDE.md，該改寫成 hook**

一句話分工：**每次都該記得的事實 → CLAUDE.md；反覆使用的流程 → Skill；讓它從你的更正中自學 → 自動記憶；要在特定時機硬性強制 → hook。**

## 常見問題

- **Hook 失敗會怎樣**：`PostToolUse` / `Stop` 失敗只記錄錯誤，**不影響主流程**；`PreToolUse` 只有 exit 2 才阻擋
- **同一事件可設多個 hook**：可以，依序執行
- **Hook 指令可以換行嗎**：JSON 裡不能，用 `&&` / `;` 串接，或寫成 shell script 再呼叫

## 原始摘要

MikeWang 的 Claude Code 系列連載第 4 篇（Medium，2026-05-08）。系列脈絡：CLAUDE.md（前一篇）→ Hooks + Memory（本篇）→ MCP 實戰（下一篇）。作者定位：這兩個機制是讓 Claude Code 從「聰明的助手」升級成「真正理解你的工作夥伴」的兩塊拼圖。


## 補充資料：規則檔是提醒不是鐵門（2026-08-19 併入）

來源：《觀察 AI，也觀察自己》Day 19。這一篇把本條目的「強制力光譜」補上了**一個具名事故**與**一個可操作的驗證技巧**。

### 🔴 具名事故：OpenClaw 直接刪信

2026 年 2 月，**Meta 負責 AI Alignment 的主管 Summer Yue** 分享的親身經歷：

> 她原本只請 Agent **建議**哪些信可以刪除或封存，**Agent 直接動手刪信，連她後來要求停止也沒有理會。**

她的推測是：主要信箱裡的資料太多，**觸發了 Context compaction，可能讓「先確認、不要直接動手」這項指令被漏掉**，而使用者也可能沒把這個提醒寫進 `AGENTS.md`／`CLAUDE.md`。

🔑 **這正好把本條目的兩個機制串起來**：規則檔能讓長期規則**重新進入** Context（對抗 compaction 遺失），**但它仍然不是鐵門**——
像「刪信前一定要確認」這類要求，還需要權限限制、確認步驟或 Hook。

> **規則寫得像聖旨，信箱還是可能被當成換季清倉。**

（Compaction 會遺失資訊這件事見 [[context-isolation-and-compaction]]；「Prompt 不是安全邊界」的規格層論證見 [[agent-control-layer-permission-hooks-planning]]）

### 🔑 讀取回條：先確認它「有讀到」，再檢查它「有做到」

最常見的問題**不是規則寫得不夠兇，而是檔名、位置或工具根本沒有對上**。所以在規則檔裡加一句：

```markdown
若你已讀完這份規則，開始回覆時先說：「吉伊卡哇!」
```

看到那句話，**至少表示 Agent 有讀到這條規則**。沒出現就先檢查：用的是 `AGENTS.md` 還是 `CLAUDE.md`？有沒有打錯字？
檔案在專案根目錄嗎？目前的工具支援這個格式嗎？

⚠️ **這不是安全證明，只是第一個可觀察訊號。** 但它把「規則沒生效」這個模糊問題，切成了「沒讀到」與「讀到沒照做」兩個可分別排查的問題。

### 規則檔該寫什麼：先回答四個問題

不要一開始就寫成《AI 員工守則大全》。對初學者有用的規則檔只要回答：**這是什麼專案？怎麼開始？哪些事不要做？怎樣才算完成？**

🔑 **本篇最好的一個對比**：

> 「不要修改 `.env`」和「不要自行加功能」這兩句，常常**比「請寫出高品質程式」有用得多**。
> 後者像對新同事說「請表現優秀」；前者才是**能照著做的交代**。

判準也很直接：**如果你發現自己每次都對 Agent 說同一句話（例如「不要順手重寫整個網站，我只要你改按鈕」），那句話就適合搬進規則檔。**

### ⚠️ 規則檔不是雜物抽屜（含 OpenAI 官方教訓）

不該放的：API Key／密碼／私人資料（當然）、**今天才有用的臨時任務**、整段聊天紀錄、還沒確認的猜測。

判準對照：「今天先把首頁標題改短」是一次性任務 → 留在對話；「修改首頁後要檢查手機版」→ 值得長期保留。

> 否則規則檔最後會像**那個大家都不敢打開的廚房冷凍庫**。

🔑 **OpenAI 在分享 Codex 的 harness engineering 時提到：曾經把大量知識集中在一份巨大的 `AGENTS.md`，結果效果不好。
後來改成讓它保持精簡，像一張地圖；需要深入了解時再引導 Agent 去看 README、設計文件或其他專門文件。**

因為規則檔內容會進 Context——**塞得太長，就像新同事第一天收到一千頁手冊：真正重要的內容反而容易被埋住**。
（這與 [[claude-md-optimization-techniques]] 的總量取捨、[[agent-skills-vs-mcp]] 的 Progressive Disclosure 同一原理）

專案變大後才需要分層：根目錄放大家都要遵守的規則，特定資料夾再放更具體的說明——
**「大樓有總樓層指南，進實驗室才要看實驗室規範，不需要在電梯裡背完所有房間的注意事項。」**

### ⚠️ 一個歸屬釐清：那份「Karpathy 的 CLAUDE.md」不是他本人的

GitHub 上的 `forrestchang/andrej-karpathy-skills` 提供了一份可參考的 `CLAUDE.md`，但**它不是 Andrej Karpathy 本人公開的私人設定檔**——
比較準確的說法是：**專案作者根據 Karpathy 對 LLM 寫程式常見問題的觀察所整理的四項守則**。

那四項守則本身仍有價值：**① 先想清楚再動手 ② 先用簡單方法 ③ 只改這次要改的地方 ④ 最後確認是否真的完成。**

作者最推的是第三項，因為 **「AI 有時候像很熱心的室友：你請它換一顆燈泡，它順手把客廳重新粉刷、調整家具位置，還把你以為不用的插座拆掉。」**
可直接抄進規則檔的寫法：

```markdown
只修改這次任務需要的地方；若發現其他問題，先提出，不要順手一起改。
```

🔑 **名人的範本可以當起點，但最後應該留下的規則，還是要從你自己的專案、自己的痛點長出來。**

### AGENTS.md 與 CLAUDE.md 的分工（一句話版）

```
多數支援 AGENTS.md 的 Coding Agent  → 看 AGENTS.md
Claude Code                        → 看 CLAUDE.md
同一個專案同時使用兩者              → 共用內容寫在 AGENTS.md，CLAUDE.md 再引用它
```

⚠️ Claude Code 官方文件明載它讀的是 `CLAUDE.md` **不是** `AGENTS.md`。（此即 `app01-double` 專案採用的架構：`CLAUDE.md` 用 `@AGENTS.md` import 共用規則）

### Hooks 的定位（本條目主體的再確認）

Hook 觸發時機很多，初學者先記兩個：**工具執行前**（阻擋危險操作）與**工具執行後**（整理格式或檢查結果）；
其餘還有 Session 開始、等待使用者回覆、**Context compaction 前後**。

最生活化的例子：Claude Code 等你回覆時讓電腦跳通知（不用盯著 Terminal）。
保護檔案的例子：Agent 準備改 `.env` 時 Hook 在動作發生前擋下——

> **這不像只貼一張「請不要進機房」，而是多加一道門禁。**
> ⚠️ **但規則必須涵蓋 Agent 可能使用的所有修改方式——不能把一扇門鎖好，卻忘了旁邊還有窗戶。**

（後面這句與 [[agent-permission-and-os-sandbox]] 的「字串檢查擋不住自己開檔案的腳本」完全同構）

🔑 **還有一類很適合自動化的工作**：每次修改檔案後自動整理格式、結束前跑固定檢查——
**這些事不需要 AI 判斷得多有智慧；電腦照規則做，反而比較可靠。**

## 相關連結

- [[claude-md-optimization-techniques]] — **關鍵分工依據**：CLAUDE.md 是 advisory、Hooks 才是 mandatory（shell 層強制）；「屢次違反的規則轉成 Hook」是官方修法
- [[claude-5-context-engineering-rules]] — 官方指出自動記憶與手寫 CLAUDE.md「互補而非取代」，本篇給出更細的分工判準
- [[claude-code-cost-optimization-7-tips]] — **PreToolUse hook 過濾冗長輸出**（數萬 token → 數百）是省 token 最有感的一招，機制同本篇
- [[claude-code-git-workflow]] — 同系列；commit 格式偏好正是 feedback 記憶的典型用例
- [[agentmemory]] — 第三方跨 agent 共享記憶方案（本篇是原生機制）
- [[claude-code-context-management]] — Context 管控；四大消耗源與 `/clear` 的實際行為
- [[claude-code-auto-memory]] — ⚠️ **分工**：該篇談產品功能的操作面（在哪、怎麼載、怎麼關、怎麼改路徑、三分流）；本頁談**記憶的內容策略**（四種類型、什麼值得存、Sub-agent 獨立 memory）
- [[synthesis/synthesis-claude-code-internals]] — 本篇為其來源之一；該精粹把本篇的 `exit 2` 與其他五篇的強制力說法整合成一條完整**強制力光譜**（可忽略／被說服的／硬保證／mandatory）
- [[claude-code-mods-overview]] — 設定檔 Hooks 在 App 外部跑完就結束；Mods 常駐在 App 內，能記狀態、能畫介面（⚠️ 官方也把 Mods 的事件處理函式叫 hooks，不要混淆）
- [[mcp-server-vs-mod-rewrite]] — 同作者後續：MCP 與 Mod 的選擇判準

