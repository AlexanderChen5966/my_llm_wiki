---
title: Opus 5.5 時代的 Claude Code：CLAUDE.md 少叮嚀、環境多設限、驗證點取代人工盯場（Lydia Hallie）
date: 2026-11-19
tags: [ai/claude, ai/agent, ai/workflow, type/how-to]
source: https://www.bnext.com.tw/article/93081/claude-code-less-is-more-opus-5-5-best-practices
type: article
status: processed
asset_value: 60
---

數位時代（李先泰，2026-11-17）整理 Anthropic 的 Claude Code 開發者關係負責人 **Lydia Hallie** 在 AI Engineer 的演講〈Rethinking Claude Code Best Practices〉（2026-11-12 上線）。⚠️ **二手整理**：文末自註「初稿為 AI 編撰」，數字與版本號未對一手核對。

## T（核心論點）

> **模型變強之後，該少的是對 AI 的叮嚀，該多的是對環境的設限。人退出迴圈的每一步，都要換成一道「由模型以外的東西」執行的限制或檢查。**

- **少**：`CLAUDE.md` 只留**程式碼看不出來的事**（例如某服務還連著程式碼裡沒直接引用的舊資料來源）；技術棧、檔案位置刪掉。「IMPORTANT！」「絕對不要！」這類強硬指令在 Opus 5.5 上**可能讓模型偏離方向、工具呼叫變多、結果更差**
- **多（安全）**：判準是「**Claude 做錯一次，收得回來嗎？**」——收得回來的交給模型；收不回來的寫成 **deny 規則**（在 auto mode 分類器之前生效），再用**沙盒或雲端乾淨機器**框住環境，因為「權限只管一條指令能不能跑，管不了它跑起來後碰得到什麼」
- **多（品質）**：設**驗證點**（任何能回答「過或沒過」的東西：測試、建置、截圖），人只看驗證點，不讀整包改動
- **結語**：最佳實踐沒消失，只是搬了家——導航→模型、核准→auto mode、限制→deny 規則與環境、把關→測試／hooks／CI、啟動→Claude Code projects。人只剩**開第一個 session** 與**驗收最後結果**

| 人原本做的事 | 搬到哪裡 |
|---|---|
| 導航（提醒它該做什麼） | 模型本身 |
| 核准（逐條按允許） | auto mode 分類器 |
| 限制（能做什麼） | deny 規則＋沙盒／雲端環境 |
| 把關（看改動對不對） | 測試、hooks、CI |
| 啟動（開每個 session） | Claude Code projects 主 session |

與 [[boris-cherny-prompt-to-verification]] 同一方向（刪提示、賭注移到驗證），本篇多補了**安全軸**（可逆性判準、deny／沙盒、npm test 攻擊鏈）與**驗證點的三層落點**（指令／hooks／CI）。

## B（適用場景）

**適用：**
- 已累積大量 CLAUDE.md／skills／hooks，想依新模型汰舊並決定「刪掉後要補什麼」
- 想讓 Claude Code 長時間在背景或雲端跑，需要設計不靠人盯的安全與品質關卡
- 檢查自己的 `settings.json` deny 規則與 hooks 是否真的擋得住

**不適用 / 注意：**
- 前提是新模型＋auto mode；舊世代模型仍需較多導航
- 🔴 **deny 規則管不到腳本自行開檔**：`Read(./.env)` 擋得住讀檔工具與它認得的 `cat`，但 `npm test` 跑起的第三方套件或臨時腳本照樣能讀——要靠沙盒；**沙盒也管不到 MCP 伺服器與 hooks**
- 🔴 **projects 派出的雲端 session 不套用 repo 設定檔裡的 deny 規則、也不執行 hooks**；要用這兩樣得在本機跑
- 自動合併不是預設；人工審查量依風險分級
- 版本號（v2.1.283）、實驗數字（13.6% vs 89%）、合併量（每日 2,500→3 萬筆）皆為媒體轉述

## R（操作步驟）

1. **刪叮嚀**：執行 `/doctor prompt-audit`（需 v2.1.283+）找出新模型不需要的指示；刪到 `CLAUDE.md` 只剩程式碼看不出來的事（範圍陷阱見 [[claude-code-prompt-audit-scope-trap]]）
2. **按可逆性分流**：可還原的錯誤交給模型；不可還原的（丟掉未提交改動、把密碼讀進對話紀錄）寫進 `settings.json` 的 `permissions.deny`，例如 `Read(./.env)`
3. **框住環境**：本機用沙盒（OS 層限制可讀檔案與可連線位置，含子程序）；更推薦雲端 session——乾淨機器、從 repo 重新 clone、不帶本機未提交檔案與憑證。雲端要 API 金鑰時設成 **network secret**（代理伺服器附在請求上，模型看不到）
4. **雲端帶得走的設定要 commit**：單一 repo 的一般雲端 session 會讀 repo 內 `.claude/settings.json` 的 deny 與 hooks，所以要 commit 進 repo
5. **設三層驗證點**：
   - **指令層**：寫清楚「完成長什麼樣」，要求拆任務、各配測試、**貼出實際輸出**
   - **hooks 層**：改檔、標記完成、結束 session 各綁對應檢查；🔴 用 `TaskCompleted`、`Stop` 擋完成時，**失敗要回傳 exit code 2**——回傳 1 只算一般錯誤，任務仍會被標成完成
   - **CI 層**：每個 PR 跑同一套檢查，沒過讓 Claude 自己回去修
6. **審查分級**：Claude Code 團隊每個 PR 先由**不帶原 session 記憶**的另一個 Claude 審查，再由另一組 agent 複核它找到的 bug；文件／文案全綠免人看、一般變更一人審、高風險兩人
7. **派工（選用）**：Claude Code projects（公測，Pro／Max／Ultra 個人方案）主 session 只拆工作與追蹤、不寫程式；建議主 session 用 Fable、執行 session 用 Opus 5.5
8. **起手式**：挑一件比平常大很多的任務，確認 CI 抓得到在意的事、設好網路權限與機密，丟雲端跑，**看它卡在哪——卡住的地方就是下一件該交出去的事**

## C（關鍵行動）

- [ ] 對照本庫 CLAUDE.md／`~/.claude/CLAUDE.md`：逐條問「這是程式碼看不出來的事嗎？」，不是就刪；清掉驚嘆號式強硬指令
- [ ] 檢查 `settings.json` 的 `permissions.deny` 是否有 `Read(./.env)` 等不可逆項目
- [ ] 檢查現有 Stop／TaskCompleted hooks 的失敗回傳值是 **2** 而不是 1
- [ ] 需要長時背景任務時，優先雲端 session，並把 deny／hooks commit 進 repo 的 `.claude/settings.json`
- [ ] 下長任務指令時固定附「完成長什麼樣＋各任務配測試＋貼實際輸出」

## 原始摘要

Anthropic 工程部落格（10/13）稱每日合併程式碼變更從 2026 年 5 月約 2,500 筆增至 9 月逾 3 萬筆。Hallie 描述內部工作方式從一次盯一兩個 session 變成同時管幾十個、每個跑數小時、中途自我驗證，甚至由一個 session 指揮其他 session（「我們基本上變成了幾十個 session 的管理者」）。今年初 Opus 4.5 時代會提早停下、人得不斷提醒，auto mode 尚不存在，所以強硬指令、逐條核准、逐檔看改動「只在那個時間點是對的」。Anthropic 8 月對照實驗：1,053 名付費專業測試者面對明顯危險指令只攔下 13.6%，auto mode 分類器攔下 89%。範例「把登入系統搬到新的 API」可能留著舊 API、只搬新使用者、或把所有人登出，三種結果都看似合理——所以需要驗證點。文末給 5 個調整（prompt-audit、deny `.env`、hooks exit code 2、寫清完成樣貌、network secret 與 repo 設定）。業配延伸閱讀已略。

## 相關連結

- [[boris-cherny-prompt-to-verification]] — 同方向的另一位發言人（Boris Cherny）：ablation 刪提示、賭注移到驗證；本篇補安全軸與驗證三層落點
- [[claude-5-context-engineering-rules]] — 官方 Claude 5 世代上下文工程新規：從「防呆」轉「授權」
- [[claude-md-optimization-techniques]] — CLAUDE.md 精簡技巧與 Claude 5 修訂版；本篇「只留程式碼看不出來的事」可作為刪減判準
- [[claude-code-prompt-audit-scope-trap]] — `/doctor prompt-audit` 的範圍陷阱與實際偵測範圍
- [[claude-skill-audit-four-step-minimal-method]] — 反對「全刪」的稽核法，與本篇「刪完補限制」可並讀
- [[claude-code-auto-mode-permissions]] — auto mode 分類器與 deny rule 的分工
- [[agent-permission-and-os-sandbox]] — 字面比對的規則必有洞，deny 是底線、沙盒才是環境邊界
- [[claude-code-projects-redesign-2026-09]] — projects 協調者＋平行 threads
- [[claude-code-hooks-memory]] — hooks 機制
