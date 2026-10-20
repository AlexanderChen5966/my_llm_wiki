---
title: Graphify：把 codebase 掃成知識圖，零 token 找出 God Nodes
date: 2026-08-07
tags: [ai/claude, dev/architecture, method/tools]
source: https://medium.com/@n913239（MikeWang，實測 graphify）
type: article
status: processed
asset_value: 72
---

## T（核心論點）

「一個 1,722 行的 ViewController，你早就知道它肥。**但你說不出它到底跟誰纏在一起**」——Graphify 用 tree-sitter 的 AST ＋ call-graph 把 codebase 掃成一張圖，回答的是**單一檔案回答不了的關係型問題**。

🔑 **關鍵事實：純程式碼專案走 AST，LLM 根本不上場**——實測 1,498 個節點、2,147 條邊，**花了零個 token，100% 都是 `EXTRACTED` 沒有一條是猜的**。

## B（適用場景）

**適用：** 中大型 codebase 的架構體檢；想量化「哪個檔案扛最多耦合」；讓 AI 拿圖去查而非給人看報告

**不適用 / 注意：**
- ⚠️ 官方自己講得很老實：**六個檔案本來就塞得進 context window，建圖只是圖個結構清楚，談不上壓縮**
- 🔴 見下方「四個不準的地方」——特別是 cohesion 指標
- 需 **Python 3.10 以上**（作者的機器只有內建 3.9，靠 `uv tool install --python 3.12` 才裝起來）

## R（安裝、實測、消費方式）

### 🔴 套件名稱的坑

> `graphify` 這個名字**還在申請取回中**，所以 PyPI 上的正式套件是 **`graphifyy`（雙 y）**，但 **CLI 指令仍然叫 `graphify`**。
> ⚠️ 專案自己特別警告：**PyPI 上其他 `graphify*` 開頭的套件都跟這個專案無關。**

規模：截至 2026-07 有 **9 萬顆以上的星**，MIT 授權，支援 **36 種程式語言**。

### 三種輸入，成本差很多

| 輸入 | 靠什麼分析 | 是否耗 token |
|---|---|---|
| **程式碼** | tree-sitter 的 AST ＋ call-graph | ❌ 零 token |
| **文件／論文** | 靠 Claude 抽概念與關係 | ✅ 耗 token |
| **圖片／截圖** | 靠 Claude 的 vision | ✅ 耗 token |

**它會先幫你算錢**（掃描前估成本）——純程式碼專案估出來是零。

### 🔑 實測產出的兩個有價值發現

**一、God Nodes（連接數最高的節點＝你的核心抽象）**
完整前十名**全部都是 ViewController**——沒有一個 Service、Model 或 Coordinator。這就是教科書上的 **Massive View Controller**。

> 價值在於量化：**不用再說「那幾個 VC 有點肥」，可以說「最肥的那個扛了 99 條邊」。**

**二、群集分析（Leiden 演算法）跑出 41 個群集**——第 9 號群集叫 “App Coordinator (navigation)”，裡面有 `AppCoordinator`、`CoordinatorType`、`CoordinatorFinishDelegate`。

**兩件事擺在一起，結論就很清楚**：這個專案**有** Coordinator、導航層確實已抽出自成乾淨群集，但 ViewController 還是掛著 99 條邊 → **這場從 Massive VC 走向 Coordinator 的遷移，做到一半。導航搬走了，業務邏輯還留在原地。**

> 🔑 **這是這個工具最有價值的一刻**：`grep Coordinator` 找得到那些檔案，但它不會告訴你「Coordinator 已經自成一個群集，而 VC 還連著 99 條邊」。**單一檔案回答不了的問題，要看關係才看得到。**

### 怎麼消費這張圖：MCP 不是必要

| 情境 | 做法 |
|---|---|
| **圖小、或只想看重點** | 直接讀固定產出的 `graphify-out/GRAPH_REPORT.md`（8 KB 出頭，給人看的）——god nodes、群集、cohesion 都在裡面。🔑 **原文 Part 3、Part 4 的分析就是這樣來的，一個 server 都沒開**。圖更小連完整 `graph.json` 都能直接餵給 LLM |
| **圖大、又想讓 AI 自己反覆查** | 才輪到 MCP server——這張 1,498 節點的圖**光 JSON 就約 40 萬 token，塞不進多數模型的 context** |

```bash
graphify-mcp --graph graphify-out/graph.json   # 純本地、預設走 stdio、連網路埠都不開
```
⚠️ MCP server 是**選配 extra**：只跑 `uv tool install graphifyy` 不會有它，要補 `uv tool install "graphifyy[mcp]"`。

起來後對外開 10 個工具：`graph_stats`、`god_nodes`、`get_neighbors`、`shortest_path`、`query_graph` 等。

> **MCP 不是必要，是為「大圖 ＋ 讓 AI 主動查」而生的。**

## 🔴 四個不準的地方（原文 Part 4，本篇最有價值的部分）

1. **它認得語法，不見得認得語意**
2. **“Surprising Connections” 一點都不 surprising**
3. **最尖銳的質疑：「那我 `wc -l` 就好了？」**——單純看行數與看關係的差別必須自己想清楚，否則這工具只是華麗的行數統計
4. 🔑 **訊號有點糊——cohesion 指標在最需要它的地方最沒有鑑別力**：
   > 分數最高的那幾個群集（0.4–0.67）**其實都只有一兩個節點**——單獨一個節點當然「內聚」。而真正有規模的群集，像那個 90 個節點的資料模型群，**cohesion 只有 0.03**。

## C（關鍵行動）

- [ ] 安裝時注意 **`pip install graphifyy`（雙 y）但指令是 `graphify`**；其他 `graphify*` 套件無關
- [ ] 純程式碼專案先跑一次——**零 token、100% EXTRACTED，沒有理由不試**
- [ ] 先讀 `GRAPH_REPORT.md`（8 KB）就好，**不要一開始就架 MCP server**
- [ ] 看 God Nodes 找出真正的核心抽象——若前十名同一類型，那就是架構訊號
- [ ] 🔴 **不要直接信 cohesion 分數**——先看該群集有幾個節點，一兩個節點的高分無意義
- [ ] 把「群集已抽出」與「舊節點仍高耦合」並讀，才看得出重構做到哪裡

## 補充資料

### 🔴 陌生 codebase 實測：apple/containerization 335 檔（2026-08-24 補充）

來源：同作者 MikeWang 系列第三篇（2026-08-21 發布，https://medium.com/@n913239/...79b192a598dc），標的為 apple/containerization（Apache-2.0，掃描版本 commit `74ace148`）。前兩篇是診斷／開刀自家 iOS 專案，這篇兌現「接手陌生大 codebase 值得用」那一行，而且**有標準答案**——`Package.swift` 裡 Apple 親口宣告了模組邊界，可以拿去對 Leiden 群集算分。

**建圖成本（比正文那次大一個量級）**

```bash
graphify extract . --code-only
```
**4.95 秒**跑完 335 個 Swift 檔，得 **6182 節點 / 15855 邊 / 315 群集**，零 token。其中 4531 個節點（73.3%）可對回 `Package.swift` 的 27 個 target。

🔑 **另一個被低估的優勢**：作者的機器（macOS 15.7.7 / Xcode 16.4）**編不動這個專案**（README 要求 Apple silicon + macOS 26 + Xcode 26），但 graphify 是純 AST 靜態分析，clone 下來就能掃——**不必先讓陌生專案跑起來才開始讀它**，對接手別人的東西省掉一整天環境地獄。

### 🔑 God node 判準修正：看 `method` 邊佔比，不是 degree

正文用「連接數最高＝核心抽象」找 god node，這次量出更精準的判準。原始度數榜前七名有四個是框架／語言型別（`Foundation` 239、`UInt64` 189、`GRPCCore` 162、`FilePath` 160；`Sendable` 原始度數 500，被內建雜訊清單濾掉），濾掉後才是專案自己的東西。

| 節點 | 總邊數 | `method` 邊佔比 | 診斷 |
|---|---|---|---|
| `LinuxContainer`（Apple） | 158（109 條是 `calls`、26 條 `method`） | **16.5%** | ✅ API 入口——「很多人用我」 |
| 正文那顆 VC | 120 | **76.7%**（92 個方法） | 🔴 病灶——「我自己扛太多」 |
| `IntegrationSuite` | — | 95.8% | ✅ 測試 suite，一測一方法，本來就該長這樣 |
| `Initd` | — | 92.9% | ✅ gRPC 服務端，一 RPC 一方法，**方法數是協定決定的** |

> 🔑 **同樣叫「連接度高」，病因完全相反。健康的 codebase 不是「沒有肥節點」，是「肥得有理」。**

Apple 整個 repo 裡自帶方法數最多的**正式程式碼**是 `Initd` 的 39 個（唯一破百的是測試）；正文那顆 VC 是 92。

### 🔴 群集 ≠ 模組，群集 ⊂ 模組（正文「41 個群集」的判讀修正）

正向指標很漂亮：**加權平均純度 87.0%**、278 個有效群集中 **197 個是 100% 純的**、可歸屬的邊 **77.6% 留在 target 內部**（跨 target 僅 22.4%）。

但反過來查就翻盤：`Containerization` 這一個 target 有 1303 個節點，**被切成 101 個群集**，最大的只佔 6.6%。

> **Leiden 幾乎不會把兩個模組混在一起，但它會把一個模組切成上百塊——它分出來的是「模組內部的功能小塊」，不是模組本身。**

用法因此改變：
- ❌ **別拿群集當模組圖看**——要模組圖，`Package.swift` 就寫在那裡，一秒看完，不需跑演算法
- ✅ **拿它在單一模組內部找次結構**——「這 1300 節點的大模組，內部其實分成哪幾塊？」才是 `Package.swift` 答不出來、只有圖答得出來的問題

（正文提到「抽出 validator 後多切出一個 18 人群集，把送出→驗證→validator 整條鏈圈起來」，正是同一個行為的自家版本。）

### 🔴 路徑查詢與自然語言問答基本不能用——以及修法

預設查詢兩個都失敗：

```bash
graphify path "cctl" "LinuxContainer"
# → cctl.swift --imports--> Logging <--imports-- LinuxContainer.swift --contains--> LinuxContainer
```
**這是廢話**——「兩個檔案都 import 了 Logging」對任何兩個檔案都成立。

```bash
graphify query "how does a container process get launched inside the VM"
# → 回一堆 ArchiveReader、archive 測試、examples/sandboxy，完全沒碰到 vminitd
```

**原因**：這張圖是**無向的**（`directed: false`）且保留了 `imports` 邊，於是 `Logging`、`UInt64`（189 邊）、`JSONEncoder` 這些到處都連的共用節點變成**蟲洞**，能把任意兩點接起來——在這種圖上求最短路徑必然被劫持。

**🔑 修法（答案其實就在圖裡，問題出在查詢策略）**：只保留 `calls` / `method` / `contains` / `implements` 這幾種「真的在做事」的邊，丟掉 `references` 與 `imports`，並把高度數節點限制在「中繼」位置。立刻得到正解：

```
Run --method--> .run() --calls--> LinuxContainer
```
兩跳，且完全對應原始碼 `Sources/cctl/RunCommand.swift:451` 的 `let container = try LinuxContainer(`。

⚠️ **過濾陷阱（作者自己踩的）**：第一版過濾器寫「degree > 60 一律排除」想擋框架型別，結果跑出「找不到路徑」——因為 `LinuxContainer` 自己就有 158 條邊，被當成雜訊擋掉了，差點把「圖裡沒有這條邊」寫成結論。

> 🔑 **god node 既是你想找的目標，也是你必須擋掉的雜訊。濾太鬆被蟲洞劫持，濾太緊把答案濾掉——這也是工具很難給好預設值的原因。**

**對 0807 那條離線路線的但書**：本地模型 + 本地圖 + 本地 code 的離線查詢路線依然成立（掃私有專案時這才是重點），但**路走不走得通，跟查詢準不準是兩件事**——換一顆腦不會讓爛查詢變好，毛病出在圖這一層。

✅ 唯一好用的是 `graphify explain "LinuxContainer"`：吐回 `.create()` / `.start()` / `.stop()` / `.exec()` / `.copyIn()` / `.copyOut()` / `VirtualMachineManager` / `Mount` / `State` / `Configuration`——**幾乎就是這個類別對外的 API 面，而一行原始碼都還沒讀**。當「挑入口」用價值最高。

### 陌生 repo 的通用筆記格式與分工

圖最後圈出四個檔（`cctl/RunCommand.swift`、`LinuxContainer.swift`、`Vminitd.swift`、`VminitdCore/Server+GRPC.swift`，合計 4572 行），作者**不是整包丟給 AI 叫它總結**，而是帶著圖給的線索問：「從 `Run.run()` 開始，順著 `.create()`、`.start()` 追下去，主機端到底怎麼指揮 VM 裡那個 agent？」有具體起點與符號名，AI 直接跳到對的段落。最後真正逐行讀的只有兩百多行。

**產出格式（對任何陌生 repo 都適用）**：怎麼分層 / 入口在哪 / 邊界在哪 / 先讀哪幾個檔。

⚠️ **圖答不出來的那一層**：`VirtualMachineAgent` 是一個 protocol，`VZVirtualMachineInstance`（macOS）與 `CHVirtualMachineInstance`（cloud-hypervisor）各自的 `dialAgent()` **回傳同一個 `Vminitd`**——兩套 hypervisor 共用同一份 guest 協定（vsock 上的 gRPC，port 1024）。**這種概念圖裡沒有也不可能有，只能靠讀。**

> **結論一句話：圖負責告訴你「該讀哪幾個檔」，AI 負責「讀懂那幾個檔」。把圖當問答引擎用會失望，當選檔器用才對。**

### 🔴 誠實的邊際價值結論

> **graphify 的邊際價值，跟專案文件的完整度成反比。**

作者主動指出 apple/containerization 對這套工具是**非典型範例**——README 寫了架構、`Package.swift` 直接給了模組圖、根目錄的 CLAUDE.md 把 host/guest 分工講得清清楚楚。那條 vsock gRPC 是讀 code 讀出來的，但**先讀 README 的話十分鐘就知道，根本不用建圖**。它最值錢的場合反而是沒人寫文件的老專案。

不過兩者不衝突：**文件給的是敘事（作者想讓你以為它長什麼樣），圖給的是度量（它實際長什麼樣）**。文件不會告訴你「這個類別有 26 個方法、109 個呼叫者」，也不會告訴你「這個被宣告成一個模組的東西，內部其實散成 101 塊」——**而兩者對不起來的地方，往往就是最該讀的地方。**

**健康 codebase 在圖上的三個特徵**：群集純度高（87%）／邊留在模組內（77.6%）／肥節點肥得有理（最連通者僅 16.5% 是自己的方法）。

⚠️ 成本誠實話：建圖 5 秒，真正的分析花了大半天。價值不在省分析時間，在於**沒有圖連該從哪個檔開始都不知道**——把「該讀哪幾個」從 335 縮到 4，對接手陌生專案來說這一步最貴。

## 相關連結

- [[refactor-with-claude-code-measured-by-graph]] — 同系列 0814 篇：那顆 92 方法 / 76.7% 佔比的 VC 重構實錄，是本篇對照組
- [[claude-code-large-codebase]] — 大型 codebase 的 context 策略
- [[code-review-graph]] — 同屬 codebase 圖譜化路線；該篇偏 review 加速（中位 ~82x），本篇偏架構診斷
- [[understand-anything-code-knowledge-graph]] — 程式碼庫轉互動式知識圖譜，含 Business Logic View；與本篇互補（那篇偏業務流程映射，本篇偏耦合結構）
- [[codegraph-token-optimization]] — CodeGraph 的 token 優化路線；本篇的零 token 是因為純 AST 不走 LLM，機制不同
- [[claude-code-markdown-single-source-multi-format]] — 同作者（MikeWang）系列
- [[claude-code-mechanism-division-series]] — 同作者的四機制分工索引
- [[hand-coding-agent-file-tools]] — ⚠️ 對照：該篇論證「小型變動專案用 grep 就夠、別上 RAG」；本篇則是「單一檔案回答不了的關係問題才需要建圖」——**兩者判準一致：先問這個問題需不需要跨檔案關係**
- [[mcp-fastmcp-server]] — MCP server 實作；本篇的 `graphify-mcp` 是純本地 stdio 的實例
