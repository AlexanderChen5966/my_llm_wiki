---
title: Claude Tag：Slack 團隊 AI 協作功能
date: 2026-06-25
tags: [ai/claude, ai/agent]
source: https://www.bnext.com.tw/article/91311/anthropic-claude-tag-slack-team-collaboration-ai
type: article
status: processed
---

## T（核心論點）

Claude Tag 是 Anthropic 在 Slack 上推出的團隊 AI 協作功能：用 @Claude 指派任務，Claude 作為「多人共用虛擬同事」運作，具跨頻道長期記憶與主動模式，是 Claude Code 路線上向團隊協作延伸的重要一步。

## B（背景脈絡）

- 發布時間：2026-06-24，Beta 開放給 Claude Enterprise 與 Claude Team 付費用戶
- 取代既有「Claude in Slack」App，管理者有 30 天遷移窗口
- 底層使用 Opus 4.8 模型
- Anthropic 內部已有約 65% 程式碼由內部版 Claude Tag 產出

## R（重要細節）

### 核心設計
- **頻道共享實體**：單一 @Claude 在頻道中所有成員都看得到它的任務與回覆，非一對一機器人
- **跨頻道長期記憶**：Claude 持續跟隨頻道討論脈絡，累積專案背景、專有名詞、團隊習慣；不跨越私密頻道
- **主動模式（ambient mode）**：啟用後 Claude 主動彙整更新、提出提醒、追蹤擱置任務
- **異步長時間任務**：可同時指派多個工作給不同頻道 Claude，背景執行數小時甚至數天

### 權限管理
- 系統管理員可為不同用途建立各自獨立的 Claude 身分（銷售 vs 工程各自隔離）
- 可設定每月 Token 消耗上限（組織 + 個別頻道層級）
- 操作紀錄可查（每項任務 + 發起人）
- 私訊模式：個人化工具與連結器，滿足私密工作流需求

### 擴展場景
- 工程：程式碼審查、批次 refactor
- 產品數據：分析報告
- 客服票務：分流處理
- 疑難技術：問題排查

## C（行動清單）

- [ ] 確認個人或組織帳號是否符合 Enterprise/Team 資格
- [ ] 評估導入 Claude Tag 的適用場景（重複性工作流優先）
- [ ] 注意：Token 上限設定是控制成本的關鍵設定項

## 相關條目

- [[claude-opus-4-8-release-notes]] — Claude Tag 底層模型
- [[claude-code-advanced-workflow]] — Claude Code 路線延伸脈絡
- [[ai-coworker-vs-ai-agent]] — AI 虛擬同事 vs Agent 概念對比
- [[agent-session-management]] — 跨 session 記憶管理相關
- [[anthropic-claude-ai-perf-sprint-metric-first-loop]] — 實際使用案例：Anthropic 團隊在 Slack 頻道以 Claude Tag 為主力做 claude.ai 效能衝刺（2026-09-29 新增）
- [[ideas/openai-devday-2026-chatgpt-platform-updates]] — 跨廠商對照（2026-10-08）：DevDay 2026 起 @ChatGPT 也進入 Slack／Microsoft Teams（Business／Enterprise；頻道、thread、DM；管理員核准工具＋個人授權雙層），另有可排程或事件觸發的 Team Tasks；aiposthub 單一來源、未實測
- [[ideas/claude-managed-agents-dynamic-workflows-every-2026-10]] — 📎 2026-10-19 新增：⚠️ 同為 Slack 入口但產品不同——Every 是用 Claude Managed Agents **自建**的公司代理（共用 Skills，後開放訂閱者），不是 Claude Tag；aiposthub 轉述官方 X 貼文
