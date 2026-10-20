---
title: B2B Manager Troubleshooting
date: 2026-05-14
tags: [dev/flutter, project/b2b-manager]
source: /Users/alexander/GitLab/b2b-manager/docs/claude/claude-troubleshooting.md
type: reference
status: processed
---

常見錯誤速查、DataTable 問題清單、監控工具說明。原始文件最後更新：2026-04-15。

## 常見錯誤（6 項）

| # | 症狀 | 根因 | 解決 |
|---|------|------|------|
| 1 | 找不到 `.g.dart`、JSON 序列化錯誤、undefined method | 忘記執行 build_runner | `flutter pub run build_runner build --delete-conflicting-outputs` |
| 2 | 重複 OAuth2 請求、rate limiting、同時 token refresh | TaskManager 未使用 | OAuth2 授權與 token refresh **必須**透過 TaskManager 包裝，禁止直接呼叫 |
| 3 | `const constructor cannot have a non-const invocation` | `const` 與 flutter_screenutil 衝突 | 移除 Widget 的 `const` 關鍵字 |
| 4 | 套件行為異常、API 不符合預期 | 誤用 `pub upgrade` 更新 external_libs | 直接修改 `external_libs/` 原始碼，不可 pub upgrade（專案特製版本）|
| 5 | API 呼叫失敗、OAuth redirect 錯誤 | 環境變數設定錯誤 | 確認 `DEPLOY` 環境變數；檢查 dev/staging/release URL 配置 |
| 6 | 登入後頁面無反應 | OAuth callback 失敗 | 本地開發必須用 `--web-port=9001`，callback 指向 `localhost:9001/callback` |

## DataTable 問題（5 項）

| 症狀 | 根因 | 解決 |
|------|------|------|
| 表格只顯示 9 列而非 10 列 | 容器用 `height: 600.h`（響應式），實際高度不足 | 改為 `height: 600`（固定 px）|
| DataCell 游標不正確 | `onTap` 或 `SelectionArea` 包裝不符規則 | 查 [[b2b-manager/architecture]] DataTable 規則第 2 節 |
| 排序箭頭顯示在錯誤欄位 | `sortColumnIndex` 多加了 `+1` | 移除 `+1`，直接傳 `sortColumnIndex ?? 0` |
| RenderFlex overflow（cell 文字換行超出高度）| 雙行 cell 的 vertical padding > 2px | 改為 `EdgeInsets.symmetric(vertical: 2, horizontal: 8)` |
| "Tried to modify a provider while the widget tree was building" | `queryApi()` 在 build phase 同步修改 Riverpod state | 用 `Future(() {})` 包裝 `setRequestPage()` |

## 監控工具

| 工具 | 用途 | 環境 |
|------|------|------|
| Firebase Analytics | 使用者行為追蹤、頁面瀏覽 | 所有環境 |
| Sentry | 錯誤追蹤與當機報告 | 正式環境 |
| `lib/util/web_debug_helper.dart` | 自訂除錯工具 | 開發環境 |

## 除錯指令

```bash
# 詳細錯誤輸出
flutter run -d chrome --verbose

# 清除快取
flutter clean && rm -rf build/ && flutter pub get

# 檢查套件衝突
flutter pub outdated
flutter pub deps
```

## 相關連結
- [[b2b-manager/architecture]] — DataTable/DataCell 完整規則、TaskManager 實作細節
- [[b2b-manager/dev-workflow]] — flutter run / build / test 常用指令
- [[b2b-manager/bugs]] — 高頻回歸模式記錄
