---
title: 各家 AI 的 Skill 支援與免費與否比較（2026-08）
date: 2026-08-24
tags: [ai/claude, type/comparison]
source: https://www.bnext.com.tw/article/91959/ai-skills-free-platform-comparison
type: note
status: processed
asset_value: 27
---

## 核心論點

Gemini Gems 十月底停用，是把重複使用的提示詞改做成 Skill 的直接理由——**把工作方法存成可攜帶格式，比綁在單一平台裡更保險**。各家 Skill 環境比想像中成熟，但免費門檻差異大。

> 📅 **時程並列附註（2026-10-08）**：本段「Gems 十月底停用」出自 2026-08 數位時代報導，保留作為當時快照。依 **Google 官方 support 頁（answer/18560919）**，三篇兩站一致：個人帳號 **2026-11** 起、Workspace 商務／企業／非營利 **2027-03**、教育帳號 **2027-06**，現有 Gems 與支援的知識檔**由系統自動轉成 Skills，不是單純停用**。另外 OpenAI 自訂 GPT 也將在 12/11 退役轉成 Plugin（媒體轉述待核）——「存成可攜格式」的論點在 Gemini 端已由官方證實，OpenAI 端仍待官方核對。→ [[custom-gpt-gems-to-skills-migration]]

## 重點整理

- 平台現況（2026-08）：Claude 在 **Customize > Skills** 上傳自製 Skill，免費即可用；**ChatGPT Work 與 Gemini Spark 雲端版目前限付費**；桌面版的 Google Antigravity 與 ChatGPT 有免費可用 Skill 的額度
- 觸發事件：Google Gemini Gems 確定於十月底停用，既有打包好的提示詞需要遷移
  - ⚠️（2026-10-08 附註）官方時程為分批自動轉換，見上方核心論點附註
- Skill 最小結構：一個資料夾內至少一份 `SKILL.md`（寫明名稱與適用場景），可另放範本、參考資料、執行任務需要的程式碼
- 三個篩選判準（適合做成 Skill 的工作）：一項工作重複三次以上、做起來有點像助理工作、你說得清楚完成標準；**需要大量臨場判斷的工作不要勉強固定成流程**
- 非工程師起手式：先帶著 AI 做一遍任務，完成後說「我想把這個重複的工作做成 Skill，以後照著我教你的方法做。需要釐清的地方先問我」，讓它反問——收到什麼資料？第一步做什麼？有哪些例外？輸出什麼格式？把回答整理好就是 Skill 雛形
- 用別人熱門 Skill 的做法：先丟給 AI 問是否適合自己的工作習慣再安裝；或先安裝跑一遍，再提出專屬判斷讓 AI 改版
- 時效性提醒：平台方案與免費額度變動頻繁，此為 2026-08 快照
  - （2026-10-08）Gemini 聊天中使用 Skills 已免費（2026-09-30 起，需個人帳號＋開啟活動記錄），只有 Spark 任務與排程需 AI Pro／Ultra（Google 官方）

## 可用之處

判斷「哪些工作值得寫成 Skill」的三條篩選判準可直接當本知識庫新增 Skill 的前置檢查；「讓 AI 反問你」的起手式則是把既有隱性流程外顯成 SKILL.md 的低成本方法，適用於還沒寫成 Skill 的手動整理習慣。

## 相關連結

- [[ai-skills-mcp-plugins-agents-comparison]]
- [[chatgpt-skills-usage-guide]]
- [[gemini-spark-personal-agent]]
- [[sop-to-skill-4-steps]]
- [[custom-gpt-gems-to-skills-migration]]
