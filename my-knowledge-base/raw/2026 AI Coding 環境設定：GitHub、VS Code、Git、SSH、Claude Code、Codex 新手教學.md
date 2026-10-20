---
title: "2026 AI Coding 環境設定：GitHub、VS Code、Git、SSH、Claude Code、Codex 新手教學"
source: "https://www.aiposthub.com/ai-coding-environment-github-vscode-claude-code-codex/"
author:
  - "[[Philo]]"
published: 2026-09-30
created: 2026-09-30
description: "AI Coding 新手如何設定環境？本文用白話解釋 GitHub、VS Code、Git、SSH、Claude Code 與 Codex，並整理正確安裝順序與完成檢查表。"
tags:
  - "clippings"
status: "inbox"
---
AI Coding 環境設定的目標，是讓 AI 能在一個可保存、可回復、可同步的專案裡工作。新手常直接安裝 Claude Code 或 Codex，等到 AI 改完檔案，才發現不知道怎麼保留版本，也不敢把結果上傳到 GitHub。

比較穩定的順序是先準備 VS Code、Git 與 GitHub，再設定 HTTPS 或 SSH 連線，親手完成第一次 Commit 與 Push，最後才把 AI Coding 工具接進來。

本文會解釋六個工具各自的角色，提供 Windows 與 macOS 都能沿用的設定順序，並告訴你做到哪一步才算環境完成。

## Git、GitHub、VS Code、SSH、Claude Code、Codex 分別做什麼？

這六個名稱不在同一層。把它們分成「編輯、版本、雲端、連線、AI」五個角色，就容易理解。

| 工具 | 白話功能 | 在 AI Coding 裡的角色 |
| --- | --- | --- |
| VS Code | 開啟資料夾、編輯檔案與使用終端機的程式編輯器 | 你與 AI 查看、修改與比較程式的工作區 |
| Git | 記錄檔案每次變更的版本控制工具 | 保存修改快照，出錯時能比較或回復 |
| GitHub | 放置 Git Repository 的雲端協作平台 | 備份專案、分享程式、管理 Issue 與 Pull Request |
| SSH | 用金鑰驗證身分的安全連線方式 | 讓本機不必每次輸入密碼就能連接 GitHub |
| Claude Code | Anthropic 的 AI Coding Agent | 讀取專案、修改檔案、執行指令並協助檢查結果 |
| Codex | OpenAI 的 AI Coding Agent | 在 IDE、CLI、桌面或雲端工作流中協助開發、Review 與除錯 |

Repository 是裝著專案檔案與 Git 歷史的資料夾。IDE 是整合編輯、終端機、除錯與擴充功能的開發環境；本文使用的 VS Code 可以承接這些工作。AI Coding Agent 則是能讀取專案、修改檔案與執行工具的 AI 助理。

最重要的關係是：VS Code 提供工作空間，Git 記錄版本，GitHub 保存遠端副本，SSH 或 HTTPS 負責連線，Claude Code 與 Codex 在這套環境裡工作。

## AI Coding 新手環境的正確設定順序

以下順序先建立可回復的專案，再加入 AI。即使某個 AI 工具之後更換，前面的 Git 與 GitHub 基礎仍能沿用。

### 步驟一：建立 GitHub 帳號與第一個 Private Repository

先建立 GitHub 帳號，並啟用兩步驟驗證。Private Repository 是只有你與獲授權成員能看到的私有程式庫，適合放尚未公開的小工具與練習專案。

建立 Repository 時，可以先加入 README。README 是專案首頁的說明文件，通常用來記錄用途、安裝方式與操作方法。

### 步驟二：安裝 VS Code 與 Git

VS Code 官方提供 Windows、macOS 與 Linux 版本。安裝後先確認能開啟一個本機資料夾，也能使用內建終端機。

Git 需要另外安裝或確認系統已提供。完成後，在終端機輸入以下指令：

```bash
git --version
```

看到版本號代表 Git 可以使用。VS Code 能開啟資料夾、終端機能顯示 Git 版本，才算完成基本工具安裝。

### 步驟三：設定 Git 的姓名與 Email

Git 會把作者資訊寫進每次 Commit。Commit 是一個有說明文字的版本快照，方便日後知道誰在什麼時間做了哪些變更。

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

這個 Email 建議使用已加入 GitHub 帳號的地址，或使用 GitHub 提供的隱私 Email。不要把密碼或 API（應用程式介面）金鑰寫進 Git 設定與專案檔案；這類金鑰是讓程式存取服務的祕密憑證。

### 步驟四：選擇 HTTPS 或 SSH 連接 GitHub

GitHub 支援 HTTPS 與 SSH 兩種常見連線方式。

- HTTPS：設定較直覺，可搭配 GitHub CLI 或 Git Credential Manager 保存憑證。
- SSH：先建立公鑰與私鑰，完成後可以重複用金鑰連線，適合長期在同一台電腦開發。

公鑰可以交給 GitHub，私鑰只能留在自己的電腦。若要使用 SSH，應替私鑰設定 Passphrase，也就是額外保護金鑰的密碼。

設定完成後，可用官方測試指令確認連線。測試成功只代表身分驗證完成，不會讓 GitHub 提供一般伺服器 Shell。

### 步驟五：Clone Repository 到電腦

Clone 是把 GitHub 上的 Repository、檔案與版本歷史完整下載到本機。

```bash
git clone YOUR_REPOSITORY_URL
```

完成後，用 VS Code 開啟 Clone 下來的資料夾。請確認左側檔案清單看得到 README，終端機目前位置也在該專案資料夾內。

### 步驟六：完成第一次 Commit 與 Push

先修改 README，例如加入一行專案目的，再依序執行：

```bash
git status
git add README.md
git commit -m "docs: add project purpose"
git push
```

`git status` 顯示哪些檔案有變化， `git add` 把指定檔案放進這次版本。接著， `git commit` 建立快照， `git push` 再把本機 Commit 上傳到 GitHub。

回到 GitHub 頁面，若能看到新的 Commit 與 README 內容，表示本機、Git 與 GitHub 已經接通。這一步比單純看到工具安裝完成更重要，因為它證明版本真的能保存與同步。

![](https://www.youtube.com/watch?v=NFjz1AGKA4c)

GitHub 官方新手教學示範如何在 VS Code 完成 Git 初始化、Commit、Branch、Push 與 Clone。影片來源： [GitHub 官方 YouTube 頻道](https://www.youtube.com/watch?v=NFjz1AGKA4c&ref=aiposthub.com) 。

### 步驟七：把 Claude Code 與 Codex 接進 VS Code

Claude Code 官方提供 VS Code 擴充功能。2026 年 9 月 30 日查詢時，官方要求 VS Code 1.94.0 以上，並可用付費 Claude 訂閱或 Claude Console 帳號登入。擴充功能支援檔案引用、修改差異、計畫檢查與多個對話。

![Claude Code 在 VS Code 側欄讀取檔案並撰寫測試的官方介面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/claude-code-vscode-official.jpg)

Claude Code 的 VS Code 擴充功能可在側欄讀取專案、規劃工作並顯示修改內容。圖片來源： Anthropic Claude Code 官方文件 。

Codex 也提供 IDE 擴充功能，可在 VS Code 與相容編輯器中讀取、修改與預覽程式碼。OpenAI 官方將 Codex 定位為能協助寫程式、Review 與除錯的 Coding Agent，也能在 CLI、桌面、網頁與行動介面中使用。

![](https://www.youtube.com/watch?v=sd21Igx4HtA)

OpenAI 官方影片示範 Codex IDE 擴充功能如何在程式編輯器中讀取、修改與檢查程式碼。影片來源： [OpenAI 官方 YouTube 頻道](https://www.youtube.com/watch?v=sd21Igx4HtA&ref=aiposthub.com) 。

兩個工具可以安裝在同一套專案環境，但第一次使用不必同時交付大型任務。先要求 AI 解釋 README 或新增一個簡單檔案，再檢查變更內容，確認 Git 能顯示 Diff。Diff 是修改前後的差異畫面，可以幫你知道 AI 實際改了什麼。

延伸閱讀： [Claude Code 新創團隊指南：從「每個人都能做 Prototype」到可驗證的 AI 開發流程](https://www.aiposthub.com/claude-code-startup-guide-ai-native-team/)

## CLAUDE.md 是什麼？為什麼要在環境完成後建立？

`CLAUDE.md` 是 Claude Code 會在工作階段載入的專案指令檔，可以記錄常用指令、技術架構、程式風格與不可修改的範圍。執行 `/init` 可以協助產生第一份檔案。

這個檔案應在基本環境與專案存在後再建立。Claude 需要先讀到實際資料夾、指令與檔案結構，產生的規則才有專案脈絡。建立後仍要人工檢查，不要把自動產生的內容直接視為正確規格。

建議第一份 `CLAUDE.md` 只保留：

- 專案用途與主要目錄。
- 安裝、啟動、測試與格式化指令。
- 允許與禁止修改的範圍。
- 完成任務前必須執行的檢查。

規則太長會降低閱讀與遵守效果。特定任務才需要的說明，可以留在獨立文件或 Skill，不必全部塞進每次都會載入的專案指令。

## AI Coding 環境完成檢查表

做到以下八項，才算環境可以開始使用：

1. GitHub 帳號可以登入，兩步驟驗證與復原方式已保存。
2. VS Code 可以開啟專案資料夾與終端機。
3. `git --version` 能顯示版本。
4. Git 作者姓名與 Email 已設定。
5. HTTPS 或 SSH 能正常連接 GitHub。
6. Repository 已成功 Clone 到本機。
7. 第一次 Commit 已 Push，GitHub 網頁能看到變更。
8. Claude Code 或 Codex 能讀取專案，而且你能在 Diff 中檢查修改。

只完成擴充功能安裝，還不代表環境完成。能把一個小變更安全地保存、同步並檢查，才是可用的 AI Coding 工作流。

延伸閱讀： [GitHub Copilot App 平行 Agent 教學：Worktree 是什麼？怎麼同時跑多個開發任務](https://www.aiposthub.com/github-copilot-app-parallel-agents-worktree/)

💻

****從零建立 AI Coding 環境****  
如果還沒完成 GitHub、VS Code、Git、SSH、Claude Code 與 Codex 的設定，課程會帶你建立環境並完成第一次 Commit 與 Push。  
[查看 Skill Shot 12：AI Coding 新手環境課](https://academy.aiposthub.com/p/skill-shot-12-github-vscode-claude-code?ref=aiposthub.com)

## AI Coding 環境常見問題

### 不會寫程式，也需要學 Git 嗎？

需要理解最基本的版本概念，但不必先背完整指令。至少要知道 `status` 、 `add` 、 `commit` 與 `push` 的用途，才能確認 AI 改了哪些檔案，也能在出錯時找到先前版本。

### GitHub Desktop 可以取代終端機嗎？

GitHub Desktop 可以處理許多常用 Git 操作，適合不熟指令的使用者。但 Claude Code、Codex 與各種開發工具仍常會使用終端機，因此建議至少熟悉開啟終端機、確認路徑與執行檢查指令。

### HTTPS 和 SSH 哪一個比較適合新手？

想快速開始，可以用 HTTPS 搭配 GitHub CLI 或 Credential Manager。若會長期在固定電腦開發，SSH 完成一次金鑰設定後也很方便。兩種方式都能安全使用，重點是保護憑證與私鑰。

### Claude Code 和 Codex 一定要二選一嗎？

不用。兩者都能在程式專案中協助閱讀、修改與檢查，你可以依帳號方案、介面與任務選擇。新手更應先把 Git、GitHub 與驗收方式準備好，避免同時使用多個 Agent 卻無法判斷結果。

### AI 修改錯誤時怎麼辦？

先停止擴大修改，用 `git status` 與 Diff 確認影響範圍。如果先前已有 Commit，可以比較或回復到已知版本。涉及刪除資料、部署、付款或正式環境時，不應只依賴 AI 自動執行。

## 結論：先完成一次可回復的修改，再追求更強的 Agent

AI Coding 工具會持續更新，但可保存、可比較、可回復的開發環境仍是基礎。先讓 VS Code、Git、GitHub 與連線方式正常工作，再安裝 Claude Code 或 Codex，新手比較容易知道每一步發生了什麼。

第一個目標不用是完整 App。只要能 Clone 專案、修改 README、完成 Commit 與 Push，再讓 AI 做一個小變更並檢查 Diff，就已建立一套能繼續擴充的工作環境。