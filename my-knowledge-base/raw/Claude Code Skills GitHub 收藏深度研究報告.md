---
title: "Claude Code Skills GitHub 收藏深度研究報告"
source: "https://hackmd.io/@BASHCAT/Sya3Qlykfl"
author:
published:
created: 2026-05-14
description: "研究主題：GitHub 上分享 Claude (Agent) Skills 的主要 repos 盤點，作為對外 blog 寫作素材"
tags:
  - "clippings"
status: processed
---
## 我把 4000+ Claude Code Skills 裝進終端機之後

說一個有點糗的真實場景。某個禮拜五晚上，我滑著 GitHub 看到 `sickn33/antigravity-awesome-skills` 寫著「installable GitHub library of 1,450+ agentic skills」，腦袋一熱， `npx antigravity-awesome-skills --claude` 一口氣全裝。隔天打開 Claude Code，光是讀取 marketplace 就卡了 5 秒，叫它做一個小重構，它居然先列出 17 個候選 skill 讓我選。

那一刻我才搞懂： **Claude Code 的 skill 生態到了 2026 已經不是「東西多 = 強」，而是「選得準 = 強」。**

這篇是我研究完官方規格、跑完 Anthropic、Trail of Bits、GitHub Changelog 的公告，又把 GitHub 上 16 個高 star 的 repo 都翻過 README 之後，整理給繁中讀者的一張「Skills 寶藏地圖」。我會用四檔分類把它們分清楚，附上可以直接 copy-paste 的 cheat sheet，最後用 6 種角色幫你決定先裝哪 3 個。

## Skills 是什麼？為什麼 2026 變元年

Anthropic 在 [2025 年 10 月 16 日的官方部落格](https://claude.com/blog/skills) 正式發表 Agent Skills，原文標題是 *Introducing Agent Skills* ，定位寫得很乾脆——「Claude 可以用 Skills 來改善它執行特定任務的表現」。當時還只是 Claude 自家功能。

兩個月後更進一步：2025 年 12 月 Anthropic 把規格 [agentskills.io](https://agentskills.io/home) 拿出來開源，正式變成跨平台「Agent Skills 開放標準」。The New Stack 把它比擬成另一個 [MCP 級別的標準布局](https://thenewstack.io/agent-skills-anthropics-next-bid-to-define-ai-standards/) ——Anthropic 先做事實標準、再開源、再讓整個工具鏈被綁進來。事後看，這個策略奏效得很快。

到 2026 年 4 月，這個標準已被 **Claude Code、 [Claude.ai](http://claude.ai/) 、Cursor、Gemini CLI、GitHub Copilot** 等多個 agent host 採用，並陸續擴散到 OpenAI Codex CLI、Antigravity、Windsurf 等工具（規格本身也維護在開放的 [agentskills/agentskills](https://github.com/agentskills/agentskills) repo 由社群貢獻——注意 `agentskills` 是獨立 org、不是 Anthropic 名下的 repo）。同時 GitHub 在 [2026-04-16 的 Changelog](https://github.blog/changelog/2026-04-16-manage-agent-skills-with-github-cli/) 上線 `gh skill` 子指令——一個 CLI 工具就能跨所有 agent host 安裝、更新、發布 skill。

換句話說：

> 2026 是「skills 變成像 npm 套件那樣被生態管理」的元年。

這也是為什麼 GitHub 上 awesome-claude-skills 這個關鍵字從 2025 年底開始爆炸長出十幾個 repo，每一個都號稱「1000+ skills」。

## 30 秒搞懂 SKILL.md 到底長什麼樣

別被「規格」兩個字嚇到。一個 skill 本質上只有一個資料夾跟一個檔案：

```
my-deployment-checklist/
└── SKILL.md
```

`SKILL.md` 最少只要兩個 YAML frontmatter 欄位就能跑：

```markdown
---
name: deployment-checklist
description: Pre-deployment validation for production releases
---

# Process

1. Run tests: \`npm test\`
2. Build: \`npm run build\`
3. Verify environment variables
4. Run database migrations
5. Document rollback plan

# Verification
- [ ] All tests passing
- [ ] Build successful
- [ ] Migrations applied
- [ ] Rollback documented
```

就這樣。Claude（或任何 agent host）會根據你的對話內容判斷 `description` 與當前任務的相關性， **只在需要時** 把完整的 [SKILL.md](http://skill.md/) 載進 context。這個「按需載入」的機制是它跟 system prompt、 [CLAUDE.md](http://claude.md/) 最大的差異——一個 system prompt 是「永遠存在的內隱規則」，一個 skill 是「該你上場時你才被叫出來」。

可選欄位也很節制： `author` 、 `version` 、 `license` 、 `user-invocable: true` （在 slash command 選單裡顯示）、 `context: fork` （在獨立 subagent 裡跑）。整套規格的設計哲學就是「越少越好」，方便讓不同工具廠商都能無痛實作。

放置位置只有兩種：

- **專案層** ： `<project>/.claude/skills/<name>/SKILL.md`
- **使用者層** ： `~/.claude/skills/<name>/SKILL.md`

知道這個之後，後面看 repo 就很容易——所有 repo 不管包裝得多花俏，最後都是把一堆 `SKILL.md` 丟進這兩個資料夾的其中一個。

## 安裝方式三代演進：從 git clone 到 gh skill

這半年安裝方式經歷了非常明顯的三代演進。如果你還在用第一代寫教學，那已經過時了。

```
#mermaid-1778728910229-e8ochs2ka{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#333;}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#mermaid-1778728910229-e8ochs2ka .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#mermaid-1778728910229-e8ochs2ka .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#mermaid-1778728910229-e8ochs2ka .error-icon{fill:#552222;}#mermaid-1778728910229-e8ochs2ka .error-text{fill:#552222;stroke:#552222;}#mermaid-1778728910229-e8ochs2ka .edge-thickness-normal{stroke-width:1px;}#mermaid-1778728910229-e8ochs2ka .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1778728910229-e8ochs2ka .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1778728910229-e8ochs2ka .edge-thickness-invisible{stroke-width:0;fill:none;}#mermaid-1778728910229-e8ochs2ka .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1778728910229-e8ochs2ka .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1778728910229-e8ochs2ka .marker{fill:#333333;stroke:#333333;}#mermaid-1778728910229-e8ochs2ka .marker.cross{stroke:#333333;}#mermaid-1778728910229-e8ochs2ka svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-1778728910229-e8ochs2ka p{margin:0;}#mermaid-1778728910229-e8ochs2ka .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#333;}#mermaid-1778728910229-e8ochs2ka .cluster-label text{fill:#333;}#mermaid-1778728910229-e8ochs2ka .cluster-label span{color:#333;}#mermaid-1778728910229-e8ochs2ka .cluster-label span p{background-color:transparent;}#mermaid-1778728910229-e8ochs2ka .label text,#mermaid-1778728910229-e8ochs2ka span{fill:#333;color:#333;}#mermaid-1778728910229-e8ochs2ka .node rect,#mermaid-1778728910229-e8ochs2ka .node circle,#mermaid-1778728910229-e8ochs2ka .node ellipse,#mermaid-1778728910229-e8ochs2ka .node polygon,#mermaid-1778728910229-e8ochs2ka .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#mermaid-1778728910229-e8ochs2ka .rough-node .label text,#mermaid-1778728910229-e8ochs2ka .node .label text,#mermaid-1778728910229-e8ochs2ka .image-shape .label,#mermaid-1778728910229-e8ochs2ka .icon-shape .label{text-anchor:middle;}#mermaid-1778728910229-e8ochs2ka .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#mermaid-1778728910229-e8ochs2ka .rough-node .label,#mermaid-1778728910229-e8ochs2ka .node .label,#mermaid-1778728910229-e8ochs2ka .image-shape .label,#mermaid-1778728910229-e8ochs2ka .icon-shape .label{text-align:center;}#mermaid-1778728910229-e8ochs2ka .node.clickable{cursor:pointer;}#mermaid-1778728910229-e8ochs2ka .root .anchor path{fill:#333333!important;stroke-width:0;stroke:#333333;}#mermaid-1778728910229-e8ochs2ka .arrowheadPath{fill:#333333;}#mermaid-1778728910229-e8ochs2ka .edgePath .path{stroke:#333333;stroke-width:2.0px;}#mermaid-1778728910229-e8ochs2ka .flowchart-link{stroke:#333333;fill:none;}#mermaid-1778728910229-e8ochs2ka .edgeLabel{background-color:rgba(232,232,232, 0.8);text-align:center;}#mermaid-1778728910229-e8ochs2ka .edgeLabel p{background-color:rgba(232,232,232, 0.8);}#mermaid-1778728910229-e8ochs2ka .edgeLabel rect{opacity:0.5;background-color:rgba(232,232,232, 0.8);fill:rgba(232,232,232, 0.8);}#mermaid-1778728910229-e8ochs2ka .labelBkg{background-color:rgba(232, 232, 232, 0.5);}#mermaid-1778728910229-e8ochs2ka .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#mermaid-1778728910229-e8ochs2ka .cluster text{fill:#333;}#mermaid-1778728910229-e8ochs2ka .cluster span{color:#333;}#mermaid-1778728910229-e8ochs2ka div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(80, 100%, 96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-1778728910229-e8ochs2ka .flowchartTitleText{text-anchor:middle;font-size:18px;fill:#333;}#mermaid-1778728910229-e8ochs2ka rect.text{fill:none;stroke-width:0;}#mermaid-1778728910229-e8ochs2ka .icon-shape,#mermaid-1778728910229-e8ochs2ka .image-shape{background-color:rgba(232,232,232, 0.8);text-align:center;}#mermaid-1778728910229-e8ochs2ka .icon-shape p,#mermaid-1778728910229-e8ochs2ka .image-shape p{background-color:rgba(232,232,232, 0.8);padding:2px;}#mermaid-1778728910229-e8ochs2ka .icon-shape rect,#mermaid-1778728910229-e8ochs2ka .image-shape rect{opacity:0.5;background-color:rgba(232,232,232, 0.8);fill:rgba(232,232,232, 0.8);}#mermaid-1778728910229-e8ochs2ka :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}Gen 1
2025 Q4
git cloneGen 2
2026 Q1
/plugin marketplaceGen 3
2026 Q2
gh skill CLI手動進 ~/.claude/skills/
易碎、難版本管理Claude Code 內建
marketplace 系統
支援 update / browseGitHub 官方 CLI
跨 agent host
支援 version pinning
```

三代差異一張表看完：

| 世代 | 指令 | 出現時間 | 特性 | 是否仍適用 |
| --- | --- | --- | --- | --- |
| Gen 1 | `git clone … ~/.claude/skills/foo` | 2025 Q4 | 手工、無版本控制、易碎 | 仍可，但僅適合「我自己寫的私房 skill」 |
| Gen 2 | `/plugin marketplace add owner/repo` + `/plugin install name@marketplace` | 2026 Q1 | Claude Code 原生 UI、可 browse | **目前最主流** |
| Gen 3 | `gh skill install owner/repo[/path][@tag] --agent claude-code` | 2026 Q2 | 跨 host、支援版本鎖定、immutable releases | 新專案首選 |

Gen 3 的 `gh skill` 是真的解決問題的。它允許版本釘選、 `gh skill publish` 還能自動啟用 GitHub [immutable releases](https://docs.github.com/repositories/releasing-projects-on-github/about-releases) 確保供應鏈完整性。對企業團隊來說，這是「終於可以把 skill 寫進採購清單」的關鍵。

我自己現在的習慣是： **個人實驗用 Gen 2、團隊與生產環境用 Gen 3、寫 demo 才用 Gen 1** 。

## 第一檔：官方原生型——安全感的起點

如果你完全不知道從哪開始，這檔不用糾結，直接裝。

### anthropics/skills

[anthropics/skills](https://github.com/anthropics/skills) （132k stars，2026-05 快照）是官方範例庫。你會在這裡找到 `pdf` 、 `docx` 、 `xlsx` 、 `pptx` （這幾個是 source-available 而非 MIT，注意授權）、 `mcp-builder` （教 Claude 建 MCP server）、 `artifacts-builder` 、 `webapp-testing` 、 `skill-creator` ，以及一個值得單獨提的 `claude-api` skill——它把整個 Anthropic API 與 Agent SDK 的最新規格、模型 ID、價格、beta header 全部包成一個 skill，是內部訊息流通最快的地方。

授權主體是 Apache 2.0，少數文件處理 skill 為 source-available。對企業合規很重要。

### anthropics/knowledge-work-plugins

[anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins) （12k stars）是 Anthropic 為「非工程師」設計的 plugin 集合，11 個 plugin 分別針對 sales、marketing、legal、finance、data、customer-support、product-management、enterprise-search、bio-research 等角色，預設綁好對應的 MCP 連接器（Slack、Notion、HubSpot、Snowflake、Databricks、BioRender、 [ClinicalTrials.gov](http://clinicaltrials.gov/) …）。

如果你的公司想推「全員 AI 化」，這是最容易上手的官方範本——直接拿 finance plugin 給會計、拿 legal plugin 給法務，連 prompt 都不用自己寫。

## 第二檔：明星框架——4 個值得 fork 的設計哲學

這檔不是「skills 多」，而是「對 agent 工作流的思考夠深」。看完 README 你會學到的不只是怎麼用，而是怎麼設計自己的 skill。

### obra/superpowers — TDD 紀律的高山

[obra/superpowers](https://github.com/obra/superpowers) （150k+ stars，2026-05 持續快速增長中）由 Jesse Vincent 維護，是 2026 上半年 Claude Code 生態裡聲量最大的個人作品。它的 30+ skills 不是隨便堆砌，而是一整套有紀律的開發方法論：

- **Test-Driven** ： `test-driven-development` 、 `testing-anti-patterns` 、 `condition-based-waiting`
- **Debug 系統化** ： `systematic-debugging` 、 `root-cause-tracing` 、 `verification-before-completion`
- **規劃／審查** ： `brainstorming` 、 `writing-plans` 、 `executing-plans` 、 `subagent-driven-development` 、 `requesting-code-review` 、 `receiving-code-review`
- **Git workflow** ： `using-git-worktrees` 、 `finishing-a-development-branch`

它的核心信念寫得很直白：「Write tests first, always」「Systematic process over guesswork」「Simplicity is the primary design goal」。如果你曾經被 Claude「自信地寫了一個沒測試的 bug fix」氣到，這套 skill 是反制方案。安裝 `/plugin marketplace add obra/superpowers-marketplace` 。

### affaan-m/everything-claude-code — 把 harness 當作品

[affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) （~175k stars，2026-05 快照）不是 skill 收藏，而是一整個「agent harness 效能優化系統」。它包了 48 個 subagent、185+ skills、34 條 always-follow rules（含 TypeScript / Python / Go / Swift / PHP 變種）、14 個 MCP server 配置。

作者宣稱在開源前已私下使用多月迭代，repo 於 2026 年 1 月上 GitHub 後成長極快。對於想把 Claude Code 當成「我這個人的延伸大腦」的人，這是最接近現成「整套人格 + 工作流」的選擇。

### JuliusBrussee/caveman — 一個 skill 也能爆紅

[JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) （57k stars）證明「單一 skill 也能變現象級」。它做的事情很有趣：強迫 Claude 用「穴居人式」的縮略英文回答，repo 自報 10 個 prompt 測試下平均約 65% output 壓縮、範圍 22–87%，極端情境（解釋 React re-render）可達 87% 上限。

我自己跑過幾次，老實說對「機械式 coding」很有效（產 boilerplate、改變數名、寫測試），但對需要長鏈推理的任務（複雜演算法、跨檔案重構）會影響思考品質。Shareuhack 在 [2026 年的實測文章](https://www.shareuhack.com/en/posts/claude-code-community-skills-agent-fleet-guide-2026) 也提到，過度壓縮在需要 chain-of-thought 的複雜推理任務上可能拖累結果。所以—— **該省 token 時 caveman 上、該想清楚時關掉** 。

### jarrodwatts/claude-hud — 終於有人做儀表板

[jarrodwatts/claude-hud](https://github.com/jarrodwatts/claude-hud) （22k stars）是一個 HUD plugin，把 context 用量、active tools、subagent 狀態、todo 進度、git 分支用視覺化方式顯示在 Claude Code 的 terminal 邊上。

它解決的是一個「你不用打它也明白現在發生什麼」的痛點。對於每天會跑 5 個以上 subagent 並行任務的人，這是必裝。

## 第三檔：awesome 集散地——把 GitHub 當 NPM 用

這檔是「我需要時去翻」的圖書館型 repo。不要每個都裝，挑 1–2 個逛即可。

| Repo | Stars | 量體 | 特色 |
| --- | --- | --- | --- |
| [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) | 59k | 1000+ | 78 個透過 Composio MCP 連接 SaaS 的 app-automation skills |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | 43k | 廣義 | 整個 Claude Code 生態的中央目錄（含 hooks、slash commands、status lines） |
| [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills) | 37k | 1450+ | `npx antigravity-awesome-skills` 一鍵部署到指定工具 |
| [wshobson/agents](https://github.com/wshobson/agents) | 35k | 185 agents + 153 skills | 拆成 80 個聚焦 plugin、25 個類別 |
| [davila7/claude-code-templates](https://github.com/davila7/claude-code-templates) | 27k | 模板 | `npx claude-code-templates@latest` 互動式選裝； [aitmpl.com](http://aitmpl.com/) 有 web dashboard |
| [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) | 21k | 1100+ | 收錄 Google、Microsoft、Stripe、Cloudflare、Vercel、Netlify、Trail of Bits、Binance 等企業官方 skills |
| [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills) | 14k | 235 | **唯一** 完整覆蓋 C-Level、Regulatory & Quality Management、Finance 等管理面 skills |
| [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) | 12k | 中型 | 分類清楚、有明確的安全警語 |
| [BehiSecc/awesome-claude-skills](https://github.com/BehiSecc/awesome-claude-skills) | 9k | 135+ | 含「健康與生命科學」「學習與知識」冷門類別 |
| [karanb192/awesome-claude-skills](https://github.com/karanb192/awesome-claude-skills) | n/a | 50+ | 每個 skill 都標星等、註明 Source 與是否人工驗證過 |

要特別點名一個 2026 年最大的陷阱——別把「skill 數量」當品質指標。 `buildtolaunch.substack.com` 在 [一篇實測文](https://buildtolaunch.substack.com/p/best-claude-code-plugins-tested-review) 直接點破：「Not everything in these repos is installable. 1,916 skills does not mean 1,916 things you can `/plugin install`.」很多 awesome list 收錄的是「指向其他 repo 的連結」而非可即裝包。挑 repo 時請看：(a) 有沒有 `plugin.json` / `marketplace.json` / `npx` 一鍵安裝（sickn33 與 davila7 屬此類）；(b) 還是只是純連結清單（hesreallyhim 屬此類，當搜尋引擎用即可）。

## 第四檔：領域利基——一招打中的單點突破

這檔是「我只要解決這一件事」的 repo，通常 star 數不高但領域深度極強。

### trailofbits/skills — 安全研究的天花板

[trailofbits/skills](https://github.com/trailofbits/skills) （5k stars）是 Trail of Bits 這家全球頂級 security 顧問公司的官方 skill 集合，包含：

- **智能合約** ： `building-secure-contracts` 、 `entry-point-analyzer`
- **程式碼審計** ： `agentic-actions-auditor` 、 `c-review` 、 `differential-review` 、 `semgrep-rule-creator` 、 `static-analysis` （CodeQL + Semgrep + SARIF）、 `supply-chain-risk-auditor` 、 `variant-analysis`
- **驗證測試** ： `constant-time-analysis` （密碼學側通道）、 `mutation-testing` 、 `property-based-testing` 、 `zeroize-audit`
- **惡意軟體** ： `yara-authoring`
- **逆向工程** ： `dwarf-expert`
- **行動安全** ： `firebase-apk-scanner`

如果你做 AppSec、CTF、bug bounty，這是不裝白不裝的權威來源。安裝 `/plugin marketplace add trailofbits/skills` 。

### 其他值得收進工具箱

- [lackeyjb/playwright-skill](https://github.com/lackeyjb/playwright-skill) （2.2k stars）：Claude 自主寫並執行 Playwright 自動化測試
- [timescale/pg-aiguide](https://github.com/timescale/pg-aiguide) （1.7k stars）：PostgreSQL skill + MCP，讓 Claude 寫出更好的 SQL
- [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) ：把每個 session 的 context 用 AI 壓縮存起來、下次自動注入相關片段——解決「Claude 三天後就不認得我做過什麼」的記憶問題
- `context7` （官方 plugin）：即時抓取最新 library 文件，避開訓練資料過時

## 我的推薦組合：6 種角色，6 套搭配

我把上面所有 repo 重新組合成 6 個角色 starter pack。每組都是「3 個 skill / plugin 起跳」，不用第一天就裝滿。

### 入門開發者（剛裝 Claude Code 不到一週）

1. `anthropics/skills` — 安裝 pdf / docx / pptx / skill-creator
2. `obra/superpowers` — 至少裝 `test-driven-development` 、 `brainstorming` 、 `writing-plans`
3. `jarrodwatts/claude-hud` — 看清楚 context 用到哪

### 進階全端工程師

1. `obra/superpowers` 完整版
2. `wshobson/agents` 挑語言對應 plugin（python / typescript / kubernetes）
3. `context7` — docs 即時 lookup
4. （選） `JuliusBrussee/caveman` — 想省 token 時切換

### 安全研究 / AppSec

1. `trailofbits/skills` — 全裝
2. `obra/superpowers` 的 `systematic-debugging` 、 `root-cause-tracing`
3. `anthropics/skills/webapp-testing`

### 行銷 / 成長

1. `anthropics/knowledge-work-plugins/marketing`
2. `ComposioHQ/awesome-claude-skills` 的 SEO / CRO / 內容 skill（如 `claude-seo` ）
3. `alirezarezvani/claude-skills` 的 marketing pod（44 skills 分 7 個專業）

### 知識工作者 / PM

1. `anthropics/knowledge-work-plugins/productivity` + `product-management`
2. `BehiSecc/awesome-claude-skills` 中的 Linear / Kanban / Google Workspace skills
3. `thedotmack/claude-mem` — 長期專案必備

### C-Level / 經營者

1. `alirezarezvani/claude-skills` 的 C-Level pod（34 skills 覆蓋全 C-suite）
2. `anthropics/knowledge-work-plugins/finance` + `legal`
3. `obra/superpowers/brainstorming` + `writing-plans` — 把策略思考結構化

## 安裝 Cheat Sheet

```bash
# === 官方 marketplace ===
/plugin marketplace add anthropics/skills
/plugins                                          # 進 UI 瀏覽

# === 明星框架 ===
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers

/plugin marketplace add jarrodwatts/claude-hud
/plugin install claude-hud
/claude-hud:setup

# === 集散地（互動式選裝）===
npx antigravity-awesome-skills --claude          # 1450+ skills
npx claude-code-templates@latest                 # davila7 模板互動 CLI

# === 領域 ===
/plugin marketplace add trailofbits/skills        # 安全
/plugin marketplace add wshobson/agents          # 多 plugin 拆裝

# === 知識工作者 ===
claude plugin marketplace add anthropics/knowledge-work-plugins
claude plugin install sales@knowledge-work-plugins

# === 2026 Q2 新標準：gh CLI（跨 agent host）===
gh skill search pptx
gh skill install anthropics/skills skills/pptx@latest --agent claude-code --scope user
gh skill update --all
gh skill publish                                  # 你也可以發布自己的
```

## 中文社群觀察與寫作機會

研究完最大的感慨： **繁中圈幾乎沒有人在維護 awesome-claude-skills** 。我用「claude skills 繁體 awesome」「claude skills 中文 推薦」等各種組合搜尋，最後找到的多半是中國大陸 medium 個人作者的短文、Reddit 翻譯、HackMD 個人筆記。

這同時是壞消息也是好消息。

壞消息是台灣團隊容易卡在「英文教學」與「實際團隊導入」之間的鴻溝，特別是中型公司的非工程同事很難自己摸索安裝流程。好消息是——這是內容創作者的肥沃土壤。任何願意把官方 README 用繁中重寫一次、加上台灣工程文化 context（例如「老闆要我加班裝這個」「主管怕資安所以連 marketplace 都不准用」）的人，都會非常受歡迎。

我自己這篇就是一個小小的補白嘗試。如果你也想開始，我建議從「fork 一個英文 awesome list 翻譯成繁中」開始，比從零寫快得多。

## 結語：先選 3 個跑 1 週

寫完這篇我發現一個反直覺的結論—— **裝得越多越廢** 。

Skill 越多，Claude 在 marketplace 載入時讀的 metadata 越多、在做任務時也要花更多 token 去判斷該選哪個。我自己 2026 年 3 月那次大裝特裝之後，跑一個簡單 refactor 居然要 7 秒才開始輸出。後來砍到只留 12 個，速度立刻回來。

所以給你的具體建議是：

1. **先選 3 個** ：1 個官方範例、1 個明星框架（強烈推薦 obra/superpowers）、1 個對應你工作的領域 skill
2. **跑 1 週** ：故意挑日常會做的 5–10 個任務全程用這 3 個
3. **再回頭評估** ：哪個你按一次「不要用這個 skill」？砍掉。哪個你會主動 `@skill-name` 召喚？保留並深入。
4. **第二週再加 2 個** ：根據第一週的真實使用情境，從集散地 repo 挑兩個新的進來

這個節奏走 6 週，你會建立起一套真正屬於自己的工作流，而不是別人 README 的拼貼。

如果你覺得這篇有用，請把它分享給身邊還在 git clone skill 的朋友——順便提醒他升級到 `gh skill` 。然後告訴我你最後留下的 3 個 skill 是什麼，我很好奇繁中工程社群最終會收斂出什麼樣的組合。

## 延伸閱讀

- [Introducing Agent Skills (Anthropic 官方公告, 2025-10-16)](https://claude.com/blog/skills)
- [Agent Skills 開放標準 (agentskills.io)](https://agentskills.io/home)
- [Agent Skills 規格 repo (agentskills/agentskills)](https://github.com/agentskills/agentskills)
- [GitHub Changelog: Manage agent skills with GitHub CLI (2026-04-16)](https://github.blog/changelog/2026-04-16-manage-agent-skills-with-github-cli/)
- [gh skill 官方手冊](https://cli.github.com/manual/gh_skill)
- [Agent Skills: Anthropic's Next Bid to Define AI Standards (The New Stack)](https://thenewstack.io/agent-skills-anthropics-next-bid-to-define-ai-standards/)
- [Claude Agent Skills: A First Principles Deep Dive (Hanchung Lee)](https://leehanchung.github.io/blogs/2025/10/26/claude-skills-deep-dive/)
- [5 Claude Code Skills That Actually Work (Shareuhack 實測)](https://www.shareuhack.com/en/posts/claude-code-community-skills-agent-fleet-guide-2026)
- [Best Claude Code Plugins (2026): Tested Review (buildtolaunch)](https://buildtolaunch.substack.com/p/best-claude-code-plugins-tested-review)
- [Agent Skills: How to Extend AI Coding Agents (byteiota 2026 Guide)](https://byteiota.com/agent-skills-how-to-extend-ai-coding-agents-2026-guide/)

---

*本文研究與寫作於 2026-05-11 完成；所有 GitHub star 數為當日快照，可能已變動。歡迎在留言區補上你最愛的 repo——尤其歡迎繁中作者的作品。*