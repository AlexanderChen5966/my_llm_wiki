---
title: "Claude Design 上線 11 天後開源版上線：Open Design 與 Claude Design 完整用法指南"
source: "https://hackmd.io/@BASHCAT/Hy1tSFG-zg"
author:
published:
created: 2026-06-08
description: "一句話速覽：Anthropic 在 4/17 推出 Claude Design，把對話變成原型；11 天後 GitHub 多了一個叫 Open Design 的開源替代品（撰稿時 60.4K stars、6.5K forks），用你本機的 coding agent 跑同樣的工作流。AI 設計工具從「網頁產品」進化成「agent runtime 上的 skill 集合」，這場仗才剛開打。"
tags:
  - "clippings"
status: processed
---
> 一句話速覽：Anthropic 在 4/17 推出 Claude Design，把對話變成原型；11 天後 GitHub 多了一個叫 Open Design 的開源替代品（撰稿時 60.4K stars、6.5K forks），用你本機的 coding agent 跑同樣的工作流。AI 設計工具從「網頁產品」進化成「agent runtime 上的 skill 集合」，這場仗才剛開打。

## 為什麼這篇值得讀

我是硬韌體工程師，平常做 PCB、寫 firmware 比畫 UI 多。但這半年我發現一件事——當你想跟 PM 確認一個介面、或想把一個 idea 變成可看的 demo，動 Figma 拉元件比寫一個 dirty prototype 還慢。AI 設計工具終於在 2026 年成熟到「我這肝可以拿來偷懶」的程度。

但市場上突然冒出兩個方向南轅北轍的玩家—— **Claude Design** （Anthropic 雲端，4/17 推出）與 **Open Design** （開源 fork，4/28 推出，11 天後就出現）。一個說「打開網頁就用，零安裝」，一個說「用你電腦上已經在跑的 coding agent，資料完全本地」。

這篇我會講清楚：

- 兩個工具各自是什麼、能做什麼、怎麼用
- 完整對比表 + 我整理的選型決策樹
- 實際 prompt、安裝指令、踩坑點
- 為什麼 11 天就被 fork——背後是 AI 設計工具品類的本質轉變

文章基於 2026 年 6 月初的官方文件與第三方評測，但 Open Design 仍是 0.8.0-preview，數字可能週週變動，下手前還是回去查 [官方 repo](https://github.com/nexu-io/open-design) 比對。

---

## 一、Claude Design 是什麼

[Claude Design](https://www.anthropic.com/news/claude-design-anthropic-labs) 是 Anthropic Labs 在 **2026-04-17** 推出的視覺設計工具，網址 `claude.ai/design` ，由 Claude Opus 4.7（目前視覺最強的模型）驅動。

### 1.1 核心介面與工作流

兩個面板： **左聊天、右即時畫布** 。你用自然語言描述想要什麼，Claude 在右邊畫給你看。可以 inline 註解元素（直接點、直接打字）、可以直接編輯文字、有滑桿可以細調間距顏色版面。

官方文件強調的 5 步驟流程：

1. **品牌整合** ：系統會去讀你的 codebase 與設計檔，自動產出組織內的設計系統
2. **多源導入** ：純文字、圖片、DOCX/PPTX/XLSX、甚至整個網站擷取
3. **精細調整** ：inline 評論 + 直編 + 滑桿
4. **協作分享** ：組織內 review、編輯權限、群組對話
5. **匯出交接** ：URL、PDF、PPTX、Canva handoff、或直接 bundle 給 Claude Code 實作

### 1.2 能產出哪些東西

| 類型 | 成熟度 |
| --- | --- |
| 互動原型 / 線框圖 | ✅ 穩定 |
| 簡報 pitch deck | ✅ 穩定 |
| 登陸頁、社群素材 | ✅ 穩定 |
| 程式碼驅動原型（語音/影片/shader/3D/AI） | ⚠️ experimental |

[DataCamp 的整理](https://www.datacamp.com/blog/claude-design) 點出一句很關鍵的提醒——Anthropic 自己說，沒給設計系統時，產出會「functional but generic」。所以第一次用之前，先把你的 codebase 或設計檔丟給它。

### 1.3 訂閱與費用

- **包含於 Claude Pro / Max / Team / Enterprise** （Free 不能用）
- 用訂閱的 quota，看起來 **獨立於 Claude Code** ——Claude Code Max 額度與 Claude Design 是否互通官方沒明確說，建議到帳戶設定頁確認
- 配額按用戶每週重置，不是團隊共享池
- Enterprise 預設關閉，要 admin 進後台手動啟用

[Julian Goldie 的對比文](https://juliangoldieaiautomation.com/blog/open-design-vs-claude-design/) 提到，按「每月約 100 個設計」的用量走 API 估算，實際成本約 £200–500/月。這數字當 reference，實際帳單要看你的 workload。

### 1.4 真實限制（你會踩到的）

- **沒有 Figma 原生匯入匯出** ——這點 Anthropic 自己承認「不取代 Figma，僅補位」
- inline 評論會偶爾消失，變通方法是貼回 chat 當 prompt
- 大 codebase 整合會延遲，變通是只給子目錄
- **治理層面有洞** ：沒審計日誌、沒管理使用報告、沒資料駐留——金融、醫療、政府這種要 audit 的場合先別碰

### 1.5 一個能用的 prompt 範本

Claude Design 的 prompt 最好包含四要素： **目標 + 版面 + 內容 + 受眾** 。例如：

```
目標：給天使投資人看的 fintech app pitch deck
版面：直式投影片，10 張
內容：問題 → 解法 → 市場 → 商業模式 → 牽引力 → 團隊 → 募資
受眾：早期 VC，不是金融專業
```

實測這比「幫我做一個 pitch deck」會直接出三倍好的版本。Claude 有時會反問細節而不是直接畫——這是好事，別嫌它煩。

---

## 二、Open Design 是什麼

[Open Design](https://github.com/nexu-io/open-design) 出生於 **2026-04-28** ，距離 Claude Design 推出只有 11 天。Apache-2.0 授權，到撰稿時（2026-06-07）已經累積 **60.4K stars、6.5K forks** ——這數字對開源新專案來說，夭壽不正常。

### 2.1 核心理念：local-first + agent-native

三個關鍵字：

- **Local-first** ：所有資料、daemon、檔案都在你電腦的 `.od/` 資料夾。SQLite + filesystem，沒雲端
- **Agent-native** ：它 **不自帶模型** ，而是呼叫你電腦已經裝好的 coding agent CLI
- **零廠商鎖定** ：BYOK（自帶金鑰），任何 OpenAI / Anthropic / Azure / Google / Ollama / LM Studio 相容端點都吃

支援超過 21 個 coding agent，包括 Claude Code、Codex、Cursor、GitHub Copilot CLI、Gemini CLI、OpenCode、Qwen、DeepSeek、Kimi…等。切換 agent 是改一行 config，不是換工具。

### 2.2 預裝資源

| 類別 | 數量（撰稿時 2026-06-07） |
| --- | --- |
| Skills（可重用設計工作流） | 259+ |
| Design Systems（ [DESIGN.md](http://design.md/) 品牌規範） | 142+ |
| Plugins（marketplace 擴充） | 261+ |

預製的 design systems 涵蓋 Linear、Stripe、Vercel、Airbnb、Tesla、Notion、Anthropic、Apple、Cursor、Supabase、Figma、小紅書…等等。要做 Linear 風格的 dashboard？選 system 就好，不用自己調 token。

### 2.3 DESIGN.md：設計系統的核心發明

這是我覺得 Open Design 最聰明的設計。傳統 design token 都用 JSON 寫，Open Design 改成 **9 區段的 Markdown 檔** ：

```
1. color
2. typography
3. spacing
4. layout
5. components
6. motion
7. voice
8. brand
9. anti-patterns
```

優點： **人類讀得懂、版本控制 diff 友善、可以寫 anti-pattern** （這在 JSON token 裡幾乎沒人寫）。切換 system 等於換一份 [DESIGN.md](http://design.md/) ，下次 render 立即用新 token。

### 2.4 能產出哪些東西

```
✓ Web / mobile / desktop 原型（single-page HTML）
✓ Live dashboards（參數可即時編輯）
✓ 簡報（15 模板、36 主題）
✓ 圖片（多 provider）
✓ HyperFrames 影片（HTML → MP4，headless Chrome 渲染）
✓ 匯出：HTML / PDF / PPTX / MP4 / ZIP
```

**HyperFrames 是 Claude Design 沒有的殺手鐧** ——把 HTML 動畫渲染成 MP4，預設模板含產品揭示、動態排版、資料圖表動畫、Logo 片尾、app 展示、社群疊層。

### 2.5 安裝（兩種路線）

**桌面 app 路線** （推薦給只想用的人）：  
直接到 [opendesigner.io](https://opendesigner.io/) 下載，macOS（Apple Silicon / Intel）與 Windows x64 都有。

**原始碼路線** （推薦給想 fork / 改 skills 的人）：

```bash
git clone https://github.com/nexu-io/open-design.git
cd open-design
corepack enable && pnpm install
pnpm tools-dev run web
```

需求：Node ~24、pnpm 10.33.x。

**接你已裝好的 coding agent** ：

```bash
od mcp install claude-code     # 或 codex / cursor / gemini ...
```

### 2.6 架構速寫

```
┌──────────────────────────────┐
│  Web UI (Next.js 16)         │ ← 可部署 Vercel
├──────────────────────────────┤
│  Local Daemon (Express)      │
│  ├─ SQLite (.od/)            │
│  ├─ Project files (.od/...)  │
│  └─ BYOK Proxy (SSRF guard)  │
├──────────────────────────────┤
│  Coding Agent CLI (你選)      │
│  Claude Code / Codex / ...   │
└──────────────────────────────┘
       SSE 即時 stream
```

整個運算在本機跑，BYOK proxy 還做 SSRF 防護，這在 indie 開源專案裡算很有 sense。

### 2.7 真實限制（別忽略）

- 版本仍是 **preview 階段** （撰稿時 0.9 系列），API 與資料結構 **還在快速演變** ——下週可能就有 breaking change
- 不該當企業穩定平台用
- 需要自己備份 `.od/` 、自己管 API key
- 沒有成熟的協作 / 向量編輯——它 **不取代 Figma**
- 文件相對少（57K stars 但社群還在長）

---

## 三、完整對比與選型決策樹

### 3.1 全面對比表

| 維度 | Claude Design | Open Design |
| --- | --- | --- |
| **授權** | 專有（Anthropic） | Apache-2.0 |
| **定價** | Pro 起 $20/mo | 免費（吃既有 CLI 額度） |
| **典型成本** | £200–500/mo（同等量 API） | 自架 ~£100/mo |
| **部署** | Anthropic 雲端 | 本地優先；Web 可部 Vercel |
| **模型** | Opus 4.7 固定 | 任何 CLI / BYOK |
| **設計系統** | 自動讀 codebase | 150 預製 + [DESIGN.md](http://design.md/) 自訂 |
| **Skills** | 內建專有 | 100+ 可組合可擴充 |
| **匯出** | URL/PDF/PPTX/Canva/Claude Code bundle | HTML/PDF/PPTX/MP4/ZIP |
| **影片輸出** | experimental | HyperFrames（成熟） |
| **協作分享** | 強 | 弱（單機為主） |
| **資料駐留** | ❌ | ✅ 完全本地 |
| **審計治理** | ❌ | 看自架條件 |
| **入門門檻** | 開網頁就用 | 需 Node / CLI / agent 已裝 |
| **廠商鎖定** | 高 | 零 |
| **適合誰** | 非技術團隊 | 已用 coding agent 的開發者 |

### 3.2 我的選型決策樹

```
你需要 AI 設計工具嗎？
├─ 已付費用 Claude Code / Codex / Cursor
│   └─ 選 Open Design（邊際成本 ≈ 0）
│       ├─ 客戶機密 / 不能上雲？→ Open Design
│       └─ 需 Canva 給非技術夥伴？→ 兩者並用
│
└─ 沒訂閱任何 agent CLI
    └─ 選 Claude Design（最低技術門檻）
        ├─ 非技術背景？→ Claude Design
        └─ 願學 dev 環境？→ Open Design
```

### 3.3 三個真實場景

**場景 A：早期創辦人做投資人 pitch deck**

沒空、沒研發團隊、要的是「下週見投資人」。→ **Claude Design** 。 `claude.ai/design` ，丟 prompt，出來能用，Canva handoff 給設計實習生再修。

**場景 B：軟體外包公司的客戶機密案**

醫療 / 金融 / 政府單位的設計稿，contract 明寫不能上雲。→ **Open Design** 。本地跑、BYOK 接公司內部的 Azure OpenAI 端點、整份輸出留在 `.od/` 。

**場景 C：indie hacker / 個人開發者**

已經訂了 Claude Code Max，每月剩超多 quota。→ **Open Design** 邊際成本 0，配 Claude Code 直接出原型，要影片用 HyperFrames，省下另外訂 Claude Design 的 $20。

### 3.4 為什麼很多人「兩者並用」

[WotAI 的對比](https://wotai.co/blog/claude-design-vs-open-design) 推一種混合用法我覺得很合理：

- **早期探索與非技術協作** ：用 Claude Design——Canva handoff、inline 評論、組織分享，這些 Open Design 都還沒做好
- **切到 production / 整合 dev workflow** ：用 Open Design——本地、可版控、可塞進 CI、可鎖 seed 做 reproducible build

---

## 四、業界視角：AI 設計工具的本質正在改變

11 天就被 fork，這件事本身就值得拆一下。我認為背後不是「Anthropic 漏洞被抓」，而是 **AI 設計工具品類的定義正在被重寫** 。

### 4.1 從「產品」到「skill 集合」

舊 SaaS 設計工具：你買一個產品，它綁一個模型、一套 design system、一個雲端後端。Figma、Canva、Adobe XD 都是這個 mental model。

新 agent-native 設計工具：你有一個 **agent runtime** （Claude Code、Codex、Cursor 不重要），上面跑各種 **skills** （設計、簡報、影片、行銷文案），skills 可組合可換可 fork。

Claude Design 是「Anthropic 把這套 skill 包成 SaaS 賣」。Open Design 是「把同樣的 skill 包散出來，讓你跑在自家 CLI 上」。 **兩者在做的事一樣，差別只在誰擁有 runtime** 。

[Augment Code 的分析](https://www.augmentcode.com/learn/open-design-claude-design-alternative) 一句話總結很到位——Claude Design 沒提供的，就是「self-hosting、模型彈性、資料駐留控制」這三件事。剛好是企業客戶會在乎、但個人開發者第一天就能解決的。

### 4.2 對開發者的實際意義

如果你已經用 Claude Code 三個月以上，你會發現一個現象：你每天的「prompting 能力」其實是 transferable 的。同樣的 prompt 風格、同樣的「給 context 再下任務」習慣，從寫 code 換到畫 UI 幾乎無痛轉移。

這就是 agent runtime 的紅利—— **你的 prompt 直覺是資產，不是被特定 SaaS 鎖死的技能** 。

換句話說，Claude Design 跟 Open Design 在比的不是「誰的 AI 比較聰明」（都是同一個 Opus 4.7 或 user 選的模型），而是「誰更尊重你已經練出來的 agent prompting 直覺」。

### 4.3 接下來會發生什麼

我的賭注（純個人觀察，當事人視角）：

1. **Anthropic 會把 Claude Design 跟 Claude Code 更深度整合** ，最終可能合併計費 quota
2. **Open Design 會分裂成「桌面 app」與「CI 用 SDK」兩條路線** ——前者給設計師、後者給 platform team
3. **Figma 會把 AI 設計 agent 內建** ——用「我們有 design 領域知識」當差異化
4. 半年內會有第三派系出現： **領域特化的 AI 設計工具** （醫療 UX、車載 UI、IoT 控制面板專用）

如果這些預測有一半對，那今天學 Claude Design 或 Open Design 的 prompting 技巧，未來兩年都還用得上。

---

## 五、實戰：今天就能跑的指令範例

### 5.1 Claude Design 三個高勝率 prompt

```
1. 探索變體
"Create 3 variations of a SaaS landing page hero section.
Goal: convert dev tool early adopters.
Layout: full-bleed hero, headline + sub + CTA + product screenshot.
Tone: confident, technical, no hype."

2. 接 codebase 套品牌
"Read the design tokens from packages/ui/tokens.
Then design a settings page for the existing dashboard
that matches the current visual language."

3. 一次出多檔給投資人
"Create a 10-slide pitch deck for a fintech app targeting Gen Z.
Include cover, problem, solution, market, demo screenshots,
business model, traction, team, ask, contact.
Export as PPTX."
```

### 5.2 Open Design 從零到第一張原型

```bash
# 1. 裝
brew install pnpm node
git clone https://github.com/nexu-io/open-design.git
cd open-design && pnpm install
pnpm tools-dev run web   # 開 localhost:3000

# 2. 接 agent（假設你已裝 Claude Code）
od mcp install claude-code

# 3. 選 design system
od system use linear   # 或 stripe / vercel / apple ...

# 4. 第一張原型
od skill prototype --prompt "an ECG monitoring dashboard for a wearable device, dark mode"

# 5. 輸出
od export html prototype-001
od export mp4 prototype-001 --hyperframe product-reveal
```

> 上面 CLI 範例的精確語法以官方為準，下指令前先 `od --help` 或翻 `od skill --help` 、 `od export --help` 。preview 階段子命令名稱有可能改。

### 5.3 自己寫一份 DESIGN.md（公司品牌專用）

```markdown
# DESIGN.md — BashCat 品牌規範

## color
- primary: #00F0FF (neon cyan)
- text: #E8E8E8
- bg: #0B0B0F

## typography
- display: Inter (700)
- body: Inter (400)
- code: JetBrains Mono

## anti-patterns
- 禁用：紅黃綠交通號誌色（會跟我們 ECG 警示衝突）
- 禁用：emoji 在 UI 文字中（保留給 marketing）
- 禁用：圓角 > 16px（不是 fintech 不要裝可愛）
```

把這檔丟進 `design-systems/bashcat/DESIGN.md` ，下次 render 就吃這套了。

---

## 六、結論：怎麼選一句話

| 你是誰 | 選 |
| --- | --- |
| 創辦人 / 非技術 / 要快 | Claude Design |
| 已用 coding agent / 機密案 / 想 fork | Open Design |
| 都要 + 不在乎錢 | 並用（探索期前者，production 後者） |

**Claude Design 不是新工具，是 Anthropic 把它已經內建的能力包成介面賣你** 。Open Design 不是 Claude Design 的 clone，是 **把同一套能力解構成 skill，讓開發者擁有 runtime** 。

對工程師來說，這比 Figma 加 AI 還重要——因為它直接改寫了「設計工具」的本質定義。今天還是 SaaS-shape 的工具，明年可能全變成 skill 包。

如果你問我自己怎麼選：我已經用 Claude Code Max，所以 Open Design 是無腦選擇。但下次要做給客戶看的 pitch、又懶得開 dev 環境，我會打開 `claude.ai/design` ，付那 $20。

兩個都用，比兩個都不用聰明。

---

## 延伸閱讀 / 參考資料

- [Introducing Claude Design by Anthropic Labs](https://www.anthropic.com/news/claude-design-anthropic-labs) — 官方公告
- [Anthropic launches Claude Design (TechCrunch)](https://techcrunch.com/2026/04/17/anthropic-launches-claude-design-a-new-product-for-creating-quick-visuals/) — 業界報導
- [What Is Claude Design? (DataCamp)](https://www.datacamp.com/blog/claude-design) — 完整解析
- [nexu-io/open-design GitHub](https://github.com/nexu-io/open-design) — 官方 repo
- [Open Design 官網](https://opendesigner.io/) — 桌面 app 下載
- [Open Design vs Claude Design 對比頁](https://opendesigner.io/compare) — 官方對比
- [WotAI 選型對比文](https://wotai.co/blog/claude-design-vs-open-design) — 第三方對比
- [Augment Code: Open Design 57.4K stars 分析](https://www.augmentcode.com/learn/open-design-claude-design-alternative) — 趨勢觀察
- [Julian Goldie 成本對比](https://juliangoldieaiautomation.com/blog/open-design-vs-claude-design/) — 實際 ROI

---

*這篇文章基於 2026-06-07 的研究資料撰寫。Open Design 仍在 preview 階段，數字與指令可能週週變動，跑之前麻煩翻一下官方 repo。未來會再寫實測篇。*