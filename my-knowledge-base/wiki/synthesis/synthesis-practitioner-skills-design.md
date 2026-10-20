---
title: 實戰者 Skills 設計模式 知識精粹
date: 2026-05-22
updated: 2026-08-04
source_count: 7
synthesis_snapshot_date: 2026-09-03
tags: [ai/agent, ai/claude, dev/architecture]
source: synthesis
type: synthesis
status: processed
sources: 7 篇
asset_value: 85
---

整合六篇條目：三位實戰者的 Skills 設計套件（Addy Osmani 20 個工程師 Skill、agent-skills 22 個生命週期 Skill、Matt Pocock 14 個組合工具箱）、輕量組合派的代表實作（grill-me）、重型框架的量化解剖（Superpowers 風險分級），以及把 Skill 效益量化的三種評估工具實測。萃取共同設計邏輯、哲學分歧，以及 2026-07 後量化數據帶來的判準翻轉。

## E（精粹論點）

三位實戰者的 Skills 指向同一個起點：**把「資深工程師的隱性判斷」轉化為 AI 可執行的顯性工作流**。但 2026-07 後的實測數據翻轉了判準——**Skill 買到的不是「更好的上限」，是「強制的一致性」**：官方 skill-creator 實測顯示改善後的 Skill 與無 Skill baseline 差距僅 ±8pt（底層模型本來就強），而**寫得模糊的 Skill 反而輸給 baseline**（92%±14% vs 100%±0%）。

一旦接受「Skill 不增加能力、只買一致性」，原本「大型生命週期框架 vs 小而可組合工具箱」的哲學分歧就不再是品味之爭，而是**風險校準問題**：正確的控制變數是變更風險與失敗代價，不是框架信仰。硬門檻措辭（「只要有 1% 可能就必須呼叫 skill」）把「通常有幫助」寫成「永遠必須」，在強模型時代壓縮的是模型的有效自由度——把規範分級（Lite / Standard / Strict）才是第三條路，實測讓核心 SKILL.md 從 3,207 行降到 110 行。

> 本論點與 [[synthesis-ai-dev-workflow-frameworks]] 的「該不該用重型框架」張力同源，但本篇多一層：那邊問「框架要多重」，這邊答「**先證明它勝過 baseline，再談輕重**」。

---

> 🔴 **2026-09-03 重跑（6→7 篇）：本精粹 E 層要求「先證明它勝過 baseline」，但怎麼建那個 baseline 一直是空的——現在有答案了。**
>
> [[search-eval-set-design]] 提供了評測集設計的可操作方法，而它最反直覺的三點正好都打在本精粹的痛處：
>
> - 🔑 **標準答案標在文件層級、不標 Chunk**——Chunk 識別碼跟著切分策略走，一換策略就作廢；**標籤依賴某個策略會讓「比較策略」自我矛盾**。**對應到 Skill：評測集若綁定某版 SKILL.md 的步驟編號，改版就等於重建評測集。**
> - 🔑 **要放進「知識庫答不了」的無答案題**——評測集測的是**能力的邊界，不是文件聊過什麼**。對應到 Skill：要放「這個 Skill 不該被觸發」的案例，否則量不到 overtrigger。
> - 🔑 **分類表比總分有價值**（三方法比分 0.75→0.92→1.00，但作者說分類表才是重點）。這直接解釋了本精粹的 ±8pt 之謎——**單一總分看不出 Skill 到底在哪些案例上贏、哪些上輸**。
> - 🔴 並附一個警告：**裸分數當不了拒答門檻**（門檻設 7.27 會正確拒絕一題但錯殺四題）。
>
> 🔑 **合起來：「Skill 買的是一致性不是上限」這個結論若要驗證，需要的不是一個總分，而是一張分類表 ＋ 一組不該觸發的案例。**

## C（共識核心）

> 前四點出現於 3/6 篇（原三套實戰者套件），第五點為 2026-08 新增的三篇獨立同結論

- **把隱性工程師直覺轉化為 AI 可執行的顯性 checklist / 工作流**：三套實戰者 Skills 共同的設計目標（[[addyosmani-engineering-skills]]、[[agent-skills-toolkit]]、[[mattpocock-claude-skills-real-engineers]]）
- **Skill 是流程，不是說明文件**：agent-skills 明確說「Skill 是工作流，不是參考資料」；Matt Pocock 的 Skills 也是工作流模式；[[grill-me-agent-deep-questioning-skill]] 更進一步——**Skill 要規定 AI 怎麼互動（一次只問一個決策點），不只規定它做什麼**
- **針對 AI 的四大失敗模式設計**：需求錯位（grill-me）、程式碼不能跑（TDD）、架構熵增（improve-codebase-architecture）、品質問題（5 軸審查 + 安全 hardening）
- **軟體工程基本功在 AI 時代更重要，不是更不重要**：agent-skills 的 Google 工程文化編碼（Hyrum's Law、Beyoncé Rule）；Matt Pocock 的「基本功比以往更重要」；Addy 的「最容易出錯的工程判斷先做成 Skill」
- 🆕 **強模型時代要減量，不是加量**（[[superpowers-skill-bloat-risk-tiering]]、[[grill-me-agent-deep-questioning-skill]]、[[agent-skill-evaluation-tools-comparison]] 三篇從不同角度獨立得出同結論）：Superpowers 14→6 個 skill、grill-me 選擇手術刀而非全自動框架、評估工具實測「模糊 Skill 不如不用」——都指向「過度約束高推理模型是淨負面」

---

## A（獨特視角）

| 來源 | 獨特貢獻 |
|---|---|
| [[addyosmani-engineering-skills]] | 核心設計邏輯：**「最容易出錯的工程判斷」優先封裝**；Skill 要有具體檢查標準（不能只說「注意安全性」）；搭配 AGENTS.md 自動載入，進入專案即啟動 |
| [[agent-skills-toolkit]] | **完整六階段生命週期**（DEFINE→PLAN→BUILD→VERIFY→REVIEW→SHIP，22 個 Skill）；**Google 工程文化編碼**（Hyrum's Law / Beyoncé Rule / Chesterton's Fence / Shift Left）；**反藉口機制**（預先堵死「測試之後再寫」）；**自動觸發**（設計 API 時自動觸發 api-and-interface-design）；**漸進式揭露**；三個 Agent Personas |
| [[mattpocock-claude-skills-real-engineers]] | **Small、composable 哲學**（大框架奪走控制權、讓 bug 難排查）；**CONTEXT.md 共同術語表**（1 個詞代替 20 個詞）；**Karpathy Skills（行為約束）vs Matt Pocock Skills（工作流模式）** 的明確對比 |
| 🆕 [[grill-me-agent-deep-questioning-skill]] | **輕量組合派的完整實作**：拒絕無效廢問（可查程式碼庫得知的事實**嚴禁向使用者提問**，只問需要商業邏輯選擇／架構權衡的決策點）；**一次僅拋一個問題並附建議選項**；下游閉環 `grill-me`→`to-prd`→`to-issues`→TDD；`grill-me-doc` 反向審查既有 PRD 的模糊條款。CyberQ 的**世代適配論**：高推理模型（Sol 5.6／Fable 5）配輕量組合技能，重型框架反而造成 token 浪費與無謂迴圈 |
| 🆕 [[superpowers-skill-bloat-risk-tiering]] | **重型框架的量化解剖**：3 億 token/日 重度使用者實測拆掉 Superpowers（核心規則 3,207 行→110 行）。**三級分工**（Lite 預設／Standard 多檔改動／Strict 高風險且需使用者明確選擇）；**子代理成本模型**（同一份需求被主代理＋實作代理＋規格審查＋品質審查各讀一次，重複越多任務越多）；**正確的控制變數清單**（變更風險／失敗代價／是否有獨立知識盲區／是否有可執行驗證手段／使用者是否明確要求）。<br>🆕 **2026-08-05 複查後強化＋一處修正**：Anthropic 官方 ablation（刪 8 成提示詞零下滑）與上游自家 24.21 GB rollout 審計為其背書；上游 null-prompt 對照實驗更狠——**絕對措辭把 bounded 任務 5/5 推向 FULL ceremony，而完全無指引的 null prompt 反而能完美分類**，即硬門檻在**主動壓制模型原生判斷力**。🔴 **但該篇 Lite 層「不做自動審查」已修正**：上游立場是 **ceremony scales, approval never does**——砍文件不砍核准 |
| 🆕 [[agent-skill-evaluation-tools-comparison]] | **把 Skill 品質從主觀變可量化**：三工具實測（官方 skill-creator 有／無技能 A/B ＋ 觸發率 train/test 切分；darwin-skill 9 維度評分 ＋ **爬山棘輪機制**——評分者與編輯者分開、只有真變好才 git commit、變差就 revert；Microsoft SkillOpt 把 SKILL.md 當神經網路權重梯度最佳化，含抗過擬合的 Gate 階段）。🔴 **關鍵數據**：模糊 Skill 92%±14% **輸給** baseline 100%±0%；darwin-skill 基線 19.2/100 →3 輪迭代 77.5/100 |
| 🆕 [[search-eval-set-design]] | 🔑 **補上本精粹缺的「怎麼建 baseline」**：標準答案標在**文件層級不標 Chunk**（否則比較策略會自我矛盾）／**放進無答案題**測邊界而非測聊過什麼／🔑 **分類表比總分有價值**（直接解釋 ±8pt 為何無資訊量）／🔴 **裸分數當不了拒答門檻** |

---

## V（知識缺口）
- ✅ **「怎麼證明勝過 baseline」已有方法（2026-09-03，原首要空白解除）**：[[search-eval-set-design]] 給出評測集設計三原則與分類表優於總分的論證
- 🆕 ⚠️ **但它是搜尋領域的方法，移植到 Skill 評測需要一次轉譯**：搜尋的「文件層級標註」對應到 Skill 的什麼層級（任務？案例？判斷點？）**沒有來源做過這個對映**——這是可立即補上的缺口

- ✅ **已補上（原缺口 3：量化效益）**：[[agent-skill-evaluation-tools-comparison]] 提供 A/B 對照數據與三套獨立評分軸，且三軸同結論。**但代價是新缺口**——見下方 LLM judge 噪聲
- 🔶 **部分補上（原缺口 1：長期維護成本）**：darwin-skill 的棘輪機制（只保留真的變好的修改）與 Superpowers 3,207→110 行的減量實測，證實膨脹是真問題且可逆；**但「框架／語言升級時 22 個 Skill 如何同步更新」仍無答案**
- ⚠️ **多 Skills 同時觸發的衝突處理**（原缺口 2，仍完全開放）：api-and-interface-design 與 security-and-hardening 同時啟動、規則衝突時誰優先？六篇皆無處理機制
- 🆕 ⚠️ **評估工具本身不可靠**：darwin-skill 分數本質是 LLM judge，SkillLens 實證 LLM 自評準確率僅 **46.4%**；官方 skill-creator 的 stddev ±14% 也顯示結果 flaky。只能用來抓弱項與做棘輪，不能當絕對值信賴——「量化」解決了**有沒有效**，沒解決**效果多大**
- 🆕 ❓ **風險分級的判定者是誰**：Superpowers 主張 Strict 級「應由使用者明確選擇而非自動觸發」，但這把判斷責任推回人；若人不會判斷風險（正是最需要框架的族群），分級制反而失效——這個遞迴問題無人處理

---

## 選擇指南（2026-08 修訂）

| 場景 | 建議 | 理由 |
|------|------|------|
| **還沒驗證過 Skill 有效性** | 先跑 skill-creator 有/無 A/B | 模糊 Skill 會輸給 baseline，未驗證就部署是負收益 |
| 需要精準對齊需求（寫碼前收斂） | grill-me → to-prd → to-issues | 一次一決策點 ＋ 強制自主查程式碼庫，不問已知資訊 |
| 需要把最容易出錯的判斷先固化 | Addy Osmani | 優先封裝高風險工程判斷，具體 checklist |
| 需要完整工程紀律、團隊協作或低自主性代理 | agent-skills | 22 個 Skill 六階段覆蓋，Google 工程文化編碼 |
| **高推理模型 ＋ 單人開發** | 輕量組合（grill-me 式）＋ 風險分級 | 重型框架在強模型上壓縮有效自由度，淨負面 |
| 高風險任務（安全/付款/權限/資料遷移/公開 API） | Strict 級完整儀式鏈 | 這是唯一應保留完整審查流程的類別 |

---

## 整合行動清單

- [ ] 🔴 **部署任何 Skill 前先做有/無 A/B 對照**，確認它真的優於 baseline——這是本次新增的第一道門檻
- [ ] 辨識自己專案中「最容易出錯的工程判斷」，這些是最先封裝成 Skill 的候選
- [ ] Skill 的 description 只寫「什麼時候用」，不寫「做什麼」——否則 AI 讀描述就跳過正文
- [ ] 把既有框架的硬門檻措辭（「任何專案都必須…」）改寫成風險分級（Lite/Standard/Strict）
- [ ] 改善 Skill 時用棘輪機制：評分者與編輯者分開、只有分數上升才保留、變差就 revert
- [ ] 不要信賴評分工具的絕對數值（LLM judge 自評準確率僅 46.4%），只看「是否勝過 baseline」與趨勢

---

## 來源條目

- [[addyosmani-engineering-skills]] — 20 個工程師 Skill：把資深工程師的審慎判斷變成 AI 可執行 checklist
- [[agent-skills-toolkit]] — 22 個 Skill 六階段生命週期：Google 工程文化 ＋ 反藉口機制 ＋ 自動觸發
- [[mattpocock-claude-skills-real-engineers]] — 14 個組合工具箱：small & composable，CONTEXT.md 術語表
- [[grill-me-agent-deep-questioning-skill]] — 輕量組合派代表實作：深度追問取代盲目奉承，一次一決策點；CyberQ 世代適配論
- [[superpowers-skill-bloat-risk-tiering]] — 重型框架量化解剖：3,207→110 行，三級風險分工，子代理成本模型
- [[agent-skill-evaluation-tools-comparison]] — 三種評估工具實測：模糊 Skill 輸給 baseline，Skill 的價值在強制一致性而非提升上限
- [[search-eval-set-design]] — 🆕 🔑 評測集設計：標文件層級、放無答案題、分類表比總分有價值（AV 82）

## 相關精粹

- [[synthesis-ai-dev-workflow-frameworks]] — 「該不該用重型框架」張力同源；本篇多一層「先證明勝過 baseline 再談輕重」
- [[synthesis-prompt-engineering-evolution]] — 提示詞工程三次躍遷；「Instructions don't add capability」與本篇「Skill 不增加能力只買一致性」是同一命題的不同層級
