---
title: B2B Manager 開發工作流程
date: 2026-05-13
tags: [dev/flutter, dev/architecture]
source: /Users/alexander/GitLab/b2b-manager/docs/claude/claude-development-workflows.md
type: reference
status: processed
---

## 摘要

B2B Manager 日常開發的常用指令與流程，包含程式碼產生、本地執行、建置部署、測試與品質檢查。

## 常用指令速查

### 開發

```bash
# 在 Chrome 執行
flutter run -d chrome

# 指定 port（OAuth callback 測試用）
flutter run -d chrome --web-port=9001

# 指定環境
flutter run -d chrome --dart-define=DEPLOY=dev
```

### 程式碼產生（修改 API 後必執行）

```bash
flutter pub run build_runner build --delete-conflicting-outputs
flutter pub run build_runner clean        # 清除舊產生檔案
flutter pub run intl_utils:generate       # 產生 i18n 檔案
```

### 建置與部署

```bash
# 生產建置（HTML 渲染器，相容性好）
flutter build web --release --web-renderer html

# 生產建置（CanvasKit，效能好）
flutter build web --release --web-renderer canvaskit

# Docker
docker build -t b2b_manager .
docker run -p 8089:8089 b2b_manager
docker-compose up
```

### 測試與品質

```bash
flutter test                              # 全部測試
flutter analyze --no-fatal-infos --no-fatal-warnings --pub
flutter clean && flutter pub get          # 清理重建
```

## 新增 API 端點流程

1. 在 `restclient/` 定義 Retrofit 方法
2. 執行 `build_runner build`（自動產生實作）
3. 在 `notifier/` 新增 StateNotifier
4. 更新 UI

## 響應式工具

```bash
# 批次更新 widget 使用 flutter_screenutil
dart run ./update_screenutil.dart
```

## 相關連結

- [[b2b-manager/architecture]] — 架構規則與注意事項
- [[b2b-manager/overview]] — 完整技術棧
