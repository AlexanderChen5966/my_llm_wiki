---
title: "Claude Code 哪些檔案該 Commit？CLAUDE.md、Settings、Rules、Skills 完整解析"
source: "https://israynotarray.com/ai/20260331/1748203647/"
author:
  - "[[Ray]]"
published: 2026-03-31
created: 2026-05-13
description: "用 Claude Code 開發之後，專案裡多了 CLAUDE.md、.claude/settings.json、rules、skills 等一堆檔案，到底哪些該 commit？哪些是個人設定不該推上去？這篇從 Claude Code 的多層設定架構跟讀取順序切入，幫你搞懂個人跟團隊的分界線。"
tags:
  - "clippings"
status: processed
---
![Claude Code 哪些檔案該 Commit？CLAUDE.md、Settings、Rules 完整解析](https://images.israynotarray.com/blog/ai/claude-code-commit/Gemini_Generated_Image_qzcindqzcindqzci_1280x684.png)

Claude Code 哪些檔案該 Commit？CLAUDE.md、Settings、Rules 完整解析

## 前言

如果你已經在用 Claude Code 做開發，應該有遇過一個困擾：專案裡突然多了一堆以前沒看過的檔案，CLAUDE.md、`.claude/settings.json` 、`.claude/rules/` 之類的，然後你就開始猶豫這些東西到底該不該 commit。

以我自己來講，之前在團隊專案中第一次用 Claude Code 的時候，就很猶豫 CLAUDE.md 該不該推上去，畢竟隊友不一定也在用 Claude Code，推上去會不會很奇怪？後來又發現專案裡面還有 `settings.json` 跟 `settings.local.json` ，兩個看起來很像，到底差在哪？

後來我去研究了一下 Claude Code 的設定架構才發現，其實它的設計已經幫你把「團隊共用」跟「個人偏好」分得很清楚了，只要搞懂它的層級關係，該不該 commit 根本不用猶豫。

所以這篇就來聊一下 Claude Code 的設定檔層級跟讀取順序，搞懂之後你就知道每個檔案的定位了。

## Claude Code 的設定層級

在講個別檔案之前，先來了解一下 Claude Code 的整體設定架構，因為這是判斷該不該 commit 的關鍵。

Claude Code 的設定不是只有一層，而是分成四個層級：

| 層級 | 誰來設定 | 作用範圍 |
| --- | --- | --- |
| Managed（組織層） | IT / DevOps 管理員 | 整個組織的所有人 |
| User（使用者層） | 你自己 | 你所有的專案 |
| Project（專案層） | 團隊 | 這個專案的所有人 |
| Local（本機層） | 你自己 | 這個專案，只有你 |

簡單來講，Managed 是公司管理員設定的強制規則，User 是你個人跨專案的偏好，Project 是團隊共用的專案規範，Local 是你個人在特定專案的設定。

這個層級架構貫穿了 CLAUDE.md、Settings、Rules 三大設定系統，所以只要記住一個原則：放在專案目錄裡面、給團隊共用的就 commit，放在你 Home 目錄或帶 `local` 字樣的就不 commit。

接下來就用這個架構來看每個檔案。

## CLAUDE.md 的讀取順序

CLAUDE.md 是 Claude Code 最核心的規則文件，但它不是只能放在專案根目錄，你可以在好幾個地方都放一份，Claude 啟動時會全部讀進來合併使用。

這邊直接用一個實際的例子來說明比較好懂。假設你在 `my-project/packages/api/` 底下執行 Claude Code，它會依序載入以下的 CLAUDE.md：

1. Managed 層級（如果公司有設定的話）
2. `~/.claude/CLAUDE.md` （你的使用者層級）
3. `my-project/CLAUDE.md` （專案根目錄）
4. `my-project/packages/CLAUDE.md` （中間目錄，如果有的話）
5. `my-project/packages/api/CLAUDE.md` （你執行指令的目錄）

用圖來看會更清楚：

![Claude Code CLAUDE.md 讀取順序圖示](https://images.israynotarray.com/blog/ai/claude-code-commit/CLAUDE%20Repository-2026-03-31-031244.png)

Claude Code CLAUDE.md 讀取順序圖示

從左到右就是載入順序，越右邊優先權越高，綠色也越亮。所以如果你的使用者層級 CLAUDE.md 寫了「回應用英文」，但專案的 CLAUDE.md 寫了「回應用繁體中文」，Claude 會聽專案的。

另外，子目錄底下的 CLAUDE.md（比如 `src/components/CLAUDE.md` ）不會在啟動時就全部載入，而是 Claude 在讀取該目錄的檔案時才會動態載入，避免一開始就把整個專案的 CLAUDE.md 全部灌進 Context。

那哪些該 commit 呢？很簡單，放在專案裡面的 CLAUDE.md 都該 commit，不管是根目錄的還是子目錄的，因為那是整個團隊共用的規範。使用者層級的 `~/.claude/CLAUDE.md` 本來就在你的 Home 目錄，不在 Repo 裡面，所以不用擔心。

如果你不知道怎麼寫 CLAUDE.md，可以看我之前寫的 [CLAUDE.md 與 Rules for AI 撰寫技巧](https://israynotarray.com/ai/20260310/3737444063/) 。

### 那我的個人偏好要寫在哪？

這是很多人會有的疑問：專案已經有一份 CLAUDE.md 了，那我自己的偏好要放哪裡？直接改專案的 CLAUDE.md 嗎？

不要這樣做。個人偏好應該寫在 `~/.claude/CLAUDE.md` ，這個檔案是你的使用者層級設定，適用於你所有的專案，而且不在任何 Repo 裡面，不會影響到隊友。

舉個例子，專案的 CLAUDE.md 可能長這樣：

```markdown
# My Project

- 使用 pnpm 作為套件管理工具
- commit message 使用 Conventional Commits 格式
```

而你個人的 `~/.claude/CLAUDE.md` 可以寫你自己的偏好：

```markdown
# 個人偏好

- 一律使用繁體中文回應
- 不要在回應中使用 emoji
- 解釋程式碼時用簡單的方式說明
```

Claude 會兩份都讀，團隊規範跟個人偏好同時生效。如果兩邊有衝突，專案層級的優先權比較高，所以不用擔心你的個人偏好會蓋掉團隊規範。

## Settings 的合併機制

Settings 的概念跟 CLAUDE.md 類似，也是有分層級的，但多了一個 Local 層。

以實際的檔案來看：

- `~/.claude/settings.json` → 你個人的全域設定（User 層）
- `.claude/settings.json` → 團隊共用的專案設定（Project 層）
- `.claude/settings.local.json` → 你個人在這個專案的設定（Local 層）

優先權從高到低是：Managed > Local > Project > User。

![Claude Code Settings 層級圖示](https://images.israynotarray.com/blog/ai/claude-code-commit/CLAUDE%20Repository-2026-03-31-031307.png)

Claude Code Settings 層級圖示

這邊有一個滿重要的細節，就是 Settings 的陣列值不是覆蓋，而是合併。

舉個例子比較好懂，假設專案的 `.claude/settings.json` 允許了兩個指令：

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run lint)",
      "Bash(npm run test)"
    ]
  }
}
```

然後你個人的 `~/.claude/settings.json` 多允許了一個：

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run dev)"
    ]
  }
}
```

Claude Code 不會用專案設定蓋掉你個人的，而是把兩邊合併起來，所以你最後會同時擁有 `npm run lint` 、 `npm run test` 、 `npm run dev` 三個允許的指令。

這個設計讓團隊可以設定共用的基礎權限，每個人再根據自己的需求額外加上去就好，不會互相干擾。

那哪個該 commit 呢？`.claude/settings.json` 是團隊共用的，該 commit。`.claude/settings.local.json` 是你個人的，不該 commit，而且如果你是用 `claude init` 初始化的話，它已經自動幫你加進 `.gitignore` 了。

## Rules 目錄

Rules 是 Claude Code 的模組化規則系統，讓你把規則拆成多個 `.md` 檔案放在 `.claude/rules/` 底下，比全部塞在一個 CLAUDE.md 裡面好管理。

跟 CLAUDE.md 一樣，Rules 也有分層級：

- `~/.claude/rules/*.md` → 你個人在所有專案通用的規則
- `.claude/rules/*.md` → 團隊共用的專案規則

使用者層級的 Rules 先載入，專案層級的後載入且優先權更高。

Rules 有一個比較特別的功能是可以透過 `paths` 來限定適用範圍：

```markdown
---
paths:
  - "src/api/**/*.ts"
---
# API 開發規範

- 所有 API 呼叫都必須使用 try-catch 包裹
```

沒有設定 `paths` 的 Rules 會在啟動時全部載入，有設定的只會在 Claude 處理到匹配路徑的檔案時才載入，省 Token 也避免不相關的規則干擾。

專案的 `.claude/rules/*.md` 該 commit，個人的 `~/.claude/rules/` 在 Home 目錄不用管。

## Skills 目錄

Skills 是 Claude Code 的技能包系統，讓你把特定任務的知識跟流程打包成一個獨立的 Skill，Claude 在遇到相關需求時會自動載入使用。

跟 Rules 一樣，Skills 也分兩個層級：

- `~/.claude/skills/` → 你個人在所有專案通用的 Skills
- `.claude/skills/` → 團隊共用的專案 Skills

每個 Skill 是一個資料夾，裡面至少要有一個 `SKILL.md` ：

```
.claude/
└── skills/
    └── code-review/
        └── SKILL.md
```

以團隊來講，像是 Code Review 的標準流程、部署前的檢查清單、或是特定框架的開發規範，這些東西做成 Skill 放在專案的 `.claude/skills/` 底下，整個團隊都能用到，所以該 commit。

而如果是你個人的 Skill（比如你自己習慣的 Debug 流程），放在 `~/.claude/skills/` 就好，不在 Repo 裡面。

如果你想了解 Skills 的細節跟寫法，可以參考 [比 MCP 更省 Token 的 Agent Skills](https://israynotarray.com/ai/20260120/4189755271/) 。

## Auto Memory

Auto Memory 的檔案存在 `~/.claude/projects/<專案>/memory/` 底下，也就是你的 Home 目錄，本來就不在 Repo 裡面，根本不會出現在 `git status` 中，所以完全不用處理。

如果你想了解 Auto Memory 的細節，可以參考 [Claude Code Auto Memory 完整教學](https://israynotarray.com/ai/20260330/2896397295/) 。

## 隊友不用 Claude Code 怎麼辦？

這應該是很多人會擔心的問題：我把 CLAUDE.md 跟 `.claude/` 推上去了，但隊友根本沒在用 Claude Code，會不會有影響？

完全不會。CLAUDE.md 對不用 Claude Code 的人來說就只是一個普通的 Markdown 檔案，`.claude/settings.json` 也只是一個靜靜躺在那邊的 JSON，不會對任何開發工具產生副作用。

反過來，把這些東西 commit 進去還有一個好處：如果隊友哪天想試試 Claude Code，clone 下來就能直接用現成的規則，不用從零開始設定。

## 推薦的.gitignore 設定

其實需要加的只有一行：

```gitignore
# Claude Code - 個人設定
.claude/settings.local.json
```

其他個人相關的東西（ `~/.claude/CLAUDE.md` 、 `~/.claude/settings.json` 、 `~/.claude/rules/` 、Auto Memory）全部都在 Home 目錄，不在 Repo 裡面，根本不需要加進 `.gitignore` 。

如果你是用 `claude init` 初始化的話，這行通常已經幫你加好了。

## 敏感資訊的提醒

最後提醒一下，不管是 CLAUDE.md 還是 Rules，千萬不要在裡面放 API Key、Token、密碼之類的東西。

這些設定檔本身不太會有這個問題，因為它們放的主要是規則跟偏好，但如果你在 CLAUDE.md 裡面寫了「使用這個 API Key 去呼叫 XXX」的話，那就危險了。

如果真的需要在規則中參考敏感資訊，用環境變數就好，在規則裡面寫「使用環境變數 `XXX_API_KEY` 」，不要把實際的值寫進去。

## 結語

回到最開始的問題，Claude Code 的設定架構其實已經幫你把團隊跟個人分得很清楚了。放在專案 `.claude/` 底下的 `settings.json` 、 `rules/` 、 `skills/` ，還有專案裡面的 CLAUDE.md，這些都是團隊共用的，commit 就對了。帶 `local` 的 `settings.local.json` 是你個人的，不要 commit。至於 `~/.claude/` 底下的東西全部都在 Home 目錄，跟 Git 無關。

個人偏好的部分也不用硬塞進專案的設定裡，Claude Code 的使用者層級（ `~/.claude/CLAUDE.md` 、 `~/.claude/settings.json` 、 `~/.claude/rules/` ）就是設計給你放個人偏好用的，寫在那邊就好，所有專案都能吃到，也不會影響隊友。

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