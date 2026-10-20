---
title: Grok Bot 七周開發：Cursor 團隊的 5 個反直覺「同事腦」決策
date: 2026-09-11
tags: [ai/agent, ai/workflow]
source: https://www.bnext.com.tw/article/92202/grok-bot-seven-weeks-five-product-decisions
type: note
status: processed
asset_value: 48
---

Cursor（併入 SpaceXAI 後）產品負責人 Roman Ugarte 還原 Grok Bot 從零到公開僅七周的開發歷程，提出 5 個反直覺決策：不併入 Cursor 既有產品、堅持物理隔離的獨立小團隊、每個 bot 配備自己的雲端電腦（而非共用本機）、核心成員親自 onboard 兩三百名早期用戶、上線前大幅砍除開發者導向的可視化細節（不逐項顯示工具呼叫）。核心可轉用判準是「colleague-pilled（同事腦）」心法：每個爭議決策都用「如果這是人類同事會怎麼做」裁決，例如同事不會共用筆電（故 bot 各自一台雲端電腦）、不會逐秒回報按鍵（故介面只顯示進度不攤開思考鏈）。若要深入查原文的完整訪談（Lenny's Podcast）。

## 相關連結

- [[grok-bot-cloud-agent-product-overview]] — Grok Bot 產品總覽（雲端電腦架構、Outer/Inner Loop、治理機制等），本篇是其開發歷程與產品決策的窄角度案例延伸
- [[grok-bot-persistent-agent-interface-design]] — 官方設計文拆解；本篇決策五「不逐項顯示工具呼叫」與該篇決定二「讓人少看一點」相呼應（2026-09-29 新增）
