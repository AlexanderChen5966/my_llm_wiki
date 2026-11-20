---
title: "12 年前的 iPhone 6 Plus 突破官方升級限制！開發者成功移植 iOS 14.8，Touch ID、Apple Pay 都能用"
source: "https://www.kocpc.com.tw/archives/703657"
author:
  - "[[Rocky]]"
published: 2026-11-17
created: 2026-11-19
description: "很多人的手邊應該都有無法升級 iOS 新版本的舊 iPhone，雖然硬體還正常，但由於 App 不再支援，舊 iPhone 也就不知道能拿來幹嘛，未來搞不好有救了。2014 年上市的 iPhone 6 Plus，早在 iOS 12 就已被 Apple 宣告停止支援，照理說再也無法升級新版本，沒想到最近有中國開發者突破了這項限制，成功安裝 iOS 14.8 版本，而且不是只開得了機，觸控、相機、Wi-Fi 都正常，連 Touch ID 跟 Apple Pay 也能用。"
tags:
  - "clippings"
status: "processed"
---
很多人的手邊應該都有無法升級 iOS 新版本的舊 iPhone，雖然硬體還正常，但由於 App 不再支援，舊 iPhone 也就不知道能拿來幹嘛，未來搞不好有救了。

2014 年上市的 iPhone 6 Plus，早在 iOS 12 就已被 Apple 宣告停止支援，照理說再也無法升級新版本，沒想到最近有中國開發者突破了這項限制，成功安裝 iOS 14.8 版本，而且不是只開得了機，觸控、相機、Wi-Fi 都正常，連 Touch ID 跟 Apple Pay 也能用。

## iPhone 6 Plus 也能跑 iOS 14.8！開發者借 iPad mini 4 系統完成移植，Touch ID、Apple Pay 都能用

中國開發者「Celestial 紗雪」近日在 Bilibili 發布影片，宣稱全球首次讓 iPhone 6 Plus 執行 iOS 14.8，可以正常同步 iCloud、配對 Apple Watch。

他把這款工具命名為 n56-patcher（N56 是 iPhone 6 Plus 的內部代號），並在 GitHub 以 GPL-3.0 授權開源。開發過程不意外的使用大量 AI 工具，包括 Claude Fable 5.2、Opus 5.5、Sonnet 5.5、Haiku 5.5、Opus 4.8 與 Sonnet 4.6 等多款模型。

根據說明，他不是去修改 iOS 12，而是從三台不同裝置的系統裡各拿一部分組起來：

| 來源裝置 | 晶片 | 拿了什麼 |
| --- | --- | --- |
| iPad mini 4（iPadOS 14.8） | A8 | 系統核心與整套系統 |
| iPhone 6s Plus（iOS 14.8） | A9 | 驅動程式、iPhone 專用的介面素材 |
| **iPhone 6 Plus 本機（iOS 12.5.8）** | **A8** | **Secure Enclave 韌體、通訊元件、每台手機各自的校正資料** |

簡單來說，骨架是來自同樣 A8 晶片的 iPad mini 4，而像 iPhone 的部分，則是借自 5.5 吋螢幕的 iPhone 6s Plus，每台手機專屬的安全鑰匙與校正資訊則由 6 Plus 自己提供。

安裝時，n56-patcher 會在手機上另外切出一個獨立的 APFS 磁碟區放這套 iOS 14.8，原本的 iOS 12.5.8 會保留下來，變成非官方的雙系統。

![12 年前的 iPhone 6 Plus 突破官方升級限制！開發者成功移植 iOS 14.8，Touch ID、Apple Pay 都能用 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261117075241_0_143eec.png "12 年前的 iPhone 6 Plus 突破官方升級限制！開發者成功移植 iOS 14.8，Touch ID、Apple Pay 都能用 - 電腦王阿達")

能讓 iPhone 6 Plus 開機，當然也是需要越獄，工具裡有整合的越獄工具 checkra1n，利用 checkm8 漏洞。

這漏洞是直接燒在晶片的開機程式裡，Apple 沒辦法透過軟體更新修掉，iPhone 5s 到 iPhone X 都受影響。

因此，iOS 14.8 是屬於「有線引導」，每次重新開機都需要把手機切到 DFU 模式，再由裝了 Xcode 的 Mac 送出開機指令。如果在外手機沒電，必須回到這台 Mac 才能再次進入 iOS 14.8。

依照目前公開的測試結果，iOS 14.8 在 iPhone 6 Plus 上可以使用的功能包括：

- 螢幕顯示與觸控
- Wi-Fi、藍牙、行動網路
- 音訊、震動、各種感測器與相機
- 健康 App，以及 iCloud、App Store 帳號登入
- Touch ID、NFC 與 Apple Pay
- Apple Watch 配對

他也提到，整份清單裡最難搞的是 Touch ID。

iOS 14 用的是比較新的溝通協定，iPhone 6 Plus 的 SEP 韌體只聽得懂 iOS 12 時代的舊版本，等於兩邊講不同版本的語言，他的解決辦法是在系統核心裡加一座「翻譯橋」。

<iframe title="Bilibili 影片播放器" src="https://player.bilibili.com/player.html?isOutside=true&#38;aid=117620867143556&#38;bvid=BV1yxsH6wEjX&#38;cid=26036803225&#38;p=1&#38;autoplay=0" allowfullscreen="allowfullscreen">&#60;br /&#62;</iframe>

那一般人能不能自己裝這個開源工具進而實現運行 iOS 14.8？不太推薦，因為門檻不低。

[n56-patcher](https://github.com/CelestialSayuki/n56-patcher) 有提供命令列工具和本地網頁介面，你需要準備一台 Mac、完整安裝 Xcode 與 Python 環境，再自行準備 iPhone 6 Plus、iPhone 6s Plus、iPad mini 4 三份對應的 Apple 韌體檔（IPSW），寫入階段還需要系統管理員權限。

GitHub 上也已經有早期使用者回報各種問題。iPhone 只有 16GB 空間也要注意，16GB 實際可用約 14.9 GiB，而 iOS 14.8 系統約佔 8.09 GiB、資料分割區約 2.1 GiB，保留下來的 iOS 12.5.8 還要約 4.2 GiB，幾乎塞滿。

![12 年前的 iPhone 6 Plus 突破官方升級限制！開發者成功移植 iOS 14.8，Touch ID、Apple Pay 都能用 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261117075110_0_fda39c.png "12 年前的 iPhone 6 Plus 突破官方升級限制！開發者成功移植 iOS 14.8，Touch ID、Apple Pay 都能用 - 電腦王阿達")