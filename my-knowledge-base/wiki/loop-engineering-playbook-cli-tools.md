---
title: Loop Engineering 實戰 Playbook：Tier 分層 runtime、7 個模式與 CLI 工具鏈
date: 2026-07-16
tags: [ai/agent, pkm/workflow, type/reference]
source: https://cobusgreyling.medium.com/loop-engineering-playbook-4460e01e88d8（Playbook）／https://github.com/cobusgreyling/loop-engineering（開源 repo）
type: reference
status: processed
asset_value: 66
---

Cobus Greyling 的 Loop Engineering 實戰參考：在 [[agent-loop-explained]] / [[loop-engineering-6-components]] 的概念層之上，補足「迴圈該住在哪裡（runtime 選型）」「先跑哪個模式」「怎麼分階段推出而不燒爆 token」三個可操作維度，並提供三支可直接 `npx` 執行的 CLI 工具（loop-audit / loop-init / loop-cost）與 7 個 clone-and-run 的生產模式。

## T（核心論點）

**Loop 是一個小型控制系統，不是產品 logo**——Grok Build 與 Claude Code 佔據了目前討論，但迴圈的基本元件是可轉移的，**該選哪個 runtime 取決於「迴圈跑多久、服務誰、被允許碰什麼」**。核心洞察：Grok/Claude Code 是絕佳的「harness + loop」表面（`/loop`、skills、sub-agents、worktrees、MCP 一站到位），對單人/小團隊通常夠用；但當迴圈需要「凌晨 3 點沒人開終端也要跑、mid-run 崩潰要能存活、暫停等人審核三天、每個 agent 要有自己隔離的機器」時，就進入了 **runtime 領域**（LangChain 在此建設）。

## B（適用場景）

判斷手上的 Loop 該用終端工具還是生產級 runtime；想從概念落地到「先跑哪個模式、怎麼安全推出」；需要量化評估自己 Loop 的就緒度或估算 token 成本；已讀過 Loop 概念（agent-loop-explained）想要下一步的操作手冊。

## R（重點整理）

### Runtime 三層分層（Playbook 的核心新框架）

| Tier | 定位 | 適用 |
|------|------|------|
| **Tier A：終端 harness 迴圈** | Grok Build / Claude Code 的 `/loop`+skills+sub-agents+worktrees+MCP | 單人/小團隊，開發者自己擁有、範圍在熟悉的 repo、有人在旁 |
| **Tier B：平台 runtime 迴圈** | LangChain LangSmith 等生產平台 | 服務產品或團隊、必須存活重啟、需要稽核軌跡、跑非自己寫的程式碼 |
| **Tier C：編輯器與輕量替代** | 編輯器綁定的 cron/webhook | 已鎖定某編輯器、想要排程但不想導入完整 agent 平台 |

**LangChain「給 agent 一台自己的電腦」的生產答案**（值得對照 Loop 設計）：LangSmith Sandboxes（每 agent 硬體隔離 microVM，含檔案系統/shell/套件/網路/持久狀態，非共用容器）／Durable execution（check-point 圖狀態，20 分鐘的執行能撐過崩潰與部署）／Cron on Agent Server（排程執行與互動 session 共用同一套 tracing 與 auth）／Human-in-the-loop interrupts（迴圈暫停、釋放資源、數天後從確切 checkpoint 恢復）。**核心判準**：「我在 TUI 裡跑著 `/loop`」不是可接受的生產答案時，就該往 Tier B 走。

**Stateful vs Stateless cron 的區分**（即使不用 LangSmith 也值得偷來用）：Stateful cron 每次用同一個 `thread_id`，agent 記得昨天（夜間研究、帶歷史的監測）＝「append 到 STATE.md」；Stateless cron 每次全新 thread（批次分流、一次性掃描）＝「讀 repo 後 exit」。

### 五塊積木 + 記憶（與 6-components 對齊，但 GitHub repo 給了可視化與 Mermaid 解剖圖）

Loop 解剖（Mermaid）：Discover → 讀寫 STATE/Memory → Worker → **Verifier sub-agent（tests + gates）** → Gate →（risky/ambiguous 時）帶完整上下文升級給人類。Sub-agents 用 **maker/checker 拆分**，Memory/State 是「任何對話之外的持久脊椎」。

### 7 個生產模式（clone-and-run，附節奏與 token 成本）

| 模式 | 節奏 | Week 1 起步 | Token 成本 |
|------|------|------------|-----------|
| Daily Triage（每日分流） | 1天–2小時 | L1 report | 低 |
| PR Babysitter（顧 PR） | 5–15 分 | L1 watch | 高 |
| CI Sweeper（掃 CI） | 5–15 分 | L2 cautious | 極高 |
| Dependency Sweeper（掃依賴） | 6時–1天 | L2 patch-only | 中 |
| Changelog Drafter | 1天或 tag 觸發 | L1 draft | 低 |
| Post-Merge Cleanup | 1天–6時 | L1 off-peak | 低 |
| Issue Triage | 2時–1天 | L1 propose-only | 低 |

**分階段推出**：L1 report（只報告）→ L2 assisted fixes（協助修）→ L3 unattended（無人值守）——先從最便宜、最安全的 report-only 起步。

### 三支 CLI 工具（GitHub repo 的可操作產出，皆免 clone 直接 npx）

- `npx @cobusgreyling/loop-audit . --suggest` — Loop Readiness Score（就緒度評分，會看 budget/run-log，分數 empty→L1→L2 遞增）
- `npx @cobusgreyling/loop-init . --pattern daily-triage --tool grok` — scaffold 現成 starter + budget/run-log
- `npx @cobusgreyling/loop-cost --pattern daily-triage --level L1` — 依節奏估算 token 花費

### 成本與安全預設（Loops 讓成本倍增得很快）

- 分流要便宜：一次模型掃訊號，不要每個 cron tick 都跑 implementer + verifier
- 只在 state 說「可行動」時才 spawn sub-agent（5 分鐘迴圈每次都跑完整 maker/checker 會在早餐前燒光有限方案額度）
- 每個項目限制迭代次數：三次嘗試後升級給人類

### 理解債（comprehension debt）——本篇特別強調的隱性風險

「AI agent 正在寫我們的程式碼，理解債會是新的技術債嗎？」速度會掩蓋「存在的東西」與「你理解的東西」之間的差距——迴圈交付了修復，不代表你知道它怎麼運作。**建構迴圈時要像一個打算繼續當這位工程師的人，而不只是按下 go 的人**。舒適的陷阱是「認知投降（cognitive surrender）」——迴圈是槓桿，不是判斷力的替代品。Osmani、Cherny（Anthropic 內部）、LangChain 團隊都從各自角度講同一件事：trace 告訴你發生了什麼，但還是得有人去讀它。

## C（關鍵行動）

- [ ] 判斷手上 Loop 屬於 Tier A（有人在旁的開發者迴圈）還是 Tier B（生產/多租戶）——別用終端工具硬扛生產級無人值守需求
- [ ] 新 Loop 一律從 L1 report-only 起步，跑穩後才升 L2/L3
- [ ] 導入前用 `npx @cobusgreyling/loop-cost` 估算節奏成本，用 `loop-audit` 評估就緒度
- [ ] 高頻迴圈（5–15 分）避免每 tick 都跑完整 maker/checker，只在 state 可行動時 spawn
- [ ] 把「理解債」納入設計考量：留下自己讀得懂的 trace 與規則，而非放任 AI 快速堆疊無人理解的程式碼

## 相關連結

- [[loopx-stateful-control-plane]] — ⚠️ **第三條路**：本篇的答案是「換 runtime」，LoopX 則是不換 runtime、在外面加一層持久狀態（Objective／Gate／Todo／Scope／Evidence／Quota）；本篇列的 runtime 需求（凌晨無人也要跑、mid-run 崩潰要存活、暫停等審核三天）在那邊分別對應 Quota-aware、Evidence Log 與 Gate
- [[agent-loop-explained]] — Loop 概念總覽（四層階梯 Prompt→Context→Harness→Loop、兩種 Loop），本篇是其操作層 Playbook
- [[loop-engineering-6-components]] — 六大組件 + Worker/Verifier 分離 + 5 個 Agent 死因（本篇的五塊積木同源，此篇補 runtime 選型與 CLI 工具）
- [[loop-engineering-human-roles]] — Loop 時代人類三職責（理解債/驗證/反認知投降，與本篇「comprehension debt」呼應）
- [[loop-engineering-implementation-patterns]] — Claude Code 具體實作模式（CLAUDE.md 迴圈協議 + Stop hook）
- [[harness-engineering]] — Harness 運行環境層（Tier A 的底層）
