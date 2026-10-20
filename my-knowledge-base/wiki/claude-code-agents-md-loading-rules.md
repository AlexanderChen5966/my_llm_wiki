---
title: Claude Code 讀取 AGENTS.md 的判定規則與兩個坑
date: 2026-09-23
tags: [ai/claude, ai/agent, type/how-to]
source: https://codelove.tw/@tony/post/aARpDq（原文 https://qiita.com/suwa_nobu/items/c795cf89d0fd4091c9cb）
type: article
status: processed
asset_value: 73
---

自 **Claude Code 2.1.277**（2026-09-18 changelog）起，專案內**若沒有 `CLAUDE.md` 就會改讀 `AGENTS.md`**。
作者以隔離安裝、`claude -p`、關鍵字探針實測 6 種檔案組合，證實官方判定表正確，
並挖出兩個**沒有任何錯誤訊息的靜默失效**。此條目直接推翻本庫 [[agents-md-guide]] 原有的「Claude Code 不會自動讀 AGENTS.md」斷言。

## T（核心論點）

🔑 **「Claude Code 不讀 AGENTS.md」這句話在 2.1.277 之前是對的，之後就不是了——既有的日文文章不是寫錯，是前提改變了。**

而真正會害人的不是判定表本身（那不難記），是**兩個沒有警告、沒有錯誤訊息的靜默失效**：

1. 🔴 **升級後的第一次 session 不會讀** — 剛升級就測一次、判定「沒效」的人會判錯
2. 🔴 **專案裡多一個 `CLAUDE.local.md`，團隊的 `AGENTS.md` 就默默消失** — `claude -p` 的紀錄裡沒有警告也沒有錯誤

🔑 加上 `AGENTS.md` **不會出現在 `/memory` 或 `/context`**（官方明載），
所以**唯一可靠的自我檢查方式就是在檔案裡放一個獨一無二的關鍵字，然後叫它把指示中的關鍵字列出來**。

## B（適用場景）

**適用：** 每開一個新專案、每次調整規則檔擺放方式；團隊共用 `AGENTS.md` 但個人想加私有備忘時；
懷疑「我的規則到底有沒有被讀到」的任何時候。

**不適用 / 注意：**
- ⚠️ 官方寫明 **Bedrock / Vertex / Foundry 尚未支援**（作者未親自驗證）
- ⚠️ **單一實測來源、非官方文件**：作者自陳每組僅測 2–3 次、未與 2.1.276 以前版本比較、
  未測子目錄 `AGENTS.md` / `@path` 匯入 / 傳播到子代理、未確認互動式 session 的顯示；
  「第一次不會讀」是在**新的設定目錄**中測得，是否與官方所說的「安裝後第一次」為同一機制**尚未確認**。
  作者還誠實記錄了測試中曾因自己腳本的 bug 產生假結果、後來重做的過程

## R（操作步驟）

### 判定表（2.1.277，6 種組合實測結果與官方表格一致）

| 專案裡放的檔案 | 實際讀到 |
|---|---|
| 只有 `AGENTS.md` | **AGENTS** ＋使用者層級 |
| `AGENTS.md` ＋ `CLAUDE.md` | **CLAUDE** ＋使用者層級（**AGENTS 沒有**）|
| `AGENTS.md` ＋ `CLAUDE.local.md` | **CLAUDE.local** ＋使用者層級（🔴 **AGENTS 沒有**）|
| `AGENTS.md` ＋ `.claude/rules/` | **AGENTS ＋ rules** ＋使用者層級（兩者都讀）|
| `AGENTS.md` ＋ `AGENTS.local.md` | **AGENTS** ＋使用者層級（**AGENTS.local 永不讀**）|
| 加上 `claude-md-and-agents-md` 設定 | **AGENTS ＋ CLAUDE** ＋使用者層級 |

🔑 **`~/.claude/CLAUDE.md` 不論任何組合都會被讀到，而且它的存在不算進「有 CLAUDE.md」的判定**——
也就是說使用者層級的 `CLAUDE.md` 不會把專案的 `AGENTS.md` 擋掉。`.claude/rules/` 同理不算在內。

### 要讓 `CLAUDE.md` 與 `AGENTS.md` 都被讀

```json
{
  "pluginConfigs": {
    "agents-md@builtin": {
      "options": { "instructionFiles": "claude-md-and-agents-md" }
    }
  }
}
```

🔴 **這段必須寫在 `~/.claude/settings.json`**。官方說明：**專案設定與本機設定都會被忽略**——
這是反直覺之處，照一般習慣寫進專案的 `.claude/settings.json` 不會生效。

### 自我檢查法（作者的測法，可直接照抄）

1. 在各檔案放入獨一無二的字串，例如
   `AGENTS.md` → `The AGENTS passphrase is AGENTS-ORANGE-7731.`；
   `CLAUDE.md` → `The CLAUDE passphrase is CLAUDE-VIOLET-4420.`
2. 請它「列出指示中所有的關鍵字」
3. 🔑 **把所有會讀檔的工具都禁掉**，否則即使指示裡沒寫，它也能自己把檔案打開，測出假陽性：

```
--disallowedTools "Read,Write,Edit,NotebookEdit,Bash,PowerShell,Glob,Grep,Task,WebFetch,WebSearch,Skill,ToolSearch"
--strict-mcp-config --mcp-config <空的 MCP 設定>
```

> 驗證環境：Windows 10 / 隔離安裝 Claude Code 2.1.277 / `claude -p` / 測試日 2026-09-19。
> Hacker News 上該討論 501pt（截至 2026-09-19）。

### 「第一次 session 不讀」的實測

在新的設定目錄只放 `AGENTS.md`，連續測 4 次：

```
Session 1   沒有讀到
Session 2   讀到 AGENTS
Session 3   讀到 AGENTS
Session 4   讀到 AGENTS
```

官方也明講：「It's your first session after you install or upgrade to a version with `AGENTS.md` support.
Claude reads `AGENTS.md` from your next session on.」

## C（關鍵行動）

- [ ] 升級 Claude Code 後**不要在第一次 session 就判斷規則檔有沒有生效**，至少開第二次
- [ ] 專案依賴 `AGENTS.md` 時，個人備忘**不要用 `CLAUDE.local.md`**（會讓團隊的 `AGENTS.md` 靜默消失）；
      改用 `.claude/rules/`（兩者都讀）或直接設 `claude-md-and-agents-md`
- [ ] 需要兩者都讀時，設定寫進 `~/.claude/settings.json`，**不要寫進專案設定**
- [ ] 想確認到底讀到什麼，用關鍵字探針——`/memory` 與 `/context` **看不到 `AGENTS.md`**
- [ ] 本庫自身以 `CLAUDE.md` 為規則載體，屬「有 CLAUDE.md」情形，**目錄下若放 `AGENTS.md` 不會被讀**

## 相關連結

- [[agents-md-guide]] — AGENTS.md 該寫什麼（設計原則、效果研究、跨工具對照）。
  🔴 該條目 C 層原有的「Claude Code 不會自動讀取 AGENTS.md」斷言已依本條目於 2026-09-23 修訂
- [[ai-agent-forbidden-tasks-agents-md]] — AGENTS.md 裡「禁止事項」的寫法
- [[codex-prompt-4-elements-agents-md]] — Codex 側的 AGENTS.md 使用脈絡（Codex 原生讀 AGENTS.md）
- [[claude-code-context-management]] — 規則檔載入與 context 成本的整體策略
- [[claude-code-file-management]] — 哪些 Claude Code 設定檔該 commit（`CLAUDE.local.md` 的定位）
- [[claude-md-guide]] — CLAUDE.md 本身的份量與寫法
- [[claude-code-mods-overview]] — 據官方入門指南（數位時代轉述），Claude Code 的 AGENTS.md 支援與 `/diff` 面板本身就是用 Mods 做的

## 原始摘要

單一來源條目（codelove.tw 轉載自 qiita/suwa_nobu，2026-09-20）。
作者另有兩篇相關實測（子代理是否拿到全部 CLAUDE.md、CLAUDE.md 該 21 個區段還是 8 行），本庫尚未收錄。
