---
title: "Agent Team 實戰（九）：A7——不是再多一個 Agent，而是讓團隊開始被看見"
date: 2026-05-01
tags: [ai/agent, type/concept]
source: https://emilwu.tw/articles/32-agent-team-a7/
type: idea
status: processed
asset_value: 48
---

「Agent Team 實戰」系列第九篇（作者 EMil Wu），是既有條目 [[agent-team-practice-10-a7sus4-persistent-identity]] 的直接前置篇（同日 2026-05-01 發表，十開頭「A7 規格已明確」正是承接本篇提出的三軸拆解法）。

**核心貢獻：** 三類真實事故推動稽核 Agent A7 從「監控者」升級為「稽核者」——silent failure（任務看起來完成但後續事件未觸發，「燈沒亮」比紅燈更難發現）、chain debug ownership（Em→C7→Dm 鏈斷裂時沒人天然負責）、team drift（correction 越堆越多、completion report 越來越稀疏的緩慢退化）。

**三正交軸拆解法：** capability（看什麼：Log Audit／Cross-Agent Validation／Health Monitoring）、lifecycle（怎麼跑：long-running daemon／periodic auditor）、operational role（誰觸發、輸出去哪：Role 1 持續動態偵測／Role 2 定期靜態稽核／Role 3 被 dispatch 觀察特定 work unit）。

**observe-only 設計原則：** 若稽核者一邊稽核一邊修正，會重新陷入「稽核者與行動者共享 context」的自我驗證迴圈，故 A7 只觀察、回報、建議，是否修正交由 GM 決定（比喻：外聘驗屋團隊 vs 屋主）。核心結論句：「A7 解決的不是防止錯誤，而是縮短錯誤變成知識、再變成機制的距離」。

## 相關連結

- [[agent-team-practice-08-ipc-limits-specialist-design]] —— 系列前作，本篇開頭直接承接第八篇末尾留下的問題（異構 Agent 產出不能用 completion report 代表結果，誰來判斷團隊是否正常運作）
- [[agent-team-practice-10-a7sus4-persistent-identity]] —— 系列下一篇，本篇的直接後續，A7sus4 是本篇三軸拆解法（capability/lifecycle/role）落地為 persistent process 的具體個案
