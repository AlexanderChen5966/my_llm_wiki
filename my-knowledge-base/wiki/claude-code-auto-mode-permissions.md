---
title: Claude Code Auto Mode 智慧權限機制深挖：Classifier / Boundary / Fallback
date: 2026-07-20
tags: [ai/claude, ai/workflow]
source: https://moksaweb.com/claude-code-auto-mode/
type: article
status: processed
asset_value: 68
---

Auto Mode 是 Anthropic 在 Claude Code v2.1.83 推出的智慧權限模式（Research Preview），介於「每個動作都要 Confirm 的 default」與「跳過所有檢查的 bypassPermissions」之間。它用一個獨立的 Server-Side Classifier Model 在每次 Tool Call 前評估動作安全性，自動處理約 95% 例行動作、擋下約 5% 高風險動作，解決長任務的 Permission Fatigue（官方稱 default 用戶平均每小時按 30-50 次 Approve）。

## T（核心論點）

Auto Mode 不是「無腦 bypass」，而是「智慧 default」——它保留完整的 Permission Layer，靠 ML Classifier 補回安全性。真正的設計精髓是 **Defense in Depth（五層防護）** 與 **Conversation Boundary（對話級動態限制）**：安全不靠單一 Classifier，而靠多層組合；限制不寫死成 Rule，而由 Classifier 每次重讀 transcript 動態偵測。但正因 Boundary 非 Rule，Context Compaction 後可能失效——需要硬保證的場景仍要寫 Deny Rule。

## B（適用場景）

**適用：** 長任務減少 Prompt 中斷（大型 Refactor、Comprehensive Test、Bulk Migration、Documentation 生成、Dependency 升級）；一般開發環境想減 Prompt 但保留安全網。
**不適用 / 注意：** Pro 訂閱用不了（需 Max/Team/Enterprise/API）；只支援 Anthropic API（不支援 Bedrock/Vertex/Foundry）；CI 用 `-p` 遇連續 Block 會直接 Abort，CI 建議改用 dontAsk 配 Pre-Approved Tools；敏感操作（Production Deploy/Migration/Security）仍建議用 default 親自審視。

> ⚠️ 2026-10-08 更新：2026-08-14 起 auto mode 成為 Pro／Max／Team 新對話預設；2026-09-25（v2.1.283）起，終端機與 VS Code 開的對話不分方案都從 auto 開始（例外：版本太舊、管理員關閉、`claude -p`）。見 [[claude-code-plan-mode-permission-rules-layering]]。支援模型清單以現行官方文件為準（本條原列 Sonnet 4.6／Opus 4.6／4.7 屬 2026-07 資訊）。

## R（操作步驟與機制拆解）

**4 個啟用必要條件（缺一不可）**：①Plan＝Max/Team/Enterprise/API（Pro 不支援）②Admin＝Team/Enterprise 需管理員在 Admin Settings 啟用 ③Model＝Sonnet 4.6/Opus 4.6/Opus 4.7（Max 限 Opus 4.7，Haiku 與 Claude 3 系列不支援）④Provider＝僅 Anthropic API。版本需 v2.1.83+。

> ⚠️ 2026-10-08 更新：2026-08-14 起 auto mode 成為 Pro／Max／Team 新對話預設；2026-09-25（v2.1.283）起，終端機與 VS Code 開的對話不分方案都從 auto 開始（例外：版本太舊、管理員關閉、`claude -p`）。見 [[claude-code-plan-mode-permission-rules-layering]]。支援模型清單以現行官方文件為準（本條原列 Sonnet 4.6／Opus 4.6／4.7 屬 2026-07 資訊）。

**啟用方式**：CLI 按 `Shift+Tab` 循環到 auto（首次要 Opt-In）；或 `claude --permission-mode auto`；或 settings.json 設 `permissions.defaultMode: "auto"`；VS Code/Desktop 有獨立 UI。Web/Mobile 只能用 Auto Accept Edits 與 Plan Mode，不支援 Auto。

**Decision Order（First Match Wins 固定順序）**：①Permission Allow/Deny Rules 立即 resolve（不走 Classifier）②Read-Only 動作與 Working Directory Edits 自動 Approve（除 Protected Paths）③其他走 Classifier ④Classifier Block 後 Claude 收到 reason 嘗試替代方案。

**Classifier 抗注入設計**：只看 User Messages / Tool Calls / CLAUDE.md 做決策，**不看 Tool Results**（避免 Hostile File Content 影響評估）；另有 Server-Side Probe 掃描 Tool Results 標記可疑內容 → 形成 Defense in Depth。五層＝Permission Rules → Working Directory Whitelist → Classifier → Conversation Boundary → Server-Side Probe。

**Default Block 動作**：`curl | bash`、送敏感資料到外部、Production Deploy/Migration、Mass Delete 雲端儲存、授予 IAM/Repo 權限、改共享基礎設施、不可逆刪除既存檔、Force Push、直接 Push 到 main。**Default Allow**：Working Directory 本機檔案操作、裝 lock file 宣告的依賴、讀 .env 送對應 API、Read-Only HTTP、Push 到你起始的或 Claude 建的 branch。（`claude auto-mode defaults` 印完整列表）

> ⚠️ 2026-10-08 來源衝突（待查官方文件）：Tim Wei G8.5（依 2026-09 官方文件、v2.1.283）列為**預設不擋**「push 到目前 repo，包括主分支」（push 內容仍會檢查，例如夾帶金鑰）；本條原來源（moksaweb，2026-07）列為預設擋（上方 Default Block「直接 Push 到 main」）。兩者並列、不擇一，**待查官方 permission-modes 現行版本**，可用 `claude auto-mode defaults` 印出本機版本的實際清單。

**Conversation Boundary**：對話中說「don't push」「wait until I review」，Classifier 當 Block Signal 直到你明確 Lift（即使 Default Rules 允許）；Claude 不能自己判斷條件已達成而解除。**限制**：Boundary 非 Rule 而是每次 re-read transcript，Compaction 移除原始訊息後可能 Lost。

**Fallback 機制**：連續 3 次或總 20 次 Block → 暫停回 default 開始 Prompt（Approve 後恢復）；閾值不可調整。被 Block 動作進 `/permissions` Recently Denied，按 `r` 可 Retry 手動 Approve；用 `/feedback` 報 False Positive。**Broad Allow Rules 進 Auto Mode 會被自動 Drop**（如 `Bash(*)`、`Bash(python*)`、Agent allow），離開時恢復；Narrow Rules 如 `Bash(npm test)` 保留。

**Subagent 3 檢查點**：啟動前評估 task description、執行時每動作走 Classifier、結束時 review action history 加 security warning；Subagent frontmatter 的 `permissionMode` 在 Auto Mode 下被忽略。**Token 開銷**約 5-15%（Read 重接近 0%、Network 重可達 15%）。

> 📎 2026-10-08：Tim Wei G8.5 引官方說法，auto mode 的審核在 Pro／Max／Team **不算你的用量**。兩說可以並存（審核本身有 token 開銷，但不計入訂閱用量），本庫並陳，未經官方逐字確認兩者關係。

## C（關鍵行動）

- [ ] Max 以上訂閱者用 `claude --permission-mode auto` 跑一次熟悉任務體驗 Classifier 行為
- [ ] Session 開始花 30 秒寫清楚具體 Boundary（如「這次只改 src 不動 tests」）
- [ ] 需硬保證的禁令寫 Permission Deny Rule，不要只靠 Conversation Boundary（Compaction 後會失效）
- [ ] 跑完到 `/permissions` Recently Denied 檢視 Classifier 攔了什麼，False Positive 用 `/feedback` 回報

## 原始摘要

Moksa 撰寫的中文圈首篇 Auto Mode 深度教學，拆解啟用條件、Classifier 邏輯、Default Block/Allow 規則、Conversation Boundary、Fallback、Decision Order、Subagent 處理與六種 Permission Mode 比較。

## 補充資料：2026-09 現行行為（Tim Wei G8.5）

> 來源：Tim Wei〈Claude code 新手篇 8.5：Auto mode — 你的預設模式到底在幫你擋什麼〉，《Claude Code 實戰手冊／新手篇》G8.5，https://ithelp.ithome.com.tw/articles/10417881 。依 2026-09 官方文件與 v2.1.283 撰寫（頁面無發布日，圖片上傳 2026-09-29）。本段只收**增量**。

1. **classifier 看不到檔案與網頁內容**：審核模型只看使用者訊息與 Claude 要做的動作，所以網頁裡藏的指令沒辦法直接騙它（官方設計）。這**印證**上方「不看 Tool Results」的抗注入設計，相關脈絡見 [[prompt-injection-vs-jailbreak]]。
2. **三種界線**：對話中講（壓縮後可能忘，即上方 Conversation Boundary）／`ask` 規則（auto mode 下也一定會問，例如 `"ask": ["Bash(git push *)"]`：平常一路做，push 前停下來問）／`deny` 規則（審核前就擋，對話中同意也無效）。
3. **「動作＋對象」才算同意**：對話中明確講出動作與對象才算使用者同意。官方例子：「幫我清一清 repo」不等於允許 force push；「把這個 branch force push 上去」才算。
4. **被擋時的訊息格式**：`bash denied by auto mode · [標籤] · /permissions`，方括號是原因標籤；作者實際遇到 `[Credential Exploration]`（Claude 想翻 git 帳密）。
5. **規則是字面比對**：`git -C 資料夾 push` 這種換寫法的 push，對不上 `Bash(git push *)`。所以 ask／deny 是「多一道確認」，不是萬無一失的鎖（同 [[agent-permission-and-os-sandbox]]「字串檢查必然有洞」）。
6. **CLAUDE.md 也會影響審核**：在 CLAUDE.md 寫「永遠不要 force push」，官方說 Claude 與審核模型都會讀到（與上方「只看 User Messages / Tool Calls / CLAUDE.md」一致）。
7. **想少按 y：auto mode 優於「自動接受編輯」**（作者觀點）：「自動接受編輯」會直接放行專案內的 `rm`、`mv`，auto mode 則先交 classifier 看。
8. **官方警語原文**：「Auto mode reduces permission prompts but does not guarantee safety. Use it for tasks where you trust the general direction, not as a replacement for review on sensitive operations.」
9. **`git stash` 實驗**（作者實測一次，自承不保證重現）：要求「恢復乾淨狀態」時，Claude 自選可回復的 `git stash`；作者明說「stash 也刪掉」後即放行（刪 stash 本在封鎖清單，但「動作＋對象」講清楚了）。

**本篇確認仍成立（2026-09）**：連續 3 次／累計 20 次被擋即回到每步詢問、`/permissions` → Recently denied 按 `r` 重試、讀 `.env` 預設允許（依 [[agent-api-key-handoff-six-methods-risk]]，讀 `.env` 等於把金鑰貼進對話，這正是 G8 建議寫 `Read(./.env)` deny 的理由）。

**反例**：同系列番外實測中，Sonnet 5 為檢查版面開 Safari、截整個螢幕，auto mode 都沒擋（作者單次觀察），見 [[claude-models-real-task-eval-tokyo-trip]]。

## 相關連結

- [[claude-code-dynamic-workflows]] — Dynamic Workflows 搭配 Auto Mode 效果最強（Auto 是三模式中的「執行連續性」層）
- [[claude-code-three-modes-distinction]] — Plan/Auto/Dynamic Workflows 三模式定位區辨
- [[claude-opus-4-8-release-notes]] — Opus 4.8 與 Auto Mode 擴大使用同期發布
- [[claude-code-goal-command]] — /goal 搭配 Auto Mode 實現「設好就走」的多輪自動完成
- [[claude-code-plan-mode-permission-rules-layering]] — 同系列 G8：模式是暫時的、規則才是永久的；規則優先序與設定分層
- [[claude-models-real-task-eval-tokyo-trip]] — 反例：Sonnet 5 開 Safari、截整個螢幕，auto mode 沒擋（作者單次觀察）
- [[agent-api-key-handoff-six-methods-risk]] — auto mode 預設允許讀 `.env`；依該篇，等於把金鑰貼進對話
- [[prompt-injection-vs-jailbreak]] — classifier 看不到檔案與網頁內容的抗注入設計
- [[agent-permission-and-os-sandbox]] — 字面比對的規則必然有洞，deny 是底線不是保險箱
