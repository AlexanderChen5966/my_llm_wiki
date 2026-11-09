---
title: Harness²（Google Research）：權重凍結、讓 agent 自己編輯 8 個 harness 元件的雙層自我演進
date: 2026-11-05
tags: [ai/agent, type/concept]
source: https://medium.com/@simon3458/ai-agent-harness-squared-intro-bdb182241a04（Medium 二手整理，Simon Liu，2026-11-04；一手為 arXiv 2610.14224、google-research/harness2，均未核對）
type: article
status: processed
asset_value: 24
---

> ⚠️ 品質註記：數字與架構細節全部來自 Medium 二手轉述，未對 arXiv 2610.14224 與 GitHub 原文核對（**未驗證**）；數字為 2026-11 快照。作者自介與訂閱推廣段不納入。asset_value 24 為邊界值（頻率判分有主觀）。

## T（核心論點）

**不改模型權重，把 agent 的「裝備」拆成可被編輯的元件，用對照實驗當驗證器，讓 agent 自己調校自己的 harness**——這是 harness 這條軸線上的第三種做法：

| 做法 | 誰改、改什麼 | 代表條目 |
|---|---|---|
| 人設計 | 人工設計 Guides × Sensors | [[harness-engineering]] |
| 訓練期改權重 | 把軌跡蒸餾進權重、讓 harness 變薄 | [[agent-trajectory-distillation-harness-zero-perplexity]] |
| **自動編輯 harness（本篇）** | 權重凍結，agent 編輯 8 個元件並累積經驗庫 | Harness² |

調校面（prompt、記憶、工具說明、skills、hooks 等）因此可被自動化；但這不代表強制規則（權限、金流、刪除等）也該交給 agent 改——與蒸餾條目結論同向，勿過度推論。

## B（適用場景）

**適用：** 想理解「harness 能不能自動演進」的設計思路；評估 CLAUDE.md／skills／subagents／hooks 這類裝備哪些可交給 agent 自動調、哪些必須留在程式層。

**不適用 / 注意：**
- 個人日常使用不太會自建此框架；本篇價值在概念對照，不在落地
- 論文數據未見表格原文，不可當成定論引用

## R（架構與數字）

### 8 個可編輯元件（C1–C8）

system prompt、guardrails、memory、tool guidance、subagents、skills、scripts、plugins／hooks。與 Claude Code 的 CLAUDE.md／skills／subagents／hooks 結構同構，所以可直接類比「哪些元件能自動改」。

### 兩層遞迴

- **Level 1 任務層（Propose-Probe-Compose）**：Reference 先跑初始 harness H0 → 並行產生通用（INV）與領域（DOM）兩類候選 → Composer 對比三組軌跡合成新版 H′；**對照實驗充當驗證器**。可用預算 k 決定走 parallel 或 sequential。
- **Level 2 跨任務層**：Reflector 把 Cue／Lesson／Evidence 寫入 `EXPERIENCE.md`；Executor 每次重置回 H0，Improver 帶著經驗庫再改。

### 數字（Medium 二手轉述，2026-11 快照，未對 arXiv 核對）

| 項目 | k=1 結果 |
|---|---|
| LAB | +11.8 個百分點 |
| JobBench | +4.5 分 |
| WorkBuddy-Bench | 最高 +11.5 分 |

另：WorkBuddy（Gemini 3.5 Flash＋OpenCode）平均 77.5（較 Base +5.1），k=5 為 80.8（+8.4）；Office 領域 74.0→83.4。ReasoningBank／Meta-Harness 的比較亦為轉述。作者自述「自己測試」，但文中只見 demo 輸出。

## C（關鍵行動）

- [ ] 把它當「harness 調校面可自動化」的概念對照，不要因這組數字就導入框架
- [ ] 引用任何數字前先對照 arXiv 2610.14224／GitHub 原文
- [ ] 想借用思路：先挑風險低的元件（tool guidance、skills）試「對照實驗＋保留有效改動」，權限與不可逆動作仍留在 deterministic enforcement

## 原始摘要

Google Research 與 Northwestern 開源 Harness²：權重凍結，把 agent 裝備拆成 C1–C8，任務層用 Propose-Probe-Compose、跨任務層用 EXPERIENCE.md 累積經驗。文章附 uv 安裝、免金鑰 demo（`harness2-demo --k 2`）與 LAB 評測 CLI 流程。

## 相關連結

- [[harness-engineering]] — 對照：人設計 harness（Guides × Sensors）vs 本篇由 agent 自動編輯 harness
- [[agent-trajectory-distillation-harness-zero-perplexity]] — 對照：訓練期改權重 vs 本篇權重凍結
- [[fara-computer-use-agent]] — 同屬 computer-use／agent 能力提升，但走合成資料訓練小模型（改權重）一側
- [[agent-skills-toolkit]] — C6 skills 元件的實務對應
- [[claude-code-glossary]] — CLAUDE.md／subagents／hooks 術語對照
- 平行案例（不另建條目）：**Agent S4**（Simular，2026-10 快照）固定 Qwen3.8-27B、只演化「介面工具庫」，OSWorld V2 完整成功率 5.56%→21.30%、每題 1.10 美元（廠商自家數字，未獨立驗證；1.10 美元不含開發成本）。並陳不裁決：同組數字中「部分得分與完整成功率方向相反」——有試跑 VM 版部分得分 51.06% 但完整成功率 19.44%（低於 21.30%）；Claude Opus 4.8 部分得分 54.80、完整成功率 20.60%。跨模型與配置不可直接比較。
