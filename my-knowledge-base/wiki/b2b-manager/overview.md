---
title: B2B Manager 專案概覽
date: 2026-05-13
tags: [dev/flutter, dev/architecture]
source: /Users/alexander/GitLab/b2b-manager/docs/claude/claude-overview.md
type: reference
status: processed
---

## 摘要

山隆 B2B 智慧平台，純 Flutter Web 應用，管理石油 B2B 業務（車輛、司機、訂單、發票）。Web Only，不支援 iOS/Android，部署於 AWS ECS (Fargate)。

## 基本資訊

| 項目 | 內容 |
|------|------|
| 版本 | 1.7.8+37 |
| 框架 | Flutter 3.3.0+ / Dart >=3.3.0 |
| 平台 | Web Only（JS + WebAssembly） |
| 基準解析度 | 1920×1024 |
| 主要語言 | 繁體中文、簡體中文、英文 |
| 部署 | Docker / Nginx → AWS ECS |

## 核心技術棧

| 分類 | 套件 | 用途 |
|------|------|------|
| 狀態管理 | flutter_riverpod ^2.6.1 | 主要狀態管理 |
| 路由 | go_router ^14.6.1 | 宣告式路由 |
| HTTP | dio + retrofit | 型別安全 REST API |
| 認證 | oauth2 + flutter_line_sdk | OAuth2 + Line 登入 |
| 圖表 | fl_chart ^0.69.0 | 折線/圓餅/長條圖 |
| 響應式 | flutter_screenutil ^5.9.3 | 1920×1024 為基準 |
| 監控 | sentry_flutter ^9.6.0 | 錯誤追蹤（正式環境）|
| 測試 | mockito ^5.4.6 | 單元測試 |

## 自訂套件（⚠️ 禁止 pub upgrade）

位於 `external_libs/`，為專案特製版本：
- `data_table_2` — 表格元件（大量客製化）
- `filter_list` — 篩選列表
- `flutter_json_view` — JSON 檢視器
- `font_awesome_flutter` — 圖示
- `multi_dropdown` → `multiselect_dropdown` — 多選下拉

## Provider 類型選擇原則

| Provider | 使用場景 |
|----------|----------|
| `StateProvider` | 簡單狀態（布林、展開/收合） |
| `StateNotifierProvider` | 複雜業務邏輯（CRUD） |
| `FutureProvider` | 非同步操作 |
| `ChangeNotifierProvider` | 與舊有 Provider 整合 |

## 響應式尺寸規則

```dart
// 正確用法
width: 200.w      // 寬度用 .w
height: 100.h     // 高度用 .h
fontSize: 14.sp   // 字體用 .sp
// 最小 icon 尺寸：20.w  最小字體：12.sp
// ⚠️ DataTable 高度例外：必須用固定 px，不用 .h
```

## 相關連結

- [[b2b-manager/architecture]] — 詳細架構與目錄結構
- [[b2b-manager/dev-workflow]] — 開發指令與工作流程
- [[b2b-manager/api]] — API 端點總覽
- [[b2b-manager/INDEX]] — 知識庫索引
