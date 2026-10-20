---
title: "實用的 12 個 Claude Code 技巧 - Vibe Coding 必備！"
source: "https://www.thisweb.dev/articles/12-claude-code-command"
author:
  - "[[ThisWeb 請網這邊走]]"
published: 2026-04-27
created: 2026-08-05
description: "身為工程師，我平常用 Claude Code 開發，除了文字溝通之外，也經常用快捷鍵或指令來提高工作效率，這篇文和你分享我最常用的 12 個指令，下面附上一份完整速查表，之後有用到 Claude Code 就可以快速拿出來對照～！"
tags:
  - "clippings"
status: processed
---
![實用的 12 個 Claude Code 技巧 - Vibe Coding 必備！](https://www.thisweb.dev/images/articles/12-claude-code-command/cover.png)

身為工程師，我平常用 Claude Code 開發，除了文字溝通之外，也經常用快捷鍵或指令來提高工作效率

這篇文和你分享我最常用的 12 個指令，下面附上一份完整速查表，之後有用到 Claude Code 就可以快速拿出來對照～！

### 1\. /btw

`/btw` 的用途是在「不中斷主流程的前提下插入額外問題」。

當你正在執行一個主要任務（例如讓 AI 改一段程式碼），中途突然想到另一個小問題，如果直接問，很容易讓模型偏離原本的上下文。

此時就能用 `/btw` 來快速問問題， `/btw` 的回覆不會被加入對話歷史，它只回覆一次，沒有後續回合，適合快速確認一個小細節。

### 2\. /compact

`/compact` 用來壓縮對話內容，釋放上下文空間（context window）。

當一個對話進行很長，模型的可用 context 快用完時，繼續往下操作會導致早期的重要資訊被截斷，甚至影響模型的判斷品質。 `/compact` 會讓模型將當前整個對話歷史濃縮成一份摘要，保留關鍵決策與任務進度，再以摘要取代原始對話繼續作業。

你也可以在指令後加上自訂說明，告訴模型哪些資訊需要優先保留，例如 `/compact 保留所有已決定的資料庫 schema 設計` 。這個指令對長達數小時的大型重構任務特別關鍵。

### 3\. Shift + Tab（Plan Mode）

`Shift + Tab` 是一個操作層級的切換工具，用來在三種權限模式之間循環切換：

| 模式 | 行為 |
| --- | --- |
| default | 只能讀取，每個操作都需詢問確認 |
| acceptEdits | 可自動讀取與編輯檔案，減少確認中斷 |
| plan | 只能讀取（如 default），但不會嘗試執行任何動作 |

進入 plan mode 後，Claude 只會讀取與分析 codebase，不編寫任何程式碼，讓你可以安全地探索現有架構、確認改動範圍，並與 Claude 來回調整計畫方向，確認後再切換到執行模式。

### 4\. /rewind

`/rewind` 是用來回到先前某個對話的指令。

當你發現模型的輸出開始偏離、或某次修改讓結果變差，可以用 `/rewind` 直接回退到某一輪對話，而不需要重新描述整個問題或手動還原程式碼。

連按兩次 `Esc` 也可以做到一樣的效果。

### 5\. Option + P / Alt + P（/model、/effort）

`Option + P` （macOS）或 `Alt + P` （Windows/Linux）用來快速切換模型，且不會清空目前輸入內容。效果等同於輸入 `/model` 指令

這讓你可以在同一段任務中，根據需求隨時切換更強或更快的模型。

如果只想單純調整思考深度，可以輸入 `/effort` 指令調整。

### 6.! 前綴（直接執行 Bash 指令）

在提示輸入前加上 `!`，可以跳過 Claude、直接在 terminal 執行一條 shell 指令，並將結果帶入對話上下文。

例如輸入 `! git log -10` ，就能直接拿到最近 10 筆 commit 的結果，並讓 Claude 以看到指令的輸出。

### 7\. /simplify

`/simplify` 並不是單純讓程式碼變短，而是讓結構更清晰。

它會讓 Claude 平行審查最近修改過的檔案，嘗試消除重複邏輯、減少不必要的抽象，或重新組織程式碼的層次，然後直接修正發現的問題。適合在功能已完成之後使用，作為一次「收尾整理」，而不是在一開始就用來生成程式碼。

你也可以指定聚焦方向，例如 `/simplify 目前修改的程式碼` ，讓審查範圍更精準。

他會是先用 `git diff` 找出你改了哪些地方，再平行啟動三個獨立的 subagent，分別負責審查程式碼複用性、品質與效率三個面向，完成後彙整結果並直接套用修正。

三個 agent 各自接收完整的 diff，但只專注於自己負責的那一類問題，不會互相干擾，也不會因為任務太雜而稀釋掉分析深度。

### 8\. /fork、/resume、/rename

`/fork` 的概念是 **保留當前狀態，開啟另一個平行的分支** 。

當你在思考不同解法或架構時，可以用 `/fork` 保留原方案，再開一條保有目前狀態的新對話，測試其他可能性，而不是直接覆蓋原本的結果。

用 `/fork` 開出分支後，就可以在不影響主線的情況下，讓兩個方向各自往下走一段，再比較結果。

而使用 `/resume` 指令可以在這些不同的對話分支之間切換，

如果想幫不同對話命名，也可以使用 `/rename` 避免混淆。

### 9\. /memory

`/memory` 用來管理 Claude Code 的記憶機制，讓 Claude 知道這個專案的背景、慣例、限制與偏好，使用後會列出目前對話載入的所有記憶檔案，包括

1. 使用者或專案的 `CLAUDE.md`
2. Rules 檔案
3. auto memory 的筆記

你可以在這個指令裡面切換 auto memory 的開關

#### auto memory 是什麼？

Auto memory 是 Claude 自己寫給自己的筆記，會跨對話保存。

Claude 會自己判斷，目前這個資訊在未來對話中是否有用，來決定要不要記錄，不是每次都存。

例如當你說，「 記住：撰寫文章時，遠永不要使用 Emoji」

Claude 就會自動寫入 auto memory 當中，之後執行任務時就會遵循這個規範。

### 10\. /remote-control

`/remote-control` （縮寫 `/rc` ）用來從 claude.ai 網頁端遠端控制你本機的 Claude Code terminal session。

它讓同一個工作流程不被綁在單一裝置上，適合長時間任務或需要切換工作環境的情境。例如你在辦公室用桌機跑了一個長達數小時的重構任務，離開後換到筆電或手機，仍可透過 claude.ai 繼續監控進度、送出新的指令，或在任務完成後審查結果。

互補的指令是 `/teleport` （縮寫 `/tp` ），方向相反：它把 claude.ai 網頁端正在進行的對話「拉回」到本機 terminal，讓你在需要更強控制能力時切換到本機環境繼續作業。

### 11\. Ctrl+G

按 `Ctrl+G` 可以直接開啟外部編輯器，讓你更方便去下 prompt。

我們也可以利用 `/config` 打開設定面板，搜尋 `Show last response in external editor`

把它勾選後，當我們用 `Ctrl+G` 打開外部編輯器後，Claude 會自動把上一輪的對話也放到編輯器裡面，讓你更好的知道目前 Claude 的回覆。

### 12\. /recap

`/recap` 的用途是在你回到一個暫停的 session 時，快速生成一份上下文摘要，讓你立刻想起「上次做到哪裡」。

當任務跨越數小時甚至隔天繼續，光靠記憶很難精確回想當時的決策與進度。 `/recap` 會掃描目前的對話歷史，生成一份精簡的狀態描述，讓你不需要從頭捲回對話、也不需要重新問 Claude「我們剛才在做什麼」。

你也可以在 `/config` 設定中開啟自動 recap，讓 Claude 在你回到 session 時自動執行這個摘要，而不是每次都要手動輸入 `/recap` 。

### 總結

這 12 個指令代表了 Claude Code 在不同層次的控制能力：有讓你管理對話狀態的（ `/btw` 、 `/rewind` 、 `/fork` ）、有讓你調配模型資源的（ `Option+P` 、 `/model` ）、有讓你管理 context 的（ `/compact` 、 `/memory` ）、也有讓你更有效率使用的（`!` 前綴、 `Shift+Tab` 、 `/simplify` 、 `/remote-control` 、 `Ctrl+G` ）。

真正理解這些工具的設計意圖，我們就能快速在對的時機用對的工具，大大提升工作效率。

## 下一篇看什麼？

01.

### [6 大 Agent 模式：打造 AI 系統必懂的架構設計](https://www.thisweb.dev/articles/6-agent-modes)

02.

### [AI Agent 是什麼？2026 零基礎完整懶人包](https://www.thisweb.dev/articles/what-is-ai-agent)

03.