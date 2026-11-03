---
title: Claudia：用 Claude Code 當製作團隊做 AI 音樂影片的流程洞察（單一來源個案，2026-10）
date: 2026-11-03
tags: [ai/agent, ai/workflow]
source: https://www.aiposthub.com/claudia-claude-music-video-pipeline-euclid-rhythm/
type: idea
status: processed
asset_value: 9
---

> 🔴 **來源性質**：aiposthub（Philo，2026-10-21）整理創作者 anabology 的 claudia.gallery 公開紀錄與 X 貼文。**所有成本、時間、流程數字皆為作者自報、單一來源、無第三方查核**。asset_value 9，偏外圍（AI 影音創作），只留流程洞察。
> ⏰ 時效：2026-10 快照；授權規則與 Suno／Seedance／Midjourney 的價格與條款以當時官網為準，會變動。
> ⚠️ 網站另列 **Solana 社群代幣**（交易手續費資助影片）與 T 恤周邊（20–35 美元）；原文明言**不評價、不推薦、未查證**該代幣。本庫同樣**不推薦、未查證**，不收錄細節。

## 一句話洞察

**人只在會改變方向或花錢的地方介入**：六個人工檢查點，其餘由代理平行處理。

## 流程洞察（作者自述）

- 分工：Claude Code 擔任整個製作團隊（研究、歌詞、分鏡、提示詞、腳本、剪輯）；Midjourney 靜態畫面、Suno 歌曲、Seedance 動態與口型。**Claude 負責企劃與工程，音樂由 Suno 生成**；Suno 無 API，須人按下生成並下載
- 六個人工檢查點：概念（3 個提案選 1）→ 歌曲（Suno 版本選一）→ 角色臉孔（4 張測試圖）→ 分鏡 → **付費前提示詞與預估費用（沉默不算同意）** → 第一版成片（先看 20–40 秒）
- 教訓：某支影片未經人讀就送出提示詞，花約 830 Higgsfield 點數換來無生命力鏡頭；之後規定**花錢的提示詞一律由人讀過才送出**
- 口型同步：用程式比對生成影片聲音與原始人聲，找出偏移點、之前剪掉再接續

## 數字（作者自報，單一來源，不可外推）

- 六支影片耗時約 13–41 小時（合計約 133 小時）；影片生成費約 93–365.90 美元（不含訂閱與硬體）
- 「Claude 用量換算」約 626–1,007 美元僅是事後依 API 牌價換算，作者用的是訂閱方案，並非實付
- 縮小規模的快速版估計 3–20 美元／2–6 小時（作者估計，非實測）
- Euclid 解說影片無公開製作紀錄，**不能套用上述數字**

## 授權（開放角色）

- 作者允許任何人用 Claudia 角色與網站圖片做作品，商用可、不必詢問；請維持成年角色、標示 AI 生成、不要重新發行那六首歌與分軌
- **不涵蓋**影片中的第三方素材（品牌影像、桌布與音效、真實 App 介面、公眾人物貼文）
- 網站聲明為對 Claude 的同人作品，與 Anthropic 無關（作者自述，未向 Anthropic 查證）

## 相關連結

- [[bitwize-music-plugin-skills-mcp-architecture]] — 同為 Claude＋Suno 的案例；該條是 plugin 架構參考，本篇是單源個案
- [[google-lyria-3-5-music-generation]] — 音樂生成競品對照
