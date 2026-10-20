---
title: coreyhaines31/marketingskills：base skill 與交叉引用的 Skill 集架構
date: 2026-08-25
tags: [ai/claude, ai/agent, type/how-to]
source: https://github.com/coreyhaines31/marketingskills
type: note
status: processed
asset_value: 24
---

## 核心論點

Corey Haines（前 Baremetrics 成長長）開源的行銷 Agent Skills 集合（MIT），符合 Agent Skills spec，可用於 Claude Code、Codex、Cursor、Windsurf。值得記的不是行銷內容，而是它的組織方式：以 `product-marketing` 當**共用 context base skill**，所有其他 skill 執行前先讀它，skill 之間再互相交叉引用。

## 重點整理

- Base skill 模式：`product-marketing` 是基礎，每個 skill 動作前先讀它取得產品、受眾與定位脈絡；避免每個 skill 各自重複詢問同一組背景
- 交叉引用鏈（每個 skill 有 Related Skills 段落）：`copywriting ↔ cro ↔ ab-testing`、`revops ↔ sales-enablement ↔ cold-email`、`seo-audit ↔ schema ↔ ai-seo`、`customer-research → copywriting / cro / competitors`
- 安裝：`npx skills add coreyhaines31/marketingskills`；**在 agent session 內執行會非互動地只裝到 `.agents/skills/`（Claude Code 不讀），必須加 `-a claude-code`**
- 另有 5 條安裝路徑：Claude Code plugin（`/plugin marketplace add` → `/plugin install marketing-skills`）、clone + copy、git submodule、fork 客製、SkillKit 多 agent 安裝
- v1.x → v2.0 遷移：重新命名 17 個 skill，`page-cro` + `form-cro` 併為 `cro`；**舊名資料夾不會被移除，需手動 `rm -rf`**
- context 檔搬家：`.claude/product-marketing-context.md` → `.agents/product-marketing.md`；舊路徑與舊檔名仍保留 fallback，不遷移也不會壞
- 分類：CRO（cro/signup/onboarding/popups/paywalls）、Content & Copy、SEO & Discovery（含 `ai-seo` 對應 AEO/GEO/LLMO）、Paid & Distribution、Measurement & Testing、Retention、Growth Engineering、Strategy & Monetization、Sales & RevOps
- 呼叫方式兩種：斜線指令 `/cro`、`/emails`、`/seo-audit`，或自然語言讓 agent 依 description 自行挑 skill
- 熱度（Tenten 中文策展手冊對照）：頁面標題稱 28K★，同站另一篇記為 31.3k 星 / 5.2k fork，數據隨時間變動；40 個 skill 分七大類、6 種安裝路徑
- 同系列策展對照：`zubair-trabzada/ai-marketing-claude`（14 個 market-* 指令，1.8k 星 / 575 fork）、`zubair-trabzada/ai-sales-team-claude`（14 個 sales skill、5 個並行 agent、BANT + MEDDIC 加權評分，658 星 / 243 fork）

## 可用之處

「base skill 供共用脈絡 + skill 間交叉引用」正好對應本知識庫的痛點：多個 Skill（knowledge-organizer / synthesizer / raw-pipeline）各自重述目錄結構與 inbox 判準。可考慮把共用脈絡收斂成一份被引用的 base，而非在每份 SKILL.md 重複。v2.0 遷移那段也提醒：Skill 改名時舊資料夾不會自動清掉，改名要附遷移指令。

## 相關連結

- [[agent-skill-design-patterns]]
- [[ai-agent-skills-popular-2026]]
- [[agent-plugins-open-standard]]
- [[aeo-seo-geo-llmo-glossary]]
- [[ai-marketing-suite-claude-skills]]
