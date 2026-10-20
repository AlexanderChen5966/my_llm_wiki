---
title: Claude Code 跨 Session 傳訊：兩個終端機互相交接，邊界收得很緊
date: 2026-08-07
tags: [ai/claude, ai/agent, ai/workflow]
source: https://www.bnext.com.tw（Claude Code v2.1.224，官方帳號 ClaudeDevs 2026-08-07）
type: article
status: processed
asset_value: 70
---

## T（核心論點）

同時開兩個終端機跑 Claude Code 時，左邊改了介面、右邊在動同一專案，過去得自己切過去把事情重講一遍。v2.1.224 的 **cross-session messaging** 把這件差事交給 AI：

> 官方說法：**「你的 session 現在可以互相傳訊息。不必再到另一個 session 重新解釋一遍，你可以叫 Claude 去講。」**

🔑 **但這條線的邊界收得很緊**：傳的是**純文字**、不傳權限、收訊端保有否決權。

## B（適用場景）

**適用：** 同時開多個終端機／多個 worktree 動同一專案；想跟長時間跑的任務要進度；把剛做完的決定交接給另一個 session

**不適用 / 注意：**
- ⚠️ **要把「整個脈絡」搬過去，官方建議改用接續舊對話（resume session），不要用這個功能**
- 🔴 **僅支援 macOS 與 Linux（含 WSL 2），原生 Windows 沒有**；透過 Amazon Bedrock、AWS 上的 Claude Platform、Google Cloud Agent Platform、Microsoft Foundry 使用時同樣沒有
- 版本要求：一般跨 session 傳訊需 **v2.1.224+**；主動聯絡**其他機器**上的 Remote Control session 需 **v2.1.225+**。用 `/list-agents` 確認自己這台拿到了沒

## R（機制與用法）

### 傳過去的是一段話，不是整段對話

訊息就是**一個 Claude 寫給另一個 Claude 的一段純文字**。接收端**只看得到這段文字和寄件端的名字**——看不到對方的對話歷史，也拿不到對方的檔案。

官方範例的訊息長相：
> 「Schema 遷移完成，新欄位是 `tenant_id`，現在 rebase 到 main 是安全的。」

把工程術語拿掉就是「我這邊改完了，你可以動了」。

### 兩個工具都不用你自己呼叫

`ListAgents`（列出聯絡得到哪些 session）與 `SendMessage`（送出訊息）**都由 Claude 自行呼叫，訊息內容也不必你擬**。你只要用日常語言講出想讓誰知道什麼。

### 三種官方場景

| 場景 | 官方／實際範例指令 |
|---|---|
| 跟長時間任務要進度 | 「問一下我另一個終端機的 session，那個 migration 跑完了沒」 |
| 交接剛做完的決定 | 「跟那個在處理 payments API 的 session 說明我們剛剛做了什麼」——🔑 **指令沒有規定訊息要怎麼寫，內容全由 Claude 決定** |
| 多 worktree 互相同步 | 「我剛把登入驗證那段的介面改掉了，通知其他正在動這個專案的 session」 |

### 接收端不會被打斷

收訊的 Claude 在**兩次工具呼叫的空檔**讀訊息，執行到一半的指令不會中斷；對方若正好閒置，才直接開一輪新對話把訊息交出去。**訊息送達後計入用量**，跟你自己打字下指令一樣。

## 🔴 三件事被直接鎖死（安全邊界）

1. **別的 session 傳來的訊息不算你本人的同意**——無法代你按下權限確認
2. **訊息不能要求接收端修改權限設定或 `CLAUDE.md` 這類設定檔**
3. **訊息裡若寫了 `/compact` 這種指令，抵達時只會被當成純文字，不會執行**

> 這三條正是把訊息當成**不可信輸入**來處理——與 [[github-skill-security-audit]]、[[chatgpt-chrome-extension-guide]] 的防 Prompt Injection 原則同一思路，只是這次由官方在協定層直接實作，不必使用者自己下防護提示詞。

**收訊端另有三段控制**：`accept`（照收）／`hold`（先扣住等你批准）／`refuse`（直接丟掉）。沒特別設定時依兩邊 session 的權限模式自動判斷；**企業可在管理設定裡一次關掉整個組織的收發**。

**節流機制（防訊息迴圈）**：同一寄件端的重複訊息會限速、短時間內完全相同的**直接丟棄**、等待讀取的訊息**上限每個 session 50 則**。→ 訊息迴圈會自己停下來。

## 🔑 官方五層分工定位（本功能補上最後一條線）

| 機制 | 處理什麼 |
|---|---|
| **subagents** | 一個 session **內部**的分工 |
| **agent teams** | 被監督的 AI 團隊（各 worker 有獨立 context、可互相通訊） |
| **agent view** | 多 session **集中監看** |
| **Remote Control** | 手機遠端操控 |
| 🆕 **cross-session messaging** | **各自獨立、由你分別駕駛的 session 之間怎麼講話** |

## C（關鍵行動）

- [ ] 多終端機／多 worktree 工作時，改用日常語言叫 Claude 去通知另一個 session，別自己切過去重講
- [ ] 需要搬**整個脈絡**時用 resume session，不要用跨 session 傳訊（它只傳一段純文字）
- [ ] 記住訊息會**計入用量**——不是免費的旁路
- [ ] 高敏感專案可設 `hold`，讓訊息先扣住等你批准再交給 Claude
- [ ] 用 `/list-agents` 確認版本與可聯絡對象；Windows 原生與各雲端平台目前沒有此功能

## 相關連結

- [[subagent-four-boundaries]] — Subagent 的四層邊界；本篇補上「**獨立 session 之間**」這一層，與 subagent（session 內）不同層級
- [[fable-5-delegation-cost-benchmark]] — ⚠️ 對照 Devin Fusion sidekick 的「常駐並行 + 共享脈絡」：本功能刻意**不**共享脈絡（只傳純文字），設計取向相反
- [[claude-code-commands-reference]] — Claude Code 命令速查；`/list-agents` 屬 session 管理類
- [[github-skill-security-audit]] — 防 Prompt Injection 原則；本功能在協定層直接鎖死三件事，是官方版的同一思路
- [[multi-agent-cli-bridge]] — CCB 用 tmux 做多 Agent 工作區；本功能是官方原生的輕量替代（僅傳訊，不做工作區管理）
- [[parallel-code-multi-agent-gui]] — 多 agent 平行開發 GUI，各自 worktree 隔離；本功能補上其缺的 worktree 間同步
- [[claude-code-harness-internals]] — Harness 承擔所有狀態管理；跨 session 傳訊是 harness 層新增的通訊管道
