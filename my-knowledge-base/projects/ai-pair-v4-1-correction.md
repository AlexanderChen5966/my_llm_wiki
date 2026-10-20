---
title: AI-Pair v4.1 校正文件
date: 2026-05-25
tags: [ai/agent, project/ai-pair]
source: 個人分析文件
type: project
status: processed
asset_value: 40
---

## T（核心論點）

AI-Pair v4 的校正文件（不新增功能）。透過交叉分析四份文件（說明文件.md、Inter-Agent Protocol v1、claude-pilot-suite、gemini-cli-subagent），移除 v4 中兩個誤判，並新發現四個實作缺口。

### 移除的誤判（2 項）

1. **Gemini 雙倍 Token 消耗**：diff-only 原則已解決，三個 reviewer 共用同一份 git diff，無中間層轉譯
2. **違反向下委派原則**：AI-Pair 是跨工具異構架構，Pilot Suite 的「只向下委派」規則不適用

### 新發現的實作缺口（4 項）

| 優先級 | 缺口 | 說明 |
|-------|------|------|
| 🔴 高 | settings.local.json 未涵蓋 v4 新模型 | 不補會中斷 autopilot 流程 |
| 🔴 高 | 移除 Claude Haiku 4.5，L1 fallback 改 GPT-4.1（FREE） | 原設計 fallback 反而比主選貴 |
| 🟡 中 | TASK dispatch 協定未傳遞 tier 資訊 | Inter-Agent Protocol v1 缺少 MODEL_TIER 欄位 |
| 🟡 中 | Level 1 靜默從 1→2 reviewer | v4 加入 copilot-reviewer 到 L1，耗時估算需更新 |

## 原始摘要

AI-Pair v4 的校正文件（不新增功能），透過交叉分析移除 v4 中兩個誤判（Gemini 雙倍 Token 消耗、違反向下委派原則），並新發現四個實作缺口：settings.local.json 未涵蓋 v4 新模型、TASK dispatch 協定缺少 tier 傳遞欄位、Level 1 從 1 個變 2 個 reviewer 未標注、Claude Haiku 4.5 的 fallback 成本矛盾應改用 GPT-4.1（FREE）。

## 相關連結

- [[projects/ai-pair-v4-refactor]] — v4 原始重構建議
- [[ai-pair-skill]] — AI-Pair wiki 條目
