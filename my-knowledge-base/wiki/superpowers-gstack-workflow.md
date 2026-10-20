---
title: Superpowers + gstack 搭配實戰指南
date: 2026-05-14
tags: [ai/agent, ai/claude, pkm/workflow]
source: https://cloud.tencent.com/developer/article/2658137
type: article
status: processed
asset_value: 80
---

## T（核心論點）

Superpowers（工程紀律）與 gstack（產品生命週期）從源碼設計就天然互補——一個自動觸發管「怎麼寫好代碼」，一個手動觸發管「做什麼、怎麼上線」，5 個關鍵交接點構成從需求到上線的完整開發閉環。

## B（適用場景）

**適用：** 同時使用 Superpowers 和 gstack 兩個插件的開發者；需要從需求到上線全流程 AI 輔助的場景；追求開發流程可預測、可複現的團隊或個人

**不適用 / 注意：**
- 兩套技能文件同時佔用上下文，token 消耗增加；若任務只需其中一個插件，建議暫時禁用另一個
- gstack 命名衝突風險：若與其他插件共存建議用 `./setup --prefix` 啟用 `/gstack-` 前綴

## R（操作步驟）

**安裝**
1. 安裝 Superpowers：`/plugin install superpowers@claude-plugins-official`
2. 安裝 gstack：`git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack && cd ~/.claude/skills/gstack && ./setup`

**CLAUDE.md 配置（關鍵）**
3. 在 CLAUDE.md 加入分工宣告，避免模糊指令引發技能衝突：
   - `Superpowers`（自動觸發）：plan、brainstorm、debug、TDD、verify、code review
   - `gstack`（手動斜杠命令）：browser、QA、ship、deploy、canary、security audit

**完整開發閉環（37 技能路由）**
4. 需求分析：`brainstorming`（Superpowers）→ `/autoplan`（gstack，多視角審查）
5. 計劃撰寫：`writing-plans` → `using-git-worktrees`（隔離分支）
6. 編碼：`subagent-driven-development` + `test-driven-development`（TDD 紅綠循環）
7. 驗證：`verification-before-completion` → gstack `/qa`（真實 Chromium 瀏覽器端到端）
8. 審查：`requesting-code-review`（Superpowers）→ gstack `/review`（Staff 級別）
9. 發布：`finishing-a-development-branch` → `/ship` → `/land-and-deploy` → `/canary`

## C（關鍵行動）

- [ ] CLAUDE.md 必須寫清楚分工裁決，否則 Claude Code 遇到模糊指令（「做個計劃」）會隨機匹配
- [ ] TDD 通過後還要跑 gstack `/qa`——單元測試無法驗證真實瀏覽器渲染、用戶交互流程
- [ ] 5 個交接點：brainstorming→/autoplan、writing-plans→/plan-eng-review、TDD→/qa、systematic-debugging→/investigate、finishing-branch→/ship

## 原始摘要

騰訊雲社區文章（2026-05-11），從源碼架構驗證兩插件的互補性：Superpowers 是 Jesse Vincent 的工程紀律框架（94% PR 拒絕率），gstack 是 Garry Tan（YC CEO）的產品全生命週期工具集。

**核心差異對比**：
| 維度 | Superpowers | gstack |
|------|------------|--------|
| 觸發方式 | 自動觸發（Agent 偵測場景）| 手動斜杠命令 |
| 核心關注 | 代碼工程方法論（TDD/調試/審查）| 產品生命週期（戰略/QA/發布/監控）|
| 安裝路徑 | `~/.claude/skills/superpowers-*` | `~/.claude/skills/gstack/` + 符號連結 |

**兩種推薦方案**：
- 方案 A（追求工程規範）：Superpowers 全自動 + gstack 在 `/autoplan`、`/qa`、`/ship` 三個節點出場
- 方案 B（個人/快速原型）：gstack Sprint 流程為主 + Superpowers 在 TDD、debugging、verification 三個節點兜底

## 補充資料

來源：raw/ 圖片素材（2026-05-25 補充）

**superpowers_flow.jpg — Superpowers 開發流程圖**
展示五步驟循環：腦力激盪與設計 → 撰寫實作計畫（TDD）→ 子代理驅動開發 → 自動化程式碼審查 → 完成與交付，標示四大核心理念（TDD 測試先行、系統化流程、降低隨機性、實證主義），以 AI 驅動「豐富的技能庫」為核心。

**superpowers_vs_gstack.jpg — Superpowers vs gstack 比較筆記**
手寫風格的學習筆記，比較 Superpowers（把 AI 提升為工程師紀律，強調 TDD 與深度驗證）與 gstack（虛擬 AI 產品團隊，角色化分工、先規劃後執行）的定位、核心特點與設計理念差異，並提供使用情境選擇建議。

## 補充資料：GSD 作為第三層穩定器

來源：codelove.tw（2026-05-22）《Claude Code 技能堆疊：如何將 Superpowers、gstack 與 GSD 結合而不陷入混亂》

### 三層分工：不是二選一，是三方協作

| 層 | 框架 | 角色 |
|----|------|------|
| 決策層 | gstack | 思考——CEO/PM/EM 角色，規劃 Sprint |
| 穩定層 | GSD | 穩定——上下文與規範的持久錨點 |
| 執行層 | Superpowers | 執行——TDD、debug、驗證工程紀律 |

> **一句話：gstack 思考，GSD 穩定，Superpowers 執行。**

### GSD 的定位（常被忽略的關鍵）

GSD（Get Stuff Done）定位為「上下文穩定器」，解決長期工作中 context 逐漸漂移的問題。當任務跨越多個 session，gstack 的規劃文件可能失去效力，GSD 提供持久的規範錨點，防止 Claude 隨著對話演進偏離最初的架構決策。

### 選擇邏輯

- 缺乏**決策能力** → 加入 gstack（給它 CEO/PM 視角）
- 缺乏**持久上下文** → 加入 GSD（防止長任務漂移）
- 缺乏**執行紀律** → 加入 Superpowers（強制 TDD 與驗證）

**注意**：無計畫地堆疊框架往往會**降低**系統穩定性，選技能的邏輯應從「你缺什麼」出發，而非「越多越好」。

## 相關連結

- [[superpowers-framework]] — Superpowers 完整 Skill 清單與 7 步驟開發工作流
- [[gstack-framework]] — gstack 23+ Skill 完整 Sprint 流程（Think→Plan→Build→Review→Test→Ship→Reflect）
- [[synthesis-ai-dev-workflow-frameworks]] — Superpowers / gstack / spec-driven / Skills 生態四篇整合精粹
