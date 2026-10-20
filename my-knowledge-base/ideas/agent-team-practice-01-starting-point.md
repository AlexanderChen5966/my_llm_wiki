---
title: "Agent Team 實戰（一）：起點——為什麼要建 Agent Team"
date: 2026-04-13
tags: [ai/agent, type/concept]
source: https://emilwu.tw/articles/23-agent-team-starting-point/
type: idea
status: processed
asset_value: 24
---

「Agent Team 實戰」系列開篇（作者 EMil Wu）。低於一般 40 分收錄門檻，比照既有系列（四）的先例作為明確例外收錄——系列完整性本身有價值，拿掉會讓系列從頭缺一角。

**核心敘事：** 作者描述凌晨兩點同時開五個 Claude Code session、人肉搬運 Context 的臨界點場景，歸納出 Subagent 模式在跨 session/跨時間場景下的三個問題：Context 不持久（session 結束脈絡全部消失）、身份不獨立（Subagent 是臨時分身、非正職員工）、跨 session 無法溝通（只能人工複製貼上或寫檔案）。明確區分官方 Claude Code Agent Teams（mailbox 機制為「同一專案裡多個 worker」設計）與作者要的「各自獨立身份與專業的多個 specialist」——兩者設計假設從出發點就不同，此差異在後續篇章展開。第一個產出是 HANDOFF.md（非程式碼、非架構圖），核心心法是「把人類腦中的隱含知識寫成 Agent 能讀懂的明文」。最終架構長成三層：Layer 1 Leader(GM)／Layer 2 Specialists(Em/C7/Dm/G7)／Layer 3 Workers，且強調「架構是從需求裡長出來的，不是設計出來的」。引用 DeepMind 研究：無結構 "bag of agents" 產生 17.2x 錯誤放大。

## 相關連結

- [[agent-team-practice-02-97-percent-markdown]] —— 系列下一篇，接續本篇的三層架構，講建構過程的實證數據
