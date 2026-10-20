---
title: taste-skill：防止 AI 生成無聊介面的前端設計 Skill 套件
date: 2026-06-17
tags: [ai/agent]
source: https://github.com/Leonxlnx/taste-skill
type: reference
status: processed
asset_value: 52
---

AI 生成的前端有個一致的問題：正確但無聊，置中且平庸——大量白底卡片、標準 flex 佈局、毫無個性的灰階按鈕。taste-skill 把「設計品味」編成可攜帶的 Agent Skill，讓模型在產出 UI 之前先過三個調控旋鈕：佈局差異化、動態強度、資訊密度。

## T（核心論點）

taste-skill 是一套可攜式前端設計 Agent Skills，核心是「反 slop（防止生成通用模板）」規則集，搭配三個數值調控旋鈕（VARIANCE/MOTION/DENSITY），以及多個針對不同視覺方向的變體 Skill（極簡主義、暴力主義、重新設計既有專案等）。透過 `npx skills add` 安裝，支援所有主流 coding agent。

## B（適用場景）

**適用：**
- AI Agent 生成的前端 UI 太過通用，缺乏個性
- 需要為 greenfield 專案快速建立一致的視覺語言
- 想在既有 codebase 上做 UI 改造（redesign-skill）

**不適用：**
- 後端 / 資料處理任務，設計規則本身干擾代碼邏輯
- 已有嚴格 Design System 規格的企業專案（可能衝突）

## R（Skill 套件結構）

### 核心三旋鈕設定（taste-skill v2）

| 旋鈕 | 範圍 | 低值 | 高值 |
|------|------|------|------|
| DESIGN_VARIANCE | 1-10 | 置中/簡潔 | 非對稱/現代實驗性 |
| MOTION_INTENSITY | 1-10 | hover 效果 | scroll/磁性動畫 |
| VISUAL_DENSITY | 1-10 | 大量留白 | 高資訊密度儀表板 |

### Skill 對照表

| Skill | Install name | 說明 |
|-------|-------------|------|
| taste-skill（v2 exp）| design-taste-frontend | 通用預設，自動推斷設計語言，v2 大幅重寫中 |
| taste-skill-v1 | design-taste-frontend-v1 | 原版保留，若 v2 有 regression 時用 |
| gpt-tasteskill | gpt-taste | 更嚴格的 GPT/Codex 版本，強化 GSAP 方向 |
| image-to-code-skill | image-to-code | 先生成參考圖再 analyze 再實作 |
| redesign-skill | redesign-existing-projects | 對既有 codebase 先 audit UI 再修 |
| soft-skill | high-end-visual-design | 柔和/高端感，大量留白+premium 字型+spring 動態 |
| minimalist-skill | minimalist-ui | Editorial 風格（Notion/Linear 感），剋制調色盤 |
| brutalist-skill | industrial-brutalist-ui | 硬工業感，Swiss 排版+強對比+實驗佈局 |
| output-skill | full-output-enforcement | 解決模型輸出截斷問題（補全而非佔位符）|

### 圖像生成 Skill（輸出設計圖，非程式碼）

| Install name | 說明 |
|-------------|------|
| imagegen-frontend-web | 網站 comp：hero/landing/多段，強視覺指導 |
| imagegen-frontend-mobile | iOS/Android/跨平台畫面 |
| brandkit | 品牌識別板：logo/調色盤/字型/應用情境 |

**Image-first 工作流：** 先用 ChatGPT Images + `imagegen-*` 生成參考圖 → 交給 Codex/Cursor/Claude Code + `image-to-code-skill` 實作

### 安裝

```bash
# 安裝全套
npx skills add https://github.com/Leonxlnx/taste-skill

# 只安裝指定 Skill
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"
```

### 框架無關性

規則針對設計意圖而非特定框架 API，可跨 React / Vue / Svelte 使用。

## C（關鍵行動）

- [ ] 新 greenfield UI 專案先安裝 `design-taste-frontend`（v2），如有問題 fallback 到 `design-taste-frontend-v1`
- [ ] 想要特定視覺方向時選擇對應變體（極簡→minimalist-ui，高端→high-end-visual-design）
- [ ] 既有專案 UI 改造用 `redesign-existing-projects` 先 audit 再修，而非直接套用通用 Skill
- [ ] 搭配 ChatGPT Images 生成參考圖，再用 `image-to-code` 實作，比純文字描述更能還原視覺意圖

## 相關連結

- [[uiux-pro-max-skill]] — 另一個前端設計 Skill，161 產業規則 + 67 風格，與 taste-skill 可搭配使用
- [[claude-code-design-skills-overview]] — 六款 Design Skill 精選，taste-skill 在其中
- [[impeccable-style-ai-coding-guide]] — 程式碼品味 Skill，與 taste-skill 形成設計+工程雙規範
- 📎 2026-10-19：[[claude-codex-video-toolchain-selection]] — 同一開發者 Leonxlnx 的產品宣傳動畫 Skill Cinetic（第三方、MIT、預設 Remotion 輸出）
