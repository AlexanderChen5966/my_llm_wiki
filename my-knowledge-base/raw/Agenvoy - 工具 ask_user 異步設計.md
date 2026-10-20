---
title: "Agenvoy - 工具 ask_user 異步設計"
source: "https://dev.to/pardnchiu/agenvoy-gong-ju-askuser-yi-bu-she-ji-3mj2"
author:
  - "[[邱敬幃 Pardn Chiu]]"
published: 2026-06-04
created: 2026-06-22
description: "Agenvoy - Make AI actually work for you — your personal AI assistant.     v0.25.13（前）Agent... Tagged with ai, agents, agenvoy, go."
tags:
  - "clippings"
status: processed
---
[Agenvoy](https://github.com/pardnchiu/Agenvoy) - Make AI actually work for you — your personal AI assistant.

---

`v0.25.13` （前）Agent 問用戶一個問題是阻塞的設計，等用戶輸入答案，然後才繼續  
為了解決大量積累的可能性，我添加了 10 分鐘的超時，雖然大多數情況下沒問題，但也讓場景被限縮在用戶必須緊盯訊息，否則有機會需要重跑  
這使用情境下，除了 `always-allow` 以外，根本無法達成長久自主運行的 Agent 這目標，因為始終需要有人盯盤  
受到 Claude 網頁版的啟發，他在 ask 的問題完是直接問題搭配回覆一起傳送給 Agent 去跑下一輪執行  
所以我在原先的 Pending 設計上添加了目標、已執行、下一步的完整紀錄，在 `ask_user` 前先行紀錄，並結束當前執行回圈，等到用戶回覆後才重新觸發流程  
這樣的設計，可以真正釋放用戶的關注，不需要擔心忘記回覆是否會需要全部重來，讓 Agent 的可用性更近一步

---

## 異步的好處？

Agent 執行一個工具：幾百毫秒  
等用戶回答一個問題：可能立即，也可能好幾小時後  
這個改動的核心是：把原本存在記憶體裡的狀態，釋放處存至磁碟上  
記憶體裡的狀態隨生命週期存在。磁碟上的狀態永遠在那裡，直到被明確刪除  
所以當前的生命週期在 ask\_user 前就會直接被中斷，就算用戶一天後才回答也沒有問題，因為回答後才讀取完整狀態紀錄，重新啟動任務

---

## ask\_user 前的狀態紀錄

中斷執行前 Agent 必須回答三個問題：目標是什麼、完成了什麼、接下來要做什麼，這樣設計有兩個目的：

- Resume 時提供 Agent 完整上下文繼續執行（包含已經查找的資料等）
- 讓 Agent 在 ask\_user 之前重新整理當前狀態，對齊需求與目標

---

## Resume 流程恢復

等用戶回答後，會先讀取狀態紀錄和用戶的答案組合成一條完整的 context，作為新的輸入交給 Agent  
不依賴對話歷史，而是完全藉由狀態紀錄的內容，告知 Agent 之前完成的步驟，用戶回答了什麼問題，請繼續下一步[Sentry](https://dev.to/sentry)Promoted

[![Sentry image](https://media2.dev.to/dynamic/image/width=775%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fi.imgur.com%2FpQHNMxL.png)](https://sentry.io/cookbook/fix-user-feedback-with-cursor-agents/?utm_source=devto&utm_medium=paid-community&utm_campaign=ai-fy27q1-aisolutions&utm_content=static-ad-cursor-cookbook-read&bb=263096)

## Tutorial: User reports a bug. A Cursor agent opens a PR. You review it.

Wire Sentry User Feedback to a Cursor Automation via MCP. When feedback gets assigned, the agent reads the issue, finds the relevant code, and drafts a fix. Step-by-step cookbook recipe, ~15-20 min setup.