# 知識庫規則說明

個人知識庫（AI 工具應用、軟體開發、技術研究、PKM）。Obsidian 介面 + Claude Code 整理引擎。

## 關注方向

AI 工具（Claude Code / MCP / Agent）、PKM 方法論、軟體開發（Flutter / Spring Boot）、知識資產化（MAPS / Skills）

## 目錄結構

⚠️ **知識庫內容全部位於 `my-knowledge-base/` 底下，不在專案根層。**
本文件提到 `raw/`、`wiki/` 等一律指 `my-knowledge-base/raw/`、`my-knowledge-base/wiki/`。

```
notes/                          ← 專案根（工作目錄）
├── my-knowledge-base/          ← 知識庫本體
│   ├── raw/                    原始素材暫存（唯讀）。PDF >10 頁需指定 pages
│   ├── wiki/                   整理後知識條目（AI 全權維護）
│   │   ├── synthesis/          精粹條目（CAVE）
│   │   └── concepts/           跨主題概念框架
│   ├── research/               深度研究分析筆記
│   ├── ideas/                  靈感與初步想法
│   ├── outputs/                AI 產出的報告與分析
│   ├── archive/                停用內容（移入，不刪除）
│   └── rules/                  完整協議（inbox 判斷 / 研究門檻 / 決策框架）
├── my-technical-notes/         技術筆記（agent-mcp/ / agent-skills/）
└── skills/                     Skill 定義（含 references/tag-taxonomy.md）
```

## 筆記格式規範

所有筆記 `.md`，開頭必須包含：

```yaml
---
title: 筆記標題
date: YYYY-MM-DD
tags: [tag1, tag2]
source: 來源網址或出處
type: article | note | idea | reference | project | meeting
status: inbox | processed | reviewed | archived
---
```

Tag 使用階層式 `/` 分隔（`ai/claude`、`pkm/workflow`、`dev/flutter` 等）。
**完整 tag 清單以 `skills/knowledge-organizer/references/tag-taxonomy.md` 為權威來源。**

## AI 操作規則

NEVER modify or delete files in `raw/`
（兩項例外已明確界定，皆屬流程元資料而非原始內容：`.processed` 旁車標記為**新增**檔案；
`.md` 的 frontmatter **`status:` 單一欄位**可改，其餘欄位與正文一律不得動）

NEVER delete notes — 停用內容一律移入 `archive/`

建立 wiki 條目時，主動尋找主題相關的既有條目並建立 `[[wikilink]]`；精粹條目與其來源條目應維持雙向連結。

新增 wiki 條目、或內容有**實質變動**時，同步更新 `wiki/INDEX.md`（單純錯字與格式修正不需要）。

修改 `skills/` 底下任何 SKILL.md 時，同步更新三處外部追蹤點（版本號與描述）：
`~/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/pkm/<skill>/`（整份複製）、同層 `SKILL_INDEX.md`、
`my-technical-notes/agent-skills/skills-list.md`。

## Inbox 判斷

判準依「能否承載 frontmatter」分流，**全庫 `.md` 一律看 frontmatter**：

- 所有 `.md`（含 `raw/`）：已處理 = frontmatter `status: processed`
- `raw/` 的非 `.md`（PDF / 圖片 / 字幕…）：已處理 = 存在同名 `.processed` 旁車標記檔

（2026-08-13 修訂：`raw/` 的 `.md` 自旁車標記制改回 frontmatter 制，因旁車標記在 Obsidian 端不可見，
造成介面上所有 raw 檔恆顯示 inbox。舊制留下的 `.md` 旁車標記檔不具判斷效力。）

**完整判斷邏輯、掃描指令與各 Skill 實作對照見 `rules/inbox-detection-protocol.md`（唯一權威來源）。**

## 任務路由

| 情境 | 動作 |
|------|------|
| raw/ 新素材 1–9 篇 | knowledge-organizer（逐篇執行）|
| raw/ 積壓 ≥ 10 篇 | raw-pipeline（三 Agent 批次）|
| 同主題 ≥ 3 篇 wiki 待整合 | knowledge-synthesizer |
| 同領域 ≥ 3 個精粹條目 | knowledge-elevation |
| wiki 內容歸屬對齊檢查 | alignment-check |
| 月底健康檢查（含精粹過時偵測） | wiki-health-check |
| 每日狀態掃描 | daily-brief-generator |

各 Skill 的完整用途與流程見其 SKILL.md（description 已自動載入，不在此重複）。

**第三方 skill**：`obsidian-second-brain`（vault-first 研究）、`scientific-brainstorming`。
它們不套用本知識庫的 inbox 協議，且**本文件的規則優先於第三方 skill 的內建慣例**——
若第三方 skill 想寫入 `raw/`、改寫規則檔或產生自己的 `_CLAUDE.md`，一律先徵詢再執行。

## 補充規則

- 素材存入門檻與研究流程 → `rules/research-protocol.md`
- 不確定的路由判斷或破壞性操作 → `rules/decision-framework.md`

## 語言

所有筆記與互動一律使用**繁體中文（台灣用語）**。技術專有名詞保留英文原文。
