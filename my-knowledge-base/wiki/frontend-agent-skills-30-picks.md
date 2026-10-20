---
title: 前端領域 30 個值得安裝的 Agent Skills（2026年6月）
date: 2026-06-15
tags: [ai/claude, ai/agent]
source: https://codelove.tw/@tony/post/agDlLq
type: reference
status: processed
asset_value: 62
---

基於 skills.sh 排行榜與 Claude Code Frontend Design Toolkit 精選，2026年6月前端 Skills 生態分七大類，安裝指令統一為 `npx skills add <owner/repo>`，支援 Claude Code / Cursor / Copilot / Codex / Gemini CLI 等 16+ 工具。

## T（核心論點）

前端 Skills 生態已相對成熟，Anthropic 官方（frontend-design、canvas-design）和 Vercel Labs（vercel-react-best-practices、vercel-deploy）提供最高可信度的基礎 Skills；`find-skills`（159萬+ 安裝）是探索更多 Skills 的最佳入口。

## B（適用場景）

**適用：**
- 設定 Claude Code 前端開發環境，想知道裝哪些 Skills
- 評估前端 Skills 的可信度（官方 vs 社群來源）
- 針對特定角色（設計師、全端、大型維護）選擇最小有效 Skills 組合

**不適用 / 注意：**
- 資料截止 2026年6月，Skills 生態迭代快，具體安裝數可能已變化
- 建議只從 Anthropic、Vercel Labs、Remotion、Supabase 等官方/知名倉庫安裝（Skill 可執行 Python 腳本，需注意安全）

## R（七大分類）

### 一、設計 / 視覺

| # | Skill | 維護者 | 安裝 |
|---|-------|--------|------|
| 1 | **frontend-design** | Anthropic 官方（277K+ 安裝）| `claude plugin add anthropic/frontend-design` |
| 2 | **web-design-guidelines** | Vercel Labs（107K 安裝）| `npx skills add vercel-labs/agent-skills` |
| 3 | **ui-ux-pro-max** | nextlevelbuilder（67種風格、97配色）| `npx skills add nextlevelbuilder/ui-ux-pro-max-skill` |
| 4 | **canvas-design** | Anthropic 官方 | `npx skills add anthropics/skills` |
| 5 | **theme-factory** | Anthropic 官方 | `npx skills add anthropics/skills` |
| 6 | **brand-guidelines** | Anthropic 官方 | `npx skills add anthropics/skills` |
| 7 | **web-artifacts-builder** | Anthropic 官方（HTML artifact）| `npx skills add anthropics/skills` |
| 8 | **fixing-motion-performance** | ibelick | `npx skills add ibelick/ui-skills` |

### 二、UI 打磨 / 無障礙

| # | Skill | 說明 | 安裝 |
|---|-------|------|------|
| 9 | **baseline-ui** | 去除「agent UI slop」| `npx skills add ibelick/ui-skills` |
| 10 | **fixing-accessibility** | WCAG 自動審查 | `npx skills add ibelick/ui-skills` |
| 11 | **fixing-metadata** | SEO / 中繼資料修復 | `npx skills add ibelick/ui-skills` |
| 12 | **a11y-agent-team** | 無障礙 Agent 團隊，22 個子 Agent | 見原文 |

### 三、React / Next.js

| # | Skill | 說明 | 安裝 |
|---|-------|------|------|
| 13 | **vercel-react-best-practices** | Vercel 工程團隊（141K 安裝）| `npx skills add vercel-labs/agent-skills` |
| 14 | **composition-patterns** | React 組合模式 / React 19 | `npx skills add vercel-labs/agent-skills` |
| 15 | **react-best-practices-build** | 建置 / 打包優化 | `npx skills add vercel-labs/agent-skills` |
| 16 | **react-view-transitions** | View Transition API 動畫 | `npx skills add vercel-labs/agent-skills` |
| 17 | **vercel-deploy** | 一鍵部署到 Vercel | `npx skills add vercel-labs/agent-skills` |

### 四、框架 / 函式庫

| # | Skill | 說明 |
|---|-------|------|
| 18 | **vue-best-practices** | Vue 3 最佳實踐 |
| 19 | **tailwind-best-practices** | Tailwind CSS |
| 20 | **typescript-pro** | TypeScript 進階 |
| 21 | **remotion-best-practices** | 用程式碼做影片（96K 安裝）`npx skills add remotion-dev/skills` |

### 五、測試 / 瀏覽器自動化

| # | Skill | 說明 |
|---|-------|------|
| 22 | **agent-browser** | AI 操作瀏覽器（41K 安裝）|
| 23 | **playwright-best-practices** | Playwright 測試 |
| 24 | **test-driven-development** | TDD 工作流 `npx skills add obra/superpowers` |

### 六、效能 / 品質 / 除錯

| # | Skill | 說明 |
|---|-------|------|
| 25 | **repomix** | 程式碼庫壓縮注入上下文 `npx skills add yamadashy/repomix` |
| 26 | **pr-review-toolkit** | 6 Agent 精審 |
| 27 | **security-guidance** | OWASP 風險自動攔截 |
| 28 | **feature-dev** | 設計→實作→審查全流程 |

### 七、元技能

| # | Skill | 說明 |
|---|-------|------|
| 29 | **find-skills** | Skills 探索入口（159萬+ 安裝）`npx skills add vercel-labs/skills` |
| 30 | **skill-creator** | 把個人經驗封裝成 Skill |

## 推薦組合

| 角色 | 建議 Skills |
|------|------------|
| 獨立前端 / 全端 | `frontend-design` + `vercel-react-best-practices` + `baseline-ui` + `fixing-accessibility` + `find-skills` |
| 設計驅動前端 | `frontend-design` + `web-design-guidelines` + `ui-ux-pro-max` + `theme-factory` + `brand-guidelines` |
| 大型專案維護 | `repomix` + `vercel-react-best-practices` + `composition-patterns` + `pr-review-toolkit` + `security-guidance` |
| 影片 / 動效方向 | `remotion-best-practices` + `react-view-transitions` + `fixing-motion-performance` |

## C（關鍵行動）

- [ ] 先裝 `find-skills`（AI 自動依情境推薦 Skill），再從以上清單補充特定角色 Skills
- [ ] 個人優先考慮：`frontend-design`（最多安裝）+ `baseline-ui`（去除 AI 樣板感）

## 相關連結

- [[claude-code-design-skills-overview]] — 六款 Design Skill 精選（frontend-design / UI Skill / Taste 等，與本條目互補）
- [[uiux-pro-max-skill]] — ui-ux-pro-max 完整說明（161 產業規則 + 67 風格）
- [[agent-skills-toolkit]] — agent-skills：22 個 Skill 完整開發生命週期
- [[claude-skills-ecosystem]] — GitHub 16 個高 star Skills Repo 四檔分類
- 📎 2026-10-19：[[ideas/claude-code-animation-prompt-qa-delivery]] — 程式動畫的需求寫法與驗收（可搭配 remotion-best-practices、fixing-motion-performance）
- 📎 2026-10-19：[[claude-codex-video-toolchain-selection]] — Remotion 專案內 `npx remotion skills add`、可重現規則（只由幀數驅動）與官方授權（≤3 名員工營利組織免費）見該條
