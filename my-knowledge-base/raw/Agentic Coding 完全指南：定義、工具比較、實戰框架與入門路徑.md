---
title: "Agentic Coding 完全指南：定義、工具比較、實戰框架與入門路徑"
source: "https://yu-wenhao.com/zh-TW/blog/agentic-coding-guide/"
author:
  - "[[WenHao Yu]]"
published: 2026-02-14
created: 2026-05-19
description: "Agentic Coding 是什麼？這篇完全指南涵蓋定義、與 Vibe Coding 的差異、5 大工具比較、產品成熟度框架與入門路徑。幫你判斷該用什麼方法，從零開始建立工作流。"
tags:
  - "clippings"
status: processed
---
🌐

這篇文章也有英文版本 [Read in English →](https://yu-wenhao.com/en/blog/agentic-coding-guide/)

Vibe Coding 和 Agentic Coding 不是二選一。它們是產品不同階段該用的不同方法。

這篇指南幫你搞清楚：你的產品在哪個階段、該用什麼方法、怎麼開始。附產品成熟度框架、工具比較表、可複製的模板和 checklist。想看這個領域怎麼演化到今天的故事版，請讀 [這篇](https://yu-wenhao.com/zh-TW/blog/agentic-coding/) 。

---

## 先搞清楚一件事：這不是二選一

「Vibe Coding 和 Agentic Coding 哪個好？」

錯誤的問題。這就像問 3D printer 和射出成型哪個好——答案取決於你的產品在什麼階段。

我做了 10 年產品經理，最後幾年在車用電子。硬體產品有明確的成熟度階段，每個階段的製造方式和品質標準完全不同：

| 產品階段 | 目的 | 製造方式 | 品質標準 |
| --- | --- | --- | --- |
| Mockup | 對齊方向、給人看 | 3D printer | 外觀差不多就好 |
| A sample | 驗證功能可行 | 小量手工 | 功能對就好 |
| B sample | 驗證製程 | 接近量產製程 | 開始看精度、外觀瑕疵 |
| PVT | 量產驗證 | 量產線試跑 | DPPM、返工率、監控全上 |
| MP | 正式量產 | 量產線 | 全面品質管理 |

沒有人會用射出成型做 mockup——開一套模具只為了做 20 個樣品？也沒有人會用 3D printer 量產——精度不夠、品質不可控、無法規模化。

**不是哪個比較好，而是產品在那個階段，就該用那個階段的方法。**

軟體開發一模一樣：

| 軟體階段 | 對應硬體 | 開發方式 | 品質標準 |
| --- | --- | --- | --- |
| Demo / POC | Mockup | Vibe Coding | 能跑、能看就好 |
| MVP | A/B sample | Vibe → Agentic 過渡 | 核心功能穩定 |
| Production | PVT | Agentic Coding | 測試、安全、可維護 |
| Scale | MP | Agentic Coding | CI/CD、監控、自動化品質閘門 |

用 Vibe Coding 做 demo？完美。就像用 3D printer 做 mockup——快、便宜、夠用。

用 Vibe Coding 做要上線收費的產品？就像用 3D printer 量產——品質不可控，出了問題沒辦法追蹤，也沒辦法系統性地改善。

帶著這個框架，我們來看這兩種方法分別是什麼。

---

## Vibe Coding 是什麼？

2025 年初，Andrej Karpathy 創造了「Vibe Coding」——憑感覺寫程式。你用自然語言描述需求，AI 產出程式碼，跑跑看，能動就好。不看 diff、不讀程式碼、Accept All。

Collins Dictionary 選它為 2025 年度詞彙。Y Combinator 2025 冬季班有 25% 的新創用 AI 生成了 95% 的程式碼。

Vibe Coding 是 demo 和 POC 階段的最佳工具。就像 3D printer 之於 mockup——速度快、門檻低、目的是對齊方向，不是交付產品。

### Vibe Coding 工作流

```plaintext
你描述需求（自然語言）
  -> AI 生成程式碼
  -> 跑跑看
  -> 有錯？貼錯誤訊息回去
  -> AI 再改一版
  -> 重複直到能動
```

人類全程引導。每一步都需要你的輸入。

### 為什麼 Vibe Coding 在它的階段很好用

| 優勢 | 為什麼在 Demo/POC 階段是對的 |
| --- | --- |
| 極低門檻，非工程師也能用 | 方向對齊不需要工程專業 |
| 原型速度極快（小時級） | Mockup 就是要快 |
| 學習新技術的好方式 | 探索階段不需要完美 |
| 做 demo 和 POC 很適合 | 這就是它設計來做的事 |

### 為什麼不能拿它做量產

跟 3D printer 一樣——超出它的設計用途，問題就會出現：

| 硬體量產要求 | 軟體對應 | Vibe Coding 有嗎？ |
| --- | --- | --- |
| DPPM 追蹤（不良率管控） | 測試覆蓋率、bug 追蹤 | 沒有 |
| QC 站（進料 / 製程 / 出貨檢驗） | 自動測試、靜態分析、安全掃描 | 沒有 |
| 返工流程（不良品怎麼修） | 版本控制、diff 審查、rollback | 幾乎沒有 |
| 追溯機制（出問題查得到源頭） | Git 歷史、變更紀錄 | 沒有版控和文檔化習慣，出問題查不到源頭 |
| SPC 製程監控 | 效能監控、錯誤率追蹤 | 沒有 |

真實案例： [EnrichLead](https://ruinunes.com/vibe-coding-trap-ai-built-mvp/) 的創辦人公開宣稱產品 100% 由 Cursor AI 寫成、零手寫程式碼。上線兩天後被攻擊——API keys 暴露在前端、沒有認證機制、資料庫完全沒保護。最後關站。不是工具不好，是 mockup 階段的做法本來就沒有這些品質機制。

---

## Agentic Coding 是什麼？

當你的產品準備從 POC 走向 production，你需要的不再是「能跑就好」，而是「可維護、安全、有測試」。這就是 Agentic Coding 的用途。

如果 Vibe Coding 是 3D printer，Agentic Coding 就是量產線。量產線跟 3D printer 的差別不是機器比較貴，而是背後有一整套工程品質體系：

| 硬體量產機制 | 軟體對應 |
| --- | --- |
| **BOM + 工程圖面** （定義要做什麼） | 需求文件、Intent Spec、Acceptance Criteria |
| **SOP + 自動化產線** （做事方法一致、減少人工變異） | CLAUDE.md、開發規範、CI/CD pipeline |
| **IQC / IPQC / OQC** （進料、製程、出貨檢驗） | 自動測試、靜態分析、安全掃描 |
| **追溯 + 不良品處理 + 矯正預防** （出問題能查、能修、能防再犯） | Git 版本控制、bug tracking、更新規則避免再犯 |
| **SPC 製程監控** （持續追蹤穩定度） | 效能監控、錯誤率追蹤、uptime dashboard |

Agentic Coding 就是把這套體系搬到軟體開發——而 AI Agent 是在這個體系裡幫你執行的角色。

### 步驟沒變，人的角色變了

軟體開發的步驟沒有變——需求、設計、實作、測試、發布，從瀑布式到敏捷都一樣。變的是 **人在每個步驟裡的角色** ：

| 開發階段 | 傳統開發 | Agentic Coding | 人的參與 |
| --- | --- | --- | --- |
| **需求定義** | 人寫需求文件 | 人寫 Intent Spec | 高（這步沒變） |
| **設計 / 計畫** | 人設計架構 | 人審核 Agent 的計畫 | 中（從設計者變審核者） |
| **實作** | 人寫程式碼 | Agent 寫，人回答提問 | 低 |
| **測試** | 人寫測試、人跑測試 | Agent 自動寫和跑測試 | 低（看結果就好） |
| **審查** | 人逐行 code review | 人看 diff + Agent 標風險 | 中 |
| **發布** | 人執行部署 | Agent 執行，人最終核准 | 低 |

人的價值集中在 **頭尾** ——定義意圖和最終審查。中間的執行和驗證，交給 Agent。

### Karpathy 的最新定義：Agentic Engineering

2026 年 2 月，Karpathy 自己升級了概念：

> “Agentic”——因為 99% 的時間你不是自己寫程式碼，而是指揮 Agent 來寫。“Engineering”——強調這是一門需要專業技能的工作。

Vibe Coding 是好玩的週末專案。Agentic Engineering 是專業工作——以監督者的角色指揮 Agent，獲得槓桿而不犧牲品質。

---

## Vibe Coding vs Agentic Coding：什麼階段用什麼方法

### 產品階段決策表

回到產品成熟度的框架——你的產品在什麼階段，決定你該用什麼方法：

| 你的產品在… | 用 | 品質標準 | 類比 |
| --- | --- | --- | --- |
| **想法驗證** | Vibe Coding | 能跑、能看就好 | 3D printer mockup |
| **MVP 開發** | Vibe → Agentic 過渡 | 核心功能穩定 | A/B sample |
| **上線產品** | Agentic Coding | 測試、安全、可維護 | PVT 量產驗證 |
| **規模營運** | Agentic Coding | CI/CD、監控、自動化品質閘門 | MP 正式量產 |

### 從 Vibe 升級到 Agentic：你需要加入什麼？

當你的產品從 mockup 走向 production，這些是你需要加進來的：

| 你需要加入的 | 為什麼 |
| --- | --- |
| **需求與規格文件化** | Agent 需要明確的目標、驗收標準、測試規格和限制條件，才能交付正確的結果 |
| **版本控制 + 分支策略** | 出問題能回溯、能 rollback |
| **自動測試** | 不靠人眼驗證品質 |
| **Diff 審查習慣** | 知道 Agent 改了什麼、為什麼改 |
| **「設目標」取代「給步驟」** | 告訴 Agent「我要什麼」，不是「怎麼做」——讓 Agent 先出計畫，你審核計畫才讓它動手 |
| **監控與日誌** | 上線後要知道東西有沒有壞——錯誤日誌、效能監控、uptime dashboard |

### 經驗法則

**如果這個東西壞了會有人打電話給你，用 Agentic Coding。**

如果這個東西只是讓大家看看方向對不對，Vibe Coding 就夠了——而且更快。

---

## 2026 工具比較

### 五大工具比較

| 工具 | 支援的主要模型 | 月費起價 |
| --- | --- | --- |
| **Cursor** (Anysphere) | Claude、GPT (OpenAI)、Gemini | 20 美元/月起 |
| **Antigravity** (Google) | Gemini 3 Pro、Claude Sonnet 4.5、GPT-OSS | 免費（Preview） |
| **GitHub Copilot** (Microsoft) | Anthropic (Claude)、OpenAI、Google 等 | 免費起 |
| **Claude Code** (Anthropic) | Claude Opus 4.6 / Sonnet | 20 美元/月起 |
| **Codex CLI** (OpenAI) | GPT 系列 | 20 美元/月起 |

選工具時，最重要的不是介面形式。大部分工具都同時提供圖形介面和命令列——Claude Code 有 CLI 也有 VS Code extension，Copilot 有 IDE 外掛也有終端機版本，Cursor 本身就是一個完整的 IDE。

**真正決定工具上限的是背後的 LLM。** 模型的推理能力、上下文長度、程式碼理解深度，這些才決定 Agent 能做到什麼程度。同一個模型放在不同介面裡，能力是一樣的。所以上面的表格才把「支援的主要模型」放在第一欄。

成本方面，大部分工具落在每月 10-20 美元起跳，差別在用量額度和進階功能。 [Cursor](https://www.cursor.com/pricing) 分三檔（20/60/200 美元）， [Claude Code](https://claude.com/pricing) 也分三檔（20/100/200 美元）， [GitHub Copilot](https://github.com/features/copilot/plans) 個人版 10 美元起、企業版每人 19 美元。 [Codex CLI](https://openai.com/codex/) 開源但需要 ChatGPT 訂閱（20 美元起）或 API 按量。 [Antigravity](https://antigravity.google/pricing) 目前 Preview 免費但有週配額限制，正式版預期會推出付費方案。

### 怎麼選？

**先選模型，再選工具。** 你想用哪家的 LLM，就從那家的工具開始——Anthropic 的 Claude 選 Claude Code 或 Cursor，OpenAI 的 GPT 選 Codex CLI 或 Copilot，Google 的 Gemini 選 Antigravity。想要彈性切換模型，Cursor 和 Copilot 都支援多家。

**再看預算。** 想先試水溫？Antigravity 免費 preview、GitHub Copilot 有免費方案。準備投入？大部分工具 20 美元/月就能開始。重度使用再考慮更高的方案。

**最後看生態。** 團隊已經在用 VS Code + GitHub？Copilot 整合最無痛。偏好 Google 生態？Antigravity。

**我的選擇：** 我用 Claude Code。原因很簡單——我重度使用 Anthropic 的模型，而企業在 coding 領域選擇 Claude 的可能性是 OpenAI 的 [2.3 倍](https://www.uncoveralpha.com/p/anthropics-claude-code-is-having) 。不只 Claude Code，Cursor、GitHub Copilot、Antigravity 背後都支援 Claude 作為模型選項。既然決定用 Anthropic 的模型，沒有理由不選他們自家的工具。

---

## 如何開始你的 Agentic Coding 之路

開始之前，先調整兩個心態：

第一，Agentic Coding 的時間分配是 **80% 想 + 審查、20% 跟 Agent 溝通、0% 自己寫程式碼** ——跟傳統開發的 80% 寫程式碼正好相反。

第二，下面的練習會從很簡單的任務開始——簡單到做法跟 Vibe Coding 幾乎一樣。這是刻意的。我們先用小任務練習「設目標 → 審計畫 → 看結果」的流程。當你的專案變大、要上線給真的使用者用，你就需要刻意學習更多工程思維和工具——測試、版控、安全檢查——這些才是 Agentic Coding 跟 Vibe Coding 真正拉開差距的地方。

四個步驟，從零開始。

### Step 1：安裝工具

回到上一節的建議：先選模型，再選工具。我用 Claude Code，寫過一篇 [Claude Code 教學：5 分鐘完成安裝與第一個任務](https://yu-wenhao.com/zh-TW/blog/claude-code-tutorial/) 。

### Step 2：建立指令檔，讓 Agent 認識你的專案

Agentic Coding 的效果取決於你給 Agent 多少上下文。最重要的設定：專案根目錄的 **指令檔** ——Claude Code 叫 CLAUDE.md，Cursor 叫 `.cursorrules` ，概念一樣。

把它想成新工程師的 onboarding 文件——你希望他第一天就知道的所有事情。

不需要一次寫完。先寫三行就夠用了：

**Day 1：三行就夠**

```markdown
# 我的小工具集

## Rules（給 Agent 的規則）
- 用台灣繁體中文和我溝通，技術名詞可以用英文
- 每個工具用一個檔案完成，雙擊就能在瀏覽器打開
- 不要用任何需要安裝的工具
```

就這樣。Agent 知道用什麼語言跟你溝通、東西要多簡單、什麼不能碰。

**一週後：每次 Agent 犯錯，你加一條規則**

比如 Agent 做出來的頁面在手機上跑版了、或是按鈕太小很難按，你就加一條：

```markdown
# 我的小工具集

## Rules（給 Agent 的規則）
- 用台灣繁體中文和我溝通，技術名詞可以用英文
- 每個工具用一個檔案完成，雙擊就能在瀏覽器打開
- 不要用任何需要安裝的工具
- 手機上也要能正常使用，按鈕不能太小
- 表單送出後要清空輸入欄位
- 數字欄位只能輸入數字
```

每一條新增的規則，都是因為 Agent 犯了一次錯。 **指令檔是活的，它會跟著你的經驗自然長大。** 當你的專案從小工具長成正式產品，這個檔案也會跟著長大——加入技術棧、指令、文件位置等更多細節。

### Step 3：執行你的第一個 Agentic 任務

選一個小任務。不要一開始就挑戰大工程——從「做一個小工具頁面」開始。

關鍵心態： **告訴 Agent「我要什麼」，不是「怎麼做」。** Agent 的產出品質，直接取決於你定義目標的品質。

我每天都用一種叫 **Intent Spec** 的格式來描述任務。它有四個區塊：

- **Goal** ——你要什麼（結果，不是步驟）
- **Acceptance Criteria** ——怎樣算做完（可以打勾的清單）
- **Constraints** ——有什麼限制
- **Non-goals** ——明確不要什麼（防止 Agent 自作主張多做）

直接看範例——做一個個人記帳頁面：

```markdown
## Goal
做一個個人記帳頁面。使用者可以輸入每筆花費（品項 + 金額），
頁面即時顯示所有紀錄和總金額。

## Acceptance Criteria
- [ ] 頁面有一個表單：品項（文字）和金額（數字）兩個欄位
- [ ] 按下「新增」後，紀錄出現在下方列表
- [ ] 列表顯示每筆的品項、金額
- [ ] 頁面底部即時顯示總金額
- [ ] 每筆紀錄有「刪除」按鈕
- [ ] 一個檔案搞定，不需要安裝任何東西

## Constraints
- 只用一個檔案完成，雙擊就能在瀏覽器打開
- 不要用任何需要安裝的工具
- 在手機上也要能正常使用

## Non-goals
- 不需要記住資料（重新整理頁面會清空，沒關係）
- 不需要分類或圖表
- 不需要支援不同貨幣
```

注意這個 spec 在做什麼——它只描述 **用起來是什麼感覺** （輸入花費、看到列表、總額會更新），完全沒說要怎麼做。Agent 會自己決定技術細節。你只管結果對不對。

你把這段 Intent Spec 直接貼給 Agent，但先別讓它動手做——先叫它出計畫。在 Intent Spec 最後加一句：「先給我計畫，不要直接寫。」流程：

1. 把 Intent Spec 貼給 Agent，結尾加上「先給我計畫，不要直接寫」
2. Agent 會列出它打算怎麼做——用什麼結構、分幾個部分
3. 你看一下：方向對嗎？有沒有多做或少做？
4. 沒問題就回覆「OK，開始做」
5. Agent 會開始寫程式，完成後告訴你檔案在哪
6. 打開做好的檔案，輸入幾筆花費試試看

### Step 4：建立肌肉記憶

第一次可能覺得寫 Intent Spec、看計畫很麻煩——直接跟 Agent 說「幫我做一個記帳頁面」不是更快嗎？

更快，但 Agent 做出來的東西可能不是你要的，然後你來回修改反而更久。Intent Spec 就是前面提到的 **需求與規格文件化** ——Agentic Coding 的第一步。把目標寫清楚，Agent 一次做對的機率高很多。

記帳頁面只是練習。同一套迴圈可以處理更複雜的任務——串股市 API 把即時報價顯示在頁面上、做一個 Google 登入系統、建一個有資料庫的完整 Web App。任務越複雜，寫好 Intent Spec 省下的來回修改就越多。

### Step 5：從 Vibe 跨到 Agentic

到 Step 4 為止，你做的事情跟 Vibe Coding 差別不大——唯一多的是 Intent Spec。現在要跨過那條線。

假設你決定做一個自己的個人網站——有首頁、關於我、作品集、部落格。這不是做完就丟的小工具，而是要上線給別人看、會一直改的東西：

- 你會持續加文章、換作品、調風格
- 改了作品集頁面， **首頁不能跟著壞掉**
- 朋友傳了你的網址給別人， **打開不能是一片空白**
- 三個月後你要改東西， **要記得當初為什麼這樣寫**

記帳頁面壞了，重做就好。個人網站壞了，你的專業形象跟著壞。這就是為什麼你需要從 Vibe Coding 升級到 Agentic Coding。

回到前面那張「從 Vibe 升級到 Agentic」的表，你已經完成了「需求與規格文件化」（Intent Spec）。接下來三件事：

**1\. 版本控制——改壞了可以退回去**

跟 Agent 說：「幫我把這個專案用 Git 管理，每次改動都記錄下來。」

你叫 Agent 改了部落格的排版，結果首頁跑版了？退回上一個版本就好，不用從頭再來。三個月後想改東西，可以看到每一次改動的紀錄——什麼時候改的、改了什麼、為什麼改。

這就是前面提到的「追溯機制」。Vibe Coding 沒有這個，出問題只能憑記憶猜。

**2\. 自動測試——確認舊功能沒壞**

跟 Agent 說：「幫網站寫測試：每個頁面都能正常打開、所有連結都能點到、圖片都能顯示。」

你的網站有首頁、關於我、作品集、部落格，每篇文章又有自己的頁面——改了一個地方，要確認其他十幾個頁面都沒壞。手動點一輪要十分鐘，而且一定會漏。自動測試幾秒鐘跑完，每個頁面都檢查到。

以後每次改東西，Agent 先跑測試，全部通過才算完成。測試沒過？Agent 自己修，修到過為止。

**3\. Diff 審查——知道 Agent 改了什麼**

你叫 Agent 在作品集加一個新專案，完成後跟它說：「給我看你改了哪些地方。」

Agent 會列出它動了哪些檔案、改了什麼。你要能判斷：它只動了作品集頁面嗎？還是不小心改到了首頁的導航列、或是部落格的樣式？

改一個地方卻影響了不相關的頁面——這是網站最常見的問題。Diff 審查讓你在上線前就抓到。

進階一步：你還可以跟 Agent 說「每次改完自動跑測試，測試過了才讓我看 diff」。這樣 Agent 交到你手上的已經是「至少沒有明顯問題」的版本，你只需要判斷方向對不對。

這三件事加進來，你就不只是在「用 AI 做東西」，而是在「用工程方法管理 AI 做的東西」。這就是 Agentic Coding。

同樣的工程實踐，放到更大的場景也一樣適用——串 API 的股市看板、有登入系統的記帳 App、甚至是收費的 SaaS 產品。專案越大，這些實踐省下的時間和避免的災難就越多。

### 熟練之後

當基本迴圈穩定了，Agentic Coding 可以延伸到寫程式以外的事：

**建立自己的 Skill——把流程和規則獨立出來**

隨著你用越久，指令檔會越來越長——規則、SOP、不同任務的流程全擠在一個檔案裡，Agent 反而搞不清楚重點。這時候你會想把特定流程拆成獨立的 Skill：部署網站一套 SOP、寫部落格文章一套 SOP、跑測試一套 SOP。需要的時候才叫出來，指令檔保持乾淨。

**串接外部服務——讓 Agent 幫你做更多事**

Agent 不只能寫程式碼，還能串接你日常用的工具。例如：讓 Agent 讀你的 Google Calendar 自動整理今天的待辦、串股市 API 每天早上寄一封投資摘要到你的信箱、追蹤你關注的 KOL 有沒有發新內容然後整理成摘要。這些不是在寫產品，而是在用 Agentic Coding 打造你自己的自動化工作流。

**串接你的數位筆記——讓 Agent 讀懂你的知識庫**

如果你用 Obsidian、Notion 或其他筆記工具，可以讓 Agent 存取你的筆記庫。這樣當你跟 Agent 討論一個新專案時，它能參考你之前寫的筆記、研究和想法——不用每次從零開始解釋背景。你的知識庫變成 Agent 的長期記憶。我自己就是這樣做的—— [從 Roam Research 搬到 Obsidian](https://yu-wenhao.com/zh-TW/blog/roam-research-to-obsidian/) 之後，Agent 可以直接讀我的筆記庫。

到這裡你手上有了完整的工具：產品階段框架幫你判斷該用什麼方法、Intent Spec 幫你定義目標、工程實踐幫你守住品質。剩下的就是做東西。

做著做著你會發現，同樣的迴圈——設定目標、讓 Agent 執行、審查結果、迭代改進——不只能用在寫程式。我用它 [打造整套 AI 工作流](https://yu-wenhao.com/zh-TW/blog/personal-panopticon/) ——管理目標、管理知識、管理每天的工作。寫程式只是起點。

---

### 延伸閱讀

Agentic Coding 之外、整套 AI 工作系統怎麼建？ [多數人學 AI 的順序反了——先方向、再系統，AI 才是槓桿](https://yu-wenhao.com/zh-TW/blog/flux-direction-system-leverage/) ，這是我把 FLUX 系統教給 12 個學員的第一手紀錄。

*如果這篇讓你有了想法， [訂閱每週一封信](https://yu-wenhao.com/zh-TW/) ——我固定寫 AI 工作流、和一路上想通的事。*

*想聊聊怎麼把 AI 融入你的工作流？ [看看我的服務](https://yu-wenhao.com/zh-TW/services/) 。*

#AI #一人公司 #Claude Code #agentic coding #vibe coding #developer-tools

## 常見問題

不會寫程式也能開始 Agentic Coding 嗎？

可以。本文的入門路徑就是為非工程師設計的——從寫 Intent Spec 開始，不需要自己寫程式碼。但要做好產品需要逐步學習架構思維和品質判斷。建議從 Vibe Coding 開始體驗，再升級到 Agentic Coding。

Intent Spec 一定要用英文寫嗎？

不用。用你最熟悉的語言寫就好，Agent 看得懂中文。重點是把目標、驗收標準、限制條件和不做的事寫清楚，語言不是障礙。

Agentic Coding 每月要花多少錢？

大部分工具每月 20 美元起。Claude Code、Cursor、Codex CLI 都在這個價位，GitHub Copilot 個人版 10 美元起。想先免費試，Antigravity 目前 Preview 免費、GitHub Copilot 有免費方案。

CLAUDE.md 指令檔要寫多長？

三行就能開始，官方建議不超過 300 行。判斷原則：對每一行問「拿掉這行，Agent 會出錯嗎？」不會就刪。檔案太長時，把特定主題的規則拆成獨立檔案，主檔案只放最重要的東西。

怎麼確認 Agent 寫的程式碼沒問題？

核心原則：給 Agent 驗證自己的方式。在 prompt 裡附上測試案例、預期輸出或截圖，讓它跑完自己確認。第二層：用獨立的 session 或 subagent 做 code review，不要讓同一個 Agent 批改自己的作業。第三層：自己用使用者角度操作一遍。如果你沒辦法驗證它，就不該上線。

我已經在用 Vibe Coding 了，什麼時候該升級到 Agentic？

五個信號中了三個就該升級：改 A 壞 B、不確定上次改了什麼、出問題只能整個重做、同樣的 bug 修了又出現、不敢碰別人（或三個月前自己）寫的程式碼。一句話：如果這個東西壞了會有人打電話給你，就該升級了。