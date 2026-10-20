---
title: 研究生論文工具箱系列總綱：外包雜事不外包思考，非工程師的 Claude Code 分工原則
date: 2026-09-23
tags: [ai/claude, ai/workflow, type/concept]
source: https://ithelp.ithome.com.tw/articles/10407323; https://ithelp.ithome.com.tw/articles/10407629; https://ithelp.ithome.com.tw/articles/10407883; https://ithelp.ithome.com.tw/articles/10408287; https://ithelp.ithome.com.tw/articles/10410477（《研究生自救指南：30 天用 Claude Code 打造我的論文工具箱》Day 1、2、3、5、12，iT 邦幫忙使用者 20183988；系列頁 https://ithelp.ithome.com.tw/users/20183988/ironman/9487）
type: article
status: processed
asset_value: 64
---

> **系列 hub 條目**：2026 iThome 鐵人賽 Claude AI 組《研究生自救指南：30 天用 Claude Code 打造我的論文工具箱》（iT 邦幫忙使用者 20183988，剪輯中沒有顯示作者名稱）。本條目承載 Day 1、2、3、5 的論點＋Day 12 第二週回顧（2026-09-29 補）；Day 9 併入 [[ai-output-verifiable-id-and-status-field]]，Day 10、11 各自成條。Day 4、6＋8、7 各自獨立成條，見下方「系列脈絡表」。

## T（核心論點）

**外包雜事，不外包思考。** 作者估計碩班時間約 **85%** 花在有明確規則、能驗證對錯的雜事上（文獻整理 35%、資料 25%、格式 20%、行政 20%），只有「讀完決定怎麼用」和「看結果決定代表什麼」這不到 15% 非本人不可。工具要吃掉的是那 85%。⚠️ 這組比例是作者**憑印象估算**，不是量測數據。

**選工具看三個判準：能累積、能稽核、能碰資料。** 這三點剛好是聊天視窗的三面牆：不能累積（每天都要重新交代）、不能稽核（做過的事埋在對話裡）、碰不到資料（不能也不該貼受試者資料）。分工原則是：**要留下來的、要重複做的、要碰到資料的進 Claude Code；問完就算的留在聊天視窗。**

**規矩放哪裡：CLAUDE.md 管背景（每次都讀），skill 管做法（用到才翻）。** 分不清時只問一句：「這段話每次啟動都需要嗎？」需要就進 CLAUDE.md，不需要就做成 skill。

📎 Day 12 實測後的版本：**AI 吃掉的是「對」，留下的是「判斷」。**「對」是對頁碼、對欄位、對格式、對資料庫，有標準答案、做一百次做法都一樣；「判斷」是這週每個工具都留給人的那一塊（摘要卡的「我的用途」、矩陣依概念排序、查證的 🟡、規則檔與老師習慣衝突時聽誰的、中文文獻）。詳見 R 層五。

## B（適用場景）

**適用：**
- 非工程師（研究生、行政、PM）第一次決定「哪些事交給 AI、交給哪個工具、規矩寫在哪」
- 帶非技術同事導入 Claude Code 時，需要一套不用懂程式也能執行的判準
- 任何要碰敏感資料的個人專案（問卷、病歷、客戶資料）

**不適用 / 注意：**
- 作者背景是**護理系出身、有臨床經驗、不會寫程式**的碩士生，論文是問卷型量性研究、APA 第七版（問卷由 Google 表單匯出 CSV，三百多筆、六十幾欄）。質性研究、理工實驗室的工作比例會不同
- **系列三條禁令**是整個系列的邊界：① 不讓 AI 寫論文內容 ② 不生成未查證的引用 ③ 未去識別化的資料不離開電腦（文中範例一律用假資料重建）
- Day 3、Day 5 與既有 CLAUDE.md／skill 條目大量重疊，本條目只收它們獨有的部分，通論請看相關連結

## R（操作步驟）

### 一、Day 2：分工的單位是「資料夾」，不是「一次對話」

1. 聊天視窗的記憶以「一次對話」為單位，Claude Code 以「一個資料夾」為單位；它的每個動作都會變成檔案變化，看得到 diff、也能退回。
2. 作者的對照例：「檢查參考文獻是否存在」在聊天視窗只能逐筆「看起來」判斷；在 Claude Code 則是寫腳本去 PubMed／DOI 查，留下可重跑的腳本與查證結果表（Day 10 展開，不在本批）。
   - 📎 2026-09-29 更正：這是 Day 2 的原文說法（原文：「寫一支腳本，逐筆去 PubMed 或 DOI 查」「一支可以重跑的腳本」）。Day 10 實際做法是 `cite-check` SKILL.md＋Claude Code 直接查 Crossref API（無 DOI 時用 PubMed MCP），**沒有獨立腳本檔**，留下的是查證報告 `literature/cite-check.md`。見 [[citation-verifier-multi-field-match-report-not-fix]]。
3. 聊天視窗仍然適合問概念、解釋英文摘要、腦力激盪。

### 二、Day 3：安裝與第一份 CLAUDE.md（作者稱為「交班單」）

1. 安裝指令原文照錄，作者**刻意不寫死版本號**，先裝 Node.js：

   ```bash
   npm install -g @anthropic-ai/claude-code
   ```

2. 三個卡點：要先開終端機；要先 `cd` 到論文資料夾再啟動（啟動目錄就是工作範圍）；第一次啟動的「信任資料夾」提示不要無腦按同意。
3. CLAUDE.md 四區塊：這個專案是什麼／我要你怎麼回話／絕對不要做的事（作者比喻為床頭的過敏標示）／現在進度到哪。「絕對不要」裡有一條值得抄：**統計數字一律從 `analysis/` 的輸出檔讀，不要憑印象寫。**
4. **實測：第一版將近 200 行，反而讓 Claude 抓不到重點**，還開始在回答裡夾雜沒問的背景；砍到 30 行以內效果才好轉。判準：「這句話會不會影響它接下來的動作？」
5. 更新訊號：**同一條規則第二次交代時，就該寫進 CLAUDE.md。**
6. 引用規則的演進：Day 1、3 的草稿寫「引用只能用 `refs/` 裡已存在的文獻」；Day 6 接上查證機制後放寬為六條規則，**以 Day 6 版本為準**（見 `ai-output-verifiable-id-and-status-field`）。

### 三、Day 5：skill 的位置與「故意缺欄位」核對手法

1. 專案級路徑 `.claude/skills/<name>/SKILL.md`，使用者級放在使用者家目錄的 `.claude/skills/`；frontmatter 的 `name` 與資料夾同名，`description` 是觸發依據。
2. 作者先用手動 `/apa7-zh` 觸發，確定它照流程走之後才放給自動判斷。
   - 🔴 Day 11 新建的 APA 校對 skill 叫 `apa-check`，並另抽出規則檔 `literature/apa-rules.md`；原文沒有交代它和 `apa7-zh` 是改名、取代還是兩個 skill，照實呈現。見 [[ai-autonomy-by-rule-clarity-verifiability-reversibility]]。
3. 🔑 **可重用的核對手法**：準備三筆測試資料，其中一筆**故意缺欄位**（缺頁碼），看 skill 是標〔缺：頁碼〕還是自己補了一個。如果它補了，表示「不猜、不補」這條規則沒有生效，就把這條規則**移到更前面、寫得更硬**。這個手法可以套用到任何「禁止型規則」的驗收。
4. 作者自承尚未解決：`description` 寫太窄會漏觸發、寫太寬會誤觸發，仍在試。
5. 🔴 Day 5 有 2 處「需作者補充」佔位字（第幾次開始複製貼上、系所的特殊格式規定），原文並未提供，本條目不引用、不推算。

### 四、系列脈絡表

| Day | 主題 | 本庫條目 |
|---|---|---|
| 1 | 為什麼：時間盤點 85／15、三條禁令、第一份 CLAUDE.md | 本條目 |
| 2 | 選工具：聊天視窗的三面牆 → 三個判準 | 本條目 |
| 3 | 背景層：CLAUDE.md＝交班單，四區塊、30 行以內 | 本條目 |
| 4 | 資料層：raw 只進不出、clean 可重生 | [[raw-immutable-derived-regenerable-data-boundary]] |
| 5 | 做法層：skill＝SOP；apa7-zh | 本條目 |
| 6 | 驗證層：捏造引用 → 必附 DOI／PMID＋必填狀態欄＋引用三讀 | [[ai-output-verifiable-id-and-status-field]] |
| 7 | 文獻檢索：PubMed MCP＋策略檔與紀錄檔分離 | [[reproducible-literature-search-strategy-log-separation]] |
| 8 | 文獻閱讀：paper-card，每句附頁碼＋〔未核對〕 | [[ai-output-verifiable-id-and-status-field]] |
| 9 | 文獻比較矩陣 lit-matrix，只收〔已核對〕的卡 | [[ai-output-verifiable-id-and-status-field]] |
| 10 | 引用查證器 cite-check：五欄比對、三色結果、只報告不修正 | [[citation-verifier-multi-field-match-report-not-fix]] |
| 11 | APA 校對 apa-check＋獨立規則檔；授權三條件 | [[ai-autonomy-by-rule-clarity-verifiability-reversibility]] |
| 12 | 第二週回顧：帳本與決議 | 本條目（R 層五） |

- **後續預告（不在本批，尚未入庫）**：Day 9 文獻比較矩陣、Day 10 引用查證腳本、Day 18 在原始資料上直接動手的事故、Day 26 skill 分享給同學、Day 27 隱私倫理。
  - 📎 2026-09-29：Day 9–12 已入庫（見上表）；Day 10 實際是查證 skill，不是腳本。下一篇 Day 13 為問卷資料清理。
- 系列仍在連載，總篇數不寫死：Day 1–4 剪輯時系列頁顯示 15 篇，Day 8 剪輯時顯示 20 篇。Day 9 剪輯時顯示 20 篇，Day 10–12 剪輯時顯示 21 篇。
- 貫穿全系列的設計模式：**AI 輸出必帶可查證欄位 → 預設為未驗證狀態 → 人工抽查後才升級 → 下游只收已驗證的輸出**。

### 五、Day 12：第二週帳本（2026-09-29 補）

🔴 本節所有數字都是**作者自述**。作者借病房品管會議的原則：看的不是誰犯錯，是哪個流程會讓人犯錯；回頭看不是為了證明工具有用，是為了找出它們在哪裡沒用。

**1. 五個工具，一張表**

| 工具 | 這週用了幾次 | 一次省下（估） | 出錯幾次（核對時抓到的） |
|---|---|---|---|
| Day 7 檢索紀錄 | 3 | 約 20 分鐘 | 1（改了檢索式） |
| Day 8 摘要卡 | 14 | 約 25 分鐘 | 3（標準差抄成人數、掃描檔硬摘、頁碼用錯） |
| Day 9 比較矩陣 | 4 | 首次約 1.5 小時，重跑幾乎為零 | 1（把長格壓短） |
| Day 10 引用查證 | 2 | 首次約 2 小時，重跑約 30 分鐘 | 1（順手改了原檔） |
| Day 11 APA 校對 | 2 | 首次約 3 小時，重跑幾乎為零 | 1（規則檔路徑錯，套用它自己的 APA） |

- 🔴 **「出錯」只算核對時抓到的**，沒抓到的不知道有幾次，這一欄天生少報
- 🔑 **省時的算法**：以前做這件事要多久，減掉現在「做完＋核對」要多久。核對時間一定要算進去，否則數字會好看到不真實。例：摘要卡以前一篇筆記 40 分鐘，現在製卡 3 分鐘＋核對 12 分鐘，省 25 分鐘

**2. 老實算帳：小輸**

- 建置：五個工具各花二到四小時，合計約 **15 小時**（含寫 SKILL.md、第一次跑、發現出錯、改規則、再跑）
- 省下：檢索 1＋摘要卡約 6＋矩陣 1.5＋查證 2.5＋校對 3，合計約 **14 小時**
- 十五對十四，嚴格算是小輸。作者認為工具的價值在後三週的重複使用，並**刻意把這個數字留下來，防止寫到 Day 30 時把帳算得比實際好看**
- 不能換算成時數的收穫：以前做完就消失的事（檢索式、筆記、查證）現在都留下可以回頭看的紀錄，老師再問「文獻怎麼來的」可以打開 `search-log.md` 回答

**3. 工具的覆蓋缺口**

- 38 筆參考文獻中 **15 筆是中文**（約四成）；查證報告的 13 筆 🔴 中，**12 筆是中文文獻「來源外」，真正捏造的只有 1 筆**（Day 6 那筆）→ 文獻工作約只有**六成**被工具覆蓋

**4. 前五篇「還不確定」的答案（決議已分派到各條目）**

| 原問題 | Day 12 決議 | 記在哪裡 |
|---|---|---|
| Day 7 結果檔越存越多 | 全部保留，`search-log.md` 備註欄標「採用」；「不要為了整齊去刪證據」 | [[reproducible-literature-search-strategy-log-separation]] |
| Day 8 先做卡會不會漏文獻 | 抽查排除的 9 篇，2 篇應收，漏失率約兩成；排除的要在「我的用途」寫原因 | [[ai-output-verifiable-id-and-status-field]] |
| Day 9 矩陣太寬 | 拆成方法學、結果兩張，改規則四、加參數 | [[ai-output-verifiable-id-and-status-field]] |
| Day 10 要不要定期重跑 | 只在口試前一週、投稿前跑；定期跑會養成「跑過就安心」 | [[citation-verifier-multi-field-match-report-not-fix]] |
| Day 11 規則檔越寫越長 | 只寫實際踩過的規則，不搬學校手冊 | [[ai-autonomy-by-rule-clarity-verifiability-reversibility]] |

**5. 三個仍無答案**

1. 中文文獻查證：華藝、國圖都沒有公開查詢介面，構想是半自動（工具整理清單 → 人工查 → 工具寫回報告），還沒試
2. 要不要讓老師知道用了這些工具：留到 Day 29
3. 語氣被改肯定（「可能相關」→「相關」）的錯仍然抓不到，因為核對看的是欄位不是措辭

## C（關鍵行動）

- [ ] 導入任何 AI 工作流前，先做一次時間盤點：哪些工作有明確規則、能驗證？那才是該外包的部分
- [ ] 用三個判準（能累積／能稽核／能碰資料）決定一件事該在聊天視窗還是 Claude Code 做
- [ ] CLAUDE.md 維持在「每句都會改變行為」的長度；同一條規則交代第二次就寫進去
- [ ] 寫完任何禁止型規則（不猜、不補、不改），用一筆故意缺欄位的測試資料驗收它是否真的生效
- [ ] 算 AI 工具省多少時間時，把核對時間算進去；建置成本要等重複使用才回本（Day 12）
- [ ] ⚠️ 術語提醒：作者說的「交班單」是 **CLAUDE.md（長期規矩）**，不是 [[claude-code-session-handoff]] 講的 session 交接檔（當下這一攤）。兩者是「記憶管長期、交接管當下」框架的兩端，不可混為一談

## 原始摘要

作者是護理出身、不會寫程式的碩士生，用 30 天記錄如何用 Claude Code 為自己的論文打造工具箱，目標是留下「一套你知道它會在哪裡壞掉的工具箱」。Day 1 盤點時間，提出「外包雜事、不外包思考」與三條禁令，並建立第一份 CLAUDE.md；Day 2 用聊天視窗的三面牆推出選工具的三個判準；Day 3 安裝 Claude Code，把 CLAUDE.md 寫成只含「會影響下一班判斷」資訊的交班單；Day 5 區分 CLAUDE.md（背景）與 skill（做法），示範 apa7-zh skill 與缺欄位測試。整個系列以護理臨床的隱喻（交班、病歷不能塗改、SOP、三讀五對）串起研究工作流。關鍵句：「一個你不知道何時會出錯的工具比沒有工具更危險，因為你會相信它。」Day 12（2026-09-29 補）回頭算第二週五個工具的帳：建置約 15 小時、省下約 14 小時，嚴格算小輸；AI 吃掉的是「對」，留下的是「判斷」；中文文獻占四成，工具幾乎碰不到。

## 相關連結

- [[academic-research-skills-ars]] — 🔑 **立場相同的對照組**：都是「AI 當副駕駛，不當機長」；ARS 是現成的學術技能包，本系列是非工程師從零自建
- [[raw-immutable-derived-regenerable-data-boundary]] — 同系列 Day 4：資料夾的保護等級
- [[ai-output-verifiable-id-and-status-field]] — 同系列 Day 6＋8：可查證欄位＋必填狀態欄（📎 2026-09-29 另收 Day 9 比較矩陣）
- [[reproducible-literature-search-strategy-log-separation]] — 同系列 Day 7：可重現的文獻檢索
- [[citation-verifier-multi-field-match-report-not-fix]] — 同系列 Day 10：引用查證器（2026-09-29 新增）
- [[ai-autonomy-by-rule-clarity-verifiability-reversibility]] — 同系列 Day 11：APA 校對與授權三條件（2026-09-29 新增）
- [[fable-5-delegation-cost-benchmark]] — Day 12「核對時間要算進省時」與委派成本要含驗收的總成本是同一方向（2026-09-29 新增）
- [[synthesis-claude-md]] — CLAUDE.md 精粹；Day 3「少即是多、寫會改變行為的事」是其共識的再確認
- [[claude-md-guide]] / [[claude-md-design-patterns]] — CLAUDE.md 該寫什麼、不該寫什麼
- [[steering-claude-seven-carriers]] — 規範放哪一層的完整版（本系列只用到 CLAUDE.md 與 skill 兩層）
- [[claude-skills-guide]] / [[sop-to-skill-4-steps]] — skill 的通論與 SOP 轉 skill 方法
- [[dual-tool-strategy]] / [[claude-code-three-modes-distinction]] — 聊天介面與 Claude Code 的分工
- [[claude-code-session-handoff]] — ⚠️ 術語防混：該篇的「交接檔」是當下這一攤，本系列的「交班單」是 CLAUDE.md
