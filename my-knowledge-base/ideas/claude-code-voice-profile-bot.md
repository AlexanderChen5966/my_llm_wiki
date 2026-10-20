---
title: Voice Profile 萃取法：讓 AI 代筆寫得像你（含反指紋）
date: 2026-08-12
tags: [ai/claude, ai/workflow]
source: https://www.bnext.com.tw/article/91812/claude-code-threads-auto-poster
type: idea
status: processed
asset_value: 45
---

## 核心想法

原文是「用 Claude Code 做 Threads 自動發文機器人」教學，但**真正可複用的不是機器人，是 voice profile 那一套方法**——它可以移植到任何 AI 代筆場景（X、LinkedIn、IG 文案、電子報、甚至知識庫的對外輸出）。

> **難的不是技術，是「寫出來像不像你」。** 如果貼文長得像 ChatGPT 那種工商風格，大家一看就知道是機器人，發了還不如不發。

## Voice Profile 四步萃取

**步驟 1：收集樣本**
過去 **8–15 篇**自己寫的真實貼文整理成一個檔案。**8 篇是最低門檻，15 篇最理想**——少於 8 篇 AI 抓不出規律。

**步驟 2：丟給 Claude 分析（指令範本）**

```
請從下面 N 篇我寫的貼文，萃取出我的 voice profile，
包含開頭習慣、句法特徵、用詞偏好、情緒詞彙、結尾習慣，
還有我絕對不會寫的東西，也就是反指紋。
輸出格式是條列，每條附 1-2 個範例。
```

🔑 **「反指紋」（我絕對不會寫的東西）是這套方法最容易被漏掉、也最有效的一項**——它把「像我」從正面舉例變成雙向約束。

**步驟 3：校對加補強**
第一版大概只有 **70 分**。自己看一遍：補上沒抓到的招牌詞、刪掉 Claude 瞎掰的。**改 3–5 輪會出 85 分的版本。**

**步驟 4：存成 `voice_yourname.md`**
之後每次叫 AI 寫東西，先把這份文件丟給它讀。

作者自己的 profile 內容舉例：場景直入的開頭、段落中間用空格停頓、招牌口頭禪與招牌字、不放 emoji 的進度更新型貼文、「免費仔」這類自我貼標籤。

## 可替換的骨架（發文機器人部分）

- **素材來源三原則**：每天有新內容／對齊品牌軸線／有公開 API 或 RSS（作者選 GitHub Trending；其他候選：Hacker News、各家更新紀錄、PapersWithCode）
- 用 Claude Code 建 Python 專案，串 Threads 的 Meta Graph API
- ⚠️ 踩坑：Windows PowerShell 中文 cp950 亂碼
- 排程三選一：Windows 工作排程器／cron-job.org + Vercel／GitHub Actions cron
- 全程約 30–60 分鐘

## 歸 ideas 的理由

發文機器人本身不是主人的需求；但 voice profile 方法（8–15 篇樣本 + 反指紋 + 3–5 輪校對）是可獨立取用的技術，值得留成靈感條目。

## 相關連結

- [[ai-flavor-removal-writing-skills]] / [[ai-flavor-removal-writing-technique-guide]] — 🔑 **同一問題的反向解法**：那邊是「移除 AI 味」，本篇是「加入你的味」；反指紋清單正是兩者的交會點
- [[claude-conversation-style-value-axes]] — Claude 對話風格的價值軸
- [[taste-skill-anti-slop-frontend]] — 「品味」如何被寫成可重用規格（前端版）
- [[sop-to-skill-4-steps]] — 把 `voice_yourname.md` 升級成正式 Skill 的路徑
- [[claude-python-automation-workflow]] — Claude Code 建 Python 自動化專案的骨架
- [[claude-code-life-management]] — 個人自動化排程的其他應用
- [[agent-memory-lifecycle]] — voice profile 本質上是一筆 Scope=Global、Type=Preference 的長期 Memory
