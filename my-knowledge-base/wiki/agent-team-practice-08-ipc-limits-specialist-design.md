---
title: "Agent Team 實戰（八）：溝通架構——OS IPC 能教你什麼，不能教你什麼"
date: 2026-04-21
tags: [ai/agent, type/concept]
source: https://emilwu.tw/articles/31-agent-team-os-limits/
type: article
status: processed
asset_value: 60
---

「Agent Team 實戰」系列第八篇（作者 EMil Wu）。直接承接番外篇的五層決策模型，逐層驗證作者自己的 Agent Team 溝通架構，是系列承先啟後的整合篇：明確回引第二篇（身份）、第五篇（dispatch）、番外篇（五層模型）。

## T（核心論點）

> **OS 管理的是同構的 process，Agent Team 管理的是異構的 specialist。** 五層決策模型的 L0-L2（Environment/Transport/Topology）OS 框架完全適用，但到了 L3-L4（Protocol/Content Contract）以及「身份載入」這個 OS 沒有的維度，OS 的統一介面假設失效——因為每個 Agent（Em/Dm/C7/G7）的架構、Context 結構、產出格式天生不同，無法用同一套規則管理。

## B（適用場景）

**適用：**
- 設計多 specialist Agent Team（各自獨立身份、目錄、workflow）的溝通與完成報告機制時
- 想借用 OS/分散式系統既有理論驗證自己的 Agent 架構是否走在合理方向上
- 需要判斷「該不該用統一格式規範所有 Agent 的輸出」這類設計決策

**不適用 / 注意：**
- 若 Agent Team 場景其實是「team of workers」（同一身份、共用 CLAUDE.md，僅指令不同，如官方 Agent Teams 的預設用例），異構 specialist 的設計思路不適用，統一介面反而更合理
- OS 框架在 L0-L2 給的是「前人怎麼處理」的參考方向，不能直接搬到 L3-L4，需要重新從實際案例推導

## R（五個 OS 框架失效點與對應設計解法）

1. **結果提取失效 → Filesystem IS the result**：OS process 可用 exit code + stdout 回報結果，但 Agent 結束時 LLM 已消亡，Stop hook 只是 shell script，無法提取需要推理的判斷（如 Flag 欄位從未被使用過）。解法是讓 Agent 的檔案系統產出本身就是自描述的 artifact，不必額外寫報告。
2. **結果呈現失效 → Inspection Map 取代統一 message format**：Em/Dm/C7/G7 的產出結構天生不同（wiki 頁面 vs 目錄結構 vs catalog entry vs registry entry），硬套統一格式只會逼 Agent 把結構化資訊壓縮成摘要。解法是建一份 per-agent 的 inspection map，讓 GM 知道「去哪看、看什麼」。
3. **傳輸品質失效 → Return Contract（<200/2000 tokens）**：OS process 不會因訊息過長而降低處理能力，但 Agent 的 Context window 有限，raw context dump 會讓推理品質斷崖式下降。解法是訂 Level 1 回報 <200 tokens、Level 2 <2000 tokens 的壓縮上限，需要更多資訊時另外指定檔案讀取。
4. **身份維度是 OS 不存在的問題 → Hybrid Dispatch**：OS process 被 fork 後繼承 parent env 即可運作，不需要「知道自己是誰」；但 Agent 的每個判斷都受身份（CLAUDE.md）影響，載入錯誤身份等於用錯誤邏輯做所有決策。這正是催生 Hybrid Dispatch（Option B 讓 Agent 從自己目錄啟動）的根本原因。
5. **排程失效 → 三種 dispatch 模式 + human judgment gate**：OS 用統一 scheduler 管理所有 process 因為介面統一（PID/signal/kill）；GM 不能用同一套規則管所有異構 Agent，因為能力、流程、Context 結構都不同，因此長出 Mode A/B-Chain/C-Notify 三種模式並依情境判斷選用。

## C（關鍵行動）

- [ ] 設計 Agent 完成報告機制前，先判斷各 Agent 的產出是否結構天生不同——若是，優先建 inspection map 而非統一 report format
- [ ] 為 Agent 間傳遞的訊息訂定 token 上限（Return Contract），把 Context Engineering 的 JIT loading/token budgets 原則延伸到溝通層
- [ ] 若 Agent 需要獨立身份運作，確認 dispatch 機制能讓它從自己的目錄啟動載入自己的 CLAUDE.md，而非繼承 spawner 的身份
- [ ] 引用 OS/分散式系統理論驗證架構時，明確標注驗證範圍止於底層（環境/傳輸/拓撲），上層設計仍需从異構 Agent 的實際案例推導

## 原始摘要

本篇直接承接番外篇的五層決策模型，逐層驗證作者的 Agent Team：L0-L2（Same machine/File I/O/Star+limited peer）完全符合 OS IPC 最佳實踐，但 L3-L4 碰壁，根因是「OS 假設 process 同構，而 Agent Team 裡每個 Agent 是異構的 specialist」。五個 OS 框架解決不了的問題各自逼出一個 OS 教科書沒有的設計：Level 1 Completion Report 的 Flag 欄位從未被使用（shell hook 無法提取 LLM 的推理判斷）；Filesystem 本身就是 self-describing 的 result，不需額外報告；統一 message format 對異構產出無意義，改用 inspection map 讓 GM 知道去哪看什麼；Context 會因訊息過長而降級（OS process 不會），需要 Return Contract 做 explore deep, return shallow；身份載入是 OS process 不存在的維度，這正是催生 Hybrid Dispatch（Option A/B）的根本原因。結論：五層模型的價值不是教你怎麼設計，而是幫你驗證底層設計是否已經做對，並精確定位上層需要 Agent 特有設計思維的邊界。

## 相關連結

- [[agent-team-practice-bonus-os-ipc-five-layer-model]] —— 本篇直接承接的番外篇，建立五層決策模型（L0-L4），本篇逐層驗證並指出 L3-L4 侷限
- [[agent-team-practice-05-dispatch-modes]] —— 身份載入問題與 Hybrid Dispatch 設計直接呼應第五篇的 Mode A/B-Chain/C-Notify 三種混合 dispatch 模式
- [[agent-team-practice-04-identity-lifecycle-refactor]] —— 「身份載入影響推理框架」的論點承接自第四篇對身份混淆代價的討論
- [[agent-team-practice-09-a7-audit-agent]] —— 系列下一篇，延續本篇末尾留下的問題（異構 Agent 產出不能用 completion report 真正代表結果，誰來判斷團隊是否正常運作），推動稽核 Agent A7 成形
