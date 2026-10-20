---
title: Claude Code 四機制的分工邊界：CLAUDE.md / Skills / Agents / settings 各管什麼
date: 2026-07-30
tags: [ai/claude, ai/workflow, type/concept]
source: https://medium.com/@n913239（MikeWang Claude Code 系列連載 14 篇，2026-04-17～07-24）
type: reference
status: processed
asset_value: 65
---

MikeWang 的 Claude Code 週更連載共 14 篇（2026-04-17～07-24）的**索引與貫穿論點**。
本條目**不重述各篇教學內容**（知識庫對 Skills/Agents/MCP 已有 40+ 條目覆蓋），只保留這系列真正獨有的價值：**四個機制之間的分工邊界與組合順序**。

## T（核心論點）

作者自己的收束：**「靠的不是模型多強，是你在它周圍搭起來的結構。」**

而這個「結構」不是單一機制，是**四個機制各管一件事、且不該互相取代**：

| 機制 | 管什麼 | 觸發方式 |
|---|---|---|
| **CLAUDE.md** | **規範**——永久生效的專案規則與陷阱 | 每次請求自動載入 |
| **Skills** | **能力**——可重複執行的多步驟流程 | **指令觸發**（需要時才載入） |
| **Agents** | **派誰去做**——角色分工與 context 隔離 | 委派時啟動 |
| **settings.json**（含 Hooks）| **系統運作**——強制執行與自動反應 | **事件觸發**（shell 層強制）|

**關鍵對照**：
- **Hooks 是事件觸發，Skills 是指令觸發**——前者你不用說它自動做，後者要有人叫它
- **CLAUDE.md 是 advisory（請求），settings/Hooks 是 mandatory（強制）**——能用設定強制的別靠自然語言（見 `attribution` 案例於 [[claude-md-optimization-techniques]]）
- **四機制共同取代的是「自律」**——不是讓你記得做，是讓你不做也會發生

## B（適用場景）

**適用：** 已經在用 Claude Code 但機制混用、不確定某件事該寫哪裡；想規劃漸進採用路徑

**不適用 / 注意：**
- 各機制的**操作教學**請看知識庫既有條目（本系列的入門段落與既有 40+ 條目高度重疊，故未逐篇建條目）
- 系列前期文章（2026-04～05）的部分細節已被 Claude 5 世代新規推翻，對照 [[claude-md-optimization-techniques]] 的世代反轉清單

## R（漸進採用路徑）

系列的時間順序天然形成一條採用路徑：

```
① 個人設定    CLAUDE.md → Hooks + Memory
② 工作方法    Plan Mode + 驗證迴圈 → Slash Commands + Custom Agents
③ 整合實作    MCP 實戰 → Git 工作流
④ 現成打包    Superpowers（別人整套方法論）
⑤ 團隊擴散    團隊導入 → CI 守住設定 → gh 顧 CI
```

## 系列 14 篇對照表

| # | 日期 | 主題 | 知識庫對應 |
|---|---|---|---|
| 1 | 04-17 | Skills 自訂指令實戰 | 既有 40+ Skills 條目已覆蓋（未另建）|
| 2 | 04-24 | Agents 攻略 | 既有 40+ Agents 條目已覆蓋。**兩條事實值得記**：subagent **不能再派生 subagent**（架構固定兩層）；`/agents` 介面建的 agent **立即生效**，手動放 `.md` 需**重開 session** |
| 3 | 05-01 | CLAUDE.md 完全攻略 | → 補進 [[claude-md-optimization-techniques]]（`CLAUDE.local.md` 第四層、`attribution` 設定）＋ **更正 [[claude-md-design-patterns]] 的階層優先度錯誤** |
| 4 | 05-08 | Hooks + Memory | ✅ [[claude-code-hooks-memory]] |
| 5 | 05-15 | MCP 實戰 | 既有 13 條 MCP 條目已覆蓋（未另建）|
| 6 | 05-22 | Git 工作流 | ✅ [[claude-code-git-workflow]]（知識庫原本 0 條目）|
| 7 | 05-28 | 團隊導入 | ✅ [[claude-code-team-rollout]]（知識庫原本 1 條目）|
| 8 | 06-05 | 平行工作流 | 既有 [[parallel-code-multi-agent-gui]]、[[multi-agent-cli-bridge]] 已覆蓋 |
| 9 | 06-12 | Plan Mode + 驗證迴圈 | 既有 5 條目已覆蓋。**兩點值得記**（見下方）|
| 10 | 06-19 | Slash Commands + Custom Agents | 既有 4 條目已覆蓋（未另建）|
| 11 | 07-01 | 從 Demo 到 Production | **三點獨有價值**（見下方）|
| 12 | 07-07 | Superpowers | → 補進 [[superpowers-framework]]（實測反證，見下方）|
| 13 | 07-17 | 幫工作流本身寫測試 | ✅ [[testing-your-ai-workflow-config-ci]] |
| 14 | 07-24 | 讓 Claude 用 gh 顧 CI | ✅ [[claude-gh-ci-monitoring]] |

## 值得單獨記下的幾點

### Plan Mode 的兩個判準（第 9 篇）

- **「精力」＝ prompt 裡的背景與約束量，不是你花的時間**
- **Plan Mode vs OpenSpec 的單一分流判準**：「**『要做什麼』清楚了嗎？**」——不清楚才需要規格工具

### 從 Demo 到 Production 的三點（第 11 篇）

1. **agent 角色分離的品質論證**（異於常見的 context 隔離論證）：
   > **「測試和實作由同一個腦袋寫，會互相將就。」**
2. 🔴 **Hook 語法糾錯**：`matcher` **只比對工具名稱**——`Bash(git commit*)` 是 **permissions 語法，不是 hook 語法**。指令層級的判斷必須由 script 從 stdin 讀 `.tool_input.command` 自行判斷，再用 **exit code 2** 擋下（詳見 [[claude-code-hooks-memory]]、[[testing-your-ai-workflow-config-ci]]）
3. **三道防線的分工**：
   > **「測試驗證你記得的事，Hook 看守你忘記的事。」**
   > 實例：被遺忘的 debug print，測試不會抓、review 可能漏，**只有 Hook 抓到**。

### Superpowers 實測的反證（第 12 篇）

兩個真任務實測，**修正了原先的三個擔憂**：不亂開 worktree、不擅自 push、**越難越穩**。

但最有價值的是那個**反證**：
> `isNumber` 對**全形數字**回傳 `true` 的漏抓——證明**流程嚴格性 ≠ 正確性**。

三方判別（強制力來源不同）：
| | 強制力 |
|---|---|
| **Plan Mode** | 內建模式＝**硬保證** |
| **Brainstorming（Superpowers skill）** | skill 內紀律＝**被說服的** |
| **OpenSpec** | 外部規格檔＝可版控的契約 |

## C（關鍵行動）

- [ ] 遇到「這件事該寫哪裡」先套四機制分工：**永久規範→CLAUDE.md／可重複流程→Skills／角色分工→Agents／要強制執行→settings + Hooks**
- [ ] 能用 settings 強制的別靠 CLAUDE.md 自然語言請求
- [ ] 寫 hook 時記住 **matcher 只比對工具名稱**，指令層級判斷要在 script 裡做
- [ ] 別把「流程很嚴格」當成「結果正確」——Superpowers 全流程跑完仍漏抓全形數字

## 相關連結

- [[claude-md-optimization-techniques]] — CLAUDE.md 技巧（含世代反轉清單與本系列補充）
- [[claude-code-hooks-memory]] — 四機制中的 Hooks + Memory
- [[claude-code-git-workflow]]／[[claude-code-team-rollout]] — 本系列填補知識庫空白的兩篇
- [[testing-your-ai-workflow-config-ci]]／[[claude-gh-ci-monitoring]] — 系列收尾的守護鏈
- [[superpowers-framework]] — Superpowers 原始設計（本系列補上實測反證）
- [[spec-driven-ai-workflow]] — OpenSpec／opsx（同作者另一篇，見其「opsx 本質」補充章節）
- [[claude-code-three-modes-distinction]] — Claude Code 三種模式區分
- [[ai-adoption-five-stages-boris-cherny]] — 組織採用階段診斷
- [[synthesis-claude-code-internals]] — 本篇為其來源之一；該精粹釐清本篇「四機制分工」是 design-time 的**設定路由**切面，與五層架構（build-time）、harness 三層（runtime）並非同一層級的分類
