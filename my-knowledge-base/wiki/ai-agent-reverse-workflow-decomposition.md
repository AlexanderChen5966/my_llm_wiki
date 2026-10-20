---
title: 建 AI Agent 先「反向拆解工作流」：非工程師的微型組織設計法
date: 2026-07-17
tags: [ai/agent, ai/workflow, pkm/workflow]
source: https://www.bnext.com.tw/article/91536/nuu6s466mbr9vtkjbvmm
type: article
status: processed
asset_value: 63
---

《數位時代》訪凱鈿資深 HR 經理 Wesley（做過一套 HR Agent）談非工程師如何搭 AI Agent。核心觀點：AI Agent 的本質是「微型組織設計」——先搞清楚工具（Tools）和技能（Skill）的差異，再用「反向拆解工作流」的邏輯，從終點往回推每一步的具體成果與所需輸入。

## T（核心論點）
建 Agent 別從「我想自動化什麼」正向想，而要**從最終輸出往回推**：先定好你要的最終成果，再往回推每一步需要哪些輸入資料，每步的 Output 就是下一步的 Input，一路往回拆，工作流自然浮現。出錯時不能只改結果，要「管理過程」——抓出流程斷點加檢核指令，才能越做越好。

## B（適用場景）
- **適用**：非工程師想搭自己的 AI Agent、把重複性工作流程化、判斷某任務值不值得 Skill 化
- **不適用**：一次性/低頻任務（ROI 不划算，見下方公式）；工作流本身會頻繁大改的場景（Skill 維護成本高於省下時間）

## R（Wesley 的思考框架）
**1. 先分清工具與技能：**
- **Tools（工具）**：讓 AI 能做事，例如連 Gmail 發信、Google 日曆查空檔
- **Skill（技能）**：教 AI 執行任務的步驟與方法，例如從留言整理洞察、撰寫社群貼文
- 一個 Agent 可擁有多個工具和技能，接力完成大任務

**2. 反向拆解工作流（從終點往回推）：**
以招募流程為例：
```
最終要「安排好的面試行程」
  ← 上一步輸出是「合格候選人名單」
    ← 再上一步是「履歷分析結果」
```
每步 Output = 下一步 Input，一路往回拆。

**3. 出錯要管理過程，不只改結果：**
安排面試出錯時，不能只要求 AI 把成果改對，必須要求它檢視整個流程哪裡漏掉——是候選人回覆日期格式錯，還是 AI「比對空檔」偷懶沒做？抓出斷點，在流程中加檢核指令。

**4. Skill 化前先算 ROI：**
> 這任務一個月做幾次 × 每次省幾分鐘 = 總省時。若梳理與維護 Skill 要花超過省下的時間，就別做。

避免「學到 Agent 概念就想把所有細碎工作都 skill 化，工作流一改又要全部重做」的陷阱。

## C（關鍵行動）
- [ ] 建 Agent 前先寫下「最終輸出」，再往回推每一步的 Output/Input
- [ ] Agent 出錯時追流程斷點而非只改結果，在斷點加檢核指令
- [ ] Skill 化前套用 ROI 公式（月頻率 × 每次省時 vs 維護成本）

## 相關連結
- [[ai-usage-five-levels]] — AI 使用五層級，第 5 層 Agent 自動化的升級前提（重複+可驗收+有價值）與本篇 ROI 判斷呼應
- [[sop-to-skill-4-steps]] — 把 SOP 變 AI 技能的 4 步，與本篇拆解法互補
- [[enterprise-ai-adoption-skill-library-buttons]] — 同一位 Wesley 的前一篇：別教員工寫 Prompt 做成按鈕給他按
- [[ai-agent-dev-concepts-practitioner]] — 工程師視角：先定義任務邊界、能用 Workflow 就別上 Agent
- [[agent-os-workstation-division]] — 用工位分工隔離上下文/責任，核查者不能是提煉者自己
