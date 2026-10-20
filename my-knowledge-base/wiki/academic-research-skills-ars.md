---
title: Academic Research Skills (ARS) — 學術研究 Claude Code 技能包
date: 2026-06-26
tags: [ai/claude, ai/agent, pkm/workflow]
source: https://github.com/Imbad0202/academic-research-skills
source_format: md
type: reference
status: processed
asset_value: 60
---

## T（核心論點）

ARS 是一套涵蓋「研究 → 撰寫 → 審查 → 修訂 → 定稿」全流程的學術研究 Claude Code 技能包，核心立場是**人機協作（AI 當副駕駛、不當機長）**：AI 不代寫論文，只處理苦工（搜文獻、排格式、驗數據、查邏輯一致性），把判斷留給研究者；其設計動機是用「人類 + AI」的組合去避開全自動 AI 研究系統的結構性失敗模式（引用幻覺、框架鎖定、諂媚讓步）。

## B（適用場景）

**適用：**
- 學術論文撰寫全流程（IMRaD / 文獻回顧 / 理論分析 / 個案研究 / 政策簡報 / 研討會論文）
- 需要對抗 AI 生成內容的誠信問題（虛構引用、統計錯誤、宣稱與來源不符）
- 作為 `notebooklm-v1.3-kit` 學術知識庫的上游／互補工具——ARS 負責「產出論文」，notebooklm kit 負責「素材整理 + 語意檢索」
- Claude Code CLI / VS Code / JetBrains（v3.7.0+，plugin 安裝）；另有 Codex CLI 姊妹版

**不適用 / 注意：**
- 不會替你定義問題、選方法、詮釋數據——這些仍需研究者腦力
- 非商業授權（CC-BY-NC 4.0）
- 跑實驗需另搭 `experiment-agent`（ARS 從不執行實驗，只登錄與對齊實驗來源憑證）
- claim-level 引用稽核（`ARS_CLAIM_AUDIT=1`）與跨模型驗證（`ARS_CROSS_MODEL`）為 opt-in，預設關閉

## R（主要內容）

**四個核心 skill（27 種模式、4 個 skill）：**

1. **Deep Research（v2.11.0）**：13 Agent 研究團隊；8 模式含 full / quick / systematic-review（PRISMA）/ socratic（蘇格拉底引導）/ fact-check / lit-review / three-way-scan。
2. **Academic Paper（v3.2.0）**：12 Agent 撰寫團隊；11 模式含風格校準、寫作品質檢查、LaTeX/APA 7.0 輸出、修訂教練、引用格式轉換、rebuttal-audit。
3. **Academic Paper Reviewer（v1.10.0）**：7 Agent 多視角同儕審查；0-100 品質量表（主編 + 3 動態審查者 + 魔鬼代言人）；決策對照 ≥80 接受 / 65-79 小修 / 50-64 大修 / <50 退稿。
4. **Academic Pipeline（v3.13.0）**：10 階段全流程調度器；含 Stage 2.5 / 4.5 兩道**不可跳過的誠信閘門**、checkpoint 確認、R&R 追溯矩陣、協作品質評估。

**對抗 AI 結構性限制的三項設計：**
- 魔鬼代言人**讓步門檻**（反駁須評分 ≥4 才允許讓步，不允許連續讓步）→ 抗諂媚
- 蘇格拉底**意圖偵測**（分辨探索型 vs 目標型使用者，探索型停用自動收束）
- **對話健康度指標**（每 5 輪靜默自檢，偵測持續同意 / 過早收束）

**引用誠信機制（v3.7–v3.8 主線）：**
- 三層引用 anchor（quote / page / section）+ 確定性引用存在性 gate（比對 Semantic Scholar / OpenAlex / Crossref / arXiv 四索引）
- claim-level 稽核：抓回 anchor 指向的原文，判斷論文 claim 是否真被引用支撐（五類 HIGH-WARN 攔截：claim-not-supported / fabricated-reference 等）
- 設計動機來自 Zhao 等人（2026）盤點 2.5M 論文、估計 2025 年單年 146,932 筆幻覺引用的 corpus-scale 發現

## C（關鍵行動）

- [ ] 評估能否將 ARS 的**誠信閘門設計**（Stage 2.5/4.5 阻斷式檢查、claim-faithfulness 稽核）思路，借鏡進 `notebooklm-v1.3-kit` 學術版的 wiki-health-check
- [ ] 比較 ARS 的「人機協作 = 副駕駛」立場與 [[human-ai-collaboration-three-layers]]、[[karpathy-autoresearch]] 的差異
- [ ] 若有實際論文需求，測試 `/ars-plan` 蘇格拉底規劃流程

## 原始摘要

GitHub repo（Imbad0202/academic-research-skills，作者吳政宜 Cheng-I Wu）的 README.zh-TW。以 Lu 等人（2026, *Nature*）的全自動 AI Scientist 與其失敗模式為對照，論證人機協作的必要性。完整 changelog 顯示密集的版本迭代（v3.7→v3.13），重心在引用誠信與跨模型驗證基建。一篇 ~15k 字論文 pipeline 估算約 $4–6。

## 相關連結
- [[paper-radar-literature-monitor]] — 論文**輸入端**的自動化（每日抓取＋興趣回饋迴圈排序），與本篇的 research→write→review 流程互補：一個管「讀什麼」、一個管「怎麼寫」
- [[notebooklm-skill-research-pipeline]] — NotebookLM 研究 pipeline（學術素材檢索面）
- [[karpathy-autoresearch]] — 自動研究方法論對照
- [[agentic-ai-researcher-roadmap]] — 研究者 AI 能力路線圖
- [[human-ai-collaboration-three-layers]] — 人機協作三層架構
- [[ai-code-review-safeguards]] — AI 審查防護機制（誠信閘門概念相通）
- [[librechat-platform]] — 同批整理的開源 AI 平台
- [[anthropic-official-17-skills]] — Claude 官方 Skills 對照
- [[grad-thesis-claude-code-toolbox-series]] — 立場相同（副駕駛不當機長）的從零自建版：非工程師研究生用 Claude Code 自建論文工具箱（2026-09-23 新增）
