---
title: "讓AI不再冷冰冰！Hugging Face發表超萌機器鴨Microduck，一隻399美元值得嗎？"
source: "https://www.bnext.com.tw/article/92035/hugging-face-microduck-robot-399"
author:
  - "[[李先泰]]"
published: 2026-08-28
created: 2026-08-28
description: "Hugging Face推出399美元機器鴨Microduck，軟體開源但硬體設計未公開。預購初期約每 4 秒即售出 1 台，預計聖誕節前交付。"
tags:
  - "clippings"
status: "processed"
---
AI 開源平台 Hugging Face 於 2026 年 8 月 27 日發表雙足機器人 Microduck 並同步開放預購，官網預購優惠價為 399 美元（約合新台幣 1.26 萬元），這隻機器鴨身高僅 25 公分、重不到 800 公克，造型如同一隻單眼鴨。它不僅能流暢行走、踢球、用鳥喙夾取物品，裝上專屬滾輪後甚至還能溜冰。

Hugging Face 共同創辦人暨首席科學家 Thomas Wolf 於 8 月 27 日接受《Bloomberg Tech》節目訪問時透露，Microduck 預購開放初期，平均約每 4 秒就售出 1 台，當時累積預購金額已達約 50 萬美元（約合新台幣 1,580 萬元）。

不過，現在下單無法立即取貨，官方表示首批產品的「目標」交付期落在 2026 年聖誕節前，且此為預估時間，並非硬性的出貨承諾。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

## 399 美元能買到什麼？

硬體規格方面，Microduck 搭載 15 顆馬達、1 顆相機、LiDAR（光達）與 2 顆 IMU（慣性測量單元），並配有一副具抓取功能的鳥喙，官方標示整機重量約 780 公克。399 美元的基本款包含：機器人本體、電池、USB-C 充電線與一支遊戲控制器。機身提供四種配色，出廠即內建 7 種已訓練好的預設動作，運算更新率達 50 Hz（每秒更新 50 次動作指令）。

需要注意的是，官方展示影片中的許多進階玩法多需額外加購。包含雷射筆、滾輪、球體與 NFC 物件的配件包（Accessories Pack）售價 39 美元（約新台幣 1,230 元）；充電器與兩顆備用電池組合（Charger & 2 Spare Batteries）同為 39 美元；針對進階開發者推出的開發套件（Dev Pack）則定價 119 美元（約新台幣 3,760 元）。

其他整合感測器還包括麥克風、喇叭、藍牙與 Wi-Fi 連線功能。頭部與鳥喙各有一組 NFC 感應天線，可用來感應外接配件。供電方面採用一顆可拆換的 NP-F550 相機電池，續航力約 1 小時，實際時間視使用情況而定。

Microduck 的內建行為讓它開箱即可與周遭環境互動，使用者能帶著它四處走動，或透過遊戲控制器觸發動作。至於用雷射筆逗弄、踩滾輪等進階玩法，所需道具皆包含在上述的擴充配件包中。

## 先在虛擬環境訓練，再同步至實體機器鴨

開發團隊 Pollen Robotics 在官方產品頁將工作流程拆解為四個步驟：在物理模擬環境中訓練行為（可運行於本地端機器或 Hugging Face Jobs）、一鍵部署至實體機器人、微調重訓後再次部署，最後將訓練好的控制策略（Policy）分享給開源社群。

本次的「開源」範圍僅限於軟體：SDK、模擬環境與強化學習工具等軟體堆疊，均採 Apache-2.0 授權於 GitHub 釋出，物理模擬則採用 MuJoCo。簡單來說，機器人運行的程式碼皆可讀取、複製修改（Fork）與重新訓練。然而，官方新聞包（Press Kit）也明確指出，機械與電子的硬體設計檔並未開源，因此嚴格來說它不算「開源硬體」。此外，部分軟體工具目前仍在開發中，預計至首批出貨前才會陸續釋出。

據《Bloomberg》報導，未來 Microduck 有望支援 Python、JavaScript 等語言來建立新行為；不過官方坦言 SDK 支援的語言仍在確認中，截至 2026 年 8 月 28 日皆為暫定。該報導也將此概念類比為已停產的樂高 Mindstorms，並認為它延續了 Raspberry Pi 等鼓勵開發者動手改裝硬體的平台脈絡。

## 賣萌背後，是一場收購傳聞與一門新生意

「我們想證明 AI 其實非常平易近人，」Wolf 向《Bloomberg》表示，他認為 AI 不必總是昂貴或令人心生畏懼，「這是任何人都能打造、都能動手做的東西。」他也直言，公司希望呈現「較為正面、可愛的 AI 樣貌。許多人形機器人看起來不總是那麼友善，甚至帶點反烏托邦色彩，但它們其實可以很可愛、很討喜。」

Microduck 由 Hugging Face 旗下的 Pollen Robotics 開發。這家位於法國波爾多的公司於 2025 年 4 月被收購，其先前推出的首款機器人 Reachy Mini 同樣走開源路線，據稱已售出約 1 萬台。綜合《Bloomberg》8 月 27 日的報導，公司期望 Microduck 銷量能突破 2 萬台，該產品目前由深圳的開源硬體商 Seeed Studio 在中國代工製造。

截至 2026 年 8 月 28 日，《The Information》報導輝達 (NVIDIA) 已同意以 129 億美元（約合新台幣 4,080 億元）收購 Hugging Face，但《Business Insider》指出雙方尚未簽署協議、交易仍可能生變，兩家公司也都沒有公開證實。

把 AI 包成可愛外型的不只 Hugging Face。《Bloomberg》指出，人形機器人與 AI 業者正面臨愈來愈多檢視，除了倫理疑慮，還包括機器人外觀有時令人不安；例如 2026 年被亞馬遜 (Amazon) 收購的 Fauna Robotics，同樣採用更偏向消費者、較具親和力的設計。

> 延伸閱讀： [比爾蓋茲警告：AI恐掀人類史最動盪時期！他提「AI稅、人類保留區」3解方](https://www.bnext.com.tw/article/92025/bill-gates-turbulent-ai-era-human-reserved)

資料來源： [《Bloomberg》](https://www.bloomberg.com/news/articles/2026-08-27/hugging-face-unveils-400-singing-skating-duck-like-robot) 、 [Pollen Robotics 官方產品頁](https://pollen-robotics.com/microduck/) 、 [Pollen Robotics 官方 press kit](https://pollen-robotics.com/microduck/press-kit/) 、 [Pollen Robotics 官方商店](https://store.pollen-robotics.com/products/microduck) 、 [《TechCrunch》](https://techcrunch.com/2026/08/26/nvidia-closes-in-on-hugging-face-acquisition/) 、 [Hugging Face 收購 Pollen Robotics 公告](https://huggingface.co/blog/hugging-face-pollen-robotics-acquisition)

本文初稿為AI編撰，整理．編輯/ 李先泰