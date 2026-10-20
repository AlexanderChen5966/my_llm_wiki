---
title: Karpathy AI 筆記流：會自己長大的個人知識庫
date: 2026-06-01
tags: [pkm/workflow, ai/claude]
source: https://www.anduril.tw/karpathy-knowledge-base/
type: note
status: processed
asset_value: 72
---

Karpathy 與 Lex Fridman 的核心思路：把 AI 從單次問答工具轉變為持續累積記憶的知識管理系統，人負責輸入，AI 負責整理與維護，知識庫越用越厚。此方法論的關鍵創新在於「分工」——原始資料（raw/）與 AI 產出的 wiki 分開存放，確保追溯性與自主筆記的獨立性。

## T（核心論點）

大部分人把 AI 當便利商店（每次問完失憶），Karpathy 把 AI 當圖書館員（持續累積）。六步驟流程：

1. **raw/ 資料夾**：不分格式，看到就存，不整理
2. **AI 編譯 wiki**：AI 讀取全部原始資料，自動寫摘要、分類、建立交叉連結
3. **對 wiki 提問**：知識庫達一定規模後（100 篇、40 萬字），直接問 AI，不需額外 RAG
4. **多格式輸出**：Markdown、Marp 簡報、matplotlib 圖表，查詢結果回存成新條目
5. **定期健康檢查**：AI 掃描矛盾、補缺口、發現跨主題關聯
6. **自製搜尋工具**：vibe coding 建簡易搜尋引擎（可選）

Lex Fridman 的延伸：互動式 HTML 頁面（可點選儀表板）、跑步時語音問答「互動式 Podcast」。

## B（適用場景）

**適用：**
- 有固定主題持續深入研究的個人（研究者、知識工作者）
- 使用 Obsidian / Notion 已有收集習慣的人
- 希望知識庫能持續累積、不用每次重新 prompt 的人

**不適用 / 注意：**
- 沒有固定研究主題、只是偶爾問問題的使用者
- 自己的筆記和 AI 產出的 wiki **必須分開存放**（Steph Ango 的提醒），否則搜尋結果與反向連結會被 AI 內容淹沒

## R（操作步驟）

一般人的三步驟簡化版（不寫程式）：

1. **建立原始資料庫**：選 Obsidian 或 Notion，裝 Web Clipper，看到就存，不整理
2. **定期讓 AI 消化**：每週把 raw 資料丟給 Claude，prompt：「整理核心概念清單、寫摘要、標出關聯、列出缺少面向」，結果存回 wiki
3. **對知識庫提問**：開新對話時先貼 wiki 當背景資料，或用 Claude Projects 永久上傳；語音問答（Lex 法）：把 wiki 精華丟進語音模式，通勤時互動學習

進階選項：Google NotebookLM（最接近此概念的免費現成工具，支援 Audio Overview）

## C（關鍵行動）

- [ ] 決定 raw/ 與 wiki 的存放工具與分離策略
- [ ] 安裝 Obsidian Web Clipper 或 Notion Web Clipper
- [ ] 設計固定的 AI 消化 prompt 模板，寫進個人 CLAUDE.md
- [ ] 嘗試用 Claude Projects 上傳 wiki 作為永久背景知識

## 原始摘要

拆解 Andrej Karpathy 與 Lex Fridman 如何用 AI 建立「會自己長大的個人知識庫」，核心概念是讓 AI 從單次問答轉變為持續累積、有記憶的知識管理系統。文章提供一般人不需寫程式即可複製八成的實作流程。

## 相關連結

- [[karpathy-knowledge-base-method]] — Karpathy 三資料夾法（raw/wiki 分工原始版本）
- [[karpathy-no-obsidian-setup]] — 不用 Obsidian 的極簡版
- [[llm-wiki-karpathy-vs-nashsu]] — Karpathy 方法論與 nashsu 桌面 App 的比較
- [[claude-code-obsidian-setup]] — Claude Code + Obsidian 完整建構指南
- [[synthesis-karpathy-llm-wiki-method]] — 四篇 Karpathy/wiki 方法論的合成精粹
