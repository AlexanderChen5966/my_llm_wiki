---
title: Fable 5 全面健檢批次計畫與可貼 Prompt
date: 2026-07-08
tags: [ai/claude, pkm/workflow, project/knowledge-base]
type: reference
status: processed
---

# Fable 5 全面健檢批次計畫

> ✅ **本計畫已於 2026-07-08 全部結案**（五批掃描 + 修正皆完成，Fable 免費窗口內達成）。下方 Prompt 保留作為**可複用健檢範本**，建議季度性重跑。

## 📋 進度追蹤

| 批次 | 狀態 | 報告 | 摘要 |
|------|------|------|------|
| A：notebooklm kit 五版本 | ✅ 完成＋已修正（2026-07-08）| [[fable-batch-a-report-2026-07-08]] | 2 個版號漂移（A-1/A-2）+ 8 處 docs 改名斷鏈（A-3）→ **11 處全數修正並驗證歸零**；凍結版無污染、key 規則一致；A-4～A-6 低優先留註記 |
| B：ai-pair-main 版本溯源 | ✅ 完成＋已修正（2026-07-08）| [[fable-batch-b-report-2026-07-08]] | 演進鏈重建（1.3→…→4.3.5 使用中）；同名衝突解除、reference 倒置同步回正本（唯一真相源=~/.claude/skills/ai-pair）、v2.3.1 歸位、cross-tool v1/v2 進廢棄區、B-3/B-5 建快照 README |
| C：notes 本體 | ✅ 完成＋已修正（2026-07-08）| [[fable-batch-c-report-2026-07-08]] | 制度級矛盾（raw/ .md 標記制）→ 五份文件回填旁車制：協議修訂 + 4 skill 升版（pipeline 1.7.0 / organizer 1.5.0 / health-check 1.5.0 / daily-brief 1.4.0），舊制指令零殘留；CLAUDE.md 補 2 skill + tag 權威註記；空孤兒檔已刪。📌 附帶發現 raw/ 新積壓 28 篇（>15 門檻）|
| D：GitLab b2b-manager | ✅ 完成＋已修正（2026-07-08）| [[fable-batch-d-report-2026-07-08]] | 治理最成熟；agy 換代斷層已修（AGENTS.md v1.2 加 UI 前置索引段、GEMINI.md 留歷史、兩份文件補附記）+ ai-pair-main 版本狀態說明 + 尾戳/typo/mcp 暫緩註記全數處理 |
| E：四個個人專案 | ✅ 完成＋已修正（2026-07-08）| [[fable-batch-e-report-2026-07-08]] | 四專案整體健康：train-booking 最乾淨（tools 7/7、文件鏈環環註明）、DownloadVideo skill 兩處同步、preschool 兩文件互補；kyoto 三份 RAG 方案已加「提案・尚未實施」狀態標頭 |

> 🏁 **五批全數完成＋修正（2026-07-08）**。合計修正：A 11 處（版號+斷鏈）、B 6 項（同名衝突/reference 同步/歸檔）、C 制度回填 5 文件+CLAUDE.md、D agy 換代 6 項、E 狀態標頭 3 份。後續待辦：raw/ 28 篇新積壓（跑 raw-pipeline）。

> 目的：趁 Claude Fable 5 限時免費窗口（2026-07-13 截止），利用其長上下文一致性比對能力，掃描散落在五個路徑的 AI 設定（Skill / MCP / CLAUDE.md / AGENTS.md）與個人知識庫，找出矛盾、過期、版本漂移與可歸檔項目。
>
> 方法依據：[[claude-fable-5-power-tasks]]（任務 2：知識庫掃描）。
> 執行方式：**只找問題，不自動修改**。Fable 抓完問題清單後，回到 Claude Code（Sonnet）確認並實際修正——維持「找問題」與「動手改」分離，避免長上下文模型在未經確認下直接改壞正式檔案。
> 建議節奏：五批**分開跑**，不要一次全塞——混雜五個不相關領域會稀釋比對品質。每批跑完先讀完報告、決定要不要修，再跑下一批。

---

## 批次 A：notebooklm kit 五版本一致性

> ✅ 已執行＋修正（2026-07-08）→ 報告：[[fable-batch-a-report-2026-07-08]]

**範圍**：`/Users/alexander/Desktop/紀錄文件資料夾/notebooklm-v1.3-kit`、`v2.0-kit`、`v2.0.1-kit`、`v2.1-kit`、`v2.1.1-kit`

**為什麼優先**：這是最近手動同步最頻繁的地方（citation_key 規則、Tier 分層、台灣中文文獻支援都是這幾天連續修改），最容易「改 A 忘了改 B」。

### Prompt

```
請讀取以下五個資料夾的 AGENTS.md 和 skills/paper-discovery/SKILL.md：

/Users/alexander/Desktop/紀錄文件資料夾/notebooklm-v1.3-kit
/Users/alexander/Desktop/紀錄文件資料夾/notebooklm-v2.0-kit
/Users/alexander/Desktop/紀錄文件資料夾/notebooklm-v2.0.1-kit
/Users/alexander/Desktop/紀錄文件資料夾/notebooklm-v2.1-kit
/Users/alexander/Desktop/紀錄文件資料夾/notebooklm-v2.1.1-kit

這五個是同一套 kit 的版本分支：v2.0 系列走 OpenAI API key 方案，v2.1 系列走免金鑰方案；
.0.1 / .1.1 是各自的「搜尋品質補強」版（Query Rewrite、期刊等級標示、滾雪球、citation_key
唯一權威、台灣中文文獻支援），v2.0-kit 和 v2.1-kit 應維持凍結不動。

請找出：
1. 版本號標示是否與檔案內容一致（AGENTS.md 開頭的版號 vs Skills 清單裡列的 skill 版號）
2. 五份 paper-discovery SKILL.md 之間，citation_key 產生規則的敘述是否一致
   （尤其「有 Zotero BBT 時以 BBT 為準」這條規則，是否每份都有且措辭一致）
3. v2.0-kit / v2.1-kit（凍結版）是否不小心混入了本該只在 .0.1/.1.1 才有的內容
   （例如台灣中文文獻支援、滾雪球功能，這些是 v2.2.0 之後才加的）
4. docs/ 底下的整合模板（codex-desktop-integration）和 wiki/INDEX.md 的版本標記
   是否與 AGENTS.md 一致
5. 五份文件之間有沒有互相矛盾的操作指引

只列出問題，不要幫我改檔案。輸出格式：
- 檔案A vs 檔案B：矛盾/不一致內容 → 建議
```

---

## 批次 B：ai-pair-main 版本溯源與歸檔判斷

> ✅ 已執行＋修正（2026-07-08）→ 報告：[[fable-batch-b-report-2026-07-08]]。⚠️ 注意：B 修正後路徑已變動（v4-test 移入 Skill存檔、v2.3.1 歸位、cross-tool v1/v2 進廢棄區），日後複用本 prompt 前先核對路徑。

**範圍**：`/Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/`

**背景**：`ai-pair-main` 系列共發現 8 個版本散落各處（根目錄「claude + copilot」版 + `Skill存檔/` 內 v1.3 / v3.1 / v4.1 / v4.2 / v4.2.5 / v4.3 / v4.3.6），另有 `Skill存檔/廢棄區/` 已標記 2 個廢棄 skill、`Skill存檔/pkm/` 存放 9 個 PKM skill 的另一份拷貝、`cross-tool-ai-setup_v1/v2/v3.md` 三份版本稿。

### Prompt

```
請讀取 /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/ 底下的內容，重點看：

1. ai-pair-main 系列的所有版本：
   - /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/ai-pair-main claude + copilot/
   - /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/ai-pair-main-v1.3/
   - /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/ai-pair-main-v3.1/
   - /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/ai-pair-main-v4.1/
   - /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/ai-pair-main-v4.2/
   - /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/ai-pair-main-v4.2.5/
   - /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/ai-pair-main-v4.3/
   - /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/ai-pair-main-v4.3.6/

2. /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/pkm/ 底下 9 個 skill 資料夾

3. 三份 cross-tool-ai-setup 版本稿：
   cross-tool-ai-setup_v1.md / _v2.md / _v3.md（都在實驗用文件根目錄）

請幫我釐清：
1. ai-pair-main 8 個版本的演進順序與版本間的功能差異摘要（各版本的 SKILL.md
   和「重構建議」系列文件為依據）；哪個版本看起來是「目前使用中」、哪些明顯是
   舊版應該搬進「廢棄區」（本知識庫規則：停用內容移入 archive，不要留在原地）
2. Skill存檔/pkm/ 的 9 個 skill，內容是否與 /Users/alexander/notes/skills/
   的正本版本一致或落後（只需比對是否有明顯落後跡象，例如缺少較新的
   changelog 條目或功能段落）
3. cross-tool-ai-setup_v1→v2→v3 每版新增了什麼，v3 的內容是否已完整涵蓋
   v1、v2 的所有要點（正式版在 GitLab b2b-manager/docs/ai-collaboration/
   cross-tool-ai-setup.md，可一併對照，行數應與 v3 一致）

只列出問題和建議，不要動任何檔案。
```

---

## 批次 C：/Users/alexander/notes 本體

> ✅ 已執行＋修正（2026-07-08）→ 報告：[[fable-batch-c-report-2026-07-08]]。⚠️ 注意：C 修正後 4 個 skill 已升版（pipeline 1.7.0 等）、b2b-manager-bugs.md 已刪除，複用本 prompt 時該檢查項可略。

**範圍**：`/Users/alexander/notes` 根目錄 CLAUDE.md、`skills/`（9 個）、`my-knowledge-base/wiki/`（234+ 篇）

### Prompt

```
請讀取 /Users/alexander/notes/CLAUDE.md，以及 /Users/alexander/notes/skills/
底下全部 9 個 SKILL.md（alignment-check、daily-brief-generator、
knowledge-elevation、knowledge-organizer、knowledge-synthesizer、
obsidian-second-brain、raw-pipeline、scientific-brainstorming、
wiki-health-check）。

請檢查：
1. CLAUDE.md 裡提到的規則（inbox 判斷、tag 規範、任務路由表）是否與各
   SKILL.md 內實際描述的邏輯一致，有沒有互相矛盾或其中一邊已過時
2. 9 個 SKILL.md 之間彼此提到對方時（例如 A 說「交給 B 處理」），
   B 裡面是否真的有對應的處理邏輯，有沒有「A 以為 B 會做，但 B 沒寫」的落差
3. 各 SKILL.md 的 Changelog 版本號是否和 CLAUDE.md 或彼此的「相關 Skill」
   引用段落提到的版本一致

接著，請掃描 /Users/alexander/notes/my-knowledge-base/wiki/ 底下的 wiki 條目
（如果數量太多無法一次讀完，請告訴我實際能處理的篇數上限，我再分批給你）：
找出同一個工具/概念在不同條目中，版本號、數據、結論互相矛盾的地方
（例如兩篇都提到某工具的能力限制，但描述的數字不同）。

另外請確認 /Users/alexander/notes/b2b-manager-bugs.md 這個檔案的內容
（我懷疑它是空檔案或孤兒檔，請告訴我它的實際大小與內容）。

只列出問題，不要動任何檔案。
```

> 註：wiki 條目數量較大，若 Fable 一次讀取有上限，建議依 INDEX.md 的分類區段（AI 工具應用／PKM／知識精粹／研究筆記等）拆成子批次。

---

## 批次 D：GitLab b2b-manager 多工具設定檔

> ✅ 已執行＋修正（2026-07-08）→ 報告：[[fable-batch-d-report-2026-07-08]]。AGENTS.md 已升 v1.2（UI 前置索引段）。

**範圍**：`/Users/alexander/GitLab/b2b-manager/`（根目錄四份設定檔 + `docs/ai-collaboration/`）

### Prompt

```
請讀取 /Users/alexander/GitLab/b2b-manager/ 根目錄的四份檔案：
CLAUDE.md、AGENTS.md、GEMINI.md、DESIGN.md

以及 /Users/alexander/GitLab/b2b-manager/docs/ai-collaboration/ 底下：
ai-pair-v3-progress.md、mcp-setup.md、dev-workflow-handoff-token-strategy.md、
cross-tool-ai-setup.md、README.md、agents-gemini-restructure.md、
session-log-template.md、design-md-migration.md

這四份根目錄設定檔分別給不同 AI 工具讀取（Claude Code / 通用 Agent /
Gemini / 設計規範），理論上關於專案結構、指令規範的核心事實應該一致，
只有工具特定的語法/格式不同。

請找出：
1. CLAUDE.md / AGENTS.md / GEMINI.md 三份對同一件事（例如套件管理器、
   目錄結構、commit 規範、測試指令）的描述是否有不一致
2. design-md-migration.md 提到的遷移內容，是否已經反映在 DESIGN.md 裡，
   還是遷移文件寫了但 DESIGN.md 沒更新
3. agents-gemini-restructure.md 的重構內容，是否已經體現在目前的
   AGENTS.md / GEMINI.md，還是重構文件寫了但沒真的做
4. mcp-setup.md 描述的 MCP 設定，和 .claude/ .codex/ 兩個目錄下的實際
   設定檔（如果你能讀取，請告訴我是否可讀，我可以提供路徑）是否一致

只列出問題和建議，不要動任何檔案。
```

---

## 批次 E：四個個人專案各自健檢

> ✅ 已執行＋修正（2026-07-08）→ 報告：[[fable-batch-e-report-2026-07-08]]。kyoto 三份 RAG 方案已加狀態標頭。

**範圍**：`kyoto-assistant`、`train-booking`、`preschool-finder-crawler`、`DownloadVideoPythonProject`

### Prompt

```
請依序讀取以下四個專案的 AI 相關設定與文件：

1. /Users/alexander/Desktop/Speckit Project/kyoto-assistant
   - .claude/settings.local.json
   - mcp_server/server.py 和 mcp_server/config.json
   - spec/ 底下 4 份 v4.1 規格文件
   - docs/ 底下 3 份「執行任務」方案文件（ChromaDB v1、v2、PageIndex）

2. /Users/alexander/PycharmProjects/train-booking
   - CLAUDE.md
   - docs/台鐵高鐵AI訂票系統需求文件.md
   - docs/台鐵高鐵AI訂票系統_架構修訂版.md
   - docs/PROGRESS.md

3. /Users/alexander/PycharmProjects/preschool-finder-crawler
   - .claude/settings.local.json
   - docs/幼兒園查詢App_MVP需求文件.md
   - docs/幼兒園查詢App_Supabase設定紀錄.md
   - docs/托嬰中心地圖整合與獨立資料庫架構設計規劃.md
   - docs/托嬰中心欄位實測與整合規劃.md
   - CHANGELOG.md

4. /Users/alexander/PycharmProjects/DownloadVideoPythonProject
   - .claude/settings.json 和 .claude/settings.local.json
   - .claude/skills/sign-off-generator-skill/SKILL.md
   - .claude/skills/media-processor/SKILL.md

請針對每個專案分別回答：
1. kyoto-assistant：mcp_server/config.json 裡設定的內容，跟 server.py
   實際實作的功能是否一致；docs/ 的 3 份執行任務方案，是否有些已經
   被取代但沒有標記或移除
2. train-booking：需求文件和「架構修訂版」之間的差異，是否修訂版的
   內容已經同步回 CLAUDE.md；PROGRESS.md 記錄的進度是否與需求文件的
   範圍一致
3. preschool-finder-crawler：「地圖整合與獨立資料庫架構設計規劃」和
   「欄位實測與整合規劃」這兩份文件對同一個資料庫設計是否有矛盾之處
4. DownloadVideoPythonProject：兩個 skill 的 SKILL.md 內容是否完整、
   description 是否清楚描述觸發時機（這兩個 skill 在
   /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/general/
   也各有一份，如果你也能讀到，順便告訴我兩處版本是否一致）

只列出問題，不要動任何檔案。
```

---

## 執行後的下一步（原計畫流程，已全數走完）

Fable 產出每批報告後，請回到這裡（Claude Code / Sonnet）並貼上報告內容或路徑，我會：
1. 逐項確認問題是否屬實
2. 判斷優先順序（矛盾 > 過期 > 可歸檔）
3. 實際動手修正（改檔案、搬移歸檔、更新版本號等）

這個「Fable 找問題、Sonnet 動手改」的分工，本身也可以視為一種雙模型分層策略——
可對照 [[llm-cost-optimization-strategies]] 的成本分層邏輯。

## 相關連結

- [[claude-fable-5-power-tasks]] — 本次批次計畫的方法依據
- [[wiki-health-check]] — 本知識庫既有的規則式健檢機制（Fable 補的是內容層級矛盾）
- [[alignment-check-2026-07-03]]
