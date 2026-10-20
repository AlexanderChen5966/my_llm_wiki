---
title: Obsidian 知識庫規劃：從整理到資產化的路線圖
date: 2026-05-13
tags: [pkm/obsidian, pkm/workflow, ai/claude]
source: Obsidian知識庫規劃_知識整理與資產化.md
type: reference
status: processed
asset_value: 70
---

本文整合五篇知識庫建設文章的共通方法，並以 MAPS 框架提出「知識資產化」路線：從 Markdown 基礎設施出發，讓 AI 負責整理、人負責思考，分五階段（5A+ 循環）把知識庫從空資料夾長成可復用的資產系統。完整文件位於 Vault 根目錄 `Obsidian知識庫規劃_知識整理與資產化.md`。

## T（核心論點）
知識庫不只是「存得到、查得到」，而是要讓每一篇筆記成為 AI 可執行的上下文素材——用 MAPS 框架把整理邏輯封裝成 Skill，讓知識庫的價值以複利方式增長。

## B（適用場景）
**適用：** 剛建立或正在優化 Obsidian 知識庫的用戶；想把散落筆記轉化為可復用資產的場景
**不適用：** 已有成熟系統且不需要重構的用戶

## R（六項共通方法）
1. **Markdown 為基礎設施**：純文字、跨平台、AI 可直接讀寫
2. **語意分層結構**：raw / wiki / outputs / ideas / research / projects / archive，收集與整理分離
3. **AI 負責整理**：人只負責輸入，Claude Code 負責摘要、分類、建立 wikilink
4. **Claude Code 直接操作本地檔案**：AI 直接進入 Vault 資料夾讀寫，不需要 MCP（MCP 為選配）
5. **Context Engineering**：筆記結構越一致，AI 的情境理解越準確
6. **漸進式建設**：先跑再說，不追求完美

## R（查詢策略）

| wiki 條目數 | 建議策略 |
|------------|---------|
| < 30 篇 | 直接讀取 |
| 30-80 篇 | `$knowledge-synthesizer` 壓縮同主題群，查詢時讀精粹條目 |
| 任何規模 | 查詢前先指定 tag 縮小範圍，再讀目標條目 |
| > 80 篇 | 考慮接 NotebookLM 或 Claude Projects |

觸發壓縮的信號：每月 `wiki-health-check` 會提示哪些主題累積 3 篇以上、適合執行 `knowledge-synthesizer`。

## C（5A+ 分階段路線）
- [ ] 第 1-2 週（AIM + ACQUIRE）：建 Vault、裝 Web Clipper、建模板
- [ ] 第 3-4 週（ATTEMPT）：手動測試整理 Prompt，處理 10 篇筆記
- [ ] 第 5-6 週（ADJUST）：優化並封裝第一個 `knowledge-organizer` Skill
- [ ] 第 7-8 週（APPLY）：擴展到更多領域，建立每日 Brief 工作流
- [ ] 持續（PLUS）：每週 15 分鐘、每月 1 小時定期回顧

## 相關連結（含查詢策略）
- [[karpathy-knowledge-base-method]] — 三資料夾法（raw/wiki/outputs），本知識庫採用此架構
- [[maps-framework]] — MAPS 四維羅盤：Mindset × Architecture × Prompt × Systems
- [[claude-skills-guide]] — Skills 設計原理，知識資產化的核心工具
- [[claude-code-obsidian-setup]] — Claude Code + Obsidian 整合實作步驟
- [[wenhao-yu-ai-workflow]] — 每日 Brief 自動化工作流範例
- [[query-strategy]] — 依規模選擇查詢方式的完整策略（直接讀／壓縮／先搜尋再讀）
