---
title: iOfficeAI/OfficeCLI：AI agent 專用的精細讀寫 Office 套件（單一 binary）
date: 2026-07-17
tags: [ai/agent, ai/mcp]
source: https://github.com/iOfficeAI/OfficeCLI
type: reference
status: processed
asset_value: 72
---

> ⚠️ **與同名的另一個 OfficeCLI 專案是不同團隊、不同定位，勿混淆。** 本條目是 **iOfficeAI/OfficeCLI**（官網 officecli.ai，單一 .NET binary），定位是「精細**讀寫既有** Office 檔的 agent 工具」——給 AI「一雙眼睛」看見並逐元素操作文件。另一個 [[officecli-io-doc-generator]]（officecli.io，npm 安裝）則是「用 prompt 從零**生成整份新** Office 檔」。兩者無關聯。

合併自 GitHub README（英文，主）+ 知乎「勇哥AI笔记」中文介紹（補充來源）。iOfficeAI 團隊開源，Apache 2.0。號稱「世界第一個為 AI agent 設計的 Office 套件」，一行命令讓任何 agent 完全掌控 Word/Excel/PowerPoint 的讀取、創建、編輯與自動化，無需安裝 Office、無依賴、跨平台。

## T（核心論點）
OfficeCLI 的關鍵創新是**內建 HTML 渲染引擎**，讓 agent 不再盲目寫 OOXML XML，而是真正「看見」文件長什麼樣，形成 **render → look → fix** 閉環（渲染→查看→修復），即使在 CI/Docker/無顯示器伺服器也能運作。搭配路徑定址（`/slide[1]/shape[2]`）、確定性 JSON 輸出、結構化錯誤碼自我修復，把 Office 自動化變成 agent 友善的可靠工具。

## B（適用場景）
- **適用**：AI agent 需精細讀寫既有 Office 檔（改字型/顏色/公式/圖表/表格）、從文件抽結構化資料到 JSON、CI/CD 文件管線、無頭 Office 自動化、批量報表
- **不適用**：從零用一句話生成整份簡報（那用 [[officecli-io-doc-generator]]）；極複雜舊版 `.doc` 或重度 VBA 巨集場景

## R（架構與核心能力）
**三層漸進式架構**（start simple，需要才深入）：
| 層 | 用途 | 命令 |
| --- | --- | --- |
| L1 讀取 | 語義化內容視圖 | `view`（text/annotated/outline/stats/issues/html/svg/screenshot）|
| L2 DOM | 結構化元素操作 | `get` `query` `set` `add` `remove` `move` `swap` |
| L3 Raw XML | XPath 直接存取，萬能降級 | `raw` `raw-set` `add-part` `validate` |

**內建引擎與生成基元：**
- **渲染引擎**：三種模式 `view html`（資源內聯獨立檔）/ `view screenshot`（每頁 PNG 給多模態 agent 讀）/ `watch`（本地 HTTP `localhost:26315` 即時刷新）
- **公式與透視引擎**：350+ Excel 函數寫入時自動計算、原生 OOXML 透視表一鍵生成（多欄位/日期分組/calculated fields）
- **模板合併** `merge`：替換 `{{key}}` 佔位符，agent 設計一次版面（貴），生產碼填 N 次（便宜、確定、零 token）
- **往返轉儲** `dump`/`batch`：序列化任何 docx/pptx/xlsx（或子樹）為可重放 batch JSON，讓 agent 從人寫範本學結構

**Agent 友善設計：**
- 確定性 `--json` 輸出、路徑定址免懂 XML 命名空間、`validate`+`view issues`+結構化錯誤碼（`not_found`/`invalid_value`）自我修復
- 駐留模式 `open`/`close`（記憶體常駐近零延遲）、批量模式 `batch`
- **內建 MCP Server**：`officecli mcp claude/cursor/vscode/lmstudio` 一鍵註冊
- 安裝後自動偵測已知 AI 工具目錄並寫入 SKILL.md，agent 讀取即掌握全部命令

**入手：** `curl -fsSL https://officecli.ai/SKILL.md`（給 agent 一行）；人類可用 GUI 桌面版 AionUi 或 `officecli install`。

## C（關鍵行動）
- [ ] agent 有讀寫既有 Office 檔需求時，優先評估 MCP 註冊方式（`officecli mcp claude`）
- [ ] 讀寫閉環善用 `view screenshot` 讓多模態模型檢查版面溢出/重疊
- [ ] 批量報表用 `merge` 模板合併，避免 agent 逐份重生成產出不一致版面
- [ ] ⚠️ 別跟 [[officecli-io-doc-generator]]（officecli.io 生成器）搞混

## 相關連結
- [[officecli-io-doc-generator]] — ⚠️ 同名不同專案：officecli.io，npm 裝、prompt 從零生成整份文件
- [[markitdown-document-conversion]] — 另一種 Office→Markdown 前處理路線（餵 LLM，非逐元素讀寫）
- [[claude-code-official-skills-top10-2026]] — Office 相關 agent skill 生態
- [[api-cli-skills-agent-architecture]] — CLI+JSON 讓產品 agent-accessible 的設計哲學（OfficeCLI 是典型範例）
