---
title: design.md：Google Labs 讓 AI 看懂設計系統的格式規範
date: 2026-06-30
tags: [ai/agent, dev/architecture, type/concept]
source: https://israynotarray.com/ai/20260627/1947382651/
type: article
status: processed
asset_value: 66
---

design.md 是 Google Labs 從 AI 設計工具 Stitch 孵化、以 Apache 2.0 開源的格式規範，把設計系統拆成「YAML 設計 Token + Markdown 設計理由」兩層，讓 Claude Code、Cursor、Copilot 這類 AI Coding 工具不只看到色碼數字，還能讀懂背後設計意圖，產出一致的 UI。

## T（核心論點）

AI 產 UI 時顏色/字級/圓角前後不一致（一下 `#3B82F6`、一下 `bg-blue-500`），根因是**沒給 AI 一份結構化的設計系統規則**。CLAUDE.md / AGENTS.md 是自然語言規則，沒有結構化色票，AI 仍要「猜」。design.md 把設計系統變成「機器能精準讀取 + 人類能讀懂理由」的單一檔案，解決一致性問題。

## B（背景與適用場景）

- Vibe Coding 時 AI 版面總是跑掉、風格不統一
- 想讓多個 AI Coding 工具（Claude Code / Cursor / Copilot）共用同一份設計真相源
- 需要把設計 Token 匯出到 Tailwind / W3C DTCG
- 注意：目前為 alpha 版，無官方 Claude Code/Cursor 整合範例，需自行在規則檔引導

## R（主要內容）

**兩層結構**：
- **YAML Front Matter**：精確設計 Token（colors / typography / rounded / spacing / components 五大類）。Token 可互相引用（`"{colors.tertiary}"`），改一處全連動。顏色支援 hex/rgb/hsl/oklch。
- **Markdown 內文**：設計理由——為什麼選這顏色、何時用、何時不要用。官方章節序：Overview / Colors / Typography / Layout / Elevation / Shapes / Components / Do's and Don'ts。僅 Colors 的 primary 為必填。

**CLI 四工具**（`npm install @google/design.md`）：
- `lint`：驗證 YAML schema、Token 引用、**WCAG AA 對比度檢查**（最實用）、orphan token、章節順序
- `diff`：比較兩版差異，列出受影響 Token/元件
- `export`：轉 Tailwind v3（json-tailwind）/ Tailwind v4（css-tailwind）/ W3C DTCG（dtcg）
- `spec`：輸出格式規範本身，丟給 AI Coding 工具讓它讀懂 schema

**使用方式**：把 DESIGN.md 放專案根目錄，在 CLAUDE.md / AGENTS.md / cursor rules 加一句「做 UI 任務時優先讀 DESIGN.md 並嚴格遵守 Token 與設計理由」。可從 VoltAgent/awesome-design-md 挑現成風格。

**開源戰略觀察**：Google 開源是「拿時間換生態」——卡標準定義權、讓競品自願當盟友推格式、累積 AI 生 UI 的訓練資料 flywheel、當 Stitch 的免費行銷漏斗（同 Chromium/K8s/TensorFlow 打法）。

**提醒**：design.md 不要塞無關內容，理念同 Agent Skills——給 AI 重點不是給 AI 全部。

## C（對自身的啟發）

- [ ] Flutter / 前端專案可建一份 DESIGN.md 統一 AI 產 UI 的設計 token
- [ ] WCAG 對比度 lint 可直接整合進設計流程做無障礙檢查
- [ ] 與 figma-to-requirements skill 對照：design.md 可當設計 token 的上游來源

## 相關連結

- [[ai-web-design-system-guide]] — AI 網頁設計系統指南
- [[agents-md-guide]] — AGENTS.md 撰寫指南
- [[claude-md-guide]] — CLAUDE.md 撰寫
- [[agent-skills-vs-mcp]] — Agent Skills 的「給重點」理念
- [[screenshot-to-enterprise-frontend-skill]] — 截圖轉前端
