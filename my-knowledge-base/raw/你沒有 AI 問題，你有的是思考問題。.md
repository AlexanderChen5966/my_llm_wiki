---
title: "你沒有 AI 問題，你有的是思考問題。"
source: "https://codelove.tw/@tony/post/apoYQ3"
author:
published:
created: 2026-08-12
description: "我以前以為 AI 讓我變懶了。後來我發現我錯了。不是 AI 讓我變懶，而是我把 AI 當成不去思考的藉口。一旦我注意到這件事，我就開始在各處看到同樣的模式——在我自己的程式碼裡、在我 review 的 PR 裡、在那些寫著「AI 說這樣應該可行」的 Slack 訊息裡，彷彿那就代表對話已經結束了..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月12日](https://codelove.tw/@tony/post/apoYQ3 "2026-08-12 08:45:11")

## 你沒有 AI 問題，你有的是思考問題。

我以前以為 AI 讓我變懶了。後來我發現我錯了。不是 AI 讓我變懶，而是我把 AI 當成不去思考的藉口。

一旦我注意到這件事，我就開始在各處看到同樣的模式——在我自己的程式碼裡、在我 review 的 PR 裡、在那些寫著「AI 說這樣應該可行」的 Slack 訊息裡，彷彿那就代表對話已經結束了。

所以我想反問你跟我當初一樣的問題：當你遇到棘手問題時，你的第一反應是什麼？

你會先思考？還是直接打開你的 AI 助手？

老實說。反正沒人在看。那一秒鐘的選擇，可能比你 GitHub 的連勝紀錄更能說明你目前的開發習慣。

---

## 我注意到一件怪事

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786519726&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoYQ3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTA4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEwOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEwOCJdXSwwXQ..&amp;dt=1786501443577&amp;bpp=5&amp;bdt=1433&amp;idt=6&amp;shv=r20260811&amp;mjsv=m202608060101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786501442%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908&amp;nras=3&amp;correlator=769561590536&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1054&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C95397129%2C95398731%2C95399118%2C122880127%2C122880160%2C122880322&amp;oid=2&amp;pvsid=6093479015012457&amp;tmod=628454059&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

幾個月前，我在做一個 Node.js 後端 API。某個端點一直回傳一個欄位為 `null` 的回應，不是每次都發生，但頻率高到讓人很煩，而且很難定位。

如果是一年前，我大概會接下來花 30～60 分鐘：

- 重讀這個端點和周邊程式碼
- 檢查那個欄位原本應該在哪裡被填入
- 從頭到尾追蹤這個請求
- 試一個做法
- 把它弄壞
- 修好它
- 最後才搞定

但那次我打開 AI 助手，直接描述這個 bug：資料庫裡明明有資料，回應裡那個欄位卻是 `null` 。

它提出了一個修正：呼叫鏈上少了一個 `await` ，所以在依賴的值還沒真正 resolve 之前，就先讀取了那個欄位。我照著修了。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3764087313&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786519726&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoYQ3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTA4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEwOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEwOCJdXSwwXQ..&amp;dt=1786501443592&amp;bpp=1&amp;bdt=1448&amp;idt=1&amp;shv=r20260811&amp;mjsv=m202608060101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786501442%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280&amp;nras=4&amp;correlator=769561590536&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1882&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C95397129%2C95398731%2C95399118%2C122880127%2C122880160%2C122880322&amp;oid=2&amp;pvsid=6093479015012457&amp;tmod=628454059&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

那個欄位不再是 `null` 了。

這本來應該讓我覺得很爽，但沒有。

修正確實有效，但當我試著解釋為什麼它有效——為什麼那個 `await` 這麼重要、到底是哪個東西在跟哪個東西競爭——我說不出來。我手上有一個能運作的端點，卻在本該理解的地方留下了一個空洞。

那就是這篇文章的起點。

---

## 我們把思考變成可選項了

在 AI 變成日常開發的一部分之前，我們很多人學會的流程是這樣：

```
問題 → 困惑 → 研究 → 假設 → 實驗 → 失敗 → 理解 → 解決
```

而現在越來越常見的流程是這樣：

```
問題 → 提示 → 答案 → 複製 → 完成
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786519726&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoYQ3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTA4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEwOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEwOCJdXSwwXQ..&amp;dt=1786501443599&amp;bpp=1&amp;bdt=1455&amp;idt=1&amp;shv=r20260811&amp;mjsv=m202608060101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786501442%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280&amp;nras=5&amp;correlator=769561590536&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2761&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C95397129%2C95398731%2C95399118%2C122880127%2C122880160%2C122880322&amp;oid=2&amp;pvsid=6093479015012457&amp;tmod=628454059&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

第二種流程可以快得多。也正因如此，它才這麼有吸引力。

但第一種流程，才是原本發生「學習」的地方。困惑不是流程中的 bug，它本來就是流程。掙扎不是浪費時間，它就是機制本身。

所以讓我精準地說出真正的問題是什麼，因為這跟大多數人以為的不一樣：

> 問題不在於 AI 給了我們答案。問題在於，我們在還沒來得及形成自己的問題之前，就先拿到了答案。

再讀一次。這整篇文章其實就是這一句話。

---

## 思考不是盯著空白編輯器發呆

當人們說「你就好好想一下問題」時，聽起來很空泛。其實不是。對開發者來說，思考是一組具體、可學習的動作：

- 把問題拆成更小的部分
- 在測試前先形成假設
- 預測系統「應該」如何運作
- 在提出解法前先理解限制條件
- 權衡取捨，而不只是看結果
- 在除錯時質疑自己的假設
- 先問「為什麼會有這個設計？」再問「我要怎麼修？」
- 決定哪些東西 **不該** 被實作

寫程式只是軟體開發中很小的一部分。更困難、也更有價值的部分，是先決定究竟該存在什麼程式碼，而那一部分不會出現在提示框裡。

---

## 最可怕的 AI 答案，是正確的答案

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=186534417&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786519730&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoYQ3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTA4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEwOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEwOCJdXSwwXQ..&amp;dt=1786501443608&amp;bpp=1&amp;bdt=1464&amp;idt=1&amp;shv=r20260811&amp;mjsv=m202608060101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786501442%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=7&amp;correlator=769561590536&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4028&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=410&amp;eid=95396139%2C95397129%2C95398731%2C95399118%2C122880127%2C122880160%2C122880322&amp;oid=2&amp;psts=AOrYGsngZOTai1Majly1hb4xgiCUz2X7w2VhrH8qiqYVHiq_i5QDVCROQQf84aPcv_R002_nKFiL39hp44Fm22AzmFIjU47G4hjJLY_MhWyk-EA5fN7GgnkEuvqmHDRv2qj-tMc%2CAOrYGskRXY79gJCI-nvMTtXgXe7No73Du2yKsRLZC_68-sBk5LbCR8PcBLLker039Mplum3Z5JfND9bbpbQ6bPu8rKlODxZKUTDfF8fMZPAistHPkA%2CAOrYGsndIf5b3zN2xoVctftDwhMm86Nh6jz6SJq2GppY4U1oQpFL8VuJHBkgA4z-vUJBhoOOayxH7kYYeYKLTTxc3rm6ZwdW-Fu5zxSmyr6igJj4ESg%2CAOrYGskAF3m7b_jJ-t5E89XvpYzZT8Jz0CQir_g67EhNeAzMEFDA11wkpUcScJzdTYjdSMBlYXDrPvClBis4JFlE_Gm7SYtClXf4UEsFpa4EIaIn6Q&amp;pvsid=6093479015012457&amp;tmod=628454059&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這點很少有人談。

錯誤的答案比較容易察覺。它會壞掉，你去查，然後在修的過程中學到一些東西。

但 **正確** 的答案，你會信任。而信任，正是思考悄悄退場的地方。

當我們被動地使用 AI 時，流程可能會變成：

```
問題 → 解答
```

除非你刻意逼它，否則它跳過的是這一段：

```
問題 → 為什麼？ → 限制條件？ → 替代方案？ → 取捨？ → 解法
```

一個正確答案，可能掩蓋了缺失的心智模型。你帶著能運作的程式碼離開，卻在本該理解的地方留下空白；這個空白要到三個月後、在正式環境、凌晨兩點、當「為什麼」終於變得重要而你卻答不出來時，才會浮現。

---

## 實驗：刻意繳「思考稅」

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1038984369&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786519752&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoYQ3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTA4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEwOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEwOCJdXSwwXQ..&amp;dt=1786501443618&amp;bpp=1&amp;bdt=1475&amp;idt=1&amp;shv=r20260811&amp;mjsv=m202608060101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786501442%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=8&amp;correlator=769561590536&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4944&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=1333&amp;eid=95396139%2C95397129%2C95398731%2C95399118%2C122880127%2C122880160%2C122880322&amp;oid=2&amp;psts=AOrYGsngZOTai1Majly1hb4xgiCUz2X7w2VhrH8qiqYVHiq_i5QDVCROQQf84aPcv_R002_nKFiL39hp44Fm22AzmFIjU47G4hjJLY_MhWyk-EA5fN7GgnkEuvqmHDRv2qj-tMc%2CAOrYGskRXY79gJCI-nvMTtXgXe7No73Du2yKsRLZC_68-sBk5LbCR8PcBLLker039Mplum3Z5JfND9bbpbQ6bPu8rKlODxZKUTDfF8fMZPAistHPkA%2CAOrYGsndIf5b3zN2xoVctftDwhMm86Nh6jz6SJq2GppY4U1oQpFL8VuJHBkgA4z-vUJBhoOOayxH7kYYeYKLTTxc3rm6ZwdW-Fu5zxSmyr6igJj4ESg%2CAOrYGskAF3m7b_jJ-t5E89XvpYzZT8Jz0CQir_g67EhNeAzMEFDA11wkpUcScJzdTYjdSMBlYXDrPvClBis4JFlE_Gm7SYtClXf4UEsFpa4EIaIn6Q%2CAOrYGskqxfeUn92EXKSqio_WfmbhaW4MBvR9C9upZrtFvutwEc8FABLqRBY349OEx4hxTP-795xEqOLHIeGdJSsJ7n_eM91Ubk9IePV4ucw3OsorKXw&amp;pvsid=6093479015012457&amp;tmod=628454059&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

所以我試了個不同的方法。在我可以打開 AI 助手之前，先強迫自己繳一筆「思考稅」。

**規則 1** - 對大多數不算太簡單的問題，先花幾分鐘思考，再去找 AI。（當然，這不適用於凌晨 2 點的正式環境事故；這是給日常問題用的，不是給救火用的。）

**規則 2** - 在問 AI 之前，先寫下我的假設。

**規則 3** - 當 AI 給出答案時，不要先問「這正確嗎？」改問：

> 「我漏了什麼？」

**規則 4** - 要三種替代方案，不要只要一個解法。

**規則 5** - 用自己的話把最後的解法大聲講出來，就像在教別人一樣。

有趣的地方不是我不用 AI 就能解決的問題變少了，而是當我真的打開 AI 助手時，我的問題變得更精準了。我不再要它替我思考；我是在請它檢查我的思考。

這才是真正的轉變： **AI 從解答機器 → 變成思考夥伴。**

---

## THINK 方法

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1621299728&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786519760&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoYQ3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTA4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEwOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEwOCJdXSwwXQ..&amp;dt=1786501443626&amp;bpp=1&amp;bdt=1483&amp;idt=1&amp;shv=r20260811&amp;mjsv=m202608060101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786501442%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280&amp;nras=9&amp;correlator=769561590536&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5869&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=2266&amp;eid=95396139%2C95397129%2C95398731%2C95399118%2C122880127%2C122880160%2C122880322&amp;oid=2&amp;psts=AOrYGsngZOTai1Majly1hb4xgiCUz2X7w2VhrH8qiqYVHiq_i5QDVCROQQf84aPcv_R002_nKFiL39hp44Fm22AzmFIjU47G4hjJLY_MhWyk-EA5fN7GgnkEuvqmHDRv2qj-tMc%2CAOrYGskRXY79gJCI-nvMTtXgXe7No73Du2yKsRLZC_68-sBk5LbCR8PcBLLker039Mplum3Z5JfND9bbpbQ6bPu8rKlODxZKUTDfF8fMZPAistHPkA%2CAOrYGsndIf5b3zN2xoVctftDwhMm86Nh6jz6SJq2GppY4U1oQpFL8VuJHBkgA4z-vUJBhoOOayxH7kYYeYKLTTxc3rm6ZwdW-Fu5zxSmyr6igJj4ESg%2CAOrYGskAF3m7b_jJ-t5E89XvpYzZT8Jz0CQir_g67EhNeAzMEFDA11wkpUcScJzdTYjdSMBlYXDrPvClBis4JFlE_Gm7SYtClXf4UEsFpa4EIaIn6Q%2CAOrYGskqxfeUn92EXKSqio_WfmbhaW4MBvR9C9upZrtFvutwEc8FABLqRBY349OEx4hxTP-795xEqOLHIeGdJSsJ7n_eM91Ubk9IePV4ucw3OsorKXw%2CAOrYGsklLQWbIqWlk21Orhc5SYccHyid_4ib8mVPpyzZwgnztj8idvXIqaLXOurwZi0hzkisqQXPJp7t5fXUMZ4i7iBsuelV24rdnlDmKDubUXlo-w&amp;pvsid=6093479015012457&amp;tmod=628454059&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

如果你想要一個比「多一點自覺」更可重複的方法（說實話，這種話通常沒人真的持續做到），那這就是我從那次實驗中整理出來的框架。

**在問 AI 之前，先 THINK：**

- **T - Try First. 先試試看。** 在求助前，先真誠地努力幾分鐘。
- **H - Hypothesize. 提出假設。** 寫下你「認為」正在發生什麼，就算你大概是錯的。
- **I - Identify Constraints. 辨識限制。** 這裡哪些能改，哪些不能改？
- **N - Need a Second Opinion. 需要第二意見。** 這時再找 AI。
- **K - Know Why. 知其所以然。** 在你能不靠 AI 解釋清楚之前，不要接受答案。

目標不是避開 AI。目標是確保在 AI 交給你一個心智模型之前，你自己已經有一個心智模型。

這個差異會反映在提示詞本身。

不要寫：

> 修這個 bug——這個欄位回傳 `null` 。

而是改成：

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3970593978&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786519778&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoYQ3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTA4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEwOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEwOCJdXSwwXQ..&amp;dt=1786501443635&amp;bpp=1&amp;bdt=1492&amp;idt=1&amp;shv=r20260811&amp;mjsv=m202608060101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786501442%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=10&amp;correlator=769561590536&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6745&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=3215&amp;eid=95396139%2C95397129%2C95398731%2C95399118%2C122880127%2C122880160%2C122880322&amp;oid=2&amp;psts=AOrYGsngZOTai1Majly1hb4xgiCUz2X7w2VhrH8qiqYVHiq_i5QDVCROQQf84aPcv_R002_nKFiL39hp44Fm22AzmFIjU47G4hjJLY_MhWyk-EA5fN7GgnkEuvqmHDRv2qj-tMc%2CAOrYGskRXY79gJCI-nvMTtXgXe7No73Du2yKsRLZC_68-sBk5LbCR8PcBLLker039Mplum3Z5JfND9bbpbQ6bPu8rKlODxZKUTDfF8fMZPAistHPkA%2CAOrYGsndIf5b3zN2xoVctftDwhMm86Nh6jz6SJq2GppY4U1oQpFL8VuJHBkgA4z-vUJBhoOOayxH7kYYeYKLTTxc3rm6ZwdW-Fu5zxSmyr6igJj4ESg%2CAOrYGskAF3m7b_jJ-t5E89XvpYzZT8Jz0CQir_g67EhNeAzMEFDA11wkpUcScJzdTYjdSMBlYXDrPvClBis4JFlE_Gm7SYtClXf4UEsFpa4EIaIn6Q%2CAOrYGskqxfeUn92EXKSqio_WfmbhaW4MBvR9C9upZrtFvutwEc8FABLqRBY349OEx4hxTP-795xEqOLHIeGdJSsJ7n_eM91Ubk9IePV4ucw3OsorKXw%2CAOrYGsklLQWbIqWlk21Orhc5SYccHyid_4ib8mVPpyzZwgnztj8idvXIqaLXOurwZi0hzkisqQXPJp7t5fXUMZ4i7iBsuelV24rdnlDmKDubUXlo-w%2CAOrYGsnAqnhtqMmUR_lB0oyskIAOwdfQiEZGxlbIW0bpo4kBw-fbfQUjXUyWqiUTiacT02swgj5f-bQIvowhLieqrfZgLNHXj1yeDJSWRzEaPtwCsw&amp;pvsid=6093479015012457&amp;tmod=628454059&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

> 我猜這個欄位會是 `null` ，是因為它在依賴的值還沒 resolve 之前就被讀取了，可能是呼叫鏈裡少了一個 `await` 。這是我的推理，我漏了什麼？

同樣的 bug。提示詞另一端的開發者，卻完全不同。

---

## 被動使用 AI vs. 會思考的 AI

| 被動使用 AI | 會思考的 AI |
| --- | --- |
| 「幫我做這個。」 | 「這是我的做法，請挑戰它。」 |
| 「修這個 bug。」 | 「這是我的假設，我漏了什麼？」 |
| 「寫出架構。」 | 「比較這些做法及其取捨。」 |
| 「解釋這段程式碼。」 | 「我先解釋，請告訴我哪裡漏了。」 |
| 「給我答案。」 | 「幫我評估這些選項。」 |

差別不在於你有沒有用 AI，而在於你把多少思考交給它。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3355444817&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786519783&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoYQ3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTA4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEwOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEwOCJdXSwwXQ..&amp;dt=1786501443641&amp;bpp=1&amp;bdt=1498&amp;idt=2&amp;shv=r20260811&amp;mjsv=m202608060101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786501442%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786501442%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=769561590536&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=7658&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=4029&amp;eid=95396139%2C95397129%2C95398731%2C95399118%2C122880127%2C122880160%2C122880322&amp;oid=2&amp;psts=AOrYGsngZOTai1Majly1hb4xgiCUz2X7w2VhrH8qiqYVHiq_i5QDVCROQQf84aPcv_R002_nKFiL39hp44Fm22AzmFIjU47G4hjJLY_MhWyk-EA5fN7GgnkEuvqmHDRv2qj-tMc%2CAOrYGskRXY79gJCI-nvMTtXgXe7No73Du2yKsRLZC_68-sBk5LbCR8PcBLLker039Mplum3Z5JfND9bbpbQ6bPu8rKlODxZKUTDfF8fMZPAistHPkA%2CAOrYGsndIf5b3zN2xoVctftDwhMm86Nh6jz6SJq2GppY4U1oQpFL8VuJHBkgA4z-vUJBhoOOayxH7kYYeYKLTTxc3rm6ZwdW-Fu5zxSmyr6igJj4ESg%2CAOrYGskAF3m7b_jJ-t5E89XvpYzZT8Jz0CQir_g67EhNeAzMEFDA11wkpUcScJzdTYjdSMBlYXDrPvClBis4JFlE_Gm7SYtClXf4UEsFpa4EIaIn6Q%2CAOrYGskqxfeUn92EXKSqio_WfmbhaW4MBvR9C9upZrtFvutwEc8FABLqRBY349OEx4hxTP-795xEqOLHIeGdJSsJ7n_eM91Ubk9IePV4ucw3OsorKXw%2CAOrYGsklLQWbIqWlk21Orhc5SYccHyid_4ib8mVPpyzZwgnztj8idvXIqaLXOurwZi0hzkisqQXPJp7t5fXUMZ4i7iBsuelV24rdnlDmKDubUXlo-w%2CAOrYGsnAqnhtqMmUR_lB0oyskIAOwdfQiEZGxlbIW0bpo4kBw-fbfQUjXUyWqiUTiacT02swgj5f-bQIvowhLieqrfZgLNHXj1yeDJSWRzEaPtwCsw%2CAOrYGsm6ZdrU8eJry1yVONHMGYE9K7T_y1WJLofieZXtEVR2ea0xJ87biBc2JjkDYugQgejUG9qg3ej6NMdnHaTylnrzoroZlb36GCriyNQ9U1AdwA8&amp;pvsid=6093479015012457&amp;tmod=628454059&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

差別在於： **到底是誰在思考。**

---

## 最好的 AI 使用者，不一定是最會下提示的人

很多 AI 討論都在講如何學會更好的 prompt engineering。我認為我們有時候把目標瞄錯了。

最好的 AI 使用者，不一定是那些最懂巧妙提示詞結構的人；而是那些知道哪些問題值得先問的人。

這不是提示詞技巧。這是把領域知識偽裝成提示詞。

領域知識 → 更好的問題 → 更好的 AI 輸出。

沒有領域知識時，流程就會反過來：AI 輸出 → 看起來很厲害 → 在沒有查證的情況下被接受，因為你根本沒有足夠的知識去查證它。

---

## 停止思考的開發者，會變成什麼樣

這不是那種危言聳聽的「AI 會取代你」段落。它更慢，也更安靜。

**階段 1** - AI 幫你更快完成工作。真的很棒。

**階段 2** - 你開始預設先問 AI，甚至還沒嘗試自己整理問題。

**階段 3** - 你可能開始不再探索替代方案。反正第一個答案已經能用了。

**階段 4** - 你開始依賴 AI 產生的解法替你思考，而不是拿它來挑戰自己的思維。

**階段 5** - 你仍然可以產出很多程式碼，但你可能很難解釋它為什麼存在、為什麼是這種結構、或者當限制條件改變時什麼會壞掉。

那不是 AI 技能問題。

那是依賴問題。

---

## 我不打算退回去

我要先講清楚，因為很容易把以上內容理解成反 AI。其實不是。

我不會退回去手動寫所有東西。AI 對此太有用了，假裝不是這樣只是在做表面功夫。

我現在還是會一直用它處理樣板程式、除錯、測試、文件、重構、腦力激盪、產生替代實作，以及探索我從沒碰過的 API。

但我改了一件事：

我不希望 AI 成為第一個開始思考我問題的東西。

我希望先由我來。

---

## 也許未來不是 prompt engineering

我認為產業對這件事的討論方向有點偏了。

真正有價值的技能組合不是：

```
AI + 100 個巧妙提示詞
```

而是：

```
領域知識 + 批判性思考 + 問題拆解 + AI + 判斷力
```

AI 讓產生大量可能解法變得更便宜、更快速。也正因如此，知道該選哪個解法、以及為什麼要選它，才會更有價值。

當產生答案變便宜時，知道該信任哪個答案就更有價值。

當程式碼生成變得越便宜，技術判斷力就越有價值。

---

## 你最該帶走的重點

不要跟 AI 比誰能更快產生原始程式碼。那不是你該贏的賽道。

你要跟自己比，看看誰能問出更好的問題。

讓 AI 寫無聊的程式碼。讓它產生替代方案。讓它找出邊界情況。讓它挑戰你的假設。

但不要把真正讓你成為開發者的那一部分外包出去：

**判斷力。**

---

## 所以，你是真的解決了問題，還是只是核准了它？

最近我一直在想這個問題：

當 AI 幫你解決了一個問題時，那真的是你解決的嗎？

還是你只是核准了那個解法？

也許最大的 AI 技能，根本不是 prompt engineering。

也許是知道什麼時候還不該 prompt。

你沒有 AI 問題。

你有思考問題。

而我也還在學著怎麼解決我自己的問題。

---

### 來聊聊

當你卡在程式問題上時，你的第一步通常是什麼？

- **A)** 思考並除錯
- **B)** 查文件 / Google
- **C)** 問 AI
- **D)** 視情況而定

如果 AI 通常是你的第一步，它有沒有曾讓你發現自己對問題的理解，比你以為的還少？

我真的很好奇。

---

我寫 AI、軟體開發，以及新工具如何改變開發者思考與工作的方式。

如果你也在想同樣的問題，歡迎在 DEV 上追蹤我。我比起預測 AI 是否會取代開發者，更有興趣的是弄清楚開發者接下來需要成為什麼樣的人。

---

原文出處： [https://dev.to/harsh2644/you-dont-have-an-ai-problem-you-have-a-thinking-problem-5f07](https://dev.to/harsh2644/you-dont-have-an-ai-problem-you-have-a-thinking-problem-5f07)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/apoYQ3) [6瀏覽](https://codelove.tw/@tony/post/apoYQ3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/apoYQ3)

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

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝8 💬4 ❤️2

235

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 💬2 ❤️1

51

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1786501442&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FapoYQ3&amp;pra=5&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTA4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEwOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEwOCJdXSwwXQ..&amp;dt=1786501442446&amp;bpp=21&amp;bdt=57&amp;idt=31&amp;shv=r20260811&amp;mjsv=m202608060101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786495103%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786495103%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786495103%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=769561590536&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C95397129%2C95398731%2C95399118%2C122880127%2C122880160%2C122880322&amp;oid=2&amp;pvsid=6093479015012457&amp;tmod=628454059&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=60" title="Advertisement" aria-label="Advertisement"></iframe>