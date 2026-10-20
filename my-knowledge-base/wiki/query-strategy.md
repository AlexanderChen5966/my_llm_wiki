---
title: 知識庫查詢策略：依規模選擇查詢方式
date: 2026-05-15
tags: [pkm/workflow, pkm/obsidian, ai/claude]
source: 對話整理
type: reference
status: processed
---

Claude Code 直接讀取 wiki/ 檔案的方式適合小型知識庫，但隨著條目增加需要調整策略。整理端（Obsidian + Claude Code）不需要換，只有查詢方式隨規模升級。

## T（核心論點）

查詢策略跟知識庫規模掛鉤：條目少時直接讀，條目多時先壓縮或先搜尋，不需要換工具。

## B（適用場景）

**適用：** 使用 Claude Code 作為查詢端、wiki/ 條目持續增長的知識庫
**不適用：** 已接 NotebookLM / Claude Projects 等 RAG 工具的場景（那些工具有自己的索引，不需要此策略）

## R（查詢策略）

### 現階段（wiki < 30 篇）：直接讀取

Claude Code 直接讀取相關 wiki/ 條目，效果好且不需要額外處理。

```
請讀取 wiki/ 中與「學習動機」相關的條目，整理研究現況。
```

---

### 路線一：知識庫內部壓縮（wiki 30-80 篇）

同主題累積 3 篇以上後，用 `knowledge-synthesizer` 提煉成一篇精粹條目。
查詢時讀 1 篇精粹，而不是讀 N 篇原始條目。

**觸發時機：**
- 同主題有 3 篇以上 wiki 條目
- wiki-health-check 報告出現「適合執行 knowledge-synthesizer」的建議

**執行方式：**
```
$knowledge-synthesizer 學習動機

產出：wiki/topic-synthesis/learning-motivation.md
     （整合 N 篇原始條目的精粹，CAVE 框架）
```

**查詢時的做法：**
```
× 不好：請讀取所有學習動機相關的 wiki 條目
○ 好的：請讀取 wiki/topic-synthesis/learning-motivation.md，回答我的問題
```

---

### 路線三：搜尋後再讀（wiki 任何規模均適用）

不讀全部檔案，先透過 tag 縮小範圍，再讀目標條目。

**單一 tag 過濾：**
```
× 不好：讀取所有 wiki/*.md，找跟「量化研究」有關的
○ 好的：先找 tags 含 research/quantitative 的條目，列出清單後再讀那幾篇
```

**效果更好——兩個 tag 交叉過濾：**

單一 tag 可能還有 10-20 篇，再加一個 tag 可縮到 3-5 篇：
```
tags 含 ai/agent → 20 篇
tags 含 ai/agent 且含 type/how-to → 3 篇
```

實際指令：
```
請先掃描 wiki/ 的 frontmatter，列出同時含「ai/agent」和「type/how-to」的條目，
再讀取清單中的條目回答我的問題。
```

**`type/` 標籤的時機：**

主題 tag（`ai/agent`）只說明「關於什麼」，當同主題條目超過 10 篇，加上 `type/` 做第二維度過濾效果明顯：

| type tag | 過濾意圖 |
|----------|---------|
| `type/how-to` | 只找有步驟的操作型條目 |
| `type/concept` | 只找概念定義型條目 |
| `type/comparison` | 只找工具或方法比較型條目 |

> **建議等 wiki > 50 篇後才開始加 `type/` tag**，條目少時直接讀反而更快。

---

### 規模對照表

| wiki 條目數 | 建議策略 |
|------------|---------|
| < 30 篇 | 直接讀取（現階段） |
| 30-80 篇 | 路線一：定期跑 knowledge-synthesizer 壓縮同主題群 |
| 任何規模 | 路線三：查詢時先 grep tag，再讀目標條目 |
| > 80 篇 | 考慮接 NotebookLM 或 Claude Projects 作為查詢端 |

## C（關鍵行動）

- [ ] 同主題累積 3 篇後執行 `$knowledge-synthesizer`，建立精粹條目
- [ ] 查詢時養成「先指定 tag 或子資料夾，再讀內容」的習慣
- [ ] 每月 `wiki-health-check` 會提示哪些主題適合壓縮

## 相關連結

- [[karpathy-knowledge-base-method]] — 知識庫查詢的原始概念（查詢結果回存 outputs/ 觸發複利）
- [[obsidian-knowledge-base-planning]] — 整體知識庫架構規劃
