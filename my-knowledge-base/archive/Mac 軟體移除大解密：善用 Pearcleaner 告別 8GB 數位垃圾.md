---
title: "Mac 軟體移除大解密：善用 Pearcleaner 告別 8GB 數位垃圾"
source: "https://vocus.cc/article/690d6b06fd8978000180187c"
author:
  - "[[Leon（亮之）]]"
published: 2025-11-07
created: 2026-05-28
description: "許多 Mac 使用者誤以為將 App 拖至垃圾桶即可徹底刪除，實則不然。本文將介紹 Pearcleaner，一款開源免費的 Mac App 清理工具，搭配 Homebrew 安裝，能協助使用者找出並移除 App 殘留於系統各處的快取、偏好設定、日誌等檔案，最大化回收硬碟空間，提升系統效能。"
tags:
  - "clippings"
---
> 「我以為早就刪掉那個 App 了……」

為什麼會看到廣告

<iframe frameborder="0" src="https://e5ed8460a6206a4df738f331133a9007.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="728" height="90" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>這是我赫然發現自己的 Mac 上竟然還殘留著 ****8GB「已刪除」App 的隱藏檔案**** 時，脫口而出的第一句話。

和大多數人一樣，過去我也都是直接把 App 拖進垃圾桶就當作「卸載完成」。但真相是： ****在 macOS 中，這樣做其實只刪掉了 App 的 60% 左右**** 。

如果你也長期用這種方式「卸載」Mac 軟體，那你的硬碟裡很可能早已堆滿了數 GB 的「數位垃圾」。 今天這篇文章，就要教你如何透過 ****Pearcleaner（搭配 Homebrew 安裝）**** ，真正徹底地卸載 Mac App，並一併清理過去遺留下來的殘骸。

> 📌 博主小補充： 過去我試過 AppCleaner，甚至付費的 CleanMyMac，但總覺得不是太侵入、就是功能冗餘，讓人難以安心。後來一度靠 Raycast 內建的卸載功能撐場，基本夠用。 直到最近測試環境爆炸、App 裝太多，512GB 的 MacBook 儲存空間直接見底——才又認真尋找更理想的清理工具。 結果意外挖到這款 ****免費、開源、支援 CLI、介面清爽又高效**** 的神器： ****Pearcleaner**** 。

---

## 為什麼「把 App 拖進垃圾桶」≠ 完全卸載？

很多人不知道： ****Mac App 就像一個愛亂放東西的房客**** 。

安裝時，它不只會把主程式放在

```
/Applications
```
資料夾，還會悄悄在系統各處留下一堆檔案：

- ```
	~/Library/Caches/
	```
	—— 快取資料（可能非常大）
- ```
	~/Library/Preferences/
	```
	—— 偏好設定檔
- ```
	~/Library/Application Support/
	```
	—— 使用者資料、資料庫、外掛等
- ```
	~/Library/Logs/
	```
- ```
	~/Library/Containers/
	```
	—— 沙盒環境（常見於 App Store 下載的 App）
- ```
	~/Library/Group Containers/
	```
	—— 多個關聯 App 共用的資料

```
.app
```
檔拖進垃圾桶時， ****只有主程式被移除，其他所有關聯檔案仍靜靜躺在系統中**** ，默默吃掉你的儲存空間，直到你手動清理為止。

![zoomable](https://images.vocus.cc/66d9250b-e6d6-4700-af70-98d928103499.jpg)

---

## Pearcleaner：Mac 缺失的「真正卸載器」

這時候， ****Pearcleaner**** 就派上用場了。

它是一款 ****免費、開源、注重隱私的 Mac App 清理工具**** ，能幫你找出並徹底刪除某個 App 的所有關聯檔案。

### Pearcleaner 的核心優勢：

- ✅ ****查全率高達 98%+**** （遠勝手動刪除的 60~70%）
- ✅ ****刪除前預覽所有檔案**** ，避免誤刪重要資料
- ✅ ****一鍵徹底清理**** ：拖入 → 預覽 → 刪除，三步搞定
- ✅ ****安全第一**** ：檔案先移入垃圾桶，可隨時復原

> 💡 它靈感來自經典工具 AppCleaner，但完全重寫，支援最新 macOS（Ventura / Sonoma / Sequoia），且原始碼公開透明，無後門疑慮。

---

## 用 Homebrew 安裝 Pearcleaner（新手友善）

即使你從沒用過終端機，也能輕鬆完成安裝。照著以下步驟操作即可。

### 第一步：透過 Homebrew 安裝 Pearcleaner

> ****Homebrew 是 macOS 上最流行的套件管理器**** ，類似 Linux 的 apt 或 yum，能一鍵安裝／更新命令列工具。

打開「終端機」（Terminal），輸入以下指令：

```
brew install --cask pearcleaner
```

等待幾秒鐘，下載安裝完成後，Pearcleaner 就會出現在你的「應用程式」資料夾中。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?norotation=true&quality=80&url=https%3A%2F%2Fimages.vocus.cc%2F87ac8b9b-31c0-4ae2-abf0-a1a6b20e2a3e.jpg&width=740&sign=tfYW5FHAgfItN9IIgK5uFklbtiY_SvFN11Mac5Gbmok) ![zoomable](https://images.vocus.cc/87ac8b9b-31c0-4ae2-abf0-a1a6b20e2a3e.jpg)

### 第二步：授予必要權限

首次啟動 Pearcleaner 時，macOS 可能會阻止執行（因為它不是來自 App Store）。

✅ 解決方法如下：

1. 前往「系統設定 > 隱私與安全性」
2. 在底部找到「仍要開啟」按鈕並點擊
3. ****重新啟動 Pearcleaner****
4. 首次執行時會要求「完全磁碟存取權限」—— ****務必允許**** ，否則無法掃描所有檔案！
![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?norotation=true&quality=80&url=https%3A%2F%2Fimages.vocus.cc%2F66ef3a8f-74ee-4345-90c9-7f638ba8a190.jpg&width=740&sign=I3P5J6BoWO5GLL8STXS87VWjJqmN6Yb9v_pFpJefFsI) ![zoomable](https://images.vocus.cc/66ef3a8f-74ee-4345-90c9-7f638ba8a190.jpg)

---

## 實戰演練：徹底卸載 VS Code

假設你之前試用過 Visual Studio Code，現在決定不再使用，想把它「連根拔起」。

### 1\. 啟動 Pearcleaner

你會看到簡潔乾淨的介面，列出所有已安裝的 App，支援依名稱、大小或安裝時間排序。

### 2\. 選擇要卸載的 App

在列表中找到 ****VS Code**** ，點選它，再點擊「卸載」按鈕。

### 3\. 查看 Pearcleaner 掃描出的檔案

這才是重點！

你會看到類似這樣的結果：

- ****App 主程式**** ：約 300 MB
- ****關聯檔案（快取、設定、擴充套件等）**** ：竟高達 ****1.2 GB****

> 🤯 注意！ ****殘留檔案比 App 本體還大四倍！**** 這就是為什麼 Pearcleaner 如此重要——它幫你看見那些「看不見的垃圾」。

確認無誤後，點擊「刪除」，所有檔案都會被移入垃圾桶（非永久刪除，可復原）。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?norotation=true&quality=80&url=https%3A%2F%2Fimages.vocus.cc%2F14b7cfea-fba1-4717-889a-14e53935ebb0.jpg&width=740&sign=yrMqq2IOV99enpsel3N2DaiexuLoSs9x9Ta2sZgSRtA) ![zoomable](https://images.vocus.cc/14b7cfea-fba1-4717-889a-14e53935ebb0.jpg)

---

## 用 Homebrew 一鍵更新 Pearcleaner

透過 Homebrew 安裝的最大好處之一就是： ****更新超簡單**** 。

傳統方式更新軟體通常要：

> 開啟 App → 檢查更新 → 下載 DMG → 拖曳覆蓋 → 輸入密碼……

而用 Homebrew，只需一行指令：

```
brew upgrade pearcleaner
```

自動下載最新版並替換，省時又省力。

---

## 你的 Mac 剛剛成功「瘦身」！

現在，你已經學會如何在 macOS 上 ****真正徹底卸載 App**** 。

回收的不只是儲存空間，更是系統的整潔與流暢度。長期使用 Pearcleaner，能有效避免「Mac 越用越卡、儲存空間莫名不足」的困擾。

---

## 延伸資源

- 🔗 [Pearcleaner GitHub 頁面](https://github.com/alienator88/Pearcleaner) （查看原始碼、提交建議、下載最新版）
- 🔗 [Homebrew 官網](https://brew.sh/) （學習更多套件管理技巧）

---

****如果你透過這篇文章成功清出幾個 GB 的空間，不妨分享給那位總在抱怨「Mac 儲存不夠用」的朋友吧！**** 😊

Happy Cleaning! 🍐✨