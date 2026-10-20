# Obsidian 個人知識庫規劃：從知識整理到資產化的完整路線圖

---

*文件版本：v2.0 | 更新日期：2026-05-15 | 適用平台：Obsidian + Claude Code*

---

## 第一部分：五篇文章的共通方法拆解

### 核心共識：AI + Obsidian = 活的知識系統

五篇文章來自不同作者、不同角度，但都指向同一個結論：**Obsidian 的本地 Markdown 檔案結構，天然適合讓 AI 直接讀寫，因此能從「被動筆記」升級為「主動知識系統」**。

以下是從五篇內容提煉出的六項共通方法：

---

### 共通方法 1：以 Markdown 為基礎設施

所有文章都強調一件事：不用資料庫、不用私有格式，用純文字 Markdown。

理由很簡單——Markdown 是 AI 最能理解的格式。Claude Code 可以直接進入你的資料夾讀取、寫入、更新，不需要複製貼上。NxCode 指南提到 Obsidian 截至 2026 年 2 月已超過 150 萬用戶，22% 年增長，正是因為本地優先的 Markdown 架構讓任何 AI 模型都能接入。

WenHao Yu 在他的個人 AI 工作流文章中，把這稱為「可程式化的基礎設施」——所有文件用 Markdown，純文字、跨平台、AI 可以直接讀寫。

**落地做法：** 在 Obsidian 中建立 Vault，所有筆記一律用 `.md` 格式，frontmatter 統一使用 YAML 標記 tags、日期、來源。

---

### 共通方法 2：資料夾結構採用語意分層

多篇文章都建議以語意功能劃分資料夾，而非按主題分類。核心邏輯是**收集和整理分離**：

```
my-knowledge-base/
├── raw/          ← 原始素材丟入區（唯讀，AI 不修改）
├── wiki/         ← 整理後的知識條目（AI 全權維護）
├── outputs/      ← AI 產出的報告、分析、每日簡報
├── ideas/        ← 靈感與初步想法
├── research/     ← 深度研究與分析筆記
├── projects/     ← 進行中的專案相關筆記
├── templates/    ← 筆記模板
└── archive/      ← 完成或停用的內容（不刪除，只移動）
```

Karpathy 的方法更極簡，只用三個資料夾（raw / wiki / outputs）加一份 CLAUDE.md 設定檔。但核心邏輯相同：**人只負責把東西丟進 raw/，AI 負責整理**。

---

### 共通方法 3：讓 AI 代替你做整理工作

這是五篇文章最大的共通主張。

經理人的文章引述 Karpathy 的做法：你只需要把資料丟進去，摘要、分類、交叉連結、維護全部交給 AI。他用這個方法在某個研究主題上累積了將近 100 篇筆記、40 萬字，而且查得動。

數位時代兩篇文章拆解了具體流程：用 Obsidian Web Clipper 一鍵擷取網頁 → Claude Code 自動讀取 raw/ → 生成摘要、加標籤、建立 wikilink → 更新 wiki/INDEX.md。

WenHao Yu 更進一步建立了完整的自動化循環：每日 AI 自動生成 Brief，涵蓋目標追蹤、積壓素材提醒、行動建議。

**共通原則：人負責輸入（capture）和思考，AI 負責整理（organize）和連結（link）。**

---

### 共通方法 4：Claude Code 直接操作檔案系統

NxCode 指南稱之為「power combo」——Claude Code 直接存取 Obsidian Vault 資料夾，實現讀取、搜尋、建立、修改筆記。

這不是在聊天框裡交換文字，而是 AI 真正進入你的檔案系統操作。五篇文章中有四篇都提到 Claude Code 是這套系統的核心引擎。

**關鍵差異：** 一般的 ChatGPT 或 Claude 網頁版只能在對話框裡互動；Claude Code 則可以直接操作你的資料夾，像一個能用你電腦的 AI 助理。

> **注意：** 不需要安裝 MCP（Model Context Protocol）外掛。Claude Code 本身就能直接讀寫本地 Markdown 檔案，MCP 是進階選配（用於連接外部服務），不是必要前提。

---

### 共通方法 5：Context Engineering（情境工程）

NxCode 文章特別提出「Context Engineering」的概念：你的筆記結構越一致（統一 tags、links、metadata），AI 就越能檢索到正確的上下文。

這呼應了所有文章的隱含訊息：**知識庫不只是「存資料」，而是「為 AI 做好情境準備」**。每一篇筆記都是未來 AI 互動的上下文素材。

關鍵做法：
- 每篇筆記開頭加 YAML frontmatter（見第三部分的標準格式）
- 相關主題用 `[[wikilink]]` 互相連結
- 維護一份 `wiki/INDEX.md` 作為知識地圖

---

### 共通方法 6：漸進式建設，不追求完美

所有文章都不約而同地警告「完美主義」的陷阱：

- NxCode：先亂輸入，之後再整理（Embrace messy initial capture）
- Karpathy：極簡結構，三個資料夾就夠
- WenHao Yu：花三個月從空資料夾長成完整系統
- 經理人文章：用最快方式建起來，先跑再說

---

### 共通方法對照表

| 方法 | NxCode 指南 | 經理人 | 數位時代(Karpathy) | 數位時代(Obsidian+CC) | WenHao Yu |
|------|:-----------:|:------:|:------------------:|:---------------------:|:---------:|
| Markdown 為基礎 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 語意分層結構 | ✅ | — | ✅（極簡版） | ✅ | ✅（LYT） |
| AI 負責整理 | ✅ | ✅ | ✅ | ✅ | ✅ |
| Claude Code 直接操作 | ✅ | ✅ | ✅ | ✅ | ✅ |
| Context Engineering | ✅ | — | — | — | ✅ |
| 漸進式建設 | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## 第二部分：知識資產化——用 MAPS 框架讓知識庫產生複利

### Axton Liu 的核心主張

Axton Liu 的 MAPS 方法論和 Claude Skills 實測文章，提供了一個關鍵的思維升級：

> **從「知識整理」到「知識資產化」**
> 知識庫不只是「查得到」，而是要變成「可復用、可傳承、可規模化」的資產。

---

### MAPS 框架應用於知識庫

MAPS 是乘法關係（M × A × P × S），任何一項為零，整體歸零。

**M（Mindset 心智模式）— 資產化思維**

關鍵轉變：從「我把筆記存好了」→「我把判斷邏輯封裝成可復用的模組」。

每次你在 Obsidian 中整理資料時，問自己：
- 這個整理動作會重複嗎？（頻率）
- 每次花多長時間？（耗時）
- 需要專業判斷嗎？（複雜度）

用 5A+ 的公式計算：**資產化價值 = 頻率（1-5）× 耗時（1-5）× 複雜度（1-5）**

- ≥ 50 → 值得封裝成 wiki 條目或 Skill
- 20-49 → 放 research/ 深化研究
- < 20 → 放 archive/ 存查

**A（Architecture 架構設計）— 模組化知識結構**

把業務流程、數據流、角色權限抽象成可演進的「資訊建築」——不是寫程式碼的能力，是畫藍圖的能力。在知識庫情境中，這意味著：
- 每個「知識模組」只負責一個主題（單一職責）
- 輸入輸出明確（frontmatter 接口清晰）
- 模組之間透過 wikilink 和 tag 連結（可組合性）

**P（Prompt 提示工程）— 將整理邏輯寫成指令**

把你的「整理筆記」習慣轉化為 Claude 可執行的 Skill。例如 TBRC 框架：
- T（核心論點）：這篇的核心主張是什麼？
- B（適用場景）：適用於什麼情況？不適用於什麼？
- R（操作步驟）：具體怎麼執行？
- C（關鍵行動）：下一步要做什麼？

**S（Systems 系統化落地）— 用 5A+ 循環迭代**

不需要一次做完，用 5A+ 循環：
1. AIM：定義你的知識庫要解決什麼問題
2. ACQUIRE：收集必要的模板和工具
3. ATTEMPT：先建一個最小可用版本
4. ADJUST：用了兩週後根據實際體驗優化
5. APPLY：穩定後擴展到更多領域
6. PLUS：持續沉澱最佳實踐，或擴展新主題領域

---

### Claude Skills 如何讓知識庫資產化

Skills 是封裝好的判斷邏輯，讓 Claude 每次按同樣標準處理輸入。有兩種核心類型：

**能力包型（Capability Package）**

把你的判斷邏輯封裝進一個 SKILL.md。例如 `knowledge-organizer` Skill 內含 TBRC 四層判斷邏輯，5 分鐘完成原本需要 30-60 分鐘的筆記整理。

**軟編排型（Soft Orchestration）**

多個獨立 Agent 協作完成複雜流程。例如 `raw-pipeline` Skill 透過三個 Agent（Team Lead → 摘要 Agent → 分類 Agent → 索引 Agent）協作，批次處理 10 篇以上的積壓素材。

---

## 第三部分：整合規劃——Obsidian 知識庫 × MAPS 資產化的實戰架構

### 整體架構

```
┌─────────────────────────────────────────────────────┐
│               你的 Obsidian Vault                    │
├─────────────────────────────────────────────────────┤
│                                                     │
│  輸入層（Capture）                                   │
│  ├─ Web Clipper 擷取文章 → raw/                      │
│  ├─ 手動拖入：截圖、PDF、逐字稿 → raw/               │
│  └─ 手動筆記 → raw/ 或 ideas/                        │
│                                                     │
│  處理層（Process）← Claude Code + Skills             │
│  ├─ knowledge-organizer（單篇 TBRC 整理）            │
│  ├─ raw-pipeline（10 篇以上批次處理）                 │
│  ├─ knowledge-synthesizer（多篇提煉精粹）            │
│  ├─ knowledge-elevation（精粹昇華 → wiki/concepts/）│
│  ├─ 自動 wikilink 交叉連結                           │
│  └─ wiki/INDEX.md 更新                               │
│                                                     │
│  資產層（Asset）← MAPS 資產化                        │
│  ├─ wiki/：結構化知識條目                            │
│  │   └─ concepts/：跨主題概念框架                   │
│  ├─ skills/：封裝的判斷邏輯                          │
│  ├─ templates/：標準化筆記格式                       │
│  └─ wiki/INDEX.md：可查詢的知識地圖                  │
│                                                     │
│  輸出層（Output）                                    │
│  ├─ outputs/brief-YYYY-MM-DD.md（每日簡報）          │
│  ├─ outputs/health-check-YYYY-MM-DD.md（月度健檢）   │
│  └─ outputs/（報告、分析、決策支持）                  │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

### 檔案格式標準

所有筆記一律使用 `.md` 格式，開頭必須包含 YAML frontmatter：

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

**Tag 規範**（使用階層式 tag，以 `/` 分隔）：

```
ai/claude     ai/agent      ai/mcp        ai/prompt-engineering
dev/flutter   dev/spring-boot             dev/architecture
pkm/workflow  pkm/obsidian  pkm/automation
project/專案名稱
```

**`type/` 標籤（wiki > 50 篇後加入）：**

當同主題條目超過 10 篇，單一 tag 過濾後還是太多，加上 `type/` 做第二維度交叉過濾：

| Tag | 說明 |
|-----|------|
| `type/how-to` | 有具體步驟的操作型條目 |
| `type/concept` | 概念定義與解釋型條目 |
| `type/comparison` | 工具或方法的比較型條目 |

使用方式：`ai/agent`（主題）+ `type/how-to`（類型）→ 從 20 篇縮到 3 篇。

**raw/ 支援的輸入格式**（拖入即可，不需預處理）：

| 格式 | 副檔名 |
|------|--------|
| Markdown 文章 | `.md` |
| PDF 文件 | `.pdf` |
| 截圖／圖表 | `.png` `.jpg` `.jpeg` `.webp` `.gif` |
| 逐字稿／字幕 | `.txt` `.srt` `.vtt` |
| 網頁存檔 | `.html` |
| Jupyter Notebook | `.ipynb` |
| 資料表格 | `.csv` |

---

### 六個核心 Skills（開箱即用）

| Skill                   | 觸發方式                                    | 用途                                               |
| ----------------------- | --------------------------------------- | ------------------------------------------------ |
| `knowledge-organizer`   | `請用 knowledge-organizer 處理 raw/xxx.md`  | 單篇素材 → 結構化 wiki 條目（TBRC 框架）                      |
| `raw-pipeline`          | `請用 raw-pipeline 批次處理 raw/`             | 10 篇以上積壓時用，三 Agent 軟編排                           |
| `knowledge-synthesizer` | `請用 knowledge-synthesizer 整合 [主題]`      | 多篇同主題 wiki → 一篇精粹條目（CAVE 框架）                     |
| `knowledge-elevation`   | `對 [主題] 執行 E 層加深` 或 `執行 meta-synthesis` | 精粹條目 → 三層昇華（自動合成 / E層加深 / 跨主題連結）→ wiki/concepts/ |
| `daily-brief-generator` | `執行 daily-brief-generator`              | 每日掃描 Vault，產出 SCQA 格式建議行動                        |
| `wiki-health-check`     | `執行 wiki-health-check`                  | 每月品質檢查（矛盾偵測、過時內容、孤立筆記）                           |
| `alignment-check`       | `執行 alignment-check`                    | 掃描補充資料段落是否放錯條目，找出歸屬不對齊並建議搬移（raw-pipeline 大批次後執行）|

Skills 檔案位於 `skills/` 資料夾，每個 Skill 有獨立的 `SKILL.md` 定義。

---

### 決策樹：現在要做什麼？

```
你現在要做什麼？
├─ 評估素材值不值得整理  → MAPS asset_value（頻率 × 耗時 × 複雜度 ≥ 50？）
├─ 批次消化積壓（10篇+）→ raw-pipeline
├─ 整理單篇成 wiki 條目  → knowledge-organizer（TBRC 框架）
├─ 多篇同主題提煉精粹    → knowledge-synthesizer（CAVE 框架）
├─ 精粹昇華為概念框架    → knowledge-elevation（E層加深 / 跨主題 meta-synthesis）
├─ 掌握知識庫目前狀態    → daily-brief-generator（SCQA 建議行動）
├─ 月度品質與健康檢查    → wiki-health-check（5A+ 階段評估）
├─ 補充內容歸屬對齊    → alignment-check（raw-pipeline 大批次後 / 健檢前）
└─ 把想法寫成有說服力的輸出 → SCQA 框架（S 現況 / C 衝突 / Q 問題 / A 答案）
```

---

### 查詢策略：依規模調整

Claude Code 直接讀取 wiki/ 檔案效果好，但隨著條目增加需要調整查詢方式。整理端不需要換，只有查詢方式跟著規模升級。

| wiki 條目數 | 建議策略 |
|------------|---------|
| < 30 篇 | 直接讀取，不需要特別處理 |
| 30-80 篇 | 定期跑 `knowledge-synthesizer` 壓縮同主題群，查詢時讀精粹條目 |
| 任何規模 | 查詢前先指定 tag 或子資料夾縮小範圍，再讀目標條目 |
| > 80 篇 | 考慮接 NotebookLM 或 Claude Projects 作為查詢端 |

**路線一：知識庫內部壓縮**

同主題累積 3 篇以上後，執行 `$knowledge-synthesizer`，把 N 篇原始條目提煉成 1 篇精粹。查詢時只讀精粹，不讀原始條目。

```
× 不好：請讀取所有學習動機相關的 wiki 條目
○ 好的：請讀取 wiki/topic-synthesis/learning-motivation.md，回答我的問題
```

觸發信號：每月 `wiki-health-check` 會自動提示哪些主題適合壓縮。

**路線三：搜尋後再讀**

不讀全部檔案，先透過 tag 縮小範圍，再讀目標條目。

```
× 不好：讀取所有 wiki/*.md，找跟「量化研究」有關的
○ 好的：先找 tags 含 research/quantitative 的條目，再讀那幾篇
```

---

### 四大框架在知識庫中的定位

```
┌────────────────────────────────────────────────────┐
│                  你的知識庫工作流                    │
├────────────────────────────────────────────────────┤
│                                                    │
│  MAPS 提供全局思維                                  │
│  → M: 每次存筆記前問「這值得資產化嗎？」             │
│  → A: 知識庫的模組化架構設計                        │
│  → P: 寫出 Claude 能精確執行的 Skill                │
│  → S: 5A+ 循環持續迭代                             │
│                                                    │
│  TBRC 負責單篇知識的結構化                          │
│  → T: 這篇的核心論點是什麼？                        │
│  → B: 適用於什麼場景？不適用於什麼？                │
│  → R: 具體怎麼操作？                               │
│  → C: 關鍵行動是什麼？                             │
│                                                    │
│  SCQA 用於建議行動和決策溝通                        │
│  → S: 知識庫現在什麼狀態？                          │
│  → C: 遇到什麼問題？                               │
│  → Q: 下一步要解決什麼？                           │
│  → A: 具體怎麼調整？                               │
│                                                    │
│  5A+ 驅動每一輪迭代                                 │
│  → AIM → ACQUIRE → ATTEMPT → ADJUST → APPLY → PLUS │
│                                                    │
└────────────────────────────────────────────────────┘
```

---

## 第四部分：分階段實施計劃（從零開始）

### 第一階段：AIM + ACQUIRE（第 1-2 週）

**目標：** 建立基礎 Vault 結構，完成工具安裝與設定

行動清單：
- [ ] 安裝 Obsidian，建立 Vault
- [ ] 建立資料夾結構（raw / wiki / outputs / ideas / research / projects / templates / archive）
- [ ] 安裝 Obsidian Web Clipper（瀏覽器擴充）
- [ ] 安裝 Claude Code（需 Anthropic 帳號）
- [ ] 將 `CLAUDE.md` 放到 Vault 根目錄（告訴 Claude 這個知識庫的規則）
- [ ] 將 `skills/` 資料夾複製到 Vault 根目錄（六個核心 Skills）
- [ ] 蒐集 3-5 篇你最常參考的文章，用 Web Clipper 存入 raw/

---

### 第二階段：ATTEMPT（第 3-4 週）

**目標：** 建立 MVP 版的知識整理流程

行動清單：
- [ ] 用 Claude Code 開啟你的 Vault 資料夾
- [ ] 用 `knowledge-organizer` Skill 處理 raw/ 中的第一篇筆記
- [ ] 觀察產出的 wiki 條目，確認格式和邏輯是否符合你的需求
- [ ] 建立 `wiki/INDEX.md`（可請 Claude 根據現有 wiki 條目自動生成）
- [ ] 調整 `CLAUDE.md` 的「我的關注方向」，填入你自己的 3-5 個主題

**測試指令範例：**

```
請用 knowledge-organizer Skill 處理這篇：raw/我的第一篇筆記.md
```

---

### 第三階段：ADJUST（第 5-6 週）

**目標：** 根據實際使用體驗調整規則，建立穩定的整理習慣

行動清單：
- [ ] 回顧前兩週的處理結果，評估品質（分類一致性、wikilink 精準度）
- [ ] 調整 CLAUDE.md 的 tag 規範，加入你領域特有的分類
- [ ] 建立第一份屬於你的筆記模板（放入 templates/）
- [ ] 嘗試積壓超過 10 篇時使用 `raw-pipeline` 批次處理

---

### 第四階段：APPLY（第 7-8 週）

**目標：** 擴展到更多領域，建立日常工作流

行動清單：
- [ ] 將整理流程應用到不同類型的內容（技術文章、會議紀錄、學習筆記）
- [ ] 開始每日或每週執行 `daily-brief-generator`
- [ ] 當同主題 wiki 累積 3 篇以上，嘗試用 `knowledge-synthesizer` 提煉精粹
- [ ] 執行第一次 `wiki-health-check`，評估知識庫健康度

---

### 第五階段：PLUS（持續）

**目標：** 持續優化，沉澱最佳實踐

行動清單：
- [ ] 每週 15 分鐘回顧：raw/ 有哪些積壓？wiki 新增了什麼？
- [ ] 每月執行一次 `wiki-health-check`
- [ ] 當同主題知識飽和，擴展新主題領域，重新進入 AIM 階段

---

### 快速啟動清單（今天就能開始）

**< 30 分鐘：**
- 安裝 Obsidian，建立一個新 Vault
- 建立 8 個語意資料夾（raw / wiki / outputs / ideas / research / projects / templates / archive）
- 用 Web Clipper 存入你收藏的第一篇文章到 raw/

**本週完成（< 3 小時）：**
- 安裝 Claude Code 並指向你的 Vault
- 把 `CLAUDE.md` 和 `skills/` 放到 Vault 根目錄
- 用 `knowledge-organizer` 處理第一篇 raw/ 素材

**本月目標：**
- wiki/ 累計 10+ 篇經過處理的知識條目
- 執行過一次 `daily-brief-generator`
- 建立 `wiki/INDEX.md` 知識地圖

---

## 附錄：資源連結

**知識庫建設參考：**
- NxCode Obsidian AI 完整指南（2026）
- 經理人：一個指令打造 AI 知識庫
- 數位時代：Karpathy LLM 知識庫方法
- 數位時代：Claude Code 整理 Obsidian 筆記
- WenHao Yu：Claude Code + Obsidian 個人 AI 助理系統

**資產化框架參考：**
- Axton Liu：MAPS 四維羅盤方法論
- Axton Liu：Claude Skills 能力包與軟編排指南
- Axton Liu：Claude Skills 從指令到資產的系統化構建指南
- 四大框架參考文件：SCQA、TBRC、MAPS、5A+
