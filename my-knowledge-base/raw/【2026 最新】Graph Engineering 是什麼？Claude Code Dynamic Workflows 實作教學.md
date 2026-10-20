---
title: "【2026 最新】Graph Engineering 是什麼？Claude Code Dynamic Workflows 實作教學"
source: "https://www.alphalab.site/graph-engineering"
author:
  - "[[Terry Chen]]"
published: 2026-07-20
created: 2026-07-29
description: "一個 prompt 只是入口。從節點、真實相依邊、驗證錨點，到 Claude Code Dynamic Workflows 的完整 5 步實作與風險守門。"
tags:
  - "clippings"
status: processed
---
台北時間 2026 年 7 月 22 日，一篇以「Graph Engineering」與「在一個視窗、用一個 prompt 建出 1000+ agent loops」為主題的 X 長文，讓 **Graph Engineering** 再次衝進 AI 圈的視野。先把關鍵誤會說清楚：依 [Claude Code 官方文件](https://code.claude.com/docs/en/workflows) ，Dynamic Workflows 的硬上限是 **每次最多 1,000 個 agents、同時最多 16 個** ，不是 1,000+ 個一起跑；「一個 prompt」也只是啟動入口，背後仍是許多 Agent 各自工作。

但去掉聳動包裝，原文抓到了一個很重要的能力：有些可拆解 AI 任務的瓶頸，可能不在模型能力，而在你把原本能平行的工作排成一條隊。這篇更新版 **Graph Engineering 教學** 不要求你先會寫程式，會從「節點、連線、驗證錨點」三個白話概念開始，再帶你用 Claude Code Dynamic Workflows 跑出第一張真正可用的工作圖。

你會學到：怎麼判斷哪些步驟真的需要等待、怎麼讓 workers 分頭做事又不互踩、怎麼用測試與原始來源阻止整張圖一起自我感覺良好，以及什麼時候一個普通 Agent 反而比較好。

## 先說結論：Graph Engineering 到底是什麼？

在本文裡， **Graph Engineering** 是一個操作性定義：你不只寫 prompt，而是設計工作的形狀——誰先做、誰能同時做、誰要等誰、錯了回哪裡、最後由什麼證據判定完成。本文沿用原 X 長文的名稱；Anthropic 的產品叫 Dynamic Workflows，LangGraph 的文件則使用 Graph API。

**🕸️ Graph Engineering ＝ Nodes（誰做）＋ Edges（誰真的要等誰）＋ Anchors（什麼才算真的做完）**

- **Node（節點）** ：一個可交付的工作單位，例如「列出檔案」「審查一個檔案」「跑測試」。
- **Edge（邊）** ：真實的相依關係。B 需要 A 的輸出，或兩者會爭用同一份可變狀態、受限資源與權限時，才應該用等待或其他同步機制協調。
- **Anchor（錨點）** ：可重現、可由外部工具或來源獨立稽核的完成證據，例如真的跑過的測試、編譯器結果、資料庫查詢或附日期的原始文件。

一句白話： **節點負責做事，邊負責協調，錨點負責讓結果接受獨立稽核。**

## Graph Engineering 和 Loop Engineering 差在哪？

[Loop Engineering](https://www.alphalab.site/loop-engineering/) 解決的是「同一件事怎麼反覆改進」：嘗試 → 檢查 → 修正 → 再來一次。Graph Engineering 解決的是「很多工作怎麼協調」：哪些 loop 可以同時跑、哪些結果要合流、哪些失敗要退回重做。

![Graph Engineering 概念圖：從單一 Loop 到有目標、驗證與真實數據錨點的工作圖](https://hacktechentertainment-iylwc.wpcomstaging.com/wp-content/uploads/2026/07/graph-engineering-02-loop-vs-graph.png)

Loop 是反覆改善一件事；Graph 是安排很多工作如何分流、等待、驗證與合流

兩者不是新舊替代關係。 **一張 graph 裡可以住很多 loops** ；「修到測試通過為止」本身就是有迴圈的圖。因此 Graph Engineering 也不必然等於 DAG（有向無環圖）。像 [LangGraph 官方 Graph API](https://docs.langchain.com/oss/python/langgraph/graph-api) 就明確支援 looping workflow。

一個實用的判斷題是： **下一步真的會讀取上一步的輸出嗎？** 「摘要這份檔案，然後查天氣」若也沒有共享資源限制，兩件事可以平行；「先列出檔案，再逐檔審查」就有真實的邊，盤點必須先完成。

## Graph Engineering 的 5 個設計動作

### ① 先找「真的邊」：沒有資料流，就別讓它空等

**痛點：** 人類寫需求時習慣一直說「然後」，但語句順序不等於資料依賴。 **解法：** 替每一步寫出輸入與輸出；如果 B 不讀 A 的輸出，兩者也不會改同一份狀態、搶同一個受限資源、碰到相同權限或 rate limit，才刪掉那條等待線。 **效果：** 原本 12 個獨立檔案的 12 段排隊，可以交給 runtime 分批平行處理；但不會因為有 12 個 Agent 就自動得到 12 倍速度，最長的相依鏈與最慢的分支仍會限制總時間。

### ② 把 Node 寫成「有合約的工作包」

**痛點：** 「幫我看看有沒有問題」太模糊，20 位 workers 可能交回多種不一致格式。 **解法：** 每個 node 都要有明確範圍、輸入、輸出 schema 與停止條件，例如「只審一個檔案；回傳檔名、精確行號、風險、可重現步驟」。 **效果：** 結果能被程式去重、排序與驗證，而不是再丟給另一個 AI 猜格式。

### ③ 在獨立處 Fan-out，在相依處 Fan-in

**痛點：** 把整個專案塞進一個 context，可能提高遺漏風險；讓所有 Agent 同時改同一檔案，又會互相覆蓋。 **解法：** 讀取型任務可依檔案、來源或研究角度 fan-out；結果需要彙整時再 fan-in。寫入型任務則要先分配檔案所有權，必要時使用 [獨立 worktree](https://code.claude.com/docs/en/worktrees) 。 **效果：** 每位 worker 專注一小塊，合流點只處理結構化成果。

### ④ 把 Verifier 放在 Edge 上，但要看證據

**痛點：** 讓執行者自己說「我完成了」，只是自評；換一個 Agent 用同樣模糊標準再看一次，也不等於真相。 **解法：** 讓 verifier 使用獨立 context，並要求它檢查可重現 evidence：測試是否真的通過、引用是否真的支持主張、行號是否真的存在。 **效果：** 驗證從「另一個意見」變成「一道可以擋住錯誤的 gate」。

### ⑤ 技術性強制 Anchor：別只靠 prompt 守住及格線

**痛點：** 如果 Agent 能把「全部測試通過」改成「大部分看起來可以」，再漂亮的 graph 都會優化錯目標；只在 prompt 寫「不准修改」也不等於真的不可修改。 **解法：** 用 CI gate、sandbox、tool permissions、唯讀外部資料或 worker 無權變更的設定，技術性強制測試、權限、成本上限與來源規則。 **效果：** 整張圖必須接受外部證據約束，而不是讓一群模型互相按讚。

## 完整走一次：路由權限稽核的工作圖

假設你要檢查 `src/routes/` 底下哪些路由漏了驗證。線性做法是一個 Agent 依序讀完所有檔案；Graph Engineering 的做法則是「先盤點一次 → 每個檔案獨立審查 → 每個發現獨立驗證 → 合成一份報告」。

![Graph Engineering 實作圖：盤點、Fan-out 平行審查、獨立驗證、收斂報告與測試錨點](https://hacktechentertainment-iylwc.wpcomstaging.com/wp-content/uploads/2026/07/graph-engineering-05-worked-graph.png)

一個 prompt 只是入口；真正的價值是背後四段可檢查、可重跑的工作圖

1. **盤點：** 一個 Agent 先輸出最多 20 個檔案的結構化清單。
2. **Fan-out：** `pipeline()` 對清單排程，每個檔案交給一位審查 Agent；runtime 依資源最多同時跑 16 位。
3. **驗證：** 新 Agent 不接收執行者的長篇自我解釋，只拿「主張＋檔案＋行號＋測試方式」去反駁。
4. **收斂：** 去重、排序，只保留 verifier 判定通過的發現，最後回到主對話的只有一份報告；重要結論仍要碰測試或原始來源。

## Claude Code Dynamic Workflows：5 步開始實作

Anthropic 在 2026 年 5 月 28 日推出 Dynamic Workflows，之後已標示為 generally available。它會依你的描述生成可閱讀、可重跑的 JavaScript 編排腳本，讓腳本持有 branch、loop 與中間變數；Agent 負責讀檔、寫檔與跑工具。這是可以親手體驗 Graph Engineering 的方式之一。

### Step 1：安裝、更新並確認版本

macOS、Linux 或 WSL 可依 [Claude Code 官方安裝文件](https://code.claude.com/docs/en/installation) 執行 `curl -fsSL https://claude.ai/install.sh | bash` 。若你也是用這個 native installer 安裝，可執行 `claude update` ；其他安裝方式請用原本的套件管理器更新。最後以 `claude --version` 確認版本不低於功能門檻 **v2.1.154** ；截至 2026 年 7 月 26 日， [官方 changelog 最新一筆是 v2.1.220](https://code.claude.com/docs/en/changelog#2-1-220) 。

### Step 2：確認方案與開關

截至 2026 年 7 月 26 日，Dynamic Workflows 適用 Claude Code 的付費方案與 API／支援的雲端供應商。Max、Team、Enterprise 預設開啟；Pro 使用者可在 `/config` 的 Dynamic workflows 列開啟。組織管理員仍可能透過 managed settings 關閉它。

### Step 3：先貼一個「讀取型、小範圍」prompt

在你熟悉的 repo 裡啟動 `claude` ，把路徑換成自己的。第一次不要直接讓 100 個 Agent 改檔；先做只讀稽核，限制 20 個檔案：

```
use a workflow to audit every route file under src/routes/
for missing authentication checks.
Run an independent verifier on each finding before reporting.
Do not edit any files.
Analyze at most 20 files for the first run.
```

完全不想碰程式碼，也可以先試官方內建的研究型 `/deep-research 你的問題` 。它會把研究拆成多個角度、平行找來源、交叉檢查，再回傳一份有引用的報告；前提是目前環境有 WebSearch 工具。這描述的是任務用途，不代表 permission-enforced 的只讀安全邊界。

### Step 4：先讀計畫，再依權限模式核准

核准流程取決於 permission mode：Default 與 acceptEdits 通常會顯示 phases 並要求確認；Auto 在首次 launch 時確認，同意會寫入 user settings，後續可能不再問；Auto 搭配 ultracode 時會直接略過。bypass、 `claude -p` 與 SDK 也可能不出現相同提示。看到核准畫面時，不要只看名字：確認每個 Agent 的工作範圍、哪些地方會寫檔、如何合併、驗證靠什麼，並可選 `View raw script` 。跑起來後輸入 `/workflows` ，可以看到各 phase 的 Agent 數、token、耗時與結果。

![Claude Code Dynamic Workflows 官方進度畫面，顯示 phases、agents、token、tools 與執行時間](https://hacktechentertainment-iylwc.wpcomstaging.com/wp-content/uploads/2026/07/graph-engineering-06-official-workflow.png)

Claude Code 官方產品公告的 workflow 進度畫面。圖片來源： Anthropic ；畫面內模型欄位是公告截圖快照，不代表你目前帳號的預設設定。

### Step 5：做對一次，再保存形狀

完成後進入 `/workflows` ，選取該 run 並按 `s` 。你可以存到專案的 `.claude/workflows/` ，讓團隊一起版本控制；個人路徑預設是 `~/.claude/workflows/` ，若設定 `CLAUDE_CONFIG_DIR` 則跟著改到該目錄下。若保存的腳本有讀取 `args` ，下次可只換「要查什麼」，保留盤點 → 展開 → 驗證 → 收斂的形狀；沒有的話就要先修改腳本。

## 腳本裡發生什麼？三段偽代碼看懂

你通常不必手寫 workflow，但看懂骨架會讓你更敢按下執行。官方範例的核心形狀可以縮成：

```
const found = await agent("列出 src/routes 下的檔案", { schema })

const audits = await pipeline(found.files, file =>
  agent(\`審查 ${file} 的驗證缺口\`)
)

return audits.filter(Boolean)
```

`agent()` 是一個節點；第一個 `await` 是真實相依邊； `pipeline()` 把檔案清單展開成 workers；最後的 `return` 是 fan-in。若要加 verifier，就在 audits 後再跑一層只接受結構化 findings 的驗證 pipeline。

## Subagents、Agent Teams、Dynamic Workflows 怎麼選？

三者都能做多 Agent，但計畫放的位置不同。Subagents 適合主 Claude 一輪一輪派少數獨立任務；Agent Teams 由 lead agent 管理長時間 peers；Dynamic Workflows 把編排寫進腳本，適合大量、形狀固定、需要重跑的工作。

![Claude Code Dynamic Workflows、Agent Teams、Subagents 比較圖：誰決定下一步、中間結果存放位置與適合任務](https://hacktechentertainment-iylwc.wpcomstaging.com/wp-content/uploads/2026/07/graph-engineering-07-tool-choice.png)

選擇關鍵：主對話逐輪決定、lead agent 即時協調，還是可讀可重跑的腳本持有整張圖

注意：截至 2026 年 7 月 26 日，Agent Teams 官方仍標示為 experimental；Dynamic Workflows 已 generally available。若你只是想讓兩位 Agent 分別查資料，不必為了流行詞硬升級成 workflow。

## 7 個常見坑：Graph 會把好設計放大，也會把壞設計放大

1. **把「最多 1,000」讀成「1,000+ 同時」。** 官方是每個 run 總計最多 1,000 個 agents、最多 16 個 concurrent；CPU 較少時還會更低。
2. **把「一個 prompt」讀成「一次模型呼叫」。** prompt 只是按下總開關；每位 Agent 都有自己的工作與用量。
3. **相信「零 token 編排」。** 中間結果留在 script variables，確實不用全部塞回主對話；但官方明確提醒，workflow 可能比一般 session 使用更多 token。
4. **把 worktree 當萬靈丹。** 它能隔離檔案寫入，不能消除兩個 Agent 對同一需求、API、資料庫或 schema 的語義衝突。
5. **讓 verifier 只評語氣。** 乾淨 context 只能降低直接污染，不能保證正確；測試、編譯器與原始來源才是 anchor。
6. **以為可以隨時插話批准下一階段。** workflow 執行中不接受一般使用者輸入；若需要每階段人工簽核，應拆成多個 workflows。
7. **把中斷恢復當成無條件保證。** 目前官方資料的文字不完全一致：產品公告與 [v2.1.196 版本紀錄](https://github.com/anthropics/claude-code/releases/tag/v2.1.196) 說長任務會保存進度、程序停止或重啟後可接續；workflow 指南仍寫 paused workflow 只在同一 session 恢復。請依實際版本與中斷方式先做小規模測試，重要長任務仍要把副作用設計成可重跑。

還有一個安全細節：workflow 腳本本身不直接碰 filesystem 或 shell，但它叫出的 Agents 會繼承你的 tool allowlist，並固定以 acceptEdits 模式執行，所以檔案編輯會自動核准。長任務開始前，務必讀 raw script、縮小路徑與權限，寫入型工作優先隔離。

## 什麼時候 Graph Engineering 是錯的選擇？

Anthropic 在 [Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) 的長期建議很一致：先用最簡單、可組合的模式，只有收益能抵過複雜度時才升級。以下四種情況，先不要畫大圖：

- **任務很小：** 改一個函式、修一個明確 bug，單一 Agent 通常協調成本較低。
- **問題還沒看懂：** 探索階段需要你持續追問與轉向，不適合讓一支艦隊先鎖死計畫。
- **每一步真的依賴前一步：** 若沒有兩個互不相依的框，graph 沒有可利用的「寬度」。
- **需要逐步人工核可：** workflow 的強項是背景廣度；嚴格 stage gate 應拆段執行。
![Graph Engineering 決策階梯：直接對話、單一 Loop、配對驗證與工作圖框架](https://hacktechentertainment-iylwc.wpcomstaging.com/wp-content/uploads/2026/07/graph-engineering-04-decision-ladder.png)

複雜度要用證據換：一次性工作先直接對話，會重複才造 loop，需要大量獨立分工才上 graph

## ❓ Graph Engineering 常見問題 FAQ

### Q1：本文所說的 Graph Engineering 指什麼？

**這是本文沿用原作者用語建立的操作性定義。** 它用來一起理解 agent graph、workflow 與 orchestration；Anthropic 的功能名稱是 Dynamic Workflows，其他工具採用自己的產品術語。

### Q2：Graph 就是 DAG 嗎？

**不一定。** DAG 不能繞回去；Agent workflow 常包含「失敗 → 修正 → 再測」的 loop，因此 graph 可以有 cycle。

### Q3：我不會寫程式，也能用 Dynamic Workflows 嗎？

**可以。** 你可以直接說 `use a workflow` ，由 Claude 生成腳本；第一次可先做研究型 `/deep-research` ，或明確要求不改檔的小範圍 audit。你仍要看懂工作範圍、權限與證據標準。

### Q4：免費 Claude 帳號能用嗎？

**Claude Code 本身需要付費方案或 API／支援的雲端帳號。** Dynamic Workflows 適用付費方案；Pro 需確認 `/config` ，組織政策也可能關閉。

### Q5：真的能同時跑 1,000 個 Agents 嗎？

**不能。** 官方是每次 run 總計最多 1,000 個，最多 16 個同時工作；其餘會分波排程，且本機 CPU 可能讓並行數更低。

### Q6：一個 prompt，費用就跟一般回答差不多嗎？

**不會這樣算。** 入口只有一個 prompt，但背後可能啟動很多 Agents。請在 `/workflows` 看 token；本文示範先限制 20 個檔案，你也可以用更小樣本量一次，再決定要不要放大。

### Q7：我可以關掉終端機，明天從原地繼續嗎？

**不要先假設一定可以或一定不行。** Anthropic 的 [產品公告](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code) 與 [v2.1.196 版本紀錄](https://github.com/anthropics/claude-code/releases/tag/v2.1.196) 寫進度會保存，程序停止或重啟後可接續；但目前 workflow 指南仍保留「paused workflow 只在同一 session 恢復」的說明。跨終端機、跨程序或跨日使用前，請用你的版本實測，並讓寫入操作可安全重跑。

### Q8：多 Agent 一定比單 Agent 更準嗎？

**不一定。** 多 Agent 能增加搜尋廣度，也會增加成本、合併與共同犯錯的機會。品質來自獨立證據、權限隔離與可重現測試，不是 Agent 人數。

## 🎯 給新手的 5 個重點

1. **先畫依賴，再叫 Agent。** 沒有真實資料流，也不共享狀態、受限資源、權限或 rate limit 的工作，才適合拿掉等待。
2. **Graph 不會取代 Loop。** Loop 是節點裡的改善迴圈，Graph 是節點之間的交通規則。
3. **平行前先定 ownership。** 讀取可按檔案展開；寫入要隔離 workspace 與合併規則。
4. **Verifier 必須碰證據。** 測試、編譯器、精確行號、原始來源，比另一個 AI 的「看起來沒問題」可靠。
5. **先用小樣本量測用量。** 本文示範限制 20 個檔案；一個 prompt 可以啟動很多工作，也可以啟動一張很大的帳單。

## 📚 延伸閱讀

- [Loop Engineering 循環工程是什麼？](https://www.alphalab.site/loop-engineering/) ——先學會讓一件事反覆改善，再把多個 loops 連成 graph。
- [AI Agent Harness 是什麼？](https://www.alphalab.site/ai-agent-harness/) ——理解 Agent ＝ Model ＋ Harness，補齊 workflow 執行層的地基。
- [30 行偽代碼自己蓋一個 Agent Harness](https://www.alphalab.site/build-ai-agent-harness/) ——把最小 agentic loop 從頭走一次。
- [Claude Code Subagents 與 Agent Teams](https://www.alphalab.site/claude-code-subagents-team/) ——看懂多 Agent 的另外兩種控制方式。
- [Claude 省 token 的 10 招](https://www.alphalab.site/how-to-save-claude-tokens/) ——Workflow 放大前，先把範圍與 context 管好。
- [Claude Code vs Codex 客觀比較](https://www.alphalab.site/claude-code-vs-codex/) ——依工作位置、Agent loop 與操作方式選工具。
- 更多教學： [AlphaLab AI 專區](https://www.alphalab.site/category/ai/) ｜想系統性學習： [AlphaLab 課程](https://www.alphalab.site/courses/)

## 結語：不要只會加 Agent，要會畫「誰等誰」

Graph Engineering 值得帶走的，不是「一個視窗塞 1,000 個 Agent」，而是把模糊的「然後」改成可檢查的工作圖： **節點有合約、邊代表真實相依與共享限制、錨點接受外部稽核。**

下次你準備叫 AI 做一長串事，先停三秒問：哪兩個步驟其實互不相干？哪一個結果一定要被測試？哪兩位 workers 可能寫到同一個地方？能回答這三題，你就已經不是只會下 prompt 的人，而是在設計一個能安全擴張的系統。

## 免責聲明與資料來源

本文僅供教育與技術研究，資料查核至 **2026 年 7 月 26 日** 。主要來源包括： [Codila 原始 X 長文](https://x.com/0xCodila/status/2079597821511020996) （作為結構與主題線索）、 [Claude Code Dynamic Workflows 官方文件](https://code.claude.com/docs/en/workflows) 、 [Anthropic 產品公告](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code) 、 [Claude Code v2.1.196 版本紀錄](https://github.com/anthropics/claude-code/releases/tag/v2.1.196) 、 [Claude Code 安裝文件](https://code.claude.com/docs/en/installation) 、 [Worktrees 官方文件](https://code.claude.com/docs/en/worktrees) 與 [Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) 。

功能、版本、方案與介面會變動，實作前請以最新官方文件與你的組織設定為準。AI 具有輸出錯誤資訊的可能，安全、權限與重要決策請由人類複核。文中偽代碼為易讀優先的教學寫法，省略型別、錯誤邊界與 API 細節，不可直接複製當作正式程式碼；實作請以官方 SDK 文件為準。 **本文無業配內容。**