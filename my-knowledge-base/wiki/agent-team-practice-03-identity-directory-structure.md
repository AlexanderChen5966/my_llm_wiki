---
title: "Agent Team 實戰（三）：身份——每個 Agent 都需要知道自己是誰"
date: 2026-04-13
tags: [ai/agent, type/how-to]
source: https://emilwu.tw/articles/25-agent-team-identity/
type: article
status: processed
asset_value: 60
---

「Agent Team 實戰」系列第三篇（作者 EMil Wu），承接系列（二）97% Markdown 之後，講建了 Agent Team 之後碰到的第一個問題：身份。核心論點是「身份＝整個目錄結構」，並以五個 Specialist Agent 的實際設計決策逐一佐證。

## T（核心論點）

**Agent 的身份不是一句 System Prompt，而是整個目錄結構的組合。** 身份是 Context 的第一層，這一層歪了，後面不管怎麼精心設計 dispatch、memory、coordination 都會跟著歪掉。完整身份由五層組合而成：CLAUDE.md（你是誰、你怎麼思考）＋ METHODOLOGY.md（你判斷事情的原則）＋ Commands 目錄（你會做什麼）＋ Skills 目錄（你知道什麼）＋ 工作目錄（你的東西/別人的東西在哪）——五者缺一，Agent 就會用猜的，而且他不會告訴你他猜了。

## B（適用場景）

**適用：**
- 設計多個持續存在、有固定職責的獨立 Agent（而非用完即丟的 Subagent）
- 需要判斷某個角色該設計成獨立 Agent 還是同一 Agent 下的 Subagent
- 治理多個 Agent 之間的溝通權限，避免 fleet 擴大後多頭管理

**不適用 / 注意：**
- Subagent 模式下身份不是問題——Subagent 沒有身份，是 parent Agent 的臨時延伸，做完就消失
- 身份設計並非一次到位，會隨系統演化而過期（見系列下一篇的 Em Refactor 案例）

## R（操作步驟）

1. **五層身份組件缺一不可**：CLAUDE.md、METHODOLOGY.md、Commands 目錄、Skills 目錄、工作目錄，任何一層空白都會讓 Agent 用猜的方式行動。
2. **canonical location 判準（C7 案例）**：每個資產只有一個標準位置，其他地方需要用時用 provision（佈建）而非複製；查詢永遠只看一個地方，更新只需要改 canonical location。
3. **Subagent vs 獨立 Agent 判準（Dm 案例）**：三個角色（Architect 設計 / Scaffolder 搭建 / Validator 驗證）若彼此 Context 需要高度互相參照、完整傳遞，就設計成同一 Agent 底下的 Subagent，而非三個獨立 Agent——呼應「Skill vs Subagent」的判斷標準：Context 是否需要留在主對話裡。
4. **sacred boundary 判準（G7 案例）**：只有 G7 可以跟 Layer 3 worker 說話，是治理決策而非技術限制，目的是避免 fleet 長到 50 個時陷入多頭管理。
5. **從行為記錄反向提煉身份（GM 案例）**：GM 是唯一「先有大量行為紀錄、再從裡面提煉身份」的 Agent，而非「先有需求再設計身份」；核心身份是一串「不要做什麼」清單（不要急著派工、不要跳過對話直接動手、不要沒諮詢知識庫就做設計決策），且 CLAUDE.md 被要求零 references 到前身 bootstrap agent 以確保 clean start。
6. **wiki 結構本身定義思考方式（Em 案例）**：Em 的身份不是「你是知識管理員」七個字，而是整個 wiki 分類體系（concepts/entities/sources）決定他怎麼理解新資訊、怎麼連結、什麼值得記錄。

## C（關鍵行動）

- [ ] 檢查自己專案裡每個 Agent/Skill 是否具備完整五層身份組件，找出缺口
- [ ] 遇到「這個角色該獨立成 Agent 還是設計成 Subagent」的決策時，用「Context 是否需要留在主對話裡」判準
- [ ] 若管理多個工作用 Agent，評估是否需要設計 sacred boundary 式的單一溝通入口

## 原始摘要

系列第三篇，核心論點：Agent 身份不是一句 System Prompt，而是整個目錄結構（CLAUDE.md+METHODOLOGY.md+Commands目錄+Skills目錄+工作目錄五層組合）。逐一分析五個 Agent 的身份設計決策：Em（wiki目錄結構本身定義思考方式）、C7（canonical location 規則）、Dm（Architect/Scaffolder/Validator 三角色用 Subagent 而非獨立 Agent）、G7（sacred boundary：只有 G7 能跟 Layer 3 對話）、GM（身份是從行為記錄反向提煉出的「不要做什麼」清單）。引用 arXiv ICLR 2025 研究：1600+ traces 中 "Disobey role specification" 是多 agent 系統關鍵失敗模式。文末預告下一篇談身份會過期需要更新。

## 相關連結

- [[agent-team-practice-01-starting-point]] —— 系列開篇，起點與三層架構的由來
- [[agent-team-practice-02-97-percent-markdown]] —— 系列（二），97% Markdown 數據，上一篇承接點
- [[agent-team-practice-04-identity-lifecycle-refactor]] —— 系列（四），身份會過期，需要定期 refactor（本篇的延續）
