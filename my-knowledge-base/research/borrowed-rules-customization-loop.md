---
title: 借來的規則檔與 Skill：借用 → 觀察摩擦 → 修改的客製化循環
date: 2026-08-24
tags: [ai/claude, pkm/workflow, type/concept]
source: https://ithelp.ithome.com.tw/articles/10404398
type: note
status: processed
asset_value: 48
---

## 核心論點

別人的 CLAUDE.md、Skill 與流程值得當起點，但不能整套照搬。真正有用的客製化不是坐在桌前先設計一份完美設定檔，而是**先讓它工作、等摩擦出現，再就那個摩擦補一條規則**。

## 重點整理

- 可操作循環：**借用 → 使用 → 觀察摩擦 → 修改 → 再驗證**。規則不是一次寫完的憲法，而是協作時根據真實摩擦留下的工作筆記
- 實例：Karpathy 風格守則要求 AI 攤開取捨，AI 也照做了，但用的是軟工術語（抽象邊界、相依性、耦合）。翻成中文只解決「英文看不懂」，解不了「概念沒學過」——缺的不是答案，是做判斷所需的背景
- 修法不是重寫整套方法，而是借用 `mattpocock/skills` 的 `wait-what` 精神（使用者沒跟上時先停下、補背景、換一種說法），再自加一條：用專案例子或生活類比說明專有名詞；可以給建議，但要附判斷依據，以及什麼條件改變時會改選另一方案
- 摩擦 → 規則的具體寫法範例：連續三次沒被告知動了哪些檔案 → 補上「完成前，先用白話列出改動的檔案、目的與尚未確認的地方」。下次用小任務試；更好檢查就留下，每個小修改都多出一頁報告就縮短
- 把整張考卷推回 AI（「幫我分析優缺點再直接選一個」）在低風險、易反悔的決定上很省時間；但若選擇會影響架構、費用或維護方式，而你連它用什麼標準判斷都不知道，得到的只是一個答案，下一次仍然不會選
- 看到熱門 Skill／規則檔先問三題：它原本在解什麼問題？和我的目標、能力、限制相符嗎？能不能先改一條規則、用一個小任務驗證？
- 「最佳做法」要追問**「這是對誰最好的做法」**——兩天做展示的小組和要維護兩年的服務，不該承受相同的流程成本
- 起手參考庫：`forrestchang/andrej-karpathy-skills`（比對著空白檔案苦想「我的 AI 工作哲學」實際得多）
- 更難改的是自己成功過的方法：一套方法曾讓自己受到肯定，就容易從「可修改的工具」變成「我就是靠這樣成功的人」

## 可用之處

本知識庫大量吸收第三方 Skill 與 CLAUDE.md 慣例（obsidian-second-brain、superpowers、Karpathy 方法），這個循環正是安全吸收的流程：先跑一次、記下摩擦、只補一條規則、用小任務驗證，而不是一次改寫整份規則檔。「這是對誰最好的做法」也適合當 alignment-check 的追問句。

## 相關連結

- [[claude-md-design-patterns]]
- [[claude-md-behavioral-control-principles]]
- [[mattpocock-claude-skills-real-engineers]]
- [[grill-me-agent-deep-questioning-skill]]
- [[mcp-server-evaluation-criteria]]
- [[skill-as-installable-work-method]]
- [[rethink-workflow-not-just-accelerate]]
