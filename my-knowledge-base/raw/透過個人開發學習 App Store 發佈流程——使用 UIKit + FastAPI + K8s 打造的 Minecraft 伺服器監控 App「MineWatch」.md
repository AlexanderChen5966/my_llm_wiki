---
title: "透過個人開發學習 App Store 發佈流程——使用 UIKit + FastAPI + K8s 打造的 Minecraft 伺服器監控 App「MineWatch」"
source: "https://codelove.tw/@tony/post/qen4zx"
author:
published:
created: 2026-07-22
description: "[](#%E5%80%8B%E4%BA%BA%E9%96%8B%E7%99%BC%E3%81%A7app-store%E3%83%AA%E3%83%AA%E3%83%BC%E3%82%B9%E3%83%95%E3%83%AD%E3%83%BC%E3%82%92%E5%AD%A6%E3%81%B6--..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月20日](https://codelove.tw/@tony/post/qen4zx "2026-07-20 17:15:11")

## 透過個人開發學習 App Store 發佈流程——使用 UIKit + FastAPI + K8s 打造的 Minecraft 伺服器監控 App「MineWatch」

## 透過個人開發學習 App Store 上架流程 ― 以 UIKit + FastAPI + K8s 製作的 Minecraft 伺服器監控應用「MineWatch」

你是不是也有這種經驗：自己在家裡或和朋友一起架設的 Minecraft Java Edition 伺服器，偏偏每次最想玩的時候就當機了。等你打開客戶端、準備登入的那一刻才發現，「啊，又掛了」。

休閒遊戲

為了解決這個問題，我用 iOS（UIKit）+ FastAPI + PostgreSQL + Kubernetes 個人開發了 **Minecraft 伺服器監控應用「MineWatch」** 。我已於 2026-07-19 將 1.0.0 提交至 App Store，目前正在等待審查。

這篇文章比起「介紹應用功能」，更著重在 **個人開發到這個規模時，為什麼會採用這樣的架構，以及為了學習 App Store 上架流程而特別意識到的設計取捨** 。

## TL;DR

- 最主要的目的是 **學習 App Store 的上架流程** 。我和實作是否正確一樣重視「為什麼這樣做」的紀錄。
- 監控採用 **Server List Ping（SLP）** ——也就是 Minecraft 用戶端在伺服器列表畫面所使用的公開協定——因此 **被監控的伺服器端完全不需要修改** 。
- API 程序與監控執行分離為 `worker` / `beat` 。即使 API/Web 掛掉，已經排進佇列的監控任務仍會繼續執行。
- 推播通知不經由 FCM，而是 **直接送到 APNs** 。如此一來，iOS 端就不需要整合 Firebase 的推播 SDK。
- 分支管理採用 git-flow。PR 會由 GitHub Copilot CLI 自動審查，並以 `VERDICT` 為觸發條件，由 GitHub Actions 自動決定合併或轉人工審查。

## 應用解決的問題

MineWatch 透過 **SLP** 從外部取得伺服器狀態。SLP 是 Minecraft 用戶端在多人遊戲畫面中使用的相同公開協定，因此對監控對象的伺服器不需要安裝外掛或修改設定。可取得的資訊如下：

- 在線 / 離線狀態
- 目前連線人數 / 最大人數
- 回應時間（延遲）、版本、MOTD
- 人數趨勢圖、運作率
- 伺服器當機時的推播通知（直接送 APNs）

## 整體架構

[![minewatch-arch.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351153%2F57d8ef26-0d26-4d6f-9c94-f31f314701f6.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=0c38342b6d70d9149ab085ea42bd8c93)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351153%2F57d8ef26-0d26-4d6f-9c94-f31f314701f6.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=0c38342b6d70d9149ab085ea42bd8c93)

重點有兩個：

1. **監控執行與 API 程序分離** 。 `beat` 會把週期性任務丟到 Redis， `worker` 負責執行，偵測到當機後直接送 APNs 推播。即使 API 掛掉，已在佇列中的任務仍會繼續執行。
2. **通知不經由 FCM** 。直接送 APNs 可讓 iOS 端不必攜帶 Firebase 推播 SDK。

## 技術堆疊

| 層級 | 技術 |
| --- | --- |
| iOS | UIKit（無 Storyboard / XIB）、MVVM、 `@Observable` 、Swift 6 strict concurrency、XcodeGen + SPM |
| API | FastAPI（Python 3.13 / uv）、SQLModel + Alembic |
| 非同步處理 | Celery（worker / beat）+ Redis |
| 資料庫 | PostgreSQL 17 |
| 驗證 | Firebase Authentication（Apple / Google 登入。後端驗證 ID token） |
| 推播通知 | 直接送 APNs（`.p8` 金鑰。不使用 FCM） |
| 容器 / 編排 | Docker、Kubernetes（F5 NGINX Ingress Controller） |
| GitOps | Argo CD |
| CI/CD | Jenkins（Mac 節點 + k8s Pod agent）、GitHub Actions |
| 邊緣 / 網路 | Cloudflare（DNS / Tunnel / Access） |

會在個人開發中組到這樣的基礎架構，一部分確實是出於興趣；但更大的原因是我意識到， **如果要穩定通過 App Store 審查，就需要一套能持續維持可運作後端的機制** 。我不希望在審查期間 API 掛掉、導致無法重現問題這類事故發生，所以至少把監控執行系統的可用性獨立切開了。

程式設計

## API 設計方針

行動應用不可能期待所有使用者都同時更新，因此後端 API 的設計重點放在「如何維持向下相容」。

- **目錄＝版本** ：採用 `app/api/<version>/<domain>/router.py` 的結構，啟動時由 `discover_routers()` 自動註冊。只要切出目錄並放入 `router.py` ，就能新增路由。
- **破壞性變更時凍結 v1 並新增 v2** 。邏輯集中在 `services/` ，router 只保留各版本的薄型適配層，以避免重複實作。
- **資料庫型別變更採用 Expand-Contract** （新增欄位 → 遷移 → 切換 → 刪除）。由於舊程式與新程式一定會有一段時間同時操作同一份資料庫，因此設計上必須能同時支援兩邊。
- **所有回應都以物件包裝** 。不直接回傳陣列（例如採用 `{"servers": [...], "total": n}` 的形式），以保留日後加入中繼資料的彈性。

`servers.host` 會使用 Fernet 金鑰加密後儲存。若要輪替金鑰，可以用逗號分隔的方式同時放入新舊金鑰，達成無停機切換。

## 分支運用與 CI/CD

配合 1.0.0 的發布，我改用了 git-flow。

- **develop** ：功能 PR 的 base。Argo CD 會自動同步到 staging。
- **release/x.y.z** ：從 develop 分支切出，只負責發布資源的穩定化與本番 overlay 標籤確認。
- **main** ：正式環境。只接受 release / hotfix 的合併，合併後會打上標籤。

PR 審查由 GitHub Copilot CLI 自動執行。Jenkins 會對每個 PR 跑 pytest 與 build 驗證，並以 Copilot 的 `VERDICT` 註解為起點，由 GitHub Actions 分流處理。

- `VERDICT: APPROVE` 且 base ≠ main → squash merge + 刪除分支
- `VERDICT: APPROVE` 且 base = main → 通知人工合併流程（git-flow 的發布由人來操作）
- `VERDICT: REQUEST_CHANGES` → 加上 `needs-human-review` 標籤

這裡我刻意設計成 **不讓 Claude 自動修正** 。目的是避免形成 AI 審查 → AI 修正 → AI 審查……的循環。

## 安全性與隱私

- **驗證** ：由 Firebase 發行的 ID token 由後端進行驗證。
- **加密儲存** ：伺服器連線目的地 `host` 會以 Fernet 加密。金鑰只存在於 k8s Secret 中。
- **封鎖內部 API** ： `/internal/*` 會由 nginx 擋下外部公開路徑，只允許從叢集內部存取。
- **App 隱私** ：收集的資料僅包含電子郵件地址、使用者 ID（帳號管理）、使用狀況與當機資料。不使用追蹤（IDFA）。

## 結語

在開發 MineWatch 的過程中，我學到最多的，其實不是「做出一個應用程式」本身，而是「 **建立一個能持續發布應用程式的環境** 」這件事的重要性。像 git-flow、Expand-Contract、API 版本控管這些決策，都是從「面對無法一次更新所有行動裝置的情況，要如何安全地持續發布」這個核心問題反推而來。

程式設計

等審查通過後，我也會另外整理審查時被指出的事項，以及實際提交流程的細節。

---

[![:sparkles:](https://cdn.qiita.com/emoji/twemoji/unicode/2728.png)](https://cdn.qiita.com/emoji/twemoji/unicode/2728.png) 即使沒有經驗也能學會！一起挑戰吧！ [![:sparkles:](https://cdn.qiita.com/emoji/twemoji/unicode/2728.png)](https://cdn.qiita.com/emoji/twemoji/unicode/2728.png)

我也有在寫 note↓

---

原文出處： [https://qiita.com/jqit-yukiono/items/d33908602fd060f5e1d1](https://qiita.com/jqit-yukiono/items/d33908602fd060f5e1d1)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qen4zx) [2瀏覽](https://codelove.tw/@tony/post/qen4zx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qen4zx)

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

🔥 [首次亮相 NVIDIA Jetson Orin Nano Super - 最經濟實惠的生成式 AI 超級計算機](https://codelove.tw/@tony/post/qvpzV3)

🔥 [【存檔版】Google Antigravity 完全指南 | 掌握下一代「基於代理」IDE 的速查表](https://codelove.tw/@tony/post/qJXye3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝7 💬2 ❤️4

224

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬6 ❤️3

109

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1784686841&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fqen4zx&amp;pra=5&amp;aiof=10&amp;asro=0&amp;itsi=4&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTI1IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjEyNSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTI1Il1dLDBd&amp;dt=1784686841757&amp;bpp=1&amp;bdt=119&amp;idt=40&amp;shv=r20260721&amp;mjsv=m202607160101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1784686301%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1784686301%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1784686301%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2528313863623&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1533&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=31099683%2C95396138%2C95396252&amp;oid=2&amp;pvsid=448157683919144&amp;tmod=1923449514&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1934%2C30%2C1934%2C30%2C1920%2C30%2C1773%2C1050%2C1533%2C1003&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.16&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=49" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260721/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>

軟體