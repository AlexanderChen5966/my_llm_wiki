---
title: 把 API 金鑰交給 AI agent 的 6 種方式：金鑰字串離 AI 看得到的地方越近，外洩路徑越多
date: 2026-10-08
tags: [ai/agent, ai/mcp, type/comparison]
source: https://codelove.tw/@tony/post/qJRyNq（codelove「小編精選－技術文章翻譯」，2026-10-06 翻譯發布；原文日期未載）；原文 https://qiita.com/songchong/items/873b4f14d26296176cfd ；完整版 https://renkeimap.jp/guide/decide/ai-agent-keys（IT 連結地圖編輯部）
type: article
status: processed
asset_value: 64
---

## T（核心論點）

> **金鑰字串離 AI 看得到的地方越近，外洩路徑就越多。最安全的是根本不把金鑰字串交給 AI，改用 OAuth。**

從使用者的操作來看，金鑰的傳遞方式可分成 6 種，風險由高到低（作者排序）：

| # | 方式 | 會發生什麼 | 評價 |
|---|---|---|---|
| ① | 貼進對話 | 送到 AI 公司，也留在對話紀錄 | 不建議 |
| ② | 寫進 `.env` 讓 AI 讀 | AI 一讀到，內容就進入對話；排除設定只擋 AI 的「讀取」功能 | 不建議 |
| ③ | 環境變數（如 `.zshrc`） | AI 看不到值，但它執行的指令一開始就繼承 | 需要縮限設定 |
| ④ | 寫進 MCP 設定檔 | 會隨設定檔一起外洩 | 改用環境變數參照，不要直接寫入 |
| ⑤ | OAuth 授權 | 不交出金鑰字串，以期限與範圍授權，外洩損害較小 | **最安全** |
| ⑥ | 雲端金鑰保管處 | Claude Code 與 Codex 的雲端中，AI 不持有真正的金鑰 | 視工具而定 |

## B（適用場景）

**適用：** 設定 MCP server 或讓 agent 呼叫外部 API 之前；檢查自己的 Claude Code／Codex／Gemini CLI／Copilot／Cursor 設定；評估「讓 AI 讀 `.env`」的風險。

**不適用 / 注意：**
- ⚠️ **原標題稱「實測」，實為官方文件比對調查**（IT 連結地圖編輯部，經 codelove 翻譯）。正文明言從 5 款工具的官方文件出發、逐行引用原文，全文沒有實際操作測試。本條標題不寫「實測」。
- 🔴 **`cat .env` 會不會被 deny 擋，兩個時間點說法不同，並陳**：
  - [[claude-code-tools]]（**2026-05**，israynotarray）：`Read(./.env)` 的 deny 只擋 Read 工具，**不擋** `Bash cat .env`。
  - 本篇（**2026-10**，依官方文件）：Claude Code **也會擋** `cat .env` 這類直接點名檔名的指令。
  - 可能是版本行為改變，**未確認**。兩篇一致的部分：`grep -r` 這類不點名檔名的讀取、或腳本自行開檔（`python script.py`）**一律擋不住**，需要 OS 沙箱（見 [[agent-permission-and-os-sandbox]]）。
- ⚠️ 第三方數字照原文標來源：Astrix Security（2025）調查公開 MCP server，**53%** 用長期固定金鑰、OAuth 僅 **8.5%**；Tracebit 實證 Gemini CLI 讀到藏有指令的文字時，可能把**整個環境變數**送出去。
- ⚠️ 各工具行為依原文調查當時的官方文件，版本會變。

## R（操作步驟）

### 1. 對話與紀錄會流去哪（方式①）

- Claude Code 與 Codex 把對話存在本機；**Claude Code 以未加密的明文保存**（官方 data-usage，原文轉述）。
- Copilot CLI 初始設定會把對話同步到 GitHub 帳號。
- Cursor 關閉 Privacy Mode 時會保存提示詞，且可能用於訓練。
- Google 使用條款要求免費版 Gemini API 不要傳送敏感資訊。

### 2. `.env` 與排除設定擋得到哪裡（方式②）

| 讀取方式 | 是否被擋 |
|---|---|
| AI 工具內建的「讀取檔案」功能（`Read(.env)`） | 擋得住 |
| 直接點名檔名的指令（`Bash(cat .env)`） | 視工具：Claude Code 會擋（本篇說法，見 B 層並陳）；Cursor 寫明代理人終端機擋不住 |
| 不點名檔名（`grep -r`）、腳本自行開檔（`python`） | **擋不住**，要靠沙箱 |

另外：**Gemini CLI 會自動載入 `.env`**；`.geminiignore` 存在，但 `.env` 預設仍會讀入，相關隱藏設定的預設值說明互相矛盾（原文指出）。和同批條目的關聯：auto mode **預設允許讀 `.env`**（[[claude-code-auto-mode-permissions]]），依本篇等於把金鑰貼進對話，這正是 G8 建議寫 `Read(./.env)` deny 的理由（[[claude-code-plan-mode-permission-rules-layering]]），但 deny 只擋 Read 工具與點名指令。

### 3. 各工具的金鑰保護設定

| 工具 | 排除讀取 | 環境變數縮限 | MCP 金鑰 | OAuth |
|---|---|---|---|---|
| Claude Code | `Read(./.env)` deny，也擋 `cat` 等點名指令 | 預設會傳遞，縮限設定要自己開 | 持有金鑰的 MCP server 可放在自己的範圍內 | 支援 |
| Codex | 可寫拒絕讀取 `.env` 的規則 | 預設**全部傳遞**，用 `shell_environment_policy` 縮限 | 用環境變數名稱指定 | 支援，`codex mcp login` |
| Gemini CLI | `.geminiignore`，但 `.env` 預設自動讀入 | （原文未列） | 建議不要直接寫入金鑰 | 支援 |
| GitHub Copilot | Content exclusion 不適用於 CLI 與代理人模式 | `--secret-env-vars` 隱藏 | 用 `$VAR` 參照 | CLI 支援，雲端代理人**不支援** |
| Cursor | `.cursorignore` 預設排除 `.env`，但終端機不在範圍 | **找不到**縮限設定 | 建議用環境變數 | 支援 |

每種工具都有「一開始什麼都沒做」的地方；Claude Code、Copilot CLI、Codex 都有縮限環境變數的設定，但**都要使用者自己啟用**。

### 4. 要交出金鑰時的 5 步

1. **不要把金鑰放進對話**
2. **建立權限縮小過的金鑰**
3. **能選 OAuth（有期限與範圍的授權）就優先用**
4. **用排除設定加沙箱縮小可讀範圍**（要連 AI 執行的指令自行開檔也擋，就得用沙箱）
5. **一旦外洩就立刻撤銷**

## C（關鍵行動）

- [ ] 在 `~/.claude/settings.json` 加 `Read(./.env)` deny，同時記住它擋不住腳本讀檔
- [ ] 檢查自己用的工具有沒有開「縮限傳給指令的環境變數」（Codex：`shell_environment_policy`；Copilot CLI：`--secret-env-vars`）
- [ ] 把 MCP 設定檔裡直接寫死的金鑰改成環境變數參照；能用 OAuth 的 MCP server 改走 OAuth
- [ ] 讓 agent 呼叫外部 API 時，另建一把最小權限、可撤銷的金鑰，不要用主金鑰
- [ ] 用 Gemini CLI 的專案，確認目錄裡的 `.env` 不會被自動載入

## 原始摘要

IT 連結地圖編輯部依 Claude Code、Codex、Gemini CLI、GitHub Copilot、Cursor 五款工具的官方文件，比較把金鑰交給 AI 代理人的 6 種方式（貼對話、`.env`、環境變數、MCP 設定、OAuth、雲端保管處），結論是金鑰字串離 AI 看得到的地方越近外洩路徑越多，OAuth 最安全；附各工具保護設定對照與 5 步建議。原標題寫「實測」，實為官方文件比對。codelove 側欄「川川電商事務所」站長推廣、熱門文章／排行榜、Google 廣告 iframe 不收。

## 相關連結

- [[agent-permission-and-os-sandbox]] — 同一觀察（子行程繼承環境變數、點名指令攔得住但腳本攔不住，要 OS 沙箱）的手刻版
- [[claude-code-tools]] — 2026-05 的「deny 不擋 `cat .env`」說法，與本篇並陳
- [[claude-code-auto-mode-permissions]] — auto mode 預設允許讀 `.env`
- [[claude-code-plan-mode-permission-rules-layering]] — deny 規則寫法與設定分層
- [[claude-code-mods-overview]] — Mods 能讀環境變數與設定（含 API key）、沒有沙箱，屬同一威脅面
- [[prompt-injection-vs-jailbreak]] — Tracebit 案例＝不可信來源 × 高權限出口
- [[mcp-server-stateless-streamable-http-oauth]] — MCP server 的 OAuth 實作面
- [[claude-code-billing-usage]] — `ANTHROPIC_API_KEY` 環境變數陷阱
- [[chatgpt-claude-privacy-protection-9-checkpoints]] — 對話資料的隱私設定檢查
