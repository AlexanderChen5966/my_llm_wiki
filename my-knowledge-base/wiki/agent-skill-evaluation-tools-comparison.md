---
title: 評估 Agent Skill 品質的三種工具實測：skill-creator vs darwin-skill vs SkillOpt
date: 2026-07-09
tags: [ai/claude, type/comparison]
source: https://codelove.tw/@tony/post/3jAvJx（原文：https://qiita.com/Syoitu/items/78d45bee1160d059c972）
type: article
status: processed
asset_value: 70
---

用同一個刻意寫得很隨便的 `commit-message-writer` Skill，實測比較三種評估/改善 Agent Skill 的工具：Anthropic 官方 skill-creator（有/無技能實測輸出對比）、社群 darwin-skill（9 維度文件品質評分+自動改善迴圈）、Microsoft SkillOpt（把 SKILL.md 當神經網路權重做梯度式最佳化）。核心發現：**模糊的 Skill 不只沒幫助，甚至會比 baseline（無 Skill）更差**——三個工具用不同評分軸都得出同一結論。

## T（核心論點）

「寫完 SKILL.md 就算了」的常見心態是危險的：本文用實測證明「較弱的初版 Skill」pass 率（92%）**低於完全不用 Skill 的 baseline（100%）**——加了流程反而拖累表現。三個評估工具的共同精神是「不是看你自己覺得好不好，而是看實際輸出來衡量」，把 Skill 品質從主觀感覺變成可量化、可迭代改善的數字。

## B（適用場景）

已經寫了 Skill 但不確定是否真的有效、想量化改善；準備大規模部署 Skill 前的品質把關；想理解 Skill 觸發率（description 精準度）與輸出品質（內文流程）是兩個獨立需要優化的維度。

## R（三工具實測對比）

**Skill 的逐層讀取機制（評估的前提知識）**：`SKILL.md` 由 YAML frontmatter（`name`+`description`，決定觸發精準度，**一直在 context 裡**）與內文（步驟/工作流程，**只在被觸發時**讀取）構成，外圍可掛 `scripts/`/`references/`/`assets/`（**只在需要時**讀取）——這個三階段機制讓大型 Skill 也能省 context。

### 1. skill-creator（Anthropic 官方，`/skill-creator` 或 `npx skills add anthropics/skills --skill skill-creator`）

- **有/無技能對比**：在 `evals.json` 定義 prompt/expected_output/assertions，同一任務跑「有技能」與「無技能」兩種情況多次，評估 sub-agent 從 transcript 引用依據判定 pass/fail，結果以 `mean ± stddev` 呈現——**stddev 大代表結果不穩定（flaky），效果可能很依賴模型**
- **同時評估觸發率**：準備約 20 個真實 prompt（混合 should-trigger 與不該觸發案例），切 train/test，失敗案例交給 Claude 重寫 description 再評估，用 test 端分數選最佳版本
- **實測數據**：較弱初版 pass 率 92%±14% vs 無技能 baseline 100%±0%（**技能反而更差**）；改善 description（移除模糊詞+加具體觸發詞）與內文（明確輸出格式/編號步驟/反例列表）後回升到 100%±0%，但作者坦言差距只有 ±8pt——因為底層模型（Opus）本來就強，**Skill 真正的價值在「強制一致性」而非提升上限**（如 scope 判定、命令式語氣、寫出修復原因，這些不能靠運氣，要每次穩定做到）

### 2. darwin-skill（社群 meta-skill，`npx skills add alchaincyf/darwin-skill`）

- **9 維度規則評分**（滿分 100，各維度有權重）：可執行的具體性（權重最高之一）、實測效能（做法同 skill-creator）、失敗模式明示、檢查點設計、高風險行為反例清單等，重點在檢視「作為文件的寫法好不好」
- **爬山式改善迴圈**：每次只選分數最低的 1 個維度改善→套用改動並 git commit→交給**獨立子 agent**重新評分→分數上升則保留、下降則 git revert→改善幅度變小時自動停止
- **兩個設計亮點**：① 評分者與編輯者分開（自己改自己評容易偏寬鬆）② 用 git 保留歷史，只有真正變好的修改才累積成 commit
- **實測數據**：同樣的雜亂 Skill 基線評分僅 **19.2/100**（幾乎所有維度都在低標），跑 3 輪迭代（改具體性→改 frontmatter→加失敗分支與 CHECKPOINT）後升到 77.5/100；**獨立於 skill-creator 的評分軸，得出同樣結論**：模糊技能輸給 baseline
- **限制與誠實揭露**：有「改善後不得超過原本 150%」的大小限制，極小技能容易一輪就超標，需人工判斷是否適用；分數本質是 LLM judge，每次跑會浮動（SkillLens 實證 LLM 自評準確率僅 46.4%），**應該用來抓弱項與做「只在真的變好時才保留」的棘輪機制，而非當絕對值信賴**

### 3. Microsoft SkillOpt（研究級工具，`github.com/microsoft/SkillOpt`）

- **概念**：把 SKILL.md 當神經網路權重做最佳化，模型本身固定不動，只優化 Markdown 內容，術語直接借用深度學習（epoch/batch size/learning rate/validation gate）
- **6 階段最佳化迴圈**：Rollout（跑任務拿分數，＝前向傳播）→ Reflect（分析成敗產出編輯建議）→ Aggregate（整合多個編輯建議）→ Select（依重要度篩選，＝learning rate 概念）→ Update（套用編輯）→ **Gate**（用獨立驗證資料評估，只有真變好才採納——這是抗過擬合的關鍵設計）
- 需要 Python 環境 + 評估資料集 + LLM API 金鑰，附 Gradio WebUI，**不是即開即用工具**，是「已有基準資料集、要正式跑最佳化」時的選項

## C（關鍵行動）

- [ ] 任何 Skill 部署前，先用 skill-creator 做「有/無技能」實測對比，確認它真的有正向效果，而非想當然爾
- [ ] 確認有效後，用 darwin-skill 補齊模糊表達/失敗分支/檢查點等「寫法品質」細節
- [ ] 只有已具備評估資料集且想大規模系統化最佳化時，才考慮導入 SkillOpt——避免用重型研究工具處理輕量需求
- [ ] 不要完全信賴評分工具的絕對數值（LLM judge 本身有噪聲），關注「是否真的比 baseline 好」與「趨勢是否上升」

## 相關連結

- [[skillopt-skill-optimization]] — SkillOpt 完整架構深入版（52 cell 全 best/tied-best 實測數據）
- [[skill-md-writing-techniques]] — SKILL.md 撰寫技巧（description 模板/A+B eval 迭代法），與本文的評估流程互補
- [[claude-code-official-skills-top10-2026]] — 官方 Skill 生態速查，skill-creator 是其中內建工具之一
- [[claude-code-skill-survivors-20-picks]] — 「純 prompt 包裝」型 Skill 的淘汰訊號，與本文「模糊 Skill 輸給 baseline」的實測互相佐證
- [[synthesis-practitioner-skills-design]] — 本篇為其來源之一；本篇的量化數據補上該精粹原本「Skills 效益缺乏客觀評估」的知識缺口
- [[llm-judge-confidence-cascade]] — 量化「LLM judge 有噪聲」在哪些任務最大：推理、程式、風格欺騙、無參考答案的文章（CMU 研究，原論文待查）
- [[ai-architecture-diagram-tool-vs-instruction-comparison]] — 評估 Skill／MCP／外掛時排除環境干擾的隔離旗標：`--setting-sources project --strict-mcp-config`（避開 `--safe-mode`，它會連待測對象一起停用）（2026-09-29 新增）
- [[anthropic-claude-ai-perf-sprint-metric-first-loop]] — 棘輪（門檻只降不升）在生產環境效能最佳化的應用，與本篇 Skill 評分的棘輪是同一機制的另一個領域（2026-09-29 新增）
