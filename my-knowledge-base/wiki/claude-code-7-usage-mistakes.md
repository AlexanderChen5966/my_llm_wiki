---
title: 說實話，你的 Claude Code 用法是錯的：7 個常見錯誤與正解
date: 2026-06-30
tags: [ai/claude, ai/workflow, type/how-to]
source: https://codelove.tw/@tony/post/37ZZ9x
type: article
status: processed
asset_value: 70
---

作者用 3 個月每天實戰（Expo + Supabase + Swift 原生通話 App）歸納出「覺得 Claude Code 不夠快」的人幾乎都犯的 7 個錯誤與對應正解。核心思維轉換：**從「寫程式」轉成「委派任務」——把 AI 當成很優秀但 3 分鐘就失憶的天才來交辦**；不要「養對話」，要「養機制」。

## T（核心論點）

Claude Code 慢或不穩，多半不是模型或方案問題，而是用法。關鍵是把長期脈絡永續化到**檔案**（CLAUDE.md / Skills / PLAN.md）而非塞進對話，並把重複性工作切給**子代理**，讓主對話當指揮官而非親自下場。

## B（適用場景）

- 覺得 Claude Code「沒想像中快」、越用越飄
- CLAUDE.md 已膨脹、MCP 全部常駐、session 從早用到晚
- 想建立可複用的委派機制而非一次性對話

## R（7 個錯誤與正解）

1. **CLAUDE.md 寫成全包式聖經**（2000 行每輪都進上下文）→ 保持最小化，只放套件管理器、部署指令、跨檔規則等；特定知識丟 Agent Skills（按需觸發）。「CLAUDE.md 是憲法，Skills 是六法全書」。
2. **所有工作塞進同一指令**（修測試+重構+文件+PR）→ lost in the middle，注意力變薄。把責任切給子代理（如唯讀的 reviewer 只回報 bug 不實作）。
3. **實作到一半就 /compact** → 壓縮會把「接下來要做什麼」摘要吃掉，重開後疊上半套實作而不自覺。只在**工作分界點（提交後）**compact；長任務先把計畫寫進 PLAN.md。
4. **MCP 一次全接還常駐** → 工具定義本身佔上下文。只接當天需要的；多個時用 Deferred Tool Loading 延遲載入 schema。「先全裝」是 2024 思維，2026 講減法。
5. **連設計都丟給 AI** → 得到「看起來能跑的地雷」（作者的 PushKit 用 let 區域變數接住 → 收不到來電）。設計是人做的，實作才交 AI；牽涉原生層/並行/生命週期一定人類親眼確認所有權與執行緒。
6. **審查也偷懶** → 產生變快 = bug 流入變快，不是變少。用專職審查子代理初審 + 人類最後確認；diff 保持小（1000 行 PR 人和 AI 都審不好）；「測試過」≠「正確」。
7. **同一對話從早用到晚** → 上下文無限變長、回應偏掉。任務一變就換 session；脈絡存檔案。「不要養對話，要養機制」。

## C（對自身的啟發）

- [ ] 檢視本知識庫的 CLAUDE.md 是否夠精簡（呼應 claude-md-optimization-techniques 的 <200 行原則）
- [ ] 重複性 wiki 維護工作可考慮切成專職子代理（如審查/對齊）
- [ ] 長任務先寫 PLAN.md，compact 只在分界點——直接可用於 raw-pipeline 這類長流程

## 相關連結

- [[claude-code-session-handoff]] — 🔑 **`/compact` 的工具選擇**：同一對話續跑用 `/compact` 即可、不必動用 handoff 檔（本篇管時機、該篇管工具選擇）

- [[claude-md-optimization-techniques]] — CLAUDE.md 精簡化技巧
- [[claude-code-context-management]] — Context 管控
- [[claude-code-12-rules-error-reduction]] — 12 條規則降低錯誤率
- [[agent-skills-vs-mcp]] — Skills vs MCP：按需載入
- [[loop-engineering-6-components]] — 迭代式工程（小步丟→驗證→修正）
