---
title: "Codex 必裝的 12 個插件，都在這裡了"
source: "https://codelove.tw/@tony/post/3Qj5Ya"
author:
published:
created: 2026-06-15
description: "Codex 不得不裝的 12 個外掛，都在這了  ==========================    最近我把 Codex 當成主力 AI 工具用了快一個月。一開始我只是拿它補程式碼、改 Bug。用了一段時間後，我越來越覺得，Codex 的強大之處不只是模型本身，更關鍵的是它背後的..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月14日](https://codelove.tw/@tony/post/3Qj5Ya "2026-06-14 11:09:43")

## Codex 不得不裝的 12 個外掛，都在這了

```
最近我把 Codex 當成主力 AI 工具用了快一個月。
```

一開始我只是拿它補程式碼、改 Bug。用了一段時間後，我越來越覺得，Codex 的強大之處不只是模型本身，更關鍵的是它背後的外掛生態。

很多人裝完 Codex，只會聊天、生成程式碼、讓它解釋錯誤。但把外掛接好以後，Codex 就不只是一個「回答問題的視窗」了，它可以直接處理設計、影片、辦公、資料和自動化任務。別人還在來回切工具、複製貼上、整理素材的時候，你可以用一句話讓 Codex 把一整段工作流程跑起來。

我這邊也有一些 AI Coding（SDD Agent AI 工具等） 和 Node 技術交流交流群，感興趣的可以加我的私人微信 **ikoala520** 進群，一起學習，共同進步。

[![Codex 外掛推薦](https://i.imgur.com/E7mxcTo.jpeg)](https://i.imgur.com/E7mxcTo.jpeg) 截至 2026 年 6 月 9 日，我重新看了一遍官方更新、外掛倉庫和社群討論。最近最明顯的趨勢是：Codex 正在從「程式設計師工具」變成「知識工作工具」。OpenAI 在 6 月 2 日發布的更新裡，把 `role-specific plugins` 、 `Sites` 和 `Annotations` 放在一起講，這其實已經說明方向了：外掛不是裝飾品，而是讓 Codex 適配不同角色、不同工具鏈、不同交付物的方式。

官方這次重點提到的角色外掛包括資料分析、創意生產、產品設計、銷售、金融市場和投資銀行業務。它們背後連接的是 Figma、Canva、Snowflake、Tableau、Salesforce、HubSpot、Gmail、Google 雲端硬碟、FactSet 等真實工具。換句話說，Codex 想做的不是「再多回答幾個問題」，而是幫你把工作成果做出來。

所以這篇文章不寫一個泛泛的外掛大全。我會按自己的使用場景，把最值得關注的 12 個外掛放在一起：有的適合設計和內容團隊，有的適合寫報告、做簡報、處理表格，有的適合自動化重複操作。你不一定要全部安裝，但如果你想真正把 Codex 用成主力工具，這裡面大概率會有幾款適合你的工作流程。

## 先說結論：不要按名字裝，要按工作流程裝

很多人裝外掛有個誤區：看到什麼紅就裝什麼，最後左側列表一堆入口，但真正用的時候還是只會問「幫我寫一下」。

我更建議按工作流程來選。比如你是內容創作者，就優先裝 `Canva` 、 `Documents` 、 `Presentations` 、 `Chrome` ；你做產品和前端，就優先裝 `Figma` 、 `Product Design` 、 `Chrome` 、 `Presentations` ；你經常處理資料，就把 `Spreadsheets` 放在前面；你做重複性桌面操作，再考慮 `Computer Use` 。

外掛的價值不是「多一個按鈕」，而是讓 Codex 能碰到真實素材、呼叫真實工具、交付真實檔案。

[![Codex 外掛工作流程地圖](https://i.imgur.com/csMuIOL.jpeg)](https://i.imgur.com/csMuIOL.jpeg)

## 工作場景推薦組合適合誰快速出圖Canva + Figma + Product Design內容營運、設計師、獨立開發者影片內容HyperFrames by HeyGen + Remotion短影片、課程、產品演示辦公交付Presentations + Spreadsheets + Documents簡報、顧問、營運、管理者專業圖表BioRender + Documents科研、醫學、教育內容重複操作Chrome + Computer Use + Google Calendar需要網頁、桌面和日程自動化的人1. Canva：最快把想法變成可看的圖

`Canva` 適合所有需要快速出圖的人。你輸入主題、用途和風格，Codex 可以呼叫設計模板生成海報、社群媒體圖片、文章封面、活動視覺或宣傳圖。

它最適合的場景不是做特別複雜的設計系統，而是「我現在就需要一張能用的圖」。比如公眾號封面、小紅書配圖、課程海報、活動邀請圖、產品發布預告，這類內容往往不值得從零開始做設計，但又不能隨便糊一張截圖。

你可以這樣用：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼用 Canva 幫我做一張公眾號封面。
主題是「Codex 必裝外掛清單」，風格專業、科技感、不要花俏。
畫面重點突出「外掛生態」和「效率工具」。
```

如果你做內容行銷、個人品牌、知識付費或專案宣傳， `Canva` 是最應該先裝的設計類外掛。

2. ## Figma：把設計需求變成可編輯稿件

`Figma` 更適合產品、UI 和前端相關工作。你可以給 Codex 一個描述、截圖、既有頁面或簡單程式碼，讓它生成可編輯的高擬真稿件，也可以讓它幫你把設計稿拆成元件和實作思路。

最近 Codex 裡產品設計相關討論很熱，本質上就是因為 AI 前端不能只會寫程式碼。一個頁面能不能用，取決於資訊層級、視覺密度、互動狀態、響應式版面和品牌氣質。 `Figma` 這類外掛可以把「寫程式碼之前的設計判斷」提前。

比較適合的提示詞是：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼基於這個產品需求，先給我 3 個不同視覺方向的 Figma 草稿。
目標用戶是 B 端營運人員，介面要克制、清楚、適合長期使用。
不要做成行銷著陸頁，優先考慮資訊掃描效率。
```

如果你經常讓 AI 寫前端，我建議把 `Figma` 放在很前面的位置。它能減少「程式碼寫完才發現審美不對」的返工。

3. ## Product Design：先做產品判斷，再讓 Codex 寫程式碼

`Product Design` 是最近很值得關注的角色外掛。它和 `Figma` 的區別在於， `Figma` 更像設計檔和畫布入口，而 `Product Design` 更像一套產品設計工作流程：先理解 brief，再做視覺方向、產品 spec、原型、UI 評論和設計評審。

這類外掛最適合解決一個老問題：AI 直接寫前端時，常常功能能跑，但頁面沒有產品感。比如資訊層級不清楚、手機版不舒服、按鈕狀態缺失、頁面像模板站。 `Product Design` 的價值，是把「寫程式碼之前的判斷」提前，讓 Codex 先像產品設計師一樣想一輪。

你可以這樣用：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼用 Product Design 幫我做一個 AI 寫作工具的工作台。
請先不要寫程式碼，先給 3 個產品方向和視覺方向。
目標用戶是技術部落客和獨立開發者，介面要專業、克制、適合長期寫作。
```

如果你的工作區能看到這個外掛，我建議產品經理、前端和獨立開發者都試一下。它不是簡單讓頁面更好看，而是讓 Codex 在開發前先完成一次「產品方向對齊」。

4. ## HyperFrames by HeyGen：用腳本生成影片

`HyperFrames by HeyGen` 適合做短影片、產品介紹和演示動畫。它的思路不是傳統剪輯，而是用 HTML 或腳本描述畫面，再自動渲染影片。

這類外掛對內容團隊很有吸引力，因為很多影片其實是結構化的：開場標題、三段賣點、產品截圖、字幕、結尾 CTA。過去你要在剪輯軟體裡一點點拖時間軸，現在可以先讓 Codex 根據文案生成一個影片結構，再交給外掛渲染。

你可以這樣用：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼把這篇文章改成 60 秒短影片腳本，並用 HyperFrames 生成一個影片草稿。
畫面節奏：開頭 5 秒給痛點，中間展示 12 個外掛，結尾給安裝建議。
```

它適合需要批量生產影片素材的人，尤其是產品宣傳、課程切片和資訊流廣告。

5. ## Remotion：更專業的影片動效

如果說 HyperFrames 更偏「快速生成」， `Remotion` 更適合做可控的影片動效。它本身是用 React 寫影片的框架，適合把腳本、資料和動畫參數結合起來，生成結構穩定、可重複使用的影片。

比如產品演示、課程片頭、資料報告動畫、版本更新影片，都可以用 Remotion 做成模板。以後你只需要換資料、換截圖、換文案，就能生成新影片。

一個典型用法是：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼用 Remotion 做一個 45 秒產品更新影片。
結構：標題開場、3 個功能點、每個功能點配截圖動畫、最後展示 QR Code。
請把動畫節奏控制得乾淨，不要過度炫技。
```

如果你做的是長期內容，而不是一次性影片， `Remotion` 的價值會更大，因為它能把影片生產變成工程化模板。

6. ## Presentations：快速生成簡報初稿

`Presentations` 是我覺得最容易被低估的外掛。很多人以為它只是「幫我生成 PPT」，但真正省時間的地方是：它會先幫你搭大綱、拆邏輯、排結構，再生成可繼續修改的簡報檔案。

日常匯報、專案回顧、募資 BP、課程講義、產品發布、內部培訓，都適合用它起稿。你不需要從空白頁開始，也不用先在文件裡寫一遍大綱再搬到 PPT。

你可以這樣用：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼幫我做一份 12 頁簡報，主題是「為什麼 Codex 外掛會改變知識工作流程」。
受眾是產品經理和內容團隊。
請先給完整大綱，再生成簡報稿，風格專業、資訊密度適中。
```

我的建議是：不要直接讓它做最終版，而是把它當作「第一版結構生成器」。結構出來後，再補業務細節和真實資料，效率會高很多。

7. ## Spreadsheets：複雜表格不用再手動折騰

`Spreadsheets` 適合處理公式、圖表、資料清洗和分析。它可以幫你整理 CSV、做樞紐分析、補公式、生成圖表，也可以解釋指標為什麼變化。

財務、營運、增長、銷售、內容資料分析都會用到它。以前你可能要在 Excel 或 Google 試算表裡來回試公式，現在可以直接說目標，讓 Codex 生成表格結構和分析結論。

比如：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼分析這份渠道投放資料，找出轉換率下降的主要原因。
請生成一個帶圖表的表格，並給出 3 條可執行建議。
```

最近官方也把資料分析外掛列為重點方向，這說明 Codex 的外掛生態正在從「寫文件、寫程式碼」擴展到「解釋業務變化」。對經常和資料打交道的人來說，這個方向會越來越重要。

8. ## Documents：長文本處理和報告寫作

`Documents` 適合處理長文本：總結材料、提煉要點、改寫報告、生成腳本、整理會議紀要、起草方案。它的價值不只是寫字，而是把一堆分散內容整理成結構化文件。

比如你有幾篇資料、幾段訪談紀錄、幾份會議紀要，可以讓 Codex 先提煉共同主題，再生成報告框架，最後匯出成文件。

你可以這樣用：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼閱讀這些材料，整理成一份 3000 字分析報告。
結構包括：背景、核心發現、風險、建議、下一步行動。
請保留關鍵事實來源，不要編造資料。
```

如果你經常寫方案、報告、文章、腳本， `Documents` 基本屬於必裝。

9. ## BioRender：科研和醫學圖表更專業

`BioRender` 更垂直，適合科研、醫學、生物、藥學和教育內容。你輸入專業需求，它可以生成更符合學術表達習慣的圖表，比如機制示意圖、實驗流程圖、細胞通路圖、醫學科普插圖。

這類圖最怕「看起來像裝飾圖」。科研和醫學場景裡，圖不是為了好看，而是為了準確表達結構、流程和關係。 `BioRender` 的價值就在於，它比通用設計工具更懂專業視覺語言。

可以這樣用：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼幫我做一張醫學科普圖，主題是「GLP-1 藥物如何影響食慾和血糖」。
要求：結構清楚，適合一般讀者理解，避免過度專業術語。
```

如果你不是科研或醫學方向，可以先不裝；但只要你有專業圖表需求，它會比通用圖片生成工具穩很多。

10. ## Chrome：讓 Codex 直接操作網頁

`Chrome` 是自動化類外掛裡最實用的一個。它可以讓 Codex 控制瀏覽器，讀取網頁、抓取資訊、填寫表單、比較頁面、檢查視覺效果。

對開發者來說，它可以用來驗收本地頁面：打開 [localhost](http://localhost/) ，點按鈕，截圖，檢查互動是否正常。對非開發者來說，它可以處理一些重複性上網任務，比如整理網頁資料、比價、輸入表單、檢查後台資料。

你可以這樣說：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼打開這個網頁，把頁面裡的產品名稱、價格和關鍵功能整理成表格。
如果有分頁，請繼續翻頁，最後給我一個彙總。
```

需要提醒的是，涉及登入、付款、發布、刪除、發信這類動作時，不要讓 AI 自動決定。最好讓它先準備、預覽和確認，再由你授權執行。

11. ## Computer Use：讓 Codex 操作本地桌面應用

`Computer Use` 比 `Chrome` 更進一步。 `Chrome` 主要控制瀏覽器，而 `Computer Use` 可以操作本地 Mac 應用和系統介面，比如點擊、輸入、捲動、拖曳、讀取螢幕內容。這意味著一些沒有 API、沒有網頁介面、只能靠桌面 UI 完成的任務，也可以交給 Codex 半自動處理。

它適合處理那些「簡單但煩」的工作：從某個桌面軟體裡匯出檔案、批量改設定、根據截圖檢查介面狀態、在多個視窗之間搬運資訊、操作一個老系統後台。對開發者來說，它也可以用來做本地應用的手動驗收，尤其是那些很難只靠命令列驗證的互動。

你可以這樣用：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼用 Computer Use 打開這個桌面應用，檢查匯出流程是否正常。
請只操作到預覽和確認頁面，不要點擊最終提交或刪除類按鈕。
如果遇到需要我授權的步驟，先停下來說明。
```

這個外掛很強，但也要更謹慎。凡是涉及付款、刪除、發布、傳送、授權、改系統設定的動作，都應該讓 Codex 先停在確認前。它最適合做「有人監督的自動化」，不要把高風險操作完全放飛。

12. ## Google Calendar：把日程也交給 Codex 管

`Google Calendar` 適合管理日程、建立會議、調整安排和協調時間。它看起來不像設計、影片外掛那麼炫，但對團隊協作很有用。

比如你可以讓 Codex 根據郵件或會議紀要自動提取待辦事項，找到合適時間，生成日程草稿；也可以讓它幫你檢查本週安排，找出衝突和空檔。

典型用法是：

```
text 體驗 AI 代碼助手 程式碼解讀複製程式碼根據這段會議紀要，幫我建立後續日程草稿。
需要包括：需求評審、設計確認、開發排程、上線回顧。
先列出建議時間和參會人，不要直接建立，等我確認。
```

日程類外掛的關鍵不是「自動替你安排一切」，而是減少低價值協調成本。真正重要的判斷，還是要你確認。

## 最近更值得關注的 3 個趨勢

除了上面 12 個具體外掛，我覺得還有 3 個趨勢值得單獨說一下。

第一，角色外掛會越來越重要。OpenAI 官方倉庫已經把銷售、資料分析、產品設計、金融市場這幾類外掛做成模板。它們不是單個工具，而是把一組技能、連接器、說明和工作流程打包。以後你可能不會只裝一個 `Figma` ，而是裝一個面向「產品設計」的完整工作流程。

第二， `Sites` 會改變交付方式。以前 AI 生成結果往往是文件、表格、簡報或程式碼檔案。 `Sites` 的方向是讓 Codex 生成可分享的互動式頁面，比如儀表板、專案看板、客戶回顧頁、活動規劃頁。這東西一旦成熟，很多臨時工具和內部頁面都可以直接用 Codex 搭出來。

第三，外掛會從「官方給什麼」走向「團隊自己客製」。官方倉庫裡已經能看到外掛模板結構，包括 `.codex-plugin/plugin.json` 、`.app.json` 、`.mcp.json` 、 `skills/` 和 `assets/` 。這意味著未來真正有價值的不是裝最多外掛，而是把自己的業務流程沉澱成外掛。

## 安裝方式和使用建議

外掛安裝很簡單。在 Codex App 左側找到 `Plugins` ，搜尋外掛名稱後直接安裝，通常不到一分鐘就能完成。部分外掛需要綁定帳號或授權連接器，按提示完成即可。

但我不建議一口氣全裝。外掛太多會讓注意力分散，也會增加權限管理成本。更合理的方式是先按自己的主要工作流程挑 3 到 5 個：

如果你主要做先裝這些寫文章和做內容 `Documents` 、 `Canva` 、 `Chrome` 、 `Presentations` 做產品和前端 `Figma` 、 `Product Design` 、 `Chrome` 、 `Presentations` 做營運和資料 `Spreadsheets` 、 `Documents` 、 `Chrome` 做課程和影片 `HyperFrames by HeyGen` 、 `Remotion` 、 `Canva` 做科研和醫學內容 `BioRender` 、 `Documents` 、 `Presentations` 做桌面自動化 `Chrome` 、 `Computer Use` 、 `Google Calendar` 另外要注意三點。

第一，不同帳號、地區和工作區能看到的外掛可能不完全一樣。Business 和 Enterprise 工作區還可能受管理員權限控制。

第二，外掛越接近真實業務系統，越要注意權限。比如瀏覽器、郵件、日曆、CRM、雲端硬碟這類外掛，最好先讓 Codex 生成草稿和建議，不要預設允許它執行不可逆操作。

第三，外掛不是越多越好。真正能提升效率的，是你能不能把它們串成固定流程。比如「Chrome 搜資料 → Documents 整理 → Presentations 出簡報 → Canva 做封面」，或者「Product Design 定方向 → Figma 細化稿件 → Codex 寫原型 → Chrome 驗收頁面」，這就比單獨呼叫某個外掛更有價值。

## 最後

用好這些外掛以後，Codex 不再只是一個聊天工具，也不只是一個程式碼助手。它更像一個能調度工具的工作台：你給它目標，它幫你找資料、做設計、寫文件、處理資料、生成簡報，甚至操作瀏覽器和日程。

當然，實際效果取決於你的使用習慣。剛開始不要追求一次完成所有事，先從一個高頻場景開始，把提示詞、素材和交付格式固定下來。等一個流程跑順了，再接第二個外掛、第三個外掛。

我這邊也有一些 AI Coding（SDD Agent AI 工具等） 和 Node 技術交流交流群，感興趣的可以加我的私人微信 **ikoala520** 進群，一起學習，共同進步。

我現在越來越確定一件事：未來拉開差距的，不是「會不會問 AI」，而是「能不能把 AI 接進真實工作流程」。Codex 外掛生態真正有意思的地方，也正在這裡。

## 延伸閱讀

資料為什麼值得看 [OpenAI：Codex for every role, tool, and workflow](https://link.juejin.cn/?target=https%3A%2F%2Fopenai.com%2Findex%2Fcodex-for-every-role-tool-workflow%2F) 官方 2026 年 6 月 2 日發布，介紹角色外掛、Sites 和 Annotations。 [OpenAI GitHub：role-specific-plugins](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fopenai%2Frole-specific-plugins) 官方角色外掛模板倉庫，可以看到銷售、資料分析、產品設計、金融市場等外掛結構。 [X 上關於 Product Design 外掛的討論示例](https://link.juejin.cn/?target=https%3A%2F%2Fx.com%2FBTCqzy1%2Fstatus%2F2063165828208746639) 社群對 Codex 設計類外掛的直觀回饋入口。

---

原文出處： [https://juejin.cn/post/7649738148373725225](https://juejin.cn/post/7649738148373725225)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3Qj5Ya) [31瀏覽](https://codelove.tw/@tony/post/3Qj5Ya)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3Qj5Ya)

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

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [我如何使用 Claude Code Skills 實現股票篩選自動化 \[Python × yfinance × Vibe Coding\]](https://codelove.tw/@tony/post/q5nlWx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝14 ❤️1

445

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

1

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1781493253&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3Qj5Ya&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuNTQiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE0OS4wLjc4MjcuNTQiXSxbIkNocm9taXVtIiwiMTQ5LjAuNzgyNy41NCJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1781493253260&amp;bpp=1&amp;bdt=183&amp;idt=88&amp;shv=r20260611&amp;mjsv=m202606090101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1781493177%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1781493177%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1781493177%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4316085836796&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390667%2C95390788&amp;oid=2&amp;pvsid=2536878457780215&amp;tmod=1085849897&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=115" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260611/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>