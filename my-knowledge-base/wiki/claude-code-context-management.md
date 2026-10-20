---
title: Claude Code Context 管控全攻略
date: 2026-05-14
tags: [ai/claude, ai/agent]
source: https://israynotarray.com/ai/20260409/1057131696/
type: article
status: processed
asset_value: 82
---

Context 是 AI 的短期記憶，到六成時注意力就開始下降；正確做法是手動存檔＋重開對話，而非依賴 Auto Compaction。Claude Code 標準 Context 200K tokens，延伸版（Opus/Sonnet 4.6）1M tokens，但啟動時已消耗 1 萬多 token。

## T（核心論點）
不要依賴 `/compact` 自動壓縮——它會丟失重要細節。正確節奏是：Context 到六成或功能完成時，手動存檔（`.claude/progress.md`）→ `/clear` → 重新開對話載入存檔繼續工作。

## B（適用場景）
**適用：** 長對話開發工作；多任務切換；Context 告警時的應對策略
**不適用：** 短暫的單一問答（用 `/btw` 即可）

## R（操作步驟）

### Context 的主要消耗來源
1. **CLAUDE.md 太長**：建議控制在 200 行以內，只放 Claude 無法從程式碼推斷的內容
2. **MCP Server 太多**：啟用 Tool Search（Deferred Tool Loading）可大幅降低啟動消耗
3. **Skill/Plugin 太多**：手動觸發的 Skill 加 `disable-model-invocation: true` 讓它不佔 Context
4. **對話太長不清理**：錯誤嘗試也佔 Context，反而影響後續判斷

### 手動存檔流程
```
請把目前工作進度整理成 markdown 存到 .claude/progress.md，
包含：目前任務、已修改檔案、待辦事項、測試結果
```
存完 → `/clear` → 新對話說「讀取 .claude/progress.md，繼續工作」

### 其他實用技巧
- `/context` 指令：查看目前 Token 使用狀況（色彩格狀圖）
- `/btw`：小問題用浮動視窗，不進對話紀錄
- `Shift+Tab × 2`：切換 Plan Mode，先想好再動手，省 Token
- `permissions.deny`：排除 `.env`、`build/` 等不需讓 Claude 讀的檔案
- Subagent 做探索：把大量檔案讀取交給 Subagent，只回傳精簡摘要給主對話
- 路徑限定規則：`.claude/rules/` 加 `paths` 屬性，只在匹配路徑時載入

## C（關鍵行動）
- [ ] 在 CLAUDE.md 加入「進度存檔規則」，讓 Claude 知道存檔時要保留哪些資訊
- [ ] 連續修正失敗兩次就 `/clear` 重來，把學到的寫進新 Prompt
- [ ] 用 Status Line 即時監控 Context 使用率

## ⚠️ 世代反轉注記：「長對話要重開」不再是鐵律（2026-07-30 併入）

本條目主張「Context 到六成或功能完成時手動存檔 → `/clear` → 重開」。**兩個獨立來源指出這條在 compaction 成熟後已不是鐵律**——但**這是取捨，不是單純推翻**：

| 來源 | 說法 |
|---|---|
| **OpenAI 開發者 Jason Liu**（2026-07-27）| 「對話太長要開新的」**已過時**——Codex 自動壓縮成熟後不再適用於所有情境。他自己有**運作五週的對話串、累積約 400 個子代理**；做法是把對話串**當同事養**（釘選、用專案名命名、寫進記憶庫、自動定時喚醒）|
| **Anthropic 工程師投影片** | 已**主動移除**「限制單次 session 長度」這條建議 |

🔑 **但 Jason Liu 自己就講了代價**（原文明列，不可省略）：
> **壓縮品質不見得每次都穩定、細節可能遺失、快取失效時 token 成本也會墊高——何時該重開一條，終究是使用者的取捨。**

**判斷依據：compaction 會遺失什麼**（見 [[claude-code-harness-internals]]）
- ✅ Auto Memory 存在磁碟，compact 後仍保留並重新注入
- 🔴 **帶 `paths:` 的 rules 與子目錄 CLAUDE.md 會先遺失**——若你依賴路徑限定規則，長對話反而更危險

→ **修正後的判準**：不是「六成就一定重開」，而是「**依賴什麼而定**」——純對話延續可以養長 thread；依賴 path-scoped rules 或子目錄 CLAUDE.md 的工作，仍該及早存檔重開。

## 🔴 更正：`/clear` 不是「清空」

本條目原本描述 `/clear` 為「清空」，**實際行為是**（[[claude-code-harness-internals]] 逐指令實測）：

| 指令 | JSONL 檔案行為 | 工作階段 ID |
|---|---|---|
| `/clear` | **建立新檔案，舊檔完整保留原樣** | 新發行 |
| `/compact` | 在**同一檔案內追加** `compact_boundary` 與摘要 | 不變 |
| `/rewind` | 以過去項目為親代形成**新分支** | 不變 |
| `/btw` | **不寫入任何東西**（一次性）| 不變 |
| `--continue` / `--resume` | 繼續**追加到既有檔案** | 不變 |
| `/branch` / `--fork-session` | 把歷史**複製到新檔案** | 新發行 |

> ⚠️ **對隱私判斷有實質影響**：`/clear` 後舊工作階段檔案**仍完整存在**，隨時可用 `/resume` 回復——它不是「刪除記錄」。

## 相關連結

- [[claude-code-12-rules-error-reduction]] — ⚠️ 該篇規則 6 的「單 session 30,000 tokens 就重開」屬舊世代建議，已於該篇加註世代注記並指回本篇
- [[agents-md-guide]] — AGENTS.md 的設計與多工具支援
- [[claude-code-file-management]] — Claude Code 設定檔架構與 commit 策略
- [[agent-skills-vs-mcp]] — Skill vs MCP 的 Token 效率比較
- [[synthesis-claude-code-internals]] — 本篇為其來源之一；該精粹以「上下文＝每次送出的整個 payload」為統一原理，推導出本篇全部 context 管控技巧的機制依據

## 🔀 Auto Memory 已拆為獨立條目（2026-09-03）

本條目原本在此附有完整的 Auto Memory 教學（儲存位置、200 行載入限制、`/memory` 管理、`autoMemoryDirectory` 自訂路徑、三分流判斷法）。**因本條目是 Context 操作的樞紐（49 個入連結），與 Auto Memory 這個產品功能屬兩個主題，已全數移至 [[claude-code-auto-memory]]。**

查 Auto Memory 請直接看該條目。與本條目仍相關的只有一句：

> ✅ **Auto Memory 存在磁碟，`/compact` 後仍保留並重新注入**——但 🔴 **帶 `paths:` 的 rules 與子目錄 `CLAUDE.md` 會先遺失**（見上方世代反轉注記）。

⚠️ **並提醒一個本庫曾出現過的實際錯誤**：本條目講的「200 行限制」指的是 **`MEMORY.md` 的硬截斷**（超出靜默截斷），**不是 `CLAUDE.md`**——後者不論多長都整份載入，它的 200 行是成本目標值。詳見 [[claude-md-guide]] 的來源歸屬更正。

## 相關連結（2026-08-12 補）

- [[context-engineering-stacking]] — 為什麼「六成」是個合理的門檻：Context 用量 40–60% 就該考慮壓縮或另開階段（非硬性，退化點視模型與任務而定）
- [[agent-context-management]] — 工程層原理：`/compact` 在做什麼、它為什麼可能壓錯東西（Protected Fields、Compaction Verification、Context Drift）
- [[agent-memory-lifecycle]] — Auto Memory 的理論對應：Selection / Extraction / Recall / Consolidation 四層（🔀 三分流判斷法已移至 [[claude-code-auto-memory]]）
- [[claude-code-session-handoff]] — 第三種工具：當下進度用一次性交接檔，不要寫進常駐的 CLAUDE.md 或 Auto Memory
- [[claude-code-auto-memory]] — 🔀 **本條目拆出**：Auto Memory 的完整機制（儲存位置、200 行硬截斷、管理與停用、自訂路徑、三分流判斷法）
