---
title: "ChatGPT Plugins 2.0 完整指南：Extensions、Creator、Sites 與 MCP Events 怎麼用？"
source: "https://www.aiposthub.com/chatgpt-plugins-extensions-sites-mcp-events/"
author:
  - "[[Philo]]"
published: 2026-09-30
created: 2026-09-30
description: "ChatGPT Plugins 2.0 完整解析：整理 Plugin Extensions、Creator、送審與探索、Sites、MCP Events、Shareable profiles，並說明開發、權限與分發策略。"
tags:
  - "clippings"
status: processed
---
OpenAI 在 DevDay 2026 重新定義 ChatGPT Plugins。新版外掛不只在對話中呼叫工具，還能在側邊欄擁有固定入口、建立互動面板與自訂檔案檢視器。Plugin Creator、送審、目錄探索、Sites、MCP Events 與可分享個人檔案，也一起構成新的開發與分發流程。

延伸閱讀： [OpenAI DevDay 2026 完整整理：25 項更新、5 條產品主線與真正值得注意的轉向](https://www.aiposthub.com/openai-devday-2026-all-announcements/)

這次更新和早期 Plugins 最大的差異，是外掛開始擁有介面、事件與持續入口。開發者能把服務做成 ChatGPT 內的原生體驗，而非只有模型在背景呼叫一個接口。使用者也能在 Sites 中加入外掛，讓它使用自己的資料與權限完成工作。

本文會整理六個部分如何配合、適合開發哪些產品、權限與審核要注意什麼。我的判斷是，Plugins 2.0 比舊版更像一個應用平台，但能否成功取決於探索品質、資料邊界與開發者能否建立可持續商業模式。

![ChatGPT Plugin Extensions 官方介面，側邊欄顯示 Canva 外掛](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/plugin-extensions.webp)

ChatGPT Plugin Extensions 官方介面，側邊欄顯示 Canva 外掛

*Plugin Extensions 讓外掛擁有固定入口、互動面板與自訂檔案體驗。圖片來源：* [*OpenAI DevDay 2026 官方回顧*](https://openai.com/index/devday-2026-recap/?ref=aiposthub.com) *。*

## Plugin Extensions 多了什麼？

Extensions 把外掛從工具呼叫提升為介面元件。它可在 ChatGPT 側邊欄有固定入口，使用者不必每次先在對話裡說明要叫用哪個服務。對高頻工作而言，固定入口能降低尋找與啟動成本。

互動面板讓外掛呈現表單、狀態、選項與結果。複雜任務若只靠文字對話，很容易漏掉必要欄位，也難以比較多個選項。結構化介面可以讓使用者清楚知道將提交什麼資料、下一步會做什麼。

自訂檔案檢視器則讓特定格式直接在 ChatGPT 中呈現。設計、資料、影音或專業文件不必全部被轉成純文字。這能保留產品特色，也讓使用者在同一工作區完成查看、討論與修改。

介面能力越多，設計責任越大。外掛必須清楚標示哪些內容來自第三方、哪些動作會改變外部資料，以及誰負責結果。不能因為畫面在 ChatGPT 裡，就讓使用者誤以為所有功能都由 OpenAI 提供或保證。

## Plugin Creator、送審與探索

Plugin Creator 降低建立外掛的門檻。開發者可描述功能、連接工具與調整介面，再進入測試與送審。這能讓更多非大型團隊嘗試，但低門檻也可能帶來品質差異。

OpenAI 同時改善送審回饋、目錄排序與推薦。對開發者而言，送審需要明確知道被拒絕原因與修改方法。對使用者而言，目錄要能分辨可信、常用與適合目前任務的外掛。

![Plugin 建立、送審與探索官方視覺，顯示 Figma、Shopify 與外掛卡片](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/plugin-creation.webp)

Plugin 建立、送審與探索官方視覺，顯示 Figma、Shopify 與外掛卡片

*新版流程希望把建立、審核與被使用者發現串成一條路徑。圖片來源：* [*OpenAI DevDay 2026 官方回顧*](https://openai.com/index/devday-2026-recap/?ref=aiposthub.com) *。*

推薦機制將直接影響生態。若排序只獎勵點擊，大量包裝相似的外掛可能淹沒有用產品。較健康的指標應包含任務完成率、留存、使用者回報、權限合理性與安全紀錄，而非只看安裝量。

## Sites 可以安裝 Plugins

OpenAI Sites 是建立與分享互動網站的工具。DevDay 更新讓支援的 ChatGPT Plugins 能加入 Sites，使用者以自己的資料與權限操作。官方列出的方案包含 Business、Enterprise、Healthcare 與 Edu。

這使外掛能從 ChatGPT 對話走向特定用途網站。例如團隊可建立內部採購入口、研究儀表板或客戶支援站，並讓外掛連接核准系統。使用者在網站中完成工作，背後仍使用自己的授權。

![OpenAI Sites 官方畫面，顯示三個連接 Plugins 的網站範例](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/sites-plugins.webp)

OpenAI Sites 官方畫面，顯示三個連接 Plugins 的網站範例

*Sites 與 Plugins 結合後，外掛可以成為特定工作站的一部分。圖片來源：* [*OpenAI DevDay 2026 官方回顧*](https://openai.com/index/devday-2026-recap/?ref=aiposthub.com) *。*

企業最需要確認權限傳遞。網站擁有者能看到什麼、外掛代表哪個使用者操作、資料是否跨工作區，以及離職後如何撤權，都要有清楚答案。使用者自己的授權應被保留，不能因為進入共用 Site 就變成所有人共享同一個高權限帳號。

延伸閱讀： [Sign in with ChatGPT 是什麼？可用 App、方案用量、隱私與費用完整整理](https://www.aiposthub.com/sign-in-with-chatgpt-plan-apps-guide/)

## MCP Events 讓外掛可以被事件觸發

MCP 是連接 AI 與工具、資料的開放協定。MCP Events 提案讓已連接 App 在事件發生時觸發自動化，例如新工單建立、文件更新或狀態改變後，啟動代理整理、分類或通知。

這和使用者主動輸入提示不同。事件驅動能讓工作即時發生，也可能在沒有人盯著畫面時執行。系統需要去重、重試、順序、逾時與失敗通知，否則同一事件可能被重複處理。

![MCP Events 官方示意圖，顯示外部事件與 Plugin automation 的連接](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/mcp-events.webp)

MCP Events 官方示意圖，顯示外部事件與 Plugin automation 的連接

*MCP Events 在發布時仍屬 proposed 規格，文章與產品文件都不應寫成已穩定完成的標準。圖片來源：* [*OpenAI DevDay 2026 官方回顧*](https://openai.com/index/devday-2026-recap/?ref=aiposthub.com) *。*

發布時官方稱它支援提案中的 MCP Events。這個狀態很重要，代表規格可能調整。開發者可先做實驗，不宜把尚未穩定的行為當成長期合約。正式產品應保留版本相容與替代觸發方式。

## Shareable profiles 是分發的另一個入口

可分享個人檔案能展示 Sites 與 Plugins，讓其他人找到並重用。對個人開發者與顧問，這相當於把作品集、產品入口與使用紀錄放在同一個身份下。

Profiles 也讓信任問題更具體。使用者需要知道開發者身份、外掛更新時間、要求權限與支援方式。若只有漂亮展示而缺少安全與維護資訊，分享功能可能加速低品質產品擴散。

企業方案中的個人檔案還要處理組織所有權。員工建立的 Site 或 Plugin 是個人作品還是公司資產，離職後誰維護，必須在一開始定義。

## 適合開發哪些 Plugins？

最適合的是高頻、資料結構清楚、能在 ChatGPT 脈絡中增加價值的工作。例如設計檔案檢視、專案狀態面板、核准表單、資料查詢與內容生產工具。介面與對話互補，比單純包一層聊天更有意義。

第二類是需要使用者身份與權限的企業流程。外掛可代表當前使用者讀取資料，產生草稿後等待批准。這比共用機器人帳號更容易追蹤責任。

不適合的產品，是沒有差異化資料或工作流，只把公開模型包成另一個輸入框。當 ChatGPT 本身持續增加能力，這類外掛很容易失去存在理由。

## 開發與審核檢查表

先定義外掛完成的單一工作與成功標準。再列出所需資料、最小權限與不可執行的動作。寫入、付款、刪除與公開發布都應在介面上顯示對象與後果，並要求確認。

錯誤狀態要清楚。外掛不能只顯示「失敗」，應說明是否已執行部分動作、能否安全重試與使用者下一步。事件觸發還要保存事件識別碼，避免重複處理。

送審前測試提示注入、越權、資料外洩與惡意檔案。介面也要在桌機與手機檢查，確保按鈕、表單與檔案檢視不會溢出或隱藏重要資訊。

最後準備隱私政策、支援管道、刪除流程與版本紀錄。外掛進入目錄只是開始，長期維護才決定信任。

## 我的觀察：Plugins 2.0 的競爭焦點是分發與留存

## 外掛商業模式怎麼設計？

最直接的模式是第三方訂閱。外掛提供基礎功能，團隊協作、歷史版本、專業資料或企業管理由開發者收費。價格頁要說清楚 ChatGPT 方案與第三方費用分開，避免使用者以為安裝後所有功能都包含。

第二種是交易或服務抽成，例如設計輸出、資料查詢、預約與採購。這類產品必須在執行前顯示價格、對象與取消規則。模型不能自行選擇付費項目，金額與同意也不能藏在長段對話裡。

第三種是企業授權。外掛連接公司資料與流程，收入來自席次、管理、稽核與支援。企業在意的不只是功能，還有單一登入、權限同步、資料區域、服務等級與離職撤權。這些能力會決定能否從個人工具升級成正式採購。

開發者也可以把外掛當成既有產品的入口。使用者在 ChatGPT 完成發現與初步任務，再回到原產品處理複雜管理。這能降低對單一平台的依賴，也保留完整客戶關係。

無論採哪一種模式，都要計算每個成功任務的模型、工具與支援成本。免費流量如果大量觸發昂貴代理，會快速侵蝕毛利。用量上限、背景任務預算與清楚的升級提示，比事後突然中斷更能維持信任。

分發來源也應被追蹤。記錄目錄、Profile、Site 與直接入口帶來的啟用、首次成功與留存，才能知道 OpenAI 生態帶來的是短期安裝還是長期客戶。

模型與工具串接逐漸標準化後，建立一個能呼叫接口的外掛不再困難。真正差異會落在使用者是否找得到、第一次是否成功、之後是否持續回來，以及權限是否值得信任。

OpenAI 同時推出 Creator、探索、Profiles、Sites 與 Marketplace，顯示它想掌握從開發到分發的完整路徑。這對開發者很方便，也增加平台依賴。排序、審核與方案政策一旦改變，產品流量可能受到直接影響。

延伸閱讀： [OpenAI Marketplace 與 AWS Bedrock Managed Agents：企業 AI 生態系完整解析](https://www.aiposthub.com/openai-marketplace-aws-bedrock-agents/)

我的結論是值得試，但要保留獨立入口與可攜式資料。把 ChatGPT 當成重要分發渠道，而非唯一生存條件，會是較穩健的策略。

若三個月後使用者只在安裝當天開啟，問題多半不在曝光，而在任務價值或首次體驗。開發者應先修正完成率與重複使用動機，再追求更多推薦流量。

## 常見問題

### Plugins 2.0 和舊版 Plugins 有什麼差別？

新版除了工具呼叫，還有側邊欄入口、互動面板、自訂檔案檢視、Creator、Sites、事件與個人檔案，產品形態更接近完整應用。

### 所有 ChatGPT 方案都能用 Extensions 嗎？

官方回顧將 Plugin extensions 列為全方案功能，但實際 rollout、地區與管理員政策可能影響入口。企業工作區也可能限制可安裝外掛。

### MCP Events 已經正式穩定嗎？

發布時官方稱其為 proposed MCP Events，仍可能調整。開發者應標明實驗狀態並保留相容方案。

### Sites 安裝外掛後會共用資料嗎？

官方表示使用者以自己的資料與權限操作。實際仍要檢查外掛請求範圍與 Site 分享設定，不能假設所有內容都自動隔離。

### 外掛可以直接向使用者收費嗎？

收費方式取決於 OpenAI 平台與外掛本身的政策。開發者應清楚揭露第三方訂閱、用量與退款規則，不要把登入或安裝誤導成免費。

## 結論

ChatGPT Plugins 2.0 把介面、工具、事件、網站與個人分發串成一套平台。Extensions 提供原生體驗，Creator 與目錄降低開發和探索門檻，Sites 擴大使用場景，MCP Events 則讓工作可由外部事件啟動。

開發者應優先解決一個高頻、可驗證的工作，限制權限並把錯誤與費用說清楚。MCP Events 仍要保留提案狀態，企業使用則先處理身份與資料邊界。只要不把平台流量當成永久保證，Plugins 2.0 值得成為產品分發的重要一站。