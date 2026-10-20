---
title: "效果直逼 ChatGPT！Ideogram 4.0 正式開源：93 億參數圖像模型，支援本地部署與微調"
source: "https://www.koc.com.tw/archives/645075"
author:
  - "[[達小編]]"
published: 2026-06-05
created: 2026-06-10
description: "Ideogram 4.0 改用 結構化 JSON 作為提示詞格式，允許定義："
tags:
  - "clippings"
status: processed
---


以往頂級審美的 AI 圖像生成領域長期由閉源模型主導，從 Midjourney 、ChatGPT 到 Nano Banana….，如果想要擁有頂級品質的 AI 圖像，幾乎都只能靠大型廠商的雲端閉源模型生成能力。近日 Ideogram 打破了這道限制：發表旗下首款開放權重模型 Ideogram 4.0，將 93 億參數的設計級圖像生成基礎模型完整公開，權重放在 Hugging Face，推論程式碼以 Apache 2.0 授權釋出在 GitHub。這不是某個既有模型的微調版本。Ideogram 強調 4.0 是「從零開始訓練的基礎模型」（trained from scratch），架構、訓練資料、文字編碼器全部自行設計。  

### Ideogram 是誰？Google Imagen 核心團隊出走創業

Ideogram AI 於 2022 年在加拿大多倫多創立，創辦團隊共 7 人，其中 4 位是 Google 影像生成模型 Imagen 論文的共同作者。核心人物包括：

- **Mohammad Norouzi** （CEO）：多倫多大學電腦科學博士，就讀期間獲得 Google 機器學習博士獎學金，前 Google Brain 高級研究科學家，Imagen 論文共同第一作者
- **Jonathan Ho** ：聯合創辦人，前 Google Brain 研究員，擴散模型奠基論文《Denoising Diffusion Probabilistic Models》的作者，這篇論文是整個擴散模型浪潮的理論起點
- **William Chan** ：聯合創辦人，前 Google Brain 研究員
- **Chitwan Saharia** ：聯合創辦人，前 Google Brain 研究員，Imagen 論文共同作者

這支團隊的技術血統直接來自 Imagen，而 Imagen 正是 Google 在文字生成圖像領域的核心計畫。從 Google Brain 集體出走創業，某種程度上也說明了這批研究者對「開源」與「獨立發展」的信念。

資金方面，Ideogram 在 2023 年 8 月成立時即完成由 a16z 與 Index Ventures 領投的 1,650 萬美元（約 NT$5.4 億）種子輪融資；2024 年 2 月再獲 8,000 萬美元（約 NT$26 億）A 輪融資。截至 2025 年，團隊規模約 49 人，年度經常性收入（ARR）約 700 萬美元（約 NT$2,275 萬）。

以不到 50 人的團隊做出 Design Arena 開放模型第一名、排版盲測打敗所有競爭者的成績，Ideogram 的人均產出效率在 AI 圖像生成領域堪稱頂尖。

### 模型架構：全單流 Diffusion Transformer

[Ideogram 4.0](https://ideogram.ai/blog/ideogram-4.0/) 採用 **全單流 Diffusion Transformer（DiT）** 架構，34 層，93 億參數。所謂「全單流」，指的是文字 token 與圖像潛在 token 被串接成同一個序列，通過同一組 Transformer 層處理，每個注意力層同時看到兩種模態。這與部分模型將文字和圖像分成兩個分支再對齊的做法不同，理論上能發展出更深度的跨模態表徵。  

架構細節：

- **文字編碼器** ：Qwen3-VL-8B-Instruct（視覺語言模型，僅用文字模式），從 13 個中間層提取隱藏狀態串接，提供從表層 token 理解到深層構圖推理的多尺度語義特徵
- **解碼器** ：凍結的 KL VAE，8× 空間壓縮，128 潛在通道
- **取樣器** ：Euler 流匹配（flow matching），非對稱 CFG（無條件 pass 完全丟棄文字 token）
- **解析度** ：原生最高 2048 × 2048 像素，支援 256 到 2048 之間以 16 為倍數的任意邊長，最長寬比可達 6:1
- **推理預設** ：V4\_TURBO\_12、V4\_DEFAULT\_20、V4\_QUALITY\_48（品質尾端在 t≈0 時降低引導強度）

只有 93 億參數的 DiT 本體是訓練過的，文字編碼器和 VAE 都保持凍結。訓練與推論共用同一套 JSON 提示詞格式，系統會在生成前驗證輸入的 JSON 結構是否合規，不合規的提示詞會被直接拒絕。

### 結構化 JSON 提示詞：不只是打字描述

這是 Ideogram 4.0 最核心的差異化功能。傳統圖像模型只能接受自然語言描述，使用者很難精確控制每個元素的位置和樣式。  

Ideogram 4.0 改用 **結構化 JSON** 作為提示詞格式，允許定義：

- **邊界框（Bounding boxes）** ：以 0-1000 正規化座標指定每個物件的位置和大小（原點左上角）
- **色彩盤** ：每張圖最多 16 個 hex 色彩，每個元素最多 5 個
- **文字元素** ：攜帶字面字串和樣式描述，支援多字體海報設計
- **背景描述、物件描述、風格設定** ：各自獨立欄位

這套系統的意義在於：訓練與推論使用完全相同的格式，模型從一開始就學會「根據結構化指令排列元素」，而不是試圖從一段模糊的文字描述中猜測佈局。對於海報、廣告、社群圖、品牌素材等需要精準排版的場景，這種控制力是自然語言提示詞無法比擬的。  

### 基準測試成績：開放模型之王

Ideogram 4.0 在多個第三方基準測試中交出了亮眼成績：

- **Design Arena** ：在所有開放權重模型中排名第一，整體僅落後 OpenAI 和 Google 的閉源模型。
- **ContraLabs 排版盲測** ：十位頂尖設計師評選，Ideogram 4.0 以 47.9% 的首選率勝出，大幅領先 Gemini 3.1 Flash Image Preview（30.0%）、FLUX.2 \[max\]（15.5%）和 Grok Imagine 1.0（15.0%）。在「你會在實際客戶工作中使用嗎？」這個問題上，設計師給出 3.55/5 的評分，同樣居首。
- **X-Omni 英文 OCR** ：文字渲染準確度達 0.97 分。
- **文字渲染跨模型比較** ：在 9.3B 參數量級中，文字渲染表現超越 Qwen-Image（20B）、FLUX.2 dev（32B）、甚至混元 HunyuanImage 3.0（80B MoE），以更少參數達到更好效果。
- **Text-to-Image Arena** ：品質模式排名第一，整體排名第九。

The Decoder 的 [獨立測試](https://the-decoder.com/ideogram-4-0-drops-as-an-open-weight-model-with-native-2k-resolution-and-improved-text-rendering/) 指出，Ideogram 4.0 在其基準提示詞中「輕鬆超越 Midjourney v8」，與 FLUX 打平，但落後 GPT-Image-2、Nano Banana Pro 和 Luma Uni-1.1。

### 開源細節：權重、授權與部署門檻

Ideogram 4.0 的開放策略分為兩層：

- **推論程式碼** ：Apache 2.0 授權，完全開源，可自由使用和修改
- **模型權重** ：Ideogram 4 Non-Commercial 協議，可在 Hugging Face 下載（需同意條款），非商業用途免費，商業用途需另行洽談授權

權重提供兩種量化版本：

- **NF4 版本** ：可在單張 24GB VRAM 的顯示卡上運行（如 RTX 4090），僅支援 CUDA
- **FP8 版本** ：支援更廣泛的硬體平台

這意味著擁有消費級高階顯示卡的創作者，可以在本地完全掌控這款模型。不需要依賴雲端服務，不需要將資料傳到外部伺服器，還可以根據自己的品牌風格進行微調（fine-tuning）。對於重視隱私和資料主權的企業和個人創作者而言，這是一個相當有吸引力的選項。

### Ideogram 4.0 Github 頁面

### 平台整合：不只是模型，是一個生態系

Ideogram 4.0 並非只有一個孤零零的模型檔案。Ideogram 同步開放了多個平台的使用管道：

- **官方網站** ： [ideogram.ai](https://ideogram.ai/) ，提供網頁版生成介面
- **API** ： [developer.ideogram.ai](https://developer.ideogram.ai/) ，三種品質層級（Turbo $0.03/張、Standard $0.06/張、Quality $0.09/張）
- **訂閱方案** ：Basic $8/月（400 張優先生成）、Plus $20/月（1,000 張）、Pro $60/月（3,000 張），所有方案都包含無限慢速生成
- **ComfyUI** ：已支援完整工作流程，需要 32GB VRAM 運行 FP8 模型
- **合作平台** ：Hugging Face、fal、Runware、Magnific、Krea AI、Leonardo AI、Picsart、Cloudflare、Replicate、Gamma、Flora AI、Kittl 等十多個平台同時上線

ComfyUI 的整合尤其值得注意。社群已經建立了完整的工作流程，從模型下載、節點配置到生成範例都有詳細教學，這大幅降低了本地部署的技術門檻。我自己也成功在本地端完成部屬並生成圖片，水準真的非常不錯，圖片大小也可由 1024 起跳：  

### 產品功能：設計工作流不只生成

除了模型本身，Ideogram 的產品端也同步更新了多項設計導向功能：

- **提示詞編輯** （Prompt Edit）：在已生成的圖像上直接修改特定區域
- **原生透明背景** ：直接輸出 PNG 透明圖層
- **分層文字** ：文字元素可獨立編輯
- **擴展與重新構圖** （Extend / Reframe）：向外擴展畫面或改變比例
- **放大** （Upscale）：提升解析度
- **混搭** （Remix）：基於現有圖像重新生成變體
- **魔術填充** （Magic Fill）：局部區域填補
- **MCP 整合** ：支援 Agent 工作流程

可編輯文字和圖層功能即將推出。

### 對產業的意義：開源追上前線

Ideogram 4.0 的發布代表一個重要訊號： **開放權重模型在設計級圖像生成領域，已經逼近閉源前線** 。在 Design Arena 上，Ideogram 4.0 僅落後 OpenAI 和 Google 的閉源模型，而在排版和文字渲染這兩個專業設計師最關心的維度上，它甚至超越了所有競爭者。

這種策略讓人想起 Meta 在 LLM 領域的做法：用開放權重換取社群生態，用生態系鞏固技術地位。Ideogram 選擇在圖像生成領域走同樣的路，而且是在一個仍以閉源為主流的市場中。對於擔心資料隱私、需要品牌客製化微調、或者單純想在本地運行的使用者而言，這是一個前所未有的選項。

當然，商業授權仍然是門檻。模型權重的 Non-Commercial 協議意味著企業若要將其整合進產品或服務中，需要另外付費取得商業授權。這是開源與「免費使用」之間的界線，也是 Ideogram 的商業模式所在。

Tags:

#### 您也許會喜歡：

[立達合法徵信社-讓您安心的選擇](https://www.liidda.com.tw/)

深入瞭解

平面設計

軟體

開發工具

Tags:

## 國外安全研究員砸 1,500 美元實測 13 款 AI 駭客能力：GPT-5.5 奪冠，Gemini 幾乎直接放棄

## GPT-5.5 的分數遙遙領先

![Rocky](https://secure.gravatar.com/avatar/53fa886a03c5073b8758dac38c5a7fca2ef94397b2c86328ddb7029458b26d7f?s=80&d=mm&r=g) by

[2026 年 06 月 05 日](https://www.koc.com.tw/archives/645064)

in [AI 趨勢與相關新聞](https://www.koc.com.tw/archives/category/ai/ai-news)

讀取中...

隨著 AI 越來越強大，很多人一定會好奇，如果是用在資安的場景，能不能真的找出漏洞？國外資安研究員 Kasra Rahjerdi 最近就做了這項實驗，故意設計一款有漏洞的假 App，然後花了 1,500 美元，測試 GPT-5.5、Claude、Gemini、DeepSeek、Qwen、Kimi 等多款模型能不能找出真正漏洞。

沒想到，原本以為 Claude 在這塊應該會領先，結果是 GPT-5.5 奪冠，在 10 次測試中成功 7 次，是這次成功率最高的模型，而且這次數還遙遙領先其他對手。而看成本效率的話，DeepSeek V4 Pro 相當不錯，每次成功平均只花 0.62 美元。至於 Gemini，這次表現就有點慘，跑很多幾乎一開始就因為安全理由拒絕繼續，完全不想嘗試。

![國外安全研究員砸 1,500 美元實測 13 款 AI 駭客能力：GPT-5.5 奪冠，Gemini 幾乎直接放棄 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/06/20260605180830_0_97dba2.jpg "國外安全研究員砸 1,500 美元實測 13 款 AI 駭客能力：GPT-5.5 奪冠，Gemini 幾乎直接放棄 - 電腦王阿達")

## GPT-5.5 在資安實測 10 次成功 7 次，DeepSeek V4 Pro 每次解題成本只要 0.62 美元，Gemini 幾乎直接拒絕

[根據 Kasra Rahjerdi 說明](https://kasra.blog/blog/i-spent-1500-seeing-if-llms-could-hack-my-app/) ，這次測試目標是一款假的書籍評論 App，前端使用 React Native / Expo 製作，後端則是 Python。表面上看起來，它就是一個可以看書評、排行榜、使用者資料的 App，任務是要找出藏在某位使用者私人評論裡的 flag。

  
圖片來源： [Kasra Rahjerdi](https://kasra.blog/blog/i-spent-1500-seeing-if-llms-could-hack-my-app/)

Kasra 也提到，這款 App 的 API 本身其實相對安全，真正的問題出在資料層。由於 App 使用 Firebase，而且內部的 google-services.json 含有 Firebase 相關資訊，因此正確方向並不是一直嘗試突破 API，而是找出 Firebase 權限設定上的漏洞，進而讀取 Firestore 裡原本不該被外部讀取的資料。

測試方面，他原本想讓每個模型跑滿 10 次，但成本已經燒到 1,500 美元，只好中途收手。他也特別強調，這不是正式科學化評測，而是一個有紀錄、但樣本數有限的個人實驗。

每次測試的預算上限為 10 美元、執行時間上限為 2 小時。

除了 Claude 採用 Claude Code 的 -p 模式之外，其餘模型大多透過 pi 這套測試框架（harness），搭配 pi-goal-x extension 執行，目的是讓模型在遇到失敗或卡關時，還是能持續嘗試不同方法完成任務。

  
圖片來源： [Kasra Rahjerdi](https://kasra.blog/blog/i-spent-1500-seeing-if-llms-could-hack-my-app/)

最終表現最好的是 GPT-5.5，10 次測試成功 7 次，解題率達 70%。平均每次測試成本為 6.62 美元，每次成功解題成本為 9.46 美元，中位數 token 使用量約 260k。

Kasra 觀察到，GPT-5.5 多數成功案例在解開 APK 後，很快就把重點轉向 Firebase，而不是被 API 或 React Native App 本身牽著走，這也成為它在這項任務中最明顯的優勢。

DeepSeek V4 Pro 的成功率雖然不如 GPT-5.5，10 次測試中成功 3 次，但成本效率非常突出。它平均每次測試只花 0.19 美元，每次成功解題成本也只有 0.62 美元，明顯比 GPT-5.5 低很多。

Claude 表現就普普，Sonnet 4.6 和 Opus 4.8 都是 10 次成功 2 次，但成本偏高。Kasra 提到，Opus 4.8 有幾次其實非常接近答案，只是最後因安全 guardrails 在後段中止，不是像 Gemini 幾乎一開始就拒絕。

Gemini 這次明顯受到安全政策影響。Gemini 3.1 Pro Preview 和 Gemini 3.5 Flash 都是 10 次成功 0 次，其中 Gemini 3.1 Pro Preview 的中位 token 使用量只有 9k，明顯低於其他模型，代表大多數很早就拒絕任務。

  
圖片來源： [Kasra Rahjerdi](https://kasra.blog/blog/i-spent-1500-seeing-if-llms-could-hack-my-app/)

至於其他模型，DeepSeek V4 Flash、MiniMax M2.7、Step 3.7 Flash 都是 10 次成功 0 次。

Qwen 3.7 Max 測了 6 次也沒有成功。Kasra 對 Qwen 的結果相當失望，因為在本地測試時，它曾是少數能完成任務的非 GPT 模型，但正式長時間測試沒能重現成功。

Tags:

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-3801907256397861&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1781041558&amp;plat=1%3A16777216%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fwww.koc.com.tw%2Farchives%2F645075&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuNTQiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE0OS4wLjc4MjcuNTQiXSxbIkNocm9taXVtIiwiMTQ5LjAuNzgyNy41NCJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1781057629848&amp;bpp=1&amp;bdt=269&amp;idt=589&amp;shv=r20260604&amp;mjsv=m202606040101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D7eb900325fb958cb%3AT%3D1764142964%3ART%3D1781057491%3AS%3DALNI_MbVZE1liWGMfPz_VpdO7wIdzmxt6g&amp;gpic=UID%3D000011bd03f7b5c2%3AT%3D1764142964%3ART%3D1781057491%3AS%3DALNI_MbBGuuzozlOZeftw8VbLcX8blwv4w&amp;eo_id_str=ID%3D5dcf144d7d675b57%3AT%3D1781055859%3ART%3D1781057491%3AS%3DAA-AfjYBnGeiCZMK9bNGnZmpq_-G&amp;nras=1&amp;correlator=370749279652&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390668&amp;oid=2&amp;pvsid=509404529594074&amp;tmod=728121441&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fwww.koc.com.tw%2Farchives%2Fcategory%2Fai&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;plas=247x636_l%7C247x636_r&amp;bz=0&amp;pgls=CAEaBTYuOC41~CAEQBRoGMy4zMS41&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=614" title="Advertisement" aria-label="Advertisement"></iframe>

No Result

View All Result

- [最新科技新聞](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac/3c%e6%96%b0%e5%93%81%e6%90%b6%e5%85%88%e5%a0%b1)
- [最新科技產品報導](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac)
- [好用App介紹](https://www.koc.com.tw/archives/category/android%e8%bb%9f%e9%ab%94%e5%88%86%e4%ba%ab%e8%88%87%e5%88%b7%e6%a9%9f%e6%95%99%e5%ad%b8)
- [電腦應用與其他教學](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac/%e9%9b%bb%e8%85%a6%e6%87%89%e7%94%a8%e8%88%87%e5%85%b6%e4%bb%96%e6%95%99%e5%ad%b8)
- [旅遊與美食](https://www.koc.com.tw/archives/category/%e7%94%9f%e6%b4%bb%e8%88%87%e6%97%85%e9%81%8a/%e6%97%85%e9%81%8a%e8%88%87%e7%be%8e%e9%a3%9f)
- [阿達自製工具](https://www.koc.com.tw/speedtest)
- [電腦王阿達團隊](https://www.koc.com.tw/contact)

No Result

View All Result

- [最新科技新聞](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac/3c%e6%96%b0%e5%93%81%e6%90%b6%e5%85%88%e5%a0%b1)
- [最新科技產品報導](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac)
- [好用App介紹](https://www.koc.com.tw/archives/category/android%e8%bb%9f%e9%ab%94%e5%88%86%e4%ba%ab%e8%88%87%e5%88%b7%e6%a9%9f%e6%95%99%e5%ad%b8)
- [電腦應用與其他教學](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac/%e9%9b%bb%e8%85%a6%e6%87%89%e7%94%a8%e8%88%87%e5%85%b6%e4%bb%96%e6%95%99%e5%ad%b8)
- [旅遊與美食](https://www.koc.com.tw/archives/category/%e7%94%9f%e6%b4%bb%e8%88%87%e6%97%85%e9%81%8a/%e6%97%85%e9%81%8a%e8%88%87%e7%be%8e%e9%a3%9f)
- [阿達自製工具](https://www.koc.com.tw/speedtest)
- [電腦王阿達團隊](https://www.koc.com.tw/contact)

![Buy Me a Coffee](https://cdn.buymeacoffee.com/widget/assets/coffee%20cup.svg)