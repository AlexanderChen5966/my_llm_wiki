---
title: B2B Manager API 總覽
date: 2026-05-13
tags: [dev/flutter, dev/architecture]
source: /Users/alexander/GitLab/b2b-manager/docs/api/api_documentation_v12.md
type: reference
status: processed
---

## 摘要

SSGS B2B Backend API，OAuth2 JWT 認證，JSON 格式，共 17 個模組端點。完整文件位於 `docs/api/api_documentation_v12.md`（最新版本）。

## 基本資訊

| 項目 | 說明 |
|------|------|
| 認證 | OAuth2 JWT Bearer Token |
| 格式 | JSON / UTF-8 |
| Swagger | `/swagger-ui/index.html` |
| OpenAPI | `/v3/api-docs` |

## API 模組清單

| # | 模組 | 說明 |
|---|------|------|
| 4.1 | Account 帳戶管理 | 帳戶 CRUD、充值、虛擬帳戶 |
| 4.2 | Group 群組管理 | 群組、公司切換 |
| 4.3 | Order 訂單管理 | 油品訂單、異動紀錄 |
| 4.4 | Invoice 發票管理 | 發票列表、下載、開立 |
| 4.5 | Car 車輛管理 | 車輛 CRUD、批次操作、Excel 匯入 |
| 4.6 | Driver 司機管理 | 司機帳號管理 |
| 4.7 | Driver Attendance 出勤 | 司機出勤記錄 |
| 4.8 | Ticket 工單系統 | 發票申請單 |
| 4.9 | Label 標籤管理 | 車輛標籤 |
| 4.10 | Referral 推薦系統 | 推薦追蹤 |
| 4.11 | EIP Onboarding | EIP 上線流程 |
| 4.12 | EIP Webhook | EIP 事件回調 |
| 4.13 | Scheduled Jobs | 排程任務 |
| 4.14 | Public API | 公開端點 |
| 4.15 | Log 日誌管理 | 操作日誌 |
| 4.16 | User 用戶資訊 | 當前用戶資料 |
| 4.17 | Account Recharge 充值 | 帳戶充值管理 |

## 版本歷程

| 版本 | 檔案 |
|------|------|
| v12（最新）| `docs/api/api_documentation_v12.md` |
| v11 | `docs/api/api_documentation_v11.md` |

## 相關連結

- [[b2b-manager/pages]] — 各頁面對應的 API 端點
- [[b2b-manager/bugs]] — API 相關的 Bug 記錄
