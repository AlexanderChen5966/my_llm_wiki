---
title: "Claude Code Auto Memory 是什麼？自動記憶功能完整教學"
source: "https://israynotarray.com/ai/20260330/2896397295/"
author:
  - "[[Ray]]"
published: 2026-03-30
created: 2026-05-19
description: "每次開新的 Claude Code 對話都要重新交代一次偏好？其實 Claude Code 有一個 Auto Memory 的功能，會在對話過程中自動把你的修正和偏好記下來，下次開對話就不用再重複講一次。這篇帶你了解 Auto Memory 的運作方式、儲存位置、載入機制，以及怎麼管理它。"
tags:
  - "clippings"
status: processed
---
![Claude Code Auto Memory 是什麼？自動記憶功能完整教學](https://images.israynotarray.com/blog/ai/claude-auto-memony/Gemini_Generated_Image_u9xcubu9xcubu9xc_1280x684.png)

Claude Code Auto Memory 是什麼？自動記憶功能完整教學

> **Note**  
> Claude Code Auto Memory 是 Claude 自動學習你偏好的功能。CLAUDE.md 是你主動寫給 Claude 的規則，Auto Memory 則是 Claude 在對話中自己記下來的經驗，存在 `~/.claude/projects/` 下的 memory 資料夾，只存在本機不會同步。

## 前言

如果你已經在用 Claude Code 做開發，應該有遇過這種狀況：每次開一個新的對話，Claude 就像失憶一樣，之前跟它說過的偏好通通都忘了，又要重新講一次。

以我自己來講，之前常常在對話中告訴 Claude 不要在文章裡面加太多 emoji，結果下一次對話又開始加了，真的滿困擾的。

之前我有寫過一篇 [CLAUDE.md 撰寫技巧](https://israynotarray.com/ai/20260310/3737444063/) ，教大家怎麼用 CLAUDE.md 來手動設定專案規則，但其實 Claude Code 的記憶系統不只有 CLAUDE.md 而已，還有一個叫做 Auto Memory 的功能，它會在對話過程中自動把你的修正和偏好記下來，下次開對話就不用再重複講一次。

## Auto Memory 跟 CLAUDE.md 差在哪？

在開始之前先快速講一下這兩者的差異，因為很多人搞不清楚它們的定位。

|  | CLAUDE.md | Auto Memory |
| --- | --- | --- |
| 由誰撰寫 | 你自己 | Claude 自動寫入 |
| 內容類型 | 指示和規則 | 學到的經驗和模式 |
| 適用範圍 | 專案、使用者或組織 | 每個 Working Tree |
| 載入方式 | 每次對話全部載入 | 只載入前 200 行或 25KB |
| 適合用在 | 程式碼規範、工作流程、專案架構 | Build 指令、Debug 心得、偏好 |

簡單來講，CLAUDE.md 是你主動寫給 Claude 的規則，像是「這個專案用 2 格縮排」、「commit message 用英文」之類的。而 Auto Memory 是 Claude 自己在跟你互動的過程中學到的東西，你不用特別去寫，它會自己記。

如果你想了解 CLAUDE.md 的部分，可以參考我之前寫的 [CLAUDE.md 與 Rules for AI 撰寫技巧](https://israynotarray.com/ai/20260310/3737444063/) ，這篇就不重複介紹了。

## Auto Memory 是什麼？

Auto Memory 簡單來講就是讓 Claude 自己做筆記。當你在對話中糾正 Claude 的行為、或是回饋某個做法好不好的時候，Claude 會自己判斷這個資訊在未來的對話中是不是有用的，如果覺得有價值就會自動存下來。

舉個例子，假設你在某次對話中跟 Claude 說：

> 不要在文章中使用太多 emoji，看起來太 AI 了

Claude 就會把這個回饋存成一個記憶檔案，之後每次開新的對話都會載入這個記憶，所以它就會記得你不喜歡 emoji。

不過也不是每次對話都會寫記憶，Claude 會自己評估這個資訊值不值得記，如果只是一次性的討論或是跟未來對話無關的內容，它就不會特別記下來。

> **Note**  
> Auto Memory 需要 Claude Code v2.1.59 以上版本，可以用 `claude --version` 確認。

## 儲存在哪裡？

這邊要先釐清一個很容易搞混的東西。你的專案裡面會有一個 `.claude/` 資料夾，但 Auto Memory 的檔案並不是放在那裡面的。專案的 `.claude/` 只負責放設定檔（ `settings.json` 、 `settings.local.json` ）和 Rules。

Auto Memory 的檔案是存在你 Home 目錄底下的 `~/.claude/projects/<專案>/memory/` 。

舉個實際的例子比較好懂，假設你的專案在 `/Users/ray/my-project` ，那：

```
/Users/ray/my-project/.claude/          ← 專案設定，在你的專案資料夾裡面
/Users/ray/.claude/projects/-Users-ray-my-project/memory/  ← Auto Memory，在你的 Home 目錄底下
```

這兩個路徑都有 `.claude` ，但一個在你的專案資料夾，一個在 Home 目錄底下，千萬不要搞混了。所以如果你去翻專案的 `.claude/` 資料夾，發現裡面只有 `settings.local.json` ，這是正常的，記憶檔案本來就不在這邊。

然後記憶的路徑是根據 Git Repo 推導的，所以同一個 Repo 底下的子目錄和 Worktree 會共用同一份記憶。如果不在 Git Repo 裡面的話就會用專案根目錄的路徑。

另外 `~/.claude/projects/` 底下的資料都是跟著你這台電腦走的（machine-local），不會同步到其他機器或雲端環境上。

### 自訂儲存位置

如果你想把記憶存到其他地方，可以在設定中指定 `autoMemoryDirectory` ：

```json
{
  "autoMemoryDirectory": "~/my-custom-memory-dir"
}
```

> **Note**  
> 這個設定只接受 Policy、Local 和 User 層級，不接受 Project 層級的 `.claude/settings.json` ，目的是避免共用專案裡面的惡意設定把記憶導去不該去的地方。

## 記憶目錄的結構

記憶目錄裡面主要有兩種東西：

```
~/.claude/projects/<project>/memory/
├── MEMORY.md          # 索引檔，每次對話都會載入
├── debugging.md       # Debug 相關的筆記
├── api-conventions.md # API 設計決策
└── ...                # Claude 自己建立的其他主題檔案
```

`MEMORY.md` 是整個記憶目錄的索引，Claude 會透過它來追蹤有哪些記憶、分別存在哪個檔案裡面。以我自己的 Blog 專案為例，目前的 `MEMORY.md` 長這樣：

```markdown
# Memory Index

- [feedback_writing_style.md](./feedback_writing_style.md) - 不要在文章中大量使用「」和 emoji，會看起來太 AI
```

然後 `feedback_writing_style.md` 裡面就是具體的記憶內容，記錄了我不喜歡在文章中使用太多引號和 emoji 這件事。

## 載入機制

這邊有一個滿重要的細節，就是 `MEMORY.md` 只有前 200 行或前 25KB（取小的那個）會在每次對話開始時載入，超過的部分是不會被讀到的。所以 Claude 會盡量讓 `MEMORY.md` 保持精簡，把比較詳細的內容拆到獨立的 Topic 檔案裡面。

那 Topic 檔案（像是 `debugging.md` 、 `patterns.md` ）不會在啟動時全部載入，而是 Claude 在需要的時候才會去讀取。所以就算你的記憶越來越多，也不會每次都把整個記憶目錄全部灌進 Context 裡面。

這個 200 行的限制只針對 `MEMORY.md` ，CLAUDE.md 的話不管多長都會全部載入（但越短遵循度越好就是了）。

## 怎麼知道 Claude 在寫記憶？

當你在 Claude Code 的介面看到「Writing memory」或「Recalled memory」的提示，就代表 Claude 正在更新或讀取記憶檔案。你不需要做什麼，它會自己處理。

## 怎麼管理 Auto Memory？

### 用 /memory 指令

`/memory` 是管理記憶的主要入口，你可以用它來查看目前對話載入了哪些記憶和指示檔案、開關 Auto Memory、快速打開記憶資料夾、或是選擇任一檔案在編輯器中開啟。

### 主動叫 Claude 記住或忘記

如果你想讓 Claude 記住某件事情，可以直接在對話中跟它說，例如「記住：一律用 pnpm 不要用 npm」，Claude 就會把這個存到 Auto Memory。

反過來，如果某個記憶已經過時了，你也可以跟 Claude 說「忘記之前關於 pnpm 的規則」，它就會去更新或刪除對應的記憶檔案。

### 手動編輯

Auto Memory 的檔案就是普通的 Markdown，你隨時可以直接到 `~/.claude/projects/` 底下找到對應的記憶目錄，手動新增、修改或刪除都沒問題。

### 停用 Auto Memory

如果你不想讓 Claude 自動記錄記憶的話，有三種方式可以關掉：

- 在對話中輸入 `/memory` ，用裡面的開關切換
- 在設定檔加上 `"autoMemoryEnabled": false`
- 設定環境變數 `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`

## 常見問題

### 不知道 Auto Memory 存了什麼

輸入 `/memory` 然後選擇記憶資料夾就能瀏覽，全部都是 Markdown 檔案，直接打開看就好。

### Auto Memory 跟 CLAUDE.md 什麼時候該用哪個？

簡單來講，確定不變的規範（程式碼風格、命名規則、Build 流程）適合寫在 CLAUDE.md 裡面，讓整個團隊共用。而比較個人化的、或是 Claude 從你的互動中學到的東西（你的偏好、你踩過的雷、某個 Bug 的 Debug 方式），就交給 Auto Memory 自動處理。

### /compact 之後 Auto Memory 會消失嗎？

不會。Auto Memory 的檔案是存在磁碟上的，跟 Context 壓縮無關。 `/compact` 只會壓縮對話紀錄， `MEMORY.md` 跟 CLAUDE.md 在 Compact 之後都會重新從檔案讀取然後注入回 Context。

你的支持會直接轉換成更多技術筆記

如果我的筆記讓你少踩一個坑、節省 Debug 的時間，  
也許你可以請我喝杯咖啡，讓我繼續當個不是 Array 的 Ray ☕

 [![buymeacoffee](https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png)](https://www.buymeacoffee.com/israynotarray)| [![line](https://israynotarray.com/images/line-images.png) ](https://store.line.me/search/zh-Hant?q=israynotarray)| [![portaly](https://israynotarray.com/images/logo.b91a6dbf.webp)](https://portaly.cc/israynotarray/support)

Terminal

ray — -zsh — 80x24

Welcome to Ray Terminal v1.0.0

Type "help" to see available commands.

\------------------------------------

➜ ~