---
title: "讓 AI 編碼精準度大幅提升的 Context7 MCP Server 完全指南"
source: "https://codelove.tw/@tony/post/3rPw7q"
author:
published:
created: 2026-08-24
description: "你好，我是ともだ。使用 AI 程式編寫助理時，你有沒有因為生成了過時資訊的程式碼而感到煩躁過？尤其是在使用最新的函式庫或框架時，常常會出現用舊版 API 寫出來的程式碼。  結果還是得自己重新查官方文件，白白浪費時間。這次要來解說的是能從根本解決這個問題的「Context7 MCP S..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月23日](https://codelove.tw/@tony/post/3rPw7q "2026-08-23 17:05:56")

## 讓 AI 編碼精準度大幅提升的 Context7 MCP Server 完全指南

你好，我是ともだ。

使用 AI 程式編寫助理時，你有沒有因為生成了過時資訊的程式碼而感到煩躁過？

尤其是在使用最新的函式庫或框架時，常常會出現用舊版 API 寫出來的程式碼。  
結果還是得自己重新查官方文件，白白浪費時間。

這次要來解說的是能從根本解決這個問題的「Context7 MCP Server」。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787536839&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3rPw7q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536254645&amp;bpp=3&amp;bdt=1530&amp;idt=3&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905&amp;nras=3&amp;correlator=3252955365934&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=943&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679&amp;oid=2&amp;pvsid=7631356327787605&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 什麼是 Context7 MCP Server？

簡單來說，它是一個能把最新文件提供給 AI 助理的工具。

如果用圖書館來比喻，就像是把只有舊參考書的圖書館，變成永遠備有最新版本的圖書館一樣。  
而且圖書館員（AI）還會自動挑選最新的書來告訴你。

Context7 是由 Upstash 開發的 MCP 伺服器。  
支援超過 15,000 個函式庫的官方文件。

最大的特色是，使用方式簡單得令人驚訝。  
只要在提示詞中加上「use context7」就能運作。

## 為什麼需要 Context7

傳統的 AI 程式編寫助理有一個很大的弱點。  
那就是，它只知道訓練當時的資訊。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3764087313&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787536839&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3rPw7q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536254654&amp;bpp=1&amp;bdt=1540&amp;idt=2&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280&amp;nras=4&amp;correlator=3252955365934&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1756&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679&amp;oid=2&amp;pvsid=7631356327787605&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

例如，即使 Next.js 15 已經推出，AI 仍可能用舊版本的知識來回答。  
或是不知道 TypeScript 5.5 的新功能，還提出舊寫法。

這個問題其實很惱人，對吧。

使用 Context7 的話，AI 會自動取得最新文件。  
也能提供特定版本、精確的程式碼範例。

結果就能得到以下好處：

錯誤變少。  
除錯時間縮短。  
可以寫出符合最新最佳實務的程式碼。

而且它還是完全免費的開源軟體。

## 導入方法：各工具的設定

導入 Context7 真的非常簡單。  
以下說明各主要工具的設定方法。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787536839&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3rPw7q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536254666&amp;bpp=1&amp;bdt=1551&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280&amp;nras=5&amp;correlator=3252955365934&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2636&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679&amp;oid=2&amp;pvsid=7631356327787605&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

### Cursor 的設定

Cursor 使用者請依照以下步驟設定。

首先，開啟設定畫面。  
進入「Cursor 設定」→「MCP」→「新增新的全域 MCP 伺服器」。

接著，加入以下設定。

```
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

如果要直接編輯檔案，請開啟 `~/.cursor/mcp.json` 並加入相同內容。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=186534417&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787536839&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3rPw7q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536254673&amp;bpp=1&amp;bdt=1559&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=3252955365934&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3474&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679&amp;oid=2&amp;pvsid=7631356327787605&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

### Claude Desktop 的設定

Claude Desktop 也很簡單。

只要在 `claude_desktop_config.json` 檔案中加入以下內容即可。

```
{
  "mcpServers": {
    "Context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

### Claude Code 的設定

如果是 Claude Code，只要一行指令就能完成設定。

```
claude mcp add context7 -- npx -y @upstash/context7-mcp
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1038984369&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787537036&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3rPw7q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536254683&amp;bpp=1&amp;bdt=1571&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C350x280%2C1005x124%2C334x250&amp;nras=9&amp;correlator=3252955365934&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4347&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=735&amp;eid=95397679&amp;oid=2&amp;psts=AOrYGskr_sFHfjbeg8ve_wDEVdA0ZTN170WoL9p9F8qQaNS8eJ_N_KmWPHg420WkipxTwhaMkrHJsg_w_W_QlFwMxuPfE-SnNfwT1mp6rqN5pypCSl8YpI74nQy7gdhkHi4kHA%2CAOrYGsnYMemkMkxuiZUJ-DWKckUQ9aC7_91UZfyqGXj9hehhrTMZ-tDmGgkNx2kMhWWuZPodcI-ey50UYyYaKrmuVRZYpX_Rnv1j_l7J%2CAOrYGsnanL97DgBFgaRnmG-rkx22ye9BMpTt-XRIGuLv0kpvLXdcHD-I1EHWprRXnuS4hTNI6XFlPnbtDkdx0Iy2irUOU-hq3hmBBExcgzRmn0sTuQ%2CAOrYGsn3xvPXnYQWWfxLZ_9dpbQelo-J7o3qePkmuFMk16DboLhLh4KsR4srGgaP3cAmhH3MmR_Hwz1ipwBOU1ZvF6a7rGJaX7voB1WO1ulPhy5vAA%2CAOrYGsl7uiuuI04PCFLURxOR0gQCzCu4oUdMrP25lSipDhEJzRQwKU7f-BoMx9YWo9BA67uZ8jMFqBW-fhS3fLCY3nsDkEL2wkh5dd_pYuCcZ8AhTQ&amp;pvsid=7631356327787605&amp;tmod=1573633200&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C42%2C1920%2C42%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

如果上述方法無法成功，也可以試試這個。

```
claude mcp add --transport http context7 https://mcp.context7.com/mcp
```

## 實際的使用方法

Context7 的用法簡單到讓人有點意外。

只要在一般提示詞中加上「use context7」即可。  
如此一來，AI 就會自動參考最新文件。

### 基本使用範例

要建立 Next.js 專案時。

```
請使用 app 路由器建立一個基本的 Next.js 專案。use context7
```

要建立資料庫操作腳本時。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1621299728&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787537119&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3rPw7q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536254687&amp;bpp=1&amp;bdt=1577&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C350x280%2C1005x124%2C334x250%2C952x280&amp;nras=10&amp;correlator=3252955365934&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5174&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1557&amp;eid=95397679&amp;oid=2&amp;psts=AOrYGskr_sFHfjbeg8ve_wDEVdA0ZTN170WoL9p9F8qQaNS8eJ_N_KmWPHg420WkipxTwhaMkrHJsg_w_W_QlFwMxuPfE-SnNfwT1mp6rqN5pypCSl8YpI74nQy7gdhkHi4kHA%2CAOrYGsnYMemkMkxuiZUJ-DWKckUQ9aC7_91UZfyqGXj9hehhrTMZ-tDmGgkNx2kMhWWuZPodcI-ey50UYyYaKrmuVRZYpX_Rnv1j_l7J%2CAOrYGsnanL97DgBFgaRnmG-rkx22ye9BMpTt-XRIGuLv0kpvLXdcHD-I1EHWprRXnuS4hTNI6XFlPnbtDkdx0Iy2irUOU-hq3hmBBExcgzRmn0sTuQ%2CAOrYGsn3xvPXnYQWWfxLZ_9dpbQelo-J7o3qePkmuFMk16DboLhLh4KsR4srGgaP3cAmhH3MmR_Hwz1ipwBOU1ZvF6a7rGJaX7voB1WO1ulPhy5vAA%2CAOrYGsl7uiuuI04PCFLURxOR0gQCzCu4oUdMrP25lSipDhEJzRQwKU7f-BoMx9YWo9BA67uZ8jMFqBW-fhS3fLCY3nsDkEL2wkh5dd_pYuCcZ8AhTQ%2CAOrYGsmpj6M2Tyx6Plm8U-OS_O-MfHXnGeVIhOYRJvWt20C9wzAF2NmiygSC8D9DZcBXFlUULL9f7wGcl7w4G4I2G6TXA4RhhUpCiUJLY--4BgSWGA&amp;pvsid=7631356327787605&amp;tmod=1573633200&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C42%2C1920%2C42%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

```
請使用 PostgreSQL 的認證資訊，建立一個刪除 city 為空白的資料列的腳本。use context7
```

### 更實用的活用方式

當你想使用 React 19 的新功能時。

```
請使用 React 19 的新 use() Hook，實作非同步資料擷取。use context7
```

當你想活用 TypeScript 5.5 的新功能時。

```
請使用 TypeScript 5.5 的新功能，建立型別安全的 API 用戶端。use context7
```

這樣一來，就會生成符合最新規格的程式碼。

不只是 Context7，要熟練運用 MCP 伺服器，先掌握整體架構會更容易理解。  
拙著《 [用 Claude Code 做中學：AI 驅動應用程式開發入門](https://amzn.asia/d/0f0bQMI4) 》（技術評論社，2026 年 9 月 8 日上市）第 7 章，除了介紹 MCP 的運作原理與新增、管理的基本方法，也有涵蓋 Context7 MCP 的活用。  
另外還有使用 Playwright MCP 進行瀏覽器自動化、使用 Supabase MCP 進行資料庫串接等，透過實作逐步進行各種實用的 MCP 活用內容。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3970593978&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787537126&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3rPw7q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536254693&amp;bpp=1&amp;bdt=1583&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C350x280%2C1005x124%2C334x250%2C952x280%2C952x280&amp;nras=11&amp;correlator=3252955365934&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6075&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2477&amp;eid=95397679&amp;oid=2&amp;psts=AOrYGskr_sFHfjbeg8ve_wDEVdA0ZTN170WoL9p9F8qQaNS8eJ_N_KmWPHg420WkipxTwhaMkrHJsg_w_W_QlFwMxuPfE-SnNfwT1mp6rqN5pypCSl8YpI74nQy7gdhkHi4kHA%2CAOrYGsnYMemkMkxuiZUJ-DWKckUQ9aC7_91UZfyqGXj9hehhrTMZ-tDmGgkNx2kMhWWuZPodcI-ey50UYyYaKrmuVRZYpX_Rnv1j_l7J%2CAOrYGsnanL97DgBFgaRnmG-rkx22ye9BMpTt-XRIGuLv0kpvLXdcHD-I1EHWprRXnuS4hTNI6XFlPnbtDkdx0Iy2irUOU-hq3hmBBExcgzRmn0sTuQ%2CAOrYGsn3xvPXnYQWWfxLZ_9dpbQelo-J7o3qePkmuFMk16DboLhLh4KsR4srGgaP3cAmhH3MmR_Hwz1ipwBOU1ZvF6a7rGJaX7voB1WO1ulPhy5vAA%2CAOrYGsl7uiuuI04PCFLURxOR0gQCzCu4oUdMrP25lSipDhEJzRQwKU7f-BoMx9YWo9BA67uZ8jMFqBW-fhS3fLCY3nsDkEL2wkh5dd_pYuCcZ8AhTQ%2CAOrYGsmpj6M2Tyx6Plm8U-OS_O-MfHXnGeVIhOYRJvWt20C9wzAF2NmiygSC8D9DZcBXFlUULL9f7wGcl7w4G4I2G6TXA4RhhUpCiUJLY--4BgSWGA%2CAOrYGsnbuh-3q18zfPCXYEK4xvPN27mbnLEIiX5yfX_gheiHC7ImOQsX9u5FFBXsaU0Rz9unPtp2-JOpWPyHdqXBKgOLjLkFAxxhOOhZdRxpNMc8fqg&amp;pvsid=7631356327787605&amp;tmod=1573633200&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C42%2C1920%2C42%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 總結

Context7 MCP Server 是一個能大幅提升 AI 程式編寫準確度的工具。

設定很簡單。  
使用方式也很單純。  
效果非常顯著。

只要加上「use context7」這句魔法咒語，就能基於最新文件生成程式碼。

被過時資訊造成的錯誤困擾的人。  
想要熟練運用最新框架的人。  
想提升整個團隊開發效率的人。

請務必試試 Context7。

導入只需要 5 分鐘。  
但它帶來的效果，卻能讓開發效率提升好幾倍。

---

原文出處： [https://qiita.com/tomada/items/42537e0b14b38797d218](https://qiita.com/tomada/items/42537e0b14b38797d218)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3rPw7q) [40瀏覽](https://codelove.tw/@tony/post/3rPw7q)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3rPw7q)

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

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝9 💬4 ❤️1

190

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

6

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="350" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=1414751727&amp;adf=3620188380&amp;pi=t.aa~a.4029027559~rp.3&amp;abgtt=6&amp;w=350&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787536839&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=350x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3rPw7q&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536254715&amp;bpp=1&amp;bdt=1601&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=7&amp;correlator=3252955365934&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=1167&amp;ady=1079&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679&amp;oid=2&amp;pvsid=7631356327787605&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=9&amp;uci=a!9&amp;btvi=5&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1787536253&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3rPw7q&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536253346&amp;bpp=5&amp;bdt=186&amp;idt=159&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787290192%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787290192%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787290192%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3252955365934&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679&amp;oid=2&amp;pvsid=7631356327787605&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=243" title="Advertisement" aria-label="Advertisement"></iframe>