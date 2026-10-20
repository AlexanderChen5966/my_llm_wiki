---
title: "Fable 5 延長到 7/12，這些是我省 Token 的方法"
source: "社群分享（原始出處未附連結，內容含三個 GitHub 專案連結）"
author:
published: 2026-07-07
created: 2026-07-09
description: "一個通用 Skill（Ponytail）+ 一個通用 MCP（codebase-memory-mcp）+ 一個 Fable 5 特化插件（pxpipe），皆未經橫向對比，作者實測後認為有效的省 Token 方法合集。"
tags:
  - "clippings"
status: processed
---
【Fable 5 延長到 7/12，這些是我省 Token 的方法】

總共是一個通用 Skill + 通用 MCP，和針對 Fable 5 特化的一個插件。

昨天 Fable 5 結束前發了篇文，不少人要我分享省 Token 方法的合集，以下都沒經過橫向對比，單純覺得有用就繼續用。若你覺得有其他更好用的，不妨一起分享給大家參考，相關連結我都放在留言。

## ▋很多人大推的 GitHub 項目 Ponytail

能讓 AI 少寫廢話，少寫重複內容，盡可能改必要的就行，精簡 Token 用量。

目前 7.7 萬星，很多人說有效，我自用也覺得有效，把項目連結丟給 Claude 讓它幫你裝就行，其他 AI 也能用，他本質就是 Skill + plugin。

要注意因為他會盡可能寫很少東西，導致 AI 失去創意，不少人分享感覺變更笨。我是讓我的 AI 改寫成適合我使用習慣的 CLAUDE.md，而不是完全照用。

## ▋將項目變成知識圖譜的 codebase-memory-mcp

這是一個通用 MCP，我的 Claude、Antigravity、Codex 都有裝，他不用嵌入模型，沒有 RAG，單純用 C 語言寫的程式，把項目連結裡面互相的關聯變成知識圖譜。

我覺得很有感，裝上後消耗明顯變少了，一樣把連結丟給 Claude 就能裝。記得要求查看項目前先用該 MCP，這樣就不會亂讀檔案，而是先了解關聯再去查。

之前分享時有大大說他比較過 10 多種類似產品，實際用 200 億 Token log 測試，最終這是他認定的第一名。使用在他 5000 多家店使用的中大型系統中。

## ▋針對 Fable 5 的省 Token 神器 pxpipe

這工具剛出不久，能把大量文字或程式碼擠在一張圖裡，再傳給 AI，利用 Fable 5 優秀的圖形辨識和腦補能力，省下圖片與文字的 Token 之間的價差，大概能省一半。

雖精度很高，但仍不算完全無損，所以工具可以視情況再開，像是需要 AI 精細的修改原始內容時，就可以先關掉。

我通常都讓 Fable 5 擔任 PM 角色，用來盤點現況，羅列要調整或開發的企劃書，所以損失的精度對我來說是可接受的。

一樣把連結丟給 Claude 裝就行，該工具還附一個網頁前端，能掌握使用工具的情況，直觀看到節省多少 Token。

## ▋相關連結

- 很多人大推的 GitHub 項目 Ponytail：https://github.com/DietrichGebert/ponytail
- 將項目變成知識圖譜的 codebase-memory-mcp：https://github.com/DeusData/codebase-memory-mcp
- 針對 Fable 5 的省 Token 神器 pxpipe：https://github.com/teamchong/pxpipe
