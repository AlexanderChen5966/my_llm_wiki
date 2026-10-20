---
title: 規格驅動 AI 工作流 知識精粹
date: 2026-06-05
updated: 2026-08-04
source_count: 9
synthesis_snapshot_date: 2026-09-03
tags: [ai/agent, dev/architecture, ai/workflow]
type: synthesis
status: processed
sources: 9 篇
asset_value: 90
---

整合七篇條目：ZeroSpec（Layer 0 環境就緒）、planning-with-files 方法論與 Skill 實作、spec-kit/OpenSpec + SDD、四框架選型比較、task-list 唯一正本、義務即程式碼。這七篇共同回答同一個問題——AI 開發為什麼失敗——並各自鎖定答案鏈路的不同位置。

## E（精粹—Deep Essence，2026-08-04 深化）

**核心洞察（一句話）**：
規格把模糊意圖鎖定為書面錨點只是上半場——**錨點必須接上機器可判定的驗證，否則規格只是換個地方存放的幻覺**。

**信心等級**：high
> 7/7 篇支持「規格先行」；新增三篇進一步補上原本缺失的下半場（驗證與狀態追蹤），且有量化佐證

**這次深化補上什麼**：
原版 E 層點出最大反例是「**錯誤的規格讓 AI 完美執行錯誤任務**」，卻沒有偵測機制。三篇新來源正好各補一塊：

| 缺的那塊 | 補上的來源 | 機制 |
|---|---|---|
| 該用哪個框架、各層怎麼銜接 | [[ai-sdd-framework-comparison]] | 四框架依「回答的問題」分工：ZeroSpec 環境基礎／OpenSpec 輕量迭代／SpecKit 企業治理／BMAD 多 Agent 協作 |
| 狀態追蹤（AI 說「做完了」到底算不算） | [[task-list-single-source-of-truth]] | `docs/task-list.md` 為唯一正本；🔴 **分開「已實作」與「已完成」兩級狀態**；完成報告用**證據格式**取代「做了什麼」空話 |
| 規格是否真的被落實 | [[obligations-as-code-tedium-is-stability]] | 每條義務接上**機器能判定**的檢查；驗證不可判定 → 整個循環空轉 |

**最關鍵的翻轉（來自義務即程式碼）**：
過去「逐條補齊十幾條隱形義務」的繁瑣是**出錯的來源**（人會累、會趕、會默默跳過）；交給 AI agent 後跑一遍清單成本趨近於零，**繁瑣就從出錯的來源翻轉成安定的來源**。這解釋了為什麼 spec-driven 在 AI 時代才真正可行——不是規格變重要了，是執行規格的邊際成本歸零了。

**對你的實際意義**：
知識庫本身正是這條鏈路的實例：`CLAUDE.md`（Layer 0 環境）＋ `rules/*.md`（規格層）＋ 各 SKILL.md（執行層）。**但目前多數規則仍是「AI 應該遵守的敘述」而非「機器可判定的檢查」**——2026-08-04 抓到的 inbox 掃描指令 bug 就是典型：協議寫對了，但沒有任何自動檢查會發現 SKILL.md 抄錯版本，靠的是月度健檢碰巧撞到。可考慮的下一步是把最關鍵的幾條規則（raw/ 唯讀、INDEX.md 同步、skill 三處同步）接上可執行驗證，而非只寫在文件裡。

**最大反例（仍然成立，且更精確）**：
規格本身必須正確——但現在可以說得更準：**危險的不是「規格錯」，是「規格錯且驗證也照著錯的規格判定」**。義務即程式碼的陷阱一節明確警告：若驗證步驟不可判定，整個 PEV 循環會空轉，而空轉的循環看起來跟正常運轉一樣。

---
*E 層深化日期：2026-08-04（前次 2026-06-05）*

<details>
<summary>E-2026-06-05（前次深化版，保存）</summary>

**核心洞察**：規格文件把模糊意圖鎖定為可跨 session 驗證的書面錨點，是 AI 開發從「66% 近似正確」到「可靠交付」的唯一路徑。

**依據**：
- [[zerospec]]：METR 2025——資深開發者在上下文不足的環境使用 AI 反而慢了 19%；Simon Willison：補對上下文比換模型更有效
- [[persistent-markdown-planning-skill]]：有 Skill vs 無 Skill 的 pass rate 96.7% vs 6.7%，差距 14.4×
- [[spec-driven-ai-workflow]]：SDD 四要素中「Out of Scope」最關鍵——AI 只知道 How，不知道 What 的邊界

**最大反例**：錯誤的規格讓 AI 完美執行錯誤任務（"perfect execution of wrong spec"）——比完全無規格更難發現，因為輸出在形式上完全正確。

</details>

<details>
<summary>E-original（原始版本，2026-06-05 前）</summary>

**規格文件是 AI 的「外部大腦」。** AI 開發失敗的根因不是模型能力不足，而是「上下文缺口」與「規格漂移」。四者定位構成一條完整鏈路：

```
Layer 0：ZeroSpec         ← 讓專案對 AI 可讀（架構邊界、版本真相）
Layer 1：spec-kit/OpenSpec ← 功能開工前鎖定範圍（What & Out of Scope）
Layer 2：planning-with-files ← 讓執行過程中的工作記憶持久化（顯式文件）
Layer 3：三檔案 Skill      ← 跨 Session 防漂移（task_plan/findings/progress）
```

</details>

---

> 🔴 **2026-09-03 重跑（7→9 篇）：E 層的「下半場」（機器可判定的驗證）現在有了機制論與原廠實作。**
>
> **① 為什麼規格會變成「換個地方存放的幻覺」——機制層答案**：[[enterprise-ai-four-layers-prompt-policy-tool-workflow]] 指出
> 🔑 **Prompt 描述「期待的行為」，不是「被保證的行為」**，而 🔴 **條件分支與重試寫在 Prompt 是期待，寫在 Workflow 才是保證**。
> **這正是本精粹 E 層的機制解釋**：規格若只存在文字層（Prompt／Markdown），它天生只能是期待；要成為保證，必須落到 Policy 或 Workflow 層。
>
> **② 原廠參考實作**：[[ai-native-sdlc-playbook-anthropic]]（Anthropic 官方）把「錨點接上驗證」做成產物鏈——
> `intent.md` → `spec.md` → `plan.md` → 程式碼 → 事故記錄**寫回** `intent.md`，🔑 而**提交鏈本身就是審計記錄**（誰提了什麼需求、Agent 產出什麼、誰批准）。
> 三個可直接抄的細節：
> - 🔑 **`plan.md` 的驗收標準＝「一個新人僅憑計畫就能實現」**——這是本精粹缺的「規格夠不夠好」的可操作判準
> - 🔑 **實現過程偏離計畫時，在同一個 commit 更新 `plan.md`**（與 [[task-list-single-source-of-truth]] 的唯一正本原則同源）
> - 每階段都有**先導／滯後指標**，例如設計階段的滯後指標是「**建置開始後的需求返工次數**」——把「規格有沒有用」變成可量測
> - ⚠️ 素材只詳述 Plan／Design／Build，Test／Deploy／Maintain 被截斷
>
> **③ 治理落點與本精粹一致**：官方把治理放在 **Hooks 作為審批門禁**，而非寫進規格文字——再次印證「錨點必須接上機器可判定的檢查」。

## C（共識核心）
> 前四點 7/7 篇；第五點為 2026-08 新增

- **AI 失敗的根本原因是上下文/規格缺口，非模型能力不足**：66% 的 AI 錯誤來自上下文不完整（[[zerospec]] 引用 Stack Overflow 2025）；長期專案最大瓶頸是上下文遺失（[[planning-with-files]]）；目標漂移與錯誤遺忘是可工程化解決的問題（[[persistent-markdown-planning-skill]]）；需求沒講清楚 ＋ 缺乏工程紀律是兩大痛點（[[spec-driven-ai-workflow]]）

- **Markdown 是跨工具、跨 Session 的最佳持久化媒介**：人可讀、AI 可讀、可進 Git、零 runtime、模型升級不失效——七篇一致

- **先規劃、後實作是唯一正確順序**：規格/脈絡必須在 AI 動手前就位，不是邊做邊想

- **零基礎設施依賴**：全部採純 Markdown、不綁平台，不依賴向量資料庫或外部服務

- 🆕 **AI 的「完成」報告不可直接採信**（[[task-list-single-source-of-truth]]、[[obligations-as-code-tedium-is-stability]] 獨立同結論）：前者要求分開「已實作」與「已完成」兩級狀態、完成報告改用證據格式；後者要求每條義務接機器可判定檢查。兩篇都指向同一件事——**「AI 說做完了」是待驗證的宣稱，不是狀態變更**

---

## A（獨特視角）

| 來源 | 獨特貢獻 |
|------|---------|
| [[zerospec]] | **Layer 0 分層命題**：把「讓 AI 看懂專案」與「讓 AI 照流程執行」分離。四類最小必要上下文（架構邊界/版本真相來源/導航/文件治理）＋ 事件觸發式 Prompt Pack（不觸發就不建立文件，防文件膨脹）＋ DRIFT.md 偵測規格與程式碼的漂移 |
| [[planning-with-files]]（方法論） | **三種記憶方案互補定位**：Memory 伺服器（隱式）＋ planning-with-files（顯式文件）＋ CLAUDE.md（靜態配置）三者不衝突。「Context Hygiene」技巧：規格文件固化後清空 context 讓 AI 重讀，大幅降低幻覺 |
| [[persistent-markdown-planning-skill]] | **量化驗證**：有 Skill vs 無 Skill pass rate 96.7% vs 6.7%。「Context Window = RAM（易失），Filesystem = Disk（持久）」類比。2-Action Rule ＋ /plan-attest SHA-256 防篡改 |
| [[spec-driven-ai-workflow]] | **SDD 方法論**：規格四要素（功能範圍/驗證規則/API 設計/**Out of Scope**）。「不在範圍內」是防止 AI 腦補的關鍵。SDD 是 TDD 的前置步驟 |
| 🆕 [[ai-sdd-framework-comparison]] | **選型層（原 V 層缺口 4 的答案）**：四框架不是競品而是回答不同問題——ZeroSpec「AI 需要知道什麼」（環境基礎）／OpenSpec「功能做什麼、做完怎麼歸檔」（輕量迭代）／SpecKit「產品從構想到上線怎麼治理」（企業治理）／BMAD「多個 AI 角色怎麼協作」（多 Agent）。⚠️ BMAD 學習曲線高（19+ 角色 Agent），不適合個人快速迭代 |
| 🆕 [[task-list-single-source-of-truth]] | **狀態追蹤層**：`docs/task-list.md` 為唯一正本——即使 Slack／GitHub Issue／AI 完成報告記載不同，**正式狀態一律以此檔為準**。🔴 **分開「已實作」與「已完成」**（寫完程式 ≠ 驗證通過）；任務 ID 不可重複使用；**進行中限定 1 件**；完成報告用證據格式。⚠️ 個別任務的目的與完成條件**不該塞進 AGENTS.md/CLAUDE.md**——那裡只放「每次都要用到的規則」 |
| 🆕 [[obligations-as-code-tedium-is-stability]] | **驗證層**：一個「功能」水面下藏著固定、會反覆出現、**可被清點**的隱形義務（稽核/權限/機敏/外部/容錯/多台安全），本質是老概念「橫切關注」。既然可清點就能寫下來 →**義務即程式碼**；協作方式從「調 prompt」改成「**設計循環**」（PEV）；每條義務接**機器能判定**的檢查。⚠️ 文中「八個面向十幾條」只是給感覺不是聖旨，須依自家程式碼長出專屬那組 |
| 🆕 [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] | 🔑 **本精粹 E 層的機制解釋**：Prompt 描述期待、不是保證；四層分工（Prompt／Policy／Tool／Workflow），🔴 **條件分支與重試寫在 Prompt 是期待、寫在 Workflow 才是保證** |
| 🆕 [[ai-native-sdlc-playbook-anthropic]] | **唯一原廠實作**：產物鏈 `intent.md`→`spec.md`→`plan.md` ＋ **提交鏈即審計記錄**；🔑 `plan.md` 驗收標準「新人僅憑計畫就能實現」；🔑 偏離計畫時**同一個 commit 更新 plan.md**；每階段先導／滯後指標（「建置開始後的需求返工次數」）；治理由 **Hooks 當審批門禁** |

---

## V（知識缺口）
- ✅ **「規格夠不夠好」有了可操作判準（2026-09-03）**：[[ai-native-sdlc-playbook-anthropic]] 的「**新人僅憑計畫就能實現**」比本精粹原有的質性描述可用得多
- 🆕 🔴 **規格該落在哪一層，仍沒有分流規則**：[[enterprise-ai-four-layers-prompt-policy-tool-workflow]] 證明文字層只能承載「期待」，但**哪些規格條目該下沉到 Policy、哪些該下沉到 Workflow、哪些留在文字就夠，沒有任何來源給判準**
- 🆕 ⚠️ **原廠產物鏈是企業／團隊尺度**：官方那套（發起人／產品負責人／策略負責人分工＋git 審批）在個人專案上會過重，**而官方也沒說最小可用子集是什麼**

- ✅ **已補上（原缺口 4：Layer 0-3 銜接點）**：[[ai-sdd-framework-comparison]] 用「各框架回答什麼問題」給出選型判準
- ✅ **已補上（原缺口：規格是否被落實）**：[[obligations-as-code-tedium-is-stability]] 的「每條義務接可判定檢查」
- 🔶 **部分補上（原缺口 3：AI 主動更新規格的閉環）**：PEV 循環與 task-list 證據格式讓「實作完成後回頭確認」有了形式，**但仍是人／AI 觸發，不是自動反向更新規格**
- ⚠️ **文件維護成本**（仍開放）：七篇都強調文件先行，但「文件過時了怎辦」只有 ZeroSpec 的 DRIFT.md 有偵測機制；task-list 的正本制解決了狀態一致性，沒解決規格本身的新鮮度
- ⚠️ **複雜度閾值**（仍開放）：各框架都說「簡單任務不需要」，但沒有量化標準——幾個步驟、多少天開發週期、多少人協作才值得導入？
- 🆕 ⚠️ **不可判定的驗證會靜默空轉**：義務即程式碼明確警告，若驗證步驟不可判定，整個循環會空轉——而**空轉的循環看起來跟正常運轉一樣**。如何自動偵測「這條檢查其實不可判定」，無人給出方法
- 🆕 ❓ **系列未完結**：義務即程式碼三部曲預告第 4 篇談 harness，完整落地方法仍在連載——本精粹的驗證層論述可能需要再更新

---

## 整合行動清單

- [ ] 新專案起步先問「Layer 0 有了嗎？」——用 ZeroSpec INIT-SCAN ＋ INIT-BUILD 建立 AGENTS.md（30 分鐘內完成）
- [ ] 用「這個框架回答什麼問題」選型，別比功能表：環境基礎→ZeroSpec／輕量迭代→OpenSpec／企業治理→SpecKit／多 Agent→BMAD
- [ ] 每個新功能開工前，執行 `spec-kit.specify` 或 `/opsx:new → ff`，明確寫出 **Out of Scope**
- [ ] 🔴 **把「已實作」與「已完成」分成兩級狀態**，AI 報告「做完了」只能推進到前者；完成報告一律要求證據格式
- [ ] 🔴 **每條規則/義務都問「機器怎麼判定它成立」**——判定不了的先改寫成可判定，否則不要納入循環
- [ ] 多步驟任務（3+ 步驟）啟動 planning-with-files 三檔案模式，並維持「進行中限定 1 件」
- [ ] 對話 context 接近上限前主動執行「Context Hygiene」——規格固化為文件後 /clear 讓 AI 重讀
- [ ] 長期維護的專案每月跑一次 ZeroSpec DRIFT.md，確保規格未漂離程式碼

---

## 來源條目

- [[zerospec]] — Layer 0 Context Readiness：讓專案「可被 AI 讀懂」的零依賴 Markdown 框架（asset_value: 78）
- [[planning-with-files]] — 文件驅動 AI Coding Agent 方法論：需求/設計/計畫/進度全部 Markdown 化（asset_value: 60）
- [[persistent-markdown-planning-skill]] — Manus 風格三檔案持久化 Skill：96.7% pass rate，Context Window = RAM（asset_value: 75）
- [[spec-driven-ai-workflow]] — SDD 方法論 ＋ OpenSpec/spec-kit ＋ Superpowers 完整五階段實踐（asset_value: 68）
- [[ai-sdd-framework-comparison]] — 四框架選型：依「回答什麼問題」分工，非功能表比較（asset_value: 88）
- [[task-list-single-source-of-truth]] — task-list.md 唯一正本：分開已實作/已完成、進行中限 1 件、證據格式報告（asset_value: 82）
- [[obligations-as-code-tedium-is-stability]] — 義務即程式碼：隱形義務可清點、PEV 設計循環、繁瑣從出錯來源翻轉為安定來源（asset_value: 68）
- [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] — 🆕 🔑 Prompt 描述期待不是保證；規格要落到 Workflow 才是保證（AV 72）
- [[ai-native-sdlc-playbook-anthropic]] — 🆕 Anthropic 官方產物鏈與提交鏈即審計記錄（AV 82）

## 相關精粹

- [[synthesis-agentic-engineering]] — 角色轉型：從寫程式到設計循環，本篇的 PEV 是其具體機制
- [[synthesis-practitioner-skills-design]] — Skills 設計：「先證明勝過 baseline」與本篇「先問怎麼機器判定」是同一種驗證優先思維
- [[synthesis-ai-dev-workflow-frameworks]] — 框架輕重張力；本篇的選型層與其互補
