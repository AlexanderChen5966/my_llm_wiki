---
title: officecli/officecli：用 prompt 從零生成整份 Office 文件的 CLI（officecli.io）
date: 2026-07-17
tags: [ai/agent, ai/workflow]
source: https://github.com/officecli/officecli
type: reference
status: processed
asset_value: 62
---

> ⚠️ **與同名的另一個 OfficeCLI 專案是不同團隊、不同定位，勿混淆。** 本條目是 **officecli/officecli**（官網 officecli.io，`npm install -g officecli`），定位是「用自然語言 prompt 從零**生成整份新** Office 檔（PPTX/DOCX/XLSX/報告/圖片）」，有 hosted 試用。另一個 [[iofficeai-officecli]]（officecli.ai，單一 .NET binary）則是「精細**讀寫既有** Office 檔並逐元素操作」的 agent 工具。兩者無關聯。

整理自 GitHub README（officecli/officecli）。命令列工具，把自然語言 prompt 轉成可編輯的 Office 檔與獨立圖片，可從終端/腳本/CI/agent 工作流呼叫。首次執行提供 hosted 匿名試用（不需本地模型端點或 API key），額度用完可買 hosted key 或切 External Mode 自帶 LLM 端點。

## T（核心論點）
officecli.io 走的是「一句 prompt → 一份成品」的生成路線：`officecli new pptx/docx/xlsx/report/img` 直接產出可編輯 Office 檔，內建 hosted 試用降低冷啟門檻。與逐元素讀寫路線（iOfficeAI）互補——它解決的是「快速從零生一份文件」而非「精修既有文件」。

## B（適用場景）
- **適用**：從 prompt 快速生成簡報/文件/工作表/報告/圖片、CI/腳本自動化生成、想先試用不想配 API key、用既有 xlsx 當證據生成 workbook-backed report
- **不適用**：需精細讀取/逐元素修改既有 Office 檔（那用 [[iofficeai-officecli]]）；需完全離線且不經 hosted 服務的場景（須自行配 External Mode）

## R（操作與能力）
**安裝**（npm 為推薦路徑）：`npm install -g officecli`（或 install script / `brew tap officecli/officecli`）

**生成命令：**
```
officecli new pptx "Q3 Business Review" --prompt "六頁 SaaS 季度回顧簡報，涵蓋成長/留存/風險/下季行動"
officecli new docx "Product Launch Brief" --prompt "..."
officecli new xlsx "Sales Pipeline" --prompt "..."
officecli new report "Q2 Review" --file ./data/q2.xlsx --prompt "..."   # workbook-backed
officecli new img "Launch Visual" --prompt "..." --ratio landscape --reference-image ./ref.png
```
- 產出預設寫到 `./output`，`--out` 換目錄、`--json` 機器可讀、`--no-publish` 純本地
- PPTX 預設自動生成並嵌入配圖，`--no-images` 純文字
- `officecli score pptx <file>`：結構化 PPTX 評分

**存取模式：** hosted trial → hosted key（`auth set-key`）→ External Mode（`config set-runtime external` 自帶模型端點）

**選配 agent 整合**（主介面仍是直接 CLI）：
- Claude Code：`/plugin marketplace add officecli/officecli` → `/plugin install officecli@officecli`
- Codex/其他本地 agent：`install-skill.sh`（可 `AUTO_INSTALL_BINARY=0` 只裝 skill）
- OpenClaw：`install-openclaw-skill.sh`，讓 Telegram/Discord/Slack 頻道直接生成本地 Office 檔

## C（關鍵行動）
- [ ] 想快速從 prompt 產一份可編輯簡報/文件時，`officecli new` 一行搞定，先用 hosted trial 試水溫
- [ ] 生產環境用 External Mode 接自家 LLM 端點避免 hosted 依賴
- [ ] ⚠️ 別跟 [[iofficeai-officecli]]（officecli.ai 精細讀寫工具）搞混

## 相關連結
- [[iofficeai-officecli]] — ⚠️ 同名不同專案：officecli.ai，單一 binary、精細讀寫既有 Office 元素
- [[design-md-google-format]] — 另一種讓 AI 產出一致視覺的路線（設計 token）
- [[ai-website-cloner-template]] — 同屬「prompt → 成品」的生成式工具範式
