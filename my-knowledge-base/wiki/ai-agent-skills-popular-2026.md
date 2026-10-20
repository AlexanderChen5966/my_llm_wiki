---
title: 2026 年最受歡迎 AI Agent Skills 生態：五大主流工作流框架
date: 2026-06-23
tags: [ai/agent, ai/claude, pkm/workflow]
source: https://codelove.tw/@tony/post/aGQ8Jx
type: note
status: processed
---

## T（主題脈絡）

2025-2026 年，Agent Skills（代理技能）成為 GitHub 上成長最快的工具類別之一。Superpowers 有 226k 顆星（超過 Vue），Karpathy 準則有 174k 顆星，兩者都只是 Markdown 資料夾，不是程式碼。格式最早由 Anthropic 定義：一個 `SKILL.md` 檔，包含名稱、一行描述、指令。現在可在 Claude Code、Kiro、Cursor 等主流代理工具上運作。

## B（核心主張）

**Agent Skills 的核心價值是「把工程師的隱性判斷變成顯性工作流」**。市場上主流 Skills 的根本分歧只有一條：「包辦整個流程」（Superpowers、GStack）vs「小而可組合」（Matt Pocock）。選擇取決於你想要把控制權交出多少。

## R（關鍵洞察）

### SKILL.md 的按需載入機制

代理啟動時只讀每個 Skill 的簡短 description，判斷是否與當前任務相關，才載入完整指令。安裝 50 個 Skill 也不會擠爆 context——但實務上，許多工具還是需要用 `/skill名稱` 明確呼叫才觸發。

### 五大主流框架比較

| Framework | Stars | 風格 | 適合 |
|-----------|-------|------|------|
| **Superpowers** (obra/superpowers) | 226k | 完整 SDD 流程，自動觸發 | 想要從想法到審查後程式碼一條龍 |
| **Karpathy 準則** (multica-ai/andrej-karpathy-skills) | 174k | 4 條簡單規則 | 覺得 Superpowers 太重，需要低門檻起點 |
| **Matt Pocock Skills** (mattpocock/skills) | 中等 | 小而可組合，反對大包系統 | 想保留控制權，按需組合 |
| **GStack** (garrytan/gstack) | 110k | 角色分工（產品/設計/工程/QA）| 習慣用「誰負責這塊」方式思考 |
| **GSD Core** (open-gsd/gsd-core) | 中等 | Context rot 防護，子代理新視窗執行 | 長時間 session 的品質保護 |

### 各框架核心機制

**Superpowers**：自動觸發 → 對話整理規格 → 你核准 → 子代理建置 + 互相審查。Token 消耗高，Kiro 用戶可跳過（SDD 已內建）。Claude Code 安裝：`/plugin install superpowers@claude-plugins-official`

**Karpathy 準則**：四個概念：先思考再寫程式 / 保持簡單 / 外科手術式修改 / 始終聚焦目標。直接放進 `CLAUDE.md` 或 `AGENTS.md`：`curl -o CLAUDE.md https://raw.githubusercontent.com/multica-ai/andrej-karpathy-skills/main/CLAUDE.md`

**Matt Pocock 精選技能**：
- `grill-me`：開始寫任何東西前，像面試一樣問你在做什麼（防止代理亂猜需求）
- `grill-with-docs`：同上 + 為專案建立共享語言文件（跨 session 效果）
- `tdd`：紅綠重構迴圈
- `diagnose`：除錯用
- `caveman`：壓縮輸出降低 token 成本
安裝：`npx skills@latest add mattpocock/skills`

**GStack**：七種角色各有斜線指令：product-vision / designer / engineer-manager / release-manager / doc-engineer / QA / post-launch-retro。安裝後每個角色獨立呼叫，而非一條流水線。

**GSD Core**：把工作拆成原子化計畫，在全新 context 視窗中執行子代理，主 session 保持輕量。解決 context rot 問題（見 [[get-shit-done-gsd-framework]]）。

### AWS Agent Toolkit

官方 AWS 推出的技能包（`github.com/aws/agent-toolkit-for-aws`），整合 AWS 服務操作的標準化 Skill，適合需要操作 AWS 資源的開發工作流。

## C（連結與延伸）

- [[skill-md-writing-techniques]] — 撰寫高品質 SKILL.md 的技術細節
- [[skill-md-9-types-and-gotchas]] — Skill 九大類型分類法
- [[get-shit-done-gsd-framework]] — GSD 框架完整說明（context rot 防護 / 6 phase loop）
- [[synthesis-ai-dev-workflow-frameworks]] — Superpowers / GStack / spec-driven 框架深度比較
- [[synthesis-practitioner-skills-design]] — 把隱性工程師判斷轉為顯性工作流（Matt Pocock 等實踐者方法）
- [[openai-codex-record-replay]] — Codex Record and Replay 讓非技術用戶也能創建 Skill
- [[concepts/agent-workflow-taxonomy]] — 六大類型導航地圖（本條目屬於⑥ Skills 工作流層）
