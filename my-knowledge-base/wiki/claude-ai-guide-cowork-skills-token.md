---
title: Claude AI 完整教學地圖：38 篇 Cowork、Skills、省 Token 全打包
date: 2026-06-01
tags: [ai/claude, ai/prompt-engineering]
source: https://www.bnext.com.tw/article/91080/claude-cowork-skills
type: note
status: processed
asset_value: 54
---

> ⚠️ **來源特性註記（2026-08-20）**：該 bnext 合輯是**持續增補的索引頁**（同一 URL `91080`），
> 篇數從 39 篇增至 40 篇，raw/ 曾兩度重新抓取。**同 URL 的後續版本一律視為同一素材**，
> 除有實質新主題外不再另行處理。


《數位時代》整理 38 篇 Claude 教學文章，從方案選擇、模型差異（Opus/Sonnet/Haiku）、提示詞技巧，到 Cowork、Skills、省 Token 等進階應用一網打盡。2026 年最重要的使用轉變：長指令細節堆疊已不如「簡短明確指令」有效，讓模型自行判斷細節。

## T（核心論點）

> ⚠️ **2026-09-16 已改版**：下方「三模式生態系（Chat / Cowork / Code）」的架構前提已變——Cowork 獨立分頁已消失，
> 併入一般對話 Chat，介面縮減為 Claude 與 Claude Code 兩入口，改由 Claude 自行判斷該用哪種能力而非使用者自選模式，
> 詳見 [[claude-cowork-merged-into-chat-2026-09]]。

2026 年 Claude 生態已從單一對話工具演化為三模式生態系（Chat / Cowork / Code），使用策略需要對應更新：

**舊方法失效的核心原因**：Anthropic 新版模型（Opus 4.7+）偏好簡潔直接指令，過去的長指令、細節堆疊方式反而降低輸出品質。

**八大學習地圖：**
1. **入門**：Chat/Cowork/Code 三模式差異、Opus/Sonnet/Haiku 選用（差距 5 倍成本）、記憶轉移功能
2. **進階提示詞**：「少即是多」官方建議、Opus 4.7 升級攻略、情境工程 5 大步驟
3. **Cowork 實戰**：非工程師 AI Agent、Markdown 知識庫、排程排毒、20 分鐘百頁 PPT
4. **Skills 養成**：Skill 是什麼、6 步驟打造第一個 Skill、X 上爆紅 26 個現成 Skill
5. **Projects & Design**：長期記憶助理（5 步驟）、簡報工具（PPTX/Canva 匯出）
6. **工作場景**：Excel 側邊欄、Word 改稿、麥肯錫風 PPT、論文整合、互動視覺
7. **省 Token**：4 個爆量情境 + 9 個技巧、PDF 處理差 22 倍、Anthropic 官方 4 條建議
8. **Claude Code 入門**：安裝教學、CLAUDE.md 12 條規則、快捷鍵與斜線指令速查

## B（適用場景）

**適用：**
- 剛開始使用 Claude 需要全景圖的人
- 已有基礎但想進階到 Cowork/Skills 的知識工作者
- 需要節省 Token 配額的付費用戶

**不適用 / 注意：**
- 這是索引文章，每個主題都有更深入的獨立教學連結
- 半年後功能會繼續更新，此地圖為 2026 年 6 月版本

## R（操作步驟）

建議學習路徑：
1. **第一章搞懂方案與模型**：確認訂閱方案，理解 Opus/Sonnet/Haiku 的成本差異
2. **依工作場景跳讀**：常做簡報 → 看 Cowork 與 Design；Excel/Word → 看第六章
3. **建立第一個 Skill**：四章 Skills 段落，從「Skill 是什麼」到 6 步驟打造
4. **省 Token 策略**：省 Token 章節，至少學會 PDF 處理最省方式

## C（關鍵行動）

- [ ] 確認目前使用的訂閱方案，評估是否需要調整（Free/Pro/Max/Team）
- [ ] 嘗試用 Cowork 模式完成一個實際工作任務
- [ ] 建立第一個 Claude Skill（從現有 26 個爆紅 Skill 中選一個安裝試用）
- [ ] 搭配姊妹篇《AI 提示詞完整地圖》建立可複用工作流

## 原始摘要

《數位時代》整理 38 篇 Claude 教學文章，從方案選擇、模型差異（Opus/Sonnet/Haiku）、提示詞技巧，到 Cowork、Skills、省 Token 等進階應用一網打盡。指出 2026 年新版模型下「長指令細節堆疊」已不如「簡短明確指令」有效。

## 相關連結

- [[ai-prompt-map-100-workflows]] — 100+ 組 AI 提示詞完整地圖（姊妹篇）
- [[claude-skills-guide]] — Claude Skills 完整指南
- [[claude-code-context-management]] — Context 管控與省 Token 策略
- [[claude-opus-4-8-release-notes]] — 最新模型 Opus 4.8 功能更新
- [[llm-cost-optimization-strategies]] — LLM 成本優化四層策略
