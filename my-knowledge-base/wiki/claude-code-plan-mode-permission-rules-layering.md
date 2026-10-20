---
title: 模式是暫時的，規則才是永久的：Plan Mode、權限模式與設定分層（2026-09 現行版）
date: 2026-10-08
tags: [ai/claude, ai/workflow, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10417723（Tim Wei〈Claude code 新手篇 8：先想清楚再動手 — Plan Mode 與權限模式〉，「Claude Code 實戰手冊／新手篇」G8；頁面無發布日，圖片上傳 2026-09-27，依 2026-09 官方文件撰寫）
type: article
status: processed
asset_value: 64
---

同系列：G5 [[claude-code-session-handoff]]、G7 [[claude-code-model-and-thinking-depth]]、G8.5 併入 [[claude-code-auto-mode-permissions]] 的補充段。作者 Tim Wei，iT 邦使用者 ID 20182796 是從圖片上傳路徑推得，頁面上沒有直接顯示。

## T（核心論點）

> **Claude stops when the work looks done.**（Claude Code 官方〈Best practices〉）

沒人先講清楚「做完」長什麼樣子時，「看起來做完」就是它唯一的判斷標準，所以容易交出一個空殼。很多人的對策是在對話裡口頭約定「先別動手、先討論」，但**那只是一句話**：它活在對話裡，對話變長被壓縮後就會被擦掉。

官方文件的說法（作者轉述大意）：對話裡講的界線不會被存成規則，每次檢查都重新讀對話，所以 context 被壓縮、那句話不見了，界線就跟著不見；**要硬保證，就寫一條 deny 規則**。

> 🔑 **Plan Mode 與 deny 規則做的是同一件事：把「你講過的話」變成「系統守得住的界線」。Plan Mode 管當下這一段，deny 規則管永久。**

這和 [[claude-code-auto-mode-permissions]]「Conversation Boundary 壓縮後可能失效、要寫 deny」是同一論點，classifier 機制見該條，本篇不重述。

## B（適用場景）

**適用：** 專案最初期、方向還沒定的時候（不是「難的任務才用」）；想確認自己現在在哪個模式；設定「永遠不准」的事；網路教學的模式設定照抄後沒生效。

**不適用 / 注意：**

🔴 **日期門檻（會讓舊教學失效）**：
- **2026-08-14 起**，auto mode 成為 Pro／Max／Team **新對話預設**（需 macOS／Linux／WSL v2.1.228+、原生 Windows v2.1.233+）。
- **2026-09-25 v2.1.283 起**，終端機與 VS Code 開的對話**不分方案**都從 auto 開始。例外只有三種：版本太舊、管理員關閉 auto mode、用 `claude -p`。
- **2026-09-01 起**，寫在**專案層**的 `defaultMode: bypassPermissions` **會被忽略**（以為設了，對話起來還是 Manual），只能寫在個人層 `~/.claude/settings.json` 或用啟動參數；`auto` 則**一直都**不能寫在專案層。
- 用 `--dangerously-skip-permissions` 開的對話，**連 Plan Mode 都擋不住改檔**：它仍被告知先規劃，但真的去改時不會被攔。官方警告框建議的替代是 auto mode。

⚠️ **deny 是底線，不是保險箱**：規則是字面比對，擋了 `git push` 還是有繞道寫法（例如 `git -C 資料夾 push`），見 [[agent-permission-and-os-sandbox]]。

⚠️ 模式名稱、狀態列字樣、核准選項文字、版本門檻都依 **2026-09 官方文件**，這塊改得特別快，畫面上的字以你眼前看到的為準。

## R（操作步驟）

### 1. 先看你在哪個模式

畫面下方狀態列的字樣：`⏵⏵ auto mode on`／`⏸ manual mode on`／`⏵⏵ accept edits on`／`⏸ plan mode on`。

- **Shift+Tab 循環**：從 auto 按第一下會**跳回 Manual**，之後 Manual → 自動接受編輯 → Plan → 回 Manual。
- **Manual 就是舊稱 default**：2026-07 改了顯示名稱，設定檔裡仍寫 `default`。

### 2. 四種模式是一條光譜（軸＝動手前要問你多少）

| 模式 | 態度 | 什麼時候用 |
|---|---|---|
| Plan Mode | 只看、只想，不動你的原始碼 | 專案最初期、方向還沒定 |
| Manual | 每個動作都問 | 動很敏感的東西、想盯著它 |
| 自動接受編輯 | 改檔不問，其他照問 | 方向定了，只剩逐檔修改 |
| auto mode | 大部分自己判斷，有風險才問 | 日常主力，現在也是預設 |

- ⚠️ 「自動接受編輯」不只改檔，也放行工作目錄內的 `mkdir`、`mv`、`cp` **與 `rm`**。
- 不在循環裡的兩個：`dontAsk` 其實**最嚴格**（沒事先允許的一律拒，給自動化腳本用）；`bypassPermissions` 什麼都不問。

### 3. Plan Mode 怎麼用

進入方式三選一：

```bash
Shift+Tab                       # 按到狀態列出現 plan mode on
/plan 幫我規劃這個新專案的架構    # 進入並順便給題目（作者最常用）
claude --permission-mode plan   # 啟動時就進
```

- ✅ 可以讀檔、可以跑「看一下情況」的查詢指令、**會把計畫寫成檔案**；❌ 不改原始碼。精確說法是「不動你的原始碼」，不是「什麼都不寫」。
- Plan 內的非唯讀指令，只要 auto mode 可用，預設交給 auto mode 的 classifier 審，不會每條都問。
- 作者實測：空資料夾下 `/plan 幫我規劃一個個人作品集網站`，它先反問四個問題（視覺風格、內容來源、語言、作品呈現）才攤計畫。

**計畫出來後三個選項**：核准並切到 auto mode（文字依環境可能顯示成「自動接受編輯」等）／核准但每次改檔都確認／告訴它哪裡要改。在計畫畫面按 **`Ctrl+G`** 可用系統編輯器直接改計畫。核准後會離開 Plan Mode，並**自動以計畫內容替對話命名**，之後 `/resume` 好找。

**補上 G7 欠的定義**：`opusplan` 判斷「規劃階段」的依據**就是是否在 Plan Mode**。Plan 裡用 Opus 想，核准後動手自動換 Sonnet，所以 `opusplan`＋Plan Mode 是一組（見 [[claude-code-model-and-thinking-depth]]）。

**別搞混**：Plan Mode 是**預防**（動手前）、`/rewind` 是**急救**（改壞後）、交接檔是**交班**（換對話時），後兩者見 [[claude-code-session-handoff]]。

### 4. 規則：deny → ask → allow，先中先贏

```json
{
  "permissions": {
    "allow": ["Bash(npm run *)", "Bash(git commit *)"],
    "deny":  ["Bash(git push *)", "Read(./.env)"]
  }
}
```

- 判斷順序 **deny → ask → allow**，先中的贏；只要有一條 deny 中了，後面寫幾條 allow 都救不回來。
- ⚠️ **萬用字元陷阱**：`*` 要放在子指令後面。官方點名 `Bash(git * main)` 看似「只允許跟 main 有關的 git 指令」，實際上**連 `git push origin main` 一起放行**；要擋 push 就明寫 `Bash(git push *)`。
- `Read(./.env)` deny 的有效範圍（擋 Read 工具與點名指令，擋不住腳本自行開檔）見 [[agent-api-key-handoff-six-methods-risk]]。

### 5. 設定分層

| 檔案 | 誰吃到 | 適合放 |
|---|---|---|
| `~/.claude/settings.json` | 你所有專案 | 個人底線（例如不准讀 `.env`） |
| `.claude/settings.json` | 這個專案，**進版控** | 團隊共用規矩 |
| `.claude/settings.local.json` | 只有你，**不進版控** | 你在這專案的偏好 |

優先序：**命令列參數 > local > 專案 > 個人**；但**任何一層的 deny 都生效**。檔案該不該 commit 另見 [[claude-code-file-management]]。managed settings 是給公司 IT 管理員用的，個人方案用不到。

### 6. 階段建議

方向未定用 **Plan** → 方向定了用 **auto**（日常主力）→ 碰資料庫、正式環境、上線時切回 **Manual** → 永遠不可碰的寫成 **deny**。「懶得一直按 y」的正解是 auto mode，不是 `--dangerously-skip-permissions`。

作者引用史丹佛 2023 研究（arXiv 2211.03622）：有 AI 助手幫忙的人寫出的程式較不安全，卻更相信自己寫得安全。

## C（關鍵行動）

- [ ] 先認得狀態列那一行字，知道自己現在在哪個模式（8 月後預設已是 auto）
- [ ] 方向還沒定的新專案，用 `/plan <題目>` 開場；計畫不對就選「告訴它哪裡要改」或 `Ctrl+G` 直接改
- [ ] 把最不能接受的兩三件事寫成 deny（例如 `Bash(git push *)`、`Read(./.env)`），放在 `~/.claude/settings.json`
- [ ] 檢查既有設定有沒有 `Bash(git * main)` 這類萬用字元放錯位置的 allow
- [ ] 專案層設定檔裡若有 `defaultMode: bypassPermissions`，知道它 9/1 起已被忽略；不要用 `--dangerously-skip-permissions` 取代 auto mode

## 原始摘要

Tim Wei「Claude Code 實戰手冊／新手篇」G8。從「AI 交出看起來做完的空殼」出發，指出口頭約定會被壓縮擦掉，Plan Mode（當下）與 deny 規則（永久）才是系統守得住的界線。內容含 8/14 與 9/25 預設改為 auto mode、9/1 起專案層 bypass 被忽略、Manual 即舊稱 default、Shift+Tab 循環、Plan Mode 能做與不能做的事、計畫核准三選項與 `Ctrl+G`、`opusplan` 的規劃階段定義、規則優先序與萬用字元陷阱、設定三層優先序。iT 邦側欄推薦影片（含「iThome鐵人賽」標籤的 webinar 廣告）與留言、追蹤列不收；本系列並非鐵人賽。

## 相關連結

- [[claude-code-three-modes-distinction]] — Plan／Auto／Workflows 三模式的定位區辨；本篇補上實際操作與 2026-08／09 預設變更
- [[claude-code-auto-mode-permissions]] — auto mode 的 classifier 機制、預設擋與不擋（含 G8.5 補充段與 push 到 main 的來源衝突）
- [[claude-code-model-and-thinking-depth]] — 同系列 G7：`opusplan` 的規劃階段就是 Plan Mode
- [[claude-code-session-handoff]] — 同系列 G5：`/rewind` 與交接檔
- [[agent-permission-and-os-sandbox]] — 字串檢查必然有洞，deny 是底線不是保險箱
- [[agent-control-layer-permission-hooks-planning]] — 自建 agent 的 Capability／Permission／計畫控制層通用架構
- [[claude-code-file-management]] — `settings.local.json` 等設定檔的版控取捨
- [[agent-api-key-handoff-six-methods-risk]] — `Read(./.env)` deny 擋得到與擋不到的地方
- [[claude-models-real-task-eval-tokyo-trip]] — 同系列番外：auto mode 沒擋 Sonnet 5 開 Safari、截螢幕，作者事後以 deny 規則封住
