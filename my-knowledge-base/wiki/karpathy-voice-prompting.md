---
title: Karpathy 的「10 分鐘語音碎念法」：先倒思緒，再讓 AI 整理
date: 2026-07-24
updated: 2026-08-03
tags: [ai/prompt-engineering, type/how-to]
source: https://www.bnext.com.tw/article/91600/karpathy-voice-prompting-method
type: article
status: processed
asset_value: 36
---

Karpathy 分享的反直覺提示技巧：懶得打字時切到語音，意識流般把背景、限制、顧慮全倒給 AI（講得亂七八糟也沒關係），讓 LLM 從雜亂輸入推測並整理出乾淨版本，達成「對頻」（mind meld）。賭的不是你表達多好，而是模型從脈絡推測意圖的能力。

## T（核心論點）

與其擠出一段精簡卻漏東漏西的文字，不如用講的把背景/目的/卡住的點/猶豫全部倒出來。LLM 很擅長從雜亂輸入推測重點，回述的版本「往往比你一開始講的乾淨許多」。當 AI 回述與你想的一致，就完成對頻，後續修正變少。

## B（適用場景）

**適用：** 腦力激盪、規劃草案、想法還很模糊時。
**不適用 / 注意：** 需要精確事實與數字的工作別全交給碎念（轉錄可能出錯、AI 可能補入你沒講的內容、錄音會上傳雲端有機密風險）；長期外包「整理思路」會讓自己組織想法的能力鈍化。

## R（三個實作技巧 + 工具）

1. **開頭先「自首」**：第一句講「以下是語音轉文字，我會講得比較跳，錯字請包涵」，讓模型把辨識錯字當雜訊。
2. **別管邏輯，想到什麼講什麼**：跳躍、自我矛盾、改口都沒關係，這些是模型理解你的線索。
3. **講完請 AI 回述**：先別要成品，請它用自己的話重述、列出不確定處、反問 2–3 個問題；確認一致再動工。

**工具**：Claude Code `/voice`（按住空白鍵講、放開轉文字；需 Claude.ai 帳號、本機使用、**官方聽寫語言 20 種目前無中文**、不消耗 token）；ChatGPT 聽寫（App 麥克風）；系統內建聽寫（免費、支援中文）。中文碎念可用 Typeless；進階雙向語音可研究開源 VoiceMode。

## C（關鍵行動）

- [ ] 下次規劃/腦力激盪任務，先語音碎念再讓 AI 回述確認，別急著要成品
- [ ] 精確數字、事實引用、對外發布內容務必逐項核對
- [ ] 讀完 AI 產出後自己重寫成幾條精煉重點，避免思考鈍化
- [ ] 中文環境目前 `/voice` 不支援中文，改用系統聽寫或 Typeless

## 原始摘要

數位時代，李先泰整理。Karpathy（OpenAI 創始成員、2026-05 加入 Anthropic）2026-07-22 X 貼文。含一般做法 vs 碎念法對照表、三個語音工具比較、三種風險（轉錄出錯/幻覺補入/錄音上雲）。

## 相關連結

- [[voicebox-local-voice-tool]] — 本機語音工具（中文語音輸入替代方案）
- [[synthesis/synthesis-prompt-engineering-evolution]] — 提示工程演進：從指令細節到 Context 系統設計
- [[blind-spot-check-prompt-technique]] — 請 AI 反問/找盲點的提示技巧（同屬「讓 AI 回述確認」家族）
- [[karpathy-ai-knowledge-base-workflow]] — Karpathy 知識庫設定法（同源「小錯滾雪球」風險）
- [[codex-advanced-tips-11]] — **同命題不同來源的獨立佐證**：OpenAI 的 Jason Liu 在 AI Engineer 大會示範「15 分鐘雜亂語音備忘錄勝過精緻 prompt」，與本篇的 10 分鐘碎念法論點一致（講話約打字三倍速、模糊線索由 AI 自己回溯查證）；他另補上 Appshots——把螢幕文字與畫面一起送出，是「輸入頻寬」思路的另一種解法
- [[karpathy-opus5-lotr-3d-case]] — Karpathy 的 Opus 5《魔戒》3D 實驗；**該篇統一注記了他 2026-05 加入 Anthropic 一事對 `karpathy-*` 系列的解讀影響**
