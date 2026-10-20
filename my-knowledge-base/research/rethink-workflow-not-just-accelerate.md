---
title: 重新設計流程 vs 只把舊流程加速
date: 2026-08-24
tags: [pkm/workflow, ai/workflow, type/concept]
source: https://ithelp.ithome.com.tw/articles/10404763
type: note
status: processed
asset_value: 36
---

## 核心論點

當 AI 把執行成本壓低，真正該重新檢查的是那套仍然能用、自己也做得很熟的既有流程。**把舊流程每一步用 AI 加速，和重新決定哪些步驟還該存在，是兩件不同的事**——只做前者只是把同一條路走快，仍停在 Local Minimum。

## 重點整理

- 兩種改法對照：
  - 只加速：逐站下載 → AI 整理 → AI 畫完所有圖 → 人再慢慢檢查
  - 重新設計：固定資料入口 → 自動保留並檢查必要欄位 → 先標出需注意的測站 → 人類核對原始資料與科學意義
- 重新設計不是把所有事丟給 AI，而是重新安排工作：重複取得與整理交給工具，人把注意力放在資料來源、檢查條件、異常判斷與解釋
- 檢查既有流程的兩個提問：這套流程當初解決了什麼限制？那些限制今天還存在嗎？
- 小規模驗證（挑一件反覆做過的小工作，新舊方法在同一問題上各跑一次）的四個比較維度：完成同一份資料各需多久、數字是否一致、出錯能否定位到哪一步、換一批資料流程能否重跑
- 四種結論都算有效：**保留**（品質／成本／可控性仍合適）、**修改**（核心判斷留下、重複操作交工具）、**替換**（依賴條件已改變）、**證據不足**（再並行幾次，不必急著宣布革命成功）
- Google effect（Sparrow/Liu/Wegner, Science 2011）：預期之後仍能從電腦取得資訊時，人較不會記住資訊本身，卻更容易記得可以到哪裡找到它——取得成本下降會讓人重新分配注意力（作者註明後續研究對可重現性仍有分歧）
- 成功悖論（Audia, Locke & Smith）：環境劇變後，過去越成功越可能續用原策略；過去的表現會提高對原策略的滿意與信心，並**減少向批評者尋找資訊**
- 相關引用：Aditya Agarwal〈When Your Life's Work Becomes Free and Abundant〉、Google Labs Situationally Aware Agents（2026-08，討論 Agent 何時通知／提問／先備草稿／保持安靜）
- 一句話結論：**「過去的做法是證據，不是定律」**——它只證明流程曾在某些條件下可行

## 可用之處

正好是本知識庫流程演進的判準：raw-pipeline、inbox 判定、tag 體系都是「當初為某個限制設計」的流程（例如旁車標記制因 Obsidian 不可見而改回 frontmatter，就是一次「條件已改變 → 替換」的實例）。四維度小規模驗證可以當成日後改流程前的固定前置步驟，避免只是把舊流程交給更快的 Agent。

## 相關連結

- [[when-ai-builds-itself-reflection]]
- [[ai-era-developer-value]]
- [[skill-decision-what-to-automate]]
- [[pkm-adoption-decision-framework]]
- [[mcp-server-evaluation-criteria]]
- [[skill-as-installable-work-method]]
- [[borrowed-rules-customization-loop]]
- [[research/ai-career-map-before-resume]] — 同系列 Day 26：把「重新決定哪些步驟該存在」用在職涯／陌生領域探索上的實例
