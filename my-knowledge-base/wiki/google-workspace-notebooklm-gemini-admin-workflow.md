---
title: Google Workspace + NotebookLM × Gemini 行政專案工作流
date: 2026-05-25
tags: [ai/workflow, pkm/automation]
source: Facebook 社群貼文 / https://github.com/mihozip/google-workspace-admin-project-workflow
type: article
status: processed
asset_value: 30
---

## T（核心論點）

為學校行政設計的半自動化工作流：收到公文後填表啟動，系統自動在 Google Drive 建立專案資料夾與追蹤文件，再搭配 NotebookLM 整理脈絡、Gemini 輔助產出草稿。核心原則：**AI 負責固定重複流程，人工保留判斷與確認責任**——正式公告、經費核銷、公文回覆仍須人工確認。

六步驟流程：填寫啟動表 → 自動建立專案 → 自動產生文件 → 日期期限通知 → 執行與追蹤 → 成果產出

工具分工：
- **Google Form + Script**：觸發整個流程
- **Google Drive**：自動建立結構化資料夾（原始公文、計畫書、工作分工、表單回覆、經費、照片、成果等）
- **Google Calendar + Gmail**：期限提醒與通知
- **NotebookLM**：整合公文、附件、會議紀錄成可問答的「專案知識庫」
- **Gemini**：協助產出草稿（公文摘要、待辦清單、成果報告初稿等）

## 原始摘要

作者為學校行政設計的半自動化工作流，收到公文後填表啟動，系統自動在 Google Drive 建立專案資料夾與追蹤文件，再搭配 NotebookLM 整理脈絡、Gemini 輔助產出草稿，強調 AI 負責固定重複流程、人工保留判斷與確認責任。完整教學與專案放於 GitHub：https://github.com/mihozip/google-workspace-admin-project-workflow

## 補充資料

來源：raw/ 圖片素材（2026-05-25 補充）

**Google Workspace＋NotebookLM × Gemini 的行政專案工作流.jpg — 視覺化流程海報**
對應主文章的視覺化宣傳海報，完整呈現「行政專案半自動工作流系統」的六步驟流程圖（填寫啟動表 → 自動建立專案 → 自動產生文件 → 日期期限通知 → 執行與追蹤 → 成果產出），以及 Google Form、Sheet、Drive、Calendar、Gmail、Gemini/NotebookLM 各工具的分工說明。

## 相關連結

- [[claude-python-automation-workflow]] — 類似的 AI + 自動化分工模型（Claude + Python 版本）
- [[notebooklm-skill-research-pipeline]] — NotebookLM 作為研究管線的進階用法
- [[notebooklm-py-cli-tool]] — NotebookLM Python CLI 工具，可與此工作流整合
- [[gemini-study-notebooks]] — Gemini 學習筆記本（診斷→短課→進度儀表板）
- [[custom-gpt-gems-to-skills-migration]] — 自訂 GPT／Gems 遷移到 Skills 的跨廠商對照（2026-10-08 快照）
