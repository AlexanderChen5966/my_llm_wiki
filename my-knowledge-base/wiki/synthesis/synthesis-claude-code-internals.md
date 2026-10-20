---
title: Claude Code 內部機制 知識精粹
date: 2026-08-04
source_count: 9
synthesis_snapshot_date: 2026-09-03
tags: [ai/claude, dev/architecture, ai/agent]
source: synthesis
type: synthesis
status: processed
sources: 9 篇
asset_value: 92
---

整合六篇從**六個不同高度**描述同一個系統的條目：規格書視角（三層架構）、逆向工程視角（proxy 攔真實 payload）、產品分層視角（五層架構）、設定分工視角（四機制邊界）、機制實作視角（Hooks + Memory）、使用者操作視角（Context 管控）。這六篇含知識庫資產值最高的兩篇（92、85），但先前沒有任何精粹整合它們的關係。

## E（精粹論點）

**幾乎所有 Claude Code 的「使用技巧」都是同一個技術事實的推論：Claude API 完全無狀態，所以「上下文」就是每次送出的整個 payload，而 harness 是唯一持有狀態的地方。**

從這一件事可以推導出六篇的全部內容：

| 常見疑問 | 推導 |
|---|---|
| 為什麼 CLAUDE.md 要短？ | 它在 payload 開頭且每次都送；**改 1 個位元組就讓後面的快取全部失效** |
| 為什麼 AI 有「記憶」？ | 記憶就是每次把歷史重送一次——**造假實驗證明模型分不出歷史是真發生過還是被塞進去的** |
| 為什麼 CLAUDE.md 擋不住某些行為？ | 它只是 payload 裡的文字（模型可以不理）；Hooks 在 harness/shell 層（模型無法繞過） |
| 為什麼 subagent 能省 context？ | 各自獨立的 payload |
| 為什麼 `/clear` 不是刪除？ | 狀態在 JSONL，`/clear` 只是開新檔，舊檔完整保留 |
| 為什麼 compaction 會丟 `paths:` rules？ | 它們不在「compact 後從磁碟自動重讀」的清單裡 |

> 🔑 **最反直覺的一層**：五層架構、四機制分工這些看起來像「產品設計選擇」的東西，**其實多半是「API 無狀態 ＋ payload 前綴快取」這兩個技術約束的必然結果**。理解約束比背功能表有效。

---

> 🔴 **2026-09-03 重跑（6→8 篇）：無狀態推論鏈往上長了一層——「這一輪送什麼」本身也是被組裝出來的。**
>
> 原 E 層說「Claude API 完全無狀態，所以上下文就是每次送出的整個 payload，harness 是唯一持有狀態的地方」。新增兩篇把這條鏈補完：
>
> 🔑 **[[system-prompt-assembly-pipeline]]：System Prompt 不是一份文件，而是 Harness 每一輪根據系統狀態組裝出來的執行介面（Runtime Artifact）。**
> 所以最難的不是「寫一句更厲害的 Instruction」，而是決定**這一輪模型到底應該看到哪些規則、能力與狀態**。它把 [[agent-context-management]] 的原則推到 System Prompt 身上——**Context 是工作區，不是倉庫**，System Prompt 同樣不該變成「所有可能有用資訊的永久垃圾桶」。
>
> **這讓推導表可以再加一列**：
>
> | 常見疑問 | 推導 |
> |---|---|
> | 為什麼同一句指令有時有效有時無效？ | 🔑 **因為它每一輪是否被組裝進 payload、以什麼形式組裝，取決於當下系統狀態**——不是模型時好時壞 |
>
> 而 [[steering-claude-seven-carriers]]（**官方 blog 中譯，屬官方立場**）從使用面給了配套判準：七種載體不是「哪個最好」，而是 🔑 **同一條指令放錯載體就會失效**，並列出五個「你正在做這些事就代表放錯地方」的症狀。
> 🔑 **兩篇合起來構成完整答案**：機制上「每輪組裝」解釋了為什麼載體會決定成敗，使用上「五個症狀」給了可自我診斷的訊號。

## C（共識核心）

> 標註出現篇數

- **狀態不在模型那邊，全在本機**（3/6：harness-internals、api-request-reverse-engineering、context-management）：Messages API 不保留任何對話歷史；harness 每次把系統提示 ＋ 過去所有訊息 ＋ 這次輸入**全部一起送出**。官方用語就叫 **agentic harness**
- **迴圈是程式的行為，不是送給模型的資料**（2/6，且兩篇獨立得出）：harness-internals 界定「**迴圈只有外層這一個**」（內層 thinking 不是迴圈）；reverse-engineering 從 payload 反證「四個詞裡**只有迴圈不在請求裡**」
- **advisory 與 mandatory 有本質差別，不是強度差別**（4/6：hooks-memory、mechanism-division、harness-internals、context-management）：CLAUDE.md／Auto Memory 都是「上下文參考」（官方定性），要硬性擋下必須用 PreToolUse hook
- **工具是我方定義並隨請求送出的，不是模型內建**（2/6）：reverse-engineering 攔到 83 個工具定義、官方 `Read` 的 description 就寫了 **1,782 個字元**；harness-internals 的注入四管道是同一件事的另一面
- **context 管控的本質是「別讓 payload 前段變動」**（3/6）：穩定內容放前面、頻繁變動的別寫進 CLAUDE.md、subagent 隔離、pull 型只注入通知

---

## A（獨特視角）

| 來源 | 獨特貢獻 |
|------|---------|
| [[claude-code-harness-internals]]（av 92）| **規格書視角**。三層架構表（API 推論／Harness 狀態／JSONL 持久化）；**提示快取機制**（`cache_control` 由 harness 指示、每請求上限 4 個標記、TTL 1h 為實測、快取讀取約 0.1x 成本）；**注入四管道**（`system` 參數／`<system-reminder>` 萬用標籤／**合成歷史**／工具結果包裝）＋ 三個實測案例（防重複讀檔、`@README.md` 被合成成假的 Read 配對、輸出過大改成 `<persisted-output>` 參照）；🔑 **pull 型設計**（TodoList 只注入「已更新，可用工具查看」的通知，不整份 push）；**compaction 保留/遺失清單** |
| [[claude-code-api-request-reverse-engineering]]（av 85）| **逆向工程視角**。🔑 **看得到明文不是破解加密**——`ANTHROPIC_BASE_URL=http://127.0.0.1:port`，Claude Code 到 proxy 這段是明文，加密發生在 proxy 轉交 API 那段，攔的是自己主動導流的流量；**四個詞的定位表**（模型/工具/context 在 payload，迴圈不在）；🔴 **造假記憶實驗**——預先塞兩則模型沒講過的假對話，它完全分辨不出；**存 `resp.content` 而非純文字**是為工具呼叫鋪路（模型回覆是「一疊積木」，`tool_use` 積木必須完整保存） |
| [[claude-code-five-layer-architecture]]（av 82）| **產品功能分層視角**。五層＝CLAUDE.md 記憶層／SKILLS 知識層／HOOKS 護欄層／SUBAGENTS 委派層／PLUGINS 分發層，各有類比（BIOS／隨身碟／CI 防線／分派部門／npm）；**Layer 3 與 Layer 4 是生產級應用的必備機制**；⚠️ **subagent 不能再產生 subagent**（架構固定兩層，無限遞迴限制）|
| [[claude-code-mechanism-division-series]]（av 65）| **設定分工視角**（decision routing）。四機制各管什麼：**CLAUDE.md 規範／Skills 能力／Agents 派誰去做／settings+Hooks 系統運作**；🔑 **Hooks 是事件觸發，Skills 是指令觸發**（前者你不用說它自動做）；「四機制共同取代的是**自律**——不是讓你記得做，是讓你不做也會發生」；🔴 **Hook 語法糾錯**：`matcher` **只比對工具名稱**，`Bash(git commit*)` 是 permissions 語法不是 hook 語法；「**測試驗證你記得的事，Hook 看守你忘記的事**」 |
| [[claude-code-hooks-memory]]（av 80）| **機制實作視角**。🔴 **PreToolUse 只有 exit code `2` 才中止工具呼叫**（其他非 0 只是非阻擋性錯誤，工具仍會執行），stderr 成為反饋給模型的錯誤訊息；工具資料**只走 stdin**（`cat \| jq -r '.tool_input.command'`），環境變數不帶工具資料；Memory 四類型（user／**feedback 最重要**／project／reference）＋ **feedback/project 要補 Why 與 How to apply** 以便判斷邊界情況；`MEMORY.md` 索引限**前 200 行或 25KB**；sub-agent 獨立 memory（名字不同則目錄完全隔離）|
| [[claude-code-auto-memory]]（av 72）| 🆕 **無狀態架構下「唯一持久」的那一塊**：Auto Memory 存在磁碟，所以 `/compact` 後仍保留並重新注入——與「`paths:` rules 會先遺失」形成對照，直接示範了 harness 的**保留清單**是一份需要逐項查證的東西。另兩個機制細節：路徑由 **Git Repo 推導**（子目錄與 Worktree 共用同一份記憶）、`autoMemoryDirectory` **刻意不接受 Project 層級**以防共用專案的惡意設定 |
| [[claude-code-context-management]]（av 82）| **使用者操作視角**。context 到六成注意力就下降；四大消耗源（CLAUDE.md 過長／MCP 過多／Skill 過多／對話不清理）；🔴 **`/clear` 不是清空**——逐指令 JSONL 行為對照表（`/clear` 開新檔舊檔完整保留、`/compact` 同檔追加 boundary、`/rewind` 分支、`/btw` 完全不寫入），**對隱私判斷有實質影響**；**Auto Memory 三分流判斷法**：要傳給同事→專案 CLAUDE.md／換專案也要→全域 CLAUDE.md／只是這台電腦別再忘→Auto Memory |

---

## 🔑 跨篇釐清一：三種分層法不是競爭關係，是三個切面

**這是本精粹最重要的整合**——六篇出現**三套互不相同的分層**，沒有任何單篇說明它們的關係，容易誤以為有矛盾：

| 分層法 | 篇數 | 回答的問題 | 層級 |
|---|---|---|---|
| **三層架構** | harness-internals | **執行時誰持有狀態** | Claude API（無狀態推論）／Harness（全部狀態）／JSONL（持久化）|
| **五層架構** | five-layer-architecture | **開發者怎麼往上疊功能** | CLAUDE.md → SKILLS → HOOKS → SUBAGENTS → PLUGINS |
| **四機制分工** | mechanism-division | **這件事該寫在哪裡** | CLAUDE.md 規範／Skills 能力／Agents 派誰／settings 強制 |

> **三者是不同抽象層的切面**：三層是**執行時架構**（runtime），五層是**功能組裝順序**（build-time），四機制是**設定路由決策**（design-time）。
> 用錯層級就會問錯問題——例如「Hooks 屬於第幾層」在五層架構是 Layer 3，但在三層架構它整個屬於 **Harness 層**（因為它是本機執行的，模型看不到也繞不過）。

---

## 🔑 跨篇釐清二：強制力光譜（六篇合起來才完整）

單篇都只講其中一段，合起來是一條完整光譜：

| 強制力 | 機制 | 為什麼 | 來源 |
|---|---|---|---|
| **最弱：可忽略** | CLAUDE.md / Auto Memory | 只是 payload 裡的文字，官方定性為「上下文參考」 | hooks-memory、context-management |
| **被說服的** | Skill 內部紀律（如 Superpowers 的 brainstorming） | 紀律寫在 Skill 正文，模型讀了才遵守 | mechanism-division |
| **硬保證** | 內建模式（Plan Mode） | harness 層的模式限制，不靠模型自覺 | mechanism-division |
| **最強：mandatory** | Hooks（`exit 2`） | shell 層強制，在工具實際執行前攔下 | hooks-memory |

**推論**：「它為什麼不聽我的」的排查順序應該是——① `/context` 確認檔案**到底有沒有被載入** → ② 有載入則檢查指示是否夠具體、有無跨檔衝突 → ③ **若是「必須在特定時機執行」的規則，本來就不該寫在 CLAUDE.md**，該改寫成 hook。

---

## 🔑 跨篇釐清三：世代分界是一種模式，不是個案

兩篇各記一個參數的世代分界，**合起來才看出趨勢**：

| 參數 | 舊世代 | 新世代 | 來源 |
|---|---|---|---|
| **thinking** | 4.5 以前 `budget_tokens` | 4.6 以後 `{"type":"adaptive"}` ＋ `output_config.effort` | harness-internals |
| **temperature** | Haiku 4.5 仍支援 | **Opus 4.7+／Sonnet 5+ 直接吃 400**（`temperature is deprecated for this model`）| reverse-engineering |
| 🆕 [[system-prompt-assembly-pipeline]] | 🔑 **把 System Prompt 從「文件」重新定義為「Runtime Artifact」**：Harness 每一輪依系統狀態組裝出的執行介面；同系列 Day 08–10（Skills／Context／Memory）在此收束——決定它們**怎麼進入模型**。並把「Context 是工作區不是倉庫」的原則套到 System Prompt 上 |
| 🆕 [[steering-claude-seven-carriers]] | **唯一官方一手的「載體選擇」判準**（官方 blog 中譯，2026-07-12）：七種載體 ＋ 🔑 **五個「放錯地方」的症狀**（自我診斷用）；並收錄本庫先前未載的 `--append-system-prompt`，官方點出它對遵循度有影響 |

> **模式**：新世代模型把越來越多控制權**從「參數微調」移向「模型自己判斷」**。這與 [[synthesis-prompt-engineering-evolution]] 的躍遷 1（結果契約）與躍遷 4（思考預算）是同一趨勢在 API 參數層的表現。
> **實務守則**：引用任何 Claude 參數行為時**必須標明模型世代**——兩篇來源在 thinking 參數上看似矛盾，實際是世代差異而非衝突。

---

## V（知識缺口）
- 🆕 🔴 **「這一輪組裝了什麼」無法觀測（與既有『注入管道可觀測性』缺口合流並升級）**：[[system-prompt-assembly-pipeline]] 說明 System Prompt 每輪動態組裝，但**使用者無法列舉「這一輪實際組進去的規則與能力有哪些」**——`/context` 只給分類 token 量。這使得「指令為何這輪失效」在原理上可解釋、實務上無法驗證
- 🆕 ⚠️ **七種載體缺「同時存在時誰贏」的規則**：[[steering-claude-seven-carriers]] 給了放錯地方的症狀，但**多個載體同時對同一行為下指令時的裁決順序沒有說明**——這與 [[synthesis/synthesis-claude-md]] 記錄的「階層合併是 concatenate 而非 override、衝突時 Claude 可能任選一個」是同一個問題的不同面向

- ⚠️ **三種分層法都缺官方權威對照**：五層架構與四機制分工都是社群／媒體歸納（非官方文件用語），只有「agentic harness」與 compaction 保留清單有官方出處。**本精粹的釐清一屬於推論，不是官方定義**
- ⚠️ **快取 TTL=1h 是實測非官方**（harness-internals 自己標註），會隨版本變動；`cache_control` 每請求上限 4 個標記亦同
- ❓ **pull 型設計的完整清單未知**：只知道 TodoList、Plan 檔案本體走 pull，但**哪些資訊是 push 哪些是 pull 沒有任何文件**——這直接影響「我的 X 到底在不在 context 裡」的判斷
- ❓ **多 harness 並行時的狀態互動完全沒人談**：六篇全部假設單一 session。多個 session／git worktree／多視窗同時跑時，Auto Memory（machine-local，同 repo 子目錄與 worktree **共用同一份**）會不會互相打斷？無來源討論
- ⚠️ **五層架構條目較舊**（2026-05-19，六篇中最舊）：Plugin 生態變化快，Layer 5 的描述是否仍準確未經驗證；`.claude/CLAUDE.md` 作為「專案層級」的路徑寫法也與現行慣例（專案根 `CLAUDE.md`）不同，**引用該層時建議先核實**
- ❓ **注入管道的可觀測性**：`<system-reminder>` 是「萬用注入標籤」，但使用者無法列舉「這次 session 到底被注入了什麼」——`/context` 只給分類 token 量，不給注入內容清單

---

## 整合行動清單

- [ ] 🔴 **先建立「上下文 = 每次送出的整個 payload」這個心智模型**——六篇的所有技巧都是它的推論，記住原理比背功能表有效
- [ ] **把穩定內容放 payload 前段**（system／CLAUDE.md），頻繁變動的一律別放——1 個位元組不同就讓後面快取全失效
- [ ] 用 `/context` 查各類別 token 消耗（**只有 `messages` 陣列單調增加**，是膨脹主因）
- [ ] 遇到「這件事該寫哪裡」套四機制分工；遇到「它為什麼不聽」套強制力光譜（載入了嗎 → 夠具體嗎 → 該不該改成 hook）
- [ ] 寫 hook 記住兩件事：**`matcher` 只比對工具名稱**、**PreToolUse 只有 `exit 2` 會擋**
- [ ] 依賴 `paths:` rules 或子目錄 CLAUDE.md 時**別養太長的對話**（compact 會先丟這些且無通知）
- [ ] 想確認實際送出什麼，架本機 proxy 看一次（`ANTHROPIC_BASE_URL=http://127.0.0.1:port`，**不需破解加密**）
- [ ] 引用任何參數行為（thinking／temperature／effort）**一律標明模型世代**
- [ ] 記住 `/clear` **不刪除舊工作階段**——涉及機敏內容時這是隱私判斷的關鍵事實

---

## 來源條目

- [[claude-code-harness-internals]] — 規格書視角：三層架構、提示快取、外層迴圈、注入四管道、pull 型設計、compaction 保留清單（asset_value: 92）
- [[claude-code-api-request-reverse-engineering]] — 逆向工程視角：proxy 攔明文、四個詞定位、造假記憶實驗、temperature 世代分界（asset_value: 85）
- [[claude-code-five-layer-architecture]] — 產品功能分層：CLAUDE.md／SKILLS／HOOKS／SUBAGENTS／PLUGINS 五層與協同流程（asset_value: 82）
- [[claude-code-context-management]] — 使用者操作視角：context 消耗源、`/clear` 不是清空、世代反轉注記（asset_value: 82）
- [[claude-code-auto-memory]] — 🆕 Auto Memory 的機制面：儲存位置由 Git Repo 推導、`MEMORY.md` **200 行靜默截斷**、`autoMemoryDirectory` 只接受 Policy/Local/User 層級（防惡意專案設定）、三分流判斷法（AV 72）
- [[claude-code-hooks-memory]] — 機制實作：PreToolUse exit 2、stdin JSON、Memory 四類型、MEMORY.md 200 行限制（asset_value: 80）
- [[claude-code-mechanism-division-series]] — 設定分工邊界：四機制各管什麼、事件觸發 vs 指令觸發、Hook 語法糾錯（asset_value: 65）
- [[system-prompt-assembly-pipeline]] — 🆕 🔑 System Prompt 是每輪組裝的 Runtime Artifact，不是一份文件
- [[steering-claude-seven-carriers]] — 🆕 **官方立場**：七種載體與五個「指令放錯地方」的症狀

## 相關精粹

- [[synthesis-claude-md]] — CLAUDE.md 方法論；本篇提供其「為什麼要短、為什麼是 advisory」的機制依據
- [[synthesis-prompt-engineering-evolution]] — 提示詞四次躍遷；本篇的世代分界模式是其躍遷 1／4 在 API 參數層的表現
- [[synthesis-agent-engineering-architecture]] — Agent 工程架構；本篇是單一產品（Claude Code）的深度剖面
- [[synthesis-spec-driven-workflow]] — 規格驅動；本篇的「Hook 看守你忘記的事」對應其「每條義務接機器可判定檢查」
