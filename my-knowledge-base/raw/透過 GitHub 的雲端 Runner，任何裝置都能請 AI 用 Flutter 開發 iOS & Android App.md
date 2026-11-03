---
title: "透過 GitHub 的雲端 Runner，任何裝置都能請 AI 用 Flutter 開發 iOS & Android App"
source: "https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-flutter-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E9%80%8F%E9%81%8E-github-%E7%9A%84%E9%9B%B2%E7%AB%AF-runner-%E4%BB%BB%E4%BD%95%E8%A3%9D%E7%BD%AE%E9%83%BD%E8%83%BD%E8%AB%8B-ai-%E7%94%A8-flutter-%E9%96%8B%E7%99%BC-ios-android-app-e8bf9e7e4f20"
author:
  - "[[彼得潘的 App / AI Neverland]]"
published: 2026-10-25
created: 2026-10-27
description: "GitHub 的雲端 Runner 讓我們能用大部分的電腦和手機開發 Flutter App，生成在 iOS & Android 運行的 App。不需要花時間安裝 Flutter SDK、Android Studio、Xcode 和模擬器，只要一台能連網用雲端 AI 的裝置，即可"
tags:
  - "clippings"
status: "inbox"
---
GitHub 的雲端 Runner 讓我們能用大部分的電腦和手機開發 Flutter App，生成在 iOS & Android 運行的 App。不需要花時間安裝 Flutter SDK、Android Studio、Xcode 和模擬器，只要一台能連網用雲端 AI 的裝置，即可操控 GitHub Runner 開發 Flutter App。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*x0lCjozFdjDFB-z_X2NJpg.png)

## [GitHub - actions/runner-images: GitHub Actions runner images](https://github.com/actions/runner-images?source=post_page-----e8bf9e7e4f20-----------------------------------------)

### GitHub Actions runner images. Contribute to actions/runner-images development by creating an account on GitHub.

github.com

以下彼得潘從手機的 Claude App 示範，請 AI 透過 GitHub 的 Runner 開發介紹萬聖節妖怪的 Flutter App，然後再請 AI 在 iOS & Android 模擬器進行 App 畫面的截圖和錄製 App 操作影片。

ps: 彼得潘也成功透過 Codex 做到一樣的事。

## Claude 使用 GitHub Runner 前的準備 & 建立 GitHub repository

請參考以下連結的說明。

## [透過 GitHub Xcode Runner，任何裝置都能透過 AI 開發 iOS App](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E9%80%8F%E9%81%8E-github-xcode-runner-%E4%BB%BB%E4%BD%95%E8%A3%9D%E7%BD%AE%E9%83%BD%E8%83%BD%E9%80%8F%E9%81%8E-ai-%E9%96%8B%E7%99%BC-ios-app-fe9226d58d64?source=post_page-----e8bf9e7e4f20-----------------------------------------)

### GitHub 的 Xcode Runner 讓我們能用大部分的電腦和手機開發 iOS App。就算是 Windows 電腦和 Android 手機，只要能連到網路，依然能用雲端 AI 操控 Xcode Runner 開發 iOS…

medium.com

## 開發 Flutter App

打開手機或電腦的 Claude App，打開 Code 頁面，點擊新增工作階段。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*ec9jwLQTw07F1Bts.png)

選擇雲端環境，GitHub Repository 選擇 HalloweenMonster。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*oPjiCx_p7LfqK3eVw-4nxw.png)

輸入 prompt。

> 請用 Flutter 開發介紹萬聖節妖怪 的 App，需求如下  
> \- 從網路找妖怪的圖片  
> \- 設定 App Icon  
> \- 使用 GitHub 的 runner，錄製 App 在 iPhone 18 Pro Max 和 Pixel 10 模擬器的操作影片。影片只需要 App 的操作，請刪除多餘的部分，再將影片壓縮傳給我  
> \- 截圖上架的圖片

ps: 使用 Codex 開發時，可在 prompt 提到 GitHub repository 的名字。

## Get 彼得潘的 App / AI Neverland’s stories in your inbox

Join Medium for free to get updates from this writer.

Claude 成功開發出萬聖節妖怪圖鑑 App。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*_hzEk-pU_8agXiLjB34nYQ.png)

Claude 分別在 iOS 和 Android 模擬器錄製了 40 秒的操作影片。

Claude 擷取畫面後，製作了 5 張添加標題的圖片。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*2IwQvMo0KOAgKTE6sBB08g.png)

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*mDmBamv1XXReAqiPRTTQfQ.png)

Claude 以月夜南瓜燈繪製 App Icon。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*myJ0MV1cKY29zDonV2dClw.png)

GitHub 的 HalloweenMonster 現在有完整的專案程式和 README。

## [GitHub - PeterPanSwift/HalloweenMonster](https://github.com/PeterPanSwift/HalloweenMonster?source=post_page-----e8bf9e7e4f20-----------------------------------------)

### Contribute to PeterPanSwift/HalloweenMonster development by creating an account on GitHub.

github.com

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*lXlzcHhvFAI_kJo-waM6Ng.png)

## 修改 App

我們隨時可以請 AI 修改 App，只要在 Claude Code 工作時綁定專案程式對應的 GitHub repository。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*oPjiCx_p7LfqK3eVw-4nxw.png)

## 製作 skill 加快 AI 開發 Flutter App 的速度

雖然 AI 成功開發出 Flutter App，不過它在過程中遇到許多問題，嘗試很多不同的做法才完成任務。建議請 AI 將整個開發的過程做成 skill，比方以下是 Claude 依據剛剛開發萬聖節 App 的經驗製作的 flutter-ci-demo-recording skill。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*UQnp1qMpshMiwtfV3W-85Q.png)

## [GitHub - PeterPanSwift/flutter-ci-demo-recording-skill](https://github.com/PeterPanSwift/flutter-ci-demo-recording-skill?source=post_page-----e8bf9e7e4f20-----------------------------------------)

### Contribute to PeterPanSwift/flutter-ci-demo-recording-skill development by creating an account on GitHub.

github.com

## 將 App 裝到實機和上架

透過 GitHub 的 Runner，我們除了能生成在模擬器執行的 App，還可以生成裝到實機和上架的 App。這部分彼得潘未來會再撰寫文章說明。