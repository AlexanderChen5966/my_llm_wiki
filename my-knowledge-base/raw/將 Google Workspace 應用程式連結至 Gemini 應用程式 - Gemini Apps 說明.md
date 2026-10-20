---
title: "將 Google Workspace 應用程式連結至 Gemini 應用程式 - Gemini Apps 說明"
source: "https://support.google.com/gemini/answer/15229592?hl=zh-HK"
author:
published:
created: 2026-05-20
description: "在 Gemini 應用程式中，你可以使用 Google Workspace 應用程式連結 Google Workspace 服務中的資料，以提升生產力和簡化工作流程。透過 Google Workspace 應用程式，你可以要求 Gemini 應用程式： 撮要、取得快速解答，以及尋找 Gmail、文件和雲端硬碟等 Google Workspace 服務中的資料 新增、編輯和取得 Goog"
tags:
  - "clippings"
status: processed
---
## 將 Google Workspace 應用程式連結至 Gemini 應用程式

在 Gemini 應用程式中，你可以使用 Google Workspace 應用程式連結 Google Workspace 服務中的資料，以提升生產力和簡化工作流程。透過 Google Workspace 應用程式，你可以要求 Gemini 應用程式：

- 撮要、取得快速解答，以及尋找 Gmail、文件和雲端硬碟等 Google Workspace 服務中的資料
- [新增、編輯和取得 Google Tasks 中的工作和提醒](https://support.google.com/gemini/answer/15230285)
- [從 Google Keep 建立及取得筆記和清單](https://support.google.com/gemini/answer/15230597)
- [在「Google 日曆」建立及管理活動](https://support.google.com/gemini/answer/15305236)

**貼士：** 連結 Google Workspace 應用程式後，系統亦會根據 Gmail 內容提供個人化的 Gemini 體驗。 [進一步瞭解根據「互連應用程式」提供的個人化體驗](https://support.google.com/gemini/answer/16598406) 。

## 所需項目

如要將 Google Workspace 連結至 Gemini 應用程式，你需要：

- **登入 Gemini 應用程式。** 請確保登入的帳戶與 Google Workspace 使用的帳戶相同。 [瞭解如何登入 Gemini 應用程式](https://support.google.com/gemini/answer/13278668) 。

- **已開啟「 [活動記錄](https://myactivity.google.com/product/gemini?utm_source=help) 」。** 關閉此設定後，Gemini 將無法連結至 Google Workspace。

**重要事項：**

- 目前只有部分 Google Workspace 服務支援 Live 對話。 [瞭解如何在 Live 對話中使用「互連應用程式」](https://support.google.com/gemini/answer/15274899#live_apps) 。
- Gemini 應用程式的答覆可能會出現幻覺現象或提供過時資訊，例如在有較新電郵的情況下，提供較舊電郵的資訊。如要檢查資料，請選取並查核列於答覆後的來源。

## 在 Gemini 應用程式中使用 Google Workspace 應用程式

1. 請確保你已登入用於 Google Workspace 的同一帳戶。
2. 要求 Gemini 應用程式從 Google Workspace 取得資料。
	- **提示：** 請勿在提示中貼上文件或電郵的網址。如要參照 Google Workspace 中的內容，請使用內容中的關鍵字，並指定服務或內容類型。如要指定 Google Workspace 服務，請輸入 `@` 並選取服務。
3. 如果你尚未連結 Google Workspace，系統會顯示連結選項。
4. 按照螢幕指示操作。

你可以在「 [互連應用程式設定](https://gemini.google.com/apps) 」中解除連結 Google Workspace 應用程式。

### 範例

- **尋找資料**
	- 喺有關大峽谷遠足之旅嘅電郵入面，Erik 提議邊啲日子？
		- 喺我嘅「雲端硬碟」入面搵最近嘅租賃協議，然後睇下保證金係幾多
- **總結文件內容**
	- 喺我嘅雲端硬碟入面搵名為「2023 年 6 月」嘅履歷，然後總結為一段簡短嘅個人陳述
- **簡化和整理資料**
	- 搵出 Ashley 撰寫有關社區清潔項目嘅文件，然後用 5 個要點總結提案

- **從對話中擷取工作至 Google Tasks**
	- 幫我計劃露營之旅，同埋整個行李清單。  
		在 Tasks 中建立提醒，提醒自己帶備這些物品。
- **將構思和筆記儲存至 Google Keep**
	- 推薦幾個京都景點，然後儲存去 Keep。
		- 幫我搵蘋果批食譜，然後將材料儲存去 Keep。

- **查看時間表和管理「日曆活動」**
	- 我日曆有咩活動？
		- 睇下 Alice 發俾我關於今個週末去太浩湖玩嘅電郵，然後將行程加入我個日曆度。

## 為何我無法連結 Google Workspace 應用程式

**如果是公司或學校帳戶，Google Workspace 管理員必須先啟用連結應用程式的功能。** [瞭解如何透過公司或學校帳戶連結 Gemini 中的應用程式](https://support.google.com/gemini/answer/14959807) 。

**Gmail 的必要設定可能已關閉。** 如要讓 Gemini 應用程式連結 Google Workspace，必須在 Gmail 設定中開啟 \[其他 Google 產品的智能功能\]。開啟後，請重新載入 Gemini 應用程式，然後嘗試再連結。

[瞭解如何變更智能功能和個人化設定](https://support.google.com/mail/answer/10079371) 。

**在提示中提及 Google Workspace 應用程式或 Google Workspace 服務。** 在每次新對話中，明確要求 Gemini 應用程式從你的個人 Google Workspace 取得或加入內容。你可以在提示中提及 Gmail 或電郵、Docs 或文件、雲端硬碟或 PDF、活動或日曆、待辦事項或 Tasks，以及筆記或 Keep。同一對話中的跟進問題則無需再提及。例如：

- **第一個提示：** 總結我最近收到 Harper 小學嘅電郵
- **下一個提示：** 邊日係睡衣日？

## Gemini 應用程式無法透過 Google Workspace 應用程式執行的操作

Gemini 應用程式無法：

- 存取「文件」或 Gmail 中的留言或圖片
- 存取你的「雲端硬碟」中的相片和影片
	- 包括任何非文件、試算表、簡報或 PDF 格式的內容
- 建立、草擬或刪除試算表、PDF、文件和簡報
- 管理「雲端硬碟」中的內容，例如建立資料夾或在資料夾之間移動內容
- 數算「雲端硬碟」或 Gmail 中的項目，或告知你「雲端硬碟」或 Gmail 的儲存空間用量

## 資料的處理方式

進一步瞭解 [Gemini 配合「互連應用程式」運作時的資料處理方式](https://support.google.com/gemini/answer/13594961#data_exchange) 。

## 相關資源

- [使用和管理 Gemini 中的「互連應用程式」](https://support.google.com/gemini/answer/13695044)
- [Gemini 應用程式私隱控制中心](https://support.google.com/gemini/answer/13594961)
- [使用 Gemini 應用程式建立和管理日曆活動](https://support.google.com/gemini/answer/15305236)
- [使用 Gemini 應用程式建立構思和筆記](https://support.google.com/gemini/answer/15230597)
- [使用 Gemini 應用程式建立工作和提醒](https://support.google.com/gemini/answer/15230285)