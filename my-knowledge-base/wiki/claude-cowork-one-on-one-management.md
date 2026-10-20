---
title: Claude Project＋Cowork 管理 1-on-1 面談：追蹤團隊承諾、面談前自動產議程
date: 2026-07-16
tags: [ai/claude, ai/workflow]
source: https://www.bnext.com.tw/article/91498/claude-cowork-one-on-one-system
type: article
status: processed
asset_value: 58
---

主管用 Claude Project 搭配 Cowork，把散落各處的 1-on-1 面談筆記整理成一個持續累積的系統：面談前 Claude 產議程、面談後標出未完成承諾。

⚠️ 提醒：Cowork 已於 2026-09-16 併入一般對話 Chat，「Cowork」現在指能力而非獨立分頁，詳見 [[claude-cowork-merged-into-chat-2026-09]]。

## T（核心論點）

AI 負責整理歷史、辨識模式、草擬問題，但每個員工的績效判斷、公平性與輔導方式仍必須由主管親自決定——這是 AI 輔助管理場景中「AI 整理脈絡、人類保留判斷」分工模式的具體實作，屬於單一應用場景的可複製流程，而非通用管理框架。

## B（適用場景）

**適用：** 帶多人團隊、需要結構化紀錄與追蹤 1-on-1 承諾事項的主管。
**不適用 / 注意：** 涉及個人隱私、健康、性格特徵的推測必須明確禁止 AI 進行，僅能根據書面紀錄客觀總結。

## R（操作步驟）

1. 在 Claude 新增「1 on 1」Project，連結存放過去面談筆記/逐字稿的資料夾；沒有舊紀錄就從錄音逐字稿開始
2. 請 Claude 讀取資料，建立 `team.md`：成員、工作內容、常見議題、未來注意主題（明確禁止推測隱私/健康/性格）
3. 請 Claude 依對團隊的理解，制定統一 1-on-1 溝通模板與面談後評估框架，存成兩份專案文件
4. 每次面談後，把逐字稿放入資料夾，請 Claude 依評估框架寫成報告，列出已達成承諾與待跟進事項，無法評估之處明確標註
5. 下次面談前 5 分鐘，用固定 prompt 查看最近紀錄、評估框架、追蹤表，讓 Claude 生成議程（需跟進事項/已改善事項/該提出的問題）

## C（關鍵行動）

- [ ] 建立 Project 時第一步就寫清楚「不要推測個人隱私、健康、性格特徵」的限制
- [ ] 評估框架與模板統一存成專案文件，避免每次面談各自為政
- [ ] 面談前 5 分鐘用固定 prompt 產議程，取代靠記憶回想

## 相關連結

- [[claude-personal-data-dashboard-case-study]] — 同系列作者另一實戰案例（圖書借閱紀錄建成長儀表板），「AI 整理歷史、人類負責判斷」的分工模式相同
- [[claude-cowork-setup-guide]] — Claude Cowork 完整設定指南，可補充本篇未涵蓋的 Project 基礎設定細節
