---
title: "OpenDuet 自架教學：開源助理需要哪些服務，模型費用與資料流向怎麼查"
source: "https://www.aiposthub.com/openduet-self-host-dependencies/"
author:
  - "[[Philo]]"
published: 2026-11-11
created: 2026-11-13
description: "OpenDuet 自架教學。把 AI 助理架在自己的主機上，和整套服務都在自己手上，是兩件需要分別確認的事。程式可能開源，但對話、記憶與模型仍會依賴外部服務。"
tags:
  - "clippings"
status: "processed"
---
把 AI 助理架在自己的主機上，和整套服務都在自己手上，是兩件需要分別確認的事。程式可能開源，但對話、記憶與模型仍會依賴外部服務。

OpenDuet 是 CopilotKit 的主動式代理專案，能在獨立電腦環境工作，並讓使用者參與需要決定的步驟。代理就是能使用工具執行任務的 AI。

我會先把它的依賴列清楚，再決定要試用、客製化，或直接採用較完整的 OpenBot。

## OpenDuet 的定位有什麼更新？

2026 年 11 月 11 日讀取的 [官方 README](https://github.com/CopilotKit/OpenDuet?ref=aiposthub.com) ，已把 OpenDuet 同時定位為 OpenBot 可選的介面外觀。原倉庫保留為社群維護的起始範本，團隊用途則引導到 OpenBot。

這讓試用者有兩個不同入口。如果目的是研究主動式助理的介面，可以先看範本，若要多人登入、個別權限與稽核紀錄，應沿官方指引檢查 OpenBot。

OpenDuet 是 CopilotKit 的獨立專案，沒有因為名稱像 Claude Duet，就變成 Anthropic 官方產品。

![CopilotKit 的 OpenBot／OpenDuet 介面截圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/size/w2400/2026/11/openduet-self-host-dependencies.png)

CopilotKit 的 OpenBot／OpenDuet 介面截圖。來源： 官方／作者發布頁 。

## 自架之前要準備哪些服務？

README 說明可以自行選模型，但對話與記憶需要 CopilotKit Intelligence 專案。 [設定文件](https://github.com/CopilotKit/OpenDuet/blob/main/openbot/docs/configuration.md?ref=aiposthub.com) 也列出資料庫、儲存憑證的加密金鑰及 Intelligence 相關設定。

可以先用一張依賴表確認責任：

| 部分 | 要確認的事 |
| --- | --- |
| 程式與執行環境 | 誰更新、備份與排除故障 |
| 模型服務 | 用哪個供應商、誰付生成費 |
| 對話與記憶 | 實際存在哪裡、如何刪除與匯出 |
| 電腦與工具 | 能開哪些網站、能寫哪些服務 |

這張表比「支援自架」四個字更接近部署決定，因為每一列都會影響費用與資料流向。

## 怎麼安排第一個試用任務？

先選沒有敏感資料、可人工核對的任務，例如整理指定公開頁面的更新，輸出成帶來源的草稿。把成功條件寫成「讀了哪些頁、找到哪些變動、結果放哪」。

接著測需要人決定時是否會停下來，以及失敗時能不能找回執行紀錄。這些行為會影響你能否放心把較長任務交出去。

自架也有持續成本。除了模型用量，還要算主機、資料庫、維護時間，以及外部服務方案，開源授權本身無法回答全部費用。

## 常見問題

### 自架代表資料完全不出主機嗎？

要依實際模型、記憶服務與連接器確認。沒有畫出資料流向前，不能只靠自架名稱判斷。

### OpenDuet 和 OpenBot 要選哪個？

依目前 README，想研究或改介面可以看範本，要供團隊使用，先評估官方持續維護的 OpenBot。

### 可以直接接公司帳號嗎？

先用最小任務確認權限和寫入行為，再逐項加入必要服務。試用結果應能讓管理者檢查。

## 先把一個任務跑成可追查結果

把模型、記憶、電腦與外部工具分別列出來，再用公開資料測一次。只有結果和依賴都看得清楚，自架才是一個具體選擇。

延伸閱讀： [Claude Skills 是什麼？免費就能用的 AI 客製化功能完整教學](https://www.aiposthub.com/claude-skills-complete-guide/)

延伸閱讀： [2026 Claude 教學：官方 90 個 Use Cases、12 個工作流程與 Prompt 範例](https://www.aiposthub.com/claude-use-cases-90-workflows-2026/)