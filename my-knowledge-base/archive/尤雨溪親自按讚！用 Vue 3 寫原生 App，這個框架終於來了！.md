---
title: "尤雨溪親自按讚！用 Vue 3 寫原生 App，這個框架終於來了！"
source: "https://codelove.tw/@tony/post/3KXrga"
author:
published:
created: 2026-05-28
description: "尤雨溪親自點讚！用 Vue 3 寫原生 App，這個框架終於來了！  ====================================> **告別 WebView 的卡頓，像寫 Vue 3 一樣寫真正的原生應用**這一次，你的 `.vue` 檔案可以直接跑出 Swift/Kotlin ..."
tags:
  - "clippings"
---


> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月27日](https://codelove.tw/@tony/post/3KXrga "2026-05-27 17:15:18")


## 尤雨溪親自點讚！用 Vue 3 寫原生 App，這個框架終於來了！

> **告別 WebView 的卡頓，像寫 Vue 3 一樣寫真正的原生應用** 這一次，你的 `.vue` 檔案可以直接跑出 Swift/Kotlin 的效能！

大家好，我是大魚。

在行動端開發的世界裡，Vue 開發者似乎總是「低人一等」。

React 有 **React Native** ，Google 有 **Flutter** ，而 Vue 生態長期缺少一個能與之並肩的「真・原生」跨平台方案。

直到 **NativeScript-Vue 3** 的出現，並被 **Vue 之父尤雨溪親自點讚** ——這一切，終於迎來了改變。

---

## 🧐 它是什麼？為什麼尤大親自站台？

**NativeScript-Vue 3** 是一個基於 Vue 3 核心的 **跨平台原生行動開發框架** 。

它的核心理念很簡單： **沒有 WebView，沒有 DOM** ——你寫的 `<template>` 直接編譯成 iOS 的 `UIKit` 和 Android 的 `AppCompat` **原生元件** 。

> 一句話總結： **Vue 的自訂渲染器 + NativeScript 原生引擎**

---

## 📱 小程式支援？先說清楚！

這是很多讀者最關心的問題，我直接把結論放在前面：

**❌ NativeScript-Vue 3 不支援微信小程式、支付寶小程式、抖音小程式等任何小程式平台。**

它的定位非常純粹： **只做 iOS 和 Android 原生 App** ，一套程式碼，兩端執行。

### 為什麼不做小程式？

這是框架的設計哲學決定的：

技術特性NativeScript-Vueuni-app / Taro

---

渲染層 **原生 UIKit/AppCompat** WebView / 小程式專用渲染  
執行環境V8 / JavaScriptCore小程式執行時 / WebView  
核心目標原生級別效能 **多端適配能力**

NativeScript 的底層原理是 **直接呼叫 iOS/Android 的原生 API** ，而小程式的執行環境是完全不同的生態，兩者不相容。

### 那我想要小程式怎麼辦？

如果你需要涵蓋 **小程式 + App + H5** ，目前主流的選擇仍然是 **uni-app** 或 **Taro** 。

但如果你符合以下條件，NativeScript-Vue 3 會是更好的選擇：

- ✅ 只做 iOS / Android，不需要小程式
- ✅ 追求接近原生的效能（重動畫、重互動、長列表流暢）
- ✅ 團隊熟悉 Vue 3 生態，不想學 Swift/Kotlin
- ✅ 需要直接呼叫硬體能力（藍牙、NFC、感測器等）

> **一句話總結** ：想多端通吃選 uni-app，想做精品原生 App 選 NativeScript-Vue。

---

## 💔 為什麼是時候說再見了？

很多朋友還在用 uni-app 或 Hybrid 方案，但其中的痛點只有開發者自己知道：

維度傳統混合開發NativeScript-Vue 3

---

**渲染機制** WebView 渲染，啟動慢、長列表卡頓 **原生渲染** ，直接呼叫系統 UI 元件  
**原生能力** 需要各種橋接、renderjs，維護成本高 **直接呼叫** 原生 API 和 SDK，零延遲  
**開發體驗** 廠商鎖倉，Vite/Pinia 支援滯後 **Vite + TS + Vue 3** 全家桶，現代前端體驗  
**包體積** 自帶 WebView 核心，體積臃腫純原生二進位包， **平均小 30%**

有了它，你不僅獲得了 React Native 同級別的效能，還保留了 Vue 3 極致的開發體驗。

---

## ✨ 三大核心特性：Vueer 的「王炸」

### 1\. 純粹的 Vue 3 開發體驗

完全支援 `<script setup>` 、Composition API、響應式系統。你甚至可以直接使用 **Pinia** 作為狀態管理，零改動接入！

```
vue 體驗AI程式碼助手 代碼解讀複製代碼<script setup>
import { ref } from 'vue';

const count = ref(0);
const message = ref('Hello Native!');
</script>

<template>
  <Page>
    <ActionBar title="我的原生App" />
    <StackLayout class="p-4">
      <Label :text="message" class="text-2xl" />
      <Button @tap="count++" :text="\`點擊次數：${count}\`" />
    </StackLayout>
  </Page>
</template>
```

### 2\. 超強「外掛」：直接呼叫原生 API

這是它最硬派的地方。它 **沒有橋接層** ，JS 和原生執行在同一執行緒， **同步直接呼叫** 。

這意味著你可以直接在 Vue 元件裡寫 Android 的 `Intent` 跳轉或 iOS 的 `CBCentralManager` ：

```
javascript 體驗AI程式碼助手 代碼解讀複製代碼// 直接在元件中呼叫原生裝置資訊
import { device } from '@nativescript/core';

console.log(\`系統版本: ${device.osVersion}\`);
console.log(\`裝置型號: ${device.model}\`);
```

### 3\. Vite 加持的極速熱重載

終於，行動端開發也能享受 Web 端的 **毫秒級 HMR** 了！儲存程式碼，模擬器直接更新，無需重新編譯。

---

## 🛠️ 5 分鐘極速上手

**第一步：安裝 NativeScript CLI**

```
bash 體驗AI程式碼助手 代碼解讀複製代碼npm install -g @nativescript/cli
```

**第二步：建立 Vue 3 專案**

```
bash 體驗AI程式碼助手 代碼解讀複製代碼ns create my-vue-app --template @nativescript/vue-template
```

**第三步：執行起來！**

```
bash 體驗AI程式碼助手 代碼解讀複製代碼# 連接手機或開啟模擬器
ns run android   # 或 ns run ios
```

你會發現，你的第一個純原生 Vue App 就跑起來了！🚀

---

## ⚠️ 避坑指南（必看！）

雖然很香，但由於架構不同， **Vue 生態的部分函式庫是無法使用的** ：

插件是否可用說明

---

**Pinia** ✅零改動，直接使用  
**vue-i18n** ✅實測正常  
**VueUse** ⚠️僅無 DOM 的 Utilities 可用  
**Vue Router** ❌官方推薦用 NativeScript 頁面導覽 `$navigateTo`  
**Vuetify / Element Plus** ❌依賴 CSS & DOM，無法渲染

> 💡 **檢測小技巧** ：安裝套件後， `grep -r "document\|window" node_modules/xxx` ，沒輸出說明大概率安全！

---

## 🗣️ 真實體驗：一個月的開發經歷告訴你

有開發者用 NativeScript-Vue 3 花了一個月時間，做了一個 **完整的日程管理+AI 助手 App** （影時間），他的真實感受是：

> **好的地方：**
> 
> - 用 Vue 寫原生 UI，Composition API 0 學習成本
> - 同步呼叫原生 API 很爽，沒有延遲
> - 包體積小，啟動快
> 
> **不太爽的地方：**
> 
> - 社群真的小，問題搜不到，文件寫得比較簡略
> - 除錯有時候頭痛，原生堆疊錯誤要自己定位
> - JS 在主執行緒執行，耗時計算會阻塞 UI（適合中小型專案）

## 🎯 寫在最後

Flutter 很強，RN 也很成熟，但如果你是 **Vue 3** 的忠實擁護者， **NativeScript-Vue 3** 是你踏入原生應用開發最優雅、最低成本的路徑。

它讓 Vue 語法第一次 **完整、無損、高效能** 地跑在 iOS & Android 上。

**但請理性選擇：**

- 需要小程式？👉 **選 uni-app 或 Taro**
- 做精品原生 App？👉 **NativeScript-Vue 值得一試**

---

原文出處： [https://juejin.cn/post/7642933445875007534](https://juejin.cn/post/7642933445875007534)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3KXrga) [12瀏覽](https://codelove.tw/@tony/post/3KXrga)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3KXrga)

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

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [50 多個必備 Telegram 機器人的終極清單](https://codelove.tw/@tony/post/37oZV3)

🔥 [Claude Code 與 Codex CLI：該選哪一個？根據我使用兩者的經驗，從不同角度解釋要點。](https://codelove.tw/@tony/post/qEl6Ma)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝20 💬11 ❤️1

609

🥈

[alicec](https://codelove.tw/@alicec)

📝1 ❤️2

84

🥉

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

4

#4

[JCConf Taiwan](https://codelove.tw/@jcconftw)

4

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次