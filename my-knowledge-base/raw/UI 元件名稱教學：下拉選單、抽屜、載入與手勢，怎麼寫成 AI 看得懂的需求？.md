---
title: "UI 元件名稱教學：下拉選單、抽屜、載入與手勢，怎麼寫成 AI 看得懂的需求？"
source: "https://www.aiposthub.com/ui-component-names-behavior/"
author:
  - "[[Philo]]"
published: 2026-11-11
created: 2026-11-13
description: "UI 元件名稱教學。跟 AI 說「做個下拉選單」，結果可能是選國家的欄位，也可能是刪除、分享的操作清單。兩者外觀相似，但使用方式不同，所以只講外觀，很容易交出另一種功能。"
tags:
  - "clippings"
status: "inbox"
---
跟 AI 說「做個下拉選單」，結果可能是選國家的欄位，也可能是刪除、分享的操作清單。兩者外觀相似，但使用方式不同，所以只講外觀，很容易交出另一種功能。

UI 元件名稱可以縮短溝通，UI 就是使用者看見並操作的介面。不過名稱仍要配上觸發方式、位置與關閉條件，才能變成可驗收的需求。

如果你正在用 Claude、Codex 做網站，我建議把元件名稱當成共同詞彙，再用一句白話釘住行為。這比背完所有英文名稱更容易開始。

## UI 元件名稱怎麼查？先從 Component Gallery 看用途

[Component Gallery](https://component.gallery/?ref=aiposthub.com) 把不同設計系統的元件整理在一起，同時列出別名。它適合查「這種東西通常叫什麼」，也能看同一個名稱有哪些不同實作。

以 Accordion 為例，它把內容收在可展開的區塊裡。但「一次只能開一項」或「能同時開多項」仍須另外交代，因為名稱本身沒有決定所有操作細節。

抽屜也是如此。 [Drawer](https://component.gallery/components/drawer/?ref=aiposthub.com) 通常從邊緣進入，Sheet 常指覆蓋部分畫面的面板，要做手機底部面板時，最好直接描述從底部開啟、能否拖曳，以及背景可不可以操作。

![The Component Gallery 的 Accordion 元件說明](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/ui-component-names-behavior.png)

The Component Gallery 的 Accordion 元件說明。來源： 官方／作者發布頁 。

## Select、Dropdown menu 與 Combobox 差在哪？

先看使用者要完成什麼，才能選到對的元件。

| 使用目的 | 常用名稱 | 需求要補的行為 |
| --- | --- | --- |
| 從固定選項選一個值 | Select | 選完後欄位顯示結果 |
| 執行分享、刪除等動作 | Dropdown menu | 點操作後清單關閉 |
| 輸入文字再搜尋選項 | Combobox | 輸入後篩選，選中後填入欄位 |

這三種需求都可能被口語叫成下拉選單，但把搜尋功能交給純選單，使用者就會少一個輸入入口。你可以把上表對應到實際任務，再請 AI 寫畫面。

## Loading 和手勢為什麼還要補一句？

「加個 loading」也會留下很多選擇。 [Spinner](https://component.gallery/components/spinner/?ref=aiposthub.com) 是旋轉等待圖示，Skeleton 是內容尚未載入時的輪廓，進度條則能呈現已完成多少。若系統根本不知道完成比例，就不要顯示像是真實進度的百分比。

手勢名稱同樣會受平台慣例影響。長按後可以出現貼著物件的選單，也可以出現底部動作表，請寫清楚你要哪一種，並附上目標平台的設計文件。

一份可直接使用的需求可以這樣寫：

> 點頭像後，在頭像右下方開啟 Dropdown menu，列出帳號設定與登出。點畫面其他位置或按 Escape 關閉，清單開啟時，不遮住整個頁面。用鍵盤也能選擇操作。

這段除了名稱，還交代了觸發、位置、關閉方式與鍵盤操作，因此驗收時能逐條點選確認。

## 常見問題

### 不知道英文名稱，能不能直接描述？

可以。先寫「要做什麼、出現在哪、怎麼消失」，再請 AI 提出適合的元件名稱，最後對照實際行為。

### Component Gallery 的別名都能互換嗎？

別名反映不同設計系統的用語。專案最好固定一套名稱，並以行為規格處理歧義。

### 要把所有元件都換成現成套件嗎？

先看既有專案的元件。已經有可用版本時，沿用它能減少樣式與操作不一致，新增套件前再確認必要性。

## 從一個互動開始寫清楚

拿目前最容易做錯的互動，補上「名稱、觸發、位置、關閉、鍵盤」五項，再請 AI 修改。完成後用滑鼠、手機與鍵盤各走一次，才知道名稱是否真的被實作成你要的行為。

延伸閱讀： [AI 做的介面怎麼去 AI 味？四則 X 貼文整理成四個做法](https://www.aiposthub.com/ai-ui-unslop-four-x-posts-names-numbers-checklist/)

延伸閱讀： [Animate UI 2.0 更新：保留 shadcn 元件，讓 AI 幫網站補上可維護的動效](https://www.aiposthub.com/animate-ui-2-shadcn-ai-motion-skill-guide/)