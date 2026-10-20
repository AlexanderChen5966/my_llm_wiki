---
title: Fable 批次 D 報告 — GitLab b2b-manager 多工具設定檔
date: 2026-07-08
tags: [ai/claude, project/b2b-manager, pkm/workflow]
type: reference
status: processed
---

# 批次 D 報告：b2b-manager 多工具設定檔一致性

> 執行：2026-07-08（Fable 5）｜範圍：根目錄四份設定檔（CLAUDE/AGENTS/GEMINI/DESIGN）× docs/ai-collaboration/ 8 份 × .claude/rules/ 7 份 × .codex/
> 結論：**核心架構（@import 單一維護點）落實良好、三方規則無實質矛盾；主要問題是「Gemini→agy 換代造成的文件時代斷層」（需你決策）+ 1 個版本遺留副本 + 3 個小項**
>
> ✅ **修正狀態（2026-07-08，使用者決策：GEMINI.md 留作歷史紀錄、AGENTS.md 採索引引用）**：
> - D-1 ✅ `AGENTS.md` v1.2 新增「UI 開發前置（所有工具通用）」段——索引引用 + on-demand 讀取 DESIGN.md，一次覆蓋 agy/Codex/Copilot；並註明 AGENTS.md 無標準 import 機制（`@path` 為各工具私有語法）、文字指引為跨工具最穩妥做法。三入口策略統一：CLAUDE.md（06-23 起）與 AGENTS.md（07-08 起）皆索引引用；GEMINI.md 硬 import 成為歷史
> - D-1/D-3 ✅ `agents-gemini-restructure.md` 與 `design-md-migration.md` 各補「2026-07-08 附記」：Gemini CLI 06-18 停止服務、agy 讀 AGENTS.md 為主、GEMINI.md 保留為歷史紀錄不再維護、DESIGN token 供給改由 AGENTS.md 索引段
> - D-2 ✅ AGENTS.md 尾戳更新（1.0/04-24 → 1.2/07-08，含版本沿革）
> - D-4 ✅ `ai-pair-main/版本狀態說明.md` 建立（標明 v3.1.0 歷史開發版、現行 4.3.5 位置、保留原地理由）
> - D-5 ✅ CLAUDE.md 驗收題 typo 修正（`pub upgrade`）
> - D-6 ✅ mcp-setup.md 加「暫緩」狀態註記（含 agy 時效提醒）；handoff 文件版本行更新為 v4.3.5

---

## 🟠 D-1｜Gemini 端敘述的三個時代斷層（需人工決策，本批最重要）

三份文件對「Gemini 工具端」的描述停在三個不同時點，彼此矛盾：

| 文件（時點）| 敘述 |
|------|------|
| `agents-gemini-restructure.md`（04-28）| Gemini CLI 讀 `GEMINI.md`（一行 @AGENTS）|
| `design-md-migration.md` 06-23 更新 | GEMINI.md **維持** `@./DESIGN.md`，理由：「Gemini CLI 沒有 rules 自動載入機制，硬 import 是唯一保證」|
| `cross-tool-ai-setup.md`（06 月版）| **Gemini CLI 已於 2026-06-18 停止服務**，接替者 `agy` 以 **AGENTS.md 為主要來源、GEMINI.md 可省略** |
| `dev-workflow-handoff…`（06-22）| gemini-reviewer 底層**已改用 agy**（v4.3 遷移完成）|

**矛盾核心**：06-23 的 GEMINI.md 決策理由建立在 **06-18 已停止服務**的 Gemini CLI 上。團隊實際已轉 agy（handoff 文件證實），而 agy 讀 AGENTS.md 為主——AGENTS.md 對 DESIGN.md 只有「文件索引引用」沒有 import，**當初想避免的「Gemini 端拿不到 DESIGN token」缺口，換代後轉移到了 AGENTS.md 層**。

**需你決策的兩件事**：
1. `GEMINI.md` 的去留：留著給 Antigravity 相容（agy 仍會讀但非必要）或直接省略？
2. agy 時代 DESIGN token 的供給路徑：靠 AGENTS.md 索引 on-demand（同 Claude 模式）就夠？還是 AGENTS.md 也要加 UI 前置指引段（同 CLAUDE.md 的做法）？

**建議修法**（保守版）：AGENTS.md 加一段「UI 開發前置：先讀 DESIGN.md」（比照 CLAUDE.md 專屬補充，讓 agy/Codex/Copilot 都受益）；GEMINI.md 保留不動（向後相容零成本）；restructure 文件補一行 agy 時代附記。

---

## ⚠️ 需修正（小項）

### D-2｜AGENTS.md 尾戳過時
尾標「最後更新 2026-04-24／文件版本 1.0」，但內容已含 04-27 v5.0 升級（Riverpod／路由保護／screenutil 完整版）與 5 月加入的 DESIGN.md 索引。→ 更新尾戳。

### D-3｜restructure 文件未跟上 GEMINI.md 現況
「GEMINI.md → @./AGENTS.md（一行）」與實際兩行（多 `@./DESIGN.md`，06-23 有記錄於 migration 文件）不符。→ 補 06-23 附記（可與 D-1 的 agy 附記一起）。

### D-4｜專案內 ai-pair-main/ 是 v3.1.0 遺留（第 9 個副本）
`b2b-manager/ai-pair-main/` = SKILL.md **v3.1.0** + 重構建議 v3/v4/v4.1——批次 B 版本地圖之外的又一副本；全域現行 4.3.5。它被 `ai-pair-v3-progress.md` 與 wiki [[ai-pair-skill]] 引用（歷史文件引用歷史副本，自洽）。→ 建議**保留原地 + 加一行 README** 標明「v3.1.0 歷史開發版，現行版在 `~/.claude/skills/ai-pair`（4.3.5）」，避免誤當現行版使用；不搬移（搬了會斷兩處引用）。

### D-5｜CLAUDE.md typo
驗收自我測試第 7 題「`pub up   grade`」多了空格。

### D-6｜現況註記（不算錯）
- `mcp-setup.md` 規劃自 04-28 懸置：5 個 TODO 全未勾、`.mcp.json` 不存在——文件與現實一致（誠實標示未實作），但已停滯 2 個多月且未涵蓋 agy；可標「暫緩」或排期。
- `dev-workflow-handoff…` 標「適用 ai-pair v4.3.0」，現行 4.3.5——機制敘述應仍有效，順手更新版本行即可。

---

## ✅ 通過項目

| 檢查項 | 結果 |
|--------|------|
| @import 單一維護點架構 | ✅ CLAUDE.md `@AGENTS.md`＋專屬補充；GEMINI.md `@AGENTS+@DESIGN`（非對稱為**刻意設計且有完整記錄**——Claude 能按需讀給引用、Gemini 端硬 import）|
| CLAUDE / AGENTS / GEMINI 核心事實一致性 | ✅ 規則集中 AGENTS.md 單點，無重複陳述→無矛盾空間 |
| `.claude/rules/` 7 份 vs AGENTS.md | ✅ 輕度重疊（restructure 已聲明接受），抽查 sortColumnIndex／輸出語言／external_libs 三處方向一致 |
| design-md-migration 宣稱 vs DESIGN.md 實體 | ✅ 全部到位：頂層 name/version、`rounded`（非 radius）、Figma→Flutter 對應表、LongButton 易混淆提醒（:251）|
| mcp-setup 宣稱 vs 實際 | ✅ 未實作且誠實標未實作（無虛報）|
| cross-tool-ai-setup vs 實驗用文件 v3 | ✅ 逐字相同（批次 B 已證）|
| .codex/skills/claude-governance-migration | ✅ 獨立治理遷移 skill，與四份設定檔無衝突 |

> **總評**：這個專案的多工具治理是五個批次裡最成熟的——單一維護點、決策留痕（migration 文件連「為什麼看似不一致」都寫了）、驗收測試三 agent 10/10。唯一的結構性問題是工具換代（Gemini CLI→agy）快於文件更新。

## 建議修正順序

1. **D-1**：等你決策後執行（建議：AGENTS.md 加 UI 前置段 + restructure 補附記）
2. D-2 / D-3 / D-5 / D-6 版本行：五分鐘小修
3. D-4：加 README 指向現行版

> 修正狀態：⬜ 待確認（D-1 含設計決策）
