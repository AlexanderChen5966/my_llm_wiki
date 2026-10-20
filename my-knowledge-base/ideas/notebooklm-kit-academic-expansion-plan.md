---
title: notebooklm-v1.3-kit 學術功能優化擴充規劃
date: 2026-06-26
tags: [ai/agent, ai/claude, pkm/workflow, project/notebooklm-kit]
source: 內部分析（整合 academic-research-skills-ars / agentic-ai-researcher-roadmap / notebooklm-skill-research-pipeline）
type: idea
status: inbox
---

## 一句話

把 ARS（學術研究 Claude Code 技能包）與「研究者 AI 路線」裡**真正適用於個人單人學術知識庫**的優點，挑進 `notebooklm-v1.3-kit`——重點補三個缺口：**引用誠信驗證、對抗式審查、文獻分流節奏**，而不是把 ARS 的多 Agent 重武器整套搬進來。

---

## 設計前提（先框住範圍，避免過度工程）

1. **單人使用**：kit 是個人學術知識庫，不引入多人協作 / 多 Agent 評審團那類重型機制（沿用 [[feedback_framework_design_decisions]] 的單人簡化原則）。ARS 的「主編 + 3 審查者 + 魔鬼代言人」要降級成**單次輕量 pass**。
2. **kit 定位是「整理 + 檢索 + 昇華」，不是「寫論文」**：ARS 的 Academic Paper / Pipeline（撰稿、投稿、rebuttal）超出 kit 範圍，**不移植**。kit 補的是「知識資產品質」這一端。
3. **不強迫對齊個人慣例**：維持 kit 既有的 `paper-summaries/` `topic-synthesis/` `concepts/` 結構。
4. **這份是規劃草案**，未動 kit 任何檔案；待你挑選優先項後再實作。

---

## 缺口分析：kit 現況 vs 學術內容的優勢

| 學術優勢（來源） | kit 現況 | 缺口 |
|------|---------|------|
| 引用誠信閘門 / claim-faithfulness 稽核（ARS Stage 2.5/4.5） | knowledge-organizer 要求 `[p.X]` 頁碼，但**從不驗證引用是否真實、claim 是否真被該頁支撐** | ⭐ 最大缺口 |
| 對抗式審查（ARS 魔鬼代言人 + 讓步門檻 + 抗諂媚） | synthesizer / elevation 產出**自信主張，無懷疑者 pass** | ⭐ 高價值 |
| 跨 paper 矛盾盤點（ARS「這幾篇 disagree 在哪」） | CAVE 的 V（Voids）有觸及，但偏「缺口」非「衝突」；wiki-health-check 有矛盾偵測但在 wiki 層 | 中 |
| 文獻分流節奏（researcher-roadmap 每天/每週/per-paper） | daily-brief 只報積壓數，無「論文分流」視角與節奏框架 | 中 |
| 證據信心分級（ARS trust-chain；roadmap citation-grounded） | elevation 的 E 層已有「信心等級」，但**未下放到每篇 R 層發現** | 中低 |
| 工具整合指引（paper-qa / Zotero / gemini-delegate 1M 跨稿） | AGENTS.md 無外部工具整合建議 | 低（選配） |

---

## 提案（依「價值 ÷ 工程量」排序）

### P1 — 引用誠信檢查（新 skill 或併入 wiki-health-check）⭐⭐⭐⭐⭐

**動機**：ARS 的核心貢獻，也是 kit 最大盲點。Zhao 等人（2026）估計 2025 年單年 146,932 筆幻覺引用——AI 整理學術素材時最容易出的就是「頁碼掛錯、claim 與來源不符」。

**做什麼（單人輕量版）**：
- 對 `paper-summaries/` 條目，逐條檢查 R 層發現的 `[p.X]` 標記：
  - **存在性**：頁碼是否落在該 PDF 實際頁數範圍內（對照 raw/papers/ 原檔頁數）
  - **claim-faithfulness（半自動）**：把該頁原文 + wiki 的 claim 並列，請 AI 判定 `ALIGNED / OVERSTATED / NOT_SUPPORTED`，輸出待人工確認清單
- 不做 ARS 的四索引線上查驗（S2/OpenAlex/Crossref/arXiv）——個人庫多為 PDF 在地素材，改用「對照原檔頁碼」即可，**advisory 不阻斷**。

**落點**：建議獨立 skill `citation-integrity-check`（職責單一），或作為 wiki-health-check 第 7 項。
**參考**：[[academic-research-skills-ars]]（Stage 2.5/4.5、三層引用 anchor）

---

### P2 — synthesis/concept 的對抗式審查模式 ⭐⭐⭐⭐

**動機**：ARS 發現 AI 有「框架鎖定 + 諂媚讓步」傾向；kit 的精粹/概念條目是「自信整合」，缺一道唱反調。

**做什麼（降級為單次 pass，非評審團）**：
- 對 knowledge-synthesizer / knowledge-elevation 新增 opt-in 的 `--devil` 模式：
  - 產出精粹後，AI 切換角色對自己的 C（共識）與 E（洞察）提出**最強反駁**
  - 借用 ARS 的**讓步門檻**：反駁須自評 1-5，≥4 才在條目裡標注「⚠️ 此共識有實質爭議」；<4 則保留原判斷並記錄「已考慮但不採納的反駁」
- 目的是讓「信心等級」有依據，不是要推翻結論。

**落點**：knowledge-synthesizer + knowledge-elevation 各加一個 opt-in 模式。
**參考**：[[academic-research-skills-ars]]（讓步門檻、對話健康度）

---

### P3 — CAVE 的 V 層強化：跨 paper 矛盾盤點 ⭐⭐⭐

**動機**：researcher-roadmap 的高頻流程「5-10 篇 PDF → 問『這幾篇 disagree 在哪』」；ARS 有結構化的跨論文矛盾盤點。kit 的 CAVE「Voids」目前偏知識缺口。

**做什麼**：
- knowledge-synthesizer 的 V 層拆成兩塊：**V1 知識缺口**（現況）+ **V2 來源衝突**（新增）
- V2 明確列出「哪兩篇在哪個論點上對立、各自證據強度、暫以何者為準」，輸出表格
- 與 wiki-health-check 的矛盾偵測互補（一個在合成當下、一個在事後巡檢）

**落點**：knowledge-synthesizer 的 CAVE 框架微調（向後相容）。

---

### P4 — daily-brief 增加「文獻分流」視角與節奏框架 ⭐⭐⭐

**動機**：researcher-roadmap 的核心洞見是「把 AI 變日常工具的關鍵是設好頻率」（每天/每週/per-paper）。kit 的 daily-brief 只報數字。

**做什麼**：
- daily-brief 新增區塊「📚 待分流論文」：raw/papers/ 未處理 PDF，依「快速掃 abstract → 判定值不值得精讀」分級
- 在 brief 末尾的 SCQA 行動建議，套用 roadmap 的節奏框架：**每天**（新進論文分流）/ **每週**（同主題跨 paper synthesis）/ **per-paper**（投稿前/精讀後 citation-integrity-check）

**落點**：daily-brief-generator 輸出格式擴充。
**參考**：[[agentic-ai-researcher-roadmap]]（必練流程表）

---

### P5 — R 層發現的證據信心分級 ⭐⭐

**動機**：ARS 的 trust-chain、roadmap 的 citation-grounded Q&A——學術知識的價值在於「可追溯 + 可信度標示」。kit 的 E 層已有信心等級，但每篇條目的 R 層發現沒有。

**做什麼**：
- knowledge-organizer 的 TBRC 學術版，R 層每條發現可選標注信心標籤：`[實證強]`（有數據+頁碼）/ `[推論]`（作者論述）/ `[待查證]`
- 與 P1 citation-integrity-check 串接：被標 `NOT_SUPPORTED` 的自動降為 `[待查證]`

**落點**：knowledge-organizer TBRC 學術版模板微調。

---

### P6 — AGENTS.md 補「外部工具整合指引」（選配）⭐

**動機**：roadmap 的工具生態（paper-qa 句子級引用 Q&A、Zotero 文獻管理、gemini-delegate 1M 跨稿 synthesis）能補 kit 在「檢索 + 長稿整合」的不足。

**做什麼**：在 AGENTS.md 加一節「選配整合」，列出何時跳出 kit 用外部工具：
- 跨 5+ 篇 PDF 一次讀完整合 → Gemini 1M context（kit 的 synthesizer 受 context 限制時）
- 句子級可追溯文獻 Q&A → paper-qa
- 文獻庫管理 → Zotero + MCP
- **僅文件指引，不綁定實作**。

**參考**：[[agentic-ai-researcher-roadmap]]（維護者自用 6 skill、Tier 0-3）

---

### P7 — 論文完成後知識回灌 kit（ARS → kit）⭐⭐⭐⭐

**動機**：kit 是長期資產引擎，但 ARS 撰稿/審查階段「打過一場仗才學到的東西」預設留在 ARS 工作區、不會回流 wiki——論文一結案就流失。這是資產化在撰稿階段斷掉的洞。

**做什麼（新 skill `paper-knowledge-backflow`）**：
論文定稿後，從 ARS 產出物中萃取三類知識，沉澱回 kit wiki：
1. **方法論決策** → `wiki/concepts/` 或 `research/`：為何選此 method、踩過的坑、trade-off（下次研究可重用）
2. **審稿攻防** → 新類別 `wiki/rebuttal-lessons/`：reviewer 打哪裡、你怎麼補強——變成「論點防禦」資產
3. **撰稿時冒出的跨 paper 新洞察** → 餵回 knowledge-elevation 的 concepts/

**輸入**：ARS 的 review report / response-to-reviewers / final draft。
**輸出**：對應的 wiki 條目（TBRC 或精粹格式），並回填 join key 串回原始 paper-summaries。
**落點**：新 skill，流程末端觸發（半自動，產出待人工確認）。
**參考**：[[academic-research-skills-ars]]（Reviewer / rebuttal-audit 產出）

---

### P8 — 撰稿素材橋接 kit → ARS（export）⭐⭐⭐⭐

**動機**：kit 精煉好的文獻回顧與概念框架，目前要手動複製進 ARS。建一個轉換器把這道接縫自動化。

**做什麼（新 skill `synthesis-to-ars-export`）**：
- 讀取指定主題的 `wiki/topic-synthesis/` + 相關 `paper-summaries/`
- 轉成 ARS 撰稿可吃的兩份素材：
  1. **lit-review seed**：CAVE 的 C/A/V 直接對應「共識/各家觀點/研究缺口」段落骨架
  2. **bibliography 清單**：帶 citation key + DOI + 頁碼，供 ARS 引用查驗對接
- 輸出到一個交換資料夾（如 `outputs/ars-handoff/`）

**落點**：新 skill，撰稿前觸發。
**依賴**：需先有統一 join key（見下方「資料流設計」）。

---

## 明確「不移植」清單（範圍控制）

| 項目 | 為何不移植 |
|------|-----------|
| ARS Academic Paper / Pipeline（撰稿、投稿、rebuttal、disclosure） | 超出 kit「整理+檢索+昇華」定位 |
| 多 Agent 評審團（主編 + 3 審查者 full panel） | 違反單人簡化原則，降級為 P2 單次 pass |
| 四索引線上引用查驗（S2/OpenAlex/Crossref/arXiv） | 個人在地 PDF 庫用頁碼對照即可，線上查驗 ROI 低 |
| PRISMA 系統性回顧、實驗來源登錄、跨模型驗證 | 重型流程，個人知識庫用不到 |
| knowledge-elevation 去學術化（active v1.1.0） | 學術版刻意保留 v1.0.0（已於前次決定）|

---

## 完整研究論文工作流與自動資料流設計

> 目標：把 roadmap 工具（探索）→ kit + NotebookLM（精粹）→ ARS（撰稿）三套**獨立系統**串成一條流水線。三套無共用資料庫，所以「自動」靠的不是程式同步，而是**共用約定（convention）+ 兩個橋接 skill**。

### 讓資料能流動的三個前提

| 前提 | 內容 | 為何關鍵 |
|------|------|---------|
| **單一 PDF 真相源** | 所有 PDF 只放一處（建議 `raw/papers/`，Zotero 用 symlink 或匯出對接） | 三套都指向同一份原檔，頁碼/引用才對得起來 |
| **單一 join key** | 每篇 paper 給一個 citation key（如 `Smith2023`），貫穿三套系統 | 這是讓「探索→精粹→撰稿→回流」能追溯同一篇的脊椎 |
| **全 Markdown + frontmatter** | 三套都是 Claude Code 友善的純文字 | 橋接 skill 只需做格式轉換，不需 API 整合 |

> **join key 規則**：PDF 檔名 = citation key（`Smith2023.pdf`）；wiki 條目檔名 = citation key（`paper-summaries/Smith2023.md`）；frontmatter 帶 `citation_key` + `doi`。ARS 引用、kit 回流都用它對位。

### 端到端流程（階段 × 工具 × 資料產物 × 流向）

```
階段                工具                      資料產物                       流向
─────────────────────────────────────────────────────────────────────────────
0 選題/RQ          你 + ARS socratic         RQ brief                       →
1 文獻探索          roadmap(SemScholar/        候選清單 → 匯入 Zotero          → 生成 citation key
                   gpt-researcher)
2 文獻精讀          roadmap(paper-qa)          claim 摘記；PDF→raw/papers/    → 檔名=citation key
3 結構化           ★kit organizer            paper-summaries/{key}.md        ← PDF
                                             （TBRC + [p.X]）
4 精粹             ★kit synthesizer          topic-synthesis/{topic}.md      ← 多篇 summaries
                                             （CAVE）
5 昇華             ★kit elevation            concepts/（長期資產）            ← 多篇 synthesis
   ─── 品質閘 ───  ★P1 引用誠信 / P2 對抗審查 在 3-5 之間隨時插入 ───
6 撰稿前橋接        ★P8 export                outputs/ars-handoff/            kit ──→ ARS
                                             （lit-review seed + biblio）
7 研究設計/執行/    你 + experiment-agent      實驗數據 / 分析結果             ⚠️ 人工為主（缺口）
  分析             + codex-delegate
8 撰稿             ARS Academic Paper         draft                          ← handoff + 數據
9 審查             ARS Reviewer               review report (0-100)          →
10 修訂/投稿        ARS revision/format/       final paper                    →
                   rebuttal
11 知識回流         ★P7 backflow              方法論決策→concepts/            ARS ──→ kit
                                             審稿攻防→rebuttal-lessons/
                                             新洞察→elevation
                                             （回填 join key 串回 summaries）
─────────────────────────────────────────────────────────────────────────────
★ = kit 範圍   ⚠️ = AI 補不了、需人工
```

### 資料流自動化的三個層次（誠實分級）

1. **靠約定就「自動對齊」**（零程式）：同一 citation key + 同一 PDF 源，三套系統天然指向同一篇——探索標的、精粹引用、撰稿 bibliography、回流串接全部對得上。
2. **半自動橋接 skill**（P8 + P7）：kit→ARS 匯出（撰稿素材）、ARS→kit 回流（知識資產），用 Claude Code skill 做格式轉換，產出待人工確認。
3. **仍然手動的決策點**（也應該手動）：探索時挑哪些 paper、研究怎麼執行、數據怎麼判讀、何時觸發 ARS——這些是學術判斷，不該自動化。

### 一句話總結這條流水線

> **資料的「對齊」用 citation key 做到自動；資料的「搬運」用 P7/P8 兩個橋接 skill 做到半自動；資料的「判斷」永遠留給你。** 個人知識資產化的閉環＝第 11 階段 P7 把撰稿階段的知識回灌 kit，讓每寫一篇論文，concepts/ 就厚一層。

---

## 建議實作順序

**第 0 步（資料流地基，先於一切）**：確立 citation key 約定 + 單一 PDF 真相源。這一步零程式、純規範，但是 P7/P8 橋接能成立的前提——寫進 AGENTS.md 即可。

1. **P1**（引用誠信檢查）——價值最高、最能體現學術庫品質
2. **P4 + P3**（daily-brief 文獻分流 + V 層矛盾）——低工程量、立即可感
3. **P2 對抗式審查**——需要較細 prompt 設計，單獨一輪
4. **P7 + P8 橋接 skill**——要先有第 0 步的 join key；P7（回流）優先於 P8，因為「資產不流失」比「撰稿省事」更核心
5. **P5 / P6** 視使用狀況再加

> **品質端（P1-P5）與資料流端（第 0 步 + P7/P8）可平行推進**：前者讓單篇知識更可信，後者讓跨系統不斷鏈。最小可用閉環＝第 0 步 + P1 + P7。

每項都是**向後相容的增量**，可獨立挑選、不需一次全做。

---

## 相關連結

- [[academic-research-skills-ars]] — ARS 技能包（P1/P2 主要來源）
- [[agentic-ai-researcher-roadmap]] — 研究者 AI 路線（P3/P4/P6 來源）
- [[notebooklm-skill-research-pipeline]] — NotebookLM 研究管線
- [[notebooklm-py-cli-tool]] — NotebookLM CLI 工具
- [[karpathy-knowledge-base-method]] — 知識庫方法論（health-check 思路源頭）
- [[feedback_framework_design_decisions]] — 單人簡化設計原則（範圍控制依據）
