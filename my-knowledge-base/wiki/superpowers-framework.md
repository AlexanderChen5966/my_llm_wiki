---
title: Superpowers：給 AI 開發的工程紀律框架
date: 2026-05-14
updated: 2026-07-30
tags: [ai/agent, ai/claude]
source: https://github.com/obra/superpowers
type: article
status: processed
asset_value: 78
source_format: image
---

Superpowers 是 Jesse Vincent（obra）開源的 AI 開發方法論框架，150k+ stars。核心問題：AI 很容易接到需求就直接衝去寫 code，缺乏人類工程師多年養成的紀律（先想清楚、寫測試、驗證完成）。一套可組合 Skill 從需求釐清到分支合併，強制建立完整的開發工作流；**本質是 Mandatory Workflow，AI 每次開發都必須走完這條路。**支援 Claude Code、Gemini CLI、Cursor、Codex CLI、GitHub Copilot CLI 等 8 個主流 AI 平台。

## T（核心論點）
AI 不缺聰明，缺的是**紀律**——「你必須」而不是「你應該」。Superpowers 的設計假設執行者（AI）品味和判斷力都欠佳、不喜歡寫測試，所以計畫要寫得極度詳細、流程要強制執行、每個潛在藉口都預先堵死。**關鍵設計原則：AI 不會直接寫程式，而是先經過思考、清晰、產出設計文件後，再由使用者確認才動工。**

## B（適用場景）
**適用：** 被 Claude「自信地寫了一個沒測試的 bug fix」氣過的開發者；需要 AI 長時間自走任務的場景
**不適用：** 一次性小任務；已有完整 CI/CD 規範的成熟團隊（可能過度規範）

## R（Skills 清單）

### 七步驟標準工作流
```
1. brainstorming       → 蘇格拉底式提問釐清需求，產出設計文件
2. using-git-worktrees → 設計確認後建立隔離 worktree 新分支
3. writing-plans       → 把設計拆成 2-5 分鐘微任務，含檔案路徑/測試步驟/驗證條件
4. subagent-driven-development 或 executing-plans
                       → 每個任務派獨立 subagent 執行 + 兩階段審查（規格合規 + 程式品質）
5. test-driven-development
                       → RED（寫失敗測試）→ GREEN（最少 code）→ REFACTOR
6. requesting-code-review
                       → 按計畫審查，依嚴重程度回報，Critical 會阻擋進度
7. finishing-a-development-branch
                       → 驗證測試，選擇 merge / PR / keep / discard，清理 worktree
```

### 協作類
| Skill | 作用 |
|-------|------|
| `brainstorming` | 蘇格拉底式提問，一次一個問題，YAGNI ruthlessly，存設計文件 |
| `writing-plans` | 極度詳細計畫：檔案路徑、完整程式碼、驗證步驟，2-5 分鐘微任務 |
| `executing-plans` | 批次執行含人工檢查點 |
| `dispatching-parallel-agents` | 並行 subagent 工作流 |
| `subagent-driven-development` | 每個任務派獨立 subagent，兩階段 review，Claude 可自走 2 小時 |
| `requesting-code-review` | code review 前置清單規範 |
| `receiving-code-review` | 接受 review 的回應規範 |
| `using-git-worktrees` | 隔離 worktree，確保主分支整潔 |
| `finishing-a-development-branch` | 合併/PR/清理標準流程 |

### 測試驅動
| Skill | 作用 |
|-------|------|
| `test-driven-development` | RED-GREEN-REFACTOR 強制循環（含 testing-anti-patterns 禁止清單）|
| `verification-before-completion` | 完成前必須真正跑測試 |

### 除錯
| Skill | 作用 |
|-------|------|
| `systematic-debugging` | 四階段根因追蹤（含 root-cause-tracing、defense-in-depth、condition-based-waiting）|
| `verification-before-completion` | 確認真的修好了 |

### Meta
| Skill | 作用 |
|-------|------|
| `using-superpowers` | 技能系統介紹，SessionStart hook 自動注入 |
| `writing-skills` | 遵循最佳實踐創建新 Skill 的指南 |

### 架構
SessionStart hook：每次對話開始自動注入 `using-superpowers` Skill，讓 AI 從一開始就知道這套規範。因為技能會自動觸發，使用者**不需要做任何特別的事**——coding agent 就直接有了 Superpowers。

### 四大核心理念
| 理念 | 說明 |
|------|------|
| TDD 測試優先 | 先寫失敗測試，再寫最少程式碼讓測試通過 |
| 系統化流程 | 每個步驟都有明確輸入、輸出與驗證標準 |
| 降低複雜度 | 任務拆解到 2-5 分鐘微任務，避免大塊不可測的工作 |
| 實證主義 | 不猜測、不假設——只相信跑通的測試 |

### 五步驟開發流程
```
1. 腦力激盪與設計
   → brainstorming Skill：蘇格拉底式提問，一次一個問題
   → AI 產出設計文件，使用者確認後才繼續

2. 撰寫實作計畫
   → writing-plans Skill：極度詳細，含檔案路徑/測試步驟/驗證條件
   → 拆分為 2-5 分鐘微任務（TDD）

3. 子代理驅動開發
   → subagent-driven-development：每個任務派獨立 subagent 執行
   → 確保每個 subagent 完成並驗證後再繼續，不讓 AI 自行主導進度

4. 自動化程式碼審查
   → requesting-code-review / receiving-code-review
   → 標示修改出現問題，強制修正

5. 完成與交付
   → 所有任務完成，會自動管理測試、整合合併、發佈 PR 等事項
```

## C（關鍵行動）

**安裝（Claude Code）：**
```
# 官方 Marketplace（最簡單）
/plugin install superpowers@claude-plugins-official

# 或 Superpowers Marketplace
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

**其他平台安裝：**
- Gemini CLI：`gemini extensions install https://github.com/obra/superpowers`
- Cursor：`/add-plugin superpowers`
- GitHub Copilot CLI：`copilot plugin marketplace add obra/superpowers-marketplace`
- Codex CLI：搜尋官方 `/plugins` marketplace
- Factory Droid、OpenCode 亦支援（見官方 README）

- [ ] **搭配 spec-kit（規格先行）**：先用 spec-kit 寫清規格文件，再讓 superpowers 控制實作紀律（見 [[spec-driven-ai-workflow]]）
- [ ] **vs gstack 決策**：需要「流程品質保證」→ Superpowers；需要「角色分工 + DevOps 全流程」→ gstack；兩者可共存
- [ ] **社群支援**：Discord `discord.gg/35wsABTejz`，作者 Jesse Vincent（Prime Radiant）

## Superpowers vs gstack 快速比較

| 維度 | Superpowers | gstack |
|------|-------------|--------|
| 定位 | 把 AI 提升為格工程流程的 Mandatory Workflow | 把 AI 拆成多個專業角色分層協作 |
| 核心強調 | 流程品質（紀律、測試、驗證）| 角色分工（CEO / 設計師 / 工程師 / Staff Engineer）|
| 設計理念 | 「AI 必須遵守紀律」——堵死每個藉口 | 「AI 可以模擬完整團隊」——Sprint flow 迭代 |
| 適用場景 | 確保 code 品質；TDD 工程紀律；防止 AI 自走偏離 | 完整產品開發；多角色戰略審查；DevOps 全流程 |
| 解決問題 | 任務前徹底把清楚問題，強迫 TDD 與深度驗證 | 先想問題再運行 Skill，把問題落地為流程 |
| 最佳拍檔 | spec-kit（規格先行）| gstack-upgrade + document-release（交付自動化）|

**使用情境判斷：**
- 工程品質有問題（不寫測試、Debug 反覆）→ **Superpowers**
- 需要多人角色視角（CEO/設計/工程師審查）→ **gstack**
- 一人開發、想跑完整交付流程 → **gstack**
- 長時間自走任務、需要防偏離保證 → **Superpowers**

## 補充視角：設計哲學深度解析

來源：高見龍文章（2026-01-20），深入分析各 Skill 設計取捨：

**brainstorming Skill 關鍵細節：**
- 一次只問一個問題（`One question at a time`）
- `YAGNI ruthlessly`：設計階段就積極砍掉不必要功能，不等到實作時
- 使用者確認設計時，AI 每 200-300 字一個段落，逐段確認——避免整份設計砸出來沒人看

**verification-before-completion 關鍵細節：**
- 宣稱完成但沒驗證是「不誠實，不是有效率」
- 五步驟通關：IDENTIFY（什麼指令能證明？）→ RUN（跑完整指令）→ READ（讀完整輸出）→ VERIFY（輸出真的符合宣稱？）→ ONLY THEN（才做宣稱）
- 警告訊號：出現「應該」「大概」「似乎」就停下來

**receiving-code-review 獨特設計：**
- 禁止「表演性贊同」（`You're absolutely right!` / `Great point!`）
- 要先理解→驗證→評估→才回應，不是照單全收
- 暗號 `Strange things are afoot at the Circle K`：想推回但不方便說時，讓人類開發者知道有話要說

**writing-skills 的 TDD 對應：**
| TDD 概念 | Skill 創作 |
|---------|-----------|
| Test case | 壓力測試場景（用 subagent）|
| Production code | SKILL.md 文件 |
| Test fails (RED) | Agent 沒有 Skill 時違反規則 |
| Test passes (GREEN) | Agent 有 Skill 時遵守規則 |
| Refactor | 堵住繞過漏洞 |

**四大設計取捨：**
1. **強制而非建議**：堵住「違反精神」的詮釋空間
2. **理性化預防表格**：不是教道理，是在堵漏洞
3. **每步驟有驗證**：做完≠好了，要跑出來看
4. **任務原子性**：每個任務可交給全新 subagent，不依賴前面 context

**已知限制（作者承認）：**
- 完全依賴 AI 自律，沒有技術手段強制執行
- `subagent-driven-development` 每個任務開 3 個 subagent，50 個任務燒 token 很快
- 教你避免過度設計的系統，本身可能就是過度設計

## 補充資料
- `superpowers_flow.jpg`：五步驟開發流程圖 + 四大核心理念（TDD / 系統化 / 降低複雜度 / 實證主義）
- `superpowers_vs_gstack.jpg`：手寫筆記風格對比分析，含四維比較與使用情境決策樹

## 補充資料（來源：obra/superpowers GitHub README，2026-05-14）

### 官方安裝方式更新（完整列表）

所有支援平台：Claude Code、Codex CLI、Codex App、Factory Droid、Gemini CLI、OpenCode、Cursor、GitHub Copilot CLI。

| 平台 | 安裝指令 |
|------|---------|
| Claude Code（官方 Marketplace）| `/plugin install superpowers@claude-plugins-official` |
| Codex App | Plugins 側欄搜尋 Superpowers → 點 + |
| Gemini CLI | `gemini extensions install https://github.com/obra/superpowers` |
| OpenCode | 告訴 OpenCode：`Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md` |
| Cursor | `/add-plugin superpowers` |

### 七步驟工作流（官方確認版本）

官方 README 確認工作流共 7 步（與先前整理一致）：
1. brainstorming → 2. using-git-worktrees → 3. writing-plans → 4. subagent-driven-development 或 executing-plans → 5. test-driven-development → 6. requesting-code-review → 7. finishing-a-development-branch

**關鍵：Agent 在任何任務前都會檢查相關 Skills。是 Mandatory workflows，不是建議。**

### Skills 完整庫

**Testing**：test-driven-development
**Debugging**：systematic-debugging、verification-before-completion
**Collaboration**：brainstorming、writing-plans、executing-plans、dispatching-parallel-agents、requesting-code-review、receiving-code-review、using-git-worktrees、finishing-a-development-branch、subagent-driven-development
**Meta**：writing-skills、using-superpowers

### 社群與商業

作者 Jesse Vincent（obra）來自 Prime Radiant 公司。Discord：`discord.gg/35wsABTejz`。如 Superpowers 幫你賺錢，可贊助：`github.com/sponsors/obra`。

## 補充資料：兩個真任務實測與一個反證（2026-07-30 併入）

來源：MikeWang Claude Code 系列連載第 12 篇（Medium，2026-07-07）。

### 修正了原先的三個擔憂

實測兩個真任務後發現：**不亂開 worktree、不擅自 push、而且「越難越穩」**——任務越複雜，強制流程的價值越明顯。

### 🔴 但最有價值的是這個反證

> `isNumber` 對**全形數字**回傳 `true` 的漏抓——完整跑完 Superpowers 的強制流程（brainstorming → writing-plans → TDD → 自動審查）**仍然沒抓到**。
>
> **證明：流程嚴格性 ≠ 正確性。**

這與 [[superpowers-skill-bloat-risk-tiering]] 的批判方向不同但可並讀：那篇說「硬門檻壓縮強模型自由度」，本則說「**就算你乖乖跑完全流程，該漏的還是會漏**」——流程能保證「有做審查」，不能保證「審查抓到了」。

### 三種規格工具的強制力來源不同

| 工具 | 強制力性質 |
|---|---|
| **Plan Mode** | Claude Code **內建模式** ＝ **硬保證**（不做不了）|
| **Brainstorming**（Superpowers skill）| **skill 內的紀律** ＝ **被說服的**（模型可以不聽）|
| **OpenSpec / opsx** | 外部**規格檔** ＝ 可版控的契約 |

→ 分流判準（同系列 Plan Mode 篇）：**「『要做什麼』清楚了嗎？」**不清楚才需要規格工具。

## 補充資料：平台支援現況核實（2026-08-05 直接查官方 README）

⚠️ 本頁上方兩處的「支援 8 個平台」清單為 **2026-05-14 記錄，已過期**。2026-08-05 直接查 `obra/superpowers` README 的現況：

**已擴為 11 個平台**（新增 3 個）：

| 平台 | 安裝 |
|---|---|
| Claude Code | `/plugin install superpowers@claude-plugins-official` |
| 🆕 **Antigravity** | `agy plugin install https://github.com/obra/superpowers` |
| Codex App | Plugins 側欄 → Superpowers → `+` |
| Codex CLI | `/plugins` → 搜尋 `superpowers` → Install |
| Cursor | `/add-plugin superpowers` |
| Factory Droid | `droid plugin install superpowers@superpowers` |
| Gemini CLI | `gemini extensions install https://github.com/obra/superpowers` |
| GitHub Copilot CLI | `copilot plugin install superpowers@superpowers-marketplace` |
| 🆕 **Kimi Code** | `/plugins` → Marketplace → Superpowers |
| OpenCode | 依 `.opencode/INSTALL.md` 指示 |
| 🆕 **Pi** | `pi install git:github.com/obra/superpowers` |

### 🔴 官方明載的兩個限制（先前未收錄）

> **「Installation differs by harness. If you use more than one, install Superpowers separately for each one.」**

**多 agent 使用者要為每個 harness 分別安裝**——這是實質維護成本：跨 N 個 agent 就有 N 份安裝與 N 次版本更新，且各 harness 的安裝機制不同（plugin / extension / 手動 fetch）。

> **Pi 的註記：「Pi has native skills, so no compatibility `Skill` tool is required」**

值得注意的訊號：**harness 開始內建原生 skill 支援後，Superpowers 的相容層就變成多餘的**。這預示長期方向是能力下沉到 harness，而非靠框架補齊。

### 上游對「太重」批評的回應：README 未變，但內部正在大改

⚠️ **本段初稿（2026-08-05 早）誤寫為「上游沒有回應」，同日查 issue tracker 後更正如下。**

README 現況確實仍明載 **「The agent checks for relevant skills before any task. Mandatory workflows, not suggestions.」**，`skills/` 目錄仍是 **14 個**（與 2026-07 批評時相同，無增減）。**但 issue tracker 顯示上游正在積極改這件事**：

| Issue/PR | 狀態 | 內容 |
|---|---|---|
| **#2063** feat(brainstorming): **three-path router — ceremony scales, approval never does** | **open PR**（2026-07-31，targets `dev`）| 把入口決策改寫成 **spike / bounded / architectural** 三路徑，**縮放產出物但每條路徑都保留人工核准** |
| **#2078** fix(sdd): batch small same-shape tasks into one dispatch | ✅ **已合併 2026-08-04** | 直接處理子代理固定開銷（每任務一次實作 dispatch ＋ 一次審查 dispatch，各帶完整 brief/diff/context）|
| **#2087** using-superpowers preamble: **consider judgment-based framing for Claude 5-generation models** | open（2026-08-04）| 直接引 Anthropic 官方 Claude 5 context engineering 部落格，指 `EXTREMELY-IMPORTANT` 區塊與 12 列 Red Flags 表「**其字面解讀甚至禁止模型讀附上的連結或提出釐清問題**」，耗約 450 字在強制執行上 |
| #2076 / #2068 | open | brainstorming 缺機械性任務排除；skill 應在流程中途重查而非只在回合開始 |

→ 所以正確說法是：**批評對「已發佈版本」仍成立，但上游已認可並在 `dev` 分支處理中**。詳細評估與數據見 [[superpowers-skill-bloat-risk-tiering]]。

**版本**：官方市集快取顯示 v6.1.1，但 repo `plugin.json` 已是 **v6.2.0**（市集快取落後）。repo 於 2026-08-05 仍有 push，**266.6k stars**（本頁上方「150k+」為 2026-05-14 數字，已過期）。

## 相關連結
- [[claude-code-mechanism-division-series]] — 該系列的索引與四機制分工邊界（本補充的來源系列）
- [[superpowers-skill-bloat-risk-tiering]] — 另一個批判角度：硬門檻在強模型時代壓縮自由度，應改風險分級
- [[spec-driven-ai-workflow]] — Superpowers + spec-kit / OpenSpec 搭配工作流
- [[gstack-framework]] — YC CEO 的 AI 開發團隊框架（13 個 Skill，更偏 DevOps 全流程與角色分工）
- [[claude-skills-ecosystem]] — 更多明星框架選擇
- [[harness-engineering]] — Guides × Sensors 的 Harness 設計理論
