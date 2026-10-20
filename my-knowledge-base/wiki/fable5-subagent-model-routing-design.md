---
title: Fable 5 省 Token 設定：subagent 角色分離模型路由（judge/builder/worker）
date: 2026-07-09
tags: [ai/claude, ai/agent, type/how-to]
source: https://codelove.tw/@tony/post/x0nYDa（原文：https://qiita.com/fwatanab/items/e2c51c2a783915b275a9）
type: article
status: processed
asset_value: 56
---

Fable 5 期間限定開放後 token 消耗太兇猛，作者設計一套「只在設計/審查用 Fable、其餘交給 Opus」的 subagent 角色分離機制，關鍵原則是「不在 skill 裡寫死模型名稱」——model 設定只放在 `.claude/agents/*.md` 的 frontmatter，skill 只呼叫角色名稱，模型隨時可換而不用改 skill。

## T（核心論點）

Token 消耗集中在「平行展開的調查類管線與實作」而非「思考判斷」，因此把 subagent 拆成三種角色（judge 審查驗證／builder 實作／worker 調查擷取）+ 主 session，讓**誤判會導致事故的部分**（設計/規劃/審查）用高階模型（Fable），**大量處理是瓶頸的部分**（實作/調查/全面擷取）用次階模型（Opus），是比「全部用同一模型」更省成本的路由原則。核心工程手法是**模型名稱與 skill 邏輯解耦**——只寫在 agent 定義檔的 frontmatter，不寫死在 skill 裡。

## B（適用場景）

限時開放的高階模型（如 Fable 5）token 消耗過快時的過渡期設定；任何想長期用「角色分工決定模型分配」而非「單一模型跑到底」的 Claude Code 多 subagent 架構；未來模型世代更替時想降低遷移成本的專案。

## R（重點整理）

**角色分配表**：

| 層級 | 負責模型 | 主要工作 |
|------|---------|---------|
| 主體 + `judge` | Fable | 設計、規劃、審查、驗證（誤判會導致事故的部分） |
| `builder` / `worker` | Opus | 實作、調查、全面擷取（大量處理是瓶頸的部分） |

**三步設定法**：
1. `.claude/agents/` 下每個角色一個 md 檔，`model:` 寫在 frontmatter（如 `worker.md` 設 `model: opus`）；**judge.md 刻意不寫 `model:`**——未指定的 agent 會繼承主 session 模型，這樣切換 `/model` 時 judge 自動跟著換
2. skill 裡只呼叫角色名稱（如「並列啟動 3 個審查者 subagent_type: judge」），完全不出現模型名稱
3. 要切換模型時，只改各 agent md 的 `model:` 欄位（1-3 處），或用 `/model` 切主體+judge

**兩個優點**：維護性（模型名稱不散落在各 skill 裡，改動集中在 1-3 個檔案）／通用性（「判斷管線 vs 大量處理管線分級」是相對規則，換成任何新一代高階模型+次階模型組合都能沿用同一套設計）。

## C（關鍵行動）

- [ ] 多 subagent 架構一律把 `model:` 放在 agent 定義檔而非寫死在 skill/prompt 裡
- [ ] 高風險判斷型 agent（judge 類）可考慮不指定 model，讓它繼承主 session、隨 `/model` 切換
- [ ] 限時開放模型（Fable 5 類）token 消耗過快時，優先評估「角色分級路由」而非直接全面降級模型

## 相關連結

- [[claude-fable-5-usage-guide]] — Fable 5 官方使用指南
- [[claude-fable-5-power-tasks]] — Fable 5 值得衝的任務清單，貴模型搭架構+便宜模型維護的同源思路
- [[agent-os-workstation-division]] — 工位分工模型（核查者不能是提煉者自己），角色分離的姊妹原則
