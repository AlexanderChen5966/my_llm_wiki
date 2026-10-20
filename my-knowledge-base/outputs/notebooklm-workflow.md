---
title: 教授個人學術 RAG 系統：Obsidian + Codex Desktop + NotebookLM 完整建立指南
date: 2026-05-15
tags: [pkm/workflow, ai/agent, pkm/obsidian]
type: reference
status: processed
source: Obsidian知識庫規劃_知識整理與資產化.md（v2.0）+ 路線 A 最終版
---

# 教授個人學術 RAG 系統
## Obsidian + Codex Desktop + NotebookLM 完整建立指南

> **適用對象**：大學教授、研究者，長期累積多元格式素材（PDF 論文、截圖、逐字稿、網頁），需要能用自然語言查詢並取得精確引用的個人知識庫。
>
> **操作門檻**：日常使用全程 GUI，不需要終端機或程式設計背景。初次設定由助理完成。

---

## 整體架構

```
┌─────────────────────────────────────────────────────────┐
│              Obsidian（素材管理視覺介面）                  │
│  拖入任何格式 → raw/ 資料夾，視覺化管理所有素材             │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│              Codex Desktop（AI 整理引擎）                 │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │  Skills（可重複執行的標準化指令）                  │    │
│  │                                                  │    │
│  │  knowledge-organizer  → 單篇素材 TBRC 結構化      │    │
│  │  raw-pipeline         → 10 篇以上批次處理          │    │
│  │  knowledge-synthesizer → 跨素材主題合成            │    │
│  │  notebooklm-skill     → 自動上傳到 NotebookLM     │    │
│  └─────────────────────────────────────────────────┘    │
│                                                         │
│  AGENTS.md（知識庫規則，告訴 Codex 怎麼運作）             │
└──────────────────────┬──────────────────────────────────┘
                       │ 全自動上傳（PDF 原檔 + wiki 摘要）
                       ▼
┌─────────────────────────────────────────────────────────┐
│              NotebookLM（教授日常查詢介面）               │
│                                                         │
│  「這篇論文的核心論點是什麼？」                            │
│  「哪些論文討論過 X 理論？」                               │
│  「幫我比較 A 和 B 兩篇的研究方法」                        │
│  「整理這個主題的研究現況」                                │
│       ↓                                                 │
│  AI 回答 + 原文出處（論文名稱 + 頁碼）                     │
└─────────────────────────────────────────────────────────┘
```

---

## 工具清單與角色分工

| 工具                       | 角色                             | 操作者            |
| ------------------------ | ------------------------------ | -------------- |
| **Obsidian**             | 素材視覺化管理介面（拖放、瀏覽）               | 教授 / 助理        |
| **Obsidian Web Clipper** | 瀏覽器一鍵存網頁到 raw/                 | 教授 / 助理        |
| **Codex Desktop**        | AI 整理引擎 GUI，執行 Skills          | 助理（初期），教授（熟悉後） |
| **notebooklm-skill**     | 自動上傳 PDF + wiki 摘要到 NotebookLM | Codex 自動呼叫     |
| **NotebookLM**           | 日常 RAG 查詢介面                    | **教授**（主要使用者）  |

---

## Skills 說明

Codex Desktop 的 Skills 和 Claude Code Skills 使用相同的 `SKILL.md` 格式，支援 GUI 觸發。

| Skill | 觸發方式 | 用途 |
|-------|---------|------|
| `knowledge-organizer` | `$knowledge-organizer` 或描述任務 | 單篇素材 → TBRC 結構化 wiki 條目 |
| `raw-pipeline` | `$raw-pipeline` | 10 篇以上積壓批次處理 |
| `knowledge-synthesizer` | `$knowledge-synthesizer` | 多篇同主題 wiki → CAVE 精粹條目 |
| `notebooklm-skill` | `$notebooklm-skill` | 上傳新 wiki 條目 + PDF 到 NotebookLM |

Skills 資料夾結構：

```
skills/
├── knowledge-organizer/
│   └── SKILL.md
├── raw-pipeline/
│   └── SKILL.md
├── knowledge-synthesizer/
│   └── SKILL.md
└── notebooklm-skill/          ← 來自 claude-world/notebooklm-skill
    ├── SKILL.md
    ├── scripts/
    └── agents/openai.yaml     ← Codex Desktop 專屬設定
```

---

## notebooklm-skill 的能力（自動化上傳）

基於 [claude-world/notebooklm-skill](https://github.com/claude-world/notebooklm-skill)，底層使用 `notebooklm-py`。

| 功能 | 支援 | 說明 |
|------|------|------|
| PDF 原檔上傳 | ✅ | 保留頁碼引用，可精確定位原文 |
| URL 加入 | ✅ | 網頁文章直接加入 |
| 文字貼入 | ✅ | wiki 摘要 .md 全文 |
| YouTube | ✅ | 自動讀取字幕 |
| 批次操作 | ✅ | 一次上傳多個來源 |
| Notebook 建立 / 管理 | ✅ | 自動建立對應主題 Notebook |
| 查詢（ask） | ✅ | 可在 Codex 裡直接問 NotebookLM |
| Artifact 產出 | ✅ | 播客、投影片、Study Guide、心智圖等 |
| 認證方式 | 一次性 Google 登入 | Session 存檔，持續數週不需重新登入 |

---

## 資料夾結構

```
my-knowledge-base/
├── raw/                        ← 所有格式素材丟這裡（唯讀）
│   ├── papers/                 ← PDF 論文
│   ├── screenshots/            ← 投影片截圖、白板照片
│   ├── transcripts/            ← 逐字稿（.txt）、字幕（.srt .vtt）
│   ├── notes/                  ← Markdown 手稿、會議記錄
│   └── web/                    ← 網頁存檔（.html）
├── wiki/                       ← Codex Desktop 整理後的結構化條目
│   ├── paper-summaries/        ← 每篇素材一個 .md 摘要
│   ├── topic-synthesis/        ← 跨素材主題精粹
│   └── INDEX.md                ← 知識地圖（自動維護）
├── outputs/                    ← 報告、每日簡報、健康檢查
├── skills/                     ← Skills 資料夾
└── archive/                    ← 已歸檔內容
```

---

## 支援的輸入格式（全部 12 種）

| 格式 | 副檔名 | Codex 讀取方式 | NotebookLM 上傳方式 |
|------|--------|--------------|-------------------|
| PDF 論文 | `.pdf` | 全文（≤10頁）或首尾節錄 | **自動**：PDF 原檔（notebooklm-skill）|
| Markdown 筆記 | `.md` | 直接讀取全文 | **自動**：wiki 摘要文字貼入 |
| 截圖／投影片 | `.png` `.jpg` `.webp` | 視覺解析，提取文字圖表 | **自動**：wiki 摘要文字貼入 |
| 逐字稿／字幕 | `.txt` `.srt` `.vtt` | 去除時間戳，提取核心討論 | **自動**：wiki 摘要文字貼入 |
| 網頁存檔 | `.html` | 提取正文，忽略導覽廣告 | **自動**：URL 直接加入 |
| Jupyter Notebook | `.ipynb` | 提取說明文字 + 程式邏輯 | **自動**：wiki 摘要文字貼入 |
| 資料表格 | `.csv` | 摘要欄位結構與關鍵數據 | **自動**：wiki 摘要文字貼入 |

> 所有格式均可自動上傳，不需手動操作。

---

## 論文 / 素材摘要的標準格式（TBRC 學術版）

每篇素材在 `wiki/paper-summaries/` 產出一份 `.md`：

```markdown
---
title: 標題
date: 整理日期
tags: [研究主題, 研究方法]
source: 原始檔案路徑 / DOI / URL
source_format: pdf | md | screenshot | transcript | html | csv
type: paper-summary
status: processed
authors: 作者（論文適用）
year: 發表年份（論文適用）
journal: 期刊名稱（論文適用）
---

## T（核心論點）
[主要主張，2-3 句話]

## B（研究設計 / 內容範圍）
**研究問題 / 主題**：
**研究方法 / 格式特性**：
**樣本 / 資料**：
**適用場景**：
**限制**：

## R（主要發現 / 關鍵內容）
1.
2.
3.

## C（對研究的意義）
- [ ] 具體啟發或後續行動

## 引用格式（論文適用）
[APA / MLA / Chicago]
```

---

## 標準工作流程

### 新素材入庫（日常）

```
Step 1  把素材拖入 raw/ 對應子資料夾          （1 分鐘）
        PDF → papers/
        截圖 → screenshots/
        逐字稿 → transcripts/
        網頁存檔 → web/

Step 2  在 Codex Desktop 輸入或點擊 Skill：
        $knowledge-organizer                  （5-15 分鐘自動完成）

        Codex 自動：
        ├─ 依格式讀取（PDF全文 / 截圖視覺解析 / SRT去時間戳）
        ├─ 產出 wiki/paper-summaries/標題.md
        └─ 更新 wiki/INDEX.md

Step 3  執行 $notebooklm-skill                （2-5 分鐘自動完成）

        notebooklm-skill 自動：
        ├─ PDF 原檔 → 上傳到 NotebookLM（保留頁碼引用）
        ├─ 網頁素材 → 加入 URL
        └─ 其他格式 → wiki 摘要文字貼入

Step 4  在 NotebookLM 查詢（立即可用）
```

### 批次處理積壓素材（10 篇以上）

```
在 Codex Desktop 執行：$raw-pipeline
→ 批次整理所有未處理素材（30-60 分鐘，不需在旁等候）

完成後執行：$notebooklm-skill
→ 批次上傳所有新條目到 NotebookLM
```

### 跨素材主題合成

```
當同主題累積 3 篇以上 wiki 條目後：

在 Codex Desktop 執行：
$knowledge-synthesizer [主題名稱]

產出 wiki/topic-synthesis/主題.md（CAVE 框架）：
C（共識）  → 學術共識是什麼？
A（爭論）  → 哪些論文有不同立場？
V（缺口）  → 這個領域還沒有人研究什麼？
E（精粹）  → 對你研究最重要的一個洞察

→ 自動上傳到 NotebookLM，讓跨論文查詢更精準
```

### 每月知識庫維護

```
在 Codex Desktop 輸入：
「執行知識庫健康檢查」

Codex 回報：
├─ 還有哪些素材尚未整理
├─ 哪些摘要可能已過時（建立超過 6 個月）
└─ 哪些主題適合執行 knowledge-synthesizer
```

---

## NotebookLM 組織方式

按**研究主題**建立 Notebook，而非按時間或作者：

```
[課程] 研究所課程名稱
[計畫] 國科會計畫名稱
[主題] 學習動機理論
[主題] 教育科技評估方法
[主題] 質性研究方法
[書]   正在閱讀的學術書籍
[會議] 2026 AERA 論文集
```

> 每個 Notebook 上限 50 份來源。主題過大時拆成子主題 Notebook。

---

## NotebookLM 教授查詢指南

### 依目的選擇問法

**找論文支持特定論點：**
```
哪些資料支持「自主學習能提升學業成就」這個主張？
列出來源名稱和對應段落。
```

**比較不同論文立場：**
```
Smith (2023) 和 Johnson (2024) 對於 X 理論的看法有什麼不同？
```

**整理研究現況：**
```
這個 Notebook 的所有資料，對「X 議題」有什麼整體共識和主要爭論？
```

**準備文獻回顧段落：**
```
根據這些資料，草擬一段「X 理論發展歷程」的文獻回顧，每個論點附引用來源。
```

**跨格式查詢（論文 + 演講 + 截圖）：**
```
關於 X 主題，這個 Notebook 裡不同格式的資料（論文、演講逐字稿、圖表摘要）各自說了什麼？
```

### NotebookLM 的學術功能

| 功能 | 說明 |
|------|------|
| **引用標記** | 答案旁 [1][2] 點擊跳到原始來源（PDF 有頁碼）|
| **研究指南** | 自動生成 FAQ + 關鍵概念，適合準備授課 |
| **Audio Overview** | 生成 10 分鐘播客摘要，適合通勤聽論文重點 |
| **儲存為筆記** | 把 AI 回答存在 Notebook，下次繼續延伸 |

---

## 初次設定步驟（一次性，由助理操作）

### 安裝工具

| 工具 | 安裝方式 |
|------|---------|
| Obsidian | obsidian.md 官網下載 |
| Obsidian Web Clipper | Chrome / Firefox 擴充套件 |
| Codex Desktop | `npm install -g @openai/codex` → `codex app` |
| notebooklm-skill | `pip install notebooklm-skill` 或 `uvx notebooklm-skill` |
| notebooklm-skill 登入 | `uvx notebooklm login`（一次性，開啟 Google 登入）|

### 建立知識庫結構

```bash
# 建立資料夾結構
mkdir -p my-knowledge-base/{raw/{papers,screenshots,transcripts,notes,web},wiki/{paper-summaries,topic-synthesis},outputs,skills,archive}

# 放入設定檔
# AGENTS.md → my-knowledge-base/
# skills/ → my-knowledge-base/skills/
```

### AGENTS.md 設定重點

```markdown
# 知識庫規則（AGENTS.md）

## 資料夾結構
raw/ 唯讀，所有素材放這裡
wiki/ 由 AI 維護，整理後的條目
outputs/ 存放報告和每日簡報

## 整理規則
處理 raw/ 素材時，依副檔名選擇讀取方式：
- .pdf → 讀全文（≤10頁）或首尾節錄（>10頁）
- .png .jpg .webp → 視覺解析
- .srt .vtt → 去除時間戳
- .html → 提取正文

產出 wiki/paper-summaries/[標題].md，使用 TBRC 學術版格式。
完成後更新 wiki/INDEX.md。

## 語言
所有筆記與互動使用繁體中文，技術術語保留英文原文。
```

---

## 各格式注意事項

| 格式 | 注意事項 |
|------|---------|
| **掃描版 PDF** | 影像式 PDF 無法讀取，需先用 OCR 工具（Adobe Acrobat / 線上 OCR）轉成文字版 |
| **超長論文（>10頁）** | Codex 讀首尾各 5 頁；核心在中段時，告訴 Codex「請重點讀取第 X-Y 頁」|
| **截圖** | 模糊或文字過小效果較差，盡量使用清晰截圖 |
| **演講錄音** | 需先用 Whisper（免費）或 MacWhisper 轉成 .txt 逐字稿再丟入 raw/ |
| **需登入的網頁** | 學術資料庫等需登入的頁面，先存成 .html 再丟入 raw/web/ |
| **CSV 資料** | 適合「整體趨勢」查詢，不適合精確欄位查詢 |

---

## 限制彙總

| 限制 | 說明 |
|------|------|
| NotebookLM 無公開 API | notebooklm-skill 透過瀏覽器自動化操作，需保持 Google 登入（Session 可持續數週）|
| 每個 Notebook 上限 50 份來源 | 超過時依子主題拆分 |
| 無跨 Notebook 即時查詢 | 相關主題合併到同一 Notebook |
| Codex Skills 需 OpenAI 帳號 | Plus / Pro / Edu 方案可完整使用 |

---

*路線 A 最終版：Obsidian + Codex Desktop + notebooklm-skill + NotebookLM*
*適用對象：大學教授、學術研究者*
*建立日期：2026-05-15*
