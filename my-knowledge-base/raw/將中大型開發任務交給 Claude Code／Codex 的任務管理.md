---
title: "將中大型開發任務交給 Claude Code／Codex 的任務管理"
source: "https://codelove.tw/@tony/post/xXYGm3"
author:
published:
created: 2026-08-04
description: "[](#%E6%A6%82%E8%A6%81)概要-------------------------像 Claude Code、Codex 這類 AI 程式開發代理，可以一起處理程式碼調查、實作、測試、文件更新等工作。如果只是小型修正，有時只要下達「請修正這個 bug」的指示就能得到足夠好的..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月03日](https://codelove.tw/@tony/post/xXYGm3 "2026-08-03 17:03:44")

## 將中大型開發任務交給 Claude Code／Codex 的任務管理

## 概要

像 Claude Code、Codex 這類 AI 程式開發代理，可以一起處理程式碼調查、實作、測試、文件更新等工作。  
如果只是小型修正，有時只要下達「請修正這個 bug」的指示就能得到足夠好的結果。

但在包含數十到數百個任務的開發中，同樣的做法就行不通了。

- 搞不清楚目前正在處理哪個任務
- 變更超出原本沒交代的範圍
- 已實作與已驗證的內容混在一起
- 文件與原始碼狀態不同步
- 收到「已完成」的回報，但在實際環境中無法運作
- 新發現的問題被硬塞進進行中的任務裡

這類問題，光靠 AI 的程式開發能力無法解決。  
重點在於，要把任務設計成讓 AI 能不迷失地工作、讓人類之後也能驗證的形式。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785806122&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxXYGm3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785804503041&amp;bpp=4&amp;bdt=1158&amp;idt=4&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785805945%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1619x969&amp;nras=3&amp;correlator=5732716768021&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=1233&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C42533293%2C122880057&amp;oid=2&amp;pvsid=1345983378044789&amp;tmod=1387686080&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

本文將介紹我們在實際把多個開發任務依序交給 AI 程式開發代理處理時，整理出的以下運作方式：

- 將 `task-list.md` 作為任務管理的唯一正本
- 原則上只允許 1 個任務處於進行中
- 若使用多個 AI，需先決定各自的職責與上下文邊界
- 明文化目的、變更範圍、禁止事項、測試與停止條件
- 將實作、測試、文件更新、提交、Draft PR 視為同一個工作單位
- 不以 AI 自我宣告為準，而是以差異、測試結果與實際環境證據判定完成

另外，本文不會列出特定專案名稱、客戶名稱、儲存庫名稱或實際任務 ID，而是以一般化方式整理。

---

## 大規模開發不能只靠「長 Prompt」來管理

大規模開發最先容易發生的，就是 Prompt 變得巨大。

如果每次都把需求、設計、禁止事項、過去的決策、測試方法、剩餘任務貼到 Prompt 裡，發指令的人和 AI 都很容易看漏重要事項。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3764087313&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785806122&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxXYGm3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785804503055&amp;bpp=1&amp;bdt=1172&amp;idt=1&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785805945%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1619x969%2C952x280&amp;nras=4&amp;correlator=5732716768021&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=2094&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C42533293%2C122880057&amp;oid=2&amp;pvsid=1345983378044789&amp;tmod=1387686080&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

另外，如果只用聊天內容來管理進度，會出現以下問題：

- 在新 session 裡，無法正確承接過去的決策
- 對話中途改變的方針，不知道到底是不是正式規格
- 每次都得重新查一次「上次做到哪裡」
- 多個 AI 或多人一起作業時，認知會分岔
- 雖然有完成回報，卻無法追蹤是根據什麼判定完成

聊天很適合作為工作的入口，但不適合當作專案的正本。

因此，必須在對話之外留下「目前正確狀態」。

---

## 基本方針：將 task-list.md 作為唯一正本

作為任務管理中心，在 repository 內放置 `docs/task-list.md` 。

這裡所說的「唯一正本」，意思是當進度判斷出現分歧時，最後以這個檔案為準。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785806122&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxXYGm3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785804503069&amp;bpp=1&amp;bdt=1186&amp;idt=1&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785805945%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1619x969%2C952x280%2C952x280&amp;nras=5&amp;correlator=5732716768021&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=2936&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C42533293%2C122880057&amp;oid=2&amp;pvsid=1345983378044789&amp;tmod=1387686080&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

即使 Slack、GitHub Issue、聊天內容與 AI 完成報告的記載不同，正式狀態仍以 `task-list.md` 內反映的內容為準。

例如可以使用以下格式：

```
| ID | 任務 | 狀態 | 進度 | 相依性 | 完成條件 | 證據 |
|---|---|---|---:|---|---|---|
| DEV-001 | 建立文章草稿 API | 完成 | 100% | - | API 測試成功 | Test #18 / PR #12 |
| DEV-002 | 防止重複建立 | 進行中 | 70% | DEV-001 | 相同 ID 發送時不會增加投稿 | 等待驗證 |
| DEV-003 | 新增審核畫面 | 未著手 | 0% | DEV-002 | 可執行規格書記載的操作 | - |
```

至少要包含以下資訊。

項目目的ID用來唯一識別任務任務名稱簡短說明要實現什麼狀態顯示未著手、進行中、等待驗證、完成、保留等進度用來補充狀態本身看不出的中間過程相依性明確指出必須先完成的任務完成條件定義什麼才算結束證據連結測試、PR、log、畫面確認等### 任務 ID 途中不可重複使用

一旦發行過的任務 ID，即使該任務已刪除，也不要再拿去用在別的用途上。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=186534417&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785806122&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxXYGm3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785804503077&amp;bpp=2&amp;bdt=1194&amp;idt=2&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785805945%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1619x969%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=5732716768021&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=3758&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C42533293%2C122880057&amp;oid=2&amp;pvsid=1345983378044789&amp;tmod=1387686080&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

如果重複使用，過去的 commit、PR、測試結果、對話裡提到的 ID 都會變成另一個意思。

如果任務不做了，不要刪掉那一列，而是把狀態改成「中止」或「非對象」，並保留理由。

### 新問題要作為新任務登錄

作業中即使發現其他 bug 或改善項目，也不要直接塞進目前任務。

先判斷它屬於以下哪一類：

1. 為了滿足目前的完成條件而必須處理
2. 雖有關聯，但不屬於目前的完成條件
3. 無關的既有問題

若是 1，就在目前任務內處理。  
若是 2 或 3，就登錄成新任務，並繼續目前作業。

如果不這樣切分，一個任務就會無限膨脹，永遠無法完成。

---

## 原則上，進行中的任務限定為 1 件

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1038984369&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785806125&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxXYGm3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785804503099&amp;bpp=4&amp;bdt=1217&amp;idt=4&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785805945%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1619x969%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=8&amp;correlator=5732716768021&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=4664&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=836&amp;eid=95396139%2C42533293%2C122880057&amp;oid=2&amp;psts=AOrYGsmqCQFobLZ2_LEaSlbvianXtbF00JGCOo-CisJMEUrGMNvvnmRJbwsI5qAJJEytrtjydcXe3EoMN-uzNQag4U95qIdYwtMVIZbexrX18IW3BAW7VQMTN8v52_3biNbqCrc%2CAOrYGslsf2oJnkrweMFLgJPUwfoE-bnP4Qp7fi50C9BEatJUb5tJy7FO95J3S191OM0FmlWIoHnLPfHcvO6uR4aP5YOeoMnFFNacnQRnYUFpKAg%2CAOrYGsn0it0D4PYhR8jbSZ6jdFKPis6eg4JSDGpBbhzpzKM0bUfsWBdCA_KXqhiX57HHIiQ3Kl0EHOaG4HO2yFhpxnIfNZr11mBz26npTu1Y8wn9OA%2CAOrYGsmcvzpfT4n6XnMatUvbllbirdyIu8L9g88AGZd1DjGCpT5C_1XHEIUpHzXoTR6wsKrmyh2VY-pekCGePZ7K3Za_FDJNKAXMG67uM_-wpSTwfg%2CAOrYGsn3NPuoTeguNXrCREQA-H33FxyEqaHMz_ehsVS0wsbbrODp6EhQOadKipZkDDnaeaowSOBmwN6zSBzNhRQySxFxmhbLWHjvwPWdlKwUys8v%2CAOrYGslWkJsMp48DAc-E8AX5swfmjw6_vbhSV6zuu5Dcy2NpziZLM2ytJTm9uL6IPS9a0bZ9eakvTX22iEbKr7NeNbgp6VKR5Yi5YCadwCzMTRuk&amp;pvsid=1345983378044789&amp;tmod=1387686080&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1959%2C30%2C1959%2C30%2C1920%2C30%2C1859%2C1050%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

正因為是大規模開發，通常才要把進行中的任務限制為 1 件。

AI 可以快速處理多個工作，但如果在同一個 repository 中同時修改多個任務，人類要檢查的組合就會暴增。

例如，若 3 個任務同時對同一個 API 或 DB 做變更，就很難判斷：

- 哪個變更是 bug 的原因
- 應該在哪個任務的測試中發現
- 規格變更是從哪個任務產生的
- 若要中途回退，該回退到哪裡

因此，基本流程如下：

1. 先決定下一個要著手的任務 1 件
2. 確認開始時的 Git 狀態
3. 只實作該任務
4. 進行測試與 review
5. 更新文件與證據
6. 建立 commit 或 Draft PR
7. 將任務標記為完成或等待驗證
8. 再進入下一個任務

即使要平行化，也只限於不會寫入相同檔案或相同資料的獨立調查與測試。  
若要平行進行多個實作，則要分離 branch 或 Git worktree。

---

## 1 個任務應包含的資訊

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1621299728&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785806170&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxXYGm3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785804503112&amp;bpp=1&amp;bdt=1230&amp;idt=1&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785805945%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1619x969%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=9&amp;correlator=5732716768021&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=5809&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=1950&amp;eid=95396139%2C42533293%2C122880057&amp;oid=2&amp;psts=AOrYGsmqCQFobLZ2_LEaSlbvianXtbF00JGCOo-CisJMEUrGMNvvnmRJbwsI5qAJJEytrtjydcXe3EoMN-uzNQag4U95qIdYwtMVIZbexrX18IW3BAW7VQMTN8v52_3biNbqCrc%2CAOrYGslsf2oJnkrweMFLgJPUwfoE-bnP4Qp7fi50C9BEatJUb5tJy7FO95J3S191OM0FmlWIoHnLPfHcvO6uR4aP5YOeoMnFFNacnQRnYUFpKAg%2CAOrYGsn0it0D4PYhR8jbSZ6jdFKPis6eg4JSDGpBbhzpzKM0bUfsWBdCA_KXqhiX57HHIiQ3Kl0EHOaG4HO2yFhpxnIfNZr11mBz26npTu1Y8wn9OA%2CAOrYGsmcvzpfT4n6XnMatUvbllbirdyIu8L9g88AGZd1DjGCpT5C_1XHEIUpHzXoTR6wsKrmyh2VY-pekCGePZ7K3Za_FDJNKAXMG67uM_-wpSTwfg%2CAOrYGsn3NPuoTeguNXrCREQA-H33FxyEqaHMz_ehsVS0wsbbrODp6EhQOadKipZkDDnaeaowSOBmwN6zSBzNhRQySxFxmhbLWHjvwPWdlKwUys8v%2CAOrYGslWkJsMp48DAc-E8AX5swfmjw6_vbhSV6zuu5Dcy2NpziZLM2ytJTm9uL6IPS9a0bZ9eakvTX22iEbKr7NeNbgp6VKR5Yi5YCadwCzMTRuk%2CAOrYGslg7JGb00jSb4ZJpGeh7i56Q7znZgBFi-cMLZqynSSB7F8GgdZIBzXz2hrVcr2TwryXWblh0v_B220QBH6SCmVz7Byx0DumGzMW6RZc5z__&amp;pvsid=1345983378044789&amp;tmod=1387686080&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1959%2C30%2C1959%2C30%2C1920%2C30%2C1859%2C1050%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

交給 AI 的任務，至少要包含以下 6 項。

### 1\. 目的

不只寫「要改什麼」，也要寫「為什麼要改」。

不好範例：

```
請修正重複建立。
```

改善範例：

```
即使外部系統重新送出相同的候選資料，
也不要建立出多個相同的草稿。
請將重送視為正常流程，重用既有的草稿。
```

目的清楚後，AI 就不只是把錯誤遮起來，而是能以使用者預期的狀態為基準來實作。

### 2\. 變更範圍

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3970593978&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785806173&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxXYGm3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785804503120&amp;bpp=1&amp;bdt=1238&amp;idt=1&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785805945%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1619x969%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280&amp;nras=10&amp;correlator=5732716768021&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=6644&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=2776&amp;eid=95396139%2C42533293%2C122880057&amp;oid=2&amp;psts=AOrYGsmqCQFobLZ2_LEaSlbvianXtbF00JGCOo-CisJMEUrGMNvvnmRJbwsI5qAJJEytrtjydcXe3EoMN-uzNQag4U95qIdYwtMVIZbexrX18IW3BAW7VQMTN8v52_3biNbqCrc%2CAOrYGslsf2oJnkrweMFLgJPUwfoE-bnP4Qp7fi50C9BEatJUb5tJy7FO95J3S191OM0FmlWIoHnLPfHcvO6uR4aP5YOeoMnFFNacnQRnYUFpKAg%2CAOrYGsn0it0D4PYhR8jbSZ6jdFKPis6eg4JSDGpBbhzpzKM0bUfsWBdCA_KXqhiX57HHIiQ3Kl0EHOaG4HO2yFhpxnIfNZr11mBz26npTu1Y8wn9OA%2CAOrYGsmcvzpfT4n6XnMatUvbllbirdyIu8L9g88AGZd1DjGCpT5C_1XHEIUpHzXoTR6wsKrmyh2VY-pekCGePZ7K3Za_FDJNKAXMG67uM_-wpSTwfg%2CAOrYGsn3NPuoTeguNXrCREQA-H33FxyEqaHMz_ehsVS0wsbbrODp6EhQOadKipZkDDnaeaowSOBmwN6zSBzNhRQySxFxmhbLWHjvwPWdlKwUys8v%2CAOrYGslWkJsMp48DAc-E8AX5swfmjw6_vbhSV6zuu5Dcy2NpziZLM2ytJTm9uL6IPS9a0bZ9eakvTX22iEbKr7NeNbgp6VKR5Yi5YCadwCzMTRuk%2CAOrYGslg7JGb00jSb4ZJpGeh7i56Q7znZgBFi-cMLZqynSSB7F8GgdZIBzXz2hrVcr2TwryXWblh0v_B220QBH6SCmVz7Byx0DumGzMW6RZc5z__%2CAOrYGsllOVUHCENL6c6AXwQbvTfzN5aKVKyWHheQM3ph9DRVytyf_P7LaC0Wb0q7DDudv4luuxj3XoJDOvc8agn4uxAcD2sJrjP58NkTn_z8j_s&amp;pvsid=1345983378044789&amp;tmod=1387686080&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1959%2C30%2C1959%2C30%2C1920%2C30%2C1859%2C1050%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

明確列出可以調查與修改的檔案或功能。

```
變更對象：
- 草稿登錄 API
- 處理外部候選 ID 的投稿 metadata
- 對應的自動化測試
- 相關設計文件與 task-list.md
```

縮小變更範圍，可以避免 AI 連「順手重構」都一起做了。

### 3\. 禁止事項

明確寫出不能做的事。

```
禁止事項：
- 不要修改已公開資料
- 不要變更 API 既有回應格式
- 不要一次修正與本任務無關的警告
- 不要為了讓測試通過而把實作改成符合期待值
- 不要在沒有根據下自行補完規格
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3355444817&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1785806197&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxXYGm3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785804503127&amp;bpp=2&amp;bdt=1245&amp;idt=2&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785805945%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785805945%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1619x969%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=5732716768021&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=159&amp;ady=7501&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=3668&amp;eid=95396139%2C42533293%2C122880057&amp;oid=2&amp;psts=AOrYGsmqCQFobLZ2_LEaSlbvianXtbF00JGCOo-CisJMEUrGMNvvnmRJbwsI5qAJJEytrtjydcXe3EoMN-uzNQag4U95qIdYwtMVIZbexrX18IW3BAW7VQMTN8v52_3biNbqCrc%2CAOrYGslsf2oJnkrweMFLgJPUwfoE-bnP4Qp7fi50C9BEatJUb5tJy7FO95J3S191OM0FmlWIoHnLPfHcvO6uR4aP5YOeoMnFFNacnQRnYUFpKAg%2CAOrYGsn0it0D4PYhR8jbSZ6jdFKPis6eg4JSDGpBbhzpzKM0bUfsWBdCA_KXqhiX57HHIiQ3Kl0EHOaG4HO2yFhpxnIfNZr11mBz26npTu1Y8wn9OA%2CAOrYGsmcvzpfT4n6XnMatUvbllbirdyIu8L9g88AGZd1DjGCpT5C_1XHEIUpHzXoTR6wsKrmyh2VY-pekCGePZ7K3Za_FDJNKAXMG67uM_-wpSTwfg%2CAOrYGsn3NPuoTeguNXrCREQA-H33FxyEqaHMz_ehsVS0wsbbrODp6EhQOadKipZkDDnaeaowSOBmwN6zSBzNhRQySxFxmhbLWHjvwPWdlKwUys8v%2CAOrYGslWkJsMp48DAc-E8AX5swfmjw6_vbhSV6zuu5Dcy2NpziZLM2ytJTm9uL6IPS9a0bZ9eakvTX22iEbKr7NeNbgp6VKR5Yi5YCadwCzMTRuk%2CAOrYGslg7JGb00jSb4ZJpGeh7i56Q7znZgBFi-cMLZqynSSB7F8GgdZIBzXz2hrVcr2TwryXWblh0v_B220QBH6SCmVz7Byx0DumGzMW6RZc5z__%2CAOrYGsllOVUHCENL6c6AXwQbvTfzN5aKVKyWHheQM3ph9DRVytyf_P7LaC0Wb0q7DDudv4luuxj3XoJDOvc8agn4uxAcD2sJrjP58NkTn_z8j_s%2CAOrYGsnN9l7vfaS1VMQTmPCpU9cI2cROvR9NuRF7ZZs9tzf-moQWMHGmQ-keUkZW082_82OKIfhigquS3i9tqheQJzVjmW9BI7YtsX14W8tQWxA&amp;pvsid=1345983378044789&amp;tmod=1387686080&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1959%2C30%2C1959%2C30%2C1920%2C30%2C1859%2C1050%2C1619%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

特別是「不要用推測補上不明點」這條非常重要。

### 4\. 完成條件

完成條件要寫成第三方能以 Yes／No 判定的形式。

```
完成條件：
- 第一次請求時會建立 1 筆草稿
- 即使同一個外部候選 ID 重新送出，也不會增加新投稿
- 第二次會回傳與第一次相同的投稿 ID
- 不同的外部候選 ID 會建立不同的草稿
- 包含既有測試在內的相關測試全部通過
```

像「能正常運作」「已妥善實作」這類說法，不能當作判定標準。

### 5\. 測試方法

指定要在哪一層驗證什麼內容。

測試　確認內容Unit Test判定邏輯本身是否正確Integration TestAPI、DB、外部 ID 的串接Regression Test既有功能是否被破壞實際環境確認在接近正式環境的配置下是否可操作不是所有任務都必須做齊全部類型。  
但要清楚區分哪些測試有做、哪些沒有做，並要求如實回報。

### 6\. 停止條件

定義 AI 不能自行判斷的界線。

```
遇到以下情況時，請停止作業並在不變更任何內容的情況下回報：
- 規格文件彼此有矛盾
- 需要刪除目標資料
- DB migration 會影響既有資料
- 需要認證資訊或額外的正式環境權限
- 超出 task-list.md 記載的變更範圍
- 開始時工作樹中已有未確認變更
```

停止條件不是在限制 AI 的能力。  
而是為了先把應由人類判斷的部分切分出來。

---

## 任務指示模板

實務上，可以使用如下模板：

```
# 目標任務

DEV-002 防止重複建立

## 開始前確認

- 確認目前 branch、HEAD、git status
- 閱讀 task-list.md 與相關規格文件
- 確認前置任務 DEV-001 已完成
- 若工作樹中已有既有變更，則停止

## 目的

即使同一個外部候選被重新送出，也不要重複建立新的草稿。

## 變更範圍

- 草稿登錄 API
- 外部候選 ID 的保存與查詢處理
- 相關測試
- 相關文件

## 禁止事項

- 不要變更既有 API 的回應格式
- 不要修改已公開投稿
- 不要進行無關的重構
- 規格不明時不要用推測實作

## 完成條件

- 只會在第一次建立草稿
- 重送時會回傳既有投稿 ID
- 不同候選 ID 會被當成不同投稿建立
- 相關測試與既有測試都成功

## 完成時要做的事

- 更新 task-list.md 的狀態、進度與證據
- 自我檢查變更差異
- 建立包含任務 ID 的 commit
- 視需要建立 Draft PR
- 依指定格式回報完成

## 停止條件

- 若發現規格矛盾、破壞性變更、權限不足、既有變更，則停止
```

每次都要用到的規則，與其反覆貼在 prompt 裡，不如放進 repository 內可持續維護的指示檔。

Codex 可使用 `AGENTS.md` ，Claude Code 可使用 `CLAUDE.md` 。若兩者都要用，建議把共通規則集中在同一處，再由另一方引用，以減少重複。

但不應把單一任務的目的與完成條件也塞進長期規則裡。

放置位置內容 `AGENTS.md` / `CLAUDE.md` 測試指令、禁止事項、Git 運作、review 標準 `task-list.md` 任務狀態、相依性、完成條件、證據個別任務指示這次的目的、對象範圍、特定確認事項設計文件系統整體規格、資料結構、判斷理由---

## 分開「已實作」與「已完成」

在大規模開發中，特別重要的是把狀態細分。

只有程式碼存在，不代表功能就正確。

例如，下列狀態都不同：

狀態意義未著手調查與實作都還沒開始調查中正在確認規格或既有程式碼實作中正在修改程式碼本地已驗證自動測試等已成功實際環境等待驗證還剩下部署端或真實資料的確認完成已滿足定義的完成條件與必要驗證保留等待外部回覆、權限、規格決定等如果沒有這種區分，AI 可能在把程式寫出來後就回報「完成」，而人類會誤以為已能在實際環境使用。

若還有只能在實際環境確認的項目，不要硬把進度設成 100%，而要標示為「實作完成・等待實際環境驗證」。

---

## 完成報告要以「證據」為中心，而不是「做了什麼」

即使 AI 回報像下面這樣，也不足以作為完成確認。

```
已實作防止重複建立的功能。
也新增了測試，運作沒問題。
```

真正需要的是第三方能追蹤的資訊。

```
## 結果

- 已實作 DEV-002
- 第一次請求：建立投稿 ID 120
- 使用相同外部候選 ID 的第二次：重用投稿 ID 120
- 投稿數量：維持 1 筆
- 不同的候選 ID：新建立投稿 ID 121

## 測試

- 目標測試：12 件成功、0 件失敗
- 相關回歸測試：48 件成功、0 件失敗
- 實際環境確認：未執行

## Git

- 開始時 HEAD：abc1234
- 結束時 HEAD：def5678
- branch：feature/DEV-002-idempotency
- commit：DEV-002 Prevent duplicate drafts

## 剩餘事項

- 部署到驗證環境後，還需要確認相同請求的重送
- 將 task-list.md 更新為「等待實際環境驗證，90%」
```

### 想確認的證據

- 變更檔案列表
- Git diff
- 執行過的指令
- 測試件數與結果
- 重現步驟與修正後結果
- 建立或更新的資料識別碼
- 尚未執行的確認項目
- commit SHA 或 PR URL
- 開始時與結束時的 `git status`

重點不是把所有事情看起來都包裝成成功。  
而是要如實回報尚未確認的事項。

---

## 將實作、測試、文件更新、Git 視為同一單位

如果先大量實作程式碼，之後才一起更新測試與文件，就會搞不清楚哪些更新對應到哪個變更。

因此，一個任務要用以下單位來收斂：

1. 規格確認
2. 實作
3. 新增與執行測試
4. 差異 review
5. 更新 `task-list.md` 與相關文件
6. commit
7. 視需要建立 Draft PR
8. 完成回報

在 commit message 或 branch 名稱中加入任務 ID，之後會更容易追蹤。

```
feature/DEV-002-idempotency
```
```
fix(DEV-002): prevent duplicate draft creation
```

Draft PR 不代表「全部都完成了」，而是用來表示變更已整理到可供 review 的單位。

若還有實際環境驗證尚未完成，必須在 PR 內文與 `task-list.md` 兩邊都明確標示。

---

## 將 Plan 與實作分開

對於變更範圍較廣的任務，不要一開始就直接實作，先只要求調查與規劃。

在規劃階段要確認：

- 參考了哪些規格與程式碼
- 預計修改哪些檔案
- 對既有功能的影響
- 對 DB 與 API 相容性的影響
- 要新增哪些測試
- 需要判斷的不明點
- 回復方法

如果在這個階段發現變更範圍比預期更大，就要拆分任務。

無論是 Claude Code 還是 Codex，都可以使用在變更前先確認規劃的 Plan 類流程。  
但光是做出計畫並不代表就安全。人類要確認這份計畫是否與 `task-list.md` 的完成條件一致，這點很重要。

---

## 若使用多個 AI，要先決定職責與上下文邊界

如果把 ChatGPT、Claude Code、Codex 等組合起來，就可以分工處理需求整理、實作、review。

但讓多個 AI 都理解同一個 repository 全貌，不一定會提升準確度。若沒有先分配職責一起使用，它們往往會讀同樣的程式與設計文件，重複做相似的調查與說明。

例如可以這樣分工：

擔任者主要職責主要參考資訊輸出人類優先順位、規格判斷、完成判定任務清單、證據、業務需求開始判斷、核准、退回ChatGPT需求整理、指示撰寫、結果 review目標任務、必要規格、變更差異實作指示、指摘事項Claude Code / Codex 程式碼調查、實作、測試目標任務所需的 repository 內資訊差異、測試結果、完成回報重點是，不要在 AI 之間的交接內容裡，每次都附上整個 repository 的說明。

交接資訊原則上只縮限於以下範圍：

- 目標任務 ID 與目的
- 變更對象與禁止變更範圍
- 完成條件與停止條件
- 實際變更差異
- 測試結果與未確認事項
- 需要判斷的論點

實作負責的 AI 會直接從 repository 內確認必要檔案。review 負責的 AI 則先接收目標任務、差異與測試結果，必要時才再確認相關檔案。

像這樣連「誰要看什麼」都先決定好，才是多 AI 運作的 harness 設計。

---

## 先決定 AI 應該在哪些情況停下來

在把大規模開發交給 AI 時，「希望它自律推進」與「不希望它自行決定」可以同時成立。

日常判斷可以交給 AI，但只要涉及業務、契約、資安、破壞性變更，就要停下來。

### 可以直接繼續的例子

- 依照既有格式新增測試
- 實作規格書明列的輸入檢查
- 在範圍內做輕微重構
- 用 lint 或 formatter 整理目標檔案
- 對已重現 bug 做最小限度修正

### 應停下來確認的例子

- 無法判定該以規格書還是實作為準
- 需要刪除或轉換既有資料
- 可能破壞公開 API 相容性
- 認證、付款、個資的處理方式要改變
- 需要變更正式環境設定
- 需要超出任務範圍的大幅設計修改
- 與使用者既有變更衝突

停止時的回報也要有固定格式。

```
## 停止理由

規格書 A 寫明重複時要回傳 409，但規格書 B 則寫成回傳既有 ID。

## 已確認

- 對象：API 規格書 4.2、基本設計書 7.1
- 目前實作：回傳 409
- 檔案變更：無
- Git 狀態：從開始到現在都沒有變更

## 需要判斷的事項

重複時的正式回應，應該是 409 還是回傳既有 ID。
```

「因為不知道所以停止」不是失敗。  
比起毫無根據地實作、增加返工，這是更安全的成果。

---

## 常見失敗例

### 失敗例 1：同時讓 ChatGPT 與 Claude Code 讀整個 GitHub

實際上，我曾經把 ChatGPT 與 GitHub 串接，並把實作交給 Claude Code 的運作方式。

當初流程如下：

1. ChatGPT 讀 GitHub，整理狀況並產出給 Claude Code 的指示
2. Claude Code 也重新讀同一個 repository 與設計文件後開始實作
3. 把 Claude Code 的完成回報交回 ChatGPT
4. ChatGPT 再確認 GitHub 並產生追加指示
5. Claude Code 接到指摘後，又重新讀相關檔案

這樣來回反覆時，同樣的程式碼、設計文件、完成回報會一次又一次進入 context。確認精度並沒有因為讀得更多而提升，反而只是不正常地消耗 token。

問題不在於使用了多個 AI，也不在於把較大範圍交給 AI。  
真正的問題在於，沒有先在 harness 端設計以下事項，就讓兩個 AI 同時理解整個專案：

- 哪個 AI 負責整理需求
- 哪個 AI 負責實作
- 哪個 AI 負責 review 什麼
- AI 之間要交換哪些資訊
- 允許重新讀取的範圍到哪裡
- 最終完成由誰判定

之後我把 ChatGPT 限定為需求整理與 review，Claude Code 限定為實作與測試。此外，交接內容只保留目標任務、變更對象、完成條件、差異、測試結果、未確認事項。

對策：

- 先決定每個 AI 的職責
- 不要每次都讓多個 AI 讀整個 repository
- review 要從任務、差異、測試結果開始
- 追加調查只限於有疑點的地方
- 不要讓 AI 彼此直接來回對話，而是由人類判定繼續、停止、完成

越是擴大 AI 的負責範圍，就越需要人類設計 AI 之間的職責分工與上下文邊界。

### 失敗例 2：一次要求多個任務一起做

```
請把所有尚未完成的任務從上到下全部實作完。
```

這種要求一旦在中途遇到規格矛盾或測試失敗，就很難判斷哪些變更該保留。

對策：

- 確認相依性後，一件一件進行
- 如果要平行處理獨立任務，就分開 branch 或 worktree
- 每個任務都獨立成 commit 或 PR

### 失敗例 3：「剩下的就請你自行處理好」

AI 並不會完全掌握優先順序、業務重要性與外部相依。

對策：

- 明確指定下一個要開始的任務 ID
- 在 `task-list.md` 記錄優先順序與相依性
- 若需要判斷，讓 AI 提出候選方案與理由，再由人類選擇

### 失敗例 4：只靠 AI 的完成回報就直接設成 100%

即使測試程式碼通過，也可能因為實際環境設定或資料條件而無法運作。

對策：

- 每個完成條件都要確認證據
- 區分本地驗證與實際環境驗證
- 若尚未驗證，就保留為「等待驗證」

### 失敗例 5：為了讓測試通過而改規格

如果 AI 把失敗測試的期待值改成符合實作，雖然測試會過，但其實沒有滿足原本需求。

對策：

- 明確寫出期待值背後的規格依據
- 修改測試時，要回報修改理由
- 修正前先重現失敗，修正後再用相同條件確認成功

### 失敗例 6：只有文件更新在前進

就算把任務清單更新成 100%，程式碼或環境本身也不會因此改變。

對策：

- 先確認程式碼差異、測試與實際環境證據，再更新狀態
- 分清楚只是文件變更，還是有功能實作
- 也要記錄「沒確認到什麼」

### 失敗例 7：每次都在 prompt 裡提醒同樣的失誤

每次都重複寫同樣的注意事項，遲早會漏掉。

對策：

- 把反覆出現的規則移到 `AGENTS.md` 或 `CLAUDE.md`
- 可用機器偵測的內容移到 CI 或 lint
- 可重複使用的工作流程做成 skill 或 template

---

## 實務使用的檢查清單

### 專案開始時

- 決定任務管理的正本
- 決定任務 ID 命名規則
- 決定狀態與進度百分比的定義
- 設定完成條件與證據欄位
- 明確設計文件、測試計畫、任務清單之間的關係
- 準備 AI 一定會讀的永久指示檔
- 記載 build、測試、lint 的執行方式
- 記載禁止事項與停止條件
- 若使用多個 AI，要先決定各自職責
- 決定 AI 之間要交接的資訊與重新讀取範圍

### 任務開始前

- 將目標任務 ID 限定為 1 件
- 確認相依任務是否已完成
- 確認目前 branch、HEAD、 `git status`
- 確認工作樹中沒有既有未提交變更
- 確認目的、變更範圍、禁止事項
- 確認完成條件能否用 Yes／No 判定
- 決定需要的測試與實際環境確認
- 確認是否有其他 AI 正在重複調查同一件事

### 實作後

- 檢查差異中是否混入非目標變更
- 比較修正前的失敗與修正後的成功
- 執行相關既有測試
- 明確列出尚未執行的驗證項目
- 更新任務清單與相關設計文件
- 在 commit 中加入任務 ID
- 確認 Draft PR 說明與實際差異一致
- 記錄開始與結束時的 Git 狀態
- 交給下一個 AI 的資訊只保留目標任務、差異、測試結果、未確認事項

### 判定完成時

- 所有完成條件都能用證據確認
- 沒有把「已實作」與「已驗證實際環境」混為一談
- 不只看 AI 說法，也確認了 Git diff
- 確認測試件數與失敗件數
- 新發現的問題已記錄為另一個任務
- 保留事項、外部依賴、已知限制都有留下

---

## 總結

把大規模開發交給 Claude Code 或 Codex 時，重點不是寫很長的 prompt。

重點是以下 6 件事：

1. 將 `task-list.md` 作為進度管理的唯一正本
2. 原則上只保留 1 個進行中任務
3. 若使用多個 AI，先決定職責與上下文邊界
4. 將目的、變更範圍、禁止事項、完成條件、停止條件明文化
5. 把實作、測試、文件更新、Git 操作視為同一個工作單位
6. 不以 AI 自我宣告為準，而是以差異、測試結果、實際環境證據判定完成

AI 程式開發代理很擅長執行明確的任務。  
但它們不會自動正確決定專案優先順序、業務判斷、風險承受度。

由人類管理「要做什麼、做到哪裡、交給誰、以什麼為完成」，由 AI 推進調查、實作、驗證、記錄。  
只要能做到這樣，即使是數十件、數百件任務，也比較能在不迷失狀態下持續開發。

交給 AI 的範圍越大，就越不是減少管理，而是要把職責、資訊流向、完成條件變成可判定的形式。

---

## 參考資料

- OpenAI: Best practices for Codex  
	[https://developers.openai.com/codex/learn/best-practices](https://developers.openai.com/codex/learn/best-practices)
- OpenAI: Custom instructions with [AGENTS.md](http://agents.md/)  
	[https://developers.openai.com/codex/agent-configuration/agents-md](https://developers.openai.com/codex/agent-configuration/agents-md)
- Anthropic: How Claude remembers your project  
	[https://docs.anthropic.com/en/docs/claude-code/memory](https://docs.anthropic.com/en/docs/claude-code/memory)
- Anthropic: Common workflows  
	[https://docs.anthropic.com/en/docs/claude-code/common-workflows](https://docs.anthropic.com/en/docs/claude-code/common-workflows)

---

原文出處： [https://qiita.com/Y-Y-dev/items/d526fb7cdbe35a3f9384](https://qiita.com/Y-Y-dev/items/d526fb7cdbe35a3f9384)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xXYGm3) [60瀏覽](https://codelove.tw/@tony/post/xXYGm3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xXYGm3)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝10 ❤️4

303

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 💬4 ❤️1

58

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1785804502&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxXYGm3&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785804502073&amp;bpp=17&amp;bdt=123&amp;idt=117&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785804494%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785804494%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785804494%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5732716768021&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C42533293%2C122880057&amp;oid=2&amp;pvsid=1345983378044789&amp;tmod=1387686080&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=151" title="Advertisement" aria-label="Advertisement"></iframe><iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260729/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=11&amp;uci=a!b&amp;btvi=5" title="Advertisement" aria-label="Advertisement"></iframe>