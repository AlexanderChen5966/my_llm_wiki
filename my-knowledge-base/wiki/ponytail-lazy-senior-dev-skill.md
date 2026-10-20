---
title: ponytail：把「懶惰資深工程師」注入 AI Agent 的 Skill
date: 2026-06-17
tags: [ai/agent, dev/architecture]
source: https://github.com/DietrichGebert/ponytail；補充自 https://knightli.com/zh-tw/2026/06/24/ponytail-ai-agent-coding-plugin-guide/、https://www.alphamatch.ai/zh/blog/ponytail-ai-coding-skill-2026 與 https://www.inside.com.tw/article/41622-ai-ponytail
type: reference
status: processed
asset_value: 64
---

有種工程師，你給他五十行，他看一眼說沒什麼，然後改成一行，而且能跑。ponytail 把這個角色塞進 AI Agent：讓模型在動手寫之前，先爬完六個判斷梯級，把「不必寫的程式碼」消滅在起跑線。

## T（核心論點）

ponytail 是一個跨 CLI 的 Agent Skill，核心是一個六梯級決策樹，強制 Agent 在寫任何程式前先問「這東西需要存在嗎？」。官方對比無 Skill 基準值，中位數測試結果：**80-94% 更少程式碼、47-77% 更低成本、3-6× 更快**。Skill 在安全邊界（資料驗證、錯誤處理、資安、無障礙）永不妥協。**重要更正**：外界一度流傳的「94% 減幅」其實是舊版單次測試的極端數字（代理嚴重過度開發的情境），開發者本人在被指出比較基準灌水後已重做測試，**平均值下修到約 54%**（詳見下方基準測試段落）。

## B（適用場景）

**適用：**
- 發現 AI Agent 動不動就安裝新套件、建 wrapper class、開新抽象層
- 想讓 Agent 優先用 stdlib/原生 API/已安裝依賴而非另起爐灶
- 需要降低生成程式碼量、加快執行速度、減少 API token 成本

**不適用：**
- 任務本質上就需要完整實作（核心業務邏輯、自訂演算法），不應套用懶惰原則
- 當 AI 產出規格已明確要求特定程式庫時

## R（六梯級決策樹與 Skill 架構）

### 六梯級決策樹（Agent 在寫任何程式前必須遵守）

```
1. Does this need to exist?   → no: skip it (YAGNI)
2. Stdlib does it?            → use it
3. Native platform feature?   → use it
4. Installed dependency?      → use it
5. One line?                  → one line
6. Only then: the minimum that works
```

**範例：** 請求「做一個日期選擇器」，無 ponytail 的 Agent 會安裝 flatpickr、建 wrapper、加樣式表。有 ponytail：直接輸出 `<input type="date">`，並加上 `<!-- ponytail: browser has one -->` 說明捷徑，方便日後升級。

### 效能基準（5 個日常任務 × 3 個模型 × 3 臂，中位數，10 次跑）

| 指標 | vs 無 Skill 基準值 |
|------|-----------------|
| 程式碼行數 | -80% 到 -94% |
| API 成本 | -47% 到 -77% |
| 完成速度 | 3-6× 更快 |
| Token 使用量 | 每項任務約 -16% |

基準任務：email validator / debounce / CSV sum / countdown timer / rate limiter。可用 `npx promptfoo eval -c benchmarks/promptfooconfig.yaml` 自行重現。

**其中一輪測試（Claude Code + Haiku 3.5，12 項任務）細節**：程式碼行數降至基準線 46%（191 行→約 88 行）／Token 降至 78%／費用降至 80%（基準 $0.15）／時間降至 73%（基準 65 秒）／**對抗性安全測試（路徑穿越/SQL 注入/Token 偽造）維持 100% 通過**，對照組 yagni-one-liner（無安全邊界的激進單行版）僅 83%（漏掉一個防護案例）——這組數據佐證 ponytail「懶惰但不犧牲安全邊界」的設計主張並非空話。倒數計時器任務中，無 Skill 代理寫出 190 行帶動畫的「儀表板」，ponytail 只交付 13 行。創作者原話：「293 行程式碼縮減到 47 行，那 246 行從未被寫出來的程式碼，從來沒有造成任何事故。」

### 三個強度等級與指令

| 指令 | 說明 |
|------|------|
| `/ponytail lite` | 較保守的懶惰程度 |
| `/ponytail full` | 預設，一般使用 |
| `/ponytail ultra` | 最激進，當 codebase 深深冒犯你時 |
| `off` | 本次 session 停用 |
| `/ponytail-review` | 審查當前 diff，回傳過度工程刪除清單 |
| `/ponytail-audit` | 審查整個 repo，不只看 diff |
| `/ponytail-debt` | 彙整所有 `ponytail:` 捷徑標記，避免「晚點再說」永遠沒發生 |

預設模式可透過環境變數 `export PONYTAIL_DEFAULT_MODE=full` 或 `~/.config/ponytail/config.json` 設定，每次對話輪次都會重新注入規則集，約束持續生效（複利式節省：現在少寫的程式碼，之後讀取上下文時也更少）。

### 各 CLI 安裝方式

| 工具 | 安裝指令 |
|------|---------|
| Claude Code | `/plugin marketplace add DietrichGebert/ponytail`（換行後另傳）`/plugin install ponytail@ponytail` |
| Claude 桌面版 | 無 `/plugin` 指令，需在 UI「Customize → personal plugins → Add from repository」手動輸入 repo 網址 |
| Codex CLI | `codex plugin marketplace add DietrichGebert/ponytail` → 進入 `/plugins` 安裝 → `/hooks` 檢查並信任兩個 lifecycle hooks |
| GitHub Copilot CLI | `copilot plugin marketplace add DietrichGebert/ponytail` + `copilot plugin install ponytail@ponytail`（或互動式 `/plugin` 指令） |
| Gemini CLI | `gemini extensions install https://github.com/DietrichGebert/ponytail`，規則集作為每次會話上下文載入 |
| Antigravity CLI | `agy plugin install https://github.com/DietrichGebert/ponytail`（Google 正將 Gemini CLI 改名為 Antigravity CLI，過渡期兩種路徑並存） |
| OpenCode | `opencode.json` 加入 `{"plugin": ["@dietrichgebert/ponytail"]}` |
| 僅要規則檔（不裝外掛） | repo 內附 `AGENTS.md` / `.cursor/rules/` / `.windsurf/rules/` / `.clinerules` / `.github/copilot-instructions.md` / `.kiro/steering/`，支援讀取專案級規則的工具可直接複製對應檔案 |

**注意**：Claude Code / Codex 外掛會執行兩個小型 Node.js lifecycle hooks，`node` 需在 PATH 內；沒有 `node` 時 skills 仍可運作，但 always-on 啟用不會自動生效（先用 `node -v` 確認）。

### ponytail: 捷徑標記機制

凡 Agent 使用捷徑時，程式碼中會留下 `ponytail: <reason>` 標記，說明升級路徑，讓「暫時先這樣」有可追蹤的記錄而非無聲埋坑。

### 適合 vs 不適合的任務類型

**適合**：前端小控制項（日期/顏色/檔案上傳等原生 HTML 常常已夠用）、工具函式（去重/排序/格式化，先查標準函式庫）、設定檔小改動、小 bug 最小修復、code review 當「過度設計檢查器」（找重複造輪子/不必要依賴/可刪程式碼）。

**不適用場景（不能為了少程式碼硬省）**：輸入校驗、權限檢查、資料丟失保護、並發與交易安全、使用者隱私、可訪問性、錯誤處理、關鍵日誌、測試覆蓋——這些安全邊界即使在最小實作下也必須處理清楚，簡單不等於粗糙。

**建議上手順序**：小專案先裝 → 用「容易被過度實作」的需求測試（如日期選擇器）對比有無 ponytail 的差異 → 讓它審查一次既有 diff → 再放進日常專案，不要一開始就拿大重構測試。

### 社群評價與方法論爭議

**流傳最廣案例**：一個日期選擇器元件套用後從 404 行縮減到 23 行（改用瀏覽器原生 `<input type="date">` 而非引入外掛元件）。

**批評聲音**：核心概念本質上是 YAGNI 原則的再包裝，並非全新發明——只要在提示詞裡加一句「請遵循 YAGNI 原則並優先採用單行解法」，就能逼近它的程式碼減量幅度。但官方對照測試顯示，純提示作法（yagni-one-liner）會犧牲部分安全防護，對抗性安全測試通過率僅 83%，低於 ponytail 的 100%（詳見上方基準測試）——**這是「懶惰但不犧牲安全邊界」能否成立的關鍵證據**。

**基準測試單一情境限制**：官方基準測試的 54% 平均減幅數字是以 Claude Code 搭配 Haiku 4.5、在單一開源專案（FastAPI + React）上測得，效果因模型而異，**部分推理型模型上成本與延遲甚至可能不減反增**，不宜直接套用到所有專案。

**成效看板指令 `/ponytail-gain`**：顯示官方基準測試的固定數字（程式碼減量/成本/速度中位數），**並非針對使用者自身專案計算的結果**，使用時勿誤解為個人化效能報告。目前版本 v4.7.0，MIT 授權。

## C（關鍵行動）

- [ ] 安裝 ponytail 後先跑 `/ponytail-audit` 看現有 repo 有多少可刪的過度抽象
- [ ] 搭配 `/ponytail-debt` 定期清理 `ponytail:` 標記，確保捷徑不永久留下
- [ ] 在 CLAUDE.md 中設定 `PONYTAIL_DEFAULT_MODE=full`，讓每次 session 自動套用
- [ ] Claude Code/Codex 外掛依賴 Node.js hooks，安裝前先確認 `node -v` 可執行
- [ ] 涉及安全邊界/資金/使用者資料/生產環境的任務，不套用「少寫」原則，最小實作仍要完整處理邊界

### 與 Caveman 的軸線區分（中文環境的關鍵優勢）

（2026-07-13 補充）常與 Caveman Prompt 混淆，但兩者作用軸線不同：Caveman 壓縮「**對話文字**」的 token（電報式縮寫，語言相依性高——壓縮句法建立在英文 tokenizer 特性上，官方 65% 數據全為英文任務），ponytail 減少「**生成程式碼**」的量（**語言中立**——省的是 code，用中文對話下指令效果不變）。**中文工作環境、輸出以程式碼為主時，ponytail 是比 Caveman 更可靠的省 token 選擇**。完整比較表見 [[github-copilot-token-reduction]]。

**開發流程整合建議**（對應三模式開發流程，詳見 research/dev-workflow-optimization-tools）：不直接照裝，**讓 AI 把六梯級規則改寫進自己的 CLAUDE.md**（避免「AI 變笨/失去創意」副作用），可只取前四梯級；多 Agent 環境則放進共讀的 AGENTS.md 讓所有實作 agent 一致遵循。

## 相關連結

- [[addyosmani-engineering-skills]] — Addy Osmani 的工程師 Skills 也強調「隱性直覺→顯性規則」，與 ponytail 理念互補
- [[impeccable-style-ai-coding-guide]] — 另一類 AI 行為規範 Skill，側重程式碼品味（排版/命名）而非精簡原則
- [[skill-md-writing-techniques]] — 若要自製類似哲學的 Skill，SKILL.md 撰寫技巧的參考資源
- [[github-copilot-token-reduction]] — Caveman vs Ponytail 完整軸線比較（中文環境選用結論）
- [[codebase-memory-mcp]] — 互補的輸入端省 token 工具（ponytail 管輸出 code、它管輸入探索）
