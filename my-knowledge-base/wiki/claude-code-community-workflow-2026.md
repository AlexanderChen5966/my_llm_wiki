---
title: Claude Code 社群工作流大整理（2026）
date: 2026-06-05
tags: [ai/claude, ai/workflow, ai/agent]
source: https://hackmd.io/@BASHCAT/BJBh08pgGe
type: article
status: processed
asset_value: 100
---

## T（核心論點）

整理 2025-2026 年 Claude Code 社群累積的最佳實踐，核心發現：**全社群唯一共識是 Plan-First**（在 Claude 寫 code 前先讓它寫 plan），其他所有工具（worktree、sub-agent、SDD、TDD）都是可選扳手。Claude Code 創造者 Boris Cherny 本人的設定意外地「乾淨」——不用 worktree、只用極少 sub-agent。客製化六件套（CLAUDE.md / Slash Commands / Skills / Sub-agents / Hooks / MCP Servers）中最關鍵的區分：CLAUDE.md 是 advisory（可被忽略），Hooks 是 mandatory（shell 層強制）。

## B（適用場景）

**適用：**
- 所有 Claude Code 使用者的工作流設計參考
- 評估是否需要 worktree、sub-agent 等進階功能的決策框架
- 硬體/韌體工程師的最小可行配置（Plan mode + CLAUDE.md + PostToolUse hook + MCP）

**不適用 / 注意：**
- Worktree 工廠模式僅適合每個 feature 至少 20 分鐘以上的場景
- Sub-agent 過度設計會 Gatekeep Context，失去整體推理能力（Shrivu Shankar 反論）
- Boris Cherny 是 Anthropic 員工用 Opus 不心疼 token，百萬級 token 對 Pro 用戶不現實

## R（操作步驟）

1. **Plan-First**：Session 開始按 Shift+Tab 兩次進入 Plan mode，討論 1-6 輪確認 plan 後再切換到 auto-accept edits
2. **CLAUDE.md 精簡化**：控制 < 200 行，用 `file:line` 指標取代貼程式碼
3. **區分 advisory vs mandatory**：「commit 前必跑測試」寫成 Hook 而非 CLAUDE.md
4. **區分 action vs instruction**：需要 Claude 做動作用 Skill，按特定方式思考用 Slash Command
5. **Sub-agent 克制原則**：先確定一個工作流會做 10 次以上，再打包成 sub-agent
6. **TDD 明示再明示**：在 plan 之後加碼 `Please add instructions to use TDD and go through the red-green-refactor cycle in baby steps`
7. **防 test masking**：用 hook 強制每次 edit 後跑測試套件，或用 sub-agent 分離寫測試和寫實作

## C（關鍵行動）

- [ ] 將 Plan mode（Shift+Tab 兩次）養成肌肉記憶
- [ ] 檢查自己的 CLAUDE.md 是否超過 200 行，超過則精簡
- [ ] 盤點 ~/.claude/ 下的 skill 和 sub-agent，砍掉過去兩週未使用超過 5 次的
- [ ] 用 10 條反模式清單自檢目前工作流

## 原始摘要

作者（BASHCAT，硬體韌體背景）整理了 Anthropic 官方、Boris Cherny 13 步設定、Boris Tane、Shrivu Shankar、Simon Willison、Cole Medin 等 power user 的長文與 Reddit 高讚討論。關鍵洞察：（1）Boris Cherny 自己根本不太用 worktree 和 custom sub-agent；（2）客製化六件套的正確使用邊界；（3）Sub-agent 社群正在分裂（Shrivu 的 Master-Clone 架構 vs Cole Medin 的 dream team）；（4）SDD 是 plan mode 的 persistent 版本，大專案才值得；（5）Claude Code codebase 只有 1.6% 是 AI 決策邏輯，98.4% 是 scaffolding。最後給出硬體/韌體工程師的三件式 starter pack 和 10 條社群血淚反模式。

## 補充資料（2026-07-16）

來源：〈Claude Code，先看這篇就夠了的整理（2026年版）〉（新鮮人工程師 fuyunoki 整理，[[ideas/claude-code-getting-started-2026-note]]）。此篇整體概觀與本條目高度重疊，但「現役工程師的實踐經驗」章節有幾則本條目未涵蓋的具體心得：

- **review 別變成挑毛病大會**：只對 review 角色下「幫我找問題」指示，容易得到大量不影響實作的小地方，全部修掉反而導致 over-engineering。應明確指示「只回報會影響正確性或需求的問題，其他一律視為可選項」
- **分段提交不只是給人看的，也是給 Claude 的**：按任務步驟分段提交，Claude 走偏時人類和 Claude 都能快速回到前一狀態，比一次做大變更再檢查的恢復速度快很多
- **UI 實作用「先看再修」反覆流程**：先給 mockup 圖讓它實作，再把實作結果截圖拿去跟 mock 比對修正，重複 2-3 次完成度明顯提升，優於想一次到位
- **auto-accept 模式的使用邊界**：跳過確認提示的自主模式適合原型開發或周邊功能；涉及核心商業邏輯的修改仍應同步給明確指示、一步一步來
- **新程式碼庫上手導覽**：直接問 Claude「這個 API 是怎麼設計的」「這功能最後一次是誰改的」，它會沿著 Git 歷史找出設計意圖，幾乎不需特殊 prompt 技巧，對新鮮人很實用
- **與 monorepo 的相容性**：schema/API 定義/實作放同一儲存庫時，Claude 較容易一次掌握整體 context

## 相關連結

- [[claude-code-advanced-workflow]] - Boris Cherny 15 個進階技巧
- [[claude-md-guide]] - CLAUDE.md 撰寫技巧
- [[claude-code-context-management]] - Context 管控全攻略
- [[claude-code-plugins-guide]] - Plugin 六元件系統
- [[spec-driven-ai-workflow]] - SDD 規格驅動工作流
- [[harness-engineering]] - Harness Engineering：scaffolding 比模型本身重要
- [[synthesis-ai-dev-workflow-frameworks]] - AI 開發工作流框架精粹
