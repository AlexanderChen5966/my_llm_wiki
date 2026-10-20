---
title: ChatGPT 排程任務的差異判準 prompt 寫法
date: 2026-08-20
tags: [ai/prompt-engineering, ai/workflow, type/how-to]
source: https://www.bnext.com.tw/article/91928/chatgpt-scheduled-task-prompt
type: note
status: processed
asset_value: 24
---

## 核心論點

ChatGPT 排程任務已能記住上次執行結果並自動比對差異，但多數人收到的仍是「每天都差不多」的廢話報告——因為 prompt 裡沒有定義「什麼變化才算有意義」，也沒有明確抑制「重整昨天資料」的行為。

## 重點整理

- 使用門檻：需 ChatGPT Plus 以上方案；入口在網頁版左側欄「排程」
- 四步驟：①寫清楚追蹤主題與精確來源清單（來源越精確越不浪費算力爬無關頁面）②定義可量化的變化判準 ③沒符合就只回「無重大變化」④設定達成後自動停止
- 判準要具體可量化，例如「出現新功能公告」「售價浮動超過 5%」「有特殊 Sale 活動」
- 關鍵抑制句：「比對上次結果，只回報符合判準的差異，沒有符合就回覆無重大變化，不要重新整理昨天的資料」——這句話是用來停止「時間戳不同、內容一樣」的廢話通知
- 通知格式要求四項：來源連結、發布時間、具體變化、建議行動
- 可直接複製的模板：「每個工作日早上 8:00，用 [來源清單] 查詢 [追蹤主題]。比對上次結果，只在出現以下情況時通知我：[有意義變化的定義]。通知時附：來源連結、發布時間、具體變化、建議行動。沒有符合的變化，只回覆：無重大變化。[目的達成後] 停止這個任務。」

## 可用之處

同一組「判準 + 抑制重述 + 停止條件」可直接搬到本知識庫的定期掃描類 Skill（daily-brief-generator、wiki-health-check）：把「什麼算有變化」寫死成可量化判準，沒變化就回一行，避免每日簡報變成內容重複的儀式性報告。

## 相關連結

- [[agent-task-system-background-scheduling]]
- [[chatgpt-prompt-4-techniques]]
- [[ai-prompt-map-100-workflows]]
