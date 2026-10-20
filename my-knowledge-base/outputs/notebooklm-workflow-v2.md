---
title: 教授個人學術知識系統 v2：NotebookLM RAG 檢索 + Obsidian 知識庫 + Codex 智慧查詢
date: 2026-05-18
tags: [pkm/workflow, ai/agent, pkm/obsidian]
type: reference
status: processed
source: notebooklm-workflow.md（v1）+ 流程重構討論
---

# 教授個人學術知識系統 v2
## NotebookLM 檢索 → Obsidian 知識庫 → Codex 智慧查詢

> **適用對象**：大學教授、研究者，長期累積多元格式素材（PDF 論文、截圖、逐字稿、網頁），需要語意檢索 + 結構化知識管理的個人系統。
>
> **與 v1 的關鍵差異**：v1 是線性流程（Obsidian → Codex → NotebookLM），NotebookLM 在末端當展示層。v2 讓每個工具回歸最擅長的角色，形成三角分工。

---

## 核心理念：三角分工

```
┌─────────────────────────────────────────────────────────┐
│           NotebookLM（RAG 語意檢索引擎）                  │
│                                                         │
│  ・丟入論文 PDF、網頁、逐字稿                              │
│  ・語意搜尋 + 原文引用（頁碼標記）                          │
│  ・角色：檢索引擎，不是最終閱讀介面                         │
└──────────┬──────────────────────────────┬───────────────┘
           │ notebooklm-skill upload      │ notebooklm-skill ask
           │ （素材上傳）                   │ （檢索結果回傳）
           ▲                              ▼
┌──────────┴──────────────────────────────────────────────┐
│           Codex Desktop（AI 編排中心）                     │
│                                                         │
│  ・指揮所有 Skill 的執行                                   │
│  ・上傳素材到 NotebookLM                                  │
│  ・向 NotebookLM 檢索，取回帶引用的結果                     │
│  ・將結果交給 knowledge-organizer 結構化                    │
│  ・教授的智慧查詢介面（問問題、找論文、比較觀點）              │
└──────────┬──────────────────────────────────────────────┘
           │ knowledge-organizer / synthesizer
           │ （結構化寫入）
           ▼
┌─────────────────────────────────────────────────────────┐
│           Obsidian（結構化知識庫）                          │
│                                                         │
│  ・wiki/ 存放所有結構化條目                                │
│  ・wikilink 互相連結，形成知識網絡                          │
│  ・長期累積、可視覺化瀏覽                                   │
│  ・角色：知識資產的家，複利累積的地方                        │
└─────────────────────────────────────────────────────────┘
```

### 為什麼比 v1 更好？

| | v1（線性） | v2（三角分工） |
|--|-----------|--------------|
| NotebookLM 角色 | 末端展示層 | RAG 檢索引擎（回歸本職） |
| 知識累積位置 | 分散在 NotebookLM + Obsidian | 集中在 Obsidian |
| 查詢方式 | 必須開 NotebookLM 網頁 | 在 Codex 問，自動調度 |
| 結構化程度 | NotebookLM 無法結構化 | Obsidian wiki 完整結構 |
| 跨主題連結 | NotebookLM 跨 Notebook 困難 | Obsidian wikilink 無限制 |

---

## 工具清單與角色分工

| 工具 | 角色 | 操作者 |
|------|------|--------|
| **NotebookLM** | RAG 檢索引擎：語意搜尋 + 原文引用 | Codex 自動調度 |
| **Obsidian** | 結構化知識庫：視覺化管理、wikilink 連結 | 教授瀏覽 / 助理維護 |
| **Obsidian Web Clipper** | 瀏覽器一鍵存網頁到 raw/ | 教授 / 助理 |
| **Codex Desktop** | AI 編排中心：執行 Skills、串接工具 | 助理（初期），教授（熟悉後）|
| **notebooklm-skill** | 雙向橋樑：上傳素材 + 檢索查詢 | Codex 自動呼叫 |

---

## Skills 說明

| Skill | 功能 | 在 v2 中的角色 |
|-------|------|---------------|
| `knowledge-organizer` | 單篇素材 → TBRC 結構化 wiki 條目 | 將 raw/ 素材或 NotebookLM 檢索結果寫入 wiki/ |
| `raw-pipeline` | 10 篇以上批次處理 | 大量素材一次整理 |
| `knowledge-synthesizer` | 多篇同主題 → CAVE 精粹條目 | 同主題累積 3 篇後合成 |
| `notebooklm-skill` | 上傳 + 查詢 NotebookLM | v2 核心橋樑，雙向連結 |
| `obsidian-second-brain` | Vault-first 深度研究 + `/notebooklm` 指令 | 掃描 vault 找知識缺口，結果回存 wiki/ |
| `scientific-brainstorming` | 五階段學術腦力激盪（缺口識別 → 新假設） | 從精粹條目出發，探索研究新方向 |
| `knowledge-elevation` | 三層知識昇華（自動合成 / E層加深 / 跨主題連結） | 將精粹條目昇華為更高層次的洞察與概念框架 |

---

## 標準工作流程

### 流程一：新素材入庫（日常，5-20 分鐘）

```
Step 1  素材拖入 raw/ 對應子資料夾                （1 分鐘）
        PDF → papers/  │  截圖 → screenshots/
        逐字稿 → transcripts/  │  網頁 → web/

Step 2  Codex 執行 $knowledge-organizer           （5-15 分鐘自動完成）
        ├─ 依格式讀取素材
        ├─ 產出 wiki/paper-summaries/標題.md（TBRC 格式）
        └─ 更新 wiki/INDEX.md

Step 3  Codex 執行 $notebooklm-skill upload       （2-5 分鐘自動完成）
        ├─ PDF 原檔 → 上傳 NotebookLM（保留頁碼引用）
        ├─ 網頁 → 加入 URL
        └─ 其他格式 → wiki 摘要文字貼入

結果：素材同時存在 Obsidian（結構化）和 NotebookLM（可檢索）
```

### 流程二：智慧查詢（日常使用的核心）

```
教授在 Codex 提問：
「哪些論文討論過自主學習對學業成就的影響？」

Codex 自動執行：
Step 1  $notebooklm-skill ask
        → NotebookLM 語意檢索，回傳帶引用的結果
        → 「Smith (2023) [p.23] 發現...」「Johnson (2024) [p.45] 認為...」

Step 2  Codex 整理回答，直接呈現給教授
        → 結構化的答案 + 原文出處

（可選）教授：「把這個整理存起來」
Step 3  $knowledge-organizer
        → 將檢索結果結構化為 wiki 條目或 outputs/ 報告
```

**查詢範例：**

| 問法 | Codex + NotebookLM 的處理 |
|------|--------------------------|
| 「找支持 X 理論的論文」 | notebooklm-skill ask → 回傳論文清單 + 引用段落 |
| 「比較 A 和 B 兩篇的研究方法」 | notebooklm-skill ask → 比較分析 + 頁碼 |
| 「整理 X 主題的研究現況」 | notebooklm-skill ask → 結果 + knowledge-organizer 存入 wiki |
| 「草擬文獻回顧段落」 | notebooklm-skill ask → Codex 組織成學術文體 |
| 「wiki 裡有沒有 X 的整理？」 | 直接讀 Obsidian wiki/，不經 NotebookLM |

### 流程三：跨素材主題合成（同主題 ≥ 3 篇時）

```
Codex 執行 $knowledge-synthesizer [主題名稱]

產出 wiki/topic-synthesis/主題.md（CAVE 框架）：
C（共識）→ 學術共識是什麼？
A（爭論）→ 哪些論文有不同立場？
V（缺口）→ 還沒有人研究什麼？
E（精粹）→ 對你研究最重要的一個洞察

（可選）$notebooklm-skill upload → 精粹條目也上傳 NotebookLM，強化後續檢索
```

### 流程四：批次處理積壓（10 篇以上）

```
Codex 執行 $raw-pipeline              （30-60 分鐘，不需在旁等候）
→ 批次整理所有未處理素材

完成後執行 $notebooklm-skill upload
→ 批次上傳所有新條目到 NotebookLM
```

### 流程五：每月知識庫維護

```
Codex 執行知識庫健康檢查，回報：
├─ 還有哪些素材尚未整理
├─ 哪些摘要可能已過時（建立超過 6 個月）
├─ 哪些主題適合執行 knowledge-synthesizer
└─ NotebookLM 各 Notebook 的來源數量（接近 50 上限時提醒拆分）
```

### 流程六：知識昇華與腦力激盪（同主題精粹完成後）

知識昇華是知識管理的最高層次：從已整理的精粹出發，找出研究缺口、生成新假設、開拓跨領域連結。使用 `obsidian-second-brain` 的 vault-first 掃描配合 `scientific-brainstorming` 的五階段探索。

```
前置條件：wiki/topic-synthesis/主題.md 已由 knowledge-synthesizer 產出

Step 1  Codex 執行 obsidian-second-brain /notebooklm [主題]
        ├─ 自動掃描 vault，找出與主題最相關的 12 篇筆記
        ├─ 產出 bundle（現有知識摘要）
        ├─ 提示教授：開 NotebookLM，貼入 bundle 作為來源（手動步驟，~3 分鐘）
        └─ 將 NotebookLM 回傳結果存回 wiki/（delta report）
           ・what's new（新發現）
           ・what's confirmed（已驗證）
           ・contradictions（矛盾待解）
           ・open questions（待探索問題）

Step 2  Codex 執行 scientific-brainstorming [主題]
        ├─ 以 wiki/topic-synthesis/ + delta report 為背景
        ├─ Phase 1：教授說明目前研究問題與困境
        ├─ Phase 2：Codex 發散探索（跨域類比、假設反轉、尺度轉換）
        ├─ Phase 3：連結模式識別（同一概念在多篇出現 → 自動觸發合成）
        ├─ Phase 4：批判性評估最有潛力的方向
        └─ Phase 5：結晶洞察 + 具體下一步（文獻搜尋、小規模實驗）

Step 3  產出存入 ideas/主題-brainstorm-YYYY-MM-DD.md
        ├─ 新假設清單（含信心等級：speculation / medium / high）
        ├─ 建議閱讀方向（下一步要找哪類論文）
        └─ 跨學科連結（其他領域的類比與啟發）
```

**整條知識流向（從素材到洞察）：**

```
raw/ 素材
  ↓ knowledge-organizer
wiki/paper-summaries/（結構化摘要）
  ↓ knowledge-synthesizer（同主題 ≥ 3 篇觸發）
wiki/topic-synthesis/（CAVE 精粹：共識 / 爭論 / 缺口 / 洞察）
  ↓ obsidian-second-brain /notebooklm（vault-first 缺口確認）
delta report（新知 / 確認 / 矛盾 / 待探問題）
  ↓ scientific-brainstorming（五階段學術腦力激盪）
ideas/（新假設 + 研究方向 + 跨域連結）
  ↓ 見流程七：knowledge-elevation（知識昇華）
wiki/concepts/（跨主題概念框架 + 高層洞察）
```

> **使用時機**：每次 knowledge-synthesizer 產出新的精粹條目後，視需求決定是否進入流程六。不是每個主題都需要跑到 ideas/，重點主題、困卡住的研究方向、準備投稿的主題優先觸發。

### 流程七：知識昇華（三層，從精粹到概念框架）

知識昇華在腦力激盪之上再走一層：把零散的精粹與洞察提煉成可復用的概念框架。三層各自獨立，可依需求單獨觸發，也可依序執行。

```
前置條件：wiki/topic-synthesis/ 已有精粹條目（流程三完成）
```

#### 第一層：自動模式偵測（Synthesis Hook）

obsidian-second-brain 在每次寫入 vault 時自動偵測：**同一概念在 3 個以上不相關來源出現 → 自動在 `wiki/concepts/` 建立合成頁面**。

無需手動觸發。但建議定期確認：

```
Codex：「列出 wiki/concepts/ 最近新增的合成頁面，摘要每頁的核心洞察」
```

產出位置：`wiki/concepts/概念名稱.md`（自動，含所有來源的 wikilink）

---

#### 第二層：CAVE E 層加深（Deep Essence）

`knowledge-synthesizer` 產出的 E（精華）層預設是自由格式，容易流於表面。E 層加深要求結構化輸出。

觸發方式：
```
Codex：「對 wiki/topic-synthesis/學習動機.md 執行 E 層加深，
        重新產出一份結構化的 E 層」
```

**結構化 E 層格式（強制）：**
```markdown
## E（精華—Deep Essence）

**核心洞察（一句話）**：
[去掉所有背景後，這個主題最重要的一個發現是什麼？]

**信心等級**：stated | high | medium | speculation
**依據**：[哪幾篇來源支撐這個洞察，含頁碼]

**對你研究的意義**：
[為什麼這個洞察對你目前的研究問題特別重要？]

**最大反例**：
[什麼情況下這個洞察會不成立？]
```

產出：更新 `wiki/topic-synthesis/主題.md` 的 E 層，原版本保留為 `E-original`。

---

#### 第三層：跨主題連結（Meta-Synthesis）

當多個主題的精粹條目累積後，跨主題再跑一次合成，找出超越單一主題的更高層概念。

觸發時機：`wiki/topic-synthesis/` 累積 3 個以上相關主題的精粹條目。

```
Codex：「讀取 wiki/topic-synthesis/ 中與「教育科技」相關的所有精粹，
        執行跨主題 meta-synthesis，找出跨越個別主題的共同概念框架」
```

執行邏輯：
```
Step 1  讀取多個 topic-synthesis 條目
Step 2  識別跨主題出現的共同概念、矛盾、意外連結
Step 3  產出 wiki/concepts/meta-教育科技框架.md
        結構：
        ・共同前提（所有主題都預設的假設）
        ・核心張力（主題間最重要的對立）
        ・上層概念（整合後的更高層框架）
        ・研究空白（跨主題後才看得到的缺口）
```

產出位置：`wiki/concepts/meta-主題.md`

---

**三層昇華的觸發節點：**

| 層次 | 觸發條件 | 觸發方式 | 產出位置 |
|------|---------|---------|---------|
| 自動模式偵測 | 同概念在 3+ 來源出現 | 自動（obsidian-second-brain） | `wiki/concepts/` |
| E 層加深 | 精粹條目的 E 層需要強化 | 手動指定主題 | 更新 `wiki/topic-synthesis/` |
| 跨主題連結 | 同領域 ≥ 3 個精粹條目 | 手動指定領域 | `wiki/concepts/meta-` |

---

## 兩個查詢入口的使用時機

v2 保留兩個查詢入口，依情境選擇：

| 情境 | 建議入口 | 原因 |
|------|---------|------|
| 找特定論文的原文段落 | Codex（經 NotebookLM） | RAG 語意檢索 + 頁碼引用 |
| 查已整理過的知識 | Codex（直接讀 wiki/） | 結構化條目，讀取更快 |
| 跨論文比較觀點 | Codex（經 NotebookLM） | 需要同時檢索多份來源 |
| 視覺化瀏覽知識網絡 | Obsidian | Graph View、wikilink 導覽 |
| 聽論文重點（通勤） | NotebookLM 網頁 | Audio Overview 功能 |
| 準備授課用研究指南 | NotebookLM 網頁 | 自動生成 FAQ + 關鍵概念 |

> **原則**：日常查詢走 Codex（自動判斷要不要調度 NotebookLM），只有 Audio Overview 和研究指南等 NotebookLM 獨有功能才直接開網頁。

---

## 資料夾結構（與 v1 相同）

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

## NotebookLM 組織方式

按**研究主題**建立 Notebook：

```
[課程] 研究所課程名稱
[計畫] 國科會計畫名稱
[主題] 學習動機理論
[主題] 教育科技評估方法
[書]   正在閱讀的學術書籍
[會議] 2026 AERA 論文集
```

> 每個 Notebook 上限 50 份來源。主題過大時拆成子主題 Notebook。

---

## 論文摘要格式（TBRC 學術版，與 v1 相同）

```markdown
---
title: 標題
date: 整理日期
tags: [研究主題, 研究方法]
source: 原始檔案路徑 / DOI / URL
source_format: pdf | md | screenshot | transcript | html | csv
type: paper-summary
status: processed
authors: 作者
year: 發表年份
journal: 期刊名稱
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
1. [p.X] 發現一
2. [p.Y] 發現二
3. [p.Z] 發現三

## C（對研究的意義）
- [ ] 具體啟發或後續行動

## 引用格式
[APA 7th]
```

---

## 支援的輸入格式（12 種）

| 格式 | 副檔名 | Codex 讀取方式 | NotebookLM 上傳方式 |
|------|--------|--------------|-------------------|
| PDF 論文 | `.pdf` | 全文（≤10頁）或首尾節錄 | PDF 原檔上傳 |
| Markdown 筆記 | `.md` | 直接讀取全文 | wiki 摘要文字貼入 |
| 截圖／投影片 | `.png` `.jpg` `.webp` | 視覺解析 | wiki 摘要文字貼入 |
| 逐字稿／字幕 | `.txt` `.srt` `.vtt` | 去時間戳，提取核心 | wiki 摘要文字貼入 |
| 網頁存檔 | `.html` | 提取正文 | URL 直接加入 |
| Jupyter Notebook | `.ipynb` | 提取說明 + 程式邏輯 | wiki 摘要文字貼入 |
| 資料表格 | `.csv` | 摘要欄位與數據 | wiki 摘要文字貼入 |

---

## 初次設定步驟（一次性，由助理操作）

### 安裝工具

| 工具 | 安裝方式 |
|------|---------|
| Obsidian | obsidian.md 官網下載 |
| Obsidian Web Clipper | Chrome / Firefox 擴充套件 |
| Codex Desktop | `npm install -g @openai/codex` → `codex app` |
| notebooklm-skill | `pip install notebooklm-skill` 或 `uvx notebooklm-skill` |
| notebooklm-skill 登入 | `uvx notebooklm login`（一次性 Google 登入） |

### 建立知識庫結構

```bash
mkdir -p my-knowledge-base/{raw/{papers,screenshots,transcripts,notes,web},wiki/{paper-summaries,topic-synthesis},outputs,skills,archive}
```

### AGENTS.md 設定（放在 my-knowledge-base/ 根目錄）

告訴 Codex 知識庫的規則：資料夾結構、raw/ 唯讀、TBRC 學術版格式、語言設定。詳見 v1 文件或打包的 AGENTS.md。

---

## 限制彙總

| 限制 | 說明 |
|------|------|
| NotebookLM 無公開 API | notebooklm-skill 透過瀏覽器自動化，需保持 Google 登入 |
| 每個 Notebook 上限 50 份來源 | 超過時依子主題拆分 |
| 無跨 Notebook 即時查詢 | 相關主題合併到同一 Notebook |
| Codex Skills 需 OpenAI 帳號 | Plus / Pro / Edu 方案 |
| notebooklm-skill ask 的回傳 | 文字結果，不含 PDF 原檔頁面截圖 |

---

## 與 v1 的差異摘要

| 項目 | v1 | v2 |
|------|----|----|
| 架構 | 線性（Obsidian → Codex → NotebookLM） | 三角分工（各司其職） |
| NotebookLM 角色 | 末端查詢介面 | RAG 檢索引擎 |
| 主要查詢入口 | NotebookLM 網頁 | Codex Desktop |
| 知識累積位置 | 分散 | 集中在 Obsidian wiki/ |
| notebooklm-skill 用法 | 單向上傳 | 雙向（upload + ask） |
| 日常操作 | 開 NotebookLM 查 | 在 Codex 問，自動調度 |
| 腦力激盪 | 無 | 流程六：obsidian-second-brain + scientific-brainstorming |
| 知識昇華 | 無 | 流程七：三層昇華（自動合成 / E層加深 / 跨主題連結）|
| 最終產出 | wiki 條目 | wiki 條目 + 研究假設（ideas/）+ 概念框架（wiki/concepts/）|

---

## 附錄：Brainstorming Skills 參考

搜尋評估後保留三個選項，目前採用前兩個搭配，第三個列為備選。

### ✅ 採用：scientific-brainstorming（K-Dense-AI）

**定位**：學術研究腦力激盪，專為研究缺口識別 + 新假設生成設計
**GitHub**：`K-Dense-AI/scientific-agent-skills`
**授權**：MIT

**五階段工作流：**
| 階段 | 內容 |
|------|------|
| Phase 1 Context | 深度理解研究現況，找出隱含假設 |
| Phase 2 Diverge | 發散探索：跨域類比、假設反轉、尺度轉換、科技思辨 |
| Phase 3 Connect | 識別跨想法的模式與意外連結 |
| Phase 4 Evaluate | 批判性評估最有潛力的方向，提出小規模驗證方案 |
| Phase 5 Synthesize | 結晶洞察 + 具體下一步（文獻方向、協作對象、實驗設計）|

**核心設計原則**：對話而非講課（研究者至少說 50%）；對研究者的工作表現真實好奇；不用對方不熟的術語；舒適於沉默，給空間思考。

**與 Superpowers brainstorming 的差異**：

| | Superpowers brainstorming | scientific-brainstorming |
|--|--------------------------|--------------------------|
| 設計場景 | 軟體需求釐清 | 學術研究探索 |
| 核心概念 | YAGNI、設計文件、spec | 研究缺口、跨域類比、假設 |
| 產出 | 軟體設計文件 | 研究假設清單 + 下一步方向 |
| 適用 | 工程師 | 研究者、教授 |

**安裝：**
```bash
/plugin install scientific-brainstorming@K-Dense-AI
# 或
gh skill install K-Dense-AI/scientific-agent-skills
```

---

### ✅ 採用：obsidian-second-brain（eugeniughelbur）

**定位**：Karpathy LLM Wiki 進化版，32 個指令讓 vault 自我改寫；含 6 個研究工具指令
**GitHub**：`eugeniughelbur/obsidian-second-brain`

**在流程六中使用的指令：**

| 指令 | 說明 |
|------|------|
| `/notebooklm [topic]` | Vault-first 來源接地研究：先掃 vault 找 12 篇最相關筆記，產出 bundle → 貼入 NotebookLM → 結果存回 wiki/（含 delta report）|
| `/research-deep [topic]` | Vault-first + Perplexity 深度研究，全自動（費用 $0.20-0.80）|

**Vault-first 的核心邏輯**：
```
先掃描你已有的知識（知道什麼）
→ 只對缺口進行外部搜尋
→ 產出 delta report（新 / 確認 / 矛盾）
→ 回寫 vault（不是追加，是改寫，更新傳播到所有相關筆記）
```

**注意**：`/notebooklm` 指令的 Step 4 需要手動操作（開 NotebookLM 網頁、貼入 bundle、複製回應），約 3-5 分鐘。

**安裝：**
```bash
# 一鍵安裝（建議）
curl -sL https://raw.githubusercontent.com/eugeniughelbur/obsidian-second-brain/main/scripts/quick-install.sh | bash

# 使用 researcher preset（最適合學術場景）
python scripts/bootstrap_vault.py --path ~/my-knowledge-base --name "教授姓名" --preset researcher
```

**researcher preset** 預設建立：Sources、Literature notes、Hypotheses、Methodology 資料夾；Kanban：Reading / Processing / Synthesized。

---

### 📋 備選：research-knowledge-super-skill（get-zeked）

**定位**：整合 Perplexity 研究 + 知識圖譜 + RAG + brainstorming + 內容合成的超級 Skill
**GitHub**：`get-zeked/research-knowledge-super-skill`

**優點**：功能最全，含統計分析與視覺化
**缺點**：功能較重，與現有 notebooklm-skill 有部分重疊

**建議使用時機**：當需要大量外部研究資料（非自有文獻）的跨領域探索時，可取代 `/research-deep` 使用。目前以 scientific-brainstorming + obsidian-second-brain 為主，此 Skill 列為備選。

---

*v2：三角分工架構（NotebookLM 檢索 + Obsidian 知識庫 + Codex 查詢）*
*v2.1：新增流程六（知識昇華 + 腦力激盪）*
*適用對象：大學教授、學術研究者*
*建立日期：2026-05-18*
*前版：outputs/notebooklm-workflow.md（v1）*
