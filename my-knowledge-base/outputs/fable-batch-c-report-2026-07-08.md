---
title: Fable 批次 C 報告 — notes 本體（CLAUDE.md × Skills × wiki）
date: 2026-07-08
tags: [ai/claude, pkm/workflow, project/knowledge-base]
type: reference
status: processed
---

# 批次 C 報告：notes 本體一致性檢查

> 執行：2026-07-08（Fable 5）｜範圍：CLAUDE.md × 9 個 SKILL.md × rules/ 協議 × wiki 定向矛盾掃描
> 結論：**發現 1 個制度級矛盾（影響 4 個 skill + 1 份協議，實際誤報面 56 篇）+ 3 個低優先項；wiki 內容層無矛盾**
>
> ✅ **修正狀態（2026-07-08，C-1～C-4 全數執行完畢）**：
> - C-1 ✅ 制度回填五份文件：`inbox-detection-protocol.md`（.md 納入旁車標記制、雙條件掃描、版本對照表更新）＋ 四 skill 升版並補 changelog——`raw-pipeline` 1.6.0→**1.7.0**、`knowledge-organizer` 1.4.0→**1.5.0**、`wiki-health-check` 1.4.0→**1.5.0**（幽靈偵測擴及 .md）、`daily-brief-generator` 1.3.0→**1.4.0**。驗證：「Edit 改 frontmatter」舊制指令全庫零殘留
> - C-2 ✅ CLAUDE.md Skills 表補列 obsidian-second-brain / scientific-brainstorming（標註第三方）
> - C-3 ✅ CLAUDE.md tag 段落加註「完整清單以 tag-taxonomy.md 為權威」
> - C-4 ✅ `b2b-manager-bugs.md`（0 bytes 孤兒檔）已刪除
> - 📌 **附帶發現**：新制雙條件掃描顯示 raw/ 有 **28 篇真實新積壓**（2026-07-03 清空後新剪藏進來的），已超過「>15 篇需立即處理」門檻——建議near期跑 raw-pipeline

---

## ❌ C-1｜raw/ .md 標記制度矛盾（制度級，頭號發現）

**CLAUDE.md（現行規則）**與**協議文件 + 四個 skill** 對「raw/ 的 .md 檔案如何標記已處理」採兩套相反制度：

| 文件 | 制度 | 狀態 |
|------|------|------|
| **CLAUDE.md**「raw/ 目錄的 .md 檔案（特殊規則）」 | raw/ 不可修改 → `.md` 一律**旁車標記檔**（雙條件掃描：`status:.*inbox` 且無 `.processed`）| ✅ 現行權威 |
| `rules/inbox-detection-protocol.md`（2026-06-22）| `.md` → **用 Edit 改 frontmatter** `status: processed` | ❌ 過時 |
| `raw-pipeline` v1.6.0（Agent C 步驟 7）| 同上，明文「用 Edit 工具更新其 frontmatter」| ❌ 過時 |
| `knowledge-organizer` v1.4.0（步驟 114-117）| 同上 | ❌ 過時 |
| `wiki-health-check` v1.4.0（第 6 項 A）| `.md` 判斷**只看 frontmatter**，無標記檢查 | ❌ 過時 |
| `daily-brief-generator` v1.3.0（步驟 1）| 同上 | ❌ 過時 |

**實際影響（已量測）**：raw/ 目前有 **56 篇** .md 是「frontmatter 仍為 `status: "inbox"` + 已有 `.processed` 標記」（Batch 20–24 依 CLAUDE.md 現行制處理的成果）。任何 skill 按自己 SKILL.md 的舊制掃描，**這 56 篇會全數誤報為未處理積壓**——daily-brief 會報假警訊、health-check 會報假積壓、pipeline 重跑會重複處理。

**歷史判讀**：CLAUDE.md 的旁車制是後來確立的（實務證據：56 個 `.md.processed` 標記存在、frontmatter 從未被改），但確立時只改了 CLAUDE.md，**協議文件與四個 skill 沒有回填**。協議文件末尾自己也寫「三個 skill 版本需保持機制一致」——恰好是它沒做到的事。

**修法（確認後執行）**：
1. `rules/inbox-detection-protocol.md`：`.md` 判斷規則改為旁車標記制（雙條件），註明 2026-07 修訂
2. `raw-pipeline` → v1.7.0：Agent A 的 .md 掃描加「無 `.processed` 標記」條件；Agent C 步驟 7 改為 `touch` 標記
3. `knowledge-organizer` → v1.5.0：同上
4. `wiki-health-check` → v1.5.0：第 6 項 A 加標記條件；幽靈標記偵測擴及 .md
5. `daily-brief-generator` → v1.4.0：步驟 1 同步
6. 各 skill 補 changelog

---

## ⚠️ 低優先（順手修或註記）

### C-2｜CLAUDE.md Skills 表少列 2 個
CLAUDE.md 的 Skills 表列 7 個，實際 `skills/` 有 9 個——缺 `obsidian-second-brain`、`scientific-brainstorming`（兩個第三方匯入 skill）。協議文件有提到它們（不適用清單），但 CLAUDE.md 主表沒有。→ 建議補兩列並標註「第三方」。

### C-3｜CLAUDE.md tag 樣例清單落後 taxonomy
CLAUDE.md 列的 tag 樣例（`ai/claude ai/agent ai/mcp ai/prompt-engineering dev/flutter dev/spring-boot pkm/workflow pkm/obsidian project/…`）缺 `ai/workflow`、`type/*`、`method/*`、`dev/architecture`、`pkm/automation` 等——這些在 `tag-taxonomy.md` 有定義且 wiki 條目大量使用。→ 建議 CLAUDE.md 在 tag 段落加一句「完整清單以 `skills/knowledge-organizer/references/tag-taxonomy.md` 為權威」。

### C-4｜孤兒空檔
`/Users/alexander/notes/b2b-manager-bugs.md`：0 bytes、2026-05-13 建立、無任何內容與引用。→ 建議直接刪除（無內容可歸檔）。

---

## ✅ 通過項目

| 檢查項 | 結果 |
|--------|------|
| CLAUDE.md 路由表 7 個 skill 全部存在 | ✅ |
| rules/ 三份協議檔全部存在 | ✅ |
| knowledge-elevation 觸發條件（同領域 ≥3 精粹）與 CLAUDE.md 路由表一致 | ✅ |
| PKM skill 單一來源（`notes/skills/` 9 個；`~/.claude/skills/` 7 個開發類，無重疊、無複本）| ✅（批次 B 修正後複查）|
| 近期新增 wikilink 存在性（multi-model-orchestration-expansion-plan / claude-fable-5-usage-guide / ai-pair-mesh-topology-design）| ✅ 三個都有目標檔（後兩者在 research/ 與 wiki/）|
| wiki 高頻數字斷言跨條目一致性（Karpathy 41%→3% ×5 條目、Anthropic 80% ×3 條目、Fable 窗口日期）| ✅ 同源一致，無矛盾 |
| 模型版本敘述差異（Opus 4.6/4.7/4.8、GPT-5.4/5.5）| ✅ 時間戳性質，各條目 date 明確（沿 7/3 健檢結論）|

> **wiki 掃描覆蓋範圍說明**：本批對 234 篇 wiki 採「定向矛盾掃描」（高頻數字斷言 + 模型版本 + 同工具叢集比對），Agent 架構叢集已於 7/3 synthesis 時逐篇比對過。未做 234 篇兩兩全比對；若需更深層語意矛盾掃描，建議以 INDEX 區段分批另跑。

## 建議修正順序

1. **C-1 制度回填**（1 協議 + 4 skill + changelog）——正在造成 56 篇誤報面
2. C-2 / C-3 CLAUDE.md 補列（兩行改動）
3. C-4 刪空檔

> 修正狀態：⬜ 待確認
