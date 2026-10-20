---
title: "Claude Code Mods 是什麼？Anthropic 讓你自訂 AI 工作流程，連介面都能改"
source: "https://www.aiposthub.com/claude-code-mods-custom-workflow/"
author:
  - "[[Philo]]"
published: 2026-10-03
created: 2026-10-06
description: "Claude Code Mods 是什麼？了解 Anthropic 新推出的自訂功能、與 Skills 和 Hooks 的差別、三個官方示範，以及安裝前需要理解的權限範圍。"
tags:
  - "clippings"
status: processed
---
Claude Code Mods 是用程式自訂 Claude Code 行為與介面的擴充功能。它能在 AI 使用工具時加入規則，也能顯示額外資訊，讓這套程式開發助手更貼近你的工作方式。

2026 年 10 月 1 日，Anthropic 在 [官方公告](https://claude.com/blog/claude-code-mods?ref=aiposthub.com) 推出 Mods。過去，使用者可以交代專案規則、提供操作指引，或設定特定事件發生時要執行的工作。Mods 進一步開放了工作過程本身，從送出指令到畫面呈現，都有更多自訂空間。

這次更新值得關注的地方，是團隊可以把每天反覆使用的流程做進工具裡。以下從 Mods 的運作方式、與 Skills 的差別，以及三個官方示範，說明它適合解決什麼問題。

## Claude Code Mods 如何運作？把規則放進 AI 的執行過程

Claude Code 每次準備使用工具、接收指令或繪製介面時，都會產生對應事件。Mod 可以接收事件，再決定要觀察、修改，或直接接手處理。依 [官方 Mods 文件](https://code.claude.com/docs/en/plugins/mods/overview?ref=aiposthub.com) ，這些程式可以用 JavaScript 或 TypeScript 撰寫，兩者都是開發軟體常用的程式語言。

例如，AI 正要執行一個指令，Mod 可以先檢查內容，讓符合條件的操作繼續，或攔下來交給使用者選擇。若需求是改善資訊呈現，Mod 也能增加側邊面板、按鈕或輸入欄位，將原本要另開視窗查看的資訊放進工作畫面。

Mods 使用 Plugin，也就是可安裝與分享的外掛套件來包裝。因此，使用者取得的是一套能持續運作的功能，團隊也有機會把相同工具分發給成員。我的判斷是，這對已有固定開發流程的人比較有價值，因為它能把反覆提醒的事項變成工具的固定行為。

## Mods、Skills 與 Hooks 有什麼差別？先看你要改哪一部分

三者都能自訂 Claude Code，但處理的問題不同。Skills 是交給 AI 閱讀的可重複使用指引，例如審查程式碼時要檢查哪些項目。依 [Skills 官方文件](https://code.claude.com/docs/en/skills?ref=aiposthub.com) ，使用者可以把這些指引放進 `SKILL.md` ，讓 Claude 在相關任務中載入，或透過指令主動使用。

Hooks 是事件發生時執行的自動化程序。 [Hooks 官方文件](https://code.claude.com/docs/en/hooks?ref=aiposthub.com) 說明了這類設定。本文把設定檔裡的傳統 Hooks 與 Mods 分開說明，因為 Mods 底層的事件處理函式也被官方稱為 hooks。傳統 Hooks 適合把既有腳本接到開發流程，Mods 則更適合需要持續互動或自訂介面的功能。

| 你的需求 | 優先考慮的方式 | 具體例子 |
| --- | --- | --- |
| 讓 AI 遵循固定操作方法 | Skills | 按照團隊規則審查程式碼 |
| 在特定事件執行既有程序 | 設定檔 Hooks | 檔案修改後執行格式檢查 |
| 加入互動介面或接手事件 | Mods | 顯示檢查面板，讓使用者選擇是否繼續 |

這個區分也決定了導入順序。若你只是一直重複貼上同一段指示，先做成 Skill 就有幫助。當文字指引無法解決「我要在哪裡看資訊、何時介入操作」的問題，Mods 才有比較明確的用途。

## 三個官方示範，讓 Mods 的用途變得具體

官方的 [Mods 入門指南](https://claude.dev/blog/getting-started-with-claude-code-mods/?ref=aiposthub.com) 介紹了 Token Weather、Blast Radius 與 Replay Theater。三者分別處理資訊顯示、操作確認與修改回顧，也反映了 Mods 能介入的不同環節。

<video controls="" width="1920" height="1080" src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/10/claude-code-mods-desktop-official.mp4" aria-label="Claude Code Mods 官方桌面版示範影片"></video>

官方桌面版示範依序展示 Token Weather、Blast Radius 與 Replay Theater。來源： 官方入門指南 。

### Token Weather：把對話容量變成看得懂的狀態

Token Weather 會在輸入框上方顯示上下文視窗的使用狀況。上下文視窗是模型單次處理資訊的容量，會包含對話、讀取的內容與其他工作資料。這個示範用天氣圖示與使用比例，讓使用者知道資訊量正在增加。

對長時間使用 AI 處理專案的人來說，這類顯示可以提供整理任務的時機。它呈現的是容量資訊，使用者仍需自己判斷哪些內容值得保留。

### Blast Radius：執行高風險指令前，先看影響

Blast Radius 會攔下示範中辨識出的高風險指令，顯示可能影響的內容，讓使用者選擇繼續或取消。例如，刪除檔案或捨棄修改前，先看清楚操作範圍，可以減少只看指令名稱就批准的情況。

但官方指南明確說明，這個示範依指令文字辨識風險。別名或其他腳本裡的操作可能躲過檢查，因此嚴格禁止的操作仍需依靠權限規則。這項限制也說明，自訂確認介面與完整安全機制之間，仍有一段距離。

![Blast Radius 官方示範，列出 git reset --hard 將捨棄的兩個檔案修改，提供繼續與取消選項](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/blast-radius-official.png)

Blast Radius 在窄版終端機列出可能被捨棄的修改，讓使用者選擇是否執行。來源： 官方入門指南 。

### Replay Theater：回看 AI 修改的過程

Replay Theater 記錄一輪工作中的檔案編輯，讓使用者透過 `/replay` 逐步查看。當 AI 一次改動多處內容，按順序回看，有助於理解每一步修改，而不只看最後留下的結果。

![Replay Theater 官方介面逐步呈現 greet 改為 welcome 的檔案差異](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/replay-theater-official.png)

Replay Theater 以步驟選擇列與前後切換按鈕，回看單輪工作的檔案編輯。來源： 官方入門指南 。

這三個示範的價值，在於把資訊送到使用者需要它的時刻。不過，示範存在只證明擴充方式可行，實際是否省時，仍要放進自己的工作流程衡量。

## 安裝 Mods 前，要理解它能存取什麼

Mods 的權限範圍，是採用前最重要的條件。 [官方組織管理文件](https://code.claude.com/docs/en/plugins/mods/admin?ref=aiposthub.com) 說明，Mod 能以使用者的權限讀寫檔案、啟動程式、連線，也能接觸對話與工具呼叫。它可能讀取包含金鑰的設定，或改寫指令內容，因此安裝來源需要像其他軟體套件一樣審慎選擇。

即使已開啟 Claude Code 的指令沙箱，也不能推論 Mod 的所有行為都受到同樣限制。沙箱是限制程式可接觸範圍的隔離機制，但官方的 [組織管理文件](https://code.claude.com/docs/en/plugins/mods/admin?ref=aiposthub.com) 指出，Mods 本身沒有這種完整隔離。Mod 直接啟動的程序，與 Claude 的工具呼叫也有不同管控路徑。

Team、Enterprise 或具管理設定的環境，會載入內建的 `sec-default` ，保護組織管理的指引與部分權限規則。不過，它沒有把所有 Mod 的檔案與網路操作都封住。企業若要限制可載入的 Mods，仍應使用對應管理政策。

因此，我比較支持先從只顯示狀態、用途單純的 Mod 開始。當功能需要讀寫檔案或自動批准操作時，就要進一步理解程式內容，以及團隊要接受的權限範圍。

## 如何開始？先定義一個反覆出現的工作問題

依 [官方建立 Mods 文件](https://code.claude.com/docs/en/plugins/mods/create?ref=aiposthub.com) ，你可以直接向 Claude Code 描述需求，讓它協助建立 Mod。需求越具體，越容易判斷功能有沒有完成。例如：「每輪工作結束後，顯示對話容量使用比例」，就比「把我的 AI 助手變得更強」容易驗收。

開始前先確認 Claude Code 版本。2026 年 10 月 2 日查詢的官方文件要求 `2.1.287` 或更新版本，可以在終端機，也就是輸入系統指令的介面，執行：

```bash
claude --version
```

建立或取得外掛後，可以用 `claude plugin validate` 檢查指定外掛資料夾，了解它處理哪些事件、呼叫哪些功能。這有助於閱讀與檢查程式，但通過檢查仍不代表取得安全認證。安裝及啟用後，還要用實際任務確認它符合原本需求。

這次更新也可在 [Claude Code v2.1.287 發布紀錄](https://github.com/anthropics/claude-code/releases/tag/v2.1.287?ref=aiposthub.com) 找到。若你已熟悉外掛管理，可以從 `/plugin` 開始查看與管理套件。

## Claude Code Mods 常見問題

### 不會寫程式，也能做 Mod 嗎？

可以讓 Claude Code 協助產生程式，需求可以用自然語言描述。但當功能涉及檔案、連線或批准操作時，仍需要有人理解它會做什麼。AI 協助建立功能，與完成程式審查是兩件不同的工作。

### Mods 會取代 Skills 嗎？

兩者可以一起使用。Skill 提供任務方法，Mod 處理執行過程與介面。先用需求決定採用哪一種，通常比把所有規則都改寫成 Mod 更容易維護。

### 在每個 Claude Code 介面都能顯示相同畫面嗎？

自訂面板主要顯示在終端機與 Claude Desktop 的 Code 分頁。其他介面可能執行事件處理功能，卻不顯示 Mod 的面板。因此，選擇外掛時要確認自己使用的介面，以及它是否提供相應呈現方式。

## 結語：值得自訂的，是你每天都碰到的流程

Claude Code Mods 讓使用者有更多空間，把資訊顯示、操作確認與修改回顧做進開發助手。對每天使用 Claude Code、又有固定流程的團隊，這是一項有實際用途的擴充能力。

我的建議是，先選一個經常發生、驗收標準清楚的問題來試做。若它確實減少切換視窗、重複提醒或確認步驟，再考慮擴大使用。若維護成本高於省下的時間，或權限範圍難以接受，保留既有 Skills 與 Hooks 會更合適。