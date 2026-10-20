---
title: Skill 是可安裝的「工作方法」：Prompt 管這一次，Skill 管這一類
date: 2026-08-24
tags: [ai/claude, ai/agent, type/concept]
source: https://ithelp.ithome.com.tw/articles/10404155
type: note
status: processed
asset_value: 48
---

## 核心論點

Skill 是「一套可以重複載入的工作方法」，與 Prompt 的分工可以一句話切開：**Prompt 管「這一次要完成什麼」，Skill 管「遇到這一類任務時通常按什麼方法做」**。真正在改變的是 SOP 的身份——從雲端硬碟裡沒人讀的文件，變成 Agent 能載入、執行、驗證、改版的組織記憶。

## 重點整理

- 最小形態：一個資料夾 + 一份 `SKILL.md`。三要素為 `name`（須與資料夾同名）、`description`（不只寫「會做什麼」，還要交代「什麼情況適合使用」，因為它同時是 Agent 判斷是否載入的依據）、Markdown 正文（真正的工作方法）
- description 反例：「很厲害的 Skill」等於履歷上寫「本人能力很好」——有自信但不知道該不該把工作交給你
- Progressive disclosure 五步載入鏈：Agent 先知道名稱與用途 → 使用者提出需求 → Agent 判斷適用 → 完整 `SKILL.md` 載入 Context → 依指示執行；附帶的參考文件、腳本、素材也是真正需要時才取用，所以裝五十個 Skill 不會每次都塞爆 Context
- 進階目錄按需擴充：`scripts/`（可執行程式）、`references/`（需要時查閱的資料）、`assets/`（範本與輸出素材）
- Skill 不只是「比較長的 Prompt」：它可以被找到、按需載入，也能帶著其他資源一起工作
- 四種觀察角度（非官方分級、非由小到大的階梯，會彼此重疊）：①封裝提問法——`mattpocock/skills` 的 `grill-me`，一次只問一題並附建議答案與理由，封裝的是問問題的方法而非答案 ②串接流程——`obra/superpowers`，需求→設計→計畫→實作→檢查→驗證，前一步輸出是下一步輸入 ③角色分工——`garrytan/gstack`，產品／工程／設計／品質／安全／發布視角，實質是同一 Agent 在不同階段載入不同問題清單 ④整理判斷框架——`alchaincyf/nuwa-skill`，從公開資料整理表達方式、心智模型、判斷方法與已知限制（專案自陳邊界：公開表達不等於真實想法，只是蒐集當下的快照）
- 規格權威來源：agentskills.io/specification；載入與觸發方式依產品而異，Claude Code 另有自動載入與手動呼叫兩種
- 核心警語：**「拿到別人的方法，不等於擁有他的判斷」**；若只把「平常怎麼做」裝進 AI，卻沒有把「什麼時候不該這樣做」一起交接，等於把資深者的盲點自動化並加速

## 可用之處

直接對應本知識庫大量自寫 Skill 的實務：description 要寫適用時機（本庫 CLAUDE.md 已用「description 已自動載入」做 progressive disclosure）、`references/tag-taxonomy.md` 正是「需要時查閱」那一層。最值得補的是那句警語——現有 SKILL.md 多半寫了「怎麼做」，卻少寫「什麼情況不該套用這個流程」。

## 相關連結

- [[agent-skill-practical-guide]]
- [[claude-skills-beginner-guide]]
- [[skill-md-9-types-and-gotchas]]
- [[sop-to-skill-4-steps]]
- [[grill-me-agent-deep-questioning-skill]]
- [[mcp-server-evaluation-criteria]]
- [[borrowed-rules-customization-loop]]
- [[rethink-workflow-not-just-accelerate]]
