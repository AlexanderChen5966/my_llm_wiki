---
title: 可重現的文獻檢索：會改版的策略檔與只追加的紀錄檔要分開（PubMed MCP 實作）
date: 2026-09-23
tags: [ai/mcp, ai/workflow, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10408927（《研究生自救指南：30 天用 Claude Code 打造我的論文工具箱》Day 7，iT 邦幫忙使用者 20183988）
type: article
status: processed
asset_value: 60
---

> 系列總綱見 [[grad-thesis-claude-code-toolbox-series]]。⚠️ 本條目與方法圖鑑 D02（[[cross-domain-discovery-methodology-atlas-d02-traceable-search-skill]]）主題相近，但**不同系列、不同作者**（本系列作者為 iT 邦幫忙使用者 20183988，方法圖鑑作者為 20183944），各自成條。D02 解決的是「結果能否溯源」，本條目解決的是「同一檢索能否再跑一次並對得上」。asset_value 60 為門檻邊界案例。

## T（核心論點）

**檢索是整條文獻流程裡最不可重現的一步。** 網頁上的 PubMed 是拿來「試」的，來回幾十次之後只留下一堆 PDF，中間走過的路全部消失。要讓它可重現，要做三件事：

1. **策略（會改版）與紀錄（只追加）分成兩份檔案。** 兩種東西混在一起就讀不了。
2. **明令 AI「不要改動任何檢索式」**，並把**實際送出的字串原封不動**寫進紀錄；想改就回策略檔改、版本號加一。
3. **每次結果都存成 PMID 清單。** PubMed 的內容會變，筆數只是數字，真正要回溯的是「那天找到的是哪幾篇」。

## B（適用場景）

**適用：**
- 文獻回顧、系統性回顧：口試委員問「你怎麼確定沒漏掉重要文獻」時，拿得出「哪一天、用什麼條件、找到幾筆」的紀錄
- 🔑 **可遷移到任何 agent 查詢或 API 呼叫紀錄**：凡是「打算怎麼做」會改版、「實際做了什麼」只會累積、而且外部資料來源會變動的情境

**不適用 / 注意：**
- PubMed MCP 由社群維護，**原文沒有指名是哪一個套件**，本條目也不補上具體套件名，請自行依伺服器說明頁設定
- 🔴 路徑用 `literature/`，與 Day 4 資料夾樹的 `refs/` 不一致；Day 7 還說「Day 4 的樹狀圖裡我只畫了一個 search-strategy.md」，但 Day 4 原圖沒有畫。照原文呈現，不自行統一（見 [[raw-immutable-derived-regenerable-data-boundary]]）
- 🔴 原文有 4 處「（需作者補充：…）」佔位字（PICO 的研究對象、主要變項、結果變項，以及 MCP 與網頁筆數「差了幾筆」），本條目不引用、不推算
- 結果檔會越積越多（一個題目、四條檢索式、改三次版本就有十幾個 CSV）。作者目前全部保留、只在紀錄檔標記哪一份是「正式採用」，並表示**還沒想清楚是不是最好的做法**，屬未定案
  - ✅ Day 12 定案：結果檔全部保留，在 `search-log.md` 備註欄標「採用」；作者的理由是「不要為了整齊去刪證據」（CSV 很小、硬碟不缺，真正要管的是哪一份是正式採用的）。另見 Day 12 使用紀錄：這個工具用了 3 次、出錯 1 次（改了檢索式），記在 [[grad-thesis-claude-code-toolbox-series]] R 層五（2026-09-29 補）

## R（操作步驟）

1. **接上 PubMed MCP**（在論文專案資料夾執行，指令原文）：

   ```bash
   claude mcp add pubmed -- <PubMed MCP 伺服器的啟動指令>
   claude mcp list
   ```

   若要讓同學共用，把設定寫進專案根目錄的 `.mcp.json`（Day 26 展開，不在本批）。
2. **策略檔 `literature/search-strategy.md`**，結構：
   - 研究問題（PICO）：放最上面，提醒自己為什麼要找
   - 關鍵字與 MeSH 對照表（概念／自由詞／MeSH）：看得到有沒有漏同義詞
   - 編號檢索式 S1–S4，標版本與建立日期（例如 S4＝S1 AND S2 AND S3）：之後的紀錄可以指名道姓
   - 限制條件：日期範圍、語言、文獻類型
   - 版本紀錄：每一次改動都被看見
3. **紀錄檔 `literature/search-log.md`**，欄位原文：

   | 日期 | 策略版本 | 檢索式編號 | 實際送出的檢索式 | 筆數 | 結果檔 | 備註 |
   |---|---|---|---|---|---|---|

4. **結果檔**：`literature/results/<日期>_S4.csv`，內容為 PMID、標題、年份、期刊。
5. **下指令的範本**（原文大意）：讀策略檔 → 用 PubMed MCP 依序執行 S1 到 S4 → **不要改動任何檢索式** → 每條執行完把日期、版本、編號、實際送出的檢索式、筆數追加到紀錄檔 → S4 結果存成 CSV。
6. **三個坑**：
   - **筆數要跟網頁對一次**：作者第一次就對不上，原因是 MCP 伺服器帶的日期篩選和網頁介面預設不同。沒有誰對誰錯，但**一定要寫進備註欄**
   - **AI 會「好心」改檢索式**：第一次沒寫「不要改動」，它在 S3 加了兩個同義詞並說這樣比較完整。可能是對的，但檢索式一旦與策略檔不一致，紀錄就沒有意義；作者會用眼睛比對實際送出的字串才算數
   - **結果檔要存**：同一條檢索式半年後跑出不同筆數很正常，存了 PMID 清單才能說「這篇是後來才出現的，所以初稿沒有納入」
7. **演進過程**：策略與紀錄一開始放在同一份檔案，做了才發現「策略會改版、紀錄只會一直往下加」，所以拆成兩份。

## C（關鍵行動）

- [ ] 任何要交代過程的查詢工作，都拆成「策略檔（可改版、有版本號）」和「紀錄檔（只追加）」兩份
- [ ] 指令裡明寫「不要改動查詢條件」，並要求紀錄**實際送出的原始字串**，不接受轉述
- [ ] 保存每次結果的識別碼清單（PMID、文件 ID），不只記筆數
- [ ] 工具與網頁介面的結果對不上時，把差異原因寫進備註，而不是擇一採信
- [ ] 🔑 可遷移性：這和 [[agent-cross-source-workflow-partial-success-traceability]] 要求每個 Tool Result 保留 Source／Retrieved At／Query／Raw Result 是同一種紀錄紀律；「AI 好心改檢索式」則與 [[claude-md-behavioral-control-principles]] 的 Surgical Changes 是同一種失敗模式（做了沒被要求的「改善」）

## 原始摘要

作者被老師問到「這些文獻是用什麼條件找的、哪一天查的、總共跑出幾筆」時答不出來，因為 PubMed 搜尋歷史只剩最近幾次。這是 Day 2 說的「碰不到資料」那面牆：聊天視窗可以幫忙想關鍵字，但它說的筆數是猜的。接上社群維護的 PubMed MCP 之後，Claude Code 能真的送出檢索式、拿回結果，Day 6 被標〔未查證〕的引用也才有機會變成〔已查證〕。文章的重點不在接工具，而在把策略寫成檔案、把每次檢索記成一行，並記錄三個實際踩到的坑。

## 相關連結

- [[cross-domain-discovery-methodology-atlas-d02-traceable-search-skill]] — 🔑 **互補但不同系列、不同作者**：D02 把「幫我查」包成交代範圍／格式／存放位置的 skill，解決「能否溯源」；本篇解決「能否重現」
- [[ai-output-verifiable-id-and-status-field]] — 同系列 Day 6＋8：PMID 是可查證欄位，接上 MCP 後〔已查證〕才會出現
- [[paper-radar-literature-monitor]] — 同屬文獻工具，但該篇管每日追蹤與篩選
- [[agent-skills-vs-mcp]] — MCP 與 skill 的分工（本篇是「碰到外部資料就用 MCP」的實例）
- [[agent-cross-source-workflow-partial-success-traceability]] — Tool Result 的溯源欄位
- [[claude-md-behavioral-control-principles]] — Surgical Changes：不做沒被要求的改動
- [[grad-thesis-claude-code-toolbox-series]] — 系列總綱（R 層五為 Day 12 第二週帳本）
- [[citation-verifier-multi-field-match-report-not-fix]] — 同系列 Day 10：沒有 DOI 的引用改用本篇的 PubMed MCP 以標題查證（2026-09-29 新增）
