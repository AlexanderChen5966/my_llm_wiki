---
title: Agentic AI 研究者學習路線（awesome-agentic-ai-zh）
date: 2026-06-25
tags: [ai/agent, pkm/workflow]
source: https://github.com/WenyuChiou/awesome-agentic-ai-zh/blob/main/branches/for-researcher.md
type: reference
status: processed
---

## T（核心論點）

研究者用 AI 的核心模式：Claude 負責規劃與審查、Codex 跑程式批次工作、Gemini 處理長稿與跨 paper 合成——三模型分工比單一工具效益高，關鍵是把每個任務分配給最適合的模型。

## B（背景脈絡）

- 來源：WenyuChiou/awesome-agentic-ai-zh 三語學習地圖（繁中/English/简中），240+ 精選資源
- 接續主路線 Track A 的 A3 或 Track B 的 Stage 7 後執行
- 維護者：[@WenyuChiou](https://github.com/WenyuChiou)（Lehigh CEE PhD candidate）

## R（重要細節）

### 研究階段 × AI 工具分工表

| 階段 | 推薦工具 |
|------|---------|
| 文獻探索 | NotebookLM → paper-qa → gpt-researcher |
| 文獻精讀 | Zotero + zotero-gpt → zotero-skills |
| 研究設計 | Claude.ai 對話 → ai-research-skills |
| 寫程式（批次） | codex-delegate |
| 寫長稿（中英文） | gemini-delegate（1M context） |
| 投稿前 audit | academic-writing-skills |

### 三模型分工原則

| 任務類型 | 用哪個 | 為什麼 |
|---------|--------|--------|
| 研究設計/假設討論 | Claude.ai | 對話協作、context memory |
| 50+ script 批次編輯 | codex-delegate | 機械式編輯快、省 Claude token |
| 長稿 draft（8 頁 paper） | gemini-delegate | 1M context、長 prose 強項 |
| Second opinion | gemini-delegate | LLM-vs-LLM 對照、揪出 Claude 偏誤 |
| 投稿前 audit | academic-writing-skills | 結構化 checklist、不靠即興判斷 |

### 必練流程（按使用頻率）

- **每天**：文獻 inbox 分流（paper-qa + zotero-gpt）
- **每天**：寫作 sprint 25 min（Claude.ai + banned-word audit）
- **每週**：5-10 篇 PDF 餵 Gemini 做跨 paper synthesis
- **每週**：Zotero 整理（重 tag + 歸檔）
- **Per paper**：投稿前 final audit（banned-word + figure-text + submission checklist）
- **Per paper**：Multi-agent peer review（Claude/Codex/Gemini 三角驗證）

### 精選工具

| 工具 | 用途 | Stars |
|------|------|-------|
| [paper-qa](https://github.com/Future-House/paper-qa) | Citation-grounded Q&A，句子級引用 | 8k+ |
| [gpt-researcher](https://github.com/assafelovic/gpt-researcher) | 自主 deep-research agent | 27k+ |
| [stanford-oval/storm](https://github.com/stanford-oval/storm) | Multi-perspective outline-then-write | 28k+ |
| [AI-Scientist-v2](https://github.com/SakanaAI/AI-Scientist-v2) | 端到端 multi-agent 科學研究 loop | 6k+ |
| [open_deep_research](https://github.com/langchain-ai/open_deep_research) | 開源版 Deep Research，parallel 搜尋+引用 | 11k+ |

### 維護者自用的 6 個研究 skill（@WenyuChiou）

> 揭露：維護者（Lehigh CEE PhD candidate）日常自用、未經第三方獨立評測，適合 PhD 寫作 / 跨 paper 文獻整理。

| 工具 | 適合階段 | 一句話 |
|------|---------|--------|
| [ai-research-skills](https://github.com/WenyuChiou/ai-research-skills) | 全流程 | 14 個研究 skill 打包成 5-plugin marketplace，一指令裝整套 |
| [research-hub](https://github.com/WenyuChiou/research-hub) | 文獻整理 | Zotero + Obsidian + NotebookLM 三工具整合，CLI/MCP/REST/dashboard 四介面 |
| [zotero-skills](https://github.com/WenyuChiou/zotero-skills) | 文獻管理 | Zotero CLI skill（搜/加/分類/標記），與 zotero-gpt 互補 |
| [academic-writing-skills](https://github.com/WenyuChiou/academic-writing-skills) | 投稿前 | banned-word audit、figure-text coupling、submission checklist |
| [codex-delegate](https://github.com/WenyuChiou/codex-delegate) | 寫程式 | Claude planner + Codex executor，batch refactor / migration |
| [gemini-delegate-skill](https://github.com/WenyuChiou/gemini-delegate-skill) | 長稿/synthesis | Claude planner + Gemini 寫 1M context 長文 / CJK / second-opinion |

> 中文研究者另可參考 [ChatPaper](https://github.com/kaixindelele/ChatPaper)（arXiv 總結+翻譯+潤色+審稿回覆，19k+ stars，非商用自訂條款）。

### 層級建議

| Tier | 工具 | 適合誰 |
|------|------|--------|
| Tier 0 | Claude.ai + NotebookLM | 非程式背景、人文社科、臨床 |
| Tier 1 | Claude Desktop + Zotero/Obsidian MCP | 有 Zotero/Obsidian 習慣者 |
| Tier 2 | Claude Code + ai-research-skills | 計算型研究者 |
| Tier 3 | Claude Code + codex + gemini + research-hub | 多工具整合 pipeline |

## C（行動清單）

- [ ] 設定「每天文獻分流 + 25 min 寫作 sprint」為基礎習慣（一個月後再升級）
- [ ] 評估自身屬於 Tier 幾，從最低需要的層級開始
- [ ] 跨 paper synthesis：試用 gemini-delegate 做 1M context 整合

## 相關條目

- [[agentic-ai-learning-roadmap]] — agentic AI 通用學習地圖
- [[agent-skills-vs-mcp]] — Skills vs MCP 的選擇
- [[claude-code-advanced-workflow]] — Claude Code 進階技巧
- [[synthesis/synthesis-agentic-engineering]] — Agentic engineering 精粹

## 補充資料

**2026-11-19 補充：來源頁面改版（2026-09-03 剪存版；https://github.com/WenyuChiou/awesome-agentic-ai-zh/blob/main/branches/for-researcher.md）**

新版整頁重寫，**定位從「三模型分工提升產出」改為「查證優先」**：「不是要讓 AI 替你當研究者」，而是找到資料、看懂資料，再確認答案真的有資料支持。上方 T 的三模型分工、工具分工表、必練流程、6 個維護者 skill、Tier 0–3 表在新版**全部移除**（保留原文作為 2026-06 版紀錄，不代表現行建議）。

| 項目 | 舊版（2026-06） | 新版（2026-09） |
|---|---|---|
| 入口 | 先完成 A3／Stage 7 | 不寫程式者可直接做第一個練習（瀏覽器＋一篇公開 paper） |
| 起手工具 | 階段 × 工具表、Tier 0–3 | 三選一：Gemini Notebook（NotebookLM 於 2026-07-16 改名）／Zotero／PaperQA2 |
| 工具表 | Stars 數 | 編輯推薦度＋授權／限制欄（2026-08-29 查核）；新增 asreview、DVC、MLflow、Zenodo、repo2docker；open_deep_research 已封存，降為「歷史參考」⭐⭐⭐；flonat/claude-research 改列 flonat/flonat-research |
| 跨 paper synthesis | 5–10 篇 PDF 餵 Gemini 問 disagree | 先問每篇各說什麼，再比較同意／衝突／條件差異；不要先寫完整故事再找引用 |
| 必修閱讀 | Effortless Academic、Sant'Anna 指南 | 官方文件：Gemini Notebook citation 與隱私、Zotero、PaperQA2、DVC、Zenodo |

- **新增第一個練習**：用《Attention Is All You Need》問三題，每答附 citation、逐條點開核對原文，找不到證據標「unsupported／未支持」，不得補不相干引用。
- **新增八個核心詞**：Source、Claim、Citation、Source Verification、Literature RAG、Reproducibility、Private Data、Human Review（citation 只是路標，不保證支持主張）。
- **新增資料保護要求**：上傳前確認授權／著作權與工具條款（公開可讀≠可交給另一個服務）；私人資料先確認 IRB、機構政策、合約與資料擁有者同意；MLflow 不寫密鑰或受試者資料；Zenodo metadata 會公開；AI-Scientist-v2 授權要求揭露機器產生的稿件。
- **可重跑紀錄**：保存資料版本、environment、seed、prompt、模型／工具版本、輸出與人工修改；結語「沒有任何工具可以代替 IRB、資料治理、作者責任或領域專家的判斷」。
- 另見新版摘要筆記 [[ideas/awesome-agentic-ai-zh-for-researcher]]。

## 相關連結

- [[paper-radar-literature-monitor]] — 開源自架論文雷達：RSS 自動抓取＋按讚/略過回頭微調排序權重，任何有 RSS 的領域可 fork