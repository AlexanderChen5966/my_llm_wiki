---
title: AI Marketing Suite：Claude Code 的職能型 Skill 套件架構
date: 2026-08-25
tags: [ai/claude, ai/agent, type/how-to]
source: https://tools.wingzero.tw/articles/4032
type: note
status: processed
asset_value: 24
---

## 核心論點

AI Marketing Suite for Claude Code（zubair-trabzada/ai-marketing-claude，MIT）把「行銷」這個職能整套包成 Skill 套件，用單一 `/market` 命名空間串起 15 個指令。它的參考價值不在行銷本身，而在「Orchestrator + Subagent + Python Script」這個可複製的 Skill 架構樣本。

## 重點整理

- 架構：1 個 `/market` Orchestrator + 14 個子 Skill；安裝程式自述為 15 Skills、5 Agents、4 Scripts
- 執行鏈路：`/market 指令 → Claude 讀取 Skill → Subagents 分工 → Python Scripts 執行 → 彙整 → Markdown / PDF`
- 5 個 Subagent 各守一個維度：Content & Messaging、CRO & Funnel、Competitive Positioning、Technical SEO & Tracking、Brand/Pricing/Growth，完整 audit 時平行處理
- 4 支 Python Script 承擔可程式化的部分：`analyze_page.py`、`competitor_scanner.py`、`social_calendar.py`、`generate_pdf_report.py`（PDF 需另裝 ReportLab）
- audit 固定加權評分：Content & Messaging 25%、CRO 20%、SEO 20%、Competitive Positioning 15%、Brand & Trust 10%、Growth & Strategy 10%，加權平均得 0–100 分
- 內建模板：Welcome 5 封、Lead Nurture 6 封、Product Launch 8 封 Email，另有 30 天內容行事曆、Client Proposal 與 Launch Checklist
- 指令範圍：`audit` / `quick` / `copy` / `emails` / `social` / `ads` / `funnel` / `competitors` / `landing` / `launch` / `proposal` / `report` / `report-pdf` / `seo` / `brand`
- 作者誠實邊界：這些分數是本 Skill 自訂的分析框架，不是 Google／Meta 官方指標，適合找問題與排優先順序，不宜當絕對績效
- 安裝：Bash 一鍵腳本或 clone 後 `./install.sh`，Skill 與 Agent 進入 Claude Code 的 Skills／Agents 環境

## 可用之處

寫自己的 Skill 時可直接借鑑三層分工：Orchestrator 只做路由、Subagent 分維度平行分析、確定性工作（抓網頁、產 PDF、排行事曆）交給 Script 而非讓模型臨場生成。固定加權評分表也是把主觀判斷變成可跨案例比較的實用手法，可套用到本知識庫的 asset_value 評分或 wiki 健檢。

## 相關連結

- [[ai-competitor-analysis-prompts]]
- [[ai-agent-skills-popular-2026]]
- [[agent-skill-design-patterns]]
- [[claude-code-plugins-guide]]
- [[marketingskills-agent-skills-collection]]
