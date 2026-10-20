---
title: OpenAI Codex 必裝 12 個插件與使用場景
date: 2026-06-15
tags: [ai/agent]
source: https://codelove.tw/@tony/post/3Qj5Ya
type: article
status: processed
asset_value: 62
---

Codex 插件生態的核心觀念：**不要按名字裝，要按工作流程裝**。插件的價值不是「多一個按鈕」，而是讓 Codex 能碰到真實素材、呼叫真實工具、交付真實檔案。

## T（核心論點）

Codex 正在從「程式設計師工具」變成「知識工作工具」，三個關鍵趨勢：
1. **角色外掛將主導插件生態**：打包 Skills + Connectors + 工作流的完整角色套件
2. **Sites 改變交付方式**：AI 輸出從文件變成可分享的互動式頁面
3. **插件走向客製化**：`.codex-plugin/plugin.json` + `skills/` 結構，企業可自定義

## B（適用場景）

**按角色選插件（不要全裝）：**

| 工作類型 | 建議插件組合 |
|---------|------------|
| 寫文章 / 內容 | Documents + Canva + Chrome + Presentations |
| 產品 / 前端 | Figma + Product Design + Chrome + Presentations |
| 營運 / 資料 | Spreadsheets + Documents + Chrome |
| 課程 / 影片 | HyperFrames by HeyGen + Remotion + Canva |
| 科研 / 醫學 | BioRender + Documents + Presentations |
| 桌面自動化 | Chrome + Computer Use + Google Calendar |

## R（12 個插件詳解）

### 設計類

**1. Canva** — 快速出圖。輸入主題+風格，生成海報/社群媒體圖片/封面。適合「我現在就需要一張能用的圖」的場景。

**2. Figma** — 把設計需求變成可編輯稿件。比 Claude Design 更適合產品 UI 和前端，提供 3 個視覺方向比較。

**3. Product Design** — 先做產品判斷再讓 Codex 寫程式碼。和 Figma 的差別：Figma 是設計畫布入口，Product Design 是完整工作流（brief → 視覺方向 → spec → 原型 → 設計評審）。

### 影片類

**4. HyperFrames by HeyGen** — 用腳本生成影片。HTML/腳本描述畫面，自動渲染，適合結構化影片（三段賣點/產品截圖/字幕）。

📎 2026-10-19：HyperFrames 另有 Claude Motion 的「Send to → HyperFrames」交接入口（單一來源轉述，見 [[claude-motion-beta-2026-10]]），與本條 Codex 插件／開源 HTML 影片工作流的安裝方式不同。

📎 2026-10-19：HyperFrames 已立專篇 [[hyperframes-heygen-html-to-video]]（README 一手，Team Lead 核對：Apache 2.0、Node.js 22+＋FFmpeg、三條官方並列的安裝路徑；⚠️ Codex 端 README 只描述 `bun run package:codex-plugin` 打包出 zip，沒有安裝指令，與本條「Codex 插件」的說法並列）。

**5. Remotion** — 更專業的可控影片動效。用 React 寫影片框架，適合做可重複使用的影片模板（版本更新影片/課程片頭）。

📎 2026-10-19：Remotion 授權（✅ 官方 LICENSE.md，Team Lead 核對）：個人、最多 3 名員工的營利組織、非營利、評估者適用 Free License，其餘須購買 Company License；安裝、可重現規則與選型見 [[claude-codex-video-toolchain-selection]]。

### 辦公類

**6. Presentations** — 快速生成簡報初稿。先搭大綱+拆邏輯+排結構，再生成可修改的簡報。當作「第一版結構生成器」使用。

**7. Spreadsheets** — 複雜表格和資料分析。CSV 整理、樞紐分析、公式補全、分析報告。

**8. Documents** — 長文本處理和報告寫作。把分散材料整理成結構化文件，適合方案/報告/腳本。

**9. BioRender** — 科研和醫學圖表。非科研方向先不裝；有專業圖表需求才裝。

### 自動化類

**10. Chrome** — 讓 Codex 直接操作網頁。讀取網頁/抓取資訊/填寫表單/驗收本地頁面。⚠️ 涉及登入/付款/刪除/發信動作要人工確認。

**11. Computer Use** — 讓 Codex 操作本地桌面應用。比 Chrome 更進一步，可操作無 API 的桌面 UI。⚠️ 高風險操作必須先確認，不要完全放飛。

**12. Google Calendar** — 日程管理。根據會議紀要建立日程草稿、找衝突和空檔。重點是產草稿+等確認，不要自動執行。

## C（關鍵行動）

- [ ] 按主要工作類型選 3-5 個插件，不要一次全裝
- [ ] 把 Chrome + 排程任務串成固定工作流（自動讀取資料 → 整理 → 輸出）
- [ ] 敏感操作（轉帳/刪檔/寄信）務必設確認護欄

## 相關連結

- [[openai-codex-sites]] — Codex Sites 功能（文件→互動網站的下一步）
- [[codex-advanced-tips-11]] — Codex 11 個進階技巧
- [[codex-plugin-workflow]] — Codex Plugin 可重用工作流架構
- [[claude-code-design-skills-overview]] — Claude Code 設計 Skills 生態（vs Codex 插件）
- 📎 2026-10-19：[[claude-motion-beta-2026-10]] — Claude Motion 的下游夥伴含 HeyGen（HyperFrames）、Runway 等
- 📎 2026-10-19：[[hyperframes-heygen-html-to-video]] — HyperFrames 專篇；[[claude-codex-video-toolchain-selection]] — Claude Code／Codex 做影片的工具選型與 Remotion 授權
