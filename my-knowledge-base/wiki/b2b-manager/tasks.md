---
title: B2B Manager 任務文件狀態
date: 2026-05-13
tags: [dev/flutter, project/b2b-manager]
source: /Users/alexander/GitLab/b2b-manager/docs/shared/readme.md
type: project
status: processed
---

## 摘要

b2b-manager 跨模組共用任務文件，由 `/doc-update` 自動維護。完整任務文件在 `docs/shared/`。

## 任務狀態

| 任務 | 說明 | 狀態 |
|------|------|------|
| `order_fuel_type_ellipsis_tooltip` | 油品消費紀錄品項欄截斷 + Hover Tooltip | ✅ 2026-03-17 |
| `invoice_status_update_task` | InvoiceStatus 完整支援 + 明細下載 API 遷移 | ✅ 2026-03-31 |
| `annotation_name_task` | 群組搜尋新增 annotationName 欄位 | ✅ 2026-03-27 |
| `batch_edit_car_requirements` | 車輛批次修改功能（含 UI 修訂、重構、搜尋模式） | ✅ 2026-04-16 |
| `batch_edit_label_clear_option` | 批次修改標籤新增「全刪除」選項 | ✅ 2026-05-11 |
| `batch_edit_filter_multiselect` | 批次修改篩選：標籤與油品改為多選 (SSGS-11402) | ✅ 2026-05-11 |
| `virtual_account_display_refactor` | 虛擬帳戶顯示條件改為 hasVirtualAccount（API v1.8）| ✅ 2026-05-11 |
| `sort_data_tables` | 全站 11 個表格排序功能（10 個子任務） | 📋 待實作 |
| `batch_edit_car_undo` | 批次修改成功後 10 分鐘復原機制（純前端） | ⏳ 待實作 |

## 開發模板

| 模板 | 位置 | 用途 |
|------|------|------|
| Playwright 測試 | `shared/test_template.md` | Flutter CanvasKit Web 測試 |
| 任務文件 v2 | `shared/test_template_v2.md` | 新版任務文件格式 |

## 待實作事項

**sort_data_tables**（優先級高）：
- 全站 11 個表格需新增排序功能
- 詳細需求見 `docs/shared/sort_data_tables.md`

**batch_edit_car_undo**：
- 批次修改後 10 分鐘內可復原
- 純前端實作（不需後端 API）

## 相關連結

- [[b2b-manager/bugs]] — 相關 Bug 記錄
- [[b2b-manager/pages]] — 各頁面架構
