---
title: AI-First Vault 設計：扁平化優於深層資料夾
date: 2026-05-28
tags: [pkm/obsidian, pkm/workflow]
source: https://muki.tw/from-folders-to-flat-vault/
type: article
status: processed
asset_value: 36
---

## 核心論點

傳統「把檔案放哪個資料夾」的視覺分類是為人腦設計的，但每天最常翻 vault 的是 AI——AI 需要的是 frontmatter、tag、index 這類能一次掃完的結構。用人腦思維設計的 vault 會讓 AI Agent 每次都要 grep 整個資料夾，耗時耗力又耗 token。

## AI-First Vault 的四個設計原則

1. **扁平結構**：所有筆記放同一層級，不用深層資料夾
2. **frontmatter 為主**：用 YAML frontmatter（title、tags、date、type、status）讓 AI 批次掃描
3. **Tag 取代資料夾**：`ai/claude`、`pkm/workflow` 等階層 tag 取代目錄樹
4. **INDEX 集中索引**：維護一個 INDEX.md，AI 查詢時先讀索引，不需遍歷所有檔案

## 對比傳統 PARA 方法

| 維度 | PARA（資料夾）| AI-First（扁平）|
|------|-------------|----------------|
| 瀏覽者 | 人眼 | AI Agent |
| 分類機制 | 視覺資料夾 | frontmatter + tags |
| 查詢方式 | 點擊導航 | grep/index 掃描 |
| 維護成本 | 需人工歸類 | AI 維護 frontmatter |

## 相關連結

- [[karpathy-knowledge-base-method]] — raw/ + wiki/ 兩層架構（也是扁平概念）
- [[lyt-framework-obsidian-ai-guide]] — LYT 框架（MOC 也是 index 概念）
- [[obsidian-knowledge-base-planning]] — 本 Vault 的規劃（已採用扁平+INDEX 設計）
