---
title: Agent Readiness 體檢：用 Cloudflare 免費工具量化「AI 讀不讀得懂你的官網」
date: 2026-08-07
tags: [ai/workflow, method/tools]
source: https://www.bnext.com.tw（Cloudflare 2026-04 分析 ＋ isitagentready.com）
type: article
status: processed
asset_value: 60
---

## T（核心論點）

**AI 代理人不是「瀏覽」你的網站，而是「解析」它**——人類先感受視覺設計與品牌氛圍，AI 代理人則依賴網站結構、標記與協定。

因此要分清兩個層次：
> **GEO 處理內容層問題；Agent Readiness 處理「網站能否被代理人發現、存取與操作」的基礎問題。**
> 🔑 **即使 GEO 做得再精準，如果網站結構讓 AI 無法順利進入或完整讀取，優化效果仍會打折扣。**

而這件抽象的焦慮現在可以被量化——Cloudflare 的 `isitagentready.com` 給 0–100 分。

## B（適用場景）

**適用：** 官網改版後確認 AI 代理人是否讀得懂；品牌／電商想知道自己在 AI 眼中的可存取性；GEO 做了但成效不明時的上游診斷

**不適用 / 注意：**
- ⚠️ **Cloudflare 預設不將 `llms.txt` 納入評分**（可透過自訂掃描檢查）
- ⚠️ **代理式商務協定雖會顯示檢查結果，截至 2026-08 也不計入分數**
- 🔴 每個未通過項目附的改善建議「**由 AI 生成，仍須由具備專業判斷的人員確認後再執行**」（官方自己的提醒）

## R（數據與檢測項目）

### Cloudflare 2026-04 掃描 20 萬個熱門網域的結果

| 項目 | 比例 |
|---|---|
| 設有 `robots.txt` | **78%**（但多數仍以傳統搜尋引擎爬蟲為主）|
| 透過 **Content Signals** 聲明 AI 使用偏好 | **只有 4%** |
| 能提供 **Markdown 版本內容** | **僅 3.9%** |

→ 這三個數字說明：**絕大多數網站的 robots.txt 是為 Google 寫的，不是為 AI 代理人寫的。**

### 體檢工具檢測什麼

`isitagentready.com`（免費，輸入網址即掃描）：

- `robots.txt` 是否**允許主要 AI 爬蟲**進入
- 有沒有提供**網站地圖**
- 內容能否以 **Markdown 格式**被有效讀取
- 網站是否公開 **MCP Server Card**、**Agent Skills** 等新興協定
- 可自訂掃描檢查 **`llms.txt`**
- 涵蓋**代理式商務**的相關標準（顯示結果但不計分）

> 實務案例：一個深耕多年、剛完成改版的品牌官網，在 AI 代理人眼中**只有 59 分**。

## C（關鍵行動）

- [ ] 用 `isitagentready.com` 對自己（與競品）官網各掃一次，先拿到基準分數
- [ ] 檢查 `robots.txt` 是否**明確允許主要 AI 爬蟲**——78% 的網站有這個檔案但多數只為傳統爬蟲設定
- [ ] 評估提供 **Markdown 版本內容**（目前只有 3.9% 的網站有，是低成本的差異化項目）
- [ ] 若已做 GEO 但成效不明，先回頭確認 Agent Readiness——**內容層優化擋不住結構層問題**
- [ ] 🔴 工具給的改善提示詞**不要直接交給程式代理人執行**，先人工確認（官方明確提醒）
- [ ] 注意 `llms.txt` 與代理式商務協定**目前不計分**，別因為分數沒動就以為沒效果

## 相關連結

- [[aeo-implementation-llms-txt]] — AEO 操作層實作與 `llms.txt` 完整規格；本篇是其上游的**基礎可存取性**檢查，兩者是內容層 vs 結構層
- [[aeo-seo-geo-llmo-glossary]] — AEO/SEO/GEO/LLMO 術語辨析；本篇補上 **Agent Readiness** 這個新層次
- [[agent-plugins-open-standard]] — Agent Plugins 1.0；本篇檢測項目中的「Agent Skills 協定」與其屬同一波標準化
- [[mcp-stateless-architecture]] — MCP 規格；本篇檢測的 **MCP Server Card** 是網站側的公開宣告
- [[openseo-ai-seo-mcp]] — AI SEO 的 MCP 工具；與本篇的診斷面互補
