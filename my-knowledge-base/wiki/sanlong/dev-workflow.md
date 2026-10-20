---
title: Sanlong — 開發工作流
date: 2026-05-18
tags: [dev/flutter/app, project/sanlong]
type: reference
status: processed
---

三隆 APP 使用 Flavor 配置三個環境（dev / staging / prod），建置前確認已安裝 Flutter 3.7.3+、Dart 3.5.0+。

## 常用指令

```bash
# 依賴安裝
flutter pub get

# 重新產生 Freezed / JSON 序列化程式碼（修改 DTO / 資料模型後必跑）
flutter pub run build_runner build --delete-conflicting-outputs

# 執行
flutter run --flavor dev

# Lint 檢查
flutter analyze

# 格式化
flutter format .
```

## Flavor 建置（Android APK）

| 環境 | 指令 | 產出路徑 |
|------|------|---------|
| Dev | `flutter build apk --flavor dev -t lib/main_android_debug.dart` | `build/app/outputs/flutter-apk/app-dev-debug.apk` |
| Staging | `flutter build apk --flavor staging -t lib/main_android_staging_debug.dart` | `build/app/outputs/flutter-apk/app-staging-debug.apk` |
| Product | `flutter build apk --flavor product -t lib/main_android_release_debug.dart --release` | `build/app/outputs/flutter-apk/app-product-release.apk` |

> Product 環境需確認 `android/key.properties` 的 Keystore 簽章設定存在。

## ADB 安裝（Android）

```bash
# 確認裝置連接
adb devices

# 安裝 APK
adb install build/app/outputs/flutter-apk/app-dev-debug.apk

# 覆蓋安裝（已安裝過時用 -r）
adb install -r build/app/outputs/flutter-apk/app-dev-debug.apk

# 移除 APP
adb uninstall tw.com.slc.sanlongpay

# 查看 Log（崩潰除錯用）
adb logcat
```

## iOS 建置

```bash
flutter build ios --flavor prod
```

> 需 Mac + Xcode + 有效 Apple 開發者憑證。詳見 `docs/other/ios_build_and_install_guide.md`。

## Flavor 設定

| Flavor | App 名稱 | Bundle ID（iOS）| ApplicationId（Android）|
|--------|---------|----------------|------------------------|
| dev | Sanlong Dev | com.sanlong.app.dev | com.sanlong.app.dev |
| prod | Sanlong | com.sanlong.app | com.sanlong.app |

## AI 工具分工

本專案採用雙 AI 工具協作：

| 工具 | 負責 |
|------|------|
| Claude Code | 架構設計、複雜業務邏輯、程式碼審查、技術決策、文件 |
| Copilot CLI | Screen Widget 實作、Controller 方法、Repository API、Freezed 模型、單元測試 |

```
需求 → Claude Code（設計）→ Copilot CLI（實作）→ Claude Code（審查）→ 完成
```

## Git 工作流

```bash
# 功能分支
git checkout -b feature/SSGS-XXXX

# 提交格式
git commit -m "功能描述

- 詳細變更 1
- 詳細變更 2

Issue: SSGS-XXXX"
```

## 相關條目

- [[sanlong/overview]] — 專案總覽
- [[sanlong/architecture]] — 架構設計
