---
title: SkillOpt：Microsoft 自我進化的 Agent Skill 最佳化框架
date: 2026-06-30
tags: [ai/agent, ai/claude, type/concept]
source: https://github.com/microsoft/SkillOpt
type: reference
status: processed
asset_value: 70
---

SkillOpt 是 Microsoft Research 開源的「skill 文字空間最佳化器」（arXiv:2605.23904）。核心理念：**把 skill 文件當成凍結 LLM 的可訓練狀態**，用類似訓練神經網路的紀律（epoch、batch、learning rate、validation gate）去訓練 skill 本身，但完全不動模型權重，部署時零額外推論成本。

## T（核心論點）

現有 agent skill 多靠手寫、強模型一次生成、或鬆散的自我修訂，沒有一個像「對 skill 本身做深度學習最佳化」、能在回饋下穩定優於起點。SkillOpt 用獨立的 optimizer 模型把「評分過的 rollout」轉成對單一 skill 文件的有界 add/delete/replace 編輯，**只有當候選編輯嚴格提升 held-out 驗證分數時才接受**。產出是一份精簡的 `best_skill.md`（典型 300–2,000 tokens），跑在未改動的目標模型上。

## B（背景與適用場景）

兩種定位（兩篇來源互補）：
- **研究框架（GitHub/論文）**：訓練可復用的自然語言 skill，含完整訓練迴圈與基準。
- **Skill/Tool 描述最佳化（中文介紹角度）**：當系統整合數十～數百個 skill/API/MCP 工具時，LLM 常「不是缺能力，而是不知道用哪個工具」。SkillOpt 自動重寫 skill 描述/metadata，讓模型更易判斷何時呼叫哪個工具——不重訓模型，只改「模型理解工具的方式」。

適合：AI Agent 平台、MCP Server 生態、多工具工作流、企業 AI 助理、Tool Calling 框架設計者。

## R（主要內容）

**訓練迴圈**：rollout → reflect → aggregate → select → update → evaluate。
- 文字版 learning-rate budget、rejected-edit buffer、epoch-wise slow/meta update 讓訓練穩定。
- 部署時 **zero inference-time model calls**。

**實測成績**：跨 6 個 benchmark、7 個目標模型、3 種執行 harness（direct chat / Codex CLI / Claude Code CLI），在全部 52 個 (model, benchmark, harness) cell 上 best 或 tied-best。GPT-5.5 上平均無 skill 準確率提升：direct chat **+23.5**、Codex loop **+24.8**、Claude Code **+19.1**。最佳化後的 skill 可跨模型規模、跨 Codex/Claude Code harness、跨鄰近 benchmark 遷移。

**生態**：
- v0.1.0 已上 PyPI（`pip install skillopt`），含完整訓練迴圈、多後端（OpenAI/Azure/Claude/Qwen/MiniMax）、6 內建 benchmark、WebUI dashboard。
- **SkillOpt-Sleep（preview）**：夜間離線自我進化伴侶，回顧過往 session、重放重複任務、在 held-out gate 後固化已驗證的 skill（給 Claude Code / Codex / Copilot）。
- 已被 gbrain、darwin-skill 等整合。

## C（對自身的啟發）

- [ ] 本知識庫的 skill（knowledge-organizer 等）描述若用 SkillOpt 思路檢視，可提升觸發準確率
- [ ] 「skill 文件 = 可訓練狀態」的觀點，呼應 skill-md 撰寫技巧——描述品質直接決定模型選對工具
- [ ] SkillOpt-Sleep 的「夜間固化已驗證 skill」概念，值得關注作為 skill 維護的自動化方向

## 相關連結

- [[agent-skill-design-patterns]] — Agent Skill 設計模式
- [[skill-md-writing-techniques]] — SKILL.md 撰寫技巧
- [[agent-skills-toolkit]] — Agent Skills 工具箱
- [[agent-skills-vs-mcp]] — Skills vs MCP
- [[reflection-memory-learning-loop]] — 反思/記憶學習迴圈（自我進化呼應）
