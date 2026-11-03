---
title: "Google 分享三種 Markdown 分工：AGENTS.md、DESIGN.md、SKILL.md 怎麼寫？"
source: "https://www.aiposthub.com/google-markdown-ai-news-20261025/"
author:
  - "[[Philo]]"
published: 2026-10-25
created: 2026-10-30
description: "Google Cloud 分享團隊使用 Markdown 指導程式代理的分工方式。專案共通規則、介面設計與特定任務步驟各自保存，能減少同一段對話背負過多不相干要求的情況。"
tags:
  - "clippings"
status: "inbox"
---
Google Cloud 分享團隊使用 Markdown 指導程式代理的分工方式。

專案共通規則、介面設計與特定任務步驟各自保存，能減少同一段對話背負過多不相干要求的情況。

## 三種 Markdown 文件各管什麼？

Google Cloud 把代理指導文件分成專案共通、視覺設計與特定流程。Markdown 是用簡單符號寫標題與清單的文字格式，適合與程式碼一起保存版本。

AGENTS.md 提供專案背景、開發與測試等共通規則。DESIGN.md 保存介面風格與具體設計值。SKILL.md 則整理一種工作該怎麼做，例如部署或內容編修。

分開保存的用途，是讓每份文件有清楚責任。改品牌顏色時，不必同時修改測試指令，更新某個任務步驟，也不必把所有規則重新塞進對話。

![Google 分享三種 Markdown 分工官方發布畫面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/google-markdown-official-cover.jpg)

Google 分享三種 Markdown 分工官方發布畫面。圖片來源： 官方 。

## 規則如何隨任務逐步載入？

常用而簡短的專案資訊，可以放在共通規則。只有特定工作需要的細節，則等任務出現時再讀取，減少代理一直帶著不相干內容的負擔。

這種做法稱為逐步載入，也就是先知道有哪些參考方法，需要時才讀完整內容。它有助於控制上下文長度，上下文就是模型當下能參照的工作資料。

Google 的文章同時提醒頂層規則要簡潔。文件愈長並不保證代理愈遵守，重複或矛盾的要求反而可能讓它花更多力氣判斷該照哪一條。

## 團隊如何維護可執行的規格？

先把現有提示詞按用途整理，確認每條規則都回答一個具體問題。能寫成可執行指令的地方，就不要只寫「確保品質」這類難以驗收的要求。

例如測試規則應指出該用哪個指令，設計規則應留下實際字體與色彩，任務技能則交代輸入、輸出與完成條件。這樣人與代理都能檢查流程是否有做到。

文件也需要版本管理與維護者。當工具更新或規則與程式現況不一致時，要修正來源，而不是讓新對話持續沿用舊流程。外部技能檔則應先讀內容，再決定是否採用。

## Markdown 常見問題

### 這三種 Markdown 文件都要寫很長嗎？

官方主張清楚分工與簡潔共通規則，細節按需要載入，沒有要求每份文件都變成長篇手冊。

### 只改檔名就能讓代理效率提升嗎？

檔名提供入口，內容仍需具體、正確且能被工具載入。是否有效，要用實際工作結果驗證。

## 讓每份規則有責任與版本

先從一條最常被問到的專案規則整理起，再把設計與特定任務流程移到各自文件。讓規則有主人、有版本、有驗收條件，才是這套 Markdown 分工最實用的部分。

延伸閱讀： [從 Vibe Coding 進階到 Agentic Engineering：Prompt、Rules、Skills、Subagents 一次搞懂](https://www.aiposthub.com/vibe-coding-agentic-engineering-prompt-rules-skills-subagents/)

延伸閱讀： [Workbench 多代理協作教學：用 Markdown 看板，把 AI 交接和驗收寫清楚](https://www.aiposthub.com/workbench-markdown-agent-handoff-acceptance/)