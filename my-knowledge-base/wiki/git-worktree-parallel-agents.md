---
title: Git Worktree 與多 agent 並行：隔離只是三個零件之一
date: 2026-08-19
tags: [ai/agent, ai/workflow, dev/architecture]
source: https://moksaweb.com/claude-code-git-worktrees/（Claude Code 官方 --worktree flag 與實務設定）
source_2: https://moksaweb.com/git-worktree-ai-agents/（worktree vs 再 clone 六維對照）
source_3: https://growthhackers.tw/blog/git-worktree-parallel-ai-agents/（產線思維與翻車現場）
source_4: https://israynotarray.com/git/20260121/2025687513/（基本操作與 -b 差異）
type: article
status: processed
asset_value: 76
---

四來源合併。規格層的邊界（Worktree 不是 Sandbox）見 [[agent-task-system-background-scheduling]] 的 Day 16 章節；本條目是**實務操作面**。

## T（核心論點）

> **git worktree 讓同一個專案在你電腦上同時展開多個獨立工作資料夾，各在一條分支、檔案互不干擾，但共用同一份版本歷史。它是多 agent 並行的前提。**

但最值得記的是 growthhackers 那篇把它放進更大的框架：

> 🔑 **真正變的不是 AI 的智商，是時間結構：從對話框到工單。**
> **隔離（worktree）只是三個零件之一**——另外兩個是**工單佇列**（分流）與**關卡**（自動門檻）。
> 🔴 **沒有這道門檻，你的並行只是「同時生產八份垃圾」。**

## B（適用場景）

**適用：**
- 想同時跑 Feature A 測試 ＋ Bugfix B 撰寫 ＋ 文件更新，而不互相蓋檔
- 多 agent（Claude Code／Codex／Gemini）並行，每隻各自一個工作區
- 你已經被「兩個 agent 同時改同一支檔案」雷過

**不適用 / 注意：**
- 🔴 **Worktree 不是 Security Sandbox**——它只隔離 Git 工作檔，**不隔離網路、Secrets、Home Directory、Process**
- ⚠️ 適合外包給並行產線的是**機械性任務**（安全套件升級、明確的 bug 修復、重複瑣事）；**需要判斷、取捨、跟人吵架的自己來**
- ⚠️ 每個 worktree 要各自安裝依賴（`node_modules` 會重複佔空間）

## R（worktree vs 再 clone 一份）

| | 再 clone 一份 | **worktree** |
|---|---|---|
| 歷史儲存 | 完整複製一遍 | **共用同一份** |
| commit 互通 | 要 push/pull 中轉 | **立即互相看見** |
| remote 設定 | 各自維護 | 跟主倉庫共用 |
| 建立速度 | 重新複製整個專案 | **幾秒鐘** |
| 集中管理 | Git 不知道彼此存在 | `git worktree list` 一眼列出 |
| 刪除善後 | 手動刪資料夾 | `git worktree remove` 一條指令 |

（Git 2.5 起提供。白話說就是「Copy 一份專案出來，但這份跟原本共用同一個 Git 倉庫」）

### 基本操作：`-b` 的有無是關鍵

```bash
git worktree add ../wt-feature-a feature-a      # 使用「現有」分支
git worktree add -b feature-b ../wt-feature-b   # 「建立新分支」並同時建工作目錄
git worktree list                                # 檢查有哪些（含遺留的）
git worktree remove ../wt-feature-a              # 清理
```

⚠️ 建完要**進資料夾各自安裝依賴**，才能指派 agent 進駐。

## R（Claude Code 官方 `--worktree`：四個實務設定）

1. **確認 Remote HEAD 正確** — `git remote set-head origin -a` 讓本機 `origin/HEAD` 同步遠端預設分支，**確保新 worktree 從正確的 base branch 分出來**
2. 🔑 **建立 `.worktreeinclude`** — 在專案根目錄列出**要自動複製到新 worktree 的 gitignored 檔案**（例如 `.env`、`.env.local`）。這解決了「新 worktree 缺設定檔跑不起來」的實務痛點
3. **啟動** — `claude --worktree feature-a` 建立 worktree 並在其中啟動 Claude Code
4. **清理** — Session 結束時**自動清理無變更的 worktree**，有變更則詢問保留或刪除；⚠️ **定期 `git worktree list` 檢查遺留**

### 非 Git 版控也能用（企業環境相關）

SVN／Perforce／Mercurial 可透過 **`WorktreeCreate` 與 `WorktreeRemove` Hook** 自訂建立與清理邏輯——設定後 `--worktree` 的預設 Git 行為就被你的腳本取代。

🔴 **但注意：使用自訂 Hook 時 `.worktreeinclude` 不會被自動處理**，必須在 Hook 腳本內自行複製需要的設定檔。

## C（關鍵行動）

- [ ] 打算開第二個 agent 時，**第一件事就是先開 worktree**（而不是等出事才補）
- [ ] 建 `.worktreeinclude` 把 `.env` 這類 gitignored 設定檔納入自動複製
- [ ] 並行之前先確認每一格都有**自動門檻**（測試／審查／CI），否則只是同時生產垃圾

## 補充資料

### 🔴 翻車現場：八隻 agent 共用暫存區與全域鎖

growthhackers 作者用一條 AI 產線跑自己的部落格，一次讓八篇文章並行生產，踩了兩個坑——**剛好就是三零件缺其中兩個**：

**坑一：沒有隔離工位。** 八隻 agent 共用同一個暫存資料夾，各自寫暫存腳本、**檔名剛好取一樣，然後互相蓋掉**。前一隻寫好的東西後一隻直接洗掉，**兩邊都覺得自己沒問題，出來的東西莫名其妙**。

**坑二：沒有排隊機制。** 生圖工具是一把**全域鎖**，同一時間只能一隻 agent 用，後面排隊的等不到就報逾時錯誤。

> 🔑 **「我看到錯誤訊息的第一反應是『這工具壞了』，還真的去查了半天工具的問題。查到後來才發現，工具好得很，是塞車。**
> **沒有排隊機制的系統，會把塞車回報成故障——然後你就會去修一個根本沒壞的東西。」**

作者的結論：**「我不是敗在 AI 不夠聰明，我是敗在我把八條線塞進一個工位。」**

⚠️ 這個「把塞車回報成故障」的失效模式，與 [[agent-error-recovery-taxonomy]] 的 Error Taxonomy 直接相關——
**資源競爭被分類成 dependency failure，於是走了錯的 Recovery 路徑**。

### 三個零件（隔離只是中間那個）

| 零件 | 內容 |
|---|---|
| **① 工單佇列** | 先做**分流（triage）**再上線——寫一個 300 字模板（要達成什麼／背景／怎樣叫做完了），把最近十張單補成這個格式，**你會馬上看出哪幾張其實根本不該開** |
| **② 隔離** | git worktree（本條目主體）|
| **③ 關卡** | 每一格都要有**自動門檻**：測試過、審查過、CI 綠燈，不過就退回上一格重做。非工程場景則是預算上限／法務禁語／品牌語氣／必填欄位 |

🔑 **一人多機能成立，關鍵是機台會自己停下來報警。**

另一個作者很欣賞的做法：**把整條工作流寫成進版本控管的檔案**，而不是每次隨口打 prompt——
原話大意是「**你用哪一隻 agent 沒有大家想的那麼重要，重要的是工作流**」。

> **翻成生意人的話：SOP 進制度，不要進某個資深員工的腦袋。**

（這與 [[harness-engineering]] 的第三維度論、[[loopx-stateful-control-plane]] 的持久狀態是同一個主張）

### ⚠️ 四盆冷水（作者自己潑的）

1. **示範用的開源專案 `factory`（Rust／MIT）還很早期** — 查證時 91 顆星、建立才兩週，作者自述 still very early stages。🔑 **「這是一個實驗，不是一個產品。別拿去跟老闆說我們要導入這個。」** 要玩先用 GitHub Actions 做一個小的
2. **不是每張單都能進工廠** — 見 B 段的適用邊界
3. **文化沒配套會被濫用** — 🔑 **「不然你只是把『隨便做』的速度加快了八倍。」**
4. **成本不可怕但不免費** — 常駐輪詢是寫死的程式邏輯，**沒工單時不燒 token**（所以可 24 小時掛著）；但真有工單時，一個跑 35 分鐘還叫子代理的任務**token 是實實在在在燒的**

### 一個可立刻做的自我量測

> **接下來三天，每次你叫 AI 做事，在旁邊記一筆：這次等了幾分鐘、這幾分鐘你在做什麼。三天之後你會有一個很難看的數字——那個數字就是你的改善空間。**

（作者稱之為「乾等時間」。工程師版的第一步就是 `claude --worktree` 開兩個工位，一邊做功能一邊修 bug，中間去倒杯咖啡）

### 為什麼「不敢開新 Session」是真痛點

moksaweb 作者的描述很具體：**「改完 Feature A 等跑測試的時候我就在發呆，不敢開新 Session 因為 Branch 切來切去會亂掉。」**
學會之後變成 Feature A 在一個 worktree 跑測試、Bugfix B 在另一個寫 code、文件更新在第三個，**三個 Claude 同時工作、人在中間輪流看進度**。

⚠️ 但他也列出實務坑：**cleanup、merge conflict、`node_modules` 重複**。

### 📎 桌面版補充：工作階段分開 ≠ 檔案隔離（2026-10-19 併入）

> ⚠️ **時效**：本段依據 **2026-04-14** Claude Code 桌面改版介紹（aiposthub 2026-10-15 重新整理），介面可能已被 **2026-09-17 Projects 重構**（Coordinator＋Threads）取代，**不可視為最新狀態**，見 [[claude-code-projects-redesign-2026-09]]。官方連結（桌面文件 https://code.claude.com/docs/en/desktop 、改版介紹 https://claude.com/resources/articles/claude-code-desktop-redesign ）未經本次核對；作者未實測。
> 來源：https://www.aiposthub.com/claude-code-desktop-parallel-session-workflow-guide/

- 🔑 **對話分開 ≠ 程式碼隔離**：官方文件說不同工作階段可以有各自的歷史與專案位置；但若兩個工作階段**指向同一份專案資料夾**，仍會讀寫同一批檔案（作者依文件推論）——要同時改同一專案，仍得另外用 worktree 隔離，**不能只靠開新對話**。這正是本條 T 層「隔離只是三個零件之一」的桌面版實例
- 執行環境：本機、雲端、SSH（官方）；SSH 是連到遠端電腦操作，不等於把本機設定原樣搬過去
- **side chat**（快捷鍵 `Cmd + ;`）：取用主對話脈絡，但**支線內容不自動回灌主線**（官方）——概念同 CLI 的 `/btw`（見 [[claude-code-commands-reference]]）；支線確認的決定要回主線明確交代
- 支線對話（side chat）≠ Git 分支：前者整理談話、後者管理程式碼修改方向，名稱混用容易高估隔離程度（作者觀點）
- 作者觀點：**並行多開不保證省 token**（重複提供脈絡、重複調查、互相修正都會增加用量）；文中「首頁文案＋文件連結兩條工作線」的網站案例為假設示例

## 相關連結
- [[synthesis/synthesis-multi-agent]] — **本篇為其來源之一**（2026-09-03 重跑）：多 agent 並行的檔案層前提
- [[cursor-origin-git-forge]] — 更上游的一步：**為 agent 而非人類 PR 節奏設計的 git forge**，本篇是在既有 git 上並行，該篇是改造 forge 本身
- [[agent-task-system-background-scheduling]] — 🔑 **規格層的邊界**：Branch ≠ Isolation、Base Commit 要記錄、**Worktree 不是 Security Sandbox**、Share Cache 不要 Share Mutable Workspace
- [[parallel-code-multi-agent-gui]] — 讓 Claude Code／Codex／Gemini 並排各跑自己 worktree 的 GUI 工具
- [[agent-error-recovery-taxonomy]] — 「把塞車回報成故障」屬於 Error Type 分類錯誤
- [[harness-engineering]] — 工作流比 agent 選擇更重要
- [[subagent-four-boundaries]] — 🔴 Subagent ≠ Parallelism
- [[claude-code-community-workflow-2026]] — Worktree／Sub-agent 的適用邊界（社群共識）
- [[herdr-agent-terminal-multiplexer]] — 並行之後的「我看不到誰在等我」問題
- [[claude-code-cost-optimization-7-tips]] — 並行的 token 成本控制
- 📎 2026-10-19：[[claude-code-projects-redesign-2026-09]] — 桌面版補充段依據的 04-14 改版之後的 09-17 Projects 重構；[[claude-code-commands-reference]] — `/btw` 與 side chat 同概念
