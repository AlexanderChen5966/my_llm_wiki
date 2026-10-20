---
title: swarm-forge：Uncle Bob 把 Clean Code 原則寫進多代理的行為規範
date: 2026-09-01
tags: [ai/agent, dev/architecture]
source: https://www.inside.com.tw/article/42246-unclebob-swarm-forge-ai-agent-deployment
type: note
status: processed
asset_value: 48
---

《Clean Code》作者 Robert C. Martin（Uncle Bob）今年 7 月宣布不再逐行閱讀 AI 產生的程式碼，並開源 swarm-forge 落實這個哲學：以 tmux session 隔離為底層的多代理並行協作平台，提供 two-pack／four-pack／six-pack 三種預設編組，代理間用 handshake 協定傳遞任務避免非同步覆蓋，另附 web 儀表板監控。核心設計是把 Clean Code 原則（可讀性、命名規範、函式長度限制）直接寫進每個代理的行為規範（constitution），把品質約束從「事後人工審查」前移到「生成端結構性限制」——與主流「先求快速產出、人工事後審查」的邏輯相反。目前僅為新聞報導層級介紹，缺乏第一手實測與踩坑細節，若日後有人實際試用並寫紀錄，可再考慮升級為完整 wiki 條目。

## 相關連結
- [[subagent-team-protocol-refactor]] — 機制不同但互補：該篇是規則精簡與執行期任務派工協定的單一定義，本篇是把約束寫進生成端行為規範
- [[agent-permission-and-os-sandbox]] — 機制不同但互補：該篇是執行期權限攔截與 OS 沙箱分級，本篇是生成端結構性約束前移
