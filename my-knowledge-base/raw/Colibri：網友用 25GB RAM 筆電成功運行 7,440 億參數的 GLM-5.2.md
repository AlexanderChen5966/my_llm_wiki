---
title: "Colibri：網友用 25GB RAM 筆電成功運行 7,440 億參數的 GLM-5.2"
source: "https://www.koc.com.tw/archives/648922"
author:
  - "[[達小編]]"
published: 2026-07-12
created: 2026-07-16
description: "Colibri 的開發者正是抓住了這個特性：每次 token 處理時，會改變的權重只有約 11GB（即被選中的 routed experts）。其餘的密集層（注意力機制、共享專家、嵌入層等，約 170 億個參數）可以在記憶體中常駐。這讓 Colibri 得以在 25GB RAM 的機器上運作。"
tags:
  - "clippings"
status: processed
---
Colibri