---
title: AI-Pair v4 重構建議
date: 2026-05-25
tags: [ai/agent, project/ai-pair]
source: 個人分析文件
type: project
status: processed
asset_value: 40
---

## T（核心論點）

AI-Pair Skill 從 v3 升級至 v4 的重構設計文件。核心變更：根據任務複雜度**動態分配 developer 與 reviewer 的模型 tier**，充分利用 Copilot Pro 帳號中 GPT-5 mini（FREE）與 GPT-4.1（FREE）的免費額度。

估算節省：
- 簡單任務（typo/i18n）：**節省 100%**（Sonnet → GPT-5 mini FREE）
- 標準任務（feature/bugfix）：**節省 67%**（Sonnet → GPT-5.4 mini 0.33x）
- 複雜任務（架構）：0% 節省，但品質更高（GPT-5.3-Codex）

### 四個 Tier（Developer）

| Tier | Developer | Reviewer | 成本 |
|------|-----------|----------|------|
| FREE | GPT-5 mini | GPT-5 mini | 0x |
| STANDARD | GPT-5.4 mini | GPT-4.1 | 0-0.33x |
| HIGH | GPT-5.3-Codex | GPT-5.4 | 1x |

### 三個 Review Level

- **Level 1**（--quick）：GPT-5 mini + Gemini Lite，~20s
- **Level 2**（預設）：GPT-4.1 + Claude native，~2min
- **Level 3**（--deep）：GPT-5.4/Codex + Claude + Gemini Preview，~3min

## 原始摘要

AI-Pair Skill 從 v3 升級至 v4 的重構設計文件，核心變更為根據任務複雜度動態分配 developer 與 reviewer 的模型 tier，充分利用 Copilot Pro 帳號中 GPT-5 mini 與 GPT-4.1 的免費額度，估算簡單任務可節省 100%、標準任務可節省 67% 成本。

## 相關連結

- [[projects/ai-pair-v4-1-correction]] — v4.1 校正文件（移除兩個誤判、新發現四個缺口）
- [[ai-pair-skill]] — AI-Pair wiki 條目
- [[claude-pilot-suite]] — 動態模型分配邏輯的靈感來源
