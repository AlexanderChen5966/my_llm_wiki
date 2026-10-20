---
title: 三隆加油站 APP (Sanlong) — 專案總覽
date: 2026-05-18
tags: [dev/flutter/app, project/sanlong]
type: reference
status: processed
---

三隆加油站 APP 是面向 iOS / Android 的加油站服務行動應用，提供 QR Code 加油、儲值金管理、會員服務、信用卡管理、加油站查詢、預約洗車等完整功能。技術棧為 Flutter + Riverpod，採用單一大型 Controller 架構（SLController）。

## 基本資訊

| 項目 | 內容 |
|------|------|
| 專案名稱 | Sanlong（三隆加油站 APP）|
| 版本 | 2.12.1+688 |
| 平台 | iOS 13.0+ / Android 5.0+（API 21+）|
| 語言 | 繁體中文（zh-TW）|

## 核心技術棧

| 層次 | 技術 | 版本 |
|------|------|------|
| 語言 | Dart | 3.5.0+ |
| 框架 | Flutter SDK | 3.7.3+ |
| 狀態管理 | Riverpod | 2.6.1 |
| HTTP | Dio | 5.8.0+1 |
| 資料模型 | Freezed + json_serializable | 2.5.8 |
| 本地儲存 | SharedPreferences | 2.2.2 |
| 生物辨識 | local_auth | 2.3.0 |
| 密碼雜湊 | argon2 | 1.0.1 |
| 多語系 | easy_localization | 3.0.7+1 |
| 程式碼生成 | build_runner | 2.4.9 |

## 核心功能

- **加油服務**：QR Code 加油、快速加油設定
- **儲值金管理**：ATM / 信用卡 / 超商儲值（中信銀行 + 藍新金流）
- **會員服務**：會員資料、點數累積、消費記錄
- **信用卡管理**：綁定信用卡、自動扣款
- **加油站資訊**：站點查詢、油價、地圖導航
- **預約洗車**：SSGS-9377（已完成，21 項任務）

## 第三方服務

| 服務 | 用途 |
|------|------|
| Firebase Analytics | 使用者行為追蹤 |
| Firebase Messaging | Push Notification |
| Firebase Crashlytics | 崩潰報告 |
| 中國信託（CTBC）| ATM / 網銀儲值 |
| 藍新金流 | 信用卡 / 超商儲值 |

## 相關條目

- [[sanlong/architecture]] — 四層架構、SLController 模式、Riverpod 用法
- [[sanlong/dev-workflow]] — 建置指令、Flavor 設定、Git 工作流
- [[sanlong/api]] — 21 個 API 模組清單
- [[sanlong/bugs]] — Bug 紀錄（GlobalKey / 優惠券過濾 / 生物辨識當機）
- [[sanlong/car-wash]] — 預約洗車功能完整紀錄（SSGS-9377）
