---
title: 213K 星的 Superpowers，90% 的人只用了 10%：六個月裝了 50 個 Skill 後留下的 20 個
date: 2026-07-09
tags: [ai/claude, type/comparison]
source: https://codelove.tw/@tony/post/xdjA4a
type: article
status: processed
asset_value: 68
---

Skills 生態上線六個月後的「清庫存帳單」：裝過 50+ 個 Skill，實際天天用的不到 5 個。本文不是推薦列表，是淘汰賽——四大類 20 個留下的 Skill + 四種常見的「該卸載」訊號。

## T（核心論點）

**Skill 不是更好的 prompt，而是帶階段門檻的工作流程模組。** Prompt 是建議、Claude 可忽略；結構良好的 Skill 定義明確階段門檻（如「紅燈測試必須失敗才能進下一步」），Claude 更傾向照做。判斷標準：「沒有它，這件事會多花多少時間？」5 分鐘內大概率是噱頭，「每次都要手動處理很煩」的才值得裝。

## B（適用場景）

篩選 Skill 是否值得安裝／保留；已裝 50+ 個 Skill 想清庫存；想理解 Skill 生態的品質分佈。

## R（四類 20 個留下的 Skill）

**工程提效（6）**：Superpowers（obra，213K★，20+ 子模組，留 test-driven-development / systematic-debugging / writing-plans）、Karpathy Guidelines（125K★，四條規則：先思考再動手/精準修改/優先簡潔/核對原始需求）、gstack（Garry Tan，93K★，型別檢查+Schema 驗證）、Frontend Design（官方，避免 AI UI 一眼被認出）、Document Skills（官方，DOCX/PDF/PPTX/XLSX）、Trail of Bits Security（安全審查清單）。

**多 Agent 編排（5，噱頭最多的類別）**：TDD（Superpowers 子模組，Red-Green-Refactor 強制階段）、Ruflo（多 Agent 流水線，作者用兩週後卸載——複雜度大於 100 步驟大型工作流才需要）、Dispatching Parallel Agents（獨立子任務並行，跨模組重構省 40% 等待）、Awesome Claude Code Subagents（模板庫非即插即用）、**Loki Mode（37 Agent 自主系統，裝 3 天卸載——修改未確認檔案、自主提交程式碼，過度自主的典型反例）**。

**記憶與上下文（4）**：Claude Mem（74K★，自動存決策/約束/架構選擇）、Claude Context（壓縮 CLAUDE.md 為最小 token 注入）、ccstatusline（token 用量視覺化）、CC Switch（67K★，模型分層切換，作者月帳單降 35%）。

**文件與辦公（3）**：Graphify（46K★，程式碼庫生成關係圖，3 分鐘理解兩年老專案）、Planning with Files（計畫先出 Markdown 檔再執行）、Claudian（Claude↔Obsidian 打通，適合寫 ADR）。

## 補充：兩個常被忽略的內建功能（非第三方 Skill）

（來源：codelove.tw 另一篇「篩了 1400 個 Skills 留 5 個」）

- **`/deep-research "問題"`**：Claude Code 內建調研功能，與單純叫 AI 搜尋的差異——多角度扇形搜尋自動擴展關鍵字、抓多個來源交叉校驗、對矛盾資訊投票標註置信度、輸出帶引用來源的結構化報告（而非一段文字）。適合技術選型/方案評估/新技術了解等決策情境，Token 消耗較高不宜頻繁用。
- **`/find-skills "關鍵字"`**：內建 Skill 搜尋入口，1400+ 個 Skill 不可能逐一翻 GitHub，用關鍵字搜尋（如「前端 UI 設計」「測試 自動化」）是及時發現新 Skill 的正規做法。

**額外佐證數據**：有人在 GitHub Issue 貼出對照實驗，12 個相同複雜度任務下，裝 Superpowers 的會話比沒裝的 token 用量平均低 14%——反直覺原因是它會先花時間問清楚需求，避免後面返工，屬於「先想清楚再動手」帶來的複利效果。

## 四種該卸載的訊號

1. **功能太窄**：每週用不到 1 次的場景，裝之前先想清楚
2. **安裝複雜、收益太低**：好 Skill 是「一行命令裝完，第二天有感」
3. **過度自主、邊界模糊**：關鍵操作前沒有「是否繼續」確認機制的，正式程式庫要謹慎（Loki Mode 案例）
4. **純 prompt 包裝**：打開 SKILL.md 若全是說明文字、無「步驟 N：驗證 X 通過才能繼續」的結構，大概率無效

## C（關鍵行動）

- [ ] 用「5 分鐘內能自己搞定 vs 每次都很煩」判斷新 Skill 是否值得裝
- [ ] 定期清庫存：檢查 `.claude/skills/` 有沒有裝了但半年沒觸發的
- [ ] 過度自主型 Skill（無確認機制、可自主提交/修改檔案）在正式程式庫要格外謹慎

## 相關連結

- [[superpowers-framework]] — Superpowers 本體（Mandatory Workflow 框架）
- [[ai-agent-skills-popular-2026]] — 2026 最受歡迎 Agent Skills 排行（Superpowers/Karpathy/gstack 交叉對照）
- [[claude-skills-ecosystem]] — Claude Skills 生態全景（16 個高 star Repo）
- [[skill-md-writing-techniques]] — SKILL.md 撰寫技巧（階段門檻設計）
- [[claude-code-file-management]] — 設定檔管理（.claude/skills/ 位置）
