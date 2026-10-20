---
title: "Agent Team 實戰（四）：身份會過期——從 99 行瘦身到 47 行的 Em Refactor"
date: 2026-04-13
tags: [ai/agent, type/how-to]
source: https://emilwu.tw/articles/26-agent-team-identity-lifecycle/
type: idea
status: processed
asset_value: 36
---

「Agent Team 實戰」系列第四篇（作者 EMil Wu），系列中發表順序最前的一篇，屬於系列必要環節（低於一般 40 分下限但例外收錄，見 Team Lead 裁決）。

**核心貢獻：** Agent 的 CLAUDE.md 是可能過期的資產，需要定期 refactor。Em 的 CLAUDE.md 從 99 行（身份定義+7步ingest+5步query+6步lint workflow全部 eager loading）瘦身到 47 行——只留 Identity + Core Rules，workflow 移到 `.claude/commands/`、page format 移到 `.claude/skills/wiki-schema`，改為 JIT loading，Context 負擔下降 50%。核心原則：CLAUDE.md 只放「我是誰」和「我怎麼判斷」，「我會做什麼」放 commands，「我知道什麼」放 skills。

**身份混淆事故案例：** GM 誤用 Agent G（bootstrap agent，「盡快建起來」的判斷框架）的身份直接動手做了 4 分鐘 commit，而非用 GM 自己「對話 before dispatch」的判斷框架。金句：「能力不夠的 Agent 會告訴你他做不到，身份過期的 Agent只會安安靜靜地用舊方式做新的事」——弄錯身份定義的代價不是做錯事，而是用錯誤的判斷框架做了看起來對的事，且沒有任何錯誤訊息。

## 相關連結

- [[agent-team-practice-03-identity-directory-structure]] —— 系列（三），承接關係：本篇的「身份會過期」直接延續（三）定義的五層身份框架
- [[agent-team-practice-05-dispatch-modes]] —— 系列下一篇，Em 首次被 spawn 時繼承 GM 身份的具體事故，直接延續本篇的身份主題
- [[agent-team-practice-07-three-layer-memory]] —— 系列第七篇，JIT loading 原則從身份層延伸到記憶層（三層各自獨立、需要時才載入/查詢）
