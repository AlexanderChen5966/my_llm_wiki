---
title: B2B Manager 知識庫索引
date: 2026-05-13
tags: [dev/flutter, project/b2b-manager]
type: reference
status: processed
---

# B2B Manager 知識庫

> 山隆 B2B 智慧平台（Flutter Web），原始文件位於 `/Users/alexander/GitLab/b2b-manager/docs/`

## 快速查詢

| 我想知道... | 去這裡 |
|------------|--------|
| 技術棧、套件版本、部署方式 | [[b2b-manager/overview]] |
| 目錄結構、Riverpod 模式、DataTable 規則 | [[b2b-manager/architecture]] |
| 開發指令、建置、測試 | [[b2b-manager/dev-workflow]] |
| API 端點有哪些模組 | [[b2b-manager/api]] |
| 各頁面的檔案結構和 API 對應 | [[b2b-manager/pages]] |
| 某個 Bug 怎麼修的 | [[b2b-manager/bugs]] |
| 目前有哪些任務、完成狀態 | [[b2b-manager/tasks]] |
| 錯誤訊息怎麼解、DataTable 怪問題 | [[b2b-manager/troubleshooting]] |

## Wiki 條目

| 條目 | 摘要 |
|------|------|
| [[b2b-manager/overview]] | 專案基本資訊、技術棧、套件清單 |
| [[b2b-manager/architecture]] | 目錄結構、TaskManager、DataTable/DataCell 高頻回歸規則 |
| [[b2b-manager/dev-workflow]] | flutter run / build / test 常用指令 |
| [[b2b-manager/api]] | 17 個 API 模組清單，v12 最新版 |
| [[b2b-manager/pages]] | 各功能頁面架構文件索引 |
| [[b2b-manager/bugs]] | 11 個 Bug 記錄 + 5 大高頻回歸模式 |
| [[b2b-manager/tasks]] | 9 個任務狀態（7 完成 / 2 待實作）|
| [[b2b-manager/troubleshooting]] | 6 項常見錯誤 + 5 項 DataTable 問題速查，含監控工具與除錯指令 |

## 原始文件結構

```
docs/
├── api/              # API 文件 v11/v12
├── bug_record/       # 11 個 Bug 修復記錄
├── claude/           # 架構規則、開發規範、外部套件說明
├── page_architecture/ # 各頁面詳細架構文件
├── shared/           # 任務文件狀態索引
├── widget/           # Widget 狀態管理指南與模板
└── ai-collaboration/ # AI 協作設定與記錄
```
