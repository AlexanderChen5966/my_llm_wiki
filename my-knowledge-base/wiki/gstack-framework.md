---
title: gstack：YC CEO 的全流程 AI 代理開發框架
date: 2026-05-14
tags: [ai/agent, ai/claude]
source: https://github.com/garrytan/gstack
type: article
status: processed
asset_value: 80
source_format: image
---

gstack 是 Y Combinator CEO Garry Tan 開源的 Claude Code 技能擴充包（58k+ stars），23+ 個 Skill 涵蓋從構思到發佈的完整 Sprint 流程。核心理念：把 AI 代理轉化為有完整分工的虛擬工程團隊——一個人也能以 810 倍速度出貨。支援 Claude Code、Codex CLI、Cursor、OpenCode、Factory Droid 等 10 個 AI 平台。

## T（核心論點）
「我已經有大概 60 天沒打一行 code 了」——Andrej Karpathy（2026 年 3 月）。Garry Tan 的回答是 gstack：2026 年生產效率達 2013 年的 **810 倍**（邏輯行數/天），年初至今已超過 2013 全年的 240 倍。重點不是誰打的，是出貨了什麼。gstack 是這個速度背後的工具棧。

## B（適用場景）
**目標用戶：** 獨立開發者、YC 創業者、全端工程師、技術主管、小型團隊
**適用：** 完整產品開發流程（從構思到發佈）；需要多角色審查的場景；10-15 個並行 Sprint
**不適用：** 一次性小任務；不需要戰略審查的 bug fix

## R（Sprint 流程 + Skill 清單）

### Sprint 流程
```
Think → Plan → Build → Review → Test → Ship → Reflect
```
每個 Skill 的輸出自動餵入下一個：`/office-hours` 寫設計文件 → `/plan-ceo-review` 讀取 → `/plan-eng-review` 寫測試計畫 → `/qa` 執行，環環相扣不漏接。

### 主要 Skill 清單

**Think（構思）**
| Skill | 角色 | 作用 |
|-------|------|------|
| `/office-hours` | **YC 辦公室問答** | **最重要的起點。** 六個強迫性問題，重新框架你的產品再寫 code。挑戰前提、生成三種實作方案、寫設計文件餵入後續所有 Skill |

**Plan（規劃）**
| Skill | 角色 | 作用 |
|-------|------|------|
| `/plan-ceo-review` | CEO / Founder | 找到需求裡隱藏的 10 星產品。四種模式：擴展 / 選擇性擴展 / 保持範圍 / 縮減 |
| `/plan-eng-review` | 工程主管 | 資料流、架構圖、邊界情境、測試矩陣，把隱藏假設攤開 |
| `/plan-design-review` | 資深設計師 | 各設計維度 0-10 評分，解釋什麼是 10 分，然後修改計畫達到 10 分。AI Slop 偵測 |
| `/plan-devex-review` | DX Lead | 開發者體驗審查：TTHW 基準、與競品比較、找出摩擦點（20-45 個強迫性問題）|
| `/autoplan` | 審查流水線 | **一個指令跑完 CEO → 設計 → 工程審查**，只把需要品味判斷的部分呈報你 |
| `/design-consultation` | 設計合夥人 | 從零建立完整設計系統，研究市場、提出創意風險、生成真實產品 mockup |

**Build（開發）**
| Skill | 作用 |
|-------|------|
| `/design-shotgun` | 產出 4-6 個 AI mockup 變體，開瀏覽器比較，迭代直到你滿意。Taste memory 學習你的偏好 |
| `/design-html` | 把核准的 mockup 轉為可出貨的 HTML/CSS（Pretext 動態排版，30KB、零依賴，偵測 React/Svelte/Vue）|

**Review（審查）**
| Skill | 角色 | 作用 |
|-------|------|------|
| `/review` | Staff Engineer | 找出通過 CI 但在生產環境爆炸的 Bug，自動修明顯的，標記完整性缺口 |
| `/investigate` | 除錯專員 | 系統性根因除錯。鐵律：沒調查清楚就不能修。3 次修法失敗就停下來 |
| `/design-review` | 設計師+工程師 | 審查後修正，atomic commit，前後截圖 |
| `/devex-review` | DX 測試員 | 實際走過 onboarding 流程，計時 TTHW，截圖錯誤，與 `/plan-devex-review` 分數比對 |
| `/cso` | 首席安全官 | OWASP Top 10 + STRIDE 威脅模型。17 個誤報排除，8/10+ 信心門檻，每個發現含具體攻擊情境 |
| `/codex` | 第二意見 | 獨立找 OpenAI Codex CLI 做 review，三種模式：審查 / 對抗式挑戰 / 開放諮詢。兩個 AI 交叉比對 |

**Test（測試）**
| Skill | 作用 |
|-------|------|
| `/qa` | 開真實 Chromium 瀏覽器、點擊、截圖、修 bug、生成 regression test、重新驗證 |
| `/qa-only` | 同 `/qa` 但只回報，不改 code |
| `/browse` | 給 agent 裝眼睛。真實 Chromium，~100ms/命令 |

**Ship（發佈）**
| Skill | 作用 |
|-------|------|
| `/ship` | 同步 main、跑測試、審查覆蓋率、push、開 PR。自動初始化測試框架 |
| `/land-and-deploy` | merge PR → 等 CI + deploy → 驗證生產環境健康。一個指令從「approved」到「verified in prod」|
| `/canary` | 發佈後監控迴圈，偵測 console errors、效能退化、頁面失效 |
| `/benchmark` | Page load / Core Web Vitals / 資源大小，每次 PR 前後對比 |
| `/document-release` | 自動更新所有文件（README / ARCHITECTURE / CLAUDE.md），`/ship` 會自動呼叫 |

**Reflect（回顧）**
| Skill | 作用 |
|-------|------|
| `/retro` | 每週工程回顧，含個人出貨 streak、測試健康趨勢。`/retro global` 跨所有 AI 工具彙整 |

**Power Tools（安全與控制）**
| Skill | 作用 |
|-------|------|
| `/careful` | 破壞性指令前警告（rm -rf、DROP TABLE、force-push）|
| `/freeze` | 鎖定只能編輯指定目錄，除錯時防止 Claude 亂改其他 code |
| `/guard` | `/careful` + `/freeze` 合一，生產環境最高安全模式 |
| `/learn` | 管理跨 session 記憶（審查、搜尋、修剪、匯出），gstack 會越用越了解你的 codebase |
| `/pair-agent` | 跨 AI 代理協作，共享瀏覽器（Claude Code + OpenClaw / Codex / Hermes 等同時看同一個網站）|

### GBrain — 跨 Session 記憶
```
/setup-gbrain  →  選擇：PGLite（本地）/ Supabase（雲端）
/sync-gbrain   →  重新索引 codebase，更新 CLAUDE.md 搜尋指引
```
gstack 可把 CEO 計畫、設計文件、retro、開發者偏好推送到 private git repo，記憶跨機器同步。

### 系統架構
```
Claude Code（Think / Plan / Build / Review / Test / Ship Skill 群組）
  └── SKILL.md 技能系統（Markdown + YAML frontmatter）
        ├── GStack Browser / Browse Server
        │     ├── Playwright / Bun + Chromium（anti-bot stealth）
        │     ├── Tab Manager、Cookie Import、Screenshot
        │     ├── Sidebar Agent（自然語言控制瀏覽器）
        │     └── Prompt Injection Defense（22MB ML 分類器 + Haiku 審查）
        ├── Shell 工具鏈 bin/
        │     ├── gstack-config、gstack-diff-scope
        │     └── gstack-model-benchmark（跨 Claude/GPT/Gemini 比較）
        └── GBrain（可選）：pgvector 向量資料庫，跨 session 持久記憶
```

## C（關鍵行動）

**安裝（30 秒）：**
```bash
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack && cd ~/.claude/skills/gstack && ./setup
```
前置：Claude Code + Git + Bun v1.0+（Windows 另需 Node.js）

**Quick Start（5 個指令感受價值）：**
```
1. /office-hours      → 描述你在做什麼
2. /plan-ceo-review   → 對任何功能想法做戰略審查
3. /review            → 對任何有改動的分支做 code review
4. /qa <staging-url>  → 測試你的 staging 環境
5. 停在這裡 — 你會知道這適不適合你
```

**典型完整 Sprint：**
```
/office-hours → /autoplan → 實作 → /review → /qa → /ship → /retro
```

- [ ] gstack 偏重 **DevOps 全流程 + 多角色戰略審查**，適合完整產品開發
- [ ] vs superpowers：superpowers 偏重 **TDD 工程紀律**（Mandatory Workflow）；兩者互補——gstack 管流程，superpowers 管紀律
- [ ] **最大獨特優勢**：內建真實瀏覽器（GStack Browser）、`/cso` 安全審查、`/codex` 跨 AI 第二意見

## 補充視角：安裝與常見問題

來源：Leo Huang 安裝教學（leoaido.com），補充實際操作細節：

**Telemetry 注意：** gstack 預設在本地記錄 skill 使用情況（`~/.gstack/analytics/skill-usage.jsonl`），但不上傳。安裝時未主動同意則不外傳。確認指令：`cat ~/.gstack/analytics/skill-usage.jsonl`；如不放心可 `rm -rf ~/.gstack/analytics/`

**更新方式：** 在 Claude Code 輸入 `/gstack-upgrade`

**Windows 支援：** 用 Git Bash 或 WSL2 執行安裝指令，建議用 WSL2

**隔離性：** gstack 安裝在 `~/.claude/skills/gstack/`，不影響現有專案檔案與設定

**驗證安裝：** 開啟 Claude Code 輸入 `/qa`，有回應就代表安裝成功

**前置條件確認：** Claude Code + Git + Bun v1.0+（`curl -fsSL https://bun.sh/install | bash`）

## 補充視角：gstack 31 項技能六大分類（vocus 版完整解析）

來源：Josh（vocus.cc），按功能分類補充技能說明：

| 分類 | 代表技能 | 定位 |
|------|---------|------|
| 開發/部署/發佈 | `ship`、`land-and-deploy`、`canary`、`document-release` | 最後一哩路：安全推送到生產 |
| 規劃/決策/戰略 | `autoplan`、`plan-ceo-review`、`cso`、`plan-eng-review` | 模擬不同職位視角審核計畫 |
| 設計/成品/審查 | `design-html`、`design-consultation`、`design-shotgun` | 把設計想法轉化為可出貨 HTML |
| 品質/調查/守護 | `qa`、`investigate`、`guard` | 穩定性保證：測試→追蹤根因→守護分支 |
| 工具/基礎設施 | `browse`、`learn`、`freeze`/`unfreeze` | 給 AI 聯網視角 + 知識索引 + 凍結保護 |
| 營運/團隊協作 | `office-hours` | 模擬開放答疑、跨團隊協作 |

## 補充資料：v0.19+ 新增功能（來源：garrytan/gstack 官方 README）

### 新增 CLI 工具（二進制）

| 命令 | 功能 |
|------|------|
| `gstack-model-benchmark` | 同一 prompt 同時跑 Claude + GPT（Codex CLI）+ Gemini，比較延遲/tokens/費用/(可選) LLM judge 品質分 |
| `gstack-taste-update` | `/design-shotgun` 的選擇（通過/拒絕）寫入持久化 taste profile，每週衰減 5%，學習你的設計偏好 |

### `/pair-agent`：跨 AI 代理協作瀏覽器

兩個不同廠商的 AI（Claude Code + OpenClaw/Hermes/Codex）可透過 gstack 共享同一個 Chrome 瀏覽器視窗：
- 每個 Agent 有獨立 tab，互不干擾
- 若安裝 ngrok，自動啟動 tunnel 支援遠端 Agent
- Scoped tokens + 速率限制 + domain 限制 + 活動歸因

### 連續 Checkpoint 模式（opt-in）

```bash
gstack-config set checkpoint_mode continuous
```
Skills 自動以 `WIP:` 前綴 commit，記錄決策/剩餘工作/失敗嘗試。`/ship` 前自動 squash WIP commits，保持 bisect 整潔。

### 多平台支援（10 個 AI Coding Agent）

gstack 支援的 AI 平台（透過 `./setup --host <name>` 指定）：
OpenAI Codex CLI、OpenCode、Cursor、Factory Droid、Slate、Kiro、Hermes、GBrain，以及預設的 Claude Code。

### 10-15 個並行 Sprint 模式

搭配 [Conductor](https://conductor.build/) 可同時跑多個 Claude Code sessions（每個獨立 workspace）。Sprint 結構（Think→Plan→Build→Review→Test→Ship）使並行 Agent 知道何時停止、何時交出，避免混亂。

- `gstack.png`：整體定位概覽（「一個人就是一個軟件工廠」哲學 + 2026 年 Skills 生態數據）
- `gstack_flow.webp`：四大核心能力說明卡片
- `gstack_develop_flow.webp`：五階段開發流程圖（各階段對應 Skill 命令）
- `gstack_role_map.webp`：規劃/審查/QA/Ship 四相位角色地圖（截圖中的 13 個為核心規劃/審查 Skill；完整清單見本文）
- `gstack_system_architecture.webp`：技術架構圖

## 補充資料

來源：raw/ 圖片素材（2026-05-25 補充）

**gstack.png — 整體定位概覽**
NotebookLM 製作的 gstack 主題簡報，標題為「一個人就是一個軟件工廠：揭秘 Gstack 與一人軍團時代」，涵蓋 AI 角色化管理（15 個獨立角色）、純文本驅動的「嗜博管理學」、閉環自動化開發流程，以及 2026 年 3 月 AI 技能生態爆發性增長數據（garrytan/gstack 55,000 stars）。

**gstack_flow.webp — 四大核心能力說明卡片**
標示 20,844 Stars、13 Skills、48hr 破萬星、MIT 開源，由 Y Combinator CEO Garry Tan 開發，主打完整開發流程、瀏覽器自動化、多角色協作、一人抵一隊四大特色，適用對象為獨立開發者、YC 創業者、全端工程師與小型團隊。

**gstack_develop_flow.webp — 五階段開發流程圖**
展示從構思（Ideation）到設計（Design）、開發（Build）、審查+測試（Review & QA）、發佈（Ship）的五個階段，並標示各階段對應的 skill 指令（/plan-ceo-review、/plan-design-review、/plan-eng-review、/review、/qa、/ship 等），以持續迭代箭頭閉環。

**gstack_role_map.webp — 13 個 Skill 角色地圖**
依規劃（/plan-ceo-review、/plan-eng-review、/plan-design-review、/design-consultation）、審查（/review）、測試（/qa、/qa-only、/qa-design-review、/browse）、發佈（/ship、/retro、/document-release、/gstack-upgrade）四階段分組，清楚標示每個 skill 的職責定位。

**gstack_system_architecture.webp — 技術架構圖**
以 Claude Code 為頂層，下層為 SKILL.md 技能系統（Markdown + YAML frontmatter），分別驅動 Browse Server（Playwright/Bun，含 Chromium 引擎、Tab Manager、Cookie Import、截圖）與 Shell 工具鏈 bin/（gstack-config、gstack-diff-scope、gstack-update-check）。

## 📎 2026-10-19 補充：刪減約千行 Markdown 指引

來源：aiposthub（Philo）〈Garry Tan 刪減 gstack 上千行指引：AI 程式代理的規則該留多少？〉，2026-10-05，<https://www.aiposthub.com/gstack-garry-tan-trim-agent-instructions/>；一手貼文 <https://x.com/garrytan/status/2106298398441951497>（附修改畫面）。⚠️ 本段只記本次事件，上方 810 倍、58k+ stars 等舊數字維持原樣、未重新核對。

- **事件**：Garry Tan 在 X 表示刪掉 gstack 約 **1,000 行** Markdown 指引，理由是前沿模型進步後部分舊技巧已不需要——🔴 **Garry Tan 自述，媒體轉述；未提供 diff 或成效數據**
- 查詢當下貼文瀏覽數 > 4 萬（媒體轉述，查詢當下數字）
- 🔴 原文明說：**不能把刪減行數換算成成功率或費用改善**，貼文也沒有給出適用所有專案的最佳指引長度
- 刪減後 repo 仍保留不同角色與流程技能（官方 repo，媒體轉述）；aiposthub 延伸閱讀標題另提到 gstack 新增 `test-audit`，本篇正文未描述（媒體轉述，僅連結）
- 規則四分類（**作者觀點，未實測**）：

| 指引類型 | 作用 | 整理時檢查 |
|---|---|---|
| 專案名詞與架構 | 讓代理理解正在處理的系統 | 是否符合目前程式 |
| 驗收標準 | 定義怎樣算完成 | 是否能直接檢查 |
| 操作權限 | 限定可做的變更 | 是否清楚且可執行 |
| 工具操作技巧 | 補足使用方式 | 新版工具是否仍需要 |

  前三類是模型再強也不會自己知道的專案知識；第四類最容易隨模型進步過期
- 驗證法（**作者觀點，未實測**）：同模型、同環境比較精簡前後的完成品質、重試次數與人工接手原因；先清重複、失效路徑與互相矛盾的規則
- 對照：[[claude-md-optimization-techniques]]、[[synthesis/synthesis-claude-md]]、[[agents-md-guide]]（規則精簡議題）、[[claude-code-skill-scaling-hidden-costs]]（Skill 說明超出預算會被靜默截斷——規則變多的另一種代價）

## 相關連結
- [[superpowers-framework]] — obra 的 TDD 工程紀律框架（Mandatory Workflow，強調流程品質；與 gstack 互補）
- [[spec-driven-ai-workflow]] — 規格驅動工作流，可與 gstack /office-hours 搭配
- [[claude-skills-ecosystem]] — 更廣泛的 Skills 生態
- [[harness-engineering]] — AI 開發系統設計的更高層次視角
