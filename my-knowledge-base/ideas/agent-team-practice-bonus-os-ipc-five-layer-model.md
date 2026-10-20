---
title: "Agent Team 實戰（番外篇）：從 OS 的 IPC 看 Agent 溝通——五層決策模型"
date: 2026-04-20
tags: [ai/agent, type/concept]
source: https://emilwu.tw/articles/30-agent-team-os-ipc/
type: idea
status: processed
asset_value: 30
---

「Agent Team 實戰」系列番外篇（作者 EMil Wu），發表順序介於第七篇與第八篇之間，與第八篇為緊密耦合的「理論／實戰」配對（低於一般 40 分下限但因與第八篇配對關係例外收錄，見 Team Lead 裁決）。

**核心貢獻：** OS IPC 概念與 Agent 架構的系統性映射（Process=Agent、fork+exec=spawn sub-agent、file+flock=filesystem mailbox、context switch=compaction+session summary）。由下而上的五層決策模型，每層限制下一層選項：

```
L4  Content Contract  — 傳什麼（memory selection + compression）
L3  Protocol          — 什麼格式（message envelope、interaction pattern、task lifecycle）
L2  Topology          — 誰跟誰講（hierarchy / star / peer / pub-sub）
L1  Transport         — 怎麼傳（pipe / file / WebSocket / HTTP）
L0  Environment       — Agent 在哪（same process / machine / network / internet）
```

用此模型比較 Claude Teams（File I/O, Star+peer, 薄 protocol）vs OpenClaw（WebSocket, Hub, 中等 protocol）vs A2A（HTTP, Peer, 厚 JSON-RPC+OAuth2.0）三系統在不同 L0 環境下的合理選擇——不是誰更好，是環境決定的取捨。作者用此模型回頭檢視自己的 Agent Team，發現 L0-L2 已無意識做對，但 L3-L4 開始碰壁，懸念留給下一篇（第八篇）具體展開。

## 相關連結

- [[agent-team-practice-07-three-layer-memory]] —— 系列前作，作者在記憶篇末尾請 Em ingest 前同事的 OS IPC 分析文，是本篇五層模型的直接來源
- [[agent-team-practice-08-ipc-limits-specialist-design]] —— 系列下一篇，逐層驗證本篇的五層模型，指出 L3-L4 在異構 Agent 場景下失效，是本篇的實戰配對篇
