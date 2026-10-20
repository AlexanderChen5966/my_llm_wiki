---
title: "Agent Team 實戰（二）：97% 是 Markdown——最反直覺的數據"
date: 2026-04-13
tags: [ai/agent, type/how-to]
source: https://emilwu.tw/articles/24-agent-team-97-markdown/
type: idea
status: processed
asset_value: 48
---

「Agent Team 實戰」系列第二篇（作者 EMil Wu），承接（一）的三層架構之後。

**核心數據與論點：** 七天 31 個 session、292 小時、325 個修改檔案中 97%（精確值 92.6%）是 Markdown，幾乎沒有程式碼，論證「建 Agent Team 的核心工作是定義規格而非寫程式」（CLAUDE.md 定義身份、METHODOLOGY.md 定義思考框架、Skill 文件、Wiki 頁面、Contract 文件、State file，程式碼是離核心較遠的下一階段）。記錄關鍵場景「平行處理的第一次」：Em 與 C7 平行完成各自任務但無人做 sense-making，促成後續 Completion Reporting Protocol 的設計，以及「GM 不只是 dispatcher 更是 sense-maker」的定位轉變。建構節奏呈間歇式（下午1-4點/深夜23-2點雙峰分布），因此進度判斷機制設計為 artifact-based（看檔案系統實際產出）而非 memory-based（靠記住對話內容）。結論定調：「Agent Team 解決的不是自動化的問題，而是結構化的問題」。

## 相關連結

- [[agent-team-practice-01-starting-point]] —— 系列開篇，起點與三層架構的由來
- [[agent-team-practice-03-identity-directory-structure]] —— 系列下一篇，講建了之後碰到的第一個問題：身份
