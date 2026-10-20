---
title: "解析 get-shit-done：專為 Claude Code 打造的 Meta-Prompting 與 Agent 開發框架"
source: "https://www.wenaidev.com/blog/zh-TW/get-shit-done-claude-code-system"
author:
  - "[[po-wen]]"
published: 2026-03-23
created: 2026-06-22
description: "介紹 GitHub 上熱門的 get-shit-done 專案，深入拆解其 Context Engineering、Multi-Agent 架構，以及探討其 Agent Prompts 中的「反企業化」與「智商衰退曲線」等高階提示詞技巧。"
tags:
  - "clippings"
status: processed
---
## Table of Contents

> **get-shit-done（GSD）是什麼？** 一套專為 Claude Code 設計的開源 Meta-Prompting 框架，用 Context Engineering 和 Multi-Agent 架構解決 AI 在長任務中上下文腐爛的問題。

get-shit-done (GSD) 目前 40k stars（gsd-build/get-shit-don)（2025 年 12 月底才出來，幾個月就衝到這個量），核心是 輕量 spec-driven orchestrator。 當我們使用 Claude Code (或任何 AI Coding Agent) 開發中大型專案時，最常遇到的痛點絕對是 **Context Window (上下文視窗) 污染** 。

隨著對話變長，AI 開始忘記最前面的需求，甚至開始胡亂猜測你的意圖，最後給出一堆無法編譯的程式碼。

為了解決這個問題，GitHub 上的熱門開源專案 [get-shit-done (GSD)](https://github.com/gsd-build/get-shit-done) 誕生了。這是一個輕量級但功能強大的 **Meta-Prompting** 與 **Spec-Driven Development** 系統。你可以把它想像成一個「AI 開發流水線的主管」，它不親自寫扣，而是透過指揮多個小 Agent 來完成工作。

這篇文章我們就來詳細拆解 GSD 的運作邏輯、Agent 架構，並 **深入潛入它原始碼中的 Agent Prompts** ，看看它是如何寫出令人驚豔的提示詞。

## GSD 的核心運作邏輯

GSD 的核心哲學是： **保持 Claude 的 Context 絕對純淨** 。

它不依賴一個無限長的對話框，而是將開發流程強制拆分為 6 個獨立階段。在每個階段，GSD 都會開啟全新的 Context (上下文)，並且透過嚴謹的 Markdown 檔案來傳遞狀態，而不是依賴 AI 自己的記憶。

![GSD 六階段運作流程圖](https://www.wenaidev.com/static/images/blog%E4%B8%8A%E5%82%B3%E8%B3%87%E6%96%99/get-shit-done-claude-code-system/workflow.svg)

圖說：GSD 的六階段 Specs-Driven 開發流程，每一步驟都會產出獨立的 Markdown 藍圖。

以下是 GSD 強制執行的 6 大開發步驟：

- **1\. Initialize (初始化階段)：** 使用 `/gsd:new-project` 命令。系統會不斷詢問釐清你的需求，挖掘邊界條件，最後產出包含所有階段的 `ROADMAP.md` 。
- **2\. Discuss (討論階段)：** 使用 `/gsd:discuss-phase` 命令。AI 不會急著寫扣，而是先挑出這個階段模糊的地方 (例如：UI 該長怎樣？API 錯誤要怎麼回傳？)。討論結果會打包成專屬的 `CONTEXT.md` 。
- **3\. Plan (計畫階段)：** 使用 `/gsd:plan-phase` 命令。AI 開始調查技術文件，並生成極細顆粒度 (Atomic) 的 XML 任務清單 (`PLAN.md`)。
- **4\. Execute (執行階段)：** 使用 `/gsd:execute-phase` 命令。這是最精彩的地方，GSD 把任務分成多個 Wave，沒有相依性的任務會 **平行開啟新的 Context** 去寫 Code，寫完立刻進行 **Atomic Git Commit** (每個任務獨立提交)。
- **5\. Verify (驗證階段)：** 使用 `/gsd:verify-work` 命令。除了跑測試，GSD 還會帶領你進行 UAT (User Acceptance Testing)。你只要告訴它哪裡不順，它就會自動產出修復計畫 (Fix Plan)。
- **6\. Ship (發布與重複)：** 確認無誤後，輸入 `/gsd:ship` 或是進入下一個 Milestone。

## Multi-Agent Orchestration 架構解析

GSD 之所以能做到平行多工，是因為它內建了一套多代理人 (Multi-Agent) 架構。

![GSD Multi-Agent 架構圖](https://www.wenaidev.com/static/images/blog%E4%B8%8A%E5%82%B3%E8%B3%87%E6%96%99/get-shit-done-claude-code-system/agents.svg)

圖說：GSD 的 Agent 協作圖，CLI Orchestrator 作為指揮官，協調不同的 Sub-Agent 完成任務。

在系統設定中，你可以清楚看到這幾種專職的 Agent：

- **CLI Orchestrator (皮包骨指揮官)：** 作為整個系統的大腦，但他自己不寫半行基礎邏輯。他的唯一工作是觸發 Lifecycle Hooks (如 pre-plan, post-execute)，呼叫其他 Agent，然後彙整 Markdown 結果。
- **Researcher Agent (研究員)：** 負責去讀開源庫的文件、Survey 網路上有哪些解決方案。
- **Planner Agent (計畫專員)：** 負責把需求轉化成 **XML 格式的 Prompt** 。因為 XML 標籤能讓 Claude 最精準地解析結構。
- **Executor Agent (執行員)：** 真正的寫扣勞工。他被限制只專注於眼前的單一任務清單。
- **Verifier Agent (含 UI Auditor)：** 負責排查錯誤。除了跑測試外，還能對 UI 進行 6 大面向的審核。

---

## 值得學習的系統設計思路

看完架構之後，你可能會想：「這跟我自己開一個 Terminal 叫 Claude 寫扣有什麼差別？」差別在於 GSD 用了幾個非常聰明的工程設計，解決了 AI 寫扣最根本的瓶頸。

### 1\. 多代理並行 + 波次執行 (Wave-Based Parallel Execution)

這是 GSD 速度上最大的殺手鐗。

傳統做法是：Plan 1 完成 → Plan 2 開始 → Plan 3 開始… 這是單線程的。GSD 不是這樣，它在 Plan 階段就已經用 YAML `depends_on` 欄位算好了每個 Plan 之間的相依關係，並把沒有相依的 Plan 分到同一個 **Wave (波次)** ，然後 **同時開出多個 Sub-Agent 去平行執行** 。

![GSD Wave-Based 平行執行模型](https://www.wenaidev.com/static/images/blog%E4%B8%8A%E5%82%B3%E8%B3%87%E6%96%99/get-shit-done-claude-code-system/wave-execution.svg)

圖說：Wave 1 的三個 Plan 因為互不相依，所以 GSD 同時 Spawn 三個獨立的 Executor Agent 平行跑。Wave 2 等 Wave 1 全部完成才開始。

這裡最關鍵的設計是： **每個 Sub-Agent 都在全新的 200k Token Context 裡面工作** 。它不會讀到其他 Plan 的垃圾訊息。Orchestrator (主 Session) 只負責派工與彙整結果，所以主 Session 的 Context 使用率始終維持在 30-40%，不會降智。

對我們來說的啟示 => 「Vertical Slice (垂直切片)」的工作拆法天然就適合平行化。如果你把任務拆成「所有 Model → 所有 API → 所有 UI」(水平分層)，那 Wave 2 = 所有 API，必須等 Wave 1 = 所有 Model 完成才能開始，根本並行不起來。但如果你拆成「User 功能端到端」、「Product 功能端到端」，兩個人（或兩個 Agent）就可以同時跑。

### 2\. Checker → Revise 自動修復迴圈

大部分的 AI Coding 工具流程長這樣： **Generate → 交差 → Bug → 人工 Debug** 。GSD 不一樣，它在 Execute 之後強制插入了一層自動化的驗證與修復迴圈。

![GSD Checker-Revise 自動修復迴圈](https://www.wenaidev.com/static/images/blog%E4%B8%8A%E5%82%B3%E8%B3%87%E6%96%99/get-shit-done-claude-code-system/checker-revise-loop.svg)

圖說：Verifier 產出結構化 YAML Gaps → Planner 針對 Gaps 精準產出 Fix Plan → Executor 自動修復 → 再跑一次 Verify，形成閉環。

這個迴圈的厲害之處在於：

- **Verifier 不只是「跑 Test」** ：它用了四級深度查核。Level 1 看檔案有沒有建出來、Level 2 看是不是只有 10 行的空殼 (Stub)、Level 3 用 `grep` 查 Import 跟調用有沒有接上、Level 4 甚至會回溯資料來源，看你的 API 是不是回傳了假資料 (`return json([])`)。
- **失敗不用人工 Debug** ：Verifier 把問題打包成結構化的 YAML `gaps` ，自動送回給 Planner。Planner 讀完 gaps 後，產出精準的 Fix Plan (不是重新規劃整個 Phase，只修壞掉的部分)。然後 Executor 自動重新執行。
- **人類只做最後一哩路** ：只有當所有自動化查核都通過之後，系統才會生成一份 UAT 清單問你：「這個按鈕按下去動畫順不順？」、「OAuth 跳轉有沒有正確帶入帳號？」。你只需要回答是或否，如果說「不」，系統又會自動把你的反饋轉化成 Gaps 進入下一輪修復迴圈。

### 3\. Context 保鮮：每個任務都是一張白紙

這是 GSD 最核心、也最容易被忽略的設計哲學。

在 GSD 的設計中，Planner Agent 被明確告知了 Claude 的「智商衰退曲線」：0-30% Context 是巔峰狀態、50% 以上開始偷懶、70% 以上就進入敷衍模式。所以 Planner 被強制要求： **每個 Plan 必須在 50% Context 以內完成** ，超過就拆。

這也是為什麼 GSD 中每個 Plan 最多只有 2-3 個 Task。不是因為不能放更多，而是因為放太多 Claude 就會開始降智。整個框架的核心思路就是： **用「多個短對話」取代「一個長對話」** ，每一輪都讓 AI 保持在最佳狀態。

### 4\. 嚴謹且自動化的需求覆蓋率追蹤

GSD 的 Roadmapper Agent 有一條鐵律： **100% 的 v1 需求必須對應到至少一個 Phase，不能有孤兒需求、不能有重複指派。**

它用 Goal-Backward (以終為始) 的方法來拆解需求 => 不是問「這一階段要做什麼？」，而是問「當這一階段完成時，使用者能觀察到什麼具體成果？」。每個成果 (Observable Truth) 都必須反向連結到具體的需求 ID。如果有落差 (Gap)，系統會標紅要求處理。

這個機制讓 GSD 從根本上避免了 AI 最常犯的毛病： **寫了一堆看起來很忙但沒有對到需求的程式碼** 。

## 總結

**get-shit-done** 的強大，不在於它多會寫提示詞，而是它把軟體工程師的 Best Practice 變成了 AI 能自動遵守的機制。

波次平行執行解決了速度、四級驗證迴圈解決了品質、Context 保鮮解決了降智、需求覆蓋率追蹤解決了完整度。這幾個機制環環相扣，構成了一套讓 Claude Code 能穩定交付中大型專案的紀律系統。

如果你正打算用 Claude Code 開發規模稍大的 Side Project，GSD 的設計思路絕對值得參考。就算你不直接用它，光是學會「Wave 拆法」跟「Checker → Revise 閉環」，就能大幅提升你用任何 AI 工具的效率。

## 常見問題

### GSD 怎麼安裝？

用 npm 安裝： `npx get-shit-done-cc@latest` ，安裝時選擇你的 runtime（Claude Code、Cursor、Windsurf、Gemini CLI 等）和作用範圍（global 或 local），完成後用 `/gsd:help` 確認。

### GSD 的 Wave 執行和自己多開幾個 Claude 視窗有什麼差別？

GSD 在 Plan 階段就根據 `depends_on` 自動算好哪些任務互不相依，然後同時派出 Sub-Agent 平行執行，每個 Sub-Agent 都有全新的 200k token Context。你自己多開視窗是手動判斷、手動協調，容易衝突或遺漏相依性。

### 所有規模的專案都適合用 GSD 嗎？

不一定。GSD 設計給獨立開發者和小團隊的中大型任務。如果只是改個小 bug 或加一個函式，直接用 `/gsd:quick` 或直接叫 Claude Code 處理就夠了，不用走完整流程。

## 相關文章

[企業 AI 客服實戰：從掃描紙本單據到自動回答的完整記錄（2026）](https://www.wenaidev.com/blog/zh-TW/enterprise-ai-chatbot-real-project-2026)

企業 AI 客服系統實戰踩坑記錄 | 飯店宴會部用 AI 自動辨識紙本訂單，搭配混合搜尋做自然語言查詢。從開源模型冷啟動 73 秒的教訓到系統優化省下一半費用，...

2026-06-04

[Claude Code 自動驗證與自主開發：別再盯著 Agent 看了（2026）](https://www.wenaidev.com/blog/zh-TW/claude-code-stop-babysitting-agents)

Claude Code 自動驗證完整教學 | Anthropic 官方三大自主開發模式 Verification、Parallelize、Background...

2026-06-02

[Anthropic Harness 設計：讓 AI Agent 連跑 6 小時不失控的架構拆解（2026）](https://www.wenaidev.com/blog/zh-TW/anthropic-harness-long-running-agent)

Anthropic 官方拆解長時間 AI Agent 的兩大失敗模式（context anxiety 與自我評價偏差），並用三代理架構 Planner / Ge...

2026-05-31