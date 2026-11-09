---
title: Cloudflare security-audit skill：六階段開源資安稽核，驗證者必須不同於發現者、沒有沙盒就不執行
date: 2026-11-04
tags: [ai/agent, ai/workflow, type/how-to]
source: https://www.aiposthub.com/cloudflare-security-audit-skill-open-source-agent-vulnerability-audit/
type: article
status: processed
asset_value: 36
---

> 🔴 **來源性質**：aiposthub（Philo，2026-10-20）二手整理官方倉庫與 Cloudflare 部落格，**作者未實測**。
> ⏳ **時間線更正**：GitHub 倉庫建立於 **2026-06-18**，與部落格《Build your own vulnerability harness》同日——**不是新品**，是 2026-10-19 X 貼文（RoundtableSpace，查詢時約 2,400 讚、22 萬瀏覽，社群熱度）把舊專案翻出來。
> 🔴 **Cloudflare 內部數字不是開源版成績**：下文標「內部」的數字屬 Cloudflare 內部完整系統，**不可當開源 skill 的預期成果**。

## T（核心論點）

這個 skill 值得保存的不是工具本身，而是**流程設計**：把漏洞獵捕拆成多階段，並規定 **「驗證者必須不同於發現者，且任務是推翻候選」**——因為找漏洞的代理很容易對自己的結論過度自信（部落格另提代理會改原始碼讓自己的證明成立）。再加兩條硬規則：**沒有 OS 級沙盒就不執行目標程式碼，疑點只標待驗證**；**只確認真正越過信任邊界的缺陷**。這套「發現／驗證分離＋證據門檻」可遷移到其他 agent 審查場景。

## B（適用與不適用）

**適用：**
- 小團隊，已在用程式代理（模型要能呼叫工具、分派平行子代理）、有隔離環境、有人力複查 confirmed 與 needs_validation
- 先在自己擁有、規模不大、沒有正式環境資料的專案試跑

**不適用 / 注意：**
- 沒有隔離環境：先補沙盒，比安裝 skill 重要
- 想在每次合併請求跑完整稽核：內部大型掃描以小時計，開源版 quick／局部耗時須用自己的專案驗證
- 想取代滲透測試或法遵稽核：官方不宣稱涵蓋率，也**不提供漏報率**；乾淨的報告只代表這一輪覆蓋有限，不是安全證明
- 掃第三方或客戶程式碼：須取得書面同意；發現漏洞依對方資安回報規定私下通報（README 的信箱用途是交流，不是通報窗口）
- 資料去向：程式碼會送往代理背後的模型服務，README 未說明，須自查供應商資料條款

## R（流程與數據）

### 六階段（開源版 README）

| 階段 | 做什麼 | 主要產出 |
|---|---|---|
| 1 偵察 | 整理架構、信任邊界、輸入入口與覆蓋清單 | architecture.md、coverage-ledger.json |
| 2 分區獵捕 | 依清單分給獨立獵手，覆蓋審查員找缺口 | 各獵手檢查紀錄 |
| 3 候選驗證 | 每個候選交給**全新**驗證員，任務是推翻它 | 驗證結論 |
| 4 結構化輸出 | 寫成記錄並用 JSON 格式規範檢查 | findings.json |
| 5 獨立記錄核對 | 新代理重新核對最終記錄中的原始碼說法 | 修正後的記錄 |
| 6 報告 | 從已驗證記錄產生報告 | REPORT.md、FINDINGS-DETAIL.md、NEEDS-VALIDATION.md |

- **三種結論**：confirmed（完整原始碼追蹤＋可觀察結果）、needs_validation（還差明確事實，不給嚴重度）、rejected。第一層防護擋住攻擊卻缺第二層，只算加固建議，不進 confirmed
- **覆蓋清單**讓重跑只補沒查過的地方；另附十份依目標類型選用的延伸文件（記憶體安全、提示注入與代理工具、HTTP 與驗證、前端、供應鏈、雲端部署、訊息與遠端呼叫、資源耗盡、資料隔離、桌面與行動）
- **兩種模式**：一般資安問題用指引模式（不建輸出資料夾、不跑六階段）；明確要求稽核整個程式庫或報告檔才進完整模式。完整模式預設寫到家目錄下的 security-audit-skill 資料夾，除非明確選被版本控制忽略的資料夾，避免稽核檔被誤提交
- **規格** quick／standard（預設）／deep；quick 與限定範圍稽核須標示部分覆蓋；可設代理呼叫總次數上限，上限連偵察與驗證都付不起就一個代理都不啟動

### 沙盒前提（SKILL.md）

- 建置與測試必須在 OS 級沙盒：關閉對外網路、環境變數白名單、目標與工具鏈唯讀、只寫指定暫存目錄、CPU／記憶體／行程數／檔案大小／磁碟／時間皆設上限
- 禁止探測已部署服務、共用基礎設施或正式環境身分，不花付費 API 額度，不做超出最小必要的操作
- **任一控制做不到＝不執行目標程式碼**，線索留在 needs_validation 並附安全驗證計畫
- Docker 內執行：部落格稱沙盒需把 seccomp／apparmor 設為 unconfined 否則無聲失敗；**官方沒討論放寬後的風險**，作者建議用專用稽核機器

### 數字：哪些屬開源版、哪些屬內部

- 開源版可說的：單次執行只找到多次總量**約一半**（Cloudflare 自己的測試）；重跑會參考前一輪記錄只補沒查過的；開源版六階段 vs 部落格原版七步（最後一步送內部收件介面）；2026-09-10 的提交把流程、輸出格式與驗證腳本整個重寫，官方未逐項說明差異
- **內部系統數字（不是開源版成績）**：原約 450 行 skill、約六週擴成可掃 128 個程式庫；20,799 個原始候選、約 12,057 個通過驗證、7,245 個發現；約 3 萬行程式庫跑一次需 3–4 小時、初步發現約 100 個；每輪補洞成本約為第一輪獵捕的一半。部落格自己也說這些數字會過時
- 內部系統另有「正式環境是否可達」判斷階段與自動修補（最年輕最慢、每個修補分支都要人審）；開源 README 無對應步驟，**可達性要自己補**

## C（關鍵行動）

- [ ] 安裝：`npx skills add https://github.com/cloudflare/security-audit-skill --skill security-audit`（需 Node.js；加 `--global` 裝使用者層級）
- [ ] 第一次試跑：確認沙盒可用 → quick ＋嚴格次數上限 → 先讀 NEEDS-VALIDATION.md → 自己重現幾個 confirmed 項目，重現得了才算有用
- [ ] 導入前確認五件事：稽核範圍（有權限）、資料去向、沙盒與預算、複查人力、重跑計畫（排好第二、三輪）
- [ ] 借用設計原則到自己的 agent 審查流程：發現者不驗證自己、驗證任務是推翻、證據不足只標待驗證
- [ ] 回官方倉庫核對最新 README（本條為 2026-10 快照，未連網核對）

## 原始摘要

Cloudflare 於 2026-06-18 開源 security-audit skill（MIT），是其內部漏洞獵捕系統的起點；六階段流程由多代理分工，驗證員須不同於發現者並嘗試推翻候選；需能分派子代理的模型、Node.js 與 OS 級沙盒，無沙盒則不執行目標程式碼；單次執行只找到多次總量約一半；部落格的內部數字不可當開源版成績。

## 相關連結

- [[agent-permission-and-os-sandbox]] — 「Prompt 不是安全邊界、要 OS 強制」的實例：本 skill 把沙盒列為前提，缺沙盒就不執行
- [[claude-security-continuous-scanning-fix-tracking]] — Enterprise 官方產品（持續掃描、修補追蹤），部署路徑與資格完全不同，條件不可互套
- [[claude-code-security-guidance-plugin]] — AI 生成程式碼的即時三層偵測，與本條「對既有程式庫系統性稽核」不同層
- [[github-skill-security-audit]] — 安裝第三方 skill 前先讀 SKILL.md；本 skill 也能執行指令
- [[anthropic-fold-subagents-back-to-main-agent]] — 「獨立驗證」才保留子代理的觀點可對照
- [[agent-skills-toolkit]] — Skills 工具導覽
- [[ideas/anthropic-cyber-mission-cidp-oss-scanner]] — 同屬 AI 資安掃描主題
