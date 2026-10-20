---
title: "AI 寫程式再進化：如何完美結合 OpenSpec 與 Superpowers 打造極致開發工作流"
source: "https://vocus.cc/article/699e670efd89780001551397"
author:
  - "[[Josh]]"
published: 2026-02-25
created: 2026-05-14
description: "OpenSpec 負責規格驅動，Superpowers 專注工程紀律。本文解析如何將兩大 AI 開發工具完美結合，涵蓋需求釐清、任務拆解到 TDD 實踐，帶您打造出高效率、零幻覺的極致自動化開發工作流。"
tags:
  - "clippings"
status: processed
---
![zoomable](https://images.vocus.cc/04fedb7d-82fc-4bdc-8aee-7cc99b2e852a.png)

在 AI 輔助寫程式（AI Coding Assistants）的時代，我們常常面臨兩個痛點：一是 AI 容易忘記初衷或產生幻覺（缺乏規格共識）；二是 AI 寫出來的程式碼往往缺乏嚴謹的架構與測試（缺乏工程紀律）。

為什麼會看到廣告

<iframe frameborder="0" src="https://9fa59fb551f44f73510b0791b725d1fb.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="728" height="90" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>為了解決這些問題， ****OpenSpec**** 和 ****Superpowers**** 這兩款開源工具應運而生。它們各自解決了不同層面的問題：
- ****OpenSpec 的強項在於「規格驅動開發 (SDD)」與「專案狀態管理」。**** 它就像是一位專案經理，透過產出提案、規格、設計和任務清單等文件，確保開發者與 AI 在寫下任何一行程式碼前，對「要做什麼」有完美的共識。
- ****Superpowers 的強項在於「工程實踐與執行框架」。**** 它更像是一位嚴格的資深技術主管，強制 AI 遵守軟體工程紀律，例如真正的測試驅動開發 (TDD)、Git Worktree 隔離、系統化除錯，以及透過子代理 (Subagent) 執行微型任務。

將這兩者結合，我們就能打造出一個「從高階規格定義到高品質程式碼落地」的完美自動化工作流。以下是發揮兩者最大綜效的最佳實踐步驟：

### 階段一：需求釐清與規格制定（Superpowers 發想 ＋ OpenSpec 定義）

在動手寫程式前，釐清需求是最重要的一步。

1. ****啟動發想：**** 當你有一個新功能想法時，先利用 Superpowers 的
	```
	brainstorming
	```
	（腦力激盪）技能。AI 會透過蘇格拉底式的提問，幫你把模糊的想法精煉成具體的架構與設計方案。
2. ****建立規格：**** 想法確立後，呼叫 OpenSpec 的指令
	```
	/opsx:new <功能名稱>
	```
	來初始化專案資料夾。
3. ****生成文件：**** 使用
	```
	/opsx:ff
	```
	(fast-forward)，讓 AI 根據剛剛腦力激盪的結論，自動生成 OpenSpec 所需的標準文件（包含
	```
	proposal.md
	```
	,
	```
	specs/
	```
	,
	```
	design.md
	```
	,
	```
	tasks.md
	```
	）。這能將對話紀錄固化為清晰的「單一真相來源」。

### 階段二：建立隔離的開發環境（Superpowers 負責）

在進入實作前，保持主幹環境的整潔至關重要。你可以觸發 Superpowers 的

```
using-git-worktrees
```
技能，讓 AI 為這個新功能建立一個獨立的開發分支和 Worktree，確保主分支不受影響，並確認測試基準線是乾淨的。

### 階段三：任務拆解與細化（結合兩者優勢）

雖然 OpenSpec 已經產出了高階的任務清單 (

```
tasks.md
```
)，但我們可以更進一步。指示 AI 使用 Superpowers 的
```
writing-plans
```
技能，將 OpenSpec 的任務清單進一步拆解為「每次只需 2 到 5 分鐘」即可完成的微型任務。確保每個微型任務都包含具體的檔案路徑、測試步驟與驗證條件。

### 階段四：紀律化的程式碼實作（Superpowers 執行，參照 OpenSpec 規格）

這是整個工作流的核心：

- ****拒絕一次性生成：**** 不要讓 AI 一次寫完所有程式碼，而是利用 Superpowers 的
	```
	subagent-driven-development
	```
	或
	```
	executing-plans
	```
	來逐一執行計畫。
- ****嚴守 TDD 規範：**** 在實作過程中，強制 AI 遵守 Superpowers 的
	```
	test-driven-development
	```
	(TDD) 技能。AI 必須先寫一個會失敗的測試（Red）、寫出能通過的最少程式碼（Green），最後進行重構（Refactor）。
- ****唯一依據：**** 此時 AI 實作的依據，必須完全來自於 OpenSpec 在階段一所定義的設計文件與規格，避免開發走偏。

### 階段五：審查與收尾（Superpowers 驗證，OpenSpec 歸檔）

1. ****程式碼審查：**** 任務完成後，使用 Superpowers 的
	```
	requesting-code-review
	```
	技能，讓 AI 自我檢查或輔助你檢查程式碼是否完全符合 OpenSpec 最初定義的規格。
2. ****分支合併：**** 確認無誤後，使用 Superpowers 的
	```
	finishing-a-development-branch
	```
	處理合併或發起 PR，並清理 Git Worktree 環境。
3. ****規格歸檔：**** 最後，使用 OpenSpec 的
	```
	/opsx:archive
	```
	指令，將這次功能的規格文件歸檔。這會自動更新專案的全局規格，為下一次的新功能開發做好準備。

### 總結設定與實用建議

要完美運作這套流程，建議在支援強大外掛系統的工具（如 Claude Code 或 Cursor）上同時載入這兩個框架。

此外， ****「上下文衛生 (Context Hygiene)」**** 是結合兩者的最大好處。當 OpenSpec 幫你把需求寫成實體文件後，你可以直接清空當前的 AI 聊天紀錄（清除 Context）；接著重新讓 AI 讀取 OpenSpec 文件，再利用 Superpowers 的流程去執行。

這種「文件化記憶」搭配「清空無效對話」的做法，能大幅降低大語言模型產生幻覺或忘記初衷的機率，徹底釋放 AI 輔助開發的最強效能。