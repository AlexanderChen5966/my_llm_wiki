---
title: Agent Skill 實戰攻略：從入門到進階
date: 2026-06-01
tags: [ai/claude, ai/workflow]
source: https://blog.ray-realms.com/agent-skill-shi-zhan-gong-lue-cong-ru-men-dao-jin-jie-ti-sheng-ni-de-ai-sheng-chan-li/
type: note
status: processed
asset_value: 90
---

Agent Skill 最核心的改變：透過「Description（說明）」讓 AI 按情境自動載入對應 SOP，而非每次手動複製貼上提示詞。Skill 還具備自我進化能力——AI 可根據執行經驗自動維護與更新 Skill 內容，實現知識傳承的複利效果。

## T（核心論點）

**Skill = 帶說明目錄的提示詞**。傳統提示詞的問題：每次需要手動找到並貼上，沒有自動化，沒有學習能力。Skill 解決了兩件事：

1. **徹底全自動化**：AI 永久記錄 Skill，任務來臨時自動比對目錄、調用對應 SOP，無需人工介入
2. **自動維護與自我進化**：AI 可根據執行過程中遇到的問題、流程變化，自行修改 Skill 內容

進化曲線：第一次探索（2 小時）→ 第二次執行（30 分鐘）→ 第三次（10 分鐘）→ 越做越順

**類比**：百科全書的目錄——你不用翻 10,000 頁，AI 看目錄就知道要載入哪個技能。

## B（適用場景）

**適用：**
- 有固定重複任務（報帳、對帳、寫報告格式）需要 AI 每次遵照相同 SOP
- 想建立可自我進化的 AI 助手，而非一直從零 prompt
- 主要工具為 Claude（web/app）；Codex 也支援

**不適用 / 注意：**
- ChatGPT 網頁本身不支援 Skill（但 Codex CLI 可以）
- Skill 名稱不能有大寫（Claude 規定），安裝第三方 Skill 時注意修復

## R（操作步驟）

**Claude 啟用 Skill：**
1. 設定 → Capabilities → 開啟「Code execution and file creation」
2. 進入 Skill 頁面
3. 可直接說「幫我創建一個 Skill 做 XX 任務」，或安裝第三方 Skill

**安裝第三方 Skill（以 skillsmp.com 為例）：**
1. 從 skillsmp.com 下載 Skill zip 檔
2. 進入 Claude Skill 頁面，拖入 ZIP 檔安裝
3. 若 Skill 名稱有大寫報錯：解壓縮 → 打開 Skill.md → 把 name 改成全小寫 → 重新壓縮 → 重新上傳
4. 安裝後，對話中直接觸發，AI 自動識別並調用

**自建 Skill 的核心檔案：**
- `Skill.md`：最重要的檔案，記錄觸發條件（Description）、核心提示詞、其他檔案說明

## C（關鍵行動）

- [ ] 盤點自己最常重複做的 3 件事，每件事寫一個 Skill
- [ ] 從 skillsmp.com 找現成 Skill，安裝 1-2 個試用
- [ ] 讓 AI 自動維護 Skill，每次執行後問 AI「這個 Skill 有沒有需要更新的地方？」
- [ ] 設計 Skill 的 Description 要精準——說明「在什麼情境下才需要載入這個 Skill」

## 原始摘要

深度介紹 Agent Skill 的核心概念與實戰做法，說明 Skill 如何透過 Description 讓 AI 按情境自動載入對應 SOP，從而實現真正的全自動化工作流。文章從入門到進階，涵蓋 Skill 建立、維護與迭代的完整方法。

## 相關連結

- [[claude-skills-guide]] — Claude Skills 完整指南（能力包型 + 軟編排型）
- [[claude-skills-asset-building]] — Skills 從指令到資產：MAPS 框架
- [[claude-skills-deep-dive]] — Progressive Disclosure 機制深度解析
- [[agent-skills-toolkit]] — 22 個 Skill 完整開發生命週期（DEFINE→SHIP）
- [[mattpocock-claude-skills-real-engineers]] — 針對 AI 四大失敗模式的工程解法
