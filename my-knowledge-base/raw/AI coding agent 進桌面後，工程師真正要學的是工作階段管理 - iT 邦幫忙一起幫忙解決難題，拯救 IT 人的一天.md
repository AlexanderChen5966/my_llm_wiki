---
title: "AI coding agent 進桌面後，工程師真正要學的是工作階段管理 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10400115"
author:
  - "[[iThome]]"
published:
created: 2026-06-09
description: "這一波 AI coding agent 的變化，表面上看起來是工具介面升級。 GitHub 把 Copilot agent 帶進桌面 App，OpenAI 讓 Codex 工作階段可以在手機上看 di..."
tags:
  - "clippings"
status: processed
---
3

## AI coding agent 進桌面後，工程師真正要學的是工作階段管理

- 分享至

這一波 AI coding agent 的變化，表面上看起來是工具介面升級。

GitHub 把 Copilot agent 帶進桌面 App，OpenAI 讓 Codex 工作階段可以在手機上看 diff、看測試、批准指令、改方向。對一般使用者來說，這很直覺：以前要坐在電腦前等 AI 改程式，現在可以把工作丟出去，人在外面也能盯進度。

可是對工程團隊來說，真正麻煩的地方不是「可以用手機 review」。

真正麻煩的是：當 agent 變成一個可以暫停、恢復、分支、開 PR、跑測試、修 review comment 的工作單位，我們原本那套靠人腦記狀態的開發流程，會開始撐不住。

以前我們管理的是 issue、branch、commit、PR。接下來更像是在管理一串還沒落地的 agent session。

這差很多。

## Session 會變成新的工作單位

GitHub Copilot App 的技術預覽很有意思，因為它不是單純把聊天框搬到桌面。它把 agentic work 包成一個個 session：可以從 issue、PR、prompt 或前一個 session 開始，每個 session 有自己的 branch、檔案、對話和任務狀態。

這個設計很合理。Agent 一旦開始改真實 repo，就不能只是「一段對話」。它會碰檔案，會產生 diff，會跑檢查，會卡在權限，會需要人類補充上下文，也可能做了一半發現方向錯了。

如果這些東西全塞在同一個聊天歷史裡，最後一定亂。

所以 session 其實是在替 agent work 補上工程邊界。它回答幾個很現場的問題：

- 這個 agent 現在到底在做哪件事？
- 它改了哪個 branch？
- 它的狀態是卡住、等待批准、已完成，還是準備開 PR？
- 如果我要接手 review，我要看哪份 diff、哪段輸出、哪個測試結果？
- 如果方向不對，我是叫它修，還是整個 session 作廢？

這些問題以前也存在，只是人類工程師會自己收在腦袋裡。Agent 不會。或者更準確地說，agent 可以保存上下文，但團隊不能把責任交給它的上下文。

上下文不是治理。

## 「手機可以批准」不是小功能，是新的信任邊界

Codex mobile 讓工作階段可以從 ChatGPT 手機 App 監看與操作，這件事我覺得比很多人想像中更值得警覺。

它的好處很明顯。你可以人在捷運上看 terminal output、看測試結果、看 screenshot、看 diff。指令需要批准時，不一定要等你回到電腦前。工作階段仍然跑在原本的機器或遠端環境，檔案、憑證和權限留在 host 上，手機看到的是狀態與操作入口。

這是很實用的設計。

但它也把 review 的邊界往外推了一段。

以前你要批准一個危險命令，大多會在本機終端機前面。你看得到專案，手邊有 IDE，可以 `git diff` 、可以查測試、可以想一下這個命令到底會動到哪裡。現在你可能在手機上看到一段摘要、一個 diff、一個測試通過訊息，然後系統問你要不要 approve。

這時候最危險的不是工具本身，而是人會開始把「我有看到一些證據」誤認成「我已經完成 review」。

手機很適合做 steering，不一定適合做 final judgment。

例如這些事情，用手機批准通常還算合理：

- 允許 agent 讀取某個明確檔案
- 讓它跑專案既有測試
- 接受一個只影響文件或註解的小修正
- 要它依照 review comment 重新整理 commit

但這些事情我會非常保守：

- 批准會修改資料庫、部署設定或憑證相關檔案的命令
- 合併跨模組的大型 diff
- 接受沒有足夠測試證據的重構
- 只看摘要就同意 agent 自動修掉 failing checks
- 在手機上確認涉及安全、付款、權限、資料刪除的變更

不是因為手機比較不專業，而是因為螢幕、注意力和上下文都不一樣。工程 review 很多時候不是「看見資訊」而已，是要能在腦中重建系統行為。這件事在手機上很難。

## PR 之前，先管好 session

GitHub 的流程比較靠近既有 PR 生命週期：看計畫、看 diff、留 feedback、跑 checks、開 PR，甚至讓 Agent Merge 處理 comments、修 failing checks，在條件滿足時合併。

這個方向很自然，因為 GitHub 的核心工作單位本來就是 repo 和 PR。

但我會提醒一件事：不要等到 PR 才開始治理 agent。

PR 是結果，session 是過程。很多風險在 PR 出現之前就已經發生了。

例如：

- agent 一開始選錯方向，但一路產生看似合理的 diff
- 它為了讓測試過，改了測試而不是修行為
- 它把本來應該局部的修改擴成一個重構
- 它沒有理解 issue 的非功能需求，例如效能、權限、相容性
- 它在錯的 branch 上工作，或和另一個 session 的改動重疊

如果團隊只把 agent 當成「會自己開 PR 的工具」，最後 review 會變成清垃圾。你會看到一個完整 diff，commit message 像真的，測試也看起來有跑，然後 reviewer 要從成品倒推它中間到底做了什麼。

很累，而且很容易漏。

比較好的做法是把 session 當成新的任務容器。它一開始就應該有邊界，而不是等 PR 才補救。

## 一個健康的 agent session 應該有什麼

我會把 agent session 管理拆成幾個很普通，但很有用的規則。

第一，任務要小。

不要把「重做整個 dashboard」丟給 agent，然後期待它自己會切任務。比較好的方式是把 session 切成可以 review 的單位，例如「新增匯出篩選條件」、「補上錯誤狀態 UI」、「把某個 API client 換成既有 wrapper」。

Session 越小，branch 越乾淨，review 越像工程，而不是考古。

第二，每個 session 要有明確的允許範圍。

最少要寫清楚：

- 可以修改哪些目錄或檔案
- 不要修改哪些邊界
- 既有行為有哪些不能變
- 必須跑哪些測試或檢查
- 如果測試失敗，可以嘗試修到什麼程度

這不是形式主義。Agent 最常見的問題不是完全不會做，而是做太多、猜太遠、順手改掉你沒打算動的東西。

第三，session 要留下 review 證據。

我不只想看最後 diff。我還想看它跑過哪些命令、哪些測試失敗過、怎麼修、哪裡需要人工判斷。如果工具能把 terminal output、test result、screenshot、approval history 都留在 session 裡，那 reviewer 的工作會輕很多。

第四，branch 命名要有規則。

如果一個人同時開三個 agent session，每個 session 又各自有 branch，命名混亂很快就會出事。至少要有 `agent/<date>/<topic>` 或類似規則，讓人一眼知道這個 branch 是誰開的、為了哪件事、是不是還活著。

第五，要定義什麼時候作廢 session。

這點很少人講，但很重要。Agent 做歪時，不一定要救。很多時候直接關掉 session，重新用更清楚的規格開始，成本更低。不要因為它已經改了三十個檔案，就覺得那些 diff 很可惜。

沉沒成本在 AI 開發裡會變得很貴。

## Human-in-the-loop 不是按 approve 的人

現在很多工具都會說自己支援 human-in-the-loop。這個詞聽起來很安全，好像只要中間有人類按幾次同意，流程就有把關。

我不太相信這麼簡單的版本。

真正的 human-in-the-loop 不是把人放在 approve 按鈕旁邊。它是要設計清楚：哪些決策可以交給 agent，哪些決策必須停下來等人，哪些決策可以在手機上做，哪些必須回到完整開發環境。

差別在這裡。

如果團隊沒有先定義這些界線，最後人類只會變成橡皮圖章。Agent 說測試過了，你按 approve。Agent 說可以 merge，你按 approve。Agent 說它已經修好 review comment，你再按一次 approve。

這不是人在迴圈裡。這是人在流程旁邊點按鈕。

我會建議團隊至少把 approval 分級：

- 低風險：讀檔、跑測試、文件小修，可以快速批准
- 中風險：局部程式碼修改、測試補強，需要看 diff 和測試輸出
- 高風險：部署、資料、權限、安全、跨模組重構，必須完整本機 review

手機適合處理前兩類的一部分。第三類不要逞強。

## 台灣團隊真正要補的，是工作流紀律

iThome 這幾天也把 Copilot desktop app、Codex mobile 這類新聞放在 AI 開發工具脈絡裡。這代表這件事不只是海外產品更新，而是台灣開發者很快就會碰到的日常問題。

我猜很多團隊的第一反應會是：太好了，工程師不在電腦前也能讓 AI 繼續跑。

這句話只對一半。

比較成熟的說法應該是：工程師不在電腦前時，也能處理一部分低風險的 session 管理工作。但真正的設計、審查、合併、責任歸屬，還是要回到團隊流程。

AI coding agent 進桌面、進手機後，工程師不是只要學新的 prompt。你要學的是怎麼管理多個半自主的工作階段。

誰可以開 session？一個 issue 可以開幾個 session？Session 可以活多久？什麼情況要關掉？誰負責把它轉成 PR？哪些 approval 可以遠端做？哪些一定要完整 review？測試失敗時，agent 可以自己修幾輪？如果它修到改測試，誰會發現？

這些問題聽起來很瑣碎，但它們才是 AI 開發工具能不能進 production workflow 的分界線。

工具會越來越順。真正會出事的地方，通常不是工具不夠聰明，而是團隊沒有把邊界講清楚。

把 session 當成新的 PR 前置單位來管理，你會比較不容易被 agent 的速度拖著走。

速度可以交給 agent。控制面要留在人手上。

## Source notes

- [GitHub Changelog: GitHub Copilot app is now available in technical preview](https://github.blog/changelog/2026-05-14-github-copilot-app-is-now-available-in-technical-preview/): branch-based agent sessions that can start from issues, PRs, prompts, or prior sessions.
- [OpenAI: Work with Codex from anywhere](https://openai.com/index/work-with-codex-from-anywhere/): remote supervision of active Codex work, including diffs, terminal output, screenshots, tests, approvals, plugins, and model changes while keeping files and credentials on the host environment.
- [iThome AI topic center](https://www.ithome.com.tw/ai): 2026-05-15 local coverage on Copilot desktop app and Codex mobile for Taiwanese readers.

- [留言1](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1780653847_6a229f1744d1f.jpg)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9001)

### 1 則留言

0

我的攻略法是讓 AI agent 直接進行敏捷開發! 精準地說就是每次的session就是一張Task卡片。再依照 to do, process, review, finish 來完成交付任務。人為只判定 review 的卡片是否可finish。目前訂閱 claude,gemini, 加上我本機訓練的 gemma 三個 AI agent 搭配 Jira 的工作流我還跑得滿順的。幾乎是能完成一個小公司的開發+運維的所有任務。

- 回應 1
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[ryanvale](https://ithelp.ithome.com.tw/users/20182096/profile) iT邦新手 5 級 ‧ 2026-06-04 18:04:19 [檢舉](https://ithelp.ithome.com.tw/users/login)

這個做法很接近我想說的 session 管理：把一次 agent run 當成一張 Task 卡，而不是一段聊天，真的會清楚很多。Jira 的 to do / process / review / finish 如果再把 agent 的輸出、測試結果、失敗原因留下來，review 會比單純看最後 diff 穩。

人只判斷 review 能不能 finish 這點我也很認同；我會額外把部署、憑證、資料、權限這類高風險卡片拉出更硬的 gate。你這套多 agent + Jira 的實作很有參考價值。

[登入發表回應](https://ithelp.ithome.com.tw/users/login)