---
title: Gemini 桌面版 Ask/Assign 雙模式與 Obsidian 整合：資料夾層級 scope 的權限模型
date: 2026-09-06
tags: [pkm/obsidian, ai/agent, type/comparison]
source: https://www.kocpc.com.tw/archives/667941（依 TestingCatalog 獨家揭露）
type: article
status: processed
asset_value: 27
---

## T（核心論點）

> **Gemini 桌面版正被推向「超級應用平台」，而它的代理權限模型是資料夾層級的 scope 控制。**

Assign 模式的核心不是能力更強，而是**操作範圍被嚴格限定**：可設一個預設工作資料夾、
回到上次使用的資料夾，或為特定任務選一個新資料夾。這個設計邏輯與 Codex 的 project-scoped
工作模式相同，用途是確保代理不會越權存取不該觸及的檔案。

主要對照軸：**Ask / Assign 對應 Codex 的 Chat / Work**，三巨頭（Gemini 桌面版、Codex、
Claude Desktop）正各自打造不同風格的「AI 作業系統」。

🔑 對本知識庫最直接的意義：**Obsidian 整合意味 Gemini 可直讀本地 Markdown vault，
可能成為 Claude Code 之外的第二條 vault 存取路徑。**

## B（適用場景）

**適用：**
- 評估 Obsidian vault 未來的 AI 存取路徑選項（除 Claude Code 之外）
- 比較三家桌面代理的權限模型與模式設計（scope 控制、雙模式切換）

**不適用 / 注意：**
- 🔴 **時態務必分清**：已上線＝Gemini 桌面版本身（macOS 可下載）、Gemini Spark macOS 版（限美國 AI Ultra 用戶）；
  **封測中＝Ask/Assign 雙模式、Obsidian 整合、Computer Use、跨電腦連線、Finder 捷徑**，公開時程未定
- ⚠️ 作者明確提醒：**尚不確定 Google 是要長期支援 Obsidian，或只是內部先用它測試**，
  待自家等效功能完成後取代——因此不宜以此為長期規劃前提
- ⚠️ 跨電腦連線的機制未明，僅**推測**類似 Codex 的 Remote 遠端控制架構

## R（各項功能與對照）

### 1. Ask / Assign 雙模式（封測中）

| 模式 | 定位 | 對應 Codex |
| --- | --- | --- |
| **Ask** | 標準 Gemini 對話介面 | Chat |
| **Assign** | 類似 Gemini Spark 個人代理但擴展能力更強 | Work |

### 2. Assign 的權限模型：資料夾層級 scope

三種設定方式：預設工作資料夾 / 回到上次資料夾 / 為特定任務選新資料夾。
與 Codex 的 project-scoped 模式相似。

### 3. Obsidian 整合（封測中）

Obsidian 是以 Markdown 為基礎的本地筆記應用，內容存在 vault 資料夾。整合後 Gemini 能直讀個人筆記庫
以提供更精準回應，並可直接對筆記提問、摘要與分析。

### 4. Computer Use 技能（封測中）

可標記 computer use 技能，從桌面版直接觸發操作**本地應用程式**（例如自動開瀏覽器、操作特定網頁
並回傳結果），直接對標 Claude Desktop 的 computer use。

### 5. Finder 整合與 Spark 的互補分工（封測中）

macOS 端可直接把任何資料夾傳給 Gemini 檢查或執行任務。
**分工**：Spark 負責持續性代理任務（雲端運作、連結 Workspace、用 Gemini 3.5 與 Antigravity 框架）；
Finder 整合提供即時檔案操作捷徑。

### 6. Customize 標籤頁與 MCP 展望

可選技能／應用程式／連接器／外掛，但目前功能仍相當有限（主要是 Google 自家應用或現有擴充）。
外界預期後續會加入**完整 MCP 支援與第三方外掛生態系**。

### 7. 未發表的程式碼線索

程式碼追蹤顯示正準備支援下一代主要模型（可能是 Gemini 4），並出現 Nano Banana 2.5 Flash 的升級跡象。

## C（關鍵行動）

- [ ] 待 Obsidian 整合公開測試時，實測它讀取 vault 的方式（是否會寫入、是否遵守 `.obsidian` 設定）
- [ ] 若評估採用，先確認資料夾 scope 能否精確限制在 `my-knowledge-base/` 而不含 `skills/`
- [ ] 觀察 Customize 標籤頁是否真的開放 MCP——這決定它能否接上既有 MCP server
- [ ] 追蹤此整合是長期方案還是內部測試工具（作者指出的關鍵不確定性）

## 原始摘要

依 TestingCatalog 揭露，Gemini 桌面版封閉測試版新增 Ask／Assign 雙模式切換、Obsidian 筆記整合、
Computer Use 技能、跨電腦連線與 macOS Finder 資料夾捷徑，目標直指 OpenAI Codex 與 Anthropic
Claude Desktop。Assign 模式的核心是資料夾層級的操作範圍控制，把代理權限嚴格限定在指定範圍，
設計邏輯對應 Codex 的 project-scoped 模式。文章明確區分已上線（Gemini 桌面版本身、Gemini Spark
macOS 版供美國 AI Ultra 用戶）與仍在封測（上述全部新功能，公開時程未定）。

## 相關連結
- [[gemini-spark-personal-agent]] — Spark 個人代理；🔑 本篇說明 Assign 模式擴展自 Spark，且 Spark 與 Finder 整合為互補分工
- [[codex-computer-use-modes]] — Codex 的 computer use 模式；本篇的主要對照對象
- [[chatgpt-chat-work-codex-comparison]] — 🔑 **主要對照軸**：Ask/Assign 直接對應 Codex 的 Chat/Work
- [[claude-code-obsidian-setup]] — 目前 vault 的 AI 存取路徑；本篇是潛在的第二條路徑
- [[obsidian-knowledge-base-planning]] — vault 架構規劃；影響資料夾 scope 該怎麼切
- [[agent-permission-and-os-sandbox]] — 資料夾層級 scope 屬於其中的權限邊界設計
- [[agent-os-workstation-division]] — 「AI 作業系統」競賽的整體分工視角
