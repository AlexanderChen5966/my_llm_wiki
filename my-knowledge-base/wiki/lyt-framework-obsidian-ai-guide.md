---
title: LYT 框架：Obsidian + AI 知識管理系統完整指南
date: 2026-05-20
tags: [pkm/obsidian, pkm/framework]
source: https://yu-wenhao.com/zh-TW/blog/lyt-framework-guide/
type: article
status: processed
asset_value: 12
---

LYT（Linking Your Thinking）是 Nick Milo 在 2020 年設計的知識管理框架，核心信念：「不要用分類整理筆記，用連結」。透過 MOC（Maps of Content）加入 Zettelkasten 缺少的「地圖」概念，讓知識能從高處俯瞰。在 AI 時代，LYT 的 MOC 結構讓 Claude Code 進入 vault 時能快速掌握整個主題的全貌，是目前 AI 相容性最高的 PKM 框架。

## T（核心論點）

LYT 讓你同時擁有「由下而上的連結」和「由上而下的鳥瞰」，這是 PARA 和 Zettelkasten 做不到的。MOC 是 AI 的「總表」——Claude Code 進入 vault 時，打開一張 MOC 就能看到整個主題的全貌和所有相關筆記，幾秒鐘掌握 context；Zettelkasten 沒有這個，AI 只能一張卡片一張卡片跳著讀，效率差很多。

## B（適用場景）

**適用：** 從 folder 地獄或 Roam / Zettelkasten 跳出來的使用者；想讓 AI（Claude Code）深度參與知識管理的人；知識庫規模快速增長（每天 5-10 則以上）的場景
**不適用 / 注意：** Ideaverse 免費版已可入門（70,000+ 下載）；ACE 頂層結構可依個人調整，不必完全照搬；LYT 管「知道」，不管「做到」，需另接目標執行框架

## R（三個核心概念）

### 1. MOC（Maps of Content）
一張 MOC 就是一則筆記，裡面放的是指向其他筆記的連結，按思路排列。一則筆記可以同時出現在多個 MOC（這是 PARA 資料夾做不到的）。建 MOC 的時機：累積 5 則以上同主題筆記，或讓 AI 自動判斷。

### 2. ACE 頂層框架
```
Vault/
├── Atlas/        ← 我知道什麼（知識卡片、MOC 索引、來源資料）
├── Calendar/     ← 我經歷了什麼（日記、回顧）
└── Efforts/      ← 我在做什麼（進行中專案、持續領域、完成產出）
```
三個空間對應三種心智模式：學習模式（Atlas）/ 反思模式（Calendar）/ 執行模式（Efforts）

### 3. 五種筆記類型（Atlas 內的知識卡片）

| 類型 | 問的問題 | 說明 |
|------|---------|------|
| Things | 這是什麼？ | 概念、框架、工具定義 |
| Statements | 我怎麼想？ | 你的觀點、洞察、原則（最有價值）|
| Questions | 我好奇什麼？ | 待探索的問題 |
| Quotes | 別人怎麼說？ | 引言 |
| People | 這個人是誰？ | 人物筆記 |

關鍵：大部分人只寫 Things，但讓系統產生價值的是 Statements——你自己的洞察。

### LYT vs PARA vs Zettelkasten（AI 相容性比較）

|  | LYT | PARA | Zettelkasten |
|--|-----|------|-------------|
| AI 相容性 | 高（MOC 是 AI 的總表）| 中（Archive 黑洞）| 低（逐卡跳讀）|
| 學習曲線（含 AI）| 低 | 低 | 中 |
| 解決的問題 | 知識網 + 導航 | 專案分類 | 原子概念連結 |

## C（關鍵行動）

- [ ] 從 Ideaverse for Obsidian（免費版）入門，理解 MOC 和 ACE 的實際運作
- [ ] 建立第一張 MOC（建議從已累積 5+ 筆記的主題開始）
- [ ] 設定 CLAUDE.md 讓 Claude Code 理解 vault 結構，讓 AI 幫忙歸檔和建連結

## 原始摘要

作者 WenHao Yu 從 Roam Research（Zettelkasten）轉到 Obsidian + LYT 的親身實踐。轉換結果：Roam 時期一週一張卡，現在每天 5 張以上 + MOC。缺口：LYT 管知識，不管行動，作者接上 12 Week Year 執行框架補足。

## 相關連結

- [[karpathy-knowledge-base-method]] — Karpathy LLM Wiki 方法（與 LYT 的對照）
- [[wenhao-yu-ai-workflow]] — WenHao Yu 的完整 AI 工作流（含每日 standup 與目標管理）
- [[claude-code-obsidian-setup]] — Claude Code + Obsidian 完整建構指南
- [[obsidian-knowledge-base-planning]] — 本知識庫的規劃文件
