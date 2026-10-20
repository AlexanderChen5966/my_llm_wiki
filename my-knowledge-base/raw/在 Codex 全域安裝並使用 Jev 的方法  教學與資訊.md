---
title: "在 Codex 全域安裝並使用 Jev 的方法 | 教學與資訊"
source: "https://tools.wingzero.tw/articles/4074"
author:
  - "[[赤紅翼零]]"
published: 2026-09-21
created: 2026-09-22
description: "說明在 Codex 全域安裝並使用 Jev 的方法。"
tags:
  - "clippings"
status: processed
---
### 在 Codex 全域安裝並使用 Jev

1. **安裝 TypeSafe Skill**
	開啟終端機，執行：
	```
	npx skills add typesafe-ai/skills --skill typesafe-ai --agent codex -g
	```
	`-g` 代表全域安裝，因此 TypeSafe Skill 不會只綁定目前的專案，而是可以在 Codex 的所有專案中使用。
2. **建立 Jev API Key**
	前往 TypeSafe / Jev Console：
	[https://console.typesafe.ai/home](https://console.typesafe.ai/home)
	點選左側的 **API Keys** ，建立一組新的 API Key。
	API Key 請妥善保存，不要直接寫入專案程式碼、AGENTS.md 或提交到 Git。
3. **將 Jev API Key 提供給 Codex**
	第一次設定時，可以依照官方方式，將 Jev API Key 儲存成一個文字檔，例如：
	```
	jev-api-key.txt
	```
	接著在 Codex 中選擇這個檔案，並輸入：
	```
	這是我的 TypeSafe / Jev API Key，請依照 TypeSafe Skill 與官方最新文件進行設定。請勿將 API Key 寫入程式碼或提交到 Git。
	```
4. **測試 Jev API**
	設定完成後，在 Codex 輸入：
	```
	請使用 TypeSafe Skill，測試一下呼叫 Jev API 是否成功。
	```
	Codex 會透過已安裝的 TypeSafe Skill 讀取最新 TypeSafe 文件，並嘗試呼叫 Jev API。
5. **開始在 Codex 中使用 Jev**
	API 測試成功後，就可以直接要求 Codex 使用 TypeSafe / Jev，例如：
	```
	請使用 TypeSafe / Jev 分析以下資料，並回傳 Jev 的判斷結果。
	```
	或：
	```
	請使用 TypeSafe Skill，判斷這些內容應該如何分類，並顯示 Jev 回傳的機率與結果。
	```
6. **之後其他專案不需要重新安裝**
	因為 TypeSafe Skill 已經使用全域模式安裝，所以之後不論開啟 Laravel、Vue、WordPress 或其他專案，都可以直接要求 Codex 使用 TypeSafe / Jev，不需要每個專案重新執行安裝指令或重新貼上 Agent Prompt。