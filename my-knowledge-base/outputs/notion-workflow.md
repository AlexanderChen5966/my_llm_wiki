---
title: 教授個人學術 RAG 系統：Obsidian + Codex Desktop + Notion 完整建立指南
date: 2026-05-15
tags: [pkm/workflow, ai/agent, pkm/obsidian]
type: reference
status: processed
source: notebooklm-workflow.md（Notion 改編版）
---

# 教授個人學術 RAG 系統
## Obsidian + Codex Desktop + Notion 完整建立指南

> **適用對象**：大學教授、研究者，需要結構化管理學術素材，並透過 Codex 自然語言查詢知識庫。
>
> **與 NotebookLM 版的核心差異**：Notion 以結構化資料庫取代 Notebook，查詢透過 Codex + Notion MCP 執行，頁碼引用來自 wiki 摘要本身（而非 PDF 索引）。

---

## 整體架構

```
┌─────────────────────────────────────────────────────────┐
│              Obsidian（素材管理視覺介面）                  │
│  拖入任何格式 → raw/ 資料夾                               │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│              Codex Desktop（AI 整理引擎）                 │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │  Skills                                          │    │
│  │  knowledge-organizer  → 單篇 TBRC 結構化（含頁碼）│    │
│  │  raw-pipeline         → 批次處理                  │    │
│  │  knowledge-synthesizer → 跨素材主題合成            │    │
│  └─────────────────────────────────────────────────┘    │
│                                                         │
│  Notion MCP（官方 API）                                  │
│  └─ 自動把 wiki .md → Notion 頁面 + Database 記錄        │
└──────────────────────┬──────────────────────────────────┘
                       │ Notion MCP 自動建立
                       ▼
┌─────────────────────────────────────────────────────────┐
│              Notion（結構化知識庫）                       │
│  知識條目庫（Database）                                   │
│  ├─ 每篇摘要 = 一個 Notion 頁面                          │
│  ├─ frontmatter = Database 屬性（標籤 / 年份 / 格式）     │
│  └─ R 層內容含頁碼引用（[p.23] 具體發現）                 │
└──────────────────────┬──────────────────────────────────┘
                       │ Codex + Notion MCP 查詢
                       ▼
┌─────────────────────────────────────────────────────────┐
│              Codex Desktop（查詢介面）                    │
│  教授向 Codex 提問                                        │
│  Codex 透過 Notion MCP 搜尋知識庫                         │
│  回答含頁碼引用（來自 wiki 摘要的 [p.X] 標記）             │
└─────────────────────────────────────────────────────────┘
```

---

## 與 NotebookLM 版的對照

| 面向 | NotebookLM 版 | **Notion 版** |
|------|--------------|--------------|
| 上傳工具 | notebooklm-skill（瀏覽器自動化）| **Notion MCP（官方 API）**|
| 認證方式 | Google 登入 + Session | **Notion API Key（一次設定）**|
| PDF 索引 | AI 直接讀 PDF 全文 | 頁碼引用存於 wiki 摘要內 |
| 引用格式 | [1] 跳到 PDF 頁碼 | [p.23] 出現在摘要文字中 |
| 查詢介面 | NotebookLM Chat | **Codex Desktop + Notion MCP** |
| 知識組織 | Notebook（主題型）| **Database（屬性 / 篩選 / 關聯）**|
| 協作能力 | 弱 | **強（多人即時共用）**|
| 穩定性 | 瀏覽器自動化（較脆弱）| 官方 API（較穩定）|

---

## Layer 1：本機端（完全不變）

資料夾結構、12 種輸入格式支援、Skills 清單與 AGENTS.md 設定，與 NotebookLM 版完全相同。

唯一差異：**TBRC 格式的 R 層要求加入頁碼標記。**

---

## TBRC 學術版格式（加入頁碼引用）

R 層每一條發現必須標記來源頁碼：

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

## B（研究設計）
**研究問題**：
**研究方法**：
**樣本 / 資料**：
**適用場景**：
**限制**：

## R（主要發現）
1. [p.23] 學生自主學習與學業成就呈正相關（r=0.67）
2. [p.45-47] 三種干預策略中，同儕互評效果最顯著
3. [p.89] 研究限制：樣本偏重大學生族群，高中生適用性待驗證

## C（對研究的意義）
- [ ] 具體啟發或後續行動

## 引用格式
Smith, J. (2023). Title. *Journal*, 12(3), 45-67. https://doi.org/...
```

> **頁碼的意義**：上傳到 Notion 後，Codex 查詢時回答裡的引用格式是 `Smith (2023), p.23`，教授可直接翻到原始 PDF 對應頁面確認。

---

## Layer 2：Notion 知識庫建立

### Notion Database 結構

建立「知識條目庫」Database，屬性對應 wiki frontmatter：

| Notion 屬性 | 類型 | 對應 frontmatter |
|------------|------|----------------|
| 標題 | Title | `title` |
| 建立日期 | Date | `date` |
| 標籤 | Multi-select | `tags` |
| 來源 | URL | `source` |
| 格式 | Select | `source_format`（pdf / md / screenshot…）|
| 狀態 | Status | `status` |
| 作者 | Text | `authors` |
| 年份 | Number | `year` |
| 期刊 | Text | `journal` |
| 相關條目 | Relation → 自身 | wikilink 關聯 |

### Notion 工作區結構

```
📚 學術知識庫（頂層頁面）
├── 🧠 知識條目庫（Database）    ← wiki/ 條目全部在這裡
├── 🔬 主題合成（Database）      ← topic-synthesis/ 條目
├── 🗺️ 知識地圖（頁面）          ← INDEX.md 對應
└── 📤 產出庫（頁面）            ← outputs/ 對應
```

### Notion MCP 自動建立頁面

Codex 整理完 wiki .md 後，透過 Notion MCP 自動執行：

```
wiki/paper-summaries/Smith2023.md
    ↓ Notion MCP
Notion 知識條目庫 → 新增一筆記錄：
  ├─ 標題：Smith (2023) - 標題
  ├─ 屬性：年份=2023, 格式=pdf, 標籤=[學習動機, 量化研究]
  └─ 頁面內容：T / B / R（含頁碼）/ C / 引用格式
```

---

## Notion MCP 設定

### 安裝官方 Notion MCP

```bash
# 安裝 Notion MCP Server
npm install -g @notionhq/notion-mcp-server

# 或使用 npx 直接執行（不需全域安裝）
npx @notionhq/notion-mcp-server
```

### 取得 Notion API Key

1. 前往 [notion.so/my-integrations](https://notion.so/my-integrations)
2. 建立新的 Integration，名稱如「Codex 知識庫」
3. 複製 API Key（格式：`secret_xxxx`）
4. 在 Notion 知識條目庫頁面 → 右上角「...」→「連結到 Integration」

### 在 Codex Desktop 設定 MCP

在 Codex 的 MCP 設定檔加入：

```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["@notionhq/notion-mcp-server"],
      "env": {
        "NOTION_API_KEY": "secret_xxxx"
      }
    }
  }
}
```

設定完成後，Codex 可用的 Notion 操作：
- 搜尋 workspace 全文
- 讀取任意頁面內容
- 建立新頁面 / 新 Database 記錄
- 更新現有頁面

---

## 標準工作流程

### 新素材入庫（日常）

```
Step 1  把素材拖入 raw/ 對應子資料夾            （1 分鐘）

Step 2  在 Codex Desktop 執行：
        $knowledge-organizer                    （5-15 分鐘）

        Codex 自動：
        ├─ 依格式讀取
        ├─ 產出 wiki/.md（R 層含頁碼 [p.X]）
        └─ 更新 wiki/INDEX.md

Step 3  Codex 透過 Notion MCP 自動建立頁面       （1-2 分鐘）
        └─ 知識條目庫新增一筆記錄，立即可查詢

Step 4  向 Codex 提問（見下方查詢指南）
```

### 批次處理積壓素材（10 篇以上）

```
$raw-pipeline → 批次整理（30-60 分鐘）
    ↓ 完成後
Notion MCP 批次建立頁面（自動）
    ↓
全部可查詢
```

### 跨素材主題合成

```
$knowledge-synthesizer [主題名稱]
    ↓
wiki/topic-synthesis/主題.md（CAVE 框架）
    ↓ Notion MCP
Notion「主題合成」Database 新增記錄
```

---

## 查詢方式：Codex + Notion MCP

教授在 Codex Desktop 提問，Codex 透過 Notion MCP 搜尋知識庫後回答。

### 查詢範例

**找支持某論點的論文：**
```
在 Notion 知識庫中，哪些論文支持「自主學習能提升學業成就」？
列出論文標題、作者、年份，以及對應的頁碼。
```

**比較不同論文：**
```
Smith (2023) 和 Johnson (2024) 的研究方法有什麼不同？
從 Notion 知識庫找出兩篇的 B 層（研究設計）資訊。
```

**整理研究現況：**
```
從 Notion 知識庫找出所有標籤含「學習動機」的條目，
整理這個主題的整體共識和主要爭論。
```

**準備文獻回顧：**
```
從 Notion 知識庫，草擬一段「X 理論發展歷程」的文獻回顧，
每個論點附上作者、年份和頁碼。
```

### Notion Database 視覺化瀏覽（無需 AI）

教授也可以直接在 Notion 介面：
- **按標籤篩選**：找出所有「質性研究」論文
- **按年份排序**：看最新的文獻
- **按格式篩選**：只看 PDF 論文或演講逐字稿
- **Gallery View**：視覺化瀏覽所有條目

---

## 初次設定步驟（由助理操作）

### 安裝工具

| 工具 | 安裝方式 |
|------|---------|
| Obsidian | obsidian.md 官網下載 |
| Obsidian Web Clipper | Chrome / Firefox 擴充套件 |
| Codex Desktop | `npm install -g @openai/codex` → `codex app` |
| Notion MCP Server | `npm install -g @notionhq/notion-mcp-server` |

### Notion 設定清單

- [ ] 建立 Notion 帳號（若尚未有）
- [ ] 建立「學術知識庫」頂層頁面
- [ ] 建立「知識條目庫」Database，設定屬性（照上方屬性表）
- [ ] 建立「主題合成」Database
- [ ] 前往 notion.so/my-integrations 建立 Integration，取得 API Key
- [ ] 在知識庫頁面連結 Integration
- [ ] 在 Codex Desktop 設定 MCP 連線

### AGENTS.md 增加 Notion 操作規則

```markdown
## Notion 知識庫操作規則

整理完 wiki/.md 後，透過 Notion MCP 執行：
1. 在「知識條目庫」建立新頁面，標題 = .md 的 title
2. 依 frontmatter 填入 Database 屬性
3. 頁面內容 = .md 全文（T / B / R / C / 引用格式）
4. 若已存在同標題頁面，更新內容而非重複建立

TBRC 的 R 層必須包含頁碼標記，格式：[p.X] 或 [p.X-Y]
```

---

## 各格式注意事項

| 格式 | 注意事項 |
|------|---------|
| **PDF 論文** | Codex 讀全文產出摘要（含頁碼）；PDF 原檔附在 Notion 頁面作為附件 |
| **截圖** | 視覺解析產出摘要；若截圖有頁碼（投影片頁數），請在 R 層標記 [slide X] |
| **逐字稿** | 去除時間戳；若有時間戳可標記 [00:23:45] 取代頁碼 |
| **網頁** | URL 存為 Notion 來源欄位；引用格式為連結 + 存取日期 |
| **掃描版 PDF** | 需先 OCR 轉文字版，否則 Codex 無法讀取頁碼 |

---

## 限制彙總

| 限制 | 說明 | 因應方式 |
|------|------|---------|
| AI 查詢範圍限於 wiki 摘要 | Notion AI / Codex MCP 讀頁面文字，非 PDF 全文 | R 層摘要寫詳盡 + 頁碼標記 |
| Notion AI 需付費方案 | Notion AI 附加套件需額外訂閱 | 用 Codex + Notion MCP 查詢可不需 Notion AI |
| Database 無上限但查詢較慢 | 條目超過 500 筆時搜尋速度下降 | 依研究計畫拆分多個 Database |
| Notion MCP 需 API Key | 每個 Integration 只能存取被邀請的頁面 | 設定時確認 Integration 連結到正確頁面 |

---

*路線 A Notion 版：Obsidian + Codex Desktop + Notion MCP + Notion*
*適用對象：大學教授、學術研究者（偏好結構化資料庫 + Codex 查詢）*
*建立日期：2026-05-15*
