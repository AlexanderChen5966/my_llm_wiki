---
title: Anthropic Cyber Mission（2026-10）：關鍵基礎設施 CIDP 與開源 OSS Scanner——找漏洞只是起點
date: 2026-10-19
tags: [ai/claude]
source: https://www.aiposthub.com/anthropic-cyber-mission-critical-infrastructure-defense/（引 https://www.anthropic.com/news/anthropic-cyber-mission）
type: idea
status: processed
asset_value: 26
---

> 🔴 **來源**：Anthropic 官方公告（2026-10-08）是一手，但本庫**只有 aiposthub／Philo（2026-10-09）的轉述**；⚠️ 官方連結未經本次核對（2026-10-19）。作者明言**未測試工業設備**；合作名單不代表部署完成。
> 為何放 ideas：OT／關鍵基礎設施資安不在本庫關注方向，個人可操作的只有 OSS Scanner（opt-in）一項。

## T（核心論點）

Cyber Mission 把 AI 資安工作擴展到兩個方向：**關鍵基礎設施**（CIDP 計畫，把前沿模型、現場工程師與威脅研究交給既有服務商）與**開源軟體**（OSS Scanner）。官方自己承認的關鍵是：**找到漏洞之後，驗證、排序與安全修補才是瓶頸**——Project Glasswing 找到許多問題，但整體風險尚未足夠降低；OT 設備使用數十年、難以停機，部分修補甚至可能要等很久。

## B（背景）

**適用：** 追蹤 Anthropic 資安能力的存取管道演變（Glasswing → CIDP／Cyber Verification Program）；理解「AI 找問題 ≠ 風險降低」。

**不適用 / 注意：**
- CIDP **不是一般使用者可直接取得的免費防禦產品**，對象是安全供應商、系統整合商、設備製造商
- 「兩年後 AI 可能更有利於防禦方」是官方**預測**，官方也說短期未必

## R（關鍵細節）

### 官方事實（公告，經轉述）

- 2026-10-08 發表 Cyber Mission 與**關鍵基礎設施防禦計畫 CIDP**
- CIDP：提供前沿 Claude 模型、現場工程師與威脅研究給原本就在保護這些設備的服務商
- **11 家創始夥伴**：Accenture、Booz Allen、CrowdStrike、Deloitte、Dragos、Hitachi、Insane Cyber、Nozomi Networks、Palo Alto Networks、PwC、Rockwell Automation
- 部分夥伴已開始與 Claude 處理漏洞，但**未逐一公開範圍與成果**
- **OSS Scanner**：免費、需主動加入（opt-in）；提早寄送**未經人工審查**的模型安全報告，適合有能力自行分類的專案；其他專案仍保留人工驗證揭露
- 官方承認：Project Glasswing 找到許多問題，但**整體風險尚未足夠降低**
- OT（operational technology）設備常設計使用數十年，修改風險高、不一定能為修補立刻停機；極少數情況修補可能需等數十年（描述困難程度，非統一等待時間）
- 開放安全供應商、系統整合商、設備製造商登記合作意願；安全團隊可申請**擴展後的 Cyber Verification Program**
- 官方預測：兩年後 AI 可能更有利於防禦方，但短期未必（預測）

### 作者觀點（aiposthub／Philo）

- 發現、驗證、修復要分開記錄：發現 100 個候選 ≠ 修復 100 個問題 ≠ 100 個現場已更新
- 模型輸出要有接手角色（安全／現場工程師、設備供應商、營運負責人），否則只是把判斷負擔轉給已很忙的團隊
- 成效看「從發現到修復的時間」與資源是否到達需要的團隊，而非合作名單

## C（連結與應用）

- [ ] 本庫若有開源專案，評估是否 opt-in OSS Scanner（前提：有能力自行分類未經人工審查的報告）
- [ ] 追蹤：[[claude-opus-4-8-release-notes]]「Mythos 級數週內向所有客戶開放」與本條分層存取的關係（並列待查）

## 相關連結

- [[claude-opus-4-8-release-notes]] — Glasswing／Mythos 級模型開放時程（⚠️ 已加 2026-10-19 並列附註）
- [[claude-opus-5-5-release-notes]] — Mythos 5.1 只開放給審核通過的組織；資安題轉送 Opus 4.8
- [[ai-agent-incident-governance-openai-anthropic]] — Mythos 5 資安測試事故（背景）
- [[ai-era-quality-assurance-test-oracle]] — 「找到 ≠ 驗證過」：沒有驗證機制的輸出只是候選
