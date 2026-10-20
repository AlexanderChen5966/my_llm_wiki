---
title: Codex 提示詞四要素框架 + AGENTS.md：讓它一次做對的溝通工作流
date: 2026-07-17
tags: [ai/agent, ai/prompt-engineering]
source: https://maplefeather.com/article/codex-prompt-tips-agents-md-2026
type: article
status: processed
asset_value: 68
---

夜羽凌（每日重度使用 Codex）整理的 Codex 溝通工作流。核心心法：「給脈絡＞改措辭」——Codex 照的是「它以為你要的」而非「你真正要的」，最傷的是「流暢地犯錯」（用很有自信的口吻說改了其實沒改）。解法不是加重語氣，是補齊脈絡、立規則檔、給強驗證。

## T（核心論點）
Codex 做錯八成不是它笨，是它在「猜」——你沒給夠脈絡它就腦補一個版本去做。修好提示詞的第一步不是把語氣加重，而是把它需要知道的東西補齊、把重複糾正變成 AGENTS.md 規則、給它能自我驗證的測試。把 Codex 當「能力很強但需要結構與邊界的資淺隊友」，而非丟願望就變出成果的許願池。

## B（適用場景）
- **適用**：每天寫程式、有可丟著非同步跑的長任務、願意花一次時間立規則檔的使用者；會動到多檔、容易做歪的任務
- **不適用**：偶爾問問題改幾行字（裝聊天介面就夠）；一次性小改動硬套四要素反而浪費；高度依賴即時看畫面微調的高互動場景（Claude Code 較順手）

## R（四要素框架 + 工作流）
**官方四要素提示框架**（一個 prompt 補齊這四塊，重講機率大降）：

| 要素 | 回答什麼 | 塞什麼 |
| --- | --- | --- |
| 目標（Goal） | 要改什麼、做什麼 | 一句話講清楚要的結果 |
| 脈絡（Context） | 哪些檔案/資料夾/錯誤訊息相關 | 檔案路徑、貼上實際報錯、參考範例 |
| 限制（Constraints） | 有什麼規範/架構/安全要求 | 不能碰哪些檔、沿用哪個慣例、不要引入新套件 |
| 完成定義（Done when） | 怎樣才算做完 | 要過哪些測試、列出實際動過哪些檔 |

> 對付「說改了其實沒改」的護身符：限制裡明講「不要碰什麼」+ 完成定義裡要求「列出動過哪些檔」。

**兩階段開關（不矛盾，是不同階段）：**
- **探索期用 `/plan`**（Shift+Tab 切計畫模式）：先讀檔收集脈絡、出書面計畫、不動原始碼，你核可再執行；需求模糊時可叫它「先訪談我把需求問清楚」
- **執行期掛「做到底」指令**（保留英文照貼辨識度最高）：`Persist until the task is fully handled end-to-end...` / `Bias to action: default to implementing...` / `deliver working code, not just a plan`

**進階工作流：**
- **Git worktree 開平行 agent**：從同 repo 切多個工作目錄各開 session 平行跑；但 worktree 只隔離「檔案」，不隔離 port/資料庫/服務
- **`/review` 內建 code review**：針對未提交變更/某 commit/base branch 自審一輪
- **`--image` / `-i` 丟截圖除錯**、`@` 帶檔案進對話、compact 壓縮長對話

**強驗證是地基**：給強驗證（跑得起來的測試/lint/build）每做錯一步就被擋下逼它自修；給弱驗證它會把小錯放大成災難（錯誤放大器）。要它「別改測試來配合改動」、要它交代怎麼驗證的。

## C（關鍵行動）
- [ ] 把四要素做成罐頭範本，下指令前 30 秒填一填
- [ ] 複雜任務先 `/plan` 核可再執行，放手跑前先設好 deny 邊界（不能碰的路徑）
- [ ] 每個重複糾正它的地方寫成 AGENTS.md 一條規則（詳見 [[agents-md-guide]]）
- [ ] 放手「做到底」很吃 token，配合成本意識（見 [[github-copilot-token-reduction]]）

## 相關連結
- [[agents-md-guide]] — AGENTS.md 完整指南（本篇把 AGENTS.md 當「投報率最高的一次性投資」，寫法細節見該條目）
- [[codex-advanced-tips-11]] — Codex 11 個進階技巧（Durable Threads / Goals / Steering / Queuing），與本篇工作流互補
- [[gpt-5-6-prompting-4-elements-framework]] — GPT-5.6 官方 4 要素框架（目標/背景/輸出/邊界），同源心法用於一般對話
- [[chatgpt-prompt-4-techniques]] — Task/Context/Output/Iteration，四要素的一般對話版
- [[codex-best-practices-guide]] — Codex 最佳實踐長文（Agent vs AI 對話本質、模型選擇、harness）
- [[claude-code-agents-md-loading-rules]] — 同一份 `AGENTS.md` 在 Claude Code 側的讀取判定（Codex 原生讀，Claude Code 自 2.1.277 起才在沒有 `CLAUDE.md` 時讀）
