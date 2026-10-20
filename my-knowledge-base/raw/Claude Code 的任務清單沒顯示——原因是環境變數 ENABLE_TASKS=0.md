---
title: "Claude Code 的任務清單沒顯示——原因是環境變數 ENABLE_TASKS=0"
source: "https://codelove.tw/@tony/post/amZKgq"
author:
published:
created: 2026-08-20
description: "複數步驟的工作一旦交給 Claude Code，它就會用**檢查清單**來顯示進度。但我這邊的環境裡，`TodoWrite` 這個功能**整個不存在**，而且是按工具別直接消失。2.1.233 的 changelog 裡寫著這段：> Todo/task-tracking tools (T..."
tags:
  - "clippings"
status: "processed"
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月20日](https://codelove.tw/@tony/post/amZKgq "2026-08-20 08:30:10")

## Claude Code 的任務清單沒顯示——原因是環境變數 ENABLE\_TASKS=0

複數步驟的工作一旦交給 Claude Code，它就會用 **檢查清單** 來顯示進度。

但我這邊的環境裡， `TodoWrite` 這個功能 **整個不存在** ，而且是按工具別直接消失。

2.1.233 的 changelog 裡寫著這段：

> Todo/task-tracking tools (TaskCreate/Get/Update/List, TodoWrite) are no longer available on Opus 4.8, Sonnet 5, Fable 5, Mythos 5, and newer models; set `CLAUDE_CODE_ENABLE_TODO_TOOLS=1` to bring them back  
> （Todo／任務追蹤工具（TaskCreate/Get/Update/List、TodoWrite）在 Opus 4.8、Sonnet 5、Fable 5、Mythos 5 及更新的模型上不再提供；設定 `CLAUDE_CODE_ENABLE_TODO_TOOLS=1` 即可把它們找回來）

**看起來像是「因為是新模型」所以就結束了，但實際上我改變條件測過之後，結果不一樣。**

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787187806&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FamZKgq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187566748&amp;bpp=4&amp;bdt=999&amp;idt=4&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787187568%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905&amp;nras=3&amp;correlator=3517120581920&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1103&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397678%2C122880370&amp;oid=2&amp;pvsid=734425893021693&amp;tmod=500019996&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**同樣是 Opus 5，只要環境變數不同，還是能用。** 被關掉的是 `CLAUDE_CODE_ENABLE_TASKS=0` 。

---

## 先看結果

這是在 **`claude -p` 的非互動模式** 下，改變條件後，詢問「目前可用工具中，列出所有以 Todo / Task 開頭的工具」得到的結果。

| 模型 | ENABLE\_TASKS | ENABLE\_TODO\_TOOLS | 可用工具 |
| --- | --- | --- | --- |
| Opus 5 | 未設定 | 未設定 | `TodoWrite` |
| Haiku 4.5 | 未設定 | 未設定 | `TaskCreate / Get / List / Update` |
| Opus 5 | 0 | 未設定 | なし |
| Opus 5 | 未設定 | 1 | `TaskCreate / Get / List / Update` |

**第 3 行跟我自己的環境條件一樣。** 當時就是 `CLAUDE_CODE_ENABLE_TASKS=0` 。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3764087313&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787187806&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FamZKgq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187566756&amp;bpp=1&amp;bdt=1007&amp;idt=1&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787187568%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280&amp;nras=4&amp;correlator=3517120581920&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1962&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397678%2C122880370&amp;oid=2&amp;pvsid=734425893021693&amp;tmod=500019996&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

> 檢證環境：Windows 10 / Claude Code 2.1.234 / `--model opus` 會解析成 `claude-opus-5`

---

## 兇手是 ENABLE\_TASKS=0

查看環境變數後，發現是這樣：

```
CLAUDE_CODE_ENABLE_TASKS      = 0
CLAUDE_CODE_ENABLE_TODO_TOOLS = (未設定)
```

**`settings.json` 裡沒有寫。** 這是從父行程傳進來的。

這個變數的意思，官方文件有寫。

> On other models, Claude Code provides the Task tools by default and `TodoWrite` only when you set `CLAUDE_CODE_ENABLE_TASKS=0`.  
> （在其他模型上，Claude Code 預設提供 Task 工具，只有在設定 `CLAUDE_CODE_ENABLE_TASKS=0` 時才會提供 `TodoWrite` ）

也就是說， `ENABLE_TASKS=0` 的意思是： **把 Task 工具關掉，改成用 TodoWrite。**

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787187806&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FamZKgq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187566761&amp;bpp=1&amp;bdt=1012&amp;idt=1&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787187568%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280&amp;nras=5&amp;correlator=3517120581920&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2866&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397678%2C122880370&amp;oid=2&amp;pvsid=734425893021693&amp;tmod=500019996&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

但問題是， **新模型預設就沒有 Task 工具。**  
這時再加上「把 Task 工具關掉」這個設定，等於是對一個本來就不存在的東西做切換，結果 **TodoWrite 也不會出現。**

**最後兩邊都消失。** 這就是實測中的第 3 行。

---

## 變數名稱與實際回來的東西不一樣

要把它打開的是 `CLAUDE_CODE_ENABLE_TODO_TOOLS=1` 。光看名字會以為是把 `TodoWrite` 帶回來，但實際上：

```
TaskCreate
TaskGet
TaskList
TaskOutput
TaskStop
TaskUpdate
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=186534417&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787187810&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FamZKgq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187566770&amp;bpp=1&amp;bdt=1021&amp;idt=1&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787187568%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=7&amp;correlator=3517120581920&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3679&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=129&amp;eid=95397678%2C122880370&amp;oid=2&amp;psts=AOrYGsmkm3m8IW3s9WT7DdyRSzVKi0vgKyspRkvO3pkYrcR13GbX4zpXvtPAxZl4FIRHGKbdFXsX4Kc5JnDM9ET17m7w9wyg2zGtS24QsKULblYT5xO144p8kgvaHysQ7xd9mIU%2CAOrYGsnXCT5yF7hVAqNQ7ZJPDVSz5_lqD1zlmnjcWuJxGJx_JrlUv4FbpJaVgJ_Xrbcz7Gidvk6Pm-T0_jgRwSgntTZRv3BZTmeF-xkcZ6-AQeZk0_M%2CAOrYGslLVTEJYaB89F2oCp4SANMycanN72TUl-EdUrpnihqOndBWAiC-4F4DFRBjJ8d5oNpV4-yGSWLNskr6BpdnqOSvBzfGHkyO7vlsd9vV2NLOrQ%2CAOrYGsndG8rL6LzshgbnzcS5rLUGiQyUc8a6fWLBBWQvyCV4eXLLBL0I3DcL-GZTnVO4scfwKwriwwDvDQM_nNYRH0qhN9_H8yeu1sNea0tCka3m0Q%2CAOrYGsljd0jsq_ZxmxW6WOZiaZjkyCZHCP8HyPvRZWXakJunnUaP83-Rkd5dyBZ5wBb0uuM_emgynS_Db_hYc9hZDRBXk48F-FkfSfwZ_H4JE7o-sI4&amp;pvsid=734425893021693&amp;tmod=500019996&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1957%2C30%2C1957%2C30%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**回來的是整套 Task 工具，沒有 `TodoWrite` 。**

官方文件也是這樣描述這個變數的。

> sets only `CLAUDE_CODE_ENABLE_TODO_TOOLS=1`, the opt-in for the models that otherwise don't get the tools  
> （只設定 `CLAUDE_CODE_ENABLE_TODO_TOOLS=1` ，這是給原本不會拿到這些工具的模型所用的 opt-in）

**變數名稱是 Todo，實際內容卻是 Task。** 不知道的話真的很容易搞混。

---

## Todo 和 Task 是不同的東西

兩者從設計上就不一樣。

| 項目 | `TodoWrite` | Task 工具 |
| --- | --- | --- |
| 更新單位 | 一次重寫整份清單 | 用 `TaskCreate` 新增 1 筆，用 `TaskUpdate` 更新 1 筆 |
| 讀取 | 無 | `TaskList` / `TaskGet` |
| 依賴關係 | 無 | 可用 `addBlocks` / `addBlockedBy` 指定 |

**Task 工具有依賴關係和負責人。** 不過這張表是根據 **官方文件的描述** 整理的，  
**我沒有實際建立依賴關係來測試。**

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1038984369&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787187839&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FamZKgq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187566779&amp;bpp=1&amp;bdt=1031&amp;idt=1&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787187568%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=8&amp;correlator=3517120581920&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4677&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1073&amp;eid=95397678%2C122880370&amp;oid=2&amp;psts=AOrYGsmkm3m8IW3s9WT7DdyRSzVKi0vgKyspRkvO3pkYrcR13GbX4zpXvtPAxZl4FIRHGKbdFXsX4Kc5JnDM9ET17m7w9wyg2zGtS24QsKULblYT5xO144p8kgvaHysQ7xd9mIU%2CAOrYGsnXCT5yF7hVAqNQ7ZJPDVSz5_lqD1zlmnjcWuJxGJx_JrlUv4FbpJaVgJ_Xrbcz7Gidvk6Pm-T0_jgRwSgntTZRv3BZTmeF-xkcZ6-AQeZk0_M%2CAOrYGslLVTEJYaB89F2oCp4SANMycanN72TUl-EdUrpnihqOndBWAiC-4F4DFRBjJ8d5oNpV4-yGSWLNskr6BpdnqOSvBzfGHkyO7vlsd9vV2NLOrQ%2CAOrYGsndG8rL6LzshgbnzcS5rLUGiQyUc8a6fWLBBWQvyCV4eXLLBL0I3DcL-GZTnVO4scfwKwriwwDvDQM_nNYRH0qhN9_H8yeu1sNea0tCka3m0Q%2CAOrYGsljd0jsq_ZxmxW6WOZiaZjkyCZHCP8HyPvRZWXakJunnUaP83-Rkd5dyBZ5wBb0uuM_emgynS_Db_hYc9hZDRBXk48F-FkfSfwZ_H4JE7o-sI4%2CAOrYGsnt0Ilnh4_SmZvWJjAD1QEKYUEVTWNJLKjV5dMrOLpXI2LzPPzp6fB8kqS9k1lVDaMDmbjxnhw0fx_hzW8Z_KN8kIGw1Ab0lY46-3dndrtF_WA&amp;pvsid=734425893021693&amp;tmod=500019996&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1957%2C30%2C1957%2C30%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**官方沒有寫明為什麼要停用。** 可以推測是朝「功能更完整的一側」整併了，但  
我沒找到明確這樣寫的文字。這只是推測。

---

## 不同模型的預設值也不同

就算不設定環境變數，結果也會因模型而異。

| 模型 | 預設提供的內容 |
| --- | --- |
| Opus 5 | `TodoWrite` |
| Haiku 4.5 | Task 工具一式 |

changelog 寫的是「Opus 4.8、Sonnet 5 以後不能用」，但  
**我直接對 Opus 5 詢問時，回傳的是 `TodoWrite`** （ `-p` 的結果）。

另外， **細節規格寫在 Agent SDK 的頁面** ，而且條件是依 SDK 版本描述的。  
**SDK 跟 CLI 的預設值不一定一致。** 到頭來， **自己在環境裡實測最快。**

---

## 該怎麼做

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1621299728&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787187851&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FamZKgq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187566784&amp;bpp=1&amp;bdt=1037&amp;idt=1&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787187568%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280&amp;nras=9&amp;correlator=3517120581920&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5660&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2058&amp;eid=95397678%2C122880370&amp;oid=2&amp;psts=AOrYGsmkm3m8IW3s9WT7DdyRSzVKi0vgKyspRkvO3pkYrcR13GbX4zpXvtPAxZl4FIRHGKbdFXsX4Kc5JnDM9ET17m7w9wyg2zGtS24QsKULblYT5xO144p8kgvaHysQ7xd9mIU%2CAOrYGsnXCT5yF7hVAqNQ7ZJPDVSz5_lqD1zlmnjcWuJxGJx_JrlUv4FbpJaVgJ_Xrbcz7Gidvk6Pm-T0_jgRwSgntTZRv3BZTmeF-xkcZ6-AQeZk0_M%2CAOrYGslLVTEJYaB89F2oCp4SANMycanN72TUl-EdUrpnihqOndBWAiC-4F4DFRBjJ8d5oNpV4-yGSWLNskr6BpdnqOSvBzfGHkyO7vlsd9vV2NLOrQ%2CAOrYGsndG8rL6LzshgbnzcS5rLUGiQyUc8a6fWLBBWQvyCV4eXLLBL0I3DcL-GZTnVO4scfwKwriwwDvDQM_nNYRH0qhN9_H8yeu1sNea0tCka3m0Q%2CAOrYGsljd0jsq_ZxmxW6WOZiaZjkyCZHCP8HyPvRZWXakJunnUaP83-Rkd5dyBZ5wBb0uuM_emgynS_Db_hYc9hZDRBXk48F-FkfSfwZ_H4JE7o-sI4%2CAOrYGsnt0Ilnh4_SmZvWJjAD1QEKYUEVTWNJLKjV5dMrOLpXI2LzPPzp6fB8kqS9k1lVDaMDmbjxnhw0fx_hzW8Z_KN8kIGw1Ab0lY46-3dndrtF_WA%2CAOrYGskEhhn5-Z8UbDQK2AJfD7Q1ntPlh8pG_YHGcBaA0K8S57U82JazrlK1d26hmWF6tuhinw3vG2_LvvgJVQ8GVJzsfdE7HpLVips5abplnlUeB3E&amp;pvsid=734425893021693&amp;tmod=500019996&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1957%2C30%2C1957%2C30%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

先 **確認自己的環境。**

```
echo "TASKS=${CLAUDE_CODE_ENABLE_TASKS:-未設定} TODO=${CLAUDE_CODE_ENABLE_TODO_TOOLS:-未設定}"
```

| 狀態 | 會發生什麼 |
| --- | --- |
| 兩者都未設定 | 保持模型預設，不用動 |
| 有 `ENABLE_TASKS=0` | 在新模型上會全部消失，這就是原因 |
| 再加上 `ENABLE_TODO_TOOLS=1` | Task 工具會回來 |
| 想要固定出現 | 可以寫進 `settings.json` |

```
{
  "env": { "CLAUDE_CODE_ENABLE_TODO_TOOLS": "1" }
}
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3970593978&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787187857&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FamZKgq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187566789&amp;bpp=1&amp;bdt=1043&amp;idt=1&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787187568%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=10&amp;correlator=3517120581920&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6471&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2858&amp;eid=95397678%2C122880370&amp;oid=2&amp;psts=AOrYGsmkm3m8IW3s9WT7DdyRSzVKi0vgKyspRkvO3pkYrcR13GbX4zpXvtPAxZl4FIRHGKbdFXsX4Kc5JnDM9ET17m7w9wyg2zGtS24QsKULblYT5xO144p8kgvaHysQ7xd9mIU%2CAOrYGsnXCT5yF7hVAqNQ7ZJPDVSz5_lqD1zlmnjcWuJxGJx_JrlUv4FbpJaVgJ_Xrbcz7Gidvk6Pm-T0_jgRwSgntTZRv3BZTmeF-xkcZ6-AQeZk0_M%2CAOrYGslLVTEJYaB89F2oCp4SANMycanN72TUl-EdUrpnihqOndBWAiC-4F4DFRBjJ8d5oNpV4-yGSWLNskr6BpdnqOSvBzfGHkyO7vlsd9vV2NLOrQ%2CAOrYGsndG8rL6LzshgbnzcS5rLUGiQyUc8a6fWLBBWQvyCV4eXLLBL0I3DcL-GZTnVO4scfwKwriwwDvDQM_nNYRH0qhN9_H8yeu1sNea0tCka3m0Q%2CAOrYGsljd0jsq_ZxmxW6WOZiaZjkyCZHCP8HyPvRZWXakJunnUaP83-Rkd5dyBZ5wBb0uuM_emgynS_Db_hYc9hZDRBXk48F-FkfSfwZ_H4JE7o-sI4%2CAOrYGsnt0Ilnh4_SmZvWJjAD1QEKYUEVTWNJLKjV5dMrOLpXI2LzPPzp6fB8kqS9k1lVDaMDmbjxnhw0fx_hzW8Z_KN8kIGw1Ab0lY46-3dndrtF_WA%2CAOrYGskEhhn5-Z8UbDQK2AJfD7Q1ntPlh8pG_YHGcBaA0K8S57U82JazrlK1d26hmWF6tuhinw3vG2_LvvgJVQ8GVJzsfdE7HpLVips5abplnlUeB3E%2CAOrYGskZ7i7x42rAFq1ajHnvTn2M0BgVfAd5pH-uh9aQONdddEqpp5HvrVfZIedo2-wl57qCxJL0Nw2kCcw7zZlK1wG0IePGl-lMINQs&amp;pvsid=734425893021693&amp;tmod=500019996&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1957%2C30%2C1957%2C30%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

另外，官方說清單會出現，是在「 **需要三個以上不同操作的複雜工作** 」時。  
如果只是短請求，本來就不會產生，所以 **把它打開也不會有差別。**

---

## 總結

- **原因是 `CLAUDE_CODE_ENABLE_TASKS=0` 。** 跟新模型組合時，整個功能會消失
- **`ENABLE_TASKS=0` 原本的意義是「關掉 Task 工具，改用 TodoWrite」。** 但如果本來就沒有 Task， `TodoWrite` 也不會出現
- **要恢復的是 `CLAUDE_CODE_ENABLE_TODO_TOOLS=1` 。** 但 **回來的是 Task 工具，不是 `TodoWrite`**
- Task 工具比 `TodoWrite` 更完整， **可以有依賴關係與負責人** （依官方文件，未實測）
- **不同模型的預設值不同** （ `-p` 可確認）。Opus 5 是 `TodoWrite` ，Haiku 4.5 是 Task 工具
- 官方文件主要是 **Agent SDK** ，與 CLI 的實際行為可能不完全一致

如果你覺得最近清單不見了， **問題可能不是模型，而是環境變數。**  
先檢查 `CLAUDE_CODE_ENABLE_TASKS` 吧。

---

**參考**

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3355444817&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787187959&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FamZKgq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187566801&amp;bpp=1&amp;bdt=1059&amp;idt=1&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787187568%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=3517120581920&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=7409&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=3808&amp;eid=95397678%2C122880370&amp;oid=2&amp;psts=AOrYGsmkm3m8IW3s9WT7DdyRSzVKi0vgKyspRkvO3pkYrcR13GbX4zpXvtPAxZl4FIRHGKbdFXsX4Kc5JnDM9ET17m7w9wyg2zGtS24QsKULblYT5xO144p8kgvaHysQ7xd9mIU%2CAOrYGsnXCT5yF7hVAqNQ7ZJPDVSz5_lqD1zlmnjcWuJxGJx_JrlUv4FbpJaVgJ_Xrbcz7Gidvk6Pm-T0_jgRwSgntTZRv3BZTmeF-xkcZ6-AQeZk0_M%2CAOrYGslLVTEJYaB89F2oCp4SANMycanN72TUl-EdUrpnihqOndBWAiC-4F4DFRBjJ8d5oNpV4-yGSWLNskr6BpdnqOSvBzfGHkyO7vlsd9vV2NLOrQ%2CAOrYGsndG8rL6LzshgbnzcS5rLUGiQyUc8a6fWLBBWQvyCV4eXLLBL0I3DcL-GZTnVO4scfwKwriwwDvDQM_nNYRH0qhN9_H8yeu1sNea0tCka3m0Q%2CAOrYGsljd0jsq_ZxmxW6WOZiaZjkyCZHCP8HyPvRZWXakJunnUaP83-Rkd5dyBZ5wBb0uuM_emgynS_Db_hYc9hZDRBXk48F-FkfSfwZ_H4JE7o-sI4%2CAOrYGsnt0Ilnh4_SmZvWJjAD1QEKYUEVTWNJLKjV5dMrOLpXI2LzPPzp6fB8kqS9k1lVDaMDmbjxnhw0fx_hzW8Z_KN8kIGw1Ab0lY46-3dndrtF_WA%2CAOrYGskEhhn5-Z8UbDQK2AJfD7Q1ntPlh8pG_YHGcBaA0K8S57U82JazrlK1d26hmWF6tuhinw3vG2_LvvgJVQ8GVJzsfdE7HpLVips5abplnlUeB3E%2CAOrYGskZ7i7x42rAFq1ajHnvTn2M0BgVfAd5pH-uh9aQONdddEqpp5HvrVfZIedo2-wl57qCxJL0Nw2kCcw7zZlK1wG0IePGl-lMINQs%2CAOrYGsmAjvX1BhVIg3EfZ5m5G6YwIDUgDYnDAU4ipfw0e4YwCUttfMFkRNR5Ov1tmvvDXLZQFJDLf6JWdw9p9nUCAeSUlGOZwWml9qW6CXhDKsNKQQ&amp;pvsid=734425893021693&amp;tmod=500019996&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1957%2C30%2C1957%2C30%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

- [Todo Lists — Claude Code Docs](https://code.claude.com/docs/en/agent-sdk/todo-tracking)
- [Claude Code changelog](https://code.claude.com/docs/en/changelog) — 2.1.233 的項目
- 檢證環境：Windows 10 / Claude Code 2.1.234

※ 引用內容同時附上原文與日文譯文。為了可讀性，譯文有做適度調整；若要精確措辭請以原典為準。

## 相關文章

- [Claude Code 的 fork，本來預設開啟卻不能用](https://qiita.com/jqit_suwa/items/11f46a9fc9cb429ceaf5) — 同樣是環境變數改變行為的案例
- [即使把 CLAUDE.md 寫得很厚也沒用](https://qiita.com/jqit_suwa/items/2dee3e3d53080c3676a0) — 與其靠指示，不如靠機制約束更有效

---

JQIT 95% 以上的工程師都是從零經驗錄用的。  
如果有興趣，歡迎也來公司網站逛逛。

▶ [公司網站](https://www.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=corporate)

我們也有工程師招募。如果有興趣，歡迎看看。

▶ [招募網站](https://recruitment.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=recruit)

---

原文出處： [https://qiita.com/jqit\_suwa/items/dda364cfccd770e110d6](https://qiita.com/jqit_suwa/items/dda364cfccd770e110d6)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/amZKgq) [3瀏覽](https://codelove.tw/@tony/post/amZKgq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/amZKgq)

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

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝12 💬4 ❤️2

297

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

7

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1787187566&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FamZKgq&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187565956&amp;bpp=6&amp;bdt=194&amp;idt=68&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787100919%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787100919%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787100919%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3517120581920&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397678%2C122880370&amp;oid=2&amp;pvsid=734425893021693&amp;tmod=500019996&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=162" title="Advertisement" aria-label="Advertisement"></iframe>