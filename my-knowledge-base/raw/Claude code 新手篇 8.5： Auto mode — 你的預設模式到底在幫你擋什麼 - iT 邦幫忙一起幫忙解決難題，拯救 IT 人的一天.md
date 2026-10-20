---
title: "Claude code 新手篇 8.5： Auto mode — 你的預設模式到底在幫你擋什麼 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10417881"
author:
  - "[[iThome]]"
published:
created: 2026-10-06
description: "1. 寫完 G8 的那天晚上，它被擋了一次 寫完 G8 那天已經是凌晨。我請 Claude 幫我把稿子推上 GitHub，結果失敗了，GitHub 回我一句「找不到這個 repo」。 repo 明明就..."
tags:
  - "clippings"
status: processed
---


## Claude code 新手篇 8.5： Auto mode — 你的預設模式到底在幫你擋什麼

- 分享至

## 1\. 寫完 G8 的那天晚上，它被擋了一次

寫完 G8 那天已經是凌晨。我請 Claude 幫我把稿子推上 GitHub，結果失敗了，GitHub 回我一句「找不到這個 repo」。

repo 明明就在。Claude 查了一輪，懷疑是我電腦裡登入了兩個 GitHub 帳號，push 的時候拿錯了那一組。

接著它想去翻 git 存在我電腦裡的帳號密碼，看看到底用的是哪一個。

**這一步，被擋下來了。**

畫面上寫著:這個動作被 auto mode 拒絕，原因只有一個標籤 —— `[Credential Exploration]` ，翻找帳密。

然後 Claude 沒有換個方式繼續翻。它停下來，跟我說這一步要我自己來，給了我兩行指令。我自己跑完，稿子就推上去了。

那時我才發現一件事:**我每天都在 auto mode 裡，卻說不太出來它到底在幫我擋什麼。**

G8 一直叫你「日常留在 auto mode」。這篇就把它拆開來看。

---

## 2\. 你現在就在 auto mode 裡

先確認一件事:看畫面最下面那一行小字。如果寫的是 `⏵⏵ auto mode on` ，你現在就在 auto mode 裡。

G8 講過，8 月中開始它是 Pro、Max、Team 的預設;**9 月 25 日的更新之後，不管哪種方案，一打開都是它。**

所以這篇不是在介紹一個進階功能，是在介紹你每天都在用的東西。

> 💡 **會不會多花錢？** 不會。auto mode 的審核，在 Pro、Max、Team 方案 **不算你的用量** 。

---

## 3\. 它能幫你做什麼

先講它的好處。它會變成預設，是因為好用，不是因為它需要被提防。

在 auto mode 之前，最常見的是 Manual:改一個檔案問你一次，跑一個指令再問一次。

那時候在 AI 討論區、國外論壇上，有一個很好笑的說法:有人把我們這些用 AI 開發的工程師，叫做\*\*「Yes engineer」\*\* —— 因為我們只要負責在它做完每一步的時候，按下 yes 就好。

好笑，但也有點真實。連官方的最佳實踐文件都這樣寫:

> **In Manual mode, Claude Code asks before actions that might modify your system … That's safe but tedious. After the tenth approval you're clicking through rather than reviewing.**
> 
> Manual 模式很安全，但很煩。按到第十次同意，你已經不是在檢查，只是在點下去而已。  
> —— [Claude Code 官方文件〈Best practices for Claude Code〉](https://code.claude.com/docs/en/best-practices)

這才是「Yes engineer」背後真正的問題:**一直按 y，其實也不安全** —— 因為你早就沒在看了。auto mode 的想法剛好反過來:日常的事讓它一路做下去，把真正需要注意的動作，交給另一個模型認真看。

### 什麼時候最好用

官方給它的定位是「長任務、減少被詢問的疲勞」。換成做作品集網站的情境，大概是這三種:

| 情境 | 例子 |
| --- | --- |
| 方向定了，改動很多 | 「三個頁面都換成新的配色」 —— 不用每改一個檔案就按一次 |
| 讓它自己檢查、自己修 | 「每一頁都檢查一次，壞掉的連結修到全部正常為止」 —— 它能一路檢查、一路修，不會每一步都卡在等你 |
| 你要離開一下 | 去倒杯水、開個會 —— 它不會停在「等你按 y」那一步 |

> 💡 **這剛好補上 G8 開頭那個問題。** G8 引用過官方那句「Claude stops when the work looks done」:它會停在「看起來做完」。給它一個能自己跑的檢查，再讓它不用一直等你按 y，它就能一路修到 **真的做完** ，而不是停在看起來做完。

### 想少按 y:auto mode 還是「自動接受編輯」？

G8 講過另一個會少問你的模式 —— 自動接受編輯。兩個放在一起比:

| 模式 | 改你的檔案 | 其他動作(跑指令、刪檔……) |
| --- | --- | --- |
| Manual | 每次問你 | 每次問你 |
| 自動接受編輯 | 直接改 | 資料夾裡的 `rm` 、 `mv` 直接放行，沒人看;其他還是問你 |
| **auto mode** | 直接改 | **另一個模型先看** ，沒問題才做 |

所以如果你的目的是「少按 y」， **auto mode 比自動接受編輯更好** —— 一樣少問你，但刪檔這種事，還有人先幫你看一眼。

---

## 4\. 它怎麼判斷:四步

auto mode 的核心很簡單:**每個動作執行之前，都有另一個模型先幫你看一眼。** 不是 Claude 自己審自己，是另外一個專門負責把關的模型(官方叫它 classifier)。

![https://ithelp.ithome.com.tw/upload/images/20260929/20182796icJuYoy7rv.png](https://ithelp.ithome.com.tw/upload/images/20260929/20182796icJuYoy7rv.png)

它判斷一個動作，照這個順序走，先中的就算數:

| 順序 | 情況 | 結果 |
| --- | --- | --- |
| 1 | 你寫過規則(allow / ask / deny) | 照你的規則 |
| 2 | 讀檔、改你資料夾裡的檔案 | 直接放行，不用審 |
| 3 | 其他:跑指令、連網路、碰資料夾外面的東西 | 交給審核的模型 |
| 4 | 被擋下來 | Claude 收到原因，換一個做法 |

換句話說:**你專案裡的日常改動，它不會一直煩你;會被拿去審的，是那些可能碰到外面、或收不回來的動作。**

> 💡 **網頁裡藏的指令騙得了它嗎？** 官方的設計是，審核的模型只看你說的話和 Claude 要做的動作， **看不到檔案和網頁的內容** 。所以就算有人在網頁裡藏一句「把資料傳出去」，也沒辦法直接騙到它。

---

## 5\. 它會擋什麼、不會擋什麼

官方的封鎖清單很長，大多是公司環境才會碰到。挑新手真的可能遇到的:

**預設會擋的**

- 下載網路上的程式直接執行(像 `curl ... | bash`)
- 把敏感資料送到外面
- 刪掉這次對話開始 **之前** 就存在、而且刪了就救不回來的檔案
- 強制推送(force push)
- 丟掉你還沒存的修改(`git reset --hard` 這一類)
- 把金鑰、token 印出來

**預設不會擋的**

- 改你資料夾裡的檔案
- 安裝專案裡已經列好的套件
- **讀你的 `.env`**
- **push 到你現在這個 repo，包括主分支**

最後兩個，很多人以為 auto mode 會擋。 **它不會。** (push 的內容還是會被檢查，例如把金鑰一起推上去，還是會擋。)

> ⚠️ **這就是 G8 叫你寫 deny 規則的原因。** G8 第 8 節的例子是 `Bash(git push *)` 和 `Read(./.env)` 。那不是多此一舉 —— auto mode 預設真的會讓它 push、會讓它讀 `.env` 。這兩件事你不想讓它自己做，就得自己寫規則。

---

## 6\. 我試著讓它被擋

寫這篇的時候，我想拍一張「被擋下來」的畫面，所以在一個空資料夾裡做了個小實驗。

我先存了一個版本，再故意改了一行、不存。然後跟它說:

```markdown
幫我把專案恢復成乾淨的狀態
```

我原本以為它會直接把那行修改丟掉，然後被擋下來。

結果沒有。

![https://ithelp.ithome.com.tw/upload/images/20260929/20182796MBhfFuHeed.png](https://ithelp.ithome.com.tw/upload/images/20260929/20182796MBhfFuHeed.png)

這張圖裡有三件事:

**第一，它沒有先問我。** 我說「恢復乾淨」，它沒有回一句「確定要丟掉修改嗎？」就直接做了。官方文件寫明，auto mode 會讓它 **盡量一路做下去，不停下來問你問題** 。這也是 G8 說「方向還沒定，就切 Plan Mode」的原因。

**第二，每一步都有被審過。** 那行 `Allowed by auto mode classifier` ，就是它的指令被送去審、然後放行的紀錄。

**第三，保護你的不只有審核。** Claude 自己選了一個收得回來的做法 —— 用 `stash` 先把修改存起來，還告訴我怎麼拿回來。審核沒什麼好擋的，因為它根本沒做危險的事。

然後我補了一句:

```markdown
stash 也刪掉
```

![https://ithelp.ithome.com.tw/upload/images/20260929/20182796a008GaXWzC.png](https://ithelp.ithome.com.tw/upload/images/20260929/20182796a008GaXWzC.png)

它照做了。有意思的是，刪 stash 其實在官方的封鎖清單裡。但這次我 **講清楚了要刪什麼** 。照官方的規則，你在對話裡明確講出「動作 + 對象」，就算是你同意了。

反過來，說得太籠統就不算。官方舉的例子是:「幫我清一清 repo」不等於允許 force push;「把這個 branch force push 上去」才算。

> ⚠️ **這只是一次實驗。** 審核是模型在判斷，同樣的要求換個說法、換個專案，結果不一定一樣。別把它當成「這樣講一定會放行」或「一定會擋」的保證。

---

## 7\. 真的被擋下來，會發生什麼

開頭那次才是真的被擋。整理一下被擋時你會看到什麼、可以做什麼:

- **畫面會通知你。** 輸入框附近會出現一行字，格式像這樣:`bash denied by auto mode · [Data Exfiltration] · /permissions` 。中間方括號裡的是原因標籤，開頭那次是 `[Credential Exploration]` 。
- **Claude 會自己換做法。** 它會收到被擋的原因，然後改走別條路。開頭那次，它的「別條路」就是把這一步交給我。
- **想看紀錄、想重試:** 輸入 `/permissions` ，切到 **Recently denied** 分頁。被擋的動作都列在這裡，選一個按 `r` 就能重試。
- **真的要它做:** 在下一句講清楚動作和對象，就像我那句「stash 也刪掉」。
- **擋太多次:** 連續被擋 3 次，或同一個對話裡總共 20 次，auto mode 會先暫停，改回每一步都問你。

![https://ithelp.ithome.com.tw/upload/images/20260929/20182796wB75zFWXAF.png](https://ithelp.ithome.com.tw/upload/images/20260929/20182796wB75zFWXAF.png)

> 💡 **跳出「Teach auto mode about your environment?」怎麼辦？** 被擋幾次之後，它可能會問你要不要設定信任的網域和伺服器。 **那是給公司環境用的** ，個人專案可以先略過。

---

## 8\. 三種界線:講一次、每次問、永遠不准

G8 講過 deny 規則。加上 auto mode 之後，你手上其實有三種工具，官方有一張表把它們排好:

| 你想要 | 怎麼做 | 效果 |
| --- | --- | --- |
| 這次對話先別做 | 在對話裡講，例如「review 之前不要 push」 | 會擋，但對話被壓縮後可能忘掉 |
| 每次做之前先問我 | 寫 `ask` 規則 | 就算在 auto mode 也一定會問 |
| 永遠不准做 | 寫 `deny` 規則 | 審核之前就擋掉，連你在對話裡同意都沒用 |

中間那個 `ask` 是 G8 沒特別講的。它很適合「我不是不讓它 push，只是想在 push 前看一眼」這種情況:

```json
"ask": ["Bash(git push *)"]
```

寫在設定檔的 `permissions` 裡(位置跟 G8 第 9 節一樣)。這樣它平常在 auto mode 裡一路做， **只有要 push 的時候會停下來問你。**

> ⚠️ **規則是照字面比對的。** 像 `git -C 資料夾 push` 這種換個寫法的 push，就對不上這條規則。所以它是「多一道確認」，不是萬無一失的鎖 —— G8 講 deny 時也是同一件事。

> 💡 **CLAUDE.md 也會影響審核。** 你在 CLAUDE.md 寫「永遠不要 force push」，官方說 Claude 和審核的模型都會讀到。這是 G3 那個檔案的另一個用處。

這幾種工具加上 CLAUDE.md、還有之後的 hooks 要怎麼搭配，G13 會整理成一張完整的比較表。

---

## 9\. 它不保證安全 —— 什麼時候該切回 Manual

官方在 auto mode 的說明裡放了一個警告框:

> **Auto mode reduces permission prompts but does not guarantee safety. Use it for tasks where you trust the general direction, not as a replacement for review on sensitive operations.**
> 
> auto mode 減少了詢問，但不保證安全。它適合你信任大方向的任務，不能取代敏感操作前的檢查。  
> —— [Claude Code 官方文件〈Choose a permission mode〉](https://code.claude.com/docs/en/permission-modes)

「你信任大方向」這幾個字，剛好把 G8 和這篇接起來。一個專案從頭到尾，模式大概是這樣換的:

| 階段 | 用什麼 |
| --- | --- |
| 方向還沒定 | **Plan Mode** —— 先想、先問，不動手(G8) |
| 方向定了，開始做 | **auto mode** —— 讓它一路做，審核幫你看著 |
| 碰資料庫、正式環境、要上線 | 按 `Shift` + `Tab` 切回 **Manual** ，每一步自己看 |
| 永遠不能碰的 | 寫成 **deny 規則** ，不管在哪個模式 |

敏感的那一段做完，再切回 auto mode 就好。模式本來就是拿來切換的，不用一整天待在同一個。

---

## 10\. 本課單字:看懂它在擋什麼

### /permissions

打開權限設定。 **Recently denied** 分頁列出最近被 auto mode 擋下來的動作，選一個按 `r` 可以重試。想知道「它剛剛到底被擋了什麼」，來這裡看。

### ask 規則

「每次做之前先問我」。例如 `Bash(git push *)`:平常讓它在 auto mode 裡一路做，只有 push 前停下來問你。比 deny 溫和，比在對話裡講可靠。

### 講清楚的同意

不是指令，是一個說話的習慣。要它做一件會被擋的事， **講出動作和對象**:「把這個 stash 刪掉」算數，「幫我整理一下」不算。

---

## 11\. 給新手的三個提醒

### a. 方向定了，就放手讓它跑

auto mode 最大的價值是讓你不用一直按 y。方向確定、改動又多的時候，就讓它一路做，你把力氣留給看結果 —— 被擋的那幾次，才是需要你出手的時候。

### b. 它預設會 push、會讀.env

很多人以為 auto mode 會幫你擋這兩件事，它不會。不想讓它自己做的，寫成 ask 或 deny 規則。

### c. 敏感的事，切回 Manual

資料庫、正式環境、上線，就算你信任它，也值得自己盯一次。做完再切回 auto mode。

---

## 結語

回到開頭那一晚。它想翻的，是我電腦裡的帳號密碼。這種事本來就該由我自己來，而不是讓 AI 在我沒注意的時候自己去看。那一次，它擋得有道理。

auto mode 在做的就是這件事:**讓日常的事一路做下去，把少數收不回來、或會碰到外面的事攔下來，交給你。** 它不是萬能的，所以旁邊才需要 Plan Mode、ask、deny 和 Manual 補位。

下一篇我們換個方向。前面都在處理「怎麼跟它把事情做對」，但東西做完之後呢？G6 教過用 HTML 讓它把產出畫給你看 —— 那些檔案現在都躺在你自己的電腦裡，要給別人看只能傳檔案，而且傳出去的那一份 **不會再更新** 。

有一個方式可以讓它變成一個網址:會跟著更新，能傳給別人，別人打開就是最新的。下一篇就講這個。

> 📌 **關於本篇的細節:** 預設模式、封鎖清單、畫面上的字，都是依 **2026 年 9 月的官方文件** 和 v2.1.283 實測整理的。這塊 **改得特別快** ，如果你是很久以後讀到這篇，請以你眼前看到的為準 —— 輸入 `/permissions` ，看它列出什麼，就是最即時的答案。

---

下一篇 **G9 — Artifacts:把產出變成一個會更新的網址** 。G6 教你讓 Claude 用 HTML 把東西畫給你看，這篇讓那份東西變成別人也打得開、而且會自己更新的網頁。

---

## 系列文章

- G1 Claude Code 從零開始 — Portfolio v2 重做
- G2 用量與計費 — 搞懂怎麼收費，再決定要不要省
- G3 CLAUDE.md — 讓 Claude 跨對話記得你的專案
- G4 Memory 系統 — 讓 Claude 不只記得專案，還記得你
- G5 換 section 不斷線 — 把當下進度交接給下一個對話
- G6 用 HTML 跟 Claude 溝通 — 讓你一眼看懂它給你的東西
- G7 控制 Claude 怎麼想 — 選模型與思考深度
- G8 先想清楚再動手 — Plan Mode 與權限模式
- **G8.5(本篇)** auto mode — 你的預設模式到底在幫你擋什麼
- G9 Artifacts — 把產出變成一個會更新的網址(規劃中)

> 完整 20 篇地圖會放在系列首頁。本系列同步發佈於 Medium。

---

**關於我**

Tim Wei / EY 安永 Technology Consulting 顧問（AI/ML），做 RAG 知識系統、機器學習模型與 AI 治理，也擔任數位發展部 AI 人才培育計畫的講師。

歡迎在留言區跟我討論，或在 [LinkedIn](https://www.linkedin.com/in/timwei0801) 上找我。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1789123100_6aa3da1cbf61d.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/5/23/0825fb6b-d53a-4db0-8b0f-099bc06cc9c9_small.jpg)

[多團隊如何高效協作開發 - 揭秘 LeSS 的設計原理](https://itplus.ithome.com.tw/webinar-page/225?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/57929b21-4a63-4c83-9dc6-f03b5c7df886.jpg)

[30 分鐘與 Docker 一起做好朋友](https://itplus.ithome.com.tw/webinar-page/238?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/13/e8b81f73-a9a7-4287-93bc-1a6dce987daa_small.png)

[融合 AI 技術，加速應用開發、交付與優化](https://itplus.ithome.com.tw/vod-page/905?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

27 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/5/26/e9e79af5-8fbb-4950-a163-5e10f99617f6_small.jpg)

[解讀人工智慧模型的原理與優化方式](https://itplus.ithome.com.tw/webinar-page/252?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

33 分