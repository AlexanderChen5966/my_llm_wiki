---
title: Impeccable Style：AI Coding Agent 程式碼風格規範指南
date: 2026-05-20
tags: [ai/agent, ai/claude]
source: https://tools.wingzero.tw/article/sn/3915
type: article
status: processed
asset_value: 12
---

Impeccable Style 是一套專為 AI coding agent 設計的程式碼風格規範網站，核心定位是「可直接給 AI 使用的 coding style guide」。目標不是讓 AI 能寫程式，而是讓 AI「寫出團隊真正願意接受的程式」。

## T（核心論點）

AI coding agent 時代，約束 AI 往往比呼叫 AI 更重要。傳統 style guide 是給人類讀的，Impeccable Style 把工程規範轉換成 AI 更容易理解與遵循的形式，本質是 machine-readable engineering culture。

## B（適用場景）

**適用：** AI 深度參與的開發團隊；長期維護的大型 codebase；使用 Claude Code / Cursor 的工程師；希望建立 AI coding 標準的公司；多人與多 agent 同時協作的場景
**不適用 / 注意：** 一次性小型專案（規範收益不明顯）；與 lint 工具（ESLint/Prettier）定位不同，是哲學層級的規範而非格式化工具

## R（操作步驟）

1. 從 Impeccable Style 網站複製適合的 style 規範內容
2. 放入專案的 AI 規則文件中（`CLAUDE.md`、`AGENTS.md`、`.cursor/rules/`、`SYSTEM_PROMPT.md`）
3. 在 Claude Code 或 Cursor 中啟用 always-on rules
4. 讓 AI 在生成與修改程式碼時自動套用規範
5. （選用）搭配 Cursor 時可拆分為 scoped rules，依資料夾套用不同規則

## C（關鍵行動）

- [ ] 把 Impeccable Style 規範整合進現有 CLAUDE.md 或 AGENTS.md
- [ ] 與 Skills（任務）、Rules（邊界）的三層架構搭配：Style 定義 AI 應該怎麼寫

## 原始摘要

Impeccable Style 涵蓋命名規範、架構偏好、工程原則，強調簡潔與可維護性，降低 AI 過度設計問題。與 Agent Rules Books、Andrej Karpathy Skills 形成互補關係：Skills 定義 AI 如何完成任務、Rules 定義 AI 能做什麼、Style 定義 AI 應該怎麼寫。

## 相關連結

- [[claude-md-guide]] — CLAUDE.md 撰寫技巧，放置 Style 規範的主要載體
- [[agents-md-guide]] — AGENTS.md 作為 Style 規範的替代載體
- [[claude-skills-ecosystem]] — Skills 在 AI 開發工作流中的位置

## 補充資料（來源：pbakaus/impeccable GitHub README，2026-05）

### 完整 23 命令清單

所有命令透過 `/impeccable <command>` 呼叫，或用 `/impeccable pin <command>` 建立獨立捷徑：

| 命令 | 功能 |
|------|------|
| `craft` | 完整 shape-then-build 流程含視覺迭代 |
| `teach` | 一次性設定：收集 design context，產出 PRODUCT.md 和 DESIGN.md |
| `document` | 從現有專案程式碼反向生成 DESIGN.md |
| `extract` | 從程式碼提取可重用元件和 tokens 到設計系統 |
| `shape` | 寫程式碼前先規劃 UX/UI |
| `critique` | UX 設計審查：層次、清晰度、情感共鳴 |
| `audit` | 技術品質檢查（a11y、效能、響應式）|
| `polish` | 最終 pass：設計系統對齊 + 上線前整備 |
| `bolder` | 強化無聊的設計 |
| `quieter` | 緩和過於大膽的設計 |
| `distill` | 提煉到本質 |
| `harden` | 錯誤處理、i18n、文字溢位、邊緣情況 |
| `onboard` | 首次使用流程、空狀態、激活路徑 |
| `animate` | 加入有目的性的動態 |
| `colorize` | 引入策略性色彩 |
| `typeset` | 修正字體選擇、層次、大小 |
| `layout` | 修正版面、間距、視覺節奏 |
| `delight` | 加入令人愉悅的細節 |
| `overdrive` | 加入技術上非凡的效果 |
| `clarify` | 改善不清楚的 UX 文案 |
| `adapt` | 針對不同裝置適配 |
| `optimize` | 效能改善 |
| `live` | 視覺變體模式：在瀏覽器中迭代元件 |

### 7 個領域參考文件

每個命令執行時自動載入所有 7 個 domain reference：

| 參考文件 | 涵蓋範圍 |
|---------|---------|
| typography | 字型系統、字體配對、模組比例、OpenType |
| color-and-contrast | OKLCH、tinted neutrals、dark mode、無障礙 |
| spatial-design | 間距系統、網格、視覺層次 |
| motion-design | 緩動曲線、staggering、reduced motion |
| interaction-design | 表單、focus states、loading patterns |
| responsive-design | Mobile-first、流體設計、container queries |
| ux-writing | 按鈕標籤、錯誤訊息、空狀態 |

### 反模式規則（部分列舉）

Impeccable 內建 27 條確定性反模式規則（無需 LLM，CLI 可獨立執行）＋ 12 條 LLM critique 規則：

- 避免過度使用字體（Arial、Inter、系統預設）
- 避免彩色背景上使用灰色文字
- 避免使用純黑或純灰（一律加入色調）
- 避免把所有東西包在 card 裡或 card 套 card
- 避免使用 bounce / elastic easing（感覺過時）

### CLI 獨立掃描（無需 AI key）

```bash
npx impeccable detect src/               # 掃描目錄
npx impeccable detect index.html         # 掃描 HTML 檔
npx impeccable detect https://example.com  # 掃描 URL（Puppeteer）
npx impeccable detect --fast --json .    # regex-only，JSON 輸出
```

偵測 24 種問題：AI slop（side-tab borders、purple gradients、bounce easing、dark glows）＋一般設計品質（行長、cramped padding、小觸控目標等）。

### 支援 11 種工具的安裝（範例：Claude Code）

```bash
# 專案級
cp -r dist/claude-code/.claude your-project/

# 全域（所有專案）
cp -r dist/claude-code/.claude/* ~/.claude/
```

其他支援工具：Cursor、OpenCode、Pi、Gemini CLI、Codex CLI、VS Code Copilot、Kiro、Trae（中國版 + 國際版）、Rovo Dev、Qoder。授權：Apache 2.0。
