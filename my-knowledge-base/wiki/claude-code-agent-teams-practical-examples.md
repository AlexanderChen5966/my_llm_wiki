---
title: "Claude Code Agent Teams 教學：4 個實戰範例打造平行 AI 團隊協作"
date: 2026-04-20
tags: [ai/claude, ai/agent, type/how-to]
source: https://moksaweb.com/claude-code-agent-teams/
type: article
status: processed
asset_value: 60
---

官方 Claude Code Agent Teams 功能的操作教學文，涵蓋四元件核心概念、啟用設定、三種品質把關 hook 完整程式碼，以及四個可直接複製的實戰 prompt 腳本。與既有 [[claude-code-agent-teams-when-to-multiply]]（判斷「何時該用」）互補，本條目聚焦「怎麼用」。

## T（核心論點）

Claude Code Agent Teams 由 Team Lead／Teammates／Task List／Mailbox 四元件組成，隊友間可直接互相發訊息、質疑彼此發現、自主認領任務，這是與 Subagent（只能向主代理回報摘要）的根本差異。實戰價值集中在四種模式：多角度審查、競爭假說除錯、跨層開發、複用子代理定義；並需搭配 TeammateIdle/TaskCompleted/TaskCreated 三種 hook 做品質把關，否則隊友可能提早進入 idle 或標記未完成的任務為完成。

## B（適用場景）

**適用：**
- 需要平行探索且隊友間需要溝通的任務：多角度 PR 審查、競爭假說除錯、跨層（前後端/測試）功能開發
- 已有 `.claude/agents/*.md` 子代理定義，想同時複用在 subagent 與 team teammate 兩種場景
- CI 中需要自動化多角度 review 並將結果回貼 PR

**不適用 / 注意：**
- token 成本約一般 session 的 7 倍（plan mode 下），小任務或線性依賴任務不划算
- 不支援 session 恢復，重啟後 in-process 隊友消失
- 兩隊友同時編輯同一檔案會互相覆蓋，需靠檔案分工避免（backend/frontend/test 各自負責不同目錄）
- teammate 從 project/user settings 載入 skills/mcpServers，子代理定義裡的 skills/mcpServers 欄位不會套用到 teammate

## R（操作步驟）

1. **啟用**：`settings.json` 加入 `"env": {"CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"}`，或啟動前 `export` 同名環境變數；需要 Claude Code v2.1.32+；`displayMode` 可設 `auto`/`in-process`/`tmux`。
2. **多角度 PR 審查**：建立團隊時分別指派 security-reviewer／performance-reviewer／test-reviewer 三個隊友各自獨立審查後彙整成依嚴重度分類的報告。
3. **競爭假說除錯**：針對根因不明的 bug，spawn 4 個各持不同假說的 investigator，要求彼此互相蒐證反駁、更新信心等級，15 分鐘後綜合存活證據判斷最可能根因——避免單一調查者的錨定偏差。
4. **跨層功能開發**：backend-dev／frontend-dev／test-dev 各自負責不同目錄（如 `src/api/`、`src/components/`、`tests/`）避免檔案衝突，test-dev 等待前兩者完成初版才開始（由任務依賴系統處理），並可要求 backend-dev/frontend-dev 先送 plan 給人審核再動手。
5. **複用子代理定義作為 teammate**：`.claude/agents/*.md` 定義的 name/description/tools/model 可直接被 `Spawn a teammate using the <name> agent type` 引用，同一份定義同時服務 subagent 與 team 模式，但 skills/mcpServers 不會套用，teammate 改從 project/user settings 載入。
6. **品質把關 hook**：`TeammateIdle` 檢查是否還有 pending 任務或 deliverables，`exit 2` 強制繼續工作；`TaskCompleted` 檢查有無檔案變更/語法錯誤/lint 錯誤，`exit 2` 拒絕標記完成；`TaskCreated` 在任務建立時做驗證。
7. **CI 自動化**：`claude --bare -p "<prompt>" --allowedTools "Read,Grep,Glob,Agent" --max-budget-usd 3.00 --max-turns 30 --output-format json` 搭配 `gh pr comment` 把彙整報告回貼 PR，建議 timeout-minutes: 15。

## C（關鍵行動）

- [ ] 若要導入 CI 自動化 PR review，先在本機用單一範例跑通 hook script 再接上 GitHub Actions
- [ ] 為每個隊友明確劃分檔案/目錄負責範圍，避免同檔案覆蓋
- [ ] 導入 TaskCompleted hook 做語法/lint 檢查，避免隊友提早標記完成

## 原始摘要

官方 Claude Code Agent Teams 功能的操作教學文，涵蓋核心概念（Team Lead/Teammates/Task List/Mailbox 四元件）、啟用設定（環境變數、settings.json、CLI flag、display mode）、品質把關 hooks（TeammateIdle/TaskCompleted/TaskCreated，附完整 shell script）、以及四個具體實戰範例的完整 prompt 腳本：多角度 PR 審查、競爭假說除錯、跨層功能開發、使用子代理定義的團隊。另附限制清單（token成本約7倍、不支援session恢復、每session僅一團隊等）與 CI 自動化腳本範例。

## 相關連結

- [[claude-code-agent-teams-when-to-multiply]] —— 官方 Agent Teams「何時該用」判斷框架（本條目談「怎麼用」，兩者互補）
