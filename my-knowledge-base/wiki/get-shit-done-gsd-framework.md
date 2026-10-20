---
title: get-shit-done（GSD）：用「上下文保鮮」對抗 Context Rot 的 Claude Code 框架
date: 2026-06-22
tags: [ai/agent, ai/claude]
source: https://github.com/open-gsd/gsd-core
type: reference
status: processed
asset_value: 80
---

GSD（Git. Ship. Done.）是 TÂCHES（glittercowboy）開源的輕量 Meta-Prompting + Spec-Driven Development 框架（64k+ stars），專為 Claude Code 設計、現已支援 Codex / Gemini CLI / Cursor / Windsurf 等多 runtime。核心解決 **Context Rot（上下文腐爛）**：把所有重研究/規劃/執行工作丟進全新 context 的 subagent，主 session 始終保持精簡。整合自 3 篇來源（gsd-build 原 repo / open-gsd/gsd-core 官方 / WenAI 深度原始碼解析）。

> **repo 遷移注意**：原 `gsd-build/get-shit-done` 已導向社群維護的 `open-gsd/gsd-core`，安裝改用 `npx @opengsd/gsd-core@latest`。

## T（核心論點）

多數 AI coding 設定在規模化時失敗，因為三件事：context bloat 悄悄拉低輸出品質、session 之間沒有共享記憶、沒有東西驗證 code 真的能跑。GSD 的核心哲學是**保持 Claude 的 Context 絕對純淨**——不依賴一個無限長的對話框，而是把開發流程強制拆成獨立階段，每階段開全新 context、用嚴謹的 Markdown 檔（`STATE.md` / `CONTEXT.md` / `ROADMAP.md` / `PLAN.md`）傳遞狀態，而非依賴 AI 自己的記憶。**「Claude Code is powerful. GSD makes it reliable.」** 這與 [[gstack-framework]]、[[superpowers-framework]] 同屬「把工程師 Best Practice 變成 AI 自動遵守的機制」這一脈。

## B（適用場景）

**適用：**
- 獨立開發者 / 小團隊的**中大型任務**（會跨越多個 context window 的專案）
- 需要平行多工加速、又要避免 subagent 互相污染 context

**不適用 / 注意：**
- 改個小 bug 或加一個函式 → 用 `/gsd:quick` 或直接叫 Claude Code 就好，不必走完整六階段
- 不要直接複製 `agents/` 或 `commands/` 檔案——必須用 installer 才能跨 runtime 相容

## R（六階段 Phase Loop + Multi-Agent 架構）

### 每個 Milestone 重複的五~六步迴圈
```
Discuss → Plan → Execute → Verify → Ship（→ 下一個 Phase）
```
| 階段 | 命令 | 重點 |
|------|------|------|
| **Initialize** | `/gsd-new-project` | 反覆釐清需求、挖邊界條件，產出含所有階段的 `ROADMAP.md` |
| **Discuss** | `/gsd:discuss-phase` | 先挑出模糊處（UI 怎樣？API 錯誤怎麼回傳？），打包成 `CONTEXT.md` |
| **Plan** | `/gsd:plan-phase` | 研究文件、生成 atomic 的 XML 任務清單 `PLAN.md`，並驗證計畫塞得進一個 fresh context |
| **Execute** | `/gsd:execute-phase` | 按 Wave 平行開新 context 寫 code，每個任務獨立 Atomic Git Commit |
| **Verify** | `/gsd:verify-work` | 走查所建之物 + UAT，診斷並修復後才宣告完成 |
| **Ship** | `/gsd:ship` | 開 PR、歸檔 phase、進下一個 |

### Multi-Agent 角色
- **CLI Orchestrator（皮包骨指揮官）**：不寫半行邏輯，只觸發 Lifecycle Hooks、呼叫 Agent、彙整 Markdown
- **Researcher**：讀開源庫文件、survey 解法
- **Planner**：把需求轉成 **XML 格式 prompt**（XML 標籤讓 Claude 最精準解析結構）
- **Executor**：被限制只專注眼前單一任務清單的「寫扣勞工」
- **Verifier（含 UI Auditor）**：排查錯誤，UI 六大面向審核

## C（關鍵行動 — 四個值得偷學的設計）

- [ ] **Wave-Based 平行執行**：Plan 階段用 YAML `depends_on` 算好相依，無相依的 Plan 分到同一 Wave，**同時 spawn 多個 Executor，各自全新 200k context**。主 session context 始終維持 30-40% 不降智。啟示：**Vertical Slice（垂直切片）天然適合平行化**——按「User 功能端到端 / Product 功能端到端」拆，而非「所有 Model → 所有 API → 所有 UI」的水平分層
- [ ] **Checker → Revise 自動修復閉環**：Verifier 用**四級查核**（L1 檔案存在 / L2 是否只是 10 行空殼 stub / L3 用 `grep` 查 import 與調用有沒有接上 / L4 回溯資料來源看是否回傳假資料 `return json([])`）→ 失敗打包成結構化 YAML `gaps` 自動送回 Planner → 產 Fix Plan（只修壞的部分）→ Executor 重跑。人類只做最後 UAT 一哩路
- [ ] **Context 保鮮**：Planner 被告知 Claude「智商衰退曲線」（0-30% 巔峰 / 50%+ 偷懶 / 70%+ 敷衍），強制每個 Plan 在 50% context 內完成、最多 2-3 個 Task。核心：**用「多個短對話」取代「一個長對話」**
- [ ] **需求覆蓋率追蹤**：Roadmapper 鐵律——100% v1 需求須對應至少一個 Phase，無孤兒需求、無重複指派。用 Goal-Backward（以終為始）：問「這階段完成時使用者能觀察到什麼成果」，每個 Observable Truth 反向連結到需求 ID

## 原始摘要

GSD 2025 年底才出現，幾個月衝到 40k→64k stars。它的強大不在提示詞多花俏，而是把軟體工程 Best Practice 變成 AI 能自動遵守的紀律系統：波次平行解決**速度**、四級驗證閉環解決**品質**、Context 保鮮解決**降智**、需求覆蓋率追蹤解決**完整度**，四者環環相扣。即使不直接用它，光學會「Wave 拆法」與「Checker→Revise 閉環」就能提升用任何 AI 工具的效率。

## 相關連結
- [[gstack-framework]] — YC CEO 的全流程框架（同樣 Claude Code skill 棧，GSD 偏 context 紀律 / gstack 偏多角色戰略審查）
- [[superpowers-framework]] — TDD 工程紀律框架（與 GSD 同屬「把紀律寫進機制」一脈）
- [[harness-engineering]] — AI 開發系統設計的更高層視角（GSD 是 harness 思想的具體實作）
- [[context-engineering-fundamentals]] — Context Engineering 基礎（GSD 對抗 context rot 的理論根基）
- [[spec-driven-ai-workflow]] — 規格驅動工作流
- [[ai-sdd-framework-comparison]] — SDD 四框架比較（ZeroSpec / OpenSpec / SpecKit / BMAD，GSD 可對照其定位）
- [[concepts/agent-workflow-taxonomy]] — GSD 屬第 ⑤ 類「規格 / 工程流程」
