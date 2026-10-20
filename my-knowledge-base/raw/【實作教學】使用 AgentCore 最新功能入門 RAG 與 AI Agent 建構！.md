---
title: "【實作教學】使用 AgentCore 最新功能入門 RAG 與 AI Agent 建構！"
source: "https://codelove.tw/@tony/post/anbXGa"
author:
published:
created: 2026-07-03
description: "這個活動用的動手做步驟，任何人都可以在自己方便的時候嘗試！（約 30 分鐘～1 小時）我們會用低程式碼快速做出來。![スクリーンショット 2026-07-02 1.29.29.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fq..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月03日](https://codelove.tw/@tony/post/anbXGa "2026-07-03 08:06:03")

## 【實作教學】使用 AgentCore 最新功能入門 RAG 與 AI Agent 建構！

這個活動用的動手做步驟，任何人都可以在自己方便的時候嘗試！（約 30 分鐘～1 小時）

我們會用低程式碼快速做出來。

[![スクリーンショット 2026-07-02 1.29.29.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fb8332127-3f40-48ec-93ff-c2574aaff04e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5a087c531e1f1faaefc66a3b2d50d4e2)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fb8332127-3f40-48ec-93ff-c2574aaff04e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5a087c531e1f1faaefc66a3b2d50d4e2)

可惜的是，最近新建立的 AWS 帳號（尤其不是公司契約的那種），Bedrock 的模型呼叫配額常常預設為 0，或是沒有使用新 Frontier Model 的權限，不過， **即使在這種情況下，我們也想辦法讓大家仍然能體驗這個動手做！**

會產生一些 AWS 使用費用（預估數十日圓起）。請自行負責執行。

因為是全伺服器無狀態架構，所以費用大多是瀏覽過程中 LLM API 的推論 token 費用。

## 事前準備

新建立 AWS 帳號

※請選擇「付費」方案

[![スクリーンショット 2026-07-02 1.10.49.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F5951a837-3722-4301-8bca-1ab6349634c7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e00849f2c95774a6d756a1ecbf262d88)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F5951a837-3722-4301-8bca-1ab6349634c7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e00849f2c95774a6d756a1ecbf262d88)

註冊後，登入管理主控台

[![スクリーンショット 2026-07-01 23.20.25.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fffa737a8-dd71-43d4-b034-222b19be0833.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=8df5817f7a256cbb41fc2a375491cf64)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fffa737a8-dd71-43d4-b034-222b19be0833.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=8df5817f7a256cbb41fc2a375491cf64)

將區域設定為「美國東部（維吉尼亞北部）」

[![スクリーンショット 2026-07-01 23.20.48.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F90794921-99c5-4b6c-aad4-2ee617bc6e66.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=97b6ba092e0159e2e3ac36d5183fde4b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F90794921-99c5-4b6c-aad4-2ee617bc6e66.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=97b6ba092e0159e2e3ac36d5183fde4b)

## RAG 編

[![スクリーンショット 2026-07-02 1.29.36.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F5d340c25-8577-4ae6-9c33-fc78b3b3e73b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b551bae7b5ec65555a9619fb6658e9b5)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F5d340c25-8577-4ae6-9c33-fc78b3b3e73b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b551bae7b5ec65555a9619fb6658e9b5)

### 上傳內部文件

前往 S3 主控台

[![スクリーンショット 2026-07-01 23.21.59.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fcda72a7a-c3cb-4df5-841d-14d16829e470.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a94c385daa39de1e36ba95099027abe9)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fcda72a7a-c3cb-4df5-841d-14d16829e470.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a94c385daa39de1e36ba95099027abe9)

建立新的 S3 儲存貯體

- 儲存貯體名稱要在全世界唯一。其他項目維持預設即可

[![スクリーンショット 2026-07-01 23.22.45.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F6dc4bee3-54d0-4ae2-a9d0-adad68a3b470.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e205e8d6c2bcbd50a2c520e3920984fa)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F6dc4bee3-54d0-4ae2-a9d0-adad68a3b470.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e205e8d6c2bcbd50a2c520e3920984fa)

下載以下的內部文件範例，並上傳到剛建立的儲存貯體

這份投影片全部都是圖片，究竟能不能正確轉換成 RAG 知識呢？敬請期待。

[![スクリーンショット 2026-07-01 23.34.10.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F5a26c75f-4397-4613-ae6e-172789d8ee08.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=9d21372a8ef39fb736a83aede126d35b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F5a26c75f-4397-4613-ae6e-172789d8ee08.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=9d21372a8ef39fb736a83aede126d35b)

### 建立 RAG 管線

前往 Bedrock 主控台

[![スクリーンショット 2026-07-01 23.35.23.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F5a4f9359-291d-4b62-a124-d718fefc207c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=916a9aa9b790f76eb2b01b1fa05e4d76)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F5a4f9359-291d-4b62-a124-d718fefc207c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=916a9aa9b790f76eb2b01b1fa05e4d76)

建置 > 知識庫 > Create Managed KB

[![スクリーンショット 2026-07-01 23.35.56.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fa5075a31-6d34-40c7-9430-491a64f54d5c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=55dce207fad7c2b52f9eda09d3162d37)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fa5075a31-6d34-40c7-9430-491a64f54d5c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=55dce207fad7c2b52f9eda09d3162d37)

從「瀏覽 S3」按鈕選擇上面的儲存貯體。其他維持預設，然後按「建立知識庫」

[![スクリーンショット 2026-07-01 23.36.51.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F66105f44-c434-4f6c-9dbb-02ca0ddceefe.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=47ee6eea5dc06be56657b122d7f8fa89)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F66105f44-c434-4f6c-9dbb-02ca0ddceefe.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=47ee6eea5dc06be56657b122d7f8fa89)

系統會自動開始建立 RAG 並進行首次同步。這需要幾分鐘，期間可以先進行下一步。

[![スクリーンショット 2026-07-01 23.41.33.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F866512fe-b2f2-4bd3-91f1-3ceacb2f8aa4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d75969c88b1ded7695b61fea30da43b6)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F866512fe-b2f2-4bd3-91f1-3ceacb2f8aa4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d75969c88b1ded7695b61fea30da43b6)

受管 KB 背後使用的 LLM 和嵌入模型也由 AWS 管理，因此即使新 AWS 帳號的 Bedrock 配額是 0，也可以使用！

## AI 代理人編

### 建立 AgentCore 閘道

先建立連接 AI 代理人與 RAG 的閘道。

[![スクリーンショット 2026-07-02 1.29.44.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F85aaea2c-4e54-4ccf-9dbd-e886b3d943b0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c95755fec7e058be190f944170a051b5)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F85aaea2c-4e54-4ccf-9dbd-e886b3d943b0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c95755fec7e058be190f944170a051b5)

前往 AgentCore 主控台（建議按右鍵以新分頁開啟）

[![スクリーンショット 2026-07-01 23.43.24.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F4e5a9fe2-c7c4-4828-be8a-997165d0d8c3.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6282d950289b0706528a5d8af6d7fc20)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F4e5a9fe2-c7c4-4828-be8a-997165d0d8c3.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6282d950289b0706528a5d8af6d7fc20)

建置 > 閘道 > 建立閘道

[![スクリーンショット 2026-07-01 23.45.10.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F71377b5a-1833-4ed9-8d48-d15f046b80fc.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e57a5e576e79919699bc2fdb140cb799)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F71377b5a-1833-4ed9-8d48-d15f046b80fc.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e57a5e576e79919699bc2fdb140cb799)

第 1 步保持預設直接按 Next

[![スクリーンショット 2026-07-01 23.45.27.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F415c7aaf-2ea3-4b9b-a1cd-a8793e5dda59.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4704f66cecaf91e2133621fd51c303c8)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F415c7aaf-2ea3-4b9b-a1cd-a8793e5dda59.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4704f66cecaf91e2133621fd51c303c8)

第 2 步在 Inbound Auth type 中選擇「IAM 權限」，然後按 Next

[![スクリーンショット 2026-07-01 23.46.22.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F31c9d22e-c171-4960-a0ec-9212016216cb.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d85520f48fb28062bafa858a1d3c3537)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F31c9d22e-c171-4960-a0ec-9212016216cb.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d85520f48fb28062bafa858a1d3c3537)

第 3 步將目標類型設為 Connectors，選擇剛建立的 KB，然後 Next > 建立閘道

[![スクリーンショット 2026-07-01 23.47.38.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F2f457a27-49dd-4b6e-a805-dcbb06bb9a45.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2fb03f9d9c24ada1a8dafd6aa3d2b38d)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F2f457a27-49dd-4b6e-a805-dcbb06bb9a45.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2fb03f9d9c24ada1a8dafd6aa3d2b38d)

這裡也可以選擇 Web Search Tool，但日文搜尋精準度不太好。建議改用本動手做後半段會介紹的 AgentCore 瀏覽器，會比較方便。

### 建立 AgentCore Harness

部署「Strands Agents + AgentCore Runtime」這組合。

[![スクリーンショット 2026-07-02 1.29.51.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F2ad28f10-aaa6-401c-a9d8-336b4f0824cf.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=876708013aa89fc8c51f73396393ab19)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F2ad28f10-aaa6-401c-a9d8-336b4f0824cf.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=876708013aa89fc8c51f73396393ab19)

建置 > Harness > 建立進階 Harness

[![スクリーンショット 2026-07-01 23.50.32.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fdddd437a-ebe1-4c88-89ec-24f3533402fe.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c0f22bd017324d44f72ef735d50e925f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fdddd437a-ebe1-4c88-89ec-24f3533402fe.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c0f22bd017324d44f72ef735d50e925f)

打開模型與系統提示詞

- API 來源：Bedrock Mantle
- 模型：GLM 5

[![スクリーンショット 2026-07-01 23.57.49.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F03b8328c-4a56-4d14-84b4-b5204fecae59.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=66a5bf1e31300406b5b72c6030b302f0)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F03b8328c-4a56-4d14-84b4-b5204fecae59.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=66a5bf1e31300406b5b72c6030b302f0)

在這裡選擇 Mantle（Bedrock 的新基礎架構），即使新 AWS 帳號的 Bedrock 配額是 0，也能呼叫模型。

不過 Claude 和 GPT 通常不會被授權使用，因此改用其他同樣聰明的模型。

啟用工具 > 閘道，選擇剛才建立的閘道，然後按「建立 Harness」

[![スクリーンショット 2026-07-01 23.55.50.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fda11bd6e-63fc-433f-861d-7608f8ddc953.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=303e7ba56afc51875ca529f90fd7d614)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fda11bd6e-63fc-433f-861d-7608f8ddc953.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=303e7ba56afc51875ca529f90fd7d614)

先在這裡選好閘道，這樣系統會自動設定從 AgentCore Harness 呼叫所需的 IAM 政策，很方便。（如果之後才加，會需要手動編輯政策。）

這裡需要 2～3 分鐘，先往下進行。

### 動作確認（RAG）

從剛建立的 KB 畫面中選擇「測試知識庫」

[![スクリーンショット 2026-07-02 0.07.05.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fe2f2e013-be83-4e7c-b06d-f7e923650ebb.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=dbbf65435d64f9b9b451bd6f28c63fd9)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fe2f2e013-be83-4e7c-b06d-f7e923650ebb.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=dbbf65435d64f9b9b451bd6f28c63fd9)

例如輸入 `KAG 也有在做工程師教育服務嗎？` 之類的問題

[![スクリーンショット 2026-07-02 0.10.14.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fcb2957b9-7bd5-47d8-9afa-99877a388e5c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=433e143836e1f2f53f8aa2e58ee4e66c)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fcb2957b9-7bd5-47d8-9afa-99877a388e5c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=433e143836e1f2f53f8aa2e58ee4e66c)

### 動作確認（代理人 + RAG）

從剛建立的 Harness 畫面中選擇「測試 Harness」

[![スクリーンショット 2026-07-02 0.11.09.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fbad92ea8-0547-41ea-b98c-d775104b4333.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=710a4696a2f16eba19751184752d35fe)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fbad92ea8-0547-41ea-b98c-d775104b4333.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=710a4696a2f16eba19751184752d35fe)

例如輸入 `KAG 也有在做工程師教育服務嗎？` 之類的問題

[![スクリーンショット 2026-07-02 0.13.53.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F2f5110f1-1e7d-4d73-be58-fb001b9009e1.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bd144e56790ff8a069c27d9e7c9c5a9b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F2f5110f1-1e7d-4d73-be58-fb001b9009e1.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bd144e56790ff8a069c27d9e7c9c5a9b)

## 新增瀏覽器工具篇

如果你在意費用，可以跳過這裡，直接進入下一個「前端建置篇」，這樣就能用比較低的成本完成整個動手做。

因為在反覆操作時，系統會一邊讀取瀏覽器畫面，一邊進行操作，容易讓送往 Bedrock 的輸入 token 數量暴增。

[![スクリーンショット 2026-07-02 1.29.59.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F4853cfcd-6a6c-4883-9a43-81dc29cce302.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=82e3090f6f79c6c71b1421d8b1ffbdac)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F4853cfcd-6a6c-4883-9a43-81dc29cce302.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=82e3090f6f79c6c71b1421d8b1ffbdac)

從 Harness Playground 右側的「新增工具」加入瀏覽器工具

[![スクリーンショット 2026-07-02 0.18.39.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fdbf8f120-1641-48f5-b0f8-2cfd66d30035.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=70dae9fa69852f11e01aa3355b291b5f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fdbf8f120-1641-48f5-b0f8-2cfd66d30035.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=70dae9fa69852f11e01aa3355b291b5f)

再加上一個問題，例如 `順便幫我用瀏覽器查一下，JAWS-UG 東京也能學到這個嗎？`

[![スクリーンショット 2026-07-02 0.38.44.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F1ee1e14c-0849-4f70-bfb1-fbee20592b8a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ac1ff57a34f51c090f03b5011f8f75e2)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F1ee1e14c-0849-4f70-bfb1-fbee20592b8a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ac1ff57a34f51c090f03b5011f8f75e2)

從右上角的「︙」選單選擇「更新現有的 Harness」，就完成部署了

[![スクリーンショット 2026-07-02 0.41.05.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Ffda901f3-9e7f-4634-997b-cc341529eb7e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b4e500faa43bd1bd70c6bc1f30b1812c)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Ffda901f3-9e7f-4634-997b-cc341529eb7e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b4e500faa43bd1bd70c6bc1f30b1812c)

## 前端建置篇

[![スクリーンショット 2026-07-02 1.30.07.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F4ec3e953-70c0-4859-baa2-7056e6afd6ab.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1027e2c53da9d8c073e341e2a5b12a59)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F4ec3e953-70c0-4859-baa2-7056e6afd6ab.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1027e2c53da9d8c073e341e2a5b12a59)

### 啟動 Web 伺服器

從畫面右上角的「>\_」圖示啟動 CloudShell

[![スクリーンショット 2026-07-02 0.43.28.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F24b1ea64-9314-427f-8aa9-05f975c98c00.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=25f1c4cc74a118c3ef23c952bfcc4837)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F24b1ea64-9314-427f-8aa9-05f975c98c00.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=25f1c4cc74a118c3ef23c952bfcc4837)

建立前端的 Python 檔案

```
nano frontend.py
```

貼上以下程式碼

[frontend.py](http://frontend.py/)

```
import boto3, uuid
import streamlit as st

# 側邊欄
harness_arn = st.sidebar.text_input("請輸入 Harness ARN")

# 頁面標題與聊天輸入框
st.title("告訴我吧！AgentCore")
prompt = st.chat_input("請輸入訊息")

if prompt:
    # 顯示使用者訊息
    st.chat_message("user").write(prompt)

    # 呼叫代理人
    response = boto3.client("bedrock-agentcore").invoke_harness(
        harnessArn=harness_arn,
        runtimeSessionId=str(uuid.uuid4()),
        messages=[{"role": "user", "content": [{"text": prompt}]}],
    )

    # 顯示狀態
    answer = ""
    with st.status("正在思考") as status:
        for event in response["stream"]:
            tool = event.get("contentBlockStart", {}).get("start", {}).get("toolUse")

            # 執行工具時，在畫面上顯示工具名稱
            if tool:
                answer = ""
                status.update(label=f"{tool.get('name')} 工具正在執行中")
            answer += event.get("contentBlockDelta", {}).get("delta", {}).get("text", "")

        # 串流完成後更新狀態
        status.update(label="已完成", state="complete")

    # 顯示代理人的回答
    st.chat_message("assistant").write(answer)
```

依照 nano 畫面底部的說明，按「Ctrl + X」>「y」>「Enter」儲存並關閉

[![スクリーンショット 2026-07-02 0.48.24.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F58c9ad13-3a68-47e8-ad13-04074f64142e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5f67cf804accc6f4d9ee2be22462647e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F58c9ad13-3a68-47e8-ad13-04074f64142e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5f67cf804accc6f4d9ee2be22462647e)

使用以下指令啟動 Web 伺服器

```
# 額外安裝 Python 函式庫
pip install -U boto3 streamlit

# 啟動 Streamlit 應用程式
streamlit run frontend.py
```

### 動作確認

因為要從瀏覽器連到這裡，所以先按加號按鈕開啟第二個 us-east-1 終端機

[![スクリーンショット 2026-07-02 0.51.25.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F48076f55-87ec-496a-bd3f-9fce1a9f10d4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b47d6ee30fbba174809ff5dcd3f6ecdd)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F48076f55-87ec-496a-bd3f-9fce1a9f10d4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b47d6ee30fbba174809ff5dcd3f6ecdd)

使用以下指令，取得可以存取這個 CloudShell 的網址

```
# 下載 Cloudflare Tunnel 用戶端
wget -O cloudflared https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64

# 授予執行權限
chmod +x cloudflared

# 將 Streamlit 的本機埠透過 Tunnel 公開到網際網路
./cloudflared tunnel --url http://localhost:8501
```

[![スクリーンショット 2026-07-02 0.53.20.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F53b40bdf-257f-4666-ac98-4d298d743471.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7c1b31b488864f49e6eec4c3fda14295)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F53b40bdf-257f-4666-ac98-4d298d743471.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7c1b31b488864f49e6eec4c3fda14295)

開啟過程中輸出的網址（ `https://XXXXX.trycloudflare.com` ）

[![スクリーンショット 2026-07-02 0.56.39.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F83e3d9d8-f19d-45aa-bb81-1803c544c5dd.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d368a3aa22641c660171532c5f5e082b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F83e3d9d8-f19d-45aa-bb81-1803c544c5dd.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d368a3aa22641c660171532c5f5e082b)

在管理主控台中複製剛建立的 Harness ARN

[![スクリーンショット 2026-07-02 0.57.21.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F1e16b629-858a-4516-a1f5-9bdc08a810e1.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6eac73e6efe39f3a94004e73f3713b52)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F1e16b629-858a-4516-a1f5-9bdc08a810e1.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6eac73e6efe39f3a94004e73f3713b52)

例如輸入 `幫我查一下 KAGAI Learning Hub 的內部文件` 之類的問題

[![スクリーンショット 2026-07-02 1.02.56.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F051446d3-8fec-4d36-b339-c9e3ad6d6bee.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d0fdb0821944836921e0275cba06c523)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F051446d3-8fec-4d36-b339-c9e3ad6d6bee.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d0fdb0821944836921e0275cba06c523)

## 整理

受管 KB 也會產生儲存空間的按量計費，所以動手做結束後，請從「刪除」按鈕把它移除。

[![スクリーンショット 2026-07-02 1.06.18.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F1ebaa6c2-d083-4c5a-8627-45ec89e609b4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fd694f5a5b39d056f1f01fcbde59e2bf)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F1ebaa6c2-d083-4c5a-8627-45ec89e609b4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fd694f5a5b39d056f1f01fcbde59e2bf)

如果沒有輸入英文的「delete」，就無法刪除ｗ

[![スクリーンショット 2026-07-02 1.07.56.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fc90bdec3-3840-4707-807c-33289fc11136.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ccb99d3c2526f057c619f431a3fea70a)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fc90bdec3-3840-4707-807c-33289fc11136.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ccb99d3c2526f057c619f431a3fea70a)

## 最後來宣傳一下

如果你對 AgentCore 感興趣，不妨再閱讀一下 JAWS-UG 成員寫的書，進一步學習！

[![ビラ2.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F5ef24ce2-90fb-4353-a0e8-896b131fc517.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=0c6b345623874cc7ed924bd618070360)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F5ef24ce2-90fb-4353-a0e8-896b131fc517.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=0c6b345623874cc7ed924bd618070360)

---

原文出處： [https://qiita.com/minorun365/items/7d06434cf830df9c54ff](https://qiita.com/minorun365/items/7d06434cf830df9c54ff)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/anbXGa) [7瀏覽](https://codelove.tw/@tony/post/anbXGa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/anbXGa)

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

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝15 💬3 ❤️3

634

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬3 ❤️3

118

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783044198&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FanbXGa&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783044197160&amp;bpp=3&amp;bdt=497&amp;idt=888&amp;shv=r20260701&amp;mjsv=m202606260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783044161%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783044161%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783044161%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2793244089527&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=988&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1599&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95395662%2C95393485&amp;oid=2&amp;pvsid=1198563768046047&amp;tmod=1091985026&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1599%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=907" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260701/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>