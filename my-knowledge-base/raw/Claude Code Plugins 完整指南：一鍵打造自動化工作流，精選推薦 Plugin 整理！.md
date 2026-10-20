---
title: "Claude Code Plugins 完整指南：一鍵打造自動化工作流，精選推薦 Plugin 整理！"
source: "https://frankknow.com/claude-code-plugins/"
author:
  - "[[犬哥網站 - Claire]]"
published: 2026-05-14
created: 2026-05-15
description: "Claude Code Plugins 是專為 Claude Code 開發環境設計的擴充系統，可以一次打包多種擴充元件（MCP、Skills、Agents、Hooks 等），讓原本只能在終端機 / IDE 進行對話、編寫程式碼的 AI，進一步連接外部工具、資料來源與自動化流程，處理更多任務。這篇文章會帶你看懂 Plugin 可以附加哪些能力、以及如何安裝這個強大的外掛技能包。"
tags:
  - "clippings"
status: processed
---
Claude Code Plugins 能 ==讓原本只能在終端機 / IDE 進行對話、編寫程式碼的 AI，進一步 **連接外部工具、資料來源與自動化流程**== ，處理更多原本做不到的任務。

如果你想用 [AI Agent](https://frankknow.com/what-are-ai-agents/) 打造一個完整工作流，那一定要懂 Plugin 可以附加哪些能力、以及如何安裝這個強大的外掛技能包！

不管你是工程師還是完全沒有技術背景的使用者，都可以照著這篇文章的步驟更了解 Plugins，讓你的 [Claude Code](https://frankknow.com/claude-ai/) 從基本款升級成真正的超級助手。

另外我們還整理了前後端開發、行銷與專案管理等，各領域的好用 Plugins 選擇，一起往下看吧：）

## Claude Code Plugins 是什麼？

**Claude Code Plugins** 是專為 Claude Code 開發環境設計的 **擴充系統** ， ==可以一次打包多種擴充元件（MCP、Skills、Agents、Hooks 等），一鍵安裝就能使用== 。

### Plugins 的主要概念

之前在 [《 Claude Skills 教學 》](https://frankknow.com/claude-skills/) 有詳細說明 Skill 是如何讓工作流程可以被複製、沿用，乍看之下很難區分 Plugin 有什麼不同。

其實 Skills 只是 Plugins 的其中一個元件，如果說 ==Skills 是「 一份寫給 Claude 看的工作手冊 」，那麼 Plugins 就是「 一整個裝備齊全的工具箱 」== ，內容涵蓋範圍更廣。

Plugins 通常會結合了設定檔、代碼和文檔，以標準化的檔案目錄結構呈現。

### Plugins 能做什麼？

安裝 Plugin 之後，Claude Code 能處理的事情，就不只是基本對話和 Claude 本身的模型能力了。以下是幾個最常見的應用方向：

![安裝 Plugin 能讓 Claude Code 擴充更多能力](https://frankknow.com/wp-content/uploads/2026/05/Whats-Plugins-ability.jpg)

安裝 Plugin 能讓 Claude Code 擴充更多能力

- **==連接外部服務==**

透過 Plugin，Claude Code 可以直接串接你日常使用的工具，例如：GitHub、Jira、Slack、Figma 等。

不需要手動切換視窗、複製貼上，Claude Code 就能直接讀取和操作這些平台上的資料。

你可以讓它自動從 GitHub 拉取最新的資料、在 Slack 發送任務完成通知，或是直接讀取 Figma 的設計規格來產出對應的程式碼。

- **==提供語言智慧==**

針對不同的程式語言，有些 Plugin 內建了 LSP（語言伺服器協定） 支援，讓 Claude Code 能更精準理解 TypeScript、Python、Go 等語言的語法邏輯。

在協助你開發時，提供更正確、更符合語言規範的修改建議。

- **==自動化開發流程==**

有了對應的 Plugin，Claude Code 可以自動執行 **Code Review、部署流程、安全掃描** 等工作，不需要每次手動觸發，大幅減少重複性的操作步驟。

- **==讓非工程師也能一鍵啟用專業工具==**

以前開發這些輔助功能，通常需要工程師花時間自己設定、串接，對非技術背景的人來說是門檻很高的任務。

但有了 Plugin，整個設定流程被大幅簡化，即使你完全沒有工程背景，也能一鍵啟用專業級的開發輔助工具，讓 Claude Code 直接幫你把流程跑起來。

### 客製網站 × AI 自動化：打造真正屬於你的高性能網站

Vibe Coding 搭配 Plugins，確實可以讓你在網站管理和開發流程上省下很多力氣，但 ==如果你的網站是架在租用平台上，AI 工具能發揮的空間，其實非常有限== 。

很多租用型平台對後端架構、資料庫存取、自動化流程都有一定程度的限制，就算你把 Plugins 設定得再完整，遇到平台本身的牆，一樣會被卡住，可控性也難以提升。

犬哥網站的客製化網頁設計服務，全面採用 WordPress 開發，讓你對網站架構、資料庫、部署流程都有完整的掌控權，不受平台限制。

![犬哥網站設計案例 ——「 Syncrobotic 星科國際 」
規劃清晰架構與視覺層次，聚焦產品技術與應用展示](https://frankknow.com/wp-content/uploads/2026/05/Web-Design-Showcase-of-SYNCROBOTIC.jpg)

犬哥網站設計案例 ——「 Syncrobotic 星科國際 」 規劃清晰架構與視覺層次，聚焦產品技術與應用展示

以犬哥網站與 **「 Syncrobotic 星科國際 」** 的合作為例，這是一家以技術應用為核心的科技品牌，在規劃網站時就把清晰完整的架構納入考量。

整體網站採用模組化架構設計， ==讓每個服務區塊、應用場域都是獨立的內容，方便日後透過 AI 工具批次更新或自動生成對應內容== ，不需要每次改版都動到整個頁面結構。

如果你也希望網站有更高的自主性，把 AI 工具的靈活性發揮出來，歡迎跟我們聊聊 [**客製化「 網頁設計 」服務**](https://frankknow.com/web-design-service/) ，幫你完整規劃 **網站架構、** [**SEO & GEO 優化**](https://frankknow.com/seo-service/) ！

[**免費諮詢犬哥網站，立即前往！**](https://frankknow.com/contact/)

---

## Plugins 包含哪些元件？

一個 Plugin 可以裝進最多六種不同類型的元件，每種元件各自負責不同的工作，有的 Plugin 只包含其中一兩種，也有的 Plugin 會把全部都打包進去，讓你一次安裝就全部到位。

![Plugins 包含六種不同類型的元件，這些元件都有各自負責的能力](https://frankknow.com/wp-content/uploads/2026/05/plugin-components.jpg)

Plugins 包含六種不同類型的元件，這些元件都有各自負責的能力

### 一、Skills（技能指令）

Skills 是最基礎、也最常見的 Plugin 元件，它的概念是「 把一套 SOP 打包成一個可以隨時呼叫的指令 」。

==透過 Skills，你可以將複雜的提示詞（Prompt）或重複性的任務封裝成簡短的指令，大幅提升操作效率== 。

**Skills 的觸發方式有兩種** ，一種是你 ==手動輸入 /指令名稱== ，或是 ==Claude 根據當下的任務情境，自動判斷並呼叫對應的 Skill== 。

比如說你裝了一個 Code Review Plugin，裡面內建「 /pr-review 」的 Skill，只要輸入這個指令，Claude 就會按照 Plugin 裡設定好的審查標準，自動完成整個 Code Review 流程。

想深入了解 Skills 的運作邏輯和完整用法，可以參考這篇 [《 Claude Skills 完整教學 》](https://frankknow.com/claude-skills/) 。

### 二、Agents（子代理人）

**Agents（子代理人）** 是針對特定任務設計獨立的 AI subagents，每一個 Agent 都有自己專屬的角色定位和能力範圍。

==當主 Claude 判斷某個任務超出自己的處理範圍，或是需要更專業的判斷時，就會把這段工作轉交給對應的 Agent 處理== 。

觸發方式一樣有兩種：Claude 可以根據情境自動判斷是否呼叫，你也可以在對話中手動指定要使用哪個 Agent。

每個 subagent 在自己的上下文視窗中執行，具有自訂系統提示、特定工具存取和獨立權限。延伸閱讀： [《 什麼是上下文視窗？ 》](https://frankknow.com/ai-token/#ftoc-heading-6)

Subagent 獨立工作後，會在主視窗回傳任務結果。

> 2026 最新功能補充：現在透過 Claude Code 的 **Agent View** 儀表板介面，你可以在單一畫面上，同時監控、管理多個在背景獨立運作的子代理人，不需要再切換多個終端機分頁了。

### 三、Hooks（事件觸發器）

Hooks 是讓 Claude Code 「 在特定時機自動做某件事 」的元件。

它會在 Claude Code 的工作流程裡設置關卡， ==當某個事件發生時，Hooks 就會自動偵測並執行你預先設定好的動作，不需要手動觸發== 。

比如設定好 Hooks，讓 Claude 在每次 **存檔後自動修正格式、提交程式碼前自動檢查、任務完成後發送通知** 。

把原本重複的手動工作變成自動化流程，減少人為疏漏的可能。

### 四、MCP Servers（串接外部服務）

MCP Servers 負責 ==打通 Claude Code 和外部世界之間的界線，讓 Claude Code 可以直接和外部 API 或服務溝通== 。

以往要讓 Claude Code 存取 GitHub、Slack、資料庫或 Figma，需要自己手動設定每個服務的串接方式。

Plugin 裡的 MCP Server 都已經把這些設定都打包好了，只要啟用 Plugin，MCP Server 就會自動啟動，對應的工具也會直接出現在 Claude 的工具列表裡。

目前常見的 MCP 整合包含 GitHub、Jira、Slack、Figma、各種資料庫等，涵蓋了非常多開發流程中常用的主要工具。

### 五、LSP Servers（語言智慧伺服器）

LSP Servers 讓 Claude Code 在開發過程中即時提供語言輔助，添加自動補全、跳到定義或滑鼠懸停顯示文件等功能。

==安裝 LSP Server 之後，Claude Code 就能提供即時程式碼分析，在每次編輯後立刻偵測語法錯誤，不用等到執行才發現== 。

目前 LSP Servers 支援的程式語言包含：C/C++、Python、TypeScript、Java、Rust、Go、Java、C#、PHP 等，基本上絕大多數開發需求都能被滿足。

不過 LSP Plugin 只負責設定 Claude Code 和語言伺服器之間的連線方式， ==語言伺服器本身的執行檔需要另外安裝，安裝完成後才能正常運作== 。

### 六、Monitors（背景監視器）

Monitors 是在背景持續運行的監控元件， ==負責觀察系統狀態並即時把資訊回傳給 Claude，讓 AI 能根據系統環境的變化做出調整== 。

你可以利用它偵測伺服器或自動化工作是否異常，或是監測某些條件符合時自動觸發後續任務。

Plugin 啟用後，Monitors 就會在工作階段開始時自動啟動，不需要額外操作，讓你不用一直盯著終端機，系統有狀況 Claude 會主動告訴你。

這個設定不只便利，也因為 Monitors 是在本地端運作，還能幫助 Claude 節省不少 [Token](https://frankknow.com/ai-token/) 資源。

### Plugins 的完整目錄長什麼樣子？

一個 Plugin 完整的結構會包含上面的六個元件，核心是「 plugin.json 」這一個 JSON 設定檔，其他元件則各自對應不同格式。

![Claude Code Plugins 完整的目錄結構](https://frankknow.com/wp-content/uploads/2026/05/Plugin-General-Structure.jpg)

Claude Code Plugins 完整的目錄結構

==其中的 **「.claude-plugin/ 」** 是唯一必要的資料夾，記錄了這個 Plugin 的基本資訊== ，Claude Code 安裝時會優先讀取這份檔案，再把其他元件部署到對應位置。

---

## 如何安裝和設定 Claude Code Plugins？

### 安裝前準備

在安裝任何 Plugin 之前，需要先確認兩件事：

1. 確認你的電腦已經安裝了 Claude Code CLI。
2. 已訂閱 Claude Pro 以上的付費方案、或 API 方案，Claude Code 才能正常運作。

如果你還沒有完成這些前置設定，可以參考這篇 [《 Claude Code 零基礎教學 》](https://frankknow.com/claude-code/#installation-and-interface) ，裡面有完整的安裝流程和介面介紹，照著步驟設定好之後再回來繼續。

### 安裝流程

Anthropic 官方提供了一個 **Marketplaces（應用程式市場）** ，讓你能夠快速瀏覽並安裝由社群或官方開發的 Plugin。

![你可以透過 Marketplaces 找到適合的 Claude Code Plugin](https://frankknow.com/wp-content/uploads/2026/05/claude-marketplace.jpg)

你可以透過 Marketplaces 找到適合的 Claude Code Plugin

你可以在網頁上的 [官方目錄](https://claude.com/plugins) ，瀏覽各類型 Plugin 的詳細功能。

![在 Claude Code 中安裝 Plugins](https://frankknow.com/wp-content/uploads/2026/05/install-plugins.jpg)

在 Claude Code 中安裝 Plugins

或是直接到 Claude Code 中，輸入指令「 /plugins 」，就會出現 Manage Plugins 的視窗，一樣可以在這裡搜尋你想使用的 Plugins。

點擊「 Install 」，並選擇系統可使用範圍的權限後，Claude Code 就會自動幫你安裝對應的 Plugin 了。

### 製作自訂 Plugins

如果現有的 Plugin 沒有符合你需求的選項，你也可以自己從頭製作一個。

Claude Code 支援兩種添加自訂 Skills、Agents、Hooks 的方法，可以根據使用情境選擇最適合的方式：

- **==Standalone（獨立設定）：==** 直接在專案目錄或個人設定裡新增 Skills、Agents、Hooks，適合只用在特定專案、不需要跨專案分享的情況。
- **==Plugins（打包分享）：==** 把所有元件整理成一個完整的 Plugin 資料夾結構，適合想在多個專案重複使用，或是想分享給團隊成員的情況。

製作自訂 Plugin 的完整步驟，包含資料夾結構怎麼建立、plugin.json 怎麼設定、各元件怎麼寫，都可以 ==參考 [Claude Code 官方文件](https://code.claude.com/docs/en/plugins) 的詳細說明== 。

如果覺得你製作的 Plugin 對其他人也有幫助，可以 ==把它 [投稿](https://claude.ai/settings/plugins/submit) 到 Claude Code 官方 Plugin Marketplace== ！

審核通過後就會出現在公開目錄裡，讓全球的 Claude Code 使用者都能安裝使用。

---

## 官方推薦 Plugins：各領域的實戰應用

Plugin 生態系的涵蓋範圍很廣，幾乎各種工作情境都有對應的選擇。

接下來，我們整理了 ==多個 **各領域熱門的 Plugins** ，和它們的實際用途== ，方便你快速找到最符合需求的工具：

### 前端開發

對於網頁設計師與工程師來說，在 Vibe Coding 時常常會遇到設計稿轉換成程式碼時有落差，或是 UI 缺乏設計感等問題。

- **==Frontend Design：==** 幫助生成具有設計感的前端程式碼，能有效避免千篇一律、生硬的「AI 風格」，讓成品更貼近現代審美。
- **==Figma：==** 對前端開發者來說很方便！Claude 可以直接讀取 Figma 設計稿，並轉換成高品質程式碼，大幅縮短從設計到開發的距離。

### Code Review 與品質

程式碼不只要能跑，還要安全且易於維護，下面這些 Plugins 就是針對高品質程式碼設計，能在開發過程中即時提供專業建議。

- **==Code Review：==** 內建多個專業審查代理人，以「 信心分數 」進行過濾，讓你快速判斷哪些是高優先度的問題、哪些只是建議性的調整。
- ==**Code Simplifier：**== 在修改程式碼後自動進行精煉與簡化，讓邏輯更清晰、降低日後的維護難度，特別適合長期執行的專案。
- **==Security Guidance：==** 當你在編輯檔案時，它會即時警告如 Command Injection 或 XSS 等常見資安風險，在撰寫程式碼的過程中就即時攔截。

### 文件與知識管理

AI 最怕資訊過期或斷片，透過這些 Plugins，可以讓 Claude 隨時保持在最新狀態。

- **==Claude.md Management：==** 負責管理與優化 CLAUDE.md 檔案，記錄每個工作階段的學習與專案規範，確保 AI 的「 專案記憶 」始終處於最新狀態，不會隨著對話變長而遺忘細節。
- ==**Notion：**== 如果你的團隊用 Notion 管理文件和知識庫，它能直接在 Claude Code 裡搜尋頁面、建立、更新文件以及管理資料庫，不需要切換視窗就完成常用操作。

### 行銷與專案管理

- **==Postiz：==** 社群行銷常需要跨平台發文，它支援超過 28 個平台（X、LinkedIn、YouTube、Instagram 等），可以直接在介面中排程發文、管理媒體並追蹤數據。
- **==Asana：==** 它整合了30 多種專案和任務管理工具，適合 PM 兼顧開發進度和專案管理，快速建立任務、更新任務指派對象並追蹤進度。
- **==Slack：==** 讓 Claude Code 能直接讀取 Slack 訊息，並在需要時把開發進度、錯誤報告或任務完成通知，自動同步到對應的團隊頻道。
- **==Firecrawl：==** 它能將任何網站轉成乾淨的 Markdown 格式，方便 Claude 直接讀取和分析，甚至能透過 AI Agent 模式單頁抓取、整站爬取，自動幫你蒐集資料。
- **==Circleback：==** 它能搜尋並存取會議紀錄、Email 與行事曆，將過去的對話紀錄轉化為可隨時查詢的知識庫，隨時讓 Claude 回顧某次會議的細節或決策。

---

## 常見問題

### Claude Code Plugins 是免費的嗎？

Plugin 本身大多數是 ==免費安裝== 的， ==但使用 Claude Code 需要有 Anthropic 訂閱方案或 API 額度== 。

部分第三方 Plugin 如果串接了付費的外部服務（例如 Firecrawl、Postiz），使用時可能需要另外準備該服務的帳號或 API 金鑰。

### Plugin 和 MCP Server 有什麼差別？

MCP Server 是一種可以跨平台使用的外部工具串接協定，Plugin 則是專為 Claude Code 設計的擴充套件。

==Plugins 除了可以內建 MCP Server 之外，還能同時打包 Skills、Agents、Hooks 等 Claude Code 專屬的元件，是更完整的功能組合包。==

### 一次可以安裝幾個 Plugins？

目前沒有明確的數量上限，但每個 Plugin 都會在對話開始時佔用一定的 Token，安裝的 Plugin 越多、裡面的元件越豐富，每次對話的 Token 消耗就越高。

==建議只安裝真正會用到的 Plugin，避免不必要的額度浪費== 。

---