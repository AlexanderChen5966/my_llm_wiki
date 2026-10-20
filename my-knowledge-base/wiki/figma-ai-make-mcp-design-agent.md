---
title: Figma AI 全解：Design Agent、Figma Make 與 Figma MCP 三者關係
date: 2026-07-20
tags: [ai/mcp, ai/workflow]
source: https://frankknow.com/figma-ai/
type: article
status: processed
asset_value: 52
---

Figma AI 是 Figma 推出的一系列 AI 功能統稱，重點不是「在 Figma 裡加幾個按鈕」，而是**串連了「設計 → 原型 → 程式碼」的整條流程**。三個進階工具各解決不同層次的問題：Figma Make（生成原型）、Figma MCP（接入 AI 開發工具）、Design Agent（畫布上直接操作）。

## T（核心論點）

Figma AI 分兩層：**內建畫布 AI 輔助**（開檔就能用，含 Design Agent、圖片編輯、自動命名、視覺搜尋、文案翻譯）與**進階 AI 工具**（需獨立入口或設定，即 Figma Make 與 Figma MCP）。三個進階工具互補而非互斥：用 Make 快速驗證想法 → 在 Figma 精修 → 接 MCP 讓工程師的 AI 工具讀設計稿開發 → 疊代時用 Design Agent 在畫布上快速調整。

## B（適用場景）

**適用（依角色）：**
- **設計師**：從 Design Agent + 圖片編輯熟悉 AI 輔助；快速驗證用 Make，既有稿修改用 Design Agent
- **前端/全端工程師**：優先接好 Figma MCP，讓 AI Coding 工具直接讀設計稿，減少截圖比對誤差
- **PM/非技術人員**：Figma Make 是最低門檻入口，能描述就能做出可點擊的產品雛形

**不適用 / 注意：**
- Design Agent 目前為 Limited Beta（分批開放付費方案）
- Figma MCP 需付費方案（依賴 Dev Mode 資料串接）；「寫入畫布」需 Full/Dev seat（Starter 每月僅 6 次）
- Figma Make 定位是「原型工具」非正式網站終點；正式上線用 Figma Sites 或 WordPress

## R（三工具詳解與串接）

### 三工具定位對照

| 工具 | 核心用途 | 最適合情境 |
|------|---------|-----------|
| **Figma Make** | Prompt 生成可運行的互動原型（HTML/CSS/React） | 從零快速驗證想法、產出可展示成品 |
| **Figma MCP** | 把 Figma 設計接入 AI 開發工具 | 工程師讓 AI coding 工具讀設計稿、精準生成程式碼 |
| **Design Agent** | AI 直接在 Figma 畫布上操作 | 既有設計中用對話指令快速疊代修改 |

### Design Agent（2026-05 推出）
在畫布裡的 AI Agent，能生成設計初稿（原 First Draft 已整合進來）、用指令修改/排列元素、以現有設計疊代、自動參照設計系統套用正確 Component 與 Token。兩個進入方式：畫布即時 Prompt（點圖層直接輸入）、左側 Agents 圖示（持續對話）。多個 Prompt 可並行。

### Figma Make
自然語言生成互動原型與可運行程式碼（中英文），官方定位「Prompt to code anything」。適合 Onboarding Flow、Data Dashboard、互動介面、工具型 App。Prompt 越具體結果越貼近想像。（與獨立產品 Figma Sites 區分：Make 輸出可運行原型，Sites 負責正式網頁發布。）

### Figma MCP
MCP（Model Context Protocol，Anthropic 提出）是讓 AI 工具與外部資料源對話的標準協議。Figma MCP Server 讓 Cursor/VS Code/Claude Code 等直接讀 Figma 設計稿。能做：從設計稿生成程式碼、讀取 Design Token/元件/Code Connect、讓 AI 反向寫入畫布（需 Remote Server）、把網頁擷取成設計圖層。支援 Claude Code、Claude、Cursor、VS Code（透過 Copilot）、GitHub Copilot CLI、Codex、ChatGPT 等。

### Claude Code 串接 Figma MCP 兩種方式

**方式一：安裝 Figma Plugin（官方推薦，最快）** — 附常見工作流的 Agent Skills：
```
claude plugin install figma@claude-plugins-official
```

**方式二：手動設定**
```
claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp
```
重啟後 `/mcp` → 選 figma → Authenticate → Allow Access，顯示 `Authentication successful` 即可。`--scope user` 讓所有專案共用。設定完成後把 Figma Frame 連結貼給 Claude Code，即可下指令「根據設計稿生成程式碼」或「在畫布新增/修改元素」。

Server 類型：**Remote Server**（官方主推，免本機安裝）vs **Desktop Server**（僅特定企業情境，官方強烈建議用 Remote）。

📌 **深入版**：規則檔全文（CLAUDE.md/DESIGN.md）、元件命名策略、seat/額度門檻與 self-healing loop 五步驟見 [[claude-code-figma-mcp-write-setup]]。

## C（關鍵行動）

- [ ] 工程師優先用方式一（Plugin）串好 Figma MCP，把 Frame 連結貼給 Claude Code 試生成
- [ ] 分清三工具：Make 驗證想法、MCP 開發、Design Agent 疊代
- [ ] 確認方案：MCP 需付費方案 + Dev Mode；寫入畫布需 Full/Dev seat

## 原始摘要

犬哥網站（Audrey）文章。Figma AI 串連設計→原型→程式碼整條流程，內建功能（Design Agent、Remove Background/Erase Object/Expand/Vectorize 圖片編輯、圖層自動命名、AI 文案翻譯、視覺搜尋三方式）開檔即用；進階工具 Figma Make 與 Figma MCP 需另設。文中夾帶 WordPress 架站業配，已略。

## 相關連結

- [[open-design-claude-design-comparison]] — Open Design vs Claude Design 設計工具比較（同屬 AI 設計工具生態）
- [[spec-first-ai-coding-workflow]] — Spec-first AI 編碼流程（有提及 MCP Figma，本篇為 Figma 側完整說明）
- [[claude-code-plugins-guide]] — Claude Code Plugin 系統（Figma Plugin 即透過此安裝）
- [[claude-design-tool]] — Anthropic Claude Design（另一 AI 視覺設計工作台對照）
