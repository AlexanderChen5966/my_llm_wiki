---
title: Claude Code 設定檔架構與 Commit 策略
date: 2026-05-14
tags: [ai/claude]
source: https://israynotarray.com/ai/20260331/1748203647/
type: article
status: processed
asset_value: 68
---

Claude Code 的設定分四層（Managed/User/Project/Local），記住一個原則即可：放在專案目錄裡給團隊共用的就 commit，帶 `local` 字樣或在 Home 目錄的就不 commit。

## T（核心論點）
**該 commit 的**：`.claude/settings.json`、`.claude/rules/`、`.claude/skills/`、專案 CLAUDE.md
**不該 commit 的**：`.claude/settings.local.json`（個人覆蓋設定）
**不在 Repo 裡的**：`~/.claude/` 下的所有東西、Auto Memory

## B（適用場景）
**適用：** 團隊使用 Claude Code 時的 Git 管理規範；新人接手專案時的設定說明
**不適用：** 個人獨立專案（沒有隊友的顧慮）

## R（操作步驟）

### 四層設定架構
| 層級 | 誰設定 | 作用範圍 | 是否 commit |
|------|--------|----------|------------|
| Managed | IT/DevOps | 整個組織 | N/A |
| User | 你自己 | `~/.claude/` 所有專案 | 不在 Repo |
| Project | 團隊 | `.claude/` 這個專案 | ✅ 是 |
| Local | 你自己 | `.claude/settings.local.json` | ❌ 否 |

### CLAUDE.md 讀取順序（優先權從低到高）
1. `~/.claude/CLAUDE.md`（User 層）
2. 專案根目錄 `CLAUDE.md`（Project 層）
3. 子目錄 `CLAUDE.md`（動態載入，只在 Claude 讀取該目錄時才載入）

Settings 陣列值是**合併**不是覆蓋，個人允許的指令和團隊允許的指令會同時生效。

### .gitignore 只需加一行
```gitignore
.claude/settings.local.json
```

## C（關鍵行動）
- [ ] 個人偏好（語言、風格）寫在 `~/.claude/CLAUDE.md`，不要動專案的
- [ ] CLAUDE.md / Rules 絕對不要放 API Key，改用環境變數
- [ ] `claude init` 會自動把 `settings.local.json` 加入 `.gitignore`

## 相關連結
- [[claude-code-context-management]] — Context 管控策略
- [[claude-code-auto-memory]] — Auto Memory 的儲存位置與管理（`~/.claude/projects/<路徑>/memory/`，非專案 `.claude/`）
- [[agents-md-guide]] — AGENTS.md 與 CLAUDE.md 的關係
- [[claude-code-disable-mouse-clicks]] — 關掉終端滑鼠攔截（環境變數）

## 補充資料（來源：Claude Code 哪些檔案該 Commit？CLAUDE.md、Settings、Rules、Skills 完整解析）

### CLAUDE.md 讀取順序（完整版）

假設在 `my-project/packages/api/` 底下執行 Claude Code，它會依序載入：

1. Managed 層級（公司 IT 設定，如果有的話）
2. `~/.claude/CLAUDE.md`（使用者層級）
3. `my-project/CLAUDE.md`（專案根目錄）
4. `my-project/packages/CLAUDE.md`（中間目錄，如果存在）
5. `my-project/packages/api/CLAUDE.md`（執行命令的目錄）

所有檔案合併使用，下層覆蓋上層的衝突規則（Settings 的陣列值是**合併**不是覆蓋）。

### Rules 系統（`.claude/rules/`）

Rules 是更細緻的規則管理，支援 `paths` 屬性做路徑限定：

```yaml
# .claude/rules/backend.md
paths:
  - "src/api/**"
  - "src/services/**"
---
這個規則只在 Claude 操作 src/api/ 或 src/services/ 時才載入。
```

路徑限定規則可以大幅降低 CLAUDE.md 的長度，把不同模組的規則分層管理。

### Skills 的 commit 策略

`.claude/skills/`（專案層 Skills）**應該 commit**——這樣團隊成員也能使用。`~/.claude/skills/`（個人層 Skills）是你的私人 Skill，**不在 Repo 裡**。

### 完整的 commit 決策表

| 路徑 | Commit? | 原因 |
|------|---------|------|
| `CLAUDE.md`（專案根目錄）| ✅ | 團隊共用的專案規範 |
| `.claude/settings.json` | ✅ | 團隊共用設定 |
| `.claude/rules/` | ✅ | 團隊共用規則 |
| `.claude/skills/` | ✅ | 團隊共用 Skills |
| `.claude/settings.local.json` | ❌ | 個人覆蓋設定（加入 .gitignore）|
| `~/.claude/CLAUDE.md` | ❌ | 個人層，不在 Repo 裡 |
| `~/.claude/skills/` | ❌ | 個人私房 Skills |
| `~/.claude/projects/*/memory/` | ❌ | Auto Memory，machine-local |

**安全提醒**：CLAUDE.md 和 Rules 絕對不要放 API Key 或 secret，改用環境變數。`claude init` 會自動把 `settings.local.json` 加入 `.gitignore`。

## 補充資料：Home 目錄那排 `.claude.json.tmp` 是什麼、可以刪嗎（2026-08-05 併入）

來源：israynotarray.com〈Home 目錄一堆 .claude.json.tmp 檔案？可以刪嗎？〉。補上本條目未涵蓋的**設定檔寫入機制與殘骸清理**。

### 檔名的四段結構

```
.claude.json.tmp.3188.1774732246449
└─ 目標檔     └tmp └PID └毫秒 Unix 時間戳
```

| 段 | 意義 |
|---|---|
| `.claude.json` | 它正要覆蓋的目標（全域設定檔）|
| `tmp` | 暫存檔標記 |
| `3188` | 產生它的 Claude Code **PID** |
| `1774732246449` | 毫秒級時間戳 |

換算時間：`date -r 1774732246 '+%Y-%m-%d %H:%M:%S'`（macOS）／`date -d @1774732246`（Linux）——換算結果會與 `ls -l` 的修改時間對得起來，**光看檔名就知道是哪天、哪個 process 留下的**。

### 為什麼要多一個暫存檔：原子寫入（atomic write）

直接開舊檔清空再寫，寫到一半當機就會變成半殘檔（新的沒寫完、舊的回不來）。所以 Claude Code 採「**先寫暫存檔，再改名蓋掉舊檔**」。

**殘骸產生時機**：直接關終端機視窗／`Ctrl+C` 強制中止／當機或強制重開。

| 殘骸大小 | 死在哪一步 |
|---|---|
| **0 bytes** | 檔案剛開好、內容還沒寫進去就被中斷（或斷電在資料落地前）|
| **80–120 KB** | 內容整份寫完了，**只差最後那步 `rename` 沒跑到** |

### 怎麼檢查與清除

```bash
ls -lh ~/.claude.json.tmp.* | wc -l   # 先看幾個
rm -f ~/.claude.json.tmp.*            # 可以刪
```

> ⚠️ 萬用字元只吃 `.claude.json.tmp.` 開頭的檔案，`~/.claude.json` 本身與 `.backup` 都不在範圍內。**官方特別提醒不要刪 `~/.claude.json`**——登入狀態與偏好設定都在裡面。

### 旁邊那幾個親戚

| 檔案 | 管什麼 | 可以刪嗎 |
|---|---|---|
| `~/.claude.json` | 全域設定檔本體（登入狀態、偏好、個人 MCP）| 🔴 **不要刪** |
| `~/.claude.json.tmp.<PID>.<時間戳>` | 原子寫入沒收尾的殘骸 | ✅ 可以 |
| `~/.claude.json.lock` | 同一套寫入流程的鎖檔，避免多 session 同時寫壞設定 | ✅ 正常會自己消失，殘留可刪 |
| `~/.claude.json.backup` | 舊版留在 Home 根的設定備份 | ✅ 可以 |
| `~/.claude/backups/.claude.json.backup.<時間戳>` | 現行備份機制（設定遷移前快照）| ✅ 官方歸在「刪掉不會損失任何使用者看得到的東西」|

**為什麼 Claude Code 自己不清**：官方的 `cleanupPeriodDays` 機制（預設 30 天）只清特定路徑；另官方更新日誌已把設定備份從 Home 根目錄移到 `~/.claude/backups/` 以減少雜亂。
