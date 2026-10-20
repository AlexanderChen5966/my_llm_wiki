---
title: "AI agent 絕對不能交辦的事情，以及我在專案中使用的 AGENTS.md 規則"
source: "https://codelove.tw/@tony/post/xXQ5W3"
author:
published:
created: 2026-06-08
description: "![f5480eb2-b8de-408e-bd0c-c4147c6a3c0c.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月06日](https://codelove.tw/@tony/post/xXQ5W3 "2026-06-06 17:03:14")

## AI agent 絕對不能交辦的事情，以及我在專案中使用的 AGENTS.md 規則

[![f5480eb2-b8de-408e-bd0c-c4147c6a3c0c.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4240966%2F7d1ef161-a105-4229-8c86-8942c59e1237.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=dadb557eeba502801b24eb03b7a5ec22)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4240966%2F7d1ef161-a105-4229-8c86-8942c59e1237.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=dadb557eeba502801b24eb03b7a5ec22)

※ 這篇文章的日文可能有些不自然的地方。本文是在使用 AI 語言支援的情況下完成的。

## 目次

## 前言

你有讓 AI 代理寫程式嗎？  
很棒！

不過，我曾經因為代理「確實按照指示正確執行」而浪費了一個小時。

**這是真實故事** ：我曾指示代理「整理儲存庫並刪除未使用的檔案」。結果代理把「未使用」解讀得過於廣泛，連部署腳本仍在參照的設定目錄也刪掉了。那個設定檔因為被寫進 `.gitignore` ，所以不在 Git 管理之下。

結果：我花了一個小時才把被刪掉的檔案復原。

教訓是： **判斷哪些任務應該交給 AI** 。這是與 AI 代理一起工作時最重要的技能。

以下我會詳細分享我的實戰經驗。

---

## 權限矩陣：何時交給代理，何時由人類審查

任務類型 代理自主度 人類審查 原因／風險 小型函式重構 **高** 不需要或只需輕微 因為很容易 revert，風險低。 新增單元測試 **高** 通常不需要 CI 會立即偵測到。 修正前端 bug（UI） **中～高** 需要目視確認 邏輯 bug 看得出來，但 UI 的細微跑版仍需要人眼判斷。 API 行為變更 **中** 需要 會影響客戶端，必須審查契約。 新增相依套件 **中** 需要 可能有安全漏洞或授權問題。 資料庫 migration **低** 必須 錯誤的 migration 可能造成資料遺失或 schema 損壞。 身分驗證／安全性邏輯 **低** 必須 這類 bug 不會被單元測試發現，往往要等到事件通報才會暴露（甚至可能是數個月後）。 基礎設施／雲端（IAM、k8s、Terraform） **非常低** 必須 權限設定錯誤會悄悄破壞系統，而且很難追查。 上線部署到正式環境 **無（僅限手動）** 必須＋手動確認 代理不知道當前是否有事故或維護排程。 簡單規則： **凡是無法輕易復原的操作，一律要設人類檢查點。**

---

## 依風險等級的實例

等級 任務範例 如果代理做錯的影響 能否復原？ **高自主** 變數名稱重構 會造成 build error，CI 會立刻偵測到，5 分鐘內可修正 ✅ 容易 **中** 變更 API 回應格式 呼叫端會報錯，但只要在正式部署前發現就沒問題 ⚠️ 可透過 commit revert **低** 刪除資料庫欄位 造成資料遺失。從備份復原可能需要數小時到數天 ❌ 非常困難 **無自主** 凌晨 3 點在正式環境部署 沒人會注意到，影響客戶，而且值班人員可能不在 ❌ 非常困難

---

## 絕對不能交給 AI 代理的 6 件事

[![05e935b7-fc07-4d94-a9d1-b31ce13e2724.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4240966%2F352f0aac-b005-4767-aa63-98382d56c40d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=545059a60f420baf8b21696c6ff7c263)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4240966%2F352f0aac-b005-4767-aa63-98382d56c40d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=545059a60f420baf8b21696c6ff7c263)

### 1\. 破壞性的檔案操作

- `rm -rf` 、 `git clean -fd` 、 `git reset --hard`
- 代理看到「clean up」這種字眼時，可能會在重構過程中直接執行 `git clean -fd` 。未提交的工作會全部消失。

### 2\. 資料庫寫入／刪除

- 沒有 `WHERE` 條件的 `DELETE` 、 `DROP TABLE` 、 `TRUNCATE` 、會動到正式資料的 migration
- 只要 `WHERE` 條件打一個字錯，整張表就可能被刪光。代理不會幫你再確認。

### 3\. 基礎設施（雲端）

- `terraform apply` 、 `kubectl delete` 、 `aws iam *`
- 權限變更特別危險。它會悄悄進行，直到某個東西壞掉才被發現。

### 4\. 上線部署到正式環境

- 即使程式碼是代理寫的，是否部署的決定仍應由你來做。
- 代理不知道是否有進行中的事故，也不知道維護時程。

### 5\. 身分驗證／安全性邏輯

- 驗證流程、授權規則、權杖處理、session 管理
- 這類 bug 不會在單元測試中被發現，通常會在事件報告中才暴露，有時甚至是數個月後。

### 6\. Secrets、.env 檔、API 金鑰

- 不要讓代理讀取或寫入這些檔案。

---

## AGENTS.md：與 AI 的契約書

在儲存庫根目錄放置 `AGENTS.md` 檔案。這個檔案會告訴代理「專案目的」「如何執行」「 **哪些東西不能碰** 」。

### 我在使用的範本

```
# AGENTS.md

## Project
[專案簡介與技術堆疊]

## Setup
\`\`\`bash
npm install   # 或 pip install -r requirements.txt
npm run dev
npm test
npm run lint
```

## Coding rules

- 只做最小必要變更，不要重構無關程式碼。
- 如果改變了行為，就要新增或更新測試。
- 不要碰超出任務範圍的檔案。
- 1 個 commit 只處理 1 件事。

## Safety rules

執行 `blocked_commands.md` 中列出的指令前，一律先請求確認。  
如果不確定是否安全 → 停下來確認。

## Definition of done

- 測試通過
- 差異能用一句話說明
- 有最終報告（見下方）

## Final report format

每次任務結束後，代理都必須提供以下內容：

1. 變更概要
2. 變更的檔案
3. 執行的測試與結果
4. 風險或前提條件
5. 尚未完成的事項
```
### blocked_commands.md（執行前需要批准）

\`\`\`markdown
# blocked_commands.md

## 破壞性的檔案操作
- rm -rf
- git clean -fd
- git reset --hard

## Git 操作
- git push --force
- git push --force-with-lease

## 資料庫操作
- DROP TABLE
- TRUNCATE TABLE
- 沒有 WHERE 條件的 DELETE
- 會修改正式環境 schema 的 migration

## 雲端／基礎設施
- terraform apply
- kubectl delete
- aws iam *
- gcloud iam *

## Secrets
- 讀寫 .env 檔的指令
- 會接觸 API 金鑰或認證資訊的指令
```

---

## Two-Agent Loop：一個負責寫，另一個負責審查

對於中等以上複雜度的任務，不要只用 1 個代理，而是用 **2 個** 。

### 代理 1 – Implementer（負責寫程式）

**提示詞：**

```
你是資深工程師。任務：[任務說明]
Context: [AGENTS.md 連結]
規則：
- 只做最小必要變更。
- 不要重構無關程式碼。
- 如果改變了行為，就新增測試。
- 最後請提供最終報告（概要、變更檔案、測試、風險、未完成事項）。
```

### 代理 2 – Reviewer（負責審查）

**提示詞：**

```
你是程式碼審查者，對實作沒有任何情感依附。
請審查這個 diff：[貼上 diff]
檢查項目：
- bug 與邊界案例
- 缺少的測試
- 安全性問題
- 非預期的行為變更
- 定義範圍外的內容

輸出：
- Critical issues（必須修正）
- Minor issues（可選）
- 應該提醒人類注意的事項

不要重寫程式碼。只要標記問題，不要修正。
```

### 為什麼有效？

審查代理對程式碼沒有「自我意識」。它能找出實作者漏掉的 bug 和邊界案例。這就是 **自動化程式碼審查** 。

---

## Final Report：讓代理負起責任

每個代理任務完成後，都要產出最終報告：

1. 變更概要
2. 變更的檔案
3. 執行的測試與結果
4. 風險／前提條件
5. 尚未完成的事項

如果代理無法清楚總結 → 這代表任務本身不夠乾淨。

另外，這些報告會累積成自動化文件。當一週後出現 bug 時，你可以精確追蹤到底改了什麼、為什麼改。

---

## 總結

AI 代理確實能提升你的 **生產力** 。但最能充分發揮它的人，往往是有把這些看似平凡的事情做好的人：

- 撰寫 `AGENTS.md`
- 設計權限等級
- 建立 `blocked_commands.md`
- 設定 Two-Agent Loop

代理在指令清楚時表現最好。那一部分是 **你的** 責任。

---

## 結語

感謝你閱讀到這裡。

如果你有 **「想委託系統開發」「想諮詢技術問題」「有想透過 AI 解決的課題」** 等需求，歡迎隨時聯絡我們 **TOMOSIA VIETNAM** 。

### 我們的主要服務

- 💻 **軟體委託開發** （Web、App、系統）
- 🤖 **AI 解決方案** （聊天機器人、影像處理、LLM 應用）
- 📱 **行動應用程式開發** （iOS / Android）
- 🔌 **Fintech / IoT 開發**
- 🔧 **BrSE（橋接工程師）支援** （讓日文溝通更順暢）

### 我們的優勢

- **已取得 ISO/IEC 27001（資訊安全）認證** ，可放心託付
- **日語對應團隊** ，由理解日本文化與商業習慣的成員直接對應
- **「Win-Win, Happy Together」文化** ，重視長期夥伴關係

🔗 **官方網站**: [tomosia.com](https://tomosia.com/)

歡迎先簡單來信或聯絡我們一句。讓我們一起用 **「越南的智慧」挑戰世界** 吧！

---

原文出處： [https://qiita.com/TOMOSIA-HieuNT/items/411d47f0d03b2fc7d20b](https://qiita.com/TOMOSIA-HieuNT/items/411d47f0d03b2fc7d20b)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xXQ5W3) [21瀏覽](https://codelove.tw/@tony/post/xXQ5W3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xXQ5W3)

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

📝6 💬2

337

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780888508&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxXQ5W3&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMjE2IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4yMTYiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjIxNiJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780888508360&amp;bpp=1&amp;bdt=88&amp;idt=50&amp;shv=r20260604&amp;mjsv=m202606020101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780888487%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780888487%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780888487%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=758656295619&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390667%2C42533294%2C31061690&amp;oid=2&amp;pvsid=1454647984308290&amp;tmod=473573085&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=68" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260604/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>