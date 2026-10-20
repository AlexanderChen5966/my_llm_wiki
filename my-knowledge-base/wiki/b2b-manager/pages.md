---
title: B2B Manager 頁面架構
date: 2026-05-13
tags: [dev/flutter, dev/architecture]
source: /Users/alexander/GitLab/b2b-manager/docs/page_architecture/
type: reference
status: processed
---

## 摘要

B2B Manager 各功能頁面的架構文件索引。每個頁面文件包含：檔案結構、API 端點、資料模型、狀態管理、UI 元件說明。

## 頁面文件索引

| 頁面 | 文件位置 | 說明 |
|------|----------|------|
| 首頁儀表板 | `page_architecture/home/home_dashboard_architecture.md` | 圖表、統計、公司切換 |
| 首頁分析 | `page_architecture/home/home_page_analysis.md` | 年度消費圖、數據載入 |
| 洗車副產品 | `page_architecture/home/car_wash_byproduct_complete_guide.md` | 洗車/副產品完整指南 |
| 發票頁面 | `page_architecture/invoices_architecture.md` | 發票列表、下載、開立 |
| 車輛列表 | `page_architecture/car_list_documentation.md` | 車隊管理、批次操作 |
| 司機列表 | `page_architecture/driver_list_documentation.md` | 司機帳號管理 |
| 訂單報表 | `page_architecture/order/report_documentation.md` | 油品消費報表 |
| 推薦系統 | `page_architecture/order/referral_documentation.md` | 推薦追蹤 |
| 群組帳號異動 | `page_architecture/order/group_account_changelog_documentation.md` | 帳戶異動紀錄 |
| 日誌頁面 | `page_architecture/log_page.md` | 操作日誌 |
| DataTable 效能 | `page_architecture/datatable_performance_optimization.md` | 表格效能優化 |
| 程式碼重構分析 | `page_architecture/code_refactoring_analysis.md` | 重構指引 |

## 發票頁面重點

**檔案：** `lib/page/invoices/`
- `invoice_page.dart` — 列表主頁面
- `invoice_data_table.dart` — 表格 + 下載觸發
- `invoice_unpaid_remind.dart` — 未付費警告橫欄
- Ticket 申請單獨立子路由

**下載流程：**
```
單筆 PDF → invoice_data_table.dart
批量下載 → download_invoices_content.dart
明細 Excel → InvoiceStateNotifier
```

## 車輛頁面重點

**檔案：** `lib/page/car/`
- `car_page.dart` — 列表主頁面
- `car_delete_page.dart` — 批次刪除頁面

**支援功能：** 搜尋、新增、編輯、刪除、批次刪除、Excel 批次上傳、匯出列表

## 相關連結

- [[b2b-manager/architecture]] — 共用架構規則（DataTable 規則等）
- [[b2b-manager/api]] — API 端點對照
- [[b2b-manager/bugs]] — 各頁面的已知 Bug
