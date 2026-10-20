---
title: Obsidian Visual Skills Pack：Canvas / Excalidraw / Mermaid 自動生成
date: 2026-05-14
tags: [pkm/obsidian, ai/claude]
source: https://github.com/axtonliu/axton-obsidian-visual-skills
type: article
status: processed
asset_value: 60
---

Obsidian Visual Skills Pack 是一個 Claude Code Skill 集合，讓 Claude 從文字描述直接生成 Obsidian 的三種視覺圖表：Canvas（卡片佈局）、Excalidraw（手繪風格）、Mermaid（流程圖）。比 MCP Server 設置更簡單——純 Markdown Skill 檔。

## T（核心論點）
Skills 比 MCP Server 設置門檻更低：只需要 Markdown 文件，Claude 按需載入，不需要額外的執行環境或 API 設定。

## B（適用場景）
**適用：** 需要在 Obsidian 中快速視覺化架構、流程、知識關係
**不適用：** 需要即時外部資料的圖表（用 MCP 更適合）

## R（三種 Skill）
| Skill | 輸出格式 | 適合用途 |
|-------|----------|----------|
| Excalidraw | `.md`（Obsidian）/ `.excalidraw` | 手繪風格、流程圖、架構圖 |
| Mermaid | 嵌入 Markdown | 層級流程、序列圖、甘特圖 |
| Canvas | `.canvas`（Obsidian） | 彩色卡片佈局、知識地圖 |

安裝：`git clone https://github.com/axtonliu/axton-obsidian-visual-skills.git ~/.claude/skills/obsidian-visual-skills`

## C（關鍵行動）
- [ ] 狀態為 Experimental，輸出品質依模型版本和輸入結構有所不同

## 相關連結
- [[claude-code-obsidian-setup]] — Claude Code + Obsidian 整合基礎設定
- [[claude-skills-guide]] — Skills 設計原理（能力包型）

## 補充資料（來源：axtonliu/axton-obsidian-visual-skills GitHub README）

### 三個 Skill 的詳細功能

**1. Excalidraw 圖表生成器**

三種輸出模式：
- **Obsidian 模式**（預設）：`.md` 格式，直接在 Obsidian 開啟
- **Standard 模式**：`.excalidraw` 格式，可在 excalidraw.com 編輯
- **Animated 模式**：`.excalidraw` 格式，搭配 excalidraw-animate 生成動畫

支援圖表類型：Flowchart、Mind Map、Hierarchy、Relationship、Comparison、Timeline、Matrix、Freeform

觸發關鍵字：`Excalidraw`、`diagram`、`flowchart`、`mind map`、`畫圖`、`流程圖`

**2. Mermaid 視覺化工具**

支援類型：Process Flow（graph TB/LR）、Circular Flow、Comparison Diagram、Mindmap、Sequence Diagram、State Diagram

內建語法錯誤預防機制，相容 Obsidian、GitHub 和其他 Mermaid 渲染器。

觸發關鍵字：`Mermaid`、`visualize`、`flowchart`、`sequence diagram`、`可视化`

**3. Obsidian Canvas 創建器**

兩種佈局模式：
- **MindMap**：以中心為原點的輻射層次結構，適合腦力激盪
- **Freeform**：自訂定位和彈性連接，適合複雜網路

觸發關鍵字：`Canvas`、`mind map`、`visual diagram`、`思维导图`

### 安裝方式

```bash
# 方式一：Plugin 系統（推薦）
/plugin marketplace add axtonliu/axton-obsidian-visual-skills
/plugin install obsidian-visual-skills

# 方式二：手動安裝
git clone https://github.com/axtonliu/axton-obsidian-visual-skills.git
cp -r axton-obsidian-visual-skills/excalidraw-diagram ~/.claude/skills/
cp -r axton-obsidian-visual-skills/mermaid-visualizer ~/.claude/skills/
cp -r axton-obsidian-visual-skills/obsidian-canvas-creator ~/.claude/skills/
```

### 中文字體注意事項

Excalifont 只涵蓋拉丁字元；CJK 手寫字體（小賴字體）需要從 Excalidraw.com 動態載入。若在離線環境或無法存取 Excalidraw.com，可從 Excalidraw GitHub 下載字體檔置於 vault 的 `Excalidraw/CJK Fonts` 資料夾，並在插件設定啟用「Load Chinese fonts from file at startup」。
