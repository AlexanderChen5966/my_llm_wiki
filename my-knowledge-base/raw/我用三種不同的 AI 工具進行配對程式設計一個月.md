---
title: "我用三種不同的 AI 工具進行配對程式設計一個月"
source: "https://codelove.tw/@tony/post/3DRNdq"
author:
published:
created: 2026-09-07
description: "AI 程式碼工具可以在幾秒內寫出一個函式。更難的問題是，那個函式是否真的該存在於你的程式碼庫中。它有遵循既有架構嗎？有處理邊界情況嗎？測試還會通過嗎？而當三個檔案之後某處出錯時，AI 能幫忙找出真正原因，而不是只再產生一個補丁嗎？為了回答這些問題，我花了一個月把 Cursor、GitHub Co..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月04日](https://codelove.tw/@tony/post/3DRNdq "2026-09-04 12:02:40")

## 我用三種不同的 AI 工具進行配對程式設計一個月

AI 程式碼工具可以在幾秒內寫出一個函式。更難的問題是，那個函式是否真的該存在於你的程式碼庫中。它有遵循既有架構嗎？有處理邊界情況嗎？測試還會通過嗎？而當三個檔案之後某處出錯時，AI 能幫忙找出真正原因，而不是只再產生一個補丁嗎？

機器學習與人工智慧

為了回答這些問題，我花了一個月把 Cursor、GitHub Copilot 和 Claude Code 當作結對程式設計工具來使用，實際處理各種開發任務：寫程式、除錯、重構函式、建立測試，以及跨多個檔案進行修改。

我不是在測試哪個工具能產生最多程式碼。我是在測試哪一個能讓真正的程式開發更快，同時不會在事後製造更多工作。

## 簡短結論

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788746137&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRNdq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788746137336&amp;bpp=1&amp;bdt=1144&amp;idt=-M&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250&amp;nras=2&amp;correlator=3080793656665&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=951&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslpihg6BakdgSjuZ5_wKsybWoCGL4SFM_ZEmL4cw_FUIlxRulMxV4O7TndtPXAJhuHSkh5x-bBPG_QaXImP_G7AT4HbkD3GUq917TgaeNFQfRU&amp;pvsid=4432001592891923&amp;tmod=1931121027&amp;uas=3&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=16" title="Advertisement" aria-label="Advertisement"></iframe>

在實際開發任務中使用這三個工具後，我不會說有哪一個是絕對冠軍。

它們各自在不同的程式設計環節表現較好：

- Cursor 在互動式寫程式，以及 AI 導向編輯器中的多檔案修改方面最強。
- GitHub Copilot 在日常寫程式、自動完成、樣板程式，以及較小的函式方面最方便。
- Claude Code 在任務需要理解更大的程式碼庫、跨檔案除錯，或透過終端機完成多步驟作業時最強。

最大的差異不在於它們產生程式碼的速度，而在於它們在產生程式碼之前，能利用多少有用的上下文。

這成了整個測試中最重要的心得。

## 我實際測試了什麼

我想避免常見的 AI 寫程式比較方式，也就是每個工具都給同一個簡單提示：

機器學習與人工智慧

> 「做一個待辦事項應用程式。」

這樣並不能告訴你太多關於真實開發的事。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788746137&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRNdq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788746137336&amp;bpp=1&amp;bdt=1144&amp;idt=-M&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280&amp;nras=3&amp;correlator=3080793656665&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1776&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslpihg6BakdgSjuZ5_wKsybWoCGL4SFM_ZEmL4cw_FUIlxRulMxV4O7TndtPXAJhuHSkh5x-bBPG_QaXImP_G7AT4HbkD3GUq917TgaeNFQfRU&amp;pvsid=4432001592891923&amp;tmod=1931121027&amp;uas=3&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=16" title="Advertisement" aria-label="Advertisement"></iframe>

因此，我使用的是在既有專案中很像日常工作的任務。

### 任務 1：新增一個函式

我從既有程式碼開始，要求每個工具實作一個缺少的函式。

例如：

```javascript
async function getUserById(id) {
  // implementation needed
}
```

需求很直接：取得使用者、處理失敗的回應、驗證回傳資料，並回傳可預期的結果。

這測試了一件基礎但重要的事：

> AI 能不能遵循既有專案的程式風格，而不是自己發明一套？

三個工具都能產生可運作的起始版本。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788746137&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRNdq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788746137336&amp;bpp=1&amp;bdt=1144&amp;idt=-M&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280&amp;nras=4&amp;correlator=3080793656665&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2592&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslpihg6BakdgSjuZ5_wKsybWoCGL4SFM_ZEmL4cw_FUIlxRulMxV4O7TndtPXAJhuHSkh5x-bBPG_QaXImP_G7AT4HbkD3GUq917TgaeNFQfRU&amp;pvsid=4432001592891923&amp;tmod=1931121027&amp;uas=3&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=17" title="Advertisement" aria-label="Advertisement"></iframe>

差異出現在整理收尾的階段。

Copilot 很擅長快速產生第一版實作。Cursor 讓我更容易參考相關檔案，並把函式調整成符合周遭專案的樣子。當我想先檢查其他地方類似函式是怎麼實作的，再做修改時，Claude Code 特別有用。

這種差異在既有應用程式中特別重要。

軟體

從零開始寫程式很容易。

寫出能融入既有程式碼庫的程式比較難。

## 除錯是更好的測試

程式碼生成並不是我看到最大差異的地方。

除錯才是。

我提供的是實際錯誤，而不是要它們憑空發明解法。

典型任務會像這樣：

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788746137&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRNdq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788746137336&amp;bpp=1&amp;bdt=1144&amp;idt=-M&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=5&amp;correlator=3080793656665&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3433&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslpihg6BakdgSjuZ5_wKsybWoCGL4SFM_ZEmL4cw_FUIlxRulMxV4O7TndtPXAJhuHSkh5x-bBPG_QaXImP_G7AT4HbkD3GUq917TgaeNFQfRU&amp;pvsid=4432001592891923&amp;tmod=1931121027&amp;uas=3&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=5&amp;fsb=1&amp;dtd=17" title="Advertisement" aria-label="Advertisement"></iframe>

`TypeError: Cannot read properties of undefined at UserList.jsx:42`

我不會直接問：

> 「修正這個錯誤。」

而是提供相關元件、API 函式與資料結構，並要求工具找出根本原因。

這樣得到的結果實用得多。

### GitHub Copilot

如果問題離我目前正在編輯的程式碼很近，Copilot 表現不錯。

程式設計

如果錯誤是因為少了 null 檢查，或明顯用了錯誤變數，它可以很快建議修正方式。

但限制在於，當原因在別處時就不一定夠力。

我有時必須手動補充更多檔案和上下文。

### Cursor

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788746142&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRNdq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788746137315&amp;bpp=1&amp;bdt=1123&amp;idt=0&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C1634x905%2C1005x124&amp;nras=8&amp;correlator=3080793656665&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4308&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=875&amp;eid=95399629%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslpihg6BakdgSjuZ5_wKsybWoCGL4SFM_ZEmL4cw_FUIlxRulMxV4O7TndtPXAJhuHSkh5x-bBPG_QaXImP_G7AT4HbkD3GUq917TgaeNFQfRU%2CAOrYGskDSWVYjJ-IMGse3krg9bexQClB12U_YlWzfVV4SAEgPpGBcMMlhZ6yyReM82Ee4SIW5-y-f96Mybs6W1E8IlTy7FNA7-rXEFCyODwcmJXEQgyMH3IGQ1lVtWQNRarB4HBC%2CAOrYGslkcckfxdk__RwdfRq9HX2KWgn_ql0fA_QlnnTbdYRGNP2gojUoyjOXkDjConaDCZteiNA-a3sM_t_WPd-6MA11vgvuOd_ad83tpCeWJHg%2CAOrYGsnFwv5pR7sJUF2SrWeu5nmhr4nRp7uacqunFn3YgaCQHKXY8KhljKothG05MEC_9cq1mo3mr0nxN2o03pdZdZ0zTIkrzVLdAt9nMsmPBsg%2CAOrYGsnTCFchaY_Wts7j8IzH05NStRvS9z4UWmrUD1LXv24hHoZdUBw1BXp_DOSwVeHJFA73Q4S4weM3d-gPQLfR4E5yDitXMCytTVK2yfUXBsg8%2CAOrYGsnBX1o7kj3qQl68i8mZi0pQ6UVYnGHGD8xsrhG-VMeu-PsF_PipWRyaOPH5OPaR5xjRzm0CBpi01XRA1awS1KwrvhelvTOvuPDEtaxb750&amp;pvsid=4432001592891923&amp;tmod=1931121027&amp;uas=3&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=7&amp;fsb=1&amp;dtd=4868" title="Advertisement" aria-label="Advertisement"></iframe>

如果相關程式碼已經在專案裡，Cursor 處理這類情況更好。

我可以請它檢查元件、API 呼叫以及相關型別，並解釋資料結構在哪裡開始和預期不符。

這讓除錯不再只是像自動完成，而更像多了一雙眼睛。

### Claude Code

當除錯任務跨越好幾個檔案時，Claude Code 特別有用。

我可以不只聚焦在丟出錯誤的那一行，而是請它追蹤資料流。

這很有價值，因為很多真實錯誤並不發生在應用程式崩潰的地方。

崩潰通常只是最後的症狀。

## 重構：AI 能省時間，也能製造問題

重構也是一個很有用的測試。

我拿已經能運作、但變得難以維護的程式碼，要求每個工具在不改變行為的情況下改善它。

程式設計

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=4060917935&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788753548&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRNdq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788746137316&amp;bpp=1&amp;bdt=1183&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C1634x905%2C1005x124%2C952x280&amp;nras=9&amp;correlator=3080793656665&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5161&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1550&amp;eid=95399629%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslpihg6BakdgSjuZ5_wKsybWoCGL4SFM_ZEmL4cw_FUIlxRulMxV4O7TndtPXAJhuHSkh5x-bBPG_QaXImP_G7AT4HbkD3GUq917TgaeNFQfRU%2CAOrYGskDSWVYjJ-IMGse3krg9bexQClB12U_YlWzfVV4SAEgPpGBcMMlhZ6yyReM82Ee4SIW5-y-f96Mybs6W1E8IlTy7FNA7-rXEFCyODwcmJXEQgyMH3IGQ1lVtWQNRarB4HBC%2CAOrYGslkcckfxdk__RwdfRq9HX2KWgn_ql0fA_QlnnTbdYRGNP2gojUoyjOXkDjConaDCZteiNA-a3sM_t_WPd-6MA11vgvuOd_ad83tpCeWJHg%2CAOrYGsnFwv5pR7sJUF2SrWeu5nmhr4nRp7uacqunFn3YgaCQHKXY8KhljKothG05MEC_9cq1mo3mr0nxN2o03pdZdZ0zTIkrzVLdAt9nMsmPBsg%2CAOrYGsnTCFchaY_Wts7j8IzH05NStRvS9z4UWmrUD1LXv24hHoZdUBw1BXp_DOSwVeHJFA73Q4S4weM3d-gPQLfR4E5yDitXMCytTVK2yfUXBsg8%2CAOrYGsnBX1o7kj3qQl68i8mZi0pQ6UVYnGHGD8xsrhG-VMeu-PsF_PipWRyaOPH5OPaR5xjRzm0CBpi01XRA1awS1KwrvhelvTOvuPDEtaxb750%2CAOrYGsklgLU1HWg7FAYCZeJIvTrJZ67pZaS04TJo5Ge4r87TkwwKMyiPAGlePmMoDNuR6DFohVNS-CHTctasRzbYHKfEEvarcqsqwwqErK5GZh199A&amp;pvsid=4432001592891923&amp;tmod=1931121027&amp;uas=1&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1960%2C37%2C1960%2C37%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

例如：

```javascript
function calculateTotal(items) {
  let total = 0;

  for (let i = 0; i < items.length; i++) {
    if (items[i].active) {
      total += items[i].price * items[i].quantity;
    }
  }

  return total;
}
```

簡單的重構很容易。

但真正的重構通常伴隨限制：

- 不要改變 API。
- 保留既有行為。
- 保持目前的資料結構。
- 不要引入另一個相依套件。
- 維持測試覆蓋率。
- 遵循專案既有慣例。

這就是工具開始表現出差異的地方。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1106437894&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788753550&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRNdq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788746137318&amp;bpp=1&amp;bdt=1186&amp;idt=0&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788753548%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788753548%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788753548%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C1634x905%2C1005x124%2C952x280%2C952x280&amp;nras=10&amp;correlator=3080793656665&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6206&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2635&amp;eid=95399629%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslpihg6BakdgSjuZ5_wKsybWoCGL4SFM_ZEmL4cw_FUIlxRulMxV4O7TndtPXAJhuHSkh5x-bBPG_QaXImP_G7AT4HbkD3GUq917TgaeNFQfRU%2CAOrYGskDSWVYjJ-IMGse3krg9bexQClB12U_YlWzfVV4SAEgPpGBcMMlhZ6yyReM82Ee4SIW5-y-f96Mybs6W1E8IlTy7FNA7-rXEFCyODwcmJXEQgyMH3IGQ1lVtWQNRarB4HBC%2CAOrYGslkcckfxdk__RwdfRq9HX2KWgn_ql0fA_QlnnTbdYRGNP2gojUoyjOXkDjConaDCZteiNA-a3sM_t_WPd-6MA11vgvuOd_ad83tpCeWJHg%2CAOrYGsnFwv5pR7sJUF2SrWeu5nmhr4nRp7uacqunFn3YgaCQHKXY8KhljKothG05MEC_9cq1mo3mr0nxN2o03pdZdZ0zTIkrzVLdAt9nMsmPBsg%2CAOrYGsnTCFchaY_Wts7j8IzH05NStRvS9z4UWmrUD1LXv24hHoZdUBw1BXp_DOSwVeHJFA73Q4S4weM3d-gPQLfR4E5yDitXMCytTVK2yfUXBsg8%2CAOrYGsnBX1o7kj3qQl68i8mZi0pQ6UVYnGHGD8xsrhG-VMeu-PsF_PipWRyaOPH5OPaR5xjRzm0CBpi01XRA1awS1KwrvhelvTOvuPDEtaxb750%2CAOrYGsklgLU1HWg7FAYCZeJIvTrJZ67pZaS04TJo5Ge4r87TkwwKMyiPAGlePmMoDNuR6DFohVNS-CHTctasRzbYHKfEEvarcqsqwwqErK5GZh199A%2CAOrYGslDDje0j5DnuGEB4uNt-WYAw3rWZ2shRUmVryy8TpAyHZjVEhcl8upxOZAFC_QphwBhgv0O68btyf1wXIy-dNXwafX616NL_exDSwNzhmL8UfE&amp;pvsid=4432001592891923&amp;tmod=1931121027&amp;uas=1&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=1960%2C37%2C1960%2C37%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Copilot 在小型重建置議方面很出色。

當我想在檢視受影響檔案的同時進行更大範圍修改時，Cursor 表現更好。

如果重構需要理解這個函式在整個儲存庫中是如何被使用的，Claude Code 就很有幫助。

### 重點：一定要檢視 diff

這後來成了我的一條規則。

> 永遠不要在不讀 diff 的情況下，直接接受大量 AI 生成的重構。
> 
> 機器學習與人工智慧

看起來更整潔的實作，不代表更安全。

AI 可能移除了重複程式碼，卻不小心改變了行為。

它也可能把某些原本故意這樣寫的東西「改善」掉，而那其實是因為應用程式的另一部分需要如此。

## 用 AI 撰寫測試

測試是三個工具都幫我省下時間的一個面向。

我可以提供現有函式，請它撰寫涵蓋以下情況的單元測試：

- 正常輸入
- 空輸入
- 無效輸入
- 缺少值
- API 失敗
- 邊界條件

第一批生成的測試通常還算合理。

但有個明顯問題。

AI 傾向根據它看到的實作來寫測試。

這會導致測試只是在確認目前程式碼的行為，而不是證明應用程式應該有什麼行為。

例如，如果實作裡有一個錯誤的預設值，AI 產生的測試可能會把這個行為寫死。

所以我後來不再問：

> 「幫我寫這個函式的測試。」

而是改成：

> 「根據下方描述的預期行為撰寫測試。包含邊界情況與失敗情境。不要假設目前實作是正確的。」

這個小改動讓測試實用得多。

## 多檔案修改改變了我的看法

當我不再只要求它們寫單一函式時，這些工具之間的最大差異就變得很明顯。

我給它們一個功能需求。

例如：

> 為現有的使用者列表加入分頁。保留目前的 API 回應格式，加入載入與錯誤狀態，更新 API 請求，保留既有篩選條件，並為新行為新增測試。

這時 AI 需要理解：

機器學習與人工智慧

1. API 請求發生在哪裡。
2. 使用者列表在哪裡渲染。
3. 狀態目前如何管理。
4. 篩選條件如何運作。
5. 測試放在哪裡。
6. 哪些檔案需要修改。
7. 既有 API 是否支援要求的行為。

這就更接近真實的軟體開發了。

### Cursor

當我想留在編輯器內，並以互動方式引導修改時，Cursor 表現很好。

我可以檢視它建議的變更，並在過程中調整實作。

### GitHub Copilot

Copilot 仍然有用，但在較大的修改上，我發現自己需要提供更多方向。

當我已經知道要做什麼，並想要有人協助把它實作出來時，它很出色。

### Claude Code

當任務需要先進行儲存庫層級的調查，再開始實作時，Claude Code 特別有幫助。

這讓它在較大的變更中很有價值，因為第一步不是寫程式，而是先找出該改哪裡。

## 哪個工具需要最少修正？

這比起生成了多少行程式碼更難衡量。

我開始注意一個更實際的指標：

AI 做完之後，我還要花多少工作？

機器學習與人工智慧

這包括：

- 修正錯誤假設
- 移除不必要的程式碼
- 更正 API
- 更改變數名稱
- 補上缺少的錯誤處理
- 重寫測試
- 修復回歸問題
- 還原不必要的修改

這改變了我對生產力的看法。

一個工具一分鐘產生 200 行，不一定比另一個工具產生 80 行有用程式碼還快，如果我得再花 30 分鐘修第一個結果的話。

對我而言，有用的程式碼比產生出來的程式碼更重要。

## 我的實際比較

| 程式設計任務 | 最佳選擇 | 原因 |
| --- | --- | --- |
| 行內自動完成 | GitHub Copilot | 輸入時建議速度快 |
| 小型函式 | GitHub Copilot | 低摩擦、快速生成 |
| 互動式重構 | Cursor | 編輯器導向工作流程強 |
| 多檔案編輯 | Cursor | 較容易引導與檢視變更 |
| 簡單錯誤除錯 | GitHub Copilot | 快速的上下文建議 |
| 跨檔案除錯 | Claude Code | 更適合儲存庫層級調查 |
| 理解陌生的儲存庫 | Claude Code | 適合追蹤專案結構 |
| 撰寫單元測試 | 三者皆可 | 有不錯的起點，但仍需人工審查 |
| 大型實作任務 | Cursor / Claude Code | 更適合多步驟工作 |
| 最終程式碼審查 | 人類開發者 | AI 不該是最後裁決者 |

## AI 結對程式設計實際改變了什麼

最大的生產力提升，不是我停止寫程式了。

軟體

而是我用不同方式寫程式。

在大量使用 AI 之前，很多時間都花在：

- 查文件
- 查語法
- 寫重複程式碼
- 建立測試樣板
- 追蹤陌生函式
- 建立解法的第一版

AI 減少了很多這類摩擦。

但另一類工作變得更重要：

- 檢視生成的程式碼
- 驗證假設
- 測試邊界情況
- 閱讀 diff
- 撰寫更好的提示
- 把大任務拆成更小的需求

所以 AI 並沒有移除工程工作。

它只是改變了我花時間的地方。

## 我必須注意的錯誤

一個月後，我對幾個反覆出現的問題變得更小心。

### 1\. AI 會自行腦補

如果需求不清楚，工具就會自己補空白。

這可能代表它會選一個不適合專案的 API 模式、函式庫、命名慣例或架構。

### 2\. 能運作的程式碼也可能是差的程式碼

某些東西可以編譯、通過基本測試，但仍然過於複雜。

### 3\. 測試可能帶來虛假的安全感

生成出來的測試套件不代表就有良好的覆蓋率。

### 4\. 大型變更需要更小的檢查點

當我把大任務拆成階段，而不是一次要求完整功能時，結果通常更好。

### 5\. Git 變得更重要

隨著 AI 做更多修改，檢視 commit 與 diff 變得不可或缺。

我想知道到底改了什麼，以及為什麼改。

## 對我最有效的結對程式設計流程

最可靠的工作流程其實很簡單。

### 步驟 1：說明既有程式碼

提供相關檔案，並要求 AI 先解釋目前行為，再做任何修改。

### 步驟 2：定義需求

明確說出哪些要改、哪些必須保持不變。

### 步驟 3：先要一個計畫

對較大的任務，先讓 AI 指出需要修改哪些檔案，再開始寫程式。

### 步驟 4：分小步實作

不要不加判斷地接受一大段變更。

### 步驟 5：檢視 diff

確認每一個有意義的修改。

### 步驟 6：執行測試

不要因為生成的程式碼看起來正確，就把它當成完成品。

### 步驟 7：請 AI 挑戰自己的解法

一個很有用的提示是：

> 「請檢查這個實作是否有邊界情況、回歸問題、不必要的複雜度，以及可能錯誤的假設。」

這常常能找出我沒注意到的問題。

## 那麼，我會選哪一個 AI 結對程式設計工具？

如果我今天要開始一個專案，我不會只根據基準測試分數或功能列表來選。

我會根據我的工作流程來選。

- **日常寫程式與自動完成：** GitHub Copilot。
- **以編輯器為中心、需要互動式 AI 協助的工作流程：** Cursor。
- **儲存庫層級除錯、調查，以及較大的終端機型任務：** Claude Code。

但有一個重要前提。

我不會讓它們任何一個變成最後的決策者。

AI 可以建議實作。

但我仍然要決定這個實作是否正確。

這就是把 AI 當作結對程式設計夥伴，和把 AI 當作程式碼產生器之間的差別。

## 結論

在一個月內使用三種不同的 AI 工具進行結對程式設計後，我得到了一個沒那麼刺激、但更實用的答案：AI 不會讓程式設計消失；它只是讓程式設計中的某些部分變得快得多。

當我需要在寫程式時快速獲得協助，GitHub Copilot 表現很好。當工作涉及互動式編輯與多檔案處理時，Cursor 更有用。當我需要調查儲存庫、追蹤問題，或透過終端機處理較大任務時，Claude Code 特別突出。

真正的生產力提升，來自 AI 生成與一般工程紀律的結合。

我還是會讀程式碼。

我還是會檢查 diff。

我還是會執行測試。

我還是會除錯。

我還是會做架構決策。

這才是今天看待 AI 結對程式設計最實際的方式。目標不是讓 AI 在你坐著不動時替你寫完整個應用程式。目標是移除重複性工作，縮短從想法到可運作實作之間的距離，並提供另一個思考困難程式設計問題的工具。

最好的 AI 結對程式設計工具，不是寫最多程式碼的那個，而是能幫你花更多時間解決工程問題、花更少時間對抗重複實作工作的那個。

---

原文出處： [https://dev.to/elsie-rainee/i-tried-pair-programming-with-three-different-ai-tools-for-a-month-2nnc](https://dev.to/elsie-rainee/i-tried-pair-programming-with-three-different-ai-tools-for-a-month-2nnc)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3DRNdq) [9瀏覽](https://codelove.tw/@tony/post/3DRNdq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3DRNdq)

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

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝20 💬4

295

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788746136&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DRNdq&amp;pra=5&amp;asro=0&amp;itsi=43&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788746136274&amp;bpp=2&amp;bdt=82&amp;idt=21&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3080793656665&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400774%2C95401045%2C122880828&amp;oid=2&amp;pvsid=4432001592891923&amp;tmod=1931121027&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=27" title="Advertisement" aria-label="Advertisement"></iframe>