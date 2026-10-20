---
title: AI 開發工作流框架知識精粹：Superpowers vs gstack
date: 2026-05-14
source_count: 9
synthesis_snapshot_date: 2026-09-03
tags: [ai/agent, ai/claude]
source: synthesis
type: synthesis
status: processed
sources: 9 篇
asset_value: 87
---

整合 Superpowers（obra）、gstack（Garry Tan）、規格驅動工作流、Skills 生態，以及兩篇實戰／批判來源（Superpowers+gstack 搭配實戰、Superpowers 風險分級反思）共六篇，提煉 AI 輔助開發框架的核心選擇邏輯。這兩個框架看似競爭，實為互補——一個解決「怎麼做對」，另一個解決「做什麼值得做」；但框架不是越多越好，關鍵在依任務風險校準流程強度。

## E（精粹論點）

**AI 代理的真正瓶頸不是模型能力，而是工作流架構——但架構本身也需要依任務風險校準，而非統一套用。** Superpowers 和 gstack 從不同角度（紀律強制 vs 角色模擬）解決同一個問題：讓 AI 像資深工程師團隊而非實習生一樣工作。兩者的共同前提都是「先想清楚再動工」，差別只在你更需要「保證不寫廢 code」還是「從更高視角判斷值不值得寫」。最強組合是兩者並用：gstack 管戰略，Superpowers 管紀律（若需長任務穩定，可再加 GSD 當上下文錨點，形成「gstack 思考、GSD 穩定、Superpowers 執行」三層分工）。

**但強模型時代出現重要反例**：把 Superpowers 的硬門檻（「1% 可能就必須呼叫 skill」「任何專案都必須先設計」）無差別套用到所有任務，反而壓縮強模型的有效自由度、拖慢流程。正確的控制變數不是「有沒有呼叫某個 skill」，而是變更風險、失敗代價、是否存在知識盲區、是否有可執行驗證。因此完整結論應是：**先用流程架構補 AI 的紀律與戰略缺口，再依任務風險分級（Lite / Standard / Strict）決定套多重的儀式，兩者缺一不可。**

> 🔴 **2026-09-03 重跑（6→9 篇）：這個主題原本全是社群框架，現在有了原廠參考實作，也有了「架構為什麼是瓶頸」的機制層答案。**
>
> **① 🔑 原 E 層說「瓶頸不是模型能力而是工作流架構」，[[enterprise-ai-four-layers-prompt-policy-tool-workflow]] 說出了為什麼**：
> **Prompt 描述「期待的行為」，不是「被保證的行為」。** 企業 AI 要分四層——Prompt（描述期待）／Policy（不可違反的邊界）／Tool（執行能力）／Workflow（控制流程），而 🔴 **條件分支與重試寫在 Prompt 是期待，寫在 Workflow 才是保證**。
> 這把本精粹的「架構補紀律缺口」從方法論升級為機制論：Superpowers／gstack 之所以有效，是因為它們把東西從 Prompt 層搬到了 Workflow 層。
>
> **② 新增原廠參考實作**：[[ai-native-sdlc-playbook-anthropic]]（Anthropic 應用 AI 團隊官方）——🔑 **程式碼已不再是瓶頸，流程變成沙漏形（中間快、兩頭慢）**；主線是「**每階段結束寫入一個版本控制的產物，下一階段讀取它**」（`intent.md`→`spec.md`→`plan.md`→程式碼→事故記錄寫回 `intent.md`），而**提交鏈本身就是審計記錄**。
> ⚠️ **與本精粹既有結論的張力**：官方主張「加速兩頭以配合建置速度」，而 [[delivery-speed-vs-maintenance-cost]] 主張「merge 前先慢下來」——同一現象的相反處方，選哪個取決於瓶頸在交付還是在維護。
>
> **③ 戰略層多一個選項**：[[wayfinder-decision-map-workflow]]——把大專案攤成決策地圖、**先做決策再動工**，與 gstack 的「從更高視角判斷值不值得寫」同位但更輕。

## C（共識核心）
> 出現於 3-4 篇來源

- **先想清楚再動工是鐵律**：Superpowers 的 `brainstorming`、gstack 的 `/office-hours`、spec-kit 的規格五步驟，都要求 AI 先產設計文件、使用者確認後才能動工（[[superpowers-framework]]、[[gstack-framework]]、[[spec-driven-ai-workflow]]）
- **流程結構解決 AI 偏離問題**：不是靠 prompt 說「請你認真一點」，而是靠強制流程讓 AI 無法跳過驗證步驟——Mandatory Workflow（superpowers）和 Sprint chain（gstack）是同一理念的不同實作（[[superpowers-framework]]、[[gstack-framework]]）
- **測試覆蓋是不可妥協的底線**：Superpowers TDD（RED-GREEN-REFACTOR）和 gstack `/ship` 自動覆蓋率審查 + `/qa` 真實瀏覽器測試，都把測試做為流程終點的門檻而非可選項（[[superpowers-framework]]、[[gstack-framework]]）
- **跨平台已是標配**：兩個框架都支援 8-10 個 AI 平台（Claude Code、Gemini CLI、Cursor、Codex CLI 等），說明 Skills 的跨平台標準化已成事實（[[superpowers-framework]]、[[gstack-framework]]、[[claude-skills-ecosystem]]）

## A（獨特視角）

| 來源 | 獨特貢獻 |
|------|---------|
| [[superpowers-framework]] | **Mandatory Workflow 概念**：技能自動觸發，使用者不需要記得用——框架幫你把紀律變成不得不走的路，不是建議而是強制。其他框架沒有這個設計哲學 |
| [[gstack-framework]] | **角色模擬 + 設計文件流**：每個 Skill 模擬具體角色（CEO/設計師/工程主管/Staff Engineer），且每個 Skill 的輸出自動餵入下一個，形成不落接的流水線。加上真實瀏覽器（GStack Browser）、GBrain 跨 session 記憶、10-15 並行 Sprint，以及 Garry Tan 自身 810× 生產效率的實證數據 |
| [[spec-driven-ai-workflow]] | **Context Hygiene 技巧**：需求固化為文件後清空當前 context，讓 AI 重讀文件再繼續——大幅降低長對話中的幻覺和忘記初衷。這是兩大框架都沒有明確說明的操作細節，但與兩者都相容 |
| [[claude-skills-ecosystem]] | **生態選擇策略**：「安裝越多越廢，選得準才是強」。入門組合：官方 1 個 + 明星框架 1 個 + 領域 1 個。gstack 和 superpowers 可以共存，但 claude-skills-ecosystem 寫作時 gstack 尚未進入清單（時間差）|
| [[superpowers-gstack-workflow]] | **唯一從源碼架構驗證互補性**並給出可操作接法：**5 個關鍵交接點**（brainstorming→/autoplan、writing-plans→/plan-eng-review、TDD→/qa、systematic-debugging→/investigate、finishing-branch→/ship）；**CLAUDE.md 分工宣告是防衝突關鍵**（不寫清楚，模糊指令會隨機匹配技能）；`--prefix` 解命名衝突；補上 **GSD 作為第三層「上下文穩定器」**，形成「gstack 思考／GSD 穩定／Superpowers 執行」 |
| [[superpowers-skill-bloat-risk-tiering]] | **唯一批判視角**（日均 3 億 token 重度使用者實測後拆掉 Superpowers）：問題不是模型被拖慢而是**流程被拖慢**；**Lite/Standard/Strict 三級分工**的具體判準；**子代理成本模型**（同一需求被主代理/實作/規格審查/品質審查各讀一次）；實測 14 skill → 6 個、SKILL.md 3207 行 → 110 行 |
| 🆕 [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] | 🔑 **本精粹「架構才是瓶頸」的機制層依據**：**Prompt 描述期待、不是保證**；四層分工（Prompt／Policy／Tool／Workflow），🔴 條件分支與重試**寫在 Prompt 是期待、寫在 Workflow 才是保證** |
| 🆕 [[ai-native-sdlc-playbook-anthropic]] | **唯一原廠參考實作**（Anthropic 官方）：🔑 程式碼不再是瓶頸、**流程呈沙漏形**；產物鏈 `intent.md`→`spec.md`→`plan.md` 且**提交鏈即審計記錄**；每階段附先導／滯後指標（如「建置開始後的需求返工次數」）；`plan.md` 驗收標準＝**新人僅憑計畫就能實現**。⚠️ 素材只詳述 Plan/Design/Build |
| 🆕 [[wayfinder-decision-map-workflow]] | **輕量戰略層**：把大專案攤成決策地圖，先做決策再動工——與 gstack 同位但儀式更少 |

## V（知識缺口）
- 🆕 ⚠️ **原廠與社群框架的取捨無人比較（2026-09-03）**：[[ai-native-sdlc-playbook-anthropic]] 是官方版本，Superpowers／gstack 是社群版本，**兩者的產物鏈與門檻設計沒有任何來源做過對照**——尤其官方那套是企業／團隊尺度，個人專案照搬會過重
- 🆕 🔴 **「加速兩頭」vs「先慢下來」的處方衝突未解**：本精粹主張用流程架構補紀律缺口，官方主張把規劃與評審也加速，而 [[delivery-speed-vs-maintenance-cost]] 用五個月帳單主張 merge 前先慢下來。**三者不能同時最大化，但沒有來源討論如何依情境選擇**

- ⚠️ **「該不該用框架」本身出現正反分歧**：四篇教你怎麼用得更好，[[superpowers-skill-bloat-risk-tiering]] 卻主張強模型時代該把它拆掉——**分歧的真正焦點不是框架好壞，而是「硬門檻 vs 風險分級」**；但沒有任何一篇給出「什麼模型強度以上就該降級儀式」的客觀判準
- ❓ **token 成本只有定性沒有定量**：[[superpowers-skill-bloat-risk-tiering]] 提出子代理成本模型（同一需求被重複讀取多次）是目前最接近的分析，但仍無實際數字；gstack 完整 Sprint vs Superpowers 七步驟誰更省，依舊無解
- ❓ **共存實踐已有解答但未驗證**：[[superpowers-gstack-workflow]] 補上了 CLAUDE.md 分工宣告與 5 交接點（原本的缺口），但這套接法只有單一來源、未見第二方複現；SessionStart hook 與 gstack 自動建議是否干擾仍未有人實測
- ⚠️ **gstack 的 Skill 數量說法不一致**：官方標題寫「23 個工具」，但 README 實際列出 30+ 個 Skill，加上 power tools 超過 35 個。各二手文章（含截圖）說法從 13 到 31 都有——需以官方 README 為準
- ❓ **非工程師使用情境**：兩個框架都假設使用者是工程師。對於 PM 或非技術創辦人想用 gstack 的情境，缺乏驗證
- ⚠️ **claude-skills-ecosystem.md 缺少 gstack**：該篇未收錄 gstack（58k+ stars），可能寫作時 gstack 尚未爆紅，INDEX 摘要已更新，但原文條目尚未補充

## 整合行動清單

**入門（先選一個跑）：**
- [ ] 新功能開發用 **gstack**：`/office-hours` → `/autoplan` → 實作 → `/review` → `/qa` → `/ship`
- [ ] 重視 code 品質 / 長時間自走 → 改用 **Superpowers**：`brainstorming` → `writing-plans` → `subagent-driven-development`

**進階（兩者共存）：**
- [ ] 用 gstack `/office-hours` 和 `/plan-*` 做戰略規劃，換到 Superpowers `writing-plans` + `subagent-driven-development` 執行
- [ ] 搭配 **spec-kit**（規格先行）+ **Context Hygiene**（固化需求後清空 context）降低長工作階段的幻覺風險

**選擇框架的決策樹：**
```
需要戰略視角（CEO/設計/工程師多角色審查）？    → gstack
需要確保 code 品質（TDD、不跳過測試）？         → Superpowers
需要瀏覽器自動化測試（UI/E2E）？               → gstack /qa + /browse
需要跨 session 記憶？                          → gstack GBrain
一人開發、想跑完整交付流程？                    → gstack
被 AI 直接衝去寫 code 氣過？                   → Superpowers
以上都要？                                     → 兩者共存
```

## 來源條目

- [[superpowers-framework]] — obra 的 Mandatory Workflow：讓 AI 開發有紀律的七步驟強制流程
- [[gstack-framework]] — Garry Tan 的虛擬工程團隊：23+ Skill 完整 Sprint，角色模擬 + 瀏覽器自動化
- [[spec-driven-ai-workflow]] — spec-kit + Superpowers 組合：規格先行解決「開工前沒想清楚」，Context Hygiene 技巧
- [[claude-skills-ecosystem]] — Skills 生態選擇策略：入門組合 + 三代安裝演進 + 選 Repo 四原則
- [[superpowers-gstack-workflow]] — 兩框架搭配實戰：5 交接點、CLAUDE.md 分工宣告、GSD 第三層穩定器（AV 80）
- [[superpowers-skill-bloat-risk-tiering]] — 批判反思：硬門檻壓縮強模型自由度，改用 Lite/Standard/Strict 風險分級（AV 70）
- [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] — 🆕 🔑 Prompt 描述期待不是保證；四層分工（AV 72）
- [[ai-native-sdlc-playbook-anthropic]] — 🆕 Anthropic 官方 SDLC：沙漏形瓶頸與產物鏈（AV 82）
- [[wayfinder-decision-map-workflow]] — 🆕 決策地圖：先做決策再動工（AV 78）
