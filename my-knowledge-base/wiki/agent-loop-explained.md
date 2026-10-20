---
title: 全網爆紅的 Loop 到底是什麼？Agent Loop 概念總覽
date: 2026-06-30
tags: [ai/agent, type/concept]
source: https://codelove.tw/@tony/post/qenW4x
type: article
status: processed
asset_value: 60
---

由 OpenClaw 創辦人 Peter Steinberger 一句「你不該再給 Agent 寫提示詞，你應該設計循環來提示 Agent」（推文破 800 萬瀏覽）引爆的概念總覽。核心：**Agent Loop 執行的是「目標」而非「指令」**，透過反覆循環讓輸出逼近目標，符合目標才終止。

## T（核心論點）

傳統迴圈重複執行明確指令；Agent Loop 追求模糊目標、每步自適應調整、能從失敗學習——本質是 ReAct 範式（推理→行動→觀察→循環）。它早已深入日常開發（CLI 回傳非零碼時 Agent 自動讀錯誤、修復、重跑測試），只是沒被注意。

## B（適用場景）

- 想搞懂 Loop / Prompt / Context / Harness 的關係
- 評估是否該把工作從「寫提示詞」升級為「設計循環系統」
- **適合**：自動化開發維護、CI/CD 修復、多 Agent 協作、資料分析報告
- **不適合**：一次性簡單對話、成本敏感專案、目標本身模糊無驗證標準

## R（主要內容）

**AI 工程化四層階梯**（由內而外堆疊，權威順序 Prompt → Context → Harness → Loop）：
| 階段 | 核心問題 | 解決 |
|------|---------|------|
| Prompt Engineering | 怎麼問？ | 讓 AI 聽懂 |
| Context Engineering | 給它看什麼？ | 讓 AI 做對 |
| Harness Engineering | 在什麼環境安全運行？ | 讓 AI 安全行動 |
| Loop Engineering | 怎麼讓它自己持續往前？ | 讓 AI 自主做完成 |

> **⚠️ 順序修正（2026-07）**：本條目原始來源（codelove 中文轉述）把 Loop 排在 Harness 之下（Prompt→Context→Loop→Harness），但經上網交叉比對 Addy Osmani 本人 blog、codecentric、O'Reilly 三個權威來源，一致順序為 **Prompt → Context → Harness → Loop**——Loop 在最外層。關鍵原理（codecentric）：「當 Agent 不再由人類逐一觸發、而是自己持續運轉時，harness 就變成了 loop」，故 Loop 嚴格位於 Harness 之上。上表已更正。

**兩種 Loop**：單 Agent Loop（一個 Agent 在自己的 ReAct 循環內迭代把事做對）vs 多 Agent 協作（多角色 Agent 共享上下文、互相呼叫把事做完，靠頻道/執行緒/任務板三組件）。

**Loop Engineering 六塊積木**（Google 工程師 Addy Osmani）：自動化機制（心跳）、工作區隔離（Worktrees）、技能（Skills）、連接器（Connectors）、子代理（Sub-agents 交叉驗證）、外部記憶（Memory 存磁碟）。

**產品化案例 PingCAP Loop**：從「1 對 1 聊天」到「1 對 N 團隊協作」；每個 Agent 有 Soul（身分）、Memory（MEMORY.md）、Skills、獨立工作空間；任務板可被 Agent 認領。

**優勢**：處理開放性任務、持續累積、並行效率、可稽核可干預。
**侷限**：Token 消耗硬傷（設計不好會繞圈燒錢）、回饋機制是命門（沒有能說「不」的測試/型別檢查就像沒裁判的比賽）、概念疊太快、成熟度仍新。

## C（對自身的啟發）

- [ ] 「從提示工程師到系統架構師」的轉變，呼應本知識庫把工作變成 skill + 機制
- [ ] 設計任何自動化流程都要有「能說不」的回饋（測試/驗證）與明確停止條件
- [ ] 外部記憶存磁碟的原則，正是本 kit 用 Markdown wiki 累積知識的理由

## 相關連結

- [[loop-engineering-6-components]] — Loop Engineering 六組件（深入實作）
- [[loop-engineering-human-roles]] — Loop 時代人類三職責
- [[loop-engineering-implementation-patterns]] — Claude Code 實作模式
- [[loop-engineering-playbook-cli-tools]] — 實戰 Playbook：Tier 分層 runtime + 7 個模式 + CLI 工具（本篇概念的操作層）
- [[harness-engineering]] — Harness 運行環境層
- [[context-engineering-fundamentals]] — Context Engineering 基礎
- [[hermes-agent]] — Hermes Agent（多 Agent 協作實例）

> 註：Loop Engineering 主題在 2026-06 有大量媒體轉述同一則 Steinberger/Cherny/Osmani 新聞（bnext / vocus / whoops / jasonchuang / codelove 等），內容與本條目及 loop-engineering 系列高度重疊，已於 2026-07-16 統一評估——重複轉述不另建條目，僅 Cobus Greyling 的 Playbook + GitHub repo 因有 Tier 分層與 CLI 工具等新內容而獨立成 [[loop-engineering-playbook-cli-tools]]。
