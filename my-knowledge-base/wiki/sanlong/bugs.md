---
title: Sanlong — Bug 紀錄
date: 2026-05-18
tags: [dev/flutter/app, project/sanlong]
type: reference
status: processed
---

三隆 APP 已記錄的 Bug 與根因分析。高頻回歸風險：Provider 重複定義、前端過濾白名單硬編碼、catch 範圍不足。

---

## BUG-01：Android 模擬器 Duplicate GlobalKey（已修復）

**發現日期**：2026-03-03 | **模組**：預約洗車（SSGS-9377）| **平台**：Android（iOS 不發生）

### 根因

兩個問題並存：

**1. Tab 切換時 Element 復用衝突**

`CarWashScreen` 的兩個 Tab 都在 loading 時，頂層類型相同（`Expanded(child: Center(child: Loading()))`），Flutter Element 復用機制在 tab 切換瞬間出現 Widget tree 結構不一致。Android 的 Skia 渲染路徑比 iOS Metal 更容易在特定時序下觸發。

**2. Provider 重複定義導致資料不同步**

`wash_payment_selector.dart` 自行定義了 `creditCardListProvider`，而正確的 Provider 是 `credit_card_screen.dart` 的 `creditCardList`。前者永遠是 null，導致信用卡後四碼不顯示、選擇後跳出「尚未綁定信用卡」。

### 修復

```dart
// Fix 1：為 Tab Expanded 加 ValueKey 強制重建
Expanded(
  key: const ValueKey('booking'),  // 或 'pending'
  child: _buildBookingTab(),
)

// Fix 2：刪除重複 Provider，改用正確的 creditCardList
// 刪除：final creditCardListProvider = StateProvider<...>((ref) => null);
final cards = widget.ref.watch(creditCardList);  // 正確 Provider
```

### 預防規則

> **新增 Widget 之前先搜尋現有 Provider**：避免重複定義導致資料不同步。條件式切換頂層類型相同的 Widget 時，必須加 `ValueKey`。

---

## BUG-02：洗車優惠券 COUPON 類型未顯示（已修復）

**發現日期**：2026-04-29 | **模組**：洗車服務 / 優惠券選擇

### 根因

`repository_api.dart` 的 `getWashVouchers()` 過濾條件只允許 `CAR_WASH` 類型：

```dart
// 修復前（問題）
.where((item) => item.voucherType == 'CAR_WASH' && item.status == 'UNUSED')
```

後端新增 `COUPON` 類型時，前端過濾白名單未同步更新，導致 `COUPON` 券靜默消失。

### 修復

```dart
// 修復後
.where((item) =>
    (item.voucherType == 'CAR_WASH' || item.voucherType == 'COUPON') &&
    item.status == 'UNUSED')
```

### 預防規則

> **後端新增 voucherType 時，前端 getWashVouchers() 的過濾白名單必須同步更新**。長期可考慮改由後端 API 層過濾（傳入查詢參數），避免前端硬編碼白名單。

---

## BUG-03：生物辨識當機（小米 14T Pro 臉形辨識）

**發現日期**：2026-05-13 | **狀態**：待修復

### 問題現象

使用者從小米 11 Pro（指紋）換機至小米 14T Pro（臉形辨識），開啟 APP 時整個崩潰，需重新安裝改用密碼登入。

### 根因

三個問題並存：
1. `auth.dart` 只 catch `PlatformException`，小米 HyperOS 臉形辨識可能拋出其他例外或 native crash
2. 缺少 `isDeviceSupported()` 檢查
3. 換機後 `bioEnable` flag 保留舊值，啟動直接觸發不相容的生物辨識

### 修復方案（待實作）

```dart
// lib/app/service/auth.dart
Future<bool> authenticate() async {
  try {
    final auth = LocalAuthentication();
    final isSupported = await auth.isDeviceSupported();  // 新增
    if (!isSupported) return false;
    final canCheck = await auth.canCheckBiometrics;
    if (!canCheck) return false;
    return await auth.authenticate(
      localizedReason: '快速登入',
      options: const AuthenticationOptions(biometricOnly: true, stickyAuth: true),
    );
  } catch (e) {  // 改為 catch 所有例外
    return false;
  }
}
```

### 除錯方式

模擬器難以重現（HyperOS 客製化行為）。建議用真實裝置 + `adb logcat` 取得 native crash log，或透過 Firebase Crashlytics 查看 stack trace。

---

## Bug 高頻模式總結

| 模式 | 描述 | 預防措施 |
|------|------|---------|
| Provider 重複定義 | 兩個地方定義同概念 Provider，資料不同步 | 新增前先搜尋現有 Provider |
| 前端過濾白名單 | 後端新增類型，前端硬編碼未跟上 | 考慮改由後端過濾，或統一維護類型清單 |
| Flutter Element 復用 | 條件切換頂層類型相同時 Android 觸發 | 使用 ValueKey 強制重建 |
| catch 範圍不足 | 只 catch 特定 Exception 類型 | 生物辨識、第三方 SDK 等用 catch(e) 兜底 |

## 相關條目

- [[sanlong/overview]] — 專案總覽
- [[sanlong/architecture]] — Provider 定義規範
- [[sanlong/car-wash]] — 洗車功能開發紀錄
