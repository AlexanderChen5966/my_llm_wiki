---
title: hung-yi-lee-skill：Karpathy 風格 LLM 知識圖譜 Skill
date: 2026-06-17
tags: [ai/agent, pkm/workflow]
source: https://github.com/voidful/hung-yi-lee-skill
type: reference
status: processed
asset_value: 58
---

hung-yi-lee-skill 是 voidful 以李宏毅教授 478 支 YouTube 影片為素材建立的 Karpathy 風格 LLM Wiki + Skill，包含 916 個知識圖譜節點、3,664 條邊，並萃取出「Teaching DNA」——讓 AI 回答 AI 問題時模擬李宏毅老師的教學風格。

## T（核心論點）

**把特定教師的知識體系「軟體化」**：不只是把影片轉成 RAG 素材，而是萃取教師的推理節奏（直覺先行→Black Box→開箱→陷阱→回顧）和語言標記，讓知識圖譜同時承載「知識」與「教學方式」。這比靜態 RAG 多一個維度：**EXTRACTED vs INFERRED 邊**區分明確事實與推導關係。

## B（適用場景）

- 需要以李宏毅風格（直覺優先、benchmark 懷疑論）解釋 AI 概念
- 構建領域知識圖譜的範本研究
- PKM 研究：如何把影片課程轉換成可查詢的持久知識庫

## R（完整內容）

### 素材來源

- 478 支 YouTube 影片 / 27 份逐字稿 / 8 個主題頁 / 4 份參考文件
- 建置時間：最新同步版本（sync 指令可更新）

### 知識圖譜規模

| 指標 | 數值 |
|------|------|
| 節點數 | 916 |
| 邊數 | 3,664 |
| 概念社群數 | 10 |

**God Nodes（最高連結度）：**
- ML Fundamentals：385 個連結
- 語言模型：251 個連結
- Transformer：83 個連結

### Teaching DNA（教學 DNA）

**教學節奏（五步驟）：**
1. 直覺先行（Intuition First）
2. Black Box（先講外部行為）
3. 開箱機制（Unbox the Mechanism）
4. 陷阱提醒（Trap Warning）
5. 簡短回顧（Quick Review）

**四大教學原則：**
1. **Benchmark 懷疑論**：不直接引用數字，質疑測量條件
2. **知識誠實**：明確說「我不確定」而非臆測
3. **具體類比**：用日常物件比喻抽象概念
4. **先講直覺再講數學**：確保直覺建立後才引入公式

**語言標記（Teaching Markers）：**
- 「你可能會想說…」（引入常見誤解）
- 「先講 black box」（宣告暫時不解釋內部）
- 「其實很簡單」（降低認知負擔的鋪墊）
- 「這邊有個陷阱」（提醒非直覺的邊界條件）

### CLI 功能

```bash
# 同步最新內容
skill sync-metadata
skill sync-transcripts

# 建立知識圖譜
skill compile
skill graph build

# 查詢
skill search "attention mechanism"
skill graph query "Transformer"

# 分析
skill graph report   # 節點中心性報告
skill lint           # 知識一致性檢查
```

### 為何優於靜態 RAG

| 特性 | 靜態 RAG | hung-yi-lee-skill |
|------|---------|-------------------|
| 知識結構 | Flat chunks | 圖結構（節點+邊） |
| 關係類型 | 無 | EXTRACTED vs INFERRED |
| 持久化 | Session-only | 永久 wiki |
| 教學風格 | 無 | Teaching DNA 萃取 |
| 更新 | 重建全部 | sync 增量更新 |

## C（關鍵行動）

- [ ] 研究其「Teaching DNA 萃取」方法：如何從逐字稿中萃取推理節奏
- [ ] 參考 EXTRACTED vs INFERRED 邊設計，應用於自己的知識庫 wikilink 類型
- [ ] 評估對知識庫建立 God Nodes（高連結度核心節點）的方法

## 相關連結

- [[synthesis-karpathy-llm-wiki-method]] — Karpathy LLM Wiki 方法論精粹
- [[karpathy-knowledge-base-method]] — 三資料夾法基礎
- [[tpki-tri-pattern-knowledge-index]] — 類型化 wikilink 與 intent→node 映射
- [[karpathy-llm-wiki-improvements]] — LLM Wiki 三大缺陷修正（類型化 wikilink 方向一致）
