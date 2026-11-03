---
title: "透過 GitHub Xcode Runner，任何裝置都能透過 AI 開發 iOS App"
source: "https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E9%80%8F%E9%81%8E-github-xcode-runner-%E4%BB%BB%E4%BD%95%E8%A3%9D%E7%BD%AE%E9%83%BD%E8%83%BD%E9%80%8F%E9%81%8E-ai-%E9%96%8B%E7%99%BC-ios-app-fe9226d58d64"
author:
  - "[[彼得潘的 App / AI Neverland]]"
published: 2026-10-23
created: 2026-10-27
description: "GitHub 的 Xcode Runner 讓我們能用大部分的電腦和手機開發 iOS App。就算是 Windows 電腦和 Android 手機，只要能連到網路，依然能用雲端 AI 操控 Xcode Runner 開發 iOS App。而且最新版的 Xcode Runner 提"
tags:
  - "clippings"
status: "inbox"
---
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*lRDDerALlK7G4fGp9GKL4Q.png)

GitHub 的 Xcode Runner 讓我們能用大部分的電腦和手機開發 iOS App。就算是 Windows 電腦和 Android 手機，只要能連到網路，依然能用雲端 AI 操控 Xcode Runner 開發 iOS App。而且最新版的 Xcode Runner 提供了 Xcode 27，因此還能測試最新的 iPhone 18 Pro & iPhone Duo 模擬器。

## [Xcode 27 runner image generally available - GitHub Changelog](https://github.blog/changelog/2026-10-15-xcode-27-runner-image-generally-available/?source=post_page-----fe9226d58d64-----------------------------------------)

### You can now use the generally available Xcode 27 runner image to build and test your Apple applications in GitHub…

github.blog

以下彼得潘從手機的 Claude App 示範，請 AI 透過 GitHub 的 Xcode Runner 開發介紹冰島的 iOS App，然後再請 AI 進行 App 畫面的截圖和錄製 App 操作影片。

ps: 彼得潘也成功透過 Codex 做到一樣的事。

## Claude 使用 Xcode Runner 前的準備

為了讓 Claude 使用 GitHub 的相關功能，請先設定 GitHub 的連接器。

打開電腦版的 Claude App，點選自訂 > 連接器 ＞探索，點擊 GitHub 整合的連接。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*yr0yNyieePtNf4Am3WXvqA.png)

點擊剛剛連接的 GitHub 整合。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*CECUqp7524OVCNlvvF2qRg.png)

點選修正安裝 Claude GitHub 應用程式。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*WhodG6MfNedSCYQrt282TQ.png)

點選安裝 GitHub 應用程式。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*FGt59yLbtihZyqkfThtsvQ.png)

安裝成功，GitHub 應用程式出現綠色勾勾。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*nbk3j9-YZXMlj_WRaphdDw.png)

ps: 若是使用 Codex 的雲端開發，可在選擇環境時連結 GitHub 帳號。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*dlDZfVJ_NT1PNX2hxCGvXg.png)

## 建立 GitHub repository

Xcode Runner 必須連結 GitHub repository 才能製作 iOS App，因此請先從 GitHub 網頁新增存放 Xcode 專案程式的 repository。在此彼得潘新增 repository Iceland-App3。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*H2r-j21Nx4atvBlo6uA4Cg.png)

我們只須建立空白專案，待會 Claude 將為我們生成完整的專案程式。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*XuToWxD8f8z4geW33eongQ.png)

## 開發 iOS App

打開手機或電腦的 Claude App，打開 Code 頁面，點擊新增工作階段。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*JRMUuxyOCCvmnkdpo5bv7Q.png)

選擇雲端環境。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*WDNSk3iBSfy5bbKMH1i17A.png)

建議點選上圖的設定按鈕，將網路存取改成 `完整網路存取權` ，方便 AI 工作時從網路抓資料。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*cdWjNF9zhaPD4qfKcFakMg.png)

GitHub Repository 選擇 Iceland-App3。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*x2RJH-qjwgYeJANWzwAN3Q.png)

輸入 prompt。

> 請用 SwiftUI 開發介紹 Iceland 的 App，需求如下  
> \- 從網路找 Iceland 的漂亮景點圖片  
> \- 設定 App Icon  
> \- 使用 GitHub 的 Xcode runner，用 Xcode 27 錄製 App 在 iPhone 18 Pro Max 模擬器的操作影片。影片只需要 App 的操作，請刪除多餘的部分，再將影片壓縮傳給我  
> \- 截圖上架 App Store 的圖片

ps: 使用 Codex 開發時，可在 prompt 提到 GitHub repository 的名字。

## Get 彼得潘的 App / AI Neverland’s stories in your inbox

Join Medium for free to get updates from this writer.

Claude 成功完成任務。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*hZ3DBXZOco-ci9Z_0JSXrA.png)

Claude 精心錄製了 87 秒的操作影片。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*qT-LPhGeHqocYmQHs5PVog.png)

Claude 擷取畫面後，製作了 5 張添加標題的圖片。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*HZ5VZrOsP2CuvPggTT1IEA.png)

Claude 以極光下的草帽山剪影繪製 App Icon。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*1sZRL3DGi0PpEbSgbRvYUw.png)

GitHub 的 Iceland-App3 現在有完整的專案程式和 README。

## [GitHub — PeterPanSwift/Iceland-App3](https://github.com/PeterPanSwift/Iceland-App3?source=post_page-----fe9226d58d64-----------------------------------------)

### Contribute to PeterPanSwift/Iceland-App3 development by creating an account on GitHub.

github.com

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*iYkDfo4U7X-SqWDTExQjKw.png)

Iceland-App3/Deliverables 存放著 Claude 剛剛建立的影片和圖片。

![](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*H2MoqbIQxVdbOsN0n8qOKA.png)

## 修改 App

我們隨時可以請 AI 修改 App，只要在 Claude Code 工作時綁定專案程式對應的 GitHub repository。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*x2RJH-qjwgYeJANWzwAN3Q.png)

## 將 App 裝到 iPhone 和上架 App Store

透過 Xcode Runner，我們除了能生成在模擬器執行的 App，還可以生成裝到 iPhone 和上架 App Store 的 App。這部分彼得潘未來會再撰寫文章說明。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*UZeNc7UKJP8kEO5JjsDp2Q.png)

Iceland App 裝到 iPhone 的畫面。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*B3-X9cgV3j4qXga6TcXZ1Q.png)

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*7MfeL7neyZZDVKI4h48Gug.png)