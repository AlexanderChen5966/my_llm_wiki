---
title: Sanlong — 預約洗車功能（SSGS-9377）
date: 2026-05-18
tags: [dev/flutter/app, project/sanlong]
source: 三隆 APP 內部 Issue SSGS-9377（預約洗車功能，2026-02-06～2026-05-06 開發實作紀錄）
type: reference
status: processed
---

預約洗車是三隆 APP 的重大新功能，Issue SSGS-9377，21 項任務全部完成（2026-02-06 ～ 2026-05-06）。採用 Mock 優先開發策略，分 7 層依序實作後再串接真實 API。

## 功能流程

```
首頁點擊洗車
  → 選擇車號（CarWashSelectCarScreen）
    → 預約洗車主頁（CarWashScreen）
        預約 Tab：選站點 + 洗車方案 + 配件
      → 訂單明細頁（CarWashOrderScreen）
          優惠券 + 會員點數 + 發票 + 付款方式
        → 預約成功/失敗 Dialog
          → 訂單詳情頁（CarWashOrderDetailScreen）
              取消預訂 → 回首頁 / 回待使用清單
    待使用 Tab：進行中訂單列表
      → 訂單詳情頁 → 取消預訂
```

## 畫面清單

| Screen | 檔案 |
|--------|------|
| 選擇車號 | `car_wash_select_car_screen.dart` |
| 預約洗車主頁（含兩 Tab）| `car_wash_screen.dart` |
| 訂單明細頁 | `car_wash_order_screen.dart` |
| 訂單詳情頁 | `car_wash_order_detail_screen.dart` |

## 關鍵 Widget

| Widget | 功能 |
|--------|------|
| `wash_station_selector.dart` | 站點選擇 Bottom Sheet（GPS 排序）|
| `wash_plan_grid.dart` | 洗車方案橫向 Grid |
| `wash_accessory_selector.dart` | 配件選擇（Switch + 複選）|
| `wash_coupon_selector.dart` | 優惠券選擇 Bottom Sheet |
| `wash_invoice_selector.dart` | 發票設定（互斥 Toggle）|
| `wash_payment_selector.dart` | 支付方式選擇 |
| `wash_order_tile.dart` | 待使用清單項目 |

## Controller 方法（命名規範）

| 方法 | 功能 |
|------|------|
| `onHomeScreenTapCarWash()` | 首頁入口，guest 攔截 |
| `onCarWashSelectCarScreenConfirm(carPlate)` | 選車確定，儲存本地 |
| `onCarWashScreenConfirm(data)` | 預約確定，含 pending 訂單檢查 |
| `onCarWashOrderScreenBooking(orderData)` | 建立訂單，含 `isBookingCarWash` 防重複 |
| `onCarWashOrderDetailScreenCancel(orderId, source)` | 取消訂單，依來源決定導航路徑 |
| `onCarWashOrderDetailScreenBack(data)` | 返回，同時呼叫 `loadUserMoney()` |
| `loadCarWashData()` | 載入站點 + 方案 + 訂單 + 優惠券 |

## 取消預訂導航邏輯

| 來源（OrderDetailSource）| 取消後導航 |
|--------------------------|-----------|
| `fromPendingTab` | `popTab()` → 回待使用清單 |
| `fromBookingSuccess` | `clearTab()` + `onHomeScreenTapCarWash()` → 回首頁 |
| `fromBlockDialog` | `clearTab()` + `onHomeScreenTapCarWash()` → 回首頁 |

## carInfo 編碼（配件 Bitmask）

| 配件 | bit 值 |
|------|--------|
| 車頂（ACC001）| `0x01` |
| 車頭（ACC002）| `0x02` |
| 車尾（ACC003）| `0x04` |

全選 = `0x01 | 0x02 | 0x04 = 7`，未選 = `0`

**注意**：2026-05-05 曾誤改為十進位加總，2026-05-07 已確認後端規格為 bitmask 並回滾。

## API 串接重點（T18-T21）

- `clientTransactionId`：用 UUID v4 做冪等保護，防止網路重送建立重複訂單
- `androidNo`：暫用 station `id`（資料庫流水號），待後端確認是否等於硬體機台編號
- JSON key 踩坑：`WashStationItemDto` 的 `androidNo` 在 API 回傳為 camelCase，讀取時不可用 `android_no`（snake_case）
- `voucherType` 過濾：需同時包含 `CAR_WASH` 和 `COUPON`，見 [[sanlong/bugs]] BUG-02
- `payWay` 對應：1 = 信用卡，2 = 儲值金

## 儲值金 / 點數即時同步

`loadUserMoney()` 的觸發點（5 處）：
1. 預約成功（`onCarWashOrderScreenBooking` API 200）
2. 取消預約成功（`onCarWashOrderDetailScreenCancel` API 200）
3. 進入訂單明細頁（`_loadData` 背景呼叫）
4. 訂單明細頁按返回（`onCarWashOrderScreenBack`）
5. 訂單詳情頁按返回（`onCarWashOrderDetailScreenBack`）

> 洗車流程的路由操作（`clearTab` / `popTab`）繞過了 `onTabScreenTapIndex`，必須手動呼叫 `loadUserMoney()` 確保首頁餘額更新。

## 本地儲存 Keys

```dart
car_wash_last_car          // 上次選擇的車號
car_wash_last_station_no   // 上次選擇的站點
car_wash_last_plan_id      // 上次選擇的方案
car_wash_last_accessories  // 上次選擇的配件（逗號分隔 ID）
```

登出不清除（換帳號仍保留上次記錄）。

## 相關條目

- [[sanlong/bugs]] — BUG-01 GlobalKey、BUG-02 優惠券過濾
- [[sanlong/api]] — 洗車服務 API 模組（15）
- [[sanlong/architecture]] — SLController 命名規範
