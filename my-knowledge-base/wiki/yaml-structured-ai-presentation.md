---
title: YAML 結構化語法控制 AI 簡報風格
date: 2026-06-02
tags: [ai/workflow, ai/prompt-engineering]
source: https://www.bnext.com.tw/article/91085/ai-presentation-yaml-style-guide
type: note
status: processed
asset_value: 30
---

## T（核心論點）

用 YAML 結構化語法取代模糊的自然語言描述（如「現代感」「專業風」），精確控制 AI 簡報工具的配色、字體與排版。YAML 是「給 AI 的視覺點菜單」——用色碼取代顏色名稱、分別指定標題與內文字體、設定禁止清單防止 AI 過度設計。

## B（適用場景）

**適用：**
- NotebookLM 搭配 Gamma 等 AI 簡報工具
- 需要精確對齊公司品牌標準色（CI）的簡報
- ChatGPT / Claude 設定數據圖表配色

**不適用 / 注意：**
- YAML 只解決最後的「視覺排版」環節，上游仍需梳理大綱
- 文字型 AI（如 NotebookLM）不直接輸出 .pptx，需配合視覺工具

## R（操作步驟）

1. **認識四個控制欄位**：
   - `color_palette:` — 用十六進位色碼（如 #003366）取代顏色名稱
   - `typography:` — 分別指定標題（Sans-serif 適合商務）與內文字體
   - `design_system:` — 給具體場景描述而非抽象形容
   - `output_constraints:` — 禁止清單（禁霓虹光、禁賽博龐克等）
2. **取得正確色碼**：用瀏覽器外掛吸取公司官網標準色
3. **YAML + 大綱一起貼給 AI**，搭配提示詞：「嚴格按照 YAML 風格規範生成」
4. **將 AI 輸出的規則貼到視覺工具**（Gamma / Canva / PowerPoint）

### 三款商務模板

| 風格 | 適用情境 |
|------|---------|
| 2.5D 立體風 | 系統流程、物流網格、智慧城市架構 |
| 高階顧問風 | 商業決策、高階主管報告（金字塔原理） |
| 軟體服務風 | 產品介紹、功能展示、系統流程 |

## C（關鍵行動）

- [ ] 建立自己的 YAML 風格模板（含公司 CI 色碼）
- [ ] 在下次簡報製作中實測 YAML + NotebookLM + Gamma 流程

## 原始摘要

教學如何透過 YAML 結構化語法精確控制 AI 簡報工具（如 NotebookLM、Gamma）的配色、字體與排版，取代模糊的自然語言描述，附三款可直接複製使用的商務風格模板。

## 相關連結

- [[ai-prompt-map-100-workflows]] — 100+ 組 AI 提示詞地圖
- [[google-workspace-notebooklm-gemini-admin-workflow]] — NotebookLM 在工作流中的應用
- [[notebooklm-py-cli-tool]] — NotebookLM Python CLI 工具
