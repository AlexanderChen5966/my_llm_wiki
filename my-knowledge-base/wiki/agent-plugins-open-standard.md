---
title: Agent Plugins 1.0：跨平台 Agent 擴充標準，安全與信任刻意留白
date: 2026-08-06
tags: [ai/agent, ai/mcp, method/tools]
source: https://techorange.com（原始：The Next Web／agent-plugins.org／AWS／Google／Vercel／AAIF）
type: article
status: processed
asset_value: 72
---

## T（核心論點）

2026-08-06，**OpenAI 與 AWS、微軟、Cursor、Vercel 共同發布開放標準 Agent Plugins 1.0，Google 隨後以核心維護者身分加入**。它解決的不是 agent 能力問題，而是**封裝與分享**問題——Agent Skills 給了可重用指令與腳本、MCP 給了外部系統連接，但**分享這些能力沒有統一的套件格式**。

主打「**一次建置，隨處運行**」（build once, run anywhere）：一個外掛就是**一個根目錄放著 `plugin.json` 的資料夾**，把 MCP servers 與 Agent Skills 包在一起。

🔴 **但 1.0 刻意做窄——只定義「怎麼打包」與「怎麼被發現」，安全與信任全部留白。**

## B（適用場景）

**適用：** 開發要跨多個 agent 平台發布的 Skill/MCP 組合；評估第三方外掛的信任風險；理解 agent 生態的標準化進程

**不適用 / 注意：**
- ⚠️ **Anthropic 不在發布名單中**（發布即支援的客戶端為 ChatGPT／Codex、Cursor、GitHub Copilot、Kiro、VS Code，AWS 與 Google 各自在旗下產品導入）——Claude Code 的 plugin 生態目前仍是自有格式，見 [[claude-code-plugins-guide]]
- 標準才 1.0，市集經營、信任機制、安全把關全未定案

## R（規格範圍與留白）

### 1.0 只定義兩件事

| 定義了 | 交給各平台自理 |
|---|---|
| ✅ 外掛**怎麼打包** | ❌ **發行**：市集、安裝、更新 |
| ✅ 外掛**怎麼被發現** | ❌ **執行體驗**：權限、審核流程、沙箱、介面 |
| | ❌ **信任**：發布者身分、來源證明、簽章、組織政策 |

### 🔴 留白正是隱憂所在

> **判斷一個外掛能不能安全執行，仍是每個客戶端各自的責任**——而《TNW》指出，**今年稍早就曾有偽造的 Agent Skills 通過資安掃描**。

→ 換句話說，標準統一了「怎麼裝」，卻沒統一「怎麼確認它安全」。這使 [[github-skill-security-audit]] 那套人工審查流程在標準化之後**更重要而非更不重要**——分發變容易了，把關沒有跟著變容易。

### 開發者反應兩極

| 立場 | 說法 |
|---|---|
| 樂見（Angie Jones）| 一直想要一套能在不同工具間帶著走的方式 |
| 反對（Dax Raad, SST）| 稱這是「**一套單薄的標準**」，認為真正有用的部分終究會落回各客戶端的專屬擴充 |

### ⚠️ 更深一層的競爭張力

《TNW》點出的雙面效應值得記住：

- **正面**：統一格式讓小型開發者得以一次觸及所有主流 Agent，是開放生態的理想情境
- 🔴 **反面**：**標準會獎勵那些使用者本來就多的客戶端，反而可能鞏固既有龍頭的優勢**

> 工具連接與打包這一層總算有了共同語言；但市集怎麼經營、外掛能不能被信任、安全該由誰把關——**這些被 1.0 刻意留白的環節，角力才正要開始**。

## C（關鍵行動）

- [ ] 要跨平台發布 Skill/MCP 組合時，改以 `plugin.json` 為根的 Agent Plugins 格式打包
- [ ] 🔴 **不要因為「符合標準」就信任外掛**——1.0 不含信任機制，且已有偽造 Skills 通過資安掃描的前例
- [ ] 安裝第三方外掛前仍走人工唯讀審查（見 [[github-skill-security-audit]]）
- [ ] 注意 Anthropic 不在此標準的發布陣營，Claude Code 外掛仍走自有格式——跨兩邊發布目前需各做一份
- [ ] 觀察後續：市集治理與簽章機制由誰主導，是這個標準真正的成敗點

## 相關連結

- [[github-skill-security-audit]] — 🔑 **配套必讀**：本標準把信任留白，故第三方外掛的唯讀安全審查（掃 webhook/curl/fetch、直接刪 `WEBHOOK_URL`）在標準化後更關鍵
- [[claude-code-plugins-guide]] — Claude Code 自有 plugin 格式與六元件；⚠️ Anthropic 不在 Agent Plugins 1.0 陣營
- [[agent-skills-vs-mcp]] — Skills 與 MCP 的分工；本標準正是把兩者包成同一個套件
- [[mcp-stateless-architecture]] — MCP 規格演進；與本標準是同一生態的兩個層次（連線協定 vs 封裝格式）
- [[claude-cowork-record-a-skill]] — ⚠️ 對照：錄製類工具產出的 Skill **綁定生成它的平台能力模型**，「錄一次到處用」目前做不到——本標準解決封裝，沒解決能力對等
- [[ai-skills-mcp-plugins-agents-comparison]] — Skills/MCP/Plugins/Agents 四者定位比較
- [[claude-code-plugins-guide]] — 補充資料：Claude 外掛提交入口的 Plugin Bundle（MCP 伺服器＋Agent Skills）與本篇定義一致
- [[custom-gpt-gems-to-skills-migration]] — 自訂 GPT／Gems 遷移到 Skills 的跨廠商對照（2026-10-08 快照）
