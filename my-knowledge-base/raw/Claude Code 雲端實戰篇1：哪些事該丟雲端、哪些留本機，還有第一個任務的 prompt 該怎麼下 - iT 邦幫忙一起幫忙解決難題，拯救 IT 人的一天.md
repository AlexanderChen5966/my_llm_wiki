---
title: "Claude Code 雲端實戰篇1：哪些事該丟雲端、哪些留本機，還有第一個任務的 prompt 該怎麼下 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10425144"
author:
  - "[[iThome]]"
published:
created: 2026-11-10
description: "這是「Claude Code 雲端實戰」系列的第 1 篇。第 0 篇從頭講了整個專案：用雲端 session 從零做一個每天整理台股盤前資訊的工具，再用排程任務讓它每個交易日自己跑（https://i..."
tags:
  - "clippings"
status: "inbox"
---
0

## Claude Code 雲端實戰篇1：哪些事該丟雲端、哪些留本機，還有第一個任務的 prompt 該怎麼下

- 分享至

> 這是「Claude Code 雲端實戰」系列的第 1 篇。第 0 篇從頭講了整個專案：用雲端 session 從零做一個每天整理台股盤前資訊的工具，再用排程任務讓它每個交易日自己跑（https://ithelp.ithome.com.tw/articles/10423566 ）。
> 
> 這篇只回答一個問題： **一件事要交給 Claude Code，該丟到雲端，還是在自己電腦跑？** 後半段講決定丟雲端之後，第一個任務怎麼下。

現在要 Claude Code 做一件事，有三種跑法：在自己電腦跑（本機）、丟到 Anthropic 的主機跑（雲端 session），或在自己電腦跑，但用手機或網頁遙控（Remote Control）。

做那個台股工具的時候，有些事我丟雲端，有些事留在本機，還有一件該交出去卻留在本機，結果失敗了。這篇就用那個專案裡的 12 件事，來回答「該丟哪裡」。

- 開發和排程：2026-10-07～10-15，Max 方案。當時雲端 session 用的 Claude Code 版本沒有記錄。
- 官方文件：2026-10-21 查證，本機 Claude Code 2.1.306。
- 額度驗證：2026-10-23，雲端 session 裡的 Claude Code 是 2.1.308。
- 台股工具只是測試題目；報告只整理公開資訊，不構成投資建議。

## 先講結論

1. **先問兩個問題** ：做這件事需不需要你的電腦（本機的檔案、本機的設定、你這邊的網路）？做的時候需不需要你一直在旁邊回答？兩個都不需要，就丟雲端。有一個需要，就留在本機；人不在電腦前，再用 Remote Control 遙控。
2. **用量怎麼扣** ：自己開的雲端 session，扣 9 月底送的那筆雲端額度；排程任務不扣額度，扣訂閱用量（第 4 節實測）。沒有額度的人，兩個都扣訂閱用量，雲端主機不另外收費（官方）。
3. **丟雲端之前，先在本機把規格寫好、推上 GitHub。** 雲端照規格做、開 PR，我回本機審過再合併。這個專案的 3 個版本都是這樣做的。

---

## 1\. 三種跑法差在哪

先看三種跑法各能做什麼。下面這張表整理自官方的比較表（2026-10-21）：

|  | 本機 | 雲端 session | Remote Control |
| --- | --- | --- | --- |
| 程式跑在哪 | 你的電腦 | Anthropic 的雲端主機 | 你的電腦 |
| 從哪裡開始 | 終端機、IDE、桌面版 | 網頁 claude.ai/code、手機 App 的 Code 分頁、桌面版選雲端環境、 `claude --cloud` | 先在電腦上開好（終端機、VS Code、桌面版），再從網頁或手機接上 |
| 電腦關掉 | 停 | 繼續跑 | 電腦要開著，session 也要開著 |
| 用得到你本機的設定 | 可以 | 不行，只有 repo 裡的 | 可以 |
| 要不要 GitHub | 不用 | 要 | 不用 |
| 網路 | 你的網路 | 看雲端環境的設定，預設只能連套件庫、GitHub 這類白名單 | 你的網路 |
| 權限模式 | 終端機裡全部都能選 | 只有「自動」「接受編輯」「計畫」三種 | 從網頁或手機可以選這三種，再加上 Manual |

官方在 Remote Control 的文件裡給了一個分法：手上的本機工作做到一半，想換一台裝置接著做，用 Remote Control；想直接開始一件事、不想先在本機準備環境、repo 不在你電腦上，或要同時跑好幾件，用雲端 session。

這個專案我沒有用 Remote Control，所以這篇只比較、不細講。Remote Control 會在另一個系列「Claude Code 實戰手冊」的 G14 專門講。

另外兩個方向的搬移不一樣：

- **雲端 → 本機** ：終端機可以用 `--teleport` 把雲端的 session 拉回本機繼續做。反過來，終端機的 session 不能推上雲端（官方）。
- **本機 → 雲端** ：桌面版可以把本機的 session 接到雲端繼續做，對話會濃縮成摘要帶過去（官方）。在側欄的 session 上按右鍵 →「開啟於」→「在以下位置繼續：雲端」，選項底下寫著「無需您的電腦即可執行」。選單裡看得到，我沒有實際送出過。

### 雲端 session 不能拿來做什麼

我一開始以為，東西放到雲端，它就會一直開著。實際用過才知道，有三件事它做不到：

1. **不能當一直開著的服務。** 關掉分頁不會停，它會繼續跑到做完手上這件事，然後閒置。閒置幾分鐘，主機就暫停，之後可能被回收。回收後重新打開，對話紀錄會還原，但還在背景跑的子代理、指令，和 `/loop` 排好的下一次，都不會還原（官方）。要每天跑，就用排程任務，每次開一個新的 session。
2. **每次都是新的 clone。** 排程任務每次開始都從 GitHub 重新 clone 一份（官方），上一次留下的檔案，只有推回 GitHub 的才在。我的工具每跑一次，就 commit 一次「data: <日期> 盤前整理」，把當天的資料和報告存回 repo，到 10/15 共 8 次。下一版想算「法人連續買賣超幾天」，也只能讀 repo 裡前幾天的資料。
3. **你電腦上的設定帶不過去。** `~/.claude/CLAUDE.md` 、你自己裝的 skills、用 `claude mcp add` 加在本機的 MCP，雲端都看不到。要讓雲端也用，就 commit 進 repo（官方）。

---

## 2\. 我這個專案：哪些事丟雲端、哪些留本機

這個工具從規劃到每天自動跑，前後有 12 件事。每件事在哪做、為什麼：

| 事情 | 在哪做 | 為什麼 |
| --- | --- | --- |
| 查證交所的資料網址、確認公開資料幾點更新 | 本機 | 要一直試，看結果再決定下一步 |
| 寫規格（SPEC.md） | 本機 | 要跟我來回討論 |
| 從零寫第一版，順便測雲端連不連得到證交所 | 雲端 | 規格寫好了，約 40 分鐘不用管；連線要在正式執行的那種主機上測才算數 |
| 審第一版的 PR | 本機 | 同一支程式從台灣再跑一次，當對照組 |
| v1.1：減少對證交所的請求 | 雲端 | 同第一版，約 20 分鐘 |
| 建雲端環境、建排程任務 | 桌面版介面 | 一次性的設定，我在桌面版點完 |
| 每個交易日 08:07 整理盤前資訊 | 排程任務 | 電腦不用開 |
| 在本機排一個 08:05 的監看，準備看 08:07 那次排程任務 | 本機 → **失敗** | Mac 睡著，計時就停了 |
| 改報告的規則 | 本機改檔案、推上 GitHub | 小改動；排程任務隔天就照新規則跑，不用動設定 |
| v2：讀新聞內文、寫今日重點 | 雲端 | 同第一版，約 40 分鐘 |
| 核對每份報告的 42 個數字 | 本機 | 要從台灣另外向證交所抓一次，跟雲端分開 |
| 整理給讀者的公開 repo | 本機 | 要用本機的檔案，做成只有一個 commit 的新 repo |

看完這張表，有三件事很清楚：

- **留在本機的，都卡在三件事之一** ：要用我這邊的網路（核對、對照組）、要來回試、要用本機的檔案。審 PR 那次最明顯：雲端說證交所擋它，我在本機用同一支程式從台灣跑，12 個資料來源全通，這才確定是雲端主機被擋，不是程式寫錯。
- **丟雲端的，共通點是** ：規格寫好了、做完會開一個 PR、中間不用我回答。
- **失敗的那一件，剛好是該交出去卻留在本機的。** Mac 一睡著，本機的計時就停了。電腦不開也要跑的事，要交給排程任務。

### 丟之前，問自己四個問題

第 0 篇列過適合丟雲端的四個特徵，這裡改成丟之前的檢查，加上不符合的時候怎麼辦：

1. **材料都在 GitHub 上嗎？** 雲端 clone 的是 GitHub 上的分支，不是你電腦裡的版本（官方）。還沒 push 的，先 push。
2. **說得出完成條件嗎？** 例如「PR 開好、測試全過」。說不出來的，先留在本機想清楚，可以用 Plan 模式（計畫模式）。
3. **中間不用你一直回答嗎？** 官方說雲端適合「不需要常常調整方向」的工作。要一直問你的，留在本機；人不在電腦前，用 Remote Control。
4. **成品有地方放嗎？** 分支、PR，或像這個工具送到 Telegram。沒有地方放，主機回收之後就沒了。

---

## 3\. 第一個任務怎麼下：本機規劃、雲端執行

決定丟雲端之後，我的做法是這樣：

```markdown
本機：跟 Claude Code 討論、寫規格 docs/SPEC.md → push 到 GitHub
   ↓
桌面版開雲端 session：選 repo、雲端環境、模型、權限模式 → 貼 prompt → 關電腦
   ↓
雲端：照規格寫程式、跑測試 → 開 PR，不合併
   ↓
本機：審 PR、跑測試 → 我合併
```

官方也這樣建議（"Plan locally, execute in the cloud"）：複雜的任務，先在本機用 Plan 模式討論做法，把計畫存進 repo、commit、push，雲端主機才 clone 得到，再開雲端 session 照著做。官方的例子用終端機：

```bash
claude --permission-mode plan
claude --cloud "Execute the migration plan in docs/migration-plan.md"
```

我用的是桌面版，沒有用 `--cloud` ，但順序一樣。

![https://ithelp.ithome.com.tw/upload/images/20261029/20182796sBRocHMmCT.png](https://ithelp.ithome.com.tw/upload/images/20261029/20182796sBRocHMmCT.png)

### 實際的 prompt

第一版我貼的是這段：

```sql
請照 docs/SPEC.md 實作這個 repo 的第一版。先讀 CLAUDE.md 和 docs/PITFALLS.md。

1. 先寫 python -m brief check，在這台雲端機器上測每個資料來源和 api.telegram.org 連不連得到，結果（狀態碼、耗時、錯誤訊息）寫進 docs/CONNECTIVITY.md。連不到的照實記錄，不要想辦法繞過。
2. 接著實作 collect 和 send。把第一次抓到的真實回應存成 tests/fixtures，測試不打網路，全部要通過。
3. 實際跑一次 collect（報告日用今天），把 JSON 存進 data/。
4. 用 send 送一則測試訊息到 Telegram：「tw-premarket-brief 測試：<collect 的結束碼>」。環境變數沒設就跳過，並在 PR 說明寫出來。
5. 遇到任何意外，在 docs/PITFALLS.md 加一列。

分支 feature/v1，做完開 PR，不要 merge。commit 不要加 Co-Authored-By。規格沒寫清楚的地方列在 PR 說明裡，不要自己決定。
```

每一句都有原因：

- **「先讀 CLAUDE.md 和 docs/PITFALLS.md」** ：雲端每次都是新的 clone，不知道我們之前討論過什麼。背景要寫在 repo 裡，叫它先讀。
- **「先寫 check，在這台雲端機器上測……連不到的照實記錄，不要想辦法繞過」** ：本機測得通，不代表雲端通。先測，結果寫成檔案帶回來。後來證交所真的擋了雲端主機（第 0 篇坑 1）。
- **「測試不打網路」「實際跑一次 collect」** ：測試用第一次抓到的真實回應，不打網路；真的連線只在最後跑一次。
- **「遇到任何意外，在 docs/PITFALLS.md 加一列」** ：雲端看到的事要寫成檔案，主機回收就沒了。第 0 篇的踩坑實錄，很多就是從這個檔案整理出來的。
- **「做完開 PR，不要 merge」** ：成品放在 PR，我回本機審過才合併。
- **「規格沒寫清楚的地方列在 PR 說明裡，不要自己決定」** ：這句最有用。它列了 27 個，每個都附上暫時的做法（第 0 篇第 3 節），我在本機一個一個決定。

大約 40 分鐘後 PR #1 開好，118 個測試全過，Telegram 收到測試訊息。這一次從雲端額度扣了美金 $11。

![https://ithelp.ithome.com.tw/upload/images/20261029/20182796Qf7AUz7l4u.png](https://ithelp.ithome.com.tw/upload/images/20261029/20182796Qf7AUz7l4u.png)

這個 session 的權限是「自動」。雲端 session 的權限只有「自動」「接受編輯」「計畫」三種（官方文件的 Auto、Accept edits、Plan）。要丟出去就關電腦的任務，不要選「計畫」：它提出做法之後，會停下來等你核准才動手。

### 審完，變成下一個 prompt

審完 PR #1，我把 27 個待決事項逐條做了決定，再寫成 v1.1 的 prompt。開頭長這樣：

```markdown
請照 docs/SPEC.md 做 v1.1，先讀 CLAUDE.md、docs/PITFALLS.md、docs/CONNECTIVITY.md。目標是減少對證交所的請求、不再用 openapi、修正法人的算法。

1. 三大法人：外資只取「外資及陸資(不含外資自營商)」，自營商 = 自行買賣 + 避險（已包含外資自營商）。……
……
7. 測試用 fixture，不打網路；新的 fixture 只抓一次。最後實際跑一次 collect（報告日用今天），不要重複抓。被擋就照實記錄，不要重試超過 SPEC 規定的次數。
```

跟第一版比，多了兩件事：

- 要先讀的檔案，多了上一輪雲端寫下的 `docs/CONNECTIVITY.md` （雲端的連線測試結果）。上一輪留下的檔案，變成下一輪的背景。
- 審查時做的決定，直接寫進步驟。它不用再猜。

這一次約 20 分鐘，美金 $5。

### 開始之前，確認三件事

1. **規格已經推上 GitHub。** 10/7 晚上要做 v2，GitHub 剛好出事（第 0 篇坑 11），規格推不上去，雲端 session 就只能等。
2. **雲端環境的網路先設好。** 預設只能連套件庫、GitHub 這類白名單，要連其他網站，先在環境加網域。這部分在 #3 細講。
3. **選好權限模式和模型。** 我寫程式用 Opus 5.5，每天的排程任務用 Sonnet 5.5。

官方對寫任務的建議有三點：寫出檔名或函式名稱、有錯誤訊息就貼上、描述你期望的結果而不只是症狀。我是用一份規格檔，把這三件事一次講完。

---

## 4\. 額度和用量怎麼扣

先講結論：

- **自己開的雲端 session** （從桌面版、網頁、終端機開都一樣）：扣 9 月底送的那筆雲端額度。
- **排程任務** （它自己跑的，和在它的執行紀錄裡接著問的）：不扣額度，扣訂閱用量。
- **沒有額度的人** ：兩個都扣訂閱用量。

為什麼這樣分，官方沒有說明，因為官方文件根本沒提那筆額度。官方只寫了沒有額度時怎麼算：

> "It counts toward your plan's usage limits alongside the rest of your Claude and Claude Code usage, and there's no separate charge for the cloud VM."

> "Scheduled tasks draw down subscription usage the same way interactive sessions do."

也就是雲端 session 跟本機的 Claude Code、跟 Claude 聊天吃同一份方案用量，雲端主機不另外收費；排程任務也扣訂閱用量。

有額度的時候怎麼扣，只能自己量。第 0 篇已經看到：手動開的 3 個 session 從額度扣了美金 $11、$5、$6，排程任務跑了 4 次，額度都沒動（第 0 篇坑 7）。

10/23 我把其他開法補測一遍。每一種開法各開一個 session，叫它讀一批檔案，再看 `/usage` 裡的費用和「雲端工作階段點數」有沒有跟著少：

| 開法 | `/usage` 費用 | 雲端點數 | 扣額度嗎 |
| --- | --- | --- | --- |
| 桌面版開（對照組） | 美金 $3.42 | $225 → $221 | 扣 |
| 終端機 `claude --cloud` | 美金 $0.98 | $221 → $220 | 扣 |
| 網頁 claude.ai/code（Safari） | 美金 $3.48 | $220 → $217 | 扣 |
| 打開排程任務的執行紀錄，在裡面繼續問 | 美金 $4.36 | $220 → $220 | **沒扣** |

![https://ithelp.ithome.com.tw/upload/images/20261029/20182796T7reYog0cl.png](https://ithelp.ithome.com.tw/upload/images/20261029/20182796T7reYog0cl.png)

結果跟開頭的結論一樣：自己開的三種都扣額度，排程任務的執行紀錄裡接著問不扣。那段時間「工作階段限制」（5 小時用量）從 4% 變 11%，對得上「扣訂閱用量」；不過我本機同時也在用 Claude Code，這 7% 不能全算在它頭上。

兩個注意：

- 雲端點數只顯示到整數美元，幾毛錢的變化看不出來，所以每一項我都讓它多讀一些檔案，花到看得出變化。終端機那項只花了 $0.98，是搭配前面幾項的數字推算的：從那時的餘額範圍來看，有扣才會掉到 220，沒扣會停在 221。
- 額度掉的幅度跟 `/usage` 的估算差不多，例如網頁那項花 $3.48，點數掉了 3。 `/usage` 是照 API 牌價估算的，不是帳單。

還沒量到的只有一件事：自己開的 session 扣了額度之後，是不是同時也吃訂閱用量。

沒領到額度的人：領取期限已經過了，額度 11/5 到期。

每次跑要花多少、錢花在哪，#4（11/20）會用四個模型跑同一天的資料來比。

---

## 5\. 最後

回到開頭的問題：一件事要丟哪裡，先問兩件事：需不需要你的電腦？需不需要你一直回答？兩個都不需要，就丟雲端；丟之前把規格推上 GitHub，PR 回來在本機審。

下一篇 #2（11/6）是給股民的：不會寫程式，也能設定自己的盤前整理，一步一步附截圖。

最後想問你： **你最近交給 AI 的一件事，會丟雲端還是留本機？** 歡迎留言。

程式碼、規格和踩坑紀錄都在 GitHub：https://github.com/timwei0801/claude-cloud-premarket-brief

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/22/d3a8eae5-87f5-4c76-ad19-a647e6c6374a_small.png)

[企業零信任安全架構的實踐](https://itplus.ithome.com.tw/vod-page/639?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

零信任資安講堂 |

43 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/167ef010-1156-489f-93ae-b8d0ef29af62_small.jpg)

[當世界都在用 AI 時，K8sGPT 賦予你 K8s 超能力](https://itplus.ithome.com.tw/vod-page/1031?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

SRE CONFERENCE |

39 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/11/3/050f266e-9f56-48c4-b577-dc412f9be36b_small.png)

[當開發者體驗成為競爭力，平臺工程如何重塑企業 IT 組織](https://itplus.ithome.com.tw/vod-page/1686?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Platform Engineering Day |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/10/30/9c262000-e6a4-42e2-bf8c-b1b008496be1.png)

[從 Hello World 看懂 AI 開發工具的計費結構——如何省得到錢](https://itplus.ithome.com.tw/vod-page/1648?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

31 分