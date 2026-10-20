---
title: Claude Code 設計 Skill 精選概覽
date: 2026-05-25
tags: [ai/claude, ai/workflow]
source: Facebook 社群貼文
type: note
status: processed
asset_value: 24
---

## T（核心論點）

六款值得嘗試的 Claude Code Design Skill，各有設計特色，適合不同使用情境：

| Skill | Stars | 特色 |
|-------|-------|------|
| **Impeccable** | 9,400 | 7 個領域參考檔案 + 20 個命令 + 反模式設計，基於 Anthropic 官方 frontend-design skill |
| **UI Skill** | 986 | 15 個 skill（含 SwiftUI），按需引用不用一次裝一整包 |
| **Taste Skill** | 3,300 | 標榜「高品味」現代化設計，GitHub readme 有生成範例可預覽 |
| **Better Icons** | 666 | MCP 工具，從 150+ 圖示集合（Lucide/Heroicons/Material 等）搜尋 200,000+ 圖示 |
| **Superdesign** | 6,100 | 開源 marketplace，整合 VS Code/Cursor/Windsurf，支援 Skills |
| **Design Plugin** | 650 | 產生多個 UI 版本 → 瀏覽器比較 → 融合精煉，迭代直到滿意 |

注意：設計本為半主觀事務，不喜歡就 let it go。

## 補充資料｜AI 設計 10 大 Skill 清單與本庫對照（2026-09-01 併入）

來源：https://tools.wingzero.tw/articles/4043（2026-08-31）。與本條目角度相同（都是設計類 Skill 清單），故併入而非另建。下表把該文 10 個 Skill 對到本庫已有的深入條目，**未有專篇者標示為新線索**：

| # | Skill | 分類 | 本庫對應 |
|---|---|---|---|
| 01 | **Hallmark** | 審美校準 | 🆕 無專篇——反 AI 味的網頁審美校準 |
| 02 | Open Design | 設計系統 | [[open-design-claude-design-comparison]]、[[claude-design-tool]] |
| 03 | UI/UX Pro Max | 頁面設計 | [[uiux-pro-max-skill]] |
| 04 | Taste Skill | 視覺提升 | [[taste-skill-anti-slop-frontend]] |
| 05 | **GSAP Skills** | 動效設計 | 🆕 無專篇——滾動觸發、時間軸編排 |
| 06 | **PencilPlaybook** | UI 規則 | 🆕 無專篇——設計心理學導向的 UI 規則 |
| 07 | **design-md-chrome** | 風格提取 | 提取網站視覺風格生成 `DESIGN.md`；格式規範見 [[design-md-google-format]]、範例集見 [[design-md-for-ai-ui]] |
| 08 | Claude Design Skill | 綜合設計 | [[claude-design-tool]] |
| 09–10 | （原文清單其餘項）| — | 見 [[frontend-agent-skills-30-picks]] |

🆕 四個本庫未收的線索：**Hallmark（審美校準）／GSAP Skills（動效）／PencilPlaybook（設計心理學 UI 規則）／design-md-chrome（風格提取）**——其中 GSAP 動效與 PencilPlaybook 的心理學導向規則，是本庫設計類條目目前完全沒覆蓋的兩個角度。

📎 2026-10-19：GSAP Skills 的授權（GSAP 被 Webflow 收購後免費含商用，限制是不能做與 Webflow 競爭的無程式碼動畫編輯器；Skills 為 MIT）、安裝指令與 aiposthub 實測結論（做的是網站動畫、非影片；只裝 Skills 仍有模板感，要搭配設計參考並寫明不要的風格），見 [[claude-codex-video-toolchain-selection]]（皆為 aiposthub 轉述與站內實測摘要）。

## 原始摘要

整理六款值得嘗試的 Claude Code Design Skill，包含 Impeccable（20 指令 + 反模式設計）、UI Skill（15 個含 SwiftUI 的按需引用 skill）、Taste Skill（標榜高品味現代設計）、Better Icons（MCP，20 萬+ 圖示搜尋）、Superdesign（與 IDE 整合的開源 marketplace）、Design Plugin（多版本 UI 比較並融合精煉）。

## 相關連結

- [[ai-native-interface-patterns]] — AI-native 介面模式（Thinking Trace／Approval Card／Streaming Text），與設計 Skill 互補：一個管「該呈現什麼」、一個管「怎麼做出來」

- [[claude-skills-ecosystem]] — 更廣泛的 Skills 生態（含 GitHub 高 star Skill Repo 分類）
- [[impeccable-style-ai-coding-guide]] — Impeccable 的深度解析條目
- [[claude-skills-guide]] — Claude Skills 完整指南
