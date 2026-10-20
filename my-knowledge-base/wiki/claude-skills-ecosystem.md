---
title: Claude Skills 生態全景：GitHub 16 個高 star Repo 深度盤點
date: 2026-05-14
tags: [ai/claude, ai/agent]
source: https://hackmd.io/@BASHCAT/Sya3Qlykfl
type: article
status: processed
asset_value: 80
---

2026 年 Claude Agent Skills 成為跨平台開放標準（Claude Code、Cursor、Gemini CLI、GitHub Copilot 皆採用），Skills 生態從個人分享演進為像 npm 套件一樣被管理。核心洞察：裝得越多越廢，選得準才是強——先選 3 個跑 1 週，再決定要不要加。

## T（核心論點）
Skills 生態到 2026 已不是「東西多 = 強」，而是「選得準 = 強」。安裝太多 Skill 會讓 Claude 在 marketplace 讀取 metadata 變慢、判斷該選哪個也消耗更多 token。理想策略：官方 1 個 + 明星框架 1 個 + 領域 1 個，跑 1 週再調整。

## B（適用場景）
**適用：** 想系統性選 Claude Skills/Plugins 的用戶；建立團隊 Skills 庫的工程師
**不適用：** 剛安裝 Claude Code 的新手（先用內建功能熟悉）

## R（四檔分類）

### 第一檔：官方原生型
| Repo | 特色 | 推薦 Skill |
|------|------|-----------|
| `anthropics/skills` (132k ⭐) | Apache 2.0，官方範例 | `pdf`, `docx`, `claude-api`, `skill-creator`, `webapp-testing` |
| `anthropics/knowledge-work-plugins` (12k ⭐) | 非工程師友好，11 個職能 plugin | sales, marketing, legal, finance 各角色 |

### 第二檔：明星框架（設計哲學值得學）
| Repo | 核心理念 | 代表 Skill |
|------|---------|-----------|
| `obra/superpowers` (150k+ ⭐) | TDD 紀律，「Write tests first, always」 | `brainstorming`, `test-driven-development`, `writing-plans` |
| `affaan-m/everything-claude-code` (~175k ⭐) | 整套 agent harness + 48 subagents + 185 skills | 全套工作流 |
| `JuliusBrussee/caveman` (57k ⭐) | 穴居人縮寫英文，壓縮 22-87% output | 省 token 時用，複雜推理時關掉 |
| `jarrodwatts/claude-hud` (22k ⭐) | 終端機 HUD 顯示 context 用量、subagent 狀態 | 每天跑 5+ subagent 的人必裝 |

### 第三檔：Awesome 集散地（當圖書館用，勿全裝）
| Repo | 量體 | 特色 |
|------|------|------|
| `ComposioHQ/awesome-claude-skills` (59k ⭐) | 1000+ | 78 個 SaaS app-automation skills |
| `hesreallyhim/awesome-claude-code` (43k ⭐) | 廣義 | 整個 Claude Code 生態中央目錄 |
| `sickn33/antigravity-awesome-skills` (37k ⭐) | 1450+ | `npx antigravity-awesome-skills --claude` 一鍵部署 |
| `wshobson/agents` (35k ⭐) | 185 agents + 153 skills | 拆成 80 個聚焦 plugin |

### 第四檔：領域利基
- `trailofbits/skills` (5k ⭐)：安全研究天花板，智能合約/程式碼審計/逆向工程
- `lackeyjb/playwright-skill`：Claude 自主寫並執行 Playwright 測試
- `thedotmack/claude-mem`：session 記憶壓縮 + 下次自動注入

## C（關鍵行動）
- [ ] **三代安裝演進**：Gen 1 git clone（個人私房 skill）→ Gen 2 `/plugin marketplace add`（主流）→ Gen 3 `gh skill install`（企業/生產環境，支援版本鎖定）
- [ ] **選 Repo 四原則**：先看官方 → GitHub star > 200 → SKILL.md 內容清晰 → 不要 require remote MCP server
- [ ] **「awesome list」≠「可安裝包」**：很多清單只是連結，看有無 `plugin.json` / `npx` 一鍵安裝才是真的能用
- [ ] **入門組合**：`anthropics/skills`（pdf/docx/skill-creator）+ `obra/superpowers`（brainstorming/TDD/writing-plans）+ `jarrodwatts/claude-hud`

## 相關連結
- [[claude-skills-guide]] — Skills 設計原理（能力包型 vs 軟編排型）
- [[claude-skills-deep-dive]] — Progressive Disclosure 機制 + Skills vs MCP 比較
- [[superpowers-framework]] — obra/superpowers 深度解析
- [[claude-code-commands-2026]] — 2026 命令演進與 Skills 統一架構
- [[agent-skills-vs-mcp]] — Skills vs MCP 選擇判斷

## 補充資料（來源：Claude Code Skills GitHub 收藏深度研究報告）

### Skills 生態從 2026 變元年的背景

Anthropic 2025 年 10 月正式發表 Agent Skills，2025 年 12 月把規格開源至 agentskills.io，正式成為跨平台標準。到 2026 年 4 月，已被 Claude Code、Claude.ai、Cursor、Gemini CLI、GitHub Copilot 採用。GitHub 在 2026-04-16 上線 `gh skill` 子指令——一個 CLI 工具跨所有 agent host 安裝、更新、發布 Skill。

### 安裝方式三代演進（更詳細版）

| 世代 | 指令 | 出現時間 | 特性 | 建議場景 |
|------|------|---------|------|---------|
| Gen 1 | `git clone … ~/.claude/skills/foo` | 2025 Q4 | 手工、無版本控制 | 自己寫的私房 Skill |
| Gen 2 | `/plugin marketplace add` + `/plugin install` | 2026 Q1 | Claude Code 原生 UI | **目前主流** |
| Gen 3 | `gh skill install owner/repo[@tag] --agent claude-code` | 2026 Q2 | 跨 host、版本鎖定、immutable releases | 企業/生產環境 |

作者建議：個人實驗用 Gen 2、團隊與生產環境用 Gen 3、寫 demo 才用 Gen 1。

### 六種角色推薦組合

| 角色 | 推薦 Skill 組合（3 個起跳）|
|------|--------------------------|
| 入門開發者 | `anthropics/skills`（pdf/docx/skill-creator）+ `obra/superpowers`（TDD/brainstorming）+ `jarrodwatts/claude-hud` |
| 進階全端工程師 | `obra/superpowers` 完整版 + `wshobson/agents`（語言對應 plugin）+ `context7` |
| 安全研究/AppSec | `trailofbits/skills`（全裝）+ `superpowers/systematic-debugging` + `anthropics/webapp-testing` |
| 行銷/成長 | `anthropics/knowledge-work-plugins/marketing` + `ComposioHQ` 的 SEO skills + `alirezarezvani/claude-skills` |
| 知識工作者/PM | `anthropics/knowledge-work-plugins/productivity` + `BehiSecc/awesome-claude-skills` + `thedotmack/claude-mem` |
| C-Level/經營者 | `alirezarezvani/claude-skills`（C-suite 34 skills）+ `anthropics/finance+legal` + `obra/superpowers/brainstorming` |

### 重要警告：「Skill 數量」≠「品質」

有研究直接指出「Not everything in these repos is installable. 1,916 skills does not mean 1,916 things you can `/plugin install`」。很多 awesome list 收錄的是指向其他 repo 的連結而非可安裝包。

判斷標準：有 `plugin.json` / `marketplace.json` / `npx` 一鍵安裝（antigravity、davila7 屬此類）才是真的能安裝；純連結清單（hesreallyhim 屬此類）當搜尋引擎用即可。

### 第三檔 Awesome 集散地（補充表格）

| Repo | Stars | 量體 | 特色 |
|------|-------|------|------|
| `ComposioHQ/awesome-claude-skills` | 59k | 1000+ | 78 個 SaaS app-automation skills |
| `hesreallyhim/awesome-claude-code` | 43k | 廣義 | 整個 Claude Code 生態中央目錄 |
| `sickn33/antigravity-awesome-skills` | 37k | 1450+ | `npx antigravity-awesome-skills --claude` 一鍵部署 |
| `wshobson/agents` | 35k | 185 agents + 153 skills | 拆成 80 個聚焦 plugin、25 個類別 |
| `davila7/claude-code-templates` | 27k | 模板 | `npx claude-code-templates@latest` 互動式選裝 |
| `VoltAgent/awesome-agent-skills` | 21k | 1100+ | 含 Google、Microsoft、Stripe、Cloudflare 等企業官方 skills |
| `alirezarezvani/claude-skills` | 14k | 235 | 唯一完整覆蓋 C-Level、財務、法務等管理面 |

### 實用 Cheat Sheet（常用安裝指令）

```bash
# 官方
/plugin marketplace add anthropics/skills

# 明星框架
/plugin marketplace add obra/superpowers-marketplace
/plugin marketplace add jarrodwatts/claude-hud
/plugin install claude-hud && /claude-hud:setup

# 集散地（互動式）
npx antigravity-awesome-skills --claude        # 1450+ skills
npx claude-code-templates@latest               # davila7 模板

# 安全研究
/plugin marketplace add trailofbits/skills

# 2026 Q2 新標準
gh skill search pptx
gh skill install anthropics/skills skills/pptx@latest --agent claude-code --scope user
gh skill update --all
```

## 補充資料（來源：get-zeked/research-knowledge-super-skill）

### Research & Knowledge Super-Skill：知識工作者導向的大型研究分析 Skill 套件

整合 Perplexity Computer 研究數據技能與 Claude Code 知識管理技能的合集 Skill，定位與 [[agent-skills-toolkit]]（工程開發生命週期導向）不同，專為深度研究需求設計。

**整合來源：**
- Perplexity Computer 系列：research-assistant、data-exploration、data-validation、data-visualization、statistical-analysis
- Claude Code 系列：Tapestry（知識圖譜）、Article Extractor、YouTube Transcript、Ship-Learn-Next、Content Creator、Brainstorming、Prompt Engineer/RAG

**涵蓋的技能模組：**

| 模組 | 說明 |
|------|------|
| 研究方法論 | 深度研究工作流、並行 subagent 研究 |
| 知識圖譜 | Tapestry 實體/關係提取、多文件合成 |
| 內容提取 | 文章提取、YouTube 逐字稿、來源品質分層 |
| 資料探索 | 欄位分析、品質評分、模式發現 |
| 統計分析 | 假設檢定、異常值偵測、趨勢分析 |
| 資料視覺化 | 圖表選型、Python 模板、無障礙設計 |
| 資料驗證 | 24 項交付前清單、7 個常見陷阱 |
| 腦力激盪 | Hard-gate 紀律、橫向思考技法 |
| RAG 系統 | 完整 pipeline、評估指標、失敗模式 |
| 迭代學習 | Ship-Learn-Next 框架 |

**安裝方式：** 上傳 `SKILL.md` 到 Perplexity Computer 用戶設定，或放入 Claude Code skills 目錄。

## 補充資料（來源：K-Dense-AI/scientific-agent-skills GitHub README，2026-05）

### Scientific Agent Skills：科學研究領域天花板 Skill 套件

K-Dense-AI 開源的 **135 個科學研究 Skills**（MIT 授權），覆蓋生物資訊、藥物發現、臨床研究、機器學習、物理學、天文學等全領域，遵循 agentskills.io 開放標準，相容 Claude Code、Cursor、Codex、Gemini CLI。

**核心數字**：
- 135 Skills / 100+ 資料庫（78 個統一 Database Lookup + 獨立 skills + BioServices/BioPython/gget 多資料庫套件）
- 70+ 優化 Python 套件 Skills（RDKit、Scanpy、PyTorch Lightning、scikit-learn、OpenMM 等）
- 支援領域：癌症基因組學、藥物靶點結合、分子動力學、RNA velocity、地理空間科學、時序預測等

**安裝方式**：
```bash
npx skills add K-Dense-AI/scientific-agent-skills          # 官方標準
gh skill install K-Dense-AI/scientific-agent-skills        # 互動式選裝
gh skill install K-Dense-AI/scientific-agent-skills scanpy  # 指定單一 skill
```

**配套工具**：K-Dense BYOK（免費開源 AI 共同科學家桌面工具，40+ 模型，全量 135 skills，資料留本機）

**安全掃描**：使用 Cisco AI Defense Skill Scanner 每週掃描，提供 `skill-scanner scan /path/to/skill --use-behavioral` 本地掃描指令。

| Skill 分類 | 代表 Skills |
|-----------|------------|
| 生物資訊 / 基因組（21+）| BioPython、Scanpy、PyDESeq2、scVelo（RNA velocity）|
| 化學資訊 / 藥物發現（10+）| RDKit、DeepChem、DiffDock（分子對接）、OpenMM |
| 臨床研究（8+）| 統一 Database Lookup（ClinVar、COSMIC、ClinicalTrials.gov）、DepMap |
| 機器學習（16+）| PyTorch Lightning、PyMC、TimesFM（Google 零樣本時序預測）|
| 地理空間（2）| GeoPandas、GeoMaster（500+ 範例） |
| 科學溝通（20+）| 文獻查詢、科學寫作、LaTeX 海報、Mermaid 圖 |
| 研究方法論（12+）| 假設生成、批判思考、What-If Oracle、grant writing |
