---
title: "Claude Design 實測：PRD 兩分鐘變原型，四條驗收過幾條？"
source: "https://www.aiposthub.com/pm-test-claude-design-prd-to-clickable-prototype/"
author:
  - "[[Philo]]"
published: 2026-10-23
created: 2026-10-30
description: "PM 實測：把寵物保險比價 App 的 PRD 丟進 Claude Design，記錄時間、四條驗收條件結果、它編造的資料，以及 12 月 14 日獨立網站關閉的時程。"
tags:
  - "clippings"
status: processed
---
Claude Design 是 Anthropic Labs 今年 4 月推出的設計工具：用文字、截圖或文件說明你要什麼，Claude 就幫你做出原型、簡報或設計稿。對 PM 來說，最想知道的是一件事：把 PRD 丟進去，真的能拿到一個可以點的原型嗎？

這是「PM 實測」的第一篇，每週只測一個工具。我拿一份自己寫的「寵物保險比價 App」PRD，附四條驗收條件，丟進 Claude Design，記錄它花多久、做對幾條，以及哪些地方不能直接相信。

結論先講：第一版兩分鐘左右就出來；我實際點過的驗收條件都通過；但三家保單的名稱和費率是它編的，PRD 沒寫的欄位它自己加了，而且這個獨立網站會在 12 月 14 日關閉，功能搬進 Claude 的 Artifacts。

## 這次怎麼測？一份 PRD、四條驗收條件、只跑一次

先把規則講清楚：本文所有數字都來自實際操作時的畫面與時間紀錄。操作與記錄由 AI 助理（Claude）在我的 Claude 帳號裡完成，我們只寫實際看到的結果，沒測到的就寫沒測。下面是丟進去的 PRD 全文，可以直接複製來試。

```
產品：毛孩保，寵物保險比價 App。
流程：1 首頁輸入寵物資料（種類、品種、年齡）→ 2 比較 3 家保單 → 3 選一家後試算保費 → 4 送出投保意願，顯示成功畫面。
驗收條件：
1. 比較表要同時顯示每年保費、理賠上限、等待期。
2. 試算頁可以調整寵物年齡和品種，保費要跟著變。
3. 送出前必須勾選同意條款，沒勾選時按鈕不能按。
4. 四個畫面之間都要能點著走完。
```

| 項目 | 內容 |
| --- | --- |
| 工具 | claude.ai/design（Claude Design，Beta）；頁面顯示的模型是 Opus 5.5（Medium） |
| 日期 | 2026 年 10 月 23 日 |
| 題目 | 上面這份 PRD。這是我寫的示範文件，不是真實公司的需求；中文、iOS、單一版本、「只做核心流程」 |
| 計時 | 從送出表單算起：約 67 秒第一個畫面出現；103 秒檢查時還在建元件；142 秒檢查時已完成。完成落在兩次檢查之間，所以只寫「兩分鐘左右」 |
| 驗收方式 | 從首頁一路點到成功頁，逐條對照 PRD 的驗收條件 |
| 次數 | 只跑一次、一份 PRD；這不是統計，只是一次紀錄 |

## Claude Design 兩分鐘左右做出了什麼？

流程很單純：選「Mobile app design」模板、送出，它會先跳出一張表單，問你要貼 PRD、做多少範圍、哪種裝置、要不要多個版本。填完送出，畫面右邊開始長出手機原型；完成後，它在回覆裡列出做了什麼，以及最後一段「需要你確認的地方」。

![比較保單畫面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/pm-test-claude-design-cmp.jpg)

比較表：三家保單、三個欄位

![試算頁，3 歲](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/pm-test-claude-design-q3.jpg)

試算頁：3 歲，每年 8,680

![試算頁，10 歲](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/pm-test-claude-design-q10.jpg)

把年齡拉到 10 歲，變成 12,870

官方在 10 月 2 日放出的 [「Build an App With Claude Design」](https://www.youtube.com/watch?v=COJAZQM1aeQ&ref=aiposthub.com) ，是 Anthropic 設計師 Nate 用同一套流程做自己的小專案：一句話請 Claude 做點擊原型、留言改細節、最後說「okay Claude, build it」。影片最後一句是「Claude makes it. You steer it.」，意思很接近這次實測的做法：它做，你來把方向盤。

![](https://www.youtube.com/watch?v=COJAZQM1aeQ)

影片：Claude 官方頻道「Build an App With Claude Design」（2026/10/2）。

## 四條驗收條件，過了幾條？

| 驗收條件 | 結果 | 我怎麼驗 |
| --- | --- | --- |
| 1　比較表同時顯示每年保費、理賠上限、等待期 | **過** | 三家保單都有這三欄（保費 5,360／8,680／4,080；理賠上限 15／30／8 萬；等待期 30／15／60 天），還標了「保費最低、保障最高、等待期最短」 |
| 2　試算頁可調年齡和品種，保費跟著變 | **年齡過；品種沒驗到** | 年齡從 3 歲拉到 10 歲，保費從 8,680 變 12,870。核對：6,800×1.10×1.16＝8,676.8，顯示 8,680；6,800×1.10×1.72＝12,865.6，顯示 12,870。品種是下拉選單，我用的操作工具選不動，所以這半條沒測 |
| 3　沒勾同意條款，按鈕不能按 | **過** | 沒勾時按鈕灰色、寫「請先勾選同意條款才能送出」，點了沒反應；勾選後可以按，進成功頁 |
| 4　四個畫面都能點著走完 | **過** | 首頁、比較、試算、投保、成功頁，一路點得通 |

![沒勾條款，按鈕鎖住](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/pm-test-claude-design-lock.jpg)

沒勾：按鈕鎖住

![勾選後可以送出](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/pm-test-claude-design-ok.jpg)

勾了：可以送出

![成功頁](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/pm-test-claude-design-done.jpg)

成功頁

這四條通過，不代表這個原型可以拿去上線；它只代表「PRD 寫得出來的行為，它做得出來」。驗收條件寫得越具體，我越容易一條一條點；這是這次最有用的發現。

## 哪些地方要小心？Claude Design 自己先講了三件事

完成後，它的回覆最後有一段「需要你確認的地方」，等於先把風險列出來：

![Claude Design 回覆中的「需要你確認的地方」](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/pm-test-claude-design-reply.jpg)

畫面：Claude Design 完成後的回覆（10/23 實測）。

- **三家保單是它編的。** 名稱和費率都是示意資料，上線前要換成真實內容。原型看起來很完整，但「完整」只限流程，內容不是你給的就是它猜的。
- **PRD 沒寫的欄位，它自己加了。** 送出頁多了飼主姓名、手機號碼（都不必填，只有同意條款會擋住送出）。這類補充很常見，也很容易被忽略：PRD 沒寫的東西，原型裡可能已經有了。
- **設計系統是空的。** 所以配色和字型是它自訂的暖色系。如果你有自己的設計系統，要先接上。

還有幾件我 **沒有測** ：第二輪修改（叫它改某個畫面要多久、會不會動到別的地方）、匯出（官方說明頁列了 zip、PDF、PPTX、Google Slides、HTML）、交給 Claude Code、接設計系統，以及這一次用掉多少額度。這些之後有測再補。

另外記一個操作上的坑：我用自動化工具把整份 PRD 打進單行提示框，換行被吃掉、只送出第一行，它就回「沒有看到 PRD」並跳出表單請我補貼。改貼進表單就正常了。這可能只是我操作工具的方式造成，不一定每個人都會遇到。

## PM 可以怎麼用？我的三個做法

1. **把驗收條件寫進 PRD。** 每一條都要能「點出來」驗證，例如「沒勾條款，按鈕不能按」。它照著做，你照著驗，來回成本最低。
2. **先看回覆最後那段「需要你確認」。** 它自己標出來的假設，就是你要補的需求。
3. **把第一版當討論材料，不當定案。** 拿它去跟設計、工程對流程，比對著文件討論快；真實資料、規則與法遵細節，還是要你自己補。

這是一次實測的心得，不是通則：只測了一份 PRD、一個裝置、一次，結果不能推到所有題目。

## Claude Design 12 月 14 日要搬家？獨立網站會關閉

實測當天，claude.ai/design 頂端就有一條橫幅：「Claude Design is moving to Artifacts in the Claude app. You can keep using this version until December 14.」

畫面：claude.ai/design 頁面頂端（10/23 實測）。

官方說明頁的說法一致：獨立網站 claude.ai/design 會在 2026 年 12 月 14 日關閉；Claude Design 變成 Artifacts 裡的模板，可以從對話、Artifacts 分頁與 Claude Code 開始。在那之前，既有專案仍在這個網站，說明頁建議先把設計系統搬到 Claude。官方部落格（10 月 8 日）補充：獨立版的專案在關閉前仍可正常使用；但那裡的聊天與留言在關閉後就看不到了，指向獨立版專案的公開連結也會失效。如果你有在用，建議先自己備份重要的專案與對話。

## Claude Design 常見問題

### 要付費嗎？哪些方案可以用？

官方說明頁（10 月 23 日讀）與 Claude 官方部落格（10 月 8 日）都寫：Docs、Slides、Design 已結束 beta，所有方案（含 Free）都能用；Enterprise 預設從 10 月 15 日開啟。細節請以 [官方說明頁](https://support.claude.com/en/articles/14604416-get-started-with-claude-design?ref=aiposthub.com) 為準。用量與 Claude 其他功能共用額度，沒有獨立額度。

### 做出來的原型可以帶走嗎？

官方說明頁寫右上角的 Export 可以下載 zip、輸出 PDF、PPTX、Google Slides 或獨立 HTML，也能交給 Claude Code。這幾項我這次沒有測。

### 兩分鐘左右是常態嗎？

不知道。這是一份 PRD、一次執行的結果；題目大小、模型與當下用量都可能讓時間不同。

### 原型裡的保單資料是真的嗎？

不是。三家保單的名稱與費率是 Claude Design 編的示意資料，它自己也在回覆裡這樣說明。

## 這週做一件事

拿一份你手上的舊 PRD，補上四條「能點出來驗收」的條件，丟進 Claude Design（或任何你在用的原型工具），再自己一條一條點。你會很快看出，哪些需求其實還沒寫清楚。

「PM 實測」每週實測一個工具。下一個想看什麼，歡迎留言。

## 資料來源

本文實測日期為 2026 年 10 月 23 日；方案、額度與網站關閉時間可能變動，以官方最新公告為準。畫面為實測當下截圖。

- [Claude Design 官方說明頁：Get started with Claude Design](https://support.claude.com/en/articles/14604416-get-started-with-claude-design?ref=aiposthub.com)
- [Claude 官方影片：Build an App With Claude Design（YouTube，2026/10/2）](https://www.youtube.com/watch?v=COJAZQM1aeQ&ref=aiposthub.com)
- [Claude 官方影片：Introducing Claude Design by Anthropic Labs（YouTube，2026/4/17）](https://www.youtube.com/watch?v=t_LBECIQQqs&ref=aiposthub.com)
- [Anthropic 教學：Using Claude Design for prototypes and UX](https://claude.com/resources/tutorials/using-claude-design-for-prototypes-and-ux?ref=aiposthub.com)
- [延伸閱讀（4 月研究預覽期的評測，功能與額度可能已變）：MacStories，Hands-On with Anthropic Labs' Claude Design Preview](https://www.macstories.net/stories/hands-on-with-anthropic-labs-claude-design-preview/?ref=aiposthub.com)
- [延伸閱讀（同上）：Product with Attitude，Claude Design 48 小時實測](https://karozieminski.substack.com/p/claude-design-review-prompts-figma)
- [Claude 官方部落格：Dashboards and Motion（2026/10/8，含 Docs、Slides、Design 結束 beta 與獨立網站關閉說明）](https://claude.com/blog/dashboards-and-motion?ref=aiposthub.com)