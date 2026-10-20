---
title: 去 AI 味寫作 Agent Skill 十選（文字版，非前端 UI）
date: 2026-07-09
tags: [ai/agent, type/reference]
source: https://tools.wingzero.tw/article/sn/3973
type: article
status: processed
asset_value: 30
---

社群整理的 10 個「去除文字 AI 味」Agent Skill 清單，鎖定文字寫作而非前端 UI（UI 領域的對應方案見 [[taste-skill-anti-slop-frontend]] / [[emil-design-engineer-skills]]）：humanizer（元老級去 AI 寫作痕跡）、Humanizer-zh（中文版）、stop-slop（去冗長調節奏）、taste-skill（給 AI 審美不寫套話）、ai-flavor-remover、shuorenhua（說人話中文改寫）、nuwa-skill（蒸餾任何人的表達風格）、writing-agent（全棧去 AI 味寫作系統）、chatgpt-comparison-detection（AI 文本檢測）、De-AI-Prompt-Enhancer（去 AI 味提示詞增強）。

## T（核心論點）

AI 生成文字的「一眼假」問題（套話、固定節奏、缺乏個人風格）與 UI 領域的「模板味」是同一類問題在不同輸出型態上的表現，社群已發展出對應的 Skill 生態，但本篇僅列出名稱與一句話定位，未附 repo 連結、star 數或實測數據，屬於待進一步查證的線索清單而非可直接評估的深度介紹。

## B（適用場景）

需要為文字寫作（部落格/行銷文案/報告）去除明顯 AI 腔調時，快速掃過清單找可能適合的工具名稱，再自行搜尋 GitHub repo 評估。

## R（十個工具速記）

| 工具 | 定位 |
|------|------|
| humanizer | 去 AI 寫作痕跡元老級 |
| Humanizer-zh | 中文版，專治中文 AI 腔 |
| stop-slop | 去冗長句、調節奏 |
| taste-skill | 給 AI 審美，不寫套話（注意：與 UI 領域同名的 taste-skill 疑似不同專案，需查證） |
| ai-flavor-remover | 通用 AI 味去除 |
| shuorenhua | 說人話，中文去味改寫 |
| nuwa-skill | 蒸餾任何人的表達風格 |
| writing-agent | 去 AI 味全棧寫作系統 |
| chatgpt-comparison-detection | AI 文本檢測（反向用途：驗證去味效果）|
| De-AI-Prompt-Enhancer | 去 AI 味提示詞增強 |

## C（關鍵行動）

- [ ] 使用前務必先查證各工具的實際 GitHub repo、star 數與最近更新時間，本清單未提供連結
- [ ] 注意本清單的 taste-skill 與 [[emil-design-engineer-skills]]（UI 動畫領域）的同名 Skill 可能是不同專案，避免混淆
- [ ] 可搭配 chatgpt-comparison-detection 類工具驗證去味效果，形成寫作→檢測→調整的迴圈

## 相關連結

- [[taste-skill-anti-slop-frontend]] — UI 領域的「去 AI 味」對應方案（三旋鈕調控）
- [[emil-design-engineer-skills]] — 同樣針對「AI 風格」問題的前端動畫 Skill
- [[anti-ai-look-image-style-prompts-38]] — **跨模態對照**：圖片端的「去 AI 感」38 種畫風提示詞。文字端靠句長與選詞破除統計指紋，圖片端靠指定實體相機/底片/光線缺陷破除「渲染感」，兩者的共同判準是🔑**加入模型不會自己選的具體約束**
- [[ai-flavor-removal-writing-technique-guide]] — 具體技巧內容本身（可直接安裝的 SKILL.md 規則清單），本條目是「工具清單」，該條目是「工具裡具體寫什麼」
