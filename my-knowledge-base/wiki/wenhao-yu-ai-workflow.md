---
title: WenHao Yu 個人 AI 工作流系統
date: 2026-05-13
tags: [pkm/automation, ai/claude, pkm/workflow]
source: https://yu-wenhao.com/zh-TW/blog/personal-panopticon/
type: article
status: processed
asset_value: 80
---

## T（核心論點）
用 Claude Code + Obsidian 打造「讓 AI 看見自己人生全貌的鏡子」——每天輸入兩個字「開工」，AI 自動跑 8 個步驟產出 daily brief，讓決策者只需決策、不再執行。

## B（適用場景）
**適用：** 需要整合多個資訊來源（郵件、行程、目標、專案）的知識工作者；願意花時間初始設定的人
**不適用：** 只需要筆記整理、不需要跨系統整合的輕量場景

## R（操作步驟）
1. 建立可程式化基礎設施（Obsidian + .md + 有 API 的服務）
2. 設定 Claude Code 指向 Obsidian Vault
3. 建立 daily brief 工作流：每天「開工」觸發 8 個步驟
4. 整合 Google Calendar、Gmail、iCloud 等資料來源
5. 建立三色決策系統（🟢 AI 執行 / 🟡 AI 備選項人決策 / 🔴 人親自處理）
6. 持續迭代：三個月從空資料夾長成完整系統

## C（關鍵行動）
- [ ] 選服務第一件事：有沒有 API？沒有 API = 數據被困住
- [ ] 做三次以上的事一定要自動化（two-strike rule）
- [ ] daily brief 的觸發詞要極度簡單（兩個字就夠）

## 原始摘要
WenHao Yu 的 Personal Panopticon 系統，靈感來自 Molly Cantillon 的「讓 AI 同時看 8 個領域」。三個設計原則：可程式化基礎設施、數據驅動決策、AI 自動化執行。Daily brief 整合目標追蹤、郵件掃描、行程同步、熱門內容抓取。花三個月從空資料夾建成完整系統。

## 相關連結
- [[karpathy-knowledge-base-method]] — 知識庫輸入端方法
- [[claude-code-obsidian-setup]] — Claude Code + Obsidian 技術設定
- [[daily-reading-assistant-ai]] — AI 每日閱讀助手（每天推 400 字）
- [[ccar-f-exam-prep-architecture-judgment-rules]] — 同一作者：CCAR-F 應試心得與架構判斷法則（2026-09-23 新增）
