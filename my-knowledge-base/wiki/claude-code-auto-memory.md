---
title: Claude Code Auto Memory：綁「這個專案 + 這台電腦」的自動記憶
date: 2026-08-12
tags: [ai/claude, pkm/workflow]
source: https://ithelp.ithome.com.tw/articles/10400396（Claude Code Auto Memory 是什麼？自動記憶功能完整教學）
type: article
status: processed
asset_value: 72
---

**2026-09-03 從 [[claude-code-context-management]] 拆出**：原本是該條目的「補充資料」段，但 Auto Memory 是一個獨立的產品功能主題（定位／儲存／載入／管理／版本／自訂路徑／三分流），而該條目本身是 49 個入連結的 Context 操作樞紐——兩個主題混在一篇會讓讀者找不到入口。拆出後兩篇各自聚焦，並與 [[claude-code-hooks-memory]] 分工（見文末）。

## T（核心論點）

Auto Memory 讓 Claude 在對話過程中**自動記下**你的偏好，下次開對話不必重複說明。它與 `CLAUDE.md` 的根本差異不是「內容不同」，而是**由誰寫、綁在哪個範圍**：

> 🔑 **CLAUDE.md 是你寫給所有人的規則；Auto Memory 是 Claude 寫給「這個專案 + 這台電腦」的筆記。**

而最容易踩的一句話判準是：

> 🔑 **這件事要不要傳給別人？**

## B（適用場景）

**適用：** 想讓 Claude 記住本機踩過的雷、build 指令、個人偏好；判斷一條規則該進 Auto Memory 還是 `CLAUDE.md`；需要關掉或改變記憶儲存位置。

**不適用 / 注意：**
- 🔴 **Auto Memory 綁「這個專案 + 這台電腦」，是 machine-local、不同步到雲端或其他機器**——重要的、要跨機器帶著走的東西**不能只押在 Auto Memory 上**。
- 🔴 **在對話裡說「記住 X」，預設一律進 Auto Memory（它自己的筆記本），不會自動去改你的 `CLAUDE.md`**——想讓它進 `CLAUDE.md` 必須明講「把這個加進 CLAUDE.md」。
- ⚠️ 需要 **v2.1.59 以上**（`claude --version` 確認）。

## R（操作步驟）

### 一、與 CLAUDE.md 的五維對照

| 比較維度 | CLAUDE.md | Auto Memory |
|---------|-----------|-------------|
| 由誰撰寫 | 你自己 | **Claude 自動寫入** |
| 內容類型 | 指示和規則 | 學到的經驗和模式 |
| 適用範圍 | 專案/使用者/組織 | **每個 Working Tree** |
| 載入方式 | 每次對話全部載入 | **只載入前 200 行或 25KB** |
| 適合用在 | 程式碼規範、工作流程 | Build 指令、Debug 心得、個人偏好 |

**使用原則**：確定不變的規範（程式碼風格、命名規則）寫 `CLAUDE.md`；個人化的偏好和踩過的雷交給 Auto Memory。

### 二、儲存位置

Auto Memory **不在**專案的 `.claude/` 資料夾，而是在 Home 目錄：

```
~/.claude/projects/<專案路徑（以 - 分隔）>/memory/
├── MEMORY.md          # 索引檔，每次對話都會載入（限前 200 行或 25KB）
├── debugging.md       # Debug 相關筆記（按需載入）
├── api-conventions.md # API 設計決策（按需載入）
└── ...                # Claude 自己建立的其他主題檔案
```

🔑 路徑根據 **Git Repo 推導**，同一 Repo 底下的**子目錄和 Worktree 共用同一份記憶**。記憶是 **machine-local**，不同步到雲端或其他機器。

### 三、🔴 載入機制的 200 行限制（是硬截斷，不是建議）

`MEMORY.md` 只有**前 200 行或前 25KB** 在每次對話開始時載入（**取小的那個**）。Topic 檔案（`debugging.md` 等）不會全部載入，而是 Claude 需要時才讀取——這樣記憶越多也不會每次都爆 context。

> 🔴 **超出 200 行的部分是靜默截斷，且此行為未寫進官方文件**（[GitHub issue #25006](https://github.com/anthropics/claude-code/issues/25006)，最終 closed as not planned）。
> ⚠️ **這個 200 行跟 `CLAUDE.md` 的 200 行完全是兩件事**——`CLAUDE.md` 不論多長都整份載入，它的 200 行是**成本目標值**而非門檻（詳見 [[claude-md-guide]] 的來源歸屬更正）。把兩者混用是本庫曾出現過的實際錯誤。

→ **索引要保持簡短**（每行一條、不超過 150 字）；記憶本身的長度影響不大。

### 四、管理 Auto Memory

- **查看**：`/memory` 指令 → 選擇記憶資料夾
- **主動記住**：「記住：一律用 pnpm 不要用 npm」
- **主動忘記**：「忘記之前關於 pnpm 的規則」（會自動找到檔案刪除並更新 `MEMORY.md`）
- **手動編輯**：直接到 `~/.claude/projects/` 編輯 Markdown 檔案
- **停用**（三種方式）：`/memory` 開關／設定 `"autoMemoryEnabled": false`／環境變數 `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`

### 五、`/compact` 不影響 Auto Memory

存在磁碟的記憶 **compact 後仍保留並重新注入**。

> ⚠️ 但這不代表 compact 什麼都不丟——**帶 `paths:` 的 rules 與子目錄 `CLAUDE.md` 會先遺失**。完整的保留／遺失清單見 [[claude-code-harness-internals]]，判斷「長對話該不該重開」的取捨見 [[claude-code-context-management]]。

### 六、自訂儲存位置

在 `settings.json` 指定：

```json
{
  "autoMemoryDirectory": "~/my-custom-memory-dir"
}
```

🔑 **只接受 Policy、Local、User 層級，不接受 Project 層級**——刻意的設計，**防止共用專案的惡意設定把你的記憶導向錯誤位置**。

### 七、🔑 三分流判斷法：Auto Memory vs 個人全域 CLAUDE.md vs 專案 CLAUDE.md

新手最容易把「用講的叫它記住」「個人全域 `CLAUDE.md`」「專案 `CLAUDE.md`」三者搞混。一句話判斷法——**這件事要不要傳給別人？**

| 這件事… | 放哪裡 | 為什麼 |
|---|---|---|
| 要傳給同事看的規矩（commit 格式、命名慣例、哪些資料夾別動）| **專案 `CLAUDE.md`** | 進 git，同事 clone 後也讀得到 |
| 「我這個人」不管換哪個專案都要的習慣（回中文、先結論後解釋）| **個人全域 `~/.claude/CLAUDE.md`** | 跟著你、不跟著專案 |
| 只是不想讓「這台電腦」的 Claude 再忘（踩過的雷、本機跑測試要先開的服務）| **Auto Memory**（用講的讓它記） | 不用進版控 |

**🔴 關鍵陷阱（重申）**：說「記住 X」預設進 Auto Memory，不會改 `CLAUDE.md`；且 Auto Memory 綁「這個專案 + 這台電腦」，**換機器不會跟過去**。

## C（關鍵行動）

- [ ] 想讓 Claude 記住一件事之前先問：**這件事要不要傳給別人？** 要 → `CLAUDE.md`；只是這台電腦的事 → 用講的就好
- [ ] 要進 `CLAUDE.md` 的必須**明講**「把這個加進 CLAUDE.md」，否則它只會寫進自己的筆記本
- [ ] 保持 `MEMORY.md` 索引精簡（每行一條、< 150 字），別讓它撞到 **200 行靜默截斷**
- [ ] 引用「200 行」時分清楚是 `MEMORY.md`（硬截斷）還是 `CLAUDE.md`（成本目標值）
- [ ] 重要且要跨機器的東西**別只押在 Auto Memory**——它不同步
- [ ] 用 `/memory` 定期看一遍它到底記了什麼（記憶不是黑盒子，是可讀可改可刪的短檔案）

## 相關連結

- [[claude-code-context-management]] — **本篇的母條目**（2026-09-03 拆出）：Context 消耗來源、六成存檔 → `/clear` 重開、`/clear` 不是清空、以及「長對話要重開」的世代反轉注記
- [[claude-code-hooks-memory]] — ⚠️ **分工說明**：該篇談**記憶的內容策略**（四種記憶類型 user/feedback/project/reference、什麼值得存、Sub-agent 獨立 memory 與三種 scope）；**本篇談產品功能的操作面**（在哪、怎麼載、怎麼關、怎麼改路徑、三分流）。兩篇的 200 行／25KB 載入限制為同一事實
- [[agent-memory-lifecycle]] — 🔑 **理論對應**：Selection / Extraction / Recall / Consolidation 四層；本篇的**三分流判斷法即該篇 Scope 概念的產品化**
- [[claude-md-guide]] — 🔴 **200 行的來源歸屬查證**：硬截斷屬 `MEMORY.md`、`CLAUDE.md` 的 200 行是成本目標值而非遵守率門檻
- [[claude-code-harness-internals]] — compaction 的完整保留／遺失清單（Auto Memory 保留、`paths:` rules 先遺失）
- [[claude-code-session-handoff]] — 第三種工具：當下進度用一次性交接檔，不要寫進常駐的 `CLAUDE.md` 或 Auto Memory
- [[steering-claude-seven-carriers]] — 官方七種載體：本篇的三分流對應其中「個人偏好用使用者級」那一格
- [[cowork-memory-browser-scheduled-tasks]] — 對照 Claude Chat／Cowork 側的記憶（Topics 亦為可讀可改的短檔案，但那套只適用雲端執行的任務）
- [[external-memory-layer-exploration-tax]] — ⚠️ **更上層的設計問題**：記憶該不該屬於模型周圍的系統、信任分級（記憶是報告不是指令）、年齡影響信心而非存在
- [[concepts/agent-handoff-memory-patterns]] — 跨 session／跨工具狀態傳遞五模式中「Framework 持久化」那一類的產品實例
