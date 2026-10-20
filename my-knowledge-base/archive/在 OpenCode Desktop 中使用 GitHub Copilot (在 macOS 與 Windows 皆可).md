---
title: "在 OpenCode Desktop 中使用 GitHub Copilot (在 macOS 與 Windows 皆可)"
source: "https://dotblogs.com.tw/jamestsai/2026/03/28/use-OpenCode-Desktop-with-GitHub-Copilot-for-macOS-and-Windows"
author:
  - "[[James's Tech & Life Blog]]"
published: 2026-03-28
created: 2026-05-22
description: "如果真要推薦近期偏 AI-driven 而生的 IDE 開發工具，目前比較有 \"獨立\" 精神的是 OpenCode \"Desktop\"。雖然個人覺得 OpenC"
tags:
  - "clippings"
---
如果真要推薦近期偏 AI-driven 而生的 IDE 開發工具，目前比較有 "獨立" 精神的是 OpenCode "Desktop"。

雖然個人覺得 OpenCode 是 CLI 主打，但 OpenCode 後來也推出了 OpenCode Desktop 可以直接在 macOS / Windows 安裝使用：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887021.png.png)

###### 會推薦使用 OpenCode Desktop 是其操作介面相當簡潔 (Claude Code、Cursor 的介面也都很簡潔)。但若要以對接到不同供應商的 AI model 這點來看的話，以 OpenCode Desktop 的切換方式，個人是覺得相當好理解。

而且 OpenCode 除了本身有提供 [Zen 的服務方案](https://opencode.ai/docs/zen/) 外，若想要直接串 Nvidia 平台上提供的 AI model 來介接使用的話，也很推薦直接使用 OpenCode ，詳情請見： [NVIDIA Build 太香了吧？搭配 OpenCode 免費幫你寫程式](https://dotblogs.com.tw/donmadotblog/2026/03/31/nvidia-build-opencode-free-ai-coding-guide) 的介紹。

當然仍是主推使用 GitHub Copilot 的囉！

---

OpenCode Desktop 在 macOS 就是使用 \**.dmg 安裝* ；在 Windows 就是 *\*.exe* 安裝：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887115.png.png)

安裝完成後，執行起來無論是在 macOS / Windows 上幾乎長一樣：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887244.png.png)

點選 OpenCode Desktop 畫面左下角的 "設定"。

切換 "提供者"，在熱門供應商當中可以看到 GitHub Copilot，點選 "連線"：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887280.png.png)

選擇連線到 GitHub Copilot 的方式：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887298.png.png)

透過 Device code 的驗證方式：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887333.png.png)

若已在瀏覽器中登入 GitHub Account，直接瀏覽 " [https://github.com/login/device](https://github.com/login/device) "：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887393.png.png)

確定是要使用該 GitHub Account 的話，點選 "Continue"。

輸入剛剛 OpenCode Desktop 所顯示的 "Device code"：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887375.png.png)

完成後點選 "Continue"：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887434.png.png)

出現 "Authorize OpenCode" 畫面：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887528.png.png)

點選 "Authorize anomalyco"。

###### 請確認上面顯示的資訊：1. 大約地理位址與 IP 位址 (若有用 VPN 的話地理位置就不一定了)。2. 時間戳 (畫面顯示的時間是不是接近看到那組 Device code 的時間)。真的沒問題才按下 "Authorize anomalyco"。

驗證無誤，瀏覽器中會出現如下畫面：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887539.png.png)

再切回 OpenCode Desktop 查看提供者：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887606.png.png)

已連線的供應商會出現 GitHub Copilot。

透過 OpenCode Desktop 開啟工作資料夾後：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774888349.png.png)

切換一下 AI model。

可以找到 GitHub Copilot 有提供的 AI model 使用：

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774887680.png.png)

在此登入的 GitHub Account 所使用的是 **GitHub Copilot Business** 的授權，若該 GitHub Account 所屬的 Organziation 有啟用該 AI model，就都可以立即使用。

![](https://dotblogsfile.blob.core.windows.net/user/james%20tsai/fb474da0-a385-4bc6-8805-9b02f4638a0e/1774889313.jpg.jpg)

---

I'm a [Microsoft MVP - Developer Technologies](https://mvp.microsoft.com/zh-tw/PublicProfile/5001645) (From 2015 ~).

![MVP_Logo](https://jamestsaitwstorage.blob.core.windows.net/blogs/mvp/Logo.png)

I focus on the following topics: Xamarin Technology, Azure, Mobile DevOps, and Microsoft EM+S.  
  
If you want to know more about them, welcome to my website:  
[https://jamestsai.tw](http://jamestsai.tw/)

---

本部落格文章之圖片相關後製處理皆透過 [Techsmith 公司](https://www.techsmith.com/) 所贊助其授權使用之 "Snagit" 與 "Snagit Editor" 軟體製作。

<iframe width="730" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-5120420174726287&amp;output=html&amp;h=280&amp;slotname=1709268749&amp;adk=1963031652&amp;adf=1892154115&amp;pi=t.ma~as.1709268749&amp;w=730&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779412642&amp;rafmt=1&amp;format=730x280&amp;url=https%3A%2F%2Fdotblogs.com.tw%2Fjamestsai%2F2026%2F03%2F28%2Fuse-OpenCode-Desktop-with-GitHub-Copilot-for-macOS-and-Windows&amp;fwr=0&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=11&amp;dt=1779412152731&amp;bpp=66&amp;bdt=1260&amp;idt=188&amp;shv=r20260521&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie_enabled=1&amp;eoidce=1&amp;prev_fmts=0x0&amp;nras=1&amp;correlator=5192789525017&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=310&amp;ady=7173&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=3459&amp;eid=31098652%2C95390668%2C95390787%2C42533293%2C95391808&amp;oid=2&amp;pvsid=7113531508813858&amp;tmod=1553137272&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=896&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7CeEbr%7C&amp;abl=CS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

### 關聯文章

- [在 Visual Studio / Visual Studio Code 使用 GitHub Copilot Chat](https://dotblogs.com.tw/jamestsai/2026/03/27/use-GitHub-Copilot-in-Visual-Studio-or-Visual-Studio-Code)
- [對 GitHub 的 Organization 中的成員設定 GitHub Copilot: 操作篇](https://dotblogs.com.tw/jamestsai/2026/02/21/Set-GitHub-Copilot-for-your-Member-in-a-GitHub-Organization-Usage)
- [透過 Microsoft Teams 整合 NDI 技術進行遠距會議連線並透過 OBS Studio 完成直播活動](https://dotblogs.com.tw/jamestsai/2026/03/10/Microsoft-Teams-with-NDI-Broadcast-and-OBS-Live-Streaming)
- [在 macOS 中安裝 GitHub Copilot Chat 給 Xcode 使用](https://dotblogs.com.tw/jamestsai/2026/03/14/Setup-GitHub-Copilot-Chat-for-Xcode-IDE)
- [讓 Claude Code 轉串接到 GitHub Copilot 服務](https://dotblogs.com.tw/jamestsai/2026/05/09/How-to-Claude-Code-use-GitHub-Copilot)
- [在 Android Studio 當中安裝 GitHub Copilot Chat 使用](https://dotblogs.com.tw/jamestsai/2026/03/11/Setup-GitHub-Copilot-Chat-for-Android-Studio-IDE)
- [在 Windows / macOS 中安裝 GitHub Copilot CLI](https://dotblogs.com.tw/jamestsai/2026/03/15/Setup-GitHub-Copilot-CLI-for-Windows-or-macOS)
- [Visual Studio 使用 GitHub Copilot 的授權採用 Device Flow 驗證](https://dotblogs.com.tw/jamestsai/2023/08/20/Visual-Studio-with-GitHub-Copilot-authorized-by-Device-Flow)
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
- 在 OpenCode Desktop 中使用 GitHub Copilot (在 macOS 與 Windows 皆可)
- [讓 Claude Code 轉串接到 GitHub Copilot 服務](https://dotblogs.com.tw/jamestsai/2026/05/09/How-to-Claude-Code-use-GitHub-Copilot)