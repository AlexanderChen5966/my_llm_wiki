---
title: B2B Manager Bug 記錄與修復模式
date: 2026-05-13
tags: [dev/flutter, dev/architecture]
source: /Users/alexander/GitLab/b2b-manager/docs/bug_record/
type: reference
status: processed
---

## 摘要

b2b-manager 已知 Bug 修復記錄。從歷史 Bug 中可提取出幾個高頻回歸區：狀態更新、游標行為、API 重複呼叫、Immutability。

## Bug 記錄索引

| Bug | 檔案 | 根本原因 |
|-----|------|----------|
| 年度消費圖表載入卡住 | `bar_chart_loading_stuck_fix.md` | 狀態未正確更新，UI 不重繪 |
| 車輛 Drawer FormatException | `car_drawer_format_exception_none_constraint.md` | `refueling_constraint_type=NONE` 時 double 解析失敗 |
| 洗車副產品過濾 Bug | `car_wash_other_product_filter_bug.md` | CAR_WASH/OTHER 新類別未同步更新前端過濾邏輯 |
| 車輛 Excel 匯入洗車方案 | `cars_upload_car_wash_enabled_bug.md` | `car_wash_enabled` 解析錯誤，`是` 未轉換為 `true` |
| 帳戶異動 productCode null | `changelog_product_filter_productcode_null_fix.md` | 未選異動來源時 productCode 為 null |
| 公司切換錯誤 | `company_switch_bug_fix.md` | 切換公司時狀態殘留 |
| Dashboard 圓餅圖回滾 | `dashboard_rollback.md` | 重構後資料來源錯誤，已回滾 |
| Immutability 重構 | `immutability_refactoring_guide.md` | PageState/PageStateNotifier immutability 問題 |
| Invoice 游標 Merge Conflict | `invoice_cursor_merge_conflict_resolution.md` | 游標邏輯 merge 衝突（2026-03-17）|
| 發票下載游標回歸 | `invoice_download_cursor_regression.md` | 不可下載格應為箭頭游標，誤改為手指 |
| 報表清空按鈕重複 API | `report_page_clear_button_duplicate_api_calls_fix.md` | 清空觸發 4 次相同 API 呼叫 |

## 高頻回歸模式

### 1. DataCell 游標被意外改變
**症狀：** 所有儲存格顯示手指游標，或不可點擊格顯示錯誤游標
**防範：** 修改表格時先看 [[b2b-manager/architecture]] 的 DataCell 游標規則

### 2. 狀態更新後 UI 不刷新
**症狀：** 資料已從 API 取得，但畫面仍顯示 loading 或舊資料
**防範：** 確認 `StateNotifier` 有發出新狀態，`ref.watch` 有訂閱到

### 3. API 重複呼叫
**症狀：** 一個操作觸發多次相同 API
**防範：** 使用 TaskManager；搜尋條件變更時確認 debounce

### 4. 新 API 欄位未同步
**症狀：** 後端新增欄位/類別後，前端過濾/解析仍用舊邏輯
**防範：** 後端 API 變更時全域搜尋相關 enum 和 filter 邏輯

### 5. Immutability 問題
**症狀：** 狀態更新後某些子物件未被正確更新
**防範：** `PageState` 的所有欄位更新都需用 `copyWith`

## 相關連結

- [[b2b-manager/architecture]] — DataTable 和游標的完整規則
- [[b2b-manager/pages]] — 各頁面架構（Bug 發生位置）
