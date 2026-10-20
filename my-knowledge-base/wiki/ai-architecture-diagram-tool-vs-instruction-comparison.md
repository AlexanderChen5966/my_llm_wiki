---
title: AI 畫 AWS 架構圖五種方式實測：指示明確度的影響大於工具選擇
date: 2026-09-29
tags: [ai/mcp, ai/prompt-engineering, type/comparison]
source: https://codelove.tw/@tony/post/anbd0a（譯自 https://qiita.com/sagochiko/items/ef77b084ff2dee1f859c）
type: article
status: processed
asset_value: 60
---

作者 sagochiko 想讓 AI 代理畫出「沒有 AI 感」、業務上能直接用的 AWS 架構圖，於是用同一組指示比較 5 種生成方式，觀察結果不滿意處、逐輪寫回指示。本條是 [[agent-era-prompt-structure]]「模型越強越要把任務講清楚」原則的**控制變因實證案例**——同一指示 × 5 種工具 × 3 次平均。

## T（核心論點）

> **同一組指示下，5 種方式（AWS Diagram MCP／aws-architecture-diagram Skill／draw.io 官方 MCP／draw.io 官方 Claude Code 外掛／什麼都不裝的 Claude Code）之間的差距，小於「逐輪把不滿寫回指示」帶來的差距；指示補齊後，裸 Claude Code 反而重複生成差異最小。最後把累積的指示封裝成 Skill。**

作者原話：「與其糾結到底該用哪個官方 MCP 或 Skill，更重要的是要有明確的指示。」並類比寫程式——指示不夠時就會冒出多餘註解或過度實作，架構圖也一樣。

## B（適用場景）

**適用：**
- 要讓 AI 代理產出架構圖、流程圖等「有明確外觀規範」的圖面
- 想**公平比較** MCP／Skill／外掛效果、需要排除個人環境干擾的評估（隔離旗標可直接沿用）
- 正在猶豫「該裝哪個官方工具」的人——先把指示寫清楚再比工具

**不適用 / 注意：**
- ⚠️ 數據為**作者實測**（sagochiko，Claude Code 2.1.278／Opus 5，2026-09，每模式 3 次平均），單一架構需求、評估標準是作者個人喜好（作者自稱構成圖喜好「帶有一點宗教性」）
- ⚠️ AWS Diagram MCP Server 1.0.23 只能輸出 PNG，且已被標示**不建議使用並停止更新**（截至 2026-09），作者僅為比較而用
- 輸出 PNG／SVG 需要 draw.io desktop
- 12 條指示中「圖示統一使用最新設計」一條**沒成功**，已從終版移除——不是每種不滿都能靠指示解決

## R（操作步驟）

### 1. 隔離測試環境（可照抄）

使用者自訂的 Skill、外掛、CLAUDE.md（AGENTS.md）都會影響輸出，比較前先只載入專案設定、只用指定的 MCP：

```bash
# 比較 MCP（AWS Diagram MCP Server、draw.io MCP Tool Server）
claude --setting-sources project --strict-mcp-config --mcp-config mcp.json

# 比較 Skill／外掛
claude --setting-sources project --strict-mcp-config --plugin-dir <外掛路徑>

# 基準組：不裝 Skill、MCP
claude --setting-sources project --strict-mcp-config
```

- 不加 `--strict-mcp-config`，claude.ai 那邊連動的 MCP（例如 AWS 文件搜尋）會殘留
- 🔴 **避開 `--safe-mode`**：它會把 CLAUDE.md、Skill、外掛、MCP 全部停用——連待測的 MCP／外掛也一起失效
- 可用 `/context all` 確認只有待比較的 MCP 被載入（Claude Code 內建 Skill 仍會保留）
- 本庫 [[claude-code-agents-md-loading-rules]] 也用過 `--strict-mcp-config --mcp-config <空設定>` 做同類隔離

### 2. 六個評估面向

可重現性（同指示每次相似）、性價比（token 與時間）、指示忠實度（不出現沒要求的編號或說明文字）、可讀性（線與標籤不重疊）、外觀（無圓角圖示或多餘框線，沒有 AI 感）、可編輯性（能手動修改的 `.drawio`）。

### 3. 逐輪把不滿寫回指示

| 覺得不對勁的地方 | 加進去的指示 |
|---|---|
| 文字和線重疊 | 不要讓線和標籤重疊 |
| 為避免重疊，圖示被改成方框 | 改用 AWS 官方圖示，不要用方框代替 |
| ECS 還是方框 | 一定要使用圖示 |
| 文字放在圖外 | 不要把文字放在圖外 |
| 線的標籤都消失了 | 線的標籤可以保留 |
| 子網路上下順序與顏色提示消失 | 可接受附加在元素上的簡短補充說明 |
| AZ 被擅自寫成 A、C | 寫正式的 AZ 名稱 |
| 使用者位置上下左右不一 | 把使用者放在圖的最上方 |
| 新舊圖示混雜 | 圖示統一使用最新設計（**失敗，終版移除**） |
| Aurora 變成執行個體圖示 | 使用服務層級的圖示 |
| 標籤位置每次不同 | 圖示標籤固定放在圖示下方 |
| 線從圖示的奇怪位置接出 | 線從圖示邊框的中央引出 |

### 4. 終版外觀規範範本（整理自作者終版指示，原文為日文）

架構內容（CloudFront／ALB／ECS Fargate／Aurora MySQL／S3／CloudWatch／2AZ VPC／NAT Gateway）依需求替換，以下外觀規範可沿用：

```
- 可用區（AZ）寫正式名稱，不要用「A」「B」之類的簡寫
- 使用者放在圖的最上端
- 每個 AZ 內，公開子網在上、私有子網在下
- 一定使用 AWS 官方圖示，不以方框或圓形代替；使用服務層級的圖示，不用執行個體等個別資源圖示
- 標籤用中文，但 AWS 服務名稱保持英文
- 圖示的標籤放在圖示下方
- 線不要壓到圖示或標籤文字；從標籤下方拉線可以；線與線不要重疊，交叉可以
- 線盡量從圖示邊的中央拉出
- 不要在圖外放標題、圖例、說明面板；線的標籤與附在元素上的簡短補充可以保留
- 圖以 draw.io 的 XML 格式存成檔案，放在 out/ 底下
```

### 5. 結果（【作者實測】，終版指示 × 3 次平均）

| 模式 | 時間 | 輸出 token | 可重現性與觀察 |
|---|---|---|---|
| AWS Diagram MCP Server（MCP，不建議） | 5.5 分鐘 | 23,393 | 每次差異大；線仍是曲線、圖示仍圓角；只有 PNG 無法再修改 |
| `aws-architecture-diagram` Skill | 5.4 分鐘 | 33,510 | 變動偏多；漏畫 CloudWatch 的線與 Aurora 複寫；NAT Gateway 變成 VPC 圖示 |
| draw.io MCP Tool Server | 4.1 分鐘 | 25,439 | 幾乎一樣，線條俐落；NAT Gateway 圖示每次不同（會變成 VPC 圖示） |
| draw.io 官方 Claude Code 外掛 | 5.8 分鐘 | 34,544 | 幾乎一樣，圖示一致、資訊量剛好；S3 位置會變 |
| Claude Code（無 Skill／MCP） | 5.4 分鐘 | 32,805 | 幾乎一樣、差異最小；沒有線條交叉 |

作者推薦 draw.io MCP、draw.io 外掛、裸 Claude Code 三者。

### 6. 封裝成 Skill

每次都寫這麼長的指示太麻煩 → 作者把一路累積的指示整理成自製 Skill，同需求能重現相同品質，也能畫沒有 VPC 的架構（如 Amazon Bedrock AgentCore、Lambda Web Adapter）。這正是 [[sop-to-skill-4-steps]] 的路徑：反覆追加的指示就是該沉澱的 SOP。

## C（關鍵行動）

- [ ] 比較任何 MCP／Skill／外掛前，先用 `--setting-sources project --strict-mcp-config` 隔離環境，不用 `--safe-mode`
- [ ] 先用裸 Claude Code 當基準組，確認工具真的有贏過「把指示寫清楚」
- [ ] 每輪只把一個不滿轉成一條指示，記錄哪條失敗、移除
- [ ] 指示穩定後封裝成 Skill，而不是每次重貼長指示
- [ ] 本庫若需要畫架構圖，優先選輸出 `.drawio` 的方式（可編輯），避開只輸出 PNG 的工具

## 原始摘要

作者以 AWS 與 draw.io 官方提供的 MCP、Skill、外掛，加上未設定任何擴充的 Claude Code，共 5 種模式，用同一組 AWS 架構指示生成架構圖；觀察生成結果中讓人介意的地方逐條加回指示（共 12 條，1 條失敗移除）。最終各模式都比初版好很多，draw.io MCP、draw.io 外掛、裸 Claude Code 表現最佳，其中裸 Claude Code 重複生成的差異最小。結論是指示明確度比工具選擇重要，並把累積指示封裝成 Skill。

## 相關連結

- [[agent-era-prompt-structure]] — 🔑 原則層：模型越強越要把任務講清楚；本篇是它的控制變因實證
- [[agent-skill-evaluation-tools-comparison]] — Skill 評估工具比較；本篇的隔離旗標可用來排除環境干擾
- [[agent-skills-vs-mcp]] — 工具形式選擇；本篇顯示形式差異小於指示差異
- [[ai-skills-mcp-plugins-agents-comparison]] — Skills／MCP／外掛／Agents 的定位比較
- [[sop-to-skill-4-steps]] — 把反覆追加的指示封裝成 Skill
- [[taste-skill-anti-slop-frontend]] — 同樣是「去 AI 感」，不同媒材（前端介面）
- [[claude-code-commands-reference]] — Claude Code 指令與旗標速查
- [[claude-code-agents-md-loading-rules]] — 同類隔離做法（`--strict-mcp-config --mcp-config <空設定>`）
