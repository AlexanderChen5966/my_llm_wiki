---
title: 不用 Obsidian 的極簡 AI 知識庫（Karpathy 原版）
date: 2026-05-13
tags: [pkm/workflow, ai/claude]
source: https://www.bnext.com.tw/article/90650/andrej-karpathy-ai-how
type: article
status: processed
asset_value: 65
---

## T（核心論點）
不需要任何筆記 App，三個資料夾加一份 CLAUDE.md 就能跑通 Karpathy 的 AI 知識庫——整理的複雜度不在工具，在於把「整理的決定」交給 AI。

## B（適用場景）
**適用：** 想最快跑通流程、不想先學筆記軟體；已有能讀本地檔案的 AI 工具（Claude Code、Cursor 等）
**不適用：** 需要視覺化圖譜、全文搜尋等進階功能時（建議搭配 Obsidian）

## R（操作步驟）
1. 建立資料夾結構：`my-knowledge-base/raw/`、`wiki/`、`outputs/`
2. 建立 `CLAUDE.md`，定義三個資料夾用途與 wiki 維護規則
3. 把素材丟進 `raw/`（不分類、不整理）
4. 下指令：「讀取 raw/ 所有內容，按 CLAUDE.md 在 wiki/ 編譯知識庫」
5. AI 自動建 INDEX.md、為每主題建 .md 檔、串聯交叉連結
6. 直接對 wiki/ 提問；定期執行健康檢查

## C（關鍵行動）
- [ ] CLAUDE.md 的品質決定輸出品質（Schema quality = output quality）
- [ ] 定期健康檢查防止「錯誤複利」（AI 把錯誤的資訊一直整合進去）
- [ ] 不需要 Obsidian，但兩條路線核心邏輯相同

## 原始摘要
數位時代文章，Nick Spisak 拆解 Karpathy 的極簡版做法：不依賴任何筆記 App，只用資料夾 + CLAUDE.md。強調「為什麼不整理反而有效」——AI 秒級完成交叉比對，知識庫隨素材自動演化。另有搭配 Obsidian 的進階版本。

## 相關連結
- [[karpathy-knowledge-base-method]] — Karpathy 方法完整版（含 Lex Fridman 延伸）
- [[claude-code-obsidian-setup]] — 搭配 Obsidian 的進階版
