---
title: MineWatch：以個人開發學 App Store 上架流程（UIKit + FastAPI + K8s）
date: 2026-07-22
tags: [dev/architecture, dev/ci-cd, ai/workflow]
source: https://codelove.tw/@tony/post/qen4zx
type: article
status: processed
asset_value: 50
---

開發者以「學習 App Store 上架流程」為主要目的，打造 Minecraft 伺服器監控 App MineWatch（UIKit + FastAPI + PostgreSQL + Celery + K8s）。文章重點不在功能介紹，而在「個人開發到這個規模時為什麼採用這樣的架構」，以及為了穩定通過審查而特別意識到的設計取捨。

## T（核心論點）

App 開發最大的學習不是「做出一個應用」，而是「**建立一個能持續發布應用的環境**」。像 git-flow、Expand-Contract、API 版本控管這些決策，全都從一個核心問題反推而來：**面對無法一次更新所有行動裝置的現實，要如何安全地持續發布**。同一原則延伸到 AI：CI 用 Copilot CLI 自動審查並以 `VERDICT` 觸發自動合併，但**刻意不讓 AI 自動修正**——避免形成 AI 審查→AI 修正→AI 審查的循環。

## B（適用場景）

**適用：**
- 個人 / 小團隊要把後端服務穩定送上 App Store，需要可持續發布的機制
- 想理解行動 App 後端「向下相容」的具體做法
- 設計 AI 參與的 CI 審查流程，思考自動化與人類把關的邊界

**不適用 / 注意：**
- 這是個人專案規模的取捨，不是所有決策都適合搬到大型團隊
- 部分基礎架構（K8s/Argo CD/Jenkins）對個人專案偏重，作者坦承有興趣成分

## R（架構取捨重點）

**技術堆疊**：iOS（UIKit 無 Storyboard、MVVM、Swift 6 strict concurrency、XcodeGen+SPM）/ FastAPI（Python 3.13、SQLModel+Alembic）/ Celery（worker+beat）+Redis / PostgreSQL 17 / Firebase Auth（後端驗 ID token）/ 直送 APNs / Docker+K8s / Argo CD / Jenkins+GitHub Actions / Cloudflare。

- **SLP 免改伺服器**：用 Minecraft 用戶端在伺服器列表所用的公開協定 Server List Ping 取狀態，被監控伺服器完全不需裝外掛或改設定
- **監控執行與 API 分離（worker/beat）**：beat 把週期任務丟 Redis、worker 執行並直接送 APNs；即使 API/Web 掛掉，佇列中的監控任務仍會繼續執行（審查期間可用性關鍵）
- **推播不經 FCM，直送 APNs**：iOS 端不必攜帶 Firebase 推播 SDK
- **API 向下相容四招**：目錄＝版本（`api/<version>/<domain>/router.py` 自動註冊）、破壞性變更凍結 v1 新增 v2（邏輯集中 services/、router 只留薄適配層）、DB 型別變更走 **Expand-Contract**（新增欄位→遷移→切換→刪除，新舊程式共存）、所有回應以物件包裝（不直接回陣列，保留加中繼資料彈性）
- **加密與封鎖**：`host` 用 Fernet 加密（金鑰只在 k8s Secret，逗號分隔新舊金鑰可無停機輪替）、`/internal/*` 由 nginx 擋外部
- **git-flow + AI 審查分流**：develop（Argo CD 同步 staging）/ release/x.y.z / main（只收 release/hotfix）；PR 由 Copilot CLI 審查，以 `VERDICT` 為 GitHub Actions 分流起點：`APPROVE` 且 base≠main→squash merge、base=main→通知人工、`REQUEST_CHANGES`→貼 `needs-human-review` 標籤

## C（關鍵行動）

- [ ] 行動 App 後端從「無法一次更新所有裝置」反推：API 版本化 + Expand-Contract + 物件包裝回應
- [ ] 監控 / 背景任務與 API 分離，確保 API 掛掉時關鍵任務仍運作
- [ ] 設計 AI 參與的 CI 時，讓 AI 審查並觸發自動合併，但保留「不讓 AI 自動修正」的邊界
- [ ] 敏感連線資訊加密（Fernet），金鑰放 Secret 並支援無停機輪替

## 相關連結

- [[fastapi-python-api-framework]] — FastAPI 的 ASGI 非同步與自動文件，本篇是它在真實上架專案的架構落地
- [[ai-code-review-safeguards]] — AI 產碼審查護欄，呼應本篇「AI 可審查但不自動修正」的人類把關設計
- [[http-query-method-rfc10008]] — API 設計相關（HTTP 動詞語義），可對照 API 向下相容思路
- [[research/app-store-review-rejection-lessons]] — 續篇：實際送審 3 次拒審到通過的 ATT/App Check/401-403 踩坑細節

## 原始摘要

原文 Qiita（jqit-yukiono）→ codelove.tw @tony 譯。以學 App Store 上架為目的打造 MineWatch（UIKit+FastAPI+PostgreSQL+Celery+K8s），分享架構取捨：SLP 免改被監控伺服器、worker/beat 與 API 分離、直送 APNs 免 Firebase SDK、目錄即版本 + Expand-Contract 維持向下相容、git-flow + Copilot CLI 以 VERDICT 觸發自動合併但刻意不自動修正。
