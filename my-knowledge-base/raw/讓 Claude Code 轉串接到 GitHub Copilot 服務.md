---
title: "讓 Claude Code 轉串接到 GitHub Copilot 服務"
source: "https://dotblogs.com.tw/jamestsai/2026/05/09/How-to-Claude-Code-use-GitHub-Copilot"
author:
  - "[[James's Tech & Life Blog]]"
published: 2026-05-09
created: 2026-05-22
description: "文章內容僅為個人實驗測試紀錄，不建議使用在生產環境。短期內有效，未來可能因相關的設計改變，如：API 調整 而失效，請斟酌。適用的 Claude 產品有：1."
tags:
  - "clippings"
status: processed
---
###### 文章內容僅為個人實驗測試紀錄，不建議使用在生產環境。

###### 短期內有效，未來可能因相關的設計改變，如：API 調整 而失效，請斟酌。

###### 適用的 Claude 產品有：1. Claude Code (CLI)2. Visual Studio Code 的 Claude Code extension

以上注意事項請知悉。

###### 本文章提到 Visual Studio Code 的 Claude Code extension 使用時，會稱之為 VS Code with Claude。

---

###### 使用的需求情境是如此：

已有訂閱 GitHub Copilot 的服務，但又不想多另外再訂閱 Claude 服務(畢竟 GitHub Copilot 當中就可以使用 Opus、Sonnet…等各家 LLM 模型)。

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/e8e7cf88-da3b-4ca4-b51f-6e913fe55400/1778339601.png.png)

經過研究一番，可以運用的方式有不少，但都會有比較多的其他考量的地方，都不是算太好的解法。

最後找到這篇在 GitHub 的 gist 介紹：  
[透過 copilot-api 和 claude-code-router，將您的 GitHub Copilot Business 訂閱變成 Claude Code 的強大後端](https://gist.github.com/cht-k/62a27d1e8efea97b49db557fd85ceaff)

看到這篇的介紹就似乎有點眉頭了。

根據該篇 gist 的介紹，也找到 [copilot-api](https://github.com/ericc-ch/copilot-api) 的 Repo 研究了一下。

該 Repo 的作者也有明說，這不是官方(指 Claude 與 GitHub Copilot) 的推薦作法，充其量只能算是一個實驗性的專案。

雖然此專案後續也沒有持續維護了，所以還是測試跑看看，會發現 OpenAI 新式 "Responses API" 的 api 格式都不適用了，所以在該 Repo 的 [issues](https://github.com/ericc-ch/copilot-api/issues/240) 當中看到了 [copilot-bridge](https://github.com/betaHi/copilot-bridge) 的 Repo。

[copilot-bridge](https://github.com/betaHi/copilot-bridge) 的作者則基本上是接續 copilot-api 的架構繼續發展下去，並能串接到 OpenAI 新式 "Responses API" 的 api 格式。

而且在該 Repo 當中的 Readme 有提到 " [Why this bridge](https://github.com/betaHi/copilot-bridge#why-this-bridge) "，有興趣的可以看看。

好，那該怎用呢？

在 Windows 中若已經有裝好 npm 的話，就直接透過下列的 npm 指令即可：

`npm exec betahi-copilot-bridge@latest -- start --port 4142 --account-type individual`

其中 --account-type 參數可用的設定有三種：

1. individual: GitHub Copilot 的訂閱是 student、pro、pro+ 的用戶。
2. business: GitHub Copilot 的訂閱是 business 的用戶。
3. enterprise: GitHub Copilot 的訂閱是 enterprise 的用戶。

###### 注意：第一次會需要經過 GitHub 的 Device 授權驗證。

執行後會看到幾個注意事項ℹ️，其中 Available models 會列出目前支援的 41 種 models：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/e8e7cf88-da3b-4ca4-b51f-6e913fe55400/1778335385.jpg.jpg)

透過 Usage Viewer 的網址：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/e8e7cf88-da3b-4ca4-b51f-6e913fe55400/1778335401.jpg.jpg)

就可以觀察目前的使用狀態分析：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/e8e7cf88-da3b-4ca4-b51f-6e913fe55400/1778336029.png.png)

在 Claude Code 當中發了一個請求後，按了一下 Fetch 就會發現 premium interactions 增加了：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/e8e7cf88-da3b-4ca4-b51f-6e913fe55400/1778336006.jpg.jpg)

由於選擇 "gpt-5.5" 按照目前的價格計算是 x7.5 的倍率，所以從剩餘 936 掉到 929。

那 Claude Code 跟 VS Code 的 Claude 要怎設定？

在電腦的使用者帳號資料夾路徑下找到.claude 資料夾，會看到 "settings.json"：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/e8e7cf88-da3b-4ca4-b51f-6e913fe55400/1778335886.png.png)

打開後加入相關設定：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/e8e7cf88-da3b-4ca4-b51f-6e913fe55400/1778336515.png.png)

```純文字
"env": {
  "ANTHROPIC_BASE_URL": "http://127.0.0.1:4142",
  "ANTHROPIC_AUTH_TOKEN": "dummy",
  "ANTHROPIC_MODEL": "gpt-5.5",
  "MODEL_REASONING_EFFORT": "medium",
  "COPILOT_WEB_SEARCH_BACKEND": "gpt-5.5"
}
```

存檔後，請重新啟動 Claude Code 或 VS Code 以利載入新設定。

接下來就可以開心地(?) 使用 Claude Code (CLI) 或 VS Code with Claude。

另外，這個 settings.json 的設定完成後，則無論是 Claude Code (CLI) (左) 或 VS Code with Claude (右) 都可以順利使用：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/e8e7cf88-da3b-4ca4-b51f-6e913fe55400/1778424027.jpg.jpg)

而在先前執行 `betahi-copilot-bridge` 的 Command Line 視窗會看到：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/e8e7cf88-da3b-4ca4-b51f-6e913fe55400/1778337650.png.png)

最後整理一下本文介紹的架構：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/e8e7cf88-da3b-4ca4-b51f-6e913fe55400/1778401296.png.png)

有興趣的人再玩玩看唷~~~

###### 本介紹 Claude Desktop 無法適用。

---

I'm a [Microsoft MVP - Developer Technologies](https://mvp.microsoft.com/zh-tw/PublicProfile/5001645) (From 2015 ~).

![MVP_Logo](https://jamestsaitwstorage.blob.core.windows.net/blogs/mvp/Logo.png)

I focus on the following topics: Xamarin Technology, Azure, Mobile DevOps, and Microsoft EM+S.  
  
If you want to know more about them, welcome to my website:  
[https://jamestsai.tw](http://jamestsai.tw/)

---

本部落格文章之圖片相關後製處理皆透過 [Techsmith 公司](https://www.techsmith.com/) 所贊助其授權使用之 "Snagit" 與 "Snagit Editor" 軟體製作。

<iframe width="730" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-5120420174726287&amp;output=html&amp;h=280&amp;slotname=1709268749&amp;adk=1963031652&amp;adf=1892154115&amp;pi=t.ma~as.1709268749&amp;w=730&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779412755&amp;rafmt=1&amp;format=730x280&amp;url=https%3A%2F%2Fdotblogs.com.tw%2Fjamestsai%2F2026%2F05%2F09%2FHow-to-Claude-Code-use-GitHub-Copilot&amp;fwr=0&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=11&amp;dt=1779412721539&amp;bpp=1&amp;bdt=49&amp;idt=3&amp;shv=r20260521&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D698460076376e1e5%3AT%3D1779412642%3ART%3D1779412642%3AS%3DALNI_MYEraUetAL0MC_iVlemSfxyK6HX-Q&amp;gpic=UID%3D000014209ed1c6e2%3AT%3D1779412642%3ART%3D1779412642%3AS%3DALNI_MYzaaKKR36LxlPP7z81HcBhoOcTLw&amp;eo_id_str=ID%3Dcd95dec3541792c5%3AT%3D1779412642%3ART%3D1779412642%3AS%3DAA-AfjZOZ-ZqwMdjr-Qzo5O_XvOw&amp;prev_fmts=250x250%2C0x0&amp;nras=1&amp;correlator=2637130142985&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=4&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=310&amp;ady=8608&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=4899&amp;eid=42533293%2C95391808&amp;oid=2&amp;psts=AOrYGsmdYW0gee60DR_kphof_FbFig1b1BKeJZZHBp6XLA2CanPPaN3-Le8PMLiZD2HACBOwQkdHynHX-alb-lENaYaryr5Slc5ux8yn8N2qBWt1zjg&amp;pvsid=6372073359723311&amp;tmod=1553137272&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fdotblogs.com.tw%2Fjamestsai&amp;fc=896&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7CeEbr%7C&amp;abl=CS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;btvi=1&amp;fsb=1&amp;dtd=33897" title="Advertisement" aria-label="Advertisement"></iframe>

### 關聯文章

- [在 Visual Studio / Visual Studio Code 使用 GitHub Copilot Chat](https://dotblogs.com.tw/jamestsai/2026/03/27/use-GitHub-Copilot-in-Visual-Studio-or-Visual-Studio-Code)
- [對 GitHub 的 Organization 中的成員設定 GitHub Copilot: 操作篇](https://dotblogs.com.tw/jamestsai/2026/02/21/Set-GitHub-Copilot-for-your-Member-in-a-GitHub-Organization-Usage)
- [透過 Microsoft Teams 整合 NDI 技術進行遠距會議連線並透過 OBS Studio 完成直播活動](https://dotblogs.com.tw/jamestsai/2026/03/10/Microsoft-Teams-with-NDI-Broadcast-and-OBS-Live-Streaming)
- [在 Windows 上安裝 nodejs 與 npm 執行環境的一篇紀錄文...](https://dotblogs.com.tw/jamestsai/2022/03/13/Install-nodejs-and-npm-on-Windows)
- [在 Visual Studio Code 中撰寫 SmartContract 並透過 Web3 進行區塊鏈服務交易 II](https://dotblogs.com.tw/jamestsai/2020/12/02/Build-a-SmartContract-in-Visual-Studio-Code-and-Do-a-Blockchain-Transaction-with-Web3-Part2)
- [在 Visual Studio Code 中安裝 Blockchain Development Kit for Ethereum 延伸模組](https://dotblogs.com.tw/jamestsai/2020/11/02/Azure-Blockchain-Service-Install-Visual-Studio-Code-Extension-for-Ethereum)
- [在 Visual Studio Code 中建置 SmartContract 並透過 Web3 進行區塊鏈服務交易 I](https://dotblogs.com.tw/jamestsai/2020/12/01/Build-a-SmartContract-in-Visual-Studio-Code-and-Do-a-Blockchain-Transaction-with-Web3-Part1)
- [在 macOS 中安裝 GitHub Copilot Chat 給 Xcode 使用](https://dotblogs.com.tw/jamestsai/2026/03/14/Setup-GitHub-Copilot-Chat-for-Xcode-IDE)
- [在 Android Studio 當中安裝 GitHub Copilot Chat 使用](https://dotblogs.com.tw/jamestsai/2026/03/11/Setup-GitHub-Copilot-Chat-for-Android-Studio-IDE)
- [在 Visual Studio Code 中撰寫 SmartContract 並透過 Web3 進行區塊鏈服務交易 III](https://dotblogs.com.tw/jamestsai/2020/12/03/Build-a-SmartContract-in-Visual-Studio-Code-and-Do-a-Blockchain-Transaction-with-Web3-Part3)
- [在 Windows / macOS 中安裝 GitHub Copilot CLI](https://dotblogs.com.tw/jamestsai/2026/03/15/Setup-GitHub-Copilot-CLI-for-Windows-or-macOS)
- [在 OpenCode Desktop 中使用 GitHub Copilot (在 macOS 與 Windows 皆可)](https://dotblogs.com.tw/jamestsai/2026/03/28/use-OpenCode-Desktop-with-GitHub-Copilot-for-macOS-and-Windows)
- [透過 Visual Studio Code 發佈 SmartContract 應用至 Azure Blockchain Service 服務並與其進行乙太坊區塊鏈交易](https://dotblogs.com.tw/jamestsai/2021/01/22/Deploy-a-SmartContract-to-Azure-Blockchain-Service-by-Visual-Studio-Code-and-Do-a-ETH-Transaction)
- [Visual Studio 使用 GitHub Copilot 的授權採用 Device Flow 驗證](https://dotblogs.com.tw/jamestsai/2023/08/20/Visual-Studio-with-GitHub-Copilot-authorized-by-Device-Flow)
- [給 WebRTC 測試連線使用的 Server-side 服務: node-dss](https://dotblogs.com.tw/jamestsai/2022/03/02/Node-Dss-a-WebRTC-Server-side-Service-note-its-just-only-for-Test)
- [對 GitHub 的 Organization 中的成員設定 GitHub Copilot: 解釋篇](https://dotblogs.com.tw/jamestsai/2026/02/17/Set-GitHub-Copilot-for-your-Member-in-a-GitHub-Organization-Explanation)
- [GitHub Copilot 在 Organization 使用中對於 Copilot 的 Custom models 設定](https://dotblogs.com.tw/jamestsai/2026/03/06/GitHub-Copilot-for-Organization-add-Custom-Models-settings)

### 系列文章

- [對 GitHub 的 Organization 中的成員設定 GitHub Copilot: 解釋篇](https://dotblogs.com.tw/jamestsai/2026/02/17/Set-GitHub-Copilot-for-your-Member-in-a-GitHub-Organization-Explanation)
- [對 GitHub 的 Organization 中的成員設定 GitHub Copilot: 操作篇](https://dotblogs.com.tw/jamestsai/2026/02/21/Set-GitHub-Copilot-for-your-Member-in-a-GitHub-Organization-Usage)
- [GitHub Copilot 在 Organization 使用中對於 Copilot 的 Custom models 設定](https://dotblogs.com.tw/jamestsai/2026/03/06/GitHub-Copilot-for-Organization-add-Custom-Models-settings)
- [在 Android Studio 當中安裝 GitHub Copilot Chat 使用](https://dotblogs.com.tw/jamestsai/2026/03/11/Setup-GitHub-Copilot-Chat-for-Android-Studio-IDE)
- [在 macOS 中安裝 GitHub Copilot Chat 給 Xcode 使用](https://dotblogs.com.tw/jamestsai/2026/03/14/Setup-GitHub-Copilot-Chat-for-Xcode-IDE)
- [在 Windows / macOS 中安裝 GitHub Copilot CLI](https://dotblogs.com.tw/jamestsai/2026/03/15/Setup-GitHub-Copilot-CLI-for-Windows-or-macOS)
- [在 Visual Studio / Visual Studio Code 使用 GitHub Copilot Chat](https://dotblogs.com.tw/jamestsai/2026/03/27/use-GitHub-Copilot-in-Visual-Studio-or-Visual-Studio-Code)
- [在 OpenCode Desktop 中使用 GitHub Copilot (在 macOS 與 Windows 皆可)](https://dotblogs.com.tw/jamestsai/2026/03/28/use-OpenCode-Desktop-with-GitHub-Copilot-for-macOS-and-Windows)
- 讓 Claude Code 轉串接到 GitHub Copilot 服務