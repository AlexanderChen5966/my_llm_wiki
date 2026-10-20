---
title: Sanlong — API 模組清單
date: 2026-05-18
tags: [dev/flutter/app, project/sanlong]
type: reference
status: processed
---

Base URL: `http://your-domain:8083`。統一回應格式：`{ "code": 200, "msg": "success", "data": {...} }`。詳細 API 規格見原始文件 `docs/api/API_DOCUMENTATION.md`。

## API 模組總覽（21 個模組）

| 編號 | 模組 | 端點前綴 |
|------|------|---------|
| 1 | 用戶管理 | `/user` |
| 2 | 賬戶管理 | `/account` |
| 3 | 銀行卡管理 | `/card` |
| 4 | 訂單管理 | `/order` |
| 5 | 車輛管理 | `/car` |
| 6 | 發票管理 | `/invoice` |
| 7 | 油站服務 | `/station` |
| 8 | 新聞管理 | `/news` |
| 9 | 消息管理 | `/msg` |
| 10 | 企業功能 | `/group` |
| 11 | 企業邀請 | `/group_invitation` |
| 12 | 企業打卡 | `/group_attendance` |
| 13 | 儲值管理 | `/recharge` |
| 15 | 洗車服務 | `/wash-car` |
| 16 | 券管理 | `/voucher` |
| 17 | 中信銀行 | `/ctbc` |
| 17b | 銀行回調 | `/bank` |
| 18 | 平台服務 | `/app` |
| 19 | 文件服務 | `/file` |
| 20 | 頁面服務 | `/page` |
| 21 | 版本管理 | `/version` |

## 洗車服務 API（15）重點端點

| 方法 | 端點 | 說明 |
|------|------|------|
| GET | `/wash-car/stations` | 站點列表（含 `androidNo` 欄位，非空才顯示）|
| GET | `/wash-car/plans/{stationNo}` | 洗車方案列表 |
| POST | `/wash-car/addOrder` | 建立洗車訂單（含 `clientTransactionId` 冪等保護）|
| POST | `/wash-car/cancelOrder` | 取消訂單 |
| GET | `/wash-car/activeOrders` | 進行中訂單列表 |

## 優惠券 API（16）重點

- `GET /voucher/list` — 回傳所有未使用券
- 洗車可用類型：`voucherType == "CAR_WASH"` 或 `voucherType == "COUPON"`
- **注意**：前端過濾時需同時包含兩種類型，見 [[sanlong/bugs]] 優惠券過濾問題

## 儲值 API（13）重點

- 中信銀行：`POST /ctbc/recharge`（rechargeType: 1=ATM, 3=網銀）
- 藍新金流：WebView 整合（信用卡 / 超商）

## 認證

所有 API 請求帶 `Authorization: Bearer {token}` Header。

## 相關條目

- [[sanlong/overview]] — 專案總覽
- [[sanlong/car-wash]] — 洗車功能完整紀錄
