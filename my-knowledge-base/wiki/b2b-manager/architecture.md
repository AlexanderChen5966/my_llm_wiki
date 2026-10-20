---
title: B2B Manager 架構規則
date: 2026-05-13
tags: [dev/flutter, dev/architecture]
source: /Users/alexander/GitLab/b2b-manager/docs/claude/claude-architecture.md
type: reference
status: processed
---

## 摘要

專案採用 Flutter Web + Riverpod 架構，目錄依功能分層。有幾個高頻回歸區（TaskManager、DataTable、DataCell 游標）需特別注意。

## 目錄結構

```
lib/
├── api/                  # 網路層
│   ├── connector/        # OAuth2、TaskManager ⚠️
│   ├── interceptor/      # Request/Response 攔截
│   ├── restclient/       # Retrofit API 定義（自動產生）
│   ├── request/          # 請求 DTO
│   └── response/         # 回應 DTO
├── page/                 # UI 頁面（依功能）
│   ├── home/            # 儀表板
│   ├── order/           # 訂單管理
│   ├── invoices/        # 發票帳單
│   ├── car/             # 車隊管理
│   ├── driver/          # 司機管理
│   └── page_route.dart  # GoRouter 路由 ⚠️
├── models/               # 資料模型與列舉
├── notifier/             # Riverpod 狀態管理
└── util/                 # 工具函數
```

## ⚠️ TaskManager 單例模式

**目的：** 防止重複的 OAuth2 認證和 token 更新請求（rate limit 保護）

```dart
// ✅ 正確
final taskManager = TaskManager();
await taskManager.executeTask(
  TaskManager.handleAuthorizationResponseTask,
  () => handleAuthorizationResponse(context),
  timeout: Duration(seconds: 30),
);

// ❌ 錯誤（可能導致重複請求）
await handleAuthorizationResponse(context);
```

預定義 Task ID：
- `handleAuthorizationResponseTask` — OAuth2 授權
- `refreshAccessTokenTask` — Token 更新

## ⚠️ DataTable 高度規則

**表格容器高度必須用固定 px，禁止用 `.h`**

```dart
// ✅ 正確
SizedBox(height: 600, child: ApiPaginatedDataTable(...))

// ❌ 錯誤（小螢幕會截掉最後一列）
SizedBox(height: 600.h, child: ApiPaginatedDataTable(...))
```

原因：`600.h` 在實際瀏覽器約 557px，顯示 10 列需 580px → 只顯示 9 列。

## ⚠️ DataCell 游標規則

| onTap | 內容 | 游標 | 用途 |
|-------|------|------|------|
| `null` | 無 | ↖ 箭頭 | 空格子 / 純展示 |
| `null` | `SelectionArea` | I I-beam | 有文字可複製 |
| `callback` | 任意 | 👆 手指 | 可點擊操作 |

```dart
// 標準純展示 cell
final isEmpty = cellText.isEmpty || cellText == '-';
return DataCell(
  onTap: null,
  isEmpty
      ? DataCellWithTip(text: cellText)
      : SelectionArea(child: DataCellWithTip(text: cellText)),
);
```

## 新增 API 端點流程

1. 在 `restclient/` 新增 Retrofit 方法
2. 執行 `flutter pub run build_runner build --delete-conflicting-outputs`
3. 在 `notifier/` 新增對應 StateNotifier
4. 更新 UI

## 相關連結

- [[b2b-manager/overview]] — 技術棧詳細說明
- [[b2b-manager/dev-workflow]] — 常用開發指令
- [[b2b-manager/bugs]] — 已知 Bug 與修復記錄
