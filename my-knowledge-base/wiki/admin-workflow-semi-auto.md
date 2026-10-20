---
title: 行政專案半自動工作流系統
date: 2026-06-25
tags: [ai/agent, pkm/workflow]
source: raw/行政專案半自動工作流系統.jpg
type: note
status: processed
source_format: image
---

## T（核心論點）

用 Google 生態（Form/Sheet/Drive/Calendar/Gmail）+ Gemini/NotebookLM 建立行政專案半自動工作流：收到公文後一鍵啟動，系統自動建立專案資料夾、產出文件、加入日曆提醒，全程追蹤至成果輸出，讓行政工作更輕鬆。

## B（背景脈絡）

- 來源：架構圖（JPG），完整說明六步驟流程 + 系統架構
- 定位：行政工作者的 AI 輔助流程，不需寫程式，使用 Google 生態 + AI 助手
- 核心工具：Google Apps Script（觸發自動化）+ Gemini/NotebookLM（AI 輔助生成）

## R（重要細節）

### 六步驟完整工作流

1. **填寫動表**：收到公文 → 在 Google Form 填入「公文類型標籤、主旨、期限」等基本資訊
2. **自動建立專案**：Apps Script 依表單觸發，自動建立 Google Drive 專案資料夾 + 結構化子目錄
3. **自動產生文件**：依公文類型產生對應文件（公文稿/簡報/計畫書/成效表格）
4. **日曆提醒與通知**：自動加入 Google Calendar 任務，重要節點 Email 通知相關人員
5. **執行與追蹤**：依「待辦追蹤表 Sheet」逐項執行，任務/負責人/狀態/截止/一目了然
6. **成果產出與結案**：使用 Gemini/NotebookLM 彙整成果，產出結案報告

### 自動建立的專案資料夾結構

```
00_原始公文與會議資料
01_公文稿（簽核文件、對外公文）
02_工作工具與過程記錄
03_簡報與視覺素材
04_表單與調查（含回覆）
05_外部夥伴資料
06_財務與預算
07_法規合約相關文件
08_成效評估與報告
09_後續下次改進記錄
```

### AI 輔助功能

- **Gemini**：協助起草文字（公文/簡報/計畫書/成效表格）
- **NotebookLM**：整理資料、彙整重點、快速整理成果

### 系統帶來的效益

- 節省約 60% 的行政時間
- 降低遺漏與文件混亂
- 大幅降低重複性工作
- 資料完整留存，利於稽核與傳承
- 輔助 NotebookLM 作為智慧資產

### 快速啟用步驟（一次設定，長期受用）

1. 準備三個 ID（Drive 資料夾 ID / Google Calendar ID）
2. 使用 Apps Script 建立自動化
3. 貼上程式碼（Apps Script）
4. 設定觸發器（onFormSubmit）
5. 開始使用

## C（行動清單）

- [ ] 評估現有行政流程中最耗時的重複部分（公文/會議/報告）
- [ ] 用 Google Form 設計資料收集表單（先做 MVP，之後再精化）
- [ ] 建立 Apps Script 觸發器（onFormSubmit 觸發 Drive 資料夾建立）
- [ ] 整合 Gemini 到文件產出環節（草稿生成）

## 相關條目

- [[ai-prompt-map-100-workflows]] — 100+ AI 工作流 Prompt 地圖
- [[claude-code-advanced-workflow]] — Claude Code 自動化工作流
- [[agent-skill-practical-guide]] — Agent Skill 實作指南
