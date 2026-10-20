---
title: Sanlong — 架構設計
date: 2026-05-18
tags: [dev/flutter/app, project/sanlong]
type: reference
status: processed
---

採用四層架構（UI → Controller → Repository → Data），以 SLController 作為唯一業務邏輯入口，所有方法遵循 `on{Screen}{Action}` 命名規範。

## 四層架構

```
UI 層（ConsumerStatefulWidget）
    ↓ ref.watch(controller)
Controller 層（SLController）
    ↓ RepositoryApi / LocalRepository
Repository 層
    ↓ Dio / SharedPreferences
資料層
```

## SLController 單一控制器模式

**設計原則**：整個 APP 只有一個 SLController，集中管理所有業務邏輯。

**命名規範**：`on{Screen}{Action}`

```dart
onStoreMoneyScreenAddMoney()  // 儲值金頁 - 新增儲值
onLoginScreenSubmit()          // 登入頁 - 提交
onHomeScreenTapCarWash()       // 首頁 - 點擊預約洗車
```

**優點**：統一業務邏輯入口、易於追蹤資料流、便於共享狀態

**注意**：單一 Controller 隨功能增加會變大，新增方法時確保命名規範一致。

## Riverpod 狀態管理

| Provider 類型 | 用途 | 範例 |
|--------------|------|------|
| `Provider` | 提供單例物件 | `controller`、`router` |
| `StateProvider` | 管理簡單狀態 | `isGuest`、底部導航索引 |
| `ConsumerWidget` | 無狀態 Widget | 靜態頁面 |
| `ConsumerStatefulWidget` | 有狀態 Widget | 表單、互動頁面 |
| `ChangeNotifierProvider` | 複雜狀態管理 | `QuickModeDataNotifier` |

**禁止使用**：
- `setState()` 管理全域狀態
- 直接在 UI 層呼叫 API
- Widget 內部處理複雜業務邏輯

## SLRouter 路由管理

基於 Flutter Navigator 2.0 自定義路由，分為主堆疊（mainStack）和底部導航堆疊（tabStacks）。

```dart
// 主堆疊導航
_router.pushMain(NavConfig(screen: Screen.carWash))
_router.popMain()

// Tab 切換
_router.tabIndex = BottomTabType.STATION.index

// 回根頁面
_router.clearTab()  // 清空 Tab 堆疊回首頁
_router.popTab()    // 只 pop 一層（回上一頁）
```

**底部 Tab 定義**（BottomTabType）：HOME / MEMBER_POINT / QRCODE / STATION / PERSONAL

## 資料模型（Freezed + json_serializable）

```
API Response (JSON)
    ↓
DTO（lib/app/common/dto/api.dart）— Freezed 不可變類別
    ↓
Data Model（lib/app/common/data/data.dart）— 業務邏輯用
    ↓
UI Layer
```

**重要**：DTO 修改後需執行 `flutter pub run build_runner build --delete-conflicting-outputs` 重新產生程式碼。

## 目錄結構重點

| 目錄 | 說明 |
|------|------|
| `lib/app/controller/controller.dart` | SLController 主控制器 |
| `lib/app/controller/repository_api.dart` | API Repository（所有 HTTP 請求）|
| `lib/app/controller/local.dart` | LocalRepository（SharedPreferences）|
| `lib/app/controller/screen_manager.dart` | SLRouter 路由管理 |
| `lib/app/common/dto/api.dart` | 所有 DTO 定義 |
| `lib/app/common/data/data.dart` | 所有資料模型 |
| `lib/app/ui/{feature}/` | 各功能模組（Screen + Widget）|

## 安全性架構

- **密碼雜湊**：argon2（Argon2id 演算法）
- **資料加密**：encrypt（AES）
- **生物辨識**：local_auth（需加 `isDeviceSupported()` 檢查，見 [[sanlong/bugs]]）
- **螢幕保護**：screen_protector（防止截圖錄影）
- **API 認證**：Bearer Token（Authorization Header）
- **規範**：所有 API 必須 HTTPS，敏感資料走 POST body

## 相關條目

- [[sanlong/overview]] — 專案基本資訊
- [[sanlong/bugs]] — 高頻問題與修復模式
