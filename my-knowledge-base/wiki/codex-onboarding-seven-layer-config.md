---
title: Codex 新手入門：七層設定地圖心智模型
date: 2026-07-20
tags: [ai/agent, ai/workflow]
source: https://codelove.tw/@tony/post/3ojwXx
type: article
status: processed
asset_value: 54
---

新手用 Codex 最大的問題不是不會提問，而是**不知道 Codex 的「幾層設定」分別管什麼**。Codex 不是只有一個聊天框，而是一個可以被設定的工作台。這篇的核心是一張「七層設定地圖」：把不同性質的需求放到正確的層，Codex 才會從「回答問題的視窗」變成「適合你工作流的助手」。

## T（核心論點）

新手最常犯的錯是把所有東西都塞進 prompt。正解是依「資訊壽命」把設定分層歸位——**臨時需求放對話，專案規則放 `AGENTS.md`，執行預設放 `config.toml`，重複流程寫成 `Skills`，外部能力交給 `Plugins`/`MCP`/`Apps`，個人偏好放 `Memory`**。分清這幾層，Codex 就不再是一堆陌生名詞。

## B（適用場景）

**適用：**
- Codex 新手，已裝好但被 `config.toml`/`AGENTS.md`/`MCP`/`Skills`/`Memory` 搞混
- 需要一個判斷「某個設定該寫在哪一層」的心智模型

**不適用 / 注意：**
- 不是官方文件翻譯；Codex 更新快，具體命令以本機 `codex --help` 與 `codex doctor` 為準（作者驗證版本為 codex-cli 0.140.0-alpha.2）
- 新手別一開始追求「全配滿」，也別用 `danger-full-access` 起步

## R（七層設定地圖 + 上手順序）

### 七層各管什麼

| 層 | 放什麼 | 位置 |
|----|--------|------|
| 對話 / prompt | 這一次任務的臨時要求 | 當下對話 |
| `AGENTS.md` | 這個專案長期遵守的規則（結構/命令/約束/驗收） | 專案根目錄 |
| `config.toml` | Codex 自己怎麼執行（model/sandbox/approval/MCP/profiles） | `~/.codex/config.toml` |
| `Skills` | 某類任務的標準流程 | `~/.codex/skills` |
| `Plugins` | 打包好的能力包（含 Skills/MCP/App/腳本/範本） | `codex plugin add` |
| `MCP servers` | 連接外部工具與資料來源（標準協定） | `codex mcp add` |
| `Memory` | 長期偏好與穩定習慣（記偏好不記事實） | Memory |
| `Apps / Connectors` | 已授權的真實應用（GitHub/Gmail/Figma/Chrome） | 桌面 App / 授權 |

### 關鍵區分
- **`AGENTS.md` vs `Skill`**：專案長期規則（「這專案用 pnpm 不用 npm」）放 AGENTS.md；某類任務標準做法（「寫文章前先做選題定位」）放 Skill。
- **`config.toml` 長期預設 vs `-c` 臨時覆蓋**：`codex -c model="o3"` 只影響本次。
- **Memory 記偏好不記事實**：寫作風格放 Memory；專案怎麼啟動放 AGENTS.md；PR 是否過 CI 應接 GitHub/MCP 即時讀。
- **MCP vs App/Connector**：MCP 是標準協定介面（適合工具/資料源）；App/Connector 是已授權的真實應用連接。

### 新手第一次設定順序
1. 安裝 Codex，完成 `codex login`
2. 跑 `codex doctor` 確認環境正常（排查問題第一站）
3. 專案根目錄寫一個簡單 `AGENTS.md`
4. 只改必要的 `config.toml`，別開太多高權限（保守 `sandbox_mode = "workspace-write"` + `approval_policy = "on-request"`）
5. 先接一個最常用外掛（GitHub 或 Browser）
6. 重複性任務再寫成 Skill
7. 需要外部資料再設 MCP
8. 最後才把長期偏好放進 Memory

### 安全底線
**讀取可以更自動，寫入必須謹慎。** 讀 PR、總結郵件、開網頁風險低；發郵件、提交表單、刪檔、發文、改權限、付款必須先停下確認。

## C（關鍵行動）

- [ ] 記住七層地圖：臨時→對話、專案規則→AGENTS.md、執行預設→config.toml、重複流程→Skills、外部能力→Plugins/MCP/Apps、偏好→Memory
- [ ] 第一週只做三件事：每個專案補 AGENTS.md、學會用 `codex doctor` 排查、只接一個最需要的外掛
- [ ] AGENTS.md 少寫願景多寫命令/約束/驗收，別寫成散文
- [ ] 別一開始就開最高權限；MCP 配好後用 `codex mcp list` + `codex doctor` 驗證

## 原始摘要

譯自掘金文章，OpenAI Codex 新手教學。強調循序漸進、保守權限、區分讀寫。常見坑六條：AGENTS.md 寫成散文、一開始開最高權限、MCP 配好忘了驗證、外掛裝太多、把 Memory 當事實庫、不區分讀與寫。

## 補充資料｜Token 效率：把「省 Token 四設定」歸位到七層地圖（2026-09-09 併入）

來源：〈GPT-6 Astra 太耗 Token？4 個設定讓 Codex 跑得更快、更省〉，tools.wingzero.tw，
2026-09-06，<https://tools.wingzero.tw/articles/4052>。

Token 消耗過快通常不是模型問題，而是**設定沒歸位**。該文的四個設定剛好落在七層地圖的三層上，
證明「設定分層」不只是新手心智模型，也是成本控制的操作面：

| 層 | 該調什麼 | 具體設定 |
|----|---------|---------|
| `config.toml`（執行預設） | 長時間任務的上下文機制 | `~/.codex/config.toml` 的 `[features.context_management]` → `experimental_mode = true`：**同一任務跨越不同上下文視窗時保留筆記，並可回頭搜尋該任務先前的訊息與工具執行結果**。價值不只是「少塞歷史」，而是降低反覆壓縮摘要而遺失早期細節。⚠️ 僅限同一任務，不代表會自動搜尋其他對話 |
| 對話 / prompt（本次任務的臨時要求） | 智慧程度與速度按任務複雜度給，不要一律拉滿 | 一般任務「中／高」通常足夠，複雜任務才用「極高／最高」；**Ultra 適用情境相當有限**。速度用「正常」即可——實測體感達舊版 `/fast` 的 2～5 倍，但**這更可能來自「完成任務的步數變少」而非生成速度變快**（舊模型常輸出無關內容或做非必要測試） |
| `AGENTS.md` ＋ `Skills` | 減少每次工作的固定載入量 | 合併重複規則、精簡 Skill 觸發描述、收窄易誤觸發的條件、長範例／模板／特殊流程外移到獨立參考文件。核心原則是**只在需要時讀取需要的資訊**，而非把文件砍短 → 詳見 [[agents-md-guide]] |

### 兩組值得沿用的守則（該文提示詞內建）

- **改設定的安全要求**：先建立可恢復、且不覆蓋既有備份的備份檔 → 以**冪等**方式修改（避免產生重複設定區塊、
  完整保留其他設定）→ 驗證 TOML 語法 → 重新讀取設定檔確認 → **分別回報「設定是否已寫入」與「執行階段是否已確認生效」**
  → 說明是否需要新建任務或重啟客戶端才生效。若客戶端／版本／登入方式不支援該功能，必須明講。
  這正好呼應本條目的安全底線（讀取可以更自動，寫入必須謹慎）。
- **誠實度守則**：🔑 **沒有實際 Token 用量或額度前後對照資料時，不得把「文件字元數下降比例」宣稱為
  「Token 節省比例」。** 同理，最佳化不得為省 Token 而降低任務品質——必要功能、明確要求的交付數量、
  權限與操作邊界、驗收標準、失敗停止條件都必須保留；若最佳化會改變預設工具或輸出數量，要明確說明。

## 補充資料｜`context_management` 的機制本質與三個生效前提（2026-09-10 併入）

來源：〈開啟 Codex 這個新功能，可以大幅降低 GPT-6 的消耗！〉，赤紅翼零，tools.wingzero.tw，
2026-09-05，<https://tools.wingzero.tw/articles/4051>。

上一則補充已把 `[features.context_management]` 歸位到 `config.toml` 層，本則補該設定**本身**的三件事：

**一、機制本質是換掉上下文管理的做法，不是「少塞一點歷史」**

原本 Codex 處理長任務是**不斷把先前對話與操作紀錄壓縮成摘要**，任務越久摘要被反覆壓縮越多次，
越前面的細節越容易消失——已經試過哪些解法、當時為何改那段程式、某個工具回傳了什麼、
哪些方法測過但失敗了。這些一旦掉出摘要，Codex 後續只能靠壓縮過的內容推理。
開啟後改成 **「筆記 ＋ 可搜尋的歷史紀錄」**：跨上下文視窗保留筆記，需要細節時**回頭搜尋同一任務內的
歷史訊息與工具執行結果**。省 Token 是副作用，主效果是**降低長任務的資訊遺失率**。

**二、改完設定不會對現有任務生效——必須重新建立一個新的 Codex 任務**

這正是上一則補充引用的安全守則裡「分別回報『設定已寫入』與『執行階段已確認生效』」那條的具體案例：
`config.toml` 寫進去 ≠ 當下那個任務已經套用。

**三、🔴 目前使用 API Key 的方式暫不支援**

生效需要三個條件同時成立：支援此功能的 Codex 客戶端 ＋ **符合資格的 ChatGPT 訂閱帳號登入** ＋ 實驗性功能仍在開放中。
以 API Key 登入的環境設了也不會動——這是**登入方式決定功能可用性**的例子，
評估任何 Codex 設定時都該先確認自己的登入路徑，而不只是確認版本號。

## 相關連結

- [[codex-desktop-2026-onboarding-guide]] — Codex 桌面版上手順序（本篇為設定分層心智模型，該篇為桌面 UI 操作動線）
- [[codex-prompt-4-elements-agents-md]] — Codex 提示詞四要素 + AGENTS.md 寫法（本篇第二層的深入）
- [[codex-learning-roadmap]] — Codex 學習路線圖總覽
- [[codex-plugin-workflow]] — Codex Plugin 可重用工作流封裝（第五層深入）
