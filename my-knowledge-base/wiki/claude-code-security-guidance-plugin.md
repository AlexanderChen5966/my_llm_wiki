---
title: security-guidance：Claude Code 官方安全稽核外掛的三層防線與封鎖設計
date: 2026-07-09
tags: [ai/claude, dev/architecture, type/reference]
source: https://codelove.tw/@tony/post/3ojzmx（原文：https://qiita.com/nogataka/items/4d2a551f89f6b4f94b01）
type: article
status: processed
asset_value: 68
---

Anthropic 2026-05 公開的官方 Claude Code 外掛 `security-guidance`：在「檔案編輯時／回合結束時／提交或 push 時」三層偵測漏洞並讓 AI 當場修正，內部基準測試稱可讓 PR 資安留言減少 30-40%。本文深入探討為何 AI 生成程式碼的威脅模型跟傳統不同、三層防線各對應哪種攻擊面、AI 自己抓自己修的迴圈風險，以及與 Snyk/CodeQL/Dependabot/SAST 的分工定位。

## T（核心論點）

代理式程式開發普及後，「生成速度超過人類審查速度」不是人力不足的數量問題，而是結構性失衡——生成端也能同步擴張，加派人手補不上。`security-guidance` 的設計思路是縮短生成與檢查之間的距離，在落差擴大前於「正在寫」的當下就攔截，而非等 CI 階段才回頭看已完成的程式碼。但作者強調這不是「裝了就安全」，AI 自己偵測、自己修正的迴圈本身仍需要環境層的「封鎖設計（containment）」兜底，不能只依賴機率式的模型層檢測。

## B（適用場景）

Claude Code CLI v2.1.144 以上的專案；想在生成當下而非 CI 階段就攔截 AI 生成程式碼的常見漏洞；評估如何把此外掛與既有 SAST/DAST/Snyk/CodeQL/Dependabot 工具鏈搭配而非取代。

## R（重點整理）

**AI 生成程式碼的威脅模型三個新特徵**（區別於傳統人類犯錯模式）：
1. **混入人類不常寫的模式**——AI 依統計傾向產生程式，可能局部不一致但語境合理，「看起來很自然」反而降低審查者警覺
2. **很像樣但錯誤的依賴新增**——AI 可能推薦權限過高的函式庫、已停止維護的舊套件、名稱相似但用途不同的套件，新增依賴比改程式本體更容易擴大攻擊面
3. **Slopsquatting（幻覺套件供應鏈攻擊）**——AI 自信生成不存在的套件名，攻擊者搶先註冊該名稱的真套件，開發者安裝後即引入惡意程式碼；是 typo squatting 的 AI 時代變體

**三層防線架構**：

| 層級 | 觸發時機 | 處理方式 | 成本 |
|------|---------|---------|------|
| 1. 模式比對 | 檔案編輯後（Edit/Write/NotebookEdit） | 決定論式快速偵測危險函式呼叫，不呼叫模型 | 輕量、不耗 token |
| 2. 回合結束審查 | 一回合變更整合完成後 | 背景模型審查 git diff，回饋給 Claude | 中等 |
| 3. 提交審查 | git commit/push 前後 | 讀周邊程式碼與呼叫來源做完整代理式審查 | 較重 |

實務限制：第 2 層每次最多 30 個檔案、連續 3 次；第 3 層每小時約 20 次；第 2、3 層只在 git repository 內運作。**關鍵陷阱**：第 3 層只在 Claude 透過 Bash 工具執行 `git commit`/`git push` 時觸發，若用 `!` shell escape 或使用者手動在終端機提交，不會進入審查範圍。

**三層對應攻擊面**：程式碼本身漏洞（注入/反序列化/DOM 操作/認證繞過）主要由第 1、2 層攔截；依賴攻擊面（過度新增依賴、slopsquatting）需要看整體變更與專案上下文，適合第 2、3 層；設定層攻擊面（權限、秘密資訊、CORS）需跨檔案上下文，主要靠第 3 層。

**安裝**：`/plugin marketplace add anthropics/claude-plugins-official` → `/plugin install security-guidance@claude-plugins-official`；團隊/雲端 session 固定啟用可寫入 `.claude/settings.json` 的 `enabledPlugins`。預設審查模型為 Claude Opus 4.7（可用環境變數換模型，會消耗一般使用額度）。

**易混淆點**：Anthropic 另有獨立 repo `claude-code-security-review`（GitHub Action，PR 開啟時在 CI 上審查），與本文的 session 內外掛 `security-guidance` 不是同一個東西，兩者可以疊加使用（前者補在 PR/CI 階段）。

**為何 SAST/DAST 在 AI 時代不夠**：速度不匹配（CI 定期掃描時，回饋回來時下一波生成又疊上去了）／上下文不足（CI 只看最終輸出程式碼，看不到「為什麼這樣寫」的生成時語境）／誤報漏報結構問題在生成量暴增後更容易導致警報疲勞。`security-guidance` 是補強而非取代，兩種軸線（決定論高再現性掃描 vs 生成附近即時帶上下文回饋）應該疊加。

**與既有工具的理念分工**：Dependabot（沿時間軸比對已知漏洞資料庫）／Snyk（依賴+遞移依賴的已知漏洞系統化管理）／CodeQL（資料流追蹤，語意層分析「危險輸入是否到達危險位置」）／`security-guidance`（正在寫、保有上下文，重速度與即時回饋而非決定論大範圍掃描）。建議組合：本地 session 內用 security-guidance 邊寫邊修 → PR/CI 用 CodeQL/Snyk/Dependabot 做決定論式靜態分析 → Stage 用 DAST/fuzzing → Production 監控日誌/WAF/執行階段防護。

**封鎖設計（containment）三層**（引自 Anthropic《How we contain Claude》）：環境層（沙箱/VM/輸出控制建立決定論邊界，如 macOS Seatbelt、Linux bubblewrap）／模型層（系統提示/分類器/訓練修正調整行為，`security-guidance` 屬此類）／外部內容層（處理 MCP 伺服器與工具輸出等信任邊界外內容）。核心原則：不能只依賴機率式的模型層，要先在環境層設好「到此為止」的決定論邊界，再讓模型在範圍內運作——即使偵測漏掉，損害也不會超出邊界。

## C（關鍵行動）

- [ ] 導入前確認 CLI 版本 ≥ v2.1.144，並在自己的技術棧上用小型 PoC 先試跑確認實際偵測能力
- [ ] 不要誤以為「裝了就一定會攔到最後一次提交」——確認團隊 commit 流程是否會繞過 Bash 工具觸發第 3 層審查
- [ ] `security-guidance` 定位為補強而非取代，仍需搭配 CI 階段的 CodeQL/Snyk/Dependabot 做決定論式掃描
- [ ] 環境層的沙箱/權限控制不能因為裝了此外掛就鬆懈，這是「封鎖設計」的核心原則
- [ ] 團隊固定啟用可寫入 `.claude/settings.json` 而非依賴每人手動安裝

## 相關連結

- [[ai-code-review-safeguards]] — AI 生成程式碼的另一種防護層設計（TypeScript 嚴格模式+ESLint），互補角度
- [[claude-code-official-skills-top10-2026]] — 官方 Skill 生態速查，Security Review 是內建 Skill 與本外掛不同機制的對照
- [[production-agent-architecture]] — Guardrails/HITL 的一般性工程設計，封鎖設計是其在資安領域的具體實作
- [[claude-code-codex-sandbox-h5i]] — Claude Code/Codex 沙盒環境 + h5i：與本外掛互補的另一層防護（外掛攔截危險程式碼生成、沙盒限制執行環境）
