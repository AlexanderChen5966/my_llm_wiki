---
title: Claude Commerce Agents：Anthropic 開源電商 AI Agent 藍圖
date: 2026-09-03
tags: [ai/claude, ai/agent, type/concept]
source: https://www.aiposthub.com/claude-commerce-agents-shopping-merchant-ai/, https://codelove.tw/@tony/post/apoBm3
type: article
status: processed
asset_value: 68
---

## T（核心論點）

Anthropic 於 2026 年 9 月 2 日以 Apache 2.0 開源 `anthropics/commerce-agents`，提供 Shopping Agent（消費者導向）與 Merchant Agent（商家導向）兩種角色，並附零售/旅遊/電信/娛樂四種產業示範。這次開源真正有價值的地方不是「AI 會推薦商品」，而是**把容易被忽略的產品安全問題直接做進程式架構**：Shopping Agent 無法直接扣款、Merchant Agent 修改需人工批准、系統只接受伺服器提供過的合法 ID 以防止模型捏造或被第三方注入指令操縱。**Prompt 可以提醒模型該怎麼做，但涉及金錢或不可逆操作時，最終權限必須掌握在程式規則、使用者身分與批准流程手上**——這是一份參考架構與可執行規格書，不是能直接上線的完整產品。

## B（適用場景）

**適用：** 團隊正在設計電商/客服類 AI Agent 的安全邊界；評估要自建 Shopping/Merchant Agent 前想先看業界參考架構；決定要用 Messages API、Claude Agent SDK 還是 Managed Agents 執行 Agent 迴圈；在 Windows 環境嘗試執行官方 Demo 前想先知道會踩什麼坑。

**不適用 / 注意：** 官方明說這是參考實作，不是 Anthropic 維護的正式產品，無 SLA、不接受外部貢獻；示範環境無正式身分驗證，MCP server 僅本機綁定；付款/物流/庫存實際處理完全不在範圍內；官方「購物車增加 35%/購買提高 60%」數據用 *up to* 字樣，非平均值且母數未公開，不應直接套用到自家情境；沒有工程資源的小商家不適合直接採用，應先用既有電商平台內建的 AI 功能。

## R（操作步驟）

**兩種 Agent 角色：**

| Agent 類型 | 服務對象 | 主要工作 |
|---|---|---|
| Shopping Agent | 消費者 | 目錄搜尋、商品比較、加入購物車、訂單追蹤／退貨 Q&A |
| Merchant Agent | 店家人員 | 銷售分析、庫存警示、價格／促銷提案、活動草案 |

**安全設計三重防線：**
1. Shopping Agent 結帳工具只顯示購物車、不接觸付款網址與憑證，無法直接扣款
2. Merchant Agent 的調價/補貨/促銷一律先建立「待批准變更」，即使對話中聲稱「已批准」也無效，必須由人在真正的批准介面確認
3. 系統只接受伺服器先前提供過的商品/變更 ID，防止模型憑空捏造 ID 或第三方評論注入惡意指令；數量、折扣、調價幅度與活動預算也可設硬性上限

**架構建議：單一 Agent + Skills，而非多 Subagent。** 一次購物對話常同時需要商品、購物車、訂單與偏好資料，頻繁交給不同 Subagent 容易遺失上下文、增加 Token 成本與延遲。因此藍圖以單一 Agent 保留完整對話，用 Skills（可重複使用的任務說明書）載入不同工作流程，只有研究範圍明確、可獨立完成的任務才交給 Subagent。

**三種執行方式：**

| 執行形式 | 差異 |
|---|---|
| Messages API | 代理迴圈自己撰寫，作為參考實作 |
| Claude Agent SDK | 迴圈交給 SDK 處理，較完整框架 |
| Claude Managed Agents | 在主機端執行，呼叫自己的 MCP 伺服器 |

**MVP 建議路徑：** 先讓 Shopping Agent 只做搜尋比價與客服問答、Merchant Agent 只做唯讀分析；確認來源正確、權限隔離無誤且評測穩定後，再加入購物車與需要人工批准的變更流程。

**規模：** 640 檔案（Python 213、TypeScript 202、Markdown 62），requirements.txt 以 editable 方式安裝 7 個未註冊 PyPI 的套件。

**Windows 執行注意事項（實測踩坑，來自 codelove.tw 補充來源）：**
README 前提條件僅列 Python 3.11+ / Node 22 / API 金鑰，完全沒提到 Windows/WSL，實測遇到三個坑：
1. **使用者名稱含日文字元時找不到套件**：`ModuleNotFoundError: No module named 'commerce_common'`——editable install 產生的 `.pth` 檔以 UTF-8 寫入，但 Python 讀取時用系統 locale（日文 Windows 為 `cp932`），路徑因此亂碼解析失敗；改成 ASCII 路徑（如 `C:\tmp\ca`）即可解決
2. **即使改用 ASCII 路徑，店面應用程式仍啟動不了**：`OSError: [WinError 193]`——`run_demo.py` 直接呼叫 `next` 而非 Windows 所需的 `next.cmd`，導致店面端（:3000）無法啟動，但 API（:8000）能正常啟動
3. **清理流程二次當機**：`AttributeError: module 'os' has no attribute 'killpg'`——`os.killpg` 為 Unix 專用；且錯誤訊息含 em dash 在 `cp932` 下也會 `UnicodeEncodeError`，需另加 `PYTHONIOENCODING=utf-8` 才能看到真正錯因

**建議：** 若只是想讀程式碼，`git clone` 就足夠；若要實際執行，建議用 WSL2 或 Linux 環境。

## C（關鍵行動）

- [ ] 設計任何涉及金錢或不可逆操作的 Agent 時，比照本藍圖把安全規則做進程式（ID 白名單、硬性上限、待批准暫存狀態），不要只靠 Prompt 提醒
- [ ] 面對多任務對話型 Agent，先評估「單一 Agent + Skills」是否足夠，不要一開始就拆成多個 Subagent
- [ ] 若要在 Windows 上執行官方 Demo，優先改用 WSL2/Linux，或至少確認路徑無日文字元、對 `next.cmd` 呼叫方式做修正
- [ ] 導入前先做唯讀 MVP（只做搜尋/比價/客服/唯讀分析），驗證權限隔離與來源正確性後才加入購物車與批准流程
- [ ] 官方「購物車增加 35%/購買提高 60%」數據不可直接引用，應以自己的轉換率、任務完成率、客單價、錯誤率做 A/B test

## 原始摘要

Anthropic 於 2026-09-02 以 Apache 2.0 開源電商 AI Agent 參考藍圖，含 Shopping Agent 與 Merchant Agent 兩種角色，四種產業示範，支援 Messages API / Agent SDK / Managed Agents 三種執行方式。核心價值在於把「不能扣款」「修改需批准」「ID 防捏造/防注入」等安全規則做進程式架構，而非僅靠 Prompt。官方明確聲明這是參考實作，非正式產品，無 SLA、不接受貢獻。實測於 Windows 環境執行 Demo 會遇到日文路徑編碼、`next.cmd` 呼叫與 `os.killpg` 三個技術性踩坑，建議改用 WSL2 或 Linux。

## 相關連結

- [[agent-skills-vs-mcp]] — Skills 與 Subagent／MCP 的分工邊界，可與本篇「單一 Agent + Skills」架構建議互相對照
- [[ai-agent-architecture-patterns]] — Agent 架構模式總覽，本篇的單一 Agent + Skills 是其中一種實務選擇
- [[enterprise-ai-permission-three-layers-governance]] — 企業 AI 權限治理框架，可與本篇「待批准變更」「ID 白名單」等安全設計對照
