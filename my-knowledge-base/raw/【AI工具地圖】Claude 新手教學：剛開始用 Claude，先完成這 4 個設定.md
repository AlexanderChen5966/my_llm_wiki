---
title: "【AI工具地圖】Claude 新手教學：剛開始用 Claude，先完成這 4 個設定"
source: "https://www.aiposthub.com/claude-beginner-guide/"
author:
  - "[[Joanna Chang]]"
published: 2026-09-03
created: 2026-09-03
description: "剛開始使用 Claude，不知道該先設定什麼？這篇 Claude 新手教學帶你完成 Memory 記憶匯入、Google Connectors、Claude Code 與 Claude in Chrome，讓 Claude 從聊天工具進一步變成能讀資料、處理檔案與操作網站的 AI 助理。"
tags:
  - "clippings"
status: processed
---
最近開始用 Claude 的人越來越多，但很多人從 ChatGPT 換到 Claude 之後，使用方式其實完全沒有改變。例如：打開聊天視窗、輸入問題、等 AI 回答。

這當然沒有錯，只是如果你停在這裡，其實還沒有真正用到 Claude 現在比較有趣的地方。

因為現在的 Claude 除了聊天之外，還可以記住你的背景、連接 Gmail 與 Google Drive、直接處理電腦裡的檔案，甚至跟著你一起操作 Chrome 裡的網站。

換句話說，你可以把 Claude 想成一個剛到職的 AI 助理。

剛開始它什麼都不知道，你每次都得重新交代工作；但如果先把你的背景資料、工作文件，以及它可以使用的工具準備好，之後交代事情就會順很多。

所以如果你是第一次開始用 Claude，我會建議先不要急著研究一大堆 Prompt。

---

## Claude 跟 ChatGPT 有什麼不同？為什麼要先做這 4 個設定？

如果你原本就在使用 ChatGPT，第一次打開 Claude 時，很容易沿用同一套習慣：輸入問題、等 AI 回答，再把結果複製出來使用。

這樣沒有錯，但如果只是停在聊天介面，其實還沒有用到 Claude 很多更實用的能力。

Claude 現在除了可以回答問題之外，也能記住你的背景、連接 Gmail 與 Google Drive、讀取電腦裡的檔案，甚至直接在 Chrome 裡協助操作網站。

所以這篇不會先教一堆 Prompt 技巧，而是先把最實用的 4 個功能設定好。

以前使用 AI，通常是：

> 你先自己找資料 → 整理資料 → 貼給 AI → 再叫 AI 處理。

設定完成後，很多情況會變成：

> 直接告訴 Claude 要完成什麼，再讓它從你授權的資料或工作環境中取得需要的內容。

接下來先從最簡單的 **Memory** 開始。

---

## 第一步：開啟 Claude Memory，把 ChatGPT 的記憶匯入 Claude

如果你已經使用 ChatGPT 一段時間，它可能已經記住一些跟你有關的資訊，例如工作背景、常用工具、寫作習慣或回答偏好。

換到 Claude 後，不需要全部重新設定一次，可以直接使用 Claude 提供的 **Import memory** 功能，把這些背景資訊整理後匯入 Claude。

### Claude Memory 是什麼？

Claude 的 **Memory** 主要用來保存未來還會持續有用的資訊，例如工作背景、常用工具、回答偏好與長期專案。

進入：

**Settings → Memory**

這裡會看到兩個跟記憶有關的設定。

**Search and reference chats**  
讓 Claude 可以搜尋過去的聊天紀錄，找出和目前問題相關的內容。

**Generate memory from chats**  
讓 Claude 根據過去的對話，建立未來可以持續參考的 Memory。

如果你打算長期使用 Claude，可以先把這兩個功能開啟。

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/-------2026-09-02-------4.29.25.png)

從Setting中進入，選擇Memory

---

### 如何把 ChatGPT 的記憶匯入 Claude？

在 Memory 頁面往下找到：

**Import memory → Start import**

Claude 會提供一段 Prompt，讓你複製到原本使用的 AI 服務。

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/-------2026-09-02-------4.30.11.png)

匯入記憶

如果你原本使用 ChatGPT，流程很簡單：

1. 按下 **Copy** ，複製 Claude 提供的 Prompt。
2. 把 Prompt 貼到 ChatGPT。
3. ChatGPT 會整理目前記得的使用者背景。
4. 複製整理結果，回到 Claude。
5. 貼到 **Paste your memory details here** 。
6. 按下 **Add to memory** 。
![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/size/w2400/2026/09/-------2026-09-02-------4.31.44.png)

將內容複製給GPT，得到的結果在反貼回去

這裡要注意，並不是把 ChatGPT 的 Memory 資料直接搬到 Claude。

實際流程是：

**ChatGPT 先把目前知道的資訊整理成文字 → 再把這份內容交給 Claude 建立新的 Memory。**

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/-------2026-09-02-------4.44.02.png)

匯入前也建議快速檢查一次內容，把公司機密、客戶資料、個人敏感資訊，以及已經過期的背景資料刪掉再匯入。

Memory 也不是存得越多越好，留下那些未來仍會影響 Claude 回答方式的資訊就夠了。

---

## 第二步：把 Connectors 連上，讓 Claude 直接讀取工作資料

設定好 Memory 後，可以進入：

**Customize → Connectors**

如果平常主要使用 Google 服務，新手可以先連接：

- Gmail
- Google Drive
- Google 日曆

**Connector（連接器）** 的作用，是讓 Claude 在你授權的範圍內直接取得其他服務中的資料，不需要每次都自己搜尋、複製，再貼進聊天視窗。

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/size/w2400/2026/09/-------2026-09-02-------4.50.33.png)

選擇Connector，並將所需的內容連接

例如連接 Gmail 後，可以直接問：

> 幫我整理這週和 XXX 專案有關的 Email，目前還有哪些事情沒處理？

連接 Google Drive 後，也可以直接請它：

> 找出 XXX 專案最新的需求文件，整理這一版修改了哪些地方。

而 Google 日曆則可以用來查看行程、尋找空檔，或協助建立與修改活動。

目前 Claude 的 Google Workspace Connectors 已經不只支援讀取資料，也能執行部分操作，例如寄送 Email、建立行事曆活動，以及管理部分 Google Drive 檔案。

不過涉及寄信、修改行程、移動檔案這類會真正改變資料的操作時，還是建議執行前自己確認一次內容。

如果你使用的是公司帳號，Connector 是否能使用，也可能受到公司管理員的權限設定限制。

---

### 第三步：安裝 Claude Code，讓 Claude 直接處理電腦裡的檔案

如果你希望 Claude 不只是回答問題，而是能直接讀取、修改你電腦裡的檔案，就可以開始使用 **Claude Code** 。

Claude Code 是 Anthropic 提供的命令列工具，會直接在 **終端機 Terminal** 裡執行。它可以讀取目前資料夾中的檔案、修改內容、執行指令，適合用來處理需要多步驟完成的任務。

例如：

- 整理專案資料夾
- 讀取多份文件並整理重點
- 批次修改 Markdown、CSV 或文字檔
- 修改網站或程式碼
- 執行測試、檢查錯誤

對新手來說，可以先把它理解成： **一般 Claude 主要在聊天視窗裡工作，Claude Code 則可以直接進到你的電腦資料夾裡做事。**

### 如何安裝 Claude Code？

#### Mac

打開 **終端機 Terminal** ，貼上官方安裝指令：

```
curl -fsSL https://claude.ai/install.sh | bash
```
![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/-------2026-09-02-------4.51.26.png)

終端機安裝

也可以使用 Homebrew：

```
brew install --cask claude-code
```

#### Windows

打開 PowerShell：

```
irm https://claude.ai/install.ps1 | iex
```

安裝完成後輸入：

```
claude
```

第一次使用時，依照畫面登入 Claude 帳號即可。

如果要讓 Claude Code 處理某個專案，先進入該資料夾，再啟動：

```
cd 你的資料夾路徑
claude
```

接著就能直接用自然語言下指令，例如：

> 幫我讀取這個資料夾裡的文件，整理目前專案進度。

或：

> 幫我檢查這個網站專案有哪些問題，先不要修改。

需要注意的是， **安裝 Claude Code 不代表它會自動取得 Gmail、Google Drive 或其他帳號資料** 。這些外部服務仍需要透過 Connector、MCP 或其他整合方式連接。

---

## 第四步：安裝 Claude in Chrome，讓 Claude 直接協助操作網頁

最後一個設定，是把 Claude 接到 Chrome。

到 Chrome 線上應用程式商店搜尋：

**Claude**

安裝官方的 [**Claude in Chrome 擴充功能**](https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn?ref=aiposthub.com) 後，Claude 就可以直接讀取你目前開啟的網頁內容，並在你授權的情況下協助操作頁面。

例如你可以請它：

> 幫我整理這個網頁的重點。

> 幫我比較這三個方案有什麼差別。

> 幫我測試這個網站的註冊流程，看看哪裡不直覺。

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/size/w2400/2026/09/-------2026-09-02-------4.51.55.png)

安裝官方的 Claude in Chrome 擴充功能 後，Claude 就可以直接讀取你目前開啟的網頁內容，並在你授權的情況下協助操作頁面

除了讀取內容之外，Claude in Chrome 也能進一步執行像是點擊、輸入文字、切換頁面等操作。

這對需要經常查資料、測試網站、填寫表單或處理後台工作的人會很方便。

不過因為瀏覽器裡可能包含帳號、公司系統或敏感資料，涉及送出表單、刪除內容、付款或其他重要操作時，還是建議自己先確認一次再執行。

---

## Claude 新手常見問題 FAQ

### Q1：Claude Memory 可以直接把 ChatGPT 的記憶搬過去嗎？

不算是直接搬移。

Claude 會先提供一段 Prompt，讓 ChatGPT 整理目前記得的使用者背景，再把這份內容貼回 Claude 建立新的 Memory。

所以本質上是「整理後重新匯入」，不是兩邊的記憶資料直接同步。

### Q2：Claude Code 一定要會寫程式才能用嗎？

不用。

Claude Code 雖然主要面向開發工作，但也可以拿來整理文件、批次修改檔案、分析 CSV，或處理資料夾裡的大量內容。

如果你的需求只是一般聊天、查資料或產出文件，其實先用一般 Claude 就夠了；需要直接操作本機檔案時，再使用 Claude Code。

### Q3：Connector 跟 Claude Code 有什麼差別？

可以先用最簡單的方式區分：

**Connector** 主要負責連接 Gmail、Google Drive、Google 日曆等外部服務。

**Claude Code** 則主要處理你電腦裡的資料夾、檔案與終端機任務。

兩個可以搭配使用，但解決的是不同問題。

### Q4：Claude in Chrome 可以直接操作網站嗎？

可以。

除了讀取目前網頁內容之外，Claude in Chrome 也能協助點擊、輸入文字與操作頁面。

但如果涉及付款、刪除資料、公司後台或其他重要操作，建議送出前還是自己確認一次。

---

## 剛開始用 Claude，不需要一次學完所有功能

如果你是第一次使用 Claude，我反而不建議一開始就研究一堆進階功能。

先完成這四件事：

**Memory → Connectors → Claude Code → Claude in Chrome**

你就會開始感受到 Claude 不只是另一個聊天型 AI，而是可以逐步接進自己的資料與工作流程。

如果平常主要只是聊天，可以先從 **Memory** 開始；如果大量使用 Gmail、Drive，就先設定 **Connectors** ；需要處理電腦檔案時，再往 Claude Code 與 Chrome 擴充功能延伸。

重點不是一次把所有功能都裝滿，而是先把你每天真的會用到的能力接起來。