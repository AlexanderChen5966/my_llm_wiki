---
title: MacBook 蓋蓋子不休眠：讓 AI Agent 整夜跑的四種方法
date: 2026-05-28
tags: [ai/workflow, ai/agent]
source: https://www.koc.com.tw/archives/642626
type: article
status: processed
asset_value: 18
---

## 核心論點

macOS 預設「蓋蓋子＝睡覺」，但 AI Agent 長時間自主開發需要 Mac 保持運作。以下四種方法可讓 MacBook 在合蓋狀態繼續運作。

## 四種方法

| 方法 | 適用場景 | 注意事項 |
|------|---------|---------|
| **1. 外接螢幕**（官方 Clamshell Mode）| 最穩定，macOS 原生支援 | 需要外接螢幕才能啟用 |
| **2. Amphetamine / Lungo**（防睡眠 App）| 彈性控制，可設定時間 | 需要安裝第三方 App |
| **3. pmset 命令**（Terminal）| 工程師友好，精確控制 | `sudo pmset -a sleep 0` 關掉睡眠 |
| **4. 外接電源 + 設定**（系統設定）| 在系統設定調整「插電時不睡眠」| 需要接電源，電池模式無效 |

## 適用情境

跑 Claude Code 長任務（/loop、/schedule、gnhf）、訓練腳本、爬蟲、AI 代理背景任務。

## 相關連結

- [[claude-code-advanced-workflow]] — /loop 排程與長時間任務
- [[gnhf-autonomous-orchestrator]] — gnhf：整晚自主迭代開發的 Orchestrator
