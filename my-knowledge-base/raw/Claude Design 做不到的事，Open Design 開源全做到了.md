---
title: "Claude Design 做不到的事，Open Design 開源全做到了"
source: "https://yujustcoding.com/step-by-step/open-design-claude-alternative.html"
author:
  - "[[Yui]]"
published: 2026-05-05
created: 2026-06-08
description: "Open Design 是免費開源的本地設計生成工具，支援 Claude Code、Cursor、Gemini CLI，不需訂閱 Claude Max。這篇帶你從安裝到實際操作，完整了解如何在自己的機器上跑出設計原型。"
tags:
  - "clippings"
status: processed
---
Claude Design 是 Anthropic 推出的設計生成工具，能從文字描述生成 UI 原型和網頁佈局。但使用條件明確：除了下載 [Claude Desktop](https://yujustcoding.zeabur.app/tool-shared/claude-guide.html) 還不夠，還需要訂閱 Claude Max，且所有運算在 Anthropic 的雲端執行。

Open Design 只推出不到幾天，就累積了 github 上累積兩萬多個stars，因為他完全解決了同一件事，出發點完全不同。這個開源專案由 nexu-io 維護，讓你在本地生成設計原型，不需要訂閱任何付費方案，也不綁定特定的 AI 工具。Cursor Agent、Gemini CLI、Codex 等九種 AI CLI 都可以直接使用。

## Claude Design 有什麼做不到？

Claude Design 有三個具體的限制，都源自它的商業模式。

**工具鎖定** ：Claude Design 只能在 Claude 的對話介面裡使用。如果你的開發流程是 Cursor 或 Gemini CLI，Claude Design 沒辦法接進去。

**雲端依賴** ：所有設計任務都要送到 Anthropic 的伺服器處理。處理有保密要求的客戶專案或公司內部設計時，這是個實際的顧慮。

**訂閱費用** ：Claude Max 訂閱費不低。如果設計生成只是偶爾用到的功能，成本效益很難平衡。

Open Design 把這三個問題一起移除了。

## Open Design 是什麼？

Open Design 是完全開源的本地設計生成工具，專案在 [GitHub](https://github.com/nexu-io/open-design) 上由 nexu-io 維護。

啟動後，系統載入 31 個 Skills 和 71 套 Design Systems，可生成的輸出格式包括網頁、桌面、行動裝置 HTML 原型、簡報（PPTX）、圖片與影片（MP4），以及 HyperFrames 互動式設計框架。

所有產出在本地沙箱環境預覽，確認後直接匯出。設計資料存在本機的 SQLite 資料庫，不送到任何外部服務。

## 支援哪些 AI CLI

Open Design 啟動時自動偵測你環境中已安裝的 AI CLI，支援清單包括：

Claude Code、Codex、Cursor Agent、Gemini CLI、OpenCode、Qwen CLI、GitHub Copilot、Hermes、Kimi CLI。

不需要手動設定，系統找到哪個 CLI 就用哪個。有 Anthropic API key 的話，也可以直接在介面貼入，走 BYOK 路徑。

## 安裝步驟

環境需求：Node.js v24 以上，pnpm 10.33.x。先確認版本：

```
node -v   # 要 24 以上
pnpm -v   # 要 10.33.x
```

```
node -v   # 要 24 以上
pnpm -v   # 要 10.33.x
```

Clone 專案並安裝依賴：

```
git clone https://github.com/nexu-io/open-design.git
cd open-design
corepack enable
pnpm install
```

```
git clone https://github.com/nexu-io/open-design.git
cd open-design
corepack enable
pnpm install
```

啟動 Web 介面：

```
pnpm tools-dev run web
```

```
pnpm tools-dev run web
```

第一次啟動，系統會在目錄下建立 `.od/` 資料夾作為本地執行環境，存放 SQLite 專案資料庫和歷史產出紀錄。這個資料夾只在你的機器上，不會同步到任何地方。

根據網址打開網頁就會看到 UI 介面：

![截圖 2026 05 05 10.46.04](https://yujustcoding.com/wp-content/uploads/2026/05/%E6%88%AA%E5%9C%96-2026-05-05-10.46.04.png)

截圖 2026 05 05 10.46.04

如果電腦已經載了 Claude Code 或 Codex，Open Design 都可以自動偵測得到。

你也可以在設定中修改語言，接上 MCP，甚至是養一隻寵物XD

![截圖 2026 05 05 10.48.34](https://yujustcoding.com/wp-content/uploads/2026/05/%E6%88%AA%E5%9C%96-2026-05-05-10.48.34.png)

截圖 2026 05 05 10.48.34

## 基本操作流程

啟動後，你會看到歡迎對話框，主要選擇兩件事：AI CLI（通常已自動偵測完成），以及要套用的 Design System。

預設使用 `web-prototype` skill 和 `Neutral Modern` 設計系統，適合快速上手，當然有興趣的也可以多方嘗試，反正免費！

![截圖 2026 05 08 11.05.18](https://yujustcoding.com/wp-content/uploads/2026/05/%E6%88%AA%E5%9C%96-2026-05-08-11.05.18.png)

截圖 2026 05 08 11.05.18

操作模式很簡單：直接說你要什麼樣的設計，選擇對應的 Skill，AI agent 生成產出，右側會出現即時預覽，確認後匯出。71 套 Design Systems 包含各種品牌規範，從中性風格到特定產品設計語言都有。選定設計系統後，生成的原型會自動套用對應的色彩和排版規則。

## 幫我產生這篇文章的IG輪播圖

稍微微調一下，就可以看到美感和 Claude Design 不相上下！

![截圖 2026 05 05 14.22.55](https://yujustcoding.com/wp-content/uploads/2026/05/%E6%88%AA%E5%9C%96-2026-05-05-14.22.55.png)

截圖 2026 05 05 14.22.55

## MCP 整合：接進現有工具鏈

Open Design 附帶一個 stdio MCP server，可以接進 Claude Code、Codex、Cursor 或任何支援 MCP 協議的客戶端。

接上之後，外部 agent 可以直接查詢你本地 Open Design 專案的設計資源，包括 Design Token、CSS 變數、JSX 元件和 HTML 入口檔。回傳的是當前檔案的實際狀態，不是上次匯出的舊版本。

設定方式是把 Open Design 的 stdio server 加進 MCP 客戶端設定檔，具體格式因 CLI 工具而異，詳細設定參考 repo 根目錄的說明文件。

## 常見問題

### 一定要有 Anthropic API Key 嗎？

不一定。如果你已安裝其他支援的 AI CLI（Cursor、Gemini CLI 等），Open Design 可以直接使用，不需要 Anthropic API Key。Key 只在你要走 BYOK 路徑時才需要。

### 生成的設計可以直接用在正式專案嗎？

可以。匯出的 HTML 原型是標準格式，可以作為前端開發的起點。PPTX 是標準 Office 格式，可以在 PowerPoint 或 Keynote 繼續編輯。

### 離線可以用嗎？

本地運算部分可以離線。AI 生成需要對應的 CLI 工具能連線，取決於你使用的 CLI 本身是否支援離線模式。設計系統選擇和預覽功能完全在本地，離線可用。

### 和 Claude Design 的輸出品質有差距嗎？

輸出品質主要取決於你使用的 AI CLI 和選擇的 Design System。使用 Claude Code 搭配 Open Design，輸出風格和 Claude Design 接近，因為底層模型相同。使用 Gemini CLI 或 Cursor，風格取決於對應模型的能力。

## 總結

Claude Design 把設計生成綁在訂閱制和雲端服務上，Open Design 把同樣的功能搬回本地，讓你自己選 AI 工具、自己控資料、不用為偶爾才用到的功能付月費。

如果你在意工具選擇的自由度，或是有資料保密的需求，可以先用開源的 Open Design 試試看。