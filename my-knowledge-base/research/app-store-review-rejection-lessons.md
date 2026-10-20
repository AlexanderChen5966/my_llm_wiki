---
title: MineWatch App Store 審查續篇：3 次拒審到通過的 ATT/App Check 踩坑紀錄
date: 2026-07-27
tags: [dev/architecture, dev/ci-cd]
source: https://codelove.tw/@tony/post/aGQ2zx（原文 https://qiita.com/jqit-yukiono/items/91755838ea8589fde90d）
type: note
status: processed
asset_value: 48
---

[[minewatch-app-store-release-architecture]]（MineWatch 架構篇）的續篇，記錄實際送審 3 次被拒到通過的具體排查過程：Guideline 5.1.2(i)（追蹤）連續 2 次被拒、4.1(c)（品牌名 Copycat）被拒 1 次。

**核心教訓**：自訂同意畫面寫明「不追蹤」不能代替 Apple 官方 ATT（App Tracking Transparency）對話框——問題出在 `FirebaseAnalytics` 套件即使沒呼叫 IDFA，仍會把 `GoogleAppMeasurementIdentitySupport`（IDFA 相關程式碼）打包進二進位檔，被 Apple 靜態分析判定為「有追蹤功能但未實作 ATT」；改用不含 IDFA 的 `FirebaseAnalyticsCore` + 統一改用系統 ATT 對話框才解決。副標題用品牌名（Minecraft）被判 Copycat，拿掉即解決（內文提及可接受，副標題不行）。

**三個只能在實機重現的坑**：
1. ATT 呼叫位置寫在 `scene(_:willConnectTo:)` 不會跳出對話框（此時 scene 尚未 `.active`，`requestTrackingAuthorization()` 悄悄回傳 `.notDetermined` 不丟例外），必須移到 `sceneDidBecomeActive`；且 CLI 啟動（`xcrun simctl launch`）也無法重現前景切換差異，只能點圖示啟動驗證。
2. 後端把 Firebase ID token 驗證失敗（401，「誰」不明，重登可能修好）與 App Check 驗證失敗（應獨立為 403，「是否正牌 App」不明，重登無用）混在一起回傳，導致 client 端錯誤引導使用者。
3. Google OAuth 同意畫面的「使用安全的流程」檢查項，要求 iOS App 實作並強制啟用 App Check——ATT 與 App Check 並非各自獨立，而是 Apple/Google/自家後端三方交錯：一處故障會在其他地方以不同錯誤形式冒出來。

**App Privacy 申報的矛盾**：實際沒有追蹤卻要跳 ATT 對話框，最終 App Store Connect 因二進位檔含 `NSUserTrackingUsageDescription` 而拒絕「不會追蹤」的申報，只能把 Firebase Analytics 使用資料改申報為「用於追蹤目的」——「實際行為」與「申報內容」必須一致，不只是改程式碼就能結束。

## 為何列為研究筆記而非 wiki 條目

asset_value 48 略低於 wiki 門檻的 50。內容雖然扎實且具體，但屬於單一專案（MineWatch）審查過程的高度情境化踩坑紀錄，泛用性不如架構篇（已是 wiki 條目 [[minewatch-app-store-release-architecture]]，asset_value 50）；核心可遷移教訓（ATT 呼叫時機、401/403 分離、App Check 三方交錯）已在此摘要完整保留，未來若累積更多 App Store 審查案例達到 3 篇以上可考慮以 knowledge-synthesizer 精粹成獨立條目。

## 相關連結

- [[minewatch-app-store-release-architecture]] — 前篇：MineWatch 整體架構設計（UIKit+FastAPI+K8s），本篇是其審查過程的續篇，已在該條目「相關連結」補充互相參照
- [[ai-code-review-safeguards]] — 前篇提及的 AI 審查護欄設計，可對照本篇「申報內容與實際行為必須一致」的合規思維
