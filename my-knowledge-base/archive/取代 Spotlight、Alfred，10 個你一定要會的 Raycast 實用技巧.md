---
title: "取代 Spotlight、Alfred，10 個你一定要會的 Raycast 實用技巧"
source: "https://raymondhouch.com/lifehacker/digital-workflow/raycast-guide/"
author:
  - "[[侯智薰 （雷蒙）]]"
published: 2025-10-22
created: 2026-05-28
description: "Raycast 是一款以「鍵盤優先」打造的效率工具，相較於 Alfred，它速度更快、擴充性更強，且免費版就內建強大的剪貼簿歷史（Clipboard History）和常用詞語（Snippets）功能。 計算機功能不只處理數學，更能用自然語言換算時區與幣別查詢，並整合了 OCR、視窗管理與上千種擴充套件，不再需要安裝一堆軟體！"
tags:
  - "clippings"
---
![](https://raymondhouch.com/wp-content/uploads/2025/10/raycast-cover.webp)

Raycast 是一款以「鍵盤優先」打造的效率工具，相較於 Alfred，它速度更快、擴充性更強，且免費版就內建強大的剪貼簿歷史（Clipboard History）和常用詞語（Snippets）功能。 計算機功能不只處理數學，更能用自然語言換算時區與幣別查詢，並整合了 OCR、視窗管理與上千種擴充套件，不再需要安裝一堆軟體！

## 為什麼需要 Raycast？

![SCR 20250926 mlyn](https://raymondhouch.com/wp-content/uploads/2025/09/SCR-20250926-mlyn-2048x1337.webp)

SCR 20250926 mlyn

你是否厭倦了在應用程式之間不斷切換，或為了執行一個簡單操作而頻繁尋找滑鼠？

這也是為什麼越來越多人選擇 **[Raycast](https://raymondhouch.com/raycast)** ，一款以「鍵盤操作」為優先所打造的效率工具。

透過 `⌘ + Space` 快捷鍵，你可以瞬間啟動 Raycast，快速搜尋檔案、執行命令、控制應用程式，甚至管理複雜的工作流程。

它讓你幾乎不需要觸碰滑鼠，就能完成從搜尋、運算到自動化的各種操作！

不僅減少了手部移動，更重要的是保持了思考的連續性，讓你專注於真正重要的工作內容。

### 跟 Spotlight、Alfred 的差異？

相較於 macOS 內建的 Spotlight 或老牌的 Alfred，Raycast 的最大優勢在於 **「速度」與極度豐富的「擴充性」** 。

在 Raycast 商店裡目前有數千個擴充外掛（不需要費心調整 Alfred Workflows），包含 Apple Notes 到 Notion、Slack、GitHub，無縫整合你日常使用的各種工具。

Alfred 內建的計算機僅能簡單的數學運算，而 Clipboard History、Snippets 這兩個功能都是需要另外付費，但在 Raycast 的免費版就能使用。

⬇️ 下面我將介紹最實用的技巧，讓你的 Raycast 變成真正的效率神器。

---

## 🧮 計算類

**[Raycast](https://raymondhouch.com/raycast)** 除了能解決複雜的數學運算，更支援 **時區、單位換算、日期與幣別轉換** 。

只要在 Raycast 搜尋框輸入數字或數學表達式，就可以進行運算，不需要再叫出計算機，而且，它能直接理解「自然語言 [^1] 」。

### 數字運算

📍 **如何使用** ：除了簡單的加減乘除外，你還可以輸入如 `200 * (17+5)` 或 `32% of 50000`

![SCR 20251015 olcb](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251015-olcb-2048x1374.webp)

SCR 20251015 olcb

### 幣別價格查詢

📍 **如何使用** ：輸入如 `1000usd twd` 或 `5000 twd jpy` ，也可以直接省略 twd，輸入 1000usd 就可以查詢。

![SCR 20251015 okwj](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251015-okwj-2048x1370.webp)

甚至查看加密貨幣價格 👀

### 時區計算

📍 **如何使用** ：輸入如 `October 13th at 9 AM Eastern` 或 `Oct 21 10:30 Pacific` 會自動幫你換算成台灣時區，線上參加國外講座好方便！

![SCR 20251015 okqv](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251015-okqv-2048x1323.webp)

SCR 20251015 okqv

💡 小提示：當答案出現後，按下 `Enter` 即可直接複製解答。  
更多介紹請參考： [https://www.raycast.com/core-features/calculator](https://www.raycast.com/core-features/calculator)

---

## 📖 文書處理

### Clipboard History 剪貼簿歷史

如果你是文字工作者，相信你也有發生過這種「嘔」狀況：寫完一大段文字，按了剪下（⌘+X）快捷鍵，結果貼上東西之前按到複製（⌘+C），然後剛剛那一大段文字就消失了 🥲

Raycast 提供強大的剪貼簿管理系統，自動記錄你複製的所有內容，包括文字、圖片、連結，可以快速找回之前複製的任何資料，不再擔心重要資訊被新內容覆蓋。

![raycast clipboard](https://raymondhouch.com/wp-content/uploads/2025/10/raycast-clipboard.gif)

raycast clipboard

📍 **如何使用** ：在搜尋框搜尋 `Clipboard` ，或是直接設定快捷鍵。

![SCR 20251016 mwpk](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251016-mwpk-2048x1320.webp)

SCR 20251016 mwpk

而且支援這些功能：

- Edit Content 編輯內容
- Send to AI Chat 傳送到 AI 對話框
- Pin Entry 釘選常用項目
- Save as Snippet 存成片段

若要更強大，我之前介紹過另一款 Paste，整體介面更清楚、一目了然。

只要透過 Shift + ⌘ + P（可自訂快捷鍵），能直接叫出 Paste 剪貼簿，然後直接查詢我這一個月的所有剪下和複製的內容。

![Paste mac app %E8%A4%87%E8%A3%BD%E8%B2%BC%E4%B8%8A%E5%B1%95%E7%A4%BA](https://raymondhouch.com/wp-content/uploads/2021/07/Paste-mac-app-%E8%A4%87%E8%A3%BD%E8%B2%BC%E4%B8%8A%E5%B1%95%E7%A4%BA.gif)

Paste mac app %E8%A4%87%E8%A3%BD%E8%B2%BC%E4%B8%8A%E5%B1%95%E7%A4%BA

※ 一樣包含在 Setapp 套餐內，更多 Setapp 應用介紹：

<iframe title="Setapp：Mac 用戶必裝的應用，一次擁有 200+ 款好用訂閱式 AppStore！Mac 應用工具推薦 — 雷蒙三十｜幫助忙碌現代人的聰明工作、好好生活的生產力指南" src="https://raymondhouch.com/lifehacker/digital-workflow/setapp-app-mac/embed/#?secret=6xDAhs1j6i#?secret=gnOfFR8ugW" width="600" height="585" frameborder="0"></iframe>

### OCR 文字辨識

內建 OCR 能夠辨識螢幕上任何文字內容，叫出 OCR 後，只需 **選取畫面區域，系統便會自動提取文字並複製到剪貼簿** 。

這功能在處理無法複製的文件、翻譯圖片文字、或從影片中提取資訊時極為實用，而且速度很快！

![SCR 20251016 neav 1](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251016-neav-1-2048x1336.webp)

SCR 20251016 neav 1

我在實際測試辨識中文時，經常會出現亂碼，所以若你有辨識中文的需求，我會更推薦 Mac 上最好用的截圖工具 [CleanShot](https://raymondhouch.com/lifehacker/digital-workflow/cleanshot/) ，它除了可以截圖錄影、美化截圖外，還 **支援多語系的 OCR 文字和 QRcode 辨識** 。

### Emoji 表情符號挑選器

在日常工作溝通中，emoji 已經成為不可或缺的表達工具。

無論是在 Slack 回應同事、在 Notion 標註任務，還是在社群媒體發文，我們每天都需要快速找到合適的 emoji 😆

macOS 原生的 emoji 選擇器搜尋功能不夠精準，而且不支援常見的別名或俗稱，你必須精確知道官方名稱才能搜到目標。想找「讚」的 emoji 時，輸入「good」或「棒」可能完全找不到結果。

![SCR 20251015 ogpx](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251015-ogpx-2048x1371.webp)

SCR 20251015 ogpx

Raycast 除了 **支援多種別名搜尋** ，還可以 **透過分類快速定位、用顏色篩選 emoji、或釘選（Pin）** 你最常使用的表情符號。

📍 **如何使用** ：直接在搜尋框輸入 emoji，或是像我一樣在 Raycast 的設定裡，用快捷鍵取代掉原本 Mac 預設的 `^ + *⌘* + Space` 。

![SCR 20251016 mzfg](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251016-mzfg-2048x1291.webp)

SCR 20251016 mzfg

### Count 字數統計

提供精確的文字統計功能，包括 **字數、字符數、段落數、閱讀時間** 等指標。

這個外掛讓你選取任何地方的文字後，就能在 Raycast 搜尋框輸入 Count 查詢字數，一鍵控制文章長度、評估閱讀時間，省去貼到其他工具去計算的麻煩。

對於作家、部落客、學術研究者或內容行銷人員來說極為實用！

📍 **如何使用** ：框選你要的文字後，呼叫出 Raycast 搜尋 `Count` 即可快速算出字數。

![SCR 20251015 olra](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251015-olra-2048x1401.webp)

SCR 20251015 olra

---

## QuickLinks 快速連結

簡單來說，就是 **建立個人化的快速連結捷徑** ！（包括網站、檔案、資料夾或應用程式）

📍 **如何使用** ：搜尋 `Quicklinks` 或是設定快捷鍵。

![SCR 20251015 oisn](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251015-oisn-2048x1390.webp)

SCR 20251015 oisn

例如：我設定了 Felo 的快速連結，只要輸入關鍵字，它就會自動幫我透過 Felo 來搜尋！

![SCR 20251015 ojel](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251015-ojel-2048x1346.webp)

SCR 20251015 ojel

📍 延伸閱讀： [Felo AI 最新應用：簡報生成、互動網頁、事實查核](https://raymondhouch.com/lifehacker/digital-workflow/felo-ai-search/)

---

## Snippets 常用詞語

強大的文字片段和範本管理系統，可以儲存常用的回覆、程式碼片段、表單範本、AI Prompts 等，能顯著減少重複打字的時間。

📍 **如何使用** ：搜尋 Snippets，會有 **新增、搜尋、匯入、匯出** Snippets 的功能

![SCR 20251022 qkwc](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251022-qkwc-2048x1400.webp)

SCR 20251022 qkwc

### 常用詞語

對於客服人員的標準回覆、程式設計師的程式碼範本、或業務人員的提案內容都極為實用。

![SCR 20251016 nikx](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251016-nikx-2048x1371.webp)

SCR 20251016 nikx

### Prompts

隨著 AI 工具普及，高品質的 AI 提示詞（Prompts）往往非常長，包含了詳細的背景設定與格式要求。

若每次都要重新打字或到筆記本複製貼上，超沒效率。

但透過 Raycast Snippets（常用詞語），你可以將整串上百字的提示詞完整儲存，之後只要透過一個極短的關鍵字就能呼叫，例如 `@diary` 。

之後在任何輸入框中，你只需輸入 `@diary` ，Raycast 就會立即為你貼上整段「日記助手」的提示詞，省下了大量從剪貼簿找的時間，不再被重複的輸入動作打斷思緒。

![SCR 20251022 qmhb](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251022-qmhb-2048x1397.webp)

直接從 Raycast 呼叫貼入 ChatGPT

💡 延伸閱讀

- [學會這些 Prompt 指令原則，開始活用 AI 工具（TRIRO 提問法框架）](https://raymondhouch.com/lifehacker/digital-workflow/chatgpt-prompt-engineering/)
- [推薦 7 款免費好用的中文 GPTs，不需付費也能使用 ChatGPT 打造的個人助理](https://raymondhouch.com/lifehacker/digital-workflow/free-chatgpt-gpts/)

---

## Windows Management 多視窗管理

透過簡單的快捷鍵，你可以 **快速將視窗對齊到螢幕左半邊、右半邊、四個角落，或是在多個螢幕間移動視窗** 。不需要用滑鼠拖曳，一個按鍵就搞定。

![SCR 20251015 olzf](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251015-olzf-2048x1364.webp)

SCR 20251015 olzf

Raycast Pro 會員還能使用 Custom Management，自定義視窗的精確尺寸、位置和偏移量。

想要一鍵將多個應用程式排列成理想的工作配置，同時查看程式碼、文件和終端機，或是處理設計稿時需要固定的視窗佈局，不需要再每次手動調整！

⬇️ 輸入「超級個體」時，就會左邊 Safari 視窗開啟《 [超級個體工作術](https://raymondhouch.com/solopreneur-blog) 》線上課程，右邊自動打開 Heptabase 的「學習筆記」頁面。

![raycast windows hepta solo](https://raymondhouch.com/wp-content/uploads/2025/10/raycast-windows-hepta-solo.gif)

raycast windows hepta solo

![SCR 20251015 osja](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251015-osja-2048x1379.webp)

SCR 20251015 osja

---

## Raycast 費用？需要付費 Raycast 嗎？

基本上大部分使用者用免費版就很夠了！ **如果你只需要基本的啟動器、視窗管理、剪貼簿歷史等功能，完全不需要付費** 。

只有當你需要 AI 功能、跨裝置同步、或自訂視窗管理時，才需要考慮升級 Pro。

![SCR 20251016 nlhy](https://raymondhouch.com/wp-content/uploads/2025/10/SCR-20251016-nlhy-2048x1378.webp)

SCR 20251016 nlhy

### 免費方案（Free）

包含所有內建擴充功能，如剪貼簿歷史、行事曆、視窗管理等，並可存取社群開發的公開擴充套件（1000+ 個）。

> [🔗 點此免費註冊](https://raymondhouch.com/raycast)

### Pro 方案

每月 $8 美元（年繳）

**Pro 主要功能：**

- Raycast AI（AI 助理、AI Chat）
- 雲端同步（Cloud Sync）：跨裝置同步所有設定、擴充、快捷鍵
- 自訂主題（Custom Themes）
- 無限剪貼簿歷史紀錄
- 自訂視窗管理指令（Custom Window Management Commands）

---

## 每年更新！雷蒙的數位工具地圖・提升工作效率、生產力應用推薦

![2026 tools](https://raymondhouch.com/wp-content/uploads/2025/09/2026-tools.webp)

查看課程介紹 （內有試看影片）

不論是外出講課、線上課，我最常收到的問題之一就是：「 **雷蒙，你剛剛在用的是哪個工具／APP？** 」

加上每年都有最新、更厲害的工具出現，總是讓人感到特別焦慮，尤其在 AI 技術出現後，新工具層出不窮，追也追不完。

於是我直接挑選出 **==25+ 個我親自使用過、值得推薦給你的數位工具==** ，讓你能思考自己的需求，把手機、電腦變成真正有價值的生產力工具！購買後能立即學習 2.5 小時的課程內容（ **每年持續更新** ） 🔥

—

### 💬 學員心得＆評價，看看他們上完課的收穫：

1. 品捷：「這堂課太佛心了！學到了很多超好用的工具，例如：Raycast 真的超方便！可以快速叫出工具，還有許多實用的插件。 [TextSniper](https://raymondhouch.com/lifehacker/digital-workflow/setapp-app-mac/) 幫助我一鍵快速複製圖片上的文字，不需要再使用多個工具才能完成。還有 [自然輸入法](https://raymondhouch.com/lifehacker/digital-workflow/typing-text-iqt-ai/) 、 [CleanShot](https://raymondhouch.com/lifehacker/digital-workflow/cleanshot/) 、 [Paste、Downie](https://raymondhouch.com/lifehacker/digital-workflow/setapp-app-mac/) 等等。上完這堂課，收穫滿滿，直接解鎖許多從沒用過的實用工具，跟著雷蒙的操作示範，直接快速上手！」
2. 張馨予（馨馨）：「每次上雷蒙的課都收穫滿滿，這次在課程中認識很多 APP，還找到了我一直在尋尋覓覓的 OCR 文字擷取器（Windows系統），可以安裝在公司電腦，大幅提升我工作效率。」

---

※ 內容註腳

[![](https://raymondhouch.com/wp-content/uploads/2023/02/power-by-lifehacker.png)](https://lifehacker.tw/)

[![](https://raymondhouch.com/wp-content/uploads/2023/02/Written-By-Human-Not-By-AI-Badge-white@2x.png)](https://notbyai.fyi/)

#### 關於作者

#### 🆕 最新文章

#### 留言回應

### 你可能也喜歡

[^1]: 「自然語言」 **就是我們平常講話、打字的方式** ，它讓你可以像跟朋友聊天一樣對電腦下指令。你不用輸入 `create event "Team Meeting" date:tomorrow time:10am` ，而是直接講：「明天早上十點安排一個團隊會議」，能理解自然語言也代表這套系統更人性化、更直觀。