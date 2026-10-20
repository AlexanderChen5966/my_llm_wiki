---
title: "Agent Team 實戰（五）：溝通——Dispatch 的兩條路和一堆坑"
date: 2026-04-14
tags: [ai/agent, type/how-to]
source: https://emilwu.tw/articles/27-agent-team-dispatch/
type: idea
status: processed
asset_value: 48
---

「Agent Team 實戰」系列第五篇（作者 EMil Wu）。

**設計歧異：** Anthropic Agent Teams 是「team of workers」設計（同一 codebase 多 worker 共享 cwd/CLAUDE.md/Skills，僅指令不同），與作者需求的「team of specialists」（每個 Agent 各自獨立身份）根本不同。GM spawn Em 時，Em 因繼承 GM 目錄而載入了 GM 的身份，4/5 檢查項 FAIL。

**Option B 解法與坑：** 改用 CMUX split-pane 讓 Agent 從自己目錄啟動解決身份問題，但衍生「GM 常忘記 cd」及 hook 新舊格式差一層 wrapper 導致 silent failure（多花 1-2 小時 debug，根因修復僅需 30 秒）兩個坑。

**三種混合 dispatch 模式：** Mode A（Agent Teams 即時協調，中間產出需留在 GM Context）、Mode B-Chain（CMUX+Hook 自動接力，實測 Em→C7 僅 6 秒，下一步固定不需 GM 判斷）、Mode C-Notify（CMUX+Hook 通知，下一步需 GM 判斷）。判斷依據：中間產出需不需要留在 GM Context？下一步是否固定？

被第八篇明確回引：身份載入問題呼應第五篇的 Hybrid Dispatch 設計。

## 相關連結

- [[agent-team-practice-04-identity-lifecycle-refactor]] —— 系列前作，本篇 Em 身份混淆事故是第四篇「身份過期」論點的具體延伸案例
- [[agent-team-practice-07-three-layer-memory]] —— 系列下一篇，開頭延續本篇的溝通/dispatch 討論，記憶問題是協調完成後的下一層問題
- [[agent-team-practice-08-ipc-limits-specialist-design]] —— 明確回引本篇的 Hybrid Dispatch 設計，用 OS IPC 五層模型驗證「身份載入」為何是 OS 沒有的維度
