---
title: Anthropic 官方 17 個 Claude Skills：完整功能與角色推薦清單
date: 2026-06-24
tags: [ai/claude, ai/agent]
source: https://www.bnext.com.tw/article/91307/anthropic-17-claude-skills-
type: reference
status: processed
asset_value: 58
---

Anthropic 在 GitHub 開源 17 個官方 Skill，分 5 大類。核心邏輯：Skill 載入時只讀 `name` + `description`（漸進式揭露），任務匹配後才載入完整指令——裝越多不代表越好，**實務建議 5-8 個精選**，功能相近的會觸發競爭。

## T（核心論點）

每個 Skill 解決「每次都要重新說明一遍」的問題：Claude 載入後，不需要重給脈絡，輸出品質更穩定。官方 Skill 的優勢是 description 已調校好觸發邏輯。

## R（17 個 Skill 完整清單）

### 類別一：文件製作（4 個）

| Skill | 功能 | 適合對象 |
|-------|------|---------|
| **pdf** | 建立、填寫、編輯、擷取 PDF | 報告、發票、合約摘要 |
| **docx** | 產生/編輯 .docx，支援樣式、表格、追蹤修訂 | 顧問、法務、PM |
| **xlsx** | 建立/解析 Excel，保留公式與格式 | 財務分析師、營運團隊 |
| **pptx** | 產生可編輯 PowerPoint，含圖表、投影片母版 | 業務、培訓、主管報告 |

四個合用構成完整文件流程：PDF 擷取 → Excel 分析 → Word 報告 → PowerPoint 簡報。

### 類別二：設計與品牌（4 個）

| Skill | 功能 | 適合對象 |
|-------|------|---------|
| **brand-guidelines** | 將品牌色彩、字型、排版規則套用至所有輸出 | 行銷、品牌、代理商 |
| **canvas-design** | 2D 畫布排版，有圖層、字型、間距概念 | 社群圖文、資訊圖表 |
| **theme-factory** | 產生完整配色、字型組合與元件風格 | 設計師快速迭代 VI |
| **algorithmic-art** | 生成參數化與程序性藝術（基於 p5.js）| 創意開發者、生成藝術 |

### 類別三：工程與開發（5 個）

| Skill | 功能 | 適合對象 |
|-------|------|---------|
| **claude-api** | 教 Claude 正確使用 Anthropic API（串流/工具呼叫/批次）| 開發者串接 Anthropic API |
| **mcp-builder** | 生成 MCP Server，打通 AI 與內部系統 | 工程師建立 Agent 工作流 |
| **frontend-design** | 現代前端模式（React、Tailwind、無障礙設計）| Claude Code 生成 UI |
| **web-artifacts-builder** | 產生複雜單頁應用（狀態管理/routing/shadcn/ui）| 教育者、產品人製作 demo |
| **webapp-testing** | 用 Playwright 自動測試 Claude 生成的前端程式碼 | 工程師讓 AI 驗證自己的輸出 |

### 類別四：溝通與內容（3 個）

| Skill | 功能 | 適合對象 |
|-------|------|---------|
| **doc-coauthoring** | 協作長篇文件，保留作者聲音，建議而非改寫 | 重視文風的寫作者 |
| **internal-comms** | 標準化全員公告、週報、狀態更新格式 | 主管、創辦人、公關 |
| **slack-gif-creator** | 生成適合 Slack 尺寸的動態 GIF | 活絡團隊文化 |

### 類別五：Meta Skill（1 個）

| Skill | 功能 |
|-------|------|
| **skill-creator** | 引導撰寫符合官方格式的自訂 Skill（含 YAML frontmatter、description 措辭、效果測量）|

## B（角色推薦起點）

| 身份 | 推薦組合 |
|------|---------|
| 每週交付文件的商務用戶 | pdf + docx + xlsx + pptx |
| 內容與品牌 | brand-guidelines + canvas-design + doc-coauthoring |
| 工程師 | claude-api + mcp-builder + frontend-design + webapp-testing |
| 管理團隊 | internal-comms + brand-guidelines |
| 用熟之後 | skill-creator → 開始打包自訂工作流 |

## C（使用原則）

- **5-8 個精選 > 全部安裝**：功能相近的 Skill 觸發競爭，過多反而降低準確度
- **適用平台**：claude.ai 免費版（需啟用 Code execution）/ claude.ai 付費版 / Claude Code / Claude API
- **安裝方法**：付費版可直接從 Skill Library 安裝；免費版需下載 ZIP 上傳；Claude Code 用指令安裝

## 相關連結

- [[claude-skills-ecosystem]] — GitHub 16 個高 star Skills Repo 分類（社群 Skill 生態）
- [[claude-agents-skill-file-comparison]] — SKILL.md vs CLAUDE.md vs AGENTS.md 三檔分層策略
- [[skill-md-9-types-and-gotchas]] — Skill 九大類型與 Gotchas 方法論
- [[ai-agent-skills-popular-2026]] — 2026 最受歡迎社群 Skills（Superpowers/Karpathy/GStack）
