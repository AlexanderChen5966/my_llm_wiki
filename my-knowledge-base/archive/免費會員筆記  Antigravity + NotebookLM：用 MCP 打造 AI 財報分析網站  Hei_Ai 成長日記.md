---
title: "免費會員筆記 | Antigravity + NotebookLM：用 MCP 打造 AI 財報分析網站 | Hei_Ai 成長日記"
source: "https://www.patreon.com/posts/mian-fei-hui-bi-152362061"
author:
  - "[[Hei_Ai 成長日記]]"
published: 2026-03-10
created: 2026-05-15
description: "免費會員筆記 | Antigravity + NotebookLM：用 MCP 打造 AI 財報分析網站由Hei_Ai 成長日記於Patreon平台發佈。加入Hei_Ai 成長日記社群即可獲取獨家內容及最新消息。"
tags:
  - "clippings"
---
![](https://www.youtube.com/watch?v=YRPtuEA2OzE)
![](https://www.youtube.com/watch?v=YRPtuEA2OzE)

### [Hei\_Ai 成長日記](https://www.patreon.com/Hei_Ai)

3月10日

3月10日

最近 Google 的 Antigravity 和 NotebookLM 越來越多人在用了。但是大部分人都是單獨使用其中一個，其實把它們組合起來，威力才是最大的。

今天要跟大家分享一個非常實用的組合—— **用 MCP 把 Antigravity 和 NotebookLM 連接起來，讓 AI 自動幫你做研究、生成網站、分析財報。**

以前要看 NVDA、AMD、美光三家公司的財報，你可能要打開三份 PDF，一頁一頁翻看那些密密麻麻的數字和表格，光是找數據做對比就要花幾個小時。但如果我跟你說，只需要一個提示詞，AI 就能幫你把這些財報變成一個可以互動的分析網站呢？營收、淨利潤、年增長，全部自動整理成圖表，而且數據來自真實財報，不是 AI 編出來的。

這篇筆記整理了影片的核心重點，讓你可以快速掌握整個流程和背後的原理。

### 📍 第一部分：認識兩個工具

在講怎麼結合之前，先快速介紹一下今天會用到的兩個工具。

**1\. Antigravity 是什麼？** Antigravity 是 Google 推出的 IDE（開發者工具）。但是千萬不要被這個名字嚇跑——雖然叫做「開發者工具」，但並不是只有開發者才可以使用。你可以把它理解成一個超級 AI 助手。我們只要使用自然語言告訴它想要做什麼，不管是寫 APP、建立網站，還是做出各種工具，完全不需要寫任何一行代碼。只要用文字描述你想要的東西，它就可以幫你實現。

**2\. NotebookLM 是什麼？** NotebookLM 也是 Google 的工具，但它的定位是一個研究工具。我們可以把不同的 PDF、網站、影片、音頻檔、Google Docs、Google Sheet 等等文件通通丟給它，它就會幫我們整理分析，然後根據我們給它的資料來回答問題。

它最重要的特點是：幻覺出現的機率會比其他 LLM（大語言模型）像 ChatGPT 或 Gemini 低非常多。不僅如此，它還可以根據我們給的資料幫我們生成 PPT、Podcast、影片說明等多種格式的內容。

### 📍 第二部分：為什麼要結合兩個工具？

有些人可能會問：我單獨使用 Antigravity 或者單獨使用 NotebookLM，其實好像也不錯，為什麼要結合起來？

原因是它們各有強項，但也各有明顯的弱點：

- **Antigravity 的問題：** 不管你使用 Antigravity、Claude Code 還是其他 IDE，它們都有一個很明顯的缺點——context window（記憶體）比較短。一次能記住的東西有限，如果給太多資料，它就可能會忘記之前的內容。
- **NotebookLM 的優勢：** NotebookLM 就完全不一樣了。一個 Notebook 可以放 50 個來源，一個來源可以放 50 萬字。這個容量要比任何 IDE 都大非常多。

所以比較好的用法是：

- 🧠 **NotebookLM 當「大腦」** → 負責記憶和研究（上網查資料、儲存資料）
- 🛠️ **Antigravity 當「雙手」** → 負責執行和建立東西

通過 MCP（Model Context Protocol）把兩者連接起來，Antigravity 需要什麼資料就直接到 NotebookLM 上面去抓取。這樣我們就有了一個更好的組合——大腦負責記，雙手負責做。

### 📍 第三部分：MCP 安裝教學（影片 3:58 開始）

安裝過程其實比想像中簡單，AI Agent 會全程引導你完成。

**完整步驟：**

1. 打開 Antigravity，點右上角 → Open Agent Manager → 建立一個新的 Workspace（你可以理解成一個工作項目，在同一個項目底下可以有很多不同的對話框來處理不同任務）
2. 在新的 Workspace 裡，建議先把模型從 Gemini 3.1 切換成 Claude Sonnet 4.6（Claude 在處理複雜指令和多步驟任務時表現更穩定）
3. 輸入安裝 NotebookLM MCP 的提示詞（提示詞在免費學習中心的帖文裡，加入就能拿到）
4. AI Agent 會問你使用哪一個 MCP 的客戶端，回答 Antigravity（如果你用 Claude Code 或 Cursor 也可以照回答）
5. 安裝過程中會彈出一個視窗，需要登入你的 Google 帳號進行授權
6. 授權完成後，回到 Antigravity 告訴它「已完成登入」
7. AI 會幫你設定並檢查 MCP Server 是否成功連接
8. 重新啟動 Antigravity → 右上角 → Additional Option → MCP Server → Manage MCP Server → 點 Refresh
9. 確認看到 NotebookLM 出現，並且顯示 31 個工具

> \* *💡 老師的 Tip：* \*如果重啟後沒看到 NotebookLM MCP，不要慌！回到原本的提示詞帖文，裡面有 GitHub 連結。去 GitHub 找到 NotebookLM MCP 的安裝指令（在「Two Ways To Use」那段），把指令複製起來給 Antigravity，讓它自我修正安裝路徑。修改完後再重新啟動一次就可以了。

> \* *💡 Claude Code 用戶注意：* \*如果用 Claude Code 安裝，有可能不會彈出登入視窗。這時候可以根據 AI 的指引手動取得 Cookies，複製貼上給 AI Agent，一樣可以完成安裝。

### 📍 第四部分：兩個實戰案例

**案例 1：Deep Research 深度研究（影片 8:42）** 這個案例展示了怎麼讓 Antigravity 控制 NotebookLM 做批量深度研究。

**具體操作：**

- 我先讓 Antigravity 用它基本的 Web Search 功能，搜尋網絡上 10 個最受歡迎的 AI 工具配合方向（例如 Antigravity + NotebookLM、Antigravity + NanoBanana 等等）
- 有了這 10 個方向後，我選了 5 個最感興趣的，讓 Antigravity 通過 NotebookLM MCP 建立 5 個不同的筆記本
- Antigravity 自動對每個筆記本發出 Deep Research 請求
- 這時打開 NotebookLM 就能看到，最近開啟的筆記本那裡已經出現了 5 個新的研究項目，正在自動進行
- 等了大約 5 分鐘，第一個 Deep Research 就完成了，引用了 33 個不同來源
- 全程我沒有打開過 NotebookLM，所有操作都是通過 Antigravity 完成的

而且 Antigravity 除了幫我們做 Deep Research 之外，還會自動開啟 5 個多媒體工作任務——簡報、音頻、心智圖、資訊圖、影片生成。 **一次操作，多份產出。**

**案例 2：財報分析網站（影片 12:31）** 這是今天的重頭戲——用 NotebookLM 的資料讓 Antigravity 生成互動式網站。

**具體操作：**

- 先在 NotebookLM 上傳 NVDA、AMD、美光三份最新季度財報的 PDF
- 回到 Antigravity，給它一個提示詞，告訴它讀取 NotebookLM 裡指定 Notebook 的內容，然後生成財報分析網站
- ⚠️ **重要注意：提示詞裡面的 Notebook 標題必須跟 NotebookLM 上面的標題一模一樣！** 因為 Antigravity 是通過標題去找筆記本的，名字對不上就找不到資料
- Antigravity 會先從 NotebookLM 提取所有需要的資料，然後制定製作計劃和任務清單
- 等大約 10 分鐘，一個完整的互動式財報分析網站就出來了
- 網站包含：營收對比、淨利潤走勢、年增長率、三家公司規模比較，全部都是互動式圖表
- 所有數據來自真實財報 PDF，不是 AI 幻覺或虛構的
- 最後還可以用 UiUX Pro Max 這個 Agent Skill 讓 AI 再美化一下界面，風格和顯示方式都會完全不一樣，非常專業

### 📍 第五部分：為什麼不直接用 IDE 的 Web Search？

影片裡有一個很重要的觀點： **通過 NotebookLM 的 Deep Research 來取代 Antigravity 原本的 Web Search。**

為什麼？因為 Antigravity 這些 IDE 本身的網絡搜尋功能並沒有那麼好用。如果通過 NotebookLM 代替原本的 Web Search，它可以搜尋到更齊全的資料，而且 Deep Research 一次可以引用 30+ 個來源做深度分析，品質遠超一般的搜尋結果。

### 📍 第六部分：延伸應用

這個方法不只是做財報分析。任何你在 NotebookLM 上研究過的資料，都可以通過 Antigravity 變成：

- 互動式數據分析網站
- APP 應用程式
- SaaS 工具
- PPT 簡報
- 數據視覺化報告
- 行業研究報告
- 競品分析工具

核心思路就是： **NotebookLM 當大腦負責記資料，Antigravity 當雙手負責執行。** 這種「AI 工具組合拳」的思維，才是 2026 年用 AI 的正確打開方式。從「一個工具做所有事」升級到「讓對的工具做對的事」。

如果想要做出影片中那個完整的互動式財報分析網站，完整的提示詞和詳細的操作指南在 [付費學習中心](https://www.skool.com/ai-plus-8477/about) 裡面。加入付費群就可以直接拿來使用了。付費群裡面除了這個提示詞以外，還有很多 n8n、Agent Skill 的自動化工具模板，每週都有 AI 直播教學，有問題隨時可以來找我。

---

### [Hei\_Ai 成長日記](https://www.patreon.com/Hei_Ai)

3月10日

3月10日

最近 Google 的 Antigravity 和 NotebookLM 越來越多人在用了。但是大部分人都是單獨使用其中一個，其實把它們組合起來，威力才是最大的。

今天要跟大家分享一個非常實用的組合—— **用 MCP 把 Antigravity 和 NotebookLM 連接起來，讓 AI 自動幫你做研究、生成網站、分析財報。**

以前要看 NVDA、AMD、美光三家公司的財報，你可能要打開三份 PDF，一頁一頁翻看那些密密麻麻的數字和表格，光是找數據做對比就要花幾個小時。但如果我跟你說，只需要一個提示詞，AI 就能幫你把這些財報變成一個可以互動的分析網站呢？營收、淨利潤、年增長，全部自動整理成圖表，而且數據來自真實財報，不是 AI 編出來的。

這篇筆記整理了影片的核心重點，讓你可以快速掌握整個流程和背後的原理。

### 📍 第一部分：認識兩個工具

在講怎麼結合之前，先快速介紹一下今天會用到的兩個工具。

**1\. Antigravity 是什麼？** Antigravity 是 Google 推出的 IDE（開發者工具）。但是千萬不要被這個名字嚇跑——雖然叫做「開發者工具」，但並不是只有開發者才可以使用。你可以把它理解成一個超級 AI 助手。我們只要使用自然語言告訴它想要做什麼，不管是寫 APP、建立網站，還是做出各種工具，完全不需要寫任何一行代碼。只要用文字描述你想要的東西，它就可以幫你實現。

**2\. NotebookLM 是什麼？** NotebookLM 也是 Google 的工具，但它的定位是一個研究工具。我們可以把不同的 PDF、網站、影片、音頻檔、Google Docs、Google Sheet 等等文件通通丟給它，它就會幫我們整理分析，然後根據我們給它的資料來回答問題。

它最重要的特點是：幻覺出現的機率會比其他 LLM（大語言模型）像 ChatGPT 或 Gemini 低非常多。不僅如此，它還可以根據我們給的資料幫我們生成 PPT、Podcast、影片說明等多種格式的內容。

### 📍 第二部分：為什麼要結合兩個工具？

有些人可能會問：我單獨使用 Antigravity 或者單獨使用 NotebookLM，其實好像也不錯，為什麼要結合起來？

原因是它們各有強項，但也各有明顯的弱點：

- **Antigravity 的問題：** 不管你使用 Antigravity、Claude Code 還是其他 IDE，它們都有一個很明顯的缺點——context window（記憶體）比較短。一次能記住的東西有限，如果給太多資料，它就可能會忘記之前的內容。
- **NotebookLM 的優勢：** NotebookLM 就完全不一樣了。一個 Notebook 可以放 50 個來源，一個來源可以放 50 萬字。這個容量要比任何 IDE 都大非常多。

所以比較好的用法是：

- 🧠 **NotebookLM 當「大腦」** → 負責記憶和研究（上網查資料、儲存資料）
- 🛠️ **Antigravity 當「雙手」** → 負責執行和建立東西

通過 MCP（Model Context Protocol）把兩者連接起來，Antigravity 需要什麼資料就直接到 NotebookLM 上面去抓取。這樣我們就有了一個更好的組合——大腦負責記，雙手負責做。

### 📍 第三部分：MCP 安裝教學（影片 3:58 開始）

安裝過程其實比想像中簡單，AI Agent 會全程引導你完成。

**完整步驟：**

1. 打開 Antigravity，點右上角 → Open Agent Manager → 建立一個新的 Workspace（你可以理解成一個工作項目，在同一個項目底下可以有很多不同的對話框來處理不同任務）
2. 在新的 Workspace 裡，建議先把模型從 Gemini 3.1 切換成 Claude Sonnet 4.6（Claude 在處理複雜指令和多步驟任務時表現更穩定）
3. 輸入安裝 NotebookLM MCP 的提示詞（提示詞在免費學習中心的帖文裡，加入就能拿到）
4. AI Agent 會問你使用哪一個 MCP 的客戶端，回答 Antigravity（如果你用 Claude Code 或 Cursor 也可以照回答）
5. 安裝過程中會彈出一個視窗，需要登入你的 Google 帳號進行授權
6. 授權完成後，回到 Antigravity 告訴它「已完成登入」
7. AI 會幫你設定並檢查 MCP Server 是否成功連接
8. 重新啟動 Antigravity → 右上角 → Additional Option → MCP Server → Manage MCP Server → 點 Refresh
9. 確認看到 NotebookLM 出現，並且顯示 31 個工具

> \* *💡 老師的 Tip：* \*如果重啟後沒看到 NotebookLM MCP，不要慌！回到原本的提示詞帖文，裡面有 GitHub 連結。去 GitHub 找到 NotebookLM MCP 的安裝指令（在「Two Ways To Use」那段），把指令複製起來給 Antigravity，讓它自我修正安裝路徑。修改完後再重新啟動一次就可以了。

> \* *💡 Claude Code 用戶注意：* \*如果用 Claude Code 安裝，有可能不會彈出登入視窗。這時候可以根據 AI 的指引手動取得 Cookies，複製貼上給 AI Agent，一樣可以完成安裝。

### 📍 第四部分：兩個實戰案例

**案例 1：Deep Research 深度研究（影片 8:42）** 這個案例展示了怎麼讓 Antigravity 控制 NotebookLM 做批量深度研究。

**具體操作：**

- 我先讓 Antigravity 用它基本的 Web Search 功能，搜尋網絡上 10 個最受歡迎的 AI 工具配合方向（例如 Antigravity + NotebookLM、Antigravity + NanoBanana 等等）
- 有了這 10 個方向後，我選了 5 個最感興趣的，讓 Antigravity 通過 NotebookLM MCP 建立 5 個不同的筆記本
- Antigravity 自動對每個筆記本發出 Deep Research 請求
- 這時打開 NotebookLM 就能看到，最近開啟的筆記本那裡已經出現了 5 個新的研究項目，正在自動進行
- 等了大約 5 分鐘，第一個 Deep Research 就完成了，引用了 33 個不同來源
- 全程我沒有打開過 NotebookLM，所有操作都是通過 Antigravity 完成的

而且 Antigravity 除了幫我們做 Deep Research 之外，還會自動開啟 5 個多媒體工作任務——簡報、音頻、心智圖、資訊圖、影片生成。 **一次操作，多份產出。**

**案例 2：財報分析網站（影片 12:31）** 這是今天的重頭戲——用 NotebookLM 的資料讓 Antigravity 生成互動式網站。

**具體操作：**

- 先在 NotebookLM 上傳 NVDA、AMD、美光三份最新季度財報的 PDF
- 回到 Antigravity，給它一個提示詞，告訴它讀取 NotebookLM 裡指定 Notebook 的內容，然後生成財報分析網站
- ⚠️ **重要注意：提示詞裡面的 Notebook 標題必須跟 NotebookLM 上面的標題一模一樣！** 因為 Antigravity 是通過標題去找筆記本的，名字對不上就找不到資料
- Antigravity 會先從 NotebookLM 提取所有需要的資料，然後制定製作計劃和任務清單
- 等大約 10 分鐘，一個完整的互動式財報分析網站就出來了
- 網站包含：營收對比、淨利潤走勢、年增長率、三家公司規模比較，全部都是互動式圖表
- 所有數據來自真實財報 PDF，不是 AI 幻覺或虛構的
- 最後還可以用 UiUX Pro Max 這個 Agent Skill 讓 AI 再美化一下界面，風格和顯示方式都會完全不一樣，非常專業

### 📍 第五部分：為什麼不直接用 IDE 的 Web Search？

影片裡有一個很重要的觀點： **通過 NotebookLM 的 Deep Research 來取代 Antigravity 原本的 Web Search。**

為什麼？因為 Antigravity 這些 IDE 本身的網絡搜尋功能並沒有那麼好用。如果通過 NotebookLM 代替原本的 Web Search，它可以搜尋到更齊全的資料，而且 Deep Research 一次可以引用 30+ 個來源做深度分析，品質遠超一般的搜尋結果。

### 📍 第六部分：延伸應用

這個方法不只是做財報分析。任何你在 NotebookLM 上研究過的資料，都可以通過 Antigravity 變成：

- 互動式數據分析網站
- APP 應用程式
- SaaS 工具
- PPT 簡報
- 數據視覺化報告
- 行業研究報告
- 競品分析工具

核心思路就是： **NotebookLM 當大腦負責記資料，Antigravity 當雙手負責執行。** 這種「AI 工具組合拳」的思維，才是 2026 年用 AI 的正確打開方式。從「一個工具做所有事」升級到「讓對的工具做對的事」。

如果想要做出影片中那個完整的互動式財報分析網站，完整的提示詞和詳細的操作指南在 [付費學習中心](https://www.skool.com/ai-plus-8477/about) 裡面。加入付費群就可以直接拿來使用了。付費群裡面除了這個提示詞以外，還有很多 n8n、Agent Skill 的自動化工具模板，每週都有 AI 直播教學，有問題隨時可以來找我。

---

隨時隨地觀看你喜愛的影片。## [相關文章](https://www.patreon.com/cw/Hei_Ai/posts)你知道 Firefly Custom Models 可以學你的風格生圖，但具體怎麼做才能訓練出一個「真的能用」的模型？ 大部分人的做法是：隨便丟十幾張圖上去，等 AI 訓練完，結果出來的東西根本不像。 問題不是工具不行，是你的準備工作沒做對。 今天這篇教你從頭到尾走完整個流程，而

已鎖定

### 付費會員筆記 | Adobe Firefly Custom Models — 從零開始訓練你的品牌 AI 生圖模型

你知道 Firefly Custom Models 可以學你的風格生圖，但具體怎麼做才能訓練出一個「真的能用」的模型？ 大部分人的做法是：隨便丟十幾張圖上去，等 AI 訓練完，結果出來的東西根本不像。 問題不是工具不行，是你的準備工作沒做對。 今天這篇教你從頭到尾走完整個流程，而

3月21日

「以前寫東西，腦袋裡會先有框架……」 「以前遇到問題，我會自己先想辦法……」 「現在？打開 ChatGPT 丟一句話就好了……」 如果這聽起來很熟悉，那你可能正在經歷一個有科學名字的現象： Cognitive Offloading — 認知卸載 而它的後果，比你想的嚴重得多。 🔬 研究數據到底說了什麼？ 1️⃣ 醫師用了 AI 之後，沒有 AI 反而更差 The Lancet 2025 年發表了一份多中心研究，對象是 19 位經驗豐富的內視鏡醫師，每人做過超過 2,000 次大腸鏡。他們用了 AI 輔助偵測息肉一段時間後，研究人員把 AI 關掉。 結果： 腺瘤檢測率從 28.4% 暴跌到 22.4%。不是回到原本水平，是比「從來沒用過 AI 之前」還差。 這是醫學界第一次正式記錄到的 AI「去技能化」效應。 2️⃣ AI 用得越多，批判性思維越差 一份涵蓋 666 人的研究發現： ✅ AI 使用頻率越高 ❌ 批判性思維測試分數越低 研究者確認了關鍵中介因素：不是工具本身有問題，而是你把「思考」這件事委託出去了。 3️⃣ 用 AI 寫完東西，幾分鐘後就忘了 MIT 研究發現，使用 ChatGPT 的參與者，記憶和創意相關的神經連接出現減弱。甚至剛寫完的東西，過幾分鐘就想不起來自己寫了什麼。 因為那些內容不是從你的腦袋裡生出來的，你只是當了一次搬運工。 🧩 為什麼我們會掉進這個陷阱？ 因為認知卸載太舒服了。想想你的工作日常： 📝 寫文章 以前： 先想框架、查資料、組織邏輯、反覆修改 現在： 丟一句 prompt，AI 寫完，看一遍覺得還行就用了 📊 做決策 以前： 分析數據、權衡利弊、跟同事討論 現在： 問 AI，它給你三個選項，你挑一個 🐛 解問題 以前： 一行一行看代碼，理解邏輯，找出根因 現在： 把錯誤訊息貼給 AI，它給你答案，你甚至不知道為什麼這樣修 每一次你跳過「自己想」這個步驟，你的認知肌肉就萎縮一點。 ACM 2025 把這叫做 "The AI Deskilling Paradox" 。短期效率的提升，正在引入更深層的長期問題。 而且研究特別指出： 初級員工和年輕人風險最大 。因為他們從入行第一天就有 AI，根本沒有機會建立自己的判斷力基礎。 💡 那 AI 到底該怎麼用才對？ 核心原則只有一個： AI 是放大器，不是替代品。 你要先有東西讓它放大，它才有價值。 ✅ 先自己想，再給 AI 寫文章前先花 2 分鐘列大綱。 做決策前先寫下你的判斷和理由。 讓 AI 優化你的想法，而不是替你產生想法。「從零生成」和「優化已有想法」是完全不同的腦力活動。 ✅ Plan → Produce → Check → Compare 自己先做一版（60 分就好）→ AI 也做一版 → 對比差異。 重點不是誰的更好，是對比的過程讓你看到自己的盲點。建議每週挑 2-3 個重要任務做一次。 ✅ 定期「無 AI 日」 已經有公司在推 Manual Monday — 一週一天核心工作不用 AI。 目的不是折磨自己，是讓你每週感受一次「沒有 AI 的自己」是什麼水平。知道哪裡在退化，才知道該加強什麼。 ❌ 不該做的： 把 AI 的第一個回答當最終答案。 讓 AI 處理你「不懂但應該懂」的工作。 從來不驗證 AI 的輸出。 🎯 AI 是拐杖還是義肢，取決於你怎麼用 拐杖幫你走得更穩，但你的腿還在。 義肢取代了你的腿，拿掉它你就站不起來。 在這個人人都在追求效率的時代，真正的競爭力不是「誰用 AI 用得更多」，而是\* 「拿掉 AI 之後，誰還能站得住」 \*。 別讓效率殺死能力。 你最依賴 AI 做的工作是什麼？如果明天 AI 全部關掉，你覺得自己的工作能力還剩幾成？ 歡迎留言分享 👇

### 免費會員筆記 | 你有沒有發現，自從開始用 AI 之後，有些事情你反而不太會做了？

2月26日

![](https://c10.patreonusercontent.com/4/patreon-media/p/post/153149659/014f9edf59bf4a31a4af137a35ab50e0/eyJ3Ijo2NDB9/1.png?token-hash=EIh89jD0qrjZP2ToGV5OcAmMZwZYQXmcgstiFeocACg%3D&token-time=1780099200)

### 免費會員筆記 | 你的 Agent Skill 寫了好幾天還在調？讓 Skill 自己優化自己！

3月16日

![](https://c10.patreonusercontent.com/4/patreon-media/p/post/156952549/32342dd520f846a4837efc8329eebb0a/eyJ3Ijo2NDB9/1.png?token-hash=i9SkZepsQDeLijznYQjUcF9ZnppoQ4a0KRurKBbfRBE%3D&token-time=1780099200)

### 為什麼你裝了一堆 Claude Skill，產出還是「不太對」？

4月30日## [最受歡迎的文章](https://www.patreon.com/cw/Hei_Ai/posts)![](https://c10.patreonusercontent.com/4/patreon-media/p/post/140647118/55870b351c5d4bc6bc132f6a1471e790/eyJ3Ijo2NDB9/1.png?token-hash=XwD_ygPXuJW2TNjAjVb5VshPPwnOLfdpHAxIJWBrLt0%3D&token-time=1780099200)

已鎖定

### 完整AI 新手入門課程｜研究・整合・摘要 一次搞定

TWD:$999 含課程影片 + 可下載 PPT + 提示詞模板 + 兩場直播 Q&A 為什麼這堂課能讓你「真的用起來」？ 多數人被工具淹沒、被提示詞嚇到，不知道從哪開始。 本課用最短學習路徑帶你上手 4 大必備工具：ChatGPT、Gemini、NotebookLM、Gamma

2025年10月7日

8

![](https://www.patreon.com/media-u/v3/591762910)

YouTube

### 免費會員筆記 | 【n8n - AI 自動化保姆級教程】 從 0 到 1 建立你的 AI 自動化工作流 (JSON、API、節點邏輯一次搞懂)

1月6日

3

Hei.Ai 成長日記的夥伴們，大家好！ 這篇是我「NotebookLM 專題影片」的文字版。筆記的目的是把你手上的零散資料（YouTube、PDF、網站、音訊）在 5 分鐘內變成有結論、能追溯來源、可收聽的摘要。 不論你是學生、上班族還是內容創作者，只要你平常要整理資訊，Not

已鎖定

### 免費會員筆記｜NotebookLM 一次上手：5 分鐘把影片、PDF、問卷變成「會回答的筆記」（含可複製Prompt）

Hei.Ai 成長日記的夥伴們，大家好！ 這篇是我「NotebookLM 專題影片」的文字版。筆記的目的是把你手上的零散資料（YouTube、PDF、網站、音訊）在 5 分鐘內變成有結論、能追溯來源、可收聽的摘要。 不論你是學生、上班族還是內容創作者，只要你平常要整理資訊，Not

2025年9月16日

8

![](https://c10.patreonusercontent.com/4/patreon-media/p/post/149214097/e6c38a07c8c040549690918ccda5631b/eyJ3Ijo2NDB9/1.png?token-hash=xeJ7AMOegq1qUls6pi1UWh_VPH3S_FV0Nc_X1-xwaY0%3D&token-time=1780099200)

### 🤯 你是不是也有這種「AI 焦慮」？

1月27日

2