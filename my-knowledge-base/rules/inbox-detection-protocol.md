---
title: Inbox 判斷協議（.md frontmatter 制 + 非 md 旁車標記制）
date: 2026-06-22
updated: 2026-08-13
tags: [pkm/workflow]
type: reference
status: processed
---

# Inbox 判斷協議

定義知識庫如何判斷素材是否「已處理」，作為所有掃描 `raw/` 的 skill 的共同依據。

## 核心規則（2026-08-13 修訂）

判準依「檔案能不能承載 frontmatter」分流，**全庫 `.md` 一律看 frontmatter**：

| 素材類型 | 位置 | 已處理的標誌 |
|----------|------|-------------|
| `.md` | `raw/` 與非 `raw/` 皆同 | frontmatter `status: processed` |
| 非 `.md`（PDF / 圖片 / 字幕 / CSV…） | `raw/` | 存在同名 `.processed` 旁車標記檔 |

非 `.md` 檔無法承載 frontmatter，旁車標記對它們仍是唯一可行方式，故保留。

例：`raw/report.pdf` 處理完 → 建立 `raw/report.pdf.processed`

**標記為已處理**

```bash
# .md：Edit 修改 frontmatter 的 status 一行
#   status: "inbox"  →  status: processed

# 非 .md：建立旁車標記——檔名一律用【單引號】
touch 'raw/<原始檔名>.processed'
```

⚠️ **`touch` 的檔名必須用單引號，不可用雙引號。** 雙引號內 shell 會展開 `$`，檔名含 `$` 時會被吃掉。
實例：`你的…一年偷燒 NT$15,680？….md` 曾產生檔名為 `…NT,680？….md.processed` 的破損標記（`$15` 被當成位置參數展開成空字串），
導致原檔看似未標記、同時多出一個對不到原檔的孤兒標記。檔名含 `` ` ``、`\` 時同理。

## 為什麼 `.md` 改回 frontmatter 制

2026-07 曾將 `.md` 一併改為旁車標記制，以貫徹 `raw/` 唯讀。代價是 **Obsidian 端完全看不到處理狀態**——`.processed` 不是 `.md`，Obsidian 不索引、Dataview 也讀不到，介面上所有 raw 檔永遠顯示 `status: "inbox"`，視覺上與「完全沒處理」無法區分。

2026-08-13 盤點發現這個代價已經造成實質誤判：768 篇 raw `.md` 中，351 篇有旁車標記但 frontmatter 仍是 `inbox`（新制處理），408 篇 frontmatter 是 `processed` 但無標記（舊制處理），兩組訊號完全反相，導致任一單一判準都會誤報約半數檔案。當次已將全部 768 篇對齊為 `status: processed`。

**唯讀規則的邊界（明確例外）**：`raw/` 的 `.md` 允許修改 frontmatter 的 `status:` 單一欄位，**不得改動其餘 frontmatter 欄位與正文任何內容**。唯讀規則保護的是原始素材內容，處理狀態屬於流程元資料，不在保護範圍內。

## 掃描未處理素材

```bash
cd /Users/alexander/notes/my-knowledge-base

echo "=== raw/ .md inbox ===" && \
for f in raw/*.md; do head -15 "$f" | grep -q '^status:.*inbox' && echo "$f"; done

echo "=== raw/ 非 .md inbox（無旁車標記）===" && \
find raw/ -type f ! -name "*.md" ! -name "*.processed" \
  | while read f; do [ ! -f "${f}.processed" ] && echo "$f"; done

echo "=== ideas / research / wiki inbox ===" && \
find . -name "*.md" \
  ! -path "*/raw/*" ! -path "*/archive/*" ! -path "*/templates/*" ! -path "*/rules/*" \
  | while read f; do head -10 "$f" | grep -q '^status:.*inbox' && echo "$f"; done | sort
```

⚠️ **兩個常見誤用**

1. 掃非 `.md` 時只寫 `find raw/ -type f ! -name "*.md" ! -name "*.processed"` 而省略後面的 `| while read f; do [ ! -f "${f}.processed" ] && echo "$f"; done`。這個 find 只排除「檔名本身叫 `.processed`」的標記檔，**不會**排除「已有配對標記」的原始檔，等於把所有非 md 素材全部列為積壓。2026-08 健檢曾在 `raw-pipeline`、`wiki-health-check` 兩份 SKILL.md 發現此錯誤。
2. 掃 frontmatter 時不限行數（`grep 'status:.*inbox' "$f"`），會被正文中提到 inbox 的段落誤中。一律加 `head -15`（raw 的 Web Clipper frontmatter 較長，非 raw 用 `head -10` 即可）。

`status: inbox` 與 `status: "inbox"` 兩種格式 YAML 語義等價（Web Clipper 產生後者），掃描須同時覆蓋——上列 `grep '^status:.*inbox'` 已涵蓋。

## Obsidian 端可見性

`raw/` 待處理清單可直接用 Dataview 查詢，見 [[raw-待處理清單]]。這是本次改回 frontmatter 制的主要效益。

## 各 Skill 的實作對照（2026-08-13 同步後）

| Skill | 角色 | 標記行為 | 版本 |
|-------|------|---------|------|
| `knowledge-organizer` | 單篇整理 | `.md` → Edit frontmatter status；非 md → `touch .processed` | v1.6.0 |
| `raw-pipeline` | 批次處理（Agent A 掃描 / Agent C 標記） | A 依上列指令掃描；C 依類型分流標記 | v1.8.0 |
| `daily-brief-generator` | 每日積壓統計 | `.md` 看 frontmatter + 非 md find，統計積壓 | v1.5.0 |
| `wiki-health-check` | 月度健康檢查 | 判未處理 + **幽靈標記偵測** | v1.6.0 |

**幽靈標記偵測（wiki-health-check 專屬）**
標記為已處理（frontmatter `processed` 或有旁車標記）但 `wiki/` 或 `research/` 找不到對應條目 → 代表當初標記了卻沒真的建立條目，列為異常需人工確認。這是健康檢查比 pipeline 多出的一層交叉驗證。

注意：交叉比對本身有先天上限——多篇素材可能合併成一條 wiki 條目、素材可能未達存入門檻而刻意不建條目、來源 URL 也未必一致。**「找不到對應條目」不等於「沒處理」**，只能列為待人工確認，不可自動翻回 inbox。

## 不適用此協議的 Skill

| Skill | 原因 |
|-------|------|
| `knowledge-synthesizer` / `knowledge-elevation` | 輸入是既有 wiki 條目，不掃 raw/ |
| `alignment-check` | 檢查 wiki 段落歸屬，不碰 raw/ |
| `scientific-brainstorming` | 不觸及 raw/ |
| `obsidian-second-brain` | 第三方匯入 skill，用自己的 kanban backlog 模型；只讀不改 raw/，刻意不套用本協議 |

## 維護注意

- 新增任何會掃描 raw/ 積壓的 skill 時，**必須**遵循此協議
- **本檔為 inbox 判斷的唯一權威來源**（2026-07-27 起）：`CLAUDE.md` 只保留一行摘要與指路，完整規則與掃描指令一律維護在此，避免兩處同步失準
- 四個會建立/讀取狀態的 skill（organizer / pipeline / brief / health-check）版本需保持機制一致

## 修訂沿革

- 2026-06：非 md 改用旁車標記，取代「wiki source cross-reference」反推（多對一、附圖合併、檔名不同導致不可靠）
- 2026-07：`.md` 一併納入旁車標記制，廢止 Edit frontmatter（批次 C 健檢 C-1）
- **2026-08-13：`.md` 改回 frontmatter 制**，旁車標記僅保留給非 md。理由見上方「為什麼」段落。舊制留下的 `.md` 旁車標記檔不具判斷效力，可忽略

## 相關

- `CLAUDE.md` — AI 操作規則（規則來源）
- [[raw-待處理清單]] — Obsidian 端 Dataview 儀表板
- [[decision-framework]] — 知識庫決策框架
- [[research-protocol]] — 研究流程協議
