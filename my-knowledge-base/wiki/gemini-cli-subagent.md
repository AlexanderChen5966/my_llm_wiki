---
title: Gemini CLI 接入 Claude Code：省 Token + 交叉驗證
date: 2026-05-14
tags: [ai/claude, ai/agent, pkm/workflow]
source: https://gyozalab.com/claude-code-gemini-cli-workflow
type: article
status: processed
asset_value: 48
---

透過 CLAUDE.md 規則讓 Claude Code 自動用 Bash 呼叫 Gemini CLI，把高耗 Token 的讀取任務（搜尋、大檔案摘要）轉包給免費的 Gemini，Claude 只負責規劃與判斷。

> ⚠️ **2026-05-19 更新**：Gemini CLI 已整併為 **Antigravity CLI**，指令從 `gemini` 改為 `agy`，遷移截止 2026-06-18。本條目的設定方式和策略仍有效，但所有 `gemini "..."` 呼叫需更新為 `agy "..."`。詳見 [[google-antigravity-platform]]。

## T（核心論點）

「這個任務會讓誰讀最多資料？」——讓 Gemini（現為 Antigravity CLI）讀，Claude 拿結論。在 CLAUDE.md 寫分工規則後，Claude 自動在背景透過 Bash 呼叫 `agy`，不需手動切換視窗；免費方案每天 1,000 次請求。

## B（適用場景）

**適用：** Claude 額度緊繃時；任務需要大量網路搜尋或讀取超長文件（Gemini 有 1M Token 上下文）；需要第二意見交叉驗證
**不適用 / 注意：** Gemini 推理和寫程式能力遜於 Claude，核心邏輯任務仍交 Claude；同時執行不超過 2 個 Gemini（分鐘上限 60 次請求）

## R（操作步驟）

1. 安裝：`npm install -g @google/gemini-cli`，執行 `gemini` 完成 Google OAuth 登入（唯一一次需開終端機）
2. 安全設定：在 `~/.gemini/settings.json` 加 `excludeTools`（封掉寫入/刪除工具，詳見下方範本）
3. 將這篇文章丟給 Claude，請它協助討論分工規則並寫入 CLAUDE.md（因人而異，無固定模板）
4. 執行時：Claude 只傳**檔案路徑**給 Gemini，絕勿先讀再轉（等於雙倍 Token 消耗）

## C（關鍵行動）

- [ ] `excludeTools` 必設，否則 Gemini 可能寫入或刪除檔案（有實測案例）
- [ ] Gemini 在 `-p` 背景模式下無工具時不報錯，直接幻覺捏造——安全邊界先設
- [ ] 交叉驗證 ≠ 讓 Claude 重跑 Gemini 的工作，那不省 Token；兩邊各做一次，人工判斷誰對

## 安全設定範本

`~/.gemini/settings.json`（Windows：`%USERPROFILE%\.gemini\settings.json`）

```json
{
  "tools": {
    "excludeTools": ["run_shell_command", "write_file", "replace"]
  }
}
```

加完後：搜尋和讀檔照常運作，Gemini 無法寫入、修改或刪除任何檔案。

## 三種分工場景

| 場景 | 分工 | 省什麼 |
|------|------|--------|
| 大量網路搜尋 | Gemini 搜尋 + 整理，Claude 拿結論 | 輸入 Token |
| 超長文件摘要 | Gemini 直接讀檔（傳路徑），Claude 使用摘要 | 輸入 Token |
| 第二意見 | 兩邊各做，人工判斷誰對 | 決策品質（不省 Token）|

## 相關連結

- [[dual-tool-strategy]] — Gemini CLI / Claude Code 分工策略（何時用哪個工具，成本規劃視角）
- [[ai-pair-skill]] — 多模型協作進階玩法（Claude + GPT + Gemini 三模型審查）

## 補充資料（來源：Claude 額度燒光光，教你怎麼把 Gemini 也叫進來上班  煎餃的調味實驗室.md）

### 實測心路歷程：Claude 派 Gemini 當小弟

閃電煎餃的實測分享，補充幾個原文未提到的細節：

**為什麼需要這個設定**：Claude Max（\$100-200/月）的額度也容易燒光——AI 太好用反而導致用量持續增加，節省 Token 的需求是真實的。

**Gemini 能做什麼、不能做什麼**：
- ✅ Google 官方搜尋（`google_web_search`），搜尋本來就是 Google 的主場
- ✅ 大量網頁/文件摘要（Gemini 1M Token 上下文，讀超長文件不流汗）
- ❌ 推理和寫程式——Gemini 比不上 Claude，核心邏輯仍交 Claude
- ⚠️ 在 `-p` 背景模式沒工具時：**不報錯，直接幻覺捏造**（實測：叫它摘要一個網址，它自信地生成了一篇根本不存在的假文章）

**解決幻覺的關鍵**：用 `excludeTools` 封掉危險工具後，Gemini 就只能做搜尋和唯讀操作，沒有工具時的行為變得可預測。

**同時跑三家模型的進階玩法**：
作者後來把 Codex 也接進來，變成「Claude 大腦 + Gemini 搜尋 + Codex 交叉驗證」的三引擎架構。截圖顯示 Claude 同時派 Gemini 和 Codex 平行讀取同一篇文章做交叉驗證（`Ran 2 commands`）。

> 「三家一起吵比我單押一家安心多了。畢竟 AI 幻覺無法避免，他們先吵一輪，我也比較放心。」

**分鐘上限提醒**：同時跑超過 2 個 Gemini 就容易撞每分鐘 60 次的限制（一個查詢背後可能觸發搜尋 + 讀網頁 + 整合 = 多次請求）。

**設定步驟再確認**：
1. `npm install -g @google/gemini-cli`
2. 執行 `gemini` 完成 Google OAuth（唯一一次需開終端機）
3. 設定 `~/.gemini/settings.json` 加 `excludeTools`（run_shell_command, write_file, replace）
4. 把這篇文章丟給 Claude 討論分工規則，讓它自己寫入 CLAUDE.md
