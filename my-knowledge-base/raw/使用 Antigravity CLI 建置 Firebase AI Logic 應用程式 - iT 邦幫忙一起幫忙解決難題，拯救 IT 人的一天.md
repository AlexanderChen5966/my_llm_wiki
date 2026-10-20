---
title: "使用 Antigravity CLI 建置 Firebase AI Logic 應用程式 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10400438"
author:
  - "[[iThome]]"
published:
created: 2026-07-22
description: "注意： 本專案已提供 Google Cloud 額度。 在這篇部落格文章中，我將示範如何使用 Antigravity CLI（這是一個透過技能 (skills) 與伺服器直接與開發工作流程整合的代理型..."
tags:
  - "clippings"
status: processed
---
0

**注意：** 本專案已提供 Google Cloud 額度。

在這篇部落格文章中，我將示範如何使用 Antigravity CLI（這是一個透過技能 (skills) 與伺服器直接與開發工作流程整合的代理型 (agentic) 人工智慧助理），並結合 Angular、Firebase Hybrid & On-device Inference Web SDK 以及 Gemini 模型，來建置一個影像分析的展示專案。使用者可以上傳一張圖片，並使用 Gemini 模型進行分析，以產生替代文字 (alt text)、標籤、推薦建議以及可用於提升圖片品質的 CSS 提示。

當此展示專案在 Chrome 148+ 瀏覽器中執行時，Hybrid & On-device SDK 會利用裝置端 Gemini Nano 模型的 Prompt API 來執行圖轉文 (image-to-text) 的任務，此時 Token 使用量為 0。當其他瀏覽器（例如 Safari 或 Firefox）執行相同的任務時，SDK 則會降級退回到雲端 AI（Gemini 3.5 Flash 模型），這將會消耗雲端 Token。

接下來，我將說明如何在我的 Angular 專案中安裝技能，並在 Antigravity CLI 中註冊 Angular 與 Stitch MCP 伺服器，以開發此展示專案的基礎架構、服務與 UI 設計。

## 1\. 工作流程

這是我從實作功能、產生 UI 畫面到將畫面對應至 Angular 元件的完整工作流程。

## 2\. 技能 (Skills)

我在我的專案中安裝了 `grill-with-docs` 、 `angular` 與 `firebase` 技能，原因如下：

- grill-with-docs：進行嚴格的問答環節，以針對新功能、重構或關鍵修復產生規格說明書。AI 負責執行徹底的分析，並投入更多心力來產生程式碼以達成任務。
- domain-modeling：此技能在 `grill-with-docs` 技能的 `SKILL.md` 中被引用，因此需要其複本。
- code-review：衍生兩個子代理 (sub-agents) 來審查變更，以偵測程式碼壞味道 (code smells) 並驗證變更是否符合規格說明書。
- angular：提供現代 Angular 架構的最佳實踐，例如使用 Signals 與 Signal Forms。
- firebase：提供 Firebase AI Logic、Firebase Remote 等相關技能。

## 3\. 將 Stitch MCP 伺服器與 Antigravity CLI 連接

Stitch 是 Google 的一項設計轉程式碼 (design-to-code) 工具，可讓開發人員與設計師使用自然語言提示詞產生互動式畫面。

由於地理圍欄 (geofencing) 的限制，香港的使用者需要啟用 VPN 才能存取 `Stitch with AI` 。接著，我可以造訪 `https://stitch.withgoogle.com/` 來建立 Stitch 專案，並在其中使用自然語言產生新畫面。

另一方面，Antigravity CLI 並沒有地理圍欄限制，因此我可以將 Stitch MCP 伺服器註冊至其全域 MCP 設定檔。

這是我在反覆嘗試後，成功讓 CLI 與 Stitch MCP 伺服器溝通的方法：

1. 導覽至 `https://github.com/davideast/stitch-mcp` 並閱讀 `Quick Start` 以初始化 MCP 伺服器。
2. 造訪 `https://stitch.withgoogle.com/settings` ，捲動至 API Key 並建立新的金鑰。複製該金鑰，因為我們會在 mcp 設定檔中用到它。

![Stitch API 金鑰](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/stitch-api-key.png)

3. 開啟 `~/.gemini/config/mcp_config.json` 以全域註冊 Stitch MCP 伺服器。請將 `<Stitch API Key>` 替換為您實際的 API 金鑰。
```json
"mcpServers": {
   "stitch": {
        "command": "npx",
        "args": ["-y", "@_davideast/stitch-mcp@latest", "proxy"],
        "env": {
            "STITCH_API_KEY": "<Stitch API Key>",
            "STITCH_USE_SYSTEM_GCLOUD": "1",
            "GOOGLE_CLOUD_PROJECT": "<google cloud project id>",
            "CI": "1",
            "DOTENV_CONFIG_QUIET": "true"
        }
   }
}
```
4. 退出 Antigravity CLI 並重新啟動，以驗證整合是否正確建立。
5. 在終端機中，詢問一個問題，例如 "List all my Stitch Projects"，然後等待回應。如果 CLI 回傳正確的回應，即代表它已能與 Stitch MCP 伺服器進行通訊。

此時 CLI 已經裝備了所需的技能與 MCP 伺服器。接下來，我可以利用 CLI 與 Gemini 模型來實作功能並產生 Angular 元件。

## 4\. 連接 Angular MCP 伺服器

同樣地，我也註冊了 Angular MCP 伺服器，以便尋找 Angular 最佳實踐並搜尋文件。

```json
"mcpServers": {
  "angular-cli": {
       "command": "npx",
       "args": ["-y", "@angular/cli", "mcp"]
    }
}
```

該伺服器公開了以下工具，以簡化 Angular 的開發：

- ai\_tutor：向 Angular AI 導師尋求程式碼產生方面的協助或提問。
- get\_best\_practices：檢索 Angular 的推薦指南和架構實踐。
- search\_documentation：搜尋 Angular 官方文件。
- list\_projects：列出您的 Angular 工作空間設定檔 (angular.json) 中所定義的專案。
- onpush\_zoneless\_migration：協助將應用程式的部分內容遷移至 OnPush 變更偵測與 zoneless（無 Zone）模式。
- run\_target：使用 Angular CLI 執行任何已設定的 architect 目標（例如 build、test、lint）。
- devserver.start、devserver.stop 與 devserver.wait\_for\_build：管理本地 Angular 開發伺服器並與其進行互動。

現在，CLI 已配備了代理技能與 MCP 伺服器，可協調 Angular 應用程式中新功能的開發。

## 5\. 使用技能與 MCP 伺服器實作功能

在我的 Firebase AI Logic 應用程式中實作了幾項功能後，我開發了兩種工作流程，並根據我是否想要建立肌肉記憶來選擇其中一種。

**工作流程 1** ：使用 `/grill-with-docs` 產生架構決策記錄 (ADR)，並將其儲存至特定路徑以供人工審查與存檔。接著，使用 `/goal` 要求編碼助理實作該 ADR，並接受變更。

例如，我輸入此提示詞來要求編碼助理實作 ADR 並包含相關技能。

*提示詞* ："/goal implement `<ADR file name>`, use Angular skill and Angular MCP server to adopt modern Angular architecture in the code"

![使用 grill-with-docs 與 goal 技能的工作流程](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/grill-with-docs-and-goal.jpg)

**工作流程 2** ：使用 `/grill-with-docs` 產生 ADR，並將其儲存至特定路徑以供人工審查與存檔。我自己動手實作該 ADR 以建立肌肉記憶，並將變更提交 (commit) 至 Git 分支。接下來，使用 `/code-review` 擷取該 Git 分支的某個提交或所有變更，並要求編碼助理衍生兩個子代理來審查變更。第一個子代理負責偵測程式碼壞味道 (code smells)，第二個子代理則根據 ADR 驗證變更，以尋找範圍蔓延 (scope creep)、遺漏或錯誤的細節。

例如，我輸入此提示詞來要求編碼助理擷取 Git 分支，並針對 ADR 進行驗證。

*提示詞* ："/code-review fetch all the commits of `<Git branch>`, and validate the changes against `<ADR filename>`."

子代理執行後會印出包含違規項目的報告。我修正 these 違規項目，對分支進行基底重置 (rebase)，並重複此工作流程，直到報告顯示 0 個違規為止。最後，我將功能分支合併 (merge) 至 `main` 分支，並將 `main` 分支推送 (push) 至遠端存放庫，以部署到 Firebase App Hosting。

![使用 grill-with-docs 與 code-review 技能的工作流程](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/workflow.jpg)

在實作完功能之後，我便擁有了 enums、types、interfaces、utilities 與 services。在下一節中，我將使用 Stitch MCP 伺服器來產生 UI 畫面，並將其對應至 Angular 元件。

## 6\. 使用 Stitch 產生 UI 畫面

此 Web 展示專案允許使用者上傳圖片，並透過提示詞讓 Gemini 模型進行分析，以產生標籤、替代文字、CSS 篩選器與影像裁剪。

**注意** ：請啟用 VPN 以存取 Stitch 網站

我使用自然語言來引導 Stitch 為我的 Web 應用程式產生兩個頁面。此 Web 應用程式包含一個首頁（登陸頁）和一個影像分析頁面。

首先，我建立了一個新的 Stitch 專案來託管這些 UI 畫面。

![新 Stitch 專案](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/stitch-project.png)

接著，我撰寫提示詞來產生首頁和影像分析頁面

![首頁](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/home-page.png)

![影像分析頁面](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/image-analysis.png)

點擊 Analysis Image 按鈕後，頁面會顯示帶有幾個頁籤的頁籤群組。

![影像分析頁籤群組](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/image-analysis-tab-group.png)

接下來，我需要使用 Antigravity CLI 與 Stitch MCP 伺服器進行通訊，以產生 `Design.md` 檔案。

提示詞："Use the Stitch MCP server to retrieve a `Stitch UI Screen` from my `Stitch Project`. Analyze its visual layout, extract the design tokens (colors, typography, and spacing), and generate a `DESIGN.md` file in the `project folder path`."

例如，我將首頁畫面的 `DESIGN.md` 儲存到根目錄，並將影像分析頁面的 `DESIGN.md` 儲存於 `image-analysis` 資料夾中。

在審查過 `DESIGN.md` 並確保一切看起來都很棒之後，我再次使用 `/goal` 將 `DESIGN.md` 對應至 Angular 元件。在提示詞中，我儘可能提供充足的上下文，以防止編碼助理將所有的 CSS 樣式、HTML 範本和邏輯通通產生在單一元件中。

*提示詞* ："/goal You are an expert Angular developer. Your goal is to implement the UI layout defined in `DESIGN.md` into production-ready Angular components. Fully read `DESIGN.md` first to extract all design tokens, spacing guidelines, colors, and layout instructions, and apply them using the following rules: Insert the context and rules...."

![Antigravity CLI 與 Stitch MCP 伺服器通訊](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/stitch-mcp-server.jpg)

## 7\. 實際站台測試

此使用案例為上傳一張圖片、執行影像分析，並顯示該分析的 Token 消耗量。

在 Chrome 148+ 上，

- 步驟 1：上傳一張圖片  
	![圖片上傳](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/chrome-image-upload.png)
- 步驟 2：點擊按鈕以產生標籤與替代文字  
	![圖片標籤與替代文字](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/chrome-tags.png)
- 步驟 3：點擊 Token 使用量 (Token usage) 頁籤，以觀察完成任務所花費的 Token。  
	![Token 使用量](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/chrome-token-usage.png)

在 Firefox 上，重複相同的步驟

- 步驟 1：上傳一張圖片  
	![圖片上傳](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/ff-image-upload.png)
- 步驟 2：點擊按鈕以產生標籤與替代文字  
	![圖片標籤與替代文字](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/ff-tags.png)
- 步驟 3：點擊 Token 使用量頁籤，以觀察完成任務所花費的 Token。  
	![Token 使用量](https://raw.githubusercontent.com/railsstudent/colab_images/refs/heads/main/blog-posts/fire-ai-logic-antigravity-cli-stitch-mcp-server/ff-token-usage.png)

## 8\. 結論

使用 Antigravity CLI、代理技能與 Stitch MCP 伺服器建置影像分析應用程式的旅程到此告一段落。

當此展示專案在 Chrome 148+ 中執行時，會使用 Gemini Nano 模型，雲端 AI 的 Token 使用量為 0。當此展示專案在其他瀏覽器中執行時，則會使用 Gemini 3.5 Flash 模型產生回應，進而消耗雲端中的 Token。

請研究 Firebase AI Hybrid & On-Device Web SDK，並善加利用裝置端模型執行簡單的 AI 任務以降低成本。

## 相關資源

- [Firebase Hybrid & On-device 影像分析應用程式 GitHub 專案](https://github.com/railsstudent/ng-firebase-image-analyzer)
- [Firebase Hybrid & On-device 推論官方指南](https://firebase.google.com/docs/ai-logic/hybrid/web/get-started?authuser=0)
- [Chrome 內建 Prompt API 開發人員文件](https://developer.chrome.com/docs/ai/prompt-api)
- [Stitch 官方網站](https://stitch.withgoogle.com/)
- [Stitch MCP 伺服器 GitHub 存放庫](https://github.com/davideast/stitch-mcp)
- [grill-with-docs 技能 GitHub 頁面](https://github.com/mattpocock/skills/tree/main/skills/engineering/grill-with-docs)
- [Angular 技能 GitHub 存放庫](https://github.com/angular/skills)
- [Firebase 技能官方說明文件](https://firebase.google.com/docs/ai-assistance/agent-skills)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/22/27c2eba5-5340-4a08-b9ad-e050940c80d7_small.png)

[Zero Trust 讀書會：英國觀點，解析 NCSC Zero Trust 準則](https://itplus.ithome.com.tw/vod-page/626?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

零信任資安講堂 |

35 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/21/05871686-40f0-4b12-a187-41075373b1a3.png)

[Organizing Hybrid Conferences: Empowering Community Events from a Technical Perspective During the Pandemic](https://itplus.ithome.com.tw/vod-page/782?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/5/24/52d4ef86-3c9f-4703-9ddc-d8a09709380d_small.png)

[Running and Securing your GenAI+LLM workloads on Kubernetes 如何在 Kubernetes 上高效運行並保護您的生成式 AI + 大語言模型工作負載](https://itplus.ithome.com.tw/vod-page/1122?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

21 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/b79a2ccc-cb72-410a-b319-3603c90679c5_small.png)

[在 Kubernetes 環境中進行開源軟件治理的實踐](https://itplus.ithome.com.tw/vod-page/1315?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

32 分