---
title: "你的 Claude Code Context 是不是都被吃光了？AGENTS.md 與 Context 管控全攻略"
source: "https://israynotarray.com/ai/20260409/1057131696/"
author:
  - "[[Ray]]"
published: 2026-04-09
created: 2026-05-13
description: "這一篇稍微來聊一下 Claude Code 的 Context 管控問題，很多人在使用 Claude Code 的時候裝了一堆 Skill、Plugin、MCP Server，然後 CLAUDE.md 又寫得落落長，結果都還沒開始工作 Context 就被吃掉一大半了。順便也會介紹一下 AGENTS.md 這個跨工具的通用標準，以及該怎麼讓你的 Context 用在刀口上。"
tags:
  - "clippings"
status: processed
---
![你的 Claude Code Context 是不是都被吃光了？AGENTS.md 與 Context 管控全攻略](https://images.israynotarray.com/blog/ai/ai-context/Gemini_Generated_Image_q7hfnjq7hfnjq7hf_1280x714.png)

你的 Claude Code Context 是不是都被吃光了？AGENTS.md 與 Context 管控全攻略

## 前言

這一篇稍微來聊一下 Claude Code 的 Context 管控問題，很多人在使用 Claude Code 的時候裝了一堆 Skill、Plugin、MCP Server，然後 CLAUDE.md 又寫得落落長，結果都還沒開始工作 Context 就被吃掉一大半了。順便也會介紹一下 AGENTS.md 這個跨工具的通用標準，以及該怎麼讓你的 Context 用在刀口上。

> **Note**  
> 如果你還不熟悉 Claude Code 的基本操作，可以先去看 [想用 Claude Code 開發？這篇帶你從入門到進階](https://israynotarray.com/ai/20260128/374686091/) 。如果你想了解 CLAUDE.md 的撰寫技巧，可以參考 [CLAUDE.md 與 Rules for AI 撰寫技巧](https://israynotarray.com/ai/20260310/3737444063/) 。

## 稍微聊一下 Context Window

在開始聊管控之前，先回顧一下 Context Window 是什麼。

不管你用的是 Claude Code、Cursor、Copilot 還是其他 AI 開發工具，背後都有一個 Context Window 的概念。你可以把它想像成 AI 的短期記憶容量，所有 AI 在這次對話中需要用到的資訊都必須塞進這個空間裡面，包含系統指令、規則檔、對話紀錄、讀取的檔案內容、指令的執行結果等等，全部都會佔這個空間。Context 越滿 AI 的表現就越差，這個概念在任何 AI 開發工具上都是一樣的。

這篇會以 Claude Code 當作範例來說明，可是背後的觀念不管你用哪一個工具都適用。目前 Claude Code 的 Context Window 大小是：

- **標準：** 200,000 tokens
- **延伸（Opus 4.6 / Sonnet 4.6）：** 1,000,000 tokens

可是實際上你能用的空間比你想像的少很多，因為在你還沒開始打字之前 Claude Code 就已經幫你塞了一堆東西進去了，像是系統指令（System Prompt）大概就吃掉好幾千個 Token，再加上工具定義、CLAUDE.md、Auto Memory、Skill 描述、MCP 工具名稱、環境資訊等等，以一個裝了不少東西的專案來講，還沒開始工作就已經用掉一萬多個 Token 了。

你可以在 Claude Code 裡面輸入 `/context` 來看目前的 Token 使用狀況，它會用一個色彩格狀圖列出每個項目各佔了多少，還會給你一些優化建議。

而且 Context Window 也不是用到滿才會有問題，以我自己的體感來講大概到六成左右 AI 的注意力就會開始下降，可能會忽略你在規則檔裡面寫的東西 or 忘記之前對話中提到的事情。當 Context 快要滿的時候 Claude Code 會自動觸發壓縮（Auto Compaction），把之前的對話紀錄壓縮成摘要來騰出空間，可是 Auto Compaction 其實滿不準確的，很容易丟失重要的資訊，所以 **不建議依賴自動壓縮** ，後面會詳細介紹正確的做法。

## 那些吃掉你 Context 的兇手

了解了 Context 之後，接下來就來聊一下哪些東西最容易在不知不覺中把你的 Context 吃光。

### CLAUDE.md 寫太長

很多人會把整個專案的文件、API 文件、詳細的架構說明全部塞進 CLAUDE.md 裡面，結果一個 CLAUDE.md 就好幾百行。可是問題是 CLAUDE.md 的內容是 **每次對話都會載入** 的，不管你這次對話用不用得到它都會佔著你的 Context。

以我自己來講，建議把 CLAUDE.md 控制在 200 行以內，只放 Claude 無法從程式碼中推斷出來的東西，像是建置跟測試的指令、團隊的 Code Style 規範（如果跟預設不同的話）、架構上的重要決策、常見的踩坑點等等。如果有些指令只跟特定目錄有關，可以用 `.claude/rules/` 搭配 `paths` 來做路徑限定，這樣只有在 Claude 存取到對應的檔案時才會載入。

之前在 [CLAUDE.md 與 Rules for AI 撰寫技巧](https://israynotarray.com/ai/20260310/3737444063/) 裡面有比較詳細的介紹。

### MCP Server 裝太多

每個 MCP Server 上面都有一堆工具定義，如果沒有開啟 Tool Search 的話所有工具的完整定義都會在啟動時一次載入到 Context 裡面，裝越多 Server 吃掉的 Token 就越多。

好消息是 Claude Code 目前預設有開啟 **Tool Search** （也叫做 Deferred Tool Loading），這個機制會讓 MCP 工具只載入名稱，等到 Claude 真的需要用到某個工具時才會去載入完整的定義，啟動時的 MCP Token 消耗可以因此大幅降低。你可以用 `/mcp` 來管理 MCP Server 的連線狀態，暫時用不到的 Server 先斷開就可以省下不少 Context。

### Skill 跟 Plugin 裝太多

Skill 的載入方式跟 MCP 類似，啟動時只會載入名稱跟描述，等到你真的呼叫了某個 Skill 之後完整內容才會載入。可是 Skill 描述有一個總量限制（預設是 Context Window 的 1%），裝太多的話描述會被截斷，導致 Claude 在配對你的指令跟 Skill 時可能出錯。

這邊有一個滿實用的技巧：如果你有一些 Skill 是屬於手動觸發型的（像是 deploy、commit 這種），可以在 SKILL.md 裡面加上 `disable-model-invocation: true` ，Claude 就完全不會看到這些 Skill 的描述，只有你手動用 `/skill-name` 的時候才會載入，完全不佔 Context。之前在 [比 MCP 更省 Token 的 Agent Skills](https://israynotarray.com/ai/20260120/4189755271/) 裡面有完整的介紹。

### 對話太長不清理

這個大概是最多人踩到的坑。很多人會在一個對話裡面做很多不同的事情，改完 A 功能又去改 B 功能，中間還問了幾個不相關的問題，結果對話紀錄越來越長 Context 越來越滿。更糟糕的是如果你一直在修正 Claude 的錯誤，那些錯誤的嘗試也會留在 Context 裡面，反而影響 Claude 後續的判斷。

「那用 `/compact` 壓縮一下不就好了？」你可能會這樣想。

可是 `/compact` 其實滿不可靠的，它在壓縮的時候很容易把重要的細節丟掉，像是你改了哪些檔案、目前的任務進度、測試結果等等，壓縮完之後 Claude 可能就忘記之前做過什麼了。

所以我比較建議的做法是 **手動存檔 + 重開對話** ，流程大概是這樣的：

1. 當 Context 到了大約六成左右（可以用 `/context` 看），or 你剛完成一個功能的時候
2. 請 Claude 進入 Plan Mode（按兩次 `Shift+Tab` ）
3. 請它把目前的進度、已修改的檔案、待辦事項等資訊整理成一份 Markdown 檔案存下來
4. 用 `/clear` 清掉整個對話
5. 重新開始對話，請 Claude 讀取剛才存的那份 Markdown 繼續工作

舉例來講你可以這樣跟 Claude 說：

```
請幫我把目前的工作進度整理成一份 markdown 存到 .claude/progress.md，
包含以下資訊：
- 目前正在做什麼任務
- 已經修改了哪些檔案
- 還有哪些事情沒做完
- 測試的結果
```

存完之後 `/clear` ，然後新對話開頭就說：

```
請讀取 .claude/progress.md，繼續之前的工作。
```

這樣就可以無縫接續了，而且你可以精準控制哪些資訊要保留、哪些可以丟掉。清掉對話之後之前那些錯誤的嘗試跟無關的對話紀錄也會一起被清掉，Claude 的注意力會比較集中。

另外幾個小技巧：不同任務之間直接用 `/clear` 重新開始就好不需要存檔，用 `/btw` 問小問題的話答案會出現在浮動視窗不會進入對話紀錄也不佔 Context，然後如果連續修正 Claude 失敗兩次以上就不要再修了，直接 `/clear` 重來把學到的東西寫進新的 Prompt 裡面會比較快。

## AGENTS.md 是什麼？

接下來要聊一個跟 CLAUDE.md 有關的東西： **AGENTS.md** 。

你可能會想說：我已經有 CLAUDE.md 了，為什麼還需要 AGENTS.md？

這邊的背景是這樣的，現在市面上的 AI 開發工具越來越多，每一個工具都有自己的規則檔格式，像是 Claude Code 用 `CLAUDE.md` 、Cursor 用 `.cursor/rules/*.mdc` 、GitHub Copilot 用 `.github/copilot-instructions.md` 。如果你的團隊裡面有人用 Claude Code、有人用 Cursor、有人用 Copilot，那你就需要維護好幾份不同格式的規則檔，而且內容都差不多，這其實滿浪費時間的。

AGENTS.md 就是為了解決這個問題而誕生的，簡單來講它是一個跨工具的通用標準，放在專案根目錄的一個 Markdown 檔案，任何支援這個標準的 AI 工具都可以讀取它。

| 面向 | CLAUDE.md | AGENTS.md |
| --- | --- | --- |
| 適用範圍 | 只有 Claude Code 讀得懂 | 跨工具通用標準 |
| 支援工具 | Claude Code | Codex CLI、Cursor、Copilot CLI、Gemini CLI 等 |
| 特殊功能 | @imports、階層載入、路徑規則、/init 產生 | 基本 Markdown 指令 |
| Claude Code 支援 | 原生讀取 | 不會自動讀取，需要透過 CLAUDE.md 引入 |

這邊有一個滿重要的事情： **Claude Code 不會自動讀取 AGENTS.md** ，它只認 CLAUDE.md。所以如果你的專案有 AGENTS.md 的話，你需要在 CLAUDE.md 裡面用 `@` 語法引入：

```markdown
@AGENTS.md

## Claude Code 專屬設定
使用 Plan mode 來處理 \`src/billing/\` 底下的變更。
```

那什麼時候該用哪一個呢？以我自己來講，如果團隊都是用 Claude Code 的話其實不太需要 AGENTS.md，用 CLAUDE.md 就好了。可是如果你的專案有多種 AI 工具在用，那 AGENTS.md 會是一個比較好的選擇，把共用的規則寫在 AGENTS.md 裡面，然後各工具各自引入再加上自己專屬的設定，至少不用每次改規則都要改好幾個地方。

## 實用的 Context 管控技巧

前面聊了很多問題，這邊整理一些實際上可以用的技巧。

### 善用 Subagent 做探索

如果你需要 Claude 去做一些探索性的工作，像是「幫我看一下這個 codebase 的架構」這種，建議用 Subagent 來做。什麼意思呢？Subagent 會有自己獨立的 Context Window，它可以讀了一堆檔案做完分析之後只把一個精簡的摘要回傳給你的主對話，這樣你的主 Context 就不會被一堆檔案內容塞爆。

### 用路徑限定規則

把只跟特定目錄有關的規則放到 `.claude/rules/` 裡面，用 `paths` 來限定什麼時候才載入：

```markdown
---
paths:
  - src/api/**
---

API 路由一律使用 RESTful 風格，回傳格式統一用 { data, error, message }。
```

這樣只有在 Claude 存取 `src/api/` 底下的檔案時才會載入這條規則，平常不會佔 Context。

### 用 Plan Mode 省 Token

按兩次 `Shift+Tab` 可以切換到 Plan Mode，在這個模式下 Claude 會先把計畫想好才開始動手，通常可以減少不少 Token 的消耗，因為它不會一邊做一邊試錯。

### 在 CLAUDE.md 加入進度存檔的規則

前面有提到手動存檔 + 重開對話的做法，你可以在 CLAUDE.md 裡面加一段規則，讓 Claude 在每次被要求存檔的時候都知道要保留哪些資訊：

```markdown
## 進度存檔規則

當被要求存檔時，請將以下資訊整理成 Markdown 存到 .claude/progress.md：
- 目前正在進行的任務描述
- 已修改的檔案清單
- 尚未完成的待辦事項
- 測試指令跟測試結果
- 任何需要注意的踩坑點
```

這樣每次你需要清理 Context 的時候，只要跟 Claude 說「存檔」它就知道該怎麼做了。

### 用 permissions.deny 排除不需要的檔案

如果你有一些檔案不想讓 Claude Code 讀取，像是 `.env` 、 `secrets/` 、 `build/` 這類敏感 or 不需要的檔案，可以在 `.claude/settings.json` 裡面用 `permissions.deny` 來排除：

```json
{
  "permissions": {
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Read(./secrets/**)",
      "Read(./build)"
    ]
  }
}
```

被排除的檔案會從搜尋結果中消失 Claude 也無法讀取，這樣就可以避免不相關的檔案內容佔掉你的 Context。

### 用 Status Line 即時監控

如果你想要隨時知道目前 Context 用了多少，可以設定 Status Line 在終端機底部顯示即時的使用率。之前在 [Claude Code Status Line 設定教學](https://israynotarray.com/ai/20260330/1992438587/) 有詳細的介紹。

## 總結

最後整理一下這篇的重點：

- **AGENTS.md** 是一個跨工具的通用標準，團隊混用多種 AI 工具時可以考慮使用，可是 Claude Code 不會自動讀取，需要在 CLAUDE.md 裡面用 `@AGENTS.md` 引入
- **CLAUDE.md 控制在 200 行以內** ，只放 Claude 無法從程式碼推斷的東西
- **定期用 `/context` 檢查** Token 使用狀況
- **Context 到六成 or 功能做完就手動存檔 + `/clear` 重開** ，不要依賴 Auto Compaction
- **用不到的 MCP Server 先斷開** ，用 `/mcp` 管理連線
- **手動觸發的 Skill 加上 `disable-model-invocation: true`** 讓它不佔 Context
- **善用 Subagent** 做探索性的工作，避免把主 Context 塞爆
- **用路徑限定規則** 、 **Plan Mode** 、 **permissions.deny** 來精準控制 Context 的使用

希望這篇有讓你對 Context 管控有更近一步的認識，畢竟 Token 不便宜，能省就省哩～

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