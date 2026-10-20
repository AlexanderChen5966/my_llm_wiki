---
title: OpenAI Codex 11 個進階技巧：從程式助手到完整工作系統
date: 2026-05-22
updated: 2026-09-17
tags: [ai/workflow]
source: https://www.koc.com.tw/archives/643417
type: article
status: processed
asset_value: 50
---

OpenAI Codex 開發體驗工程師 Jason Liu 整理的 11 個進階用法，核心洞察是：Codex 已從「程式碼助手」進化為「完整電腦工作系統」——透過 Durable Threads 保留跨 session 脈絡、Goals 設定可驗證的任務終點、Steering/Queuing 介入任務執行、Shared Memory 管理長期工作知識。

## T（核心論點）

電腦上的許多工作都透過程式碼中介：shell 指令、瀏覽網頁、呼叫 API、匯出文件、觸發自動化。隨著 Browser/MCP/Computer Use 介面開放給 Codex，它不再只是程式助手，而是能接管完整電腦工作流程的 Agent 系統。**Durable Threads + Goals + Shared Memory** 是讓 Codex 承擔長期任務的三個關鍵機制。

## B（適用場景）

**適用：** 重度使用 Codex 的開發者；希望讓 AI 承擔跨 session 長期任務；需要在非編碼時段維持任務進行

**不適用 / 注意：**
- Appshots、Goal 模式等功能需付費方案（Business+）
- Computer Use 目前僅支援 macOS
- Shared Memory 仍建議把重要規則放在 AGENTS.md（不是只靠 Memories）

## R（11 個技巧）

### 1. Durable Threads：固定工作脈絡的持久工作區

「長期保留脈絡的 Codex Thread」——不是一次性聊天視窗，而是持久工作區：
- 發版 thread（固定處理發版流程）
- 文件審查 thread
- 外部監控 thread
- Chief of Staff thread（通用助理）

快捷鍵：Command-1 到 Command-9 直跳已 pin 的 thread，不需重新建立脈絡

### 2. 語音輸入：捕捉粗略想法

- 不需先組織成精確 prompt，直接說出模糊起點
- 例：「我記得 Slack 裡有人提過某件事，幫我找一下」
- 適合：想法還沒成形時，用語音觸發搜尋與整理

### 3. Steering：任務中途修正方向

- Codex 執行任務時，可即時補充指令修正方向
- 不需等任務完成後整包重做
- 例：「這個區塊太大、間距不對、文案不對」→ 直接在執行中調整

### 4. Queuing：預排下一步任務

- 在不打斷當前任務的情況下，把下一步工作先排進隊列
- 例：「等這件事完成後，把預覽連結傳給審查者」
- 讓 Codex 像能連續處理工作的助理，而非只能一問一答

### 5. Browser / Chrome Extension / Computer Use

| 工具 | 適用場景 | 限制 |
|------|---------|------|
| In-app Browser | 預覽已渲染網頁、共同查看 + 附視覺註解 | 不支援登入狀態、cookies、擴充功能 |
| Chrome Extension | 需要登入狀態的工具（LinkedIn、Salesforce、Gmail、內部系統） | 使用已登入 Chrome |
| Computer Use | macOS GUI 操作、桌面 App、只在 GUI 發生的 bug | 目前僅 macOS |
| MCP Servers | 呼叫外部服務、擴充工具集 | 需另行設定 |

### 6. Skills：封裝重複流程

- 把反覆出現的工作流程打包成 Skills
- 下次遇到類似任務，Codex 不需重新學習整套流程

### 7. 行動端：任務不中斷

- ChatGPT 行動版中的 Codex：離開桌面後仍可查看進度、回答問題、核准下一步、調整方向
- 檔案、憑證、權限保留在機器上；更新即時同步到手機
- 適合：長時間任務不需一直坐在電腦前

### 8. 自動化排程

- 讓 Codex 按排程執行工作：每天產生報告、定期檢查 repository、持續追蹤外部回饋來源

### 9. Goals：設定可驗證的任務終點

「長期任務目標」——不是叫 Codex「繼續做」，而是給明確終點：
- **要做到什麼程度才算完成**
- **如何確認成功**（測試通過、benchmark 達標、bug 能重現並修正）
- **哪些限制不能破壞**
- 關鍵：**可驗證**——不要說「把計畫完成」，要定義具體完成條件

### 10. Side Panel：同一介面檢查產出

- 不需匯出檔案、切換工具
- 可預覽：PDF、試算表、文件、簡報、程式碼、網頁
- 追蹤：Codex 的計畫、來源、產出物、摘要

### 11. Shared Memory：管理長期工作脈絡

把重要資訊放在穩定位置（不只是聊天紀錄）：
- **外部儲存**：Obsidian vault、Git、Dropbox、Google Drive 的 TODO/決策/阻塞點/負責人
- **Codex Memories 功能**：把先前 thread 中有用的脈絡帶到未來工作（穩定偏好、重複流程、技術棧、已知問題）
- **注意**：重要規則仍應放在 AGENTS.md 或版控文件，Memories 是輔助性本地回憶層

## C（關鍵行動）

- [ ] 建立專用 Durable Thread：為常見工作流程（發版/文件審查/外部監控）各建一個 thread，使用 Command-1~9 快捷鍵固定
- [ ] 設定 Goals 時加入「可驗證」條件：不說「完成功能」，說「測試通過 + build 不爆」
- [ ] 建立 Shared Memory 架構：重要工作脈絡放 Git 或 Obsidian，Codex Memories 做輔助

## 補充資料：Codex 2026 新功能（DeTools 工具翼零，2026-05-22）

**Appshots（雙 Command 截圖）**：Mac 上同時按左右 Command 鍵，直接把當前視窗截圖（含螢幕外文字內容）送進 Codex。解決了「看到 bug 需要截圖→貼上→補充說明」的斷點，一鍵帶入完整上下文。

**`/goal` 正式 GA**：從實驗性升為正式功能。可給 Codex 一個目標並讓它持續工作數小時乃至數天；可隨時查看進度、調整方向或暫停。搭配 side chat 可監控主任務而不打斷它。

**進階標註模式**：設計師可直接在設計稿上圈選區域並標註，Codex 根據標註實作對應程式邏輯，縮短設計→開發的溝通成本。

**團隊 Plugin 共享 + Analytics**：組織可統一管理成員安裝的 Plugin、查看使用數據，Codex 從個人開發助手進化為完整 AI 開發協作平台。

## 補充資料：Jason Liu AI Engineer 大會 workshop（2026-08-03 併入）

來源：數位時代（李先泰，2026-07-27）〈OpenAI專家曝光Codex設定：15分鐘雜亂對話更勝「精緻 Prompt」〉（https://www.bnext.com.tw/article/91633/openai-codex-jason-liu-guide ），整理自 [AI Engineer 大會 workshop 影片](https://www.youtube.com/watch?v=il1c1a2FufU)（2026-07-24 發布，70 分鐘）。**與本條目 11 技巧同一講者**，此處補上 workshop 才有的操作細節。

> ⚠️ **長對話／compaction 那一段不在此展開**——已由主流程寫進 [[claude-code-context-management]] 的「世代反轉注記」（含 Jason Liu 自述的五週對話串、約 400 個子代理，以及他自己講明的代價：壓縮品質不穩、細節可能遺失、快取失效時 token 成本墊高）。

### 三幕框架：灌情境 → 讓 AI 工作 → 對外行動

#### 第一幕・輸入頻寬（補強本頁技巧 2「語音輸入」）

- **講話約是打字的三倍速，且對 AI 不需要組織語言**。一段雜亂的 15 分鐘語音備忘錄，夾雜「我記得上週好像跟 Charlie 開過一個關於 SDK 的會」這種模糊線索，AI 會自己翻 35 則會議訊息找出他指的是哪一場
- **Appshots 的完整操作步驟**（補強本頁既有補充資料）：
  1. 在 Codex 開啟要接續工作的對話
  2. 切換到目標 App，把視窗放到最前面
  3. **同時按下鍵盤左右兩側的 ⌘ Command 鍵**
  4. 首次使用允許兩項 macOS 權限：「螢幕與系統音訊錄製」、「輔助使用」
  5. Appshot 附加到對話後再輸入要求
  - 🔑 **關鍵差異**：傳統截圖只有像素；Appshot 連同**系統輔助使用介面（accessibility）讀得到的文字**一起送出，一張 Slack 頻道的 Appshot 能帶上頻道代號與成員帳號 ID，AI 一次呼叫就能直接發文回覆
- **個人記憶庫結構**（補強本頁技巧 11 Shared Memory）：公開範本 [jxnl/personal-monorepo-template](https://github.com/jxnl/personal-monorepo-template)
  - `projects/` 每一條工作流｜`people/` 協作者與長期人物脈絡｜`AGENTS.md` 工作規範
  - **核心原則：程式碼歸專案 repository，vault 專心保存人物、決策、待辦與專案狀態**
  - 記憶庫做 git 版控，出門幾天回來跑一次 `git diff` 就能**驗收 AI 更新了什麼**
- **「像我一樣寫」skill**（可直接複製）：
  ```
  讀我過去六個月寫的所有 email 與 Slack 訊息，整理一份「像我一樣發訊息」的風格指南，之後幫我代擬任何訊息時都套用它。
  ```
  再補一句讓它自我進化：
  ```
  以後每次執行這個技能，如果你學到新東西，可以直接修改技能檔案本身。
  ```

#### 第二幕・thread automation（補強本頁技巧 8「自動化排程」）

觸發語就是一句自然語言 **「keep an eye on this」（幫我盯著這件事）**，Codex 會據此建立官方所稱的 **thread automation**——類似心跳（heartbeat）的定期喚醒機制，把訊息定時排回**同一條對話串**；同一條可掛多個排程，也能調頻率或設條件停止。

> 🔑 **關鍵修正**：早期自動化每次都開新對話，**如今正確做法是讓同一條對話串被反覆喚醒，記憶才能累積**。

可複製的兩則指令：
```
盯著這個 pull request：有新的意見回饋就修掉，隨時保持可合併、rebase 在主分支上、CI 測試通過。
```
```
幫我建立並釘選一條「幕僚長」對話串：每天早上 8 點喚醒，檢查我所有已連接的服務，整理出今天最重要的事，附上可直接點開的連結回報給我。
```

**Goal 模式補充**（補強本頁技巧 9）：2026-05-21 於 Codex app、IDE 擴充與 CLI 全面開放；官方影片提到已有使用者讓單一目標連續推進**超過 100 小時**。Jason Liu 自建的 **Ultra Goal** 流程是把目標寫進 `GOAL.md` 檔案——好處是**任務跑到一半可直接改檔案調整範圍**，再配一份工作日誌掌握進度。
⚠️ 他用這招把 Python 的 Rich 套件改寫成 Rust 版（完成條件＝原有單元測試全過），但**自己直說這是測試極限的實驗「你不該在正式工作裡這樣做」**。

#### 第三幕・對外行動與護欄

- 案例：讓 Codex 示範 iMovie 操作、用 DocuSign 簽文件、找線上傳真服務傳醫療紀錄；最經典是亞馬遜退款——客服排他 75 分鐘等候，他把交涉全權交給 AI 去洗澡，回來時信用卡入帳約 400 美元
  ```
  客服把我排進等候隊伍了。每 5 分鐘查一次進度，等候時間低於 5 分鐘後改成每分鐘查，輪到之後持續交涉，直到拿回退款為止。
  ```
- 🔴 **他自己提出的警告（最值得記的一句）**：模型太「執著」時會自己繞路——某次 Slack 連接器不能上傳檔案，模型就打算自己打開瀏覽器按送出。
  > **AI 繞過限制完成任務，和 AI 繞過安全機制，是同一種行為。**
- **分層護欄**：`AGENTS.md` 是**行為指引，不是繞不過的硬性邊界**；真正能強制的是**沙箱、權限核准政策、網路規則**這類系統層控制，組織還能再加管理設定（他所在環境限制 AI 不能寄信給公司外信箱）。他自己搭配 **auto review 權限模式**：動作要跨出沙箱邊界時，把預定行動連同前後脈絡交給審查代理把關，低風險自動放行、高風險停下要人確認
  → 這與 [[awesome-agent-architecture-from-scratch]] 的 **Capability scaffolding vs Control plane 二分法**完全同構

### 💰 省 token 三招（問答段，最實用）

Jason Liu 點名的迷思：**很多人把推理強度全開到最高（x-high），以為會拿到最好結果**。實際上調高推理強度是在速度、運算量與思考深度之間取捨，**不保證每個簡單任務都因此更好**——他舉例朋友只是要找折價券卻開 x-high，模型跑了兩分鐘掃遍各網站比價。他自己的幕僚長對話串**預設中等強度**，簡單的表單、報到類任務甚至交給輕量模型。

1. 指示 AI「沒有更新就只回覆『無更新』」：
   ```
   這條排程如果沒有新進展，只回覆「無更新」兩個字。
   ```
2. **檢視喚醒頻率**——這件事真的需要每 30 分鐘醒一次，還是每天一次就夠？
3. **為長期任務設停止條件**，達標就休眠

> ⚠️ **前提揭露**：他是 OpenAI 員工，token 用量幾乎不設限；他自承部分做法「聽起來像 AI 精神錯亂」，並承認「派子代理逐條驗證待辦清單」這類設定**非常燒 token，一般用戶未必划算**。務實路徑是從幕僚長對話串這類低成本設定試起。

---

## 補充資料：Sol 規劃 ／ Luna 執行的子代理設定（2026-08-03 併入）

來源：赤紅翼零 DeTools（2026-08-03）〈讓 GPT-5.6 Sol 規劃，GPT-5.6 Luna 執行，Codex 極致省錢大法〉（https://tools.wingzero.tw/articles/4006 ）。**原文僅 1.3KB，只有操作細節可取。**

> 🔴 **不可引用的說法**：原文宣稱此設定「讓 ChatGPT Plus 也能發揮接近 **Pro 方案 20 倍** 的使用效果」——**未附任何來源、測試方法或數據，本知識庫視為未經證實，不得引用**。

**可取的操作細節**：Codex 支援自訂子代理（subagent），可做**模型分工**——`GPT-5.6 Sol` 負責決策與規劃，`GPT-5.6 Luna` 負責執行與處理大量工作。

1. 前置：**設定 → 組態（Configuration）→ 可用推理強度（Available Reasoning Effort）**，把 **Max** 選項開啟
2. 把這段提示詞交給規劃端（Sol）：

> 請在 `~/.codex/agents/luna-worker.toml` 建立一個名為 `luna_worker` 的自訂代理程式。使用以下設定：
> ```toml
> model = "gpt-5.6-luna"
> model_reasoning_effort = "max"
> ```
> 請為它加入適合執行**範圍明確之委派工作**的描述與指令。保留我其餘的設定不變，並根據我目前安裝的 Codex 版本驗證此設定是否有效。接著顯示設定變更差異（diff），之後在需要執行子代理任務時使用 `luna_worker`。

**可帶走的**：子代理設定檔位置 `~/.codex/agents/*.toml`、可指定 `model` 與 `model_reasoning_effort` 兩個欄位、以及「讓 AI 自己寫設定檔＋出示 diff＋依當前版本驗證」這個安全的自我設定手法。

⚠️ **與本頁其他來源的張力**：同頁 Jason Liu 明確反對「推理強度全開到最高」；本篇卻要求把執行端固定在 `max`。兩者未必衝突（分工架構下執行端負責重活），但**若照做，應對照上方「省 token 三招」自行實測，而非預設 max 一定划算**。

## 補充資料：快捷鍵速查與 Quick Chat／Side Chat 額度釐清（2026-09-17 併入）

來源：《數位時代》李先泰（2026-09-14）〈Codex快捷鍵教學！10組必學操作一次看，Quick Chat一鍵叫ChatGPT當軍師〉（https://www.bnext.com.tw/article/92234/codex-keyboard-shortcuts-quick-chat-guide ）。Codex 於 2026 年 7 月 9 日併入 ChatGPT 桌面 App 後，Chat/Work/Codex 三模式共用一個視窗。

**10 組 macOS 預設快捷鍵速查**（括號內為 Windows/Linux 對應鍵）：

| 快捷鍵 | 功能 | 用途 |
|---|---|---|
| ⌘⌥N（Ctrl+Alt+N） | Quick chat（ChatGPT only） | 開一般 ChatGPT 對話，討論完可帶回 Codex |
| ⌘⌥S（Ctrl+Alt+S） | Open side chat（Codex only） | 不離開主任務，開一條支線討論 |
| ⌃⇧M（Ctrl+Shift+M） | Open model picker | 切換模型 |
| ⌃1/⌃2/⌃3（Alt+1/2/3） | Switch to Chat, Work, or Codex | 三種模式互切 |
| ⌘P（Ctrl+P） | Search files（Codex only） | 搜尋專案檔案 |
| ⌃⇧G（Ctrl+Shift+G） | Open review tab（Codex only） | 開 Review 分頁看整批修改 |
| ⌘⌥B（Ctrl+Alt+B） | Toggle review panel（Codex only） | 開關側邊 Review 面板 |
| ⌃`（Ctrl+`） | Toggle terminal（Codex only） | 開關內建終端機 |
| ⌘⌥A（Ctrl+Alt+A） | Next chat needing attention（Codex only） | 跳到下一個待處理對話 |
| ⌘K（Ctrl+K） | Open command menu | 搜尋功能與快捷鍵 |

優先記三組：⌘⌥N、⌘⌥S、⌘K。Review 兩鍵分工：⌃⇧G 開獨立分頁一次檢查整批修改；⌘⌥B 在工作畫面旁展開/收合面板，適合邊下指令邊對照差異（需求專案在 Git 儲存庫內，可切「最後一輪」只看 Codex 剛改動的部分，差異行按「+」留言、再要求「請依留言修改」）。

**Quick Chat vs Side Chat 的糾正說明**：社群流傳「Quick Chat 是省 Codex 額度的官方側門」，**官方文件只能確認兩件事，沒有「省額度」這個結論**：
- OpenAI 說明中心寫明 Codex、ChatGPT Work、ChatGPT for Excel、Workspace Agents 共用同一額度池；Quick Chat（⌘⌥N）開的是一般 ChatGPT 對話，不在此清單內
- 但一般對話本身也有依方案訂的用量上限，官方沒有給出「能省多少」的數字

核心區別：**Quick Chat 是離開 Codex 找 ChatGPT 當軍師**（不污染任務脈絡，適合任務還沒想清楚時）；**Side Chat 是留在 Codex 裡開會**（主任務跑著、旁邊問一下，7/30 更新後 Chrome 擴充功能可把網頁畫記文字直接送進 Side Chat）。

⚠️ 需注意同名混淆：Option+Space 原本是 ChatGPT 舊版 Quick Chat 鍵，7 月合併初期一度誤觸 Codex/Work 任務、被回報耗用大量額度；9/11 更新後已改綁「Pets」桌面小幫手的懸浮控制列，其 Quick Chat 不屬於任何專案，官方未說明額度歸屬。本文與本條目所指 Quick Chat 一律為 ⌘⌥N。

## 相關連結

- [[claude-code-advanced-workflow]] — Boris Cherny 15 個進階技巧（Claude Code 版對比）
- [[karpathy-voice-prompting]] — **同命題不同來源**：Karpathy 的「10 分鐘語音碎念法」與本頁 Jason Liu 的「15 分鐘雜亂語音備忘錄勝過精緻 prompt」是兩個陣營（Anthropic／OpenAI）對同一件事的獨立佐證——賭的都是模型從雜亂脈絡推測意圖的能力
- [[claude-code-context-management]] — **長對話／compaction 世代反轉注記的所在地**（Jason Liu 五週對話串與其代價已寫在該處，本頁不重複）
- [[awesome-agent-architecture-from-scratch]] — Capability scaffolding vs Control plane 二分法，對應本頁「AGENTS.md 是指引、沙箱與權限才是邊界」的分層護欄
- [[codex-computer-use-modes]] — Computer Use 模式細節，對應第三幕的對外行動
- [[claude-code-goal-command]] — Goal 指令的 Claude Code 對照
- [[claude-code-commands-reference]] — 命令速查表
- [[agentic-engineering-sop]] — 六階段 SOP（Goals 對應 Tasks/ROADMAP 設計）
- [[dual-tool-strategy]] — Gemini CLI + Claude Code 分工策略
