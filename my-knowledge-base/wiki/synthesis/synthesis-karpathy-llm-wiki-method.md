---
title: Karpathy LLM Wiki 方法論 知識精粹
date: 2026-05-22
source_count: 10
synthesis_snapshot_date: 2026-09-03
tags: [pkm/workflow, pkm/obsidian, ai/agent]
source: synthesis
type: synthesis
status: processed
sources: 10 篇
asset_value: 88
---

整合 Karpathy LLM Wiki 七個面向的觀點（原版方法、極簡版、nashsu 產品對比、結構缺陷分析、六步驟知識庫工作流、取代 RAG 的實作、以程式碼為正本的實戰案例），萃取出跨版本的共識、現有實作共同缺少的機制，以及唯一一篇真實 dogfooding 驗證出的營運極限。

## E（精粹論點）

Karpathy 方法的核心是**分工，不是工具**——人負責輸入，AI 全權維護；分工對了，工具與規模都是次要問題。但把七篇擺在一起後浮現一個更硬的結論：**Ingest 只是一次性工作，Lint 才是這套系統真正的營運成本**，而 Lint 有兩層——內部矛盾 LLM 能大量自動清除，**與現實（程式碼/實際行為）的落差則永遠需要知道正解的人來核對**，內部一致性檢查從結構上就抓不到。所以能長期運作的 LLM Wiki，除了在輸入端加矛盾偵測、結構端加語意 wikilink、維護端加主動缺口偵測，更關鍵的是**把「現實變更 → 受影響知識重新核對」制度化**，而不是指望某次自動化 Lint 跑完就等於同步完成。

---

> 🔴 **2026-09-03 重跑（7→10 篇）：這套「土法煉鋼」的模式被 Google 正式化，而 Lint 的第二層拿到了新證據。**
>
> **① 模式本身升級為開放標準**：[[okf-open-knowledge-format]]——Google Cloud 2026-06-12 開源的 OKF（Apache-2.0），本質就是**一個裝滿 Markdown 的資料夾 + YAML frontmatter**。它把 Karpathy 的私人做法變成廠商中立標準，論點是 🔑 **「限制 Agent 的不再是模型參數，而是缺乏可信、可更新、可追溯的 Context」**，解法是**回到最原始的工具**、用「格式作為唯一契約」解決互操作性。**這對本精粹是強力外部驗證**：純文字 + frontmatter 不是將就，而是刻意的架構選擇。
>
> **② 原 E 層「與現實的落差永遠需要知道正解的人核對」拿到了機制層解釋**：[[karpathy-opus5-lotr-3d-case]] 顯示 🔴 **LLM 無法有效原生地「看」影片、也不能親自把成品用一遍，所以沒辦法審核自己的產出**（Opus 5 的替代方案是不同時間點截圖再從靜態圖反推，Karpathy 形容「非常緩慢且費力」，中途搞砸好幾次）。
> 🔑 **這正是兩層 Lint 為什麼必須分開的根本原因——不是 LLM 偷懶，是它拿不到驗收所需的感官通道。** 內部一致性它能查（純文字比對），與現實的落差它結構上就查不到。
>
> **③ 純文字的可攜性有了獨立論證**：[[external-memory-layer-exploration-tax]] 從 Agent 記憶層抵達同一結論——🔑 **模型變成可替換的運算資源，記憶成為持久的組織狀態**；並補上本精粹缺的信任維度（**記憶是一份報告，不是一條指令**；年齡影響信心而非存在）與成本論證（**探索稅**：真正昂貴的不是 token，是人類又要再解釋一次）。

## C（共識核心）

> 出現於 2 篇以上來源

- **三層架構**（Raw 不可變 / Wiki 由 AI 全權維護 / Schema 定義規則）是所有版本的共同骨架——無論用 Obsidian 還是純資料夾（[[karpathy-knowledge-base-method]]、[[karpathy-no-obsidian-setup]]、[[llm-wiki-karpathy-vs-nashsu]]、[[llm-wiki-over-rag-claude-code]]、[[llm-wiki-code-as-source-of-truth-case-study]]）
- **人負責輸入，AI 全權整理**：人的角色是策展（curate）而非分類——不整理、先存再說（[[karpathy-knowledge-base-method]]、[[karpathy-no-obsidian-setup]]、[[karpathy-ai-knowledge-base-workflow]]、[[llm-wiki-over-rag-claude-code]]）
- **Ingest → Query → Lint 三操作循環**是運轉模型：Ingest 每次影響 10–15 個頁面、Query 先讀 index 再深入、有價值的答案回存成新頁（[[llm-wiki-over-rag-claude-code]]、[[karpathy-knowledge-base-method]]、[[llm-wiki-code-as-source-of-truth-case-study]]）
- **Schema 品質決定輸出品質**：CLAUDE.md / schema.md 的清晰度直接影響 wiki 頁面的準確性（[[karpathy-no-obsidian-setup]]、[[llm-wiki-karpathy-vs-nashsu]]、[[llm-wiki-over-rag-claude-code]]）
- **定期健康檢查（Lint）是必要機制**，否則錯誤會被反覆整合進去（[[karpathy-knowledge-base-method]]、[[karpathy-no-obsidian-setup]]、[[karpathy-ai-knowledge-base-workflow]]、[[llm-wiki-code-as-source-of-truth-case-study]]）
- **規模門檻約 100 篇 / 40 萬字**才顯現效果，且適用場域是「數百篇」量級而非數十萬（[[karpathy-knowledge-base-method]]、[[karpathy-ai-knowledge-base-workflow]]、[[llm-wiki-over-rag-claude-code]]）
- **AI 產出必須與人工筆記分離存放**：不只 raw/wiki 分離，自己的筆記若與 AI wiki 混放，搜尋結果與反向連結會被 AI 內容淹沒（Steph Ango 提醒；[[karpathy-ai-knowledge-base-workflow]]、[[karpathy-knowledge-base-method]]）
- **知識複利機制**：查詢結果回存讓探索本身也積累知識（[[karpathy-knowledge-base-method]]、[[llm-wiki-over-rag-claude-code]]）

---

## A（獨特視角）

| 來源 | 獨特貢獻 |
|------|---------|
| [[karpathy-knowledge-base-method]] | **知識複利**：outputs/ 回存機制讓每次查詢都成為新素材；Lex Fridman 跑步時語音問 wiki 的互動式 Podcast 延伸 |
| [[karpathy-no-obsidian-setup]] | **工具無關論**：零 App 也能跑通，關鍵不是工具而是「把整理決定交給 AI」；提供最低門檻的驗證路徑 |
| [[llm-wiki-karpathy-vs-nashsu]] | **兩步驟 CoT Ingest**（分析→生成，顯著提升品質）；**Purpose.md**（知識庫的「為什麼」）；**4 訊號知識圖譜 + Louvain 社群偵測**；**主動缺口偵測**（自動觸發 Deep Research，vs 原版被動 Lint）|
| [[karpathy-llm-wiki-improvements]] | **語意 wikilink**（`@supersedes`/`@contradicts`/`@causes` 等 24 種關係）；**Penfield 持久化後端**（跨 session 保圖結構，BM25+向量+圖遍歷混合搜尋）；**Vault Linker Skill** |
| [[llm-wiki-code-as-source-of-truth-case-study]] | **唯一真實 dogfooding 驗證**：83 筆知識 4 天後即腐化；**兩層 Lint 的關鍵區分**（①內部矛盾可自動化 ②與程式碼落差只能人工核對）；「事實正確但導覽感不理想」這類比錯誤更隱蔽的品質問題；以原始碼為 Single Source of Truth 對抗幻覺 |
| [[llm-wiki-over-rag-claude-code]] | **LLM Wiki vs RAG 的完整對照與選型表**（更新頻率/資料規模/知識性質/想要的答案）；圖書館 vs 倉庫、編譯 vs 直譯的結構類比；**log.md append-only 事件時間線**（既有版本皆無）；三層「誰寫誰讀」分工表 |
| [[karpathy-ai-knowledge-base-workflow]] | **不寫程式的三步驟簡化版**（可複製八成）：Web Clipper 存 → 每週丟給 Claude 消化 → Claude Projects 永久上傳；點名 **NotebookLM 為最接近的免費現成工具**；Steph Ango 的「AI 產出與自主筆記必須分離」提醒 |
| 🆕 [[okf-open-knowledge-format]] | 🔑 **把本方法論升格為開放標準**：Google Cloud OKF（2026-06-12，Apache-2.0，v0.1）＝Markdown 資料夾 + YAML frontmatter；**「格式作為唯一契約」**解決各家目錄廠商 SDK 與知識圖譜 schema 的鎖定（換模型就得重洗一次）。整合自 4 篇來源（Google 官方解讀／導入指南／SEO 視角／中文點評）|
| 🆕 [[karpathy-opus5-lotr-3d-case]] | 🔴 **本精粹兩層 Lint 區分的機制層依據**：Opus 5 兩小時寫 5,500 行 three.js 生出可走動的《魔戒》世界（空間推理純靠程式碼），**但無法「看」影片或親自玩過，因此無法自審成品**——只能靠不同時間點截圖反推動態，「非常緩慢且費力」|
| 🆕 [[external-memory-layer-exploration-tax]] | **從 Agent 記憶層抵達同一結論**：記憶該屬於模型周圍的系統、純文字是最重要的無聊決策（Claude 寫 GPT 讀、本地模型可質疑）。並補上本精粹缺的兩塊——**信任分級**（未審核記憶＝「Agent X 在時間 Y 說這是真的」，AI 改動已審核記憶就掉徽章；🔑 **審核不能是寫入路徑，必須是升級機制**）與**過時處理**（🔑 年齡影響信心、不影響存在）|

---

## V（知識缺口）
- ✅ **「純文字會不會太簡陋」已有外部答案（2026-09-03）**：[[okf-open-knowledge-format]] 顯示 Google 在企業級場景做同樣選擇，並把理由講成「格式作為唯一契約」。原本屬於個人偏好的決定，現在有標準背書
- 🆕 🔴 **Lint 第二層（與現實核對）的自動化被證明有結構性上限**：[[karpathy-opus5-lotr-3d-case]] 指出 LLM 拿不到驗收所需的感官通道。**這意味著「人核對現實」不是暫時的工程缺口，而是短期內無法繞過的分工**——但沒有任何來源討論「該核對哪些、多久核對一次」的取樣策略
- 🆕 ⚠️ **信任分級與本方法論的 Lint 尚未接合**：[[external-memory-layer-exploration-tax]] 的「已審核/未審核」兩級信任若套進 LLM Wiki，等於要在條目層加審核狀態欄位——**本庫目前只有 `status: processed`，沒有區分「AI 整理過」與「人核對過」**，這是可立即補上的缺口

- ⚠️ **第二層 Lint 無法自動化，是結構性硬限制**：[[llm-wiki-code-as-source-of-truth-case-study]] 證實「與現實的落差」只能由知道正解的人拿原始資料核對——但**沒有任何一篇給出這個人工環節的具體制度設計**（誰負責、多久一次、如何從變更反推受影響頁面）
- ⚠️ **矛盾解決機制**：所有篇都提到需偵測矛盾，但「衝突的兩個 wiki 頁面誰優先？如何合併？」沒有一篇提供完整流程——nashsu 只做標記，不做裁決
- ❓ **docs drift 的速度沒人量化**：實戰案例顯示 4 天就腐化，但那是單一產品單一團隊的樣本；不同領域的知識半衰期、該多久 Lint 一次，完全沒有依據
- ❓ **規模邊界仍未解**：[[llm-wiki-over-rag-claude-code]] 給了「數百篇適合 LLM Wiki、數千至數百萬該用 RAG」的粗略分界，但「單一 wiki 頁面何時該拆分？主題邊界如何決定？」依然無解
- ❓ **Model Collapse 預防**：多篇補充警告 LLM 反覆讀自己產出會磨平細節、風格單一化（Nature 2024），但沒有任何一篇提供具體對策

---

## 整合行動清單

- [ ] 把「人負責策展、AI 負責整理」的分工寫進 CLAUDE.md（這是 Schema 品質的起點）
- [ ] **把 Lint 明確拆成兩層**：①LLM 自動清內部矛盾 ②人工拿一手來源核對現實落差——不要以為跑完自動化 Lint 就等於同步完成
- [ ] **建立「來源變更 → 受影響知識重新核對」的制度化流程**，而非依賴個人記得去同步
- [ ] 升級 raw-pipeline Agent A 為兩步驟 CoT Ingest：Step 1 分析（找實體/偵測矛盾）→ Step 2 生成 wiki
- [ ] 為重要 wikilink 加語意類型標注（至少 `@supersedes` 和 `@contradicts`，優先處理有分歧的主題）
- [ ] 考慮加入 Purpose.md（統一 ingest 方向）與 **log.md**（append-only 記錄 ingest/query/lint 事件，便於回溯腐化時點）
- [ ] 驗證知識品質時，除了對錯，也要檢查「事實正確但表達角度不理想」這類隱蔽問題

---

## 來源條目

- [[karpathy-knowledge-base-method]] — 原版方法論：三資料夾法 + 知識複利（丟進去 → AI 整理 → 查詢回存）
- [[karpathy-no-obsidian-setup]] — 極簡版：零 App，只要三資料夾 + CLAUDE.md；驗證工具無關論
- [[llm-wiki-karpathy-vs-nashsu]] — 產品對比：nashsu 在骨架上加 CoT Ingest / 知識圖譜 / 主動 Deep Research
- [[karpathy-llm-wiki-improvements]] — 結構缺陷分析：語意 wikilink / Vault Linker / Penfield 持久化後端
- [[llm-wiki-over-rag-claude-code]] — 取代 RAG 的實作：選型對照表、Ingest/Query/Lint 三操作、log.md（AV 75）
- [[karpathy-ai-knowledge-base-workflow]] — 六步驟工作流與不寫程式的三步驟簡化版；AI/人工筆記分離提醒（AV 72）
- [[llm-wiki-code-as-source-of-truth-case-study]] — 真實 dogfooding：83 筆 4 天腐化、兩層 Lint 的關鍵區分（AV 62）
- [[okf-open-knowledge-format]] — 🔑 **Karpathy LLM-Wiki 模式被 Google 正式化為開放標準**（OKF，2026-06-12 Apache-2.0）：本質就是「裝滿 Markdown 的資料夾 + YAML frontmatter」，用「格式作為唯一契約」解決廠商鎖定
- [[karpathy-opus5-lotr-3d-case]] — 🆕 Opus 5 兩小時 5,500 行《魔戒》3D；🔴 **LLM 無法自審成品**（拿不到驗收的感官通道）
- [[external-memory-layer-exploration-tax]] — 🆕 記憶屬於模型周圍的系統、純文字可攜、信任分級與探索稅（AV 85）
