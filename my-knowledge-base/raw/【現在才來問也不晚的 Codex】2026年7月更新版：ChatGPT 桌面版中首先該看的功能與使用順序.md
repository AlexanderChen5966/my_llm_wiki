---
title: "【現在才來問也不晚的 Codex】2026年7月更新版：ChatGPT 桌面版中首先該看的功能與使用順序"
source: "https://codelove.tw/@tony/post/apo5B3"
author:
published:
created: 2026-07-14
description: "[](#%E3%81%93%E3%81%AE%E8%A8%98%E4%BA%8B%E3%81%AE%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88)本文重點-----------------------------------------------------------..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月13日](https://codelove.tw/@tony/post/apo5B3 "2026-07-13 17:30:10")

## 【現在才來問也不晚的 Codex】2026年7月更新版：ChatGPT 桌面版中首先該看的功能與使用順序

## 本文重點

- 2026 年 7 月的更新中，ChatGPT 桌面版朝向把 **一般聊天 / Work / Codex** 整合在一起。Work 正在分批推出中
- 開始使用 Codex 的順序，建議先從 **專案・任務・工作階段 -> 資料夾 / 瀏覽器 -> 稽核 -> 技能 / 外掛程式** 這樣走最自然
- **技能 / 外掛程式** 非常重要，但不是第一天就該全部設定好的功能。先完成一個小任務再說
- 技能主要放在 `.agents/skills` ，Codex 的設定、歷史與工作階段通常在 `~/.codex` 。不過這些位置可能包含驗證資訊與對話紀錄，請不要隨意分享
- GPT-Live 是有趣的新功能，但它不是 Codex 桌面版本體，而是作為 Web / 行動版 ChatGPT Voice 的附加體驗

## 1\. 導入：大型更新後，首先要看什麼？

你好，我是 AI 大叔。

2026 年 7 月 9 日，OpenAI 為新版 ChatGPT 桌面版推出了整合一般聊天、Work、Codex 的更新。這裡要注意的是，這三者不一定會一直以並排分頁的形式出現。一般聊天從左側導覽列的 **Quick chat** 開始，Work 與 Codex 則從左上角的模式切換選擇。由於 Work 正在分批推出，有些帳號可能還看不到。 [ChatGPT 的更新說明](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) 與 [ChatGPT Work and Codex](https://help.openai.com/en/articles/20001275)

> **新版 Codex 桌面版，是把諮詢、本地開發、瀏覽器驗證與稽核整合起來的「AI 工作管制塔」。**

不需要把所有按鈕都背起來。先掌握使用順序，之後再把需要的功能補上。

> 注意：可用方案、地區、工作空間設定與應用程式的分批推出狀態不同，畫面上顯示的項目與名稱也會不一樣。本文以 2026 年 7 月 13 日的官方資訊為基礎。

### GPT-5.6 Sol / Terra / Luna 簡易對照表

只看名字很難選，先依工作性質分類就不容易迷路。

模型一句話說明適合的工作 **Sol** 最擅長深度思考的主力模型複雜實作、模糊問題、調查分析、設計、最終稽核 **Terra** 日常工作平衡型一般開發、多工具協作、不需要 Sol 那麼深的工作 **Luna** 快速且便宜地處理明確重複工作擷取、分類、轉換、結構化摘要、大量制式處理

不知道怎麼選時，先從 Sol / 標準推論開始；日常作業用 Terra；輸入輸出都很明確的大量處理則偏向 Luna。可用模型與設定會依方案與分批推出狀態而異。 [Models](https://learn.chatgpt.com/docs/models)

[![左側顯示專案與功能選單、右側顯示瀏覽器的 Codex 桌面版](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2F24487a4c-a7c9-4a47-abcb-89ffe69ecda3.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=469566dfadbacd7794ae4cea233389e5)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2F24487a4c-a7c9-4a47-abcb-89ffe69ecda3.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=469566dfadbacd7794ae4cea233389e5)

左側側邊欄會列出新的任務、排程、外掛程式、網站、拉取請求、聊天與專案清單。右側側邊面板是瀏覽器，可開啟正在處理的網站來確認與加註。

## 2\. 概念解說：聊天 / Work / Codex 有什麼不同？

先把這三個角色分開，思緒會清楚很多。這不是畫面上的分頁結構，而是用途上的整理。

場所一句話說明適合做的事 **聊天** 用來對話的地方提問、腦力激盪、簡短草稿、輕量諮詢 **Work** 用來交辦的地方調查、分析、文件、表格、產出成果 **Codex** 用來打造、修正、驗證的地方程式碼、本機檔案、測試、Git、PR、瀏覽器確認

可以把聊天想成跟同事隨口問一句；Work 是把需求書交出去，讓對方做出成果；Codex 則是把檔案、終端機、瀏覽器與稽核畫面都備齊的開發工作台。

### Codex 是 Cursor 或 Claude Code 的替代品嗎？

是否能完全取代，要看你的使用方式。

如果你本來就有在用 Cursor 或 Claude Code，Codex 桌面版的價值不只是「多一個終端機」。它的重點在於： **保留專案與工作階段、在瀏覽器中確認、檢視差異與 PR、用技能 / 外掛程式重用工作流程** 。

擅長 CLI 的人，繼續用 Claude Code 或 Codex CLI 也沒問題。把 Codex 桌面版看成負責前後確認、延續與共享的整合介面，會更合理。

### 首先要記住的順序，和實務中的重要性不同

這點很重要。技能 / 外掛程式是長期來看超重要的功能，但如果第一天就從設定開始，往往會在還沒弄懂它到底方便在哪之前就結束了。

最先接觸的順序目的1. 專案 / 任務 / 工作階段知道作業放哪裡，以及如何從上次繼續2. 資料夾 / 工作目錄決定要讓 Codex 看哪些檔案3. 小型請求例如：查測試失敗原因、修 README4. 差異 / 稽核確認 AI 改了什麼5. 側邊面板 / 瀏覽器邊看檔案邊用實際畫面驗證6. 技能 / 外掛程式把成功流程變成可重複使用7. 模型 / 推論 / 速度調整難度、等待時間與成本8. 終端機 / 排程依照既有開發流程與定期工作補上

我個人認為的實務優先順序是： **專案 / 工作階段、技能 / 外掛程式、資料夾 / 瀏覽器、差異 / PR 稽核** ，放在最前面。

## 3\. 實踐：分清案件入口，分別使用

### 3-1. 左側專案：先創建工作區

專案是把相關的聊天、任務、檔案、指示與參考資料統整在一起的容器。 [Projects 官方指南](https://learn.chatgpt.com/docs/projects) 說得很清楚，若是同一個主題或同一個程式碼庫會反覆處理，先整理進專案會是正確做法。

像「文章撰寫」「自家產品 A」「徵才網站改版」這類，請依工作單位來切分。這樣可以減少每次都從零開始說明的次數。

但要注意，ChatGPT 的專案和電腦上的資料夾、也就是 **本機專案** ，並不是同一件事。前者是把對話與資料脈絡整理在一起的容器，後者則是讓 Codex 能處理本機工作目標的入口。如果要動到 repository，第一件安全措施就是別選錯目標資料夾。

### 3-2. 新增任務與工作階段歷史：不是新增聊天

任務要依成果單位切分。不過， **不需要因為對話變長，就一定要切成新任務。**

- 修正登入問題
- 回應 PR 的稽核留言
- 調查本週的規格變更
- 撰寫 Qiita 文章草稿

像這樣，只要完成條件不同，就應該分成不同任務。同一專案中建立的話，共用資料與指示會更容易。反過來說，如果你還在深入同一個問題，延續同一個工作階段會更好，因為前面的調查、判斷與差異都能接續下去。

[![不破壞延續工作的 Codex 運用方式](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2F6f532987-462d-46d6-8069-95f0d2acb066.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=275879bedda003b4f73bee14fcc109e7)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2F6f532987-462d-46d6-8069-95f0d2acb066.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=275879bedda003b4f73bee14fcc109e7)

#### 長度很長的任務，與其截斷，不如壓縮後繼續

在長任務中，舊的嘗試與失敗會混在一起，判斷容易變鈍。雖然有自動壓縮，但建議在節點使用 `/compact` ，只保留重要前提後再繼續。 [Developer commands](https://learn.chatgpt.com/docs/developer-commands)

情境建議動作目的相同，只是調查或實作拖得很長的同一任務持續進行，並在節點使用 `/compact` 完成條件或目標資料夾改變了建立新任務想花數小時以上推進同一個大目標用 `/goal` 明確標示完成條件，並在同一任務中持續進度想試另一個方案，但也想保留原本脈絡用 `/fork` 分支這裡是和 Claude Code 容易混淆的地方。 **不是沒有 `/clear` ，而是 CLI 才有。** CLI 的 `/clear` 會重置畫面與任務脈絡，開始新任務；而 ChatGPT 桌面版目前的命令清單沒有 `/clear` ，而是設計成使用 `/compact` 、 `/fork` 與新任務來區分。

過去的任務與聊天不只是歷史紀錄，而是保留上次判斷、試過的指令與變更差異的「工作日誌」。如果迷路了，先搜尋歷史，不要急著亂開新任務。

#### 工作階段是存在哪裡？

Codex 本機狀態的根目錄是 `CODEX_HOME` ，通常是 `~/.codex` 。常見位置如下。 [環境變數官方參考](https://learn.chatgpt.com/docs/config-file/environment-variables) 與 [疑難排解](https://learn.chatgpt.com/docs/reference/troubleshooting) 可以查到相關資訊。

內容代表性位置工作階段紀錄 `~/.codex/sessions` 已封存工作階段 `~/.codex/archived_sessions` 歷史 `~/.codex/history.jsonl` 記憶 `~/.codex/memories` 設定、認證、內部狀態 `~/.codex/` 底下這些地方可能包含對話紀錄與驗證資訊。尤其像 `auth.json` 這類檔案，不要放進 Git，也不要出現在截圖裡。知道存放位置很重要，但直接手動編輯，建議先用 UI 或官方命令替代。

### 3-3. 側邊面板：資料夾與瀏覽器會改變體驗

側邊面板這個說法會因版本不同而有些微差異。這裡指的是橫向顯示的 **檔案、計畫 / 摘要、稽核、瀏覽器、輔助任務** 。

我個人覺得 Codex 桌面版比 CLI 更好用的最大原因就在這裡。它不只是把工作交給 AI，而是可以像 **IDE 一樣邊看檔案邊下指令** ，對正在看的位置直接說明需求。

這不是能力高低的問題，而是工作相性的問題。比起只靠終端機口頭描述檔案路徑與畫面狀況，能直接一邊看 Markdown、規格書與原始碼，一邊請它「修這段」「確認這個函式的意圖」，對包含非工程師在內的許多人來說都更好操作。

特別有用的是下面兩個：

- **資料夾 / 檔案** ：可以在查看 Markdown、規格書、原始碼的同時，針對目標直接下指令，不會失焦
- **瀏覽器** ：可直接打開實際畫面，並用註解標出你想改的地方

瀏覽器的優勢在於 **註解** 。打開畫面後可以直接指定修改位置，幾乎不需要再用文字描述「右上角那個按鈕」，也不用在截圖上畫紅圈。 [瀏覽器官方指南](https://learn.chatgpt.com/docs/browser) 說明了瀏覽器外掛、與一般瀏覽器不同的個人檔案、以及高風險操作會要求確認等事項。

```
請用 @Browser 開啟 http://localhost:3000。
以手機寬度檢查設定畫面，並在版面跑掉的地方加上註解。
只針對註解位置做最小修正，然後在同一個畫面重新確認，並簡短整理變更內容。
```

用了瀏覽器之後，流程會從「看起來有修」進步到「在自己的畫面上，指著需要修的地方確認過」。

[![在專案中一邊看 Markdown 稿一邊下指令的範例](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2F1f545c34-6148-4be5-9194-b48570e237ab.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d8e62c9d78b427273a97b5e1d59eb9c3)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2F1f545c34-6148-4be5-9194-b48570e237ab.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d8e62c9d78b427273a97b5e1d59eb9c3)

### 3-4. 摘要顯示切換、下方介面、終端機

畫面上的「摘要」至少容易混淆成兩種意思。

1. **推論摘要** ：AI 在過程中顯示多少簡短的思考內容
2. **變更摘要 / diff 範圍** ：這次只看本次回合，還是看整個任務的差異

前者可以透過設定 `model_reasoning_summary` 調整為 `auto` 、 `concise` 、 `detailed` 、 `none` 。 [設定參考](https://learn.chatgpt.com/docs/config-file/config-reference) 顯示，如果你覺得顯示太長，也可以改短。

下方介面主要是顯示終端機的地方，可以查看測試、lint、開發伺服器與 Git 指令的輸出。整合式終端機會和任務 / 專案 / worktree 的工作脈絡連動，因此不必每次都複製貼上錯誤內容，也更容易共享狀況。 [整合式終端機](https://learn.chatgpt.com/docs/integrated-terminal) 也有說明 `Cmd/Ctrl + J` 與 `Ctrl + backtick` 的快捷鍵。

但終端機不是每個人都必須使用。若你原本就是用 Claude Code 或 IDE 內建終端機工作，也不必強行整合成一套。Codex 桌面版完全可以偏向瀏覽器、差異檢視與 PR 處理來使用。

### 3-5. 稽核畫面與拉取請求：務必看 AI 的變更

這裡是 **只要拿 Codex 做程式開發或團隊開發，就必不可少** 的功能。AI 越會寫程式，越不能省略稽核。

不過，並非所有人都在做開發。如果你主要是寫文章、做調查、寫文件、搭網站雛形，那先把檔案與瀏覽器用好就很夠了。稽核畫面與拉取請求，則可以理解為「進入使用 Git 管理變更、並與他人交換程式碼」之後才需要掌握的功能。

稽核畫面是用來檢視 Git 變更差異、留言、stage / unstage、revert，以及用 `/review` 做自我稽核的地方。 [Code review 官方指南](https://learn.chatgpt.com/docs/code-review) 有說明，Codex 可能也會看到它沒有碰過的本機工作樹變更。這裡要特別注意，不要把自己或同事尚未提交的變更，和 AI 的變更混在一起。

在拉取請求中，可以一邊看 PR 的脈絡、差異與稽核留言，一邊直接請它修正。GitHub 端也有 `@codex review` 的請求方式。 [GitHub 連動官方指南](https://learn.chatgpt.com/docs/third-party/github) 請先確認，必要時安裝並完成 `gh` 的登入驗證。

```
請檢查這個 PR 的稽核留言。
先列出 P1 等級的指摘中可以處理的候選項目，並在修正前等我確認。
修正後請簡短整理 diff、影響範圍與測試結果。
```

與其「讓它寫」，不如「一起看差異」。這個習慣，是長期安全使用 Codex 的關鍵。

### 3-6. 技能與外掛程式：把自己的工作變成可重用

[![技能與外掛程式的分工](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2Fd1f56450-6490-4665-840a-4e6b8d62bdf7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=df558736dea04faed2a4b3366debc6fe)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2Fd1f56450-6490-4665-840a-4e6b8d62bdf7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=df558736dea04faed2a4b3366debc6fe)

當工作開始重複時，就是技能與外掛程式上場的時候。第一次要用的話，建議從 Codex 內建的 **技能建立器（ `$skill-creator` ）** 開始。它會一邊問你想自動化什麼，一邊幫你寫出 `SKILL.md` 草稿。 [Build skills](https://learn.chatgpt.com/docs/build-skills)

技能是可重用的流程說明書，外掛程式則是把技能或外部連線一起封裝發送的套件。先從像「每週報告要用這個格式摘要」這種單一制式作業開始就好。 [Skills & Plugins](https://learn.chatgpt.com/docs/skills-and-plugins)

外掛程式不需要一開始就從自製 SaaS API 串接下手。建議先裝一個 **官方提供的外掛程式** ，體驗一下它新增了什麼能力。之後若真的有需要，再透過 MCP 連到 SaaS API 或內部工具。外掛程式雖然方便，但權限也會跟著擴大，因此一定要確認提供者與要求的權限。

用語大致角色技能教你如何進行工作的手冊外掛程式包含技能與連線功能的發佈套件應用程式 / MCP連接外部資料或外部操作的介面#### 技能保存在哪裡？`.agents` 與 `.codex` 有什麼不同

先記住這樣就夠了。

```
~/.agents/skills/              # 給自己使用的技能
<repo>/.agents/skills/         # 在 repository 中共用的技能
~/.codex/                      # Codex 的設定、歷史、工作階段、驗證資訊等狀態
```

官方的搜尋對象包含：repository 的 `$REPO_ROOT/.agents/skills` 、個人的 `$HOME/.agents/skills` 、系統的 `/etc/codex/skills` ，以及 OpenAI 內建技能。 [Build skills](https://learn.chatgpt.com/docs/build-skills) 有完整整理。

`.agents` 是放「工作型態」的架子；`.codex` 是放「Codex 本身的東西」的架子。後者可能包含機密資訊與內部狀態，所以不要像管理技能資料夾那樣隨便編輯或共享。

第一個要建立的技能不必很大。像「PR 稽核用」「部落格草稿用」「事故報告摘要用」這種每月會重複好幾次的工作，挑一個先做就好。

#### Claude Code 的技能能接續過來嗎？

可以。雖然也可以手動把 `.claude` 底下的技能複製過來，但 **建議先試官方的匯入功能** 。在 ChatGPT 桌面版的 `Settings > Import` 裡，可以從相容的代理程式選擇要繼承的內容，包括 instructions、設定、技能、外掛程式、專案、最近任務、MCP 設定、hooks、子代理程式等。原本 Claude Code 端的設定不會被修改或刪除。 [Import from another agent](https://learn.chatgpt.com/docs/import)

如果要手動搬移，建議不要一次把 `.claude/skills` 整包搬過去，而是逐一放到 `.agents/skills` 測試。因為路徑、引用目標、外部指令與權限設定，有時還會殘留 Claude Code 的特定前提。

### 3-7. 模型選擇、推論等級、速度：一開始用預設就好

[![模型、推論與速度的選擇方式](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2Fdaa86236-e0fe-4108-a3ec-9f02d455edba.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=0db0284614038ff1f2c26b6b44db1cab)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2Fdaa86236-e0fe-4108-a3ec-9f02d455edba.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=0db0284614038ff1f2c26b6b44db1cab)

[![設定模型、推論等級與速度的畫面](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2Fc0c1b57b-1c98-4bfb-9f3c-8ccd5f24fd3a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=48846003df94ac23c9d18aa0aa5a98e8)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4341543%2Fc0c1b57b-1c98-4bfb-9f3c-8ccd5f24fd3a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=48846003df94ac23c9d18aa0aa5a98e8)

這三個看起來只是旋鈕，實際上很容易讓人猶豫。

項目是什麼？一開始怎麼看？模型找誰幫忙？複雜工作用強模型，重複工作用輕模型推論等級讓它想多深想不出來時先用標準。設計、調查、難解 bug 時再提高速度 / Fast 模式讓它多快完成用在小任務或想減少等待的情況模型輸出的差異每次現場判定，其實非常難。我認為比較實際的做法，是依流程先訂一個大略規則。

流程先選的依據調查、分析、規劃、稽核最上位模型，推論也開高一點實作、修改中位到上位模型，只在難點提高推論例行工作、大量單調處理下位模型，把輸入與輸出格式講清楚

這不是絕對正解，而是用來依任務重要度、失敗成本與等待時間調整的暫定交通整理。官方也建議從能達成所需結果的最低推論等級開始，在需要規劃、分析、確認時再往上調。 [Models](https://learn.chatgpt.com/docs/models)

#### Fast 模式是奢侈品嗎？

我認為， **基本上把它當作奢侈品就好** 。Fast 模式不是提高品質或推論能力的功能，而是透過額外消耗 credit 讓支援的模型跑得更快。依照目前官方說明，它比標準快 1.5 倍，而 GPT-5.5 會消耗 2.5 倍 credit，GPT-5.4 則是 2 倍。 [Speed](https://learn.chatgpt.com/docs/agent-configuration/speed)

也就是說，這是在用金錢換等待時間。對於截止前的短反覆修正、或是需要在本機反覆確認的工作很有價值。相反地，若是背景執行、長時間調查、或單純大量處理，先考慮像 Luna 這樣較輕的模型，或調整推論等級，通常成本效益會更好。

### 3-8. 排程與網站：等工作穩定之後再使用

**排程** 適合單次預約、定期執行、監控型工作。像每天早上的差異摘要、每週的相依性檢查、每週 PR 整理都可以考慮。 [Automations](https://learn.chatgpt.com/docs/automations) 有更詳細的說明。

不過，定期化要等手動任務先穩定之後再做。每次都得說明「今天的狀況」才能動作的提示詞，很快就會過時。定期任務應該把輸入、判斷條件、輸出格式與異常處理都明確寫出來。若使用本機檔案，電腦與應用程式必須保持開啟。

```
每週一早上 9 點，請檢查這個 repository 尚未處理的 Issue 與最近的 PR。
請整理出本週應著手的候選項目，最多 5 項；若沒有適合的項目，請回報「沒有重大更新」。
```

**網站（Sites）** 是用來託管與分享 ChatGPT 建立的網站或 Web App 的功能。當你想把原型、內部工具或 LP 公開出去時很方便。 [Sites 官方指南](https://learn.chatgpt.com/docs/sites) 說明這是公開測試版，所以方案與設定可能會有差異。公開網址要當成正式網址看待，不要把秘密值寫進提示詞或設定檔。

### 3-9. Computer Use、worktree 與側邊任務：限制使用場景

**Computer Use** 是一邊看 GUI 應用程式、一邊進行點擊與輸入的功能。如果有瀏覽器或 API / MCP 這類結構化連線，優先用那些。只有在真的需要直接操作畫面時才使用。macOS 有螢幕錄製與輔助使用權限限制，Windows 則是只能操作前景桌面。 [Computer Use](https://learn.chatgpt.com/docs/computer-use) 請先確認。

**worktree** 是從同一個 Git repository 建立獨立的工作區，讓多個變更比較不容易互相衝突的機制。適合大型實作與平行作業。 [Git worktrees](https://learn.chatgpt.com/docs/environments/git-worktrees) 的適用對象是 Git repository。

側邊任務與子代理程式，適合用在把任務切開會更有利的情況。與其一開始就平行化，不如先讓單一任務能順利跑完到稽核，整體往往會更快。

### 3-10. Goal mode 與 Record & Replay：長任務與「示範後學會」的工作

**Goal mode** 是讓工作帶著目的與完成條件前進的功能。像「改善後台搜尋延遲，並完成量測與測試」這類需求，應在一開始就把完成定義交代清楚。工作越大，初始需求中越應該寫明「不要做什麼」與「什麼情況需要先詢問」。

**Record & Replay** 是把你在畫面上做過的一次操作，轉成可重複使用的技能。適合那些很難用文字說明、但流程又固定的工作。這是 2026 年 6 月時 macOS 上的功能，且需要啟用 Computer Use 並符合提供地區條件。 [更新說明](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) 如果環境可用，建議先從「每個月都在同一畫面做的登錄流程」開始試用。

## 4\. 壁壘與解決：初次使用的卡頓與隱藏的營運難題

### 壁壘 1：功能太多，很想全部設定

這很自然。但一開始真正需要的不是設定，而是 **先完成一個小任務** 。

先建立專案、選好資料夾、請它做一個小修改，再用差異與瀏覽器確認。做到這裡，就能看出技能 / 外掛程式的價值了。

### 壁壘 2：技能、外掛程式、MCP 搞混了

回到 3-6 的表格就夠了。只要能判斷：想重用流程就用技能，需要外部資料或操作就用外掛程式 / 應用程式 / MCP，就可以往下做。

### 壁壘 3：AI 修好了，但不知道改了什麼

這時候稽核畫面就派上用場了。從一開始就在需求裡寫上「最小修改」「變更差異」「測試結果」「影響範圍」。

```
請盡量只做最小變更。
修正後，請回報 diff、測試結果與受影響的畫面，最多 3 點。
若有不確定的地方，請在實作前先提問。
```

這不是不信任 AI，而是要把成果整理成讓人類能確認的形式。

### 壁壘 4：模型設定反而變成流程的正解搜尋

老實說，這裡很容易陷入泥沼。

解法是「先以預設值為基準」。失敗了再提高推論等級或模型；只是輕量制式工作，就往下調。Fast 模式不是提升品質的手段，而是只有在需要時才使用、用來縮短等待時間的奢侈品。只要有比較對象，就比較容易判斷。

### 運用難題 1：專案逐漸亂七八糟

老實說，這個問題目前還沒有完美答案。因為專案很方便，所以很容易按工作、驗證、文章各開一個。回過神來清單就會變長，開始花時間找「這個作業到底在哪裡」。

官方的想法很明確。 **只有會持續進行的工作、會產出多個成果的工作、以及需要共用相同檔案或資料的工作，才放進專案。** 單次就結束的調查或小型驗證，不要建專案，直接用單一任務即可。 [Projects, chats, and tasks](https://learn.chatgpt.com/docs/projects)

整理顯示的方法，官方建議是把專案或常用任務釘選、改成能看出成果的短名稱、把完成的任務封存，並使用搜尋。先採用「持續性的才建專案」「單次的用任務」「完成就封存」這三條規則，會改善很多。

不過，目前官方說明裡沒有看到替專案加上資料夾、標籤、階層的功能。我自己也還沒完全整理好。這是產品層面未來還需要解決的問題。

### 運用難題 2：技能與外掛程式愈來愈多

因為方便就一直加；加了之後又忘記；相似的東西越來越多；舊流程或失效連線還留著。這就是技能 / 外掛程式運用的常見問題。

就連我的環境裡，技能與外掛程式也接近 200 個，沒辦法全部掌握與維護。Codex 在技能很多時，會依照上下文量縮短最初提供的清單，甚至省略一部分。 [Build skills](https://learn.chatgpt.com/docs/build-skills) 也就是說，不是「裝越多就一定能隨時更聰明地使用」。

官方有提供不刪除而是停用不需要技能的方法。只要在 `~/.codex/config.toml` 放個別設定，就能把暫停中的技能保留住，同時從初始候選中排除。

```
[[skills.config]]
path = "/path/to/skill/SKILL.md"
enabled = false
```

另外，團隊共用的技能應放在 repository 的 `.agents/skills` ，個人用的則放在 `~/.agents/skills` ，這也是官方預期的分法。至於外掛程式，在組織環境中，管理員可以負責發放、停用與接管連線應用程式的權限。 [Build skills](https://learn.chatgpt.com/docs/build-skills) [Plugins in Codex](https://help.openai.com/en/articles/20001256-plugins-in-codex)

短期內比較實際的做法是：明確標示常用項目並呼叫、把個人用與團隊用分開、每季停用沒在使用的項目。至於「只停用技能型外掛程式」的管理機制，目前還比較弱，這部分也還在發展中。

### 運用難題 3：不知道該選哪個模型才對

每次都想精準比較輸出差異，其實很難。因此目前我都是用「調查、分析、規劃、稽核用最上位」「實作用中位到上位」「例行與大量單調處理用下位」這種依流程區分的方式在跑。

但這仍然不是萬能答案。你可以只挑 5～10 個代表性任務，為不同模型做一個小型評估表，記錄「是否可接受」「重做次數」「所需時間」「credit」。這樣就能慢慢脫離只靠感覺選模型的狀態。每季回顧一次，也比較能跟上模型更新。

## 5\. 總結：最初的 30 分鐘看這些就夠

新版 ChatGPT 桌面版功能很多，因此入口處很容易讓人迷路。

但你不需要一開始就全部用上。

1. 建立一個 **專案**
2. 把一個 **任務** 切小
3. 確認目標 **資料夾 / 工作目錄**
4. 請它修正或調查，並看 **差異 / 稽核**
5. 用 **瀏覽器** 確認實際運作
6. 當你看到會重複的工作，再加入 **技能 / 外掛程式**

先拿你手上正在處理的一個小 bug 修正或 README 更新，完整用 Codex 做一次。若你在過程中覺得「這個流程每次都會用到」，那下一步就是建立技能。從那裡開始，它就會慢慢變成屬於你的工作台。

## 附註：GPT-Live 是給行動端用的嗎？

是的。定位上它是 **不是 Codex 桌面版本體，而是 ChatGPT Voice 的新 Live 體驗** 。

GPT-Live 是以自然雙向對話為目標的語音體驗，正在向 Web 與 iOS / Android 推出。實際用起來，對腦力激盪非常適合。即使思緒還沒整理好也能直接開口，中途插話或改變方向的體驗很有趣。 [ChatGPT Voice 說明](https://help.openai.com/en/articles/20001274) 與 [Introducing GPT-Live](https://openai.com/index/introducing-gpt-live/) 都顯示，它很適合用來邊講邊整理想法、發想點子。

目前，Live 的對話還不能直接接到 Work 或 Codex，也不能在畫面共享背後直接跑代理程式。Live 本身在初期也不支援影片與螢幕共享。 [ChatGPT Voice 說明](https://help.openai.com/en/articles/20001274)

另一方面，從 ChatGPT 行動 App 可以啟動並延續已連接的 Mac / Windows 主機上的 Codex 工作。 [ChatGPT Release Notes](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) 如果未來能順暢地從 Live 把工作交給 Codex，工作方式應該會有很大的變化。這是我個人很期待的下一步。

## 參考的官方資訊

- [ChatGPT Release Notes](https://help.openai.com/en/articles/6825453-chatgpt-release-notes)
- [What’s new in ChatGPT](https://learn.chatgpt.com/docs/whats-new)
- [ChatGPT desktop app](https://learn.chatgpt.com/docs/app)
- [Projects](https://learn.chatgpt.com/docs/projects)
- [Skills & Plugins](https://learn.chatgpt.com/docs/skills-and-plugins)
- [Plugins in Codex](https://help.openai.com/en/articles/20001256-plugins-in-codex)
- [Build skills](https://learn.chatgpt.com/docs/build-skills)
- [Import from another agent](https://learn.chatgpt.com/docs/import)
- [Code review](https://learn.chatgpt.com/docs/code-review)
- [Browser](https://learn.chatgpt.com/docs/browser)
- [Integrated terminal](https://learn.chatgpt.com/docs/integrated-terminal)
- [Models](https://learn.chatgpt.com/docs/models)
- [Speed](https://learn.chatgpt.com/docs/agent-configuration/speed)
- [Developer commands](https://learn.chatgpt.com/docs/developer-commands)
- [Automations](https://learn.chatgpt.com/docs/automations)
- [Sites](https://learn.chatgpt.com/docs/sites)

---

原文出處： [https://qiita.com/TMiyamoto/items/6bd89423d5b38916e48d](https://qiita.com/TMiyamoto/items/6bd89423d5b38916e48d)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/apo5B3) [50瀏覽](https://codelove.tw/@tony/post/apo5B3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/apo5B3)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [Anthropic 為何限制中國大陸使用 Claude？](https://codelove.tw/@tony/post/qO2P8a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝17 💬3 ❤️5

741

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝3 💬6 ❤️4

170

🥉

[流引色群@jptd847utpp](https://codelove.tw/@sp9863)

1

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783990932&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fapo5B3&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783990932738&amp;bpp=2&amp;bdt=437&amp;idt=93&amp;shv=r20260713&amp;mjsv=m202607070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783990683%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783990683%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783990683%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4605277644522&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1676&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95396252&amp;oid=2&amp;pvsid=6806391712215732&amp;tmod=761720134&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1676%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=113" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260713/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>