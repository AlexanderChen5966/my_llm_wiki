---
title: Wiki 健康檢查報告 — 2026-07-20
date: 2026-07-20
tags: [pkm/workflow, type/reference]
type: reference
status: processed
---

# Wiki 健康檢查報告 — 2026-07-20

## 整體評分
- wiki 條目數：322（含 synthesis 10 / concepts 5）
- research 條目：38｜ideas 條目：67
- raw 素材：.md 576（已處理 564 / 未處理 12）｜非 .md 未處理 0
- 問題總數：實質 3 類（嚴重 0 / 警告 3 / 建議 2）
- **健康指數：良好**（斷鏈/矛盾/來源三大品質維度基本乾淨，主要是「連結網密度」與「新進積壓」兩項待補）

---

## 🔴 嚴重問題（需立即處理）
**無。** 已於本次檢查當場修掉唯一的實質矛盾：

- ✅ **已修**：`harness-engineering.md:123` 相關連結殘留舊四層順序（Prompt→Context→Loop→Harness），是先前順序修正時漏改的殘留，已改為正確的 **Prompt→Context→Harness→Loop**。

---

## 🟡 警告（建議本週處理）

### 1. 孤立筆記 38 篇（0 入連結，僅能從 INDEX 找到）
近期 Batch 27–31 快速新增的條目大多「只有向外連結、沒有被既有條目連回」，導致在 wikilink 圖上孤立。代表性清單：
`chatgpt-skills-usage-guide` / `claude-code-codex-sandbox-h5i` / `enterprise-rag-smb-architecture` / `fde-frontend-ai-career-transition` / `free-ai-courses-6-platforms-2026` / `harness-engineering-vue3-admin-practice` / `http-query-method-rfc10008` / `kotlin-toolchain-amper-merger` / `llm-concepts-visual-cards-1-60` / `llm-wiki-code-as-source-of-truth-case-study` / `superpowers-skill-bloat-risk-tiering` / `system-prompts-leaks-repo` / `yc-designer-claude-curation-workflow` / `ai-agent-reverse-workflow-decomposition` / `blind-spot-check-prompt-technique` / `claude-2026-beginner-guide` / `claude-code-prompt-library-52` / `chatgpt-blender-mcp-3d-dashboard-case-study` / `codex-computer-use-modes` / `hung-yi-lee-skill` / `paper-radar-literature-monitor` / `warp-agentic-dev-environment` / `notion-ai-workspace-analysis`…（共 38 篇）
> 註：`wiki/b2b-manager/tasks.md`、`troubleshooting.md` 為專案文件非知識條目，已排除。
> **建議**：不需逐篇補；優先為高價值的孤立條目，從既有相關條目補 1-2 個回連（例如 `enterprise-rag-smb-architecture` ← `rag-retrieval-augmented-generation`；`llm-concepts-visual-cards-1-60` ← `llm-how-it-works-no-math`）。

### 2. 未處理素材 12 篇（今日 2026-07-20 新剪藏）
- `看這篇就懂：Claude Code 完全攻略指南`
- `Claude Code Auto Mode 完整教學：4 步驟啟用智慧權限`
- `Claude Workflows 是什麼？2026 研究預覽與 Plan Mode、Auto Mode 差在哪`
- `一人用AI產出翻10倍…Claude Code創始人揭「AI採用5階地圖」`
- `ChatGPT深度研究怎麼用？2026最新 Deep Research 5步驟操作與提示詞`
- `Codex 做自媒體 必備十大 Skill`
- `code-review-graph`（GitHub README + 中文 ×2，共 3 份 → 同工具需合併）
- `farion1231/cc-switch`（多 CLI 帳號切換工具）
- `realiti4/claude-swap`（Claude 帳號切換+限額輪替）
- `Voicebox`（本機語音複製/聽寫/Agent 語音輸出）
> **建議**：執行 raw-pipeline 處理（已達門檻）。注意 code-review-graph 三份合併、cc-switch 與 claude-swap 是兩個不同的帳號切換工具需區分。

### 3. 重複表格：implementation-patterns 與 playbook-cli-tools
`loop-engineering-implementation-patterns.md:152-191` 含「cobusgreyling/loop-engineering CLI（loop-audit/init/cost）+ 7 個生產模式表格」，與 `loop-engineering-playbook-cli-tools.md` 整篇主體實質重疊（同 3 CLI + 同 7 模式 + L1/L2/L3）。
> **判斷**：兩篇主軸不同（implementation 主打 CLAUDE.md+Stop hook+fixer 三件套；playbook 主打 Tier A/B/C runtime 選型），不建議合併，建議**把 implementation-patterns 尾端 CLI/7 模式段落濃縮成一兩句 + 指向 playbook**（playbook 為該內容權威篇），消除重複表格。

---

## 🟢 建議（下次處理）

### 1. 兩個「4 要素框架」條目分工偏薄
`chatgpt-prompt-4-techniques`（Task/Context/Output/Iteration）與 `gpt-5-6-prompting-4-elements-framework`（目標/背景/輸出/邊界）欄位幾乎一一對應、核心心法相同。目前已互連且 chatgpt 篇標對方為「進階版」，方向對但區分偏薄。
> **建議**：兩篇開頭各補一句「與 X 篇的差異在於…」（chatgpt 篇＝新手通用對話+Iteration 迭代心法；gpt-5-6 篇＝綁定 GPT-5.6 Sol「別微管理強模型」），非必要，不需合併。

### 2. 過時版本號 9 篇（低優先）
wiki 有 9 篇提及較舊版本號（Opus 4.x / GPT-5.x / Gemini 1-2），抽查多為正確的歷史記述，非必然過時，暫不需動。

---

## 品質維度確認（本次驗證為乾淨，無須行動）
- **斷鏈**：✅ 全庫掃描 0 真斷鏈（曾標記的 8 個全為刻意示例 `[[wikilink]]`/`[[Previous Analysis]]`、skill 參照 `[[wiki-health-check]]`、TOML 語法 `[[skills.config]]`）
- **缺來源**：✅ 8 篇 source 空白者皆為 synthesis/concepts 型（多來源整合，設計上不放單一 source）
- **矛盾**：✅ 除已修的 1 處，四層順序、迭代上限、Worker/Verifier、AGENTS.md 載入規則、OfficeCLI 兩專案定位等跨篇描述皆一致
- **重複主題**：多數為刻意的角度分層（Loop 五篇不同深度、AGENTS/CLAUDE/SKILL 各對應不同檔、兩篇 bnext 新手總覽為索引 vs 教學互補），僅上述 1 組表格重複需處理
- **幽靈標記**：機械偵測在此規模（156 個 .md 標記、大量刻意排除的離題文）誤報過高，不適合量化；抽查未見異常，改人工按需檢查

---

## 優先處理清單（2026-07-20 全數處理完成）
1. ✅ **raw-pipeline 已處理 12 篇新剪藏**（Batch 32）→ 8 wiki + 2 ideas；code-review-graph 三份合併為單一條目（數據以 GitHub README 為準並註明中文版出入）、cc-switch 與 claude-swap 分建兩篇互連並標「不同專案」。indexer 已同步為新條目補 4 個回連（codegraph-token-optimization→code-review-graph、ai-usage-five-levels→ai-adoption-five-stages、claude-code-dynamic-workflows→three-modes+auto-mode、claude-code-billing-usage→claude-swap）避免再製造孤立。
2. ✅ **implementation-patterns 重複表格已瘦身** → CLI/7 模式段落濃縮成一段 + 指向 [[loop-engineering-playbook-cli-tools]]（權威篇），並補雙向互連。
3. ✅ **孤立條目已補回連**（7 篇高價值：enterprise-rag-smb-architecture / llm-concepts-visual-cards-1-60 / harness-engineering-vue3-admin-practice / fde-frontend-ai-career-transition / claude-code-codex-sandbox-h5i / chatgpt-skills-usage-guide / claude-code-prompt-library-52），孤立數 38→31；本批新條目也已補回連（見第 1 項）。
4. ✅ **chatgpt vs gpt-5-6 分工說明已補**（兩篇開頭各加一句差異：新手通用+Iteration vs GPT-5.6 Sol+Boundaries）。

> **處理後新增待辦**：處理期間又新剪藏進 1 篇（`Claude 怎麼用？…犬哥網站`，2026-07-20），疑與 Claude 入門飽和群重疊，待人工判定 build/ideas/排除。

---

## 🔄 5A+ 階段評估

**當前階段：ACQUIRE（輕微）+ APPLY**

理由：品質維度（矛盾/斷鏈/來源）健康良好且矛盾已即時修掉，知識庫運作順暢（偏 APPLY）；但今日又新進 12 篇未處理素材（含 3 份需合併），且 38 篇近期條目尚未織入連結網，屬輕度 ACQUIRE——收集速度略快於「整合進圖譜」的速度。

下一步：先跑 raw-pipeline 消化 12 篇（回到 backlog=0），再花一輪為近期孤立條目補回連，讓新知識真正織進 wikilink 圖譜而非只掛在 INDEX，即可穩定回到 APPLY。
