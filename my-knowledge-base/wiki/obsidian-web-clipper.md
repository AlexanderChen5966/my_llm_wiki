---
title: Obsidian Web Clipper 使用指南
date: 2026-05-13
tags: [pkm/obsidian, pkm/automation]
source: https://muki.tw/obsidian-web-clipper-ai-tool/
type: article
status: processed
asset_value: 60
---

## T（核心論點）
Obsidian 官方免費瀏覽器擴充，一鍵將網頁存成本地 Markdown，並可在擷取時透過 AI Prompt 自動生成摘要與翻譯——是知識庫輸入端的核心工具。

## B（適用場景）
**適用：** 需要頻繁從網頁收集素材的場景；配合 Claude Code 自動整理流程
**不適用：** PDF、影片字幕等非網頁素材（需其他工具）；需要雲端同步的情境（本工具存本地）

## R（操作步驟）
1. 安裝 Chrome 擴充（搜尋 "Obsidian Web Clipper"）
2. 右鍵 → 選項 → 語言設為「繁體中文」
3. Default 範本 → 筆記位置設為 `my-knowledge-base/raw/`
4. 設定 AI 解譯器（Gemini / Anthropic API Key）
5. 自訂 Prompt：`{{"summary the content and return in Traditional Chinese"}}`
6. 看到值得保存的網頁 → 點擴充圖示 → 預覽 → 存檔

## C（關鍵行動）
- [ ] 存入路徑必須設為 `raw/`，不要直接存到 wiki/
- [ ] AI Prompt 設定繁體中文摘要，減少後續整理工作
- [ ] 存完後定期用 knowledge-organizer Skill 批次處理 raw/

## 原始摘要

Obsidian Web Clipper 的核心優勢：存成本地 Markdown（Claude Code 可直接讀取）、支援完整網頁格式（圖片、表格、引用）、可根據網址自動套用不同範本（YouTube、Arxiv、GitHub 等）。作者原本用 Readwise，因客製化不足而改用此工具。

社群範本資源：Kepano clipper-templates（支援 Arxiv、IMDB、Google Maps 等）。

## 相關連結
- [[karpathy-knowledge-base-method]] — 此工具是 Karpathy 方法的輸入端
